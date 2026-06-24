# Trigger Matrix

Use this file to pressure-test whether the active development workflow split still behaves as intended.

It is not a runtime skill.
The prompt fragments are authoritative for default behavior, and skill descriptions/bodies are authoritative for routing. This file validates those contracts.

## Default Behavior Cases

| Prompt | Expected routing |
| --- | --- |
| Fix this small TypeScript error. | Base default behavior |
| Change this label in a Vue component. | Base default behavior |
| Explain how this Spring service method works. | Base default behavior |
| Add this small request parameter to the endpoint. | Base default behavior |
| Implement the approved steps from this existing plan file. | Base default behavior |
| Continue this paused task using the current issue or work-item draft. | Base default behavior |
| What is the current goal and why are you doing this? | Base default behavior |
| We are still inspecting these files; do not start rewriting yet. | Base default behavior |

## Workflow Skill Cases

| Prompt | Expected routing |
| --- | --- |
| This test is flaky; diagnose it. | `debug-systematically` |
| Add regression tests for this bug. | `test-strategy` |
| Review these changes. | `review-and-finish` |
| Finish this branch. | `finish-branch` |
| Plan this refactor. | `plan-work` |
| Where should this interface live? | `design-codebase` |
| You are hallucinating; reread the files and reassess. | `reliability-check` |
| You are drifting; stop and reassess the active stage. | `reliability-check` |

## Manual Workflow Cases

| Prompt | Expected routing |
| --- | --- |
| Manually invoke `agent-workflow` for frontend and backend slices. | `agent-workflow` |
| Manually invoke `issue-workflow` to turn this into a PRD. | `issue-workflow` |
| Manually invoke `decision-map` for this vague multi-session direction. | `decision-map` |
| Manually invoke `memory-handoff` to prepare for context compression. | `memory-handoff` |

## Shared Default Rule Smoke Cases

These validate prompt-fragment behavior without restating the prompt layer.

| Prompt | Expected behavior |
| --- | --- |
| What does this file say? | Read the file before claiming. |
| Here is another model's review; apply it. | Treat it as reference input and verify before changing code. |
| We are only inspecting; do not rewrite yet. | Stay in inspection mode. |
| Is this done? | Name verification evidence or state the gap. |
| Continue from this handoff file. | Read the named artifact first and follow the latest user request. |
| We already reviewed this. Continue with the next step. | Move to the next requested action instead of repeating the review. |
| Start the changes based on the conclusion above. | Use the settled conclusion and begin the requested action. |

## Maintenance / Meta Cases

| Prompt | Expected routing |
| --- | --- |
| Which workflow should handle this? | Answer from `routing-contract.md` and the skill descriptions |
| Review the trigger boundaries. | Use `routing-contract.md` plus the trigger and non-trigger tests |

## Failure Signals

- ordinary coding requires `agent-workflow`
- manual-only workflows trigger from ordinary natural-language prompts
- ordinary coding becomes planning or review chatter
- settled review or planning conclusions are re-opened without new evidence, contradiction, or a new request
- "continue", "start", or "execute" after a completed analysis still loops back into repeated summary
- the base default behavior layer drifts apart from the workflow skills that assume it
- default behavior rules start creeping back into workflow skills unnecessarily
- completion claims appear without evidence
- duplicate durable artifacts are created for the same tracked work
- tracked work stays falsely in progress after completion, pause, or mainline switch
- output shifts toward long process narration instead of code or findings
