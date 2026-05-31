---
name: agent-minimalism
description: Use when designing, reviewing, or optimizing Agent workflows, AI automations, dynamic workflows, tool-calling pipelines, token-heavy processes, or AI product task orchestration. Helps decide which parts should be rules, templates, scripts, single LLM calls, fixed workflows, local Agents, or full dynamic Agents, with the principle of default workflowization and necessary Agentization.
metadata:
  short-description: Minimize Agent use by routing only uncertainty to Agents
---

# Agent Minimalism

Use this skill to turn a task or workflow into the lowest-complexity reliable design.

Core principle:

> Default to workflowization. Use Agents only where uncertainty genuinely requires them.

Agents should handle uncertainty, exploration, recovery, and open-ended judgment. They should not be the default wrapper around deterministic steps.

## When To Apply

Apply this skill when the user is:

- Designing an Agent, AI workflow, automation pipeline, or tool-calling system.
- Trying to reduce token usage, latency, cost, instability, or context sharing overhead.
- Converting a fully Agentic process into a mixed workflow.
- Reviewing whether a dynamic workflow is over-engineered.
- Building reusable patterns for content production, research, sales ops, customer support, coding tasks, data analysis, or product operations.

## Operating Model

Classify each step into the lowest sufficient level:

| Level | Use when | Preferred implementation |
|---|---|---|
| L0 Rule | Inputs, checks, transformations, or outputs are deterministic | Code, config, schema validation, regex, template |
| L1 Single LLM | Semantic work is needed, but no loop or tool observation is needed | One LLM call with structured output |
| L2 Fixed Workflow | Multiple steps exist, but the path is mostly known | Pipeline, DAG, state machine |
| L3 Local Agent | One node is uncertain and may need tools, retries, or judgment | Agent only inside that node |
| L4 Dynamic Agent | The goal is open-ended and the path cannot be known upfront | Planner, tools, memory, iterative execution |

Rule of thumb:

> If a lower level can solve it reliably, do not move it to a higher level.

## Workflow

1. Map the current task chain.
   - Identify input, output, intermediate artifacts, tools, failure modes, and decision points.

2. Separate certainty from uncertainty.
   - Deterministic: make it code, template, config, validation, or a fixed workflow.
   - Semantic but bounded: use a single model call.
   - Uncertain, exploratory, or self-correcting: consider local Agent use.

3. Route by complexity.
   - Prefer L0, then L1, then L2, then L3, then L4.
   - Treat full dynamic Agents as the last resort.

4. Place Agents at exception points.
   - Use Agents as complex-node handlers or failure recovery handlers.
   - Avoid putting Agents at the main entrance unless the whole task path is unknown.

5. Produce a design recommendation.
   - Show which steps move to rules/workflow/LLM/Agent.
   - State the uncertainty that justifies each Agent node.
   - Estimate cost, stability, and token impact qualitatively.

## Required Output Shape

When reviewing or designing, include:

- Current workflow diagnosis.
- Step classification table with L0-L4 labels.
- Recommended architecture.
- Agent justification for every Agent node.
- Token/stability implications.
- Implementation next steps.

Keep the answer practical. The goal is a usable design discipline, not a philosophical essay.

## References

- For review criteria, read `references/review-checklist.md`.
- For reusable router and architecture templates, read `references/router-template.md`.
