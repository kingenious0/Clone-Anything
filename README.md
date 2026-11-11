# 🧬 Clone Anything

**Give your AI agent a website or app, and it builds you a copy — then checks its own work to prove the copy actually matches the original.**

It works with any AI coding assistant: OpenCode, Claude Code, Cursor, Antigravity, and others.

---

## What does it do?

You know how you can say *"clone this website"* to an AI and it builds… something? The problem is, nobody checks if the copy actually looks like the real thing. The AI just says *"Done! It's perfect!"* — and sometimes it isn't.

This skill fixes that with one simple rule:

> **The AI must PROVE it looks right — by taking pictures of both, comparing them side by side, and showing you a score.**

No score, no "done."

## The 3 phases

Think of it like copying a drawing in art class:

**Phase 0 — Study the original**
First, look carefully at what you're copying. What colors does it use? What fonts? What pages does it have? Write it all down in a one-page plan.

**Phase 1 — Copy it, then check your work**

1. **Take "before" pictures** of the real site (every page, on computer and phone screen sizes)
2. **Build the copy** using the same colors and fonts you found
3. **Take "after" pictures** of your copy
4. **Compare the pictures** — the computer scores how close each page is, out of 100
5. **Fix the worst pages** and check again — until most pages score at least **85/100**

Then you get a **report card**, like this:

```
HOW CLOSE IS THE COPY?

  Home page     94/100  ⭐ Great
  Pricing page  87/100  ⭐ Great
  Checkout      82/100  👀 Almost — header is a bit off
  Profile       71/100  👀 Needs work — not enough reference photos

  Overall: 83.5 / 100
```

You always know exactly where the copy stands — no guessing, no fake "it looks perfect!"

**Phase 2 — Make it actually work (optional)**
The first copy runs on *fake sample data* (like a movie set — looks real, nothing behind the walls). If you want the copy to have real accounts, real data, or a real database, you say the word and it builds that too. **It never does this without asking you first.**

## What can you copy from?

| You give it… | It uses… |
|---|---|
| 🌐 A **website link** | The site's own colors, fonts, and pages |
| 📱 An **Android app file** (.apk) | The app's real color list and screens |
| 🖥️ A **computer program** (.exe) | The actual code inside (if it's the common kind) |
| 🖼️ **Screenshots** | The pictures you gave it |
| ✏️ Just **the app's name** | It searches the web for what the app looks like |

You can even combine them — more clues = a better copy.

## "Can you make it different too?"

Yes! That's **remix mode**:

1. First it builds an **exact copy** and saves it
2. *Then* it applies your changes — *"make it green," "call it Finlytics," "different logo"*
3. Every change is saved as its own step, so you can **undo any single one** and go back to the original look

The exact copy is always just one command away.

## What if something breaks or we take a break?

This skill comes with good habits built in, so the project never falls apart:

- **It always tests before saying "done"** — and shows you what it ran 🧪
- **It saves a checkpoint every time something works** 📸 — one bad edit can never ruin your project, you can always go back
- **It keeps a "where were we" note** 📝 — so tomorrow's session reads it and picks up right where it left off
- **It knows when to stop and think** 🧠 — if the same thing fails 3 times in a row, instead of trying random fixes, it rolls back to the last working version and asks you before continuing

## Honest limitations (no lying allowed)

- The **look and feel** gets copied (pages, buttons, colors, layout) — with **fake data** inside
- The **brain behind the site** (the real database, logins, payments) can't be read from a link — that's Phase 2, only if you ask
- Don't publish a copy with someone else's **logo or brand name** — the skill swaps in neutral placeholders to keep you out of trouble

## Install it (pick your assistant)

The whole skill is just one file: **[SKILL.md](./SKILL.md)**

| Your AI assistant | Put the file here |
|---|---|
| **OpenCode** | `~/.config/opencode/skills/clone-anything/` |
| **Claude Code** | `~/.claude/skills/clone-anything/` |
| **Cursor** | copy the text into `.cursor/rules/clone-anything.mdc` |
| **Antigravity / Codex** | paste it into your `AGENTS.md` file |
| **GitHub Copilot** | `.github/copilot-instructions.md` |
| **Windsurf** | `.windsurfrules` |

Not sure? If your AI can read a text file you point it at — it works.

## Try it (just say one of these)

- *"Clone this site: https://example.com"*
- *"Rebuild this app — here are screenshots"*
- *"Copy Stripe's homepage but make it purple"*
- *"(mid-project) It's broken again / where were we?"* ← the rescue habits kick in

## What you end up with

```
your-project/
├── CLONE-SPEC.md     ← the one-page plan
├── PLAN.md           ← the to-do list
├── PROGRESS.md       ← the "where were we" note
├── COVERAGE.md       ← which pages are done / not done
├── tokens.json       ← the colors & fonts we found
├── original/         ← pictures of the real thing
├── clone/            ← your copy
└── FIDELITY.md       ← the report card
```

## License

MIT — copy it, fork it, remix it. That's the whole point. 🧬
