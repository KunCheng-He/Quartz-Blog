> [!NOTE] Chinese-LLaMA-Alpaca-3
> [Chinese-LLaMA-Alpaca-3](https://github.com/ymcui/Chinese-LLaMA-Alpaca)在 `Meta` 发布的原版 `LLaMA` 模型的基础上扩充了中文词表并使用中文数据进行了二次训练，进一步提升了中文基础语义理解能力。同时，中文`Alpaca` 模型进一步使用了中文指令数据进行精调，显著提升了模型对指令的理解和执行能力。另一方面，因为 `Meta` 发布的 `LLaMA` 模型禁止商用，并没有正式开源模型权重，需要进行申请，所以该项目仅发布了 `LoRA` 权重，可以理解为 `LLaMA` 的一个补丁，需要合并参数后使用。

# 0 前言

📌 使用[[📃Text-Generation-WebUI]]与 `LLaMA` 模型进行交互的话，可以下载原版 `LLaMA` 模型参数与 `LoRA` 权重，然后进行分别加载，**合并模型参数**并不是必选项。

# 1. 模型参数的合并与转换

参考文档：[官方Wiki](https://github.com/ymcui/Chinese-LLaMA-Alpaca/wiki/%E6%89%8B%E5%8A%A8%E6%A8%A1%E5%9E%8B%E5%90%88%E5%B9%B6%E4%B8%8E%E8%BD%AC%E6%8D%A2)  参考视频：[bilibili大学](https://www.bilibili.com/video/BV1xz4y18749)

## 1.1 权重参数下载与解压

首先需要下载原版Llama模型的权重参数：[LLaMA-7B](https://huggingface.co/nyanko7/LLaMA-7B/tree/main) ，[llama-13b](https://huggingface.co/huggyllama/llama-13b/tree/main)

现在第一次跑，我先只下载 `LLaMA-7B` 的权重参数，放到服务器下如下：

![[Pasted image 20240409153021.png]]

之后进入[Chinese-LLama-Aplaca](https://github.com/ymcui/Chinese-LLaMA-Alpaca)仓库下载微调过后的 `LLaMA` 和 `Alpaca`

![[Pasted image 20240409153355.png]]

手动下载压缩包然后上传到实例，此时文件所在目录为：`/root/sj-tmp/` ，我们在 `/root/llama` 路径下执行以下解压命令：

```shell
unzip /root/sj-tmp/chinese_llama_plus_lora_7b.zip -d ./chinese_llama_plus_lora_7b
unzip /root/sj-tmp/chinese_llama_plus_lora_7b.zip -d ./chinese_llama_plus_lora_7b
```

解压完成后目录内容如下图所示：

![[Pasted image 20240409154840.png]]

## 1.2 拉取仓库和安装环境

先拉取最新的仓库，我直接拉取到 `/root` 目录下：

```shell
git clone https://github.com/ymcui/Chinese-LLaMA-Alpaca.git
```

仓库拉取完成后创建需要的虚拟环境

```shell
conda create -n Chinese-LLaMA-Alpaca python==3.10
conda activate Chinese-LLaMA-Alpaca
cd Chinese-LLaMA-Alpaca/  # 进入拉取的仓库目录
pip install -r requirements.txt  # 安装依赖
```

下载转换脚本，该脚本可以将原版的 `LLaMA` 参数文件转换为 `huggingface` 格式。因仓库文件太多，所以我们单独下载转换脚本即可【[转换脚本下载地址](https://github.com/huggingface/transformers/blob/main/src/transformers/models/llama/convert_llama_weights_to_hf.py)】，下载完成后上传到服务器的 `/root` 目录下：

![[Pasted image 20240409164009.png]]

## 1.3 执行转换脚本

**执行转换脚本前，将`7B/tokenizer.model`文件放到`7B`同级目录下**

```shell
mv ./7B/tokenizer.model ./
```

![[Pasted image 20240409164702.png]]

`--input_dir` 指定的是原版参数所在的文件夹内，这里不用指定到 `7B` ，指定到 `/root/llama` 即可，`--model_size` 设置模型大小，这里指定 `7B`，剩余文件就能定位到 `/root/llama/7B` 文件夹内，`--output_dir` 存放转换好的 `HF版` 权重（⚡可以执行会出错，后续有问题解决办法，可以先执行解决办法防止报错⚡）

```shell
python convert_llama_weights_to_hf.py \
--input_dir /root/sj-tmp/llama \
--model_size 7B \
--output_dir /root/sj-tmp/llama/7B_hf
```

---

❌执行以上代码首先遇到了如下问题：

```txt
ImportError:
LlamaConverter requires the protobuf library but it was not found in your environment. Checkout the instructions on the
installation page of its repo: https://github.com/protocolbuffers/protobuf/tree/master/python#installation and follow the ones
that match your environment. Please note that you may need to restart your runtime after installation.
```

💡解决（我安装最新的版本会报错，提示安装该版本或更低的版本）：

```shell
pip install protobuf==3.20.0
```

---

转换需要一定时间，转换完成后输出文件夹内容如下：

![[Pasted image 20240409171631.png]]

## 1.4 合并LoRA权重

> 可以进行*单LoRA权重合并*，需要的话自行参考[官方Wiki](https://github.com/ymcui/Chinese-LLaMA-Alpaca/wiki/%E6%89%8B%E5%8A%A8%E6%A8%A1%E5%9E%8B%E5%90%88%E5%B9%B6%E4%B8%8E%E8%BD%AC%E6%8D%A2)，我这里进行的是*多LoRA权重合并*

参数说明：
- `--base_model`：存放HF格式的LLaMA模型权重和配置文件的目录
- `--lora_model`：中文LLaMA/Alpaca LoRA解压后文件所在目录
	- 📌**两个LoRA模型的顺序很重要，不能颠倒。先写LLaMA-Plus-LoRA然后写Alpaca-Plus/Pro-LoRA**
- `--output_type`：指定输出格式，可为`pth`或`huggingface`。若不指定，默认为`pth`，我们指定为 `huggingface`
- `--output_dir`：指定保存全量模型权重的目录，默认为`./`

开始合并权重，这里我们使用新脚本

```shell
cd /root/sj-tmp/Chinese-LLaMA-Alpaca/

python scripts/merge_llama_with_chinese_lora_low_mem.py \
--base_model /root/sj-tmp/llama/7B_hf \
--lora_model /root/sj-tmp/llama/chinese_llama_plus_lora_7b,/root/sj-tmp/llama/chinese_alpaca_pro_lora_7b \
--output_dir /root/sj-tmp/llama/7B_full_model \
--output_type huggingface
```

合并完成后新的目录结构如下：

![[Pasted image 20240415103058.png]]

# 2. 模型推理

## 2.1 Transformer原生推理

> [!tips] Transformer原生推理
> [教程链接](https://github.com/ymcui/Chinese-LLaMA-Alpaca/wiki/%E4%BD%BF%E7%94%A8Transformers%E6%8E%A8%E7%90%86) ，**仅用该方式做测试** ，官方要求：`CPU` 运行 `7B` 确保有 `32G` 内存，`GPU` 运行 `7B` 确保有 `20G` 显存（测试结果：显存使用 `14GB` 左右）

```shell
python scripts/inference/inference_hf.py \
--base_model /root/sj-tmp/llama/7B_full_model \
--with_prompt \
--interactive
```

效果如图：
![[Pasted image 20240415110545.png]]

> ❌ 如果提示 `TypeError: not String` ，引发该问题可能是终端输入汉字后，输入错误，使用退格键删除造成的异常

## 2.2 Text-Generation-WebUI

该项目的部署可参考[[📃Text-Generation-WebUI]]进行部署，这里仅在仓库克隆时重命名为 `llama-webui`，项目启动环境也重新创建了一个 `llama` 环境安装相关依赖。然后将我们合并好的参数模型直接放到该项目的 `models` 目录下，这里我将合并后的参数重命名为 `merged_chinese_alpaca_pro`

```shell
python server.py --model merged_chinese_alpaca_pro --listen --listen-port 8080
```
