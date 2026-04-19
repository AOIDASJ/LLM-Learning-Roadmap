# 量化技术详解 ⭐

## 1. 量化基础

### 1.1 什么是量化
将高精度（FP32/FP16）权重映射到低精度（INT8/INT4/INT1），减少存储和计算开销。

### 1.2 量化分类

| 类型 | 描述 | 是否需要数据 | 精度损失 |
|------|------|-------------|----------|
| PTQ (Post-Training Quantization) | 训练后量化 | 少量校准数据 | 较大 |
| QAT (Quantization-Aware Training) | 量化感知训练 | 需要训练数据 | 较小 |

### 1.3 基本量化公式

**对称量化：**
$$x_q = \text{round}\left(\frac{x}{s}\right), \quad s = \frac{\max(|x|)}{2^{b-1}-1}$$

**非对称量化：**
$$x_q = \text{round}\left(\frac{x - z}{s}\right), \quad s = \frac{x_{max} - x_{min}}{2^b - 1}, \quad z = \text{round}\left(-\frac{x_{min}}{s}\right)$$

### 1.4 量化粒度

| 粒度 | 精度 | 效率 | 描述 |
|------|------|------|------|
| Per-tensor | 低 | 最高 | 整个张量一个缩放因子 |
| Per-channel | 中 | 高 | 每个通道一个缩放因子 |
| Per-group | 高 | 中 | 每 128 列一组缩放因子 |
| Per-element | 最高 | 最低 | 每个元素独立（无意义） |

## 2. LLM 量化的挑战

### 2.1 异常值问题
- LLM 中少量权重/激活值远大于其他（Outlier）
- 这些 Outlier 对量化精度影响巨大
- **关键发现**：超过 6.7B 参数后，激活中出现系统性 Outlier

### 2.2 权重量化 vs 激活量化

| 量化对象 | 难度 | 原因 |
|----------|------|------|
| 权重 (Weight) | 容易 | 静态分布，可离线校准 |
| 激活 (Activation) | 困难 | 动态分布，依赖输入，有 Outlier |
| KV Cache | 中等 | Key 有 Outlier，Value 较好 |

### 2.3 W8A8 / W4A16 等表示法
- WxAy：W=权重位宽，A=激活位宽
- W8A8：权重 INT8 + 激活 INT8
- W4A16：权重 INT4 + 激活 FP16（最常见）
- W4A8：权重 INT4 + 激活 INT8（更激进）

## 3. 主流量化方法

### 3.1 LLM.int8() (2022)
- **方法**：混合精度分解
  - 90% 的维度用 INT8 计算
  - 10% 的 Outlier 维度用 FP16 计算
- **效果**：几乎无精度损失
- **限制**：速度提升有限（混合精度开销）

### 3.2 GPTQ ⭐ (2022)

#### 核心思想
基于 OBQ (Optimal Brain Quantization) 的逐层量化：

1. 逐行量化权重矩阵
2. 每量化一个权重，用 Hessian 信息调整未量化权重
3. 补偿量化误差

#### 关键改进
- Lazy Batch 更新：批量处理提高效率
- Cholesky 分解：稳定 Hessian 逆计算

#### 实践
```python
from auto_gptq import AutoGPTQForCausalLM

model = AutoGPTQForCausalLM.from_quantized(
    "TheBloke/Llama-2-7B-GPTQ",
    model_basename="model",
    use_safetensors=True,
    device="cuda:0",
    quantize_config=None  # 使用模型内置配置
)
```

#### GPTQ 量化等级
| 配置 | 描述 | 7B 大小 |
|------|------|---------|
| GPTQ-8bit | 8bit 量化 | ~7GB |
| GPTQ-4bit (128g) | 4bit, group=128 | ~4GB |
| GPTQ-3bit | 3bit 量化 | ~3GB |

### 3.3 AWQ (Activation-Aware Weight Quantization) ⭐ (2023)

#### 核心思想
- 不是所有权重同等重要
- 重要性由激活值决定：**激活值大的通道对应的权重更重要**
- 对重要权重乘以缩放因子后再量化

#### 方法
$$\text{scale} \cdot W = \text{scale} \cdot Q(W) \neq Q(\text{scale} \cdot W)$$

AWQ 选择最优缩放因子使量化误差最小：
$$s^* = \arg\min_s \left\| Q(s \cdot W) \cdot X - s \cdot W \cdot X \right\|$$

#### 优势
- 无需反向传播/重构（比 GPTQ 快）
- 比相同位宽的 GPTQ 精度更好
- 支持任意量化位宽

