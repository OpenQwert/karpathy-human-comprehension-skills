# Karpathy Human Comprehension Skills

## A design specification for agent-native explanation, oversight, and disposable cognitive artifacts

**Working repository name:** `karpathy-human-comprehension-skills`  
**Canonical skill name:** `human-comprehension`  
**Status:** Design draft  
**Language:** English  
**Primary inspiration:** Andrej Karpathy, X post, 2 October 2026  
**Original post:** https://x.com/karpathy/status/2105819303471976479

---

## 1. Executive thesis

As language models and coding agents become more autonomous, the bottleneck moves.

The difficult part is no longer only producing code, analysis, research, or other work. The difficult part increasingly becomes **understanding what the model produced well enough to supervise it**.

Karpathy's October 2026 post is important because it treats this not primarily as a writing problem, but as a change in the economics and interface of intelligence.

The post begins from a simple observation: people will spend more time understanding model outputs. It then moves through several possible representations:

- controlled technical writing, inspired by ASD-STE100;
- diagrams and images;
- interactive web pages;
- bespoke explainer videos.

The deeper claim arrives at the end. As LLMs do more of the operational work, human work will "rise up the abstractions into oversight and understanding." At the same time, because "intelligence and code are increasingly abundant," users can request "large, custom, discardable software artifacts" that previously would have been too expensive to create for a single moment of understanding.

This document treats those statements as the foundation for an agent skill.

The skill is **not** a style preset.

It is **not** a command that rewrites every answer into simplified English.

It is **not** a rule that every complex response must become a video.

It is a **human-comprehension layer** between autonomous machine work and human judgment.

Its job is to answer a different question:

> What representation will let a human understand, inspect, challenge, and act on this result with the least unnecessary cognitive work?

A compact formulation is:

> **Reason densely. Render for understanding. Preserve fidelity. Optimize for supervision.**

---

## 2. Source grounding

### 2.1 What Karpathy explicitly proposes

Karpathy's post gives four increasingly rich ways to receive model output.

### Writing

He suggests asking an LLM to explain a topic in ASD-STE100, a controlled form of English originally designed for aerospace maintenance documentation. He notes that the full standard is stringent and suggests asking for something like "80% of the way" toward STE.

The practical intuition is straightforward:

- fewer stylistic synonyms;
- clearer verbs;
- shorter sentences;
- lower ambiguity;
- more explicit subjects and actions;
- prose that is easier to scan.

### Diagrams and images

The next move is representational rather than stylistic.

If the subject has structure, a diagram can expose that structure directly instead of encoding it indirectly in paragraphs.

A diagram is especially useful for:

- components;
- flows;
- dependencies;
- sequences;
- states;
- hierarchies;
- spatial relations.

### Interactive web pages

Karpathy then points to HTML as a native output format for LLMs.

A web page can do things static prose cannot:

- reveal information progressively;
- let the reader expand or collapse detail;
- visualize state;
- expose parameters;
- connect explanation to interaction;
- combine prose, diagrams, controls, and animation in one artifact.

The important idea is not "HTML looks nicer."

The important idea is that **the explanation can itself become software**.

### Bespoke explainer videos

Karpathy says he is especially bullish on fully custom explainer videos for arbitrary topics.

This matters because video combines:

- pacing;
- narration;
- animation;
- visual focus;
- temporal sequencing.

The artifact can be made for one person, one question, one moment, and then discarded.

### The concluding shift

The most important part of the post is not any individual format.

It is the economic and cognitive shift behind them.

Karpathy argues that:

1. LLMs will perform more of the legwork autonomously.
2. Human work will move upward toward oversight and understanding.
3. Intelligence and code are becoming abundant.
4. Therefore, software artifacts that were previously too expensive to justify can now be generated purely to help someone understand something.
5. Those artifacts do not necessarily need to become products, libraries, or maintained applications.

That last point changes the conceptual unit of software.

Software can become **ephemeral cognition infrastructure**.

---

## 3. Why the post is more than an output-format checklist

A shallow reading of the post produces this:

```text
Text
  ↓
Diagram
  ↓
HTML
  ↓
Video
```

That reading is useful, but incomplete.

The deeper reading is:

```text
Machine work becomes cheaper
        ↓
Machine output becomes larger and more autonomous
        ↓
Human attention becomes relatively scarcer
        ↓
Understanding becomes a systems bottleneck
        ↓
Representation becomes an engineering problem
```

The key resource is no longer only compute, tokens, code, or model intelligence.

It is also:

> **human comprehension bandwidth**

This is the foundation of the proposed skill.

---

## 4. The philosophical core

## 4.1 Human work rises "up the abstractions"

The phrase matters.

It does not mean humans simply stop working while agents work for them.

It means the *location of human effort* changes.

In a manual workflow, the human often performs low-level execution:

