# 预训练范式

## 1. GPT 系列演进

### GPT-1 (2018)
- **架构**：Decoder-Only, 12 层, 117M 参数
- **范式**：无监督预训练 + 有监督微调
- **贡献**：验证了预训练+微调的两阶段范式

### GPT-2 (2019)
- **架构**：48 层, 1.5B 参数
- **范式**：Zero-Shot，无需微调
- **贡献**：证明大规模模型可 zero-shot 推理

### GPT-3 (2020)
- **架构**：96 层, 175B 参数
- **范式**：In-Context Learning (ICL)
- **贡献**：Few-Shot Learning，涌现能力

### GPT-4 (2023)
- **架构**：MoE, ~1.8T 参数（推测）
- **范式**：多模态 + RLHF + Safety
- **贡献**：多模态理解，强推理能力

## 2. 开源模型谱系

### LLaMA 系列
| 模型 | 参数量 | 词表大小 | 注意力 | 训练数据 |
|------|--------|----------|--------|----------|
| LLaMA-1 | 7B-65B | 32k | MHA | 1.4T tokens |
| LLaMA-2 | 7B-70B | 32k | GQA(70B) | 2T tokens |
| LLaMA-3 | 8B-70B | 128k | GQA | 15T tokens |
| LLaMA-3.1 | 8B-405B | 128k | GQA | 15T+ tokens |

### 其他重要模型
- **Mistral/Mixtral**：Sliding Window Attention + MoE
- **Qwen**：多语言优化，中文能力强
- **DeepSeek**：MoE 架构创新
- **Gemma**：Google 开源，轻量高效
- **Phi**：微软，小模型高质量数据

## 3. 预训练范式分类

### 3.1 Decoder-Only（主流）
- **优势**：生成能力强，推理效率高
- **代表**：GPT, LLaMA, Mistral
- **趋势**：已成为大模型主流架构

### 3.2 Encoder-Decoder
- **优势**：输入理解+输出生成兼顾
- **代表**：T5, BART, Flan-T5
- **趋势**：在特定任务（翻译、摘要）仍有优势

### 3.3 MoE (Mixture of Experts)
- **核心思想**：每个 token 只激活部分专家
- **代表**：Mixtral 8×7B, DeepSeek-MoE, GPT-4
- **优势**：参数量巨大但计算量可控

```
标准 Transformer:   每个 token → 所有参数
MoE Transformer:    每个 token → Top-K 专家 → 部分参数

Router(x) → expert_probs
top_k_experts = topk(expert_probs, k=2)
output = Σ gate_i * Expert_i(x)
```

## 4. 预训练数据配方

### 4.1 数据来源
- Web 爬虫 (CommonCrawl)
- 书籍 (Books3)
- 代码 (GitHub)
- 学术论文 (arXiv)
- Wikipedia
- 高质量对话数据

### 4.2 数据配比
- Web 数据：60-70%
- 代码：10-15%
- 学术：5-10%
- 书籍：5-10%
- 对话：2-5%

> 关键发现：代码数据对推理能力提升显著

## 5. 学习要点

- [ ] 理解 GPT 系列的范式演进
- [ ] 区分 Decoder-Only / Encoder-Decoder / MoE
- [ ] 理解 MoE 的路由机制
- [ ] 了解开源模型的技术差异
- [ ] 理解数据配比的影响
