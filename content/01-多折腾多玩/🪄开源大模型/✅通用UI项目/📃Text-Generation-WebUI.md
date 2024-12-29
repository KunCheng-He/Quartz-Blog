> [!NOTE] Text-Generation-WebUI
> [Text-Generation-WebUI](https://github.com/oobabooga/text-generation-webui)是适用于大模型的 `Gradio WebUI`，目标是成为文本生成领域的 `StableDiffusion`

# 0 安装过程

首先将仓库代码克隆下来：
```shell
git clone https://github.com/oobabooga/text-generation-webui.git
cd text-generation-webui
```

之后安装项目运行所需的环境：
```shell
conda create -n text-generation-webui python=3.11
conda activate text-generation-webui
pip install -r requirements.txt
```

安装完成之后就可以启动成功了，这里通过 `--listen` 参数将启动的地址设置为 `0.0.0.0`，通过 `--listen-port` 指定了启动的端口，如不需要进行特定设置，则直接启动即可
```shell
python server.py --listen --listen-port 8080
```

# 1 使用说明

📌 该项目支持多个开源大模型，只要涉及到的文件夹有两个： `models` 和 `loras`
- `models` ：存放模型参数的文件夹，通常模型参数文件需要从[Hugging Face](https://huggingface.co/models?pipeline_tag=text-generation&sort=downloads)进行下载
- `loras` ：存放使用自有数据重新训练的 `loRA` 文件

将模型参数相关文件放入对应的文件夹后，即可正式启动该项目进行聊天对话，如果有多个模型，启动时可以通过 `--model` 参数进行指定：
```shell
# 启动项目前请确保项目的环境正确
conda activate text-generation-webui

# 指定模型启动(将具体的模型名称放入以下 model_name 的位置)
python server.py --model [model_name] --listen --listen-port 8080
```
