---
name: docs-verify
description: Verifies, through independent sub-agents, whether a written document accurately conveys the intended meaning even without the original conversation context. Trigger this after writing documents such as multi-turn conversation summaries, handoff docs, onboarding materials, sub-agent task instructions, project briefs, and similar materials.
---

# docs-verify

Tests whether an independent reader can reconstruct the same **decision boundaries** from your document — not whether the writing is smooth, but whether someone acting solely on this document will think the way you do and make the same decisions.

## Why this is needed

LLMs are known to be strong at understanding but weaker at expression. Documents organized by an LLM tend to be written with heavy reliance on the many contextual assumptions and background information already shared by the existing user or across prior sessions, rather than sufficiently accounting for a new reader who must understand the project by reading the document alone.

This skill uses fresh agents as crash-test dummies to detect failures in information transfer and iteratively improve the document.

## Workflow

### Step 1: Write validation questions

Create validation questions.  
Try to find facts that are implicitly assumed but not explicitly stated in the document by referring back and forth between the document and the conversation sessions.
Only you know the subtle context, multi-turn nuance, and implicit agreements that may not have been fully captured in the document.
Trap and subtle questions must be included. If the reader invents an answer, that means they relied on training priors rather than the document.

For complex documents, it can help to organize an **intent packet** before designing questions, so you can more consciously notice what might be missing:

Goal / non-goals / constraints / priority order / term definitions  
Assumptions and unknowns / examples that qualify and examples that do not / failure modes  
Decision rules ("If X and Y conflict, choose X")

### Step 2: Blind recipient test

Send the document + probes to a fresh agent **with no session context**.  
Use at least 2 recipients, and ideally models of different capability levels:

- **Recipient A** (e.g. Opus/pro): responds to the probes
- **Recipient B** (e.g. Sonnet/flash): independently responds to the same probes

Check whether there is a performance gap between a frontier model like Opus/pro and a lighter model.  
If a lighter model such as Sonnet or Flash is inaccurate, you should suspect that the document is not sufficiently robust.  
That said, in such cases you should examine whether the gap comes from the document’s explanation quality or simply from differences in model capability, and you may ask additional questions to agents within the same session.

### Step 3: Evaluate + revise the document ITERATIVELY

You evaluate the returned answers. Focus not on the score, but on **where the interpretations diverged**.  
Synthesize multiple evaluation factors, then either improve the document or end the verification.

However, for an ITERATED document, you must restart from Step 2 using a **new agent session**.  
If you reuse the previous probes, the document will overfit to the test.

#### **CRITICAL** when revising the document

- When the expected answer was B but the agent answered A, LLMs have a very strong tendency to revise the document by writing something like "It is not A, it is B."
- This is REWARD HACKING and must **never** be done. Improve the **quality of the document’s information transfer itself**.
