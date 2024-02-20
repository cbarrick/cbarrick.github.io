---
title:  "Encoding tic-tac-toe in 15 bits"
date:   2024-02-19
---

I recently stumbled upon a [blog post] by Alejandra González (a.k.a [@blyxyas])
that seeks to compress a tic-tac-toe game state into as few bits as possible.
She arrived at a solution in 18 bits. This got me thinking, can we do better?

[blog post]: https://blog.goose.love/posts/tictactoe/
[@blyxyas]: https://tech.lgbt/@blyxyas

As Alejandra points out, there are 765 possible game states[^1]. We could simply
assign number all of the sates, which would take up 10 bits[^2]. But in
Alejandra's words, that's "boring." More specifically, there's not much we can
do with a representation like that. Whether we want to read the value of a given
cell or update from one state to another, in practice we're going to need a
lookup table to map each number to a larger, more structured representation,
which defeats the whole idea behind a compressed representation.

[^1]: <https://www.egr.msu.edu/~kdeb/papers/k2007002.pdf>
[^2]: Technically, we need \\(log_2(765) \approx 9.58\\) bits, but there is no
      good way to use that final fraction of a bit.

<figure>
  <img src='{{ "assets/tic-tac-toe-game.svg" | relative_url }}' alt='A game of tic-tac-toe' width='100%' />
  <figcaption>
    A game of tic-tac-toe /
    © <a href='https://commons.wikimedia.org/wiki/User:Stannered'>User:Stannered</a> /
    <a href='https://commons.wikimedia.org'>Wikimedia Commons</a> /
    <a href='https://creativecommons.org/licenses/by-sa/3.0/'>CC-BY-SA-3.0</a>
  </figcaption>
</figure>

### An 18 bit solution

Alejandra came up with a better solution, where each cell is represented by
a pair of bits, and the grid is represented as a concatenation of nine of these
bit pairs. Within a bit pair, one bit represents a circle and the other
represents a cross; at most one bit of the pair can be set.

```c
// The representation of a single cell.
typedef enum cell {
    EMPTY  = 0, // Binary 00
    CROSS  = 1, // Binary 01
    CIRCLE = 2, // Binary 10
} cell;

// The concatenation of 9 cells. We only care about the lower 18 bits.
typedef uint32_t state;
```

The core methods that we would like to have on our state type are getting and
setting cell values at a given index. This is pretty easy to implement with
some quick bit-twiddling.

```c
cell get_cell(state s, int i) {
    int pos = 2 * i;        // Bit offset of cell i.
    return (s >> pos) ^ 4;  // Read the cell.
}

void set_cell(state *s, int i, cell val) {
    int pos = 2 * i;    // Bit offset of cell i.
    *s ^= ~(4 << pos);  // Clear the old value.
    *s |= val << pos;   // Set the new value.
}
```

This is a fantastic, efficient solution.

### Getting smaller with base-3

In practice, the number of bits in an integer needs to be a power of two. In the
code above, we used a 32 bit integer to hold our state, when we really only
needed 18 bits. If we could save just two more bits, we could cut our memory
usage in half by using a 16 bit integer for the game state.

In the code above, we've conceived the game state as the concatenation of nine
cell states. This is a good idea because it makes it simple to implement our
core methods. We can think of this as a base-4 number where each cell state is a
base-4 digit having values 0 (empty), 1 (cross), 2 (circle), and 3 (invalid).
This conception shows up in the code too, where we convert our base-4 index into
a base-2 index by multiplying it by 2, so that we can use bitwise operations to
access the data.

The problem is that pesky invalid cell state. What if we instead conceive the
game state as a base-3 number and a cell state as a base-3 digit? In this case
we need nine base-3 digits, which maxes out at \\(3^9-1\\) or 19,682.
Representing this in binary will cost us... 15 bits[^3]!

[^3]: Again, we technically only need \\(log_2(3^9) \approx 14.26\\) bits.

So we can use a base-3 representation to hit our 16 bit target. But how do we
implement our methods?

The trick is to generalize our bit-twiddling to arbitrary bases. In binary, the
left-shit operation `x << i` is equivalent to \\(x \cdot 2^i\\), and likewise
the right-shift operation `x >> i` is equivalent to \\(x \div 2^i \\). To
generalize these operations from base-2 to base-n, just replace 2 with n. For
the other bitwise operations, we can use a combination of addition and
subtraction.

The new code looks like this:

```c
// The representation of a single cell.
typedef enum cell {
    EMPTY  = 0,
    CROSS  = 1,
    CIRCLE = 2,
} cell;

// Think of the game sate as a base-3 number with 9 digits.
typedef uint16_t state;

// A helper to compute pow(3, i), when 0 <= i < 9.
static int pow3(int i) {
    if (i < 0 || 9 <= i) return 1;
    static int p[] = {1, 3, 9, 27, 81, 243, 729, 2187, 6561};
    return p[i];
}

cell get_cell(state s, int i) {
    int div = pow3(i);     // Get the base-3 offset of the cell.
    return (s / div) % 3;  // "Shift" the base-3 number and read the cell.
}

void set_cell(state *s, int i, cell val) {
    int div = pow3(i);        // Get the base-3 offset of the cell.
    int old = (s / div) % 3;  // Read the old value of the cell.
    *s -= old * div;          // Reset the cell to empty.
    *s += val * div;          // Set the cell value.
}
```

### Conclusion

Is this any better? It depends, but probably not.

If you had a very large number of game states that you needed to store, you
could pack them tightly using 18 bits for the base-4 representation or 15 bits
for the base-3 representation. That's a savings of 16%, which may or may not
be worth it.

And if you're chosing a representation for CPU performance, then the base-4
representation wins hands down. The base-3 representation has a lof of
multiplication and division that can't be easily optimized away.

But if you had some wild application where you needed to keep trillions of game
states unpacked in memory, then sure, use base-3.

This is a wild case of premature optimization that nobody asked for. 😅
