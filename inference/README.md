# 推理与部署

## 创建安装环境
conda create -n tla python=3.9
pip install -r requirement

## 命令行交互形式
- python inference/inference_hf.py \
    --base_model path_to__Tibetan-Llama2_or_Tibetan-alpaca_dir \
    --with_prompt \
    --interactive

例：
- python inference/inference_hf.py \
     --base_model /home/rigthub/Tibetan-Llama2-Tibetan-Alpaca/tibetan-Llama2-7B \
     --with_prompt \
     --interactive

- python inference/inference_hf.py \
     --base_model /home/rigthub/Tibetan-Llama2-Tibetan-Alpaca/tibetan-Alpaca-7B \
     --with_prompt \
     --interactive


## web图形界面交互形式
- python inference/gradio_demo.py \
     --base_model path_to_Tibetan-alpaca_dir

例：
- python inference/gradio_demo.py \
     --base_model  /home/rigthub/Tibetan-Llama2-Tibetan-Alpaca/tibetan-Llama2-7B \
     --tokenizer_path  /home/rigthub/Tibetan-Llama2-Tibetan-Alpaca/tibetan-Llama2-7B

- python inference/gradio_demo.py \
     --base_model  /home/rigthub/Tibetan-Llama2-Tibetan-Alpaca/tibetan-Alpaca-7B \
     --tokenizer_path  /home/rigthub/Tibetan-Llama2-Tibetan-Alpaca/tibetan-Alpaca-7B



## 参数说明
- --base_model {base_model} ：存放HF格式的LLaMA-2模型权重和配置文件的目录
- --lora_model {lora_model} ：中文Alpaca-2 LoRA解压后文件所在目录，也可使用??Model Hub模型调用名称。若不提供此参数，则只加载--base_model指定的模型
- --tokenizer_path {tokenizer_path}：存放对应tokenizer的目录。若不提供此参数，则其默认值与--lora_model相同；若也未提供--lora_model参数，则其默认值与--base_model相同
- --only_cpu: 仅使用CPU进行推理
- --gpus {gpu_ids}: 指定使用的GPU设备编号，默认为0。如使用多张GPU，以逗号分隔，如0,1,2
- --alpha {alpha}：使用NTK方法拓展上下文长度的系数，可以提升有效上下文长度。默认为1。如果不知道怎么设置，可以保持默认值，或设为"auto"。关于取值对性能的影响可参考#23
- --load_in_8bit或--load_in_4bit：使用8bit或4bit方式加载模型
- --max_memory：多轮对话历史中存储的最大序列长度（以token计），默认1024
- --use_vllm：使用vLLM作为LLM后端进行推理
- --speculative_sampling: 使用投机采样加速推理
- --draft_base_model {draft_base_model}: 存放HF格式的LLaMA-2小模型权重和配置文件的目录
- --draft_lora_model {draft_lora_model}: 小模型的LoRA文件目录。若不提供此参数，则只加载--base_model指定的模型
- --draft_model_load_in_8bit或--draft_model_load_in_4bit: 使用8bit或4bit方式加载小模型，降低显存占用
- --flash_attn: 使用Flash-Attention加速推理
