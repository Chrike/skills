# Deepening

Use this when improving a cluster of shallow modules or choosing where a seam should live.

## Dependency Categories

| Category | Meaning | Design move |
| --- | --- | --- |
| In-process | Pure computation or in-memory state | Merge behavior behind one interface; test directly. |
| Local-substitutable | Local stand-in exists, such as an in-memory filesystem or local database test double | Keep the dependency behind the module; test with the stand-in. |
| Remote owned | Your own service across HTTP/gRPC/queue/process | Define a port at the seam; production adapter uses transport, tests use an in-memory adapter. |
| True external | Third-party service or uncontrolled system | Inject a port; tests use a mock or fake adapter focused on observable outcomes. |

## Method

1. Map current callers and what they must know.
2. Identify leakage: ordering, flags, branching, data shaping, retries, auth, cache invalidation, or error translation in callers.
3. Pick the seam where callers should stop knowing those details.
4. Design the smallest interface that can carry the behavior.
5. Move tests to the new interface when safe; delete old shallow tests after replacement coverage exists.

## Cautions

- Do not add ports/adapters for a single concrete implementation unless tests or real runtime variation justify it.
- Do not expose internal seams only because tests want them.
- Do not deepen unrelated modules while fixing a narrow bug.
- Keep migration incremental when many callers depend on the old shape.
