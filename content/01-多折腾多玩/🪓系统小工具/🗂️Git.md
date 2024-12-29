+ Git的三个分区
	+ 工作区 .git所在目录
	+ 暂存区 .git/index
	+ 本地仓库 .git/objects

---

# 1 基本必备的配置

```shell
# 初始化一个仓库
git init

# 查看暂存区内容
git ls-files

# 查看状态
git status

# 基本信息配置
git config --global user.name ""
git config --global user.email ""

# 查看配置信息
git config --global --list

# 临时代理配置
git clone https://xxxxxxxx.git --config "http.proxy=proxyHost:proxyPort"

# 给 Github 单独配置代理
git config --global http.https://github.com.proxy http://host:port
git config --global https.https://github.com.proxy http://host:port
```

# 2 回退版本

|         命令          | 工作区 | 暂存区 |
| :-----------------: | :-: | :-: |
| `git reset --soft`  |  ✅  |  ✅  |
| `git reset --hard`  |  ❌  |  ❌  |
| `git reset --mixed` |  ✅  |  ❌  |

# 3 分支的管理

```shell
# 查看现有分支
git branch

# 创建新的分支
git branch [branch name]

# 切换分支
git checkout [branch name]

# 将本地的当前分支推送到远程的某个分支
git push origin [branch name]

# 删除本地或远程的分支(: 表示删除远程)
git branch -d [branch name]
git push origin :[branch name]

# 合并本地分支到主分支
git merge [branch name]
```

# 4 删除文件

```shell
# 移除已经追踪的文件
git rm -f --cached [file name]
```
