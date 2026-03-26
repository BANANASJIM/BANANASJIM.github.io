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

---
### Architectural Principles

Great systems are not defined by their ability to avoid failure, but by their ability to survive it. Designing for resilience means accepting failure as an inevitability and building a system that can recover with grace and precision.

1.  **Assume Failure.** The first step in building a resilient system is to assume it will fail. A network connection will drop, a service will time out, or the machine will restart. A workflow that depends on a single, uninterrupted process to succeed is a workflow that is doomed to fail. The most important question is not "what if it fails?" but "when it fails, what happens next?"

2.  **Externalize State for Auditability.** An agent's workspace should be treated like an airplane's "black box" flight recorder. Every significant decision, tool call, and output must be externalized to a durable artifact (e.g., a file, a log entry). This ensures that when a failure occurs, there is a perfect, immutable record of what happened, enabling precise debugging and recovery.

3.  **Recovery as Replay.** When state is externalized, recovery is not a matter of guesswork. A new process can inspect the last successfully written artifact and resume the workflow from that exact point. The system is not trying to reconstruct a lost "thought process"; it is simply continuing a sequence of state transformations from the last known-good state.
