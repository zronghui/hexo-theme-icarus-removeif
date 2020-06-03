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

分类账 -- 账本

# 学习

**channel** : 定义 Fabric 网络功能, 比如块的制作方式和节点可以使用的 Fabric 版本 、 定义了哪些组织是联盟的成员。channel是特定网络成员之间的私有通信层。 只有被邀请到频道的组织才能使用频道，而网络中的其他成员是看不见的。每个channel都有一个独立的区块链分类账。 被邀请“加入”其同行的组织存储通道分类账并验证通道上的交易。

**ordering nodes**: 允许 peers 专注于验证事务并将它们提交到分类账中, 从客户端接收经过认可的事务后，它们对事务的顺序达成共识，然后将它们添加到块中

**peers**: 验证交易并将交易块添加到区块链分类账时，它们不会决定交易的顺序

**smart contract**: 智能合约。包含管理区块链分类账上资产的业务逻辑。 由网络成员运行的应用程序可以调用智能合同在分类账上**创建**资产，以及**更改和转移**这些资产。 应用程序还可以**查询**智能合同来**读取**分类账上的数据。

**multiple signatures**: 多重签名。为了确保交易有效，使用智能合同创建的交易通常需要由多个组织签署，以便提交给渠道分类账。

**chaincode**: 在 fabric 中，是智能合约打的包。链码安装在一个组织的 peer 上，在将链码部署到通道之前，通道的成员需要就链码定义达成一致，以建立链码治理。 当所需的组织数量一致时，链码定义可以提交给通道，并且链码已经可以使用了。然后部署到一个通道，在那里它可以用来认可交易和与区块链分类账互动。 

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

### Bring up the test network

[Using the Fabric test network — hyperledger-fabricdocs master 文档](https://hyperledger-fabric.readthedocs.io/zh_CN/latest/test_network.html)



```shell
cd fabric-samples/test-network
./network.sh -h
./network.sh down # 从以前的运行中删除任何容器或工件
./network.sh up # 启动网络
docker ps -a # 应该可以看到由 network.sh 脚本创建的三个节点
./network.sh createChannel
# createChannel 主要的操作：
# 1. Creating channel mychannel
# 2. Join Org1 peers to the channel...
# 3. Join Org2 peers to the channel...

export GOPROXY=https://goproxy.io
export GO111MODULE=on

./network.sh deployCC # 在通道上启动链码
# deployCC 主要的操作：
# 1. 安装 golang 的依赖
# 2. Chaincode is packaged on peer0.org1
# 3. Chaincode is installed on peer0.org1
# 4. Chaincode is installed on peer0.org2
# 5. Query installed successful on peer0.org1 on channel
# 6. Chaincode definition approved on peer0.org1 on channel 'mychannel'
# 7. Checking the commit readiness of the chaincode definition on peer0.org1 on channel 'mychannel'...
# 8. Checking the commit readiness of the chaincode definition on peer0.org2 on channel 'mychannel'...
# 9. Chaincode definition approved on peer0.org2 on channel 'mychannel'
# 10. Chaincode definition committed on channel 'mychannel'
# 11. Querying chaincode definition on peer0.org1 on channel 'mychannel'...
# 12. Querying chaincode definition on peer0.org2 on channel 'mychannel'...
# 13. Invoke transaction successful on peer0.org1 peer0.org2 on channel 'mychannel'
# 14. Querying on peer0.org1 on channel 'mychannel'...
```



![image-20200528105117015](https://i.imgur.com/pQZr7wJ.png)



![image-20200528183519799](https://i.imgur.com/Lse0e7F.png)



#### 查询、更改

```shell
export PATH=${PWD}/../bin:${PWD}:$PATH
export FABRIC_CFG_PATH=$PWD/../config/

# Environment variables for Org1
export CORE_PEER_TLS_ENABLED=true
export CORE_PEER_LOCALMSPID="Org1MSP"
export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
export CORE_PEER_ADDRESS=localhost:7051

# 查询
peer chaincode query -C mychannel -n fabcar -c '{"Args":["queryAllCars"]}'
# 更改
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls true --cafile ${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem -C mychannel -n fabcar --peerAddresses localhost:7051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt --peerAddresses localhost:9051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt -c '{"function":"changeCarOwner","Args":["CAR9","Dave"]}'

# 查询运行在 org 2对等节点上的链码
# Environment variables for Org2
export CORE_PEER_TLS_ENABLED=true
export CORE_PEER_LOCALMSPID="Org2MSP"
export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt
export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp
export CORE_PEER_ADDRESS=localhost:9051

peer chaincode query -C mychannel -n fabcar -c '{"Args":["queryAllCars"]}'

./network.sh down
```



![image-20200528204924722](https://i.imgur.com/KWkBjMD.png)


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


