非常强大的一个 `shell` 工具，用过之后就回不去了，详细内容可看 [官方网站](https://ohmyz.sh/)

# 0-安装

安装 `oh-my-zsh` 之前，需要先确保你的 `Linux` 系统 **已经安装** 好了 `zsh` ，这一步完成之后，下载官方的安装脚本，进行自动安装

```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

以上步骤需要魔法，如果没有魔法，可参考 [[🪄Server·魔法]] 进行配置。

# 1-插件与配置

`oh-my-zsh` 可以根据需求安装许多强大的插件，其插件目录为 `.oh-my-zsh/custom/plugins` ，通过命令进入该目录，就可使用 `git clone` 来下载插件了

```shell
# 先进入目录
cd ~/.oh-my-zsh/custom/plugins

# zsh-autosuggestions 根据历史记录提供命令建议
git clone https://github.com/zsh-users/zsh-autosuggestions.git

# zsh-syntax-highligting 提供语法高亮
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
```

插件下载完成后还需要进行配置，其配置文件在 `~/.zshrc` ，我常用的一些配置如下

```shell
# 修改主题
# bira jonathan
ZSH_THEME="fino"

# 启用插件
plugins=(
		git
		pip
		extract  # 使用 x 全部解压
		zsh-autosuggestions
		zsh-syntax-highlighting
)
```

配置完成后执行 `source .zshrc` 即可，若默认 `shell` 未更改，重启即可

# 2-更新

可手动更新

```shell
omz update
```

# 3-Powerlevel10k

该主题可以根据自己的喜好高度自定义，配置过程仅需要根据给出的样式进行选择即可。仓库地址：[Github/powerlevel10k](https://github.com/romkatv/powerlevel10k?tab=readme-ov-file#oh-my-zsh)，可直接参考仓库复制命令执行即可

```shell
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k

vim ~/.zshrc
ZSH_THEME="powerlevel10k/powerlevel10k"
```
