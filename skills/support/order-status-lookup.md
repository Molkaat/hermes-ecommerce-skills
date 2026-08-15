---
name: order-status-lookup
description: Use when a customer asks about the status, tracking, or delivery estimate of an existing order.
version: 1.0.0
metadata:
  hermes:
    category: support
    tags: [orders, tracking, shopify]
---

## When to Use
- Customer asks "where is my order", "when will it arrive", or gives an order number unprompted.
- Customer references a delivery date that has already passed.

## Quick Reference
- Tool: `shopify.get_order(order_id | email)`
- Tool: `carrier.track(tracking_number)`
- Timeout budget: 3s per call, 1 retry on failure.

## Procedure
1. Resolve the order. If the customer gave an order number, look it up directly. If they only gave an email, search recent orders for that email and confirm the right one before proceeding.
2. Pull current status from Shopify. If status is "shipped," fetch live tracking from the carrier tool.
3. If tracking shows no movement in 48+ hours, do not guess a cause. Say delivery may be delayed and offer to escalate if it doesn't move in 24 more hours.
4. Always give a specific next step, not just a status. "It's in transit, expected Thursday" beats "it's shipped."

## Pitfalls
- Carrier API timeouts are common during peak hours. Retry once, then fall back to Shopify's last-known status rather than leaving the customer without an answer.
- Do not confirm a delivery date the carrier hasn't confirmed. Offer a range if the estimate is soft.
- Multiple orders under one email is common. Never assume the most recent order is the one being asked about.

## Verification
- Response includes: order status, a real timestamp or date, and one concrete next step.
