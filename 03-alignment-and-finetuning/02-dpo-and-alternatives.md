# DPO 及替代方案

## 1. DPO (Direct Preference Optimization) ⭐

### 1.1 核心思想
**跳过奖励模型**，直接从偏好数据优化策略模型

### 1.2 数学推导

RLHF 的最优策略：
$$\pi^*(y|x) = \frac{1}{Z(x)}\pi_{ref}(y|x) \exp\left(\frac{1}{\beta}r(x,y)\right)$$

由此反推奖励函数：
$$r(x,y) = \beta \log \frac{\pi^*(y|x)}{\pi_{ref}(y|x)} + \beta \log Z(x)$$

代入 Bradley-Terry 模型，消除 Z(x)：
$$\mathcal{L}_{DPO} = -\mathbb{E}\left[\log \sigma\left(\beta \log \frac{\pi_\theta(y_w|x)}{\pi_{ref}(y_w|x)} - \beta \log \frac{\pi_\theta(y_l|x)}{\pi_{ref}(y_l|x)}\right)\right]$$

### 1.3 DPO vs RLHF

| 方面 | RLHF | DPO |
|------|------|-----|
| 模型数量 | 4 个 (Actor, Critic, RM, Ref) | 2 个 (Policy, Ref) |
| 训练复杂度 | 高 (PPO 不稳定) | 低 (标准 SFT 式训练) |
| 在线/离线 | 在线 (需要采样) | 离线 (固定数据集) |
| 效果 | 强 (可在线迭代) | 接近 (需要好数据) |
| 计算成本 | 高 | 低 |

### 1.4 DPO 实践要点
- **β 值**：0.1-0.5，太大会欠拟合，太小会过拟合
- **参考模型**：必须是 SFT 后的模型
- **数据质量**：偏好对比必须清晰明确
- **长度偏差**：DPO 倾向于生成更长的回答，需长度归一化

### 1.5 DPO 变体

#### IPO (Identity Preference Optimization)
- 用更稳健的损失替代 logistic 损失
- 更不容易过拟合
$$\mathcal{L}_{IPO} = \mathbb{E}\left[\left(h(x,y_w,y_l) - \frac{1}{2\beta}\right)^2\right]$$

#### KTO (Kahneman-Tversky Optimization)
- 只需要好/坏标签，不需要成对偏好
- 数据效率更高
$$\mathcal{L}_{KTO} = \begin{cases} \lambda_w \cdot (1 - v(x,y)) & \text{if good} \\ \lambda_l \cdot v(x,y) & \text{if bad} \end{cases}$$

#### ORPO (Odds Ratio Preference Optimization)
- 将 SFT 和对齐合并为一步
- 无需参考模型
$$\mathcal{L}_{ORPO} = \mathcal{L}_{SFT} + \lambda \cdot \mathcal{L}_{OR}$$

#### SimPO (Simple Preference Optimization)
- 用序列平均 log 概率替代参考模型
- 更简单，效果相当

## 2. RLHF 的在线改进

### 2.1 Online RLHF (迭代式)
1. 用当前策略采样
2. 人类/RM 标注偏好
3. 更新 RM
4. 更新策略
5. 重复

**代表**：Llama 3 使用 5 轮迭代 RLHF

### 2.2 Self-Play RL
- 模型与自身博弈
- 逐步提升回答质量
- **代表**：SPIN (Self-Play Fine-Tuning)

### 2.3 GRPO (Group Relative Policy Optimization)
- DeepSeek 提出
- 组内采样多个回答，组内相对排名作为奖励
- 无需单独的奖励模型
- **代表**：DeepSeek-R1

## 3. 对齐方法选择指南

```
需要快速原型？  → DPO
有大量偏好数据？  → DPO / IPO
只有好坏标签？  → KTO
想一步到位？  → ORPO
有在线标注能力？  → Online RLHF
追求极致效果？  → 迭代 RLHF
```

## 4. 学习要点

- [ ] 理解 DPO 的数学推导
- [ ] 区分 DPO / IPO / KTO / ORPO
- [ ] 理解 DPO 的长度偏差问题
- [ ] 了解在线 RLHF 的优势
- [ ] 能够根据场景选择对齐方法