```text
inspect file
edit function
run command
read log
change configuration
repeat
```

In an agentic workflow, more of this can be delegated:

```text
goal
  ↓
agent execution
  ↓
tests
  ↓
result
```

The human therefore spends proportionally more time on:

- deciding what matters;
- determining whether the result is correct enough;
- inspecting assumptions;
- evaluating trade-offs;
- detecting omissions;
- deciding what happens next.

The human moves from being primarily an operator to being increasingly a **supervisor of abstractions**.

That creates a new interface requirement.

A raw transcript of everything the agent did is often too low-level.

A vague summary is often too lossy.

The agent needs to produce an intermediate representation that preserves the important structure while compressing irrelevant operational detail.

That is a comprehension problem.

---

## 4.2 Output is an interface, not merely an answer

Traditional chat assumes that the natural terminal state of reasoning is prose.

That assumption was reasonable when language models mostly produced text.

It becomes weaker when models can also generate:

- code;
- SVG;
- HTML;
- interactive simulations;
- dashboards;
- narrated animations;
- temporary applications.

The answer no longer has to be a paragraph.

The answer can be a **purpose-built interface to the underlying result**.

This leads to an important design principle:

> Do not ask only, "What should the model say?"  
> Ask, "What interface should exist between this result and the human who must understand it?"

---

## 4.3 Representation should match the topology of the idea

Different kinds of information have different shapes.

A procedure is sequential.

An architecture is relational.

A state machine is transitional.

A comparison is multidimensional.

A dataset is explorable.

A mathematical derivation is symbolic.

A UI workflow is temporal and visual.

A failure investigation is causal and evidential.

The default error of text-first systems is to flatten all of these into paragraphs.

The proposed skill should instead treat representation as a routing problem.

```text
procedure            → concise technical prose
relationship graph   → diagram
state transition     → state diagram
large comparison     → table or interactive HTML
repository structure → navigable explorer
temporal process     → animation or video
evidence review      → evidence map + source-linked text
```

This is not multimodality for decoration.

It is multimodality as **structure preservation**.

---

## 4.4 Abundant code changes what software is for

Traditional software economics assume substantial creation cost.

Because software is expensive to design, implement, test, deploy, and maintain, people normally build software when the expected repeated value justifies the cost.

Agentic code generation weakens that assumption.

If generating a small custom application becomes cheap enough, software can be created for a single use.

Examples:

- a local HTML page explaining one pull request;
- an interactive diagram for one unfamiliar repository;
- a temporary simulator for one scientific mechanism;
- a visual comparison of three architecture options;
- a narrated animation explaining one bug;
- a one-off interface over one dataset;
- a disposable dashboard for one decision.

The artifact may be valuable even if it is deleted ten minutes later.

Its purpose is not persistence.

Its purpose is **cognitive transfer**.

This suggests a new category:

> **Disposable cognitive software**

or:

> **Ephemeral comprehension artifacts**

The artifact is software, but its product is understanding.

---

## 4.5 "Discardable" does not mean careless

This distinction is important.

A disposable artifact has a short maintenance horizon.

It does **not** follow that:

- correctness is optional;
- security is irrelevant;
- fabricated data is acceptable;
- citations do not matter;
- the artifact can misrepresent the source.

In fact, rich artifacts can increase epistemic risk.

A polished diagram, interactive page, or narrated video may look more authoritative than plain text.

Therefore:

> The richer the presentation layer, the stronger the fidelity checks should become.

This is one of the most important extensions beyond the original post.

---

## 5. What this project derives beyond Karpathy's post

The following ideas are **design derivations**, not claims that Karpathy explicitly specified them.

### 5.1 Comprehension should be a first-class agent subsystem

The post suggests output forms.

This project turns the idea into a persistent layer:

```text
Task
  ↓
Agent execution
  ↓
Verification
  ↓
Comprehension rendering
  ↓
Human oversight
```

### 5.2 The agent should route representations automatically

The user should not always need to say:

```text
make a diagram
```

or:

```text
turn this into HTML
```

A mature agent should infer when the structure of the result warrants a different representation.

### 5.3 Presentation should happen at the human boundary

A style constraint can affect reasoning if it is applied too early.

Therefore, the skill should usually let the agent perform technical work in its natural high-density form first.

Then it should render the result for the human.

```text
dense machine work
      ↓
verified internal result
      ↓
human-facing render
```

This is preferable to:

```text
simplified language constraint
      ↓
all reasoning and work
```

### 5.4 Representation must be fidelity-preserving

Rendering is a transformation.

Transformations can distort.

The skill therefore needs explicit checks for:

- numerical consistency;
- causal consistency;
- code references;
- uncertainty;
- source attribution;
- omitted caveats;
- diagram/text agreement.

### 5.5 The cheapest sufficient representation should win

Karpathy's post uses repeated "but even better" transitions.

