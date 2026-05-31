# Complexity Router Template

Use this reference to turn Agent Minimalism into an implementation pattern.

## Architecture Pattern

```text
Task Input
  |
  v
Normalize and validate input
  |
  v
Complexity Router
  |
  +--> L0 Rule / script / template
  |
  +--> L1 Single LLM structured call
  |
  +--> L2 Fixed workflow / DAG / state machine
  |
  +--> L3 Local Agent for one uncertain node
  |
  +--> L4 Full dynamic Agent only for open-ended tasks
  |
  v
Validate output
  |
  v
Return result or escalate to recovery Agent
```

## Routing Questions

Score each question as yes/no:

- Is the input shape known?
- Is the output shape known?
- Can the success criteria be validated by code or schema?
- Are the steps known in advance?
- Can the task finish in one model call?
- Does the task require observing tool results and adapting?
- Does failure require autonomous diagnosis?
- Is the goal open-ended?

Routing:

| Condition | Route |
|---|---|
| Known input/output and code-checkable success | L0 |
| Needs semantic transformation but no loop | L1 |
| Needs several known steps | L2 |
| Has one uncertain or failure-prone step | L3 |
| Open-ended goal with unknown path | L4 |

## Pseudocode

```pseudo
function routeTask(task):
  normalized = normalize(task)

  if canSolveWithRules(normalized):
    return runRulePath(normalized)

  if needsOnlySingleSemanticPass(normalized):
    result = callLLMOnce(normalized, structuredSchema)
    return validateOrEscalate(result)

  if hasKnownMultiStepPath(normalized):
    result = runFixedWorkflow(normalized)
    if result.ok:
      return result
    return runRecoveryAgent(result.failureContext)

  if hasLocalizedUncertainty(normalized):
    fixedContext = prepareCompactContext(normalized)
    return runLocalAgent(fixedContext, allowedTools, stopCondition)

  return runDynamicAgent(normalized, budget, allowedTools, stopCondition)
```

## State Handoff Template

Prefer compact state objects over chat history:

```json
{
  "task_id": "string",
  "intent": "string",
  "input_refs": ["path-or-url"],
  "known_fields": {},
  "workflow_stage": "string",
  "uncertainty": "string",
  "allowed_tools": ["string"],
  "constraints": ["string"],
  "validation_schema": {},
  "failure_context": {
    "step": "string",
    "error": "string",
    "observations": []
  }
}
```

## Design Output Template

```markdown
## Workflow Diagnosis

Current design:

Main token/stability risks:

## Step Classification

| Step | Current approach | Recommended level | Implementation | Why |
|---|---|---|---|---|

## Recommended Architecture

Use:

Avoid:

## Agent Nodes

| Agent node | Uncertainty handled | Tools | Stop condition | Fallback |
|---|---|---|---|---|

## Token Controls

- State passed as:
- Large artifacts passed by:
- Tool results summarized by:
- References loaded when:

## Next Steps

1.
2.
3.
```

## Common Refactors

| Over-Agentic pattern | Lower-cost replacement |
|---|---|
| Agent formats structured data | Schema mapper or template |
| Agent runs a fixed checklist | Deterministic workflow |
| Agent retries blind failures | Validation plus targeted recovery handler |
| Multiple Agents share full context | Shared compact state plus references |
| Planner decides every step | Static DAG with Agent only at uncertain nodes |
