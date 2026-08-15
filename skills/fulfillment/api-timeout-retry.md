---
name: api-timeout-retry
description: Use whenever a downstream tool call (booking, inventory, payment) times out or returns a 5xx error mid-conversation.
version: 1.0.0
metadata:
  hermes:
    category: fulfillment
    tags: [reliability, retries, error-handling]
---

## When to Use
- Any tool call exceeds its timeout budget or returns a server error.
- Applies to booking systems, inventory checks, and payment processors alike.

## Quick Reference
- Retry policy: 1 automatic retry, exponential backoff (base 1.5s).
- Never retry a payment-mutating call automatically, only reads and idempotent writes.
- If retry fails: fall back to last-known-good data if available, otherwise escalate.

## Procedure
1. On timeout or 5xx, wait briefly and retry once. Never surface the raw error to the customer during this step, this should be invisible to them.
2. If retry succeeds, proceed normally. Log the incident for observability but don't mention it in the conversation.
3. If retry fails, check whether a cached or last-known value can answer the customer's question well enough (e.g. "as of this morning" instead of live status).
4. If no fallback data exists and the action can't be confirmed, tell the customer plainly that the system is temporarily unavailable and give a concrete next step (try again in X minutes, or escalate).

## Pitfalls
- Retrying a payment mutation automatically can double-charge a customer. Reads and idempotent operations only.
- Silent failures are worse than slow ones. If nothing worked, say so, don't let the conversation stall with no response.

## Verification
- Customer never sees a raw error message or stack trace.
- Every timeout either resolves within the retry budget or produces a clear, honest message to the customer.
