<div align="center">

# ⚒️ Techneering

### 工程，带着古老的纪律。

**写好规范，趁热打铁，放心交付。**

[![license](https://img.shields.io/badge/license-MIT-black?style=flat-square)](LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/devnomad-byte/techneering?style=flat-square&label=stars&color=yellow)](https://github.com/devnomad-byte/techneering/stargazers)
[![skills](https://img.shields.io/badge/skills-16-orange?style=flat-square)](#16-个技能)
[![platform](https://img.shields.io/badge/platform-Win%20%7C%20mac%20%7C%20Linux-555?style=flat-square)](#环境要求)
[![claude](https://img.shields.io/badge/Claude%20Code-plugin-blueviolet?style=flat-square)](#快速开始)
[![codex](https://img.shields.io/badge/Codex-plugin-412f3d?style=flat-square)](#快速开始)

`tn:` · 一个支持 **Claude Code** 和 **Codex** 的技能插件

[English](README.md) · **简体中文**

</div>

---

## 问题在哪

模型单独写代码会**漂移**。该问的时候它在猜；该停的时候它在过度构建；十分钟前的决策它转头就忘；不验证就声称完成，把锅留给你——在 review 时、在生产环境、在凌晨两点——才发现它悄悄写错了哪里。

产出的样子像代码，却不是手艺。

## 我们的纪律

Techneering 用一套 **harness（挽具）**——Anthropic 对"包裹智能体的脚手架"的称呼——把模型套住，让它的力气始终对准*你*指定的方向。16 个技能把一次变更从想法，经规范和代码，一路走到验证、归档的发布。

> **规范是契约，harness 是纪律。**
> 每一行代码都能回溯到一条需求，绝不靠拍脑袋上线。

---

## 四层，一个系统

AI 工具的演进经历了四层。大多数工具只停在某一层。Techneering 为四层而生。

| 层 | 回答的问题 | 在 Techneering 里 |
|----|-----------|-------------------|
| **Prompt** | 怎么写好一条指令 | 每个 `SKILL.md` 都是一份精心设计的 prompt |
| **Context** | 怎么管理 AI 看到的上下文 | 工作区——文件承载状态，不靠聊天记忆 |
| **Harness** | 怎么约束 agent 的行为 | 16 个 skill 组成的脚手架：*规范=契约，harness=纪律* |
| **Loop** | 怎么让 AI 迭代到质量达标 | `assemble → redgreen → gate → audit → inspect → ship → vault` |

这四层不是四个独立功能——而是同一个系统从不同角度看的样子。而且 Techneering 保留了 loop 系统通常忽略的部分：明确的**失败上限**、**生成 ≠ 评估**、以及一个**深度旋钮**让微小改动跳过全套仪式。

---

## 16 个技能

名字遵循**锻造工坊**：规范是原料，代码是锻件，每个技能是车间里的一个工位。

### 🧭 熔炉 — 核心流程

| 技能 | 调用 | 做什么 |
|------|------|--------|
| **compass** | `tn:compass` | 网关。会话启动自动加载——选对路径 |
| **scout** | `tn:scout` | 大声思考。探索想法，不写文件 |
| **draft** | `tn:draft` | 一次性生成 proposal + specs + design + tasks |
| **forge** | `tn:forge` | 实现。内部链式调用 blueprint → assemble |
| **audit** | `tn:audit` | 双层核对：代码↔规范 + 代码↔真实需求 |
| **vault** | `tn:vault` | 把 delta spec 合并进主 spec，变更移入归档 |

### 🧠 图纸台 — 思考与规划

| 技能 | 调用 | 做什么 |
|------|------|--------|
| **spark** | `tn:spark` | 头脑风暴设计、检测编码风格、推荐框架 |
| **blueprint** | `tn:blueprint` | 把规范拆成可测试的小任务 |

### 🔨 铁砧 — 执行

| 技能 | 调用 | 做什么 |
|------|------|--------|
| **assemble** | `tn:assemble` | 每任务派一个子 Agent，两阶段审查 |
| **redgreen** | `tn:redgreen` | 测试驱动：红(失败测试) → 绿(最小代码) → 重构 |
| **diagnose** | `tn:diagnose` | 系统化调试：先找根因再修复 |

### 🔍 检验台 — 质量关卡

| 技能 | 调用 | 做什么 |
|------|------|--------|
| **gate** | `tn:gate` | 铁律：跑命令、读输出，*然后*才能声明完成 |
| **inspect** | `tn:inspect` | 全局跨任务审查——一致性、架构、安全 |

### 📦 出料区 — 工具

| 技能 | 调用 | 做什么 |
|------|------|--------|
| **isolate** | `tn:isolate` | 创建隔离 git worktree，验证测试基线 |
| **ship** | `tn:ship` | 分支集成：合并 / PR / 保留 / 丢弃 |
| **craft** | `tn:craft` | 前端设计指导（检测到 UI 关键词自动叠加） |

---

## 快速开始

Techneering 同时支持 **Claude Code** 和 **Codex**。同一套技能、同一个 `tn:` 前缀——选你的宿主。

**Claude Code：**

```bash
# 1. 注册 marketplace
claude plugin marketplace add https://github.com/devnomad-byte/techneering.git

# 2. 安装——推荐项目级
claude plugin install tn@techneering -s project

# 3. 启动 Claude Code，开锤
claude
```

**Codex：**

```bash
# 1. 注册 marketplace（克隆仓库作为来源）
codex plugin marketplace add devnomad-byte/techneering

# 2. 安装
codex plugin install tn@techneering

# 3. 启动 Codex，开锤
codex
```

然后输入 <kbd>/tn:scout</kbd> 跑一圈。每次会话启动，`tn:compass` 自动注入（两个宿主都通过 `SessionStart` hook 实现）——你说想做什么，Techneering 自动路由到对的工位。

---

## 工作原理

Techneering 根据变更规模选四条路径之一，然后一个工位一个工位带你走完：

| 路径 | 流程 |
|------|------|
| **大型变更** | `spark → draft → forge → gate →` 你选验证深度 |
| **中型变更** | `draft → forge → gate →` 你选验证深度 |
| **紧急 bug** | `diagnose → redgreen → gate →` 你选验证深度 |
| **微小改动** | `直接编辑 → gate` *(可选)* |

**随时可用：**
- `scout` —— 纯思考，不写文件
- `craft` —— 检测到前端工作自动叠加

`forge` 内部自动链式两个技能——**blueprint** 把工作拆成任务，**assemble** 每任务派一个新子 Agent，每个任务后做规范合规 + 代码质量审查。

<details>
<summary><b>gate 通过后，选验证深度</b></summary>

| 选项 | 流程 | 适用 |
|------|------|------|
| **1** 完整验证 | `audit → inspect → ship → vault` | 生产代码 |
| **2** 标准流程 | `audit → ship → vault` | 常规功能 |
| **3** 快速提交 | `ship → vault` | 原型、小改动 |
| **4** 仅验证 | `audit` | 只检查不提交 |

</details>

每个技能把进度写入磁盘。关掉终端，明天回来——Techneering 扫描文件，从你上次收锤的地方继续。

---

## 设计哲学

> **你驾驭 AI，而不是反过来。**

| 原则 | 含义 |
|------|------|
| **缰绳在你手里** | 每一步都是*你*批准的移交。AI 提议，你决策。任何东西不过人工关卡不上线。 |
| **规范就是契约** | 需求活在文件里，不在聊天里。每一行代码都能回溯到一条 `SHALL`/`MUST`——漂移变得可见，而不是隐形。 |
| **生成 ≠ 评估** | 写代码的 agent 永远不给它打分。独立的审查者（`tn:audit`、`tn:inspect`）对照规范和现实检查——模型给自己的作业打分总是太宽松。 |
| **上下文是文件，不是记忆** | 每个技能从磁盘读输入、把输出写回磁盘。新会话、新子 agent，同一个事实来源——任何东西都不只活在某人的脑子里。 |
| **够用的最简 harness** | 四条路径，不是一条。微小改动跳过全套仪式；只有生产代码才走完每个关卡。复杂度在任务需要时才加，绝不提前。 |

这个模式——拆解、用产物移交、生成与评估分离——来自 [Anthropic 关于长时运行智能体的研究](https://www.anthropic.com/engineering/harness-design-long-running-apps)。Techneering 把它用在**人机协作**开发上：harness 让 AI 守纪律，你让 harness 对准正确的目标。

Loop 是最新的一层——[Addy Osmani 在 2025 年命名的模式](https://addyosmani.com/blog/loop-engineering/)：别再手动 prompt 每一步，去设计那套替你做这件事的系统。Techneering 从一开始就是这么设计的——包括大多数 loop 系统忽略的部分：明确的**失败上限**（`tn:diagnose` 连续 3 次修复失败就停）、**生成 ≠ 评估**（写代码的永远不给自己打分）、以及一个**深度旋钮**让微小改动跳过全套仪式。

---

## 工作区

运行 `tn:draft` 会在项目根目录生成一个独立工作区。它是唯一事实来源，任何东西都不只活在聊天里。

```
techneering/
├── config.yaml          # 技术栈 · 编码风格 · 质量规则
├── specs/               # 模块的权威定义
├── changes/
│   └── <name>/          # 每个变更一个目录
│       ├── .tn.yaml
│       ├── proposal.md  # 为什么——动机
│       ├── specs/       # 做什么——SHALL/MUST · WHEN/THEN
│       ├── design.md    # 怎么做——决策与权衡
│       └── tasks.md     # 待办——checkbox 追踪，可续作
└── archive/             # 已完成的变更，完整保留
```

`tn:audit` 把每行代码回溯到一条需求。`tn:vault` 把完成的 spec 合并进主 `specs/`，让知识**沉淀**，而不是蒸发。

---

## 环境要求

任选**一个**宿主（Techneering 两个都支持，也可以都装）：

| 依赖 | 为什么需要 |
|------|-----------|
| **Claude Code CLI** *或* **Codex CLI**（最新版） | 运行技能的宿主 |
| **Git** 2.20+ | `isolate` 工位需要 worktree |
| **Bash** 4.0+ | 会话启动 hook（两个宿主共用） |

支持 Windows 10/11 · macOS 12+ · Linux。

---
## Star History

<a href="https://www.star-history.com/?repos=devnomad-byte%2Ftechneering%2Ctechneering%2Ftechneering&type=date&legend=top-left">
 <picture>
   <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/chart?repos=devnomad-byte/techneering%2Ctechneering/techneering&type=date&theme=dark&legend=top-left" />
   <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/chart?repos=devnomad-byte/techneering%2Ctechneering/techneering&type=date&legend=top-left" />
   <img alt="Star History Chart" src="https://api.star-history.com/chart?repos=devnomad-byte/techneering%2Ctechneering/techneering&type=date&legend=top-left" />
 </picture>
</a>