# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.1.0] - 2026-03-28

### Changed

- **CLAUDE.md size recommendation**: Corrected "~500 lines upper limit" claim to match the official recommendation of "under 200 lines". The 500-line figure was not sourced from current official documentation.
- **Installation command**: Updated from `npm install -g @anthropic-ai/claude-code` to the official `curl -fsSL https://claude.ai/install.sh | bash` installer.
- **`CLAUDE_AUTOCOMPACT_PCT_OVERRIDE` and `MAX_THINKING_TOKENS`**: Marked as undocumented/unofficial env variables not present in official docs.
- **`/undo` and `/context`**: Marked as not listed among official bundled skills; availability may vary across versions.

### Added

- **`@import` syntax in CLAUDE.md**: Document the `@path/to/file` import feature with 5-hop recursion limit (both EN and TR).
- **Subagent nesting prohibition**: "Agents cannot spawn other agents" added to Section 5 Common Mistakes (both EN and TR).
- **Skill description 250-character cap**: Hard limit documented in Section 4 Golden Rules (both EN and TR).
- **Full frontmatter field tables**: Added reference tables for all agent and skill frontmatter fields (`background`, `effort`, `isolation`, `maxTurns`, `initialPrompt`, `permissionMode`, `memory`, `disallowedTools`, `mcpServers`, `hooks`, `skills`, `context: fork`, `user-invocable`, `argument-hint`, etc.).
- **`.claude/CLAUDE.md` location**: Added as a valid alternative to root-level `CLAUDE.md` in the Versioning section.
- **`$ARGUMENTS[N]` / `$N` indexed argument syntax**: Added to Section 3 Commands.
- **`user-invocable: false`**: Documented in both Section 3 (Commands) and Section 4 (Skills) frontmatter tables.

## [1.0.0] - 2026-03-27

### Added

- Initial public release of the Claude Code efficiency guide (English and Turkish).
- Contributing guidelines and repository licensing.
- Standard open-source project governance and collaboration templates.

[Unreleased]: https://github.com/suayip-isik/claude-golden-rules/compare/v1.1.0...HEAD
[1.1.0]: https://github.com/suayip-isik/claude-golden-rules/compare/v1.0.0...v1.1.0
[1.0.0]: https://github.com/suayip-isik/claude-golden-rules/releases/tag/v1.0.0
