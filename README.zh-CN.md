Languages: [English](README.md) | [简体中文](README.zh-CN.md)

# 编码代理技能集

[![skills.sh](https://skills.sh/b/Chrike/coding-agent-skills)](https://skills.sh/Chrike/coding-agent-skills)

一套面向 Claude Code 和 Codex 辅助开发的轻量级技能套件。

目标是在保持日常编码高效的同时，为代理提供清晰的工作流，以便在实际需要时进行调试、测试、规划、评审、交接、可靠性修正及任务委派。

## 简介

本仓库包含用于 AI 辅助软件开发的专用 `SKILL.md` 文件。

该套件围绕一条简单规则设计：

> 从轻量开始。仅当任务、风险或用户请求确有必要时，才升级流程。

它并不试图将每个编码任务都变成正式流程。

## 技能列表

### 自动工作流技能

当请求明确匹配时，代理可自动选择这些技能。

| 技能                   | 适用场景                                                     |
| ---------------------- | ------------------------------------------------------------ |
| `debug-systematically` | 原因不明的 Bug、不稳定行为、回归问题、性能瓶颈、反复修复失败 |
| `test-strategy`        | 测试、TDD、Mock、不稳定测试、回归覆盖                        |
| `review-and-finish`    | 代码评审、评审反馈、完成/修复/通过验证、PR 反馈              |
| `plan-work`            | 显式规划、方案对比、路线图、任务拆解、垂直切片               |
| `design-codebase`      | 架构、接缝、接口、适配器、领域语言、原型设计                 |
| `reliability-check`    | 针对幻觉、猜测、过时上下文、方向错误、无依据的自信、源码与记忆混淆、示例与任务混淆的显式重新评估 |

### 手动工作流技能

这些是用于高成本、有副作用、持久性或低频操作的显式命令工作流。它们适用于有意调用的场景，而非日常的自动路由。

| 技能             | 适用场景                                          |
| ---------------- | ------------------------------------------------- |
| `finish-branch`  | 显式提交、推送、合并、PR 准备、丢弃变更、分支收尾 |
| `agent-workflow` | 显式子代理、并行代理、委派实现                    |
| `issue-workflow` | PRD、Issue 草稿、可录入跟踪系统的工作项、分诊     |
| `memory-handoff` | 上下文压缩、交接、状态恢复                        |
| `decision-map`   | 跨会话持久化决策图                                |

## 仓库结构

```text
.
├── README.md
├── README.zh-CN.md
├── prompts/
│   ├── AGENTS.fragment.md
│   └── CLAUDE.fragment.md
├── tests/
│   ├── trigger-matrix.md
│   ├── expected-routing.md
│   ├── non-trigger-cases.md
│   └── routing-contract.md
└── skills/
    ├── debug-systematically/
    │   ├── SKILL.md
    │   └── references/
    ├── test-strategy/
    │   ├── SKILL.md
    │   └── references/
    ├── review-and-finish/
    │   ├── SKILL.md
    │   └── references/
    ├── finish-branch/
    │   └── SKILL.md
    ├── plan-work/
    │   ├── SKILL.md
    │   └── references/
    ├── design-codebase/
    │   ├── SKILL.md
    │   └── references/
    ├── agent-workflow/
    │   └── SKILL.md
    ├── issue-workflow/
    │   └── SKILL.md
    ├── memory-handoff/
    │   └── SKILL.md
    ├── decision-map/
    │   └── SKILL.md
    └── reliability-check/
        └── SKILL.md
```

## 安装

工作流技能的源文件夹位于 `skills/` 下。

基础默认行为的源片段位于 `prompts/` 下。

路由/评估产物位于 `tests/` 下。

仅将所需技能复制或符号链接到代理运行时扫描的目录中。应将提示词片段整合进 `AGENTS.md`、`CLAUDE.md` 或等效的运行时级指令中，而不是作为技能安装。

### Claude Code

项目级安装：

```bash
mkdir -p .claude/skills
rsync -a skills/ .claude/skills/
```

用户级安装：

```bash
mkdir -p ~/.claude/skills
rsync -a skills/ ~/.claude/skills/
```

### Codex

项目级安装：

```bash
mkdir -p .agents/skills
rsync -a skills/ .agents/skills/
```

用户级安装：

```bash
mkdir -p ~/.agents/skills
rsync -a skills/ ~/.agents/skills/
```

## 使用 skills.sh 安装

`skills` CLI 会安装 `skills/` 中的运行时技能。
`prompts/` 与 `tests/` 仍然是仓库内的源材料，不会作为运行时 skill 安装。

列出可用 skills：

```bash
npx skills add Chrike/coding-agent-skills --list
```

安装全部 skills：

```bash
npx skills add Chrike/coding-agent-skills --all
```

安装指定 skills：

```bash
npx skills add Chrike/coding-agent-skills --skill debug-systematically --skill test-strategy
```

为 Claude Code 安装指定 skills：

```bash
npx skills add Chrike/coding-agent-skills --skill debug-systematically --skill test-strategy --agent claude-code
```

为 Codex 安装指定 skills：

```bash
npx skills add Chrike/coding-agent-skills --skill debug-systematically --skill test-strategy --agent codex
```

## 提示词与测试维护

- `CLAUDE.fragment.md` 和 `AGENTS.fragment.md` 是平行的源材料变体，应保持语义一致。
- `AGENTS.fragment.md` 和 `CLAUDE.fragment.md` 是常驻默认行为的权威来源。
- `routing-contract.md` 是套件级路由与组合的维护层摘要。
- 每个运行时技能的 `description` 和 `SKILL.md` 正文是该技能触发边界的权威来源。
- `trigger-matrix.md` 是针对共享默认规则和高频路由的精简冒烟测试。
- `expected-routing.md` 是正向路由示例集。
- `non-trigger-cases.md` 是反触发与漂移回归检查。
- 将 `tests/` 和本 README 视为验证/概览层，而非第二规范标准。若其与提示词或技能文本不一致，请更新摘要/测试。
- 保持这些文件精简稳定；除非确实属于常驻默认层，否则避免将工作流特定细节移出技能文件。

## 推荐起步

从匹配您实际工作流的最小集合开始。

### 核心自动集合

1. 来自 `prompts/` 的基础默认行为
2. `debug-systematically`
3. `test-strategy`
4. `review-and-finish`

### 可选自动技能

如果您经常需要显式规划、设计或重新评估，可添加以下技能：

- `plan-work`
- `design-codebase`
- `reliability-check`

### 可选手动工作流

仅在您需要针对较重操作的显式命令工作流时添加：

- `finish-branch`
- `agent-workflow`
- `issue-workflow`
- `memory-handoff`
- `decision-map`

## 设计原则

### 保持日常编码轻量

大多数任务应保持在基础默认流程中：

1. 读取相关文件
2. 进行小范围修改
3. 运行最小有效检查
4. 报告变更内容及验证结果

### 正常使用运行时工具

这些技能不会取代 Claude Code 或 Codex 的能力。

如果可用，代理仍应使用：

- LSP
- 代码索引
- 语义搜索
- MCP 工具
- IDE 上下文
- 托管隔离
- 记忆
- 已配置的子代理
- 仓库原生脚本与检查

运行时能力是工具，不是工作流升级。

### 避免不必要的仪式感

不要为普通任务手动启动重型工作流：

- PRD
- 规格说明
- Issue
- ADR
- 严格 TDD
- 子代理协调
- 手动 worktree
- 分支
- 提交
- 推送
- 全量测试扫描
- 大规模重构

仅在用户要求或任务明确需要时使用重型工作流。

区分运行时能力与工作流升级：如果运行时已提供安全的项目感知工具、托管隔离、记忆或已配置的子代理，请正常使用这些能力。不要将其视为启动提交、推送、合并、丢弃或清理等有副作用分支操作的许可。

### 完成需有证据

在声称工作已完成、已修复、已通过、已就绪或已评审之前，代理应指明支持该声明的命令或观察结果。

如果跳过了检查，请明确说明。

## 自定义

保持改动精简。

推荐的改动：

- 收紧触发条件
- 移除不使用的工作流
- 明确何时停止
- 为重复出现的真实故障添加参考

避免导致所有任务变慢的改动。

## 状态

这是一个实用的个人开发工作流层。

其旨在塑造 Claude Code 和 Codex 的行为，而非取代其默认能力。