For an implemented skill, however, richer should not mean universally better.

Video is not inherently better than text for:

- a shell command;
- a three-line diff;
- an error message;
- a configuration value.

The practical rule should be:

> **Use the least expensive representation that preserves the structure needed for understanding.**

This prevents the skill from becoming an artifact-generation gimmick.

---

## 6. Project identity

A useful repository name should acknowledge the inspiration while clearly distinguishing this work from existing Karpathy-inspired coding-guideline repositories.

Recommended repository name:

```text
karpathy-human-comprehension-skills
```

Recommended canonical skill name:

```text
human-comprehension
```

This separation is intentional.

The repository name communicates intellectual lineage.

The skill name communicates portable function.

Suggested structure:

```text
karpathy-human-comprehension-skills/
├── README.md
├── LICENSE
├── CHANGELOG.md
├── sources/
│   └── karpathy-2026-10-02.md
├── skills/
│   └── human-comprehension/
│       ├── SKILL.md
│       ├── references/
│       ├── templates/
│       ├── scripts/
│       └── examples/
└── adapters/
    ├── codex/
    ├── claude-code/
    ├── opencode/
    └── deepseek-harness/
```

---

## 7. Relationship to `andrej-karpathy-skills`

There are several repositories using the name `andrej-karpathy-skills` or closely related variants.

A prominent family of these projects packages Karpathy-inspired coding-agent principles such as:

- think before coding;
- keep solutions simple;
- make surgical changes;
- surface assumptions;
- define success criteria;
- verify before declaring completion.

Those projects are about **agent execution behavior**.

This project is about **human comprehension after or during agent execution**.

The distinction is structural:

```text
andrej-karpathy-skills
        │
        │ shapes
        ▼
How the agent works
        │
        ▼
Execution quality
```

versus:

```text
karpathy-human-comprehension-skills
        │
        │ shapes
        ▼
How the agent exposes its work
        │
        ▼
Oversight quality
```

They can be complementary.

A combined lifecycle could be:

```text
User intent
   ↓
Execution discipline
   ↓
Agent work
   ↓
Verification
   ↓
Human comprehension rendering
   ↓
Human judgment
```

They should not be presented as the same project.

They also should not imply official affiliation or endorsement by Andrej Karpathy.

A README should state clearly that the project is inspired by public comments and is independently implemented.

---

## 8. Core design objective

The skill should minimize:

```text
time-to-correct-understanding
```

rather than merely:

```text
response length
```

or:

```text
visual attractiveness
```

A useful output lets the human answer quickly:

1. What happened?
2. Why did it happen?
3. What changed?
4. What evidence supports the result?
5. What remains uncertain?
6. What should I inspect?
7. What decision do I need to make?

This means the skill should optimize for **supervision**, not presentation aesthetics.

---

## 9. Non-goals

The skill should not:

- force STE-style prose into all reasoning;
- rewrite every response into a rigid template;
- produce a diagram when prose is clearer;
- generate HTML merely because HTML is available;
- produce video by default;
- hide uncertainty to make explanations cleaner;
- replace source evidence with a polished visualization;
- confuse a beautiful artifact with a verified result;
- turn every trivial task into a multi-file deliverable;
- introduce a new framework when a short answer is sufficient.

---

## 10. The human-comprehension pipeline

Recommended pipeline:

```text
┌────────────────────┐
│   Agent performs   │
│      the work      │
└─────────┬──────────┘
          │
          ▼
┌────────────────────┐
│ Verify raw result  │
└─────────┬──────────┘
          │
          ▼
┌────────────────────┐
│ Estimate human     │
│ comprehension cost │
└─────────┬──────────┘
          │
          ▼
┌────────────────────┐
│ Select             │
│ representation     │
└─────────┬──────────┘
          │
          ▼
┌────────────────────┐
│ Render explanation │
└─────────┬──────────┘
          │
          ▼
┌────────────────────┐
│ Fidelity check     │
└─────────┬──────────┘
          │
          ▼
┌────────────────────┐
│ Human review       │
└────────────────────┘
```

The rendering stage must not silently become another reasoning stage that invents new facts.

---

## 11. Comprehension cost

The skill needs a practical way to decide whether plain text is enough.

A conceptual model is:

```text
Comprehension Cost
≈
Information Volume
× Relationship Complexity
× Domain Difficulty
× Decision Consequence
× Uncertainty
```

This does not need to become a literal mathematical score in v0.1.

It can start as a heuristic.

Signals that comprehension cost is rising include:

- many interacting components;
- many modified files;
- several causal steps;
- several competing explanations;
- a long temporal process;
- a large evidence base;
- high uncertainty;
- important irreversible decisions;
- information that must be explored from multiple viewpoints.

---

## 12. Representation router

The central behavior should be routing, not forced escalation.

### Minimal text

Use for:

