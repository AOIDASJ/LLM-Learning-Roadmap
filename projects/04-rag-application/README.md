# 项目4：RAG 应用开发

## 目标

构建一个完整的 RAG 应用，支持文档上传、知识检索和智能问答。

## 预计时间

5-7 天

## 前置知识

- RAG 架构原理
- Embedding 与向量数据库
- LangChain / LlamaIndex

## 实现清单

### Phase 1: 基础架构 (Day 1-2)
- [ ] 搭建 FastAPI 后端
- [ ] 集成 LLM（vLLM / OpenAI API）
- [ ] 实现基本的问答接口

### Phase 2: 文档处理 (Day 2-3)
- [ ] 实现 PDF/Markdown/TXT 解析
- [ ] 实现 Chunking（Recursive Splitter）
- [ ] 实现 Embedding 生成
- [ ] 存入向量数据库（Chroma/Milvus）

### Phase 3: 检索增强 (Day 3-4)
- [ ] 实现向量检索
- [ ] 实现 BM25 关键词检索
- [ ] 实现混合检索（RRF 融合）
- [ ] 实现 Reranker 重排序

### Phase 4: 高级功能 (Day 4-6)
- [ ] Query 改写 / Multi-Query
- [ ] 对话历史记忆
- [ ] 引用来源标注
- [ ] 流式输出

### Phase 5: 前端与部署 (Day 6-7)
- [ ] 实现聊天界面（Gradio/Streamlit）
- [ ] 文档上传界面
- [ ] Docker 容器化
- [ ] 部署上线

## 技术栈推荐

| 组件 | 推荐 | 替代 |
|------|------|------|
| 框架 | LangChain | LlamaIndex |
| Embedding | bge-large-zh-v1.5 | bge-m3 |
| 向量数据库 | Chroma | Milvus, Qdrant |
| Reranker | bge-reranker-large | Cohere |
| 前端 | Gradio | Streamlit |
| 后端 | FastAPI | Flask |

## 验收标准

- 能上传文档并建立索引
- 问答有引用来源
- 检索准确率 > 80%
- 支持多轮对话
- 响应延迟 < 3s
