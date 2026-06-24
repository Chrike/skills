Languages: [English](README.md) | [简体中文](README.zh-CN.md)

# Coding Agent Skills

[![skills.sh](https://skills.sh/b/Chrike/coding-agent-skills)](https://skills.sh/Chrike/coding-agent-skills)

A lightweight skill suite for Claude Code and Codex-assisted development.

The goal is to keep ordinary coding fast while still giving the agent clear workflows for debugging, testing, planning, review, handoff, reliability correction, and delegated work when those workflows are actually needed.

## What This Is

This repository contains focused `SKILL.md` files for AI-assisted software development.

The suite is designed around a simple rule:

> Start lightweight. Escalate only when the task, risk, or user request justifies it.

It does not try to turn every coding task into a formal process.

## Skills

### Automatic Workflow Skills

These can be selected by the agent when the request clearly matches.

| Skill                  | Use when                                                     |
| ---------------------- | ------------------------------------------------------------ |
| `debug-systematically` | Unclear bugs, flaky behavior, regressions, slow paths, repeated failed fixes |
| `test-strategy`        | Tests, TDD, mocks, flaky tests, regression coverage          |
| `review-and-finish`    | Code review, review feedback, done/fixed/passing verification, PR feedback |
| `plan-work`            | Explicit planning, approach comparison, roadmap, task breakdown, vertical slices |
| `design-codebase`      | Architecture, seams, interfaces, adapters, domain language, prototypes |
| `reliability-check`    | Explicit reassessment for hallucination, guessing, stale context, wrong direction, unsupported confidence, source-vs-memory confusion, or example-vs-task confusion |

### Manual Workflow Skills

These are explicit command workflows for high-cost, side-effecting, durable, or rarely needed actions. They are useful when intentionally invoked, not as everyday automatic routing.

| Skill            | Use when                                                     |
| ---------------- | ------------------------------------------------------------ |
| `finish-branch`  | Explicit commit, push, merge, PR preparation, discard, branch wrap-up |
| `agent-workflow` | Explicit subagents, parallel agents, delegated implementation |
| `issue-workflow` | PRDs, issue drafts, tracker-ready work items, triage         |
| `memory-handoff` | Context compression, handoff, resume state                   |
| `decision-map`   | Durable multi-session decision maps                          |

## Repository Structure

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

## Installation

The workflow-skill source folders live under `skills/`.

The base default-behavior source fragments live under `prompts/`.

The routing/evaluation artifacts live under `tests/`.

Copy or symlink only the skills you want into the directory your agent runtime scans. Adapt the prompt fragments into `AGENTS.md`, `CLAUDE.md`, or equivalent runtime-level instructions instead of installing them as skills.

### Claude Code

Project-level install:

```bash
mkdir -p .claude/skills
rsync -a skills/ .claude/skills/
```

User-level install:

```bash
mkdir -p ~/.claude/skills
rsync -a skills/ ~/.claude/skills/
```

### Codex

Project-level install:

```bash
mkdir -p .agents/skills
rsync -a skills/ .agents/skills/
```

User-level install:

```bash
mkdir -p ~/.agents/skills
rsync -a skills/ ~/.agents/skills/
```

## Install With skills.sh

The `skills` CLI installs the runtime skills from `skills/`.
The prompt fragments and tests remain source material in this repository.

List available skills:

```bash
npx skills add Chrike/coding-agent-skills --list
```

Install all skills:

```bash
npx skills add Chrike/coding-agent-skills --all
```

Install selected skills:

```bash
npx skills add Chrike/coding-agent-skills --skill debug-systematically --skill test-strategy
```

Install selected skills for Claude Code:

```bash
npx skills add Chrike/coding-agent-skills --skill debug-systematically --skill test-strategy --agent claude-code
```

Install selected skills for Codex:

```bash
npx skills add Chrike/coding-agent-skills --skill debug-systematically --skill test-strategy --agent codex
```

## Prompt And Test Maintenance

- `CLAUDE.fragment.md` and `AGENTS.fragment.md` are parallel source-material variants and should stay semantically aligned.
- `AGENTS.fragment.md` and `CLAUDE.fragment.md` are the authoritative source for always-on default behavior.
- `routing-contract.md` is the maintenance-layer summary of suite-level routing and composition.
- Each runtime skill's `description` and `SKILL.md` body are the authoritative source for that skill's trigger boundary.
- `trigger-matrix.md` is the compact smoke test for shared default rules and high-frequency routing.
- `expected-routing.md` is the positive-routing example set.
- `non-trigger-cases.md` is the anti-trigger and drift-regression check.
- Treat `tests/` and this README as validation/overview layers, not a second normative spec. If they drift from prompts or skill text, update the summaries/tests.
- Keep these files small and stable; avoid moving workflow-specific detail out of skills unless it truly belongs in the always-on default layer.

## Recommended Start

Start with the smallest set that matches your actual workflow.

### Core Automatic Set

1. Base default behavior from `prompts/`
2. `debug-systematically`
3. `test-strategy`
4. `review-and-finish`

### Optional Automatic Skills

Add these if you regularly ask for explicit planning, design, or reassessment:

- `plan-work`
- `design-codebase`
- `reliability-check`

### Optional Manual Workflows

Add these only if you want explicit command workflows for heavier actions:

- `finish-branch`
- `agent-workflow`
- `issue-workflow`
- `memory-handoff`
- `decision-map`

## Design Principles

### Keep Ordinary Coding Lightweight

Most tasks should stay in the base default flow:

1. read relevant files
2. make a narrow change
3. run the smallest useful check
4. report what changed and what was verified

### Use Runtime Tools Normally

These skills do not replace Claude Code or Codex capabilities.

If available, the agent should still use:

- LSP
- code index
- semantic search
- MCP tools
- IDE context
- managed isolation
- memory
- configured subagents
- repository-native scripts and checks

Runtime capabilities are tools. They are not workflow escalation.

### Avoid Unnecessary Ceremony

Do not manually start heavy workflows for ordinary tasks:

- PRDs
- specs
- issues
- ADRs
- strict TDD
- subagent coordination
- manual worktrees
- branches
- commits
- pushes
- full test sweeps
- broad refactors

Use heavy workflows only when the user asks or the task clearly requires them.

Treat runtime capabilities differently from workflow escalation: if the runtime already provides safe project-aware tools, managed isolation, memory, or configured subagents, use those capabilities normally. Do not treat that as permission to start side-effecting branch actions such as commit, push, merge, discard, or cleanup.

### Completion Requires Evidence

Before claiming work is complete, fixed, passing, ready, or reviewed, the agent should name the command or observation that supports the claim.

If checks were skipped, say so plainly.

## Customization

Keep changes small.

Good changes:

- tighten a trigger
- remove workflows you do not use
- clarify when to stop
- add a reference for a repeated real failure

Avoid changes that make every task slower.

## Status

This is a practical personal-development workflow layer.

It is meant to shape Claude Code and Codex behavior, not replace their default capabilities.
