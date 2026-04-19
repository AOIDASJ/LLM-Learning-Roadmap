# 项目2：SFT 微调对话模型

## 目标

使用 LoRA/QLoRA 对开源 LLM 进行指令微调，构建一个中文对话模型。

## 预计时间

5-7 天

## 前置知识

- LoRA/QLoRA 原理
- SFT 训练流程
- HuggingFace Transformers

## 实现清单

### Phase 1: 环境准备 (Day 1)
- [ ] 安装 LLaMA-Factory
- [ ] 下载基座模型（Qwen2.5-7B / LLaMA-3-8B）
- [ ] 准备训练数据集

### Phase 2: 数据准备 (Day 1-2)
- [ ] 收集/构造指令数据（至少 1000 条）
- [ ] 数据格式化（Alpaca / ShareGPT 格式）
- [ ] 数据质量检查

### Phase 3: LoRA 微调 (Day 2-4)
- [ ] 配置 LoRA 参数（r=16, alpha=32）
- [ ] 启动训练
- [ ] 监控 loss 曲线
- [ ] 调参优化

### Phase 4: 评估与部署 (Day 4-6)
- [ ] 人工评估对话质量
- [ ] 合并 LoRA 权重
- [ ] 用 vLLM 部署
- [ ] 对比微调前后效果

### Phase 5: DPO 对齐 (Day 6-7，可选)
- [ ] 构造偏好数据
- [ ] DPO 训练
- [ ] 评估对齐效果

## 推荐工具

| 工具 | 链接 | 说明 |
|------|------|------|
| LLaMA-Factory | [GitHub](https://github.com/hiyouga/LLaMA-Factory) | 首选，WebUI 零代码 |
| Axolotl | [GitHub](https://github.com/OpenAccess-AI-Collective/axolotl) | 配置驱动微调 |
| TRL | [GitHub](https://github.com/huggingface/trl) | DPO/RLHF 训练 |

## 推荐数据集

| 数据集 | 说明 |
|--------|------|
| Alpaca-zh | 中文指令数据 |
| ShareGPT | 对话数据 |
| MOSS | 中文对话数据 |
| BelleGroup | 中文指令数据 |

## 验收标准

- 微调后模型能正确遵循中文指令
- LoRA 参数 < 1% 原始参数
- 训练 loss 收敛
- 对话质量明显优于基座模型
