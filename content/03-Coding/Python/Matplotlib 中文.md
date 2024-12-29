> Linux 下使用 `matplotlib` 来画图是，如果有中文，总是会有警告，并且画出的图中，中文显示为乱码的方框

# 0-安装字体

网络上直接搜 `SimHei.ttf` 就能找到很多下载资源，这里我就默认已经下载到家目录下，然后执行如下命令：

```shell
sudo su  # 切换为 root
cd /usr/share/fonts
mkdir my_fonts
cd my_fonts
mv ~/SimHei.ttf ./
mkfontscale
mkfontdir
fc-cache
```

执行完以上命令后，在对应的文件夹下会多出 `fonts.dir` 和 `fonts.scale` 这两个文件，就说明已经安装成功了，可以使用命令查看

```shell
fc-list | grep SimHei
```

# 1-设置字体

设置字体很简单，但是设置之前，建议先删除 `matplotlib` 的缓存，路径在 `~/.cache/matplotlib` ，缓存删除之后，在画图之前通过以下代码设置字体即可：

```python
plt.rcParams['font.family']="SimHei"
```
