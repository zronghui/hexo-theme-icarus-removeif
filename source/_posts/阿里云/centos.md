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
yum install git lsof -y
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



## redis

[centos安装redis - 掘金](https://juejin.im/post/5ecc754bf265da770f51f373)

```shell
# wget http://download.redis.io/releases/redis-5.0.7.tar.gz
rsync -azvhP ~/Downloads/Compressed/redis-5.0.7.tar.gz  root@47.93.53.47:/tmp
cd /tmp
tar xf /tmp/redis-5.0.7.tar.gz
cd redis-5.0.7
make
make PREFIX=/usr/local/redis install

mkdir /usr/local/redis/etc
cp redis.conf /usr/local/redis/etc
vim /usr/local/redis/etc/redis.conf

1）配置redis为后台启动：daemonize no  修改为 daemonize yes
2）开启外网访问：bind 127.0.01  注释掉
3）配置密码：requirepass 设置密码

pip install iredis
iredis --raw

vim ~/.zshrc
export PATH=/usr/local/redis/bin:$PATH

# 启动
redis-server /usr/local/redis/etc/redis.conf

iredis --raw
```



## netdata 监控

装上发现好像也没什么用处



官网说可以一行命令安装，然而需要翻墙，centos 翻墙很不方便

[All your monitoring education in one place. Learn | Learn](https://learn.netdata.cloud/)

[Centos安装Netdata - 掘金](https://juejin.im/post/5d005db06fb9a07ec75513a0)

[记一次Centos7安装NetData_一花一世界-CSDN博客](https://blog.csdn.net/llwy1428/article/details/98726043)

端口 19999

```shell
# 准备
yum install autoconf automake curl gcc git libmnl-devel libuuid-devel lm_sensors make MySQL-python nc pkgconfig python python-psycopg2 PyYAML zlib-devel -y
yum install zlib-devel gcc make git autoconf autogen guile-devel automake pkgconfig uuid-dev libuuid-devel vim lrzsz -y
# 安装
git clone https://github.com/firehol/netdata.git --depth=1
cd netdata
# 安装较慢，但是可以成功，需要等待
./netdata-installer.sh
# 检验
curl 127.0.0.1:19999
# 开机自启
systemctl enable netdata

# 过程中执行了以下命令，若安装失败可以试试
wget https://github.com/libuv/libuv/archive/v1.34.0.tar.gz
tar -zxvf v1.34.0.tar.gz
cd libuv-1.34.0
sh autogen.sh
./configure
make
make check
make install
ldconfig -v


It will be installed at these locations:
   - the daemon     at /usr/sbin/netdata
   - config files   in /etc/netdata
   - web files      in /usr/share/netdata
   - plugins        in /usr/libexec/netdata
   - cache files    in /var/cache/netdata
   - db files       in /var/lib/netdata
   - log files      in /var/log/netdata
   - pid file       at /var/run/netdata.pid
   - logrotate file at /etc/logrotate.d/netdata
```



## **Centos7 安装部署 jiacrontab web

[iwannay/jiacrontab: 简单可信赖的任务管理工具](https://github.com/iwannay/jiacrontab)

**简介**：

提供可视化界面的定时任务管理工具。

允许设置每个脚本的超时时间，超时操作可选择邮件通知管理者，或强杀脚本进程。
允许设置脚本的最大并发数。
一台server管理多个client。
每个脚本都可在server端灵活配置，如测试脚本运行，查看日志，强杀进程，停止定时

。。。

```shell
gcl https://github.com/iwannay/jiacrontab.git
cd jiacrontab
make build
cd build/jiacrontab/jiacrontab_admin/
nohup ./jiacrontab_admin &> jiacrontab_admin.log &
- # 返回上级目录
cd build/jiacrontab/jiacrontabd
nohup ./jiacrontabd &> jiacrontabd.log &
```

去 47.93.53.47:20000 填写管理员账号密码

配置定时任务的地方：

<img src="https://i.loli.net/2020/06/14/wuZJikAQpNDS9Yd.png" alt="image-20200614202318387" style="zoom:50%;" />







## Centos 7系统优化脚本

[Centos 7系统优化脚本 - 掘金](https://juejin.im/post/5d80da83e51d4561fd6cb591)



## supervisor

[Supervisor: A Process Control System — Supervisor 4.2.0 documentation](http://supervisord.org/)

[supervisor用法 - 掘金](https://juejin.im/post/5d80da83e51d45620c1c5471)

用途：

**提供了一种统一的方式来start、stop、monitor你的进程**

**可以在本地或者远程命令行或者web接口来配置Supervisor**

在linux下的很多程序通常都是一直运行着的，一般来说都需要自己编写一个能够实现进程start/stop/restart/reload功能的脚本，然后放到**/etc/init.d/**下面。但这样做也有很多弊端，第一我们要为每个程序编写一个类似脚本，第二，当这个进程挂掉的时候，linux不会自动重启它的，想要自动重启的话，我们还要自己写一个监控重启脚本。

 而supervisor则可以完美的解决这些问题。supervisor管理进程，就是通过fork/exec的方式把这些被管理的进程，当作supervisor的子进程来启动。这样的话，我们**只要在supervisor的配置文件中，把要管理的进程的可执行文件的路径写进去就OK了**（**优点一：配置方便**）。第二，被管理进程作为supervisor的子进程，**当子进程挂掉的时候，父进程可以准确获取子进程挂掉的信息的，所以当然也就可以对挂掉的子进程进行自动重启**，当然重启还是不重启，也要看你的配置文件里面有木有设置autostart=true了。 supervisor通过INI格式配置文件进行配置，很容易掌握，它为每个进程提供了很多配置选项，可以使你很容易的重启进程或者自动的轮转日志。
supervisor**可以对进程组统一管理**，也就是说咱们**可以把需要管理的进程写到一个组里面**，然后我们把这个组作为一个对象进行管理，如**启动，停止，重启**等等操作。而linux系统则是没有这种功能的，我们想要停止一个进程，只能一个一个的去停止，要么就自己写个脚本去批量停止。

```shell
pip install supervisor
echo_supervisord_conf > /etc/supervisord.conf
mkdir /etc/supervisord.d/
vim /etc/supervisord.conf
用 / 搜索 9001

[include]
files = /etc/supervisord.d/*.conf
[inet_http_server]         ; inet (TCP) server disabled by default
port=*:9001        ; ip_address:port specifier, *:port for all iface
username=zronghui              ; default is no username (open server)
password=a123456               ; default is no password (open server)


supervisord -c /etc/supervisord.conf
curl 127.0.0.1:9001

# 若想 kill 已有的 supervisor 进程：
ps -ef|grep super
kill 'pid'

```

### supervisor管理

```shell
用 supervisorctl

update 更新新的配置到supervisord（不会重启原来已运行的程序）
reload，载入所有配置文件，并按新的配置启动、管理所有进程（会重启原来已运行的程序）
start xxx: 启动某个进程
restart xxx: 重启某个进程
stop xxx: 停止某一个进程(xxx)，xxx为[program:theprogramname]里配置的值
stop groupworker: 重启所有属于名为groupworker这个分组的进程(start,restart同理)
stop all，停止全部进程，注：start、restart、stop都不会载入最新的配置文
reread，当一个服务由自动启动修改为手动启动时执行一下就ok
```





![image-20200614212200846](https://i.loli.net/2020/06/14/gj3lNsewnGkHKVP.png)
