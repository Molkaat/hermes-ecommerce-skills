---
name: billing-dispute-escalation
description: Use when a customer reports a duplicate charge, incorrect amount, or unauthorized transaction.
version: 1.0.0
metadata:
  hermes:
    category: support
    tags: [billing, escalation, human-handoff]
---

## When to Use
- Any mention of duplicate charges, wrong amounts, or a transaction the customer doesn't recognize.
- This is always out of automated scope. Do not attempt to resolve it directly.

## Quick Reference
- Tool: `slack.notify(channel="#billing-escalations", priority)`
- Tool: `crm.tag_conversation(tag="billing_dispute")`
- SLA to communicate to customer: response within a few hours during business hours.

## Procedure
1. Acknowledge the issue specifically, repeat back the amount and rough date if the customer gave one, so they know they were heard correctly.
2. Do not attempt a refund, credit, or explanation of the charge. Billing disputes require account access this agent doesn't have.
3. Tag the conversation and notify the billing channel with full context: customer ID, order ID if available, amount in question, and the customer's own description of the issue.
4. Set expectations clearly with a real timeframe, not "someone will be in touch."

## Pitfalls
- Do not speculate about why the duplicate charge happened (processor glitch, retry logic, etc.), even if it seems obvious. Wrong guesses here erode trust fast.
- Do not close the conversation as resolved. Mark it escalated and pending.

## Verification
- Escalation tagged and routed within the same turn, not deferred to a follow-up message.
- Customer received a specific timeframe, not a vague reassurance.
