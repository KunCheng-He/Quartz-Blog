> [!tip] 相关资料
> [Quartz - Github](https://github.com/jackyzha0/quartz) [Quartz - 文档](https://quartz.jzhao.xyz/)

# 1. 背景

`obsidian` 很好用，我想外网访问，然后我就了解到了 `Quartz` ，所以开始盘它。没有对比过其他方案，但是我尝试后我感觉是满足我需求的。

# 2. 基本部署

参考 [Quartz 自己的官方文档](https://quartz.jzhao.xyz/) 即可，主要步骤如下：

```shell
1. Fork 仓库：https://github.com/jackyzha0/quartz
2. 使用 Git 克隆自己的仓库
3. 安装依赖（我克隆的仓库版本是 v4.4.0，推荐使用 node -> 22.12.0）
(修改 .node-version 中的内容为：v22.12.0)
npm i
npx quartz create

4. 布局什么的可以都先不用管，把知识库整个目录复制到 content 中即可
5. 执行命令本地预览效果
npx quartz build --serve

6. 没有问题，推送到仓库
7. 使用 `CloudFlare` 部署该仓库，并发布，如下图，部署时设置几个参数如下：
Production branch: V4
Framework preset: None
Build command: npx quartz build
Build output directory: public
```

![[Pasted image 20241230191919.png]]

# 3. 自定义域名

自行购买，我使用的是 [GoDaddy](https://www.godaddy.com/)

为站点配置自己的域名
![[Pasted image 20241230203448.png]]

其他域名服务商的域名需要 `Cloudflare` 来进行解析，系统会提示进行 **DNS 转移** ，前面的步骤根据提示操作即可，直到这里，获取到这两个 `DNS` 服务器的地址之后
![[Pasted image 20241230211509.png]]

回到域名购买商这里对**域名服务器**进行设置
![[Pasted image 20241230211248.png]]

设置生效后在 `Cloudflare` 上就可以看到该域名已经处于活动状态了，然后建议进行**速度优化**，白给的功能，建议都打开
![[Pasted image 20241230220014.png]]

最后回到项目激活该域名即可。**我还在项目的 `quartz.config.ts` 中配置了 `baseUrl` 为本域名**。