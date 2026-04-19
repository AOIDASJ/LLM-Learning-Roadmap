# 🚀 大模型关键知识点与优质学习资源

> 不是从头造轮子，而是站在巨人肩膀上。本文汇总各领域最优质的现有开源项目、权威文档和课程，按你的关注点（部署、量化、推理加速）重点标注。

---

## 一、🔥 综合性 LLM 学习项目（首推）

### 1. [mlabonne/llm-course](https://github.com/mlabonne/llm-course) ⭐⭐⭐⭐⭐
- **70K+ Star**，GitHub 最全免费 LLM 课程
- 分三条路线：LLM 工程师 / LLM 科学家 / 企业路线
- 含 Colab Notebook 可直接运行
- **覆盖**：分词、微调、量化、DPO、RLHF、评估
- 🔥 量化部分特别详细：GPTQ、AWQ、bitsandbytes 实操

### 2. [datawhalechina/llm-cookbook](https://github.com/datawhalechina/llm-cookbook)
- 中文！基于吴恩达大模型系列课程翻译复现
- **覆盖**：Prompt Engineering → RAG → 模型微调
- 适合国内开发者，无语言障碍

### 3. [dive-into-llms](https://github.com/eastlakeside/dive-into-llms)（动手学大模型）
- 免费编程实践教程，可运行代码示例
- 面向学生和开发者

### 4. [microsoft/generative-ai-for-beginners](https://github.com/microsoft/generative-ai-for-beginners)
- 微软官方，12 课入门生成式 AI
- 多语言（含中文）

---

## 二、⭐ 部署 / 量化 / 推理加速（你重点关注）

### 量化技术

