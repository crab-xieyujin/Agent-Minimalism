# Agent Minimalism

Agent Minimalism 是一个 WorkBuddy Skill，帮助设计更低成本、更可靠的 Agent 工作流。

核心理念：

> 默认工作流化。仅在真正存在不确定性时使用 Agent。

它帮助团队审查 AI 工作流，判断哪些部分应实现为确定性规则、单次 LLM 调用、固定工作流、局部 Agent 或完整动态 Agent。

## 为什么重要

很多 Agent 系统变得昂贵且不稳定，原因是每个步骤都被包裹进 Agent。Agent Minimalism 给团队提供一套实用的审查规范：

- 将确定性工作保留在代码、Schema、模板和固定工作流中。
- 语义转换有边界时，使用单次模型调用。
- 仅在不确定性、探索、恢复或判断点处放置 Agent。
- 减少 token 增长、延迟、成本和上下文共享开销。

## 复杂度分级

| 级别 | 适用场景 | 推荐实现方式 |
|---|---|---|
| L0 规则 | 输入、检查、转换或输出是确定性的 | 代码、配置、Schema 校验、正则、模板 |
| L1 单次 LLM | 需要语义处理，但不需要循环或工具观察 | 单次 LLM 调用 + 结构化输出 |
| L2 固定工作流 | 存在多步骤，但路径基本已知 | Pipeline、DAG、状态机 |
| L3 局部 Agent | 有一个节点存在不确定性，可能需要工具、重试或判断 | 仅在该节点使用 Agent |
| L4 动态 Agent | 目标开放，路径无法提前知道 | Planner + 工具 + 记忆 + 迭代执行 |

## 项目文件

- `SKILL.md` — WorkBuddy Skill 定义和主工作流。
- `references/review-checklist.md` — Agent 工作流设计审查清单。
- `references/router-template.md` — 路由器模式与设计输出模板。
- `agents/workbuddy.md` — WorkBuddy Agent 触发描述。
- `docs/Agent-Minimalism-作品说明材料.docx` — 使用场景、问题、方案、输入输出边界说明文档。

## 安装到 WorkBuddy

将此目录放入 WorkBuddy Skill 路径即可（用户级或项目级均可）：

```
# 用户级（跨项目可用）
~/.workbuddy/skills/agent-minimalism/

# 项目级（仅当前项目可用）
{workspace}/.workbuddy/skills/agent-minimalism/
```

## 使用方式

在 WorkBuddy 对话中直接触发：

```
使用 agent-minimalism 审查这个工作流，减少不必要的 Agent、token 和上下文开销。
```

预期输出包括：

- 当前工作流诊断。
- 带 L0-L4 标签的步骤分类表。
- 推荐架构。
- 每个 Agent 节点的使用理由。
- Token 与稳定性影响说明。
- 下一步实施建议。

## License

MIT
