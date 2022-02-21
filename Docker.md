# 1.介绍

## 1.1 为什么会有Docker出现

一款产品从开发到上线，从操作系统到运行环境，再到应用配置。作为开发+运维之间的协作我们需要关心很多东西，这也是互联网公司都不得不面对的一个问题，特别是各种版本的迭代之后，不同版本之间的兼容对运维人员都是一种考验。

Dock发展之所以这么迅速，也是因为它给了一个标准化的解决方案。
在安装Docker时，把原始环境一模一样复制过来，开发人员利用Docker可以消除写作编码时“在我机器上可正常工作”的问题。达到应用程序跨平台的无缝接轨运作。

![image-20211229184148062](G:\Typora 文档\Docker.assets\image-20211229184148062-16407745100773.png)

## 1.2 Docker理念

* Docker是基于Go语言实现的云开源项目。
* Docker的主要目标是“Build，Ship and Run Any App,Anywhere”，也就是通过对应用组件的封装、分发、部署、运行等生命周期的管理，使用户的APP（可以是一个WEB应用或数据库应用等等）及其运行环境能够做到“一次封装，到处运行”。
* Linux容器技术的出现就解决了这样一个问题，而 Docker 就是在它的基础上发展过来的。将应用运行在 Docker 容器上面，而 Docker 容器在任何操作系统上都是一致的，这就实现了跨平台、跨服务器。只需要一次配置好环境，换到别的机子上就可以一键部署好，大大简化了操作

![image-20211229191020683](G:\Typora 文档\Docker.assets\image-20211229191020683-16407762219334.png)

**解决了运行环境和配置问题软件容器，方便做持续集成并有助于整体发布的容器虚拟化技术**

## 1.3 虚拟机技术

虚拟机是带环境安装的一种解决方案，它可以在一种操作系统里面运行另一种操作系统，比如在Windows操作系统里面运行Linux系统，应用程序对此毫无感知，因为虚拟机上和真实系统一模一样，而对于底层系统来说，虚拟机就是一种普通文件，不需要直接删除对其他部分没有影响。



## 1.4 容器虚拟化技术

Linux发展出了另一种虚拟化技术，Linux容器简写为LXC（Linux Containers）。Linux容器不是模拟一个完整的操作系统，而是对进程进行隔离。有了容器就可以将软件运行所需要的所有资源打包到一个隔离的容器中。容器与虚拟机不同，不需要捆绑一整套操作系统，只需要软件工作所需的库资源和设置。系统因此而变得高效轻量并保证部署在任何环境中的软件都可以运行。



**比较Docker和传统虚拟化方式的不同之处**

Docker 项目的目标是实现轻量级的操作系统虚拟化解决方案。 Docker 的基础是 Linux 容器（LXC）等技术。

在 LXC 的基础上 Docker 进行了进一步的封装，让用户不需要去关心容器的管理，使得操作更为简便。用户操作 Docker 的容器就像操作一个快速轻量级的虚拟机一样简单。

下面的图片比较了 Docker 和传统虚拟化方式的不同之处，可见容器是在操作系统层面上实现虚拟化，直接复用本地主机的操作系统，而传统方式则是在硬件层面实现。

![image-20211229194729830](G:\Typora 文档\Docker.assets\image-20211229194729830-16407784510045.png)

# 2. 安装Docker

## 2.1 Linux版本要求

Docker支持以下的CentOS版本:
[CentOS7](https://so.csdn.net/so/search?q=CentOS7)(64-bit)
CentOS6.5(64-bit)或更高的版本

## 2.2 Docker的基本组成

![image-20211229202705790](G:\Typora 文档\Docker.assets\image-20211229202705790-16407808280946.png)

1. 镜像（images）

- **镜像就是模版，容器是这个镜像的实例。**
- 就是一个只读的模版，镜像可以用来创建Docker容器，一个镜像可以创建很多容器。容器与镜像的关系类似于面向对象变成的对象与类。

| Docker | 面向对象 |
| ------ | -------- |
| 容器   | 对象     |
| 镜像   | 类       |

2. 容器（container）

- Docker利用容器(Container)独立运行的一个或一组应用。容器是用镜像创建的运行实例。
- 它可以被启动，开始，停止，删除。每个容器都是相互隔离的，保证安全的平台。
- 可以把容器看作是一个简易版的Linux环境（包括root用户权限，进程空间，用户空间和网络空间等）和运行在其中的应用程序。容器的定义和镜像一模一样，也是一堆层的统一视角，唯一区别在于容器的最上面那一层是可读可写的。

3. 仓库（Repository）

- 集中存放镜像文件的场所。仓库和仓库注册服务器(Registry)是有区别的。仓库注册服务器上往往存放着多个仓库，每个仓库中又包含了多个镜像，每个镜像又不同的标签。
- 仓库分为公开仓库和私有仓库两种形式。最大公开仓库（Docker hub)。

