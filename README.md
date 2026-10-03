# Karpathy Human Comprehension Skills

让智能体的复杂工作结果更容易被人理解、核查和监督。

`human-comprehension` 是一个可移植的 Markdown 技能。它根据审阅需求选择简洁文本、比较表格或关系图示，并保留证据、不确定性与实际验证范围。

本项目独立开发，受 Karpathy 关于理解智能体输出的讨论启发，与 Andrej Karpathy 没有官方关联或背书。完整思想背景见[英文设计文档](karpathy-human-comprehension-skills-design.md)，首版范围见[收敛设计](human-comprehension-skill-design-zh.md)。

## v0.1 能力

- 按关键关系、比较维度和审阅需求选择表达，避免文件数量或字数触发不必要的图示。
- 保留完成、部分完成、失败或阻塞状态，准确报告已进行的检查与未运行的测试。
- 对照已有材料检查数字、因果、代码标识符和跨格式一致性。
- 图示能力不足时提供可读的文本回退。

技能本体只有两个文件：

```text
skills/human-comprehension/
├── SKILL.md
└── references/
    └── diagram-guidelines.md
```

## 使用

下载或克隆此仓库，将整个 `skills/human-comprehension` 文件夹复制到宿主支持的技能发现目录，保持参考文档的相对路径。

```text
git clone https://github.com/OpenQwert/karpathy-human-comprehension-skills.git
```

本项目工作环境已有 `~/.agents/skills` 技能目录。其他环境的发现路径、重载方式及自动调用支持，需要按实际宿主确认。仅复制 `SKILL.md` 会使图示参考不可用。

支持显式技能调用的宿主中，可以这样提出任务：

```text
$human-comprehension
解释这次代码修改的结果、关键交互与验证边界。
```

也可以要求纯文本、表格或特定详略；技能会保留你的呈现偏好。自动发现与调用由宿主负责。

## 验证与边界

技能格式验证和行为验收记录见[验证报告](docs/validation-report.md)。行为验收覆盖收敛设计的十个案例，不代表已证明人类理解速度或错误发现率得到改善。

该技能使用已有工作产物和证据；呈现保真检查不能替代底层任务核验。v0.1 不自动生成 HTML 或视频，也不提供安装器、事实验证运行时或多平台适配器。实际使用反馈决定后续扩展。
