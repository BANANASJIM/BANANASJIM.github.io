---
layout: post
title: "Deterministic Gates vs Semantic Review: Using Tools for What They Can Prove"
date: 2026-03-25 12:06:00 -0700
categories: [ai, context-engineering]
---

An AI agent, designed to manage a software project, is reviewing a code change submitted by another agent. Its goal is to ensure the change is safe to merge. It reads the code, and its internal monologue sounds something like this: "The code looks reasonable. The logic seems correct. It appears to follow our style guide. I don't *see* any obvious security vulnerabilities. I'll approve it."

The pull request is merged. A week later, the site goes down. A subtle race condition in the "reasonable" code, which was not apparent from a surface-level semantic reading, was triggered under load.

The failure here was not in the agent's ability to reason, but in the task it was assigned. We asked a semantic, probabilistic tool (an LLM) to perform a deterministic, formal verification task. We asked it to *feel* if the code was correct, instead of *proving* it.

To build reliable multi-agent systems, especially those where agents review each other's work, we must be disciplined about what we ask them to do. Review tasks must be separated into two distinct categories: deterministic gating and semantic review.

### The Problem: Confusing "Looks Good" with "Is Correct"

Large language models are masters of semantics. They have a powerful, intuitive grasp of what "good code" looks like. They can spot stylistic errors, comment on clarity, and even identify common anti-patterns. This is the "semantic review." It's the equivalent of a human code review, focusing on readability, maintainability, and architectural soundness. It is incredibly valuable.

However, an LLM cannot, by its nature, *prove* that code is correct. It cannot exhaustively trace every logical path, simulate every possible concurrent state, or formally verify that the code adheres to a specification. These tasks belong to a different class of tools: the deterministic gates.

Deterministic gates are tools that provide a binary, provably correct answer to a specific question.
*   Does the code compile? (The compiler)
*   Does it pass the unit tests? (The test runner)
*   Is it free of known security vulnerabilities? (The static analysis scanner)
*   Does it conform to the style guide? (The linter)

When we conflate these two types of review, we build fragile, unreliable systems. We ask the LLM to do the linter's job, and it misses a formatting error. We ask it to do the test runner's job, and it approves code with a failing test because the logic "seemed plausible."

### Anti-Pattern: The LLM as the Sole Gatekeeper

A dangerous anti-pattern is to set up a workflow where one agent's work is approved or rejected based solely on the opinion of another agent. Agent A submits a pull request. Agent B is given the diff and a prompt: "Review this pull request. If it is high quality and correct, approve it. Otherwise, request changes."

This is a recipe for disaster. Agent B's approval is a semantic judgment, not a guarantee of quality. It's a "vibe check" for code. This creates several problems:

1.  **Unpredictable enforcement:** The same code might be approved one day and rejected the next, based on the whims of the model. The standards are not objective.
2.  **False sense of security:** We see an "Approved by AI" badge and assume the code has been rigorously vetted. In reality, it has only been given a semantic thumbs-up.
3.  **Failure to catch regressions:** An LLM reviewer is unlikely to notice that a change, while seemingly reasonable in isolation, causes a regression in a distant part of the system. A comprehensive test suite would catch this instantly.

### Pattern: A Pipeline of Deterministic Gates Followed by Semantic Review

A robust review process is a pipeline. The work must pass through a series of deterministic gates *before* it is ever presented to a semantic reviewer (human or AI).

The workflow for merging a change should look like this:

1.  **Agent A Submits PR:** The agent proposes a change.
2.  **CI Pipeline (Deterministic Gates):** A continuous integration server automatically triggers and runs a series of checks. This is the data plane of the review process.
    *   `make build` -> **PASS/FAIL**
    *   `make lint` -> **PASS/FAIL**
    *   `make test` -> **PASS/FAIL**
    *   `make scan-vulnerabilities` -> **PASS/FAIL**
3.  **The Hard Gate:** If *any* of these deterministic checks fail, the process stops. The pull request is marked as "Failed CI," and the output of the failing tool is sent back to Agent A. There is no ambiguity, no negotiation. The code is provably broken. It is not ready for review.
4.  **Semantic Review (The Creative Step):** Only if *all* deterministic gates pass is the change presented for semantic review. Now, Agent B (or a human) is invoked. Its prompt is different: "All automated checks have passed. Please review this code for clarity, maintainability, and strategic alignment with our project goals. Provide suggestions for improvement or approve if none are needed."

In this model, we are using tools for what they are good at.

*   **The Tools:** Prove correctness, enforce objective standards, and catch regressions with perfect fidelity.
*   **The LLM:** Provides subjective, creative, and architectural feedback—the kind of nuanced advice that deterministic tools can't offer.

### Practical Guidance for Implementation

1.  **Invest in Your CI:** The foundation of any reliable agentic software development process is a fast, comprehensive, and ruthlessly deterministic CI/CD pipeline. Your test suite, linter, and static analysis tools are your most important governance assets.
2.  **Make CI Output the API:** The feedback loop for a failing build should be entirely automated. The agent that submitted the code should receive the raw output from the failing tool (e.g., the Jest test failure report or the ESLint error list). Its next task is simple and non-negotiable: "Fix the errors reported by the tool."
3.  **Scope the Semantic Review:** Clearly define what you expect from the LLM reviewer in your prompt. Explicitly tell it what *not* to check for. "You do not need to check for style errors or failing tests; the automated pipeline has already verified this. Focus on the 'why' of the change, not the 'what'."
4.  **Chain the Agents:** The process is a chain of responsibility. The CI system is the first agent in the chain. It's a simple, deterministic agent. The LLM reviewer is the second. A human might be the third, for final sign-off. Each agent has a specific, well-defined role.

### Conclusion: Trust, but Verify with Tools

In the world of AI, the old adage "trust, but verify" takes on a new meaning. We can trust our agents to be creative, to reason semantically, and to generate novel solutions. But we must *verify* their work with tools that are incapable of lying, guessing, or being "mostly sure."

Stop asking your agents to perform tasks that require deterministic proof. Let the compiler be the compiler. Let the test runner be the test runner. Let your linter be your linter.

Reserve your powerful language models for the tasks that only they can do: understanding intent, providing architectural guidance, and judging the subjective elegance of a solution. By separating the provable from the plausible, we can build multi-agent systems that are not only fast and innovative but also robust, reliable, and worthy of our trust.