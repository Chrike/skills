# Prototypes

Build a prototype only when a design question needs runnable feedback. Prototype code is temporary and exists to answer one question.

## Logic Prototype

Use for business logic, state transitions, or data shape.

- State the question at the top of the prototype.
- Use the project's existing language and task runner.
- Keep state in memory unless persistence is the question.
- Put reusable logic behind a small pure interface; keep terminal/browser shell throwaway.
- Surface the full relevant state after each action.
- Provide one project-local command to run it.

## UI Prototype

Use when the question is visual structure or interaction shape.

- Prefer variants inside an existing page or route.
- Use `?variant=` or the project's equivalent to switch options.
- Create 2 structurally different variants by default when visual comparison matters. Use 3 only when the design space is genuinely broad.
- Keep real data fetching when useful, but avoid real mutations.
- Remove losing variants and any switcher when a direction is chosen.

## Cleanup

When the question is answered, either delete the prototype or fold the validated decision into real code. Capture only the answer and why it mattered.

Do not add tests, production polish, broad abstractions, new runtimes, or persistent infrastructure to a prototype unless that is the specific question being tested.
