---
name: clone-anything
description: "Clone any app or website from a URL, screenshots, screen recording, app name, APK, or desktop EXE — and PROVE the clone's fidelity instead of claiming it. Self-contained super-skill: Phase 1 extracts ground truth (design tokens from CSS or APK colors.xml, route inventory, Electron asar source), builds a pixel-faithful surface clone on mock data, then runs a closed loop — screenshot original vs clone at matched viewports, SSIM/pixel-diff per route, auto-iterate on the worst routes until a similarity bar is met — and delivers a fidelity report card + screen coverage matrix. Includes a full build-discipline protocol (verify-don't-claim, green-state git checkpoints, PROGRESS.md handoff ledger, reproduce-first debugging, thrash detection with rollback) so multi-session builds don't collapse. Remix mode records 'but change X' edits as revertible overrides. Phase 2 (opt-in) rebuilds the backend. Use when the user says 'clone this site', 'rebuild this app', 'make me a copy of X', gives a URL/app name/APK/EXE/screenshot wanting a replica, reskins an existing product, or when a build starts spiraling mid-clone."
license: MIT
compatibility: universal
metadata:
  source: clone-anything
  category: build-protocol
  agents: opencode, claude-code, cursor, antigravity, codex, copilot, windsurf, aider
---

> **Agent-agnostic:** this skill assumes no particular harness. Use whatever tooling your agent actually has — built-in browser/screenshot tools, Playwright/Puppeteer scripts, or an MCP browser for ground-truth captures and clone diffs; any SSIM/pixelmatch implementation (Node, Python, or ImageMagick compare) for scoring; and your agent's native file/terminal/git tools for the build loop. **If a capability is missing, degrade honestly:** tell the user which verification step you cannot run and substitute the closest evidence (e.g., DOM/style comparison instead of pixel diff) — never skip verification silently.

# Clone Anything

The user gives you something — a URL, an app name, screenshots, an APK, an EXE — and wants "the same thing, but <optionally: different>". Existing tools do a single blind generation and call it done. This skill makes two promises and keeps both:

> **Promise 1 — Fidelity is measured, not claimed.** Ground truth is captured from the original; the clone is diffed against it route by route; the deliverable is a report card, not an opinion.
>
> **Promise 2 — The build doesn't collapse.** A discipline protocol (verify-don't-claim, green-state checkpoints, handoff ledger, thrash detection) keeps a multi-session clone from becoming a pile of half-working screens.

```
PHASE 0  INTAKE      → identify source(s), triage feasibility, write CLONE-SPEC.md
PHASE 1  SURFACE     → extract → build → diff → iterate → report card   (THE CORE)
PHASE 2  BACKEND     → opt-in: rebuild real data/auth/API behind the clone
         (Build Discipline runs through ALL phases)
```

**The scope contract (state it upfront, every time):** Phase 1 clones the *surface* — UI, design, screens, flows — running on mock data. A website's backend cannot be recovered from its URL. Phase 2 rebuilds it separately with the user's explicit go-ahead. Never let the user believe the server-side was cloned.

## When to Use This Skill

- "Clone this website," "rebuild this app," "make me a copy of X," "steal this design"
- User provides a URL, screenshots, a screen recording, an APK file, or a desktop EXE as the thing to replicate
- User names an app ("make an Airbnb clone") — find its web presence and run the URL pipeline
- "Copy it but change the colors/branding/wording" → clone first, then remix
- A clone (or any app) build feels like it's spiraling: repeated failures, context ran out, "where were we?", "it keeps breaking"

---

# PHASE 0 — Intake & Scope

## Step 1: Collect the source material

| Input | Extraction pipeline |
|-------|---------------------|
| **URL** | Fetch HTML/CSS → compute style token inventory; crawl route tree; screenshot each route |
| **App name** | Web search: official site, app-store listing, press shots, promo videos → feed best URL into the URL pipeline; screenshots as fallback ground truth |
| **Screenshots / recording** | Extract tokens directly (color quantization, font matching, spacing grids); build route list from visible navigation; lower confidence — flag it |
| **APK** | Decompile (`apktool d`) → `res/values/colors.xml`, `strings.xml`, `styles.xml`, layout XML, drawables, `AndroidManifest.xml` screen inventory — the app's *actual* design system |
| **EXE** | Check if Electron (`resources/app.asar` present) → **extract the asar: that's the original frontend source, the highest-fidelity input that exists.** Otherwise extract strings/icons/web-view URLs; WebView shell → real UI is at the embedded URL → switch to URL pipeline |

