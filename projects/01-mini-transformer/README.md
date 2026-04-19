# 项目1：手写 Mini-Transformer

## 目标

从零实现一个 GPT-2 风格的小型 Transformer 语言模型，深入理解每个组件。

## 预计时间

3-5 天

## 前置知识

- Python / PyTorch 基础
- 线性代数基础
- 已阅读 Attention Is All You Need

## 实现清单

### Phase 1: 基础组件 (Day 1-2)
- [ ] 实现 Scaled Dot-Product Attention
- [ ] 实现 Multi-Head Attention
- [ ] 实现 Position-wise FFN
- [ ] 实现 Layer Normalization
- [ ] 实现 Residual Connection

### Phase 2: 模型组装 (Day 2-3)
- [ ] 实现 Transformer Block
- [ ] 实现位置编码 (Sinusoidal)
- [ ] 实现 GPT 模型（Decoder-Only）
- [ ] 实现权重初始化

### Phase 3: 训练与生成 (Day 3-4)
- [ ] 实现字符级/子词级分词器
- [ ] 实现 CLM 训练循环
- [ ] 实现自回归文本生成
- [ ] 在小数据集上训练（如 Shakespeare）

### Phase 4: 进阶 (Day 4-5)
- [ ] 替换为 RoPE 位置编码
- [ ] 替换为 SwiGLU 激活函数
- [ ] 替换为 RMSNorm
- [ ] 对比不同配置的效果

## 参考实现

| 资源 | 链接 | 说明 |
|------|------|------|
| minGPT | [github.com/karpathy/minGPT](https://github.com/karpathy/minGPT) | Karpathy 的最小 GPT |
| nanoGPT | [github.com/karpathy/nanoGPT](https://github.com/karpathy/nanoGPT) | 更简洁的版本 |
| lit-gpt | [github.com/Lightning-AI/litgpt](https://github.com/Lightning-AI/litgpt) | Lightning AI 版 |
| The spelled-out intro to LLMs | [YouTube](https://www.youtube.com/watch?v=zjkBMFhNj_g) | Karpathy 视频教程 |

## 验收标准

- 模型能在 Shakespeare 数据集上生成可读文本
- 能解释每个组件的数学原理
- 训练 loss 稳定下降
