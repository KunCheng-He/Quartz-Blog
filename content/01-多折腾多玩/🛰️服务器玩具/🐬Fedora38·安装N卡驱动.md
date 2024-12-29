# 0-准备工作

+ 🎞️**驱动下载**
	+ 进入 [Nvidia官网](https://www.nvidia.cn/Download/index.aspx?lang=cn) 选择自己对应的显卡型号和系统直接下载即可
+ 🖼️**关闭图形界面**
	+ 如果安装的是服务器版本的系统，没有装桌面，可以直接跳过这一步。具体的操作命令如下：

```shell
# 查看默认模式
# 输出 multi-user.target 说明默认就是终端界面
systemctl get-default

# 停止图形化界面服务
service graphical.target stop

# 修改命令行模式为默认
systemctl set-default multi-user.target
```

+ 🎭**禁用nouveau驱动**
	+ 这是开源驱动，不是官方驱动。有的系统会开机自启，所以将 `nouveau` 加入黑名单；有的会将其启动提前到系统初始化阶段，这个需要修改 `grub` 文件，然后重新生成引导文件。具体操作如下：

```shell
# 加入黑名单
# 在 /etc/modprobe.d 文件夹中新建 nouveau-blacklist.conf 文件
sudo vim /etc/modprobe.d/nouveau-blacklist.conf
# 写入：
blacklist nouveau 
options nouveau modeset=0

# 修改引导文件
# 在 /etc/default/grub 文件的 GRUB_CMDLINE_LINUX 参数中添加如下参数：
sudo vim /etc/default/grub
# 各个参数之间通过空格隔开即可
rd.driver.blacklist=nouveau

# 重新生成引导文件
grub2-mkconfig -o /boot/grub2/grub.cfg

# 完成上述操作后需要重启
reboot

# 重启之后可以检测一下是否真的禁用了
# 没有输出就是禁用成功了
lsmod | grep nouveau
```

# 1-驱动安装

安装驱动的过程，就是将源文件拿过来，重新编译一下，所以需要安装一下编译工具

```shell
sudo dnf install binutils make gcc
```

还有一个条件，**kernel-headers和kernel-devel两个包的版本要与内核版本一致**，查看命令如下，哪一个版本不对就重新安装哪一个包即可

```shell
# 查看内核版本
uname -r

# 查看 kernel-headers 和 kernel-devel 的版本
rpm -q kernel-headers kernel-devel
```

**上述问题解决后**，找到刚才下载的驱动文件，切换 `root` 用户安装即可

```shell
sudo su
sh NVIDIA-Linux-x86_64-xxx.xxx.xx.run

# 安装过程一路 yes 即可
# 如果安装完成需要改回默认图形界面，则需要执行：
systemctl set-default graphical.target

# 查看显卡状态
nvidia-smi
```