Accept **multiple inputs at once** and fuse them (APK + screenshots + listing URL). More ground truth = higher fidelity.

## Step 2: Triage honestly (before any code)

- ✅ **Cloneable surface:** marketing sites, landing pages, dashboards, e-commerce, content apps, most mobile app screens
- ⚠️ **Hard (state it, then attempt):** editors (Figma-class), real-time collab, complex state machines, canvas/WebGL — promise *structural* fidelity, not pixel identity
- ❌ **Out of Phase 1:** the backend/data/auth/payments (→ Phase 2), native OS integration, DRM'd or login-walled content (say so; ask for screenshots as substitute ground truth)

## Step 3: Write `CLONE-SPEC.md` (one page, the single source of truth)

This file is the spec, the plan's anchor, and the remix ledger. A blank section means you haven't asked enough questions. **Keep it to one page** — longer means the scope is wrong.

```markdown
# CLONE-SPEC — <clone name> of <original>

## Source(s) received
- URL: · APK: · EXE: · Screenshots: · App name:

## Scope contract
- Phase 1 = surface clone on mock data (original's backend NOT recoverable)
- Phase 2 (opt-in, separate approval) = real backend

## v1 route/screen list (MAX 5 core screens)
1. /home  (desktop + mobile)
2. ...
(Any further screens go to the v2 list — non-negotiable anti-scope-creep rule)

## Explicitly NOT building (v2 list)
- ...

## Design tokens found                 ← becomes tokens.json
Colors: #0A84FF … · Fonts: Inter … · Radii: 12px … · Spacing grid: 8px

## Remix overrides (baseline = pixel-faithful)
- none yet / <user's "but change X" list, numbered>

## Done looks like
- [ ] Fidelity bar met: ≥85% mean SSIM, no route below 70%
- [ ] Coverage matrix complete: every screen built or visibly deferred
```

Rules:

- **Exactly 5 routes in v1.** A 6th goes to the v2 list — visible, considered, deferred. This is the single most effective anti-scope-creep control that exists.
- **"Explicitly NOT building" is the most valuable section.** Every casual "can it also do…?" gets parked here so the user sees it was considered, not ignored.
- **Baseline rule:** the default clone is pixel-faithful. "But change X" requests are remix overrides recorded above, never silently baked in — this keeps a diffable ground-truth baseline.

**Legality guardrail:** for public deployment, clone layout/structure/UX patterns — not logos, trademarks, copyrighted images, or brand assets. Replace brand assets with neutral stand-ins by default; users can supply their own. Personal/learning use needs no changes, but say the rule exists.

---

# BUILD DISCIPLINE (runs through every phase)

The failure modes this section kills: "done" that was never run, working features deleted by the next edit, sessions that can't resume, and endless patching of a misunderstood bug.

## Boring stack defaults

Pick from this table unless the user has an explicit, informed reason not to. Novelty is the enemy of a solo AI-built project.

