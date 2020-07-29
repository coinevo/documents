# EVO Staking（PoS mining）Tutorial

Evo employs PoS (Proof of Stake) consensus mechanism, which is different from Bitcoin's PoW (Proof of Work). The mining process in PoS system is called staking. The block producer will get 6.5EVO, as well as the transaction fees and gases as block reward. So the real reward is usually more than 6.5 evo in total.

Basic requirements for staking：

1. Run a Coinevo fullnode, and keep online (Since Coinevo is using PoS, we don't need any mining machine, just PC or even Raspberry Pi can run a fullnode);
2. Have some EVO in the wallet (fullnode)（Any amount of EVO can be used for staking, more QTUM means higher possibility to stake).

If you have no EVO yet, please get some from market before you doing following staking settings.

Currently, Coinevo Core wallet is the only wallet that support Coinevo PoS staking. Note that other wallets like mobile wallet and Qtum Electrum are not able to stake for the time being.

Two ways to stake:

* Method 1：Staking with evod, using command line, suitable for Linux/OSX/Windows/Raspberry Pi users who are familiar with command line tools.
* Method 2：Staking with `evo-qt` wallet, with GUI, suitable for common users.

Either way works in the same way for staking, so you can choose either method you like.

## Method 1：Staking with `evod` (command line)

### 1. Run `evod`

Follow the guidance to run `evod`:

```
./evod -daemon
```

Staking is default on for evod, so no need for other options if you only want to stake.

### 2. Send some EVO to your wallet

First you can generate a new address with：

```
./evo-cli getnewaddress
```

This will generate a new address with Prefix '1'. You can send some EVO to this new generated address for staking. You can generate as many addresses as you like, and send arbitrary EVO as you like for staking.

Note：**The coin should wait for 500 blocks before being able to stake, i.e. about 17 to 24 hours to MATURE.**. 

After the EVO node syncing to the latest block, you can check current balance with `./evo-cli getbalance` or get utxo list with`./evo-cli listunspent`。（[what is UTXO?](https://github.com/coinevo/documents/blob/master/zh/Qtum-Blockchain-Guide.md#utxo账户模型)）。

Please do following steps after your coin is mature.

### 3. Check staking info

Check current staking info with：

```
./evo-cli getstakinginfo
```

You might get the result like this：

```
{
  "enabled": true,
  "staking": true,
  "errors": "",
  "currentblocktx": 0,
  "pooledtx": 0,
  "difficulty": 3693409.779133397,
  "search-interval": 1577,
  "weight": 309584575558555,
  "netstakeweight": 1948540143266404,
  "expectedtime": 805
}
```

`enabled` means if your wallet have enabled staking, it should be true by default. `staking` means if your wallet is currently staking (mining). `weight` stands for the amount of EVO that is staking right now, with unit 10^-8EVO, here in the example, we have 0.532EVO staking. `expectedtime` stands for the expected time that you will get a reward, the unit is second.

### 4. How to stake if the wallet is encrypted？

If your wallet is not encrypted, you can skip this section. However, for security, we recommand you encrypt your wallet.

Coinevo wallet can be encrypted with `encryptwallet`. However, staking will be stopped when it is encrypted. For example, `./evo-cli getstakinginfo` for a encrypted wallet：

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

See `staking` turns to `false`, which means wallet is not staking.

You can use `walletpassphrase` to unlock wallet for staking：

```
./evo-cli walletpassphrase "<your passphrase>" 99999999 true
```


After unlocking, you can double check `getstakinginfo`, it should look the same with previous unlocked result, `staking` become true.

## Method 2: Staking with evo-qt wallet (official PC wallet)

Current supported platform: Mac/Linux/Windows.

### 1. Open Evo qt wallet

Launch the wallet.

### 2. Send some EVO to your wallet

If you already have some EVO in your wallet, you might skip this step.


Note：**The coin should wait for 500 blocks before being able to stake, i.e. about 17 to 24 hours to MATURE.**. 

### 3. Check staking status

The flash sign at the bottom of wallet shows staking info :

**Solid black flash means it is staking now**. For more information, you can put your mouse on the flash, e.g.:

![staking info](staking.png)

* `Staking`: if it is staking；
* `Your weight is`: How many EVO are able to used for staking, unit is EVO;
* `Network weight is`: How many EVO are staking in the network, unit is EVO；
* `Expected time`: expected time to get reward, unit is Day.

**Hollow flash measn it is not staking**

Possible reasons for not staking：

* 1.There is no coins of no mature coins (more than 500 confirmations(blocks)) -- Solution: send some EVO to the wallet and wait for 500 blocks (about 17 hours);

![No mature coins](not-mature.png)

* 2.Wallet is locked/encrypted -- Solution: unlock the wallet for staking. ([How to unlock?](../Encrypt-and-Unlock-Qtum-Wallet/README.md))

![Not staking due to encryption](locked.jpg)

**No flash sign means staking is disabled**

* 3.Staking is disabled -- Solution: enable staking in the evo.conf (-staking=true)([How to set evo.conf？](../Guidance-of-Qtum-Deployment-and-RPC-Settings.md))

![Staking disabled](staking-disabled.jpg)

## About block reward

The block producer will get more than 6.5 EVO rewards, something to keep in mind:

* The reward come from a new transaction, you can check balance to see if you get the reward.
* Once succesfully stake, you will get 0.65EVO reward immediately.
* **Other 5.85EVO will be sent to you after 500 blocks (about 17 or 24 hours), in continuous 9 blocks, within each block you will get 0.65QTUM,**, so in total it will be 5.85QTUM.
* The staked coins (UTXO) will be locked for 500 blocks, during this period, it cannot be spent nor be used to stake. 

## How to disable staking?

Staking is by default enabled for Coinevo wallet. If you need to disable staking for some reason (for example exchanges are always recommanded to disable staking), you might following anyone of the 3 ways below:

1 Add `-staking=false` when running Coinevo node：

```
./evod -staking=false -daemon
```

For qt wallet, it is like：

```
./evo-qt -staking=false
```

2 Add config `staking=false` in evo.conf;([How to set evo.conf？](../Guidance-of-Qtum-Deployment-and-RPC-Settings.md))

3 Encrypt wallet, since encrypted wallet will automatically stop staking.([How to unlock?](../Encrypt-and-Unlock-Qtum-Wallet/README.md))
