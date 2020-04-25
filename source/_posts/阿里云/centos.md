---
title: 阿里云
toc: true
uniqueId: '2020-03-02 00:11:19/"阿里云".html'
date: 2020-03-02 08:11:19
thumbnail:
categories:
- 阿里云
tags:
keywords:
---


[TOC]

<!--more-->

## 【阿里云】高校学生“在家实践”计划

[【阿里云】高校学生“在家实践”计划](https://developer.aliyun.com/adc/student/?spm=a2c6h.13788096.J_7970846300.1.75da7638uOFYrv&accounttraceid=45ab3d56815a4be6ab3feac336a73784hxcl#ecscolleges-collocation-stu)

<img src="https://i.loli.net/2020/03/02/WxfQkdBtVR56gYa.png" alt="WxfQkdBtVR56gYa" style="zoom:50%;" />

## 登录问题



<img src="https://i.loli.net/2020/03/02/uEcSq12LNJgMTDk.png" alt="uEcSq12LNJgMTDk" style="zoom: 33%;" />





<img src="https://i.loli.net/2020/03/02/ZhKupy5dwkm6Qci.png" alt="ZhKupy5dwkm6Qci" style="zoom: 50%;" />



<img src="https://i.loli.net/2020/03/02/OzEniY3MoPCAWbx.png" alt="OzEniY3MoPCAWbx" style="zoom:50%;" />



## Ssh 登录

利用 ssh config editor



## 从零配置服务器

### oh my zsh 安装及美化

```shell
yum update
yum install git
yum -y install zsh # yum -y reinstall zsh # 覆盖安装
cat /etc/shells
chsh -s /bin/zsh
# git wget curl 任选其一，wget 没有成功过
# git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
# sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

# 3 个插件
# autojump、zsh-autosuggestion 以及 zsh-syntax-highlighting
git clone git://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions
git clone git://github.com/zsh-users/zsh-syntax-highlighting $ZSH_CUSTOM/plugins/zsh-syntax-highlighting

git clone git://github.com/wting/autojump.git
cd autojump/
python ./install.py
# Please manually add the following line(s) to ~/.zshrc:
#[[ -s /root/.autojump/etc/profile.d/autojump.sh ]] && source /root/.autojump/etc/profile.d/autojump.sh
#autoload -U compinit && compinit -u

vim ~/.zshrc

plugins=(
  git
  autojump
  zsh-autosuggestions
  zsh-syntax-highlighting
)

# rsync 免密传输
#  本机：
/usr/local/bin/rsync -azvhP ~/.ssh/id_rsa.pub root@47.93.53.47:/root/.ssh
#  远程：
cd .ssh;cat id_rsa.pub > authorized_keys

# nerd-fonts 下载
mkdir -p ~/.local/share/fonts
cd ~/.local/share/fonts && curl -fLo "Droid Sans Mono for Powerline Nerd Font Complete.otf" https://github.com/ryanoasis/nerd-fonts/raw/master/patched-fonts/DroidSansMono/complete/Droid%20Sans%20Mono%20Nerd%20Font%20Complete.otf

# powerlevel9k 主题下载
git clone https://github.com/bhilburn/powerlevel9k.git ~/.oh-my-zsh/themes/powerlevel9k
# You then need to select this theme in your ~/.zshrc:
ZSH_THEME="powerlevel9k/powerlevel9k"

rsync -azvhP ~/.vimrc root@47.93.53.47:/root/
rsync -azvhP ~/.dotfiles/.powerlevel9k root@47.93.53.47:/root/

# colosls 安装
yum install gcc-c++ patch readline readline-devel zlib zlib-devel ibffi-devel openssl-devel make bzip2 autoconf automake libtool bison sqlite-devel
curl -sSL https://rvm.io/mpapis.asc | gpg2 --import -
curl -sSL https://rvm.io/pkuczynski.asc | gpg2 --import -
curl -L get.rvm.io | bash -s stable
source /etc/profile.d/rvm.sh
rvm reload
rvm requirements run
rvm list known
ruby --version
rvm install 2.7
rvm use ruby-2.7.0-preview1
rvm use 2.7 --defaults
gem install colorls

alias ls='colorls -A'
alias lc='colorls -lA --sd'
```

### Java 8

```shell
yum install java-1.8.0-openjdk
java -version
export JAVA_HOME='/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.242.b08-0.el7_7.x86_64/jre/bin/java'
echo JAVA_HOME

# Find Java’s Path
update-alternatives --config java
```

### nodejs cnpm

```shell
yum install -y gcc-c++ make
curl -sL https://rpm.nodesource.com/setup_12.x | sudo -E bash -
sudo yum install nodejs
node -v
npm install -g cnpm --registry=https://registry.npm.taobao.org
```



[阿里云服务器端口8080开放-百度经验](https://jingyan.baidu.com/article/95c9d20d624d1eec4e756125.html)

### Rsync 报错

```shell
rsync: on remote machine: -vlogDtpXre.iLsfxC: unknown option
rsync error: syntax or usage error (code 1) at /BuildRoot/Library/Caches/com.apple.xbs/Sources/rsync/rsync-52.200.1/rsync/main.c(1337) [server=2.6.9]
rsync: connection unexpectedly closed (0 bytes received so far) [sender]
rsync error: error in rsync protocol data stream (code 12) at io.c(226) [sender=3.1.3]
```

是版本不一致导致的

```shell
brew install rsync
where rsync
# 修改~/.alias
alias rsync=/usr/local/bin/rsync
```



### 安装指定版本 ruby

因 yum install ruby 的版本太低，需要用 rvm 安装指定版本的 ruby

[How To Install Ruby on CentOS/RHEL 7/6 - TecAdmin](https://tecadmin.net/install-ruby-latest-stable-centos/)



## python3.8 安装

```shell
# wget https://www.python.org/ftp/python/3.8.0/Python-3.8.0.tgz
# 下载太慢，在本机下载好之后传过去
rsync -azvhP Python-3.8.0.tgz root@47.93.53.47:/root/
tar zxf Python-3.8.0.tgz
cd Python-3.8.0
# 编译前准备
yum install  -y gcc-c++ gcc make cmake zlib-devel bzip2-devel openssl-devel ncurse-devel libffi-devel
# 编译安装 Python3.8
./configure prefix=/usr/local/python3 --enable-optimizations
make && make install
export PATH=$PATH:/usr/local/python3/bin/

pip3 install --upgrade pip
```





