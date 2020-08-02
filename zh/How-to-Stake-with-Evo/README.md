# Evo量子链Staking（PoS挖矿）教程

Evo采用PoS共识机制，与比特币等采用的PoW机制有所不同。与比特币中挖矿类似，Evo PoS机制中称为Staking。每次Staking成功可累计获得至少**4 EVO**作为奖励。实际获得奖励一般超过4EVO，因为交易手续费和合约调用费用也将作为Staking奖励。

开始Staking需满足两个基本条件：

1. 运行Evo全节点，并保持在线（不需要矿机，任何PC/Mac，甚至树莓派都可以运行Evo全节点）；
2. 拥有EVO量子币（无论数量多少都可以Staking，但拥有Evo数量越多，挖到矿的可能性越高）。

如果你还没有EVO量子币，请先通过各种平台获取一定数量EVO备用。

Evo官方核心钱包可以运行Evo全节点，是目前唯一支持Staking的钱包。请注意，**手机钱包暂不支持Staking**。

有两种方式进行Staking：

* 方式一：用命令行运行`evod`进行Staking；
* 方式二：用PC版`evo-qt`钱包进行Staking。

方式一适用于熟悉Linux/OSX/Windows命令行操作的用户，包括树莓派用户；方式二适用于适用有图形界面钱包的用户。读者可以根据自身需求选择其中一种方式进行Staking。两种方式完全等效，Staking（挖矿）效果没有任何区别。

## 方式一：用命令行运行`evod`进行Staking

### 1. 运行`evod`

`evod`运行和交互的方法请参考教程《[如何部署Evo量子链节点](../Guidance-of-Evo-Deployment-and-RPC-Settings.md)》中“获取Evo节点”和“部署Evo节点”部分。

按照以上教程运行`evod`:

```
./evod -daemon
```

Staking功能在钱包未加密时将默认开启，无需其他设置。

### 2. 转入EVO量子币

首先获取钱包地址，命令为：

```
./evo-cli getnewaddress
```

命令将返回一个新生成的地址，地址以Q开头。可以向该地址转入一笔或多笔EVO用于Staking。读者可以用同样方法生成任意多地址，并向地址中转入任意多笔Evo进行Staking。

