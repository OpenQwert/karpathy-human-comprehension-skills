# 评审前中文设计草稿快照

保存于 2026-10-04，供追溯评审依据。当前 v0.1 实现建议见 [收敛设计](../human-comprehension-skill-design-zh.md)。

---

# Human Comprehension Skill

## 面向 Codex、Claude Code、DeepSeek Harness 等 Agent 的人类理解与监督输出层

**版本：v0.1 Design Draft**

---

# 1. 背景

随着 Codex、Claude Code、OpenCode、DeepSeek Harness 等 Coding Agent 的自主能力不断增强，一个新的瓶颈正在出现：

> Agent 完成工作的速度越来越快，但人类理解、验证和监督 Agent 输出的速度没有同步提升。

过去 Coding Agent 的重点通常是：

- 如何规划任务；
- 如何调用工具；
- 如何修改代码；
- 如何减少错误；
- 如何验证结果；
- 如何控制上下文。

但随着 Agent 能够独立执行越来越长的任务，人类开始面对另一个问题：

> **Agent 可能已经完成了正确的工作，但人类需要花大量时间才能理解它到底做了什么。**

Andrej Karpathy 在 2026 年 10 月的一次讨论中提出了一套值得关注的表达层级：

```text
Technical Text
      ↓
Diagram
      ↓
Interactive HTML
      ↓
Explainer Video
```

其中技术文本可以进一步采用类似 ASD-STE100 的受控语言原则，使表达更加：

- 简洁；
- 明确；
- 可扫描；
- 减少歧义；
- 降低阅读负担。

这一思路本质上并不是简单的“输出美化”。

它实际上指向一个更重要的问题：

> **如何提高 Human Oversight Bandwidth。**

即：

```text
Agent autonomy ↑
        │
        ▼
Agent output volume ↑
        │
        ▼
Human review burden ↑
        │
        ▼
需要新的理解与监督层
```

因此，本项目不将其定义为一个普通的 `explain` Skill，而将其抽象为：

# Human Comprehension Layer

或者：

# Supervisory Output Layer

其职责是：

> 将 Agent 的复杂内部工作结果，编译成人类能够快速理解、检查和决策的表达形式。

---

# 2. 项目目标

构建一个个人长期使用的通用 Skill：

```text
human-comprehension
```

使其能够运行于：

- OpenAI Codex；
- Claude Code；
- OpenCode；
- Gemini CLI；
- 自建 DeepSeek Harness；
- 其他支持 Markdown Skill / System Instruction / Agent Skill 的 Agent。

该 Skill 不负责完成核心任务。

它负责：

```text
Agent Work
    ↓
Raw Result
    ↓
Human Comprehension Layer
    ↓
Human-readable Result
```

也就是：

> **工作层与表达层解耦。**

---

# 3. 核心原则

## 3.1 Reason densely, present clearly

Agent 的内部推理、代码分析、日志分析和数据处理不应该受到 STE 或简化语言的限制。

内部工作可以：

- 高密度；
- 高技术性；
- 使用完整专业术语；
- 使用复杂推理；
- 保留必要上下文。

只有在向人类交付结果时，才进行重新编译。

因此：

```text
Internal reasoning
        │
        │ unrestricted
        ▼
Raw technical result
        │
        ▼
Presentation compiler
        │
        ▼
Human-readable output
```

不应设计成：

```text
STE language
      ↓
Agent reasoning
```

而应设计成：

```text
Agent reasoning
      ↓
STE / Diagram / HTML / Video
```

---

# 4. 为什么不能简单要求 Agent 始终使用 STE

ASD-STE100 的优势主要在于：

- 降低句法复杂度；
- 限制歧义；
- 控制词汇；
- 强调动作；
- 强调明确主语；
- 提高操作说明可读性。

但如果将这种限制用于整个 Agent 推理阶段，会产生潜在副作用：

### 1. 信息密度下降

复杂技术问题往往需要：

- 条件表达；
- 假设；
- 抽象概念；
- 多层因果关系。

过早进行语言简化可能损失信息。

### 2. 影响模型推理

某些模型在自由技术语言环境下推理质量更高。

### 3. 增加 Token 使用

受控语言有时会导致：

```text
一句复杂表达
```

变成：

```text
多句简单表达
```

最终反而增加 token。

因此本 Skill 应坚持：

