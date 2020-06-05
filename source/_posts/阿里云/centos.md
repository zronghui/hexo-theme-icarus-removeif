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

[云服务器管理控制台](https://ecs.console.aliyun.com/?spm=5176.13329450.home-res.list-id.6baf4df5QGfirl#/server/region/cn-beijing)



## 【阿里云】高校学生“在家实践”计划

[【阿里云】高校学生“在家实践”计划](https://developer.aliyun.com/adc/student/?spm=a2c6h.13788096.J_7970846300.1.75da7638uOFYrv&accounttraceid=45ab3d56815a4be6ab3feac336a73784hxcl#ecscolleges-collocation-stu)

<img src="https://i.loli.net/2020/03/02/WxfQkdBtVR56gYa.png" alt="WxfQkdBtVR56gYa" style="zoom:50%;" />

## 登录问题



<img src="https://i.loli.net/2020/03/02/uEcSq12LNJgMTDk.png" alt="uEcSq12LNJgMTDk" style="zoom: 33%;" />





<img src="https://i.loli.net/2020/03/02/ZhKupy5dwkm6Qci.png" alt="ZhKupy5dwkm6Qci" style="zoom: 50%;" />



<img src="https://i.loli.net/2020/03/02/OzEniY3MoPCAWbx.png" alt="OzEniY3MoPCAWbx" style="zoom:50%;" />



## Ssh 登录

利用 ssh config editor





## oh my zsh 安装及美化

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

## Java 8

[Java SE Development Kit 8 - Downloads](https://www.oracle.com/java/technologies/javase-jdk8-downloads.html)
[国内借助阿里云快速下载Oracle JDK-鹞之神乐](https://www.kagura.me/dev/20200424112618.html)

下载 jdk-8u251-linux-x64.tar.gz

```shell
# yum 安装好像有问题
# yum install java-1.8.0-openjdk
# java -version
# export JAVA_HOME='/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.242.b08-0.el7_7.x86_64/jre/bin/java'

rsync -azvhP ~/Downloads/Compressed/jdk-8u251-linux-x64.tar.gz root@47.93.53.47:/tmp
sudo tar xf /tmp/jdk-8u251-linux-x64.tar.gz -C /usr/java

export JAVA_HOME=/usr/java/jdk1.8.0_251
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar

echo JAVA_HOME

# Find Java’s Path
update-alternatives --config java
```

## maven

[在 CentOS 8 上安装 Maven - GoBeta](https://www.gobeta.net/linux/how-to-install-apache-maven-on-centos-8/)

[Maven – Maven Documentation](https://maven.apache.org/guides/index.html)

```shell
# 首先使用以下 wget 命令在 /tmp 目录中下载 Apache Maven
# wget https://www-us.apache.org/dist/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz -P /tmp
# 下载速度慢，command 点击链接下载后，用 rsync 拷贝
rsync -azvhP ~/Downloads/Compressed/apache-maven-3.6.3-bin.tar.gz root@47.93.53.47:/tmp

sudo tar xf /tmp/apache-maven-3.6.3-bin.tar.gz -C /opt
# 为了更好地控制 Maven 版本和更新，我们将 创建一个符号链接   maven ，该链接指向 Maven 安装目录
sudo ln -s /opt/apache-maven-3.6.3 /opt/maven
# 要升级您的 Maven 安装，只需解压缩较新的版本并更改符号链接以指向它即可

# 设置环境变量
vim /etc/profile.d/maven.sh

export JAVA_HOME=/usr/java/jdk1.8.0_251
export M2_HOME=/opt/maven
export MAVEN_HOME=/opt/maven
export PATH=${M2_HOME}/bin:${PATH}

chmod u+x /etc/profile.d/maven.sh
source /etc/profile.d/maven.sh

mvn --version

# 代理设置
vim /opt/maven/conf/settings.xml
# 找到 mirrors，添加：
<mirror>
  <id>nexus-aliyun</id>
  <mirrorOf>central</mirrorOf>
  <name>Nexus aliyun</name>
  <url>http://maven.aliyun.com/nexus/content/groups/public</url>
</mirror>
```



**到时候注意 /etc/profile.d 这个目录也需要备份**

## nodejs cnpm

```shell
yum install -y gcc-c++ make
curl -sL https://rpm.nodesource.com/setup_12.x | sudo -E bash -
sudo yum install nodejs
node -v
npm install -g cnpm --registry=https://registry.npm.taobao.org
```



[阿里云服务器端口8080开放-百度经验](https://jingyan.baidu.com/article/95c9d20d624d1eec4e756125.html)

## Rsync 报错

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



## 安装指定版本 ruby

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



## docker 安装

[Install Docker Engine on CentOS | Docker Documentation](https://docs.docker.com/engine/install/centos/#install-docker-engine)

```shell
# 卸载旧版本
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
                  
# 用存储库进行安装
# 安装 yum-utils 包(它提供 yum-config-manager 实用工具)并设置稳定存储库
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
# 安装 DOCKER 引擎
sudo yum install docker-ce docker-ce-cli containerd.io
# 验证指纹是否与060A 61c51b558a7f 742B 77AA C52F EB6B 621E 9F35匹配，如果是，接受它

# 启动 Docker
sudo systemctl start docker
sudo docker run hello-world
# 开机自启
sudo systemctl enable docker
```

镜像加速

配置文件: /etc/docker/daemon.json

阿里云 ID 见：https://cr.console.aliyun.com/undefined/instances/mirrors

```json
{
  "registry-mirrors": ["https://my-id.mirror.aliyuncs.com", "http://hub-mirror.c.163.com"]
}
```



```shell
sudo systemctl restart docker
# 或者
sudo systemctl stop docker
sudo systemctl start docker
```



docker info 查看是否生效



## go

[CentOS 7 安装Golang - 简书](https://www.jianshu.com/p/8f0646e3858c)
[Go下载 - Go语言中文网 - Golang中文社区](https://studygolang.com/dl)

```shell
wget 上面 Linux 的链接
sudo tar -C /usr/local/ -xzvf go1.10.2.linux-amd64.tar.gz
vim ~/.zshrc

export GOROOT=/usr/local/go
export GOPATH=~/goProject 
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin

export GOBIN=$GOPATH/bin

go version
-> go version go1.14.3 linux/amd64


```

```shell
mkdir -p ~/goProject/src/test
cd ~/goProject/src/test
vim test.go

package main

import "fmt"
func main() {
    fmt.Println("Hello Golang")
}

./test
```



## php 安装

```shell
rpm -qa|grep php
# 逐个卸载
yum -y remove php*

rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm

yum -y install php72w php72w-cli php72w-common php72w-devel php72w-embedded php72w-fpm php72w-gd php72w-mbstring php72w-mysqlnd php72w-opcache php72w-pdo php72w-xml
yum install php72w-bcmath
```



php 安装后配置需要 pdo_sqlite.so ，用 grep 找到需要的配置文件，将其注释

```shell
grep -Hrv ";" /etc/php.d | grep -E "extension(\s+)?=pdo_sqlite.so"
vim /etc/php.d/pdo_sqlite.ini
# 用; 注释需要 sqlite 的那行
# ; Enable pdo_sqlite extension module
# ;extension=pdo_sqlite.so
```



## tmux 安装

https://github.com/tmux/tmux

```shell
brew install tmux

# centos
j test
git clone https://github.com/tmux/tmux
git clone https://github.com/tmux/tmux.git
cd tmux
sh autogen.sh && ./configure && make
which tmux
./tmux -V
mv ./tmux /usr/bin/
tmux
```



