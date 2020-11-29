---
title: 阿里云
toc: true
uniqueId: '2020-03-02 00:11:19/"阿里云".html'
encrypt: true
password: 1
abstract: 咦，这是一篇加密文章，好像需要输入密码才能查看呢！
message: 嗨，请准确无误地输入密码查看哟
wrong_pass_message: 不好意思，密码没对哦，在检查检查呢！
wrong_hash_message: 不好意思，信息无法验证！
date: 2020-03-02 08:11:19
thumbnail:
categories:
- 阿里云
tags:
keywords:
---


[TOC]

<!--more-->

centos ubuntu 各种软件安装教程汇总：

[Linux Tips, Tricks and Tutorials | Linuxize](https://linuxize.com/)



阿里云 114 一年

[阿里云学生机-云服务器学生机优惠-学生机推荐-云翼计划-阿里云](https://promotion.aliyun.com/ntms/act/campus2018.html?spm=5176.14145661.J_3598540520.ace-channel-latest-activity-card.48b51875YXgkTo)

[阿里云云服务器管理控制台](https://ecs.console.aliyun.com/?spm=5176.13329450.home-res.list-id.6baf4df5QGfirl#/server/region/cn-beijing)



## 【阿里云】高校学生“在家实践”计划

[【阿里云】高校学生“在家实践”计划](https://developer.aliyun.com/adc/student/?spm=a2c6h.13788096.J_7970846300.1.75da7638uOFYrv&accounttraceid=45ab3d56815a4be6ab3feac336a73784hxcl#ecscolleges-collocation-stu)

<img src="https://i.loli.net/2020/03/02/WxfQkdBtVR56gYa.png" alt="WxfQkdBtVR56gYa" style="zoom: 25%;" />

## 登录问题



<img src="https://i.loli.net/2020/03/02/uEcSq12LNJgMTDk.png" alt="uEcSq12LNJgMTDk" style="zoom: 33%;" />





<img src="https://i.loli.net/2020/03/02/ZhKupy5dwkm6Qci.png" alt="ZhKupy5dwkm6Qci" style="zoom: 50%;" />



<img src="https://i.loli.net/2020/03/02/OzEniY3MoPCAWbx.png" alt="OzEniY3MoPCAWbx" style="zoom:50%;" />



## Ssh 登录

利用 ssh config editor

## 时间同步

[[Centos 7 telnet 安装与配置 - 简书](https://www.jianshu.com/p/42f6443fa717)]([https://www.jianshu.com/p/42f6443fa717](ticktick://ttMarkdownLink))

[[CentOS 7同步时间的2种方法 - 小z博客](https://www.xiaoz.me/archives/12989)]([https://www.xiaoz.me/archives/12989](ticktick://ttMarkdownLink))

## centos ubuntu

centos yum

ubuntu apt-get

基本用法一致，可以在 Ubuntu 中 alias yum=apt-get 然后用法就一致了



## yum 换源

```shell
wget http://mirrors.163.com/.help/CentOS7-Base-163.repo
mv CentOS7-Base-163.repo /etc/yum.repos.d
yum makecache
```





## *tmux 安装

https://github.com/tmux/tmux

```shell
brew install tmux

# 简单安装 2.1版本
yum/apt install tmux

# 最新版本(ubuntu 老是安装失败)
j test
# git clone https://github.com/tmux/tmux
# 加速镜像
git clone https://github.com.cnpmjs.org/tmux/tmux.git
cd tmux
yum/apt install automake
sh autogen.sh && ./configure && make
which tmux
./tmux -V
mv ./tmux /usr/bin/
tmux
```



## *oh my zsh 安装及美化

### 基础安装

```shell
# centos
yum update
yum install git lsof -y
yum -y install zsh # yum -y reinstall zsh # 覆盖安装
# ubuntu
apt-get install zsh


cat /etc/shells
chsh -s /bin/zsh
# git wget curl 任选其一，wget 没有成功过
# git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
# sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"

# 进入网页，复制代理地址 https://github.com/ohmyzsh/ohmyzsh/blob/master/tools/install.sh

#sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
sh -c "$(curl -fsSL https://cdn.jsdelivr.net/gh/ohmyzsh/ohmyzsh@master/tools/install.sh)"


# 3 个插件
# autojump、zsh-autosuggestion 以及 zsh-syntax-highlighting
git clone git://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions
git clone git://github.com/zsh-users/zsh-syntax-highlighting $ZSH_CUSTOM/plugins/zsh-syntax-highlighting

git clone https://shrill-pond-3e81.hunsh.workers.dev/github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions
git clone https://shrill-pond-3e81.hunsh.workers.dev/github.com/zsh-users/zsh-syntax-highlighting $ZSH_CUSTOM/plugins/zsh-syntax-highlighting

# git clone git://github.com/wting/autojump.git
# cd autojump/
# python ./install.py

# Please manually add the following line(s) to ~/.zshrc:
#[[ -s /root/.autojump/etc/profile.d/autojump.sh ]] && source /root/.autojump/etc/profile.d/autojump.sh
#autoload -U compinit && compinit -u

cd ~/.oh-my-zsh/plugins
wget https://cdn.jsdelivr.net/gh/skywind3000/z.lua@master/z.lua

vim ~/.zshrc

plugins=(
  git
  zsh-autosuggestions
  zsh-syntax-highlighting
)
eval "$(lua ~/.oh-my-zsh/plugins/z.lua  --init zsh)"


source ~/.zshrc

# 导入 autojump 的数据
# mac 用 brew 安装 autojump 的备份位置与其他方式不同
FN="/Users/zhangronghui/Library/autojump/autojump.txt"
awk -F '\t' '{print $2 "|" $1 "|" 0}' $FN >> ~/.zlua

```

目录跳转相关：

[终端目录跳转软件 autojump 和 z 使用介绍 - 知乎](https://zhuanlan.zhihu.com/p/52401463)
[rupa/z: z - jump around](https://github.com/rupa/z)
[终端 - 快速目录跳转 - z_lua](https://juejin.im/post/6844903955504300040)

最终解答：

[z.lua/README.cn.md at master · skywind3000/z.lua](https://github.com/skywind3000/z.lua/blob/master/README.cn.md)



### 美化

没时间可以不弄

```shell
# rsync 免密传输
#  本机：
/usr/local/bin/rsync -azvhP ~/.ssh/id_rsa.pub root@47.93.53.47:/root/.ssh
#  远程：
cd .ssh;cat id_rsa.pub > authorized_keys

# nerd-fonts 下载
mkdir -p ~/.local/share/fonts
cd ~/.local/share/fonts 
cp ~/zronghui/Droid\ Sans\ Mono\ for\ Powerline\ Nerd\ Font\ Complete.otf .
# curl -fLo "Droid Sans Mono for Powerline Nerd Font Complete.otf" https://github.com/ryanoasis/nerd-fonts/raw/master/patched-fonts/DroidSansMono/complete/Droid%20Sans%20Mono%20Nerd%20Font%20Complete.otf

# powerlevel9k 主题下载
git clone https://github.com/bhilburn/powerlevel9k.git ~/.oh-my-zsh/themes/powerlevel9k
# You then need to select this theme in your ~/.zshrc:
ZSH_THEME="powerlevel9k/powerlevel9k"

rsync -azvhP ~/.vimrc root@47.93.53.47:/root/
rsync -azvhP ~/.dotfiles/.powerlevel9k root@47.93.53.47:/root/

# colosls 安装

# gem 安装
# centos
yum install -y gcc-c++ patch readline readline-devel zlib zlib-devel ibffi-devel openssl-devel make bzip2 autoconf automake libtool bison sqlite-devel

# ubuntu 
apt-get install -y git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties libffi-dev nodejs mysql-client libmysqlclient-dev git htop


curl -sSL https://rvm.io/mpapis.asc | gpg2 --import -
curl -sSL https://rvm.io/pkuczynski.asc | gpg2 --import -
curl -L get.rvm.io | bash -s stable
source /etc/profile.d/rvm.sh
rvm reload
rvm requirements run
rvm list known
rvm install 2.7  # 可能需要重试许多次，可以弄个失败自动重试的脚本。不要尝试直接从文件安装, 我失败了
# rvm use ruby-2.7.0-preview
rvm use 2.7 --default
# ruby --version
gem install colorls

alias ls='colorls -A'
alias lc='colorls -lA --sd'
```

## bat 安装

[sharkdp/bat: A cat(1) clone with wings.](https://github.com/sharkdp/bat)
[how to install on CentOS 7? · Issue #325 · sharkdp/bat](https://github.com/sharkdp/bat/issues/325)



### centos

这里用下面这个脚本就好了

```shell
vim install_bat.sh
# 拷贝下面的脚本
chmod u+x install_bat.sh
./install_bat.sh
```



install_bat.sh

```shell
#!/bin/sh

red=`tput setaf 1`
green=`tput setaf 2`
bold=`tput bold`
reset=`tput sgr0`

echo ""
echo "${bold}${green}Installing the latest CentOS release version of bat command from GitHub...${reset}"

echo ""
echo "${bold}Note${reset}: For more information, please see https://github.com/sharkdp/bat."

echo ""
echo "Finding the latest version tag from Github..."

BAT_VERSION=$(curl --silent "https://api.github.com/repos/sharkdp/bat/releases/latest" | grep -Eo '"tag_name": "v(.*)"' | sed -E 's/.*"([^"]+)".*/\1/')
BAT_RELEASE="bat-$BAT_VERSION-x86_64-unknown-linux-musl"
BAT_ARCHIVE="$BAT_RELEASE.tar.gz"

echo ""
echo "Version ${bold}$BAT_VERSION${reset} found, downloading ${bold}$BAT_ARCHIVE${reset} from GitHub..."

# https://github.com.cnpmjs.org/sharkdp/bat/releases/download/v0.16.0/bat-musl_0.16.0_amd64.deb
# curl -sOL "https://github.com/sharkdp/bat/releases/download/$BAT_VERSION/$BAT_ARCHIVE"
# 这里用了代理
curl -sOL "https://github.com.cnpmjs.org/sharkdp/bat/releases/download/$BAT_VERSION/$BAT_ARCHIVE"

echo ""
echo "Unarchiving ${bold}$BAT_ARCHIVE${reset} to ${bold}$HOME/$BAT_RELEASE${reset}..."

tar xzvf $BAT_ARCHIVE -C $HOME/

echo ""
echo "Copying executable to ${bold}/usr/local/bin/bat${reset}..."

sudo sh -c "cp $HOME/$BAT_RELEASE/bat /usr/local/bin/bat"

echo ""
echo "Removing ${bold}$BAT_ARCHIVE${reset} and cleaning up..."

rm $BAT_ARCHIVE

unset BAT_ARCHIVE
unset BAT_RELEASE
unset BAT_VERSION

if command -v bat &> /dev/null
then
  echo ""
  echo "${bold}${green}Finished installing $(bat --version).${reset}"
else
  echo ""
  echo "${bold}${red}Installation failed! Please examine this script and try steps manually.${reset}"
  exit 1
fi

```



## *ssh/rsync 免密码

```shell
# 本地：
ssh-keygen
/usr/local/bin/rsync -azvhP ~/.ssh/id_rsa.pub root@47.93.53.47:/root/.ssh
#  远程：
cd .ssh;cat id_rsa.pub > authorized_keys
```

## *python3.8 安装

[python3.8.1 安装 - 安安](https://blog.90.vc/archives/371)

```shell
# wget https://www.python.org/ftp/python/3.8.0/Python-3.8.0.tgz
# 下载太慢，在本机下载好之后传过去
rsync -azvhP Python-3.8.0.tgz root@47.93.53.47:/root/
tar zxf Python-3.8.0.tgz
cd Python-3.8.0

# 编译前准备
# centos
yum install  -y gcc-c++ gcc make cmake zlib-devel bzip2-devel openssl-devel ncurse-devel libffi-devel
# ubuntu
apt install software-properties-common
apt install build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev libsqlite3-dev wget libbz2-dev

# 编译安装 Python3.8
./configure prefix=/usr/local/python3
make && make install
export PATH=$PATH:/usr/local/python3/bin/

# 换源
# [修改pip镜像地址 - 安安](https://blog.90.vc/archives/369)
mkdir ~/.pip
cat > ~/.pip/pip.conf << EOF
[global]
index-url=http://mirrors.aliyun.com/pypi/simple/

[install]
trusted-host=mirrors.aliyun.com
EOF


pip3 install --upgrade pip
pip3 install ipython
```

常用Python库安装

```shell
yum update
yum install python3-pip

# alias pip='pip3.8'
alias pip='/usr/local/python3/bin/pip3.8'
#alias python=python3.8
alias python='/usr/local/python3/bin/python3.8'
pip install --upgrade pip
pip install -U six
pip install matplotlib numpy pretty_errors icecream wget environs ipython pandas xlrd pylightxl jupyter
# pip3 install iredis
/usr/local/python3/bin/pip3.8 install -r /root/code/zronghui_xxxtrequirements.txt
```



### aliyun centos jupyter 安装

参考：

[阿里云Centos7下搭建Jupyter Notebook服务（亦可自建外网访问） - ❤····随手 - 我家Ai智障](http://www.mclover.cn/blog/index.php/archives/157.html)

```shell
pip3 install jupyter
jupyter notebook --generate-config
开头输入以下内容：
c.NotebookApp.allow_root = True
c.NotebookApp.ip = '*'
c.NotebookApp.open_browser = False
c.NotebookApp.port = 20006

jupyter notebook
```

注:  输出配置文件的非注释行

```shell
grep -E  -v "^#|^$" jupyter_notebook_config.py 
```



## *docker 安装

### centos

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
sudo yum install -y docker-ce docker-ce-cli containerd.io
# 验证指纹是否与060A 61c51b558a7f 742B 77AA C52F EB6B 621E 9F35匹配，如果是，接受它

# 启动 Docker
sudo systemctl start docker
sudo docker run hello-world
# 开机自启
sudo systemctl enable docker


# docker compose
[Install Docker Compose | Docker Documentation](https://docs.docker.com/compose/install/)
```

### ubuntu

[How to Install Docker on Ubuntu 20.04 | Linuxize](https://linuxize.com/post/how-to-install-and-use-docker-on-ubuntu-20-04/)

```shell
apt update
apt install docker-ce docker-ce-cli containerd.io
apt update
apt list -a docker-ce
systemctl status docker
docker container run hello-world

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

## *Java 8

[Java SE Development Kit 8 - Downloads](https://www.oracle.com/java/technologies/javase-jdk8-downloads.html)
[国内借助阿里云快速下载Oracle JDK-鹞之神乐](https://www.kagura.me/dev/20200424112618.html)

下载 jdk-8u251-linux-x64.tar.gz

```shell
# yum 安装好像有问题
# yum install java-1.8.0-openjdk
# java -version
# export JAVA_HOME='/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.242.b08-0.el7_7.x86_64/jre/bin/java'

rsync -azvhP ~/Downloads/Compressed/jdk-8u251-linux-x64.tar.gz root@47.93.53.47:/tmp
mkdir /usr/java
sudo tar -xf /tmp/jdk-8u251-linux-x64.tar.gz -C /usr/java

export JAVA_HOME=/usr/java/jdk1.8.0_251
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar

echo $JAVA_HOME

# Find Java’s Path
update-alternatives --config java
```



## *redis

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
# 开机自启
crontab -e
@reboot redis-server /usr/local/redis/etc/redis.conf

iredis --raw
```



redis-cli -h 47.93.53.47 -p 6379 slaveof 101.200.240.225 -p 6379

### 解决redis远程连接不上的问题

```shell

```



redis现在的版本开启redis-server后，redis-cli只能访问到127.0.0.1，因为在配置文件中固定了ip，因此需要修改redis.conf（有的版本不是这个文件名，只要找到相对应的conf后缀的文件即可）文件以下几个地方。

1.bind 127.0.0.1改为 #bind 127.0.0.1 (注释掉)

2.protected-mode yes 改为 protected-mode no





### *redis 迁移

[redis-utils-cli - npm](https://www.npmjs.com/package/redis-utils-cli)

```shell
# 在 47.93.53.47 中
npm install redis-utils-cli -g
redis-utils copy 47.93.53.47 101.200.240.225

# 在此之前，需要在 2  个服务器的redis 执行以下操作
iredis --raw
CONFIG SET protected-mode no
```

## *job

将命令行规划成任务

[liujianping/job: JOB, make your short-term command as a long-term job. 将命令行规划成任务的工具](https://github.com/liujianping/job)

安装：

```shell
wget -O - -q https://raw.githubusercontent.com/liujianping/job/master/install.sh | sh -s
cp /root/code/test/job/bin/job /usr/bin/
```



注意若 job s ./dailyreport.sh

sh 文件需以 #!/usr/bin/env bash 开头，否则 failed: repeat 0 failed: fork/exec /root/dailyreport.sh: exec format error



```shell
$: job -h
Job, make your short-term command as a long-term job

Usage:
  job [flags] [command args ...]

Examples:

	(simple)      $: job echo hello
	(schedule)    $: job -s "* * * * *" -- echo hello
	(retry)       $: job -r 3 -- echox hello
	(repeat)      $: job -n 10 -i 100ms -- echo hello
	(concurrent)  $: job -c 10 -n 10 -- echo hello
	(timeout cmd) $: job -t 500ms -- sleep 1
	(timeout job) $: job -T 3s -r 4 -- sleep 1

Flags:
  -t, --cmd-timeout duration       job command timeout duration
  -c, --concurrent int             job concurrent numbers
  -h, --help                       help for job
  -T, --job-timeout duration       job timeout duration
  -i, --repeat-interval duration   job repeat interval duration
  -n, --repeat-times int           job repeat times, 0 means forever (default 1)
  -r, --retry int                  job command retry times when failed
  -s, --schedule string            job schedule in crontab format
      --version                    version for job
```



## *supervisor

[Supervisor: A Process Control System — Supervisor 4.2.0 documentation](http://supervisord.org/)

[supervisor用法 - 掘金](https://juejin.im/post/5d80da83e51d45620c1c5471)

配置文件详解：[supervisor配置文件详解](https://www.lagou.com/lgeduarticle/8145.html)

[解决unix:///tmp/supervisor.sock no such file的问题 | 一点心怡 | 前端娃](https://zanjs.com/2018/04/09/supervisord/%E8%A7%A3%E5%86%B3unix-tmp-supervisor-sock-no-such-file%E7%9A%84%E9%97%AE%E9%A2%98/)

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
username=*****              ; default is no username (open server)
password=*****               ; default is no password (open server)


supervisord -c /etc/supervisord.conf
supervisorctl reload

curl 127.0.0.1:9001

# 若想 kill 已有的 supervisor 进程：
ps -ef|grep super
kill 'pid'

```



cd /etc/supervisord.d

cat *

注意不能有~，得替换成相应的路径 如 /root

```
dailyReport.conf 以下几个配置文件类似

[program:dailyReport]
command=job -s "* * * * *" -- /root/dailyreport.sh   ; 被监控的进程路径
directory=/root               ; 执行前要不要先cd到目录去，一般不用
priority=1                    ;数字越高，优先级越高
numprocs=1                    ; 启动几个进程
autostart=true                ; 随着supervisord的启动而启动
autorestart=false              ; 自动重启。。当然要选上了
startretries=10               ; 启动失败时的最多重试次数
exitcodes=0                   ; 正常退出代码（是说退出代码是这个时就不再重启了吗？待确定）
stopsignal=KILL               ; 用来杀死进程的信号
stopwaitsecs=10               ; 发送SIGKILL前的等待时间
redirect_stderr=true          ; 重定向stderr到stdout

[program:redis]
command=redis-server /usr/local/redis/etc/redis.conf   ; 被监控的进程路径
directory=/root               ; 执行前要不要先cd到目录去，一般不用
priority=1                    ;数字越高，优先级越高
numprocs=1                    ; 启动几个进程
autostart=true                ; 随着supervisord的启动而启动
autorestart=true              ; 自动重启。。当然要选上了
startretries=10               ; 启动失败时的最多重试次数
exitcodes=0                   ; 正常退出代码（是说退出代码是这个时就不再重启了吗？待确定）
stopsignal=KILL               ; 用来杀死进程的信号
stopwaitsecs=10               ; 发送SIGKILL前的等待时间
redirect_stderr=true          ; 重定向stderr到stdout

[program:xxxt]
command=/usr/local/python3/bin/python3.8 manage.py runserver 0.0.0.0:8033 --insecure   ; 被监控的进程路径
directory=/root/code/zronghui_xxxt/SearchWeb               ; 执行前要不要先cd到目录去，一般不用
priority=1                    ;数字越高，优先级越高
numprocs=1                    ; 启动几个进程
autostart=true                ; 随着supervisord的启动而启动
autorestart=true              ; 自动重启。。当然要选上了
startretries=10               ; 启动失败时的最多重试次数
exitcodes=0                   ; 正常退出代码（是说退出代码是这个时就不再重启了吗？待确定）
stopsignal=KILL               ; 用来杀死进程的信号
stopwaitsecs=10               ; 发送SIGKILL前的等待时间
redirect_stderr=true          ; 重定向stderr到stdout

[program:xxxtScrapy]
enviroment=InCrontab=true ; 设置环境变量
command=job -s "*/5 * * * *" -- ./crontab.sh   ; 被监控的进程路径
directory=/root/code/zronghui_xxxt               ; 执行前要不要先cd到目录去，一般不用
priority=1                    ;数字越高，优先级越高
numprocs=1                    ; 启动几个进程
autostart=true                ; 随着supervisord的启动而启动
autorestart=false              ; 自动重启。。当然要选上了
startretries=10               ; 启动失败时的最多重试次数
exitcodes=0                   ; 正常退出代码（是说退出代码是这个时就不再重启了吗？待确定）
stopsignal=KILL               ; 用来杀死进程的信号
stopwaitsecs=10               ; 发送SIGKILL前的等待时间
redirect_stderr=true          ; 重定向stderr到stdout

[program:ssr_autocheckin]
command=job -s "* * * * *" -- /usr/local/python3/bin/python3.8 main.py   ; 被监控的进程路径
directory=/root/code/ssr_autocheckin        ; 执行前要不要先cd到目录去，一般不用
priority=1                    ;数字越高，优先级越高
numprocs=1                    ; 启动几个进程
autostart=true                ; 随着supervisord的启动而启动
autorestart=true              ; 自动重启。。当然要选上了
startretries=10               ; 启动失败时的最多重试次数
exitcodes=0                   ; 正常退出代码（是说退出代码是这个时就不再重启了吗？待确定）
stopsignal=KILL               ; 用来杀死进程的信号
stopwaitsecs=10               ; 发送SIGKILL前的等待时间
redirect_stderr=true          ; 重定向stderr到stdout
```



[supervisor添加环境变量 - 简书](https://www.jianshu.com/p/9f81b42fea2a)
[python - How to set environment variables in Supervisor service - Stack Overflow](https://stackoverflow.com/questions/17055951/how-to-set-environment-variables-in-supervisor-service)

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

## mysql

```shell
# 如果不用 docker 安装非常恶心

docker pull mysql:5.7
docker run -d --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root mysql:5.7

mkdir -p /usr/local/docker/mysql/{config,data}
# 将容器中的 mysql 配置文件复制到宿主机中指定路径下，路径你可以根据需要，自行修改
docker cp mysql:/etc/mysql/mysql.conf.d/mysqld.cnf /usr/local/docker/mysql/config
# 将容器中的 mysql 存储目录复制到宿主机中
docker cp mysql:/var/lib/mysql/ /usr/local/docker/mysql/data

docker rm -f mysql

docker run -d \
--name mysql \
-p 3306:3306 \
-v /usr/local/docker/mysql/config/mysqld.cnf:/etc/mysql/mysql.conf.d/mysqld.cnf \
-v /usr/local/docker/mysql/data/mysql:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=root \
mysql:5.7


```



## elasticsearch

安装贼麻烦，还是用 docker 吧

### docker

```shell
docker pull elasticsearch:6.8.6

docker run -d --name es686 -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:6.8.6
# docker run -d --name es686 --net dnw_es_680 -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:6.8.6


# ./bin/elasticsearch-plugin install https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v6.8.6/elasticsearch-analysis-ik-6.8.6.zip
./bin/elasticsearch-plugin install https://shrill-pond-3e81.hunsh.workers.dev/github.com/medcl/elasticsearch-analysis-ik/releases/download/v6.8.6/elasticsearch-analysis-ik-6.8.6.zip
# docker cp elasticsearch-analysis-ik-6.8.6.zip es686:/usr/share/elasticsearch
# ./bin/elasticsearch-plugin install elasticsearch-analysis-ik-6.8.6.zip

```

**elasticSearch Docker启动报错：max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]**

在宿主主机中：

```shell
echo "vm.max_map_count=262144">>/etc/sysctl.conf
/sbin/sysctl -p
```

**Failed to clear cache for realms [[]]** 可以忽略

### centos

```shell
rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
 # 文件从本地 rsync 过去
sudo apt-get install alien
sudo alien -i elasticsearch-6.8.6.rpm
# sudo rpm -ivh elasticsearch-6.8.6.rpm --nodeps

```

mkdir -p /etc/yum.repos.d

vim /etc/yum.repos.d/elasticsearch.repo

```shell
[elasticsearch-6.x]
name=Elasticsearch repository for 6.x packages
baseurl=https://artifacts.elastic.co/packages/6.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
```

```shell
# yum install 特别慢
yum makecache
yum install elasticsearch

sudo /bin/systemctl daemon-reload
sudo /bin/systemctl enable elasticsearch.service
# 启动或停止
sudo systemctl start elasticsearch.service
# sudo systemctl stop elasticsearch.service
# 检查Elasticsearch是否正在运行
curl -XGET 'localhost:9200'

export PATH=/usr/share/elasticsearch/bin:$PATH
elasticsearch-plugin install https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v6.8.8/elasticsearch-analysis-ik-6.8.8.zip
pip3 install 'elasticsearch>=6.0.0,<7.0.0'
# 重启 es，加载 ik plugin
sudo systemctl stop elasticsearch.service
sudo systemctl start elasticsearch.service

```

### utuntu

```shell
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add - 
echo "deb https://artifacts.elastic.co/packages/6.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-6.x.list
apt update
apt install elasticsearch


```

外网访问
vim /etc/elasticsearch/elasticsearch.yml 

找到 network.host，改为

network.host: 0.0.0.0



vim /etc/sysconfig/elasticsearch

设置JAVA_HOME环境变量

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







![image-20200614212200846](https://i.loli.net/2020/06/14/gj3lNsewnGkHKVP.png)



