> STE 是 Presentation Layer，而不是 Reasoning Layer。

---

# 5. Skill 的核心任务

Human Comprehension Skill 负责五件事情：

```text
1. 判断信息复杂度

2. 判断用户理解成本

3. 选择合适表达模态

4. 将结果重新编译

5. 验证转换过程中是否发生信息失真
```

完整流程：

```text
Agent completes task
        │
        ▼
Raw Result
        │
        ▼
Complexity Analysis
        │
        ▼
Comprehension Router
        │
 ┌──────┼────────┬─────────┐
 ▼      ▼        ▼         ▼
Text  Diagram   HTML      Video
 │      │        │         │
 └──────┴────┬───┴─────────┘
             ▼
      Verification Layer
             │
             ▼
        Human Review
```

---

# 6. 不应该设计成 `/explain`

很多现有 Skill 的思路是：

```text
/explain X
```

然后生成：

```text
text
diagram
html
video
```

这种方式适合教学，但不完全适合 Coding Agent。

更好的机制应该是：

> Agent 根据“人类理解成本”自动判断是否需要升级表达模态。

例如：

```text
任务：
修改 OAuth authentication flow
```

Agent 修改了：

```text
auth.ts
callback.ts
middleware.ts
session.ts
```

传统输出可能是：

```text
Modified authentication flow.
Updated callback handling.
Added session validation.
```

Human Comprehension Skill 可以判断：

```text
涉及多个文件
+
涉及认证流程
+
存在流程关系
```

于是自动输出：

```text
简短摘要
+
Authentication Flow Diagram
+
修改文件列表
+
验证结果
```

而无需用户明确输入：

```text
画个图解释
```

---

# 7. Comprehension Router

Router 是整个 Skill 的核心。

建议采用以下规则。

---

## 7.1 Level 0 — Minimal

适用于：

- 简单事实；
- 单个命令；
- 单个配置；
- 极短回答。

例如：

```text
npm install
```

或者：

```text
配置文件位于 ~/.config/xxx
```

输出：

```text
短文本
```

---

# 8. Level 1 — Controlled Technical Text

适用于：

- 操作步骤；
- 安装教程；
- Debug 指导；
- 配置说明；
- CLI 命令；
- Agent 执行摘要。

特点：

```text
短句
明确主语
一个句子一个主要动作
避免模糊代词
明确条件
明确结果
```

例如：

错误：

```text
After doing that it should work unless something else is interfering.
```

改进：

```text
Restart the service.

Then run:

systemctl status nginx

If nginx is active, continue to the next step.

If nginx is inactive, inspect the service log.
```

---

# 9. Level 2 — Diagram

适用于：

- 架构；
- 数据流；
- 调用关系；
- 状态变化；
- 文件依赖；
- Agent 工作流；
- 模块交互。

例如：

```text
User
 │
 ▼
Frontend
 │
 ▼
API
 │
 ▼
Authentication
 │
 ▼
Database
```

触发条件可以包括：

```text
>= 3 interacting components

或

存在明显数据流

或

存在多阶段 pipeline

或

文字描述超过一定复杂度
```

---

# 10. Level 3 — Interactive HTML

适用于：

- 多维系统；
- 大型架构；
- 数据探索；
- 多模块代码库；
- Agent trace；
- Pipeline 状态；
- 多实验结果；
- 对比多个方案。

HTML 可以包含：

```text
tabs
filters
expand/collapse
hover
dependency graph
timeline
search
code links
```

例如：

```text
Repository Architecture Explorer
```

用户点击：

```text
auth
```

即可看到：

```text
files
dependencies
entry points
functions
call chain
```

---

# 11. Level 4 — Explainer Video

只用于真正适合时间序列解释的内容，例如：

- UI workflow；
- 系统演示；
- onboarding；
- 复杂操作流程；
- temporal process；
- simulation；
- 教学。

Video 不应成为默认输出。

因为它：

- 制作成本高；
- 不易搜索；
- 不易引用；
- 不易精确修改；
- 不适合代码 diff。

因此 Level 4 应属于：

```text
optional escalation
```

而不是默认路径。

---

# 12. 推荐 Router 逻辑

可以采用如下伪代码：

