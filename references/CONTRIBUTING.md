# Contributing to DevChar

DevChar is a markdown skill — contributing is much easier than a typical open source project. There's no build step, no test suite to run, and no dependencies to install.

---

## What you can contribute

### New commands
Add new slash commands to `SKILL.md` under the Commands table and write the corresponding instruction in the Behaviour section.

Good command ideas:
- `/deps` — explain the project's dependency tree
- `/summary <file>` — plain-English summary of any file
- `/compare <file1> <file2>` — explain the difference between two files
- `/test-coverage` — identify untested code paths

### Better context rules
If you find that DevChar loads the wrong context for a specific question type, improve the rules in `references/context-rules.md`. Add a new entry to the Question Type → Context Map.

### New example sessions
Add real examples to `references/examples.md`. The best examples show an edge case being handled well — uncertainty, a complex question, a session memory callback.

### Platform compatibility fixes
If you test DevChar on a platform and find it behaves differently, open an issue or PR with:
- Platform name and version
- What command you ran
- What DevChar did
- What it should have done

### README improvements
Better install instructions, more usage examples, clearer explanations.

---

## How to submit

1. Fork the repo
2. Make your changes
3. Test them — run DevChar with your changes in a real project
4. Open a PR with a short description of what you changed and why
5. That's it

---

## Good first issues

Look for issues labelled `good first issue`. These are small, well-scoped tasks that don't require deep knowledge of the codebase:
- Adding an example session
- Improving a command description
- Testing on a new platform and reporting results
- Fixing a typo or clarifying confusing documentation

---

## Code of conduct

Be direct, be kind, be useful. Focus on the work.
