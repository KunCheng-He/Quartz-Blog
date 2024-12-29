> [!NOTE] Qwen1.5-通意千问
> [Qwen1.5](https://github.com/QwenLM/Qwen1.5)是阿里云千问团队开发的大语言模型，是 `Qwen` 系列的改进版本。阿里云只开源了模型参数，网页交互可以使用[[📃Text-Generation-WebUI]]，具体参考[WebUI部署文档](https://qwen.readthedocs.io/zh-cn/latest/web_ui/text_generation_webui.html)

# 0 安装

📌[Qwen1.5-14B-Chat](https://huggingface.co/Qwen/Qwen1.5-14B-Chat)模型需要的显存大小超过 `24GB`，所以显存大小不足则只能跑[Qwen1.5-7B-Chat](https://huggingface.co/Qwen/Qwen1.5-7B-Chat)

🌟[Qwen1.5](https://github.com/QwenLM/Qwen1.5)使用[[📃Text-Generation-WebUI]]进行 `Web` 交互，所以在部署好 `Text-Generation-WebUI` 后，将模型参数放入该项目 `models` 文件夹下即可

```shell
cd models
# 大文件clone不下来就手动上传进行覆盖
git clone https://huggingface.co/Qwen/Qwen1.5-7B-Chat
```

最后回到 `Text-Generation-WebUI` 项目主目录下启动即可

```shell
python server.py --model Qwen1.5-7B-Chat --listen --listen-port 8080
```
