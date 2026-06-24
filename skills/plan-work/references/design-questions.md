# Design Questions

Use these only when the request is too vague to plan safely. Ask one question at a time and prefer assumptions when the answer will not change implementation.

## Clarify

- What outcome should be true when this is done?
- Who or what uses the changed behavior?
- Is backward compatibility required?
- Are there data migration, permissions, performance, or reliability constraints?
- Which existing pattern should this follow?

## Compare

When there are real alternatives, present:

- Option A: simplest change that fits current architecture.
- Option B: cleaner design with more migration or refactor cost.
- Option C: temporary bridge or compatibility path, only when needed.

Recommend one option and explain the trade-off in project terms: risk, maintainability, delivery time, verification, and future change cost.

## Stop Conditions

Stop planning and ask before implementation when:

- Requirements conflict.
- The safest approach depends on product intent the code cannot reveal.
- The work would require destructive data changes, broad dependency changes, or system/global setup.
- The user explicitly asked for plan-only output.
