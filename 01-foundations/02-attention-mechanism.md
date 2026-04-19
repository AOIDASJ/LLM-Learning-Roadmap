# 注意力机制详解

## 1. Scaled Dot-Product Attention

$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$$

### 为什么除以 $\sqrt{d_k}$？
当 $d_k$ 较大时，点积结果方差增大，softmax 进入梯度饱和区。缩放因子使方差归一化。

## 2. Multi-Head Attention (MHA)

```python
# 伪代码
Q = X @ W_q  # [batch, seq, d_model]
K = X @ W_k
V = X @ W_v

# 拆分多头
Q = Q.reshape(batch, seq, n_heads, d_k).transpose(1, 2)  # [batch, n_heads, seq, d_k]
K = K.reshape(batch, n_heads, seq, d_k).transpose(1, 2)
V = V.reshape(batch, n_heads, seq, d_v).transpose(1, 2)

attn = softmax(Q @ K.T / sqrt(d_k)) @ V  # [batch, n_heads, seq, d_v]
output = attn.transpose(1,2).reshape(batch, seq, d_model) @ W_o
```

## 3. Multi-Query Attention (MQA)

- **核心思想**：所有 Head 共享同一组 K, V，仅 Q 有多头
- **优点**：大幅减少 KV Cache 显存，推理加速
- **代表模型**：PaLM, Falcon

## 4. Grouped-Query Attention (GQA)

- **核心思想**：在 MHA 和 MQA 之间取折中，G 个 Head 共享一组 KV
- **优点**：兼顾速度和质量
- **代表模型**：LLaMA-2 70B, Mistral

```
MHA:  n_heads 组 Q, K, V     (质量最高，速度最慢)
GQA:  n_heads 组 Q, G 组 K,V (质量速度平衡)
MQA:  n_heads 组 Q, 1 组 K,V (速度最快，质量略降)
```

## 5. FlashAttention

- **问题**：标准 Attention 需要 O(N²) 显存存储注意力矩阵
- **方案**：分块计算 (Tiling)，利用 SRAM 减少显存读写
- **效果**：显存从 O(N²) 降到 O(N)，速度提升 2-4x

### FlashAttention-2 改进
- 优化 GPU 线程分工
- 减少 non-matmul FLOPs
- 自动处理因果遮罩

## 6. 稀疏注意力

| 方法 | 策略 | 代表 |
|------|------|------|
| Local Attention | 只关注局部窗口 | Longformer |
| Sliding Window | 滑动窗口注意力 | Mistral |
| Linear Attention | 核函数近似 | Performer |
| Mixture of Depths | 动态跳过层 | |

## 7. 学习要点

- [ ] 推导 Scaled Dot-Product Attention 的计算
- [ ] 理解 MHA / GQA / MQA 的区别与权衡
- [ ] 理解 FlashAttention 的分块计算原理
- [ ] 了解稀疏注意力的动机与方法
- [ ] 实现一个 Multi-Head Attention 模块
