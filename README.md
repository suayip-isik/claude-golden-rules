# Claude Code — Efficiency Guide

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Languages: EN / TR](https://img.shields.io/badge/Languages-EN%20%2F%20TR-blue.svg)](README.tr.md)

> A practical reference for developers on how to use Claude Code effectively — covering file type selection, cost optimization, security, error recovery, and team workflow.

---

## What Is This?

This guide consolidates the patterns, anti-patterns, and architectural decisions that matter most when working with Claude Code day-to-day. It is written for developers who are already familiar with the basics and want to get more out of their setup without wasting tokens or breaking things.

The guide covers six core file types (`CLAUDE.md`, `rules/`, `commands/`, `skills/`, `agents/`, `settings.json`) and seven operational topics that the official documentation leaves mostly to trial and error.

---

## Contents

| #   | Topic                                             |
| --- | ------------------------------------------------- |
| 1   | [CLAUDE.md — The Project Constitution](#)         |
| 2   | [Rules — Conditionally Loaded Instructions](#)    |
| 3   | [Commands — Manually Triggered Workflows](#)      |
| 4   | [Skills — On-Demand Expertise](#)                 |
| 5   | [Agents — Specialists in Separate Contexts](#)    |
| 6   | [settings.json — Permissions and Automations](#)  |
| 7   | [Security — Secrets and Permission Management](#) |
| 8   | [Error Recovery and Checkpoint Strategy](#)       |
| 9   | [Debugging and Troubleshooting](#)                |
| 10  | [Testing and Validation](#)                       |
| 11  | [Versioning and Team Workflow](#)                 |
| 12  | [Conflict Resolution Rules](#)                    |
| 13  | [Cost Optimization — Token = Money](#)            |
| 14  | [General Architecture Decisions](#)               |
| 15  | [Resources and References](#)                     |

---

## Quick Start

If you have five minutes, start with these three things:

**1. Keep CLAUDE.md under 200 lines.**
Every line loads with every message. A 2,000-line CLAUDE.md costs 10× more tokens than a 200-line one over a 50-message session — and instruction-following quality drops after ~150 rules anyway.

**2. Use path-scoped rules.**
CSS rules should not load while you're editing a TypeScript file. Add `paths:` frontmatter to every rule file.

```yaml
---
paths:
  - "src/styles/**/*.css"
---
```

**3. Add a basic deny list on day one.**
Before Claude touches anything dangerous, lock it down in `settings.json`:

```json
{
  "permissions": {
    "deny": [
      "Bash(rm -rf *)",
      "Bash(git push --force*)",
      "Bash(git reset --hard *)",
      "Bash(curl * | bash)"
    ]
  }
}
```

---

## File Type Decision Matrix

Not sure where something belongs? Use this tree:

```
"Is this relevant to EVERY task?"
├── Yes → CLAUDE.md
└── No  → "Is it specific to certain file types?"
    ├── Yes → rules/ (path-scoped)
    └── No  → "Will I trigger it manually?"
        ├── Yes → commands/
        └── No  → "Does it need a rich template or script?"
            ├── Yes → skills/
            └── No  → "Should it run in a separate context?"
                ├── Yes → agents/
                └── No  → rules/ (global)
```

---

## Cost at a Glance

| Strategy                                       | Estimated Savings |
| ---------------------------------------------- | ----------------- |
| `/clear` between tasks                         | 30–50%            |
| Reduce CLAUDE.md to 200 lines                  | 20–30%            |
| Write targeted prompts (`@file.ts:45-60`)      | 15–25%            |
| Choose the right model (Haiku / Sonnet / Opus) | 30–80%            |
| Plan Mode before coding                        | 20–40%            |
| Path-scoped rules                              | 10–20%            |
| Remove unused MCP servers                      | 5–30%             |

---

## Files in This Repository

```
├── CLAUDE.md                                  # Claude Code guidance for this repo
├── claude-code-efficiency-guide.md            # Full guide (English)
├── claude-code-verimli-kullanim-rehberi.md    # Full guide (Turkish)
├── README.md                                  # This file (English)
└── README.tr.md                               # Turkish README
```

---

## Who This Is For

- Developers already using Claude Code who want a leaner, faster setup
- Teams onboarding new members to a Claude Code workflow
- Anyone who has hit context limits, unexpected costs, or rule-following failures and wants to understand why

This guide does **not** cover general prompt engineering, Claude.ai web interface usage, or the Anthropic API directly.

---

## Contributing

Found something outdated, incorrect, or missing? Pull requests are welcome.

Claude Code evolves quickly — if a behavioral change in a new release makes part of this guide wrong, please open an issue or submit a correction.

---

## License

[MIT](LICENSE) — free to use, adapt, and share.

---

## Acknowledgements

- Shrivu Shankar (Anthropic engineer) — insight on the slash command anti-pattern
- Anthropic's official Claude Code documentation — the authoritative source this guide builds on
