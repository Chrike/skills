# Expected Routing

Use this file to validate that common prompt shapes route to the right layer or skill after the current refactor.

It is not a runtime skill.
The prompt fragments are authoritative for default behavior, and skill descriptions/bodies are authoritative for routing. This file validates those contracts.

## Base Default Behavior

These prompts should stay in the base default behavior layer, not load a heavier workflow skill first:

| Prompt Shape | Expected Routing | Why |
| --- | --- | --- |
| Fix this small TypeScript error. | base default behavior | ordinary narrow implementation |
| Change this label in a Vue component. | base default behavior | direct edit with focused verification |
| Explain how this Spring service method works. | base default behavior | code reading and explanation, not planning |
| Add this small request parameter to the endpoint. | base default behavior | small implementation change |
| Implement this feature. | base default behavior first, then task-specific escalation only if the request clearly requires it | avoid default routing into heavy workflows |
| Implement the approved steps from this existing plan file. | base default behavior | existing durable plan should guide execution without reopening planning |
| Continue this paused task using the current issue or work-item draft. | base default behavior | reuse tracked state instead of reopening workflow-layer artifact creation |
| Start implementing the approved plan above. | base default behavior | settled planning should feed execution directly |
| Start the reviewed fix above. | base default behavior | settled review should feed execution directly until new evidence changes it |
| Answer what you are doing, then continue the current task. | base default behavior | direct status answer plus continued execution stays in the default layer |
| We already cancelled that older direction. Continue with the current task only. | base default behavior | settled cancellations should hold unless the user reopens them |
| Handle these remaining prompt-file fixes in one pass. | base default behavior | batched continuation should stay in ordinary execution flow |

## Core Workflow Skills

These prompts should route to the named workflow skill:

| Prompt Shape | Expected Routing | Why |
| --- | --- | --- |
| This test is flaky; diagnose it. | `debug-systematically` | diagnosis before patching |
| The API returns wrong data after the refactor. | `debug-systematically` | unclear regression |
| Add regression tests for this bug. | `test-strategy` | test design and proof |
| Use TDD for this feature. | `test-strategy` | explicit testing mode |
| Review these changes. | `review-and-finish` | explicit review request |
| Can I call this done? | `review-and-finish` | explicit completion verification |
| Finish this branch. | `finish-branch` | explicit branch-ending action |
| Commit these changes. | `finish-branch` | explicit side-effect action |
| Plan this refactor. | `plan-work` | explicit planning ask |
| Which approach should we take? | `plan-work` | compare approaches before implementation |
| Where should this interface live? | `design-codebase` | architecture / seam decision |
| This module is hard to test. | `design-codebase` | design question, not just test syntax |

## Manual Workflow Layers

These prompts should only route to the manual workflow-layer skills named below when the user explicitly invokes the manual skill:

| Prompt Shape | Expected Routing | Why |
| --- | --- | --- |
| Manually invoke `agent-workflow` for frontend and backend slices. | `agent-workflow` | manual delegation workflow |
| Manually invoke `issue-workflow` to turn this into a PRD. | `issue-workflow` | durable work-item request |
| Manually invoke `issue-workflow` to break this PRD into issues. | `issue-workflow` | tracker-style decomposition |
| Manually invoke `decision-map` for this vague direction. | `decision-map` | durable multi-session frontier |
| Manually invoke `memory-handoff` before context compression. | `memory-handoff` | explicit handoff / compression |
| Manually invoke `memory-handoff` to update the current handoff note before we pause. | `memory-handoff` | explicit handoff update request |
| Manually invoke `memory-handoff` to resume from the named handoff file. | `memory-handoff` | explicit resume request |
| You are hallucinating; reread the files and reassess. | `reliability-check` | explicit corrective reassessment |
| Which workflow should handle this? | routing contract plus skill descriptions | explicit suite-level routing question |

## Review Split Check

The review/branch split should stay sharp:

- `review-and-finish` handles review, review feedback, and done/fixed/passing checks.
- `finish-branch` handles commit, push, merge, PR preparation, discard, and branch wrap-up.
- A combined request such as "review these changes, then help me finish the branch" may use both, in that order.
