# cocoex-website — agent context

_Shared rules (Pi + Hermes): `../AGENTS.md` (gh/). Read it if it isn't in your context._

cocoex.xyz — scroll-driven WebGL site for **cocoex**, a transdisciplinary social-impact
lab (art, community, social impact). Vanilla JS (no frameworks), GSAP ScrollTrigger +
Lenis smooth scroll, custom WebGL shaders, built with Vite. The full engineering
reference lives in `docs/engineering-context.md`.

## Always-on constraints (do not break)
- **Vanilla JS only** — no frameworks. Deps stay `gsap@3.12.5` + `lenis@1.3.4` + Vite.
- **Never hardcode `vh`** in animations or CSS heights — reference `src/scroll/timeline.js`
  (`phase()`/`sectionSpan()`/CSS vars). The only exception is the deliberate
  `.comet-panel-intro` (200vh) / `.comet-panel-tabs` (300vh) literals in `comet.css`.
- **One `Renderer` RAF loop.** Off-screen layers are gated by section (`section-gate.js`);
  never start a private `requestAnimationFrame`. DPR cap `min(devicePixelRatio, 2)` is
  non-negotiable. Stay under 8 WebGL contexts (5 exist; Safari cap).
- **Use the starfield factory** (`createStarfield`, `invert`/`intensity` uniforms) — don't
  write new starfield shaders. Register `webglcontextlost`/`restored` on any new context.
- **ScrollTrigger strips `vh` to `px`** in `start`/`end` strings — convert vh→px first
  (`vhToPx`) in function-form triggers with `invalidateOnRefresh: true`. (`comet.js` still
  uses raw vh strings — a known latent bug, left intentionally; don't "fix" by halves.)
- **iOS viewport:** scroll-layout math uses `vhToPx` (measures the real CSS `vh` via a
  probe), never `innerHeight`. Keep `html, body { height: 100% }` and `body { overflow-y: auto }`.
  Never re-enable Lenis `normalizeScroll`, never `scroll-behavior: smooth` on `html`, never
  `overflow-y: scroll` on both `html` and `body`.
- **Intro overlay teardown** is driven by its own ScrollTrigger at the *unbuffered* intro
  end (`sectionSpan('intro').endVh`), NOT by the section gate (the 75vh lead would hide it early).
- **Safari + blurred filter + scrubbed opacity = jank:** bake the effect into the asset
  (`filter: none`) — don't try `translateZ(0)`.
- Accessibility: 44px min touch targets (the shrunken footer icons are the deliberate
  exception), `prefers-reduced-motion`, visible focus outlines, keyboard nav.

## Where to look (read on demand)
- `docs/README.md` — docs index.
- `index.html`, `src/**`, `src/styles/*.css` — the code is the reference for *what* it does.

## Workflows
- Animation/WebGL/CSS work → `static-web-dev` skill (`.pi/skills/static-web-dev/SKILL.md`).
- End of a work session / before commit → `doc-minder` skill (`.pi/skills/doc-minder/SKILL.md`) (keeps docs honest).
- Pre-push checklist → `/pre-push` (`.pi/prompts/pre-push.md`).

## Conventions
```bash
npm install && npm run dev    # http://localhost:5173
npm run build                 # → dist/
npm run preview
```
Test across Chrome/Firefox/Safari + iOS/Android, verify reduced-motion and keyboard nav,
Lighthouse 95+. Commit style: Conventional Commits.