**总结**

- Docker本身是一个容器运行载体称为管理引擎，我们把应用程序和配置依赖打包好形成一个可交付的运行环境，这个打包好的运行环境就似乎image镜像文件，只有通过这个镜像文件才能生成Docker容器。image文件可以看作是容器的模板，Docker根据image生成容器的实例，同一个image文件，可以生成多个同时运行的容器实例。
- image文件生成的容器实例，本身也是夜歌文件，称为镜像文件。
- 一个容器运行一种服务，当我们需要的时候，就可以通过docker客户端创建一个对应的运行实例，也就是我们容器。
- 仓库就是放了一堆镜像的地方，我们可以把镜像发布到仓库中，需要的时候从仓储中拉下来就行。

## 2.3 安装Docker

CentOS 6

```shell
yum install -y epel-release
yum install -y docker-io
/etc/sysconfig/docker
service docker start
docker version
```

更高版本使用https://docs.docker.com/engine/install/centos/链接安装

## 2.4 阿里云镜像加速

针对CentOS7系统要求，配置环境设置为：

```
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://6osq6ebc.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```



## 2.5 hello world实例

```bash
[root@dongwei ~]# docker run hello-world

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

hello world运行过程中背后的逻辑：由于修改了镜像所以会直接去阿里云镜像仓库找，不去Docker Hub上查找该镜像。

![image-20220113204729516](G:\Typora 文档\Docker.assets\image-20220113204729516-16420780511341.png)

# 3. Docker底层原理

## 3.1 Docker是怎么工作的？

Docker是一个Client-Server结构的系统，Docker守护进程运行在主机上，然后通过Socket连接从客户端访问，守护进程从客户端接收命令并管理运行在主机上的容量，**是一个运行时环境，就是我们前面所说到的集装箱**，结构示意图如下所示：

![image-20220113211726788](G:\Typora 文档\Docker.assets\image-20220113211726788-16420798479842.png)

## 3.2 为什么Docker比VM要快

* docker有着比虚拟机更少的抽象层。由于docker不需要Hypervisor实现硬件资源虚拟化，运行在docker容器上的程序直接使用的都是实际物理机的硬件资源，因此在CPU、内存利用率上docker将会在效率上有着明显的优势。
* docker利用的是宿主机的内核，而不需要Guest OS。因此，当新建一个容器时，docker不需要和其他虚拟机一样重新加载一个操作系统内核，当新建一个虚拟机时，虚拟机软件需要加载Guest OS，整个新建过程是分钟级别的，而docker由于直接利用宿主机的操作系统，则省略了返回过程，因此新建一个docker容器只需要几秒钟。

两种区别示意图：

![image-20220113213145435](G:\Typora 文档\Docker.assets\image-20220113213145435-16420807070623.png)



# 4. Docker常用的命令

## 4.1 帮助命令

1. docker version

```bash
[root@dongwei ~]# docker verison
docker: 'verison' is not a docker command.
See 'docker --help'
[root@dongwei ~]# docker version
Client: Docker Engine - Community
 Version:           20.10.12
 API version:       1.41
 Go version:        go1.16.12
 Git commit:        e91ed57
 Built:             Mon Dec 13 11:45:41 2021
 OS/Arch:           linux/amd64
 Context:           default
 Experimental:      true

Server: Docker Engine - Community
 Engine:
  Version:          20.10.12
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.16.12
  Git commit:       459d0df
  Built:            Mon Dec 13 11:44:05 2021
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.4.12
  GitCommit:        7b11cfaabd73bb80907dd23182b9347b4245eb5d
 runc:
  Version:          1.0.2
  GitCommit:        v1.0.2-0-g52b36a2
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```



2. docker info

```bash
[root@dongwei ~]# docker info
Client:
 Context:    default
 Debug Mode: false
 Plugins:
  app: Docker App (Docker Inc., v0.9.1-beta3)
  buildx: Docker Buildx (Docker Inc., v0.7.1-docker)
  scan: Docker Scan (Docker Inc., v0.12.0)

