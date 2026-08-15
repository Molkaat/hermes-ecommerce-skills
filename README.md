# E-Commerce Support Skill Library (Hermes Agent)

A small, production-style skill library for running a customer-facing support agent on [Hermes Agent](https://github.com/NousResearch/hermes-agent), built around one real question: what does an agent do when things don't go smoothly, not just when they do.

This isn't a "hello world" skill dump. Every skill here encodes a judgment call that a support agent has to make in production: when to guess vs. when to ask, when to retry vs. when to say something failed, and when to hand off to a human instead of pretending it can help.

## Why skills, not just tools

Tools give an agent hands. Skills give it judgment. A tool can call the Shopify API, but it has no opinion on whether to trust a stale timestamp, whether to ask a clarifying question first, or when a request has crossed into "call a human" territory. That judgment lives here, in the skill files, not in the tool code.

## Structure

```
skills/
  support/
    order-status-lookup.md            # core happy-path support flow
    ambiguous-request-clarification.md # what to do when intent is unclear
    billing-dispute-escalation.md     # when to hand off, not resolve
  fulfillment/
    api-timeout-retry.md              # what happens when a backend call fails
```

Two categories on purpose: `support` skills are conversation-facing judgment calls, `fulfillment` skills are about reliability when the systems underneath the conversation misbehave. Keeping them separate makes it easy to find the right one fast, which matters because Hermes loads a compact catalog first and only pulls the full skill body when something actually matches.

## Design choices worth calling out

**Every skill has a "Pitfalls" section based on what actually goes wrong**, not hypothetical edge cases. Multiple orders under one email. Retrying a payment mutation and double-charging someone. Guessing at why a duplicate charge happened instead of just escalating it. These are the mistakes that erode trust fastest in a live deployment, so they're written down rather than left to be discovered the hard way.

**Nothing here auto-resolves billing disputes or irreversible actions.** The `billing-dispute-escalation` skill is deliberately narrow: acknowledge, tag, route, set an honest timeframe, don't attempt to explain or fix it. An agent that tries to be helpful about something it has no authority over is worse than one that hands off cleanly.

**Retries are invisible to the customer, failures are not.** `api-timeout-retry` treats one retry as normal plumbing the customer should never see, but treats a failed retry as something that deserves an honest, specific message rather than silence or a stall.

## Multi-client deployment note

If you're running this for more than one store or client, don't fork these skill files per client. Isolate at the Hermes profile level instead, separate config, memory, and credentials per client, sharing this same skill library across all of them. The judgment calls in these files are general; what should stay per-client is data, not procedure.

## Status

Written as a reference implementation and portfolio piece. Not tied to a specific live deployment. Feedback and PRs on edge cases I've missed are welcome.

---
Built by Molka Trabelsi — AI integration, RAG systems, and voice/chat agents.
