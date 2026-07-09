# Kiln Collective

**Live:** https://bswxyz.github.io/kiln-collective/ · **Build notes:** https://bswxyz.github.io/kiln-collective/guide/

A warm, tactile marketplace site for a fictional cooperative ceramics studio — small-batch
handmade pieces, wheel-throwing classes, and studio-bench booking. Part of the
[Parable 25 design showcase](https://bswxyz.github.io/fable-hub/).

---

## The concept

Kiln Collective is twenty-three potters sharing four kilns in a former print shop. The site sells
the two things a co-op actually sells: objects with fingerprints in them, and the feeling of a room
where nobody is in a rush. Pieces are numbered per batch ("KC-041·03"), attributed to their maker,
and flagged "One of one" when they are exactly that. Classes show seats as hand-drawn dots and a
sold-out class opens a waitlist instead of greying out. The interaction patterns are borrowed from
shipped products (Etsy's maker-attributed product cards, Airbnb Experiences' matter-of-fact
"3 spots left" scarcity, team strips à la Passionfroot) — see `_research/kiln-collective.md` in the
showcase repo for the Mobbin study.

## Design system

- **Palette (terracotta on cream):**
  `--bg:#f3e9dd` cream paper · `--card:#f8f1e6` raised sheet · `--ink:#2a1c14` fired-dark ·
  `--umber:#7a4a2e` secondary text (6.1:1) · `--clay:#c65d3b` terracotta (large accents/linework
  only — 3.4:1) · `--clay-deep:#a04527` small clay text (4.9:1) · `--sage:#8a9b7a` quiet green ·
  `--sage-deep:#5a6b4d` · `--line:rgba(42,28,20,.12)`. Contrast decided which token goes where.
- **Type:** `Fraunces` (display, with the `SOFT` axis maxed and `WONK` on — thrown, not machined) ·
  `Work Sans` (body) · `DM Mono` (prices, batch numbers, hours — the shop-ledger voice).
- **Signature motion:** the "cone six" curve, `cubic-bezier(.65,.05,.18,1)` — a slow ramp and a
  gentle settle, like a firing schedule. Clipped-line hero intro, 1.1–1.5s reveals, drawn-on SVG
  strokes staggered 160ms apart.
- **Signature techniques:** (1) an SVG `feTurbulence` paper-grain tile, inlined as a data URI and
  blended `multiply` over the whole page; (2) hand-drawn-style SVG linework — wobbly frames,
  squiggle underlines, arrows, six vessel illustrations and four maker's marks — that draws itself
  in on scroll via `pathLength="1"` + `stroke-dashoffset`; (3) the uneven-corner `border-radius`
  trick so buttons and badges look inked, not stamped.

## Stack — why Astro

- **[Astro 5](https://astro.build), zero client framework.** The whole site is content: six pieces,
  three classes, four makers, one story. Those live as typed arrays in page frontmatter and render
  to plain HTML at build time. The only JavaScript that ships is one small inline script
  (IntersectionObserver reveals, draw-on stagger, counters).
- **Components without a runtime.** `Vessel.astro` and `MakerMark.astro` are author-time
  components — reusable SVG illustrations that cost nothing in the browser.
- **Static output** drops straight into `docs/` for GitHub Pages — no server, no Actions workflow.

## Running locally

```bash
git clone https://github.com/bswxyz/kiln-collective
cd kiln-collective
npm install
npm run dev        # dev server at localhost:4321/kiln-collective/
npm run build      # static build → ./docs
npm run preview    # serve the built output
```

## Structure

```
astro.config.mjs            site/base/outDir — the GitHub Pages recipe
src/styles/global.css       all styling — design tokens live in :root at the top
src/pages/index.astro       the page: data arrays in frontmatter, sections, inline script
src/pages/guide.astro       the "how it was built" write-up (styled to the site)
src/components/Vessel.astro     six hand-drawn vessel illustrations (moonjar, pitcher, mugs…)
src/components/MakerMark.astro  four maker's-mark stamps
public/assets/hero.jpg      the one photograph on the site (3:2, compressed)
public/.nojekyll            tells Pages to serve files as-is
docs/                       BUILT OUTPUT — committed so Pages serves main + /docs
```

Design tokens: `src/styles/global.css` `:root`. A growing catalog would graduate the piece/class
data from frontmatter arrays into Astro content collections without touching the templates.

## Demo vs. real — what a production version would need

This is an intentionally-scoped design showcase. What's **fictional/mocked** today:

- **The shop, makers, classes and numbers are invented.** No real studio, no real inventory;
  "spots left" and the counters are static data, not live state.
- **No commerce.** "Save a seat" / "Book a bench" link to the visit section; a real version needs
  checkout (Stripe), inventory that actually decrements one-of-one pieces, order/shipping flows
  and email receipts.
- **No booking backend.** Real class booking needs a calendar source of truth, capacity locking
  (two people buying the last seat), waitlist notifications and cancellation policies.
- **No accounts or CMS.** Makers would need profiles and a way to list pieces — Astro content
  collections + a headless CMS, or a small database.
- **The hero photograph is AI-generated** (the only raster image on the site); a real co-op would
  shoot its actual pieces — the SVG-illustrated cards would become photography.

What's **real** and reusable as-is: the entire visual system, the paper-grain and draw-on
techniques, the accessible reveal/reduced-motion/keyboard layer, and the Pages deploy recipe.

## License

[MIT](LICENSE). Design & build by **Parable** (Anthropic's Claude). The hero photograph is
AI-generated; the ceramics, makers and studio are fictional.