```python
def select_output_mode(result):

    if result.is_simple_fact:
        return "minimal"

    if result.is_sequential_instruction:
        return "controlled_text"

    if result.has_relationships:
        return "diagram"

    if result.has_many_dimensions:
        return "interactive_html"

    if result.requires_temporal_demonstration:
        return "video"

    return "controlled_text"
```

进一步可以考虑：

```text
complexity_score
```

例如：

```text
+1 多文件修改
+1 多服务
+1 多阶段 workflow
+1 数据流
+1 状态机
+1 >=5 entities
+1 >=3 alternative solutions
+1 用户需要做决策
```

当：

```text
score <= 1
```

使用 Text。

```text
score 2–3
```

Text + Diagram。

```text
score >= 4
```

考虑 Interactive HTML。

---

# 13. 一个重要原则：Progressive Disclosure

不要一次把所有信息展示给用户。

理想结构：

```text
Summary
   ↓
Key changes
   ↓
Diagram
   ↓
Details
   ↓
Evidence
   ↓
Logs
```

也就是：

```text
第一屏：
我需要知道什么？

第二层：
发生了什么？

第三层：
为什么？

第四层：
技术细节是什么？
```

这比：

```text
直接输出 3000 字技术报告
```

更适合 Agent。

---

# 14. Verification Layer

这是整个系统最重要、也是很多类似 Skill 缺失的一层。

因为任何重新表达都有可能产生：

```text
Information Distortion
```

因此：

```text
Raw Result
      ↓
Presentation
      ↓
Verification
```

必须检查：

## 14.1 Claim Preservation

新输出不得添加原结果不存在的事实。

---

## 14.2 Numerical Fidelity

数字必须一致。

例如：

```text
Raw:
Latency reduced from 310 ms to 180 ms.
```

Diagram 不得写：

```text
Latency reduced by 50%.
```

因为真实下降约为：

```text
42%
```

---

## 14.3 Code Fidelity

Diagram 中的：

```text
Function A → Function B
```

必须能够在代码中验证。

---

## 14.4 Causal Fidelity

不能把：

```text
A 与 B 同时出现
```

表达成：

```text
A causes B
```

---

## 14.5 Diagram–Text Consistency

Diagram 和文本之间不得存在冲突。

---

# 15. 推荐目录结构

建议 canonical Skill：

```text
human-comprehension/
│
├── SKILL.md
├── README.md
├── VERSION
│
├── references/
│   │
│   ├── modality-router.md
│   ├── controlled-language.md
│   ├── diagram-guidelines.md
│   ├── interactive-html.md
│   ├── video-guidelines.md
│   ├── verification.md
│   └── fidelity-rules.md
│
├── templates/
│   │
│   ├── summary.md
│   ├── diagram.html
│   ├── explorer.html
│   └── report.html
│
├── scripts/
│   │
│   ├── ste_lint.py
│   ├── html_check.py
│   ├── diagram_check.py
│   └── consistency_check.py
│
└── examples/
    │
    ├── code-change.md
    ├── architecture.md
    ├── debugging.md
    └── research-analysis.md
```

---

# 16. SKILL.md 的职责

`SKILL.md` 不应该过长。

它只负责：

```text
WHEN
WHY
WHAT
ROUTING
VERIFY
```

具体规则放入：

```text
references/
```

原因是：

> 避免每一次调用都把大量规范塞入上下文。

也就是：

```text
SKILL.md
≈ Router
```

而不是：

```text
SKILL.md
≈ entire manual
```

---

# 17. 推荐 SKILL.md 初始结构

```markdown
---
name: human-comprehension
description: >
  Converts complex agent results into forms that humans can
  understand and verify quickly. Selects concise technical text,
  diagrams, interactive HTML, or video based on comprehension cost.
---

# Human Comprehension

Use this skill after completing substantial work.

Do not constrain internal reasoning with presentation rules.

First complete the task.

Then convert the result for human supervision.

## Core principle

Reason densely.
Present clearly.

## Routing

Use minimal text for simple facts.

Use controlled technical text for procedures and operational guidance.

Use a diagram when relationships, architecture, data flow, or state
transitions are central.

Use interactive HTML when the result contains many entities,
dimensions, dependencies, or explorable information.

Use video only when temporal demonstration materially improves
understanding.

## Progressive disclosure

Present information in this order when appropriate:

1. Summary
2. Important changes
3. Visual explanation
4. Technical detail
5. Evidence
6. Logs

## Fidelity

Do not introduce facts during presentation transformation.

Preserve:

- numbers
- uncertainty
- causal relationships
- file names
- function names
- evidence
- source relationships

Verify the final presentation against the raw result.

Load detailed rules from references only when required.
```

