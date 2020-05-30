---
title: iterm
toc: true
recommend: 1
uniqueId: '2020-03-03 08:25:46/"iterm".html'
date: 2020-03-03 16:25:46
thumbnail:
categories:
- Mac
tags:
keywords:
---

[TOC]

<!--more-->

#  主题配置

<img src="https://i.loli.net/2020/03/04/9ckelYunJqL6m35.png" alt="9ckelYunJqL6m35" style="zoom:33%;" />



## 准备工作

下载**iTerm**色彩方案[iTerm2-Color-Schemes](https://github.com/mbadolato/iTerm2-Color-Schemes)并解压

进入**Preference > Profiles**

- 设置**Terminal > Report Terminal Type**，选择`xterm-256color`
- 设置**Colors > Color Presets > Import**，导入`iTerm2-Color-Schemes`的`schemes文件夹`，选择自己喜欢的颜色方案(选的是`dracula`)
- 设置**Window > Transparency**，调整窗口透明度
- 设置**Session > Status bar enabled > Configure Status Bar**，拖拽需要显示的状态

## 图标字体安装

```shell
export http_proxy=http://127.0.0.1:1087;export https_proxy=http://127.0.0.1:1087;
brew tap caskroom/fonts
brew cask install font-hack-nerd-font
brew install ruby
gem install colorls
```





## colorls 安装

[athityakumar/colorls: A Ruby gem that beautifies the terminal's ls command, with color and font-awesome icons.](https://github.com/athityakumar/colorls)

```shell
brew install ruby
# 替换你的Ruby版本
brew link --overwrite ruby
# 你还可以使用rbenv在计算机上安装和运行不同版本的Ruby brew install rbenv

gem install colorls
# 问题，gem/ruby 安装的 colorls 命令找不到
colorls
# zsh: command not found: colorls
gem which colorls
# /usr/local/lib/ruby/gems/2.6.0/gems/colorls-1.3.3/lib/colorls.rb
/usr/local/lib/ruby/gems/2.6.0/bin/colorls

vim ~/.zshrc
export PATH="/usr/local/lib/ruby/gems/2.6.0/bin:$PATH"
vim ~/.alias
```



## powerlevel9k --zsh 主题安装

```shell
git clone https://github.com/bhilburn/powerlevel9k.git ~/.oh-my-zsh/themes/powerlevel9k
# You then need to select this theme in your ~/.zshrc:
ZSH_THEME="powerlevel9k/powerlevel9k"
```



[Show Off Your Config · Powerlevel9k/powerlevel9k Wiki](https://github.com/Powerlevel9k/powerlevel9k/wiki/Show-Off-Your-Config)

用户分享的主题配置

以下选择第一个配置

[daniruiz/dotfiles: ~/.dotfiles](https://github.com/daniruiz/dotfiles)

选取其 powerlevel9k 配置

```bash
# ----- promt -----
PS1="%F{cyan} %~ >%F{blue}> %F{reset}"

# -------------------------------- POWERLEVEL ---------------------------------
POWERLEVEL9K_MODE=nerdfont-complete
POWERLEVEL9K_PROMPT_ON_NEWLINE=true
POWERLEVEL9K_RPROMPT_ON_NEWLINE=true
POWERLEVEL9K_SHORTEN_DIR_LENGTH=2
POWERLEVEL9K=truncate_beginning
POWERLEVEL9K_TIME_BACKGROUND=black
POWERLEVEL9K_TIME_FOREGROUND=white
POWERLEVEL9K_TIME_FORMAT=%D{%I:%M}
POWERLEVEL9K_STATUS_VERBOSE=false
POWERLEVEL9K_VCS_CLEAN_FOREGROUND=black
POWERLEVEL9K_VCS_CLEAN_BACKGROUND=green
POWERLEVEL9K_VCS_UNTRACKED_FOREGROUND=black
POWERLEVEL9K_VCS_UNTRACKED_BACKGROUND=yellow
POWERLEVEL9K_VCS_MODIFIED_FOREGROUND=white
POWERLEVEL9K_VCS_MODIFIED_BACKGROUND=black
POWERLEVEL9K_COMMAND_EXECUTION_TIME_BACKGROUND=black
POWERLEVEL9K_COMMAND_EXECUTION_TIME_FOREGROUND=blue
POWERLEVEL9K_FOLDER_ICON=
POWERLEVEL9K_STATUS_OK_IN_NON_VERBOSE=true
POWERLEVEL9K_STATUS_VERBOSE=false
POWERLEVEL9K_COMMAND_EXECUTION_TIME_THRESHOLD=0
POWERLEVEL9K_VCS_UNTRACKED_ICON=●
POWERLEVEL9K_VCS_UNSTAGED_ICON=±
POWERLEVEL9K_VCS_INCOMING_CHANGES_ICON=↓
POWERLEVEL9K_VCS_OUTGOING_CHANGES_ICON=↑
POWERLEVEL9K_VCS_COMMIT_ICON=' '
POWERLEVEL9K_MULTILINE_FIRST_PROMPT_PREFIX='%F{blue}╭─'
# POWERLEVEL9K_MULTILINE_FIRST_PROMPT_PREFIX='%F{blue}╭─%F{red}'
POWERLEVEL9K_MULTILINE_LAST_PROMPT_PREFIX='%F{blue}╰%f '
#POWERLEVEL9K_CUSTOM_OS_ICON='echo   $(whoami) '
POWERLEVEL9K_CUSTOM_OS_ICON_BACKGROUND=red
POWERLEVEL9K_CUSTOM_OS_ICON_FOREGROUND=white
POWERLEVEL9K_LEFT_PROMPT_ELEMENTS=(custom_os_icon ssh root_indicator dir dir_writable vcs)
POWERLEVEL9K_RIGHT_PROMPT_ELEMENTS=(command_execution_time status background_jobs time ram)

if [[ $(tty) == /dev/pts/* ]]; then
	source /usr/share/zsh-theme-powerlevel10k/powerlevel10k.zsh-theme;
else
	clear
	echo
	echo
fi
```



## vim 简单配置

.vimrc

```
syntax on
set number
set ruler

" 高亮当前行
set cursorcolumn
set cursorline
highlight CursorLine   cterm=NONE ctermbg=black ctermfg=green guibg=NONE guifg=NONE
highlight CursorColumn cterm=NONE ctermbg=black ctermfg=green guibg=NONE guifg=NONE
```



## 参考链接

[打造 Mac 下高颜值好用的终端环境 | 编程和酒](https://blog.biezhi.me/2018/11/build-a-beautiful-mac-terminal-environment.html)

[Show Off Your Config · Powerlevel9k/powerlevel9k Wiki](https://github.com/Powerlevel9k/powerlevel9k/wiki/Show-Off-Your-Config)

[daniruiz/dotfiles: ~/.dotfiles](https://github.com/daniruiz/dotfiles)

## 系统默认终端美化

[10 个 Terminal 主题，让你的 macOS 终端更好看 - 少数派](https://sspai.com/post/53008)



# iTerm2使用技巧

## 启动一个连接到远程server的终端

选择“Preferences->Profiles”，新增一个profile，并设置启动的快捷键和command命令

## 分屏

使用快捷键“cmd+d”实现左右分屏，快捷键“^+cmd+d”实现上下分屏, 效果如图：

<img src="https://i.loli.net/2020/03/03/NH94jOBeYg8GZmk.jpg" alt="NH94jOBeYg8GZmk" style="zoom:33%;" />



### 搜索及文本复制

使用“cmd+f”可以调出搜索框进行文本搜索，然后有个很奇妙的快捷键“**tab**”键，使用它后会自动高亮当前文本后面的内容。最后按enter键将高亮文本复制到剪切板上。这几个按键连用代替了使用鼠标复制文本内容！

<img src="https://i.loli.net/2020/03/03/gSTdjsQw7r1UPnv.jpg" alt="gSTdjsQw7r1UPnv" style="zoom:50%;" />



## 没啥用的-显示当前行、右侧显示时间线、自动命令补全

<img src="https://i.loli.net/2020/03/03/M8uQ3JlOIdczbSe.png" alt="M8uQ3JlOIdczbSe" style="zoom:33%;" />

## 调出复制过的文本历史

快捷键：“^+cmd+h”。

## 按键回放

这个简直功能太强大了！它能回放一段时间内的你敲过的所有字符。
快捷键：“cmd+alt+b”，如图会弹出一个进度条，按左右键就可以实现按键回放了。





## mac iterm2 以单词为单位移动的快捷键设置

[Mac下iTerm2光标按照单词快速移动设置_运维_skyyws的博客-CSDN博客](https://blog.csdn.net/skyyws/article/details/78480132?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task)

<img src="https://i.loli.net/2020/02/26/1aVChvexXkQbPI8.png" alt="1aVChvexXkQbPI8" style="zoom: 33%;" />

<img src="https://i.loli.net/2020/02/26/Y5PywSNJf1bHWrQ.png" alt="Y5PywSNJf1bHWrQ" style="zoom:33%;" />
