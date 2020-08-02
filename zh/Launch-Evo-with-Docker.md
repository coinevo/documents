# 如何用Docker运行Evo节点

## 导读

本教程将介绍Evo Docker镜像的使用方法。教程实例环境为Linux Ubuntu。OSX和Windows操作基本相同，不赘述。

教程假设读者能够熟练使用Linux/OSX命令行工具，并已正确安装Docker环境，熟悉基本Docker命令。若不符合此要求，请自行搜索Docker相关安装和使用教程，完成安装后继续阅读。

## 获取Evo Docker镜像

请选用以下两种方式之一获取镜像:

### 1. 从Docker hub直接拉取镜像

```
$ docker pull evo/evo
```
或者，

### 2. 通过DockerFile构建镜像

Dockerfile地址为：[https://github.com/pandazwb/evo-docker/blob/master/release/Dockerfile](https://github.com/pandazwb/evo-docker/blob/master/release/Dockerfile)

可直接下载文件，或复制内容到本地Dockerfile。

构建镜像的命令为：

```
$docker build --rm -t evo/evo.
```

## 创建本地数据路径和配置文件

Docker容器中产生的数据在默认条件下不会保存，用户可以将数据导出，自动保存到本地。本教程建立的数据路径为`/data/evo-data/`，读者可自定义需要的数据路径。建立时请确保此路径为空，且与其他程序无冲突：

```
sudo rm -rf /data/evo-data
sudo mkdir -p /data/evo-data
sudo chmod a+w /data/evo-data
```

(注：上述为Linux命令。mac和windows用户可自行建立数据路径。)

为了实现rpc调用，必须设置`rpcuser`和`rpcpassword`。可以通过evo.conf配置文件完成设置。请在本地建立`${PWD}/evo.conf`文件（其中`${PWD}`为当前路径），包含内容为：

```
rpcuser=evo
rpcpassword=evotest
```

如果用户还有其他参数需要配置，可以参考[配置文件范例(点击打开)](https://github.com/coinevo/evo/blob/1a926b980f03e97322c7dd787835bec1730f35d2/contrib/debian/examples/evo.conf)。

## 运行Evo节点

完成上述准备工作后，即可运行Evo节点，命令如下:

```
$ docker run -d --rm --name evo_node \
             -v ${PWD}/evo.conf:/root/.evo/evo.conf \
             -v /data/evo-data/:/root/.evo/ \
             evo/evo evod
```

通过docker命令可以查看容器的运行状态：

```
$ docker ps
```

关闭容器中evo节点，可用如下命令：

```
$ docker run -i --network container:evo_node \
             -v ${PWD}/evo.conf:/root/.evo/evo.conf \
             -v /data/evo-data/:/root/.evo/ \
             evo/evo evo-cli stop
```

上述命令采用`${PWD}/evo.conf`文件中的配置，并且所有区块数据会保存到本地`/data/evo-data`路径中。

## 通过`evo-cli`与`evod`进行交互

通过如下Docker命令，可使用`evo-cli`与容器中运行的evo节点进行交互，如:

```
$ docker run -i --network container:evo_node \
             -v ${PWD}/evo.conf:/root/.evo/evo.conf \
             -v /data/evo-data/:/root/.evo/ \
             evo/evo evo-cli getinfo
```

如需获取完整的evo-cli命令列表，请使用:

```
$ docker run -i --network container:evo_node \
             -v ${PWD}/evo.conf:/root/.evo/evo.conf \
             -v /data/evo-data/:/root/.evo/ \
             evo/evo evo-cli help
```
