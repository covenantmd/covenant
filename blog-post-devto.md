---
title: "Your AI Agent Has a Rejection Log. Here's Why It Matters."
published: false
description: "COVENANT.md is an open standard for AI agent behavioral governance — who the agent is, what it owes, and what it's hiding from you."
tags: ai, opensource, agentai, llm
canonical_url: https://github.com/covenantmd/covenant
---

Every AI agent you use is hiding things from you.

Not maliciously. It's just how they work. Your support agent noticed a billing error but didn't mention it because it wasn't related to the ticket. Your code review bot skipped three auto-generated files and never told you which ones.

Agent drift isn't a bug. It's what happens when a system makes decisions and nobody writes them down.

## The gap

We've got guardrails for input/output filtering. We've got AGENTS.md for agent configuration. We've got system prompts for behavior. But system prompts are invisible to the user — there's zero transparency about what the agent chose not to say.

## COVENANT.md

It's a single markdown file in your repo. Three required sections:

**IDENTITY** — Who is this agent? What can it see? What can't it see? And here's the important part: "The repo hasn't been modified since January 8" is an observation. "You haven't focused on this since January 8" is a judgment wearing observation's clothing. Agents need to know the difference.

**BOUNDARIES** — Hard stops, escalation triggers, and two policies almost no agent has: a *repetition policy* and a *silence policy*. The repetition default: **if you mentioned it yesterday and nothing changed, don't mention it again.** For silence: when a user stops engaging, hold multiple interpretations. Don't just pick one.

**ACCOUNTABILITY** — Every observation tagged as `observed` (from data) or `inferred` (speculation). And the thing I think matters most: the **rejection log**.

## The rejection log

After every interaction, the agent logs what it observed but chose not to surface — and cites the rule that drove the decision.

Say a support agent handles a password reset. It notices a $47 billing error:

```
## Withheld
- Billing discrepancy: customer charged $47 for downgraded plan
  REASON: Not related to stated issue.
  CONFIDENCE: HIGH that this is a billing error.

## Open Questions
- Is there a policy for proactively surfacing billing errors?
```

Without the log, nobody knows. With it, a weekly audit catches a systemic downgrade bug.

## How it fits

| | AGENTS.md | COVENANT.md | Guardrails |
|---|---|---|---|
| **Governs** | Capabilities | Conduct | Content |
| **Audience** | Developers | End users & auditors | Security teams |
| **Analogy** | Job description | Oath of office | Metal detector |

## Try it

[github.com/covenantmd/covenant](https://github.com/covenantmd/covenant) — it's under 200 lines, MIT licensed, four example covenants, and there's a copy-paste template in the README.

Every agent you deploy is already deciding what to surface and what to suppress. Right now those decisions are invisible. That's not a feature — it's just negligence with a friendly interface.

Start with the rejection log. The rest follows.
