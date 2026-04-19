# Scaling Laws

## 1. Kaplan Scaling Laws (2020)

### 核心发现
模型性能与三个因素呈幂律关系：
- **参数量 N**
- **数据量 D**
- **计算量 C**

$$L(N) = \left(\frac{N_c}{N}\right)^{\alpha_N}, \quad \alpha_N \approx 0.076$$
$$L(D) = \left(\frac{D_c}{D}\right)^{\alpha_D}, \quad \alpha_D \approx 0.095$$
$$L(C) = \left(\frac{C_c}{C}\right)^{\alpha_C}, \quad \alpha_C \approx 0.050$$

### 关键结论
- 更大的模型更样本高效
- 固定计算预算下，训练大模型 fewer steps 优于小模型 more steps
- 暗示当时大多数模型 undertrained

## 2. Chinchilla Scaling Laws (2022) ⭐

### 核心修正
**最优模型大小和数据量应该等比例增长！**

$$N_{opt}(C) \propto C^{0.5}, \quad D_{opt}(C) \propto C^{0.5}$$

### 实践意义
| 计算量 (FLOPs) | Kaplan 最优参数 | Chinchilla 最优参数 | 最优数据量 |
|-----------------|-----------------|---------------------|-----------|
| 10^21 | 16B | 1B | 20B tokens |
| 10^22 | 67B | 10B | 200B tokens |
| 10^23 | 280B | 67B | 1.5T tokens |

### 影响
- GPT-3 (175B) 训练 300B tokens → **严重 undertrained**
- Chinchilla (70B) 训练 1.4T tokens → 同等计算更优
- LLaMA 系列采用 Chinchilla 配方训练

## 3. LLaMA 的反 Chinchilla 策略

### 动机
- 推理成本比训练成本更重要
- 训练一次，推理无数次

### 策略
- 用更多数据训练较小模型
- LLaMA-1 7B: 1T tokens (Chinchilla 建议 ~70B)
- 训练成本增加，但推理效率大幅提升

## 4. Emergent Abilities (涌现能力)

### 定义
当模型规模超过某个阈值时，某些能力突然出现

### 已观察到的涌现
- Few-shot 学习：~10B 参数
- Chain-of-Thought：~100B 参数
- 指令遵循：需要 SFT/RLHF

### 争议
- 部分研究认为涌现是评估指标的非线性导致
- 线性指标下可能更平滑

## 5. 过拟合与数据需求

### 重复数据的影响
- 1 epoch 训练最优
- 重复数据导致过拟合
- 数据去重至关重要

### 数据质量 vs 数据量
- 质量提升 > 数量增加（Phi 系列证明）
- 高质量数据过滤（perplexity 过滤、质量分类器）

## 6. 计算量估算

$$C \approx 6ND$$
- $N$ = 模型参数量
- $D$ = 训练 token 数
- 6 = 前向 2 + 反向 4

### 示例
- LLaMA-2 70B: C ≈ 6 × 70B × 2T = 8.4 × 10²³ FLOPs
- A100 理论算力: 312 TFLOPS (BF16)
- 所需 A100 小时: 8.4×10²³ / (312×10¹² × 3600) ≈ 747,000 小时

## 7. 学习要点

- [ ] 理解 Kaplan 和 Chinchilla Scaling Laws 的差异
- [ ] 理解 LLaMA 的反 Chinchilla 策略
- [ ] 计算训练所需的计算量
- [ ] 理解涌现能力与争议
- [ ] 理解数据质量对 Scaling 的影响
