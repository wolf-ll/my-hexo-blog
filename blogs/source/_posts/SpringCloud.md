---
title: SpringCloud
date: 2024-10-08 20:44:24
auther: 小狼
summary: 微服务理论，实践，八股
categories: 后端
img: /medias/featureimages/9.jpg
tags:
  - 框架
  - 微服务
---

# SpringCloud

## Docker

Docker 是一个开源的容器化平台，它使开发者能够自动化应用程序的部署、管理、扩展和运行。Docker 利用容器技术，允许在一个隔离的环境中运行应用程序，并确保不同环境（如开发、测试、生产环境）之间的一致性。这种技术使得应用程序可以在各种不同的操作系统和基础设施上运行，而不必担心兼容性问题。

* **镜像：**在Docker中，镜像是一个轻量级、独立的**可执行软件包**，包含运行应用程序所需的所有内容，包括代码、运行时库、环境变量和配置文件。镜像是容器的基石，容器实际上是在镜像的基础上创建的运行实例。
* **容器：**容器是镜像的**运行实例**。它包含了应用程序及其所有依赖项，以隔离应用程序及其环境，确保它在任何环境中都能一致运行。容器是可移植和可部署的，可以在任何支持Docker的系统上运行。
* **仓库：**Docker仓库是用于存储和组织镜像的地方。仓库可以是公共的（如Docker Hub）或私有的，用户可以通过仓库来分享和获取镜像。

<img src="SpringCloud\image-20241031111502848.png" alt="image-20241031111502848" style="zoom:67%;" />

Docker本身包含一个后台服务，我们可以利用Docker命令告诉Docker服务，帮助我们快速部署指定的应用。Docker服务部署应用时，首先要去搜索并下载应用对应的镜像，然后根据镜像创建并允许容器，应用就部署完成了。