- direct facts;
- one command;
- one configuration value;
- a short correction;
- a tiny code change.

### Controlled technical text

Use for:

- procedures;
- debugging steps;
- operational instructions;
- concise change summaries;
- warnings;
- handoffs.

### Diagram

Use when relationships are the content:

- architecture;
- dependency structure;
- data flow;
- execution sequence;
- state transition;
- causal chain;
- evidence relationships.

### Interactive HTML

Use when the reader benefits from exploration:

- complex repositories;
- many related findings;
- parameter comparisons;
- experiment results;
- multi-dimensional decisions;
- long technical explanations;
- timelines with drill-down;
- large PR review surfaces.

### Explainer video

Use when pacing and temporal guidance materially improve understanding:

- onboarding;
- unfamiliar concepts;
- dynamic systems;
- UI workflows;
- animations;
- mathematical intuition;
- simulations.

### Hybrid artifact

Often the best representation is not a single modality.

For example:

```text
one-paragraph conclusion
        +
architecture diagram
        +
expandable evidence
        +
manual-test checklist
```

The router should be allowed to compose formats.

---

## 13. A better mental model than "levels"

Avoid treating the representations as a prestige hierarchy.

Use this model instead:

```text
                 ┌───────────────┐
                 │   Structure   │
                 └───────┬───────┘
                         │
        ┌────────────────┼─────────────────┐
        ▼                ▼                 ▼
   Sequential        Relational        Explorable
        │                │                 │
        ▼                ▼                 ▼
      Text            Diagram            HTML
                                            │
                                            ▼
                                     Temporal teaching
                                            │
                                            ▼
                                          Video
```

The question is not:

> What is the richest format we can generate?

The question is:

> What representation exposes the important structure with the least friction?

---

## 14. Controlled language: use the philosophy, not fake compliance

ASD-STE100 is a real standard maintained by the ASD Simplified Technical English Maintenance Group.

Its original motivation is highly relevant: technical English can be ambiguous, especially for readers with different language backgrounds, and ambiguity in maintenance instructions can create safety risks.

The official standard includes controlled vocabulary and formal writing rules.

For this project, the default should **not** claim strict ASD-STE100 compliance unless a real checker and the authoritative standard are being used.

Instead, define a mode such as:

```text
STE-inspired technical English
```

or:

```text
80% STE-style technical prose
```

Recommended principles:

- prefer one term for one concept;
- prefer simple verbs;
- name the actor;
- prefer active voice for instructions;
- keep one main action per instruction;
- make conditions explicit;
- avoid decorative synonym variation;
- avoid pronouns with unclear referents;
- separate action from explanation;
- preserve domain-specific technical nouns;
- preserve exact code identifiers, paths, commands, and API names;
- do not remove uncertainty when uncertainty is real.

Example:

Weak:

```text
After doing that, it should probably work unless something else is interfering.
```

Better:

```text
Restart the service.

Run:

systemctl status nginx

If nginx is active, continue.

If nginx is inactive, inspect the service log.
```

The gain is not elegance.

The gain is reduced ambiguity.

---

## 15. Dense reasoning, clear rendering

This project should preserve a strict boundary:

```text
internal work ≠ human-facing render
```

The agent may need:

- long code traces;
- dense technical notes;
- competing hypotheses;
- large search results;
- intermediate calculations;
- ugly debugging logs.

Do not force those stages into simplified prose.

Instead:

```text
work naturally
      ↓
verify
      ↓
compress structure
      ↓
render for the reader
```

This boundary is especially important for research and coding.

Compression before understanding can destroy information.

Compression after verification can reduce cognitive load.

---

## 16. Progressive disclosure

A good comprehension artifact should not expose all detail at once.

A useful order is:

```text
Outcome
  ↓
Key changes / key claims
  ↓
Structure
  ↓
Evidence
  ↓
Technical detail
  ↓
Raw logs or trace
```

The user should be able to stop as soon as they know enough.

Interactive HTML is especially valuable here because the same artifact can support several depths of inspection.

Example:

```text
Summary
├── Architecture
│   ├── Auth
│   ├── Session
│   └── API
├── Changes
│   ├── File A
│   ├── File B
│   └── File C
├── Verification
└── Raw evidence
```

This is better than a single wall of text when the result is structurally complex.

---

## 17. Fidelity layer

The skill should treat every presentation transformation as potentially lossy.

### 17.1 Claim fidelity

Do not add factual claims merely to make a narrative smoother.

### 17.2 Numerical fidelity

Preserve exact numbers.

If raw data says:

```text
310 ms → 180 ms
```

do not casually render it as:

```text
about 50% faster
```

unless that transformation is calculated and semantically appropriate.

### 17.3 Uncertainty fidelity

Preserve distinctions such as:

- confirmed;
- inferred;
- plausible;
- unverified;
- unknown.

