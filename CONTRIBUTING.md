# 🤝 Contributing

Contributions welcome — the skill gets smarter every time someone actually uses it.

## Easiest ways to help

- 🐛 **Found it failing?** [Open an issue](https://github.com/kingenious0/clone-anything/issues) — include the URL/app you cloned, which agent you used (OpenCode, Claude Code, Cursor…), and what went wrong
- 💡 **Have an idea?** File it as an issue first — big changes are more fun when discussed before someone builds them
- 🌍 **Translated or tested it on a new agent?** Tell us how the install path went — we'll add it to `INSTALL.md`

## Sending a PR

1. Fork and branch: `git checkout -b fix/my-change`
2. Keep the repo's two-audience split intact:
   - **`README.md`** stays **kid-friendly** — no jargon; explain hard words or don't use them
   - **`SKILL.md`** stays **agent-precise** — exact steps, scores, commands
3. **Test your change for real:** run the skill against a small site (or use a sample from `examples/`) and confirm the flow still holds — a PR that changes behavior should say what you observed, with evidence
4. Open the PR and describe: what changed, why, and what you verified

## Project conventions

- Everything ships under [MIT](LICENSE)
- Fidelity claims need numbers — same rule for PRs as for the skill itself: **no score, no "done"**
- Respect the [Disclaimer](DISCLAIMER.md): contributions shouldn't enable impersonation or infringement