| Need | Default |
|------|---------|
| Web framework | **Next.js** (or the ecosystem's plainest equivalent) — huge training surface = the agent knows it cold |
| Backend | The same framework's API routes — one process, no CORS headaches |
| Database | **SQLite** (Prisma/Drizzle) → Postgres only when multi-user |
| Auth | None for MVP; then NextAuth/Auth.js |
| Styling | Tailwind + tokens from `tokens.json` — every color/font from ground truth, not taste |
| Hosting | Vercel / Railway / Fly.io — **get a URL live in session 1** |
| Payments | Stripe hosted Checkout — never build a payment form |

## The build loop (verify, don't't claim)

Turn CLONE-SPEC's routes into `PLAN.md`: numbered, small (one edit-verify cycle each), each with a concrete verification command. Then repeat:

```
1. PICK     → next unchecked task from PLAN.md
2. EDIT     → smallest change that completes it
3. VERIFY   → run the command, OBSERVE the output
             (for a clone route: the diff-check in Phase 1 Step 3 IS the verify)
4. COMMIT   → only after a green verify
5. LEDGER   → check the box in PLAN.md, append a line to PROGRESS.md
```

- **Build counts, tests count, a real HTTP 200 counts, a diff score counts. Reading your own code does not count.**
- **Paste the evidence.** One line of real output beats three paragraphs of prose.
- **Verify fails → you are not done.** Two failures in a row → stop and go to Thrash Detection.

## Green-state checkpoints

- `git init` on session 1 if needed. **Commit after every green verify:** `feat: /pricing clone ✓ 87% diff` or `fix: cart total NaN ✓ verified`.
- **Never commit red states.** WIP goes to a `wip/` branch, never the mainline.
- Risky change (dependency upgrade, big refactor)? `git checkout -b experiment/<name>` first.
- **Golden rule:** any working screen must survive one careless edit. Rollback for a frightened user is one line: *"run `git revert` on the last commit — I'll walk you through it."*

## The handoff ledger (`PROGRESS.md`)

Context windows die; sessions end mid-task. The ledger makes that harmless. Maintain it **append-only, newest first**:

```markdown
# PROGRESS — <clone of X>
Updated: <date> · Last verified state: <commit sha> · App runs: yes/no

## ✅ Verified working (with evidence)
- [date] /home — diff 94% green, committed abc1234, live at <url>

## 🚧 In progress
- /checkout — diff at 82%, iterating on header spacing; tests last green abc1234

## ⚠️ Known issues
- Profile form misaligns at 390px (repro: shrink viewport on /profile)

## 🧭 Next exact step
- Re-run diff on /checkout, fix header spacing, commit.

## 🗺️ Where things live
- Tokens: tokens.json · Spec: CLONE-SPEC.md · Coverage: COVERAGE.md
- Clone: clone/* · Ground truth: original/* · Mock data: clone/src/data/
```

- **Session-start ritual:** read `PROGRESS.md` first, **re-run the last known-green command** to confirm state matches the ledger (it can lie after an interrupted edit), then continue.
- **Session-end ritual:** update the ledger before stopping for any reason. "Next exact step" must be concrete enough that a stranger — or a contextless you — could act on it cold.

## Debugging protocol — reproduce first, minimal fix

1. **Reproduce** the failure on demand (specific command/click path). Can't reproduce → investigate state, don't guess.
2. **Read the actual error** — full stack trace, first meaningful frame. Not the vibe.
3. **One hypothesis at a time.** Change one thing, re-run.
4. **Verify the fix AND the neighbors** — re-run the full suite/diff, because fixes are how features break.
5. **Commit with the repro in the message.**

Same bug survives **two fix attempts** → stop patching, re-read the code fresh, or reset. A third patch is almost always working on a misunderstanding, not a bug.

## Thrash detection — reset instead of patch

Signs the build is spiraling:

- ❌ Same test/diff check failed 3+ times in a row
- ❌ Diffs growing each attempt (small fix → rewrite file → rewrite feature)
- ❌ Editing the same file in conflicting ways across turns
- ❌ User asked "is it broken?" twice
- ❌ You can no longer state, in one sentence, **why** the current change should work

**The reset ritual:**

```bash
git status                   # what's actually changed
git stash -u                 # or commit to wip/branch — never lose work
git checkout <last-green>    # return to verified ground
```

Then say, plainly:

> "We've tried three fixes and it's still failing, so I'm probably working from a wrong assumption. Here's what I now believe is true: <X>. I'm rolling back to the last version that worked (<commit>). Should I retry with this new understanding?"

This is the protocol working, not failure. Rolling back costs one minute; patching a misunderstood codebase for an hour costs the project.

---

# PHASE 1 — The Surface Clone Loop

## Step 1: Capture ground truth → `original/`

- Screenshot **every route** in the inventory at two viewports (1440×900 and 390×844) using whatever browser/screenshot tooling your agent has (see Agent-agnostic note). Save `original/<route>_<viewport>.png`.
- Write `tokens.json`: palette (dominant + accent + neutrals), font families & sizes, radii, shadows, spacing scale — from computed CSS (URL), `colors.xml` (APK), asar source (EXE), or color quantization (screenshots). **Note the source next to each token.**
- ASAR hit → store original frontend source under `original/source/`.
- Record interaction notes per route: nav behavior, modals, hover states, form fields.

## Step 2: Build the clone → `clone/`

- Scaffold with the boring-stack defaults; apply `tokens.json` as CSS variables.
- One route per `PLAN.md` task, ordered by the inventory. **Mock data** matches the original's *shape* (same field names, plausible values), marked `// MOCK` in an isolated `data/` layer so Phase 2 swaps it for a real API by editing one layer.
- Run every change through the Build Discipline loop: verify → commit → ledger.

## Step 3: The closed loop — diff, iterate, prove

For each route:

```
1. SCREENSHOT  clone at the SAME viewport as original/<route>_<viewport>.png
2. DIFF        SSIM or pixelmatch → similarity score (catch layout offsets too:
               a 40px vertical shift must score low, not "close enough")
3. ITERATE     worst-scoring regions first: fix tokens/spacing/typography, re-shoot
4. STOP        route ≥ bar (85%) — commit ✓ — or 3 iterations hit:
               mark ⚠️, log the reason in COVERAGE.md, move on (don't thrash)
```

**Fidelity report card** (the deliverable — numbers, never "it looks identical"):

```
FIDELITY REPORT — clone-of-x · bar: 85% mean / 70% floor
  /home      94%  ✅
  /pricing   87%  ✅
  /checkout  82%  ⚠️  — header spacing off
  /profile   71%  ⚠️  — mobile source lacked ground truth
  Mean: 83.5% · Floor pass: yes · Coverage: 5/5
```

## Step 4: Coverage matrix — no silent gaps

Maintain and deliver `COVERAGE.md`:

| Screen | Built | Diffed | Score | Status |
|--------|-------|--------|-------|--------|
| /home | ✅ | ✅ | 94% | verified |
| /settings | ⬜ | ⬜ | — | deferred (v2 list) |

Any screen not built must be **visibly deferred with a reason** — never quietly absent.

## Step 5: Remix mode ("but change X")

- Commit the pixel-faithful baseline **first** — that commit is the revert point.
- Each override appended to CLONE-SPEC's Remix overrides as a numbered, testable item: `#1 palette → forest green`, `#2 rename headline to …`.
- One commit per override: `remix #1: palette → forest green`.
- Ship a **remix diff summary** (override list + before/after screenshots) so any single override can be reverted.

---

# PHASE 2 — Backend Rebuild (opt-in, explicit approval required)

Only after the Phase 1 report card is delivered and the user explicitly says go. Scope and cost roughly double — the user deserves that choice.

1. **Reverse the surface:** the mock-data layer documents every shape/flow the UI expects → that's the API contract.
2. **Design the backend** from observed flows: entities, endpoints, auth, edge cases the original visibly handles (empty states, error toasts, pagination).
3. **Re-spy the original:** poke its real flows (or user-provided HAR/screenshots) to match behavior — validation messages, status transitions, limits.
4. **Swap the mock layer** behind a feature flag; mocks stay for tests.
5. **Verify behavior, not just rendering:** end-to-end tests per flow; extend CLONE-SPEC with a behavioral checklist and report completion with observed evidence — same rule as fidelity: numbers, not claims.

---

# Failure Modes & Countermeasures

| Failure | Countermeasure |
|---|---|
| "It looks the same" with no evidence | Report card is mandatory; no numbers, no "done" |
| Changes silently baked into the clone | Baseline-first commits; overrides only via Remix list |
| Backend pretended into existence | Scope contract in Phase 0; mock layer visibly `// MOCK` |
| Screens silently dropped | COVERAGE.md exists before "done"; gaps shown, not hidden |
| Impossible source (login-walled, WebGL monster) | Triage gate says so upfront; request screenshots; promise structure not pixels |
| Diminishing returns on iteration | 3-iteration cap per route → ⚠️ + move on; mean score is the metric |
| IP-dangerous clone (logos, brands) | Neutral stand-ins by default; guardrail stated in CLONE-SPEC |
| "Done" claimed without running anything | Verify-don't-claim; paste command output every time |
| Working screen lost to a careless edit | Green-state commits; never commit red |
| New session can't resume | PROGRESS.md + re-verify last green before continuing |
| Endless patch loop on a failing fix | Two-attempt rule → thrash detection → reset ritual |

# Definition of Done

- [ ] Scope contract stated and acknowledged before coding began
- [ ] CLONE-SPEC one page, 5 routes, v2 list populated, tokens sourced (not eyeballed)
- [ ] Every inventory route built **or visibly deferred** in COVERAGE.md
- [ ] Each built route diffed at both viewports; per-route score recorded; mean ≥ 85%, floor ≥ 70%
- [ ] Report card delivered with evidence — never "it looks identical"
- [ ] Remix overrides (if any) each a separate revertible commit, baseline committed first
- [ ] Mock layer isolated and marked; Phase 2 not started without explicit approval
- [ ] PROGRESS.md current as of the last commit; PLAN.md boxes checked
- [ ] Everything above verified by running it — claims without pasted output don't count
