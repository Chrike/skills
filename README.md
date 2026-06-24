Languages: [English](README.md) | [简体中文](README.zh-CN.md)

# Coding Agent Skills

A lightweight skill suite for Claude Code and Codex-assisted development.

The goal is to keep ordinary coding fast while still giving the agent clear workflows for debugging, testing, planning, review, handoff, reliability correction, and delegated work when those workflows are actually needed.

## What This Repository Contains

This repository contains:

- installable runtime skills under `skills/`
- source fragments for always-on instructions under `prompts/`
- maintenance and validation material under `tests/`

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

## Installation

Install only the runtime skill folders you want from `skills/`.

Common target locations:

- Claude Code: project `.claude/skills/` or user `~/.claude/skills/`
- Codex: project `.agents/skills/` or user `~/.agents/skills/`

Use `prompts/` as source material for your always-on instruction file such as `AGENTS.md` or `CLAUDE.md`.

Keep `tests/` as maintenance and validation material rather than runtime skills.
Do not copy `tests/` into `.claude/`, `.agents/`, or other runtime install targets.

## Repository Layout

- `skills/` contains installable runtime skills.
- `skills.sh.json` controls skills.sh page grouping only; it does not affect runtime behavior or skill routing.
- `prompts/` contains source fragments for always-on default behavior.
- `tests/` contains routing and boundary checks used to maintain the suite.
- Manual workflow skills include `agents/openai.yaml` to disable implicit Codex invocation.
- If summary text drifts from the prompt fragments or skill bodies, update the summaries instead of creating a second spec in the README.

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

## Customization

Keep changes small.

Good changes:

- tighten a trigger
- remove workflows you do not use
- clarify when to stop
- add a reference for a repeated real failure

Avoid changes that make every task slower.
