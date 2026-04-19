# SFT 与 RLHF

## 1. Supervised Fine-Tuning (SFT)

### 1.1 目标
将预训练模型从"续写文本"转化为"遵循指令回答问题"

### 1.2 数据构造

#### 指令数据格式
```json
{
  "instruction": "请解释什么是机器学习",
  "input": "",
  "output": "机器学习是人工智能的一个分支..."
}
```

#### 数据来源
| 方法 | 描述 | 质量 |
|------|------|------|
| 人工标注 | 专业标注员编写 | 高 |
| Self-Instruct | 模型自生成指令 | 中 |
| 开源数据集 | Alpaca, ShareGPT 等 | 中-高 |
| GPT-4 蒸馏 | 用 GPT-4 生成 | 高 |

### 1.3 训练细节
- **学习率**：1e-5 ~ 5e-5（比预训练小 10-100x）
- **Epochs**：2-5（过多会过拟合）
- **损失**：仅计算 output 部分的交叉熵
- **关键**：忽略 prompt 部分，只对 response 做 loss

### 1.4 常见问题
- **灾难性遗忘**：SFT 后预训练能力下降
- **过拟合**：小数据集上容易过拟合
- **解决**：混合预训练数据、LoRA 微调

## 2. RLHF (Reinforcement Learning from Human Feedback)

### 2.1 三阶段流程

```
阶段1: 预训练模型 → SFT 模型
阶段2: SFT 模型 + 偏好数据 → 奖励模型 (RM)
阶段3: SFT 模型 + RM → PPO 优化 → 对齐模型
```

### 2.2 奖励模型训练

#### 偏好数据
```json
{
  "prompt": "写一首关于春天的诗",
  "chosen": "春风拂面花如海...",     ← 人类偏好
  "rejected": "春天来了花开了..."     ← 人类不偏好
}
```

#### 训练目标
$$\mathcal{L}_{RM} = -\mathbb{E}_{(x,y_w,y_l)}\left[\log \sigma\left(r(x, y_w) - r(x, y_l)\right)\right]$$

### 2.3 PPO 优化

#### 核心思想
- 用奖励模型作为环境
- 策略模型生成回答
- 奖励模型评分
- PPO 更新策略

#### 目标函数
$$\mathcal{L}_{PPO} = \mathbb{E}\left[\min\left(r_t(\theta)\hat{A}_t, \text{clip}(r_t(\theta), 1-\epsilon, 1+\epsilon)\hat{A}_t\right)\right]$$

其中优势函数：
$$\hat{A}_t = r(x,y) - \beta \cdot \text{KL}[\pi_\theta \| \pi_{ref}]$$

#### KL 散度约束
- 防止策略偏离参考模型太远
- β 控制约束强度
- 过大 → 模型不变化；过小 → reward hacking

### 2.4 RLHF 的挑战

| 问题 | 描述 | 解决方案 |
|------|------|----------|
| Reward Hacking | 模型找到 RM 漏洞获得高分 | 在线更新 RM、KL 约束 |
| 训练不稳定 | PPO 超参敏感 | 仔细调参、多次实验 |
| 成本高昂 | 需要 4 个模型同时加载 | LoRA、共享参数 |
| 人类偏见 | 标注者偏好不一致 | 多人标注、质量控制 |

## 3. Constitutional AI (CAI)

### 3.1 核心思想
- 用一组原则 (Constitution) 替代人类反馈
- 模型自我批评和修正

### 3.2 流程
1. 模型生成有害回答
2. 模型根据原则自我批评
3. 模型根据批评修正回答
4. 用 (有害回答, 修正回答) 偏好对训练 RM
5. PPO 训练

### 3.3 代表
- Claude 系列（Anthropic 首创）

## 4. 实践建议

### 4.1 SFT 阶段
- 数据质量 > 数据数量
- 1000 条高质量数据 > 10000 条低质量数据
- 多样化的任务类型

### 4.2 RLHF 阶段
- RM 质量是关键
- 偏好标注一致性检查
- β 值通常 0.01-0.1
- PPO batch size 建议 512+

## 5. 学习要点

- [ ] 理解 SFT 的数据构造和训练流程
- [ ] 理解 RLHF 三阶段的原理
- [ ] 理解 PPO 中的 KL 约束
- [ ] 了解 Reward Hacking 问题
- [ ] 理解 Constitutional AI 的创新点