Server:
 Containers: 3
  Running: 0
  Paused: 0
  Stopped: 3
 Images: 1
 Server Version: 20.10.12
 Storage Driver: overlay2
  Backing Filesystem: extfs
  Supports d_type: true
  Native Overlay Diff: true
  userxattr: false
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 1
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 io.containerd.runtime.v1.linux runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 7b11cfaabd73bb80907dd23182b9347b4245eb5d
 runc version: v1.0.2-0-g52b36a2
 init version: de40ad0
 Security Options:
  seccomp
   Profile: default
 Kernel Version: 3.10.0-957.el7.x86_64
 Operating System: CentOS Linux 7 (Core)
 OSType: linux
 Architecture: x86_64
 CPUs: 2
 Total Memory: 1.934GiB
 Name: dongwei
 ID: KNXR:LYO7:QXV4:GCP3:IVXW:N2YC:B64T:TWIF:NELA:SZR4:CDR7:3ANK
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Registry: https://index.docker.io/v1/
 Labels:
 Experimental: false
 Insecure Registries:
  127.0.0.0/8
 Registry Mirrors:
  https://6osq6ebc.mirror.aliyuncs.com/
 Live Restore Enabled: false
```



3. **docker --help**(这个比较重要)

```bash
[root@dongwei ~]# docker --help

Usage:  docker [OPTIONS] COMMAND

A self-sufficient runtime for containers

Options:
      --config string      Location of client config files (default "/root/.docker")
  -c, --context string     Name of the context to use to connect to the daemon (overrides DOCKER_HOST env var and default context set with "docker context use")
  -D, --debug              Enable debug mode
  -H, --host list          Daemon socket(s) to connect to
  -l, --log-level string   Set the logging level ("debug"|"info"|"warn"|"error"|"fatal") (default "info")
      --tls                Use TLS; implied by --tlsverify
      --tlscacert string   Trust certs signed only by this CA (default "/root/.docker/ca.pem")
      --tlscert string     Path to TLS certificate file (default "/root/.docker/cert.pem")
      --tlskey string      Path to TLS key file (default "/root/.docker/key.pem")
      --tlsverify          Use TLS and verify the remote
  -v, --version            Print version information and quit

Management Commands:
  app*        Docker App (Docker Inc., v0.9.1-beta3)
  builder     Manage builds
  buildx*     Docker Buildx (Docker Inc., v0.7.1-docker)
  config      Manage Docker configs
  container   Manage containers
  context     Manage contexts
  image       Manage images
  manifest    Manage Docker image manifests and manifest lists
  network     Manage networks
  node        Manage Swarm nodes
  plugin      Manage plugins
  scan*       Docker Scan (Docker Inc., v0.12.0)
  secret      Manage Docker secrets
  service     Manage services
  stack       Manage Docker stacks
  swarm       Manage Swarm
  system      Manage Docker
  trust       Manage trust on Docker images
  volume      Manage volumes

Commands:
  attach      Attach local standard input, output, and error streams to a running container
  build       Build an image from a Dockerfile
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container
  diff        Inspect changes to files or directories on a container's filesystem
  events      Get real time events from the server
  exec        Run a command in a running container
  export      Export a container's filesystem as a tar archive
  history     Show the history of an image
  images      List images
  import      Import the contents from a tarball to create a filesystem image
  info        Display system-wide information
  inspect     Return low-level information on Docker objects
  kill        Kill one or more running containers
  load        Load an image from a tar archive or STDIN
  login       Log in to a Docker registry
  logout      Log out from a Docker registry
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  ps          List containers
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  run         Run a command in a new container
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  search      Search the Docker Hub for images
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  version     Show the Docker version information
  wait        Block until one or more containers stop, then print their exit codes

Run 'docker COMMAND --help' for more information on a command.

