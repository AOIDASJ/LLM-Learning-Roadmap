# 推理加速技术 ⭐

## 1. 推理瓶颈分析

### 1.1 自回归生成的瓶颈
- 每生成一个 token 都需要读取全部模型参数
- 生成阶段是 **内存带宽受限 (Memory-Bound)**，而非计算受限
- 单用户推理：GPU 利用率可能 <5%

### 1.2 关键指标

| 指标 | 全称 | 含义 |
|------|------|------|
| TTFT | Time to First Token | 首 token 延迟 |
| TPOT | Time per Output Token | 每 token 生成时间 |
| Throughput | tokens/second | 吞吐量 |
| Latency | 总延迟 | TTFT + TPOT × 输出长度 |

### 1.3 Prefill vs Decode

| 阶段 | 计算 | 特点 |
|------|------|------|
| Prefill | 处理输入 prompt | 计算密集型 (Compute-Bound) |
| Decode | 逐 token 生成 | 内存密集型 (Memory-Bound) |

## 2. KV Cache 优化

### 2.1 KV Cache 原理
- 缓存已计算的 Key, Value，避免重复计算
- 空间复杂度：$O(n_{layers} \times n_{heads} \times d_k \times seq\_len \times 2)$

### 2.2 KV Cache 显存估算
$$\text{KV Cache Size} = 2 \times n_{layers} \times d_{model} \times seq\_len \times \text{sizeof(dtype)}$$

| 模型 | 上下文长度 | KV Cache (FP16) |
|------|-----------|-----------------|
| 7B | 4K | ~2GB |
| 70B | 4K | ~16GB |
| 70B | 128K | ~512GB |

### 2.3 PagedAttention ⭐

#### 问题
- 传统 KV Cache：预分配最大长度 → 显存浪费/碎片
- 实际序列长度变化大

#### 方案
- 将 KV Cache 分成固定大小的 Page（类似 OS 虚拟内存）
- 按需分配 Page
- 支持跨请求共享（Prefix Caching）

```
传统: [====      ====        ]  大量碎片
Paged: [P1][P2][P3]... 按需分配
```

### 2.4 KV Cache 压缩
- **量化**：INT8/INT4 KV Cache
- **稀疏化**：只保留重要 token 的 KV（H2O, Scissorhands）
- **蒸馏**：用小模型蒸馏 KV（不是标准做法）

## 3. FlashAttention ⭐

### 3.1 核心思想
- 标准 Attention：需要将 N×N 注意力矩阵写入 HBM
- FlashAttention：分块 (Tiling) 计算，利用 GPU SRAM

### 3.2 计算流程
```
标准: Q,K,V → HBM 读写 N² 注意力矩阵 → 慢
Flash: Q,K,V → 分块 → SRAM 内计算 → 只写结果 → 快
```

### 3.3 效果
| 指标 | 标准 Attention | FlashAttention-2 |
|------|---------------|-----------------|
| 显存 | O(N²) | O(N) |
| 速度 | 1x | 2-4x |
| 支持 | 任意 | 需 CUDA |

### 3.4 FlashAttention-2 改进
- 优化线程分配
- 减少 non-matmul FLOPs
- 支持 GQA/MQA

### 3.5 FlashAttention-3
- 针对 H100 优化
- 异步 + FP8 支持

## 4. Speculative Decoding (投机解码) ⭐

### 4.1 核心思想
- 用小模型 (Draft Model) 快速生成候选 token
- 大模型 (Target Model) 并行验证
- 接受正确 token，拒绝错误 token

### 4.2 工作流
```
1. Draft Model 自回归生成 k 个候选 token [t1, t2, t3, t4, t5]
2. Target Model 一次前向验证所有候选
3. 假设 t1-t3 正确，t4 错误
4. 接受 t1-t3，从 t4 位置重新采样
5. 每步实际生成 3-4 个 token
```

