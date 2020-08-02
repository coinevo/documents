# EVO 区块链测试网络使用指南

## 1. 下载安装主网钱包
请参考 [《如何部署Evo量子链节点》](./Guidance-of-Evo-Deployment-and-RPC-Settings.md) 中“获取Evo节点”的部分。

## 2. 启动测试网络
使用命令 `./evod --testnet` 或者 `./evo-qt --testnet` 启动程序即可进入测试网络模式。

## 3. 获取测试币
访问 [测试网络水龙头](http://testnet-faucet.coinevo.tech/#!/)，输入钱包地址领取测试币。

## 4. 测试网络浏览器
[https://testnet.coinevo.tech/](https://testnet.coinevo.tech/)  
[https://testnet.coinevo.tech/](https://testnet.coinevo.tech/)

## 5. 回归测试模式
使用命令 `./evod --regtest ` 或者 `./evo-qt --regtest` 启动程序即可进入回归测试模式。

通过运行 `./evo-cli --regtest generate [num]` 可以生成新的区块。





