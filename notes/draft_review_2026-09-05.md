# Draft review — 2026-09-05

Review of `docs/draft/index.html` vs. live `docs/index.html` (https://vitaminae.ch/draft/).

## Content contradictions — need a decision

1. **Locations mismatch.** Draft (`:784`) lists one address: Zeughausstrasse 31, 8004 Zürich. Live (`docs/index.html:64,71`) lists two: Manegg (Pergamin II, Maneggstrasse 33, 8041) and Stauffacher (Badenerstrasse 47, 8004). Draft's own welcome copy on live still says "in Manegg, Stauffacher oder online." Which is current?
2. **"2+ Standorte" claim** (`:483`) doesn't match — only one physical location + Online is shown.
3. **Group size contradiction.** "3–6 Personen" (hero pill `:462`, USP `:480`, Standardkurse meta `:539`) vs. "Max. 5 Teilnehmende" in both new-course schedule columns (`:706,713`).
4. **Module length / pricing mismatch.**
   - `:524`: "Standardkurse sind in drei Module unterteilt. Jedes Modul umfasst 12 Lektionen." Live: "Jedes **Niveau** ist in drei Module unterteilt... Ein Modul umfasst **10** Lektionen." (10→12 — intentional?)
   - Draft dropped the `A1.1/A1.2/A1.3` example that made "Modul 1" in the schedule legible.
   - No package matches a 12-lesson module (only a 10er-Paket exists). Schedule shows CHF 840 for 90-min/12 lessons = 70 CHF/Lek — the package rate — meaning the advertised 74 CHF/Lek never applies to any course actually sold.
5. **Years of experience drift.** `:502` "seit über 15 Jahren **lebe und unterrichte**" vs. `:503` "seit über 13 Jahren begleite ich" vs. USP/chip "13+". Live correctly says just "**lebe** seit über 15 Jahren" (living, not teaching) — draft rewrite introduced the conflict.
6. **Copyright year** `:827` — "© 2025" while advertising October 2026 courses.
7. **"Zertifizierte Lehrerin"** (`:461`) — no certification is actually named anywhere; chips list degrees only.
8. **"SwissRE"** (`:509`, `:688`) — correct name is "Swiss Re". Also confirm permission to name clients.

## Accessibility

9. **No mobile nav at all.** `:434` — `.nav-links{display:none}` under 900px with no replacement. CTA "Probelektion buchen" disappears entirely on phones.
10. **Form labels not associated** `:814–816` — no `for=`/`id=`, screen readers announce unlabelled fields.
11. **Terra `#d45626` fails AA contrast** — 4.07:1 on white, 3.42:1 on peach `#kurse` bg. Affects `.lbl`, `.loc-link`, `.sched-day`, `.contact-detail.email`, and white-on-terra buttons (`.btn-nav`, `.btn-submit`, `.ppkt`, `.btn-primary`). `--terra2 (#b83d18)` clears 4.5:1.
12. **Carousel dots have no accessible name** `:892` — empty `<button>`s, need `aria-label`.
13. **Autoplay with no pause control** `:906` — 5.2s auto-advance, only pauses on `mouseenter` (no keyboard/touch escape). Off-screen slides stay in tab order.
14. **No `prefers-reduced-motion` guard** anywhere (badge pulse, reveal transitions, carousel).
15. **`html{font-size:18px}`** (`:28`) overrides user's browser font-size setting; use `100%`.
16. **Footer text contrast** rgba(255,255,255,.5) on navy = 4.47:1, marginal fail.

## Dead code shipped to production

17. **Tweaks panel is builder scaffolding** — `:831–847`, `:862–882`, `body.vibrant` CSS throughout (~120 lines unreachable; nothing sends the postMessage that reveals it). Note: vibrant palette also has white-on-gold at 2.24:1 if ever shown.
18. **`<template id="__bundler_thumbnail">`** at `:438–440` between `</head>` and `<body>` — bundler leftover.
19. **`data-screen-label`** attributes on every section — no effect, dead.
20. **Unused hero CSS:** `.btn-primary`, `.btn-ghost`, `.hero-right`, `.hero-photo` (`:113–135`).
21. **Scroll-reveal never actually triggers on scroll** — `:952` marks everything visible 1.5s after load regardless of scroll position. Either drop the 1500ms fallback or the whole reveal system.

## Layout / visual

22. **Expanding one course card leaves a gap next to it** — `:210` grid-auto-flow:column shares row heights across both columns.
23. **White stripe between About and Kurse** `:516` — matches neither `#fdf9f3` nor `#fbe8d7` neighbour.
24. **`#kurse{background:#fbe8d7}`** (`:159`) hardcoded outside token system, silently overrides `.section-alt` class on `:519`.
25. **Floral dividers inconsistent** — appear 3× but not between Kurse→Neue Kurse or Neue Kurse→Testimonials.

## Copy & formatting

26. **Wrong closing quotes** — all six testimonials open `„` close with straight `"` instead of `"`.
27. **`ß` on a Swiss site** (Fließend, Spaß, Maßgeschneidert, großartig) — Swiss German uses `ss`. Live has the same issue, so may be a house-style call.
28. **Three languages in one section** — nav "Testimonials" (EN), heading "Testimonios" (ES), rest German.
29. **Mixed du/Sie** — "deine Ziele" (Privatkurse `:620`) vs. "Ihr Team" (Firmenkurse `:685`). Defensible for B2B but should be deliberate.
30. **Two currency formats** — "840 CHF" vs "CHF 840.–".
31. **Redundant heading** `:699–700` — eyebrow repeats the h2 text almost verbatim.
32. **Inconsistent first schedule row** `:707` — different format from siblings `:708,709`.
33. **`#neue-kurse` missing from nav** despite being time-sensitive; `id="top"` on `:441` is unused.
34. **"Raum inklusive"** under Privatkurse (`:625`) next to "Vor Ort oder online" — doesn't apply online.

## SEO / performance / legal

35. **No `noindex`** on draft — will be indexed as duplicate content against live page.
36. **Oversized images:**
    - `gloria_photo` — 1668×2222, 2.5 MB PNG, displayed at max 380px. Should be ~760px WebP, ~150 KB.
    - `logo` — 908×437, 743 KB PNG, this is the LCP element. Should be well under 100 KB or SVG.
37. **No `width`/`height` on any `<img>`** → layout shift. No `loading="lazy"` below fold, no `fetchpriority="high"` on hero logo.
38. **Missing meta description, OG tags, favicon, canonical.**
39. **No Impressum / Datenschutzerklärung** — commercial Swiss site collecting form data needs a privacy notice.
40. **Contact form has no spam protection, no thank-you page** `:811` — add web3forms honeypot + `redirect` hidden field.
41. **`lang="de"`** could be `de-CH`; `:785` uses `share.google/` shortlink vs. live's more durable `maps.app.goo.gl/`.

---

**Top 4 before going live:** address discrepancy (#1), mobile nav (#9), module count/pricing mismatch (#4), strip the tweaks panel (#17).
