# Condition-Based Waiting

Use when tests or scripts rely on guessed sleeps, arbitrary timeouts, or timing windows that pass sometimes and fail under load.

## Rule

Wait for the condition that proves progress, not for a guessed duration.

## Patterns

| Situation | Wait For |
| --- | --- |
| Async state | state becomes the expected value |
| Event stream | specific event appears |
| Queue or batch | count reaches expected size |
| File/process | file exists, process exits, port responds |
| UI | element is visible/enabled or network call completes |

## Minimal Helper Shape

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

Use the repo's existing test utilities when they exist.

## Legitimate Sleeps

A fixed sleep is acceptable only when testing real timing behavior such as debounce, throttle, retry backoff, or scheduled polling. First wait for the triggering condition, then document why that duration is meaningful.
