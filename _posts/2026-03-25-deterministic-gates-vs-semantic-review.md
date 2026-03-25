---
layout: article
title: "Deterministic Gates vs Semantic Review"
description: "Use tools for what tools can prove; use reviewers for what tools cannot."
---

A common mistake is asking LLMs to enforce constraints that deterministic tools can verify perfectly.

## Division of labor

**Deterministic gates** should handle:
- formatting and naming rules
- reference integrity
- required metadata
- test/build checks

**Semantic review** should handle:
- intent alignment
- architecture fit
- risk tradeoffs
- design coherence

## Why this split works

- Lower context load
- Fewer repeated instructions
- Better reliability under scale
- Cleaner failure diagnosis

## Anti-patterns

- Prompting “remember style X” instead of linting style X.
- Treating semantic reviewer as primary syntax checker.
- Shipping without machine-verifiable pre-merge checks.

## References

- Spotify Engineering, *Context Engineering for Background Coding Agents*  
  https://engineering.atspotify.com/2025/11/context-engineering-background-coding-agents-part-2
- LangChain, *Context Engineering for Agents*  
  https://blog.langchain.com/context-engineering-for-agents/
