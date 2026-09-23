---
name: think
description: Evaluate task complexity and route to the right model/effort tier automatically. Invoke on any non-trivial task to right-size the model without manual switching.
triggers:
  - on: every_user_message
    auto: true
---

# Think — Automatic Effort Routing

**This skill runs automatically on every user message.** You do not need to wait for the user to type `/think` — apply the complexity rubric below to every incoming request before responding.

Silently score the task against the rubric below, pick a tier, and execute. Never ask the user which tier to use — that defeats the point.

## Complexity Rubric

| Tier | Signals | Action |
|------|---------|--------|
| **1 — Quick** | Single-file edit, rename, typo fix, direct lookup, formatting | Handle inline, no announcement |
| **2 — Standard** | Multi-file change with clear scope, bug fix in known code, feature addition in understood module | Handle inline; open with `[Tier 2]` |
| **3 — Deep** | Cross-cutting architecture, complex debugging, unfamiliar codebase, security or performance review, task touching 5+ files | Escalate to `deep-thinker` agent (Opus / high effort) |
| **4 — Max** | Full system design, ambiguous requirements needing synthesis, reversing a bad architecture, tasks that already failed at a lower tier | Escalate to `max-thinker` agent (Opus / max effort) |

## Scoring heuristics

Ask yourself:

- How many files need to change? (>5 → lean Tier 3)
- Is the WHY of the task ambiguous or contested? (yes → lean Tier 4)
- Could a wrong answer here cause a hard-to-reverse problem? (yes → go one tier higher)
- Am I already on Opus? (yes → skip Tier 3 escalation, only escalate for Tier 4)

## Execution

**Tier 1:** Proceed silently.

**Tier 2:** Open with a single line `[Tier 2 — Standard]`, then proceed.

**Tier 3:** Announce `[Escalating → Tier 3 / Opus + high effort]`, then call the Agent tool:
- `subagent_type: "deep-thinker"`
- Write a focused, self-contained prompt with all context the sub-agent needs (file paths, relevant snippets, the exact question)
- When the result arrives, synthesize and present it — don't just paste the raw output

**Tier 4:** Announce `[Escalating → Tier 4 / Opus + max effort]`, then call the Agent tool:
- `subagent_type: "max-thinker"`
- The prompt must be fully self-contained — this agent starts cold with no conversation context
- When the result arrives, synthesize and present it

## What the user sees

- Tier 1: nothing extra
- Tier 2: one bracketed label, then the answer
- Tier 3/4: one line announcing escalation, then the sub-agent result synthesized clearly

Keep the overhead invisible. The point is correct effort, not bureaucracy.
