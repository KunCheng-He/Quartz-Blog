> [!quote] 参考资料
> [ArchLinux-安装指南](https://wiki.archlinuxcn.org/wiki/%E5%AE%89%E8%A3%85%E6%8C%87%E5%8D%97) [ArchLinux-后续选配](https://wiki.archlinuxcn.org/wiki/%E5%BB%BA%E8%AE%AE%E9%98%85%E8%AF%BB) [安装的视频教程](https://www.bilibili.com/video/BV1Nt42137e3)

# 0 系统安装前的准备

## 0.0 镜像下载

可以在[ArchLinux官网](https://archlinux.org/download/)进行下载

![[Pasted image 20240503111346.png]]

之后正常制作启动盘，最后安装之前希望设备有**网络连接**

## 0.1 网络设置

显示当前网络设备

```shell
ip link
```

使用的是有线网络连接比较简单，如果使用的是 `WIFI`，则需要执行以下命令进行连接：

```shell
iwctl
[iwd] device list
[iwd] station <device> scan
[iwd] station <device> get_networks
[iwd] station <device> connect <network-name>
[iwd] exit
```

最后检查网络连接是否正常

```shell
ping archlinux.org
```

## 0.2 更新系统时间

正常情况下网络连接后，系统时间将自动同步。执行以下命令可以确保系统时间会自动通过网络从时间服务器同步

```shell
timedatectl set-ntp true
```

## 0.3 硬盘相关操作

查看当前硬盘设备

```shell
fdisk -l
```

结果中以 `rom`、`loop` 或者 `airootfs` 结尾的设备可以被忽略。结果中以 `rpbm`、`boot0` 或者 `boot1` 结尾的 `mmcblk*` 设备也可以被忽略。

查看到硬盘设备后，使用 `cfdisk` 工具进行硬盘的分区

```shell
cfdisk <disk-name>
```

> 🌟 *选择分区表的类型*：1. DOS：传统的MBR（主引导记录）分区表类型，支持最多4个主分区。2. GPT：GUID分区表，支持更大的硬盘容量和更多的分区，适用于UEFI启动。3. Sun：适用于Solaris操作系统的分区表类型。4. Mac：适用于Mac OS X的分区表类型。

这个工具比较直观，主要用到的有 `New`，用于*新建分区*，`Type` 用户更改分区的文件类型，`Write` 用于写入新的分区表，`Quit` 在以上操作执行完成后退出。退出后可以重新执行 `ffisk -l` 查看分区是否成功。

**分区完成**之后可以**格式化分区**

```shell
# 格式化 Boot 分区（-n 起名字）
mkfs.fat -F 32 -n ARCHBOOT <efi_boot>

# SWAP 分区
mkswap <swap_partition>

# Root 分区（-L 起名字）
mkfs.ext4 -L ARCHROOT <root_partition>

# Home 分区
mkfs.ext4 -L ARCHHOME <home_partition>
```

最后**挂载各个分区**

```shell
# 挂载 / 分区
mount <root_partition> /mnt

# 挂载 /home 目录
mount --mkdir <home_partition> /mnt/home

# 挂载 EFI 分区
mount --mkdir <efi_boot> /mnt/boot

# 挂载交换分区
swapon <swap_partition>
```

# 1 安装ArchLinux

## 1.0 选择镜像站

正常情况下，系统在连接到互联网后，`reflector` 会通过选择20个最新同步的HTTPS镜像站并按下载速率对其进行排序来更新镜像列表。可以**检查** `/etc/pacman.d/mirrorlist` 文件，看看镜像站是否合适

```shell
cat /etc/pacman.d/mirrorlist
```

我习惯性的配置**清华源**，在该文件顶部添加清华源

```shell
vim /etc/pacman.d/mirrorlist

# 添加清华源
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch

# 更改好镜像源后更新软件包的缓存
pacman -Syyu
```

## 1.1 安装必须的软件包

- `base`：是一个软件包组，包含了Arch Linux系统的基本组件，例如Shell工具、核心库等。安装`base`软件包组是建立一个基本的Arch Linux系统所必需的。
- `linux`：是Linux内核软件包，是系统的核心组件之一。安装Linux内核软件包是确保系统正常运行的关键。
- `linux-firmware`：是包含设备固件文件的软件包，用于支持硬件设备的正常运行。

```shell
pacstrap -K /mnt base linux linux-firmware networkmanager vim man-db man-pages texinfo
```

# 2 基本系统配置

## 2.0 生成fstab

`fstab` 文件可用于定义磁盘分区，各种其他块设备或远程文件系统应如何装入文件系统。(**建议检查一下生成后的 fstab 文件**)

```shell
genfstab -U /mnt >> /mnt/etc/fstab
```

## 2.1 chroot到新系统

📌注意命令使用的是 `arch-chroot`

```shell
arch-chroot /mnt
```

## 2.2 设置时区

```shell
# 设置时区
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

# 生成 /etc/adjtime
hwclock --systohc
```

## 2.3 区域和本地化设置

程序和库如果需要本地化文本，都依赖区域设置，后者明确规定了地域、货币、时区日期的格式、字符排列方式和其他本地化标准。需要设置这两个文件：`locale.gen` 与 `locale.conf`。

🚩**首先**，编辑 `/etc/locale.gen`，然后取消掉 `en_US.UTF-8 UTF-8` 前的注释符号

接着执行 `locale-gen` 以生成 locale 信息：

```shell
locale-gen
```

然后创建 `locale.conf` 文件，并编辑设定 `LANG` 变量

```shell
vim /etc/locale.conf

LANG=en_US.UTF-8
```

> [!important] 为什么不设置中文
> ❌ 并不推荐在此设置任何中文 locale，这可能会导致 tty 上中文显示为方块。如果您不经常使用 tty ，或是稍后需要安装桌面环境，则在不使用 tty 后可以设置为中文的 locale 。

## 2.4 root密码与主机名称

```shell
# 设置 root 密码
passwd

# 设置主机名称
vim /etc/hostname
<随便设置名称即可>
```

## 2.5 网络服务

开机自动启动网络管理服务，后续如果有需要，可以方便的进行配置

```shell
systemctl enable NetworkManager.service
```

## 2.6 CPU微码

*微码*是处理器内部的指令集，它们用于控制处理器的各种功能和行为。微码更新通常用于修复处理器的漏洞、提高性能、增强稳定性和兼容性。

```shell
# 选择自己对应的 CPU
pacman -S intel-ucode
pacman -S amd-ucode
```

## 2.7 安装引导程序

```shell
pacman -S grub efibootmgr

# 多系统的话还需要安装
pacman -S os-prober ntfs-3g

grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB

# 多系统需要
vim /etc/default/grub
# 去除 GRUB_DISABLE_OS_PROBER=false 的注释

grub-mkconfig -o /boot/grub/grub.cfg
```

## 2.8 重启计算机

```shell
# 退出 chroot 环境
exit

# 手动卸载被挂载的分区
umount -R /mnt

# 关机
shutdown now
```

**之后拔出U盘以后再启动电脑**，见到以下界面安装成功

![[Pasted image 20240503150000.png]]

# 3 Arch后续选配

## 3.0 网络设置

一切选配的前提，先确保网络是正确的，可以执行以下命令，方便的连接 `wifi`

```shell
nmtui

# 确认网络的可用性
ping archlinux.org
```

## 3.1 显卡驱动

暂略

## 3.2 新建普通用户

我们使新用户默认使用 `zsh`，所以需要提前执行 `pacman -S zsh` 进行安装，然后再执行以下命令添加新用户

```shell
# 安装 sudo
pacman -S sudo

# 创建用户
useradd -m -G wheel -s /bin/zsh <new_user_name>

# 设置密码
passwd <new_user_name>

# 编辑 sudo 文件
EDITOR=vim visudo
# 去掉 %wheel ALL=(ALL:ALL)ALL 的注释
```

## 3.3 CN源与AUR

> 📌 建议重这里开始，先解决**魔法**，参考[[🪄Server·魔法]]，这样就不需要 `CN源` 了，直接安装 `yay` 即可。

先配置*CN源*，执行命令 `sudo vim /etc/pacman.conf` 进行编辑，在文件末尾写入以下内容：

```shell
[archlinuxcn]
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
```

之后通过以下命令安装 `archlinuxcn-keyring` 包导入 `GPG key`❌

```shell
sudo pacman -Sy archlinuxcn-keyring
```

---
📌 **以上错误解决办法**
2023 年 12 月后，在新系统下安装 `archlinuxcn-keyring` 时可能会出现错误：
```
error: archlinuxcn-keyring: Signature from "Jiachen YANG (Arch Linux Packager Signing Key) " is marginal trust
```

需要在本地信任 farseerfc 的 GPG key，然后再重新安装
```shell
pacman-key --lsign-key "farseerfc@archlinux.org"
```

---

之后安装 `yay` 使用 `AUR`

```shell
pacman -S yay
```

## 3.4 openssh服务

```shell
sudo yay -S openssh

# 开启服务
sudo systemctl start sshd.service
sudo systemctl enable sshd.service

# 查看服务状态
systemctl status sshd.service
```

## 3.5 oh-my-zsh

参考[[😍Oh-My-Zsh]]

## 3.6 Gnome桌面

参考[[👣Gnome桌面]]
