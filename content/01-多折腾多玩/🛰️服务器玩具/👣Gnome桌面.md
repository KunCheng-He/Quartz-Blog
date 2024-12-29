> [!tip] [Gnome](https://www.gnome.org)
> `Gnome` 默认在 `Wayland` 上运行，传统的 `X` 程序通过 `Xwayland` 运行。

# 0 安装

安装 Gnome 有两个软件包可用：
- [gnome](https://archlinux.org/groups/x86_64/gnome/) 包含基本的桌面环境和一些集成良好的应用
- `gnome-extra` 包含其它 `GNOME` 应用，如邮件客户端、IRC客户端、GNOME Tweaks 和一些游戏。请注意，这个组建立在 `gnome` 包组之上

这里我们安装基本的桌面环境即可：

```shell
yay -S gnome
```

> 🌟**Tips**：删除一些我不想使用的软件
> yay -Rs epiphany gnome-calculator gnome-contacts gnome-maps gnome-music snapshot totem

# 1 启动Gnome

`GNOME` 可以使用显示管理器以图形方式启动，也可以从控制台手动启动（可能会缺少某些功能）。`gnome` 包组的显示管理器是[GDM](https://wiki.archlinuxcn.org/wiki/GDM "GDM")。

```shell
sudo systemctl enable gdm.service
sudo shutdown now
```

# 2 Gnome美化

美化之前需要安装响应的工具

```shell
sudo yay -S gnome-tweak-tool make
```

## 2.0 插件的安装

🌟`Gnome` 的插件安装配合浏览器网页与浏览器插件使用，打开[插件网站](https://extensions.gnome.org)，即可安装所需插件(⚡**不建议安装太多**，在 `Gnome` 升级时总是出现插件不兼容的情况)

- [>] `User Themes`：从用户目录加载 shell 主题
- [>] `Dash to Dock`：Gnome Shell 的 dock 栏（💡不行的话考虑禁用自带的dock，然后使用 `Plank`）
- [>] `Coverflow Alt-Tab`：替换 Alt-Tab，以覆盖流方式迭代窗口
- [>] `Fildem global menu`：Gnome 的全局菜单

## 2.1 美化包网站

> [!quote] 站点
> [😶‍🌫️主题](https://www.gnome-look.org/s/Gnome/browse?cat=135&ord=rating) [⚡图标](https://www.gnome-look.org/s/Gnome/browse?cat=132&ord=rating) [🔒登录界面](https://www.gnome-look.org/s/Gnome/browse?cat=131&ord=rating) [👐开机动画](https://www.gnome-look.org/s/Gnome/browse?cat=108&ord=rating)

- ["] 各个部分所在文件夹：
	- [>] *主题* - `~/.themes`
	- [>] *图标* - `~/.icons`
