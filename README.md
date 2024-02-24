cbarrick.github.io
==================================================

The source code of my personal website, built with Jekyll and hosted on GitHub
Pages.

Building
-------------------------

The site is built with Jekyll, a static site generator in Ruby. I try to
support the Ruby versions from Homebrew (macOS), Debian, and GitHub Pages, but
I make no guarantees. Both Jekyll and Ruby are pretty stable these days, so
this will probably work in other distributions.

```shell
# Get Ruby. For macOS, use the version in Homebrew.
$ brew install ruby

# Install the bundler gem for environment and dependency management.
$ gem install bundler

# Use bundler to install the dependencies, including Jekyll.
$ bundle install

# Use the usual Jekyll commands to build or serve the site.
# Prefix the commands with `bundle exec` to use the bundled environment.
$ bundle exec jekyll build

# Update the environment often.
$ bundle update
```


Development
-------------------------

### Jekyll Plugins

Follow these steps to install a new Jekyll plugin.

###### 1. Only use supported plugins.

GitHub Pages only supports a fixed set of plugins, the list of which can be
found here: https://pages.github.com/versions/

###### 2. Use the `github-pages` gem.

We only list one gem in the Gemfile: `github-pages`.

The `github-pages` gem transitively depends on all gems supported in the GitHub
Pages build environment. There is no need to specify any other dependencies
because they will be ignored by GitHub.

###### 3. Register plugins in `_config.yml`.

List the desired plugins in `_config.yml` under the `plugins` section.

While Jekyll allows you to register plugins in the Gemfile under the
`:jekyll_plugins` group, this is not supported by GitHub Pages. We must
explicitly list plugins in `_config.yml` to host the site on GitHub.


Acknowledgements
-------------------------

- [minireset.css][] - Jeremy Thomas - [MIT][minireset-license]
- [Simple Icons][] - Each icon is a trademark their respective company
- [Fira Sans & Fira Mono][] - Mozilla - [OFL 1.1][fira-license]
- [Fira Code][] - The Fira Code Project Authors - [OFL 1.1][fira-code-license]
- [jekyll-compress-html][] - Anatol Broder - [MIT][jekyll-compress-html-license]

[minireset.css]: https://github.com/jgthms/minireset.css
[Simple Icons]: https://simpleicons.org/
[Fira Sans & Fira Mono]: http://mozilla.github.io/Fira/
[Fira Code]: https://github.com/tonsky/FiraCode
[jekyll-compress-html]: https://github.com/penibelst/jekyll-compress-html

[minireset-license]: https://github.com/jgthms/minireset.css/blob/master/LICENSE
[jekyll-compress-html-license]: https://github.com/penibelst/jekyll-compress-html/blob/master/LICENSE
[fira-license]: https://github.com/mozilla/Fira/blob/master/LICENSE
[fira-code-license]: https://github.com/tonsky/FiraCode/blob/master/LICENSE


License
-------------------------

© Copyright 2019 Chris Barrick

The contents of this website are licensed to the public under the
[Creative Commons Attribution-ShareAlike 4.0 International][cc-by-sa-4.0]
license (CC-BY-SA-4.0).

[cc-by-sa-4.0]: https://creativecommons.org/licenses/by-sa/4.0/
