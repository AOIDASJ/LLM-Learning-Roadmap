# Agent 与工具调用

## 1. Agent 概述

### 1.1 什么是 Agent
- LLM 作为推理核心，自主规划、决策和执行
- 能调用外部工具获取信息或执行操作
- 循环：感知 → 规划 → 行动 → 观察 → ...

### 1.2 Agent 核心能力
| 能力 | 说明 |
|------|------|
| 规划 | 将复杂任务分解为子任务 |
| 工具调用 | 使用外部 API/工具 |
| 记忆 | 短期（对话历史）+ 长期（向量存储） |
| 反思 | 评估行动结果，调整策略 |

## 2. Agent 框架

### 2.1 ReAct (Reasoning + Acting)
```
Thought: 我需要查找北京今天的天气
Action: search_weather("北京")
Observation: 北京今天晴，25°C
Thought: 我已经得到答案
Answer: 北京今天天气晴朗，温度25°C
```

### 2.2 Function Calling / Tool Calling

#### OpenAI 格式
```json
{
  "tools": [
    {
      "type": "function",
      "function": {
        "name": "get_weather",
        "description": "获取指定城市的天气",
        "parameters": {
          "type": "object",
          "properties": {
            "city": {"type": "string", "description": "城市名"}
          },
          "required": ["city"]
        }
      }
    }
  ]
}
```

### 2.3 MCP (Model Context Protocol) ⭐

Anthropic 提出的开放协议，标准化 LLM 与外部工具的连接。

```
Claude Desktop ←→ MCP Client ←→ MCP Server ←→ 工具/数据源
```

- **MCP Server**：提供工具和资源
- **MCP Client**：与 LLM 集成
- 官方文档：[modelcontextprotocol.io](https://modelcontextprotocol.io/)

### 2.4 主流框架对比

| 框架 | 特点 | 适合 |
|------|------|------|
| LangChain | 生态最大，组件丰富 | 通用 Agent 开发 |
| LangGraph | 图编排，状态管理 | 复杂工作流 |
| CrewAI | 多 Agent 协作 | 团队模拟 |
| AutoGen | 微软，多 Agent 对话 | 对话式协作 |
| Dify | 低代码平台 | 快速搭建 |

## 3. Multi-Agent 系统

### 3.1 架构模式
| 模式 | 描述 |
|------|------|
| 顺序 | Agent A → Agent B → Agent C |
| 并行 | Agent A ∥ Agent B → 汇总 |
| 层级 | Manager → Worker × N |
| 对话 | Agent 之间自由通信 |

### 3.2 关键设计
- **角色定义**：每个 Agent 有明确职责
- **通信协议**：Agent 间消息传递
- **冲突解决**：多数投票、层级决策
- **共享记忆**：信息共享机制

## 4. 优质学习资源

| 资源 | 链接 |
|------|------|
| AgentGuide | [github.com/adongwanai/AgentGuide](https://github.com/adongwanai/AgentGuide) |
| LangGraph 文档 | [langchain-ai.github.io/langgraph](https://langchain-ai.github.io/langgraph/) |
| MCP 官方文档 | [modelcontextprotocol.io](https://modelcontextprotocol.io/) |
| CrewAI | [github.com/crewAIInc/crewAI](https://github.com/crewAIInc/crewAI) |

## 5. 学习要点

- [ ] 理解 ReAct 模式
- [ ] 掌握 Function Calling
- [ ] 了解 MCP 协议
- [ ] 理解 Multi-Agent 架构
- [ ] 实践构建一个 Agent 应用
