---
layout: post
title: "A Reference Blueprint for AI Harness + Context Engineering"
date: 2026-03-25 12:08:00 -0700
categories: [ai, context-engineering]
---

Over the course of this series, we've explored a set of principles for building reliable, production-grade AI agents. We've moved from the simplistic model of a single, monolithic "god prompt" to a more sophisticated, structured, and architecturally sound approach. The core theme has been consistent: reliability comes not from a smarter model or a better prompt, but from better engineering of the system *around* the model.

This is the discipline of Harness and Context Engineering.

The harness is the machinery that drives the agent. The context is the fuel it runs on. Getting them right is the difference between a brittle prototype and a resilient, scalable system.

This final article serves as a reference blueprint, weaving together the patterns we've discussed into a cohesive architectural model. This is not a literal code implementation, but a conceptual framework for designing robust agentic systems.

### The Blueprint: A Multi-Layered Architecture

A well-architected agent system is not a single, monolithic block. It is a set of distinct, cooperating layers, each with a clear responsibility.

**Layer 1: The Artifact-Based State Store**

This is the foundation. The single source of truth for the agent's task is not in memory, but on disk (or in a database).

*   **Principle:** Stateless Agent Operations.
*   **Implementation:** A dedicated workspace directory for each task. Every piece of state—the mission, the plan, intermediate results, logs, final outputs—is a file.
    *   `mission.md`: The initial, high-level goal.
    *   `plan.md`: The agent's generated plan.
    *   `workspace/`: A subdirectory for created and modified artifacts.
    *   `logs/`: A directory for detailed, structured logs (e.g., `tool_calls.jsonl`).
*   **Benefit:** Resilience, reproducibility, and debuggability. The task can be paused, resumed, and inspected at any time simply by looking at the files.

**Layer 2: The Data Plane (The Tool Executor)**

This is the "doing" layer. It is a deterministic, non-intelligent system responsible for executing actions.

*   **Principle:** Harness Control Planes.
*   **Implementation:** A simple process or service that exposes a set of tools. Crucially, these tools should have a text-native interface.
    *   A library of command-line interface (CLI) tools, following the Unix philosophy.
    *   Each tool performs one, well-defined task (e.g., `file-read`, `git-commit`, `api-call`).
    *   The Data Plane holds the necessary credentials and operates under a strict, auditable security policy. It has no language model in it.
*   **Benefit:** Safety and determinism. Risky actions are handled by code, not by a probabilistic model. The tool surface is inspectable and can be used by humans and agents alike.

**Layer 3: The Control Plane (The Agent Core)**

This is the "thinking" layer. It is where the language model lives. Its sole purpose is to decide what to do next.

*   **Principle:** Separating Decision and Execution.
*   **Implementation:** An agent harness that orchestrates the LLM's operation in a loop.
    1.  **Assemble Context:** The harness reads the state from the Artifact Store.
    2.  **Stage Context:** It applies Progressive Disclosure, building a minimal, purpose-built prompt for the current step. It doesn't dump everything.
    3.  **Invoke Model:** It calls the LLM with the staged context. The LLM's only output is a command to be executed by the Data Plane.
    4.  **Execute via Data Plane:** The harness sends the command to the Data Plane and gets back the result (stdout, stderr, exit code).
    5.  **Persist State:** The harness writes the result of the execution to the Artifact Store (e.g., updating a log file or creating a new workspace file).
    6.  The loop repeats.
*   **Benefit:** Focus and clarity. The LLM is freed from the burden of state management and low-level execution details, allowing it to focus on high-level reasoning.

**Layer 4: The Memory Subsystem**

This is not just another part of the artifact store; it's an active system for managing context over time.

*   **Principle:** Hot vs Cold Memory Stratification.
*   **Implementation:**
    *   **Hot Memory:** The context window for a single LLM call. It is assembled on-demand by the harness.
    *   **Cold Memory:** A persistent, searchable knowledge base. This can be a vector database, a full-text search index, or even just a well-organized directory of markdown files.
    *   **The Retriever:** A component of the harness responsible for fetching relevant information from Cold Memory and injecting it into the Hot Memory just-in-time.
    *   **The Consolidator:** A process (which can itself use an LLM) that runs periodically to summarize raw workspace artifacts and logs and commit the distilled knowledge into Cold Memory.
*   **Benefit:** Scalability for long-running tasks. The agent can "remember" vast amounts of information without overflowing its context window.

**Layer 5: The Governance and Escalation Framework**

This is the human interface, designed for efficient oversight.

*   **Principle:** Human Authority Without Killing Throughput.
*   **Implementation:** An asynchronous, tiered system built on top of the Data Plane.
    *   The Data Plane is configured with policies that map specific tool calls to escalation tiers (e.g., `file-write` is Tier 1 FYI, `git-merge` is Tier 2 Soft Approval, `db-delete` is Tier 3 Hard Approval).
    *   An asynchronous notification system (e.g., a Slack bot) handles the communication, allowing the agent to continue working on other tasks while waiting for human input.
    *   The review process itself is a pipeline, using Deterministic Gates (CI checks) before any Semantic Review (human or AI).
*   **Benefit:** A safe, auditable system that uses expensive human attention efficiently.

### Putting It All Together: A Worked Example

Imagine an agent tasked with: "Monitor our user feedback channel and fix any reported bugs."

1.  **Scheduler:** A cron job triggers the agent harness once an hour.
2.  **Harness Starts (Control Plane):**
    *   It checks the **Artifact Store**. Sees no active task.
    *   It assembles a "research" context, providing the agent with the `read-feedback-channel` tool.
3.  **Agent Reasons:** It calls `read-feedback-channel`.
4.  **Data Plane Executes:** The tool runs, fetches new messages, and returns them as text.
5.  **Harness Persists:** The feedback is saved to `artifacts/new_feedback.txt`.
6.  **Harness Loops:**
    *   It assembles a "planning" context, providing the feedback text and tools for file system exploration.
7.  **Agent Reasons:** It identifies a bug report in the feedback. It decides to fix it and creates a `plan.md` file: "1. Replicate the bug. 2. Write a fix. 3. Open PR."
8.  **Harness Loops (and on and on...):** The process continues, with the harness staging the context for each phase (coding, testing, PR creation). Each step reads from and writes to the artifact store. When the agent is ready to open a PR, the **Governance Framework** kicks in, running the CI pipeline (deterministic gates) and then, if it passes, notifying a human for a final semantic review.

### Conclusion: Engineering Over Alchemy

The days of treating agent development as prompt-crafting alchemy are numbered. Building reliable agents is a software engineering discipline. It requires architecture, rigor, and a focus on building robust systems, not just clever prompts.

This blueprint—based on stateless operations, stratified memory, staged context, separated control planes, and efficient human oversight—provides a path forward. It's a model that embraces the power of LLMs for reasoning while grounding them in the deterministic, auditable, and resilient world of classical software engineering. By adopting these principles, we can begin to build the next generation of AI agents: not just impressive demos, but trustworthy partners in our most critical automated tasks.