## 问题汇总：

1. ~~区块链运维层面 随着节点增加删除下线等 需要我们做的工作还有哪些~~
2. ~~区块链规模的大小对数据查询效率的影响~~
3. 合约设计相关思考，如何结合业务设计合约，各个模块的设计，表的设计等
4. 节点准入涉及的权限问题



## 1. 区块链运维层面需要做的工作

### 1.新增节点

组织结构生成之后可以随时添加或修改吗？

目前，Hyperledger Fabric 无法对已生成的组织结构进行修改；所以需要提前做好规划

### 链码的打包升级

在实际场景中，由于需求场景的变化，链码也需求实时做出修改，以适应不同的场景需求。所以我们必须能够对于已成功部署并运行状态中的链码进行升级操作。

首先，先将修改之后的链码进行安装，然后使用 upgrade 命令对已安装的链码进行升级，具体实现如下：

**安装：**

```shell
peer chaincode install -n mycc -v 2.0 -p github.com/chaincode/chaincode_example02/go/
```

**升级：**

```shell
peer chaincode upgrade -o orderer.example.com:7050 --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem -C $CHANNEL_NAME -n mycc -v 2.0 -c '{"Args":["init","a", "100", "b","200"]}' -P "OR ('Org1MSP.peer','Org2MSP.peer')"
```

链码升级之后， 之前旧版本的链码还能使用吗？

升级是一个类似于实例化操作的交易，它会将新版本的链码与通道绑定。**其他与旧版本绑定的通道则仍旧运行旧版本的链码**。换句话说，升级只会一次影响一个提交它的通道。



### 其他：某节点（order 节点或 peer 节点）掉线后怎么重新加入集群 并恢复、同步数据

## 2. 区块链规模的大小对数据查询效率的影响

区块链的效率主要受共识机制影响

### fabric 的共识机制

<img src="https://i.loli.net/2020/07/31/jmu2U8yksAY36el.png" alt="image-20200731100157053" style="zoom: 33%;" />



区块链规模越大，节点越多，背书越慢

### 具体速度

衡量单位: tps 每秒吞吐量

**TPS=区块大小/（出块间隔\*交易字节大小）**

![image-20200731100306637](https://i.loli.net/2020/07/31/qd1EPvMi7Ogo3la.png)

平均吞吐量: 大概 60tps，还是挺慢的

### 几种典型区块链在性能功能上的差异

![几种典型区块链在性能功能上的差异与作用](https://i.loli.net/2020/07/31/eANG1aWoX47LmTh.png)







参考：

[几种典型区块链在性能功能上的差异与作用 - 知乎](https://zhuanlan.zhihu.com/p/137601849)

[(3 封私信) 区块链技术每秒的交易量低的瓶颈在哪儿，可能会如何解决？ - 知乎](https://www.zhihu.com/question/50156061)

[Hyperledger Fabric 学习三：共识机制 - 简书](https://www.jianshu.com/p/443b55d28e8d)

[IBM 超级账本Fabric 性能测试_郑泽洲的博客-CSDN博客_fabric性能测试](https://blog.csdn.net/wxid2798226/article/details/81709837)









