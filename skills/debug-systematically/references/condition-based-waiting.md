# Condition-Based Waiting

Use when diagnosis, tests, or scripts rely on guessed sleeps, arbitrary timeouts, or timing windows that pass sometimes and fail under load.

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

## Debugging Guidance

- Prefer existing project or framework wait utilities.
- If no helper exists, use a small polling loop with a deadline, a clear condition description, and a useful timeout error.
- Log the last observed state or last error when it helps diagnose why the condition never became true.
- Remove temporary polling or diagnostic code unless it becomes an intentional test utility.

## Legitimate Sleeps

A fixed sleep is acceptable only when testing real timing behavior such as debounce, throttle, retry backoff, or scheduled polling. First wait for the triggering condition, then document why that duration is meaningful.
