---
title: docker 
date: 2020-04-05
categories:
- linux
- docker
tags:
- docker
---

## 常用docker命令
#### 镜像管理
```
列表|docker images	|查看本地docker仓库的所有镜像
docker images 列出本地所有镜像
检索 docker search keywork eg: docker search tomcat 去docker hub搜索镜像的详细信息
docker search(-s) nginx 搜索相关镜像　加上-s 参数 选出至少start数范围的镜像
docker pull（-a） 镜像名:版本号　拉取镜像,-a pull all
拉取 docker pull keywork:tag eg: docker pull tomcat:latest|	tag是可选的，不指定默认拉取latest最新版本
<!--more-->
docker push 192.168.0.100:5000/ubuntu 　　  推送镜像库到私有源
删除|docker rmi imageId eg: docker rmi 6408fdc94212	|删除本地docker仓库镜像
docker rmi（-f） 镜像名：版本号/镜像ID 　　删除镜像 （加上 -f 参数 强制删除）
docker rmi $(docker images -q)　　删除所有镜像
docker rmi $(docker images | grep "none" | awk '{print $3}') 删除所有名字中带“none” 关键字的镜像

docker save docker.io``/tomcat``:7.0.77-jre7 >``/root/mytomcat7``.``tar``.gz 导出镜像`
docker load < ``/root/mytomcat7``.``tar``.gz　　导入镜像`
```
### 容器管理
```
docker ps 查看当前正在运行的容器
docker inspect name/image[name/image...] 查看详细
docker ps -a 查看所有容器的状态
docker start/stop(-t) id/name[name...] 启动/停止某个（多个）容器 -t 指定时间
docker kill (-s) name[name...] 强制中断 -s指定SIGINT信号类型，默认“kill”
docker restart (-t) name[name...] 重启 -t 指定时间
docker pause name　暂停　　docker unpause name 继续
docker rm(-$) name[name...] 移除 
    -f　　--force=false　　强制移除运行中容器
    -l　　--link=false　　 移除指定链接，保留底层容器
    -v　 --volumes=false  移除容器关联卷
docker commit(-$)name 镜像名:版本号 　　提交指定容器为镜像
    -a, --author="" 　　　　作者
    -m, --message="" 　　 简要说明
    -p, --pause=true 　　　暂停容器再提交
docker logs(-$) name　　输出指定容器日志信息
    -f　　跟踪日志输出
    -t　　显示时间戳 类似 tail -f
    --tail 在日志的末尾输出指定行数日志（默认所有日志）
docker attach id 进入某个容器(使用exit退出后容器也跟着停止运行)
docker exec -ti id 启动一个伪终端以交互式的方式进入某个容器（使用exit退出后容器不停止运行）
docker run(-$) IMAGE [COMMAND] [ARG...] 　 运行一个容器
    -d　　　　　　　　  指定容器运行于前台还是后台，默认为false   
    -i　　　　　　　　  打开STDIN，用于控制台交互，默认为false 
    -t　　　　　　　　  分配tty设备，该可以支持终端登录，默认为false  
    -u, --user=""       指定容器的用户  
    -a, --attach=[]      登录容器（必须是以docker run -d启动的容器） 
    -w　　　　　　　　  指定容器的工作目录 
    -c   　　　　　　　设置容器CPU权重，在CPU共享场景使用  
    -e, --env=[]        指定环境变量，容器中可以使用该环境变量  
    -m　　　　　　　　  指定容器的内存上限  
    -P, --publish-all=false 指定容器暴露的端口  
    -p, --publish=[]      指定容器暴露的端口  
    -h　　　　　　　　　指定容器的主机名  
    -v, --volume=[]      给容器挂载存储卷，挂载到容器的某个目录 
    --volumes-from=[]    给容器挂载其他容器上的卷，挂载到容器的某个目录 
    --cap-add=[]　　　　 添加权限
    --cap-drop=[]   　 删除权限
    --cidfile=""　　　　　 运行容器后，在指定文件中写入容器PID值，监控系统用法  
    --cpuset=""　 　　　 设置容器可使用哪些CPU，此参数可以用来容器独占CPU  
    --device=[]   　　　  添加主机设备给容器，相当于设备直通  
    --dns=[]            指定容器的dns服务器  
    --dns-search=[]      指定容器的dns搜索域名，写入到容器/etc/resolv.conf文件  
    --entrypoint=""       覆盖image的入口点  
    --env-file=[]          指定环境变量文件，文件格式为每行一个环境变量  
    --expose=[]         指定容器暴露的端口，即修改镜像的暴露端口  
    --link=[]            指定容器间的关联，使用其他容器的IP、env等信息  
    --lxc-conf=[]         指定容器的配置文件，只有在指定--exec-driver=lxc时使用  
    --name=""          指定容器名字，links特性需要使用名字  
    --net="bridge"       容器网络设置: 
        bridge 使用docker daemon指定的网桥    
        host  //容器使用主机的网络  
        container:NAME_or_ID >//使用其他容器的网路共享IP和PORT等网络资源  
        none 容器使用自己的网络（类似--net=bridge）
    --privileged=false     指定容器是否为特权容器，特权容器拥有所有的权限
    --restart="no"        指定容器停止后的重启策略: 
        no：　　　　容器退出时不重启  
        on-failure：  容器故障退出（返回值非零）时重启  
        always：　　 容器退出时总是重启  
    --rm=false      指定容器停止后自动删除容器(不支持以docker run -d启动的容器)  
    --sig-proxy=true 设置由代理接受并处理信号，SIGCHLD，SIGSTOP和SIGKILL不代
例：
docker run -i -t centos6.8 进入到默认的线程”/bin/bash”，直接进入控制台操作
docker run -i -t -d centos6.8 进入到默认的线程”/bin/bash”，后台运行
docker run -d --restart=always centos6.8 ping www.docker.com 带命令启动
docker run -d --name=server-dbcentos6.8-mysql /usr/bin/mysql_safe -d 容器的名称为server-db，同时激活了数据库mysql的后台线程
docker run -d --name=server-db -p 3306:3306 -v /server/mysql-data:/
mysql-datacentos6.8-mysql /usr/bin/mysql_safe –d
docker run -d --name=server-db -p 3306:3306 centos6.8-mysql /usr/bin/mysql_safe –d 服务器宿主机与容器端口映射并暴露
docker run -d --name=server-http --link=server-db  -p 8080:80centos6.8-httpd /usr/bin/httpd --DFOREGROUND 映射服务器宿主机的8080端口，关联service-db 
docker run -it --rm centos6.8　　容器进程结束后，立马自动删除
```

