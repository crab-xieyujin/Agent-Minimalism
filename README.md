# Agent Minimalism

Agent Minimalism is a Codex skill for designing lower-cost, more reliable Agent workflows.

Core idea:

> Default to workflowization. Use Agents only where uncertainty genuinely requires them.

It helps teams review an AI workflow and decide which parts should be implemented as deterministic rules, a single LLM call, a fixed workflow, a local Agent, or a full dynamic Agent.

## Why It Matters

Many Agent systems become expensive and unstable because every step is wrapped in an Agent. Agent Minimalism gives teams a practical review discipline:

- Keep deterministic work in code, schemas, templates, and fixed workflows.
- Use one LLM call when semantic transformation is bounded.
- Place Agents only at uncertainty, exploration, recovery, or judgment points.
- Reduce token growth, latency, cost, and context-sharing overhead.

## Complexity Levels

| Level | Use when | Preferred implementation |
|---|---|---|
| L0 Rule | Inputs, checks, transformations, or outputs are deterministic | Code, config, schema validation, regex, template |
| L1 Single LLM | Semantic work is needed, but no loop or tool observation is needed | One LLM call with structured output |
| L2 Fixed Workflow | Multiple steps exist, but the path is mostly known | Pipeline, DAG, state machine |
| L3 Local Agent | One node is uncertain and may need tools, retries, or judgment | Agent only inside that node |
| L4 Dynamic Agent | The goal is open-ended and the path cannot be known upfront | Planner, tools, memory, iterative execution |

## Repository Contents

- `SKILL.md` - The Codex skill definition and main workflow.
- `references/review-checklist.md` - Review checklist for Agent workflow designs.
- `references/router-template.md` - Router pattern and design output template.
- `agents/openai.yaml` - Agent interface metadata.

## Example Use

Ask Codex:

```text
Use $agent-minimalism to review this workflow and reduce unnecessary Agent, token, and context overhead.
```

Expected output includes:

- Current workflow diagnosis.
- Step classification table with L0-L4 labels.
- Recommended architecture.
- Agent justification for every Agent node.
- Token and stability implications.
- Implementation next steps.

## License

MIT
