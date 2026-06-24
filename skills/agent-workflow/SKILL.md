---
name: agent-workflow
description: Manual-only. Use when the user explicitly invokes this skill for subagents, parallel agents, delegated implementation, or coordinating multiple independent agent tasks.
disable-model-invocation: true
---

# Agent Workflow

Coordinate delegated agent work only when it saves time or context. Keep ordinary implementation in the lightweight development flow.

## First Decision

- Do not use this skill for ordinary code edits, straightforward bugs, normal planning, or normal review.
- Do not delegate just because a task is large or multi-file.
- Use this only when the user explicitly invokes this skill.
- Once this skill is active, you may continue using it after the user approves delegation for independent slices you identified.
- This does not forbid runtime-provided automatic subagent routing when a configured subagent clearly matches a self-contained, verbose, or read-only task.
- If tasks share the same files, hidden state, or uncertain architecture, keep the work in one controller flow until the boundary is clear.

## Delegation Fit

Delegate when all are true:

1. There are at least two independent work slices or investigations.
2. Each slice has a clear owner: files, module, subsystem, failing test file, or question.
3. Each slice can make progress from a focused prompt and project files, without the whole chat history.
4. Write scopes do not overlap, or the task is read-only.
5. The controller can review and integrate the results.

Do not delegate when failures may share one root cause, the task needs one coherent design decision, or coordinating agents costs more than doing the work.

## Controller Contract

Before dispatching:

- State the slices and ownership boundaries.
- Include only task-local context, paths, commands, constraints, and expected output.
- Tell agents they are not alone in the codebase and must not revert unrelated changes.
- For coding slices, require changed paths, test commands, and result summary.
- For read-only slices, require evidence paths and concise conclusions.

If the environment provides actual delegated-agent runtime support, dispatch agents and work on non-overlapping controller tasks while they run when parallel controller work is possible; otherwise wait for results before integrating.

If no actual delegated-agent runtime support exists, do not simulate delegation or claim agents were dispatched. Either produce task briefs for the user to run, or execute the slices sequentially in the controller flow.

Do not duplicate delegated work.

After agents return:

1. Read results before trusting them.
2. Check changed paths for overlap or conflicts.
3. Integrate deliberately.
4. Run focused verification that covers the combined result.
5. Report what each agent did and what you verified.

## File Handoffs

Use files when prompts or reports would become long:

- task brief: exact requirements and ownership
- report: findings, changed paths, commands, results, concerns
- review notes: controller decisions and unresolved items

Prefer project-local scratch paths that are easy to remove. Do not create durable ledgers unless the work is long-running, likely to hit context compression, or the user asks.

## Optional Isolation

Do not create worktrees, branches, commits, pushes, or dependency installs by default.

If isolation is needed, first detect whether the current agent/runtime already provides it. Prefer built-in isolation features when available. Use manual `git worktree` only after explicit user approval and project-local safety checks.

## Boundaries

This skill does not replace:

- `plan-work` for deciding what to build
- `debug-systematically` for unclear shared-root failures
- `review-and-finish` for explicit code review or completion evidence
- `finish-branch` for explicit commits, PRs, or branch wrap-up
- `memory-handoff` for compression and session resume

Do not force every delegated task through implementer plus reviewer plus final branch review. Add review agents only when risk, independence, and user intent justify the cost.
