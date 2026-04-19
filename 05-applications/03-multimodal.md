# 多模态应用

## 1. 多模态模型概览

### 1.1 视觉语言模型 (VLM)

| 模型 | 参数量 | 特点 |
|------|--------|------|
| GPT-4V | - | 最强多模态理解 |
| Claude 3.5 Sonnet | - | 图片+文档理解 |
| LLaVA-1.6 | 7B-34B | 开源，效果好 |
| Qwen-VL-2 | 7B-72B | 中文多模态 |
| InternVL-2 | 2B-76B | 开源多模态标杆 |
| CogVLM2 | 19B | 视觉理解+生成 |

### 1.2 架构范式

```
标准 VLM 架构:
  Image → Vision Encoder (ViT/SigLIP) → Projector → LLM
  Text  → Tokenizer → Embedding → LLM → Output

统一架构 (Fuyu, Chameleon):
  Image → Patches → Tokenize → LLM → Output
  Text  → Tokenize → LLM → Output
```

## 2. 关键技术

### 2.1 Vision Encoder
- **CLIP ViT**：最常用，预训练的视觉编码器
- **SigLIP**：Sigmoid 替代 Softmax，更好
- **InternViT**：6B 参数视觉编码器

### 2.2 Projector (视觉-语言对齐)
- **线性投影**：简单有效
- **MLP**：多层感知机
- **Q-Former** (BLIP-2)：查询 Transformer
- **Perceiver Resampler** (Flamingo)：压缩视觉 token

### 2.3 图像分辨率策略
- 固定分辨率（224×224）
- 动态分辨率（按需缩放）
- 切片策略（大图切成多块）

## 3. 语音多模态

### 3.1 语音大模型
| 模型 | 功能 |
|------|------|
| Whisper | 语音识别 |
| GPT-4o | 语音对话 |
| Qwen-Audio | 语音理解 |
| SALMONN | 语音+声音理解 |

### 3.2 语音合成 (TTS)
| 模型 | 特点 |
|------|------|
| VITS | 端到端 TTS |
| Bark | 多语言+音效 |
| ChatTTS | 中文对话 TTS |
| CosyVoice | 阿里，语音克隆 |

## 4. 多模态 RAG

- 图文混合检索
- 视觉 Embedding + 文本 Embedding
- 表格/图表理解增强

## 5. 学习要点

- [ ] 理解 VLM 的架构范式
- [ ] 了解主流 VLM 模型
- [ ] 理解 Vision-LLM 对齐方法
- [ ] 了解语音多模态应用
