---
layout: article
title: "Failure, Recovery, and Auditability"
description: "Designing agent workflows to fail safely and recover cleanly."
---

Reliable systems are not systems that never fail. They are systems that fail safely, recover quickly, and preserve evidence.

## Failure taxonomy

- Constraint violation
- Intent mismatch
- Environment/runtime error
- Unknown/ambiguous outcome

Each class should have a predefined response.

## Recovery loop

1. Detect and classify
2. Record deviation
3. Choose disposition (fix / accept / defer)
4. Reconcile before merge

## Audit requirements

- Every exception decision is recorded
- Every bypass has explicit signal
- Every deferred issue has a destination and owner

## Anti-patterns

- Silent retries without evidence
- Merge-first, reconcile-later culture
- Untracked emergency edits

## References

- AWS Prescriptive Guidance, *Lifecycle management*  
  https://docs.aws.amazon.com/prescriptive-guidance/latest/agentic-ai-serverless/prompt-agent-and-model.html
- Inkeep, *Why Agents Fail*  
  https://inkeep.com/blog/context-engineering-why-agents-fail