注意：**刚转入的EVO需要等待500个区块确认才可用于Staking，即大概需要等待17小时**。这与Evo采用的MPoS共识机制有关，对其运行原理感兴趣的读者可以参考《[Evo区块链指南](../Evo-Blockchain-Guide.md#mpos共识算法)》进一步了解。

在区块同步完成后，可以通过`./evo-cli getbalance`查看钱包余额，或`./evo-cli listunspent`查看所有UTXO。（[什么是UTXO?](../Evo-Blockchain-Guide.md#utxo账户模型)）。

建议在EVO转入500个区块后再进行以下步骤，因为确认数小于500个的UTXO无法进行Staking。

### 3. 查看Staking状态

通过以下命令可以查看staking状态：

```
./evo-cli getstakinginfo
```

运行类似结果如下：

```
{
  "enabled": true,
  "staking": true,
  "errors": "",
  "currentblocksize": 1000,
  "currentblocktx": 0,
  "pooledtx": 5,
  "difficulty": 5683612.564280176,
  "search-interval": 46,
  "weight": 53206430,
  "netstakeweight": 2278172497819029,
  "expectedtime": 5480654870
}
```

其中`enabled`代表是否开启Staking功能，该功能是默认开启的;`staking`代表目前是否有EVO正在Staking，`true`即代表正在Staking;`weight`代表目前正在staking的Evo数量，单位是10^-8EVO，本例子中约0.532EVO;`expectedtime`代表目前你挖到矿的期望时间，单位是秒。

### 4. 加密的钱包如何Staking？

如果读者不需要对钱包加密，请跳过此步骤。

但请注意，未加密的钱包发送EVO将无需任何认证，建议读者对钱包进行加密。（[如何加密钱包？](../Encrypt-and-Unlock-Evo-Wallet/README.md)）

钱包可以通过`encryptwallet`命令进行加密，进一步保证资金安全。然而，在钱包加密的状态下，Staking功能将被默认关闭。加密后`./evo-cli getstakinginfo`将获得如下结果：

```
{
  "enabled": true,
  "staking": false,
  "errors": "",
  "currentblocksize": 1000,
  "currentblocktx": 0,
  "pooledtx": 94,
  "difficulty": 5788429.670171153,
  "search-interval": 0,
  "weight": 53206430,
  "netstakeweight": 2438496688951881,
  "expectedtime": 0
}
```

注意`staking`状态变为`false`，说明没有在Staking。

通过如下`walletpassphrase`命令可以对钱包进行解锁：

```
./evo-cli walletpassphrase "<你设置的密码>" 99999999 true
```

其中第一个参数为用户加密时设置的密码，第二个参数`99999999`为需要解锁的时间，单位是秒，可以根据用户需要进行设置；第三个参数表示是否只解锁staking功能，设置为true则表示只解锁用于Staking，而发送EVO仍需要输入密码。若第三个参数缺省，则表示完全解锁钱包，不仅可以Staking，也可以正常发送EVO。

解锁后用`getstakinginfo`可查看状态，一切正常的话即可以在钱包加密状态下Staking了。

## 方式二：用PC版Evo-qt钱包Staking

Evo-qt钱包的基本使用方法请参考[qt钱包教程(点击打开)](../Evo-Wallet-Tutorial/README.md)。目前支持的Mac/Linux/Windows，用户可以自行下载安装。

### 1. 打开Evo qt钱包

打开已经安装好的Evo钱包。

### 2. 转入EVO量子币

如果钱包中已有EVO可跳过此步骤。

若钱包中无EVO，则向钱包地址中转入一定数量的EVO，方法请参考[qt钱包教程(点击打开)](../Evo-Wallet-Tutorial/README.md)。

注意，新转入的EVO需要等待500个区块（约17小时）的成熟时间，才可进行Staking。因此，建议用户等待500个区块后再进行以下步骤。

### 3. 查看Staking状态

通过钱包右下角的闪电标志可以查看Staking的状态。

**若闪电为实心，表示正在Staking**。将鼠标放到闪电标志上，可以看到Staking相关信息，如下图所示：

![正在Staking](staking.png)

* `Staking`表示正在挖矿；
* `Your weight is`表示当前你正在参与Staking的EVO数量，单位是1EVO；
* `Network weight is`表示网络中正在参与Staking的EVO数量，单位是1EVO；
* `Expected time`表示挖到矿的期望时间，单位是天。

**若闪电为空心，表示不在Staking**。可能的原因有：

* 钱包里没有超过500个区块确认的EVO -- 解决方法：这时请向钱包转入EVO，并等待500个区块（约17小时）；

![没有成熟的EVO导致无法Staking](not-mature.png)

* 钱包处于锁定状态 -- 解决方法：解锁钱包。[如何解锁钱包?](../Encrypt-and-Unlock-Evo-Wallet/README.md)

![钱包未解锁导致无法Staking](locked.jpg)

**若无闪电标志，说明禁用了Staking功能**

* 钱包未开启staking功能 -- 解决方法：修改运行参数或配置文件，开启Staking。[如何添加配置文件？](../Guidance-of-Evo-Deployment-and-RPC-Settings.md#rpc调用设置)

![钱包未开启Staking](staking-disabled.jpg)

## 关于Staking奖励

如果用户顺利挖到一个区块，可以累积获得超过4EVO的奖励。关于挖矿奖励有以下几点需要注意：

* 奖励会以一笔新交易的形式发送给你，命令行用户可通过`getbalance`命令查看余额变化，qt钱包用户可以直接看到收入的交易；
* Staking成功，你会立刻收到一笔0.4EVO的奖励；
* **剩余3.6EVO的奖励会在500个区块（约17个小时）之后，在连续九个区块中奖励给你，每个区块你将获得0.4EVO**，与上条中0.4EVO合计共4EVO；
* Staking成功的那个币（UTXO）将被锁定500个区块，直到500区块之后才可以进行交易或继续进行Staking。为了不让资金锁定太久，用户可以选择将一个大的UTXO分成若干个较小的UTXO，这样只有挖到矿的那个UTXO会被锁定；

这一奖励机制和Evo采用的MPoS机制有关，有兴趣了解原理的读者可以参考《[Evo区块链指南](../Evo-Blockchain-Guide.md#mpos共识算法)》。

## 如何关闭Staking功能

Evo钱包会默认开启Staking，但有些情况用户或交易所想要关闭该功能。有以下几种方式可以停止Staking：

1 命令行用户可以在运行时加上`-staking=false`选项，如：

```
./evod -staking=false -daemon
```

启动qt钱包的命令：

```
./evo-qt -staking=false
```

2 在配置文件`evo.conf`中添加`staking=false`，[如何使用配置文件？](../Guidance-of-Evo-Deployment-and-RPC-Settings.md#rpc调用设置)

3 锁定钱包，钱包在锁定状态下会自动停止Staking。[如何锁定钱包？](../Encrypt-and-Unlock-Evo-Wallet/README.md)
