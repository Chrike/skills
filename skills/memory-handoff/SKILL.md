---
name: memory-handoff
description: Manual-only. Use when the user explicitly invokes this skill to prepare for context compression, resume after compression, create or update a handoff, preserve project state for another session, or recover the current task from a user-named handoff or memory file.
disable-model-invocation: true
---

# Memory Handoff

Preserve enough state to continue accurately without turning every task into documentation work.

## First Decision

- If the user asks to compress context, update the named handoff file or existing project handoff note before they compress.
- If resuming after compression, read the named handoff or memory file first and continue from the latest user intent.
- If the task is ordinary coding and no handoff/resume is involved, do not use this skill.
- If existing artifacts already capture a detail, link to them instead of duplicating it.

## Update Memory

When preparing a handoff, write a compact project-local update that includes:

1. Current objective in one sentence.
2. Latest user intent, including corrections or changed priorities.
3. Files changed or created, with paths.
4. Current status: completed, in progress, blocked, or deliberately deferred.
5. Next concrete action.
6. Explicit "do not do" items that prevent drift.

Prefer the existing handoff file, task note, or user-named handoff or memory file when one exists.

## Resume From Memory

Before acting after a resume:

1. Read the handoff or memory file the user names. If none is named, use the existing handoff file or task note only when there is a single obvious candidate; otherwise state the candidate before relying on it.
2. Read any directly referenced planning/review file needed for the current next step.
3. Identify the newest user request and let it override older plans.
4. State the current objective briefly, then continue.

Do not restart audits, re-argue settled decisions, or act on an older plan if the handoff or memory file records a correction.

## Handoff Shape

Keep handoffs short and operational:

- what the project is trying to accomplish
- what has already been decided
- what was changed
- what remains unclear
- what to do next
- what not to do

Use bullet lists when they make state easier to scan. Avoid narrative session history.

## Boundaries

Do not store secrets, credentials, private data, or unrelated user information.

Do not copy large source content into handoff notes. Reference paths instead.

Do not make memory updates a default step for small tasks. Use them at compression points, handoffs, long-running work, or when the user asks.
