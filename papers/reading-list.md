# 📖 论文阅读清单

> 按学习阶段和优先级分类。🔴 必读 / 🟡 推荐 / 🔵 拓展

---

## 阶段一：基础理论

### Transformer 架构
| 优先级 | 论文 | 链接 | 关键贡献 |
|--------|------|------|----------|
| 🔴 | Attention Is All You Need (Vaswani et al., 2017) | [arXiv:1706.03762](https://arxiv.org/abs/1706.03762) | Transformer 架构 |
| 🔴 | BERT: Pre-training of Deep Bidirectional Transformers (Devlin et al., 2019) | [arXiv:1810.04805](https://arxiv.org/abs/1810.04805) | 双向预训练 |
| 🔴 | Language Models are Few-Shot Learners / GPT-3 (Brown et al., 2020) | [arXiv:2005.14165](https://arxiv.org/abs/2005.14165) | In-Context Learning |
| 🟡 | RoFormer: RoPE (Su et al., 2021) | [arXiv:2104.09864](https://arxiv.org/abs/2104.09864) | 旋转位置编码 |
| 🟡 | Train Short, Test Long: ALiBi (Press et al., 2022) | [arXiv:2108.12409](https://arxiv.org/abs/2108.12409) | 线性偏置位置编码 |
| 🟡 | GLU Variants (Shazeer, 2020) | [arXiv:2002.05202](https://arxiv.org/abs/2002.05202) | SwiGLU 激活函数 |
| 🔵 | GQA: Training Generalized Multi-Query Transformer (Ainslie et al., 2023) | [arXiv:2305.13245](https://arxiv.org/abs/2305.13245) | 分组查询注意力 |

---

## 阶段二：预训练与规模化

### Scaling Laws
| 优先级 | 论文 | 链接 | 关键贡献 |
|--------|------|------|----------|
| 🔴 | Scaling Laws for Neural Language Models (Kaplan et al., 2020) | [arXiv:2001.08361](https://arxiv.org/abs/2001.08361) | Kaplan Scaling Laws |
| 🔴 | Training Compute-Optimal LLMs / Chinchilla (Hoffmann et al., 2022) | [arXiv:2203.15556](https://arxiv.org/abs/2203.15556) | 计算最优配比 |
| 🟡 | LLaMA: Open and Efficient Foundation Language Models (Touvron et al., 2023) | [arXiv:2302.13971](https://arxiv.org/abs/2302.13971) | 开源 LLM 标杆 |
| 🟡 | LLaMA 2 (Touvron et al., 2023) | [arXiv:2307.09288](https://arxiv.org/abs/2307.09288) | 开源+对齐 |

### 分布式训练
| 优先级 | 论文 | 链接 | 关键贡献 |
|--------|------|------|----------|
| 🔴 | ZeRO: Memory Optimizations Toward Training Trillion Parameter Models (Rajbhandari et al., 2020) | [arXiv:1910.02054](https://arxiv.org/abs/1910.02054) | ZeRO 优化 |
| 🟡 | Megatron-LM (Shoeybi et al., 2019) | [arXiv:1909.08053](https://arxiv.org/abs/1909.08053) | 张量并行 |
| 🟡 | Efficient Large-Scale Language Model Training (Li et al., 2023) | [arXiv:2104.04473](https://arxiv.org/abs/2104.04473) | 3D 并行 |

### MoE
| 优先级 | 论文 | 链接 | 关键贡献 |
|--------|------|------|----------|
| 🟡 | Mixtral of Experts (Jiang et al., 2024) | [arXiv:2401.04088](https://arxiv.org/abs/2401.04088) | 开源 MoE |
| 🔵 | Switch Transformers (Fedus et al., 2021) | [arXiv:2101.03961](https://arxiv.org/abs/2101.03961) | 简化 MoE |

---

## 阶段三：对齐与微调

### SFT 与 RLHF
| 优先级 | 论文 | 链接 | 关键贡献 |
|--------|------|------|----------|
| 🔴 | Training language models to follow instructions / InstructGPT (Ouyang et al., 2022) | [arXiv:2203.02155](https://arxiv.org/abs/2203.02155) | RLHF 标杆 |
| 🔴 | Constitutional AI (Bai et al., 2022) | [arXiv:2212.08073](https://arxiv.org/abs/2212.08073) | AI 自我对齐 |
| 🟡 | Self-Instruct (Wang et al., 2023) | [arXiv:2212.10560](https://arxiv.org/abs/2212.10560) | 自生成指令 |

### DPO 及替代方案
| 优先级 | 论文 | 链接 | 关键贡献 |
|--------|------|------|----------|
| 🔴 | DPO: Direct Preference Optimization (Rafailov et al., 2023) | [arXiv:2305.18290](https://arxiv.org/abs/2305.18290) | 直接偏好优化 |
| 🟡 | IPO: Identity Preference Optimization (Azar et al., 2023) | [arXiv:2310.12036](https://arxiv.org/abs/2310.12036) | DPO 正则化改进 |
| 🟡 | KTO: Model Alignment as Prospect Theory (Ethayarajh et al., 2024) | [arXiv:2402.01306](https://arxiv.org/abs/2402.01306) | 非成对偏好 |
| 🟡 | ORPO (Hong et al., 2024) | [arXiv:2403.07691](https://arxiv.org/abs/2403.07691) | SFT+对齐一步完成 |

### 参数高效微调
| 优先级 | 论文 | 链接 | 关键贡献 |
|--------|------|------|----------|
| 🔴 | LoRA (Hu et al., 2021) | [arXiv:2106.09685](https://arxiv.org/abs/2106.09685) | 低秩自适应 |
| 🔴 | QLoRA (Dettmers et al., 2023) | [arXiv:2305.14314](https://arxiv.org/abs/2305.14314) | 4-bit 量化微调 |
| 🟡 | DoRA (Liu et al., 2024) | [arXiv:2402.09353](https://arxiv.org/abs/2402.09353) | 权重分解 LoRA |

---

## 阶段四：部署与优化 ⭐

### 量化
| 优先级 | 论文 | 链接 | 关键贡献 |
|--------|------|------|----------|
| 🔴 | GPTQ: Accurate Post-Training Quantization (Frantar et al., 2022) | [arXiv:2210.17323](https://arxiv.org/abs/2210.17323) | 逐层最优量化 |
| 🔴 | AWQ: Activation-aware Weight Quantization (Lin et al., 2023) | [arXiv:2306.00978](https://arxiv.org/abs/2306.00978) | 激活感知量化 |
| 🔴 | SmoothQuant (Xiao et al., 2023) | [arXiv:2211.10438](https://arxiv.org/abs/2211.10438) | W8A8 平滑量化 |
| 🟡 | LLM.int8() (Dettmers et al., 2022) | [arXiv:2208.07339](https://arxiv.org/abs/2208.07339) | 混合精度 INT8 |
| 🟡 | The Era of 1-bit LLMs / BitNet (Ma et al., 2024) | [arXiv:2402.17764](https://arxiv.org/abs/2402.17764) | 三值网络 |
| 🔵 | QuIP / QuIP# (Chee et al., 2023) | [arXiv:2307.01785](https://arxiv.org/abs/2307.01785) | 不相干性量化 |
| 🔵 | AQLM (Egiazarian et al., 2024) | [arXiv:2401.08604](https://arxiv.org/abs/2401.08604) | 加法量化 |

### 推理加速
| 优先级 | 论文 | 链接 | 关键贡献 |
|--------|------|------|----------|
| 🔴 | FlashAttention (Dao et al., 2022) | [arXiv:2205.14135](https://arxiv.org/abs/2205.14135) | 分块注意力 |
| 🔴 | FlashAttention-2 (Dao, 2023) | [arXiv:2307.08691](https://arxiv.org/abs/2307.08691) | 优化并行 |
| 🔴 | vLLM / PagedAttention (Kwon et al., 2023) | [arXiv:2309.06180](https://arxiv.org/abs/2309.06180) | 虚拟内存 KV Cache |
| 🟡 | Fast Inference from Transformers via Speculative Decoding (Leviathan et al., 2023) | [arXiv:2302.01318](https://arxiv.org/abs/2302.01318) | 投机解码 |
| 🟡 | Medusa (Cai et al., 2024) | [arXiv:2401.10774](https://arxiv.org/abs/2401.10774) | 多头投机 |
| 🟡 | H2O: Heavy-Hitter Oracle (Zhang et al., 2023) | [arXiv:2306.14048](https://arxiv.org/abs/2306.14048) | KV Cache 稀疏化 |

---

## 阶段五：应用开发

### RAG
| 优先级 | 论文 | 链接 | 关键贡献 |
|--------|------|------|----------|
| 🔴 | Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks (Lewis et al., 2020) | [arXiv:2005.11401](https://arxiv.org/abs/2005.11401) | RAG 原始论文 |
| 🟡 | Self-RAG (Asai et al., 2023) | [arXiv:2310.11511](https://arxiv.org/abs/2310.11511) | 自适应检索 |
| 🟡 | Corrective RAG (Yan et al., 2024) | [arXiv:2401.15884](https://arxiv.org/abs/2401.15884) | 纠错检索 |

### Agent
| 优先级 | 论文 | 链接 | 关键贡献 |
|--------|------|------|----------|
| 🔴 | ReAct (Yao et al., 2023) | [arXiv:2210.03629](https://arxiv.org/abs/2210.03629) | 推理+行动 |
| 🟡 | Toolformer (Schick et al., 2023) | [arXiv:2302.04761](https://arxiv.org/abs/2302.04761) | 自学工具 |
| 🟡 | Tree of Thoughts (Yao et al., 2023) | [arXiv:2305.10601](https://arxiv.org/abs/2305.10601) | 树搜索推理 |

### 多模态
| 优先级 | 论文 | 链接 | 关键贡献 |
|--------|------|------|----------|
| 🟡 | LLaVA (Liu et al., 2023) | [arXiv:2304.08485](https://arxiv.org/abs/2304.08485) | 视觉指令微调 |
| 🟡 | BLIP-2 (Li et al., 2023) | [arXiv:2301.12597](https://arxiv.org/abs/2301.12597) | Q-Former 对齐 |

---

## 📊 阅读进度统计

| 阶段 | 必读(🔴) | 推荐(🟡) | 拓展(🔵) | 已读 |
|------|---------|---------|---------|------|
| 一：基础理论 | 3 | 3 | 1 | 0 |
| 二：预训练与规模化 | 3 | 4 | 1 | 0 |
| 三：对齐与微调 | 3 | 5 | 0 | 0 |
| 四：部署与优化 ⭐ | 6 | 4 | 2 | 0 |
| 五：应用开发 | 3 | 5 | 0 | 0 |
| **合计** | **18** | **21** | **4** | **0** |
