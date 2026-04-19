# 分词器与词嵌入

## 1. 分词器 (Tokenizer)

### 1.1 BPE (Byte Pair Encoding)
- **原理**：从字符级开始，反复合并最高频的字符对
- **代表**：GPT-2, GPT-3 (tiktoken)
- **步骤**：
  1. 初始化词表为所有单字符
  2. 统计相邻 token 对频率
  3. 合并最高频 token 对，加入词表
  4. 重复直到达到目标词表大小

```python
# BPE 简化示例
corpus = ["low", "lower", "newest"]
# 初始: l o w </w>, l o w e r </w>, n e w e s t </w>
# 第1轮合并: e+s → es (频率最高)
# 第2轮合并: es+t → est
# ...
```

### 1.2 SentencePiece
- **原理**：将文本视为字节流，直接在字节级做 BPE/Unigram
- **优点**：语言无关，无需预分词
- **代表**：LLaMA, T5, ALBERT

### 1.3 Unigram Language Model
- **原理**：从大词表开始，逐步剪枝低频 token
- **优点**：一个字符串可有多种分词方式，概率最优
- **代表**：T5, ALBERT (配合 SentencePiece)

### 1.4 WordPiece
- **原理**：类似 BPE，但合并标准基于似然增益而非频率
- **代表**：BERT, DistilBERT

### 1.5 各分词器对比

| 分词器 | 粒度 | 语言依赖 | 代表模型 | 词表大小 |
|--------|------|----------|----------|----------|
| BPE | 子词 | 需预分词 | GPT 系列 | ~50k |
| SentencePiece BPE | 子词 | 语言无关 | LLaMA | ~32k |
| SentencePiece Unigram | 子词 | 语言无关 | T5 | ~32k |
| WordPiece | 子词 | 需预分词 | BERT | ~30k |
| Tiktoken | 子词 | 需预分词 | GPT-4 | ~100k |

## 2. 词嵌入 (Embedding)

### 2.1 Token Embedding
- 将 token ID 映射为 $d_{model}$ 维向量
- 可学习参数：`[vocab_size, d_model]`

### 2.2 位置编码 (Positional Encoding)

#### Sinusoidal (原始 Transformer)
$$PE_{(pos, 2i)} = \sin(pos / 10000^{2i/d_{model}})$$
$$PE_{(pos, 2i+1)} = \cos(pos / 10000^{2i/d_{model}})$$

#### RoPE (Rotary Position Embedding) ⭐
- **核心**：通过旋转矩阵编码相对位置
- **优点**：自然编码相对位置，外推性好
- **代表**：LLaMA, Mistral, Qwen

```python
# RoPE 简化实现
def apply_rope(x, freqs):
    # x: [batch, seq, n_heads, d_k]
    x_complex = torch.view_as_complex(x.reshape(*x.shape[:-1], -1, 2))
    x_rotated = x_complex * freqs  # 逐元素旋转
    return torch.view_as_real(x_rotated).flatten(-2)
```

#### ALiBi (Attention with Linear Biases)
- **核心**：在注意力分数上加线性偏置
- **优点**：无需位置编码，训练短序列推理长序列
- **代表**：BLOOM, MPT

### 2.3 嵌入层与输出层共享权重
- **Tied Embedding**：输入嵌入矩阵和输出 LM Head 共享参数
- **优点**：减少参数量
- **代表**：GPT-2, OPT
- **不共享**：LLaMA, 大多数现代模型

## 3. 学习要点

- [ ] 理解 BPE 的合并过程
- [ ] 区分不同分词器的优劣
- [ ] 理解 RoPE 的旋转位置编码原理
- [ ] 实现 Tokenizer 的编码/解码
- [ ] 理解 Tied Embedding 的作用
