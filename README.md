# DevChar

**The read-only AI coding agent. Ask anything. Nothing gets touched.**


Stop copy-pasting code into ChatGPT every time you have a question. DevChar adds a dedicated, stateless `/chat` command to your favorite AI agents (Claude Code, Cursor, Gemini CLI). It lets you interrogate your codebase in plain English, with full context awareness, and zero risk of the AI breaking your files.

---

## Why DevChar?

AI coding agents are built to *do things*—write code, run commands, edit files. That makes developers hesitate to ask exploratory questions like *"why does this exist?"* or *"what will break if I change this?"* because the agent might act on it. 

DevChar is the antidote. 

* **Absolute Transparency:** Read-only mode means zero risk. Ask anything; your files remain untouched.
* **Zero-Setup & Terminal Native:** No PTY wrappers, no Node.js scripts, no vector databases. Just native `grep` and `find` feeding precise context to your LLM.
* **Fixes Cursor's Container Amnesia:** Bypasses Cursor's dev-container indexing failures entirely by relying on native shell commands for file discovery.

---

## Install in 10 Seconds

```bash
# Global install (works across all projects)
mkdir -p ~/.claude/skills/devchar
curl -o ~/.claude/skills/devchar/SKILL.md https://raw.githubusercontent.com/yourusername/devchar/main/SKILL.md
curl -o ~/.claude/skills/devchar/references/context-rules.md https://raw.githubusercontent.com/yourusername/devchar/main/references/context-rules.md
curl -o ~/.claude/skills/devchar/references/examples.md https://raw.githubusercontent.com/yourusername/devchar/main/references/examples.md

# Project-specific install (only active in this project)
mkdir -p .claude/skills/devchar
# same curl commands with .claude/ instead of ~/.claude/
```

That's it. No npm install, no build step, no configuration.

---

## Commands

| Command | What it does |
|---|---|
| `/chat <question>` | Ask anything about your codebase |
| `/explain` | Explain the current file in plain English |
| `/why <thing>` | Explain why a piece of code exists |
| `/pattern` | Check if current file follows project conventions |
| `/chat --deep <question>` | Load full import graph before answering |
| `/chat --file <path> <question>` | Scope to a specific file or directory |
| `/chat --recent <question>` | Focus on files changed in the last 3 days |
| `/done` | Exit DevChar mode |

---

## How it works

DevChar uses a three-tier context injection system to load only what's relevant — not your entire codebase.

**Tier 1 (always):** Open files, recent git changes, project structure — ~2–4k tokens, always relevant.

**Tier 2 (smart):** Detects filenames, function names, and domain concepts in your question and loads only the matching files via `grep` and `find`. Bypasses the host agent's indexer entirely — works even when Cursor's indexer fails in dev containers.

**Tier 3 (explicit):** Full import graph loading, triggered only with `--deep`. You control when to go deep.

---

## Platform support

| Platform | Status | Notes |
|---|---|---|
| Claude Code | ✅ Full support | All tiers, native shell execution |
| Cursor | ✅ Full support | Tier 1+2 default, bypasses indexer |
| Gemini CLI | ✅ Full support | 1M token window, full file loading |
| VS Code + Claude | ✅ Full support | Same as Claude Code |
| Windsurf | ⚠️ Experimental | Tier 1 only; MCP bridge in roadmap |
| OpenAI Codex | 🔜 Coming | Testing in progress |

---

## Usage examples

```
/chat why is the login taking so long?
/explain src/hooks/useAuth.ts
/why do we have a separate ErrorBoundary on every route?
/pattern
/chat --deep how does the payment flow connect to the order service?
/chat --file src/auth/ what does the token refresh logic do?
```

---

## Roadmap

- [ ] MCP bridge for Windsurf compatibility
- [ ] `/chat --since <date>` to focus on recent changes
- [ ] Auto-detect platform and adjust context budget
- [ ] `/summary` command — generate a plain-English overview of any file
- [ ] Multi-file comparison mode
- [ ] Session export — save your Q&A session as a markdown file

---

## Contributing

DevChar is just markdown — contributing is easy.

- **New commands:** Add them to `SKILL.md` under the Commands section
- **Better context rules:** Edit `references/context-rules.md`
- **Example sessions:** Add to `references/examples.md`
- **Bug reports:** Open an issue with your platform, question, and what went wrong

See [CONTRIBUTING.md](CONTRIBUTING.md) for details.

---

## Why open source?

Because every developer has this problem. DevChar should be a community-built tool, not a product. If you find it useful, star it and share it.

---

## License

MIT — do whatever you want with it.
