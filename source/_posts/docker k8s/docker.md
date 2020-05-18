---
title: docker
toc: true
recommend: 1
uniqueId: '2020-05-08 09:30:20/"docker".html'
date: 2020-05-08 17:30:20
thumbnail:
categories:
- docker k8s
tags:
keywords:
---

[TOC]

<!--more-->

## docker 服务 端口对应

gitlab 8001

yunmusic 8002

portainer 9000

## 学习资料

[动手玩Docker - 网易云课堂](https://study.163.com/course/courseLearn.htm?courseId=1002892012#/learn/video?lessonId=1003323253&courseId=1002892012)

[Docker 教程 | 菜鸟教程](https://www.runoob.com/docker/docker-tutorial.html)



## 1.docker 安装

[Install Docker Engine on CentOS | Docker Documentation](https://docs.docker.com/engine/install/centos/#install-docker-engine)

[MacOS Docker 安装 | 菜鸟教程](https://www.runoob.com/docker/macos-docker-install.html)

[Docker 镜像加速 | 菜鸟教程](https://www.runoob.com/docker/docker-mirror-acceleration.html)

### centos

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



### docker-compose

Linux 需要安装 docker-compose ；Mac win 自带



```shell
sudo yum install epel-release
#sudo pip install docker-compose # 此时 alias 失效
pip3 install docker-compose
docker-compose version

```



### Mac

[MacOS Docker 安装 | 菜鸟教程](https://www.runoob.com/docker/macos-docker-install.html)

从手动下载安装开始看

~~特慢的 brew~~



![FL5WbDVsj2myoTq](https://i.loli.net/2020/05/09/FL5WbDVsj2myoTq.png)



## 2.docker 操作

[docker 操作 · 雪之梦技术驿站](https://snowdreams1006.tech/devops/docker-ops.html)



[查看 docker 容器使用的资源 - sparkdev - 博客园](https://www.cnblogs.com/sparkdev/p/7821376.html)

- 帮助命令

```bash
docker command --help
```

- 运行容器

> `docker run [OPTIONS] IMAGE [COMMAND] [ARG...]`

```bash
docker run -it ubuntu /bin/bash
```

- 退出容器

> `exit`

- 查看容器

```bash
docker ps -a
```

- 启动容器

```bash
docker start b750bbbcfd88 
```

### 容器-container

- 后台运行

```bash
docker run -itd --name ubuntu-test ubuntu /bin/bash
```

- 停止容器

```bash
docker stop <容器 ID>
docker kill <容器 ID> # 强制停止
```

- 重启容器

```bash
docker restart <容器 ID>
```

- 进入容器

> `docker attach` 和 `docker exec`

```bash
docker attach 1e560fca3906 
```

> 注意: 如果从这个容器退出,会导致容器的停止.

```bash
docker exec -it 243c32535da7 /bin/bash
```

- 导出容器

```bash
docker export 1e560fca3906 > ubuntu.tar
```

- 导入容器

```bash
docker import - test/ubuntu:v1
```

- 删除容器

```bash
docker rm -f 1e560fca3906
```

- 清理掉所有处于终止状态的容器

```bash
docker container prune
```

- 端口映射 ？

```bash
docker port bf08b7f2cd89
```

- 查看容器日志

```bash
docker logs -f bf08b7f2cd89
```

- 查看容器进程

```bash
docker top wizardly_chandrasekhar
```

### 镜像操作

- 列出镜像

```bash
docker images
```

- 下载镜像

```bash
docker pull
```

- 查找镜像

```bash
docker search httpd
```

- 删除镜像

```bash
docker rmi hello-world
```

- 创建镜像

```bash
docker commit -m="updated" -a="snowdreams1006" eb3c83541f05 snowdreams1006/ubuntu
```

- 构建镜像

```
FROM    centos:6.7
MAINTAINER      Fisher "fisher@sudops.com"

RUN     /bin/echo 'root:123456' |chpasswd
RUN     useradd runoob
RUN     /bin/echo 'runoob:123456' |chpasswd
RUN     /bin/echo -e "LANG=\"en_US.UTF-8\"" >/etc/default/local
EXPOSE  22
EXPOSE  80
CMD     /usr/sbin/sshd -D
```

> Dockerfile

```bash
docker build -t runoob/centos:6.7 .
```

- 设置镜像标签

```bash
docker tag 860c279d2fec runoob/centos:dev
```



### web 应用

- 随机映射

```bash
docker run -d -P training/webapp python app.py
```

- 指定端口

```bash
docker run -d -p 5000:5000 training/webapp python app.py
```

- 指定地址

```bash
docker run -d -p 127.0.0.1:5001:5000 training/webapp python app.py
```

### 容器互联

- 命名容器

```bash
docker run -d -P --name runoob training/webapp python app.py
```

- 新建网络

```bash
docker network create -d bridge test-net
```

- 连接容器

```bash
docker run -itd --name test1 --network test-net ubuntu /bin/bash
```

sudo docker logs -f -t --tail 10 s12

### 常用操作(我用过的操作)：

| 命令                     | 用途                                                         |
| ------------------------ | ------------------------------------------------------------ |
| docker ps -a             | 查看所有容器                                                 |
| docker stop xxx          | 停止容器                                                     |
| docker restart xxx       | 重启容器                                                     |
| docker container prune   | 清除所有停止的容器                                           |
| -p outDocker:inDocker    | 端口映射                                                     |
| docker stats             | 类似top, 查看各个容器的 CPU mem 占用情况（为什么不是 states） |
| docker stats --no-stream | 查看一次                                                     |
| docker images            | 查看所有 image                                               |
| docker log -f xxxxx      | 查看容器日志                                                 |
| docker rmi xx            | 删除 image                                                   |
|                          |                                                              |

### 容器资源限制：内存、CPU、带宽

[Docker - 常用命令汇总2（容器资源限制：内存、CPU、带宽）](https://www.hangge.com/blog/cache/detail_2413.html)

一、内存限额
1，参数说明
2，使用样例
二、CPU 限额
1，参数说明
2，使用样例
三、Block IO 宽带限额
1，block io 权重
2，限制 bps 和 iops 



### Dockerize

[Better Docker experience with Dockerize](https://rock-it.pl/better-docker-containers-with-dockerize-wrapper/)

假设有docker-compose.yml内容如下：

```yaml
version: '2'
services:
  database:
    image: postgres

  application:
    image: registry.example.com/our-application
    command: node
    ports:
      - 80:80
```

文件没问题，但是逻辑有问题，node 应用依赖于 database，有一定几率 node 启动失败

解决办法，使用 dockerize

在 dockerfile 里添加：

```shell
ENV DOCKERIZE_VERSION v0.4.0
RUN wget https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && tar -C /usr/local/bin -xzvf dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && rm dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz
```

修改 docker-compose.yml

```yaml
services:
  ...
  application:
    image: registry.example.com/our-application
    command: dockerize -wait tcp://database:5432 node
  ...
```

更多用法、参数

```yaml
# redirect files to stdout and stderr
dockerize \
  -stdout info.log \
  -stdout perf.log \
  ...

# wait for 2 services with 10s timeout
dockerize \
  -wait tcp://db:5432 \
  -wait http://web:80 \
  -timeout 10s \
  ...

# template option
dockerize \
  -template nginx.tmpl:nginx.conf \
  ...
```



### docker django es

[elasticsearch - Docker Hub](https://hub.docker.com/_/elasticsearch?tab=description)

[Install Elasticsearch with Docker | Elasticsearch Reference [7.5] | Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/7.5/docker.html)

## 3.docker gitlab :8001

主要看 gitlab 官方文档

[GitLab Docker images | GitLab](https://docs.gitlab.com/omnibus/docker/)

[docker + gitlab · 雪之梦技术驿站](https://snowdreams1006.tech/devops/docker-gitlab.html)
[docker下gitlab安装配置使用(完整版) - 简书](https://www.jianshu.com/p/080a962c35b6)

[通过 docker 搭建自用的 gitlab 服务 - 掘金](https://juejin.im/post/5a4c9ff36fb9a04507700fcc#heading-9)
[使用 Dockcer-Compose 安装 Gitlab 服务 | Michael翔](https://michael728.github.io/2019/06/15/docker-compose-install-gitlab-runner/)
[sameersbn/docker-gitlab: Dockerized GitLab](https://github.com/sameersbn/docker-gitlab)

For Linux users set the path to /srv

export GITLAB_HOME=/srv

For Mac OS users, use the user’s $HOME folder.

export GITLAB_HOME=$HOME

```shell
export GITLAB_HOME=/srv
# export GITLAB_HOME=$HOME
sudo docker run --detach \
  --hostname gitlab.zronghui.com \
  --publish 443:443 --publish 8001:80 \
  --name gitlab \
  --restart always \
  -m 500M \
  --volume $GITLAB_HOME/gitlab/config:/etc/gitlab \
  --volume $GITLAB_HOME/gitlab/logs:/var/log/gitlab \
  --volume $GITLAB_HOME/gitlab/data:/var/opt/gitlab \
  gitlab/gitlab-ce:latest
```

**由于某些疏忽，Mac 和服务器的 gitlab home都为空**

所以：

```shell
sudo docker run --detach \
  --hostname gitlab.zronghui.com \
  --publish 443:443 --publish 8001:80 \
  --name gitlab \
  --restart always \
  --volume /gitlab/config:/etc/gitlab \
  --volume /gitlab/logs:/var/log/gitlab \
  --volume /gitlab/data:/var/opt/gitlab \
  gitlab/gitlab-ce:latest
```

### mem 占用过高问题解决：

然而并没有解决，启动不了，而且 mem 占用仍然很高，差点的服务器跑不起来

[git - High memory usage for Gitlab CE - Stack Overflow](https://stackoverflow.com/questions/36122421/high-memory-usage-for-gitlab-ce)
[Reducing the amount of memory used by gitlab - Ed Spencer - Performance obsessive web developer](https://edspencer.me.uk/posts/2017-07-30-reducing-the-amount-of-memory-used-by-gitlab/)
[【Git学习】解决GitLab内存消耗大的问题_运维_欧阳鹏-CSDN博客](https://blog.csdn.net/qq446282412/article/details/84066417)

vim /gitlab/config/gitlab.rb

```ruby
# 去除注释
unicorn['worker_processes'] = 2 # 最低为 2
postgresql['shared_buffers'] = "256MB"
postgresql['max_worker_processes'] = 1 # 默认为 8
sidekiq['concurrency'] = 1 # 默认为 25
prometheus_monitoring['enable'] = false
```

之后执行

```shell
docker exec -it gitlab gitlab-ctl reconfigure
docker exec -it gitlab gitlab-ctl restart
```



<img src="https://i.loli.net/2020/05/10/7ilcTwUIMGAkYj6.jpg" alt="7ilcTwUIMGAkYj6" style="zoom:50%;" />

等待3分钟，访问 127.0.0.1:8001 ,设置 root 密码

添加 group，添加用户，将用户添加到 group 里，创建 group 的项目

我感觉可以直接创建 root 的项目，反正是自己一个人使用

语言设置：

![zfFZkTYc4HjMhKW](https://i.loli.net/2020/05/10/zfFZkTYc4HjMhKW.png)

### 添加本地已有仓库

```shell
git remote add gitlab http://47.93.53.47:8001/group1/zronghui_xxxt
git push -u gitlab master

cot .git/config

[remote "origin"] 下面添加 gitlab 的 URL:
url = http://47.93.53.47:8001/group1/zronghui_xxxt
```



## 4. 解锁网易云灰色歌曲 :8002

[nondanee/UnblockNeteaseMusic: Revive unavailable songs for Netease Cloud Music](https://github.com/nondanee/UnblockNeteaseMusic)

[进阶配置 · Issue #48 · nondanee/UnblockNeteaseMusic](https://github.com/nondanee/UnblockNeteaseMusic/issues/48)
[食用指南 · Issue #22 · nondanee/UnblockNeteaseMusic](https://github.com/nondanee/UnblockNeteaseMusic/issues/22)
[全端通用方案搭建教程 · Issue #527 · nondanee/UnblockNeteaseMusic](https://github.com/nondanee/UnblockNeteaseMusic/issues/527)

```shell
docker run --name yunmusic -p 8002:8080 nondanee/unblockneteasemusic
```



## 5. rsshub :1200

[DIYgod/RSSHub: 🍰 Everything is RSSible](https://github.com/DIYgod/RSSHub)
[介绍 | RSSHub](https://docs.rsshub.app/)



[部署 | RSSHub](https://docs.rsshub.app/install/#docker-compose-bu-shu)

部分RSS需要单独配置后方可生成，如 pixiv、disqus、twitter、youtube、telegram、github

```shell
mcd rsshub
wget https://raw.githubusercontent.com/DIYgod/RSSHub/master/docker-compose.yml
docker-compose up -d

# 启动成功，但是外网无法访问
git clone https://github.com/DIYgod/RSSHub.git
# 或者只下载项目下的 lib/middleware/header.js
vim lib/middleware/header.js
# 将 allow-origin 改为 0.0.0.0
# 'Access-Control-Allow-Origin': '0.0.0.0',
docker cp lib/middleware/header.js 8c144637ddef:/app/lib/middleware/
docker restart 8c144637ddef
```



## 9.Django 工具

### kitematic -- Mac管理 docker

### portainer -- 在网页端远程管理 docker

[portainer/portainer: Making Docker management easy.](https://github.com/portainer/portainer)

```shell
docker volume create portainer_data
docker run -d -p 9000:9000 -p 8000:8000 --name portainer --restart always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer
```



启动起来后，开启 9000 端口，然后在本地：

访问 http://47.93.53.47:9000/

设置密码，选择 Local--Manage the local Docker environment



[推荐5款好用的开源Docker工具 - InfoQ](https://www.infoq.cn/article/687ItzzHZ2P3pN5PMVSb)

watchtower ：自动更新 Docker 容器
docker-gc ：容器和镜像的垃圾回收
docker-slim ：面向容器的神奇减肥药
rocker ：突破 Dockerfile 的限制
ctop：容器的类顶层接口



## **常用的Docker

[汇总一下我常用的Docker镜像以及说明 - 矿渣社区 - 其他系统 - 软件专区](http://bbs.nas66.com/thread-8409-1-1.html)

google 更多

1. CentOS6

```C
镜像：treasureboat/ssh
用户名：root
密码：123456
建站的，配合这个脚本超好用：
yum install -y wget && wget -O install.sh http://download.bt.cn/install/install.sh && sh install.sh
#
#该镜像安装aria的步骤为：
wget http://ftp.tu-chemnitz.de/pub/linux/dag/redhat/el6/en/x86_64/rpmforge/RPMS/rpmforge-release-0.5.3-1.el6.rf.x86_64.rpm 
rpm -ivh rpmforge-release-0.5.3-1.el6.rf.x86_64.rpm 
yum -y install aria2
```

2. Ubuntu18

```C
镜像：rastasheep/ubuntu-sshd
用户名：root
密码：root
```

3. 宝塔面板

```C
镜像：tangniyuqi/baota
用户名：admin
密码：admin123
```

4. Debian

```C
镜像：hklcf/debian-ssh-docker
用户名: root
密码：password
```

5. Centos

```C
tutum/centos:latest .
tutum/centos:centos5 centos5
tutum/centos:centos6 centos6
tutum/centos:centos7 centos7
用户名：root
密码随机，在docker日志中查看密码
```

6. OwnCloud

```C
镜像：imdjh/owncloud-with-ocdownloader
端口：80
就是owncloud
```

7. peerflix-server（磁力下载工具）

```C
镜像：fish/peerflix-server
端口：9000
```

8. Owncloud（可以启用外部存储并具有下载功能）

```C
镜像：limaofeng/owncloud-with-ocdownloader
端口：80
```

9. chrome浏览器

```C
镜像：
consol/centos-xfce-vnc #（支持中文）
consol/ubuntu-xfce-vnc #（不支持中文）
密码：vncpassword
端口：5901
```

10. xware-kodexplorer

```C
镜像：mrlyc/xware-kodexplorer
端口：80
初始账号密码：admin
```

宝塔面板

```C
镜像：tangniyuqi/baota
用户名：admin
密码：admin123
```

### tomcat

网址：[https://hub.docker.com/_/tomcat/](https://hub.docker.com/_/tomcat/)

```
# 拉取
docker pull tomcat
# 运行
docker run -di --name tomcat -p 8081:8080 tomcat
```

### Nginx

网址：[https://hub.docker.com/_/nginx/](https://hub.docker.com/_/nginx/)

```
# 拉取
docker pull nginx
# 运行
docker run -di --name nginx -p 80:80 nginx
# 静态资源放入 nginx 中 /usr/share/nginx/html 目录下
```

### _MySQL_

网址：[https://hub.docker.com/_/_mysql_/](https://hub.docker.com/_/mysql/)

```
#拉取
docker pull mysql
# 运行
docker run --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=ROOT_Password -d mysql
```

#### _MySQL_ 连接问题

![](https://i.loli.net/2020/05/18/lzPnNQmOyBsW5kr.png)

```
问题：Client does not support authentication protocol requested by server; consider upgrading MySQL client

解决方案：进入容器内，登录容器内的MySQL，在SQL中执行以下命令即可：
alter user 'root'@'%' identified with mysql_native_password by '19491001';
FLUSH PRIVILEGES;
```

### Oracle

网址：[https://hub.docker.com/r/wnameless/oracle-xe-11g/](https://hub.docker.com/r/wnameless/oracle-xe-11g/)

```
# 拉取
docker pull wnameless/oracle-xe-11g
# 运行，默认账号 system，默认密码 oracle，-e 允许远程连接
docker run -d --name oracle -p 49161:1521 -e ORACLE_ALLOW_REMOTE=true wnameless/oracle-xe-11g
```

### _Redis_

网址：[https://hub.docker.com/_/_redis_/](https://hub.docker.com/_/redis/)

```
# 拉取
docker pull redis
# 运行
## 无密码运行
docker run -d --name redis -p 6379:6379  redis
## 有密码运行
docker run -d --name redis10 -p 6379:6379 redis --requirepass 19491001
```

### MongoDB

网址：[https://hub.docker.com/_/mongo/](https://hub.docker.com/_/mongo/)

```
# 拉取
docker pull mongo
# 运行
## 无账号密码运行
docker run -d --name mongo -p 27017:27017 mongo
## 账号密码运行
docker run -d --name mongo  -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=mongo -e MONGO_INITDB_ROOT_PASSWORD=19491001 mongo
```

### Gitlab

网址：[https://hub.docker.com/r/gitlab/gitlab-ce/](https://hub.docker.com/r/gitlab/gitlab-ce/)

```
# 使用 docker 拉取 gitlab 镜像
docker pull gitlab/gitlab-ce
# 运行 giltab 镜像
docker run -di --name=gitlab-ce -p 7000:80 gitlab/gitlab-ce
# 进入 gitlab 容器中
docker exec -it gitlab-ce /bin/bash
# 执行 giltab 配置,访问 http://IP:7000 即可，首次访问需要配置 root 密码
gitlab-ctl reconfigure
```

### Jenkins

DockerHub 官网：[https://hub.docker.com/_/jenkins/](https://hub.docker.com/_/jenkins/)

使用：

```
# 拉取
docker pull jenkins
# 运行
docker run -di --name simbajenkins -p 7000:8080 -p 50000:50000 jenkins
```

### Rancher

DockerHub 官网：[https://hub.docker.com/r/rancher/server/](https://hub.docker.com/r/rancher/server/)

使用：

```
# 拉取
docker pull rancher/server
# 运行
docker run -di --name simbarancher -p 7001:8080 rancher/server
```



## 参考、学习资料



[wsargent/docker-cheat-sheet: Docker Cheat Sheet](https://github.com/wsargent/docker-cheat-sheet)
[veggiemonk/awesome-docker: A curated list of Docker resources and projects](https://github.com/veggiemonk/awesome-docker)
[yeasy/docker_practice: Learn and understand Docker technologies, with real DevOps practice!](https://github.com/yeasy/docker_practice)



[谁在运行我的Kubernetes Pod？容器运行时的过去、现在和未来 - InfoQ](https://www.infoq.cn/article/tp28JvvgkZ-UPDY4THk5)

[docker 操作 · 雪之梦技术驿站](https://snowdreams1006.tech/devops/docker-ops.html)
[docker compose · 雪之梦技术驿站](https://snowdreams1006.tech/devops/docker-compose.html)

[Docker Compose | 菜鸟教程](https://www.runoob.com/docker/docker-compose.html)



