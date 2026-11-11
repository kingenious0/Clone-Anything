# CLONE-SPEC — "LaunchPad" clone of stripe.com (SAMPLE)

> Sample file shipped with the clone-anything skill to show what a filled-in spec looks like.

## Source(s) received
- URL: https://stripe.com · Screenshots: 6 (landing, pricing, products, docs-home, login, contact)

## Scope contract
- Phase 1 = surface clone on mock data (original's backend NOT recoverable)
- Phase 2 (opt-in, separate approval) = real backend — **not started**

## v1 route/screen list (MAX 5 core screens)
1. `/` home — desktop + mobile
2. `/pricing` — desktop + mobile
3. `/products` — desktop + mobile
4. `/docs` — desktop
5. `/login` — desktop + mobile

## Explicitly NOT building (v2 list)
- Real payment processing → v2
- Blog & changelog pages → v2
- Locale switcher (7 languages) → v2

## Design tokens found                 ← becomes tokens.json
- Colors: `#635bff` (brand), `#0a2540` (ink), `#425466` (body), `#f6f9fc` (wash)
- Fonts: Inter (headings 700/800, body 400) — loaded via next/font
- Radii: 16px cards / 999px pills · Shadows: `0 15px 35px rgba(50,50,93,.1)`
- Spacing grid: 8px · Source: computed CSS (URL pipeline)

## Remix overrides (baseline = pixel-faithful)
- #1 replace purple `#635bff` with teal `#0d9488`
- #2 rename all "Stripe" wordmarks to "Finlytics"
- #3 pricing headline → "Pricing that scales with you"

## Done looks like
- [ ] Fidelity bar met: ≥85% mean SSIM, no route below 70%
- [ ] Coverage matrix complete: every screen built or visibly deferred
