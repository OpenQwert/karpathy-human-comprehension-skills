---
name: human-comprehension
description: >
  Make complex agent results easier to review: explain interacting changes,
  multi-step debugging conclusions, and architecture with concise text,
  tables, or diagrams while preserving evidence and verification limits.
  Use when these relationships or limits materially affect a human decision.
---

# Human Comprehension

Help the reader understand what happened, inspect the evidence, and decide
what to do. Choose a representation that reduces the combined cost of
generation, reading, and review while preserving meaning.

## When to apply

Apply at a final handoff, a blocked or failed handoff, or a significant result
that needs human review. Explanation tasks can gather evidence and choose a
representation together. Keep routine progress updates brief.

Use this skill when interacting changes, causal steps, competing explanations,
architecture, comparison dimensions, or verification limits affect review.
File count and response length are clues, not thresholds. A complex problem
in one file can need a diagram; a mechanical edit across many files can need
only a sentence. Simple facts, values, and commands usually need a direct answer.

Respect the user's language, format, and desired depth. Explicit invocation
does not require a diagram or a new file. Discovery and automatic invocation
depend on the host.

## Ground the handoff

Use the user's goal, actual changes, source material, observed tool output,
calculations, and verification records already available in the task. Do not
require a new intermediate schema or private reasoning as evidence.

Distinguish these responsibilities:

- Task verification checks the underlying behavior or conclusion. Report only
  checks actually performed and their coverage.
- Presentation fidelity checks whether the explanation preserves that result.
  It does not establish that the underlying task is correct.

Complete the domain work and required verification within the task's scope.
For a partial, blocked, or failed task, report that state and the supported
findings. This skill does not expand authorization for changes, installation,
publishing, or external calls. Read additional evidence within the existing
scope if needed; otherwise qualify or omit an unsupported claim.

## Choose the representation

Start with concise text. Add a format only when it helps a specific review action.

| Review action | Representation |
| --- | --- |
| Understand an outcome, condition, or action | Prose or short steps |
| Compare alternatives, attributes, or evidence status | A compact table with the key judgment |
| Follow interactions, branches, transitions, or a causal chain | A short conclusion plus a focused diagram |
| Inspect changes and their limits together | Text with a necessary table or diagram |
| Review partially supported relationships | Qualified text or a diagram restricted to the supported part |

Combine formats when needed. Do not use a complexity score or an ordered
ladder of increasingly rich formats.

When a diagram would help, read [Diagram guidelines](references/diagram-guidelines.md)
before producing it. Keep the explanation readable if rendering is unavailable.

v0.1 does not automatically generate HTML or video. If the user explicitly
requests such an artifact, follow the relevant task workflow and host
capabilities, preserving these fidelity rules.

## Make the result reviewable

Lead with the actual outcome or current state. Explain the decisive changes,
relationships, and reasons. Put important evidence and qualifications near
the claims they support; link deeper details when useful.

Include what matters to this handoff:

- Outcome and completion state.
- Changes, relationships, and reasons needed for review.
- Inspectable evidence and actual verification coverage.
- Uncertainty that could change the judgment.
- A concrete decision only when the user genuinely needs to make one.

These are information responsibilities, not mandatory headings. Avoid empty
sections, repeated summaries, invented risks, and unnecessary next decisions.

Use clear actors and conditions, consistent terms, and executable steps.
Preserve technical nouns, identifiers, paths, commands, and API names. Simplify
wording without deleting uncertainty or turning a conditional claim into a
universal claim. These principles apply across languages; do not claim
ASD-STE100 compliance or a measurable percentage of compliance.

## Check fidelity before delivery

Compare key claims and every added visual relationship with available evidence:

- **Claims and status:** Add no unsupported facts. Preserve partial, blocked,
  or failed status.
- **Numbers:** Preserve values, units, denominators, and scope. State the basis
  of derived quantities; do not invent precision or ambiguous speedup claims.
- **Relationships:** Preserve call, dependency, flow, state, and causal meaning.
  Co-occurrence alone does not establish causation.
- **Uncertainty:** Keep confirmed observations, inferences, unknowns, and
  unverified areas distinguishable.
- **Verification:** Report only observed checks. Say when relevant tests were
  not run. A fidelity check is not a passing test or build.
- **Traceability:** Point important claims to accessible source locations or
  observed records. Do not fabricate links, locations, or check results.
- **Consistency:** Text, tables, and diagrams must agree about the same facts,
  states, numbers, and relationships.

Correct conflicts before delivery. When evidence is missing, narrow or qualify
the explanation, or remove the unsupported relationship. This self-check
does not prove domain correctness or improved human comprehension.

Prefer references to existing evidence over copying full logs. A temporary
artifact should not become the only evidence for an important conclusion.
Retention and extra file creation follow the original task and environment.
