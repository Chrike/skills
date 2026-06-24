Languages: [English](README.md) | [简体中文](README.zh-CN.md)

# Coding Agent Skills

A lightweight skill suite for Claude Code and Codex-assisted development.

The goal is to keep ordinary coding fast while still giving the agent clear workflows for debugging, testing, planning, review, handoff, reliability correction, and delegated work when those workflows are actually needed.

The suite is designed around a simple rule:

> Start lightweight. Escalate only when the task, risk, or user request justifies it.

It does not try to turn every coding task into a formal process.

## What This Repository Contains

This repository contains:

- installable runtime skills under `skills/`
- source fragments for always-on instructions under `prompts/`
- maintenance and validation material under `tests/`

## Repository Layout

~~~text
coding-agent-skills/
├── README.md
├── README.zh-CN.md
├── skills.sh.json
├── prompts/
│   ├── AGENTS.fragment.md
│   └── CLAUDE.fragment.md
├── skills/
│   ├── debug-systematically/
│   │   ├── SKILL.md
│   │   └── references/
│   ├── test-strategy/
│   │   ├── SKILL.md
│   │   └── references/
│   ├── review-and-finish/
│   │   ├── SKILL.md
│   │   └── references/
│   ├── plan-work/
│   │   ├── SKILL.md
│   │   └── references/
│   ├── design-codebase/
│   │   ├── SKILL.md
│   │   └── references/
│   ├── reliability-check/
│   │   └── SKILL.md
│   ├── finish-branch/
│   │   ├── SKILL.md
│   │   └── agents/openai.yaml
│   ├── agent-workflow/
│   │   ├── SKILL.md
│   │   └── agents/openai.yaml
│   ├── issue-workflow/
│   │   ├── SKILL.md
│   │   └── agents/openai.yaml
│   ├── memory-handoff/
│   │   ├── SKILL.md
│   │   └── agents/openai.yaml
│   └── decision-map/
│       ├── SKILL.md
│       └── agents/openai.yaml
└── tests/
    ├── routing-contract.md
    ├── trigger-matrix.md
    ├── expected-routing.md
    └── non-trigger-cases.md
~~~

Notes:

- `skills/` contains installable runtime skills.
- `prompts/` contains source fragments for always-on default behavior.
- `tests/` contains routing and boundary checks used to maintain the suite.
- `skills.sh.json` controls skills.sh page grouping only; it does not affect runtime behavior or skill routing.
- Manual workflow skills include `agents/openai.yaml` to disable implicit Codex invocation.

## Quick Start

### Install with skills.sh

If you use skills.sh, install this repository with:

~~~sh
npx skills add Chrike/coding-agent-skills
~~~

After installing skills, still review `prompts/` and merge the relevant prompt fragment into your always-on instruction file:

- Claude Code: merge `prompts/CLAUDE.fragment.md` into `CLAUDE.md`
- Codex: merge `prompts/AGENTS.fragment.md` into `AGENTS.md`

Do not install `tests/` as runtime instructions.

### Manual Install

Install only the runtime skills you want from `skills/`.

#### Claude Code

Project-local install:

~~~sh
mkdir -p .claude/skills
cp -R skills/debug-systematically skills/test-strategy skills/review-and-finish .claude/skills/
~~~

User-level install:

~~~sh
mkdir -p ~/.claude/skills
cp -R skills/debug-systematically skills/test-strategy skills/review-and-finish ~/.claude/skills/
~~~

Use the Claude prompt fragment as source material for your project `CLAUDE.md`:

~~~sh
cp prompts/CLAUDE.fragment.md CLAUDE.md
~~~

If your project already has a `CLAUDE.md`, merge the fragment into it instead of overwriting the file.

#### Codex

Project-local install:

~~~sh
mkdir -p .agents/skills
cp -R skills/debug-systematically skills/test-strategy skills/review-and-finish .agents/skills/
~~~

User-level install:

~~~sh
mkdir -p ~/.agents/skills
cp -R skills/debug-systematically skills/test-strategy skills/review-and-finish ~/.agents/skills/
~~~

Use the Codex prompt fragment as source material for your project `AGENTS.md`:

