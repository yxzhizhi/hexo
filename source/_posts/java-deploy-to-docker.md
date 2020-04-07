---
title: java deploy to docker
date: 2020-04-06 15:36:42
tags:
- [java,docker]
---

------------------------------

ubuntu普通用户登录操作docker提示无权限的解决办法
======

```shell
sudo groupadd docker
sudo usermod -aG docker ${USER}
newgrp docker     #更新用户组
docker ps    #测试docker命令是否可以使用sudo正常使用
#1 添加docker用户组(一般安装docker时会自动添加)
sudo groupadd docker 
#2 将指定用户添加到docker用户组中 注:将USER替换为自己的用户名
sudo gpasswd -a USER docker
#3 重启docker服务
sudo systemctl restart docker
#4 退出SSH连接，重新登录
```
<!--more-->
grep 正则
======
```tex
1、或操作
grep -E '123|abc' filename  // 找出文件（filename）中包含123或者包含abc的行
egrep '123|abc' filename    // 用egrep同样可以实现
awk '/123|abc/' filename   // awk 的实现方式
2、与操作
grep pattern1 files | grep pattern2 //显示既匹配 pattern1 又匹配 pattern2 的行。
3、其他操作
grep -i pattern files   //不区分大小写地搜索。默认情况区分大小写，
grep -l pattern files   //只列出匹配的文件名，
grep -L pattern files   //列出不匹配的文件名，
grep -w pattern files  //只匹配整个单词，而不是字符串的一部分（如匹配‘magic’，而不是‘magical’），
grep -C number pattern files //匹配的上下文分别显示[number]行，
```
删除容器
=======

```bash
# 根据镜像id删除
docker rm $(docker ps -a|grep -E '1b9cf56dec97|4eb9989fc00f'| awk '{print $1}')
docker rm $(docker ps -a|grep 1b9cf56dec97| awk '{print $1}')
# 删除未命名的容器[通常由dockfile文件执行run命令自动产生的中间镜像]
docker rmi $(docker images | grep "none" | awk '{print $3}')
```

------

### 镜像 

1. 获取镜像 
```sudo docker pull oraclelinux```

2. 后台运行

```sudo docker run -itd --name oraclelinux_dev oraclelinux /bin/bash```

1. 查询容器
```sudo docker ps #47e0e5a52244 oraclelinux_dev```

4. 进入容器
在使用 -d 参数时，容器启动后会进入后台。此时想要进入容器，可以通过以下指令进入：

```docker attach  $ sudo docker attach 47e0e5a52244```
 #注意： 如果从这个容器退出，会导致容器的停止。
docker exec：推荐大家使用 docker exec 命令，因为此退出容器终端，不会导致容器的停止。

```sudo docker exec -it 47e0e5a52244 /bin/bash```

5. 导出容器
如果要导出本地某个容器，可以使用 docker export 命令。

```docker export 47e0e5a52244 > oraclelinux_dev.tar```

6. 导入容器快照
可以使用 docker import 从容器快照文件中再导入为镜像，以下实例将快照文件 oraclelinux_dev.tar 导入到镜像 test/oraclelinux_dev:v1:

```cat docker/oraclelinux_dev.tar | sudo docker import - dev/oraclelinux_dev:v1```

```sudo docker import oraclelinux_dev dev/oraclelinux_dev```

7. 下面的命令可以清理掉所有处于终止状态的容器。
```docker container prune```

用Docker搭建Python的开发环境
======

```bash
sudo docker pull python:3.7.6
cd /home/zhanghuo/PycharmProjects/db_manage_vue
docker run  -v /home/zhanghuo/PycharmProjects/db_manage_vue:/usr/src/db_manage_vue  -w /usr/src/db_manage_vue python:3.7.6 python ./manage.py runserver
注意事项：
-v 将主机的py文件目录挂载到容器中的/usr/src/db_manage_vue
-w 指定容器的/usr/src/db_manage_vue目录为工作目录
python ./manage.py runserver 用容器中的python命令来执行工作目录的pyth.py
dockerfile
FROM python:3.7.6
COPY . /usr/src/db_manage_vue
WORKDIR /usr/src/db_manage_vue
RUN pip install -r requirements.txt
ENTRYPOINT ["python"]
CMD ["./manage.py runserver"]

-- 生成依赖包文件
pip freeze > requirements.txt
创建镜像
sudo docker build -t db_manage_vue/python .
运行容器：
docker run id/db_manage_vue/python
### docker-compose

```

docker compose安装
======
参考daocloud的https://get.daocloud.io/#install-compose
------
1. github

```bash
https://github.com/docker/compose/releases
curl -L https://github.com/docker/compose/releases/download/1.25.4/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```

1. daocloud
Docker Compose 存放在Git Hub，不太稳定。
你可以也通过执行下面的命令，高速安装Docker Compose。

```bash
sudo curl -L https://get.daocloud.io/docker/compose/releases/download/1.25.4/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

你可以通过修改URL中的版本，可以自定义您的需要的版本。
3. 测试安装

```bash
docker-compose --version
```

### 重命名镜像

```bash
docker tag IMAGEID(镜像id) REPOSITORY:TAG（仓库：标签）
#例子
docker tag ca1b6b825289 registry.cn-hangzhou.aliyuncs.com/xxxxxxx:v1.0
```

修改镜像源
======

```bash
 vi /etc/docker/daemon.json
{"registry-mirrors": ["https://s75dbt4b.mirror.aliyuncs.com"]}
```

然后进入到 mysql 容器中将 django 数据库文件导入：

```bash
#docker inspect --format "{{.State.Pid}}" mysql
12674
#nsenter --target 12674 --mount --uts --ipc --net --pid
root@91308514f209:/# cd /etc/mysql/conf.d/
root@91308514f209:/etc/mysql/conf.d# mysql -uroot -p jianshu \< jianshu.sql
```

docker 发布 django项目
======

```bash
打包django项目
先生成需要的python模块列表
pip freeze >req.txt
然后打包程序
tar cvf django1.tar ./django1
scp到docker服务器的/python目录下解压
确定基础镜像版本
docker pull centos:7.3.1611
运行此镜像
docker run -d -i -v /python:/python -tcentos:7.3.1611
-d为后台运行
-v 为映射本地目录到docker中
准备程序运行环境
然后进入到运行的docker中，安装需要的软件包以及模块
yum install xxxx
pip install –r req.txt
生成新镜像
docker commit bd486b5df131 centos/django
运行
docker run -itd -p 8000:8000  -v /python:/python -w /python centos/django1python /python/django1/manage.py runserver 0.0.0.0:8000
运行的时候使用
docker export db509b5a599f >django.tar
然后在目标服务器
cat django.tar | docker import -centos/django
然后新建/python/
将django1.tar项目文件传送到目录上，解压
运行
docker run -itd -p 8000:8000  -v /python:/python -w /python centos/djangopython /python/django1/manage.py runserver 0.0.0.0:8000
查看项目库运行日志：
docker logs fba7f2b32fb0 -f
-docker 发布 django项目 end-
```

------------------------------
