
<div align="center">
    <h1>
        Tibetan-Llama2 & Tibetan-Alpaca
    </h1>
</div>

## 🗂️ 目录དཀར་ཆག
- [ 🔥 Tibetan-Llama2&Tibetan-Alpaca藏文大语言模型](#-Tibetan-Llama2&Tibetan-Alpaca藏文大语言模型)
  * [ 📌 藏文词表扩充](#-藏文词表扩充)
  * [ 🌟 参数设置](#-参数设置)
  * [ 🚀 实验数据介绍](#-实验数据介绍)
  * [ 🎯 实验结果分析](#-实验结果分析)
    + [ 🌐 客观效果评测](#-客观效果评测)
    + [ 💪 文本生成评测](#-文本生成评测)
    + [ 💡 模型生成示例](#-模型生成示例)
  * [ 🤝 模型使用](#-模型使用)
    + [ 🤗 模型下载](#-模型下载)
    + [ 💻 推理与部署](#-推理与部署)


# 🔥 Tibetan-Llama2 & Tibetan-Alpaca藏文大语言模型བོད་ཡིག་གི་སྐད་དབྱིབས་ཆེན་པོ།
本项目通过基于LORA的参数高效微调方法，训练了Tibetan-Llama2和Tibetan-Alpaca藏文大语言模型，分别包括7B和13B两种规模，以上模型是基于Llama2模型架构构建的，经过较大规模数据的增量预训练和指令微调，具备了对藏文的深入理解和处理能力。Tibetan-Llama2和Tibetan-Alpaca在藏文理解和生成任务中表现出了较高的效率和性能，并且在多个领域都有广泛的应用前景。
<div align="center">
<img src="https://github.com/ymaoj/Tibetan-Llama2-Tibetan-Alpaca/blob/main/inference/Flow-Diagram.png">
</div>

 
## 📌 藏文词表扩充
Llama2中只有十五个藏文字构建，分别是“་”、“ས”、“ོ”、“ག”、“ར”、“བ”、“ད”、“ང”、“ི”、“ུ”、“ན”、“མ”、“ེ”、“ལ”、“ྱ”。说明Llama2训练时使用了少量的藏文语料，但其理解和生成藏文文本的能力是有限的。为了增强分词器对藏文文本的支持，首先在藏文纯文本数据39.6G上通过SentencePiece技术训练藏文分词器，随后将藏文分词器合并到原始的Llama2分词器中。因此，得到了一个合并的分词器，称之为Tibetan-Llama2分词器，该分词器的词汇量为54949。其中32000是原始Llama2的词汇量，54949是扩充后的Tibetan-Llama2分词器的新词汇量，扩充了22949个藏文标记。


|     分词方法          	|     长度    	|     分词结果                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      	|
|-----------------------	|-------------	|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------	|
|     音节切分          	|     16      	|     'ཁྱོད','ཀྱི','གདོང','ནི','པད','མ','བཞིན','།','།','མིག','དག','ཨུཏྤལ','སྔོན','པོ','བཞིན','།'                                                                                                                                                                                                                                                                                                                                                                                                                                                  	|
|     Llama2            	|     81      	|     '▁','<0xE0>','<0xBD>','<0x81>','ྱ','ོ','ད','་','<0xE0>','<0xBD>','<0x80>','ྱ','ི','་','ག','ད','ོ','ང','་','ན','ི','་','<0xE0>','<0xBD>','<0x94>',<br>'ད','་','མ','་','བ','<0xE0>','<0xBD>','<0x9E>','ི','ན','<0xE0>','<0xBC>','<0x8D>','<0xE0>','<0xBC>','<0x8D>','མ','ི','ག','་','ད','ག','་',<br>'<0xE0>','<0xBD>','<0xA8>','ུ','<0xE0>','<0xBD>','<0x8F>','<0xE0>','<0xBE>','<0xA4>','ལ','་','ས','<0xE0>','<0xBE>',<br>'<0x94>','ོ','ན','་','<0xE0>','<0xBD>','<0x94>','ོ','་','བ','<0xE0>','<0xBD>','<0x9E>','ི','ན','<0xE0>','<0xBC>','<0x8D>'    	|
|     Tibetan-Llama2    	|     10      	|     '▁ཁྱོད་ཀྱི་','གདོང་','ནི་','པད་མ་','བཞིན།།','མིག་','དག་','ཨུཏྤལ་','སྔོན་པོ་','བཞིན།'                                                                                                                                                                                                                                                                                                                                                                                                                                                        	|
## 🌟 参数设置

|     设置                   	|     Tibetan-Llama2-7B/13B    	|     Tibetan-Alpaca-7B/13B    	|
|----------------------------	|-----------------------------------	|-----------------------------------	|
|     Training data          	|     39.6GB                        	|     715K                          	|
|     Epochs                 	|     1                             	|     1                             	|
|     Batch size             	|     1                             	|     4                             	|
|     Dropout Rate           	|     -                             	|     0.05                          	|
|     Peak learning rete     	|     2e-4                          	|     1e-4                          	|
|     Max sequence length    	|     1024                          	|     1024                          	|
|     LoRA rank              	|     64                            	|     64                            	|
|     LoRA alpha             	|     128                           	|     128                           	|
|     LoRA weights           	|     QKVO,MLP                      	|     QKVO,MLP                      	|
|     Training Precision     	|     bf16                          	|     bf16                          	|
|     训练时长               	|     348h/673h                     	|     362h/535h                     	|

## 🚀 实验数据介绍
对于Tibetan-Llama2模型，本文利用39.6GB的藏文纯文本对Llama2进行增量预训练，增强了模型对藏文语义的理解。在获得Tibetan-Llama2模型后对其进行指令微调，加入多种指令提示数据集，使用了大约715128条指令数据（4.37GB），包括开放问答、摘要生成、新闻生成、释义生成、时态生成以及文本分类等数据。所有Alpaca模型都是基于各自的Tibetan-Llama2模型进行训练的。例如，Tibetan-Alpaca-13B是在Tibetan-Llama2-13B上训练的。


|     <img width=60/>类型<img width=60/>        	|     <img width=170/>数量<img width=170/>      	|
|-----------------	|---------------	|
|     <img width=40/>释义生成    	|     <img width=165/>58544     	|
|     <img width=40/>时态生成    	|     <img width=170/>2484      	|
|     <img width=40/>新闻生成    	|     <img width=160/>241292    	|
|     <img width=40/>开放问答    	|     <img width=160/>170449    	|
|     <img width=40/>摘要生成    	|     <img width=160/>234166    	|
|     <img width=40/>文本分类    	|     <img width=170/>8193      	|
## 🎯 实验结果分析
由于大语言模型的评估是一项具有挑战性的工作，缺乏标准化的藏文大模型的评估基准，使评估工作变得复杂，尽管本文采用的评估方法有一定的参考性，但也无法准确衡量模型在不同领域的性能。
  - "综合评估大模型能力仍然是亟待解决的重要课题，单个数据集的结果并不能综合评估模型性能。推荐用户在自己关注的任务上进行测试，选择适配相关任务的模型。"
### 🌐 客观效果评测
本文在自建的机器阅读理解任务多项选择数据集中随机挑选了两千条数据进行0-shot和few-shot（5-shot）实验，其验证集包含1600条数据，测试集包含400条数据。本实验在测试集上比较了四种不同的模型（Tibetan-Llama2-7B、Tibetan-Llama2-13B、Tibetan-Alpaca-7B、Tibetan-Alpaca-13B），性能是通过准确率（Accuracy）这一指标来衡量的。
|     Model                 	|     <img width=46/>0-shot(%)<img width=46/>    	|     <img width=46/>few-shot(%)<img width=46/>    	|
|---------------------------	|------------------	|--------------------	|
|     Tibetan-Llama2-7B     	|     <img width=60/>21.59        	|     <img width=70/>24.81          	|
|     Tibetan-Llama2-13B    	|     <img width=60/>22.08        	|     <img width=70/>27.05          	|
|     Tibetan-Alpaca-7B     	|     <img width=60/>25.31        	|     <img width=70/>26.80          	|
|     Tibetan-Alpaca-13B    	|     <img width=60/>26.98        	|     <img width=70/>28.54          	|

### 💪 文本生成评测
由于Tibetan-Llama2模型只具备文本续写能力，无法在该模型上进行各下游任务的文本生成评测，因此本文只在Tibetan-Alpaca模型上进行比较。本文的评估集旨在全面评估Tibetan-Alpaca模型在广泛的藏语自然语言理解和生成任务。该评估集包括12000个样本，涵盖六个不同的任务，通过原始答案与生成的答案之间计算Rouge-L分数来评价模型性能。
|     <img width=50/>Model<img width=50/>       	|     <img width=18/>Tibetan-Alpaca-7B<img width=18/>    	|     <img width=18/>Tibetan-Alpaca-13B<img width=18/>    	|
|-----------------	|--------------------------	|---------------------------	|
|     <img width=40/>摘要生成    	|     <img width=60/>31.70                	|     <img width=60/>43.84                 	|
|     <img width=40/>新闻生成    	|     <img width=60/>1.58                 	|     <img width=60/>3.77                  	|
|     <img width=40/>开放问答    	|     <img width=60/>5.41                 	|     <img width=60/>7.89                  	|
|     <img width=40/>时态生成    	|     <img width=60/>83.50                	|     <img width=60/>85.49                 	|
|     <img width=40/>释义生成    	|     <img width=60/>9.94                 	|     <img width=60/>12.76                 	|
|     <img width=40/>文本分类    	|     <img width=60/>76.63                	|     <img width=60/>78.94                 	|
|     <img width=49/>Avg         	|     <img width=60/>32.79                	|     <img width=60/>38.78                 	|
### 💡 模型生成示例
<img src="https://github.com/ymaoj/Tibetan-Llama2-Tibetan-Alpaca/blob/main/inference/Generate-Example.png">

## 🤝 模型使用
### 🤗 模型下载
| 模型名称                  |   类型   | 大小 |                    下载地址                    |
| :------------------------ | :------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
| Tibetan-Llama2-7B | 基座模型 | 12.9 GB | [[🤗HF]](https://huggingface.co/ymaoj/Tibetan-Llama2-7b) |
| Tibetan-Llama2-13B | 基座模型 | 24.7 GB | [[🤗HF]](https://huggingface.co/ymaoj/Tibetan-Llama2-13b) |
| Tibetan-Alpaca-7B | 指令模型 | 12.9 GB | [[🤗HF]](https://huggingface.co/ymaoj/Tibetan-Alpaca-7B) |
| Tibetan-Alpaca-13B | 指令模型 | 24.7 GB | [[🤗HF]](https://huggingface.co/ymaoj/Tibetan-Alpaca-13b)|
### 💻 推理与部署
Tibetan-Llama2为基座模型，不具有指令理解能力，推荐使用Tibetan-Alpaca进行交互。
详细请看[[rd]](https://github.com/ymaoj/Tibetan-Llama2-Tibetan-Alpaca/tree/main/inference#readme))
#### 🤖 命令行交互形式
```
python inference/inference_hf.py \
    --base_model path_to__Tibetan-Llama2_or_Tibetan-Alpaca_dir \
    --with_prompt \
    --interactive
```

#### 📈 web图形界面交互形式
```
python inference/gradio_demo.py \
     --base_model  path_to_Tibetan-Alpaca_dir \
     --tokenizer_path  path_to_Tibetan-Alpaca_dir
```


#### 📝 提示词
交互过程中使用以下提示词效果更佳。
  - 摘要生成
```
གཤམ་གྱི་ཚན་པ་འདིའི་ནང་དོན་གཞིར་བཟུང་ནས་ནང་དོན་གནད་བསྡུས་ཞིག་འབྲི་རོགས།
གཤམ་གྱི་ཚན་པ་དེའི་ནང་དོན་གནད་བསྡུས་འབྲི་རོགས།
```
  例：གཤམ་གྱི་ཚན་པ་འདིའི་ནང་དོན་གཞིར་བཟུང་ནས་ནང་དོན་གནད་བསྡུས་ཞིག་འབྲི་རོགས།དབྱེ་ཧྲ་ཞེས་པའི་དྲག་རླུང་གི་ཤུགས་རྐྱེན་ཐེབས་པའི་དབང་གིས་དབྱིན་ཇི་དང་ཨེར་ལན་གྱི་ས་ཁུལ་མང་བོར་ཆར་ཆེན་འབབ་པའི་ནམ་ཟླ་བྱུང་ནས་འགྲིམ་འགྲུལ་ལ་བཀག་རྒྱ་ཐེབས། དབྱིན་ཇིའི་སྨྱན་བྱད་ཀྱིས་བསྒྲགས་པ་ལྟར་ན། ས་དེའི་ཚེས་ཉེར་གཉིས་ཉིན་གྱི་སྔ་དྲོར། དབྱིན་ཇིའི་སུའུ་ཀི་ལན་ས་ཁུལ་གྱི་ལྕགས་ལམ་ཞབས་ཞུ་མཚམས་ཆད་ཡོད། དབྱིན་ཇིའི་གནམ་གཤིས་བརྟགས་དཔྱད་ཅུའུ་ཡིས་དབྱིན་ཇིའི་ས་ཁུལ་མང་ཆེ་བར་རླུང་ཆེན་གཡུག་པའི་ཉེན་བརྡ་སེར་པོ་བཏང་ཡོད་ལ། དབྱིན་ཀི་ལན་དང་ཝེ་ཨར་ཧྲི་ས་ཁུལ་དུ་རླུང་འཚུབ་འབྱུང་ངེས་རེད། དེ་ལས་གཞན་དྲག་རླུང་གི་ཤུགས་རྐྱེན་དབང་གིས་ཨེར་ལན་གྱི་ཏུའུ་པའོ་ལིན་གནམ་གྲུ་ཐང་གིས་ཚེས་ཉེར་གཅིག་ཉིན་གནམ་གྲུ་གཏོང་ཐེངས་བརྒྱ་དང་བཅུ་ལྷག་གཏོང་མཚམས་བཞག་ཡོད་ལ། འགྲུལ་བ་མང་བོ་གནམ་གྲུ་ཐང་དུ་ཁགས་ཡོད།\
  例：གཤམ་གྱི་ཚན་པ་དེའི་ནང་དོན་གནད་བསྡུས་འབྲི་རོགས།དབྱེ་ཧྲ་ཞེས་པའི་དྲག་རླུང་གི་ཤུགས་རྐྱེན་ཐེབས་པའི་དབང་གིས་དབྱིན་ཇི་དང་ཨེར་ལན་གྱི་ས་ཁུལ་མང་བོར་ཆར་ཆེན་འབབ་པའི་ནམ་ཟླ་བྱུང་ནས་འགྲིམ་འགྲུལ་ལ་བཀག་རྒྱ་ཐེབས། དབྱིན་ཇིའི་སྨྱན་བྱད་ཀྱིས་བསྒྲགས་པ་ལྟར་ན། ས་དེའི་ཚེས་ཉེར་གཉིས་ཉིན་གྱི་སྔ་དྲོར། དབྱིན་ཇིའི་སུའུ་ཀི་ལན་ས་ཁུལ་གྱི་ལྕགས་ལམ་ཞབས་ཞུ་མཚམས་ཆད་ཡོད། དབྱིན་ཇིའི་གནམ་གཤིས་བརྟགས་དཔྱད་ཅུའུ་ཡིས་དབྱིན་ཇིའི་ས་ཁུལ་མང་ཆེ་བར་རླུང་ཆེན་གཡུག་པའི་ཉེན་བརྡ་སེར་པོ་བཏང་ཡོད་ལ། དབྱིན་ཀི་ལན་དང་ཝེ་ཨར་ཧྲི་ས་ཁུལ་དུ་རླུང་འཚུབ་འབྱུང་ངེས་རེད། དེ་ལས་གཞན་དྲག་རླུང་གི་ཤུགས་རྐྱེན་དབང་གིས་ཨེར་ལན་གྱི་ཏུའུ་པའོ་ལིན་གནམ་གྲུ་ཐང་གིས་ཚེས་ཉེར་གཅིག་ཉིན་གནམ་གྲུ་གཏོང་ཐེངས་བརྒྱ་དང་བཅུ་ལྷག་གཏོང་མཚམས་བཞག་ཡོད་ལ། འགྲུལ་བ་མང་བོ་གནམ་གྲུ་ཐང་དུ་ཁགས་ཡོད།
  - 新闻生成
```
གོང་གསལ་གྱི་ཁ་བྱང་འདི་དང་འབྲེལ་བ་ཡོད་པའི་རྩོམ་ཞིག་འབྲི་རོགས།
ཁ་བྱང་ནི་"  "ཡིན་པའི་རྩོམ་ཐུང་ཞིག་འབྲི་རོགས།
```
  例：མཚོ་སྔོན་གྱིས་བྱ་ཐབས་སྣ་ཚོགས་སྤྱད་དེ་མཐོ་སློབ་སློབ་ཐོན་སློབ་མའི་ལས་ཞུགས་ལ་ཞབས་འདེགས་ཞུ་བ།གོང་གསལ་གྱི་ཁ་བྱང་འདི་དང་འབྲེལ་བ་ཡོད་པའི་རྩོམ་ཞིག་འབྲི་རོགས།\
  例：ཁ་བྱང་ནི་“མཚོ་སྔོན་གྱིས་བྱ་ཐབས་སྣ་ཚོགས་སྤྱད་དེ་མཐོ་སློབ་སློབ་ཐོན་སློབ་མའི་ལས་ཞུགས་ལ་ཞབས་འདེགས་ཞུ་བ།“ཡིན་པའི་རྩོམ་ཐུང་ཞིག་འབྲི་རོགས།
  - 开放问答\
  例：བོད་ཅེས་པའི་མིང་འདི་ཇི་ལྟར་བྱུང་བ་དང༌། བོད་ཅེས་པའི་མིང་དེར་གོ་དོན་ཡོད་མེད་ཀྱི་འདོད་ཚུལ་མི་འདྲ་བ་གང་དག་ཡོད་དམ།\
  例：ཀྲུང་གོའི་སྲོལ་རྒྱུན་དུས་ཆེན་ཆེན་པོ་བཞི་གང་དག་ཡིན།
  - 时态生成
```
-བྱ་ཚིག་འདིའི་ད་ལྟ་བ་དང་འདས་པ།མ་འོངས་པ།སྐུལ་ཚིག་སོ་སོ་གང་དག་ཡིན།
-བྱ་ཚིག་འདིའི་དུས་གསུམ་ནི་གང་དག་ཡིན།
```
  例：འཁྱམ-བྱ་ཚིག་འདིའི་ད་ལྟ་བ་དང་འདས་པ།མ་འོངས་པ།སྐུལ་ཚིག་སོ་སོ་གང་དག་ཡིན།\
  例：འཁྱམ-བྱ་ཚིག་འདིའི་དུས་གསུམ་ནི་གང་དག་ཡིན།
  - 释义生成
```
-མདུན་གྱི་ཐ་སྙད་འདི་ལ་འགྲེལ་བཤད་རྒྱག་རོགས།
ཐ་སྙད་འདི་ལ་འོས་འཚམ་གྱི་འགྲེལ་བ་བྱོས།
```
  例：དྲན་འཛིན-མདུན་གྱི་ཐ་སྙད་འདི་ལ་འགྲེལ་བཤད་རྒྱག་རོགས།\
  例：དྲན་འཛིན།ཐ་སྙད་འདི་ལ་འོས་འཚམ་གྱི་འགྲེལ་བ་བྱོས།
  - 文本分类
```
ཚན་པ་འདིའི་ནང་དོན་རིགས་གང་ལ་གཏོགས།
```
  例：ཚན་པ་འདིའི་ནང་དོན་རིགས་གང་ལ་གཏོགས།བོད་ཀྱི་སློབ་འབྲིང་དྲ་བ་ཐེངས་འདིའི་འགྲན་བསྡུར་བྱས་པར་བརྒྱུད་རྫོང་ཡོངས་ཀྱི་བོད་ཡིག་དགེ་རྒན་རྣམས་ཀྱིས་སྨྱན་མང་ཁྲིད་ཆས་བཟོ་བའི་སྤྲོ་སེམས་བསྐྱེད་པ་མ་ཟད་ཕྱིས་སུ་སློབ་ཁྲིད་ཀྱི་ཁྲོད་དུ་སྨྱན་མང་ཁྲིད་ཆས་བཀོལ་སྤྱོད་བྱས་ཏེ་སློབ་མའི་སྦྱངས་འབྲས་མཐོར་འདེགས་བྱེད་པར་སྐུལ་མ་ཐེབས་པའོ་༢༠༡༢ལོའི་ཟླ་༡༡ཚེས་༡༧ནས༡༨ཉིན་བར་དཔའ་རིས་བོད་རང་སྐྱོང་རྫོང་སློབ་གསོ་ཅུའུ་ཡིས་གཙོ་སྒྲུབ་བྱེད་པའི་རྫོང་ཡོངས་ཀྱི་སྐད་གཉིས་སློབ་གྲྭའི་བོད་ཡིག་སྨྱན་མང་ཁྲིད་ཆས་འགྲན་བསྡུར་དུས་ལྟར་དུ་དཔའ་རིས་མི་རིགས་སློབ་འབྲིང་དུ་སྤེལ་ཐེངས་འདིའི་འགྲན་བསྡུར་དུ་ཞུགས་པའི་སྨྱན་མང་ཁྲིད་ཆས་༧༠ལྷག་ཙམ་ཡོད་པ་དང་སློབ་ཆུང་དང་སློབ་འབྲིང་ཁག་གཉིས་སུ་བགོས་ཡོད་དཔྱད་བསྡུར་མཁན་རྣམས་ཀྱིས་གཟབ་ནན་ངང་འདེམས་བསལ་བྱས་མཐར་ཕུལ་བྱུང་ཅན་གྱི་སྨྱན་མང་ཁྲིད་ཆས་༡༧ལ་བྱ་དགའ་ཐོབ།


### 
指导老师：柔特；才让加。\
如果您有任何问题，欢迎联系yangmaojia2017@163.com

## 致谢
感谢Llama-2研究开发人员及相关项目，感谢Chinese-LLaMA-Alpaca-2项目以及相关人员。

## 免责声明
本项目基于由Meta发布的Llama-2模型进行开发，使用过程中请严格遵守Llama-2的开源许可协议。模型生成的内容可能会因为计算方法、随机因素以及量化精度损失等影响其准确性，因此，本项目不对模型输出的准确性提供任何保证，也不会对任何因使用相关资源和输出结果产生的损失承担责任。如果将本项目的相关模型用于商业用途，开发者应遵守当地的法律法规，确保模型输出内容的合规性，本项目不对任何由此衍生的产品或服务承担责任。

### 局限性声明
虽然本项目中的模型具备一定的藏文理解和生成能力，但也存在局限性，
可能会产生不可预测的有害内容以及不符合人类偏好和价值观的内容；
由于算力和数据问题，相关模型的训练并不充分，藏文理解能力有待进一步提升。



