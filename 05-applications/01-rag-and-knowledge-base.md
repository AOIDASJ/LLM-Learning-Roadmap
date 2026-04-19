# RAG 与知识库

## 1. RAG 架构

```
用户问题 → Query 改写 → Embedding → 向量检索 → 重排序 → LLM 生成 → 回答
                ↓                        ↓
           Query 扩展              Context 组装
```

## 2. 核心组件

### 2.1 文档处理

#### Chunking 策略
| 策略 | 方法 | 适合场景 |
|------|------|----------|
| 固定大小 | 每 512 token 一块 | 通用 |
| 递归分割 | 按段落/句子递归切分 | 通用（推荐） |
| 语义分割 | 按语义相似度切分 | 长文档 |
| 文档结构 | 按标题/章节切分 | 结构化文档 |

#### 关键代码
```python
from langchain.text_splitter import RecursiveCharacterTextSplitter

splitter = RecursiveCharacterTextSplitter(
    chunk_size=512,
    chunk_overlap=50,
    separators=["\n\n", "\n", "。", ".", " ", ""]
)
chunks = splitter.split_text(document)
```

### 2.2 Embedding 模型

| 模型 | 维度 | 中文 | 说明 |
|------|------|------|------|
| bge-large-zh-v1.5 | 1024 | ✅ | 中文最佳 |
| bge-m3 | 1024 | ✅ | 多语言多粒度 |
| text-embedding-3-large | 3072 | ✅ | OpenAI |
| gte-Qwen2 | 1536 | ✅ | 阿里最新 |

### 2.3 向量数据库

| 数据库 | 类型 | 特点 |
|--------|------|------|
| Milvus | 分布式 | 大规模生产 |
| Chroma | 嵌入式 | 轻量开发 |
| Pinecone | 云服务 | 零运维 |
| Weaviate | 混合 | 向量+关键词 |
| Qdrant | Rust | 高性能 |
| FAISS | 库 | Meta，离线 |

### 2.4 检索策略

| 策略 | 描述 |
|------|------|
| 向量检索 | 语义相似度 |
| BM25 关键词检索 | 精确匹配 |
| 混合检索 | 向量 + BM25 融合 |
| 多路召回 | 多种策略并行 |
| Query 改写/扩展 | HyDE, Multi-Query |

### 2.5 重排序 (Reranker)

- 检索后用 Cross-Encoder 精排
- 模型：bge-reranker-large, Cohere Rerank
- Top-K → Rerank → Top-N (N < K)

## 3. 高级 RAG 技术

### 3.1 Self-RAG
- 模型自己决定是否需要检索
- 检索后评估相关性
- 无关则不使用检索结果

### 3.2 Corrective RAG (CRAG)
- 检索结果质量评估
- 质量低时触发 Web 搜索补充
- 自我纠错

### 3.3 Graph RAG
- 将知识构建为知识图谱
- 图结构检索增强
- 微软开源：[GraphRAG](https://github.com/microsoft/graphrag)

### 3.4 Agentic RAG
- 用 Agent 编排检索流程
- 多步检索、工具调用
- LangGraph 实现

## 4. 优质学习资源

| 资源 | 链接 |
|------|------|
| RAG 完整教程（中文） | [vivy-yi.github.io/rag-tutorial](https://vivy-yi.github.io/rag-tutorial/) |
| LangChain RAG 教程 | [python.langchain.com](https://python.langchain.com/docs/tutorials/rag/) |
| LlamaIndex 文档 | [docs.llamaindex.ai](https://docs.llamaindex.ai/) |
| GraphRAG | [github.com/microsoft/graphrag](https://github.com/microsoft/graphrag) |

## 5. 学习要点

- [ ] 理解 RAG 的完整流程
- [ ] 掌握 Chunking 策略选择
- [ ] 理解向量检索 vs 混合检索
- [ ] 了解 Self-RAG / CRAG / Graph RAG
- [ ] 实践构建一个 RAG 应用
