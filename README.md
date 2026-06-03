# Handoff: Johanna Janebäck — UX/UXR Portfolio Website

## Overview
A personal portfolio site for **Johanna Janebäck**, a UX/UI, brand & product designer based in Göteborg, Sweden, targeting UX / product / design-lead roles (notably the Swedish Tax Agency, Skatteverket). It is a scrolling marketing-style site: a **home page** (hero → about → featured work → experience & education → skills → contact) plus **four full case-study pages**. The aesthetic is warm, editorial and distinctive — a soft rose ground, a blurred pink→peach "aura" gradient, plum-grey ink, a lime spark accent, and a serif/sans type pairing.

## About the Design Files
The files in this bundle are **design references created in HTML/CSS** — working prototypes that show the intended look, layout, copy and behavior. They are **not** meant to be shipped as-is. The task is to **recreate these designs in the target codebase's environment** (e.g. React/Next, Astro, Vue, plain static site) using its established patterns, component model and tooling. If no codebase exists yet, choose an appropriate stack — this site is content-light and largely static, so a static-site generator (Astro/Eleventy) or Next.js static export is a great fit.

The prototypes use a small amount of vanilla JS (sticky-nav toggle, IntersectionObserver scroll-reveal) and one **React island** (the Tweaks panel) that is a **prototyping/demo affordance only — do NOT ship it**. See "Tweaks panel" below.

## Fidelity
**High-fidelity (hifi).** Final colors, typography, spacing, imagery and interactions are all specified. Recreate the UI faithfully using the target codebase's libraries. Exact tokens are listed under "Design Tokens".

---

## Global structure & layout

- **Canvas:** full-width, content constrained to `max-width: 1180px` with horizontal padding `--gutter: clamp(22px, 5vw, 80px)`, centered (`.wrap`).
- **Vertical rhythm:** sections use `.section` → `padding-block: clamp(72px, 11vw, 150px)`; tighter variant `.section--tight` → `clamp(48px, 7vw, 96px)`.
- **Section eyebrow** (`.eyebrow`): 13px, weight 600, `letter-spacing: 0.22em`, uppercase, color `--muted`, preceded by a 9×9px lime square.
- **Background:** `--rose #ECE4E2` everywhere except the dark footer/CTA blocks (`--ink #2B2535`).
- **Motion:** elements with `.reveal` start `opacity:0; translateY(26px)` and animate to visible (`opacity 1; none`) over `.9s cubic-bezier(.22,.61,.36,1)` when scrolled into view (IntersectionObserver, threshold 0.12, rootMargin `0px 0px -8% 0px`, unobserve after firing). Respect `prefers-reduced-motion: reduce` (no transform/transition; also disable smooth scroll).

### The "aura" gradient (signature element)
A blurred multi-radial blob, reused in hero and dark blocks. Implementation:
```css
.aura {
  position: absolute; pointer-events: none; z-index: 0;
  filter: blur(60px) saturate(112%);
  opacity: var(--aura-strength, 0.92);
  background:
    radial-gradient(38% 58% at 72% 32%, rgba(231,95,191,0.95), transparent 62%),
    radial-gradient(42% 52% at 58% 50%, rgba(240,122,168,0.78), transparent 64%),
    radial-gradient(40% 56% at 44% 62%, rgba(244,167,126,0.72), transparent 66%),
    radial-gradient(30% 46% at 30% 72%, rgba(246,199,154,0.55), transparent 70%);
}
```
On dark blocks it sits at lower opacity with `blur(80px)`. A thin white "wave line" SVG (`stroke: rgba(255,255,255,0.85); stroke-width:1.4`) rides over the hero aura.

---

## Screens / Views

