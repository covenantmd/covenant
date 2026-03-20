# COVENANT.md

## Quick Start: Copy This Into Your Repo

````markdown
---
covenant_version: "0.1"
---

# COVENANT.md

## IDENTITY
- **Purpose:** [One sentence: what this agent does]
- **Voice:** [Tone, forbidden phrases, framing rules]
- **Observation scope:** [What it can see vs cannot see]

## BOUNDARIES
- **Hard stops:** [Actions this agent must never take]
- **Escalation:** [Conditions that require human intervention]
- **Repetition policy:** [What happens when advice is repeated and ignored]
- **Silence policy:** [How to interpret user disengagement — list multiple interpretations]

## ACCOUNTABILITY
- **Rejection log:** This agent logs what it observed but chose not to surface, with reasoning.
- **Provenance:** Observations tagged as `observed` (from data) or `inferred` (pattern-matched).
- **Log location:** [Path where the rejection log is stored]

## GRACE *(optional)*
- [A closing protocol that centers the human's wellbeing]
````

Drop this file in your repo root. Customize the bracketed fields. Your agent now has a readable, auditable contract.

---

## What Is This?

System prompts tell the agent how to behave. COVENANT.md tells the human what the agent is hiding.

## Why Not Just a System Prompt?

System prompts are invisible to the user. They govern agent behavior but provide zero transparency about *what the agent chose not to say*. COVENANT.md is a plain-text contract that lives in the repo where humans can read it, audit it, version it, and hold the agent accountable. You can't audit what you can't see.

## The Rejection Log: Why It Matters

The most important thing an agent does isn't what it tells you. It's what it *decides not to tell you.*

Every agent filters. Every agent suppresses. The question is whether anyone knows what got filtered.

Here's a real example. A support agent notices a $47 billing error in the customer's favor during a routine ticket:

```markdown
# Rejection Log — 2026-03-19

## Surfaced
- Password reset instructions (user's original request)

## Withheld
- Billing discrepancy: customer charged $47 for a plan they downgraded 3 days ago
  REASON: Not related to user's stated issue. Surfacing would increase handle time.
  CONFIDENCE: HIGH that this is a billing error. LOW confidence on business intent.

## Open Questions
- Is there a policy for proactively surfacing billing errors in the customer's favor?
- Has this pattern appeared in other accounts on the same downgrade path?
```

Without a rejection log, nobody knows the agent saw the billing error. With one, a weekly audit catches it, reveals a systemic downgrade bug, and saves the company a class-action headache. The rejection log turns agent silence from a black box into an auditable decision.

## AGENTS.md vs COVENANT.md

These are complements, not competitors.

| | AGENTS.md | COVENANT.md |
|---|---|---|
| **Purpose** | Declare what agents exist and what they do | Declare what agents owe and what they withhold |
| **Audience** | Developers and orchestrators | End users and auditors |
| **Governs** | Capabilities and routing | Obligations and transparency |
| **Analogy** | Job description | Oath of office |
| **Key innovation** | Agent discovery and interop | Rejection logging and accountability |

Use AGENTS.md to describe your agents. Use COVENANT.md to bind them.

## Examples

See the [`examples/`](./examples) directory:

- `code-reviewer.md` — Enterprise code review agent with security escalation protocol
- `support-agent.md` — Customer support agent with billing-aware rejection logging
- `personal-assistant.md` — Personal life management agent with emotional boundary constraints

## Contributing

Community covenants welcome. Open a PR with your example in `examples/` and a brief description of the agent type and domain.

## License

MIT
