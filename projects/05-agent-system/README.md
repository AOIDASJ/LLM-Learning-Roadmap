# 项目5：Agent 系统构建

## 目标

构建一个具有工具调用能力的 Agent 系统，能自主规划和执行多步任务。

## 预计时间

7-10 天

## 前置知识

- ReAct 框架
- Function Calling
- LangChain / LangGraph

## 实现清单

### Phase 1: 基础 Agent (Day 1-3)
- [ ] 实现单轮工具调用（搜索、计算器）
- [ ] 定义 Tool Schema
- [ ] 实现 ReAct 循环（Think→Act→Observe）
- [ ] 集成 LLM 进行推理

### Phase 2: 多工具集成 (Day 3-5)
- [ ] 搜索工具（Web Search / API）
- [ ] 代码执行工具（Python REPL）
- [ ] 文件读写工具
- [ ] 数据库查询工具

### Phase 3: 记忆系统 (Day 5-6)
- [ ] 短期记忆（对话历史管理）
- [ ] 长期记忆（向量存储检索）
- [ ] 记忆摘要与压缩

### Phase 4: 多 Agent 协作 (Day 6-8，可选)
- [ ] 设计 Agent 角色（研究者、编码者、审查者）
- [ ] 实现 Agent 间通信
- [ ] 实现任务分配与汇总

### Phase 5: 前端与测试 (Day 8-10)
- [ ] Gradio 聊天界面
- [ ] 思考过程可视化
- [ ] 端到端测试

## 推荐框架

| 框架 | 链接 | 特点 |
|------|------|------|
| LangGraph | [GitHub](https://github.com/langchain-ai/langgraph) | 图编排，最灵活 |
| LangChain Agent | [文档](https://python.langchain.com/docs/modules/agents/) | 上手最快 |
| CrewAI | [GitHub](https://github.com/crewAIInc/crewAI) | 多 Agent |

## 验收标准

- Agent 能自主完成多步任务（如"查询某公司信息并生成报告"）
- 工具调用正确率 > 90%
- 思考过程可追溯
- 支持多轮对话中的上下文保持
