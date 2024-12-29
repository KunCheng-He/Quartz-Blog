> [!NOTE] OpenSoraPlan 开源Sora方案
> [OpenSoraPlan](https://github.com/PKU-YuanGroup/Open-Sora-Plan)是由北大-兔展AIGC联合实验室共同发起的开源项目，本项目希望通过开源社区的力量复现 `Sora`。

# 0 安装

首先克隆该项目，然后安装项目所需环境

```shell
# 克隆项目仓库
git clone https://github.com/PKU-YuanGroup/Open-Sora-Plan
cd Open-Sora-Plan

# 安装项目所需环境
conda create -n opensora python=3.8 -y
conda activate opensora
pip install -e .
```

以上执行完成后，修改启动文件 `opensora/serve/gradio_web_server.py` 最后一行，指定 `host` 和 `port`（如不需要，可以不改）

```python
demo.launch(server_name='0.0.0.0', server_port=8080)
```

以上完成后即可启动项目

```shell
python -m opensora.serve.gradio_web_server
```