Do not convert:

```text
likely caused by
```

into:

```text
caused by
```

### 17.4 Causal fidelity

Do not transform correlation into causation.

### 17.5 Code fidelity

If a diagram says:

```text
middleware.ts → validateSession()
```

that relationship should be supported by the repository.

### 17.6 Source fidelity

A rendered artifact should preserve the ability to trace important claims back to their evidence.

### 17.7 Cross-format consistency

If the text, diagram, and HTML disagree, the artifact has failed even if each component looks plausible by itself.

---

## 18. Persuasion-risk principle

Presentation quality changes trust.

A polished video can make a weak claim feel stronger.

An attractive diagram can make a speculative architecture look factual.

An interactive page can imply precision that the underlying data does not have.

Therefore:

```text
presentation power ↑
        ⇒
verification requirement ↑
```

This should become an explicit rule in the skill.

For high-persuasion formats:

- show uncertainty;
- preserve sources;
- expose assumptions;
- allow inspection of the underlying text or data;
- label generated interpretations;
- separate observation from inference.

---

## 19. Disposable artifacts and durable evidence

A useful distinction is:

```text
Artifact: disposable
Evidence: durable
```

The HTML page can be deleted.

The video can be deleted.

The temporary SVG can be deleted.

But the underlying source references, test outputs, data, or code state should remain recoverable when they matter.

This prevents "ephemeral software" from becoming "ephemeral truth."

---

## 20. Coding-agent completion report

For substantial coding work, the skill can use a standard semantic structure without forcing a rigid visual format.

Recommended fields:

```text
Outcome

What changed

Why it changed

Affected surface

Architecture / flow

Verification

Not verified

Remaining risks

Next decision
```

Example:

```text
Outcome

Session expiry handling now rejects expired sessions.

What changed

- login.ts writes an expiry timestamp
- session.ts validates the timestamp
- middleware.ts returns 401 for expired sessions

Flow

Login
  ↓
Create session
  ↓
Store expiry
  ↓
Middleware
  ↓
Check expiry
  ├─ valid   → continue
  └─ expired → 401

Verification

- unit tests passed
- authentication tests passed
- build passed

Not verified

- production Redis behavior
- multi-region session replication

Remaining risk

The change does not alter distributed session synchronization.
```

The structure makes the result supervisable.

---

## 21. Research-agent mode

The same skill can support research.

Instead of only generating a prose literature summary, it can produce:

```text
claim
  ↓
supporting evidence
  ↓
contradictory evidence
  ↓
evidence quality
  ↓
uncertainty
```

Possible artifact:

```text
Executive finding
   +
evidence graph
   +
study table
   +
interactive source explorer
   +
unresolved questions
```

The purpose is not to make a review visually impressive.

The purpose is to let the researcher see:

- which claim rests on which evidence;
- where evidence is weak;
- where studies conflict;
- what is direct versus extrapolated;
- what would change the conclusion.

---

## 22. Agent-to-agent implications

Karpathy's post is about understanding model outputs, primarily from the human side.

But the same controlled-language idea has a second use:

```text
Agent → Agent
```

A multi-agent system benefits when instructions explicitly specify:

- actor;
- action;
- input;
- output;
- constraints;
- success condition;
- evidence required.

Weak handoff:

```text
Check the auth issue and fix it.
```

Better handoff:

```text
Inspect auth.py.

Find the cause of JWT validation failure.

Modify only authentication-related code.

Run the authentication tests.

Return:
1. root cause
2. changed files
3. test result
4. remaining uncertainty
```

This may deserve a separate future skill:

```text
agent-instruction
```

The present project should keep it conceptually adjacent but operationally separate.

---

## 23. Skill architecture

Recommended v0.1 structure:

```text
skills/human-comprehension/
├── SKILL.md
├── references/
│   ├── representation-router.md
│   ├── controlled-language.md
│   ├── diagram-guidelines.md
│   ├── fidelity.md
│   └── completion-report.md
└── examples/
    ├── coding-change.md
    ├── debugging.md
    ├── architecture.md
    └── research-review.md
```

Later:

```text
skills/human-comprehension/
├── SKILL.md
├── references/
│   ├── representation-router.md
│   ├── controlled-language.md
│   ├── diagram-guidelines.md
│   ├── html-artifacts.md
│   ├── video-artifacts.md
│   ├── fidelity.md
│   └── completion-report.md
├── templates/
│   ├── review.html
│   ├── repository-explorer.html
│   └── evidence-browser.html
├── scripts/
│   ├── prose_lint.py
│   ├── consistency_check.py
│   ├── html_check.py
│   └── render_check.py
└── examples/
```

---

## 24. Proposed `SKILL.md`

A first implementation can stay compact.

