jrbourbeau.github.io
====================

[![Build Status](https://travis-ci.org/jrbourbeau/jrbourbeau.github.io.svg?branch=source)](https://travis-ci.org/jrbourbeau/jrbourbeau.github.io)

This repository contains source code for jrbourbeau.github.io

## Installation

[Install Hugo](https://gohugo.io/getting-started/installing/):

```
brew install hugo
```

## Usage

Build and serve the site locally with:

```
hugo serve
```

The site should now be available at http://localhost:1313/

## Deployment

This website is hosted using GitHub Pages. Note that for user pages, the build website must [live in the `master` branch](https://help.github.com/en/articles/user-organization-and-project-pages#user-and-organization-pages-sites) of the repository. Because of this, we use the `source` branch of this repository for development and use [`doctr`](https://github.com/drdoctr/doctr) to automatically deploy our built website to the `master` branch.

## License

[MIT License](LICENSE)
