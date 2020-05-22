---
title: fabric
toc: true
recommend: 1
uniqueId: '2020-05-21 07:28:00/"fabric".html'
date: 2020-05-21 15:28:00
thumbnail:
categories:
- blockchain
tags:
keywords:
---



[TOC]

<!--more-->

# 学习



# 实践

[hyperledger/fabric-samples](https://github.com/hyperledger/fabric-samples)

### 安装

[https://raw.githubusercontent.com/hyperledger/fabric/master/scripts/bootstrap.sh](https://raw.githubusercontent.com/hyperledger/fabric/master/scripts/bootstrap.sh)

此脚本实现以下功能：

克隆 hyperledger/fabric-samples 仓库
检出适当的版本标签
将指定版本的Hyperledger Fabric平台特定二进制文件和配置文件安装到fabric-samples下的/bin和/config目录中
下载指定版本的Hyperledger Fabric docker镜像

```shell
mcd fabric
# 将 bootstrap.sh 弄到这个目录里
chmod u+x bootstrap.sh
./bootstrap.sh

export PATH=/root/fabric/fabric-samples/bin:$PATH
```




# 工具



# 其他

### **Hyperledger Composer (已经过时了)**

[**Installing**](https://hyperledger.github.io/composer/latest/installing/installing-index)

[Installing pre-requisites](https://hyperledger.github.io/composer/latest/installing/installing-prereqs)

Install nvm and switch node to 8

install docker、 vs code、Hyperledger Composer plugin for vscode

```shell
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.0/install.sh | bash
# new iterm2 tab
nvm —-version

nvm install v8
nvm use 8
node --version

```