```markdown
---
name: human-comprehension
description: >
  Render substantial agent work into the representation that lets a human
  understand and supervise it efficiently. Use concise technical prose,
  diagrams, interactive artifacts, or video when their structure materially
  improves comprehension. Preserve evidence, uncertainty, and factual fidelity.
---

# Human Comprehension

Use this skill at the boundary between completed agent work and human review.

## Core rule

Reason densely.
Render clearly.
Preserve fidelity.
Optimize for supervision.

## Do the work first

Do not constrain technical reasoning with presentation rules.

Complete and verify the underlying work before compressing or reformatting it,
unless the representation is itself required to perform the task.

## Select the cheapest sufficient representation

Use concise text for direct facts and small changes.

Use controlled technical prose for procedures, debugging instructions, and
handoffs.

Use a diagram when relationships, architecture, flows, states, or sequences
are central.

Use interactive HTML when the result benefits from exploration, drill-down,
comparison, filtering, or progressive disclosure.

Use video only when pacing, animation, narration, or temporal demonstration
materially improves understanding.

Do not generate a richer artifact merely because you can.

## Progressive disclosure

Prefer this order when useful:

1. outcome
2. key changes or claims
3. structure
4. verification or evidence
5. technical detail
6. raw logs or source trace

## Fidelity

Preserve:

- numbers
- code identifiers
- file names
- causal relationships
- uncertainty
- source attribution
- test status
- unsupported areas

Do not turn inference into fact.

Do not let diagrams, HTML, or video imply more certainty than the source.

## Verification

For richer artifacts, increase verification effort.

Check important claims against the underlying work.

Check cross-format consistency.

Make important evidence inspectable.
```

---

## 25. Activation policy

The skill should trigger for substantial results, not every answer.

Good triggers:

- several modified files;
- architecture changes;
- multi-stage debugging;
- complex failure analysis;
- repository exploration;
- multi-source research synthesis;
- several interacting systems;
- high-consequence decisions;
- long autonomous agent runs;
- results whose structure is hard to preserve in prose.

Weak triggers:

- one command;
- one value;
- one short factual answer;
- one obvious edit;
- trivial copy changes.

The goal is to reduce cognitive overhead, not create a new source of it.

---

## 26. Cross-agent deployment

Maintain one canonical skill.

Example:

```text
~/agent-skills/
└── human-comprehension/
```

Then expose it to different agents through adapters or symlinks.

### Codex

Possible target:

```text
~/.agents/skills/human-comprehension/
```

Repository-level `AGENTS.md` should contain only the activation policy, not the entire skill.

Example:

```markdown
## Human-facing completion output

For substantial work, use the human-comprehension skill.

Apply presentation rules after completing and verifying the technical work.

Prefer the cheapest representation that preserves the structure a reviewer
needs to understand.
```

### Claude Code

Possible target:

```text
~/.claude/skills/human-comprehension/
```

or project-local skill storage.

Claude-specific artifact features may be used as optional enhancements, but the canonical skill should remain portable.

### OpenCode and similar agents

Use the canonical Markdown skill where supported.

Avoid creating semantically different versions for each tool.

### DeepSeek-based harnesses

DeepSeek is a model family, not a universal skill runtime.

The harness should provide:

```text
skill discovery
      ↓
trigger evaluation
      ↓
load SKILL.md
      ↓
load references on demand
      ↓
render artifact
      ↓
verify
```

A minimal harness function can conceptually be:

```python
load_skill("human-comprehension")
```

The important part is dynamic instruction loading, not any specific vendor API.

---

## 27. Canonical source strategy

Do not maintain:

```text
codex-human-comprehension
claude-human-comprehension
deepseek-human-comprehension
```

as separate conceptual projects.

Maintain:

```text
human-comprehension
```

as the source of truth.

Use adapters only for:

- directory conventions;
- manifests;
- invocation syntax;
- tool-specific capabilities.

This prevents behavioral drift.

---

## 28. Validation tooling

### Controlled-prose linter

Possible checks:

- very long sentences;
- ambiguous pronouns;
- unnecessary synonym variation;
- hidden actors;
- vague modal language;
- mixed instructions and explanations;
- inconsistent terminology.

The linter should not claim formal ASD-STE100 compliance unless it actually implements the standard correctly.

### Consistency checker

Compare raw result and rendered output for:

- numbers;
- file names;
- function names;
- URLs;
- test results;
- status words;
- confidence labels.

### Diagram checker

Check:

- missing nodes;
- orphan nodes;
- impossible arrows;
- inconsistent labels;
- overflow;
- hidden uncertainty;
- mismatch with source structure.

### HTML checker

Check:

- page loads;
- no fatal console errors;
- controls function;
- content remains readable at common viewport sizes;
- no missing evidence;
- no accidental external dependency when self-contained output was requested.

### Video checker

At minimum:

- preserve a text script;
- verify key factual claims;
- inspect representative frames;
- confirm labels and equations;
- verify narration against the script.

