# network command

```shell
docker network ls
docker network create
docker network connect
docker network disconnect
docker network prune
docker network rm
```

```text
NETWORK ID     NAME      DRIVER    SCOPE
ab61656424cf   bridge    bridge    local
0ab35d18775f   host      host      local
b86f88f1c4c1   none      null      local
```

# 网络模式

```shell
docker inpsect bridge
```

```text
[
    {
        "Name": "bridge",
        "Scope": "local",
        "Driver": "bridge",
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.17.0.0/16",
                    "Gateway": "172.17.0.1"
                }
            ]
        },
        "Options": {
            "com.docker.network.bridge.name": "docker0",
        },
    }
]
```

## bridge

创建一个 Docker 容器时，默认情况下，Docker 会使用 bridge 网络。
每个容器都会被分配一个内部的 IP 地址，这个地址在默认的 Docker 网络中是唯一的。容器可以通过容器名或者 IP 地址进行通信

bridge 类似于一个NAT路由器，网络内所有的设备分配一个内部IP，而对外只暴露一个固定IP，也就是Gateway。bridge 本身并不对外暴露 IP 地址；相反，它允许容器通过 NAT 来共享宿主机的公网 IP 地址。这意味着从外部网络来看，所有来自容器的流量都像是来自宿主机的 IP 地址

## host

用 host 网络模式，容器将不会获得自己的 IP 地址，而是使用宿主机的网络堆栈。 这使得容器能够直接与宿主机的网络进行交互，但这也意味着容器可以访问宿主机上的所有端口。

使用host网络模式的时候，就相当于一个应用进程，使用host的ip和端口. 使用 host 网络模式可以提供更好的网络性能，因为它避免了网络地址转换（NAT）的开销. 

## container

新创建的容器没有自己的IP，而是和一个指定的container共享ip和端口。两个container只共享网络，但文件系统还是隔离的. 但如果被依赖的容器关掉之后，依赖容器也失去了网络支持

```shell
--network container:tomcat85
```

## 自定义网络

只有在同一个网络才能够用容器名互相连接，因为ip地址对应的容器是会变动的

```shell
docker network create --driver bridge mynetwork
docker run --network <network-name> -d <image-name>
```

如果容器已经创建，可以使用 docker network connect 命令将其连接到自定义网络：

```shell
docker network connect <network-name> <container-name>
```

