---
title: eclipse/che
date: 2020-04-05
categories:
- linux
- che
- eclipse
tags:
- che
top: 100
---

### portainer
```
下载镜像
docker pull portainer/portainer
基于镜像运行容器

docker run -d -p 9000:9000 --restart=always -v /var/run/docker.sock:/var/run/docker.sock --name prtainer  portainer/portainer

up: admin/yx375656

```
<!--more-->

### eclipse/che
``` 
启动服务
-v /var/…是选择docker文件
-v /opt/…是选择容器存放位置
-e CHE_HOST 设置主机的IP地址

单用户：
docker run -ti --rm --name che6 -v /var/run/docker.sock:/var/run/docker.sock -v /home/zhanghuo/LinuxHome/docker/eclipseche:/data  -e CHE_PORT=8080 -e CHE_HOST=192.168.10.225 eclipse/che:6.19.0 start

多用户：
docker run -ti -e CHE_MULTIUSER=true -v /var/run/docker.sock:/var/run/docker.sock -v /home/zhanghuo/LinuxHome/docker/eclipseche:/data  -e CHE_PORT=8080 -e CHE_HOST=192.168.6.203 eclipse/che:6.19.0 start
实际创建:
docker run -ti -e CHE_MULTIUSER=true -v /var/run/docker.sock:/var/run/docker.sock -v /home/zhanghuo/LinuxHome/docker/eclipseche:/data  -e CHE_PORT=8080  eclipse/che:6.19.0 start
```

### 获取镜像
```
~/docker-tags eclipse/che
sudo docker pull eclipse/che:6.19.0
```
### 启动服务
```
sudo docker run -it --rm -v /var/run/docker.sock:/var/run/docker.sock -v /home/zhanghuo/LinuxHome/docker/eclipseche:/data eclipse/che:6.19.0 start
启动完毕后，访问 “http://:8080/” 来验证安装。

```
```
创建 workspace 和 project
访问 “Workspaces -> Add Workspace”；
在 “New Workspace” 页面选择 “java Default Java Stack with JDK 8, Maven and Tomcat.” 然后 “CREATE & OPEN”；
在 “Workspace” 页面，选择 “Create Project…”；
在 “Create New Project” 窗口，选择 Java -> Maven 项目，然后输入一个Name，比如: “test”，并进入下一步；
勾选 “From Archetype:” 并选择 “org.apache.maven.archetypes:maven-archetype-quickstart:RELEASE”，同时输入 “Artifact ID” 和 “Group ID”，然后 “Create” 来创建工程；
运行工程
选中并打开工程，然后在 “Manage commands” 依次创建三个 Maven 命令，并运行。

build
mvn clean install -f ${current.project.path}
1
test
mvn clean test -f ${current.project.path}
1
run
mvn exec:java -Dexec.mainClass="test.App" -f ${current.project.path}
```

### eclipse/che 命令 -ubuntu
##### 1. apt-get update
##### 2. [安装docker,国内源安装](https://link.jianshu.com/?t=https://yeasy.gitbooks.io/docker_practice/content/install/ubuntu.html)

##### 3.[配置镜像仓库](https://link.jianshu.com/?t=https://yeasy.gitbooks.io/docker_practice/content/install/mirror.html#ubuntu-1604、debian-8-jessie、centos-7)

##### 4.解决内存溢出的问题

```javascript
Adjust memory and swap accounting
When users run Docker, they may see these messages when working with an image:

WARNING: Your kernel does not support cgroup swap limit. WARNING: Your
kernel does not support swap limit capabilities. Limitation discarded.
To prevent these messages, enable memory and swap accounting on your system. To enable these on system using GNU GRUB (GNU GRand Unified Bootloader), do the following.

Log into Ubuntu as a user with sudo privileges.

Edit the /etc/default/grub file.

Set the GRUB_CMDLINE_LINUX value as follows:

GRUB_CMDLINE_LINUX="cgroup_enable=memory swapaccount=1"
Save and close the file.

Update GRUB.

$ sudo update-grub
Reboot your system.
```

##### 5关闭防火墙

[网络相关配置](https://link.jianshu.com/?t=http://www.cnblogs.com/wclwcw/p/6140263.html)

```javascript
ufw disable
```

-  安装

```javascript
docker pull eclipse/che:5.17.0
```

-  启动(第一次启动需要下载其他镜像)

```javascript
docker run -it --rm -e CHE_PORT=8120 -v /var/run/docker.sock:/var/run/docker.sock -v /c/8120/tmp:/data eclipse/che:5.17.0 start
```

-  修改che的样式文件，去掉左边导航栏

```javascript
docker cp /che/index.html che-8120:/home/user/eclipse-che-5.17.0/tomcat/webapps/dashboard
```

-  停止

```javascript
docker run -it --rm -e CHE_PORT=8120 -v /var/run/docker.sock:/var/run/docker.sock -v /c/8120/tmp:/data eclipse/che:5.17.0 stop
```

-  重启

```javascript
docker run -it --rm -e CHE_PORT=8120 -v /var/run/docker.sock:/var/run/docker.sock -v /c/8120/tmp:/data eclipse/che:5.17.0 restart
```

-  che API

启动che的时候的终端返回信息最后一行是che的api地址

-  创建che工作空间

```javascript
curl -X POST -H 'Content-Type: application/json' -d '{"name":"myworkspace","projects":[],"commands":[{"name":"build","type":"mvn","attributes":{"goal":"Build","previewUrl":""},"commandLine":"mvn clean install"],"environments":{"myworkspace":{"recipe":{"location":"eclipse/ubuntu_jdk8","type":"dockerimage"},"machines":{"dev-machine":{"attributes":{"memoryLimitBytes":"2147483648"},"agents":["org.eclipse.che.exec","org.eclipse.che.terminal","org.eclipse.che.ws-agent","org.eclipse.che.ssh"],"servers":{}}}}},"defaultEnv":"myworkspace","links":[]}' http://localhost:8080/api/workspace
//其中-d为创建工作空间所需json参数，具体请看下一小节
```

-  修改che的运行时环境

访问che的webide，点击左侧stacks，在右侧的列表中选择自己需要的运行时环境点进进入详情界面



下拉找到row configuration，复制其中json数据里的workspaceconfig部分代码（注意只取 "workspaceConfig":后面的{}已经其中的信息），作为访问创建che工作空间api的参数


-  在项目中使用che api需要执行以下命令

```javascript
git clone http://github.com/eclipse/che
cd cde
git checkout 5.17.x
cd core
mvn install
```

-  chedir 初始化工作空间和项目

```javascript
cd /ChedirDocker/project
mkdir che8081project
cd che8081project
//创建Chedir文件
docker run -it --rm -e CHE_PORT=8081 -v /var/run/docker.sock:/var/run/docker.sock -v /c/8081/tmp:/data  -v  /ChedirDocker/project/che8081project:/chedir eclipse/che:5.17.0 dir init
//进行相应修改
vim Chedir
//启动容器
/var/run/docker.sock:/var/run/docker.sock -v /c/8081/tmp:/data  -v  /ChedirDocker/project/che8081project:/chedir eclipse/che:5.17.0 dir up
//销毁容器
/var/run/docker.sock:/var/run/docker.sock -v /c/8081/tmp:/data  -v  /ChedirDocker/project/che8081project:/chedir eclipse/che:5.17.0 dir down
```