#### docker option
```
    --api-enable-cors=false      在远程API中启用CORS 头
    -b, --bridge=""          　　桥接网络 使用“none” 禁用容器网络
    --bip=""             　　　网桥模式                     
    -d, --daemon=false         守护者模式
    -D, --debug=false          debug 模式
    --dns=[]             　　  强制 docker 使用指定 dns 服务器
    --dns-search=[]         　 强制 docker 使用指定 dns 搜索域
    -e, --exec-driver="native"     强制 docker 运行时使用指定执行驱动器
    --fixed-cidr=""          　　 固定IP的IPv4子网(例: 10.20.0.0/16)必须镶套在桥子网中(由-b or --bip定义)                     
    -G, --group="docker"        当在守护模式中运行时，组指向-H指定的unix套接字。使用""禁用组设置。
    -g, --graph="/var/lib/docker"   容器运行的根目录路径
    -H, --host=[]            　 套接字绑定到守护模式。使用一个或多个tcp://主机:端口，unix:///路径/到/套接字，fd://*或fd://socketfd.
    --icc=true            　　  inter-container跨容器通信
    --insecure-registry=[]        使用指定的注册表启用不安全通信(没有HTTPS的证书验　证和启用HTTP回退)(例如，localhost:5000或10.20.0 /16)
    --ip="0.0.0.0"          　　 绑定容器端口时使用的IP地址
    --ip-forward=true           使用net.ipv4.ip_forward转发
    --ip-masq=true        使IP伪装成桥的IP范围
    --iptables=true          　　启用Docker添加iptables规则
    --mtu=0              　　  设置容器网络mtu               
    -p, --pidfile="/var/run/docker.pid"   指定守护进程pid文件位置
    --registry-mirror=[]       　　指定一个首选的镜像仓库（加速地址）         
    -s, --storage-driver=""        强制 docker 运行时使用指定存储驱动
    --selinux-enabled=false       开启 selinux 支持
    --storage-opt=[]         　　设置存储驱动选项
    --tls=false            　　　 开启 tls
    --tlscacert="/root/.docker/ca.pem"　　只信任提供CA签名的证书
    --tlscert="/root/.docker/cert.pem"    tls 证书文件位置
    --tlskey="/root/.docker/key.pem" 　　 tls key 文件位置
    --tlsverify=false         　　　　　 使用 tls 并确认远程控制主机-v, 
    --version=false   输出 docker 版本信息
```

