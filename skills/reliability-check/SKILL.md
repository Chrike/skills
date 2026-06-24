---
name: reliability-check
description: Use when the user explicitly asks the agent to reassess evidence, source use, the active stage, stale context, unsupported confidence, hallucination, guessing, source-vs-memory confusion, example-vs-task confusion, or whether it read the right files.
---

# Reliability Check

Reassess evidence, sources, and active stage when the user explicitly challenges reliability. Use this to correct unsupported conclusions without turning ordinary work into a default ceremony.

## First Decision

- Use this only when the user explicitly challenges reliability, evidence, source use, active stage, stale context, hallucination, guessing, or asks for reassessment.
- Do not use this merely because a request involves saved state, examples, reviews, plans, or uncertainty; the base default behavior already covers ordinary evidence discipline.
- Do not use this for ordinary coding, debugging, test writing, planning, architecture, review, issue drafting, delegation, or handoff unless the user flags reliability trouble.
- Stop new edits and unrelated tool actions while reassessing; reading named evidence is allowed.
- If the user names files, memory, review notes, or plans, read those before making claims about them.

## Reliability Loop

1. Restate the latest reliability concern or user correction in one sentence.
2. Identify the active stage: inspect, explain, plan, implement, validate, hand off, or finish branch.
3. Separate:
   - current source-backed facts
   - user-stated corrections
   - external or reference material
   - assistant assumptions
   - unverified claims
4. Reread the named files or artifacts before making source claims.
5. State the correction:
   - wrong source
   - wrong stage
   - wrong artifact
   - wrong workflow
   - unsupported conclusion
   - stale state
6. Continue only from the corrected current state, and state the next concrete action.

## Boundaries

This skill does not replace:

- the base default behavior for ordinary implementation.
- `debug-systematically` for code bugs and root-cause diagnosis.
- `test-strategy` for testing choices and regression proof.
- `review-and-finish` for explicit code review, feedback handling, or completion verification.
- `finish-branch` for explicit branch-ending actions.
- `plan-work` for requested implementation planning.
- `memory-handoff` for routine compression and resume.

Use the base default behavior layer for the standing evidence rules: read before claiming, separate checked facts from inference, and verify challenged claims from source.

Do not turn this into a universal "think harder" step, a long checklist, a second suite router, or a reason to reduce code output on normal tasks.