---

## 29. Development roadmap

### v0.1 — Text + diagram + fidelity

Implement:

```text
representation router
controlled technical prose
diagram guidance
fidelity checklist
completion report
```

This is enough to test the central hypothesis.

### v0.2 — Automated verification

Add:

```text
prose_lint
consistency_check
diagram validation
```

### v0.3 — Interactive HTML

Add reusable patterns for:

- PR review;
- architecture explorer;
- evidence browser;
- comparison dashboard;
- incident explainer.

### v0.4 — Agent-to-agent instruction skill

Split shared controlled-language logic into reusable primitives.

### v0.5 — Video

Add video only after the lower-cost representations prove useful.

Video is the most persuasive and most expensive mode. It should arrive late.

---

## 30. Evaluation framework

The project should be evaluated by whether it improves human oversight.

Useful metrics:

### Time to correct understanding

How long before the user can accurately explain what the agent did?

### Review accuracy

Can the user identify errors, omissions, and unsupported assumptions?

### Follow-up burden

How many clarification questions are needed after the agent reports completion?

### Artifact overhead

Did the representation save more time than it cost to generate and inspect?

### Fidelity failures

How often does the rendered artifact distort the underlying result?

### Trigger precision

How often does the skill activate when it should not?

### Representation fit

Did the chosen medium actually match the information structure?

A beautiful artifact with poor review performance is a failure.

---

## 31. Anti-patterns

### Artifact maximalism

Bad:

```text
Every substantial response becomes HTML.
```

Better:

```text
HTML only when exploration or progressive disclosure is useful.
```

### Video prestige

Bad:

```text
Video is the highest level, so it is the best output.
```

Better:

```text
Video is one representation for temporal teaching.
```

### Fake STE

Bad:

```text
Claim strict ASD-STE100 compliance because sentences are short.
```

Better:

```text
Call it STE-inspired unless the actual standard is enforced.
```

### Premature compression

Bad:

```text
Force all reasoning into simplified language.
```

Better:

```text
Do dense work first, then render.
```

### Beautiful hallucination

Bad:

```text
Generate a polished diagram from uncertain relationships without labels.
```

Better:

```text
Mark uncertainty and preserve traceability.
```

### Throwaway evidence

Bad:

```text
Delete the only record of why the conclusion was reached.
```

Better:

```text
The presentation artifact may be disposable; the evidence should remain inspectable.
```

---

## 32. The deeper architecture

The long-term architecture is not:

```text
User
  ↓
LLM
  ↓
Answer
```

It is closer to:

```text
User intent
   ↓
Planning
   ↓
Domain skills
   ↓
Autonomous execution
   ↓
Technical verification
   ↓
Human-comprehension compiler
   ↓
Review interface
   ↓
Human judgment
```

This suggests that future agent harnesses may need a dedicated **presentation compiler** in the same way software systems have:

- compilers;
- renderers;
- serializers;
- observability layers;
- debuggers.

The comprehension layer is not identical to any of these, but it plays a similar translational role.

It converts one representation of work into another representation optimized for a different consumer.

The consumer is the human supervisor.

---

## 33. A useful analogy: compilation

The agent's raw work product can be treated as an intermediate representation.

```text
Agent IR
```

The human does not necessarily need to read the entire IR.

The system can compile it into several targets:

```text
Agent IR
   ├── concise technical text
   ├── architecture diagram
   ├── interactive HTML
   └── narrated explainer
```

The correct target depends on the task.

This analogy helps impose discipline.

A compiler should preserve semantics.

A comprehension renderer should also preserve semantics.

If the render changes the meaning, it is a compiler bug.

---

## 34. A second useful analogy: observability

Traditional observability helps humans understand software systems through:

- logs;
- metrics;
- traces;
- dashboards.

Autonomous agents create a similar problem.

The underlying execution may be too complex to inspect directly.

Human-comprehension artifacts can become a form of **agent observability**.

Examples:

- architecture maps;
- decision traces;
- evidence graphs;
- verification summaries;
- risk surfaces;
- interactive execution explorers.

This project therefore sits at the intersection of:

```text
explanation
observability
interface design
agent supervision
```

---

## 35. Risk surface as a default output concept

One of the most useful additions to agent reporting is an explicit split between what was verified and what was not.

Example:

```text
Verified

✓ unit tests
✓ build
✓ lint
✓ local authentication flow

Not verified

○ production database
○ external identity provider
○ multi-region replication
○ browser-specific behavior
```

This improves trust calibration.

It replaces:

```text
Everything works.
```

with:

```text
Here is the boundary of what the agent actually knows.
```

That is a direct contribution to oversight.

---

## 36. The project in one sentence

> **Karpathy Human Comprehension Skills turns agent output into purpose-built, fidelity-preserving interfaces for human understanding and supervision.**