---

# 18. Controlled Language 不必严格复制 ASD-STE100

个人使用没有必要完全实现航空工业 STE 标准。

建议构建：

# STE-inspired Technical English

即：

```text
80% STE philosophy
20% engineering flexibility
```

核心规则即可。

---

# 19. 推荐 Controlled Language 规则

## Rule 1

每句话尽量只表达一个主要动作。

---

## Rule 2

优先主动语态。

例如：

```text
The script creates the database.
```

优于：

```text
The database is created by the script.
```

---

## Rule 3

明确动作主体。

避免：

```text
It will then update it.
```

使用：

```text
The migration script updates the schema.
```

---

## Rule 4

减少模糊词。

避免：

```text
probably
maybe
somehow
normally
usually
```

除非这些不确定性本身是真实信息。

---

## Rule 5

明确条件。

例如：

```text
If port 8080 is occupied, change the application port.
```

---

## Rule 6

步骤与解释分离。

例如：

```text
Run:

npm install

This command installs the project dependencies.
```

---

# 20. Coding Agent 专用输出模式

对于 Codex / Claude Code，建议定义：

```text
Change Summary
```

固定输出模板：

```text
Goal

What changed

Why

Affected files

Architecture impact

Verification

Remaining risks
```

复杂任务增加：

```text
Diagram
```

---

# 21. Coding Change Example

例如 Agent 修改了：

```text
src/auth/login.ts
src/auth/session.ts
src/api/middleware.ts
```

建议输出：

```text
Goal

Fix session expiration handling.

What changed

The login handler now creates a session expiry timestamp.

The session middleware validates the timestamp.

Expired sessions return HTTP 401.

Architecture

Login
  │
  ▼
Create Session
  │
  ▼
Session Store
  │
  ▼
Middleware
  │
 ┌┴─────────────┐
 ▼              ▼
Valid         Expired
 │              │
 ▼              ▼
Continue       401

Verification

- Unit tests passed.
- Session expiration test passed.
- Existing authentication tests passed.

Remaining risk

The change does not modify distributed session synchronization.
```

---

# 22. 与 AGENTS.md 的关系

不建议把全部 Skill 内容写入：

```text
AGENTS.md
```

AGENTS.md 只需要告诉 Agent：

> 什么时候调用 Human Comprehension Skill。

例如：

```markdown
## Human-readable outputs

For substantial changes, optimize the final response for human
supervision.

Use the human-comprehension skill when the result contains:

- multiple modified files
- architecture changes
- multi-stage workflows
- non-trivial debugging
- system interactions
- complex research conclusions

Do not simplify internal reasoning.
Apply presentation rules only after completing the task.
```

---

# 23. Claude Code

可以：

```text
~/.claude/skills/human-comprehension/
```

或者项目级：

```text
.claude/skills/human-comprehension/
```

Claude Code 特有工具可以作为 optional enhancement。

例如存在：

```text
AskUserQuestion
```

时可以使用。

不存在时直接退化成标准文本。

核心 Skill 不应依赖 Claude 专用功能。

---

# 24. Codex

推荐放入：

```text
~/.agents/skills/human-comprehension/
```

如果 Codex 环境支持 Agent Skills 自动发现，则直接安装。

否则：

```text
AGENTS.md
```

只引用该 Skill。

例如：

```text
For complex outputs, follow:
~/.agents/skills/human-comprehension/SKILL.md
```

---

# 25. DeepSeek Harness

DeepSeek 本身不是问题。

关键是 Harness 是否支持：

```text
system prompt
dynamic context loading
skill discovery
tool routing
```

如果支持 Markdown Skill，可以直接使用：

```text
skills/human-comprehension/SKILL.md
```

如果不支持 Skill runtime，可以实现：

```python
load_skill("human-comprehension")
```

实际上只需要完成：

```text
读取 SKILL.md
        ↓
判断是否触发
        ↓
动态加载 references
        ↓
注入模型 context
```

即可。

---

# 26. Canonical Skill Strategy

不要维护：

