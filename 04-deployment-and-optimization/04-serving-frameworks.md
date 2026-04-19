# 推理服务框架

## 1. 框架对比总览

| 框架 | 开发者 | GPU | CPU | 量化格式 | 张量并行 | 特点 |
|------|--------|-----|-----|----------|----------|------|
| **vLLM** | UC Berkeley | ✅ | ❌ | GPTQ/AWQ | ✅ | PagedAttention，生态最好 |
| **TGI** | HuggingFace | ✅ | ❌ | GPTQ/AWQ/bnb | ✅ | 生产就绪，Docker 友好 |
| **TensorRT-LLM** | NVIDIA | ✅ | ❌ | INT8/INT4/FP8 | ✅ | 极致性能 |
| **llama.cpp** | 社区 | ✅ | ✅ | GGUF | ❌ | 轻量，跨平台 |
| **LMDeploy** | InternLM | ✅ | ❌ | AWQ/W4A16 | ✅ | Turbomind 引擎 |
| **Ollama** | Ollama | ✅ | ✅ | GGUF | ❌ | 极简体验 |
| **MLC-LLM** | CMU | ✅ | ✅ | 多种 | ❌ | 编译优化，端侧 |

## 2. vLLM ⭐

### 2.1 快速开始
```bash
# 安装
pip install vllm

# 启动 OpenAI 兼容 API
python -m vllm.entrypoints.openai.api_server \
    --model meta-llama/Llama-3-8B-Instruct \
    --tensor-parallel-size 1 \
    --gpu-memory-utilization 0.9

# 请求
curl http://localhost:8000/v1/chat/completions \
    -H "Content-Type: application/json" \
    -d '{"model": "meta-llama/Llama-3-8B-Instruct", "messages": [{"role": "user", "content": "Hello"}]}'
```

### 2.2 关键参数
| 参数 | 说明 | 建议值 |
|------|------|--------|
| `--tensor-parallel-size` | GPU 并行数 | 按需 |
| `--gpu-memory-utilization` | GPU 显存利用率 | 0.85-0.95 |
| `--max-model-len` | 最大序列长度 | 按需 |
| `--quantization` | 量化方式 | awq/gptq |
| `--enable-prefix-caching` | 前缀缓存 | 开启 |
| `--speculative-model` | 投机解码模型 | 可选 |

### 2.3 官方文档
- 📖 [docs.vllm.ai](https://docs.vllm.ai/)
- 🔧 [GitHub](https://github.com/vllm-project/vllm)

## 3. TGI (Text Generation Inference)

### 3.1 Docker 一键部署
```bash
docker run --gpus all -p 8080:80 \
    -v $PWD/data:/data \
    ghcr.io/huggingface/text-generation-inference:latest \
    --model-id meta-llama/Llama-3-8B-Instruct \
    --quantize awq
```

### 3.2 特点
- HuggingFace 官方维护
- Flash Attention + Paged Attention
- 水印与安全过滤
- 适合生产环境

## 4. TensorRT-LLM

### 4.1 使用流程
```bash
# 1. 转换 checkpoint
python convert_checkpoint.py --model_dir ./llama-8b \
    --output_dir ./trt_ckpt --tp_size 1

# 2. 构建 TensorRT 引擎
trtllm-build --checkpoint_dir ./trt_ckpt \
    --output_dir ./trt_engines \
    --gemm_plugin auto \
    --gpt_attention_plugin auto

# 3. 运行推理
python run.py --engine_dir ./trt_engines \
    --tokenizer_dir ./llama-8b
```

### 4.2 适合场景
- 追求极致性能
- NVIDIA GPU 环境
- 需要生产级推理服务

## 5. 选择建议

```
快速验证？     → Ollama / vLLM
生产部署？     → vLLM / TGI
极致性能？     → TensorRT-LLM
CPU 推理？     → llama.cpp / Ollama
端侧部署？     → MLC-LLM / llama.cpp
```

## 6. 学习要点

- [ ] 掌握 vLLM 部署与调参
- [ ] 了解 TGI 的 Docker 部署
- [ ] 理解 TensorRT-LLM 的构建流程
- [ ] 能够根据场景选择合适框架
