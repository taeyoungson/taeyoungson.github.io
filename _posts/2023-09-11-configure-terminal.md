---
layout: post
title: 터미널 세팅하기
date: 2023-09-11 00:00:00-0400
description: 우분투 터미널 개발 세팅하기
tags: zsh vim ubuntu
categories: terminal
related_posts: false
giscus_comments: true
toc:
  sidebar: left
---

## Pre-Installation
---

Dotfile이란 shell, vim 및 다양한 툴들의 설정파일들을 통틀어 지칭하는 단어입니다.
잘 구성된 개발환경은 업무 생산성에도 도움이 되죠!
대체로 . (dot)으로 시작하는 파일 이름을 지니기에 dotfile이라 일컬어 집니다.

본 포스트에서는 제가 사용하는 우분투 터미널 dotfile setup 방식을 다루고, 구성된 docker image를 공유하고자 작성되었습니다.

{% include figure.html path="assets/img/plugin_usages/coding-cat.gif" class="img-fluid rounded z-depth-1" caption="그럼, 시작해볼까요?" %}


### Getting Ready

설치엔 python (> 3.6)이 필요하므로 설치해 줍니다.
*docker, venv, anaconda* 등 선호하는 툴을 이용해 설치할 환경을 구축합니다.
설치되어있는 python 버전은 다음 명령어로 확인할 수 있습니다.

```bash
python --version # > 3.6
```

*curl, git, node, npm, zsh* 이 필요하므로 설치 되어있지 않다면 먼저 설치해 줍니다.

```bash
apt-get update
apt-get install git curl tmux zsh
```

기존 vim과 zsh를 위한 설정파일을 따로 사용하고 있다면 자동으로 백업되지 않으므로 설치전에 백업을 해주도록 합시다.

```bash
# 설치 전
mv $HOME/.vimrc $HOME/.vimrc.bak
mv $HOME/.zshrc $HOME/.zshrc.bak

# 설치 후
cat $HOME/.virmc.bak >> $HOME/.vimrc && rm .vimrc.bak
cat $HOME/.zshrc.bak >> $HOME/.zshrc && rm .zshrc.bak

# 위는 일반적인 백업 방식이며 본인 설정에 맞게 다시 병합해주도록 합니다.
```

또, 설치에는 *neovim (>= 0.9.2)* 이 필요하므로 [neovim](https://github.com/neovim/neovim) 에서 직접 설치해 줍니다.
*apt-get* 을 이용해 설치시에는 버전이 낮으므로, 직접 빌드하는 것을 권합니다.
설치한 후에는 다음과 같이 버전을 확인할 수 있습니다.

```bash
nvim --version # >= 0.9.2
```

## Installation

저는 보통  clone받아 몇 가지를 커스터마이징 해서 사용합니다.
저는 ZSH와 더불어 잘 세팅되어 있는 [wookayin's dotfile](https://github.com/wookayin/dotfiles) 로부터 설치를 합니다.
설치 과정은 전부 자동화 되어 있기에 거의 건드릴 것이 없습니다.
이 자리를 빌어 끝내주는🔥 설정 파일 공유 해주신 [wookayin](https://github.com/wookayin) 님께 감사를 전합니다!

```bash
curl -fsSL https://dotfiles.wook.kr/etc/install | bash
```

```bash
# some useful tools
apt-get install tree silversearcher-ag
```

*만일 백업해 둔 설정파일들이 있으면 원하는 내용들은 다시 적절히 병합해줍니다*

## Pre-built Images

제가 사용하기 위해 미리 만들어 놓은 이미지는 제 [docker hub](https://hub.docker.com/r/tiqqun/ubuntu/tags)에서 pull 받아 사용하실 수 있습니다.
ubuntu 22.04를 기준으로 작성되었습니다.

```bash
docker pull tiqqun/ubuntu:22.04
docker run -it --name ubuntu2204 tiqqun/ubuntu:22.04
```

## Usage

FZF

- [junegunn](https://github.com/junegunn)님의 fzf plugin을 이용한 커맨드 히스토리 서치 (*Ctrl-R*로 매핑되어 있음)
{% include figure.html path="assets/img/plugin_usages/fzf-search-big.gif" class="img-fluid rounded z-depth-1" %}

- 똑같이 fzf를 이용한 Folder navigation (*Ctrl-E*로 매핑되어 있음)
{% include figure.html path="assets/img/plugin_usages/fzf-cd-widget.gif" class="img-fluid rounded z-depth-1" %}

---
## FAQ
에러 내용을 통해 검색하시면 더 빠릅니다 🔍

- Spawning language server with cmd: 'ruff-lsp' failed. The language server is either not installed.
```sh
pip install ruff-lsp
```

- [mason-lspconfig.nvim] failed to install pyright. Installation logs are available in :Mason and...
```vim
:MasonInstall --force pyright
```

-
