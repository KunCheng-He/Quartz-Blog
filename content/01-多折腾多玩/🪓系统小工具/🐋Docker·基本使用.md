# 0-基本安装

> [!tip] Tips
> [Docker安装](https://docs.docker.com/engine/install/) 部分不同的 Linux 发行版可能不太相同，可见官网，此外还可以安装 [Docker Compose 插件](https://docs.docker.com/compose/install/linux/)。找镜像去 [DockerHub](https://hub.docker.com/)

**Ubuntu**

```shell
# 卸载旧版本
sudo apt-get remove docker docker-engine docker.io containerd runc

# 更新索引，允许apt使用HTTPS使用仓库
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg

# 添加Docker官方的GPG Key
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

然后设置仓库

```shel
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

开始安装

```shell
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

# 1-启动服务

```shell
# 查看docker版本
docker version

# docker服务管理
sudo systemctl start docker.service
sudo systemctl status docker.service
sudo systemctl stop docker.service
sudo systemctl restart docker.service
sudo systemctl enable docker.service

# 查看docker信息
docker info
```

# 2-权限问题

如果当你执行 `docker version` 时出现如下问题：

![[Pasted image 20230401162159.png]]

这是因为docker进程使用 Unix Socket 而不是 TCP 端口。而默认情况下，Unix socket 属于 root 用户，因此需要 root 权限才能访问。所以解决办法如下：

```shell
# 添加docker用户组（一般已经有了）
sudo groupadd docker

#将当前用户添加至docker用户组
sudo gpasswd -a $USER docker

#更新docker用户组
newgrp docker
```

# 3-拉取镜像的网络问题

+ [全部被墙后，自建docker镜像的方法](https://github.com/whyun-pages/docker-registry)
+ 上方操作也不好用了，可以参考[官方文档](https://docs.docker.com/engine/daemon/proxy/#httphttps-proxy)设置代理

# 4-基本操作

首先需要清楚，我们拉取得是镜像，基于一个镜像，我们可以构造很多容器

```shell
# 查看本地所有镜像
docker image ls
docker images

# 搜索镜像，如果不要latest版本的镜像，就需要指定TAG
docker search <镜像名字>:<TAG>

# 拉取镜像
docker pull <镜像名字>

# 删除镜像
docker rmi <镜像名字>

# 运行一个容器
# --name 指定容器名，不指定自动命名
# -i 以交互式形式运行容器
# -t 分配一个伪终端
# -d 在后台运行容器，不要阻塞当前终端窗口
# -p 配置端口
# TAG 如果这个镜像是latest版本就不用加标签，否则把标签加在后面
docker run --name <这个容器的名字> -d -p 本机端口:容器端口 <镜像ID或镜像名>:TAG <一些Linux命令>

# 停止一个容器
docker stop <容器ID或容器名>

# 查看所有的容器
docker ps -a

# 启动已停止运行的容器
docker start <容器ID或容器名>

# 进入正在运行的容器（以下这个命令退出容器后容器不会停止运行）
docker exec -it <容器ID或容器名> </bin/bash>(运行的命令)

# 删除容器
docker rm -f <容器ID或容器名>

# 清理掉所有处于终止状态的容器
docker container prune

# 拷贝文件
docker cp 主机文件路径 容器ID或容器名:容器路径  # 主机中文件拷贝到容器中
docker cp 容器ID或容器名:容器路径 主机文件路径  # 容器中文件拷贝到主机中

# 查看docker正在运行的一些进程
docker container ps

# 查看容器的日志
docker logs 容器ID或容器名字
```