A more technical version:

> **Human Comprehension is a presentation compiler for autonomous agents: it selects and renders the cheapest representation that preserves the structure, evidence, and uncertainty a human needs to review the work.**

---

## 37. Design principles

The project can be reduced to ten principles.

1. **Do the work before simplifying the report.**
2. **Treat human attention as a scarce systems resource.**
3. **Choose representation based on information structure.**
4. **Use the cheapest sufficient medium.**
5. **Keep rich artifacts disposable when persistence adds no value.**
6. **Keep evidence traceable even when the artifact is disposable.**
7. **Increase verification as presentation becomes more persuasive.**
8. **Preserve uncertainty and causal boundaries.**
9. **Optimize for review and judgment, not beauty.**
10. **Let the human rise in abstraction without losing the ability to inspect reality.**

---

## 38. Why this deserves to be a skill

A normal prompt solves a local formatting problem.

A skill can encode a recurring operational principle.

This principle will recur whenever an agent:

- modifies a codebase;
- completes a research synthesis;
- investigates an incident;
- explores a dataset;
- compares technical options;
- performs a long autonomous task;
- hands work back to a human.

The repeated question is:

> How should this work be rendered so the human can supervise it?

That is stable enough to deserve a reusable skill.

---

## 39. Recommended first implementation

Do not build the entire vision at once.

Start with:

```text
human-comprehension/
├── SKILL.md
└── references/
    ├── representation-router.md
    ├── controlled-language.md
    ├── diagram-guidelines.md
    └── fidelity.md
```

Test it on real Codex and Claude Code sessions.

Collect examples where:

- the diagram helped;
- the diagram did not help;
- the prose became too compressed;
- the skill activated unnecessarily;
- the final answer hid important evidence;
- HTML would genuinely have saved time.

Let usage shape the router.

The project should itself follow a minimalism principle:

> Do not build a comprehension framework so large that humans need another comprehension framework to understand it.

---

## 40. Source notes

### Primary inspiration

Andrej Karpathy, X, 2 October 2026:

https://x.com/karpathy/status/2105819303471976479

The original X page was not directly fetchable from the research environment used to prepare this document. The wording and structure of the post were cross-checked against multiple independently indexed mirrors and contemporaneous summaries.

### Indexed copy / mirror of the post

TwStalker indexed copy:

https://x.twstalker.com/karpathy/status/2105819303471976479

Thread Navigator indexed discussion containing the post:

https://threadnavigator.com/thread/2106093565554123083/

### Contemporary English analysis

ExplainX, "Karpathy on Understanding LLM Output: 4 Formats Ranked":

https://www.explainx.ai/blog/karpathy-understand-llm-outputs-ste100-diagrams-html-video-2026

ExplainX, "Karpathy on Discardable Software":

https://www.explainx.ai/blog/karpathy-discardable-software-artifacts-code-abundant-2026

Nic's Notes, "Making Claude Speak Clearly to Me":

https://notes.nicolasdeville.com/nicai/clear-replies/

### ASD-STE100

Official ASD-STE100 site:

https://www.asd-ste100.org/

About Simplified Technical English:

https://asd-ste100.org/about_STE.html

Official Issue 9 PDF:

https://www.asd-ste100.org/assets/files/ASD-STE100_ISSUE9.pdf

The official material is the authoritative source for actual ASD-STE100 rules. This project should avoid claiming strict compliance unless it implements and verifies those rules.

### Related Karpathy-inspired agent skill repositories

`multica-ai/andrej-karpathy-skills`:

https://github.com/multica-ai/andrej-karpathy-skills

`LearnPrompt/andrej-karpathy-skills`:

https://github.com/LearnPrompt/andrej-karpathy-skills

These projects package Karpathy-inspired coding and agent-engineering principles. They are conceptually adjacent but solve a different problem from the human-comprehension layer proposed here.

---

## 41. Final perspective

The most important implication of Karpathy's post is not that future AI answers should contain more diagrams.

It is that **the cost structure of explanation is changing**.

When code was expensive, building a custom interface to explain one result was absurd.

When code becomes cheap, the interface can be generated on demand.

When agents were weak, humans spent their time performing the work.

When agents become stronger, humans spend more time deciding whether the work is correct, relevant, safe, and useful.

Those two shifts meet at the same point:

```text
abundant machine capability
        +
scarce human comprehension
        ↓
purpose-built cognitive artifacts
```

That is the opportunity for this project.

The long-term goal is not to make agents more verbose, more visual, or more theatrical.

It is to let autonomous systems do more work **without forcing humans to surrender understanding of that work**.

The skill should therefore preserve a simple contract:

> The agent may rise in autonomy.  
> The human should rise in abstraction.  
> Neither should require the human to give up inspectability.

That is the core of **Karpathy Human Comprehension Skills**.
