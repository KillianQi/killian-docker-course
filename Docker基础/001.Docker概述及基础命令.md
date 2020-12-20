# Docker 概述

## Docker 为什么会出现

一款产品从开发到上线，经历两套环境，包括应用环境和应用配置。

常遇到一个问题就是：在A的环境中可以运行，但是不能再B的环境中运行！

版本更新导致服务不可用，对于运维来说，考验十分大。环境配置十分麻烦，每一个机器都要部署环境，例如Redias, ES, Hadoop...，费时费力。

传统发布项目，都是只发布jar，war等，剩余环境部署都交由运维完成，但现在在Docker的帮助下，可以将开发应用和部署环境同时打包部署上线，一套流程完成。

![20201220113021](https://raw.githubusercontent.com/KillianQi/KillianQi-Killian-Private-Image/main/img/20201220113021.png)

Docker的技术实现方式类同于集装箱，核心思想就是隔离。

Docker 通过隔离机制，将服务器利用到极致。

所有的技术的出现都是因为问题的出现，我们需要解决问题。

> Docker

Dokcer 是基于Go语言开发的开源项目

官网：https://www.docker.com/
![20201220114219](https://raw.githubusercontent.com/KillianQi/KillianQi-Killian-Private-Image/main/img/20201220114219.png)

Docker官网文档地址：https://docs.docker.com/
![20201220114327](https://raw.githubusercontent.com/KillianQi/KillianQi-Killian-Private-Image/main/img/20201220114327.png)

Docker官方仓库地址：https://hub.docker.com/
![20201220114520](https://raw.githubusercontent.com/KillianQi/KillianQi-Killian-Private-Image/main/img/20201220114520.png)


## Docker能做什么
>传统的虚拟机

![20201220114806](https://raw.githubusercontent.com/KillianQi/KillianQi-Killian-Private-Image/main/img/20201220114806.png)

虚拟机技术的缺点：
1.系统资源占用多
2.冗余步骤多
3.启动慢

>容器化技术


*容器化技术不是模拟的一个完整的操作系统*

![20201220115330](https://raw.githubusercontent.com/KillianQi/KillianQi-Killian-Private-Image/main/img/20201220115330.png)


比较Docker 和虚拟机技术的不同：
* 传统虚拟机，虚拟相关硬件，运行一个完整的操作系统，然后在这个操作系统上安装和运行软件
* 容器内的应用直接运行在宿主机的内容，容器是没有自己的内核的，也没有虚拟硬件，所以轻便
* 每个容器间是互相隔离的，每个容器都有自己的文件系统，互不影响


> DevOps 开发运维


**应用更快的交付和部署**

传统：繁杂的帮助文档，安装成
Docker：打包镜像发布测试，一键运行

**更便捷的升级和扩容**
Docker技术使得部署应用就像搭积木一样简单！
打包为一个镜像，弹性扩展

**更简单的系统运维**
在容器化之后，开发，测试环境都是高度一致的

**更高效的计算资源利用**
Docker是内核级别的虚拟化，可以在一个物理机上运行大量的容器实例，有效节省资源，服务器性能可以被压榨到极致！



## Docker的安装
### Docker的基本组成

![20201220120858](https://raw.githubusercontent.com/KillianQi/KillianQi-Killian-Private-Image/main/img/20201220120858.png)

**镜像（image）：**
Docker镜像就好比是一个模板，可以通过这个模板来创建容器服务，通过一个镜像可以创建多个容器。

**容器（container）：**
Docker利用容器技术，独立运行一个或多个应用，是通过镜像来创建的。
简而言之，可以把一个容器理解为一个基础的Linux系统

**仓库（repository）：**
仓库就是存放镜像的地方。仓库分为共有仓库和私有仓库，比如Docker Hub，Aliyun等。
Docker安装完成后默认使用的是官方源，如需使用阿里云，腾讯云等容器服务，需单独配置镜像加速。

### 安装Docker
> 环境准备

1.Linux基础
2.操作系统（Linux，Windows等）
3.连接至服务器

> 环境查看

```
# 系统内核版本高于3.10
[root@killiandocker01 /]# uname -r
3.10.0-693.el7.x86_64
```
```
# 系统版本
[root@killiandocker01 /]# cat /etc/os-release
NAME="CentOS Linux"
VERSION="7 (Core)"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="7"
PRETTY_NAME="CentOS Linux 7 (Core)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:7"
HOME_URL="https://www.centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"

CENTOS_MANTISBT_PROJECT="CentOS-7"
CENTOS_MANTISBT_PROJECT_VERSION="7"
REDHAT_SUPPORT_PRODUCT="centos"
REDHAT_SUPPORT_PRODUCT_VERSION="7"

```

> 安装

帮助文档 https://docs.docker.com/engine/install
```
# 1.卸载旧版本Docker
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

```
# 2. 安装Docker需要的依赖环境
yum install -y yum-utils
```
```
# 3. 设置镜像的仓库
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo         #默认是从国外镜像节点下载，与阿里云节点二选一
    http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo #该节点为阿里云节点
    
    
yum update           #完成后更新yum并使之生效
yum makecache fast
```
```
# 4. 安装Docker  docker-ce 社区版 docker-ee为企业版
yum install docker-ce docker-ce-cli containerd.io
```
```
# 5. 启动Docker
systemctl start docker
```
```
# 6. 使用Docker Version查看Docker安装是否成功
```

![20201220125055](https://raw.githubusercontent.com/KillianQi/KillianQi-Killian-Private-Image/main/img/20201220125055.png)

```
# 7. Hello-world
docker run hello-world
# 以下为命令及输出信息

[root@killiandocker01 /]# docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
0e03bdcc26d7: Pull complete
Digest: sha256:1a523af650137b8accdaed439c17d684df61ee4d74feac151b5b337bd29e7eec
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

```
```
# 8.查看下载的hello-world的镜像
docker images
[root@killiandocker01 /]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
hello-world   latest    bf756fb1ae65   11 months ago   13.3kB
```

### 了解卸载Docker
``` 
# 1.卸载Docker所需依赖
yum remove docker-ce docker-ce-cli containerd.io
```
```
# 2.删除资源
rm -rf /var/lib/docker      #docker的默认工作路径
```

### 阿里云镜像加速（针对阿里云服务器）
1.进入容器服务
![20201220130309](https://raw.githubusercontent.com/KillianQi/KillianQi-Killian-Private-Image/main/img/20201220130309.png)

2.找到镜像加速地址
![20201220130409](https://raw.githubusercontent.com/KillianQi/KillianQi-Killian-Private-Image/main/img/20201220130409.png)

3.配置使用
```
sudo mkdir -p /etc/docker     

sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://[your_own_account].mirror.aliyuncs.com"]  
}
EOF

# 编辑配置文件
# 注意链接中填入自己的账户信息

sudo systemctl daemon-reload

sudo systemctl restart docker
```

### 回顾Hello-World流程

```
[root@killiandocker01 /]# docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
0e03bdcc26d7: Pull complete
Digest: sha256:1a523af650137b8accdaed439c17d684df61ee4d74feac151b5b337bd29e7eec
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

![20201220131708](https://raw.githubusercontent.com/KillianQi/KillianQi-Killian-Private-Image/main/img/20201220131708.png)


### 底层原理
**Docker是怎么工作的**

Docker是一个Client - Server 结构的系统，Docker的守护进程运行在主机上。通过Socket从客户端访问。

Docker接收到Docker-Client 的指令，就会执行这个命令

![20201220132335](https://raw.githubusercontent.com/KillianQi/KillianQi-Killian-Private-Image/main/img/20201220132335.png)

**Docker为什么比Virtual Machine快？**
1. Docker 有着比VM更少的抽象层
2. Docker 利用的是宿主机的内核，VM需要额外搭建一个Guest OS
![20201220132634](https://raw.githubusercontent.com/KillianQi/KillianQi-Killian-Private-Image/main/img/20201220132634.png)

所以说，新建一个容器的时候，Docker 不需要像虚拟机一样重新加载一个操作系统内核，避免引导。
虚拟机是加载一个Guest OS，启动速度约为分钟级；而Docker 是利用宿主机的操作系统，省略了这个过程，启动速度为秒级启动。

## Docker的常用命令
### 帮助命令

```
docker version                   #显示docker的版本信息
docker info                      #显示docker的系统信息，包括镜像和容器的数量
docker [command] --help          #帮助命令
```

**帮助文档**
文档地址：https://docs.docker.com/reference/
![20201220133721](https://raw.githubusercontent.com/KillianQi/KillianQi-Killian-Private-Image/main/img/20201220133721.png)


### 镜像命令
**docker images** 查看所有本地主机上的镜像
```
[root@killiandocker01 /]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
hello-world   latest    bf756fb1ae65   11 months ago   13.3kB

#解释
REPOSITORY  镜像的仓库源
TAG         镜像的标签
IMAGE ID    镜像的ID
CREATED     镜像的创建时间
SIZE        镜像的大小

#可选项

Options:
  -a, --all             Show all images (default hides intermediate images) #列出所有镜像
  -q, --quiet           Only show image IDs                                 #只显示镜像的ID
```

**docker search**搜索镜像
```
[root@killiandocker01 /]# docker search mysql
NAME                              DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql                             MySQL is a widely used, open-source relation…   10281     [OK]
mariadb                           MariaDB is a community-developed fork of MyS…   3801      [OK]
mysql/mysql-server                Optimized MySQL Server Docker images. Create…   750                  [OK]
percona                           Percona Server is a fork of the MySQL relati…   515       [OK]

#可选项，通过收藏数量来过滤
--fileter=[TAG]=value       #筛选标签(TAG) 为值(value)

[root@killiandocker01 /]# docker search mysql --filter=STARS=5000
NAME      DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql     MySQL is a widely used, open-source relation…   10281     [OK]
```

**docker pull** 拉取镜像
```
# 下载镜像 docker pull 镜像名称[:tag] 
[root@killiandocker01 /]# docker pull mysql
Using default tag: latest               #如果不写tag，默认就是latest
latest: Pulling from library/mysql
6ec7b7d162b2: Pull complete             #分层下载，layer 为docker image的核心，ufs联合文件系统
fedd960d3481: Pull complete
7ab947313861: Pull complete
64f92f19e638: Pull complete
3e80b17bff96: Pull complete
014e976799f9: Pull complete
59ae84fee1b3: Pull complete
ffe10de703ea: Pull complete
657af6d90c83: Pull complete
98bfb480322c: Pull complete
9f2c4202ac29: Pull complete
a369b92bfc99: Pull complete
Digest: sha256:365e891b22abd3336d65baefc475b4a9a1e29a01a7b6b5be04367fcc9f373bb7     #签名
Status: Downloaded newer image for mysql:latest              
docker.io/library/mysql:latest            #真实地址


docker pull mysql 等价于
docker pull docker.io/library/mysql:latest

# 指定版本下载
[root@killiandocker01 /]# docker pull mysql:5.7
5.7: Pulling from library/mysql
6ec7b7d162b2: Already exists
fedd960d3481: Already exists
7ab947313861: Already exists
64f92f19e638: Already exists
3e80b17bff96: Already exists
014e976799f9: Already exists
59ae84fee1b3: Already exists
7d1da2a18e2e: Pull complete
301a28b700b9: Pull complete
979b389fc71f: Pull complete
403f729b1bad: Pull complete
Digest: sha256:d4ca82cee68dce98aa72a1c48b5ef5ce9f1538265831132187871b78e768aed1
Status: Downloaded newer image for mysql:5.7
docker.io/library/mysql:5.7
```

![20201220141458](https://raw.githubusercontent.com/KillianQi/KillianQi-Killian-Private-Image/main/img/20201220141458.png)

**docker rmi** 删除镜像
``` 
[root@killiandocker01 /]# docker rmi -f [镜像id]                       # 删除指定的容器
[root@killiandocker01 /]# docker rmi -f [镜像id] [镜像id] [镜像id]      # 删除多个容器
[root@killiandocker01 /]# docker rmi -f $(docker images -aq)           # 删除所有容器
```

### 容器命令
**有了镜像才可以创建容器**
```
docker pull centos
```
**新建容器并启动**
```
docker run [可选参数] image

# 参数说明
--name="Name"   容器名称，用来区分容器
-d              以后台运行方式，启动docker
-it             使用交互方式运行，进入容器查看内容
-p              指定容器的端口  -p 8080:8080
    -p  主机ip：主机端口：容器端口
    -p  主机端口：容器端口
    -p  容器端口
    容器端口
-P              随机暴露一个端口

# 测试，启动并进入容器
[root@killiandocker01 /]# docker run -it centos /bin/bash   #注意运行命令后，host的变化
[root@db7ffd01b7f6 /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
# 从容器中退回到主机
[root@db7ffd01b7f6 /]# exit
exit
[root@killiandocker01 /]# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```

**列出所有容器** 
```
docker ps 
```

```
docker ps -a          # 列出当前正在运行的容器以及已经退出的历史容器
[root@killiandocker01 /]# docker ps -a
CONTAINER ID   IMAGE          COMMAND       CREATED         STATUS                          PORTS     NAMES
db7ffd01b7f6   centos         "/bin/bash"   4 minutes ago   Exited (0) About a minute ago             zen_ishizaka
31f21d48c7a7   bf756fb1ae65   "/hello"      2 hours ago     Exited (0) 2 hours ago                    wizardly_swanson
```
```
docker ps -n=[value]  # 显示最近创建的[value]个容器
[root@killiandocker01 /]# docker ps -a -n=1
CONTAINER ID   IMAGE     COMMAND       CREATED         STATUS                     PORTS     NAMES
db7ffd01b7f6   centos    "/bin/bash"   6 minutes ago   Exited (0) 4 minutes ago             zen_ishizaka
```
```
docker ps -q          #只显示容器id
[root@killiandocker01 /]# docker ps -aq
db7ffd01b7f6
31f21d48c7a7
```

**退出容器**
```
exit            # 直接停止容器并退出
Ctrl + P + Q    #容器不停止退出
```

**删除容器**
```
docker rm 容器id                    # 删除指定的容器，不能删除正在运行的容器，-f可强制删除
docker rm -f $(docker ps -aq)       # 删除所有的容器
docker rm -a -q | xargs docker rm   # 删除所有的容器
```

**启动和停止容器的操作**
```
docker start    [容器id]        # 启动容器
docker restart  [容器id]        # 重启容器
docker stop     [容器id]        # 停止当前正在运行的容器
docker kill     [容器id]        # 强制停止当前正在运行的容器
```

### 常用的其他命令
**后台启动容器**
```
# 使用docker run -d 启动镜像
[root@killiandocker01 /]# docker run -d centos
# 启动容器后，使用docker ps后发现，刚启动的容器又停止了
# docker容器使用后台云心，必须要有一个前台进程，docker发现没有前台应用，就会自动停止
```

**查看日志**
```
docker logs

[root@killiandocker01 /]# docker logs --help

Usage:  docker logs [OPTIONS] CONTAINER

Fetch the logs of a container

Options:
      --details        Show extra details provided to logs
  -f, --follow         Follow log output
      --since string   Show logs since timestamp (e.g. 2013-01-02T13:23:37Z) or relative (e.g. 42m for 42 minutes)
  -n, --tail string    Number of lines to show from the end of the logs (default "all")
  -t, --timestamps     Show timestamps
      --until string   Show logs before a timestamp (e.g. 2013-01-02T13:23:37Z) or relative (e.g. 42m for 42 minutes)
```
**查看容器中的进程信息**
```
docker top [docker_id]

[root@killiandocker01 /]# docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED         STATUS         PORTS     NAMES
fbee06ce2532   centos    "/bin/bash"   6 minutes ago   Up 6 minutes             musing_sutherland
[root@killiandocker01 /]# docker top fbee06ce2532
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                25322               25300               0                   15:19               pts/0               00:00:00            /bin/bash
root                25467               25300               0                   15:20               pts/1               00:00:00            /bin/bash
[root@killiandocker01 /]#
```

**查看docker元数据信息**
```
docker inspect [docker_id]
[root@killiandocker01 /]# docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED         STATUS         PORTS     NAMES
fbee06ce2532   centos    "/bin/bash"   9 minutes ago   Up 9 minutes             musing_sutherland
[root@killiandocker01 /]# docker inspect fbee06ce2532
[
    {
        "Id": "fbee06ce2532d333a10bf5835459919ed18f4adfbf48e6694e1841a6d327dd4e",
        "Created": "2020-12-20T07:19:07.192332747Z",
        "Path": "/bin/bash",
        "Args": [],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 25322,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2020-12-20T07:19:07.556048552Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:300e315adb2f96afe5f0b2780b87f28ae95231fe3bdd1e16b9ba606307728f55",
        "ResolvConfPath": "/var/lib/docker/containers/fbee06ce2532d333a10bf5835459919ed18f4adfbf48e6694e1841a6d327dd4e/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/fbee06ce2532d333a10bf5835459919ed18f4adfbf48e6694e1841a6d327dd4e/hostname",
        "HostsPath": "/var/lib/docker/containers/fbee06ce2532d333a10bf5835459919ed18f4adfbf48e6694e1841a6d327dd4e/hosts",
        "LogPath": "/var/lib/docker/containers/fbee06ce2532d333a10bf5835459919ed18f4adfbf48e6694e1841a6d327dd4e/fbee06ce2532d333a10bf5835459919ed18f4adfbf48e6694e1841a6d327dd4e-json.log",
        "Name": "/musing_sutherland",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": [
            "90604c0e065b5b03af1a4a33bbd4104fc3fce14e3e5c213bc96a7b8660d8d959"
        ],
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "host",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/39bbd3427d86c66474171812761080915b145e69225278d5cd02783e89c6d23b-init/diff:/var/lib/docker/overlay2/121930a68b3e4b588bedb4a732c0b2a22429679d09dba875b379c00d536f5f04/diff",
                "MergedDir": "/var/lib/docker/overlay2/39bbd3427d86c66474171812761080915b145e69225278d5cd02783e89c6d23b/merged",
                "UpperDir": "/var/lib/docker/overlay2/39bbd3427d86c66474171812761080915b145e69225278d5cd02783e89c6d23b/diff",
                "WorkDir": "/var/lib/docker/overlay2/39bbd3427d86c66474171812761080915b145e69225278d5cd02783e89c6d23b/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "fbee06ce2532",
            "Domainname": "",
            "User": "",
            "AttachStdin": true,
            "AttachStdout": true,
            "AttachStderr": true,
            "Tty": true,
            "OpenStdin": true,
            "StdinOnce": true,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/bash"
            ],
            "Image": "centos",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20201204",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS"
            }
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "e65926c8bb9002c6c0792eba1dc6ee1b315ab640ae1b3b57c6f1f3c15935ebf5",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/e65926c8bb90",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "60aa790d6d58f194670903401b8a23db462b1d37932e37246a03841723985536",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "cd62b31001f3a7f6056a7b687375a177bdd2100988a860c578c624b328e8b54c",
                    "EndpointID": "60aa790d6d58f194670903401b8a23db462b1d37932e37246a03841723985536",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]
[root@killiandocker01 /]#
```

**进入当前正在运行的容器**
```
方式一（会新建新的终端，可以在里面进行操作）
# 通常都是使用后台运行方式运行容器，但如果需要修改配置，进入容器时，则需要以下命令

docker exec -it  [容器id] bashShell
[root@killiandocker01 /]# docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS          PORTS     NAMES
fbee06ce2532   centos    "/bin/bash"   14 minutes ago   Up 14 minutes             musing_sutherland
[root@killiandocker01 /]# docker exec -it fbee06ce2532 /bin/bash
[root@fbee06ce2532 /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  test  tmp  usr  var
[root@fbee06ce2532 /]# ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 07:19 pts/0    00:00:00 /bin/bash
root        15     0  0 07:20 pts/1    00:00:00 /bin/bash
root        31     0  0 07:33 pts/2    00:00:00 /bin/bash
root        47    31  0 07:33 pts/2    00:00:00 ps -ef
```

```
方式二（进入容器正在执行的终端）
docker attach [容器id]
[root@killiandocker01 /]# docker attach  fbee06ce2532
[root@fbee06ce2532 /]#
```

**从容器内拷贝文件到主机上**
```
docker ps 容器id:容器内路径 目的主机的路径

[root@killiandocker01 home]# docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS          PORTS     NAMES
fbee06ce2532   centos    "/bin/bash"   26 minutes ago   Up 26 minutes             musing_sutherland
[root@killiandocker01 home]# docker exec -it fbee06ce2532 /bin/bash     
[root@fbee06ce2532 /]# cd /home/                                        # 进入相关目录
[root@fbee06ce2532 home]# pwd
/home
[root@fbee06ce2532 home]# ls                                            # 查看目录下的文件
killian.sh
[root@fbee06ce2532 home]# read escape sequence
[root@killiandocker01 home]# pwd                                        # 返回主机
/home
[root@killiandocker01 home]# ls
killian  killian.txt
[root@killiandocker01 home]# docker cp fbee06ce2532:/home/killian.sh /home # 将容器内文件复制到主机相关目录下
[root@killiandocker01 home]# pwd
/home
[root@killiandocker01 home]# ls                                         # 查看主机目录下文件
killian  killian.sh  killian.txt
[root@killiandocker01 home]#
```

### 小结
![20201220161922](https://raw.githubusercontent.com/KillianQi/KillianQi-Killian-Private-Image/main/img/20201220161922.png)
```
docker --help
  attach      Attach local standard input, output, and error streams to a running container     # 进入当前正在运行的镜像
  build       Build an image from a Dockerfile                                                  # 通过Dockfile定制镜像
  commit      Create a new image from a container's changes                                     # 提交当前容器为新的镜像
  cp          Copy files/folders between a container and the local filesystem                   # 从容器中拷贝指定的文件或目录到宿主机
  create      Create a new container                                                            # 创建一个新的容器，作用类似run，但不启动
  diff        Inspect changes to files or directories on a container's filesystem               # 查看docker 的变化
  events      Get real time events from the server                                              # 从docker服务获取容器实时事件
  exec        Run a command in a running container                                              # 在已存在的容器上执行命令
  export      Export a container's filesystem as a tar archive                                  # 掏出容器的内容流作为一个tar归档文件
  history     Show the history of an image                                                      # 展示出一个镜像形成历史
  images      List images                                                                       # 列出系统当前镜像
  import      Import the contents from a tarball to create a filesystem image                   # 从tar包中的内容创建一个新的文件系统镜像
  info        Display system-wide information                                                   # 显示系统相关信息
  inspect     Return low-level information on Docker objects                                    # 查看容器详情
  kill        Kill one or more running containers                                               # 结束指定容器
  load        Load an image from a tar archive or STDIN                                         # 从一个tar包中加载一个镜像
  login       Log in to a Docker registry                                                       # 注册或登录一个docker服务器
  logout      Log out from a Docker registry                                                    # 从当前登录的docker服务器中登出
  logs        Fetch the logs of a container                                                     # 输出当前容器日志信息
  pause       Pause all processes within one or more containers                                 # 暂停容器
  port        List port mappings or a specific mapping for the container                        # 查看映射端口对应的容器内部源端口
  ps          List containers                                                                   # 列出容器列表
  pull        Pull an image or a repository from a registry                                     # 从docker镜像源服务器拉取指定的镜像或者哭镜像
  push        Push an image or a repository to a registry                                       # 推送指定的镜像或者库镜像至docker源服务器
  rename      Rename a container                                                                # 重命名容器
  restart     Restart one or more containers                                                    # 重启一个或多个正在运行的容器
  rm          Remove one or more containers                                                     # 移除一个或多个容器
  rmi         Remove one or more images                                                         # 移除一个或多个镜像
  run         Run a command in a new container                                                  # 创建一个新的容器，并运行命令
  save        Save one or more images to a tar archive (streamed to STDOUT by default)          # 保存一个镜像为一个tar包
  search      Search the Docker Hub for images                                                  # 在docker hub中搜索镜像
  start       Start one or more stopped containers                                              # 启动停止中的容器
  stop        Stop one or more running containers                                               # 停止容器
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE                             # 给源中的镜像打标签
  top         Display the running processes of a container                                      # 查看容器中运行的进程信息
  unpause     Unpause all processes within one or more containers                               # 取消暂停容器
  update      Update configuration of one or more containers                                    # 更新一个或多个容器的配置
  version     Show the Docker version information                                               # 查看docker的版本号
  wait        Block until one or more containers stop, then print their exit codes              # 截取容器停止时的退出状态值
```
