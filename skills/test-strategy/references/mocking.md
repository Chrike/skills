# Mocking

Mocks isolate boundaries; they are not the behavior under test.

## Good Mock Targets

- External APIs and SDK clients.
- Email, payment, notifications, queues, cloud services.
- Time, randomness, network, filesystem, or process calls when the real boundary is slow or unreliable.
- Expensive service startup when the behavior under test is above it.

## Avoid

- Mocking your own internal modules by default.
- Asserting mock component existence, mock test IDs, or mock call counts as the main proof.
- Mocking before understanding the real side effects.
- Partial response mocks that omit fields the system may consume.
- Test-only methods in production classes.

## Gate

Before adding or keeping a mock, answer:

1. What real boundary does this replace?
2. What side effects does the real dependency have?
3. Does this test rely on those side effects?
4. Would a real integration-style test be simpler?

If the mock setup is larger than the behavior being tested, reconsider the seam.