# [Docker 容器镜像删除](https://www.cnblogs.com/q4486233/p/6482711.html)
```
1.停止所有的container，这样才能够删除其中的images：
docker stop $(docker ps -a -q)
如果想要删除所有container的话再加一个指令：
docker rm $(docker ps -a -q)
2.查看当前有些什么images
docker images
3.删除images，通过image的id来指定删除谁
docker rmi <image id>
想要删除untagged images，也就是那些id为<None>的image的话可以用
docker rmi $(docker images | grep "^<none>" | awk "{print $3}")
要删除全部image的话
docker rmi $(docker images -q)
```

**1、导出某个容器**
导出某个容器，非常简单，使用docker export命令，语法：docker export $container_id > 容器快照名
**2、导入某个容器**--docker import命令
有了容器快照之后，我们可以在想要的时候随时导入。导入快照使用docker import命令。
例如我们可以使用cat centos.tar | docker import - my/centos:v888 导入容器快照作为镜像
处理本地的容器快照导入为镜像，我们还可以通过指定一个URL或者目录来导入。
例如在某个网络上有个快照image_test.tgz：docker import http://xxxx.com/image_test.tgz test/image_test
**镜像保存/载入**：docker load/docker save；将一个镜像导出为文件，再使用docker load命令将文件导入为一个镜像，会保存该镜像的的所有历史记录。比docker export命令导出的文件大，很好理解，因为会保存镜像的所有历史记录。
**容器导入/导出**：docker import/docker export；将一个容器导出为文件，再使用docker import命令将容器导入成为一个新的镜像，但是相比docker save命令，容器文件会丢失所有元数据和历史记录，仅保存容器当时的状态，相当于虚拟机快照。
**3、删除容器**
可以使用"docker rm 容器id"来删除一个终止状态的容器；若要删除一个运行中的容器，需要加-f参数。
## **清除所有未使用或悬空的图像，容器，卷和网络**
Docker提供了一个命令，可以清理悬空的任何资源（图像，容器，卷和网络）（与容器无关）：
```javascript
docker system prune
```
要另外删除任何已停止的容器和所有未使用的图像（不只是悬空图像），请将该`-a`标志添加到命令：
```javascript
docker system prune -a
```
## **删除Docker镜像**
### **删除一个或多个特定图像**
使用带有`-a`标志的命令`docker images`可以找到要删除的图像的ID。这将显示每个图像，包括中间图像层。当您找到要删除的图像时，可以将其ID或标记传递给`docker rmi`：
**列表：**
```javascript
docker images -a
```
**去掉：**

```javascript
docker rmi Image Image
```

### **删除悬空图像**

Docker图像由多个图层组成。悬空图像是与任何标记图像无关的图层。它们不再用于目的并占用磁盘空间。它们可以通过添加具有值`dangling=true`的`-f`过滤器标志到`docker images`的命令来定位。如果您确定要删除它们，可以使用以下`docker images purge`命令：

**注意：**如果您在不标记图像的情况下构建图像，则图像将显示在悬空图像列表中，因为它与标记图像无关。您可以通过在构建时提供标记来避免这种情况，并且可以使用docker tag命令追溯标记图像。

**列表：**

```javascript
docker images -f dangling=true
```

**去掉：**

```javascript
docker images purge
```

### **根据图案删除图像**

你可以使用组合模式`docker images`和`grep`找到相匹配的图像。一旦您满意，您可以通过使用`awk`来删除它们`docker rmi`。请注意，这些实用程序不是由Docker提供的，并不一定适用于所有系统：

**列表：**

