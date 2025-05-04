---
layout: post
title: My terminal setup on macOS Sonoma 14.0
description: A blog post on my personal terminal setup for macOS Sonoma 14.0
date: '2023-10-20T00:00:00.000Z'
author: Anirvan Mandal
categories: [development, terminal, osx]
tags: osx iterm zsh

---

### Introduction

The default terminal on macOS looks outdated and lacks modern features. iTerm2 is a better alternative because it comes with customisable 
features and enhances functionality.

### Prerequisites

We need to install `homebrew` and `zsh` before proceeding. Skip this step if it's already installed on your machine.

#### Install Homebrew

```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

You can use `homebrew` to now install `zsh`

```shell
brew install zsh
```

### Installation

You can now use homebrew to install iTerm2

```shell
brew install --cask iterm2
```

### Oh-my-zsh

Install the [oh-my-zsh](https://ohmyz.sh/) framework to manage plugins & themes for your terminal

```shell
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

### Configuring themes

Oh my Zsh comes with over 100 bundled themes. You can choose a theme of your liking from [here](https://github.com/ohmyzsh/ohmyzsh/tree/master/themes)
I personally use [p10k](https://github.com/romkatv/powerlevel10k). Head over to the link for documentation on how to install and use the theme.

### Plugins

There are over 300+ plugins available with oh-my-zsh. Take a look at the [wiki](https://github.com/ohmyzsh/ohmyzsh/wiki/Plugins) for more details.
Plugins I use

```shell
plugins=(aliases brew colored-man-pages copyfile copypath gh git golang heroku macos nvm npm rails ruby rvm zsh-autosuggestions zsh-syntax-highlighting web-search)
```

### Changing iTerm color scheme

https://www.iterm2material.design/

### Conclusion

There are many more themes, plugins & settings to play with to make your terminal unique.