```text
codex-version
claude-version
deepseek-version
```

三个 Skill。

应该维护：

```text
canonical skill
```

例如：

```text
~/agent-skills/human-comprehension/
```

然后：

```text
Claude
      ─┐
Codex  ─┼─→ canonical skill
OpenCode─┤
DeepSeek─┘
```

通过：

```text
symlink
```

或 installer 进行映射。

---

# 27. 推荐本地结构

例如：

```text
~/agent-skills/

├── human-comprehension/
├── coding-discipline/
├── research/
├── literature-review/
├── debugging/
└── visualization/
```

然后：

```text
~/.agents/skills/
```

软链接：

```text
human-comprehension
    →
~/agent-skills/human-comprehension
```

Claude：

```text
~/.claude/skills/
```

同样链接。

这样：

```text
修改一次
```

所有 Agent 同步生效。

---

# 28. 与 Karpathy Coding Principles 的关系

另一个常见 Skill 是基于 Karpathy 更早提出的 Coding Agent 原则：

```text
Think Before Coding
Simplicity First
Surgical Changes
Goal-Driven Execution
```

这个 Skill 应该和 Human Comprehension 分离。

建议：

```text
coding-discipline
        │
        ▼
Agent work quality
```

而：

```text
human-comprehension
        │
        ▼
Agent output quality
```

两者可以串联：

```text
User Request
      │
      ▼
Coding Discipline
      │
      ▼
Agent Execution
      │
      ▼
Verification
      │
      ▼
Human Comprehension
      │
      ▼
User
```

---

# 29. 更完整的 Agent Architecture

长期来看，可以形成：

```text
                   USER
                    │
                    ▼
             Intent Analysis
                    │
                    ▼
            Planning / Router
                    │
                    ▼
             Domain Skills
                    │
                    ▼
            Agent Execution
                    │
                    ▼
               Validator
                    │
                    ▼
        Human Comprehension Layer
                    │
                    ▼
                  USER
```

其中：

```text
Domain Skills
```

解决：

> 怎么做。

而：

```text
Human Comprehension
```

解决：

> 怎么让人理解。

---

# 30. 一个更重要的未来方向：Agent-to-Agent Communication

STE-inspired language 不仅可以服务：

```text
Agent → Human
```

还可以用于：

```text
Agent → Agent
```

特别是在多 Agent 系统中。

例如：

```text
Planner Agent
      │
      ▼
Executor Agent
      │
      ▼
Verifier Agent
```

Agent 间 instruction 可以要求：

```text
Explicit subject
Explicit action
Explicit input
Explicit output
Explicit condition
Explicit success criteria
```

例如：

错误：

```text
Check it and fix the issue.
```

改成：

```text
Inspect auth.py.

Find the cause of the failed JWT validation.

Modify only authentication code.

Run the authentication tests.

Return:

1. root cause
2. changed files
3. test result
```

这实际上可以形成：

# Agent Instruction Language

这是后续很值得单独开发的 Skill。

---

# 31. Human Comprehension 与 Agent Communication 应拆分

建议未来形成两个项目：

```text
human-comprehension
```

负责：

```text
Agent → Human
```

以及：

```text
agent-instruction
```

负责：

```text
Agent → Agent
```

二者共享：

```text
controlled-language
```

模块。

最终：

```text
controlled-language
       │
       ├── human-comprehension
       │
       └── agent-instruction
```

---

# 32. Verification 未来可以自动化

初版依赖 LLM 自检。

后续可以引入：

```text
lint
```

例如：

```text
ste_lint.py
```

检查：

```text
句子长度
被动语态
模糊词
复杂条件
代词
```

另外：

```text
consistency_check.py
```

检查：

```text
raw result
vs
final output
```

重点抽取：

```text
numbers
file names
function names
URLs
claims
```

进行一致性比较。

---

# 33. Diagram Validation

Diagram 生成后至少检查：

```text
是否有孤立节点
是否存在文本 overflow
是否有错误箭头
是否遗漏关键组件
```

如果使用 HTML/SVG，可以自动截图：

```text
desktop
tablet
mobile
```

检查布局。

---

# 34. Interactive HTML Validation

HTML 应检查：

```text
页面是否打开
JS 是否报错
按钮是否有效
链接是否有效
内容是否溢出
数据是否完整
```

原则是：

