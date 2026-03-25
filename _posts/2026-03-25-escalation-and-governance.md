---
layout: article
title: "Escalation and Governance"
description: "Human authority without killing automation throughput."
---

Automation without governance fails quietly; governance without automation stalls delivery.

Good harness design balances both by making escalation explicit.

## Escalation model

- Default path: constrained auto-execution
- Exception path: human decision required
- Emergency path: bypass with mandatory audit trail

## Design requirements

- Fail-closed in non-interactive environments
- Evidence included in escalation request
- Decision recorded with timestamp and scope
- Post-action reconciliation required

## What to escalate

- Cross-boundary writes
- Policy violations
- Irreversible operations
- Ambiguous authority conflicts

## Anti-patterns

- Silent auto-allow in headless mode
- Global permanent grants for convenience
- Unlogged emergency bypasses

## References

- Anthropic, *Effective Context Engineering for AI Agents*  
  https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
- AWS Prescriptive Guidance, *Agent lifecycle governance*  
  https://docs.aws.amazon.com/prescriptive-guidance/latest/agentic-ai-serverless/prompt-agent-and-model.html
