# Agent Minimalism Review Checklist

Use this checklist before approving a new AI workflow, Agent design, automation chain, or token optimization plan.

## 1. Task Shape

- What is the exact input?
- What is the exact output?
- Are input and output structured, semi-structured, or open-ended?
- Can the task be expressed as a fixed sequence?
- Which steps are repeated often enough to justify a reusable template or script?

## 2. Step Classification

For each step, assign the lowest sufficient level:

| Step | Input | Output | Variability | Lowest sufficient level | Reason |
|---|---|---|---|---|---|
| Example | Extract fields | JSON | Low | L0 Rule | Schema is stable |
| Example | Rewrite summary | Text | Medium | L1 Single LLM | Semantic transformation only |
| Example | Publish draft | Platform artifact | Low | L2 Fixed Workflow | Tool sequence is known |
| Example | Diagnose failed publish | Recovery action | High | L3 Local Agent | Needs observation and judgment |

Classification rules:

- If correctness can be checked by schema, use L0.
- If one semantic pass is enough, use L1.
- If steps are known but multi-stage, use L2.
- If only one node is uncertain, use L3.
- If the whole path is unknown, use L4.

## 3. Agent Justification

Every Agent node must answer:

- What uncertainty does this Agent handle?
- What tools can it use?
- What observations does it need?
- What is the stop condition?
- What is the fallback if it fails?
- Can this be replaced by a fixed workflow plus validation?

If the uncertainty cannot be named clearly, the Agent is probably unnecessary.

## 4. Token And Context Risk

Check whether the design creates shared-token growth:

- Does each node receive the full history?
- Are large artifacts copied across nodes?
- Are tool results summarized before being passed onward?
- Are only needed fields passed between steps?
- Can state be stored as compact structured data instead of chat history?
- Are references loaded only on demand?

Risk signal:

> Dynamic workflows can grow token cost combinatorially when many nodes share rich context.

Mitigations:

- Pass artifacts by reference instead of embedding full content.
- Use structured state objects.
- Summarize tool outputs before handoff.
- Keep deterministic steps outside the Agent loop.
- Route only failure cases or uncertain nodes to Agents.

## 5. Stability Review

Ask:

- Which steps are nondeterministic?
- Which steps need retries?
- Which outputs are validated?
- Which failures are expected and recoverable?
- Which steps should never be left to model judgment?
- Does each Agent have a narrow responsibility?

Prefer designs where deterministic steps dominate the main path and Agents are isolated.

## 6. Approval Standard

Approve the design only if:

- Most deterministic work is L0-L2.
- Agent use is limited to named uncertainty.
- Agent boundaries are narrow.
- Context handoff is compact.
- Outputs have validation or acceptance criteria.
- The design can explain why lower levels were insufficient.

Reject or revise if:

- The Agent wraps a fixed checklist.
- The Agent mainly formats, copies, uploads, or validates deterministic data.
- Multiple Agents share the same large context by default.
- The workflow cannot state clear stop conditions.
- The design uses dynamic planning where a fixed DAG would work.
