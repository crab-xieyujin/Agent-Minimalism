# Agent Minimalism — WorkBuddy Agent 描述

## 触发条件

当用户提出以下请求时，加载 `SKILL.md` 并执行工作流：

- 设计或审查一个 Agent、AI 自动化、工具调用管道或工作流
- 减少 Agent 系统的 token 用量、延迟或成本
- 评估现有工作流是否过度使用 Agent
- 将全量 Agent 流程重构为混合工作流

## 默认提示词

```
使用 agent-minimalism 审查这个工作流，减少不必要的 Agent、token 和上下文开销。
```

## 调用策略

- 允许隐式触发：当用户描述 Agent 或 AI 工作流设计问题时，无需用户明确指定 Skill 名称。
- 在 WorkBuddy 中安装后，可通过技能名称直接调用：`agent-minimalism`。
