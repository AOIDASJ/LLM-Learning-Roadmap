# 分布式训练

## 1. 并行策略概览

```
┌─────────────────────────────────────────────┐
│              3D Parallelism                  │
│                                              │
│  数据并行 (DP) × 张量并行 (TP) × 流水线并行 (PP) │
└─────────────────────────────────────────────┘
```

## 2. 数据并行 (Data Parallelism)

### 2.1 标准 DP
- 每张卡持有完整模型副本
- 数据分片，梯度聚合 (AllReduce)
- 限制：单卡放不下模型时无法使用

### 2.2 ZeRO (Zero Redundancy Optimizer) ⭐

| 阶段 | 切分内容 | 显存节省 | 通信量 |
|------|----------|----------|--------|
| ZeRO-1 | 优化器状态 | 4x | =DP |
| ZeRO-2 | + 梯度 | 8x | =DP |
| ZeRO-3 | + 模型参数 | N倍 | 1.5x DP |

```python
# ZeRO-3 工作流
forward:
  参数 All-Gather → 前向计算 → 释放非本地参数
backward:
  参数 All-Gather → 反向计算 → 梯度 Reduce-Scatter → 释放参数
```

### 2.3 FSDP (Fully Sharded Data Parallel)
- PyTorch 原生实现的 ZeRO-3
- 更好的易用性和与 PyTorch 生态集成

## 3. 张量并行 (Tensor Parallelism)

### 3.1 原理
将单个算子的参数切分到多张卡上

#### MLP 的张量并行
```
标准: Y = GeLU(XA)B       A: [h, 4h], B: [4h, h]
TP:  Y₁ = GeLU(XA₁)B₁    A₁: [h, 4h/N], B₁: [4h/N, h]
     Y₂ = GeLU(XA₂)B₂
     Y  = Y₁ + Y₂          # AllReduce
```

#### Attention 的张量并行
- 按注意力头切分 Q, K, V
- 每张卡计算部分头

### 3.2 通信
- 每层需要 2 次 AllReduce (forward + backward)
- 通信量与 TP 度成正比
- **建议**：仅在节点内使用 TP（NVLink 高带宽）

## 4. 流水线并行 (Pipeline Parallelism)

### 4.1 原理
将模型按层切分到不同卡上，形成流水线

### 4.2 调度策略

| 策略 | Bubble 率 | 显存 | 说明 |
|------|-----------|------|------|
| GPipe | (P-1)/M | 高 | 同步，全批次 |
| 1F1B | (P-1)/(M+P-1) | 中 | 交替前向后向 |
| Interleaved 1F1B | 更低 | 中 | 微块交错 |

```
GPipe (P=4, M=8):
F F F F F F F F B B B B B B B B  ← 高 Bubble
1F1B:
F F F F B F B F B F B F B B B B  ← 低 Bubble
```

### 4.3 限制
- Bubble 无法完全消除
- 层间依赖导致无法完全并行
- **建议**：跨节点使用 PP

## 5. 序列并行 (Sequence Parallelism)

### 5.1 动机
- 长序列训练时，激活显存是瓶颈
- LayerNorm / Dropout 的激活不依赖注意力

### 5.2 方法
- 将序列维度切分到 TP 组内
- LayerNorm/Dropout 的激活按序列分片
- Attention 仍需 All-Gather

### 5.3 代表
- Megatron-LM Sequence Parallelism
- Ring Attention（无限长序列）

## 6. 混合精度训练

### 6.1 FP16 混合精度
```
FP32 权重 → FP16 前向 → FP32 损失 → FP16 反向 → FP32 更新
```
- 问题：溢出风险、梯度下溢
- 解决：Loss Scaling（放大损失，减小梯度下溢）

### 6.2 BF16 混合精度 ⭐
- 动态范围与 FP32 相同（8 bit 指数）
- 精度较低（7 bit 尾数 vs FP16 10 bit）
- 无需 Loss Scaling
- **现代训练首选**

### 6.3 FP8 训练
- 进一步减少显存和通信量
- 需要硬件支持 (H100)
- 仍处于实验阶段

## 7. 显存优化

### 7.1 梯度检查点 (Gradient Checkpointing)
- 丢弃部分中间激活，反向时重计算
- 用计算换显存：显存降低 60-70%，速度慢 20-30%

### 7.2 激活重计算
- 选择性重计算：只重计算 Attention（便宜），保留 MLP 激活（贵）

## 8. 训练框架

| 框架 | 特点 |
|------|------|
| Megatron-LM | 3D 并行先驱，NVIDIA |
| DeepSpeed | ZeRO 优化，微软 |
| FSDP | PyTorch 原生 |
| Megatron-Deepspeed | Megatron+DeepSpeed 融合 |
| vLLM (训练) | 训练+推理一体化 |

## 9. 学习要点

- [ ] 理解 DP/TP/PP 的原理与适用场景
- [ ] 理解 ZeRO 各阶段的切分策略
- [ ] 掌握 BF16 混合精度训练
- [ ] 理解梯度检查点的权衡
- [ ] 能够规划模型的并行训练方案
