# Qwen2.5 Fine-tuning

## 0 参考视频

[如何将Qwen2.5-7B模型微调为某个行业的专家？超低成本手把手带你从零微调酒店推荐行业大模型，环境配置+模型微调+模型部署+效果展示详细教程！](https://www.bilibili.com/video/BV1YQ5rzfEaQ)

## 1 申请在线账号

## 训练流程

```Mermaid
graph TD
    A[(加载数据集)] --> B[按特定格式拼接输入输出]
    B --> C[文本转 Token IDs]
    C --> D[数据规整器]
    D-->|数据批次#40;Batch#41;| E
    F[加载 Tokenizer]-->C
    G[加载模型] --> H[注入参数 LoRA, QLoRA]
    H-->E
    S-->E
    subgraph I[数据预处理]
    B
    C
    end
    subgraph J[ ]
    G
    H
    end
    subgraph E[训练器:针对每个批次]
    K[计算Loss]
    L[更新参模型参数]
    M[更新学习率]
    N[阶段性eval、log、save]
    end
    subgraph S[定义训练超参,<b>超参</b>类比于配置项 包括]
        S1[1.模型超参:<br> 1网络层数、参数维度<br> 2 Attention Head数<br> 3 激活函数类型等]
        S2[2.训练超参:<br> 1 学习率、批次大小<br> 2 求解器类型、参数<br> 3 学习率调节方式、参数<br> 4 Dropout概率等]
    end
```

## 2.1 下载模型
从huggingface下载Qwen模型

```bash
# 下面的命令选择其中的一个

# 从huggingface下载模型
git clone https://huggingface.co/Qwen/Qwen3-0.6B
git clone --depth 1 https://huggingface.co/Qwen/Qwen3-0.6B

# 使用国内镜像网站更快
git clone https://hf-mirror.com/Qwen/Qwen3-0.6B

# 不下载版本控制文件(.git)
# 为了加快速度,选择0.6B版本
git clone --depth 1 https://hf-mirror.com/Qwen/Qwen3-0.6B
```
