# [know it sy(all)](https://knowit.syall.work/)

## Overview

**know it sy(all)** is a blog about technical concepts, whether practical or not, from my experience as a programmer.

## Usage

The website itself is built with [hugo](https://gohugo.io/), decorated with the [hyde theme](https://github.com/spf13/hyde).

All of the content is managed through several commands.

### Site Configuration

The site title, publish directory, description, and main menu can all be configured in `config.toml`.

### Making a New Post

```shell
hugo new posts/<name-of-post>.md
```

### Running on a Local Server

```shell
hugo server -D # -D = Drafts Included
```

### Create a Build

```shell
hugo
```

### Deployment

The site is published on GitHub Pages through the `master` branch `docs/` directory, so publishing is as simple as commiting the build!

There is also a `CNAME` file in the `docs/` directory for publishing.
