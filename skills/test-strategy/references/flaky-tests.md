# Flaky Tests

Use when tests pass sometimes, fail under load, depend on timing, or contain guessed sleeps.

## Rule

Wait for the condition that proves progress, not a guessed duration.

## Replace Sleeps With Conditions

| Waiting For | Better Signal |
| --- | --- |
| Async state | state equals expected value |
| Event | event appears in stream |
| Queue/batch | count reaches expected size |
| File/process | file exists, process exits, port responds |
| UI | element visible/enabled or network call complete |

## Helper Shape

```ts
async function waitFor<T>(
  check: () => T | null | undefined | Promise<T | null | undefined>,
  description: string,
  timeoutMs = 5000
): Promise<T> {
  const start = Date.now();
  let lastError: unknown;
  while (Date.now() - start < timeoutMs) {
    try {
      const value = await check();
      if (value !== null && value !== undefined) return value;
    } catch (error) {
      lastError = error;
    }
    await new Promise(resolve => setTimeout(resolve, 25));
  }
  throw new Error(
    `Timed out waiting for ${description}` +
      (lastError ? `; last error: ${String(lastError)}` : "")
  );
}
```

Use existing framework helpers first, such as Playwright assertions, Testing Library `findBy*`, or repo-specific wait utilities.

Fixed sleeps are acceptable only when testing real timing behavior such as debounce, throttle, retry backoff, or polling intervals. First wait for the triggering condition, then document why the duration matters.
