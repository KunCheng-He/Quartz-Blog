# 参考资料

[B站视频教程](https://www.bilibili.com/video/BV1HN411X7Lj) [使用 nmcli 命令行工具配置静态 IP 地址](https://cloud.tencent.com/developer/article/1876071)

# 具体步骤

> 📌 以下步骤中，[网络共享无法指定虚拟交换机的话，安装一个VirtualBox应该就可以了](https://zhuanlan.zhihu.com/p/670556225)，可以只使用最后的 `nmtui` 来配置静态 `ip` 

> ⚠️⚠️⚠️ 按照以下步骤配置完成，虚拟机是可以访问外网的，但是：**重启之后，虚拟机就没有权限访问网络了，IP确实是固定了**，害，无大语，放弃。。。`备注：` VirtualBox v7.0.13 版本吧，安装完虚拟机之后，重启后在启动虚拟机，会报错，有 dll 文件冲突（我使用超级管理员权限无法删除该 dll 库，换 Vmware 再试试）

![[Pasted image 20231205130408.png]]

创建完成虚拟交换机后，打开 `控制面板` 里对网络进行设置

![[Pasted image 20231205130740.png]]

先开启我们物理网卡的网络共享，我这里就是我的 `WLAN`，然后再设置虚拟交换机的IP网段

![[Pasted image 20231205131249.png]]

以上图片设置网络共享中，没有显示 `家庭网络连接`，这里补充一张图展示正常情况下的样子，解决办法见 `本节开头部分`

![[Pasted image 20231205182815.png]]

然后回到 `Hyper-V` 管理器里，将对应虚拟机的网络改为新建的虚拟交换机

![[Pasted image 20231205131657.png]]

**以上基本的设置完成后，就可以启动虚拟机，连接进入虚拟机后，配置静态IP即可，可参考以上的文档资料，以下是进行可视化配置需要的命令**

```shell
sudo dnf install NetworkManager-tui
sudo nmtui
```
