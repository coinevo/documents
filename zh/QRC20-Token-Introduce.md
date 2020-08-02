# QRC20 Token 指南

## QRC20简介

QRC20是 EVO 上的代币标准，其内容和以太坊上的[ERC20标准](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-20-token-standard.md)基本一致。

合约代码需实现以下函数接口和事件：

```
function name() constant returns (string name)
function symbol() constant returns (string symbol)
function decimals() constant returns (uint8 decimals)
function totalSupply() constant returns (uint256 totalSupply)
function balanceOf(address _owner) constant returns (uint256 balance)
function transfer(address _to, uint256 _value) returns (bool success)
function transferFrom(address _from, address _to, uint256 _value) returns (bool success)
function approve(address _spender, uint256 _value) returns (bool success)
function allowance(address _owner, address _spender) constant returns (uint256 remaining)
event Transfer(address indexed _from, address indexed _to, uint256 _value)
event Approval(address indexed _owner, address indexed _spender, uint256 _value)
```

## 代码书写

我们为大家提供了一份示例代码：[QRC20Token](https://github.com/coinevo/QRC20Token)，你可以使用这份合约代码来发布你的代币。

在文件`QRC20Token.sol`中，修改name、symbol、totalSupply这三个变量值，他们分别代表代币名称、代币符号、代币发行总量。

![](https://s.coinevo.tech/uploads/7cf98db2a2f60f944e8295ced1b76917.png)


## 合约发布

### 安装钱包

从 [https://eco.coinevo.tech/wallet](https://eco.coinevo.tech/wallet) 或者 [https://github.com/coinevo/evo/releases/latest](https://github.com/coinevo/evo/releases/latest) 根据操作系统和架构选择对应的链接下载最新的 Evo Core 钱包。

钱包安装完成后，需要转入一定数量(大于1个)的EVO到钱包中用来支付后续创建合约的手续费。
点击`Request payment`即可获得你的钱包地址。

![](https://s.coinevo.tech/uploads/0f30abe838aca957a1bacb2ed9209575.png)

### 编译合约

在浏览器中打开网址[https://ethereum.github.io/browser-solidity/](https://ethereum.github.io/browser-solidity/) 

点击左上角的 “+” 按钮，新建文件 SafeMath.sol 和 QRC20Token.sol，将之前写好的合约代码复制粘贴进去。
![](https://s.coinevo.tech/uploads/095deff475a970dc2a25b9e6960436db.png)
 
在页面右侧，点击 “detail” 按钮，在弹出的页面中拷贝 BYTECODE 的 object 项内容，并保存下来。
![](https://s.coinevo.tech/uploads/47380517f0f34253511cb2c6bfc77bb7.png)
![](https://s.coinevo.tech/uploads/fd6f45a90362e4b27a345a4557c4c5e4.png)

### 发布合约

打开Evo Core钱包，进入“Smart Contract” =》 “Create”， 将上一步所得的16进制BYTECODE粘贴到文本框中。
![](https://s.coinevo.tech/uploads/bb9efeaad7f8068a982ad948aa02e0e5.png)

点击“Create Contract”按钮，保存返回结果中的 SenderAddress 和 ContractAddress 以便后续使用。
![](https://s.coinevo.tech/uploads/3d739f869437a96cabd9e2171199b118.png)

等待片刻，让交易得到确认，我们的合约就创建成功了。根据合约代码，所有的初始代币都会分配给合约的创建者，也就是SenderAddress。

### 测试网络

在合约发布到主网之前，可先在测试网络中进行测试，测试网络中操作方法和主网的一致。

用命令行启动Evo-qt的时候，带上`--testnet`即可进入测试网络。

测试网络中的Evo可在 [测试币水龙头](https://testnet-faucet.coinevo.tech/) 中获得。

## 钱包使用

### 添加代币

在 Evo Core钱包中，进入 “QRC Token” 页面，点击 “Add Token”，填写发布合约时所得的ContractAddress，并选择SenderAddress作为Token Address, 点击“Confirm”按钮代币就添加成功了。

![](https://s.coinevo.tech/uploads/0b1aa0c2354522ec07cb41913f0b48d4.png)

如果在Token Address的下拉列表中没有找到之前的SenderAddress，向SenderAddress里发送一些Evo后再试一次。

### 接发代币

在 “QRC Token” 页面，点击选择需要操作的代币，然后点击下面的“Send” 和“Receive”进行发送和接收。

![](https://s.coinevo.tech/uploads/49e40c268a4e75c9e1afe06c46f58496.png)


## 其他常见问题

* 发送代币需要一定的EVO作为手续费，请确保持有代币的那个EVO地址下有足够的余额。
* 发送代币会预扣0.1 EVO的手续费，交易确认后会以一笔挖矿收入的形式返还多余的手续费，实际消耗的手续费大约为0.02 EVO。
* 不符合QRC20标准的合约不会自动添加到[官方区块链浏览器](https://ex.coinevo.tech/)中, 在主网上发布合约前请检查好代码。




