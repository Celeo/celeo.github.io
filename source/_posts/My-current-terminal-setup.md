---
title: My terminal setup
date: 2022-05-29 16:29:11
tags:
- technology
- programming
---

The programs and tools I use to be productive in the terminal.

<!-- more -->

This is a setup on Mac OSX. I prefer Linux over Mac's unix-like environment, but for the purposes of most of this, they work similarly enough to not really notice.

## Emulator and shell

[iterm2](https://iterm2.com/) running [zsh](https://www.zsh.org/) with [oh-my-zsh](https://ohmyz.sh/) (mostly because it works and I was able to optimize my shell startup in other places so that the shell spins up effectively instantly) and the [starship](https://starship.rs/) prompt. I've also used [alacritty](https://github.com/alacritty/alacritty) but prefer iterm2 on Mac.

## Text editor

I'm moving to [helix](https://github.com/helix-editor/helix) from [neovim](https://github.com/neovim/neovim).

## Locating stuff

- [exa](https://github.com/ogham/exa) to replace `ls`
- [fd](https://github.com/sharkdp/fd) to replace `find`
- [rg](https://github.com/BurntSushi/ripgrep) to replace `find` + `grep`
- [zoxide](https://github.com/ajeetdsouza/zoxide) to quickly get to folders
- [joshuto](https://github.com/kamiyaa/joshuto) because Mac's Finder application is awful

## Scripts

I have a few aliases and functions in my .zshrc file, but I've moved most scripts to [navi](https://github.com/denisidoro/navi).

## git

I have a global gitignore file for things like "node_modules", "target", "dist", "build", etc. directories. I use the [delta](https://github.com/dandavison/delta) diff visualizer. I used to handle merge conflicts by hand until I had to undertake some very complicated conflicts and I was pointed to [IntelliJ's 3-way diff](https://intellij-support.jetbrains.com/hc/en-us/community/posts/360006808659-3-way-diff-window). I have the [gh](https://github.com/cli/cli) GitHub tool installed, though I rarely use it.

## Language versioning

I use [fnm](https://github.com/Schniz/fnm) to manage NodeJS versions, `npm` to manage Node dependencies (though I've used [yarn v1](https://classic.yarnpkg.com/lang/en/docs/cli/) often as well) and [poetry](https://github.com/python-poetry/poetry) for Python virtual environments and dependency management. For Java and Kotlin, it's [maven](https://github.com/apache/maven), and for programming languages that have their stuff together, their official CLI tools.

## Other

- [jq](https://github.com/stedolan/jq)
- [pandoc](https://github.com/jgm/pandoc)
- [tokei](https://github.com/XAMPPRocky/tokei)
- [watchexec](https://github.com/watchexec/watchexec)
- [httpie](https://github.com/httpie/httpie)