```python
from awq import AutoAWQForCausalLM

model = AutoAWQForCausalLM.from_pretrained("model_name")
tokenizer = AutoTokenizer.from_pretrained("model_name")

quant_config = { "zero_point": True, "q_group_size": 128, "w_bit": 4 }
model.quantize(tokenizer, quant_config=quant_config)
model.save_quantized("model-awq-4bit")
```

### 3.4 SmoothQuant (2022)

#### 核心思想
- **问题**：激活有 Outlier → 激活量化困难
- **方案**：将激活的难度"转移"到权重
- 数学上等价于逐通道缩放

$$Y = (X \cdot \text{diag}(s)^{-1}) \cdot (\text{diag}(s) \cdot W) = \hat{X} \cdot \hat{W}$$

- 选择 s 使 $\hat{X}$ 和 $\hat{W}$ 的量化难度均衡
- 迁移强度 α：$s_j = \max(|X_j|)^\alpha / \max(|W_j|)^{1-\alpha}$

#### 效果
- 实现 W8A8 量化几乎无精度损失
- 适合推理加速（INT8 矩阵乘法）

### 3.5 GGUF 量化 (llama.cpp)

#### 特点
- CPU/CPU+GPU 混合推理
- 多种量化等级
- k-quant 方法：关键层高精度，非关键层低精度

#### 量化等级详解
| 类型 | 方法 | 质量排名 |
|------|------|----------|
| Q8_0 | 均匀 8-bit | 1 (最好) |
| Q6_K | 6-bit k-quant | 2 |
| Q5_K_M | 5-bit k-quant (medium) | 3 |
| Q5_0 | 均匀 5-bit | 4 |
| Q4_K_M | 4-bit k-quant (medium) | 5 |
| Q4_0 | 均匀 4-bit | 6 |
| Q3_K_M | 3-bit k-quant (medium) | 7 |
| Q2_K | 2-bit k-quant | 8 |

### 3.6 BitNet / 1.58-bit (2023-2024)

#### BitNet b1.58
- 权重仅取 {-1, 0, +1} 三值
- $\log_2(3) \approx 1.58$ bit
- 用加法替代矩阵乘法
- 训练时用直通估计器 (STE)

#### 效果
- 与 FP16 同等规模模型性能相当
- 推理速度大幅提升
- 显存降低 10x+

## 4. KV Cache 量化

### 4.1 动机
- 长序列场景下 KV Cache 占用大量显存
- 70B 模型，128K 上下文：KV Cache ~30GB

### 4.2 方法
- **KV8**：KV Cache 用 INT8 存储
- **KV4**：KV Cache 用 INT4 存储
- **分组量化**：每 128 个 token 一组缩放因子

### 4.3 注意事项
- Key 比 Value 更难量化（有 Outlier）
- 4-bit KV Cache 质量损失需要注意
- vLLM / TensorRT-LLM 已支持 KV Cache 量化

## 5. 量化效果对比

### 5.1 7B 模型量化对比 (MMLU)

| 方法 | 位宽 | MMLU | 显存 |
|------|------|------|------|
| FP16 | 16 | 63.2 | 14GB |
| GPTQ-8bit | 8 | 62.8 | 7GB |
| GPTQ-4bit | 4 | 61.5 | 4GB |
| AWQ-4bit | 4 | 62.0 | 4GB |
| GGUF-Q4_K_M | ~4.5 | 61.8 | 4GB |
| SmoothQuant W8A8 | 8/8 | 62.5 | 7GB |

### 5.2 选择建议

```
追求最高精度？ → AWQ-4bit / GPTQ-8bit
追求最快推理？ → SmoothQuant W8A8 + TensorRT
CPU 端推理？   → GGUF Q4_K_M / Q5_K_M
极致显存压缩？ → GGUF Q2_K / Q3_K_M
前沿探索？     → BitNet 1.58-bit
```

## 6. 量化实践工具

| 工具 | 支持 | 特点 |
|------|------|------|
| AutoGPTQ | GPTQ | 成熟稳定 |
| AutoAWQ | AWQ | 速度快 |
| llama.cpp | GGUF | CPU 友好 |
| TensorRT-LLM | INT8/INT4 | 极致性能 |
| bitsandbytes | 8bit/4bit | QLoRA 基础 |
| Quanto # | 多种 | HuggingFace 新工具 |

## 7. 学习要点

- [ ] 理解对称/非对称量化
- [ ] 掌握 GPTQ 和 AWQ 的原理与区别
- [ ] 理解 SmoothQuant 的迁移思想
- [ ] 了解 BitNet 三值网络
- [ ] 实践模型量化部署
