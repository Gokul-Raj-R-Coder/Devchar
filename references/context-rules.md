# DevChar Context Rules — Reference

This file is loaded by SKILL.md when Tier 2 or Tier 3 context injection runs. It provides detailed decision rules for what to load in ambiguous cases.

---

## Question Type → Context Map

### "How does X work?"
**Load:** The file(s) implementing X + any files that call it (consumers).
**Avoid:** Test files, migration files, config files unless X is explicitly about configuration.
**Example:** "How does the authentication middleware work?" → load `middleware/auth.ts`, then grep for files that import it.

---

### "Why does X exist?"
**Load:** Git history for that file (`git log --follow -p <file> | head -60`), plus the file itself.
**Also check:** Related issues or PR descriptions if a `.github/` directory exists.
**Example:** "Why do we have a custom logger instead of console.log?" → load the logger file + its git log.

---

### "Is this the right way to do X?"
**Load:** 3 other examples of similar patterns in the codebase to compare against.
**How:** `grep -r "<pattern>" --include="*.ts" -l | head -3 | xargs cat`
**Example:** "Is this the right way to handle errors?" → find 3 other error handlers in the codebase, compare patterns.

---

### "What's the difference between X and Y?"
**Load:** Both X and Y in full.
**Example:** "What's the difference between UserService and AuthService?" → load both files completely.

---

### "What will break if I change X?"
**Load:** X itself + everything that imports X (consumers) + any tests for X.
**How:**
```bash
# Find all consumers
grep -r "import.*X\|require.*X" --include="*.ts" --include="*.js" -l | head -10
# Find tests
find . -name "*.test.*" -o -name "*.spec.*" | xargs grep -l "X" 2>/dev/null | head -5
```

---

### "What does this error mean?"
**Load:** The file where the error is thrown + the call stack files if identifiable from the error message.
**Also:** `grep -r "<error message>" --include="*.ts" --include="*.js" -l | head -3`

---

### Architecture / "How is the project structured?"
**Load:** Only the directory tree (Tier 1 already covers this). Don't load individual files.
**Add:** README.md if it exists (`cat README.md | head -80`).

---

## File Size Handling

| File size | Strategy |
|---|---|
| < 100 lines | Load in full |
| 100–300 lines | Load in full on Claude Code / Gemini CLI. On Cursor: `head -60` + `tail -30` |
| 300–800 lines | `head -80` + `tail -40` + grep for the relevant function/class |
| > 800 lines | grep for the specific symbol only. Offer to load more if needed. |

---

## What to Never Load

- `node_modules/` — never
- `.git/` — only `git log` and `git diff` commands, never raw object files
- `dist/` or `build/` — compiled output, not useful
- `*.lock` files — `package-lock.json`, `yarn.lock`, `Cargo.lock`
- Binary files — images, compiled binaries, fonts
- `.env` files — never load environment files, even partially

---

## Token Counting Heuristics

Use these rough estimates to stay within platform budgets:

| Content | Approx tokens |
|---|---|
| 1 line of code | ~5–10 tokens |
| 100-line file | ~500–800 tokens |
| 300-line file | ~1,500–2,500 tokens |
| Full `git diff` (small PR) | ~1,000–3,000 tokens |
| Directory tree (40 files) | ~300–500 tokens |
| package.json | ~200–600 tokens |

**Platform budgets:**
- Cursor: keep total loaded context under 40,000 tokens
- Claude Code: keep under 80,000 tokens
- Gemini CLI: keep under 150,000 tokens (degradation threshold)