### 4.3 加速比
- 理论加速：取决于 draft model 的准确率
- 实际加速：1.5x - 3x
- Draft model 越准确 → 加速越高

### 4.4 变体

| 变体 | Draft 来源 | 特点 |
|------|-----------|------|
| Standard SpecDec | 小模型 | 需要额外模型 |
| Self-Speculative | 同模型早退层 | 无需额外模型 |
| Medusa | 多头预测 | 添加额外预测头 |
| EAGLE | 特征级预测 | 更高效的 draft |
| Lookahead Decoding | Jacobi 迭代 | 无需额外模型 |

### 4.5 Medusa
- 在原模型上添加多个额外预测头
- 每个头独立预测未来第 i 个 token
- 不需要额外的 draft model

```python
# Medusa 头结构
class MedusaHead(nn.Module):
    def __init__(self, d_model, vocab_size):
        self.linear = nn.Linear(d_model, vocab_size)

# 原模型输出 → Medusa Head 0 → 下一个 token
#             → Medusa Head 1 → 下下个 token
#             → Medusa Head 2 → 下下下个 token
```

## 5. Continuous Batching ⭐

### 5.1 Static Batching 的问题
- 一个 batch 中所有序列必须等最长的完成
- 短序列完成后 GPU 空闲

### 5.2 Continuous Batching
- 序列完成后立即移出 batch
- 新序列立即填入空位
- GPU 始终保持满载

```
Static:  [Seq1 (完成)...等待........] [Seq2......(完成)....等待]
Continuous: [Seq1][Seq3][Seq5] [Seq2][Seq4][Seq6]  ← 不断替换
```

### 5.3 Iteration-level Scheduling
- 每次 forward 前重新决定 batch 组成
- 实现：vLLM, TGI, TensorRT-LLM

## 6. 模型并行推理

### 6.1 Tensor Parallelism (推理)
- 与训练 TP 类似
- 需要节点内 NVLink
- vLLM 原生支持

### 6.2 Pipeline Parallelism (推理)
- Micro-batching 流水线
- 延迟较高，不推荐

### 6.3 分层推理 (Layer-wise Offloading)
- GPU 放不下的层 offload 到 CPU
- 速度慢（CPU-GPU 数据传输）
- 适合无卡或小卡场景

## 7. 长上下文优化

### 7.1 上下文扩展方法

| 方法 | 原理 | 可扩展范围 |
|------|------|-----------|
| NTK-Aware Scaling | 修改 RoPE 基频 | 4-8x |
| YaRN | NTK + 温度缩放 | 8-16x |
| ALiBi | 线性偏置，天然外推 | 2-4x |
| LongRoPE | 优化 RoPE 缩放 | 8x+ |

### 7.2 长上下文推理挑战
- KV Cache 线性增长
- Attention O(N²) 计算复杂度
- 解决：稀疏注意力、滑动窗口

### 7.3 滑动窗口注意力 (Sliding Window Attention)
- Mistral 使用：窗口大小 4096
- 每层只关注最近 W 个 token
- 信息通过多层传播获取更远上下文

## 8. 推理加速总结

| 技术 | 加速方向 | 加速幅度 | 复杂度 |
|------|----------|----------|--------|
| FlashAttention | Prefill 加速 | 2-4x | 低 |
| KV Cache + PagedAttn | 显存效率 | 2-4x 并发 | 中 |
| Continuous Batching | 吞吐量 | 2-5x | 中 |
| Speculative Decoding | 延迟 | 1.5-3x | 中-高 |
| 量化 | 全方位 | 2-4x | 低 |
| GQA/MQA | KV Cache | 减少 4-8x | 需训练 |

## 9. 学习要点

- [ ] 理解推理的内存带宽瓶颈
- [ ] 掌握 PagedAttention 原理
- [ ] 理解 FlashAttention 的分块计算
- [ ] 掌握 Speculative Decoding 工作流
- [ ] 理解 Continuous Batching
- [ ] 了解长上下文优化方案
