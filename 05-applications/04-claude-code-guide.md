# Claude Code 使用指南

## 1. Claude Code 是什么

Claude Code 是 Anthropic 推出的 AI 编程助手，运行在终端中，能直接读写代码、执行命令、管理项目。

## 2. 安装与配置

### 2.1 安装
```bash
# macOS / Linux
curl -fsSL https://claude.ai/install.sh | sh

# 或通过 npm
npm install -g @anthropic-ai/claude-code
```

### 2.2 配置
```bash
# 设置 API Key
export ANTHROPIC_API_KEY=your_key

# 启动
claude
```

### 2.3 VS Code 集成
- 安装 Claude Code 扩展
- 在 VS Code 终端中直接使用

## 3. 核心功能

### 3.1 代码生成与编辑
```
> 帮我写一个 FastAPI 的 CRUD 接口
> 重构这个函数，提升可读性
> 添加单元测试
```

### 3.2 代码理解
```
> 解释这个项目的架构
> 这个函数做了什么
> 找到所有使用了这个 API 的地方
```

### 3.3 调试与修复
```
> 这个报错是什么原因
> 帮我修复这个 bug
> 为什么这个测试失败了
```

### 3.4 Git 操作
```
> 帮我写一个 commit message
> 创建一个新分支
> 查看 git diff
```

### 3.5 项目管理
```
> 初始化一个 React 项目
> 添加 TypeScript 支持
> 配置 ESLint 和 Prettier
```

## 4. 高级用法

### 4.1 Skills 系统
- 可以创建自定义 Skill 扩展能力
- 本项目已配置 `llm-learning-tutor` Skill

### 4.2 上下文管理
- 使用 `/add` 添加文件到上下文
- 使用 `/clear` 清除上下文
- 自动读取项目结构

### 4.3 MCP 集成
- 连接外部工具和数据源
- 扩展 Claude Code 的能力边界

## 5. 最佳实践

### 5.1 提示技巧
- 明确具体的需求描述
- 提供足够的上下文
- 分步骤完成复杂任务
- 及时反馈和纠正

### 5.2 安全注意事项
- 不要让 AI 直接执行危险命令
- 审查 AI 生成的代码
- 保护敏感信息

## 6. 优质学习资源

| 资源 | 链接 |
|------|------|
| 官方文档 | [code.claude.com/docs](https://code.claude.com/docs/zh-CN/quickstart) |
| 中文社区文档 | [claudecn.com/docs](https://claudecn.com/docs/) |
| 中文使用指南 | [github.com/claude-code-chinese/claude-code-guide](https://github.com/claude-code-chinese/claude-code-guide) |
| 中文文档镜像 | [claude-cn.org](https://www.claude-cn.org/claude-code-docs-zh/readme) |

## 7. 学习要点

- [ ] 安装并配置 Claude Code
- [ ] 掌握核心命令和交互方式
- [ ] 了解 Skill 系统的扩展
- [ ] 掌握高效使用 Claude Code 的技巧
