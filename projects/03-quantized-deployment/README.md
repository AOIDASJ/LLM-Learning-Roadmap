# 项目3：量化部署实战 ⭐

## 目标

对一个 7B 模型完成量化→部署→加速→压测全流程，掌握生产级部署能力。

## 预计时间

7-10 天

## 前置知识

- 量化技术（GPTQ/AWQ/GGUF）
- vLLM / llama.cpp 部署
- 推理加速原理

## 实现清单

### Phase 1: 基线部署 (Day 1-2)
- [ ] 用 vLLM 部署 FP16 基座模型
- [ ] 压测基线性能（TTFT, TPOT, Throughput）
- [ ] 记录显存占用和 GPU 利用率

### Phase 2: GPTQ 量化 (Day 2-3)
- [ ] 使用 AutoGPTQ 对模型做 4-bit 量化
- [ ] 部署量化模型
- [ ] 对比精度损失（MMLU/推理质量）
- [ ] 对比性能提升

### Phase 3: AWQ 量化 (Day 3-4)
- [ ] 使用 AutoAWQ 对模型做 4-bit 量化
- [ ] 对比 GPTQ 和 AWQ 的精度与速度
- [ ] 选择更优方案

### Phase 4: GGUF + llama.cpp (Day 4-5)
- [ ] 转换为 GGUF 格式（Q4_K_M, Q5_K_M）
- [ ] 用 llama.cpp 本地部署
- [ ] 对比 CPU/GPU 混合推理性能

### Phase 5: 推理加速 (Day 5-8)
- [ ] 启用 FlashAttention-2
- [ ] 配置 Continuous Batching
- [ ] 尝试 Speculative Decoding
- [ ] 配置 KV Cache 量化
- [ ] 调优 PagedAttention 参数

### Phase 6: 压测与报告 (Day 8-10)
- [ ] 使用 benchmark 工具全面压测
- [ ] 输出部署方案对比报告
- [ ] 选择最优部署方案

## 压测工具

```bash
# vLLM 内置 benchmark
python -m vllm.entrypoints.openai.api_server --model <model>
python benchmarks/benchmark_serving.py --backend vllm --model <model>

# locust / wrk HTTP 压测
```

## 对比表格模板

| 方案 | 模型大小 | TTFT | TPOT | Throughput | 显存 | MMLU |
|------|----------|------|------|-----------|------|------|
| FP16 基线 | | | | | | |
| GPTQ-4bit | | | | | | |
| AWQ-4bit | | | | | | |
| GGUF-Q4_K_M | | | | | | |
| AWQ+vLLM加速 | | | | | | |

## 验收标准

- 完成至少 3 种量化方案对比
- 性能报告含 TTFT/TPOT/Throughput 指标
- 找到最优的精度-速度平衡方案
- 理解每个优化步骤的原理