```javascript
docker images -a |  grep "pattern"
```

**去掉：**

```javascript
docker images -a | grep "pattern" | awk '{print $3}' | xargs docker rmi
```

### **删除所有图像**

通过添加`-a`到`docker images`命令，可以列出系统上的所有Docker映像。一旦确定要全部删除它们，就可以添加`-q`标志以将图像ID传递给`docker rmi`：

**列表：**

```javascript
docker images -a
```

**去掉：**

```javascript
docker rmi $(docker images -a -q)
```

## **删除容器**

### **删除一个或多个特定容器**

使用带有该`-a`标志的`docker ps`命令可以找到要删除的容器的名称或ID：

**列表：**

```javascript
docker ps -a
```

**去掉：**

```javascript
docker rm ID_or_Name ID_or_Name
```

### **退出时取出容器**

如果您知道何时创建容器，一旦完成就不想保留它，您可以运行`docker run --rm`以在退出时自动删除它。

**运行和删除：**

```javascript
docker run --rm image_name
```

### **删除所有已退出的容器**

您可以使用以下`docker ps -a`状态定位容器并对其进行过滤：创建，重新启动，运行，暂停或退出。要查看已退出容器的列表，请使用`-f`标志根据状态进行过滤。当您确认要删除这些容器时，使用`-q`将ID传递给`docker rm`命令。

**列表：**

```javascript
docker ps -a -f status=exited
```

**去掉：**

```javascript
docker rm $(docker ps -a -f status=exited -q)
```

### **使用多个过滤器移除容器**

可以通过使用附加值重复过滤器标志来组合Docker过滤器。这导致满足任一条件的容器列表。例如，如果要删除标记为**Created的**所有容器（运行具有无效命令的容器时可能导致的状态）或**Exited**，则可以使用两个过滤器：

**列表：**

```javascript
docker ps -a -f status=exited -f status=created
```

**去掉：**

```javascript
docker rm $(docker ps -a -f status=exited -f status=created -q)
```

### **根据图案移除容器**

您可以使用`docker ps`和grep的组合找到与模式匹配的所有容器。当您对要删除的列表感到满意时，可以使用`awk`和`xargs`提供ID给 `docker rmi`。请注意，这些实用程序不是由Docker提供的，并不一定适用于所有系统：

**列表：**
```javascript
docker ps -a |  grep "pattern”
```
**去掉：**
```javascript
docker ps -a | grep "pattern" | awk '{print $3}' | xargs docker rmi
```
### **停止并移除所有容器**
您可以查看系统上的容器`docker ps`。添加`-a`标志将显示所有容器。当您确定要删除它们时，可以添加`-q`标志以向 `docker stop`和`docker rm`命令提供ID：
**列表：**
```javascript
docker ps -a
```
**去掉：**

```javascript
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)
```
## **删除卷**
### **删除一个或多个特定卷 - Docker 1.9及更高版本**
使用此`docker volume ls`命令可找到要删除的卷名称。然后，您可以使用以下`docker volume rm`命令删除一个或多个卷：
**列表：**
```javascript
docker volume ls
```
**去掉：**

```javascript
docker volume rm volume_name volume_name
```

### **删除悬空卷 - Docker 1.9及更高版本**

由于卷的位置与容器无关，因此在移除容器时，不会同时自动删除卷。当卷存在且不再连接到任何容器时，它称为悬空卷。要找到它们以确认您要删除它们，可以使用带过滤器的命令`docker volume ls`将结果限制为悬空卷。当您对列表感到满意时，可以用`docker volume prune`将它们全部删除：

**列表：**

```javascript
docker volume ls -f dangling=true
```

**去掉：**

```javascript
docker volume prune
```

### **删除容器及其容量**

如果您创建了一个未命名的卷，则可以将其与具有该`-v`标志的容器同时删除。请注意，这仅适用于*未命名的*卷。成功删除容器后，将显示其ID。请注意，没有引用卷的删除。如果未命名，则会以静默方式从系统中删除。如果它被命名，它会默默地保持存在。

**去掉：**

```javascript
docker rm -v container_name
```

## **结论**

本教程介绍了一些用于使用Docker删除图像，容器和卷的常用命令。每个都可以使用许多其他组合和标志。



