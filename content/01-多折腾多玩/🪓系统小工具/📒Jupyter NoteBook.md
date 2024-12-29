[Jupyter NoteBook](https://jupyter.org/) 部署在自己的服务器上，可以自由提供一个在线的 `Python` 代码运行环境，还是比较方便的。开始之前需要自行创建虚拟环境，并执行 `pip install jupyter` 安装库。

# 0-配置服务

可以先选择一个目录作为 `NoteBook` 的根目录，在对应的目录下执行命令，生成配置文件

```shell
> jupyter notebook --generate-config
Writing default config to: /home/ubuntu/.jupyter/jupyter_notebook_config.py
```

生成的配置文件的路径需要稍微记一下，然后就是生成密码的 `sha1` 值

```shell
$ python
Python 3.9.0 (default, Sep 21 2022, 16:48:59)
[GCC 7.5.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> from notebook.auth import passwd
>>> passwd()
Enter password:
Verify password:
'argon2:$argon2id$v=19$m=10240,t=10,p=8$kkn'
```

记住我们设置的密码，这是我们登录 `jupyter` 时的密码，其中程序生成的对应的 `sha1` 值需要**复制**一下，然后进入配置文件进行修改

```shell
# 这是刚才配置文件的路径
vim /home/ubuntu/.jupyter/jupyter_notebook_config.py

# 修改配置文件里的如下内容
--------------------------------------------------------
# 所有 ip 均可访问
c.NotebookApp.ip='0.0.0.0'

# 这里设置的值为刚刚程序生成的值，登陆时输入我们自己设置的密码
c.NotebookApp.password = u'刚刚程序生成的 sha1 值'

# 启动不自动打开浏览器
c.NotebookApp.open_browser = False

# 端口可改可不改
c.NotebookApp.port =8888
```

# 1-开启服务

首先需在服务器上开启我们设置的对应端口，这样外网才可以访问，使用 `nohup` 命令将任务挂在后台即可

```shell
nohup jupyter notebook &
```