> Interactive HTML 是一种新的表达形式，而不是 decoration。

---

# 35. HTML 的真正价值

它最适合解决：

```text
人类无法同时处理所有信息
```

的问题。

通过：

```text
progressive disclosure
```

将复杂结构折叠。

例如：

```text
Repository
 │
 ├── Backend
 │    ├── Auth
 │    ├── Database
 │    └── API
 │
 └── Frontend
      ├── Components
      └── State
```

点击模块后再展开。

而不是：

```text
一次展示所有细节
```

---

# 36. Skill 的触发条件

建议自动触发条件：

```text
任务修改 ≥ 3 个文件

或

任务涉及 ≥ 3 个系统组件

或

输出正文预计 > 800 字

或

存在 architecture / pipeline / workflow

或

用户需要比较多个方案

或

需要解释复杂失败原因

或

Agent 完成一次较长自主任务
```

---

# 37. 不应该触发的情况

以下任务不应该调用：

```text
简单 shell 命令

简单 factual answer

单个代码 bug

一句配置修改

非常短的问答
```

否则会造成：

```text
presentation overhead
```

---

# 38. 一个关键指标：Comprehension Cost

以后甚至可以让 Agent 判断：

```text
comprehension_cost
```

概念上：

```text
Comprehension Cost
=
Information Volume
×
Relationship Complexity
×
Domain Difficulty
×
Decision Importance
```

如果：

```text
cost low
```

→ text。

如果：

```text
cost medium
```

→ text + diagram。

如果：

```text
cost high
```

→ interactive output。

---

# 39. 人类监督比“漂亮输出”更重要

这个 Skill 的终极目标不是：

```text
让回答看起来更漂亮
```

而是：

```text
提高人类发现 Agent 错误的能力
```

因此好的输出应该让用户快速回答：

```text
Agent 做了什么？

为什么这样做？

哪些地方发生变化？

这些变化是否合理？

证据是什么？

哪些地方仍然存在风险？
```

---

# 40. 建议加入 Risk Surface

复杂 Agent 工作完成后，最好自动输出：

```text
Remaining Risks
```

例如：

```text
Verified

✓ unit tests

✓ build

✓ lint


Not verified

○ production database

○ external API

○ browser compatibility
```

这比一句：

```text
Everything works.
```

更适合作为 Human Oversight。

---

# 41. 推荐统一结果结构

长期可以形成统一的：

```text
Agent Completion Report
```

格式：

```text
1. Outcome

2. What changed

3. Why

4. Architecture / workflow

5. Verification

6. Evidence

7. Remaining uncertainty

8. Next action
```

---

# 42. 与 Research Agent 的结合

这一 Skill 不只适用于代码。

也可以用于科研 Agent。

例如：

```text
Literature retrieval
        │
        ▼
Evidence extraction
        │
        ▼
Evidence graph
        │
        ▼
Human Comprehension Layer
```

输出：

```text
结论
+
证据等级
+
关系图
+
交互证据浏览器
```

因此它可以成为：

```text
coding
research
data analysis
system administration
```

共享的基础 Skill。

---

# 43. 推荐开发顺序

不要一开始实现四种模态。

建议：

## v0.1

实现：

```text
Router
+
Controlled Text
+
Diagram
```

这是收益最大的部分。

---

## v0.2

增加：

```text
Verification
+
Consistency Checker
```

---

## v0.3

增加：

```text
Interactive HTML
```

---

## v0.4

增加：

```text
Agent-to-Agent controlled instructions
```

---

## v0.5

最后考虑：

```text
Video
```

Video 优先级实际上最低。

---

# 44. 最小可用版本

第一版甚至只需要：

```text
human-comprehension/
├── SKILL.md
└── references/
    ├── controlled-language.md
    ├── diagram.md
    └── verification.md
```

即可开始使用。

不要一开始过度工程化。

---

# 45. 推荐 v0.1 工作逻辑

```text
Agent completes task
        │
        ▼
Is result complex?
        │
   ┌────┴────┐
   │         │
  No        Yes
   │         │
   ▼         ▼
Normal     Apply
Output     Human Comprehension
             │
             ▼
      Controlled Summary
             │
             ▼
      Need relationships?
             │
        ┌────┴────┐
        │         │
       No        Yes
        │         │
        ▼         ▼
      Final     Diagram
                   │
                   ▼
                Verify
                   │
                   ▼
                 Final
```

