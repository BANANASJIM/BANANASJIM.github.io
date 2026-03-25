---
layout: article
title: "Text-Native Tool Surfaces"
description: "Why composable, Unix-like interfaces improve agent reliability."
---

LLMs are text prediction systems. Harnesses should respect that.

Tool surfaces that are text-native, stable, and composable are easier for models to use reliably than opaque UIs or ad-hoc APIs.

## Practical implications

Prefer:
- clear CLI contracts
- predictable stdout/stderr
- line-oriented outputs
- composable pipelines

This aligns with patterns heavily represented in public technical corpora.

## Why Unix composition helps

Unix-style pipes encourage single-purpose tools and explicit data flow. That reduces ambiguity for both humans and models.

## Anti-patterns

- Tool outputs with unstable formats per run
- Hidden side effects with no textual trace
- Monolithic “do-everything” commands without inspectable intermediates

## References

- Manus, *Context Engineering Lessons*  
  https://manus.im/blog/Context-Engineering-for-AI-Agents-Lessons-from-Building-Manus
- Prompting Guide, *Context Engineering Guide*  
  https://www.promptingguide.ai/guides/context-engineering-guide