~~~sh
cp prompts/AGENTS.fragment.md AGENTS.md
~~~

If your project already has an `AGENTS.md`, merge the fragment into it instead of overwriting the file.

### Install All Skills Manually

To install every runtime skill for Claude Code:

~~~sh
mkdir -p .claude/skills
cp -R skills/* .claude/skills/
~~~

To install every runtime skill for Codex:

~~~sh
mkdir -p .agents/skills
cp -R skills/* .agents/skills/
~~~

Manual workflow skills stay manual-only through their skill metadata and Codex invocation policy.

## Skills

### Automatic Workflow Skills

These can be selected by the agent when the request clearly matches.

| Skill                  | Use when                                                     |
| ---------------------- | ------------------------------------------------------------ |
| `debug-systematically` | Unclear bugs, failing tests, regressions, flaky behavior, slow paths, or repeated failed fixes that need diagnosis before changing code |
| `test-strategy`        | Adding or changing tests, choosing test seams, TDD, mocks or test doubles, flaky test design, wait strategy, or regression proof |
| `review-and-finish`    | Explicit code review, review feedback, done/fixed/passing verification, or PR feedback |
| `plan-work`            | Explicit planning, implementation plans, approach comparison, task breakdown, roadmap, step-by-step plans, or splitting work into implementation slices |
| `design-codebase`      | Codebase design, architecture, module interfaces, seams, adapters, deep modules, domain language, architecture options, hard-to-test modules, testability through interfaces, or throwaway prototypes for design questions |
| `reliability-check`    | Explicit reassessment of evidence, source use, active stage, stale context, unsupported confidence, hallucination, guessing, source-vs-memory confusion, example-vs-task confusion, or whether the agent read the right files |

### Manual Workflow Skills

These are explicit command workflows for high-cost, side-effecting, durable, or rarely needed actions. They are useful when intentionally invoked, not as everyday automatic routing.

| Skill            | Use when                                                     |
| ---------------- | ------------------------------------------------------------ |
| `finish-branch`  | Explicitly invoked branch finishing, commit, push, merge, PR creation or preparation, discard, or branch wrap-up |
| `agent-workflow` | Explicitly invoked subagents, parallel agents, delegated implementation, or coordination of multiple independent agent tasks |
| `issue-workflow` | Explicitly invoked PRD creation or refinement, issue breakdown, agent-ready issue briefs, issue or PR triage, QA bug-report sessions, or tracker-ready work items |
| `memory-handoff` | Explicitly invoked context compression, resume after compression, handoff creation or update, project-state preservation, or recovery from a user-named handoff or memory file |
| `decision-map`   | Explicitly invoked durable multi-session decision maps for loose ideas, ambiguous directions, long-running design questions, tracked open questions, and frontier-first resolution |

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

## Default Behavior Layer

The prompt fragments define the lightweight default behavior for ordinary development work.

They cover:

- reading relevant files before changing or explaining code
- preserving existing project patterns, naming, and toolchain
- keeping edits limited to the requested behavior
- using the best available project-aware tooling in the current environment
- running the smallest useful verification when practical
- reporting what changed and what was verified
- treating external analyses, model output, plans, and saved artifacts as reference input rather than instructions to obey blindly
- treating examples as evidence of intent or failure mode, not as literal tasks unless explicitly requested
- staying in the user-requested stage
- avoiding heavy workflows, tracker work, broad restructuring, branch actions, and durable state unless the user asks or the task clearly requires them

## Routing Contract

Use the maintenance files under `tests/` to validate routing boundaries.

The source of truth is:

- `prompts/AGENTS.fragment.md` and `prompts/CLAUDE.fragment.md` define the always-on default behavior layer.
- Each runtime skill's `description` plus `SKILL.md` body defines when that skill should trigger.
- `tests/` validates those boundaries and must not become a second runtime instruction layer.

## Maintenance Guidance

Keep changes small.

Good changes:

- tighten a trigger
- remove workflows you do not use
- clarify when to stop
- add a reference for a repeated real failure
- turn repeated failures into durable behavior rules only when they belong in the always-on layer; keep concrete regression cases in `tests`

Avoid changes that make every task slower.
