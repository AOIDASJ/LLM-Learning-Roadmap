# 端侧部署

## 1. 端侧部署动机

- **隐私**：数据不离开设备
- **延迟**：无网络往返
- **离线**：不依赖网络连接
- **成本**：无 API 调用费用

## 2. 端侧部署框架

### 2.1 llama.cpp ⭐
- C/C++ 实现，跨平台
- Apple Silicon (Metal) 优化
- Android / iOS 支持

```bash
# macOS Metal 加速
make METAL=1
./main -m model.gguf -p "Hello" -n 512 -t 8

# iOS 集成
# 使用 llama.swift 或直接编译为 iOS 库
```

### 2.2 MLC-LLM
- 基于 Apache TVM 的编译优化
- 支持 iOS / Android / Web
- 自动算子融合和优化

```python
# 编译模型
python -m mlc_llm compile ./model \
    --target iphone \
    --opt O3 \
    -o model-ios.tar
```

### 2.3 Apple MLX
- Apple 官方 ML 框架
- 统一内存架构优化
- 适合 Apple Silicon

```python
import mlx.core as mx
from mlx_lm import load, generate

model, tokenizer = load("mlx-community/Phi-3-mini-4bit-instruct")
response = generate(model, tokenizer, prompt="Hello", max_tokens=256)
```

### 2.4 Core ML
- Apple 生态原生推理
- 通过 coremltools 转换
- 支持 Neural Engine 加速

### 2.5 ONNX Runtime Mobile
- 跨平台移动端推理
- 支持量化模型

### 2.6 MediaPipe
- Google 端侧 ML 框架
- 内置 LLM Inference API
- Android / iOS / Web

## 3. 端侧部署挑战

| 挑战 | 解决方案 |
|------|----------|
| 显存不足 | 2-4bit 量化 |
| 计算能力弱 | 小模型 (1-3B) |
| 电池消耗 | 限制 batch size，优化推理 |
| 存储空间 | 量化压缩，按需下载 |

## 4. 推荐端侧模型

| 模型 | 参数量 | 量化后大小 | 适合设备 |
|------|--------|-----------|----------|
| Phi-3 Mini | 3.8B | ~2GB (Q4) | iPhone 15+ |
| Gemma 2B | 2B | ~1.5GB (Q4) | 中端手机 |
| Qwen2.5-1.5B | 1.5B | ~1GB (Q4) | 中端手机 |
| TinyLlama 1.1B | 1.1B | ~0.7GB (Q4) | 低端设备 |

## 5. 学习要点

- [ ] 了解端侧部署的框架选择
- [ ] 掌握 llama.cpp 的跨平台编译
- [ ] 理解 MLC-LLM 的编译优化流程
- [ ] 了解 Apple MLX 的使用
