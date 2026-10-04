# Karpathy Human Comprehension Skills

**English** | [简体中文](README.zh-CN.md)

Make complex agent work easier to review.

An agent can finish a task and still leave you with work to do: understand what changed, follow the interactions, and decide whether the evidence supports the conclusion. `human-comprehension` is a Markdown skill that helps agents explain their results in a form you can inspect.

It starts with concise prose, adds a table or diagram when that helps, and keeps evidence, uncertainty, and verification limits visible.

## When to use it

Use it when understanding the relationships matters to your review:

- **Code changes:** explain how changes work together and what was actually checked.
- **Debugging:** connect observations to conclusions without treating a plausible cause as a confirmed one.
- **Architecture:** show dependencies, branches, or state transitions you need to inspect.
- **Decisions:** compare alternatives while keeping missing evidence and estimates explicit.
- **Incomplete work:** make clear what is finished, what is blocked, and what remains unverified.

A small edit may need only a sentence. A complex interaction in one file may need a diagram. The skill chooses by what the reader needs to understand, rather than by file count or response length.

## Install

Clone the repository or download it from GitHub:

```sh
git clone https://github.com/OpenQwert/karpathy-human-comprehension-skills.git
```

Copy or link the entire `skills/human-comprehension` directory into your agent's skill directory:

| Agent | Personal installation path | Explicit invocation |
| --- | --- | --- |
| Codex | `~/.agents/skills/human-comprehension/` | `$human-comprehension` |
| Claude Code | `~/.claude/skills/human-comprehension/` | `/human-comprehension` |

Here, `~` is your home directory. Keep `SKILL.md` and `references/diagram-guidelines.md` together; the skill loads the diagram reference when needed. Copying only `SKILL.md` leaves that reference unavailable.

Both agents document support for linked skill folders. See the official [Codex skill documentation](https://learn.chatgpt.com/docs/build-skills) and [Claude Code skill documentation](https://code.claude.com/docs/en/skills) for discovery and configuration details. For other agents, use the skill location and loading behavior documented by that host.

## Try it

In Codex:

```text
$human-comprehension
Explain how these changes work together, point me to the evidence, and tell me what was verified.
```

In Claude Code:

```text
/human-comprehension Explain how these changes work together, point me to the evidence, and tell me what was verified.
```

You can also specify the format or depth you want:

```text
Compare the three options in a table. Keep estimates and unknowns explicit.
```

```text
Explain the retry and failure paths in plain text. Keep it brief and say which tests were not run.
```

The skill respects your language and presentation preferences. Invoking it does not require a diagram or a new file. Automatic invocation depends on the agent's skill support and configuration.

## How it works

The skill uses the task's existing changes, source material, tool output, calculations, and verification records. It chooses a format to support a specific review action:

| What the reader needs to do | Useful format |
| --- | --- |
| Understand an outcome, condition, or next step | Concise prose or short steps |
| Compare alternatives, attributes, or evidence | A compact table |
| Follow interactions, branches, or transitions | A short explanation with a focused diagram |
| Review findings with gaps in the evidence | Qualified text, with only supported relationships shown |

Formats can be combined. If diagram rendering is unavailable or unknown, the explanation stays readable through text, a table, or a simple text diagram.

Before delivery, the skill checks that the explanation preserves the available evidence: numbers and units, identifiers, relationship meanings, completion status, uncertainty, and the scope of checks actually performed. Text, tables, and diagrams should agree.

**Checking an explanation against the evidence does not verify the underlying task.** The skill must not turn “the change is written” into “the behavior is tested,” or “these events happened in sequence” into “one caused the other.”

## Scope and validation

The v0.1 skill consists of two files:

```text
skills/human-comprehension/
├── SKILL.md
└── references/
    └── diagram-guidelines.md
```

It provides instructions for the agent. It does not include an installer, a separate fact-checking runtime, or host-specific adapters. v0.1 does not automatically generate HTML or video.

The skill passed the format validator. In one trial with the skill explicitly loaded, an independent agent handled ten synthetic scenarios, and all ten outputs passed the subsequent review against the v0.1 design. The [validation report](docs/validation-report.md) includes the inputs, outputs, and review limits **in Chinese**.

These checks do not establish automatic discovery, end-to-end compatibility across hosts, or improved comprehension for real users. The trial did not include visual inspection of rendered Mermaid diagrams.

## Design background

- [Original design proposal — English](karpathy-human-comprehension-skills-design.md): the broader motivation and ideas considered.
- [v0.1 design — Chinese](human-comprehension-skill-design-zh.md): the narrowed implementation scope and acceptance criteria.
- [Skill instructions — English](skills/human-comprehension/SKILL.md): the behavior agents actually load.

The original proposal describes a broader vision; the skill instructions and v0.1 design define the current implementation.

This is an independent project inspired by Karpathy's discussion of understanding agent output. It is not affiliated with or endorsed by Andrej Karpathy.
