# 模型部署方案

## 1. 部署架构概览

```
用户请求 → API Gateway → 负载均衡 → 推理集群
                                    ├── vLLM Worker × N
                                    ├── TGI Worker × N
                                    └── TensorRT-LLM Worker × N
```

## 2. 模型格式与导出

### 2.1 格式对比

| 格式 | 精度 | 文件大小 | 加载速度 | 代表 |
|------|------|----------|----------|------|
| PyTorch (.pt/.bin) | FP32/BF16 | 大 | 慢 | 原生 |
| Safetensors | FP32/BF16 | 大 | 快 | HuggingFace |
| GGUF | 2-8bit | 小 | 快 | llama.cpp |
| ONNX | FP32/FP16 | 中 | 中 | 通用 |
| TensorRT (.engine) | FP16/INT8 | 小 | 最快 | NVIDIA |

### 2.2 导出流程

```
PyTorch → ONNX → TensorRT
         ↓
      OpenVINO (Intel)
         ↓
      Core ML (Apple)
```

### 2.3 Safetensors
- **优势**：零拷贝加载、内存安全、跨框架
- 已成为 HuggingFace 默认格式
```python
from transformers import AutoModel
model = AutoModel.from_pretrained("model_name", use_safetensors=True)
```

## 3. GPU 部署方案

### 3.1 vLLM ⭐

#### 核心特性
- PagedAttention：高效 KV Cache 管理
- Continuous Batching：动态批处理
- Tensor Parallelism：多 GPU 并行
- Speculative Decoding：投机解码加速

#### 部署命令
```bash
# 单 GPU
python -m vllm.entrypoints.openai.api_server \
    --model meta-llama/Llama-2-7b-chat-hf \
    --tensor-parallel-size 1

# 多 GPU
python -m vllm.entrypoints.openai.api_server \
    --model meta-llama/Llama-2-70b-chat-hf \
    --tensor-parallel-size 4 \
    --gpu-memory-utilization 0.9
```

#### 性能参考 (A100)
| 模型 | 延迟 (TTFT) | 吞吐量 |
|------|-------------|--------|
| 7B | ~50ms | ~200 tok/s |
| 13B | ~80ms | ~120 tok/s |
| 70B (4×A100) | ~200ms | ~50 tok/s |

### 3.2 TGI (Text Generation Inference)

#### 核心特性
- HuggingFace 官方推理服务器
- Flash Attention + Paged Attention
- 水印与安全过滤
- Docker 一键部署

```bash
docker run --gpus all -p 8080:80 \
    ghcr.io/huggingface/text-generation-inference:latest \
    --model-id meta-llama/Llama-2-7b-chat-hf
```

### 3.3 TensorRT-LLM

#### 核心特性
- NVIDIA 官方，极致性能
- 支持所有量化格式
- Inflight Batching
- KV Cache 复用

#### 构建引擎
```python
# 将 HF 模型转为 TensorRT 引擎
python convert_checkpoint.py --model_dir ./llama-7b \
    --output_dir ./trt_ckpt \
    --tp_size 1

trtllm-build --checkpoint_dir ./trt_ckpt \
    --output_dir ./trt_engines \
    --gemm_plugin auto
```

## 4. CPU 部署方案

### 4.1 llama.cpp ⭐
- C/C++ 纯实现，无 GPU 依赖
- GGUF 格式，支持 2-8 bit 量化
- Apple Silicon 优化 (Metal)
```bash
./main -m model.gguf -p "Hello" -n 512
```

### 4.2 GGUF 量化等级

| 量化 | 比特 | 7B 大小 | 质量损失 |
|------|------|---------|----------|
| Q8_0 | 8bit | ~7GB | 几乎无 |
| Q5_K_M | 5bit | ~4.5GB | 很小 |
| Q4_K_M | 4bit | ~3.6GB | 小 |
| Q3_K_M | 3bit | ~2.8GB | 中等 |
| Q2_K | 2bit | ~2.2GB | 较大 |

### 4.3 Ollama
- 基于 llama.cpp 的封装
- 一键拉取和运行模型
```bash
ollama run llama3
ollama run mistral
```

## 5. 云端部署

### 5.1 托管服务
| 服务 | 提供商 | 特点 |
|------|--------|------|
| SageMaker | AWS | 全托管，自动扩缩 |
| Vertex AI | GCP | 集成 PaLM/Gemini |
| Azure ML | Azure | 集成 OpenAI |
| Together AI | Together | 开源模型推理 |
| Fireworks AI | Fireworks | 高性能推理 |
| Anyscale | Anyscale | Ray 生态 |

### 5.2 Serverless 部署
- 按请求计费
- 自动扩缩容
- 适合低流量场景

## 6. 部署最佳实践

### 6.1 延迟优化
1. 模型量化 (INT8/INT4)
2. KV Cache 优化
3. Continuous Batching
4. Speculative Decoding
5. 模型蒸馏

### 6.2 吞吐优化
1. Dynamic Batching
2. 增大 batch size
3. Tensor Parallelism
4. 多实例部署

### 6.3 成本优化
1. 选择合适的量化等级
2. Spot Instance
3. 请求排队与批处理
4. 缓存频繁请求

## 7. 监控与运维

### 7.1 关键指标
- TTFT (Time to First Token)
- TPOT (Time per Output Token)
- Throughput (tokens/second)
- GPU 利用率
- KV Cache 利用率

### 7.2 工具
- Prometheus + Grafana
- OpenTelemetry
- vLLM 内置 metrics

## 8. 学习要点

- [ ] 理解不同部署方案的特点
- [ ] 掌握 vLLM 部署与调优
- [ ] 理解 GGUF 量化等级选择
- [ ] 了解 TensorRT-LLM 的使用
- [ ] 掌握延迟与吞吐的优化策略
