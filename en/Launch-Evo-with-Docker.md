# How to run Evo node with docker

## Quick start

This tutorial use Linux Ubuntu as example, OSX and windows platform are almost the same.

Suppose the reader has basic command line knowledge, and also have docker correctly installed and configured. If not, please learn about command line and Docker first.

For more details, please refer to [github](https://github.com/coinevo/evo-docker.git)

## Get docker image

You might take either way:

### Pull a image from Public Docker hub

```
$ docker pull evo/evo
```

### Or, build evo image with provided Dockerfile

The Dockerfile can be downloaded on github: [Dockerfile](https://github.com/coinevo/evo-docker/blob/master/release/Dockerfile)

```
$docker build --rm -t evo/evo .
```

For historical versions, please visit [docker hub](https://hub.docker.com/r/evo/evo/)

## Prepare data path and evo.conf

In order to use user-defined config file, as well as save block chain data, -v option for docker is recommended.

First chose a path to save evo block chain data:

```
sudo rm -rf /data/evo-data
sudo mkdir -p /data/evo-data
sudo chmod a+w /data/evo-data
```

Create your config file, refer to the example [evo.conf]!(https://github.com/coinevo/evo/blob/1a926b980f03e97322c7dd787835bec1730f35d2/contrib/debian/examples/evo.conf). Note rpcuser and rpcpassword to required for later `evo-cli` usage for docker, so it is better to set those two options. Then please create the file ${PWD}/evo.conf with content:

```
rpcuser=evo
rpcpassword=evotest
```
## Launch evod

To launch evo node:

```
## to launch evod
$ docker run -d --rm --name evo_node \
             -v ${PWD}/evo.conf:/root/.evo/evo.conf \
             -v /data/evo-data/:/root/.evo/ \
             evo/evo evod

## check docker processed
$ docker ps

## to stop evod
$ docker run -i --network container:evo_node \
             -v ${PWD}/evo.conf:/root/.evo/evo.conf \
             -v /data/evo-data/:/root/.evo/ \
             evo/evo evo-cli stop
```

`${PWD}/evo.conf` will be used, and blockchain data saved under /data/evo-data/

## Interact with `evod` using `evo-cli`

Use following docker command to interact with your evo node with `evo-cli`:

```
$ docker run -i --network container:evo_node \
             -v ${PWD}/evo.conf:/root/.evo/evo.conf \
             -v /data/evo-data/:/root/.evo/ \
             evo/evo evo-cli getinfo
```

For more evo-cli commands, use:

```
$ docker run -i --network container:evo_node \
             -v ${PWD}/evo.conf:/root/.evo/evo.conf \
             -v /data/evo-data/:/root/.evo/ \
             evo/evo evo-cli help
```
