[📄 使用文档](https://ehang-io.github.io/nps/#/) [🧑‍💻 Github](https://github.com/ehang-io/nps)

# 0-服务器端安装

先到 `Github` 发布界面下载服务器端对应的版本，下载完成后解压

```shell
tar -zxvf linux_amd64_server.tar.gz

# 解压完成直接安装
sudo ./nps install
```

安装完成后，多余的文件就可以删除了，先修改一下配置文件

```shell
# 切换为 root
sudo su
cd /etc/nps/conf

# 编辑配置文件
vim nps.conf

# 桥接端口我想改一下
bridge_port=40005

# web面板设置主要改端口和账户
web_username=xxx
web_password=xxx
web_port = 40001

# 听闻有漏洞，防止别人直接进后台，一下两个参数随意设置
auth_key=xxxxx
auth_crypt_key = xxx
```

以上，服务器端就配置完成了，执行 `sudo nps start` 就可以启动了，然后登录后台，记得打开服务器对应端口的防火墙

# 1-服务端设置

首先就是添加客户端（允许配置文件连接为**是**也可以）

![[Pasted image 20230816111334.png]]

添加完成后，再次点击客户端刷新就可以看到了，然后点击隧道添加即可，如果要访问内网机器对应的端口，一般使用 `TCP隧道` 即可（同样记得打开防火墙）

# 2-客户端设置

先到 `Github` 发布页面下载对应的客户端版本，执行解压命令即可

```shell
tar -zxvf linux_amd64_client.tar.gz -C ./NPC
```

如果想直接启动，在服务端的界面可以直接看到对应的启动命令，复制执行即可

![[Pasted image 20230816112016.png]]

为了方便使用，我们可以注册到系统服务，命令如下：（我没成功，可以参考下一节）

```shell
# 以下参数已自己具体服务器为准
sudo ./npc install -server=xxx -vkey=xxx -type=tcp

# 启动/停止
sudo npc start
sudo npc stop

# 如果需要更换配置的话需要先卸载再重新注册
./npc uninstall
```

> 💥 **不知道为什么，我按照官方的文档，使用系统服务这种方式失败了，但是使用无配置模式启动又是成功的，所以我打算自己来建立一个系统服务**

# 3-自建系统服务启动

首先确保无配置模式启动是成功的

```shell
# 切换为root
sudo su

# 编写配置文件
vim /etc/systemd/system/npc.service

# 文件内容如下，其中启动命令自行更改
[Unit]
Description=NPC
[Service]
Type=simple
ExecStart=npc -server=xxx -vkey=xxx -type=tcp
[Install]
WantedBy=default.target

# 最后执行命令启动即可
systemctl start npc
systemctl enable npc
```

如果启动服务时遇到服务无法启动，参考 [[🪄Server·魔法#^f7190c| 安全机制]]
