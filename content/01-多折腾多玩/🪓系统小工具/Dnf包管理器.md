# 0-换源

参考 [清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/help/fedora/) 操作即可

# 1-基本命令

```shell
# 查看 dnf 版本
dnf –-version

# 更新本地缓存，使用所选择的软件源镜像
sudo dnf makecache

# 显示系统中可用的 DNF 软件库
dnf repolist

# 显示系统中可用和不可用的所有的 DNF 软件库
dnf repolist all

# 列出用户系统上的所有来自软件库的可用软件包和所有已经安装在系统上的软件包
dnf list

# 列出所有安装了的 RPM 包
dnf list installed

# 列出所有可供安装的 RPM 包
dnf list available

# 搜索软件库中的 RPM 包
dnf search pack_name

# 查找某一文件的提供者
dnf provides pack_name

# 查看软件包详情
dnf info pack_name

# 安装软件包
dnf install pack_name

# 升级软件包
dnf update pack_name

# 检查系统软件包的更新
dnf check-update

# 升级所有软件包
dnf update 或 dnf upgrade

# 删除软件包
dnf remove pack_name 或 dnf erase pack_name

# 删除无用孤立的软件包
dnf autoremove

# 删除缓存的无用软件包
dnf clean all

# 获取有关某条命令的使用帮助（cmd是我们想查的命令）
dnf help cmd

# 查看所有的 DNF 命令及其用途
dnf help

# 查看 DNF 命令的执行历史
dnf history

# 查看所有的软件包组
dnf grouplist

# 安装一个软件包组("Educational Software"是一个软件包组)
dnf groupinstall 'Educational Software'

# 升级一个软件包组中的软件包
dnf groupupdate 'Educational Software'

# 删除一个软件包组
dnf groupremove 'Educational Software'

# 从特定的软件包库安装特定的软件(epel是我们指定的软件包库)
dnf –enablerepo=epel install pack_name

# 更新软件包到最新的稳定发行版
dnf distro-sync

# 重新安装特定软件包
dnf reinstall pack_name

# 回滚某个特定软件的版本
dnf downgrade pack_name
```
