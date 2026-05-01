# Human Automatica Skills

Claude Code slash commands from [Human Automatica](https://humanautomatica.com). Copy any `.md` file into your project's `.claude/commands/` folder and invoke it as a slash command.

MIT License. Free to use.

---

## AI Artifact Skills

Four skills for working safely with AI instruction artifacts — system prompts, agent instructions, tool descriptions, and any other operational AI text where the words have behavioral consequences.

| Skill | What it does |
|---|---|
| [`/ha-compress`](compress/ha-compress.md) | Reduce an artifact to a target character limit without weakening its behavior |
| [`/ha-evaluate`](evaluate/ha-evaluate.md) | Assess an artifact for quality, gaps, and how much it can safely be compressed |
| [`/ha-compare`](compare/ha-compare.md) | Rank two or more compressed candidates against the original |
| [`/ha-repair`](repair/ha-repair.md) | Restore a lossy or unsafe compression using the original as reference |

These four skills are designed to work together. Each one surfaces next-step recommendations pointing you to whichever skill fits what comes next.

---

### The problem they solve

AI instruction artifacts aren't regular text — they're behavioral contracts. A system prompt that tells an agent what it can and can't do, what steps to follow, and where its boundaries are is doing real work. Shortening it the wrong way removes safeguards, drops required steps, or introduces drift you won't notice until the agent does something unexpected.

Most compression tools treat this like any other editing task. These skills don't.

---

### How they work together

A typical workflow:

```
/ha-evaluate [artifact]          → understand headroom and gaps first
/ha-compress [artifact] to 6000  → compress with known constraints
/ha-compare [A] vs [B] against [original]  → pick the safer candidate
/ha-repair [compressed] against [original] → restore anything lost
```

Each skill ends with a **Next Steps** block telling you when to reach for another one. You don't need to know the full workflow upfront — the skills guide you through it.

---

### The classification model

Every skill uses the same internal classification model to decide what can be safely removed:

| Class | What it is | What happens to it |
|---|---|---|
| **FOUNDATION** | Core identity, purpose, boundaries | Never removed |
| **STRICT** | Non-negotiable rules, prohibitions, safeguards | Never weakened |
| **REQUIRED** | Execution-critical behaviors and steps | Only shortened if full function remains intact |
| **FLEXIBLE** | Style, formatting, examples, optional phrasing | Removed first |
| **EXTERNALLY-OWNED** | Boilerplate whose authoritative definition lives outside the artifact | Safe to remove when it doesn't affect execution |

This ordering is enforced. FLEXIBLE goes first. FOUNDATION and STRICT are last resorts.

---

### The output — what makes these different

Every result includes a structured audit report you can act on:

| Field | What it tells you |
|---|---|
| **Fit Status** | Fits / Does Not Fit / Near Fit / Cannot Safely Compress |
| **What Was Preserved** | The behaviors, safeguards, and constraints that survived intact |
| **What Changed** | Minor wording or structure changes |
| **What Was Intentionally Reduced** | Safe removals — examples, repetition, duplication |
| **What Was Lost** | Any meaningful loss, or "No material loss identified" |
| **Drift Risk** | Low / Medium / High — how much behavioral uncertainty was introduced |
| **Final Verdict** | Safe / Unsafe |

The skills will not claim a result is safe if it isn't. If the target limit can't be reached without material behavioral damage, `/ha-compress` issues a **Cannot Safely Compress** verdict, states the minimum safe character count, and tells you exactly what's blocking further reduction.

---

### When to use which skill

**Start with `/ha-evaluate`** if you're not sure how much compression is possible, or if you want to check an existing compression for drift.

**Use `/ha-compress`** when you have a target limit and want the safest reduction possible.

**Use `/ha-compare`** when you've produced multiple compressed versions and need to know which one to trust.

**Use `/ha-repair`** when a compression produced an Unsafe verdict, or when you know something critical got dropped.

---

## License

MIT © 2026 Brad Flowers
