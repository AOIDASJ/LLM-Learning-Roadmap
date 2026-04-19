# 训练目标与损失函数

## 1. 预训练目标

### 1.1 Causal Language Modeling (CLM) / 自回归语言模型
- **任务**：给定前文，预测下一个 token
- **损失**：交叉熵损失
$$\mathcal{L}_{CLM} = -\sum_{t=1}^{T} \log P(x_t | x_{<t})$$
- **代表**：GPT 系列, LLaMA, Claude

### 1.2 Masked Language Modeling (MLM)
- **任务**：遮蔽部分 token，预测被遮蔽的 token
- **损失**：仅计算被遮蔽位置的交叉熵
$$\mathcal{L}_{MLM} = -\sum_{t \in \text{masked}} \log P(x_t | x_{\backslash \text{masked}})$$
- **代表**：BERT, RoBERTa

### 1.3 Prefix Language Modeling
- **任务**：前缀部分双向注意力，生成部分单向注意力
- **代表**：T5, UniLM

### 1.4 Denoising Objective
- **任务**：输入带噪声的序列，恢复原始序列
- **噪声类型**：span masking, token masking, token deletion
- **代表**：BART, T5

## 2. 微调训练目标

### 2.1 Supervised Fine-Tuning (SFT)
- **目标**：在指令-回答对上做 CLM
- **损失**：仅计算回答部分的交叉熵（忽略指令部分）
$$\mathcal{L}_{SFT} = -\sum_{t \in \text{response}} \log P(y_t | x, y_{<t})$$

### 2.2 Reward Modeling
- **目标**：学习人类偏好评分
- **损失**：Bradley-Terry 模型
$$\mathcal{L}_{RM} = -\log \sigma(r(x, y_w) - r(x, y_l))$$
其中 $y_w$ 是偏好回答，$y_l$ 是不偏好回答

### 2.3 RLHF (PPO)
- **目标**：最大化奖励同时约束偏离参考模型
$$\mathcal{L}_{RLHF} = -\mathbb{E}[r(x,y)] + \beta \cdot \text{KL}[\pi_\theta \| \pi_{ref}]$$

### 2.4 DPO (Direct Preference Optimization)
- **目标**：直接从偏好数据优化策略，无需奖励模型
$$\mathcal{L}_{DPO} = -\mathbb{E}\left[\log \sigma\left(\beta \log \frac{\pi_\theta(y_w|x)}{\pi_{ref}(y_w|x)} - \beta \log \frac{\pi_\theta(y_l|x)}{\pi_{ref}(y_l|x)}\right)\right]$$

## 3. 辅助损失

### 3.1 辅助预测头
- **Next Sentence Prediction (NSP)**：BERT 的辅助任务
- **Sentence Order Prediction (SOP)**：ALBERT 替代 NSP

### 3.2 正则化
- **Label Smoothing**：软化 one-hot 标签
- **Dropout**：随机丢弃神经元
- **Weight Decay**：L2 正则化

## 4. 损失函数深入

### 4.1 Cross-Entropy Loss
$$\mathcal{L}_{CE} = -\sum_{i=1}^{V} y_i \log \hat{y}_i$$
- $V$ = 词表大小，$y$ = one-hot 真实分布，$\hat{y}$ = 预测概率

### 4.2 KL Divergence
$$D_{KL}(P \| Q) = \sum_{i} P(i) \log \frac{P(i)}{Q(i)}$$
- 衡量两个分布的差异
- RLHF 中用于约束策略偏离

### 4.3 Focal Loss
$$\mathcal{L}_{FL} = -\alpha_t (1 - p_t)^\gamma \log(p_t)$$
- 解决类别不平衡问题
- $\gamma > 0$ 降低易分样本权重

## 5. 学习要点

- [ ] 理解 CLM 和 MLM 的区别
- [ ] 理解 SFT 损失的计算范围（仅 response）
- [ ] 推导 DPO 损失与 RLHF 的等价关系
- [ ] 理解 KL 散度在 RLHF 中的作用
- [ ] 实现不同训练目标的损失计算