### 1. Home — Navigation (`.nav`)
- **Fixed** top bar, full width, `padding: 20px var(--gutter)`. Left: wordmark `Johanna Janebäck` in italic display font (Afacad, 700, 21px) preceded by an 11×11px lime square. Right: links `About · Work · Experience · Skills` + a pill CTA `Contact`.
- **Scrolled state** (`.is-stuck`, toggled when `scrollY > 40`): background `rgba(236,228,226,0.72)`, `backdrop-filter: blur(16px) saturate(120%)`, 1px bottom hairline `rgba(70,63,87,0.14)`, reduced vertical padding (14px).
- Link hover: ink color + an underline that wipes in left→right (`::after`, 1.5px). CTA hover: fills with `--ink`, text becomes `--rose`.
- **Responsive:** below 760px, non-CTA links hide (`.cta` stays). (A mobile menu was not designed — implement a hamburger/sheet using the codebase's pattern.)

### 2. Home — Hero (`.hero`)
- `min-height: 100svh`, vertically centered content, `padding-top: 96px`, `overflow: hidden`. Aura + wave-line behind (z 0/1), content at z 3.
- Content: eyebrow `Portfolio · Göteborg, Sweden`; `H1` name `Johanna Janebäck` (serif Adamina); role line `UX/UI, Brand & Product Design` (italic Afacad, `--ink-soft`); sub-paragraph; two buttons (`View selected work` primary, `Get in touch` ghost).
- **Sub copy:** "I design end-to-end UX and UI, from user research and accessibility to product strategy, design systems and brand. I combine product design, leadership and teaching."
- **Three layout variants** exist, switched by `body[data-hero="…"]`. **Ship the `split` variant** (the chosen default): left-aligned, name `clamp(44px, 8.4vw, 108px)`, role block `clamp(21px, 3vw, 38px)`, aura pushed to the right (`right:-10vw; top:8%; width:62vw; height:60vh`). (The `centered` and `stacked` variants are prototype-only alternatives driven by the Tweaks panel; you only need `split`.)

### 3. Home — About (`#about`)
- Two-column grid `0.85fr / 1.15fr`, gap `clamp(36px,6vw,88px)`, vertically centered. Stacks to 1 column below 820px (portrait caps at 380px wide).
- **Left — portrait:** `assets/portrait.jpg`, B&W (`filter: grayscale(1) contrast(1.02)`), `aspect-ratio: 4/5`, `object-fit: cover; object-position: top center`. It is **zoomed to 1.1×** anchored at the top: wrap the image in a clip element with `border-radius: var(--radius); overflow: hidden`, and apply `transform: scale(1.1); transform-origin: top center` to the `<img>` (so the crop comes off the bottom, top stays). A lime 40×40px square (`.mark`) sits at top-left `-14px/-14px`. A soft pink/peach blurred glow sits behind, bottom-right (`::before`, `filter: blur(34px)`, z -1).
- **Right — text:** eyebrow `What's my thing?`; a large serif statement; two body paragraphs; a sign-off line with a lime dot.
  - **Statement** (`.about-statement`, serif `clamp(28px,3.6vw,46px)`, line-height 1.12): "I'm a design and product leader who wants to build *impactful, user-centered* things that really matter." — the phrase **"impactful, user-centered" is italic** (Afacad italic via `em`); "really matter" is regular.
  - **Body P1:** "I'm a designer and product owner with a master's in Interaction Design & Technologies and a bachelor's in Design & Product Development from Chalmers University of Technology. I work the whole way across the stack from product management, UX, research and brand."
  - **Body P2:** "I enjoy taking a fuzzy problem, research it with the people it's for, shape the strategy, and then design something that's genuinely nice to use. I'm happiest delivering work that makes a real, measurable difference."
  - **Sign-off:** "Based in Göteborg · open to UX, product & design-lead roles"

### 4. Home — Featured work (`#work`)
- Head row (`.work-head`, space-between, wraps): left = eyebrow `Selected work` + `H2` **"Featured projects."**; right = blurb (max 320px): "Research, product, systems and brand, most from my years leading design at Satcube, plus my master's thesis on accessibility."
- **Four case cards** (`.case-card`), stacked vertically with `gap: clamp(28px,4vw,54px)`. Each card: 2-col grid `1fr/1fr`, `gap: clamp(24px,4vw,60px)`, `background: --paper #FBF8F7`, `border: 1px solid --paper-edge #EFE7E4`, `border-radius: 26px`, `padding: clamp(22px,3vw,40px)`. **Even-indexed cards flip** the media to the right (`:nth-child(even) .case-card__media { order: 2 }`). Hover: `translateY(-5px)`, shadow `0 30px 60px -34px rgba(43,37,53,0.4)`. Stacks to 1 col below 820px.
  - **Media** (`.case-card__media`): `border-radius:16px; overflow:hidden; aspect-ratio:4/3`, image `object-fit: cover`.
  - **Body:** index (serif, `--muted`) · category (12.5px, 600, 0.18em upper, color `--lime-deep #6FB94E`) · `H3` title (`clamp(26px,3vw,40px)`) · one-line description (`--body`, max ~42ch) · "Read the case →" link (arrow nudges right on hover).
- **The four cards (in order):**
  1. `→ case-cognitive-app.html` · idx "01 — Master thesis" · cat "UX Research · Accessibility" · **"An accessible activities app, designed WCAG AAA"** · "For people with intellectual disabilities — built on workshops, interviews, testing and observation, with no decision made without the people it was for." · img `assets/card-grunden.jpg`
  2. `→ case-satcube-webgui.html` · idx "02 — UX / UI · Satcube" · cat "Complex systems · Accessibility" · **"An intuitive way to manage satellite terminals, for novices and experts alike"** · "The WebGUI configures and monitors a satellite terminal in real time, designed so a first-timer understands it as well as a pro." · img `assets/card-webgui.jpg`
  3. `→ case-satcube-website.html` · idx "03 — Web · Design system · Satcube" · cat "Web Design · Product ownership" · **"Satcube website and design system, one site with many uses"** · "Shop, dashboard, news and product pages with very different goals, unified by a design system I built from scratch." · img `assets/card-website.jpg`
  4. `→ case-satcube-brand.html` · idx "04 — Brand · Satcube" · cat "Brand Identity" · **"An accessible identity for a technical industry"** · "A complete visual brand led solo, from competitor analysis and stakeholder workshops to a logotype drawn from the layers of the atmosphere." · img `assets/card-brand.jpg`

### 5. Home — Experience & Education (`#cv`)
- Eyebrow `Where I've been`; two-column grid `1fr/1fr`, gap `clamp(40px,6vw,90px)` (stacks below 820px).
- **Left "I've worked"** — items (`.cv-item`, grid `auto/1fr`, top hairline, 22px vertical padding). Each: a 38×38px rounded icon tile (`background: rgba(255,255,255,0.6); border:1px solid --hair`, holds a 20×20 stroked SVG, `--ink-soft`) + role (serif 21px) + meta (`--muted` 15px) + field (14.5px `--ink-soft`).
  - **Design Lead & Product Owner** · Satcube · 2022–2025 · "Owned design & roadmap for the website, WebGUI and app — plus brand and art direction — across the space industry." · **icon: rocket**
  - **Project Manager & UX/UI Designer** · Noor Digital · 2025 · "Client projects at a digital marketing agency — from scoping to shipped UI." · **icon: pen**
- **Right "I've studied"** — three items (icon tiles, no field): MSc · Learning and Leadership · Chalmers (people icon); MSc · Interaction Design and Technologies · Chalmers (cursor icon); BSc · Design and Product Development · Chalmers (compass icon). All "Chalmers University of Technology".

### 6. Home — Skills (`#skills`)
- Eyebrow `What I bring`; three cards (`.skill-card`, `--paper` bg, `--paper-edge` border, `border-radius:20px`), grid `repeat(3,1fr)` (1 col below 760px). Each: `H3` + bulleted list (bullets are 6×6px lime squares).
  - **Product:** Agile product ownership · Roadmapping & prioritisation · KPI tracking · Stakeholder workshops
  - **Design & UX:** UI / UX design · Design systems · Brand identity · Accessible design (WCAG)
  - **Research:** Usability testing · Interviews & observation · A/B testing · Personas & synthesis

### 7. Home — Contact / Footer (`#contact`, `.footer`)
- Dark block (`background: --ink`), aura at ~0.5 opacity behind. Eyebrow `Let's talk`. Contact lines (`.foot-contact`, 18px, white links with subtle underline): email `johanna.janeback@hotmail.com` (`mailto:`) and phone **+46 72 358 49 67** (`tel:+46723584967`). Meta row: Location "Göteborg, Sweden"; Elsewhere → LinkedIn (**placeholder `#` — needs real URL**); Open to "UX · Accessibility · Product"; "Back to top ↑".

### 8. Case-study pages (`case-*.html`, shared `case.css`)
All four share one template. Top-left fixed **"← All work"** pill (`.case-nav-back`, links to `index.html#work`); the main nav is right-aligned with links back to `index.html#…`.
Structure per case:
- **Case hero** (`.case-hero`): aura top-right; eyebrow (lime square + "NN · Category · Satcube"); `H1` `.case-title` (serif `clamp(34px,5.6vw,76px)`, max 17ch — often one word italic for accent); dek (`.case-dek`, `clamp(18px,1.9vw,23px)`, `--ink-soft`, max 32ch); a **meta row** (`.case-meta`, flex, top hairline) of label/value pairs: My role · Company/Context · Year · Focus/Tools.
- **Body sections** built from a small kit: `.two-col` (sticky `H2` left / `.rich` prose right, stacks below 860px); `.pull` (large serif pull-quote with a lime highlight `.hl` = `linear-gradient(transparent 62%, var(--lime) 62%)`); `.steps` (numbered process — big outlined serif numerals via `-webkit-text-stroke`, tag + `H3` + copy, separated by hairlines); `.cards` (auto-fit min 240px highlight cards with a rounded icon tile); `.metrics` (big serif numerals — note: most "At a glance" metric blocks were intentionally removed); `.gal`/`.gal-2`/`.gal-3` (image galleries); figures + `figcaption`/`.cap` (14px `--muted`, `margin-top: 22px`); `.next-case` (dark rounded CTA banner linking to the next case — the four cases loop 1→2→3→4→1).
- **Per-case specifics:**
  - **Cognitive app** (case 1): phone mockups (`.phone` = dark rounded bezel around screenshot), activity-category chips (`.cat` pills with small icons), 5 process steps, 4 WCAG decision cards (one uses an inline 2×2-grid SVG), lessons.
  - **WebGUI** (case 2): hero = transparent-background laptop render `assets/webgui-dash.png`; results gallery `assets/webgui-cards.png` (transparent laptop) + two tooltip crops `webgui-2.png`/`webgui-3.png`. **Note:** the two laptop PNGs already have their surrounding background removed (transparent) — keep them transparent so they sit on the page color.
  - **Website** (case 3): hero = full Applications-page screenshot `assets/web-applications.png` (rounded, 1px border); design-system section shows two real pages side-by-side (`web-store.png`, `web-motion.png`).
  - **Brand** (case 4): primary logo lockup `assets/satcube-logo-tagline.png` on a `#f4f4f4` tile; mark gallery = icon-construction `satcube-icon.png` on light tile + light logo `satcube-logo-light.png` on a **#262626** tile; a **swatches** row (Medium grey `#616161`, Dark grey `#262626`, Light grey `#FAFAFA`, then accent gradients **Jord** `#C67C66→#EAADB1`, **Skog** `#80B67D→#AED37C`, **Vulkan** `#EABB90→#F3EB73`, **Horisont** `#81A6C8→#97D1D9`); an **Imagery** section (tall-left / stacked-right photo grid: `brand-img-desert.png`, `brand-img-forest.png`, `brand-img-responder.png`).

> Two further case pages exist in the source project (Art Direction with an IBC "Best Booth Design" finalist outcome, and a Merch case) but are **currently hidden** — not linked from the home grid or the case loop. They are out of scope for this handoff unless Johanna re-enables them.

---

## Interactions & Behavior
- **Sticky nav:** toggle `.is-stuck` at `scrollY > 40` (passive scroll listener).
- **Scroll reveal:** IntersectionObserver as described in "Motion". One-shot (unobserve after reveal).
- **Smooth in-page scroll** for `#`-anchor nav (disable under reduced-motion).
- **Hover affordances:** nav underline wipe; button lift + shadow + arrow nudge; case-card lift + shadow + arrow nudge; `case-link`/`case-nav-back` arrow translate.
- **No data fetching, forms, or auth.** Links are `mailto:`, `tel:`, in-page anchors, and page-to-page navigation. LinkedIn is a placeholder.
- **Responsive breakpoints used:** 860px (case two-col), 820px (about + cv + case-card), 760px (skills + nav links), 680px (brand imagery grid).

## State Management
Effectively none for production. The only stateful piece is the prototype **Tweaks panel** (see below) — exclude it.

## Tweaks panel — DO NOT SHIP
`index.html` loads React + Babel + `tweaks-panel.jsx` to render a small floating "Tweaks" control that lets a reviewer switch the hero layout (centered/split/**stacked**→"Editorial"), the accent "spark" color, and aura intensity. It writes `body[data-hero]`, `--lime`, and `--aura-strength`. **This is a design-review affordance only.** In production: pick the **split** hero, set the lime accent to the chosen value, drop React/Babel/the panel entirely, and bake the tokens in.

---

## Design Tokens
```
/* Ground */
--rose        #ECE4E2   /* page background */
--rose-deep   #E4D9D6
--rose-tint   #F4EEEC
--paper       #FBF8F7   /* cards */
--paper-edge  #EFE7E4   /* card borders */

/* Ink (plum-grey) */
--ink         #2B2535   /* headings */
--ink-soft    #4B4459   /* sub-headings */
--body        #5E576F   /* body text */
--muted       #8C8597   /* captions / meta */
--hair        rgba(70,63,87,0.14)  /* hairlines */

/* Accents */
--magenta     #E75FBF
--pink        #F07AA8
--peach       #F4A77E
--apricot     #F6C79A
--lime        #9CDE7B   /* spark accent (squares, bullets, chips) */
--lime-deep   #6FB94E   /* category labels */

/* Satcube brand (used inside case content) */
--navy        #112A46
--gold        #E9AD81

/* Radius */
--radius      18px      /* (cards use 20–26px; pills 999px) */

/* Easing */
--ease        cubic-bezier(.22,.61,.36,1)

/* Layout */
--maxw        1180px
--gutter      clamp(22px, 5vw, 80px)
```

### Typography
- **Display / headings:** **Adamina** (serif), weight 400. Google Fonts.
- **Italic display / accents & brand wordmark:** **Afacad** (ital 400–700). Google Fonts. Used for the hero role line, the nav wordmark, and italicized accent words.
- **Body / UI:** **Fustat** (weights 300–800). Google Fonts.
- Import: `https://fonts.googleapis.com/css2?family=Adamina&family=Afacad:ital,wght@0,400..700;1,400..700&family=Fustat:wght@300..800&display=swap`
- Base body: 18px / line-height 1.6. Headings `letter-spacing: -0.01em`, line-height ~1.04. `text-wrap: pretty` on paragraphs. `::selection` = magenta bg / white text.

### Spacing / shadow notes
- Section padding, gutters and most type sizes are fluid (`clamp()`), values inline above.
- Card hover shadow `0 30px 60px -34px rgba(43,37,53,0.4)`; button hover `0 12px 30px -12px rgba(43,37,53,0.5)`; device/figure shadows `0 30–40px 70–80px -40px rgba(43,37,53,0.45)`.

## Assets
All in `assets/` (copied into this bundle). Real photography/screenshots supplied by Johanna; logos are Satcube brand assets.
- `portrait.jpg` — B&W portrait (home about).
- Home card thumbnails: `card-grunden.jpg`, `card-webgui.jpg`, `card-website.jpg`, `card-brand.jpg`.
- WebGUI case: `webgui-dash.png`, `webgui-cards.png` (**transparent-background** laptop renders), `webgui-2.png`, `webgui-3.png` (tooltip crops), `webgui-phone.png` (currently unused).
- Website case: `web-applications.png`, `web-store.png`, `web-motion.png`, `web-resources.png` (resources unused).
- Brand case: `satcube-logo-tagline.png`, `satcube-logo-light.png` (white, for dark bg), `satcube-icon.png`, `brand-img-desert.png`, `brand-img-forest.png`, `brand-img-responder.png`.
- Cognitive-app case: `app-1.png`, `app-2.png`, and category icons `icon-music/nature/game/craft/party/film.png`.
- (Art-direction/merch assets `card-artdirection.jpg`, `card-merch.jpg`, `satcube-brand-letters.png`, `satcube-wordmark.png`, `website-header.png` exist but belong to hidden/legacy content.)
- **Icons** (experience/education + case decision cards) are inline stroked SVGs (`stroke: currentColor; stroke-width: 1.7`) — reproduce with the codebase's icon set (e.g. Lucide: rocket, pen/edit, users, mouse-pointer, compass, grid).

## Files in this bundle
- `index.html` — home page (all sections + the prototype Tweaks island).
- `case-cognitive-app.html`, `case-satcube-webgui.html`, `case-satcube-website.html`, `case-satcube-brand.html` — the four case studies.
- `styles.css` — global tokens, layout, nav, buttons, footer, aura, reveal, chips.
- `case.css` — case-study layer (hero, two-col, steps, cards, metrics, galleries, device frames, swatches, next-case).
- `tweaks-panel.jsx` — prototype-only Tweaks shell (exclude from production).
- `assets/` — all images & icons.

## Implementation suggestions
- Componentize: `Nav`, `Hero`, `About`, `CaseCard`, `WorkList`, `CvColumn`/`CvItem`, `SkillCard`, `Footer`, and a `CaseLayout` (hero + meta + the section kit: `TwoCol`, `Pull`, `Steps/Step`, `Cards/Card`, `Gallery`, `NextCase`). Case content is data — model each case as a structured object and render through `CaseLayout`.
- Promote the design tokens to CSS variables / a theme file. Keep the fluid `clamp()` values.
- Ship the **split** hero only; bake lime `--lime` and `--aura-strength: 0.92` (current saved value). Remove React/Babel/Tweaks.
- Keep the transparent WebGUI laptop PNGs transparent.
- Accessibility (this is a UX/accessibility-forward portfolio — hold it to a high bar): semantic landmarks (`header`/`nav`/`main`/`footer`), one `h1` per page, visible focus states, `prefers-reduced-motion` honored, alt text on all images, and AA+ contrast (the plum-on-rose body text already targets this).
