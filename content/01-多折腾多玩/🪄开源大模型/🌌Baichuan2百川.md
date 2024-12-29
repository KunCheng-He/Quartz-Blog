> [!NOTE] BaiChuan2-百川大模型
> [BaiChuan2](https://github.com/baichuan-inc/Baichuan2)是百川智能推出的**新一代开源大语言模型**，采用 **2.6 万亿** Tokens 的高质量语料训练。该开源项目自带 `WebUI` 的交互页面，同时也支持 [[📃Text-Generation-WebUI]]

# 0 安装

克隆该项目仓库，安装环境，然后即可启动

```shell
# 克隆仓库
git clone https://github.com/baichuan-inc/Baichuan2.git
cd Baichuan2

# 安装项目依赖
conda create -n Baichuan2 python=3.11
conda activate Baichuan2
pip install -r requirements.txt

# 启动项目，会自动下载网络模型，所以需要保证网络通畅
streamlit run --server.address 0.0.0.0 --server.port 8080 web_demo.py
```

使用 `Text-Generation-WebUI` 进行部署话，可以在 [BaiChuan - Hugging Face](https://huggingface.co/baichuan-inc) 处下载模型的参数文件
