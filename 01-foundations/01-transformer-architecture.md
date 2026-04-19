# Transformer 架构精讲

## 1. 整体架构

Transformer 由 Vaswani 等人在 2017 年提出，核心思想是**完全基于注意力机制**，摒弃 RNN/CNN 的序列依赖。

```
Input → [Embedding + Pos Enc] → [Encoder × N] → Output
                                    ↑
                              [Self-Attention]
                              [Feed-Forward]
                              [Layer Norm]
                              [Residual Conn]
```

### 1.1 Encoder-Decoder 架构
- **Encoder**: 自注意力 + FFN，双向上下文
- **Decoder**: Masked 自注意力 + Cross-Attention + FFN，单向生成

### 1.2 Decoder-Only 架构（GPT 系列）
- 仅使用 Masked Self-Attention
- 自回归生成，适合文本生成任务
- 代表：GPT-2/3/4, LLaMA, Claude

### 1.3 Encoder-Only 架构（BERT 系列）
- 双向 Self-Attention
- 适合理解任务（分类、NER）
- 代表：BERT, RoBERTa, DeBERTa

## 2. 关键组件

### 2.1 Self-Attention
$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$$

### 2.2 Multi-Head Attention
将 Q, K, V 投影到 h 个子空间，分别计算注意力后拼接：
$$\text{MultiHead}(Q,K,V) = \text{Concat}(\text{head}_1, ..., \text{head}_h)W^O$$

### 2.3 Feed-Forward Network
$$\text{FFN}(x) = W_2 \cdot \text{GELU}(W_1 x + b_1) + b_2$$

### 2.4 Residual Connection + Layer Norm
$$\text{output} = \text{LayerNorm}(x + \text{SubLayer}(x))$$

## 3. 现代变体与改进

| 改进点 | 原始方案 | 现代方案 | 代表模型 |
|--------|----------|----------|----------|
| 位置编码 | Sinusoidal | RoPE / ALiBi | LLaMA, Mistral |
| 归一化 | Post-LN | Pre-LN / RMSNorm | GPT-3, LLaMA |
| 激活函数 | ReLU | SwiGLU / GeGLU | LLaMA, PaLM |
| 注意力 | 标准 MHA | GQA / MQA | LLaMA-2, Mistral |

## 4. 学习要点

- [ ] 理解 Self-Attention 的计算流程
- [ ] 理解 Multi-Head 的并行计算
- [ ] 理解残差连接和 Layer Norm 的作用
- [ ] 区分 Pre-Norm 和 Post-Norm
- [ ] 理解 Masked Attention 的因果遮罩

## 5. 推荐资源

- 📄 论文：Attention Is All You Need (Vaswani et al., 2017)
- 🎬 视频：Jay Alammar - The Illustrated Transformer
- 💻 实践：[项目1 - 手写 Mini-Transformer](../projects/01-mini-transformer/)