```



## 4.2 镜像命令

1. **docker images：列出本地主机上所有的镜像**

```bash
[root@dongwei ~]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
hello-world   latest    feb5d9fea6a5   3 months ago   13.3kB
```

参数说明：

`REPOSITORY`：表示镜像的仓库源

`TAG`：镜像的标签

`CREATED`：镜像的创建时间

`SIZE`：镜像大小

同一个仓库源可以有多个TAG，代表的是这个仓库源有不同的版本，我们使用`REPOSITROY:TAG`来定义不同的镜像。如果你不指定一个镜像的版本标签，例如你只是用ubuntu，docker将会默认使用`ubuntu:latest`

镜像。

该命令还有其他OPTIONS选项：

`-a`：列出本地所有的镜像（含中间映像层）

`-q`：只显示镜像ID

`--digests`：显示镜像的摘要信息

`--no-trunc`：显示完整的镜像信息

```bash
[root@dongwei ~]# docker images -q
feb5d9fea6a5
[root@dongwei ~]# docker images --digests
REPOSITORY    TAG       DIGEST                                                                    IMAGE ID       CREATED        SIZE
hello-world   latest    sha256:2498fce14358aa50ead0cc6c19990fc6ff866ce72aeb5546e1d59caac3d0d60f   feb5d9fea6a5   3 months ago   13.3kB
[root@dongwei ~]# docker images --no-trunc
REPOSITORY    TAG       IMAGE ID                                                                  CREATED        SIZE
hello-world   latest    sha256:feb5d9fea6a5e9606aa995e879d862b825965ba48de054caab5ef356dc6b3412   3 months ago   13.3kB
```

2. **docker search 查找 xxx镜像名字，是在https://hub.docker,com中查找的**

该命令中的其他OPTIONS说明：

`--no-trunc`：显示完整的镜像描述

`-s`：列出收藏数不少于指定值的镜像

`--automated`：只列出automated build类型的镜像

3. **docker pull 镜像名称：下载镜像**
4. **docker rmi 镜像名称ID：删除镜像**

说明：

当在运行时需要强制删除，删除单个指令`docker rmi -f 镜像ID`

删除多个`docker rmi -f 镜像名称1:TAG 镜像名2:TAG`

删除全部`docker rmi -f $(docker images -qa)`

## 4.3 容器命令

有镜像才可以能创建容器 ，所以需要先创建一个CentOS镜像

```bash
docker pull centos
```

1. **新建并启动容器**

```bash
docker run [OPTIONS] IMAGES
```

OPTIONS中选型说明：

`--name`：为容器指定一个名称

`-i`：以交互模式运行容器，通常与`-t`配合使用

`-t`：为容器重新分配一个伪终端，通常与`-i`同时使用。

2. **列出当前所有的容器**

```bash
docker ps
```

OPTIONS说明：

`-a`：列出当前所有正在运行的容器+历史上运行过的

`-l`：显示最近创建的容器

`-n`：显示最近n个创建的容器

`-q`：静默模式，只显示容器编号

`--no-trunc`：不截断输出

3. **退出容器**

`exit`：容器停止退出

`ctrl + P + Q`：容器不停止退出

4. **启动容器**

```bash
docker start 容器ID或容器名
```

5. **重启容器**

```bash
docker restart 容器ID
```

6. **停止容器**

```bash
docker stop 容器ID
```

7. **强制停止容器**

```bash
docker kill 容器ID
```

8. **删除已经停止的容器**

```bash
docker rm 容器ID
```

一次删除多个容器

```bash
docker rm -f $(docker ps -a -q)
docker ps -a -q |xargs docker rm
```

**重要的知识点**

1. **启动守护式容器**

```bash
docker run -d 容器名
```

这样启动一个守护式容器会出现一个问题，使用`docker ps -a `的命令时会发现**容器已经退出**，很重要的一点是：**docker容器后台登录的话，就必须有一个前台进程**。容器运行的命令如果不是那些一直挂起的命令，就会自动退出。

2. **查看容器日志**

```bash
docker logs -f -t --tail 容器ID
```

`-f`：跟随最新的日志打印

`-t`：是加入时间戳

`--tail`：数字显示最后多少条

3. **查看容器内运行的进程**

```bash
docker top 容器ID
```

4. **查看容器内部细节**

```bash
docker inspect 容器ID
```

5. **进入运行的容器并以命令行交互**

* `docker exec -it 容器ID bashshell`

* 重新进入`docker attah 容器ID`

  上述两者的区别，`attach`直接进入容器启动命令的终端，不会启动新的进程，`exec`是在容器中打开新的终端，并且可以启动新的进程

6. **从容器内拷贝文件到主机上**

```bash
docker cp 容器ID:容器内路径 目的主机路径
```

# 5. Docker镜像

## 5.1 Docker镜像介绍

Docker镜像是一种轻量级、可执行的独立软件包，**用来打包软件运行环境和基于运行环境开发的软件**，它包含运行某个软件所需要的所有内容，包括代码、运行时库、环境变量以及配置文件。

1. **UnionFS联合文件系统**

**1）什么是UnionFS**
联合文件系统（Union File System）：2004年由纽约州立大学石溪分校开发，它可以把多个目录(也叫分支)内容联合挂载到同一个目录下，而目录的物理位置是分开的。UnionFS允许只读和可读写目录并存，就是说可同时删除和增加内容。UnionFS应用的地方很多，比如在多个磁盘分区上合并不同文件系统的主目录，或把几张CD光盘合并成一个统一的光盘目录(归档)。另外，具有写时复制(copy-on-write)功能UnionFS可以把只读和可读写文件系统合并在一起，虚拟上允许只读文件系统的修改可以保存到可写文件系统当中。

**2）docker的镜像rootfs，和layer的设计**
任何程序运行时都会有依赖，无论是开发语言层的依赖库，还是各种系统lib、操作系统等，不同的系统上这些库可能是不一样的，或者有缺失的。为了让容器运行时一致，docker将依赖的操作系统、各种lib依赖整合打包在一起（即镜像），然后容器启动时，作为它的根目录（根文件系统rootfs），使得容器进程的各种依赖调用都在这个根目录里，这样就做到了环境的一致性。

不过，这时你可能已经发现了另一个问题：难道每开发一个应用，都要重复制作一次rootfs吗（那每次pull/push一个系统岂不疯掉）？

比如，我现在用Debian操作系统的ISO做了一个rootfs，然后又在里面安装了Golang环境，用来部署我的应用A。那么，我的另一个同事在发布他的Golang应用B时，希望能够直接使用我安装过Golang环境的rootfs，而不是重复这个流程，那么本文的主角UnionFS就派上用场了。

**Docker镜像的设计中，引入了层（layer）的概念**，也就是说，用户制作镜像的每一步操作，都会生成一个层，也就是一个增量rootfs（一个目录），这样应用A和应用B所在的容器共同引用相同的Debian操作系统层、Golang环境层（作为只读层），而各自有各自应用程序层，和可写层。启动容器的时候通过UnionFS把相关的层挂载到一个目录，作为容器的根文件系统。

需要注意的是，rootfs只是一个操作系统所包含的文件、配置和目录，并不包括操作系统内核。这就意味着，如果你的应用程序需要配置内核参数、加载额外的内核模块，以及跟内核进行直接的交互，你就需要注意了：这些操作和依赖的对象，都是宿主机操作系统的内核，它对于该机器上的所有容器来说是一个“全局变量”，牵一发而动全身。


**特性：一次用时加载多个文件系统，但是从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录。**

2. **Docker镜像加载原理**

docker 的镜像实际上由一层一层的文件系统组成，这种层级的文件系统UnionFS。

bootfs(boot file system) 主要包含bootloader和kernel，bootloader 主要是引导加载kernel，Linux刚启动时会加载bootfs文件系统，在Docker镜像的最底层是bootfs。这一层与我们典型的Linux/Unix系统是一样的，包含boot加载器和内核。当boot加载完成之后整个内核就存在内存中了，此时内存的使用权已由bootfs转交给内核，此时系统也会卸载bootfs。

rootfs （root file system），在bootfs之上。包含的就是典型Linux系统中的 /dev ，/proc，/bin ，/etx 等标准的目录和文件。rootfs就是各种不同的操作系统发行版。比如Ubuntu，Centos等等。

对于一个精简的OS，rootfs可以很小，只需要包括最基本的命令、工具和程序库就可以了，因为底层直接用Host（宿主机）的kernel，自己只需要提供rootfs就行了，由此可见对于不同的Linux发行版，bootfs基本是一致的，rootfs会有差别，因此不同的发行版可以公用bootfs。

![img](G:\Typora 文档\Docker.assets\u=2584226687,1601050733&fm=26&gp=0-16423980120192.jpg)

3. **docker为什么会采用这种分层结构？**

**最大的一个好处就是共享资源**，比如有多个镜像都从相同的base镜像构建而来，那么宿主机只需要在磁盘上保存一份base镜像，同时内存中也只需要加载一份base镜像，就可以为所有容器服务。

## 5.2 镜像特点

* Docker镜像都是只读的
* 当容器启动时，一个新的可写层被加载到镜像的顶部
* 这一层常被称作容器层，容器层之下是镜像层

## 5.3 Docker镜像commit操作补充

`docker commit`提交容器副本使之成为一个新的镜像



案列展示：

1. **首先使用命令调用开启一个tomcat**

```bash
docker run -it -p 8888:8080
```

中间可能会出404的问题，所以需要将文件中的webapps删除，更改为webapps.dist

```bash
[root@dongwei ~]# docker run -d -p 8888:8080 tomcat
c8f3386daabd994af7358b31cf500b7e9e192747655202c938febd72cc45dad7
[root@dongwei ~]# docker ps
CONTAINER ID   IMAGE     COMMAND             CREATED         STATUS         PORTS                                       NAMES
c8f3386daabd   tomcat    "catalina.sh run"   6 seconds ago   Up 5 seconds   0.0.0.0:8888->8080/tcp, :::8888->8080/tcp   xenodochial_agnesi
[root@dongwei ~]# docker exec -it c8f3386daabd /bin/bash
root@c8f3386daabd:/usr/local/tomcat# ls -l
total 196
-rw-r--r--. 1 root root 18994 Dec  2 22:01 BUILDING.txt
-rw-r--r--. 1 root root  6210 Dec  2 22:01 CONTRIBUTING.md
-rw-r--r--. 1 root root 60269 Dec  2 22:01 LICENSE
-rw-r--r--. 1 root root  2333 Dec  2 22:01 NOTICE
-rw-r--r--. 1 root root  3378 Dec  2 22:01 README.md
-rw-r--r--. 1 root root  6905 Dec  2 22:01 RELEASE-NOTES
-rw-r--r--. 1 root root 16517 Dec  2 22:01 RUNNING.txt
drwxr-xr-x. 2 root root  4096 Dec 22 17:07 bin
drwxr-xr-x. 1 root root  4096 Jan 19 04:39 conf
drwxr-xr-x. 2 root root  4096 Dec 22 17:06 lib
drwxrwxrwx. 1 root root  4096 Jan 19 04:39 logs
drwxr-xr-x. 2 root root  4096 Dec 22 17:07 native-jni-lib
drwxrwxrwx. 2 root root  4096 Dec 22 17:06 temp
drwxr-xr-x. 2 root root  4096 Dec 22 17:06 webapps
drwxr-xr-x. 7 root root  4096 Dec  2 22:01 webapps.dist
drwxrwxrwx. 2 root root  4096 Dec  2 22:01 work
root@c8f3386daabd:/usr/local/tomcat# cd webapps
root@c8f3386daabd:/usr/local/tomcat/webapps# ls -l
total 0
root@c8f3386daabd:/usr/local/tomcat/webapps# cd ..
root@c8f3386daabd:/usr/local/tomcat# rm -rf webapps
root@c8f3386daabd:/usr/local/tomcat# mv webapps.dist webapps
```

最后出现的界面为
![image-20220119125356050](G:\Typora 文档\Docker.assets\image-20220119125356050-16425680374931.png)



2. **删除tomcat中的docs**

```bash
[root@dongwei ~]# docker exec -it c8f3386daabd /bin/bash
root@c8f3386daabd:/usr/local/tomcat# ls -l
total 188
-rw-r--r--. 1 root root 18994 Dec  2 22:01 BUILDING.txt
-rw-r--r--. 1 root root  6210 Dec  2 22:01 CONTRIBUTING.md
-rw-r--r--. 1 root root 60269 Dec  2 22:01 LICENSE
-rw-r--r--. 1 root root  2333 Dec  2 22:01 NOTICE
-rw-r--r--. 1 root root  3378 Dec  2 22:01 README.md
-rw-r--r--. 1 root root  6905 Dec  2 22:01 RELEASE-NOTES
-rw-r--r--. 1 root root 16517 Dec  2 22:01 RUNNING.txt
drwxr-xr-x. 2 root root  4096 Dec 22 17:07 bin
drwxr-xr-x. 1 root root  4096 Jan 19 04:39 conf
drwxr-xr-x. 2 root root  4096 Dec 22 17:06 lib
drwxrwxrwx. 1 root root  4096 Jan 19 04:41 logs
drwxr-xr-x. 2 root root  4096 Dec 22 17:07 native-jni-lib
drwxrwxrwx. 2 root root  4096 Dec 22 17:06 temp
drwxr-xr-x. 7 root root  4096 Dec  2 22:01 webapps
drwxrwxrwx. 1 root root  4096 Jan 19 04:41 work
root@c8f3386daabd:/usr/local/tomcat# cd webapps
root@c8f3386daabd:/usr/local/tomcat/webapps# ll
bash: ll: command not found
root@c8f3386daabd:/usr/local/tomcat/webapps# ls -l
total 20
drwxr-xr-x.  3 root root 4096 Dec 22 17:06 ROOT
drwxr-xr-x. 15 root root 4096 Dec 22 17:06 docs
drwxr-xr-x.  7 root root 4096 Dec 22 17:06 examples
drwxr-xr-x.  6 root root 4096 Dec 22 17:06 host-manager
drwxr-xr-x.  6 root root 4096 Dec 22 17:06 manager
root@c8f3386daabd:/usr/local/tomcat/webapps# rm -rf docs
root@c8f3386daabd:/usr/local/tomcat/webapps# ls -l
total 16
drwxr-xr-x. 3 root root 4096 Dec 22 17:06 ROOT
drwxr-xr-x. 7 root root 4096 Dec 22 17:06 examples
drwxr-xr-x. 6 root root 4096 Dec 22 17:06 host-manager
drwxr-xr-x. 6 root root 4096 Dec 22 17:06 manager
root@c8f3386daabd:/usr/local/tomcat/webapps# 
```

![image-20220119130028321](G:\Typora 文档\Docker.assets\image-20220119130028321-16425684295722.png)

3. **将这个没有docs为模板的Tomcat提交成为一个新镜像**

```bash
[root@dongwei ~]# docker commit -a="dongwei" -m="tomcat without docs" c8f3386daabd dongwei/tomcat:1.2
sha256:9a8953fc18213f9fe603313624e4977af6dd5aac78ca9a6e4d61ef4febce43dd
```

# 6. Docker容器数据卷

## 6.1 简介

docker的理念将运行的环境打包形成容器运行，运行可以伴随容器，但是我们对数据的要求是希望持久化，容器之间可以共享数据，Docker容器产生的数据，如果不通过docker commit生成新的镜像，使得数据作为容器的一部分保存下来，**那么当容器被删除之后，数据也就没了，为了能够保存数据，在docker容器中使用卷**。卷就是目录或者文件，存在于一个或者多个容器中，但是不属于联合文件系统，因此能够绕过Union File System提供一些用于持久化数据或共享数据的特点



## 6.2 作用

卷的设计目的就是数据的持久化，完全独立与容器的生命周期，因此Docker不会在容器删除时删除其挂载的数据卷。
 特点：
   1. 数据卷可以在容器之间共享和重用数据。
   2. 卷的更改可以直接生效。
   3. 数据卷的更改不会包含在镜像的更新中。
   4. 数据卷的生命周期一直持续到没有容器使用它为止。
       **容器的持久化**

       **容器间继承+共享数据**

##  6.3 数据卷

容器内添加有两种方式：

* 直接命令添加
* DockerFile添加

**直接命令添加**

```bash
docker run -it -v /宿主机绝对路径目录:/容器内目录 镜像名
```

![image-20220119143341113](G:\Typora 文档\Docker.assets\image-20220119143341113.png)



**查看是否绑定**

```bash
 docker inspect 75b810bc855c 
