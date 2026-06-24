# Non-Trigger Cases

Use this file to validate that the current skill suite does not route ordinary work into the wrong workflow.

It is not a runtime skill.
The prompt fragments are authoritative for default behavior, and skill descriptions/bodies are authoritative for routing. This file validates those contracts.

## Heavy Skills Must Not Trigger By Default

These prompt shapes should not trigger the named skills unless the user explicitly asks for that workflow:

| Prompt Shape | Must Not Trigger | Why |
| --- | --- | --- |
| Fix this small TypeScript error. | `agent-workflow`, `issue-workflow`, `decision-map` | ordinary coding should stay light |
| Change this label in a Vue component. | `plan-work`, `design-codebase`, `review-and-finish`, `finish-branch` | small edits should not become process |
| Explain how this service works. | `plan-work`, `design-codebase`, `issue-workflow` | code explanation is not architecture review by default |
| Add this small request parameter to the endpoint. | `issue-workflow`, `decision-map` | small implementation should not become durable workflow |
| Implement this feature. | `agent-workflow` | delegation must stay explicit or approved |
| Implement the approved steps from this existing plan file. | `plan-work`, `decision-map`, `memory-handoff` | existing durable plan should guide execution without reopening adjacent workflows |
| Continue with the changes based on the plan above. | `plan-work`, `reliability-check` | settled planning should guide execution instead of reopening analysis |
| Start implementing the reviewed fix. | `review-and-finish`, `reliability-check` | settled review should not restart before new evidence appears |
| Start implementing the approved plan above. | `plan-work`, `reliability-check` | settled planning should not restart before new evidence appears |
| Start the reviewed fix above. | `review-and-finish`, `reliability-check` | settled review should guide execution instead of reopening adjacent workflows |
| You already have enough context. Stop planning and implement the next step. | `plan-work`, `reliability-check` | sufficient context should lead to execution rather than another planning loop |
| Continue this paused task using the current issue or work-item draft. | `issue-workflow`, `decision-map`, `memory-handoff` | existing tracked state should be reused instead of reopening artifact workflows |
| What is the current goal and why are you doing this? | `reliability-check` | ordinary status questions should not become corrective workflows by default |
| What are you doing right now, and what is the next step? | `reliability-check`, `plan-work` | direct status-and-next-step questions should stay in the default layer |
| We are still inspecting these files; do not start rewriting yet. | `reliability-check`, `plan-work`, `design-codebase` | stage alignment should stay in the default layer unless the user explicitly asks for corrective reassessment or a new workflow |
| This example is only to clarify the intent, not the implementation direction. | `plan-work`, `design-codebase`, `reliability-check` | clarifying examples should not be turned into task instructions by default |
| We already cancelled that older direction. Continue with the current task only. | `reliability-check`, `decision-map` | settled cancellations should hold without reopening adjacent tracks |
| Handle these remaining prompt-file fixes in one pass. | `plan-work`, `agent-workflow` | ordinary batched execution should not escalate into a new workflow |

## Review / Branch Split Must Not Collapse

These prompt shapes should keep `review-and-finish` and `finish-branch` separate:

| Prompt Shape | Must Not Trigger | Why |
| --- | --- | --- |
| Review these changes. | `finish-branch` | review should not imply commit/push/merge |
| Can I call this done? | `finish-branch` | completion verification is not branch cleanup |
| Finish this branch. | `review-and-finish` alone | branch-ending actions should not be misrouted as review |
| Commit these changes. | `review-and-finish` alone | explicit side effect should route to manual branch skill |

## Corrective / Meta Skills Must Stay Explicit

These skills should not appear unless the user clearly asks for their layer:

| Skill | Must Not Trigger For | Why |
| --- | --- | --- |
| `reliability-check` | ordinary implementation, ordinary review, ordinary planning | corrective layer should not become universal preflight |
| `reliability-check` | ordinary status questions about current goal or progress | direct state answers should come from the default layer unless the user explicitly flags drift or reassessment |
| `reliability-check` | ordinary stage reminders such as staying in inspection before implementation | preventive stage alignment should come from the default layer unless the user explicitly asks for correction |
| `memory-handoff` | small tasks without compression/resume/handoff | do not turn every task into note-taking |
| `decision-map` | one-session ambiguity, normal refactors, approach comparison | durable artifacts should stay rare |

## Default Layer Must Not Drift Back Into Skills

These are failure conditions:

- workflow skills start repeating broad default rules that already live in `prompts/`
- `debug-systematically` becomes the default path for obvious one-line fixes
- `test-strategy` makes TDD feel mandatory when the user did not ask for it
- `review-and-finish` starts implying branch actions again
- `finish-branch` starts being treated like an ordinary always-on skill
- ordinary direct status questions stop getting direct answers
- continuation requests restart planning or review instead of using settled conclusions
- explanation gets used to justify staying in the wrong stage instead of doing the requested next action
- cancelled or superseded tracks restart without an explicit ask
- ordinary work starts creating duplicate durable artifacts instead of updating the existing tracked state
- completed or paused tracked work is left stale in durable artifacts without a status update