| 资源 | 链接 | 说明 |
|------|------|------|
| **Awesome-LLM-Quantization** | [GitHub](https://github.com/pprp/Awesome-LLM-Quantization) | 量化论文/代码大全，持续更新 |
| **AutoGPTQ** | [GitHub](https://github.com/AutoGPTQ/AutoGPTQ) | GPTQ 量化官方实现 |
| **AutoAWQ** | [GitHub](https://github.com/casper-hansen/AutoAWQ) | AWQ 量化官方实现 |
| **bitsandbytes** | [GitHub](https://github.com/TimDettmers/bitsandbytes) | QLoRA 的基础，8bit/4bit 量化 |
| **llama.cpp** | [GitHub](https://github.com/ggerganov/llama.cpp) | GGUF 量化 + CPU/GPU 推理标杆 |
| **GPTQ 论文** | [arXiv:2210.17323](https://arxiv.org/abs/2210.17323) | GPTQ: Accurate Post-Training Quantization |
| **AWQ 论文** | [arXiv:2306.00978](https://arxiv.org/abs/2306.00978) | AWQ: Activation-aware Weight Quantization |
| **SmoothQuant 论文** | [arXiv:2211.10438](https://arxiv.org/abs/2211.10438) | SmoothQuant: W8A8 量化 |
| **BitNet 论文** | [arXiv:2402.17764](https://arxiv.org/abs/2402.17764) | 1-bit LLMs |

### 推理加速

| 资源 | 链接 | 说明 |
|------|------|------|
| **LLM-Inference-Acceleration** | [GitHub](https://github.com/shishishu/LLM-Inference-Acceleration) | 推理加速论文系统整理 |
| **vLLM** | [GitHub](https://github.com/vllm-project/vllm) | PagedAttention + Continuous Batching 推理框架 |
| **TensorRT-LLM** | [GitHub](https://github.com/NVIDIA/TensorRT-LLM) | NVIDIA 极致性能推理 |
| **FlashAttention** | [GitHub](https://github.com/Dao-AILab/flash-attention) | FlashAttention-2/3 |
| **vLLM 官方文档** | [docs.vllm.ai](https://docs.vllm.ai/) | 部署调优必读 |
| **PagedAttention 论文** | [arXiv:2309.06180](https://arxiv.org/abs/2309.06180) | vLLM 核心论文 |
| **Speculative Decoding 综述** | [arXiv:2401.07851](https://arxiv.org/abs/2401.07851) | 投机解码全景 |
| **Medusa** | [GitHub](https://github.com/FasterDecoding/Medusa) | 多头投机解码 |
| **EAGLE** | [GitHub](https://github.com/SafeAILab/EAGLE) | 特征级投机解码 |

### 模型部署

| 资源 | 链接 | 说明 |
|------|------|------|
| **vLLM** | [GitHub](https://github.com/vllm-project/vllm) | 首选 GPU 部署方案 |
| **TGI** | [GitHub](https://github.com/huggingface/text-generation-inference) | HuggingFace 官方推理服务 |
| **Ollama** | [GitHub](https://github.com/ollama/ollama) | 一键本地部署 |
| **LMDeploy** | [GitHub](https://github.com/InternLM/lmdeploy) | 漫步者推理框架 |
| **MLC-LLM** | [GitHub](https://github.com/mlc-ai/mlc-llm) | 编译优化，端侧部署 |

---

## 三、微调与对齐

| 资源 | 链接 | 说明 |
|------|------|------|
| **LLaMA-Factory** ⭐ | [GitHub](https://github.com/hiyouga/LLaMA-Factory) | 最强微调框架，WebUI 零代码微调，支持 LoRA/QLoRA/DPO |
| **PEFT** | [GitHub](https://github.com/huggingface/peft) | HuggingFace 官方 PEFT 库 |
| **TRL** | [GitHub](https://github.com/huggingface/trl) | HuggingFace RLHF/DPO 训练库 |
| **DPO 论文** | [arXiv:2305.18290](https://arxiv.org/abs/2305.18290) | DPO 原始论文 |
| **InstructGPT 论文** | [arXiv:2203.02155](https://arxiv.org/abs/2203.02155) | RLHF 标杆论文 |
| **LoRA 论文** | [arXiv:2106.09685](https://arxiv.org/abs/2106.09685) | LoRA 原始论文 |
| **QLoRA 论文** | [arXiv:2305.14314](https://arxiv.org/abs/2305.14314) | QLoRA 原始论文 |
| **Constitutional AI** | [arXiv:2212.08073](https://arxiv.org/abs/2212.08073) | Anthropic CAI |

---

## 四、RAG 与 Agent 开发

| 资源 | 链接 | 说明 |
|------|------|------|
| **LangChain** | [GitHub](https://github.com/langchain-ai/langchain) | LLM 应用开发框架 |
| **LangGraph** | [GitHub](https://github.com/langchain-ai/langgraph) | Agent 工作流编排 |
| **LlamaIndex** | [GitHub](https://github.com/run-llama/llama_index) | RAG 专用框架 |
| **RAG 完整教程** | [vivy-yi.github.io/rag-tutorial](https://vivy-yi.github.io/rag-tutorial/) | 中文 RAG 从入门到生产部署 |
| **AgentGuide** | [GitHub](https://github.com/adongwanai/AgentGuide) | Agent 开发学习指南 |
| **MCP 协议** | [modelcontextprotocol.io](https://modelcontextprotocol.io/) | Anthropic MCP 官方文档 |
| **Qwen3 + LangChain RAG** | [知乎教程](https://zhuanlan.zhihu.com/p/1971888894615192229) | 实战教程 |

---

## 五、Claude Code 使用

| 资源 | 链接 | 说明 |
|------|------|------|
| **官方文档** | [code.claude.com/docs](https://code.claude.com/docs/zh-CN/quickstart) | 官方快速开始 |
| **中文社区文档** | [claudecn.com/docs](https://claudecn.com/docs/) | Claude 中文开发社区 |
| **Claude Code 中文站** | [claude-cn.org](https://www.claude-cn.org/claude-code-docs-zh/readme) | 完整中文文档镜像 |
| **中文使用指南** | [GitHub](https://github.com/claude-code-chinese/claude-code-guide) | 最易懂的中文指南 |
| **知乎保姆级教程** | [知乎](https://zhuanlan.zhihu.com/p/1985832650116715724) | 不用学命令行 |

---

## 六、必读核心论文（按优先级排序）

### 第一优先级（必读）
1. **Attention Is All You Need** — [arXiv:1706.03762](https://arxiv.org/abs/1706.03762)
2. **GPT-3** — [arXiv:2005.14165](https://arxiv.org/abs/2005.14165)
3. **InstructGPT (RLHF)** — [arXiv:2203.02155](https://arxiv.org/abs/2203.02155)
4. **LoRA** — [arXiv:2106.09685](https://arxiv.org/abs/2106.09685)
5. **Chinchilla Scaling Laws** — [arXiv:2203.15556](https://arxiv.org/abs/2203.15556)

### 第二优先级（量化/部署方向必读）
6. **GPTQ** — [arXiv:2210.17323](https://arxiv.org/abs/2210.17323)
7. **AWQ** — [arXiv:2306.00978](https://arxiv.org/abs/2306.00978)
8. **SmoothQuant** — [arXiv:2211.10438](https://arxiv.org/abs/2211.10438)
9. **FlashAttention-2** — [arXiv:2307.08691](https://arxiv.org/abs/2307.08691)
10. **vLLM (PagedAttention)** — [arXiv:2309.06180](https://arxiv.org/abs/2309.06180)

### 第三优先级（对齐/应用）
11. **DPO** — [arXiv:2305.18290](https://arxiv.org/abs/2305.18290)
12. **QLoRA** — [arXiv:2305.14314](https://arxiv.org/abs/2305.14314)
13. **Constitutional AI** — [arXiv:2212.08073](https://arxiv.org/abs/2212.08073)
14. **RoPE** — [arXiv:2104.09864](https://arxiv.org/abs/2104.09864)
15. **BitNet** — [arXiv:2402.17764](https://arxiv.org/abs/2402.17764)

---

## 七、快速上手路径（针对你的重点）

### 如果你只有 2 周 → 聚焦部署/量化/加速

```
Day 1-2:  跑通 llama.cpp + Ollama 本地部署体验
          → https://github.com/ollama/ollama

Day 3-4:  学习量化原理，实操 GPTQ/AWQ 量化
          → mlabonne/llm-course 的量化 Colab
          → LLaMA-Factory WebUI 一键量化

Day 5-7:  部署 vLLM 推理服务
          → https://docs.vllm.ai/en/latest/getting_started/quickstart.html
          → 压测对比不同量化方案

Day 8-10: 推理加速深入
          → FlashAttention 集成
          → Speculative Decoding 配置
          → Continuous Batching 调优

Day 11-14: 综合实战项目
          → 选一个 7B 模型，完成：量化 → 部署 → 加速 → 压测
          → 输出部署方案文档
```

### 如果你有 1 个月 → 加上微调

```
Week 1:  基础理论速览 (mlabonne/llm-course L1-L3)
Week 2:  LLaMA-Factory 微调实战 (LoRA/QLoRA/DPO)
Week 3:  量化部署实战 (GPTQ/AWQ + vLLM)
Week 4:  推理加速 + 综合项目
```