```



![image-20220119143615083](G:\Typora 文档\Docker.assets\image-20220119143615083-16425741761483.png)

**容器和宿主机之间可以实现文件共享**

**容器关闭后在宿主机更改文件，容器再次启动数据仍然同步**

**带权限的命令**

```bash
docker run -it -v /宿主机绝对路径目录:/容器内目录:ro 镜像名
ro：read only
```



**DockerFile添加**

* 在根目录下新建mydocker文件夹并进入

* 在DockerFile中使用VOLUME指令给镜像添加一个或者多个数据卷
   `VOLUME["/dataVolumeContainer","dataVolumeContainer2","dataVolumeContainer3"]`
    出于可移植和分享的考虑，用-v命令这种方法不能够直在DockerFile中实现，由于宿主机目录是依赖于特定宿主机的，并不能保证所有的宿主机都存在这样的特定目录

* 编写Dockerfile文件

```bash
# volume test
FROM centos
VOLUME ["/dataVolumeContainer1","/dataVolumeContainer2"]
CMD echo "finished,...........success"
CMD /bin/bash
```

* build后生成镜像-----获得新的镜像

```bash
[root@dongwei mydocker]# docker build -f /mydocker/Dockerfile -t dongwei01/centos .
Sending build context to Docker daemon  2.048kB
Step 1/4 : FROM centos
 ---> 5d0da3dc9764
