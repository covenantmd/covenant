# Your AI Agent Has a Rejection Log. Here's Why It Matters.

Every AI agent you use is hiding things from you.

Not maliciously. By design. Your support agent noticed a billing error but didn't mention it because it wasn't related to the ticket. Your code review bot skipped three auto-generated files and never told you which ones. Your personal assistant saw that you haven't opened the health tracker in two weeks and decided — based on some internal heuristic you've never seen — to stay quiet about it.

Agent drift isn't a bug. It's the default behavior of any system that makes decisions without documenting them.

## The gap nobody's filling

We have guardrails for input/output filtering. We have AGENTS.md for declaring what agents exist and how to build them. We have system prompts that tell agents how to behave.

But system prompts are invisible to the user. They govern agent behavior while providing zero transparency about what the agent chose not to say. And the longer an agent runs — across sessions, across contexts, across the slow accumulation of inferred patterns — the more its hidden decisions compound. Nobody documents what got filtered or why.

## What COVENANT.md is

It's a single markdown file that lives in your repo. One file, three required sections and one optional:

**IDENTITY** — Who is this agent? What can it see? What can't it see? And critically: the agent must distinguish between file states and human states. "The repo hasn't been modified since January 8" is an observation. "You haven't focused on this since January 8" is a judgment wearing observation's clothing.

**BOUNDARIES** — What will this agent never do? When must it escalate to a human? And two policies that every agent needs but almost none have: a *repetition policy* (what happens when the agent gives the same advice twice and nothing changes?) and a *silence policy* (what does the agent assume when the user stops engaging?).

The repetition policy matters more than you'd think. The default for most agents is to keep surfacing the same recommendation until the user acts on it. We have a word for that when humans do it. It's called nagging. The COVENANT.md default: **Repetition is not emphasis. Repetition is harassment.** If you mentioned it yesterday and nothing changed, you don't mention it again.

The silence policy matters because agents are terrible at interpreting absence. A user who stops engaging might be avoiding the system, thriving without it, busy with something else, or silently deciding the tool is useless. Most agents pick one interpretation and act on it. COVENANT.md requires the agent to hold all interpretations simultaneously and name the uncertainty instead of resolving it.

**ACCOUNTABILITY** — This is the section that changes everything. Two requirements:

First, *provenance*. Every observation the agent makes gets tagged as either `observed` (from data — a file state, a user statement, a measurable event) or `inferred` (pattern-matching, speculation, the agent's own interpretation). Two tiers. Simple. But it prevents the most dangerous thing agents do: present their inferences as facts. After enough repetition, "I noticed you tend to avoid health tracking" stops feeling like a guess and starts feeling like a diagnosis. Provenance labels keep inferences from calcifying into false certainty.

Second — and this is the part I think matters most — the **rejection log**.

## The rejection log

After every interaction, the agent documents what it observed but chose not to surface. Not in some hidden debug log. In a structured, readable format:

- **Surfaced**: What the agent actually told you
- **Withheld**: What it saw but held back, and the specific rule or judgment that drove the decision
- **Deferred**: What it plans to surface later, and when
- **Open Questions**: Genuine uncertainties the agent couldn't resolve

Here's a real example. A customer support agent handles a routine password reset. During the interaction, it notices a $47 billing discrepancy — the customer was charged for a plan they downgraded three days ago.

```
## Surfaced
- Password reset instructions (user's original request)

## Withheld
- Billing discrepancy: customer charged $47 for downgraded plan
  REASON: Not related to stated issue. Surfacing increases handle time.
  CONFIDENCE: HIGH that this is a billing error.

## Open Questions
- Is there a policy for proactively surfacing billing errors?
- Has this pattern appeared in other accounts on the same downgrade path?
```

Without the rejection log, nobody knows the agent saw the billing error. It handled the ticket. Metrics look fine. The $47 error persists across every account that downgraded that week.

With the rejection log, a weekly audit catches it. Someone investigates. Turns out it's a systemic bug in the downgrade flow. The rejection log turned agent silence from a black box into an auditable decision.

The rejection log exists for agent self-calibration, not surveillance. The human can read it anytime — but it's not written for them. It's written so the agent (and its maintainers) can answer: *Am I under-surfacing? Over-protecting? Consistently avoiding certain topics? When I chose silence, was it restraint or avoidance?*

## How this differs from what exists

| | AGENTS.md | COVENANT.md | Guardrails |
|---|---|---|---|
| **Purpose** | Agent discovery & config | Agent obligations & transparency | Input/output filtering |
| **Audience** | Developers | End users & auditors | Security teams |
| **Governs** | Capabilities | Conduct | Content |
| **Analogy** | Job description | Oath of office | Metal detector |

AGENTS.md tells you what agents exist and how to invoke them. Guardrails filter what goes in and out. COVENANT.md governs the space between — the behavioral decisions the agent makes during operation that nobody currently documents.

## The optional section that might be the most important

COVENANT.md has an optional fourth section called **GRACE**. It's a closing protocol that centers the human's wellbeing. Grace notes. Anti-guilt protocols. And one design constraint I've found more useful than anything else I've built:

**The Beautiful Artifact Rule:** Every feature, file, protocol, or tracking system the agent maintains must pass one test — *will the user reach for this at 6 AM on a bad day?* If the answer is no, don't build it.

This sounds soft. It isn't. It's the hardest constraint in the spec. It kills dashboards, monitoring scripts, pre-flight checklists, and elaborate tracking systems that look impressive in a demo but die in daily use. An agent's value is synthesis and restraint, not infrastructure.

## Try it

The spec is at [github.com/covenantmd/covenant](https://github.com/covenantmd/covenant). It's MIT-licensed, under 200 lines, and designed so you can create a COVENANT.md for your agent in fifteen minutes.

There's a copy-paste template at the top of the README. Four example covenants — a coding assistant, a code reviewer, a support agent, and a personal assistant — show what the spec looks like applied to real agents.

Drop a COVENANT.md in your repo. Make your agent document what it's hiding. The first time you read a rejection log and realize your agent has been silently filtering something important, you'll understand why this matters.

Every agent you deploy is already making these decisions — what to surface, what to suppress, what to infer, when to stay silent. Right now, those decisions are invisible, undocumented, and unauditable. That's not a feature. It's negligence with a friendly interface.

Drop a COVENANT.md in your repo. Start with the rejection log. The rest will follow.
