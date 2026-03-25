---
layout: article
title: "Harness Control Planes"
description: "Separate decision and execution planes to reduce agentic drift."
---

A robust harness separates **decision** from **execution**.

If one runtime both decides policy and executes changes, drift and unsafe shortcuts increase.

## Two-plane model

- **Decision plane**: intent alignment, role selection, escalation, approval routing.
- **Execution plane**: run scoped tasks under constrained permissions.

## Why separation matters

- Clear accountability
- Reduced privilege leakage
- Better audit trails
- Easier recovery and replay

## Key interfaces

- Structured task contract (inputs, constraints, expected outputs)
- Explicit escalation channel
- Standardized completion signal
- Merge/reconciliation gate

## Anti-patterns

- Giving workers implicit policy authority.
- Letting execution agents self-approve exceptions.
- Hidden side effects outside declared task scope.

## References

- AWS Prescriptive Guidance, *Prompt, agent, and model lifecycle management*  
  https://docs.aws.amazon.com/prescriptive-guidance/latest/agentic-ai-serverless/prompt-agent-and-model.html
- Microsoft, *Multi-Agent Reference Architecture*  
  https://microsoft.github.io/multi-agent-reference-architecture/
- Inkeep, *Context Engineering: Why Agents Fail*  
  https://inkeep.com/blog/context-engineering-why-agents-fail
