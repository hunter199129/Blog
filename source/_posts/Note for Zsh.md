---
title: 'Note for Zsh'
date: 2019-05-09 14:20:00
tags: [Linux, zsh]
categories: Linux
---

My Zsh Note

<!--More-->

## Installation

## Plugins

### [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions): Auto complete

1. Clone this repository into $ZSH_CUSTOM/plugins (by default ~/.oh-my-zsh/custom/plugins)

        git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

2. Add the plugin to the list of plugins for Oh My Zsh to load (inside ~/.zshrc):

        echo plugins=(zsh-autosuggestions) >> ~/.zshrc

3. Start a new terminal session.

## Themes

### [Spaceship ZSH](https://github.com/denysdovhan/spaceship-prompt)

1. Clone this repo:

        git clone https://github.com/denysdovhan/spaceship-prompt.git "$ZSH_CUSTOM/themes/spaceship-prompt"

2. Symlink spaceship.zsh-theme to your oh-my-zsh custom themes directory:

        ln -s "$ZSH_CUSTOM/themes/spaceship-prompt/spaceship.zsh-theme" "$ZSH_CUSTOM/themes/spaceship.zsh-theme"

3. Set ZSH_THEME="spaceship" in your .zshrc.

        sed -i 's/robbyrussell/spaceship/g' ~/.zshrc
