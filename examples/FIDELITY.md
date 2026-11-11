# FIDELITY REPORT — LaunchPad (clone of stripe.com) (SAMPLE)

**Bar:** ≥85% mean SSIM · ≥70% floor · **Viewports:** 1440×900 + 390×844 · **Method:** screenshot pixel-diff

| Route | Desktop | Mobile | Mean | Status |
|-------|---------|--------|------|--------|
| `/` home | 94% | 91% | **94%** | ✅ verified |
| `/pricing` | 87% | 85% | **87%** | ✅ verified |
| `/products` | 89% | 84% | **86%** | ✅ verified |
| `/docs` | 88% | — (desktop-only) | **88%** | ✅ verified |
| `/checkout`* | 82% | 78% | **82%** | ⚠️ header spacing off — 3 iterations hit, logged in COVERAGE |
| `/login` | 71% | 74% | **71%** | ⚠️ floor pass, but form layout diverged — ground truth was screenshots only |

**Overall mean: 84.7%** · **Floor pass: yes (no route <70%)** · **Coverage: 6/6 routes**

## Notes
- `/checkout` loss is concentrated in a 40px offset between hero bands at 1440px — root cause: original uses `scroll-margin` on an anchor the clone doesn't have.
- `/login` was the lowest-confidence input (no URL access, screenshots only). Expected.
- Scores are evidence, not opinions — raw images in `original/` and `clone/.screenshots/`, diff script: `tools/diff.mjs`.

## Remix applied after baseline (baseline commit: `a1c9f2e`)
- #1 teal palette ✓ · #2 wordmark swap ✓ · #3 headline ✓ — each a separate revertible commit.
