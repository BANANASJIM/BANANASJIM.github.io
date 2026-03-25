---
layout: article
title: "Progressive Disclosure for Agents"
description: "Load less, decide better: staged context for reliable execution."
---

Progressive disclosure means loading context in layers as needed, not all at once.

This is one of the highest-leverage reliability patterns in agent systems.

## The layer model

1. **Identity layer** — who am I, what are hard constraints?
2. **Role layer** — what does this role own?
3. **Task layer** — what exactly is being done now?
4. **Reference layer** — deep docs loaded only on demand.

## Benefits

- Lower token noise
- Higher instruction adherence
- Better role discipline
- Lower cost and latency

## Design rules

- Each layer should be self-contained.
- Lower layers should not rewrite upper-layer authority.
- Keep references path-based and explicit.

## Good trigger points

Load deeper context only when:
- the task crosses subsystem boundaries,
- ambiguity is detected,
- reviewer flags missing evidence,
- deterministic checks fail and require explanation.

## Anti-patterns

- Preloading all docs “just in case.”
- Embedding large examples in always-on prompts.
- Allowing role docs to override policy docs.

## References

- Anthropic, *Effective Context Engineering for AI Agents*  
  https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
- Cursor Docs, *Rules*  
  https://cursor.com/docs/context/rules
- LangChain, *Context Engineering for Agents*  
  https://blog.langchain.com/context-engineering-for-agents/
- HumanLayer, *Writing a Good CLAUDE.md*  
  https://www.humanlayer.dev/blog/writing-a-good-claude-md
