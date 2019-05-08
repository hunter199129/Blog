---
title: 'Linux Applications I use'
date: 2019-05-08 17:00:00
tags: [Linux, software]
categories: Linux
---

記錄一下我Linux會用到的軟體

不然每次重灌都要重找一次，有點麻煩...

<!--More-->

Note: 都是以debian/ubuntu based的為主
Note2: ✔ 代表必須

## Applications

### Environment

- [✔️ Anaconda (python)](https://www.anaconda.com)

- [✔️ Nodejs](https://nodejs.org)

- [✔️ Snap](https://snapcraft.io)

        sudo apt install snapd

### Account

- [✔️ Bitwarden](https://bitwarden.com)

        sudo snap install bitwarden

### Android

- [✔️ KDEConnect](https://community.kde.org/KDEConnect)

        sudo apt install kdeconnect indicator-kdeconnect

- [✔️ PushBullet](https://www.pushbullet.com/)

- [scrcpy](https://github.com/Genymobile/scrcpy): Display and control your Android device

### Backup

- [borg](https://www.borgbackup.org/): Deduplicating archiver with compression and authenticated encryption.

- [restic](https://restic.net/): Fast, secure, efficient backup program (support cloud backup)

### Browser

- [✔️ Chrome](https://www.google.com/intl/zh-TW/chrome)

- [✔️ Firefox ESR(Extended Support Release)](https://www.mozilla.org/en-US/firefox/organizations/)

        snap install --channel=esr/stable firefox

### Chat

- [caprine](https://github.com/sindresorhus/caprine): Elegant Facebook Messenger desktop app

- [Franz](https://meetfranz.com/)

### Download

- [MegaSync](https://mega.nz/#sync)

### Editor

- [✔️ vim](https://www.vim.org)

        sudo apt install vim 

- [✔️ VSCode](https://code.visualstudio.com)

        sudo snap install vscode
### File Management

- [✔️ 7-zip](https://www.7-zip.org/)

        sudo apt install p7zip-full

- [ranger](https://ranger.github.io/): A VIM-inspired filemanager for the console

### Gaming

- [✔️ Steam](https://store.steampowered.com/)

        sudo apt install steam

### Input

- [✔️ ibus for Chinese](http://chewing.im/projects/ibus-chewing)

        sudo apt install ibus-chewing

  >[Debian & Ubuntu 設定中文輸入法](https://hunter199129.github.io/blog/2018/05/29/Debian%20&%20Ubuntu%20設定中文輸入法/)


### Music

- [Spotify](https://www.spotify.com)

        sudo snap install spotify

- [Audacious](https://audacious-media-player.org/): An open source audio player that plays your music how you want it, without stealing away your computer’s resources from other tasks.

        sudo apt install audacious

### Note Taking

- [Notable](https://github.com/notable/notable)

### System & Command Line Utility

- [✔️ fkill-cli](https://github.com/sindresorhus/fkill-cli)

        npm install -g fkill-cli

- [Neofetch](https://github.com/dylanaraps/neofetch)

        sudo apt install neofetch

- [Stacer](https://oguzhaninan.github.io/Stacer-Web/)

### Terminal

- [✔️ oh-my-zsh](https://ohmyz.sh/)

  1. install zsh

          sudo apt install zsh

  2. install oh-my-zsh

          sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

  3. change default bash
  
          chsh -s $(which zsh)

- [✔️ tmux](https://github.com/tmux/tmux)

        sudo apt install tmux

- [Hyper](https://hyper.is/)

- [bat](https://github.com/sharkdp/bat): A cat(1) clone with wings.

- [TLDR pages](https://tldr.sh/): Simplified and community-driven man pages

        sudo npm install -g tldr

- [ffsend](https://github.com/timvisee/ffsend)

        sudo snap install ffsend

- [ripgrep (rg)](https://github.com/BurntSushi/ripgrep): More powerful grep

- [WTF](http://wtfutil.com): A personal terminal-based dashboard utility, designed for displaying infrequently-needed, but very important, daily data.

- [lazygit](https://github.com/jesseduffield/lazygit): A simple terminal UI for git commands, written in Go with the gocui library.

### Video

- [✔️ vlc](https://www.videolan.org)

        sudo apt install vlc

- [✔ AceStream](www.acestream.org)

        sudo snap install acestreamplayer

## Others

### Customization

- [OpenDesktop](https://www.opendesktop.org)

REF: [Awesome-Linux-Software](https://github.com/luong-komorebi/Awesome-Linux-Software)
