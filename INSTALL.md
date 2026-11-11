# 📦 Install Clone Anything

The whole skill is one file: [`SKILL.md`](../SKILL.md). Pick your agent below.

---

## OpenCode

Skills live in `~/.config/opencode/skills/<name>/SKILL.md`
(Windows: `%USERPROFILE%\.config\opencode\skills\`)

```bash
git clone https://github.com/kingenious0/clone-anything.git
mkdir -p ~/.config/opencode/skills/clone-anything
cp clone-anything/SKILL.md ~/.config/opencode/skills/clone-anything/
```

Restart OpenCode — the skill triggers on phrases like *"clone this site"* or *"rebuild this app"*.

## Claude Code

```bash
mkdir -p ~/.claude/skills/clone-anything
cp clone-anything/SKILL.md ~/.claude/skills/clone-anything/
```

Project-level instead? Use `.claude/skills/clone-anything/SKILL.md` inside the repo.

## Cursor

```bash
mkdir -p .cursor/rules
cp clone-anything/SKILL.md .cursor/rules/clone-anything.md
```

Or paste the content into an existing rule file. Rules apply automatically per project.

## Antigravity, Codex, and any `AGENTS.md` agent

Paste the **contents** of `SKILL.md` into your project's `AGENTS.md` (or point your
agent at the file in its instructions). If your tool has its own skill/command folder,
drop the file there — same content everywhere.

## GitHub Copilot

```bash
mkdir -p .github
cp clone-anything/SKILL.md .github/copilot-instructions.md
```

(Or merge it into an existing `copilot-instructions.md`.)

## Windsurf

Paste the contents into `.windsurfrules` at your project root.

## Aider

```bash
aider --read clone-anything/SKILL.md
```

Or add `clone-anything/SKILL.md` to `CONVENTIONS.md`.

---

## Not listed?

If your AI assistant can read a text file you point it at, it works. Give it the
`SKILL.md` contents as standing instructions.

## Requirements

- Any AI coding agent that reads instructions
- **Git** — the checkpoint protocol is load-bearing (no git = no safety net)
- **A way to screenshot web pages** — Playwright, a built-in browser tool, or an MCP browser.
  No screenshot ability at all? The skill degrades honestly: it substitutes
  DOM/style comparison for pixel diff and says so in the report card.
