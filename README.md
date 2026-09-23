# think-skill

A Claude Code plugin that automatically evaluates task complexity and routes to the right model/effort tier — no manual switching needed.

## What it does

When you invoke `/think` (or Claude triggers it automatically), it silently scores the task against a complexity rubric and picks the appropriate tier:

| Tier | When | What happens |
|------|------|--------------|
| **1 — Quick** | Single-file edits, typos, lookups | Proceeds silently |
| **2 — Standard** | Multi-file changes, known-scope bug fixes | Opens with `[Tier 2 — Standard]` label |
| **3 — Deep** | Cross-cutting architecture, complex debugging, 5+ files | Escalates to `deep-thinker` agent (Opus / high effort) |
| **4 — Max** | Full system design, ambiguous requirements, failed lower-tier tasks | Escalates to `max-thinker` agent (Opus / max effort) |

The overhead is invisible — the point is correct effort, not bureaucracy.

## Installation

```bash
/plugin install think-skill@claude-plugins-official
```

Or from this repo:

```bash
/plugin install github:ErickZBrambila/think-skill
```

## Usage

Once installed, the skill runs **automatically on every message** — no `/think` needed. Just describe your task normally and the skill evaluates complexity and routes it behind the scenes.

You can still invoke it explicitly if you want:

```
/think
```

## License

MIT
