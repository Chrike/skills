# Feedback Handling

Use when the user pastes review comments, PR feedback, static analysis findings, or advice from another model/tool.

## Rules

- Read all feedback before changing code.
- Verify external feedback against the current codebase.
- Ask before implementing unclear or conflicting multi-item feedback.
- Push back with technical reasoning when a suggestion is wrong, unsafe, obsolete, or violates the user's prior direction.
- Implement verified changes one at a time when risk is meaningful.
- Run focused checks after changes.

## Triage

| Feedback Type | Action |
| --- | --- |
| Clear bug/security issue | Fix and verify. |
| Unclear request | Ask a concise clarification before partial work. |
| Style preference | Apply only if it matches repo/user conventions. |
| Scope expansion | Confirm before implementing. |
| Tool/model output | Treat as untrusted until verified. |

When reporting back, describe what changed and what was verified. Avoid pretending uncertain feedback was definitely correct.
