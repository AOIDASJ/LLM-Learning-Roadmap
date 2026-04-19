# LoRA / QLoRA 参数高效微调

## 1. 为什么需要 PEFT (Parameter-Efficient Fine-Tuning)

### 1.1 全量微调的问题
- 7B 模型全量微调需要 ~28GB 显存（仅参数）
- 加上梯度、优化器状态：~100GB+
- 大多数人无法承担

### 1.2 PEFT 的目标
- 只训练少量参数（<1% 原始参数）
- 效果接近全量微调
- 显存需求大幅降低

## 2. LoRA (Low-Rank Adaptation) ⭐

### 2.1 核心思想
- 冻结原始权重 $W$
- 添加低秩分解矩阵 $\Delta W = BA$
- $B \in \mathbb{R}^{d_{out} \times r}$, $A \in \mathbb{R}^{r \times d_{in}}$
- $r \ll \min(d_{in}, d_{out})$

$$h = Wx + \Delta Wx = Wx + BAx$$

### 2.2 关键设计
- **初始化**：A 用高斯初始化，B 初始化为零 → $\Delta W = BA = 0$，训练开始等于原始模型
- **缩放**：$\Delta W = \frac{\alpha}{r} BA$，α 控制更新幅度
- **推理合并**：$W_{merged} = W + \frac{\alpha}{r} BA$，推理无额外开销

### 2.3 超参数选择

| 参数 | 建议值 | 说明 |
|------|--------|------|
| rank (r) | 8-64 | 任务简单用小 r，复杂用大 r |
| alpha (α) | 16-32 | 通常 α = 2r |
| 目标模块 | Q, V / 全部 | 微 Q,V 即可，全量更优 |

### 2.4 LoRA 应用位置

```
标准选择: 仅 Attention 的 Q, V
         → 参数少，效果好

扩展选择: Attention 的 Q, K, V, O + FFN 的 gate, up, down
         → 参数多，效果更好

最新研究: 只微调 Embedding 和 LM Head
         → 极少参数，意外有效
```

## 3. QLoRA ⭐

### 3.1 核心思想
- 将基座模型量化为 4-bit
- 在量化模型上添加 LoRA
- LoRA 参数保持高精度（BF16）

### 3.2 三大创新

#### 4-bit NormalFloat (NF4)
- 专为正态分布权重设计的数据类型
- 信息论最优量化

#### Double Quantization
- 对量化常数再次量化
- 每参数节省 0.37 bit 显存

#### Paged Optimizers
- 利用 CPU 内存分页管理优化器状态
- 避免显存溢出

### 3.3 显存对比

| 方法 | 7B 模型显存 | 65B 模型显存 |
|------|------------|------------|
| 全量微调 (BF16) | ~120GB | ~1.2TB |
| LoRA (BF16) | ~30GB | ~260GB |
| QLoRA (4-bit) | **~6GB** | **~48GB** |

### 3.4 QLoRA 效果
- 在多数任务上与全量微调差距 <1%
- 65B 模型可在单张 A100 上微调

## 4. 其他 PEFT 方法

### 4.1 Adapter
- 在 Transformer 层中插入小型 Adapter 模块
- 结构：Down → Nonlinear → Up
- 推理有额外开销

### 4.2 Prefix-Tuning
- 在每层添加可学习的 prefix 向量
- 不修改模型参数
- 占用输入长度

### 4.3 Prompt Tuning
- 仅在输入层添加可学习 prompt
- 最简单，效果有限

### 4.4 IA³
- 通过向量缩放 Infused Attention 和 FFN
- 比 LoRA 更少参数

### 4.5 DoRA (Weight-Decomposed Low-Rank Adaptation)
- 将权重分解为幅度和方向
- 方向上应用 LoRA
- 效果优于 LoRA

## 5. LoRA 实践

### 5.1 训练代码框架
```python
from peft import LoraConfig, get_peft_model

config = LoraConfig(
    r=16,
    lora_alpha=32,
    target_modules=["q_proj", "v_proj", "k_proj", "o_proj",
                    "gate_proj", "up_proj", "down_proj"],
    lora_dropout=0.05,
    task_type="CAUSAL_LM",
)

model = get_peft_model(base_model, config)
model.print_trainable_parameters()
# trainable params: 13M || all params: 6.7B || trainable%: 0.19%
```

### 5.2 合并与导出
```python
model.merge_and_unload()  # 合并 LoRA 权重
model.save_pretrained("merged_model")
```

### 5.3 多 LoRA 服务
- vLLM / LoRAX 支持同时服务多个 LoRA
- 基座模型只加载一次
- 不同请求动态切换 LoRA

## 6. 学习要点

- [ ] 理解 LoRA 的低秩分解原理
- [ ] 掌握 QLoRA 的三大创新
- [ ] 理解 LoRA 超参数选择
- [ ] 实践 LoRA 微调
- [ ] 了解多 LoRA 部署
