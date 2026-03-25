---
layout: article
title: "Instruction Compliance Windows"
description: "How instruction density affects reliability in agentic systems."
---

Large prompts feel safe, but they often reduce reliability.

The practical issue is not context length alone; it is **instruction density**. As rule count grows, models begin to drop constraints selectively, then globally. This behavior is repeatedly reported in instruction-following studies and production reports.

## Core principle

Treat instructions as a constrained budget, not a dumping ground.

A useful operating model:
- Keep always-loaded governance compact.
- Move role-specific details to on-demand docs.
- Push deterministic checks into tools, not prose.

## Why adherence drops

Three forces stack together:
1. **Priority dilution** — too many “must” statements flatten priority.
2. **Middle loss** — important constraints buried in the center are under-weighted.
3. **Conflict ambiguity** — overlapping rules create silent arbitration by the model.

## Practical compliance window

There is no universal magic number, but practical practice is clear:
- Keep core governance short and stable.
- Keep role guides scoped and explicit.
- Avoid repeated wording of the same rule in multiple places.

A strong heuristic:
- **Global rules:** short and policy-like.
- **Local rules:** task- and role-bound.
- **Machine-enforceable rules:** moved to hooks/tests/lints.

## Anti-patterns

- “One giant rules file for everything.”
- Repeating formatting constraints in every prompt.
- Mixing immutable policy with task hints in the same block.
- Using prose to enforce what tooling can guarantee.

## Implementation pattern

Use a 3-layer model:
1. **Core policy layer** (always loaded)
2. **Role layer** (loaded by responsibility)
3. **Task layer** (loaded by current objective)

Then run deterministic gates before and after generation.

## What to measure

- Constraint violations per 100 runs
- Rework caused by missed instructions
- Ratio of prose constraints vs tool-enforced constraints
- Prompt size growth over time (governance bloat)

## References

- Anthropic, *Effective Context Engineering for AI Agents*  
  https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
- Chroma Research, *Context Rot*  
  https://research.trychroma.com/context-rot
- LangChain, *Context Engineering for Agents*  
  https://blog.langchain.com/context-engineering-for-agents/
- arXiv, *How Many Instructions Can LLMs Follow?*  
  https://arxiv.org/html/2507.11538v1
