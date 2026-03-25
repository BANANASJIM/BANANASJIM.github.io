---
layout: article
title: "Hot vs Cold Memory Stratification"
description: "A practical memory model for long-running agent workflows."
---

Most agent failures in long projects are memory failures, not generation failures.

A practical fix is memory stratification:
- **Hot memory**: always-loaded, compact, high-authority constraints.
- **Cold memory**: large, historical, loaded only when relevant.

## Why this works

Always-loading everything increases noise and degrades adherence. Always-loading too little causes repeated rediscovery.

Hot/cold memory creates a stable center with scalable retrieval.

## What belongs in hot memory

- Non-negotiable behavior constraints
- Role boundaries
- Escalation policy
- Critical path entrypoints

Hot memory should be stable and small.

## What belongs in cold memory

- Deep design notes
- Historical decisions
- Previous investigations
- Long-form references

Cold memory should be rich and indexed.

## Retrieval policy

Use intent-gated retrieval:
1. Read objective.
2. Load minimal cold slices tied to objective.
3. Cite source paths in outputs.
4. Drop irrelevant slices in next step.

## Anti-patterns

- Putting historical narrative into always-loaded prompts.
- Embedding full decision logs into every task context.
- Treating chat transcripts as durable memory.

## Minimal memory architecture

- `core-policy.md` (hot)
- `roles/*.md` (warm/on-demand)
- `design/`, `research/`, `archive/` (cold)
- retrieval index over cold store

## References

- Anthropic, *Effective Context Engineering for AI Agents*  
  https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
- Letta, *Context Repositories*  
  https://www.letta.com/blog/context-repositories
- Microsoft, *Multi-Agent Short-Term Memory*  
  https://microsoft.github.io/multi-agent-reference-architecture/docs/memory/Short-Term-Memory.html
- O’Reilly/MongoDB, *Why Multi-Agent Systems Need Memory Engineering*  
  https://www.oreilly.com/radar/why-multi-agent-systems-need-memory-engineering/
