---
layout: article
title: "Intent Alignment as Infrastructure"
description: "Why AI engineering needs harness design and context architecture, not just better prompts."
---

Most teams still evaluate coding agents through a narrow lens: model capability, benchmark scores, and prompt tricks.

That lens is incomplete.

In real engineering environments, failures rarely come from a model’s inability to produce code. They come from **intent drift** across long workflows: requirements mutate, assumptions disappear between sessions, local optimizations override system goals, and quality gates fail to protect architectural intent.

If you care about reliable outcomes, the core question is not:

> "How smart is the model?"

It is:

> "How well does the system preserve intent across decisions, execution, and review?"

This is where **harness design** and **context engineering** become first-class concerns.

## 1) Prompting is necessary, but insufficient

Prompt engineering can improve local output quality. It does not, by itself, guarantee consistency across multi-step engineering work.

Why?

- Long contexts degrade performance (context-rot behavior has been repeatedly observed in empirical studies)
- Instruction overload reduces adherence as rule count grows
- Multi-agent workflows amplify communication and state-consistency failures
- Human approval often becomes symbolic when escalation paths are weakly designed

These are systems failures, not wording failures.

## 2) Intent alignment is a systems property

Intent alignment must be enforced through architecture:

- **Context architecture**: what information enters, when, and in what granularity
- **Control architecture**: who decides, who executes, what can be bypassed, what must be audited
- **Verification architecture**: which constraints are deterministic (hooks/lints/tests) vs semantic (review/judgment)

In mature setups, model intelligence is only one layer. Reliability comes from constraints and feedback loops around it.

## 3) The practical shift: from assistant usage to operating model

A robust AI engineering stack typically shares these properties:

1. **Stateless execution bias**  
   Durable state lives in files, version control, and artifacts—not chat history.

2. **Minimal effective context**  
   Context is budgeted for signal, not volume.

3. **Progressive disclosure**  
   Load high-level instructions first, task-specific materials later, deep references on demand.

4. **Deterministic enforcement first**  
   If a rule can be machine-checked, enforce it with tools—not repeated prompt text.

5. **Human authority at decision boundaries**  
   Escalation protocols should be explicit, auditable, and hard to bypass accidentally.

6. **Composable text-native tooling**  
   Systems that expose clear text interfaces and composable command surfaces are generally easier for LLM workflows to operate reliably.

## 4) Why this matters now

As model capabilities rise, failure cost rises too. A more capable model can generate incorrect but plausible decisions faster, at larger scope.

So the priority is not simply smarter agents. The priority is **intent-preserving infrastructure**.

That is the purpose of this series.

## What this series will cover

1. **Instruction Compliance Windows** — how instruction load affects reliability  
2. **Hot vs Cold Memory** — practical context stratification patterns  
3. **Progressive Disclosure** — staged loading for lower drift  
4. **Stateless Agent Operations** — reproducibility through artifact-first workflows  
5. **Harness Control Planes** — separation of decision and execution layers  
6. **Escalation & Governance** — human-in-the-loop without killing velocity  
7. **Deterministic Gates vs Semantic Review** — where each belongs  
8. **Text-Native Tool Surfaces** — why composability improves operational reliability  
9. **Failure Modes and Recovery** — designing for auditability and rollback  
10. **Reference Blueprint** — a practical implementation checklist

## References

- Anthropic, *Effective Context Engineering for AI Agents*  
  https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
- LangChain, *Context Engineering for Agents*  
  https://blog.langchain.com/context-engineering-for-agents/
- Chroma Research, *Context Rot*  
  https://research.trychroma.com/context-rot
- arXiv: *How Many Instructions Can LLMs Follow?*  
  https://arxiv.org/html/2507.11538v1
- Letta, *Context Repositories*  
  https://www.letta.com/blog/context-repositories
- JetBrains Research, *Efficient Context Management for Coding Agents*  
  https://blog.jetbrains.com/research/2025/12/efficient-context-management/
- Spotify Engineering, *Context Engineering for Background Coding Agents*  
  https://engineering.atspotify.com/2025/11/context-engineering-background-coding-agents-part-2
