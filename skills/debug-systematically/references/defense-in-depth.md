# Defense In Depth

Use after finding a root cause involving invalid data, unsafe paths, missing state, or assumptions that can be bypassed by another caller.

## Checkpoints

Add only the layers that fit the bug:

| Layer | Purpose |
| --- | --- |
| Entry boundary | Reject invalid user/API/CLI input early. |
| Domain operation | Enforce invariants before important behavior. |
| Environment guard | Prevent dangerous context-specific operations, especially in tests or scripts. |
| Diagnostic context | Leave useful error messages for future failures. |

## Guidance

- Validate where data enters and where damage happens.
- Prefer clear errors over silent fallback.
- Avoid adding every possible guard when one invariant and one boundary check solve the class.
- Add regression coverage for the bypass that caused the bug when a correct seam exists.
- Do not keep debug-only logging unless it is intentional operational telemetry.
