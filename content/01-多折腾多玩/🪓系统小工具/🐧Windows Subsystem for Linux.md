> [!quote] WSL
> 适用于 Linux 的 Windows 子系统 (WSL) 是 Windows 的一项功能，可用于在 Windows 计算机上运行 Linux 环境，而无需单独的虚拟机或双引导。 WSL 旨在为希望同时使用 Windows 和 Linux 的开发人员提供无缝高效的体验。[官方文档](https://learn.microsoft.com/zh-cn/windows/wsl/)  **WSL还是有问题，谨慎**

# 0 WSL 基本命令

```shell
# 安装wsl，默认会安装ubuntu，使用 -d 参数可以指定发行版
# 也可以使用参数设置默认不安装发行版
wsl --install
wsl --install -d <发行版名称>
wsl --install --no-distribution

# 卸载Linux发行版
wsl --unregister <发行版名称>

# 更改发行版的默认用户
<发行版名称> config --default-user <username>

# 查看可在线获得的发行版
wsl --list --online
wsl -l -o

# 列出已经安装的发行版
wsl --list --verbose

# 设置默认的Linux发行版
wsl --set-default <发行版名称>

# 更新wsl，检查wsl状态，检查wsl版本，关闭wsl
wsl --update
wsl --status
wsl --version
wsl --shutdown
```

# 1 ArchLinux

> 商店里有一个非官方的ArchLinux，据说还存在一些问题，这里使用另一个[ArchWSL](https://github.com/yuk7/ArchWSL)项目（🚫该版本无法安装docker）

该项目里提供了三种安装方式，在 `release` 页面下载 `Arch.zip` 文件后，将解压文件提取到拥有写权限的文件夹内，我习惯在 `Document` 目录下创建一个同名的文件夹，之后双击运行可执行程序两次即可，第一次是创建磁盘映像，第二次则初始化 `keyring`。完成之后进行如下基本操作
```shell
# 设置root密码
passwd

# 添加日常用户并设置密码
echo "%wheel ALL=(ALL) ALL" > /etc/sudoers.d/wheel
useradd -m -G wheel -s /bin/bash <username>
passwd <username>
exit
```

重新启动 `Arch` 后，如果默认用户还是 `root` ，可以根据 `wsl` 的命令进行更改，之后就是进行系统的初始化、配置源等基本操作
```shell
sudo pacman-key --init
sudo pacman-key --populate
sudo pacman -Syy archlinux-keyring

# 后面配置镜像源参考清华源
# 地址：https://mirrors.tuna.tsinghua.edu.cn/help/archlinux/
```

# 2 WSL使用主机魔法

两个脚本，需要时使用 `source` 执行即可
```shell
# ~/.proxy 内容如下
#!/bin/bash
export ALL_PROXY="http://$(cat /etc/resolv.conf |grep -oP '(?<=nameserver\ ).*'):7890"

# ~/.unproxy
#!/bin/bash
export ALL_PROXY=""
```
