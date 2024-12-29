> [!quote] 相关资料
> [Scoop官方网站](https://scoop.sh)  [安装与介绍视频](https://www.bilibili.com/video/BV188411n7Ha)

# 0 安装与网络

打开 `window11` 的终端开始执行以下命令即可，如果遇到网络问题，请打开**系统代理**即可
```shell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
Invoke-RestMethod -Uri https://get.scoop.sh | Invoke-Expression
```

为了保证网络通畅，可以给 `scoop` 设置代理
```shell
scoop config proxy 127.0.0.1:7890
```

# 1 常用命令

```shell
# 安装与卸载软件
scoop install <PackageName>
scoop uninstall <PackageName>

# scoop 更新
scoop update

# bucket相当于仓库，可以有很多不同的人打包的仓库
# bucket相关命令
scoop bucket list  # 本地有的
scoop bucket known  # 查看官方有哪些bucket
scoop bucket add <BucketName>  # 添加新的bucket

# 查看所有安装的软件
scoop list
```

# 2 推荐软件

这部分仅列举我个人习惯使用 `scoop` 安装管理的软件
> `7zip` `git` `miniconda3` `corretto17-jdk` `corretto8-jdk`
