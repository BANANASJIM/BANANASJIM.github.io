---
layout: post
title: "Progressive Disclosure for Agents: Staged Context for Reliable Execution"
date: 2026-03-25 12:02:00 -0700
categories: [ai, context-engineering]
---

You hire a new junior developer and give them their first task: "Fix the login bug." You also hand them the entire 5-million-line codebase, the complete architectural diagrams for every service, the full 5-year backlog of all bugs ever reported, and the company's 10-year financial projections. You then expect them to productively fix the bug.

This is, of course, absurd. No human works this way. We'd give them a link to the specific bug ticket, pointers to the relevant files in the authentication service, and a quick overview of the immediate goal. We provide context progressively, as needed.

Yet, when we work with AI agents, we often do the equivalent of this data dump. We front-load the prompt with every piece of information the agent *might* possibly need for a multi-step task, hoping it can correctly parse, prioritize, and apply the right information at the right time. This "everything up front" approach is a primary source of agent failure.

To build reliable agents, we must adopt a strategy of progressive disclosure, staging the delivery of context to align with the agent's current focus.

### The Problem with Context Overload

When we provide too much information at once, we trigger a cascade of failure modes:

1.  **Instruction Obfuscation:** The most critical instruction for the *current step* is buried in a sea of context relevant only to future steps. This is a direct cause of the compliance failures discussed in previous articles. The agent might focus on a constraint meant for step 5 while working on step 1, leading it down a completely wrong path.
2.  **Loss of Focus:** Faced with an overwhelming amount of information, the model's attention scatters. It may latch onto an interesting but irrelevant detail from the documentation and ignore the immediate task. It tries to solve for the entire problem at once, rather than proceeding step-by-step.
3.  **Increased Hallucination:** When the context is too broad, the model has more room to invent connections and relationships that don't exist. It might try to use a tool mentioned in the documentation for one service to solve a problem in another, simply because both were present in the initial prompt.
4.  **Performance Degradation:** Large contexts are computationally expensive and slow. More importantly, they often lead to lower-quality output. A model given a tightly-scoped context for a specific task will almost always produce a better, more relevant response than a model given a sprawling, unfocused one.

We are, in effect, forcing the agent to do its own information foraging and prioritization, a complex cognitive task that even humans struggle with.

### Anti-Pattern: The Monolithic "God Prompt"

The most common manifestation of this problem is the "God Prompt"—a single, massive prompt template designed to handle every possible stage of a complex workflow. It contains placeholders for the mission, the plan, the scratchpad, the full tool documentation, user profiles, style guides, and more.

The harness's job is simply to fill in all the placeholders and send this behemoth to the model for every single step. The hope is that the model will learn to navigate this complex structure, paying attention only to the relevant sections.

This rarely works for long-running tasks. The structure itself consumes a huge number of tokens, and the model's ability to "pay attention" to the right part of the prompt degrades as the filled-in data grows. It's a brittle, inefficient, and unreliable way to manage context.

### Pattern: Staged Context via a Multi-Phase Harness

The solution is to design an agent harness that acts as a context manager, implementing progressive disclosure. The harness, not the model, is responsible for managing the task's state and providing the model with only the information needed for the current stage.

This leads to a multi-phase design, where different stages of the task use entirely different prompt structures.

Consider a simple "write code and open a pull request" agent. A monolithic approach would put everything in one prompt. A progressive disclosure approach would break it into distinct phases:

**Phase 1: Planning**
*   **Goal:** Decompose the user's request into a high-level plan.
*   **Context Provided:**
    *   The user's request ("Fix the login bug").
    *   High-level documentation about the available services.
    *   A tool to list files in a directory (`list_files(path)`).
*   **Expected Output:** A sequence of steps, e.g., "1. Explore the `auth-service` code. 2. Identify the bug. 3. Write a fix. 4. Write a test. 5. Open a PR."
*   **Prompt Structure:** Tightly focused on planning. No need for detailed coding function documentation or PR templates yet.

**Phase 2: Implementation (repeated for each step)**
*   **Goal:** Execute a single step from the plan (e.g., "Write a fix").
*   **Context Provided:**
    *   The specific step being executed.
    *   The relevant files read from the previous step.
    *   A rich set of tools for reading, writing, and editing code (`read_file`, `write_file`, `edit_file`).
*   **Expected Output:** The code for the fix, written to a file.
*   **Prompt Structure:** Heavily focused on code generation and file manipulation. The overall plan can be summarized to a single line in the prompt to maintain focus.

**Phase 3: Review and Pull Request**
*   **Goal:** Create a pull request for the changes.
*   **Context Provided:**
    *   A diff of the changes made.
    *   Tools for interacting with the Git/GitHub APIs (`git_diff`, `github_create_pr`).
    *   The PR template and contribution guidelines.
*   **Expected Output:** A call to the `github_create_pr` tool with a well-written title and description.
*   **Prompt Structure:** Focused on summarization and communication. The code itself is less important than the summary of *why* it was changed.

In this model, the harness is a state machine. The completion of one phase triggers the transition to the next, and with it, a completely new, purpose-built context for the model.

### Practical Guidance: How to Stage Your Context

1.  **Decompose Your Workflow:** Identify the distinct logical phases of your agent's task. Common phases include Planning, Research, Implementation, Review, and Communication.
2.  **Define the Context for Each Phase:** For each phase, determine the minimal set of information the model needs to succeed.
    *   What is the *one* primary goal of this phase?
    *   Which tools are essential for this phase? Exclude all others.
    *   What background information is critical? Summarize or exclude everything else.
3.  **Use an Artifact-First Approach:** The output of one phase should be a durable artifact (a file, a database entry) that becomes the input for the next. The plan from the Planning phase is written to `plan.md`. The code from the Implementation phase is written to `.py` files. This makes the state transitions clean and decouples the phases.
4.  **The Harness as State Manager:** The harness code is responsible for loading the correct prompt template for the current phase, populating it with the artifacts from the previous phase, and calling the model. The model doesn't need to know what phase it's in; it just gets a clear, focused task.

### Conclusion: From Data Dump to Dialogue

Progressive disclosure is about changing our mental model of agent interaction. We are not programming by dumping data into a context window. We are having a structured dialogue with the model, where each turn in the conversation is carefully staged to provide focus and clarity.

By taking on the burden of context management, our agent harnesses can dramatically improve reliability. We stop asking the model to be a master of prioritization and instead empower it to be a master of execution for a series of well-defined, tightly-scoped tasks. This shift—from a single, monolithic god prompt to a dynamic, multi-phase conversation—is a fundamental step in the evolution from brittle toys to reliable, production-ready AI agents.