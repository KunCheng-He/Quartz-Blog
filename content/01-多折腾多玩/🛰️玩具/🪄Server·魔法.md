# 0-魔法简介

这里我自己使用的都是 `clash` 的订阅地址，所以需要在服务器上运行 `clash` 小猫咪这个软件来实现魔法功能。其中资料来源于 [地址](https://cloud.tencent.com/developer/article/2173567)，为了防止失效，自行记录一份。

# 1-手动安装
## 1.0-安装Clash

这一步很简单，可以到 [Github](https://github.com/Dreamacro/clash/releases/) 上直接下载，一般下载 `clash-linux-amd64-v3-vxx.gz` 的包，可以自行下载然后再上传到服务器

```shell
mkdir .clash  # 将下载的包放到这个目录下
gunzip clash-linux-amd64-v3-vxx.gz
mv clash-linux-amd64-v3-vxx.gz clash  # 重命名一下
```

## 1.1-配置服务

这里还需要两个文件，一个 `config.yaml` 是我们订阅地址下载的文件，可以使用 `wget` 命令直接下载，另一个是 `Country.db` ，这个文件在运行服务后也会自己下载，如果有问题的话可以从本地 `Clash` 的订阅里找到这个文件上传上去，都放到之前新建的 `clash` 文件夹下

```shell
# 通过命令直接下载订阅地址的配置文件
wget -O ./config.yaml 订阅地址

# 运行服务（建议先不运行，先创建为系统服务后直接启动服务）
./clash -d .
```

如果运行服务后出现端口报错，可以在 `config.yaml` 文件里进行更改。

此外，为了方便使用，我们创建一个 `systemctl` 服务进程来维护 `clash` 

```shell
# 如果没有以下文件夹，就使用命令自行创建
mkdir -p ~/.config/systemd/user
```

目录创建好了之后，开始写配置文件

```shell
vim ~/.config/systemd/user/clash.service
```

其内容如下，**具体目录** 需根据实际情况来更改

```
[Unit]
Description=Clash Daemon

[Service]
ExecStart=/home/ubuntu/.clash/clash -d /home/ubuntu/.clash

[Install]
WantedBy=default.target
```

使用以下命令来管理服务（可以先不启动，装好面板再启动查看）

```shell
systemctl --user enable clash.service # 开机自启
systemctl --user start clash.service # 启动
systemctl --user stop clash.service # 停止
systemctl --user status clash.service # 查看服务状态
```

## 1.2-可视化界面查看

📌 可以使用可视化界面来查看信息，首先进入到 `clash` 所在的文件目录，将 `clash-dashboard` 克隆到本地的 `ui` 文件夹里 😁（推荐后面一个面板）

```shell
git clone -b gh-pages https://github.com/Dreamacro/clash-dashboard ui
```

克隆完成后，编辑 `config.yaml` 文件，进行配置（端口可改）

```text
external-controller: 0.0.0.0:9090  # 记得打开对应的端口
external-ui: ./ui  # 指定了面板ui的位置
```

之后访问 `ip:9090/ui/` 即可

---

🪟 除了以上面板，这里也可以使用以下面板，更改对应的 `external-ui` 即可

```shell
wget https://github.com/haishanh/yacd/archive/gh-pages.zip
unzip gh-pages.zip
mv yacd-gh-pages/ ui/
```

## 1.3-系统级服务

🌟 以上配置的服务，如果是在服务器上，当通过 `ssh` 用该用户登录时，服务才会启动，用户退出后，服务就终止了。为了保证该服务能够随服务器一直运行，可以更改为系统级服务，过程和上面大致类似，将 `clash` 和 `config.yaml` 、`ui` 放到 `/opt/clash` 目录下，然后配置服务项：

```shell
vim /etc/systemd/system/clash.service
```

配置文件内容如下：

```shell
[Unit]
Description=Clash-Fedora
[Service]
Type=simple
ExecStart=/opt/clash/clash -f /opt/clash/config.yaml
[Install]
WantedBy=default.target
```

> 🚨 **坑**：这里可能系统启用了SELinux或AppArmor安全机制，导致服务无法启动，解决办法：
> 1. 关闭SELinux安全机制：`setenforce 0` (恢复安全机制：`setenforce 1`)
> 2. 设置安全上下文：`chcon -t bin_t /opt/clash/clash`

^f7190c

剩下就是启动服务，查看状态，开机启动

```shell
systemctl start clash
systemctl status clash
systemctl enable clash
```

# 2-docker部署

参考这个[YouTube](https://www.youtube.com/watch?v=WQY5QIVYT0s)视频部署即可

# 3-Proxychains

> 这部分使用的下来的感受是不推荐，仅记录还有一种方法

`Proxychains` 用来重定向网络连接，在需要走代理的命令前加上 `proxychains` 就可以强制通过代理服务来访问网络

```shell
# 可能后面有更新的版本，需要根据实际情况再查资料
sudo apt install proxychains4
```

安装完成后进行配置

```shell
# 创建配置目录
mkdir ~/.proxychains

# 写配置文件
vim ~/.proxychains/proxychains.conf

# 配置文件的内容如下，根据 config.yaml 来进行更改
[ProxyList] socks5 127.0.0.1 7891
#http 127.0.0.1 7890
#https 127.0.0.1 7890
```

~~配置完成后测试能够正常使用魔法~~ 魔法使用规则模式，检测出来仍然是本地IP

```shell
proxychains curl cip.cc
```

之后在需要使用的代理的命令前加上 `proxychains` 即可。

# 4-命令代理

通过设置命令，直接走代理服务，就可以不用 `proxychains` ，使用这个的时候，在执行 `omz update` 时我遇到有问题，在 `.zshrc` 中添加如下代码，之后 `source .zshrc` 即可

```shell
alias setproxy="export http_proxy=http://127.0.0.1:7890;export https_proxy=http://127.0.0.1:7890"
alias unsetproxy="unset http_proxy;unset https_proxy;"
```

# 5-脚本代理（最推荐）

就用两个脚本即可，`.proxyrc`

```shell
#!/usr/bin/zsh
export ALL_PROXY="http://xxxxx:7890"
```

`.unproxyrc`

```shell
#!/usr/bin/zsh
export ALL_PROXY=""
```

使用时用 `source` 启用对应文件即可
