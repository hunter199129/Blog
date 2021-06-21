---
title: 'Note for Zsh'
date: 2019-05-09 14:20:00
tags: [Linux, zsh]
categories: Linux
---

My Zsh Note

<!--More-->

## Installation Guide

    # 1. install zsh
    sudo apt install zsh
    
    # 2. install oh-my-zsh
    sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

## Plugins

### [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions): Auto complete

    # 1. Clone this repository into $ZSH_CUSTOM/plugins (by default ~/.oh-my-zsh/custom/plugins)
    git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

    # 2. Add the plugin to the list of plugins for Oh My Zsh to load (inside ~/.zshrc):
    sed -i '/plugins=(git)/a plugins=(zsh-autosuggestions)' ~/.zshrc
    echo "plugins=(zsh-autosuggestions)" >> ~/.zshrc

## Themes

### [Spaceship ZSH](https://github.com/denysdovhan/spaceship-prompt)

    # 1. Clone this repo:
    git clone https://github.com/denysdovhan/spaceship-prompt.git "$ZSH_CUSTOM/themes/spaceship-prompt"

    # 2. Symlink spaceship.zsh-theme to your oh-my-zsh custom themes directory:
    ln -s "$ZSH_CUSTOM/themes/spaceship-prompt/spaceship.zsh-theme" "$ZSH_CUSTOM/themes/spaceship.zsh-theme"

    # 3. Set ZSH_THEME="spaceship" in your .zshrc.
    sed -i 's/robbyrussell/spaceship/g' ~/.zshrc

## Font

- **NEW UPDATE AT 20210621**
    - use [nerd fonts](https://github.com/ryanoasis/nerd-fonts) for all fonts install

- Old method below  
    Install Font via [awesome-terminal-fonts](https://github.com/gabrielelana/awesome-terminal-fonts)

        # 1. Clone the repo
        git clone https://github.com/gabrielelana/awesome-terminal-fonts.git

        # 2. install using the script
        sudo bash awesome-terminal-fonts/install.sh

    or using [powerline/fonts](https://github.com/powerline/fonts) if there's no font is appropriate

## Other

P.S. To change default bash

    chsh -s $(which zsh)

