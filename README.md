cbarrick.github.io
==================================================

The source code of my personal website, built with Jekyll and hosted on GitHub Pages.

Building
-------------------------

The site is built with Jekyll, a static site generator in Ruby. I try to support the Ruby versions from Homebrew (macOS), Debian, and GitHub Pages, but I make not guarantees. Both Jekyll and Ruby a pretty stable these days, so this will probably work in other distributions.

```shell
# Get Ruby. For macOS, just use the version in Homebrew.
$ brew install ruby

# Install the bundler gem for robust dependency management.
$ gem install bundler

# Use bundler to install the dependencies, including Jekyll.
$ bundle install

# Use the usual Jekyll commands to build or serve the site, but
# prefix the commands with `bundle exec` to use the bundled
# dependencies.
$ bundle exec jekyll build

# Update the environment often.
$ bundle update
```


Development
-------------------------

### Jekyll Plugins

Follow these steps to install a new Jekyll plugin.

###### 1. Only use supported plugins.

GitHub Pages only supports a fixed set of plugins, the list of which can be found here: https://pages.github.com/versions/

###### 2. Use the `github-pages` gem.

We only list one gem in the Gemfile: `github-pages`.

The `github-pages` gem transitively depends on all gems supported in the GitHub Pages build environment. There is no need to specify any other dependencies because they will be ignored by GitHub.

###### 3. Register plugins in `_config.yml`.

List the desired plugins in `_config.yml` under the `plugins` section.

Jekyll allows you to register plugins directly in the Gemfile by placing them under a group called `:jekyll_plugins`, and the official Jekyll docs advise this method. However, the Gemfile is ignored by GitHub Pages, so we must use the `_config.yml` method instead.


License & Acknowledgements
-------------------------

© Copyright 2019 Chris Barrick

The contents of this website are licensed under the [Creative Commons Attribution 4.0 International](https://creativecommons.org/licenses/by/4.0/) license (CC-BY-4.0).

This site uses [minireset.css](https://github.com/jgthms/minireset.css) as a CSS reset. It is copyright Jeremy Thomas and released under the [MIT license](https://github.com/jgthms/minireset.css/blob/master/LICENSE).

This site uses the [Material Design icons](http://google.github.io/material-design-icons/) for common UI/UX icons, which are copyright Google and released under the [Apache License Version 2.0](https://www.apache.org/licenses/LICENSE-2.0.txt).

This site uses the [Simple Icons](https://simpleicons.org/) for SVG icons of popular brands. Each icon is a trademark their respective company.

This site uses the [Fira](http://mozilla.github.io/Fira/) suite of fonts and the [Fira Code](https://github.com/tonsky/FiraCode) derivative font, which are copyright The Mozilla Foundation and The Fira Code Project Authors respectively. All fonts are licensed under the [SIL Open Font License, Version 1.1](https://scripts.sil.org/OFL).

This site uses the [jekyll-compress-html](https://github.com/penibelst/jekyll-compress-html) layout to minify the HTML. It is copyright Anatol Broder and released under the [MIT license](https://github.com/penibelst/jekyll-compress-html/blob/master/LICENSE).
