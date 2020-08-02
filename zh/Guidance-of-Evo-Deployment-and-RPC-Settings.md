# 如何部署Evo量子链节点

本教程包含Evo量子链的部署、运行以及RPC调用等内容。

教程假设读者能够熟练使用Linux,Mac或Windows命令行。若您不符合此要求，或只关心Evo qt钱包的使用方法，请参考另一篇Evo钱包使用教程。

## 获取Evo节点

可以通过以下四种方法之一获得Evo节点程序：

**1. 直接下载二进制文件**

如果你并不关心Evo的源码，部署Evo节点最方便的方法是在[Evo release page（点击打开）](https://github.com/coinevo/evo/releases)下载最新的二进制文件，目前支持的平台包括Linux，Windows，OSX。建议选择最新版进行下载，本教程以撰写时的最新版v0.14.13为例。

（注意，你所看到最新版的版本号可能不同，如这里是0.14.13，其他字符串保持不变）

* **Mac**用户请下载：`evo-0.14.13-osx64.tar.gz`
* **Linux**用户请下载： `evo-0.14.13-i686-pc-linux-gnu.tar.gz`(32位)或`evo-0.14.13-x86_64-linux-gnu.tar.gz`(64位)
* **Windows**用户请下载：`evo-0.14.13-win32.zip`(32位)或`evo-0.14.13-win64.zip`（64位）
* **树莓派**用户请下载：`evo-0.14.13-arm-linux-gnueabihf.tar.gz`

下载压缩包解压后,`<解压路径>/bin/`下包含`evod`和`evo-cli`，即为本教程要用到的Evo节点可执行文件。


**2. Linux可通过apt安装**

具体教程可参见[https://github.com/coinevo/evo/wiki/Setting-up-EVO-Ubuntu,-Debian-and-Mint-repository](https://github.com/coinevo/evo/wiki/Setting-up-EVO-Ubuntu,-Debian-and-Mint-repository)，目前支持的平台为Ubuntu，Debian和Mint。

树莓派用户也可以通过apt安装，具体教程参见[https://github.com/coinevo/evo/wiki/Installing-Evo-on-Raspberry-Pi](https://github.com/coinevo/evo/wiki/Installing-Evo-on-Raspberry-Pi)。

按照教程安装完毕后，可以通过直接从命令行调用`evod`和`evo-cli`等。

**3. 通过源代码编译**

如果你想从源代码直接编译Evo，可以从Github上下载最新源码: [https://github.com/coinevo/evo](https://github.com/coinevo/evo)。

具体编译方法请参考 [https://github.com/coinevo/evo/blob/master/README.md](https://github.com/coinevo/evo/blob/master/README.md)。目前支持较好的编译平台包括Linux和OSX。其他平台的依赖环境可能有所不同。

编译成功后，在`<path>/src`路径下同样得到上文的二进制可执行文件：`evod`,`evo-cli`。

**4. 通过Docker获取evo镜像**

关于docker安装和使用方法请参照docker官方教程。这里假设docker环境已正确安装。

Evo官方在docker hub上的镜像为`evo/evo`,可通过以下命令获取：

```
docker pull evo/evo:latest
```

通过上文方法获取evo可执行程序，其中与节点部署及RPC调用相关的是：

* `evod`：Evo核心程序，即真正的Evo全节点程序
* `evo-cli`：Evo命令行接口，可以和Evo核心程序进行交互，实现本地RPC调用

## 部署Evo节点

Docker容器的使用方法略有不同，但原理一致。读者可参考另一教程《[如何用docker运行evo节点](Launch-Evo-with-Docker.md)》。

下面以Ubuntu为例，部署Evo节点。Mac和Windows命令行与Linux保持一致，不再赘述。

通过`./evod`, 即可运行Evo全节点：

```
./evod -daemon
```

其中`-daemon`表示进程后台驻留。如果用户想通过节点查看合约相关events（如查看收发QRC20代币的记录等），可以在运行`evod`时添加`-logevents`选项。

 更多选项，可通过以下命令查看：

```
./evod -help
```

结束运行请执行：

```
./evo-cli stop
```

不同平台的默认的数据路径不同：

* Linux：`~/.evo/`
* Mac OSX：`~/Library/Application Support/Evo`
* Windows:`%APPDATA%\Evo`

首次运行如果默认路径不为空，请清空之后再运行节点（清空即删除路径下所有文件，清空前注意备份！）。读者也可以通过`-datadir`选项指定数据路径。

首次运行需要同步所有区块，节点运行日志路径为`~/.evo/debug.log`。

## 本地RPC调用

节点正常运行后，可通过`evo-cli`进行交互，实现本地RPC调用。
例如：

```
oldclock@raven:~/evo-0.14.3/bin$ ./evo-cli getinfo
{
  "version": 140300,
  "protocolversion": 70016,
  "walletversion": 130000,
  "balance": 0.00000000,
  "stake": 0.00000000,
  "blocks": 12126,
  "timeoffset": 0,
  "connections": 8,
  "proxy": "",
  "difficulty": {
    "proof-of-work": 1.52587890625e-05,
    "proof-of-stake": 886731.5868738915
  },
  "testnet": false,
  "moneysupply": 100028504,
  "keypoololdest": 1505186997,
  "keypoolsize": 100,
  "paytxfee": 0.00000000,
  "relayfee": 0.00400000,
  "errors": ""
}

```

获取所有RPC命令列表，请运行:

```
./evo-cli help
```

获取RPC调用的使用说明，请使用`./evo-cli help <RPCcmd>`，例如：

```
./evo-cli help getinfo
```
## RPC调用设置

通过`./evo-cli help getinfo`我们可以获取通过jsonrpc实现getinfo调用的使用说明：

```
Examples:
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getinfo", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:3889/
```

实例中给出了json报文的详细格式，但默认条件下无法进行调用。只有在设置rpc用户名和密码后，才可正常使用。设置方法如下：

方法一：新建配置文件`~/.evo/evo.conf`，内容包含

```
rpcuser=test  #rpc用户名
rpcpassword=test1234  #rpc密码
#以上两项必须设置。默认情况下，只允许本地的RPC连接，
#要实现远程调用，可通过设置rpcallowip声明所有允许访问的ip
#ipv4和ipv6都可设置，且可是设置多个ip。例如：
#rpcallowip=192.168.77.51/255.255.255.0
#rpcallowip=1.2.3.4/24
#rpcallowip=2001:db8:85a3:0:0:8a2e:370:7334/96
```

更多配置参数可以参考：[evo.conf实例（点击打开）](https://github.com/coinevo/evo/blob/1a926b980f03e97322c7dd787835bec1730f35d2/contrib/debian/examples/evo.conf)

设置完成后，重启节点即完成配置。

方法二：重新运行Evo节点，并加入特定参数：

```
./evod -rpcuser=test -rpcpassword=test1234 -rpcallowip=192.168.77.51/255.255.255.0
```

各参数含义与配置文件中相同，不再赘述。

## RPC调用实例

设置完成并重新运行节点后，即可进行远程RPC调用。本文中运行Evo节点的服务器ip为192.168.77.188，默认端口为3889。例如，在macbook上对ubuntu中运行的Evo节点进行rpc调用，将返回和本地调用相同结果：

```
zhongwenbins-MacBook-Pro:~ zhongwenbin$ curl --user test:test1234 --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getinfo", "params": [] }' -H 'content-type: text/plain;' http://192.168.77.188:3889/
{"result":{"version":140300,"protocolversion":70016,"walletversion":130000,"balance":0.00000000,"stake":0.00000000,"blocks":12197,"timeoffset":0,"connections":8,"proxy":"","difficulty":{"proof-of-work":1.52587890625e-05,"proof-of-stake":650787.7561123729},"testnet":false,"moneysupply":100028788,"keypoololdest":1505186997,"keypoolsize":100,"paytxfee":0.00000000,"relayfee":0.00400000,"errors":""},"error":null,"id":"curltest"}
```

用postman等工具可看到更加直观的效果：
![postman截图](https://s.coinevo.tech/uploads/e7398325af231cf42a708303af2a5098.png)

自此Evo节点的部署及rpc调用设置全部完成。

## Nginx配置实例（选读）
从上文实例可以看出，rpc命令必须包含rpc用户名，密码，并指定端口号3889。如果想部署通用API，避免输入用户密码及端口号，可以考虑用Nginx实现。这样做的好处是可以隐藏用户名和密码，当用户名密码甚至是端口号发生改变时，也不影响api，同时还可以对外部rpc调用进行适当过滤，保证安全性。关于Nginx的安装和基本使用，可自行搜索相关教程。

例如，

* 运行Evo节点的服务端ip为`192.168.77.188`，节点正常运行
* api代理端ip为`192.168.77.51`，已安装Nginx

设置步骤如下：

1.设置服务端Evo节点的rpc配置文件（参考上文），将api代理端的ip加入rpcallowip中，并重启节点，例如：

```
rpcuser=test
rpcpassword=test1234
rpcallowip=192.168.77.51/255.255.255.0
```

2.配置代理端Nginx：

```
    server {
        listen       80;
        server_name  localhost;

        location / {
            proxy_pass http://192.168.77.188:3889;  #反代到端口3889
            proxy_set_header Authorization "Basic dGVzdDp0ZXN0MTIzNA==";  #本例中为test:test1234的base64编码
        }
    }
```

3.设置完成后，直接访问代理端即可进行RPC调用(无需输入用户名密码和端口号)，如：

```
zhongwenbins-MacBook-Pro:~ zhongwenbin$ curl  --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getinfo", "params": [] }' -H 'content-type: text/plain;' http://192.168.77.51/
{"result":{"version":140300,"protocolversion":70016,"walletversion":130000,"balance":0.00000000,"stake":0.00000000,"blocks":12250,"timeoffset":0,"connections":8,"proxy":"","difficulty":{"proof-of-work":1.52587890625e-05,"proof-of-stake":651324.7815933984},"testnet":false,"moneysupply":100029000,"keypoololdest":1505186997,"keypoolsize":100,"paytxfee":0.00000000,"relayfee":0.00400000,"errors":""},"error":null,"id":"curltest"}
```

以上设置实例仅供参考，读者可根据自身需求作更多设置，或是选择其它工具代替。

## 实用命令和文档

读者在部署节点或RPC调用时遇到问题，可优先参考以下实用命令或文档：

* 编译问题参考文档： [https://github.com/coinevo/evo/blob/master/README.md](https://github.com/coinevo/evo/blob/master/README.md)。
* 查看evod的所有选项：`./evod -help`
* 查看所有rpc命令： `./evo-cli help`
* 查看某rpc命令使用说明（如getinfo）： `./evo-cli help getinfo`
* 配置文件示例：[evo.conf（点击打开）](https://github.com/coinevo/evo/blob/1a926b980f03e97322c7dd787835bec1730f35d2/contrib/debian/examples/evo.conf)
