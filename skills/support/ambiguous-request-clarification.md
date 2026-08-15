---
name: ambiguous-request-clarification
description: Use when a customer's request has more than one reasonable interpretation, especially around dates, sizes, or product variants.
version: 1.0.0
metadata:
  hermes:
    category: support
    tags: [clarification, intent, confidence]
---

## When to Use
- Confidence score on intent classification is below 0.6.
- The request contains relative time language ("next week," "the other one," "like I mentioned before") without enough context to resolve it.

## Quick Reference
- Confidence threshold for auto-clarifying: < 0.6
- Never guess and act on anything involving payment, cancellation, or shipping address changes.

## Procedure
1. Identify exactly which part of the request is ambiguous. Don't ask a vague clarifying question, ask about the specific gap.
2. Offer the two (at most three) most likely interpretations as concrete options, don't make the customer restate the whole request from scratch.
3. Wait for confirmation before taking any action that can't be easily undone (cancellations, address changes, refunds).
4. If the customer's follow-up is still ambiguous after one clarification round, escalate to a human rather than guessing twice.

## Pitfalls
- Asking an open-ended "could you clarify?" is worse than no clarification at all, it shifts the cognitive load back onto the customer. Always propose the likely options.
- Don't clarify low-stakes ambiguity (e.g. tone of a message). Reserve this skill for anything with a real cost if guessed wrong.

## Verification
- Zero irreversible actions taken on an unconfirmed interpretation.
- Customer's second message directly answers the clarifying question, rather than re-explaining the whole request (sign the question was well targeted).