---

# 46. 本项目与现有项目的关系

现有相关思路可以大致分成三类。

## Explain Ladder

思路：

```text
Text
↓
Diagram
↓
HTML
↓
Video
```

值得借鉴：

```text
multimodal explanation
```

但需要进一步改造成：

```text
automatic comprehension routing
```

---

## STE Skills

值得借鉴：

```text
controlled technical language
lint
deterministic validation
```

但不应该严格照搬完整航空标准。

---

## Karpathy Coding Skills

值得借鉴：

```text
Think Before Coding
Simplicity First
Surgical Changes
Goal-Driven Execution
```

但应该作为：

```text
Execution Skill
```

而不是 Human Comprehension Skill。

---

# 47. 最终推荐 Skill 体系

未来个人 Agent 环境可以形成：

```text
skills/

├── coding-discipline/
│
├── human-comprehension/
│
├── agent-instruction/
│
├── research-evidence/
│
├── debugging/
│
├── visualization/
│
└── project-management/
```

其中：

```text
coding-discipline
```

控制：

> Agent 怎么工作。

```text
human-comprehension
```

控制：

> Agent 怎么向人解释。

```text
agent-instruction
```

控制：

> Agent 怎么向其他 Agent 下任务。

---

# 48. 核心思想总结

Human Comprehension Skill 最重要的不是：

```text
STE100
```

也不是：

```text
Diagram
```

更不是：

```text
Video
```

真正核心是：

> **把“人类理解成本”作为 Agent 系统中的一等工程问题。**

传统 Agent：

```text
Task
 ↓
Agent
 ↓
Answer
```

新的模型应该是：

```text
Task
 ↓
Agent
 ↓
Execution
 ↓
Verification
 ↓
Comprehension Compilation
 ↓
Human
```

随着 Agent autonomy 增强：

```text
Execution Cost ↓
```

但：

```text
Oversight Cost ↑
```

因此 Human Comprehension Layer 很可能逐渐成为未来 Agent Harness 的基础组成部分。

---

# 49. 推荐第一阶段实现

对于个人使用，建议第一阶段只实现以下三个能力：

```text
1. Controlled Technical Summary

2. Automatic Diagram Decision

3. Fidelity Verification
```

先验证：

> 它是否真的降低了你阅读 Codex / Claude Code 长任务输出的成本。

如果有效，再加入：

```text
Interactive HTML
```

不要一开始投入大量时间实现视频系统。

---

# 50. 项目的一句话定义

> **Human Comprehension Skill is a presentation compiler for autonomous agents. It converts complex machine work into forms that humans can understand, verify, and supervise efficiently.**

中文可以定义为：

> **Human Comprehension Skill 是自主 Agent 的“人类理解编译层”：它不负责完成任务，而负责把复杂的机器工作结果转换成人类能够快速理解、核验和监督的表达形式。**

---

# 51. 推荐项目名称

首选：

```text
human-comprehension
```

其他可选：

```text
supervisory-output

human-oversight

comprehension-layer

agent-explainer

human-interface
```

其中：

```text
human-comprehension
```

语义最中性，也最适合作为通用 Skill 名称。

---

# 52. 下一步

建议直接从：

```text
v0.1
```

开始。

只建立：

```text
SKILL.md

references/
  controlled-language.md
  diagram.md
  verification.md
```

先同时挂载到：

```text
Codex
Claude Code
```

实际运行一段时间。

重点观察：

```text
是否减少阅读时间

是否减少反复追问

是否更容易发现 Agent 错误

Diagram 是否真正有帮助

Skill 是否频繁误触发
```

之后再根据真实使用数据调整 Router。

这样比一开始试图设计一个“大而全”的 Agent communication framework 更稳妥。

---

# 53. 最终设计原则

整个 Skill 可以浓缩成六条规则：

```text
Complete the work first.

Do not simplify reasoning.

Estimate human comprehension cost.

Choose the cheapest useful representation.

Preserve the original information.

Optimize for supervision, not decoration.
```

即：

> **先完成工作，不限制推理；评估理解成本，选择最低成本但足够有效的表达方式；转换过程中保持信息忠实；最终目标是让人更容易监督 Agent，而不是让输出显得更漂亮。**