Step 2/4 : VOLUME ["/dataVolumeContainer1","/dataVolumeContainer2"]
 ---> Running in 997ce0eed32d
Removing intermediate container 997ce0eed32d
 ---> f4ee56213c3a
Step 3/4 : CMD echo "finished,...........success"
 ---> Running in d514507080f2
Removing intermediate container d514507080f2
 ---> d3d1acee8c49
Step 4/4 : CMD /bin/bash
 ---> Running in e8dceee30d55
Removing intermediate container e8dceee30d55
 ---> 533493bd4fd4
Successfully built 533493bd4fd4
Successfully tagged dongwei01/centos:latest

```

* 运行新的镜像可以直接看到有两个活动卷

![image-20220119161041961](G:\Typora 文档\Docker.assets\image-20220119161041961-16425798430074.png)

* 运行之后其主机对应的挂载位置为：

![image-20220119162608946](G:\Typora 文档\Docker.assets\image-20220119162608946-16425807709125.png)



* 备注

Docker 挂载主机目录Docker出项cannot open directory .:Permission denied
 解决办法：在挂载目录后面 多加一个--privileged=true参数即可
 docker run -it -v /mydatavolume:/datavolumecontainer --privileged=true 镜像名



## 6.4 数据卷容器

1. **是什么**

命名的容器挂载数据卷，其他容器通过挂载这个（父容器）实现数据共享，挂载数据卷的容器，称之为数据卷容器。

通俗的讲就是活动硬盘挂载活动硬盘实现数据的传递依赖。

2. **总体介绍**

我们再上个步骤中已经完成了新的镜像生成两个容器~dongwei01centos

3. **具体操作**

名称分别为dc01,dc02和dc03，这三个容器的关系是dc02和dc03都继承与dc01，下面我们通过实例操作试一下。
 ⑴创建dc01的容器，命令：`docker run -it --name dc01 dongwei01/centos`

(2)创建dc02的容器，继承dc01容器
   命令：`docker run -it --name dc02 --volumes-from dc01 dongwei01/centos`

**结论**

容器之间配置信息的传递，数据卷的生命周期就一直持续到没有容器使用它为止。



# 7. DockerFile

## 7.1 Dockerfile简介

1. **Dockerfile是什么**

Dockerfile就是用来构建Docker镜像的构建文件，是由一系列命令和参数构成的脚本。

2. **Dockfile三步骤**

* 手动编写一个dockerfile文件
* 有了这个文件之后，直接docker build命令执行，获得一个自定义的镜像
* docker run

## 7.2 Dockerfile构建过程解析

### 1. dockerfile基础

* 每条保留字指令都必须为大写字母且后面要跟随至少一个参数
* 指令顺序从上到下，顺序执行
* `#`表示注释
* 每条指令都会创建一个新的镜像层，并对镜像进行提交。

