# Vertical Slices

Use vertical slices when a feature, refactor, or PRD is too large to implement safely in one pass.

## Slice Rules

- Each slice delivers a narrow but complete path through the needed layers.
- A finished slice is demoable, testable, or otherwise verifiable on its own.
- Prefer user-visible or behavior-visible outcomes over layer-only tasks.
- Put enabling refactors first only when they make the feature easier or safer.
- Keep dependencies explicit; avoid slices that can only be understood after every other slice is done.

## Good Slice Shape

```markdown
1. Add the minimal data path for <one behavior>
   - Touches: schema/model/API/UI as needed
   - Proves: user can complete the narrow path
   - Verification: focused test, request, or UI flow
   - Blocked by: none

2. Add <next behavior or variant>
   - Builds on: slice 1
   - Proves:
   - Verification:
```

## Avoid

- "Build backend", then "build frontend", then "write tests" as separate horizontal slices.
- Large infrastructure work before any behavior is proven.
- Tracker-specific issue publishing unless the user asks for issue workflow.
- Splitting so finely that no slice can be reviewed or verified alone.
