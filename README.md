# 🧠 大模型知识学习 - 系统化学习路线图

> 目标：以最高效的方式建立完整的大模型知识体系，涵盖核心原理、技术架构、实际应用与工程化部署能力。

---

## 📂 项目结构

```
大模型知识学习/
├── README.md                          # 总览与路线图（本文件）
├── progress.md                        # 学习进度追踪
├── resources.md                       # 优质开源资源汇总
│
├── daily-digest/                      # 📰 每日知识推送（自动采集）
│   ├── README.md                          # 推送索引
│   └── YYYY-MM-DD.md                      # 按日期归档的推送
│
├── 01- foundations/                    # 第一阶段：基础理论
│   ├── 01-transformer-architecture.md     # Transformer 架构精讲
│   ├── 02-attention-mechanism.md          # 注意力机制详解
│   ├── 03-tokenizer-and-embedding.md      # 分词器与词嵌入
│   └── 04-training-objectives.md          # 训练目标与损失函数
│
├── 02-pretraining-and-scaling/         # 第二阶段：预训练与规模化
│   ├── 01-pretraining-paradigm.md         # 预训练范式
│   ├── 02-scaling-laws.md                 # Scaling Laws
│   ├── 03-data-engineering.md             # 数据工程与清洗
│   └── 04-distributed-training.md         # 分布式训练
│
├── 03-alignment-and-finetuning/        # 第三阶段：对齐与微调
│   ├── 01-sft-and-rlhf.md                # SFT 与 RLHF
│   ├── 02-dpo-and-alternatives.md        # DPO 及替代方案
│   ├── 03-lora-and-qlora.md              # LoRA / QLoRA 参数高效微调
│   └── 04-prompt-engineering.md           # 提示工程
│
├── 04-deployment-and-optimization/     # 第四阶段：部署与优化（重点）
│   ├── 01-model-deployment.md             # 模型部署方案
│   ├── 02-quantization.md                 # 量化技术详解
│   ├── 03-inference-acceleration.md       # 推理加速技术
│   ├── 04-serving-frameworks.md           # 推理服务框架
│   └── 05-edge-deployment.md              # 端侧部署
│
├── 05-applications/                    # 第五阶段：应用开发
│   ├── 01-rag-and-knowledge-base.md       # RAG 与知识库
│   ├── 02-agent-and-tool-use.md           # Agent 与工具调用
│   ├── 03-multimodal.md                   # 多模态应用
│   └── 04-claude-code-guide.md            # Claude Code 使用指南
│
├── papers/                             # 论文文献清单
│   ├── reading-list.md                    # 按阶段分类的论文清单
│   └── paper-notes-template.md            # 论文笔记模板
│
├── projects/                           # 实践项目
│   ├── 01-mini-transformer/               # 项目1：手写 Mini-Transformer
│   ├── 02-sft-chatbot/                    # 项目2：SFT 微调对话模型
│   ├── 03-quantized-deployment/           # 项目3：量化部署实战
│   ├── 04-rag-application/                # 项目4：RAG 应用开发
│   └── 05-agent-system/                   # 项目5：Agent 系统构建
│
└── .codebuddy/
    └── skills/
        └── llm-learning-tutor/        # AI 学习导师 Skill
```

---

## 🗺️ 学习路线图总览

### 🎯 核心原则
- **快速成效**：每个阶段 1-2 周完成，2-3 月完成全部
- **理论+实践**：每学一个理论点，立即动手实现
- **重点强化**：部署、量化、推理加速占比 40%

### ⏱️ 时间规划

| 阶段 | 时间 | 核心目标 | 产出 |
|------|------|----------|------|
| **阶段一：基础理论** | 第1-2周 | 理解 Transformer 全貌 | 手写 Mini-Transformer |
| **阶段二：预训练与规模化** | 第3-4周 | 掌握训练范式与数据工程 | 数据处理 Pipeline |
| **阶段三：对齐与微调** | 第5-7周 | 掌握微调与对齐技术 | SFT 对话模型 |
| **阶段四：部署与优化** ⭐ | 第8-11周 | 量化部署与推理加速 | 量化部署实战 |
| **阶段五：应用开发** | 第12-14周 | RAG/Agent/多模态 | 完整应用项目 |

### 📊 学习进度追踪

详见 [progress.md](./progress.md)，每个知识点标记状态：
- 🔴 未开始
- 🟡 进行中
- ✅ 已完成

---

## 📋 每日学习节奏建议

```
Morning (1h):   阅读论文/理论文档
Midday (1.5h):  动手实践/代码编写
Evening (0.5h): 复盘笔记/更新进度
```

---

## 🔗 快速入口

- [📖 论文阅读清单](./papers/reading-list.md)
- [🛠️ 实践项目指南](./projects/)
- [📊 学习进度追踪](./progress.md)
- [🤖 AI 学习导师](./.codebuddy/skills/llm-learning-tutor/) — 随时问它学习问题！