### 2. dockerfile执行流程

1. docker从基础镜像运行一个容器
2. 执行一条指令并对容器作出修改
3. 执行类似docker commit的操作提交一个新的镜像层
4. docker再基于刚提交的镜像运行一个新容器
5. 执行dcokerfile中的下一条指令直到所有指令都执行完毕

### 3. 小结

从应用软件的角度来看，Dockerfile，Docker镜像以及Docker容器分别代表软件的三个不同阶段：

* Dockerfile是软件的原材料
* Docker镜像是软件的交付品
* Docker容器则可以认为是软件的运行状态

Dockerfile面向开发，Docker镜像成为交付标准，Docker容器则涉及部署与运维，三者缺一不可，三者合力充当Docker体系的基石。

![image-20220121114923676](G:\Typora 文档\Docker.assets\image-20220121114923676-16427369647001.png)



## 7.3 Dockerfile体系结构---保留字指令

`FROM`：基础镜像，当前新镜像是基于哪个镜像的

`MAINTAINER`：镜像维护者的姓名和邮箱地址

`RUN`：容器构建时需要运行的命令

`EXPOSE`：当前容器对外暴露出的端口号

`WORKDIR`：指定在创建容器后，终端默认登录进来的工作目录，一个落脚点

`ENV`：用来在构建镜像过程中设置环境变量

