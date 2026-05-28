# Changelog

All notable changes to DevChar are documented here.

---

## [0.1.0] — Initial release

### Added
- Core `/chat` command with read-only Q&A mode
- `/explain` command for plain-English file explanation
- `/why` command for reasoning about code existence
- `/pattern` command for convention checking
- Three-tier context injection system (Tier 1: always, Tier 2: question-aware, Tier 3: explicit)
- `--deep`, `--file`, and `--recent` flags for Tier 3 expansion
- Platform support: Claude Code, Cursor, Gemini CLI, VS Code
- Experimental Windsurf support (Tier 1 only)
- Session memory across questions in the same conversation
- `references/context-rules.md` — detailed context loading decisions
- `references/examples.md` — 6 real example sessions
