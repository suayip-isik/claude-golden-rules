# Plan: GitHub Public Release — International Open-Source Standards

## Context

The `claude-golden-rules` repo is a bilingual (English/Turkish) Claude Code efficiency guide being prepared for public release. Currently it has good content but is missing several standard files required for a well-maintained open-source project. Additionally there are broken filename references in both README files.

---

## Issues to Fix

### 1. Broken Filename References (README.md + README.tr.md)

Both READMEs reference `claude-altin-kurallar-ve-maliyet.md` in the "Files in This Repository" section, but the actual file is `claude-code-verimli-kullanim-rehberi.md`.

- `README.md` line 110: change `claude-altin-kurallar-ve-maliyet.md` → `claude-code-verimli-kullanim-rehberi.md`
- `README.tr.md` line 110: same fix

### 2. Create Missing Files

| File                                        | Notes                                                                                                      |
| ------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| `CODE_OF_CONDUCT.md`                        | Referenced in CONTRIBUTING.md line 39 — must exist. Use Contributor Covenant v2.1 (standard).              |
| `SECURITY.md`                               | Standard GitHub security policy. Since this is a docs-only repo, simply direct reporters to GitHub Issues. |
| `CHANGELOG.md`                              | Document initial v1.0.0 state. Follow Keep a Changelog format.                                             |
| `.gitignore`                                | Minimal: OS files (.DS_Store, Thumbs.db), editor files (.idea/, .vscode/).                                 |
| `.github/ISSUE_TEMPLATE/bug_report.md`      | Template for reporting errors/outdated content.                                                            |
| `.github/ISSUE_TEMPLATE/feature_request.md` | Template for suggesting new sections/content.                                                              |
| `.github/pull_request_template.md`          | PR checklist template.                                                                                     |

### 3. Add Badges to README.md

Add license and language badges at the top of README.md (after the title, before the blockquote description).

---

## Files to Modify

- `README.md` — fix filename reference + add badges
- `README.tr.md` — fix filename reference

## Files to Create

- `CODE_OF_CONDUCT.md`
- `SECURITY.md`
- `CHANGELOG.md`
- `.gitignore`
- `.github/ISSUE_TEMPLATE/bug_report.md`
- `.github/ISSUE_TEMPLATE/feature_request.md`
- `.github/pull_request_template.md`

---

## Implementation Details

### README.md badges (insert after `# Claude Code — Efficiency Guide` line, before `>`):

```markdown
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Languages: EN / TR](https://img.shields.io/badge/Languages-EN%20%2F%20TR-blue.svg)](README.tr.md)
```

### CODE_OF_CONDUCT.md

Use Contributor Covenant v2.1 standard template with contact placeholder (GitHub Issues link or repo URL for enforcement contact).

### SECURITY.md

Simple: state no executable code in repo, direct vulnerability/concern reports to GitHub Issues.

### CHANGELOG.md

Keep a Changelog format. One entry: `[1.0.0] - 2026-03-27` with initial release description.

### .gitignore

Cover: `.DS_Store`, `Thumbs.db`, `.idea/`, `.vscode/`, `*.swp`, `*.swo`, `*~`

### .github/ISSUE_TEMPLATE/bug_report.md

Fields: Description, Expected content, Actual content, Guide section affected, Language (EN/TR).

### .github/ISSUE_TEMPLATE/feature_request.md

Fields: What gap does this fill, Proposed content/section, Language (EN/TR/Both).

### .github/pull_request_template.md

Checklist: describes changes, updates both EN+TR if applicable, follows style guidelines.

---

## Verification

1. Confirm `CODE_OF_CONDUCT.md` exists and link in CONTRIBUTING.md resolves
2. Confirm README.md "Files in This Repository" section shows correct Turkish guide filename
3. Confirm README.md badges render correctly on GitHub
4. Confirm `.github/` templates appear when creating new issues/PRs on GitHub
5. Run: `grep -r "claude-altin-kurallar" .` — should return no results after fix