`ADD`：将宿主机目录下的文件拷贝进镜像且ADD命令会自动处理URL和解压tar压缩包

`COPY`：类似ADD，拷贝文件和目录到镜像中，将从构建上下文目录中<源路径>的文件/目录复制到新的一层的镜像内<目标路径>位置，

有两种写法：

* COPY src dest
* COPY["src","dest"]

`VOLUME`：容器数据卷，用于数据保存和持久化工作

`CMD`：指定一个容器启动时要运行的命令，Dockerfile中可以有多个CMD指令，但只有最后一个生效，CMD会被`docker run`之后的参数替换掉。

```
CMD 容器启动命令
CMD 指令的格式和RUN类似，也是两种格式
 1. shell格式：CMD<命令>
 2. exec格式：CMD["可执行文件","参数1","参数2".....]
参数列表格式：CMD["参数1","参数2",....]。在指定了ENTRYPOINT指令后，用CMD指定具体的参数。
```

`ENTRYPOINT`：指定一个容器启动时要运行的命令，`ENTRYPOINT`和`CMD`一样都是在指定容器启动程序以及参数。docker run之后的参数可以被当做参数传递给ENTRYPOINT。

`ONBUILD`：当构建一个被继承的Dockerfile时运行的命令，父镜像在被子继承后父镜像的onbuild被触发。

小结

![image-20220122110541969](G:\Typora 文档\Docker.assets\image-20220122110541969-16428207432711.png)

## 7.4 案例

从阿里云拉取的精简版centos出现的问题如下:

![image-20220122112238112](G:\Typora 文档\Docker.assets\image-20220122112238112-16428217601812.png)

编写Dockerfile文件如下：

```bash
FROM centos
MAINTAINER dongwei<dongwei613@gmail.com>

ENV MYPATH /usr/local
WORKDIR $MYPATH

RUN yum -y install vim
RUN yum -y install net-tools

EXPOSE 80

CMD echo $MYPATH
CMD echo "success..............ok"
CMD /bin/bash
```











































