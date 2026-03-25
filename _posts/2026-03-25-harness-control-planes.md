---
layout: post
title: "Harness Control Planes: Separating Decision and Execution Layers"
date: 2026-03-25 12:04:00 -0700
categories: [ai, context-engineering]
---

An AI agent is given the goal of "optimizing our cloud spend." It correctly identifies an unused, oversized database. It then formulates a plan: "1. Snapshot the database. 2. Create a new, smaller database. 3. Restore the snapshot. 4. Delete the old database." The agent proceeds to execute this plan. It successfully performs steps 1-3. Then, for step 4, it constructs the command `aws rds delete-db-instance --db-instance-identifier prod-database-main` and executes it. The production database is gone.

The agent did exactly what it planned to do. The plan was logical. The execution was flawless. The failure was one of governance. The agent, a single, monolithic entity, possessed both the ability to *decide* to delete the database and the ability to *execute* that deletion. The decision-making logic and the execution capability were fused together.

This fusion of decision and execution is a ticking time bomb in any powerful agent system. To build safe and reliable agents, we must architecturally separate the *control plane* (the layer that decides what to do) from the *data plane* (the layer that performs the action).

### The Monolithic Agent: A Single Point of Failure

Most simple agent implementations are monolithic. A single model instance, running within a single process, is given a prompt that includes the goal, the context, and the documentation for a set of powerful tools. The model's output is then parsed, and if it contains a tool call, that tool is executed with the provided arguments.

In this design, the model is the planner, the scheduler, and the trigger-man, all in one. This concentration of power creates several critical risks:

1.  **Unreviewable Decisions:** High-risk decisions are made and executed in a single, atomic step. The decision to delete a database and the command to do so are generated in the same thought process. There is no opportunity for an external system—or a human—to review and veto the decision before it is acted upon.
2.  **Escalation of Privilege:** The agent process must hold the credentials for every action it *might* perform. An agent tasked with cloud optimization must have, at all times, the IAM permissions to delete databases, modify security groups, and resize instances, even when it's just in the research phase of its task. This violates the principle of least privilege.
3.  **Brittleness to Prompt Injection:** An attacker who can influence the agent's prompt can potentially trick the monolithic agent into executing dangerous commands. Because the decision and execution are tightly coupled, a single malicious input can propagate directly to a destructive action.

### Anti-Pattern: The "Are You Sure?" Prompt

A common but ineffective attempt to solve this is to add another layer of prompting. When the agent decides to perform a risky action, the harness catches it and asks the same model, "This seems dangerous. Are You sure you want to do this?"

This is a weak defense. It relies on the same semantic, non-deterministic process that made the initial decision. The model, having just justified its decision, is highly likely to double down. It will rationalize its choice and reply, "Yes, I am sure. According to my plan, this is the correct next step." We are asking the component that made the mistake to be its own safety inspector.

This also fails to address the underlying architectural flaw: the agent still possesses the raw capability to execute the dangerous action.

### Pattern: A Separate Control Plane and Data Plane

A robust architecture separates the agent system into at least two distinct layers:

*   **The Control Plane:** This is the "thinking" layer. Its job is to reason, plan, and *propose* actions. It is an agent, likely a powerful language model, whose output is not commands, but structured *intents*. It might have access to read-only tools to gather information, but it has zero access to tools that can modify the state of the external world. Its final output is a proposal, like: `{ "action": "delete_database", "target": "prod-database-main-oversized-old", "reason": "This database has been successfully migrated to a new, smaller instance." }`.
*   **The Data Plane:** This is the "doing" layer. It is a simple, deterministic system that receives intents from the control plane. Its job is to validate these intents and, if they are valid, execute them. It is not a language model. It is a piece of traditional, auditable code.

The data plane acts as a mandatory governance layer. When it receives the `delete_database` intent, it doesn't just blindly execute it. It performs a series of checks defined in code:

1.  **Policy Enforcement:** Does the policy engine allow database deletions at this time? Is there a change freeze in effect?
2.  **Safety Checks:** Has the database been backed up recently? Is the `target` name on a list of protected, mission-critical resources? Does the `reason` field match an approved change request ID?
3.  **Human-in-the-Loop Escalation:** Is this action on the list of operations that require human approval? If so, the data plane does not execute the action. Instead, it pushes a notification to a human operator with the details of the proposed intent and buttons to "Approve" or "Deny."

Only when all these deterministic checks pass does the data plane execute the final action.

### Practical Guidance: Building a Two-Tier Harness

1.  **Isolate the Control Plane Agent:** Run the thinking agent in a sandboxed environment. Provide it with a set of read-only tools (`aws rds describe-db-instances`) and a single, special tool: `propose_action(intent)`. This is the *only* way it can affect the outside world. It should have no credentials for any other services.
2.  **Define a Structured Intent Schema:** Don't let the control plane propose actions in natural language. Define a rigid JSON schema for your intents. This makes parsing and validation on the data plane side simple and reliable. The schema *is* your API between the layers.
3.  **Build a Deterministic Data Plane Executor:** The data plane can be a simple script, a serverless function, or a workflow engine. It receives the intent JSON, runs it through a pipeline of validation code, and, if successful, executes the action using its own, tightly-scoped credentials.
4.  **Codify Your Governance Rules:** The rules for what is and isn't allowed should be explicit code in the data plane. This is far more reliable than putting them in a prompt. Your safety net should be made of `if` statements, not semantic suggestions.

### Conclusion: From Monolith to Microservices

Separating the control and data planes is essentially applying the principles of microservice architecture to agent design. We are breaking a complex, monolithic system into smaller, specialized, and independently-secured components.

The control plane is our "decision service." It's creative, flexible, and powerful, but fundamentally untrusted. Its job is to think, not to do.

The data plane is our "execution service." It's simple, stupid, and deterministic, but fundamentally trusted. It holds the keys to the kingdom and operates under a strict, auditable set of rules.

This separation allows us to harness the incredible planning and reasoning capabilities of modern LLMs without giving them the direct power to cause catastrophic failures. It moves our safety mechanisms out of the fuzzy, probabilistic world of natural language prompts and into the clear, deterministic world of code. By building systems with this architectural discipline, we can create agents that are not only powerful but also predictably safe.