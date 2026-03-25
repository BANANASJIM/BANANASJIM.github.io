---
layout: article
title: "Stateless Agent Operations"
description: "Why durable state belongs in files and version control, not chat memory."
---

Treating LLM sessions as long-term memory systems is a reliability trap.

A safer pattern is stateless execution:
- the model processes current context,
- durable state lives in files and version history,
- each run can be replayed and audited.

## Why stateless wins

- Reproducibility improves.
- Recovery is easier.
- Audits are evidence-based.
- Session resets do not destroy system memory.

## Durable state primitives

- Markdown/structured files for decisions and plans
- Git commits for atomic checkpoints
- Branches/worktrees for isolated alternatives
- Explicit references between artifacts

## Operational pattern

- Read current objective + required artifacts
- Execute one bounded change
- Commit with traceable metadata
- Re-enter next step with fresh context

## Anti-patterns

- “Remember this for later” without writing it.
- Relying on long chat for process-critical state.
- Merging multiple objectives into one opaque run.

## References

- Letta, *Context Repositories*  
  https://www.letta.com/blog/context-repositories
- arXiv, *Git Context Controller*  
  https://arxiv.org/abs/2508.00031
- arXiv, *Everything is Context*  
  https://arxiv.org/abs/2512.05470