ubuntu18安装docker：[Ubuntu18.04安装Docker完整教程_ubuntu 18 安装 docker-CSDN博客](https://blog.csdn.net/x7536987/article/details/124808845)

**启动docker**

```cmd
# 启动docker
sudo systemctl enable docker
sudo systemctl start docker

# 设置docker开机自启动
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
```

**建立docker用户组**

默认情况下，docker命令会使用Unix socket与Docker引擎通讯。而只有root用户和 docker 组的用户才可以访问 Docker 引擎的 Unix socket。出于安全考虑，一般 Linux 系统上不会直接使用 root 用户。因此，更好地做法是将需要使用 docker 的用户加入 docker 用户组。

```cmd
sudo groupadd docker
# 将当前用户加入docker
sudo usermod -aG docker $USER
```

### 创建容器

部署应用的步骤：
1、搜索镜像
2、拉取镜像
3、创建容器
4、操作容器中的应用

```PowerShell
docker run -d \
  --name mysql \
  -p 3306:3306 \
  -e TZ=Asia/Shanghai \
  -e MYSQL_ROOT_PASSWORD=123 \
  mysql
```

- `docker run -d` ：创建并运行一个容器，`-d`则是让容器以后台进程运行
- `--name mysql ` : 给容器起个名字叫`mysql`，你可以叫别的
- `-p 3306:3306` : 设置端口映射。
  - **容器是隔离环境**，外界不可访问。但是可以**将宿主机端口映射到容器内端口**，当访问宿主机指定端口时，就是在访问容器内的端口了。
  - 容器内端口往往是由容器内的进程决定，例如MySQL进程默认端口是3306，因此容器内端口一定是3306；而宿主机端口则可以任意指定，一般与容器内保持一致。
  - 格式： `-p 宿主机端口:容器内端口`，示例中就是将宿主机的3306映射到容器内的3306端口
- `-e TZ=Asia/Shanghai` : 配置容器内进程运行时的一些参数
  - 格式：`-e KEY=VALUE`，KEY和VALUE都由容器内进程决定
  - 案例中，`TZ=Asia/Shanghai`是设置时区；`MYSQL_ROOT_PASSWORD=123`是设置MySQL默认密码
- `mysql` : 设置**镜像**名称，Docker会根据这个名字搜索并下载镜像
  - 格式：`REPOSITORY:TAG`，例如`mysql:8.0`，其中`REPOSITORY`可以理解为镜像名，`TAG`是版本号
  - 在未指定`TAG`的情况下，默认是最新版本，也就是`mysql:latest`

镜像的名称不是随意的，而是要到DockerRegistry中寻找，镜像运行时的配置也不是随意的，要参考镜像的帮助文档，这些在DockerHub网站或者软件的官方网站中都能找到。

### 常用命令

| **命令**       | **说明**                       | **文档地址**                                                 |
| :------------- | :----------------------------- | :----------------------------------------------------------- |
| docker pull    | 拉取镜像                       | [docker pull](https://docs.docker.com/engine/reference/commandline/pull/) |
| docker push    | 推送镜像到DockerRegistry       | [docker push](https://docs.docker.com/engine/reference/commandline/push/) |
| docker images  | 查看本地镜像                   | [docker images](https://docs.docker.com/engine/reference/commandline/images/) |
| docker rmi     | 删除本地镜像                   | [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) |
| docker run     | 创建并运行容器（不能重复创建） | [docker run](https://docs.docker.com/engine/reference/commandline/run/) |
| docker stop    | 停止指定容器                   | [docker stop](https://docs.docker.com/engine/reference/commandline/stop/) |
| docker start   | 启动指定容器                   | [docker start](https://docs.docker.com/engine/reference/commandline/start/) |
| docker restart | 重新启动容器                   | [docker restart](https://docs.docker.com/engine/reference/commandline/restart/) |
| docker rm      | 删除指定容器                   | [docs.docker.com](https://docs.docker.com/engine/reference/commandline/rm/) |
| docker ps      | 查看容器                       | [docker ps](https://docs.docker.com/engine/reference/commandline/ps/) |
| docker logs    | 查看容器运行日志               | [docker logs](https://docs.docker.com/engine/reference/commandline/logs/) |
| docker exec    | 进入容器                       | [docker exec](https://docs.docker.com/engine/reference/commandline/exec/) |
| docker save    | 保存镜像到本地压缩文件         | [docker save](https://docs.docker.com/engine/reference/commandline/save/) |
| docker load    | 加载本地压缩文件到镜像         | [docker load](https://docs.docker.com/engine/reference/commandline/load/) |
| docker inspect | 查看容器详细信息               | [docker inspect](https://docs.docker.com/engine/reference/commandline/inspect/) |

<img src="SpringCloud\image-20241031111225467.png" alt="image-20241031111225467" style="zoom: 67%;" />

### 容器迁移

当我们运行一个容器的时候（如果不使用卷的话），我们做的任何文件修改都会被记录于容器存储层里。而 Docker 提供了一个 docker commit 命令，可以将容器的存储层保存下来成为镜像。换句话说，就是在原有镜像的基础上，再叠加上容器的存储层，并构成新的镜像。以后我们运行这个新镜像的时候，就会拥有原有容器最后的文件变化。

docker commit 的语法格式为：

```cmd
docker commit [options] <container_ID或container_name> [<new_image_name>[:<label>]]
```

我们可以用下面的命令将容器保存为镜像：

```cmd
docker commit \
    --author "XX" \
    --message "XX" \
    container_name \
    new_image_name:label
```

其中 --author 是指定修改的作者，而 --message 则是记录本次修改的内容。这点和 git 版本控制相似，不过这里这些信息可以省略留空。我们可以在 docker image ls 中看到这个新定制的镜像。我们还可以用 docker history image_name[:label] 具体查看镜像内的历史记录。

### 数据卷

docker的问题：docker容器删除后，在容器中产生的数据也会随之销毁；docker容器和外部机器不能直接交换文件（只能与宿主机进行）；容器之间不能进行数据交互。

容器提供程序的运行环境，但是**程序运行产生的数据、程序运行依赖的配置都应该与容器解耦**。

**数据卷（volume）**是一个虚拟目录，是**容器内目录**与**宿主机目录**之间映射的桥梁。 

数据卷的作用：
1、容器数据持久化
2、外部机器与容器间接通信
3、容器之间数据交换

<img src="SpringCloud\image-20241031143605424.png" alt="image-20241031143605424" style="zoom:67%;" />

在上图中：

- 我们创建了两个数据卷：`conf`、`html`
- Nginx容器内部的`conf`目录和`html`目录分别与两个数据卷关联。
- 而数据卷conf和html分别指向了宿主机的`/var/lib/docker/volumes/conf/_data`目录和`/var/lib/docker/volumes/html/_data`目录

这样以来，容器内的`conf`和`html`目录就 与宿主机的`conf`和`html`目录关联起来，我们称为**挂载**。此时，我们操作宿主机的`/var/lib/docker/volumes/html/_data`就是在操作容器内的`/usr/share/nginx/html/_data`目录。只要我们将静态资源放入宿主机对应目录，就可以被Nginx代理了。

> `/var/lib/docker/volumes`这个目录就是默认的存放所有容器数据卷的目录，其下再根据数据卷名称创建新目录，格式为`/数据卷名/_data`。
>
> **为什么不让容器目录直接指向宿主机目录呢**？
>
> - 因为直接指向宿主机目录就与宿主机强耦合了，如果切换了环境，宿主机目录就可能发生改变了。由于容器一旦创建，目录挂载就无法修改，这样容器就无法正常工作了。
> - 但是容器指向数据卷，一个逻辑名称，而数据卷再指向宿主机目录，就不存在强耦合。如果宿主机目录发生改变，只要改变数据卷与宿主机目录之间的映射关系即可。
>
> 不过，我们通过由于数据卷目录比较深，不好寻找，通常我们也**允许让容器直接与宿主机目录挂载而不使用数据卷**

数据卷的相关命令有：

| **命令**              | **说明**             | **文档地址**                                                 |
| :-------------------- | :------------------- | :----------------------------------------------------------- |
| docker volume create  | 创建数据卷           | [docker volume create](https://docs.docker.com/engine/reference/commandline/volume_create/) |
| docker volume ls      | 查看所有数据卷       | [docs.docker.com](https://docs.docker.com/engine/reference/commandline/volume_ls/) |
| docker volume rm      | 删除指定数据卷       | [docs.docker.com](https://docs.docker.com/engine/reference/commandline/volume_prune/) |
| docker volume inspect | 查看某个数据卷的详情 | [docs.docker.com](https://docs.docker.com/engine/reference/commandline/volume_inspect/) |
| docker volume prune   | 清除数据卷           | [docker volume prune](https://docs.docker.com/engine/reference/commandline/volume_prune/) |

注意：容器与数据卷的挂载要在创建容器时配置，对于创建好的容器，是不能设置数据卷的。而且**创建容器的过程中，数据卷会自动创建**。

#### nginx的html目录挂载

```cmd
# 1.首先创建容器并指定数据卷，注意通过 -v 参数来指定数据卷
docker run -d --name nginx -p 80:80 -v html:/usr/share/nginx/html nginx

# 2.然后查看数据卷
docker volume ls
# 结果
DRIVER    VOLUME NAME
local     44c23cc5778160c799ff3cc24c64f456e65c0442058cbcbf78a277ab8ff73623
local     html

# 3.查看数据卷详情
docker volume inspect html
# 结果
[
    {
        "CreatedAt": "2024-10-31T14:48:10+08:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/html/_data",
        "Name": "html",
        "Options": null,
        "Scope": "local"
    }
]

# 4.查看/var/lib/docker/volumes/html/_data目录
ll /var/lib/docker/volumes/html/_data
# 结果
total 8.0K
-rw-r--r-- 1 root root 497 10月  2 23:13 50x.html
-rw-r--r-- 1 root root 615 10月  2 23:13 index.html

```

#### 匿名卷

```cmd
# 1.查看MySQL容器详细信息
docker inspect mysql
# 关注其中.Config.Volumes部分和.Mounts部分
        "Config": {
            "Volumes": {
                "/var/lib/mysql": {}	# 容器声明了一个本地目录，需要挂载数据卷，但是数据卷未定义。这就是匿名卷
            },
            .......
            
"Mounts": [
            {
                "Type": "volume",
                "Name": "44c23cc5778160c799ff3cc24c64f456e65c0442058cbcbf78a277ab8ff73623",
                "Source": "/var/lib/docker/volumes/44c23cc5778160c799ff3cc24c64f456e65c0442058cbcbf78a277ab8ff73623/_data",
                "Destination": "/var/lib/mysql",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            }
        ],

```

Mounts中的关键属性：

- Name：数据卷名称。由于定义容器未设置容器名，这里的就是匿名卷自动生成的名字，一串hash值。
- Source：宿主机目录
- Destination : 容器内的目录

上述配置是将容器内的`/var/lib/mysql`这个目录，与数据卷`44c23cc5778160c799ff3cc24c64f456e65c0442058cbcbf78a277ab8ff73623`挂载。于是在宿主机中就有了`/var/lib/docker/volumes/44c23cc5778160c799ff3cc24c64f456e65c0442058cbcbf78a277ab8ff73623/_data`这个目录。这就是匿名数据卷对应的目录，其使用方式与普通数据卷没有差别。

#### 挂载本地目录

可以发现，数据卷的目录结构较深，如果我们去操作数据卷目录会不太方便。在很多情况下，我们会直接将容器目录与宿主机指定目录挂载。挂载语法与数据卷类似：

```cmd
# 挂载本地目录
-v 本地目录:容器内目录
# 挂载本地文件
-v 本地文件:容器内文件
```

**注意**：本地目录或文件必须以 `/` 或 `./`开头，如果直接以名字开头，会被识别为数据卷名而非本地目录名。

```cmd
 # 删除原来的MySQL容器
docker rm -f mysql

# 创建并运行新mysql容器，挂载本地目录
docker run -d \
--name mysql \
-p 3306:3306 \
-e TZ=Asia/Shanghai \
-e MYSQL_ROOT_PASSWORD=123 \
-v /home/yf/mysql/data:/var/lib/mysql \
-v /home/yf/mysql/conf:/etc/mysql/conf.d \
-v /home/yf/mysql/init:/docker-entrypoint-initdb.d \
--privileged=true  mysql
 
 # 守护者模式进入容器，操作MySQL
 docker exec -it mysql mysql -uroot -p123
```

### 镜像

镜像之所以能让我们快速跨操作系统部署应用而忽略其运行环境、配置，就是因为**镜像中包含了程序运行需要的系统函数库、环境、配置、依赖。**因此，自定义镜像本质就是依次准备好程序运行的基础环境、依赖、应用本身、运行配置等文件，并且打包而成。

镜像文件不是随意堆放的，而是按照操作的步骤分层叠加而成，每一层形成的文件都会单独打包并标记一个唯一id，称为**Layer**（**层**）。这样，如果我们构建时用到的某些层其他人已经制作过，就可以直接拷贝使用这些层，而不用重复制作。

#### Dockerfile

由于制作镜像的过程中，需要逐层处理和打包，比较复杂，所以Docker就提供了自动打包镜像的功能。我们只需要将打包的过程，每一层要做的事情用固定的语法写下来，交给Docker去执行即可。

而这种记录镜像结构的文件就称为**Dockerfile**，其对应的语法可以参考官方文档：https://docs.docker.com/engine/reference/builder/

| **指令**       | **说明**                                     |
| :------------- | :------------------------------------------- |
| **FROM**       | 指定基础镜像                                 |
| **ENV**        | 设置环境变量，可在后面指令使用               |
| **COPY**       | 拷贝本地文件到镜像的指定目录                 |
| **RUN**        | 执行Linux的shell命令，一般是安装过程的命令   |
| **EXPOSE**     | 指定容器运行时监听的端口，是给镜像使用者看的 |
| **ENTRYPOINT** | 镜像中应用的启动命令，容器运行时调用         |

```dockerfile
# 在jdk环境基础上制作java镜像
# 基础镜像
FROM openjdk:11.0-jre-buster
# 设定时区
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
# 拷贝jar包
COPY docker-demo.jar /app.jar
# 入口
ENTRYPOINT ["java", "-jar", "/app.jar"]
```

当Dockerfile文件写好以后，就可以利用命令来构建镜像了。

首先，将课前资料提供的`docker-demo.jar`包以及`Dockerfile`拷贝到虚拟机的`/root/demo`目录：

<img src="SpringCloud\image-20241102184713448.png" alt="image-20241102184713448" style="zoom:67%;" />

然后，执行命令，构建镜像：

```Bash
# 进入镜像目录
cd /root/demo
# 开始构建
docker build -t docker-demo:1.0 .
```

命令说明：

- `docker build `: 就是构建一个docker镜像
- `-t docker-demo:1.0` ：`-t`参数是指定镜像的名称（`repository`和`tag`）
- `.` : 最后的点是指构建时Dockerfile所在路径，由于我们进入了demo目录，所以指定的是`.`代表当前目录，也可以直接指定Dockerfile目录：
```Bash
    # 直接指定Dockerfile目录
    docker build -t docker-demo:1.0 /root/demo
```

## Nginx

### 概念

* Nginx (engine x) 是一个**高性能的HTTP和反向代理web服务器**，同时也提供了IMAP/POP3/SMTP服务。Nginx是由伊戈尔·赛索耶夫为俄罗斯访问量第二的Rambler.ru站点（俄文：Рамблер）开发的，第一个公开版本0.1.0发布于2004年10月4日。2011年6月1日，nginx 1.0.4发布。
* 其特点是占有内存少，并发能力强，事实上nginx的并发能力在同类型的网页服务器中表现较好，中国大陆使用nginx网站用户有：百度、京东、新浪、网易、腾讯、淘宝等。在全球活跃的网站中有12.18%的使用比率，大约为2220万个网站。
* Nginx 是一个安装非常的简单、配置文件非常简洁（还能够支持perl语法）、Bug非常少的服务。Nginx 启动特别容易，并且几乎可以做到7*24不间断运行，即使运行数个月也不需要重新启动。你还能够不间断服务的情况下进行软件版本的升级。
* Nginx代码完全用C语言从头写成。官方数据测试表明能够支持高达 50,000 个并发连接数的响应。

### 作用

**正向代理**： 平时需要访问国外的浏览器很慢，比如我们要看推特，看GitHub等等。我们直接用国内的服务器无法访问国外的服务器，或者是访问很慢。所以我们需要在本地搭建一个服务器来帮助我们去访问。这种就是正向代理。（**浏览器中配置代理服务器**）

<img src="https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2021/01/25/kuangstudy46bdad36-d3e0-43b0-a223-43360b7e8fc7.png" alt="img" style="zoom:67%;" />

**反向代理**： 我们访问淘宝的时候，淘宝内部肯定不是只有一台服务器，它的内部有很多台服务器，那我们进行访问的时候，因为服务器中间session不共享，那我们是不是在服务器之间访问需要频繁登录，这个时候淘宝搭建一个过渡服务器，对我们是没有任何影响的，我们是登录一次，但是访问所有，这种情况就是反向代理。对我们来说，**客户端对代理是无感知的，客户端不需要任何配置就可以访问，我们只需要把请求发送给反向代理服务器，由反向代理服务器去选择目标服务器获取数据后，再返回给客户端**，此时反向代理服务器和目标服务器对外就是一个服务器，暴露的是代理服务器地址，隐藏了真实服务器的地址。（**在服务器中配置代理服务器**）

<img src="https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2021/01/25/kuangstudy62a15097-6e2a-4dbe-bcf5-f0d7cab81089.png" alt="img" style="zoom:67%;" />

### 负载均衡

负载均衡建立在现有网络结构之上，它提供了一种廉价有效透明的方法扩展网络设备和服务器的带宽、增加吞吐量、加强网络数据处理能力、提高网络的灵活性和可用性。
负载均衡（Load Balance）其意思就是分**摊到多个操作单元上进行执行**，例如Web服务器、FTP服务器、企业关键应用服务器和其它关键任务服务器等，从而共同完成工作任务。
Nginx给出来三种关于负载均衡的方式。

#### 轮询法（默认方法）

每个请求按时间顺序逐一分配到不同的后端服务器，如果后端服务器down掉，能自动剔除。
适合服务器配置相当，无状态且短平快的服务使用。也适用于图片服务器集群和纯静态页面服务器集群。

#### weight权重模式（加权轮询）

指定**轮询几率**，weight和访问比率成正比，用于后端服务器性能不均的情况。
这种方式比较灵活，当后端服务器性能存在差异的时候，通过配置权重，可以让服务器的性能得到充分发挥，有效利用资源。weight和访问比率成正比，用于后端服务器性能不均的情况。权重越高，在被访问的概率越大

#### ip_hash

上述方式存在一个问题就是说，在负载均衡系统中，假如用户在某台服务器上登录了，那么该用户第二次请求的时候，因为我们是负载均衡系统，每次请求都会重新定位到服务器集群中的某一个，那么已经登录某一个服务器的用户再重新定位到另一个服务器，其登录信息将会丢失，这样显然是不妥的。
我们可以采用ip_hash指令解决这个问题，如果客户已经访问了某个服务器，当用户再次访问时，会将该请求**通过哈希算法，自动定位到该服务器。**每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器，可以解决session的问题。

### 动静分离

在我们的软件开发中，有些请求是需要后台处理的，有些请求是不需要经过后台处理的（如：css、html、jpg、js等等文件），这些不需要经过后台处理的文件称为静态文件。**让动态网站里的动态网页根据一定规则把不变的资源和经常变的资源区分开来**，动静资源做好了拆分以后，我们就可以根据静态资源的特点将其做缓存操作。提高资源响应的速度。

Nginx的静态处理能力很强，但是动态处理能力不足，因此，在企业中常用动静分离技术。动静分离技术其实是采用代理的方式，在server{}段中加入带正则匹配的location来指定匹配项针对PHP的动静分离：**静态页面交给Nginx处理，动态页面交给PHP-FPM模块或Apache处理。**在Nginx的配置中，是通过location配置段配合正则匹配实现静态与动态页面的不同处理方式。

### 常用命令

```cmd
cd /usr/local/nginx/sbin/
./nginx  启动
./nginx -s stop  停止
./nginx -s quit  安全退出
./nginx -s reload  重新加载配置文件  如果我们修改了配置文件，就需要重新加载。
ps aux|grep nginx  查看nginx进程

==防火墙相关==
# 开启
service firewalld start
# 重启
service firewalld restart
# 关闭
service firewalld stop
# 查看防火墙规则
firewall-cmd --list-all
# 查询端口是否开放
firewall-cmd --query-port=8080/tcp
# 开放80端口
firewall-cmd --permanent --add-port=80/tcp
# 移除端口
firewall-cmd --permanent --remove-port=8080/tcp
#重启防火墙(修改配置后要重启防火墙)
firewall-cmd --reload
# 参数解释
1、firwall-cmd：是Linux提供的操作firewall的一个工具；
2、--permanent：表示设置为持久；
3、--add-port：标识添加的端口；
```

## 微服务

### 概念

**单体架构（monolithic structure）**：顾名思义，整个项目中所有功能模块都在一个工程中开发；项目部署时需要对所有模块一起编译、打包；项目的架构设计、开发模式都非常简单。（优点：架构简单、部署成本低）

当项目规模较小时，这种模式上手快，部署、运维也都很方便，因此早期很多小型项目都采用这种模式。但随着项目的业务规模越来越大，团队开发人员也不断增加，单体架构就呈现出越来越多的问题：

- **团队协作成本高**：试想一下，你们团队数十个人同时协作开发同一个项目，由于所有模块都在一个项目中，不同模块的代码之间物理边界越来越模糊。最终要把功能合并到一个分支，你绝对会陷入到解决冲突的泥潭之中。
- **系统发布效率低**：任何模块变更都需要发布整个系统，而系统发布过程中需要多个模块之间制约较多，需要对比各种文件，任何一处出现问题都会导致发布失败，往往一次发布需要数十分钟甚至数小时。
- **系统可用性差**：单体架构各个功能模块是作为一个服务部署，相互之间会互相影响，一些热点功能会耗尽系统资源，导致其它服务低可用。

微服务架构，首先是服务化，就是将单体架构中的功能模块从单体应用中拆分出来，独立部署为多个服务。（优点：降低服务的耦合度，有利于服务的升级拓展）同时要满足下面的一些特点：

- **单一职责**：一个微服务负责一部分业务功能，并且其核心数据不依赖于其它模块。
- **团队自治**：每个微服务都有自己独立的开发、测试、发布、运维人员，团队人员规模不超过10人（2张披萨能喂饱）
- **服务自治**：每个微服务都独立打包部署，访问自己独立的数据库。并且要做好服务隔离，避免对其它服务产生影响

微服务的特征：

1、单一职责：拆分粒度更小，每一个服务都对应唯一的业务能力，做到单一职责
2、面向服务：微服务对外暴露业务接口
3、自治：团队独立、技术独立、数据独立、部署独立
4、隔离性强：服务调用做好隔离、容错、降级，避免出现级联问题

**缺点**：不同的功能都做成了服务集群，方法之间不再那么方便互相调用了，因此我们需要进行服务的治理。

微服务架构解决了单体架构存在的问题，特别适合大型互联网项目的开发，因此被各大互联网公司普遍采用。大家以前可能听说过分布式架构，分布式就是服务拆分的过程，其实微服务架构正是分布式架构的一种最佳实践的方案。

### 技术对比

<img src="SpringCloud\2310d1c901b6a9822ecdad5f634da19a.png" alt="img" style="zoom:67%;" />

* **注册中心**： 维护微服务中每个结点的信息，并且监控结点的状态。
* **配置中心**： 它可以统一去管理整个服务群体成千上百的这些配置。如果以后有些配置需要变更，只需要去找到配置中心便可。它可以去通知相关的微服务，实现配置的“热更新”。
* **服务网关**： 由网关将用户请求路由到微服务群，在路由过程中可以做负载均衡。
  路由或者服务之间调用时做好服务的容错处理，避免因服务故障带来的级联失败。还要做好服务保护，隔离等措施。

### SpringCloud

微服务拆分以后碰到的各种问题都有对应的解决方案和微服务组件，而SpringCloud框架可以说是目前Java领域最全面的微服务组件的集合了。

- SpringCloud是目前国内使用最广泛的微服务框架。[官网地址](https://spring.io/projects/spring-cloud)
- SpringCloud集成了各种微服务功能组件，并基于SpringBoot实现了这些组件的自动装配，从而提供了良好的开箱即用体验:

| **SpringCloud版本**                                          | **SpringBoot版本**                    |
| :----------------------------------------------------------- | :------------------------------------ |
| [2022.0.x](https://github.com/spring-cloud/spring-cloud-release/wiki/Spring-Cloud-2022.0-Release-Notes) aka Kilburn | 3.0.x                                 |
| [2021.0.x](https://github.com/spring-cloud/spring-cloud-release/wiki/Spring-Cloud-2021.0-Release-Notes) aka Jubilee | 2.6.x, 2.7.x (Starting with 2021.0.3) |
| [2020.0.x](https://github.com/spring-cloud/spring-cloud-release/wiki/Spring-Cloud-2020.0-Release-Notes) aka Ilford | 2.4.x, 2.5.x (Starting with 2020.0.3) |
| [Hoxton](https://github.com/spring-cloud/spring-cloud-release/wiki/Spring-Cloud-Hoxton-Release-Notes) | 2.2.x, 2.3.x (Starting with SR5)      |
| [Greenwich](https://github.com/spring-projects/spring-cloud/wiki/Spring-Cloud-Greenwich-Release-Notes) | 2.1.x                                 |
| [Finchley](https://github.com/spring-projects/spring-cloud/wiki/Spring-Cloud-Finchley-Release-Notes) | 2.0.x                                 |
| [Edgware](https://github.com/spring-projects/spring-cloud/wiki/Spring-Cloud-Edgware-Release-Notes) | 1.5.x                                 |
| [Dalston](https://github.com/spring-projects/spring-cloud/wiki/Spring-Cloud-Dalston-Release-Notes) | 1.5.x                                 |

### 服务拆分

目标：

> 1、单一职责：不同微服务，不要重复开发相同业务，要保证微服务内部业务的完整性
> 2、数据独立：不要访问其它微服务的数据，否则会导致数据耦合
> 3、面向服务：将自己的业务暴露为接口，尽量保证接口外观不变，供其它微服务调用

方式：

* **纵向拆分**：按照项目的功能模块来拆分一个个服务。尽可能提高服务的内聚性。
* **横向拆分**：各个功能模块之间有没有公共的业务部分，如果有将其抽取出来作为通用服务。

一般微服务项目有两种不同的**工程结构**：

- **完全解耦**：每一个微服务都创建为一个独立的工程，甚至可以使用不同的开发语言来开发，项目完全解耦。
  - 优点：服务之间耦合度低
  - 缺点：每个项目都有自己的独立仓库，管理起来比较麻烦
- **Maven聚合**：整个项目为一个Project，然后**每个微服务是其中的一个Module**
  - 优点：项目代码集中，管理和运维方便（授课也方便）
  - 缺点：服务之间耦合，编译时间较长

#### 示例

<img src="SpringCloud\image-20241104102557229.png" alt="image-20241104102557229" style="zoom:67%;" />

* 将原始单体架构中的依赖项、配置信息、属于该服务的controller，service，mapper等复制到当前module
* 根据当前业务需要删除或新增（修改）依赖项/配置项
* 注意修改代码中引入的包的路径，包括sql语句引用的mapper路径
* **特别注意：不同业务间有数据或服务调用的，不能再通过依赖注入的方式调用，需要做远程服务调用（见下方子标题）**

### 服务调用

<img src="SpringCloud\image-20241104103234051.png" alt="image-20241104103234051" style="zoom:67%;" />

就是购物车业务中需要查询商品信息，但商品信息查询的逻辑全部迁移到了`item-service`服务，导致我们无法查询。

最终结果就是查询到的购物车数据不完整，因此要想解决这个问题，我们就必须改造其中的代码，**把原本本地方法调用，改造成跨微服务的远程调用**（RPC，即**R**emote **P**roduce **C**all）。

回顾一下http请求原理，简单来说无非就是浏览器发出http请求信息，而服务中利用@GetMapping去接受请求，查询数据库后返还相应的信息给前端。因此，应该思考如何使得java代码发出http请求。

#### RestTemplate

Spring提供了一个工具叫做**RestTemplate**，专门用于在java代码中发起http请求。

* spring框架提供的RestTemplate类可用于在应用中调用rest服务，它简化了与http服务的通信方式，统一了RESTful的标准，封装了http链接，我们只需要传入url及返回值类型即可。相较于之前常用的HttpClient，RestTemplate是一种更优雅的调用RESTful服务的方式。
* RestTemplate默认依赖JDK提供http连接的能力（HttpURLConnection），如果有需要的话也可以通过setRequestFactory方法替换为例如Apache HttpComponents、Netty或OkHttp等其它HTTP library。
* 其实spring并没有真正的去实现底层的http请求（3次握手），而是集成了别的http请求，spring只是在原有的各种http请求进行了规范标准，让开发者更加简单易用，底层默认用的是jdk的http请求。

优点：连接池、超时时间设置、支持异步、请求和响应的编解码

缺点：依赖别的spring版块、参数传递不灵活

**使用--主要方法**

```java
   delete() 在特定的URL上对资源执行HTTP DELETE操作
   
   exchange() 在URL上执行特定的HTTP方法，返回包含对象的ResponseEntity，这个对象是从响应体中 映射得到的
   
   execute() 在URL上执行特定的HTTP方法，返回一个从响应体映射得到的对象
   
   getForEntity() 发送一个HTTP GET请求，返回的ResponseEntity包含了响应体所映射成的对象
   
   getForObject() 发送一个HTTP GET请求，返回的请求体将映射为一个对象
   
   postForEntity() POST 数据到一个URL，返回包含一个对象的ResponseEntity，这个对象是从响应体中映射得
   到的
   
   postForObject() POST 数据到一个URL，返回根据响应体匹配形成的对象
   
   headForHeaders() 发送HTTP HEAD请求，返回包含特定资源URL的HTTP头
   
   optionsForAllow() 发送HTTP OPTIONS请求，返回对特定URL的Allow头信息
   
   postForLocation() POST 数据到一个URL，返回新创建资源的URL
   
   put() PUT 资源到特定的URL
```

**1.将RestTemplate注入到spring容器中。**

```java
package com.example.demo.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.http.client.ClientHttpRequestFactory;
import org.springframework.http.client.SimpleClientHttpRequestFactory;
import org.springframework.web.client.RestTemplate;

/**
 * RestTemplate配置类
 */
@Configuration
public class RestTemplateConfig {

    @Bean
    public RestTemplate restTemplate(ClientHttpRequestFactory factory){
        return new RestTemplate(factory);
    }

    @Bean
    public ClientHttpRequestFactory simpleClientHttpRequestFactory(){
        SimpleClientHttpRequestFactory factory = new SimpleClientHttpRequestFactory();
        factory.setReadTimeout(5000);//单位为ms
        factory.setConnectTimeout(5000);//单位为ms
        return factory;
    }
}
```

**2.远程调用**

```java
private void handleCartItems(List<CartVO> vos) {
    // TODO 1.获取商品id
    Set<Long> itemIds = vos.stream().map(CartVO::getItemId).collect(Collectors.toSet());
    // 2.查询商品
    // List<ItemDTO> items = itemService.queryItemByIds(itemIds);
    // 2.1.利用RestTemplate发起http请求，得到http的响应
    ResponseEntity<List<ItemDTO>> response = restTemplate.exchange(
            "http://localhost:8081/items?ids={ids}",	// 请求路径
            HttpMethod.GET,		// 请求方式
            null,		// 请求实体
            new ParameterizedTypeReference<List<ItemDTO>>() {
            },	// 返回值类型
            Map.of("ids", CollUtil.join(itemIds, ","))	// 请求参数
    );
    // 2.2.解析响应
    if(!response.getStatusCode().is2xxSuccessful()){
        // 查询失败，直接结束
        return;
    }
    List<ItemDTO> items = response.getBody();
    if (CollUtils.isEmpty(items)) {
        return;
    }
    // 3.转为 id 到 item的map
    Map<Long, ItemDTO> itemMap = items.stream().collect(Collectors.toMap(ItemDTO::getId, Function.identity()));
    // 4.写入vo
    for (CartVO v : vos) {
        ItemDTO item = itemMap.get(v.getItemId());
        if (item == null) {
            continue;
        }
        v.setNewPrice(item.getPrice());
        v.setStatus(item.getStatus());
        v.setStock(item.getStock());
    }
}
```

在这个过程中，`item-service`提供了查询接口，`cart-service`利用Http请求调用该接口。因此`item-service`可以称为服务的提供者，而`cart-service`则称为服务的消费者或服务调用者。

### 服务注册和发现

上一节内容中服务调用存在问题，服务调用使用http请求，网址直接定死了，**如果我们有多个服务集群（多实例部署），亦或是网址在后续开发过程中出现变更，就会产生不方便。**

> 服务提供者：一次业务中，被其它微服务调用的服务（提供接口给其他微服务）
> 服务消费者：一次业务中，调用其它微服务的服务（调用其它微服务提供的接口）
>
> 提供者与消费者的概念是相对的，**一个服务既可以是提供者也可以是消费者**。
>
> 服务消费者该如何获取服务提供者的地址信息？
> 如果有多个服务提供者，消费者该如何选择？
> 消费者如何得知服务提供者的健康状态？
> 新增的服务提供者如何被消费者感知？

<img src="SpringCloud\image-20241104113255813.png" alt="image-20241104113255813" style="zoom: 50%;" />

#### 注册中心

在大型微服务项目中，服务提供者的数量会非常多，为了管理这些服务就引入了**注册中心**的概念。注册中心、服务提供者、服务消费者三者间关系如下：

<img src="SpringCloud\image-20241104113924738.png" alt="image-20241104113924738" style="zoom: 50%;" />

流程如下：

- 服务启动时就会**注册自己的服务信息**（服务名、IP、端口）到注册中心
- 调用者可以从注册中心**订阅**想要的服务，获取服务对应的实例列表（1个服务可能多实例部署）
- 调用者自己对实例列表**负载均衡**，挑选一个实例
- 调用者向该实例发起**远程调用**

当服务提供者的实例宕机或者启动新实例时，调用者如何得知呢？

- 服务提供者会**定期向注册中心发送请求，报告自己的健康状态**（心跳请求）
- 当注册中心长时间收不到提供者的心跳时，会认为该实例宕机，将其从服务的实例列表中**剔除**
- 当服务有新实例启动时，会**发送注册服务请求**，其信息会被记录在注册中心的服务实例列表
- 当注册中心**服务列表变更时，会主动通知微服务**，更新本地服务列表
  - 微服务通常会维护一个**本地缓存的服务列表**，用于优化服务调用的性能（避免每次调用时都从注册中心获取）。当注册中心通知微服务服务列表变更时，微服务会根据收到的增量更新，及时刷新它们本地缓存的服务列表。这样，在下一次进行跨服务调用时，微服务可以确保使用的是最新的服务实例信息。

#### Nacos注册中心

目前开源的注册中心框架有很多，国内比较常见的有：

- Eureka：Netflix公司出品，目前被集成在SpringCloud当中，一般用于Java应用
- Nacos：Alibaba公司出品，目前被集成在SpringCloudAlibaba中，一般用于Java应用
- Consul：HashiCorp公司出品，目前集成在SpringCloud中，不限制微服务语言

以上几种注册中心都遵循SpringCloud中的API规范，因此在业务开发使用上没有太大差异。由于Nacos是国内产品，中文文档比较丰富，而且同时具备**配置管理**功能（后面会学习），因此在国内使用较多。

**Nacos部署**：

[‌﻿⁠﻿﻿‬‌⁠‬﻿﻿‍﻿﻿﻿⁠‌‍‌‬‬﻿⁠﻿‌⁠day03-微服务01 - 飞书云文档](https://b11et3un53m.feishu.cn/wiki/R4Sdwvo8Si4kilkSKfscgQX0niB)

[Linux 通过 Docker 部署 Nacos 2.2.3 服务发现与配置中心_nacos2.2.3虚拟机安装详情-CSDN博客](https://blog.csdn.net/u013737132/article/details/132592040)

<img src="SpringCloud\image-20241104161640633.png" alt="image-20241104161640633" style="zoom:67%;" />

#### 服务注册

将特定service注册到nacos：1.引入依赖；2.配置Nacos地址；3.重启实例

`pom.xml`中添加依赖：

```xml
<!--nacos 服务注册发现-->
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>
```

`application.yml`中添加nacos地址配置：

```yaml
spring:
  application:
    name: item-service # 服务名称
  cloud:
    nacos:
      server-addr: 192.168.150.101:8848 # nacos地址
```

#### 服务发现

服务的消费者要去nacos订阅服务，这个过程就是服务发现，步骤如下：1.引入依赖；2.配置Nacos地址；3.发现并调用服务

* 服务发现除了要引入nacos依赖（和服务注册的依赖一样）以外，由于还需要**负载均衡**，因此要引入SpringCloud提供的LoadBalancer依赖。
* 配置nacos地址--和服务注册的配置一样。因为任何一个微服务都可以调用别人，也可以被别人调用，即可以是调用者，也可以是提供者。所以依赖和配置都是一样的。
* 服务调用者必须利用负载均衡的算法，从多个实例中挑选一个去访问。常见的负载均衡算法有：随机，轮询，IP的hash，最近最少访问等。服务发现需要用到一个工具，DiscoveryClient，SpringCloud已经帮我们自动装配，我们可以直接注入使用：

<img src="SpringCloud\image-20241104174127926.png" alt="image-20241104174127926" style="zoom:67%;" />

### OpenFeign

RestTemplate的问题：

> 1、代码可读性差，方法调用不统一（一会远程调用，一会本地接口调用）
> 2、url参数比较复杂，难以维护

因此，我们必须想办法改变远程调用的开发模式，让**远程调用像本地方法调用一样简单**。而这就要用到OpenFeign组件了。

Feign是一个声明式的http客户端，我们只需要把发http请求所需要的信息声明出来即可，剩下的东西都交给Feign来实现。

1.引入feign依赖和负载均衡器：

```xml
  <!--openFeign-->
  <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-openfeign</artifactId>
  </dependency>
  <!--负载均衡器-->
  <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-loadbalancer</artifactId>
  </dependency>
```

2.在微服务的Application启动类添加开启Feign的功能：加注解@EnableFeignClients

3.编写feign客户端

```java
@FeignClient("userservice") //声明出服务名称
public interface UserClient {

    @GetMapping("/user/{id}")
    User findById(@PathVariable("id") Long id);
}
```

主要是基于SpringMVC的注解来声明远程调用的信息

- `@FeignClient("item-service")` ：声明服务名称
- `@GetMapping` ：声明请求方式
- `@GetMapping("/items")` ：声明请求路径
- `@RequestParam("ids") Collection<Long> ids` ：声明请求参数
- `List<ItemDTO>` ：返回值类型

4.使用feign客户端

```java
    @Resource
    private OrderMapper orderMapper;

    @Resource
    private UserClient userClient;

    public Order queryOrderById(Long orderId){
        //查询订单
        Order order = orderMapper.findById(orderId);
        //根据用户id来查询用户，用Feign实现远程调用
        User user = userClient.findById(order.getUserId());
        //将用户注入到order中
        order.setUser(user);
        // 4.返回
        return order;
    }
```

## 网关

网关的作用：**身份认证和权限校验**：通过才能去访问微服务；**服务路由**：当通过网关后，还需要根据请求的类型，将请求转发到对应的微服务中；**负载均衡**：确定了转发的微服务之后，微服务中的多个实例之间还应该做负载均衡；**请求限流**：限制访问的请求量

- 统一配置管理，解决微服务的配置文件重复和配置热更新问题。

在SpringCloud当中，提供了两种网关实现方案：

- Netflix Zuul：早期实现，目前已经淘汰
- SpringCloudGateway：基于Spring的WebFlux技术，完全支持响应式编程，吞吐能力更强

### 网关路由

> 网关路由，解决前端请求不同数据时要访问不同的入口，需要维护多个入口地址的问题。

1.创建网关微服务

2.引入SpringCloudGateway、NacosDiscovery依赖

```xml
<!--网关-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-gateway</artifactId>
</dependency>
<!--nacos discovery-->
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>
<!--负载均衡-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-loadbalancer</artifactId>
</dependency>
```

3.编写启动类

4.编写网关的路由配置及Nacos地址（application.yaml）

```yaml
server:
  port: 8080
spring:
  application:
    name: gateway
  cloud:
    nacos:
      server-addr: 192.168.150.101:8848
    gateway:
      routes:
        - id: item # 路由规则id，自定义，唯一
          uri: lb://item-service # 路由的目标服务，lb代表负载均衡，会从注册中心拉取服务列表
          predicates: # 路由断言，判断当前请求是否符合当前规则，符合则路由到目标服务
            - Path=/items/**,/search/** # 这里是以请求路径作为判断规则
        - id: cart
          uri: lb://cart-service
          predicates:
            - Path=/carts/**
        - id: user
          uri: lb://user-service
          predicates:
            - Path=/users/**,/addresses/**	# 路径满足/user/开头的就符合要求，托管给userservice服务
        - id: trade
          uri: lb://trade-service
          predicates:
            - Path=/orders/**
        - id: pay
          uri: lb://pay-service
          predicates:
            - Path=/pay-orders/**
```

网关本身并没有做什么操作，但是可以访问到相应信息，也就是说网关只是把请求转发给微服务。

<img src="SpringCloud\image-20241106103631894.png" alt="image-20241106103631894" style="zoom: 50%;" />

#### 路由断言

我们在配置文件中写的断言规则只是字符串，这些字符串会被断言工厂（Predicate Factory）读取并处理，转变为路由判断的条件。

| **名称**   | **说明**                       | **示例**                                                     |
| :--------- | :----------------------------- | :----------------------------------------------------------- |
| After      | 是某个时间点后的请求           | - After=2037-01-20T17:42:47.789-07:00[America/Denver]        |
| Before     | 是某个时间点之前的请求         | - Before=2031-04-13T15:14:47.433+08:00[Asia/Shanghai]        |
| Between    | 是某两个时间点之前的请求       | - Between=2037-01-20T17:42:47.789-07:00[America/Denver], 2037-01-21T17:42:47.789-07:00[America/Denver] |
| Cookie     | 请求必须包含某些cookie         | - Cookie=chocolate, ch.p                                     |
| Header     | 请求必须包含某些header         | - Header=X-Request-Id, \d+                                   |
| Host       | 请求必须是访问某个host（域名） | - Host=\**.somehost.org,**.anotherhost.org                   |
| Method     | 请求方式必须是指定方式         | - Method=GET,POST                                            |
| Path       | 请求路径必须符合指定规则       | - Path=/red/{segment},/blue/**                               |
| Query      | 请求参数必须包含指定参数       | - Query=name, Jack或者- Query=name                           |
| RemoteAddr | 请求者的ip必须是指定范围       | - RemoteAddr=192.168.1.1/24                                  |
| weight     | 权重处理                       |                                                              |

### 网关登录校验

> 网关鉴权，解决统一登录校验和用户信息获取的问题。

我们的登录是基于JWT来实现的，校验JWT的算法复杂，而且需要用到秘钥。如果每个微服务都去做登录校验，这就存在着两大问题：

- 每个微服务都需要知道JWT的秘钥，不安全
- 每个微服务重复编写登录校验代码、权限校验代码，麻烦

既然网关是所有微服务的入口，一切请求都需要先经过网关。我们完全可以把登录校验的工作放到网关去做，这样之前说的问题就解决了：只需要在网关和用户服务保存秘钥；只需要在网关开发登录校验功能。

<img src="SpringCloud\image-20241106104942057.png" alt="image-20241106104942057" style="zoom: 50%;" />

#### 网关过滤器

Gateway内部工作流程：

1. 客户端请求进入网关后由`HandlerMapping`对请求做判断，找到与当前请求匹配的**路由规则**（**`Route`**），然后将请求交给`WebHandler`去处理。
2. `WebHandler`则会加载当前路由下需要执行的**过滤器链**（**`Filter chain`**），然后按照顺序逐一执行过滤器（后面称为**`Filter`**）。
3. 图中`Filter`被虚线分为左右两部分，是因为`Filter`内部的逻辑分为`pre`和`post`两部分，分别会在请求路由到微服务**之前**和**之后**被执行。
4. 只有所有`Filter`的`pre`逻辑都依次顺序执行通过后，请求才会被路由到微服务。
5. **微服务返回结果后，再倒序执行`Filter`的`post`逻辑。**
6. 最终把响应结果返回。

最终请求转发是有一个名为`NettyRoutingFilter`的过滤器来执行的，而且这个过滤器是整个过滤器链中顺序最靠后的一个。**如果我们能够定义一个过滤器，在其中实现登录校验逻辑，并且将过滤器执行顺序定义到`NettyRoutingFilter`之前**，这就符合我们的需求了

网关过滤器链中的过滤器有两种：

- **`GatewayFilter`**：路由过滤器，作用范围比较灵活，可以是任意指定的路由`Route`. 
- **`GlobalFilter`**：全局过滤器，作用范围是所有路由，不可配置。

> 过滤器链之外还有一种过滤器，HttpHeadersFilter，用来处理传递到下游微服务的请求头。例如org.springframework.cloud.gateway.filter.headers.XForwardedHeadersFilter可以传递代理请求原本的host头到下游微服务。

**使用**

`Gateway`中内置了很多的`GatewayFilter`，详情可以参考官方文档：

https://docs.spring.io/spring-cloud-gateway/docs/3.1.7/reference/html/#gatewayfilter-factories

`Gateway`内置的`GatewayFilter`过滤器使用起来非常简单，无需编码，只要在yaml文件中简单配置即可。而且其作用范围也很灵活，配置在哪个`Route`下，就作用于哪个`Route`.

例如，有一个过滤器叫做`AddRequestHeaderGatewayFilterFacotry`，顾明思议，就是添加请求头的过滤器，可以给请求添加一个请求头并传递到下游微服务。

实际的使用只需要在application.yaml中这样配置：

```yaml
spring:
  cloud:
    gateway:
      routes:
      - id: test_route
        uri: lb://test-service
        predicates:
          -Path=/test/**
        filters:
          - AddRequestHeader=key, value # 逗号之前是请求头的key，逗号之后是value
```

验证是否增加了该请求头，在test服务中获取并打印：

```java
@GetMapping("/{id}")
public User queryById(@PathVariable("id") Long id, @RequestHeader(value = "Truth", required = false) String truth) {
    System.out.println("truth = " + truth);
    return userService.queryById(id);
}
```

而如果我们希望全局都可以增加这个过滤器，也就是全局都会增加这个请求头，只需要：

<img src="SpringCloud\image-20241106111049279.png" alt="image-20241106111049279" style="zoom: 80%;" />

#### 自定义过滤器

> 无论是`GatewayFilter`还是`GlobalFilter`都支持自定义，只不过**编码**方式、**使用**方式略有差别。

自定义`GatewayFilter`不是直接实现`GatewayFilter`，而是实现`AbstractGatewayFilterFactory`。(**该类的名称一定要以`GatewayFilterFactory`为后缀**)

```java
@Component
public class PrintAnyGatewayFilterFactory extends AbstractGatewayFilterFactory<Object> {
    @Override
    public GatewayFilter apply(Object config) {
        return new GatewayFilter() {
            @Override
            public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
                // 获取请求
                ServerHttpRequest request = exchange.getRequest();
                // 编写过滤器逻辑
                System.out.println("过滤器执行了");
                // 放行
                return chain.filter(exchange);
            }
        };
    }
}
```

配置：

```yaml
spring:
  cloud:
    gateway:
      default-filters:
            - PrintAny # 此处直接以自定义的GatewayFilterFactory类名称前缀类声明过滤器
```

**动态配置参数：**

```java

@Component
public class PrintAnyGatewayFilterFactory // 父类泛型是内部类的Config类型
                extends AbstractGatewayFilterFactory<PrintAnyGatewayFilterFactory.Config> {

    @Override
    public GatewayFilter apply(Config config) {
        // OrderedGatewayFilter是GatewayFilter的子类，包含两个参数：
        // - GatewayFilter：过滤器
        // - int order值：值越小，过滤器执行优先级越高
        return new OrderedGatewayFilter(new GatewayFilter() {
            @Override
            public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
                // 获取config值
                String a = config.getA();
                String b = config.getB();
                String c = config.getC();
                // 编写过滤器逻辑
                System.out.println("a = " + a);
                System.out.println("b = " + b);
                System.out.println("c = " + c);
                // 放行
                return chain.filter(exchange);
            }
        }, 100);
    }

    // ==========自定义配置属性，成员变量名称很重要，下面会用到===========
    @Data
    static class Config{
        private String a;
        private String b;
        private String c;
    }
    // 将变量名称依次返回，顺序很重要，将来读取参数时需要按顺序获取
    @Override
    public List<String> shortcutFieldOrder() {
        return List.of("a", "b", "c");
    }
        // 返回当前配置类的类型，也就是内部的Config
    @Override
    public Class<Config> getConfigClass() {
        return Config.class;
    }

}
```

yaml中配置

```yaml
spring:
  cloud:
    gateway:
      default-filters:
            - PrintAny=1,2,3 # 注意，这里多个参数以","隔开，将来会按照shortcutFieldOrder()方法返回的参数顺序依次复制
```

自定义GlobalFilter：直接实现GlobalFilter即可，而且也无法设置动态参数

```java
@Component
public class PrintAnyGlobalFilter implements GlobalFilter, Ordered {
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        // 编写过滤器逻辑
        System.out.println("未登录，无法访问");
        // 放行
        // return chain.filter(exchange);

        // 拦截
        ServerHttpResponse response = exchange.getResponse();
        response.setRawStatusCode(401);
        return response.setComplete();
    }

    @Override
    public int getOrder() {
        // 过滤器执行顺序，值越小，优先级越高
        return 0;
    }
}
```

#### 过滤器链执行顺序

请求进入网关会碰到三类过滤器：当前路由的过滤器、DefaultFilter、GlobalFilter。
请求路由后，会将当前路由过滤器和DefaultFilter、GlobalFilter合并到一个过滤器链中，排序后依次执行每个过滤器。

排序的规则：

> 每一个过滤器都必须指定一个int类型的order值，order值越小，优先级越高，执行顺序越靠前：
> （1）GlobalFilter通过实现Ordered接口，或者添加@Order注解来指定order值，由自己决定
> （2）路由过滤器和defaultFilter的order是由Spring指定的，是按照配置文件中的声明顺序来递增的
> 当过滤器的order值一样时，会按照defaultFilter＞路由过滤器＞GlobalFilter的顺序执行

#### 登录校验

```java
@Component
@RequiredArgsConstructor
@EnableConfigurationProperties(AuthProperties.class)
public class AuthGlobalFilter implements GlobalFilter, Ordered {

    private final JwtTool jwtTool;

    private final AuthProperties authProperties;

    private final AntPathMatcher antPathMatcher = new AntPathMatcher();

    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        // 1.获取Request
        ServerHttpRequest request = exchange.getRequest();
        // 2.判断是否不需要拦截
        if(isExclude(request.getPath().toString())){
            // 无需拦截，直接放行
            return chain.filter(exchange);
        }
        // 3.获取请求头中的token
        String token = null;
        List<String> headers = request.getHeaders().get("authorization");
        if (!CollUtils.isEmpty(headers)) {
            token = headers.get(0);
        }
        // 4.校验并解析token
        Long userId = null;
        try {
            userId = jwtTool.parseToken(token);
        } catch (UnauthorizedException e) {
            // 如果无效，拦截
            ServerHttpResponse response = exchange.getResponse();
            response.setRawStatusCode(401);
            return response.setComplete();
        }

        // ==================== TODO 5.如果有效，传递用户信息 ==============================
        System.out.println("userId = " + userId);
        // 6.放行
        return chain.filter(exchange);
    }

    private boolean isExclude(String antPath) {
        for (String pathPattern : authProperties.getExcludePaths()) {
            if(antPathMatcher.match(pathPattern, antPath)){
                return true;
            }
        }
        return false;
    }

    @Override
    public int getOrder() {
        return 0;
    }
}
```

#### 用户信息传递

> 由于网关发送请求到微服务依然采用的是`Http`请求，因此我们可以**将用户信息以请求头的方式传递到下游微服务**。然后微服务可以**从请求头中获取登录用户信息**。考虑到微服务内部可能很多地方都需要用到登录用户信息，因此我们可以利用SpringMVC的拦截器来实现登录用户信息获取，并存入ThreadLocal，方便后续使用。

1.保存用户信息到请求头

```java
// 发送给前端新token
response.getHeaders().add("authorization", token);
// 暴露头
response.getHeaders().add("Access-Control-Expose-Headers", "authorization");

// 将当前请求头中已经过期的token替换成新的token
// 将新的token转发给微服务
ServerHttpRequest.Builder requestBuilder = request.mutate();
// 先删除，后新增
requestBuilder.headers(k -> k.remove("Authorization"));
requestBuilder.header("Authorization", token);
ServerHttpRequest requestNew = requestBuilder.build();
exchange.mutate().request(requestNew).build();
```

2.拦截器获取用户

```java
package com.hmall.common.interceptor;

import cn.hutool.core.util.StrUtil;
import com.hmall.common.utils.UserContext;
import org.springframework.web.servlet.HandlerInterceptor;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class UserInfoInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        // 1.获取请求头中的用户信息
        String userInfo = request.getHeader("user-info");
        // 2.判断是否为空
        if (StrUtil.isNotBlank(userInfo)) {
            // 不为空，保存到ThreadLocal
                UserContext.setUser(Long.valueOf(userInfo));
        }
        // 3.放行
        return true;
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        // 移除用户
        UserContext.removeUser();
    }
}
```





# 参考

黑马SpringCloud：[2024最新SpringCloud微服务开发与实战，java黑马商城项目微服务实战开发（涵盖MybatisPlus、Docker、MQ、ES、Redis高级等）_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1S142197x7/?spm_id_from=333.337.search-card.all.click&vd_source=bf952648bf410c0b9b23bf213e3d24ba)

配套文档：https://b11et3un53m.feishu.cn/wiki/space/7229522334074372099?ccm_open_type=lark_wiki_spaceLink&open_tab_from=wiki_home

docker：[docker全流程使用指南_docker工作流程-CSDN博客](https://blog.csdn.net/qq_41196612/article/details/131083964)

[一文快速学会Docker软件部署-CSDN博客](https://blog.csdn.net/m0_52380556/article/details/135483179?spm=1001.2014.3001.5502)

狂神说Nginx：[Nginx快速入门-KuangStudy-文章](https://www.kuangstudy.com/bbs/1353634800149213186)

[Nginx详解（一文带你搞懂Nginx）-CSDN博客](https://blog.csdn.net/hyfsbxg/article/details/122322125)

微服务：[【1.2】认识微服务--微服务技术对比&SpringCloud_springclould 和其他微服务框架比较-CSDN博客](https://blog.csdn.net/Eumenides_Suki/article/details/128487022)

[详解SpringCloud微服务技术栈：认识微服务、服务拆分与远程调用_spring微服务之间调用-CSDN博客](https://blog.csdn.net/m0_52380556/article/details/135583389?spm=1001.2014.3001.5502)

