---
name: docs-verify
description: "Verifies whether a document conveys its intended meaning to readers who lack the original conversational context, using an independent subagent. Trigger right after writing any document that a context-less executor will act on — handoffs, onboarding materials, subagent task instructions, project directives, multi-turn conversation summaries, internal project READMEs."
---

## Why this matters

LLMs are known to have strong comprehension but weaker expressive precision. When an LLM organizes a document, it tends to write under the implicit premise of a reader who already shares the session's accumulated context and information — not the fresh human or agent who will actually deep-dive into the project by reading "the document + project files." This tendency is strong and well-documented.

Therefore, an independent agent must verify: "does this document actually transfer the information I intend to transfer?"

The author cannot detect omissions by re-reading their own document, because they already hold the context. **Self-verification is structurally invalid.**

What we verify is not prose fluency, but whether someone acting on the document alone forms the same thinking as you — whether they reconstruct the same **decision boundaries**.

## 1. Probe design

Only you, the author, know the implicit agreements that may not have made it into the document. Generate questions that verify not just the explicit facts to be conveyed, but whether tacit knowledge and context got through.

Freely mix reconstruction, boundary, priority, counterfactual, trap, and tacit-knowledge questions.

For complex documents, organize an intent packet before designing probes: goals/non-goals, constraints and priority ordering, term definitions, positive and negative examples, decision rules. Construct probes densely and strictly.

## 2. Blind receiver test

Send document + probes to a **fresh agent with zero session context**. Asking an agent within the same session shares the context and invalidates the test itself. Default: a single lightweight model (Sonnet/Flash). On failure, retry with Opus/Pro to determine whether it is a model limitation or the document's failure to transfer context and information properly.

## 3. Evaluation and iteration

**You** evaluate. Not a score — focus on **where it diverged**. Pass threshold: trap questions passed, boundary/tacit-knowledge questions answered via citation from the document. If it falls short, revise the document and retest **with a new session**.

## CRITICAL — no reward hacking

What needs revision is not the surface wrong answer, but the transfer quality that produced it. When the expected answer is B and the receiver answers A, LLMs have a very strong tendency to append "it's B, not A" to the document body. That is only patching the test. Build a document that improves the fundamental quality of explanation and information transfer — not a document that passes the test.
