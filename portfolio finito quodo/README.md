# Handoff: Boglarka Dobos — Portfolio Site

## Overview
A personal portfolio for Boglarka (Bogi) Dobos, multimedia design student and barista. The site is a single-page app with eight views: a flower-field **Home** that acts as the navigation, four project pages (**2040**, **Blå Sol**, **Coffee & Connect**, **CPHFW**), an **Arts** gallery, a **Who Am I** about page, and a **Contact** page. A shared footer appears on every view except Home.

Navigation is unconventional and is the core idea: hand-drawn flowers drift slowly around the Home canvas and each flower is a link. There is no top nav bar.

## About the Design Files
The files in this bundle are **design references created in HTML** — a prototype that shows the intended look, copy and behaviour. They are **not production code to copy directly**.

The task is to **recreate these designs in the target codebase's environment** (React, Vue, Astro, plain HTML/CSS, etc.) using its established patterns. If there is no codebase yet, pick the framework that fits the project — for a portfolio of this size, a static site (plain HTML/CSS + a little JS, or Astro/Vite + React) is the appropriate choice; no backend is required.

`Portfolio.dc.html` is authored in a component runtime specific to the design tool: `{{ ... }}` are template holes, `<sc-if>` / `<sc-for>` are conditional/loop tags, `style-hover="..."` is a hover style, and the logic lives in a `class Component` at the bottom of the file. Read it as a spec, not as a source file to port line by line.

## Fidelity
**High fidelity.** Colors, typography, copy, imagery and interactions are final. Recreate the UI to match. All images in `assets/` are the real, final assets.

## Design Tokens

### Colors
| Role | Hex |
| --- | --- |
| Page background (cream) | `#e6e5e1` |
| Ink / body text | `#39351e` |
| Warm cream (text on dark grounds) | `#f4e3d3` |
| Green (accent, section kickers) | `#4b8f7d` |
| Footer green (ground) | `#2F6154` |
| Yellow (accent blocks, footer links) | `#eed25d` / `#EED25D` |
| Red (accent blocks, kickers) | `#d6483e` / `#D6493F` |
| Coffee & Connect page ground | `#f4e3d3` |
| Coffee & Connect accent | `#e8571b` |

Rules of use: cream ground + ink text is the default. Phase/section blocks inside case studies are filled with yellow, red or green; text on red and green is full-opacity `#f4e3d3` / `#e6e5e1` (never alpha-muted — contrast). Kickers (`h6`) are green on cream, cream on colour.

### Typography
- Headings: **Baloo 2** (weights 600, 800), loaded from Google Fonts.
- Body: the design-system body font (fallback: system sans-serif stack).
- Home title: 56px, letter-spacing 0.02em, uppercase.
- Page titles (`h1`): large display; the CPHFW page uses 120px, 2040 uses 180px with line-height 0.82.
- Body copy: 17px / line-height 1.5, `opacity: 0.85` on cream grounds.
- Figure captions: 13px, letter-spacing 0.02em, uppercase-ish; on colour blocks full opacity.
- Flower labels & kickers: 13–14px, letter-spacing 0.08–0.1em, uppercase.

### Spacing
The prototype uses a `--space-*` scale from its design system. Equivalent values: `--space-2` 8px, `--space-3` 12px, `--space-4` 16px, `--space-6` 32px, `--space-8` 64px. Page content is centred with `max-width: 1080px` (case studies 1160px) and `padding: 0 64px`.

### Radius & shadows
- Pills/bubbles and buttons: `border-radius: 999px`.
- Tool logo tiles: `border-radius: 18px`, `box-shadow: 0 2px 10px rgba(57,53,30,0.12)`; on hover `0 10px 24px rgba(57,53,30,0.22)`.
- Section dividers: 1px line, `background:#39351e; opacity:0.15`, inset by `--space-8`.
- Back buttons: 44px circle, 1px `#39351e` border, transparent fill.

### Flower clip-path
Photos on Who Am I are clipped to a seven-circle flower shape. Defined once as an SVG `clipPath` with `clipPathUnits="objectBoundingBox"`:
centre circle `cx .5 cy .5 r .3`; petals `r .22` at (.5,.24), (.725,.37), (.725,.63), (.5,.76), (.275,.63), (.275,.37). Applied with `clip-path: url(#flowerClip)`.

## Screens / Views

### 1. Home
- **Purpose**: landing page and the site's only navigation.
- **Layout**: a fixed **1440×900** stage, centred in the viewport and scaled down with `transform: scale(s)` where `s = min(vw/1440, vh/900, 1)`; parent has `overflow:hidden`. Ground `#e6e5e1`.
  - IMPORTANT: measure the container after layout. Never store a scale of 0 (guard `if (!w || !h) retry on next frame`), and re-measure with a `ResizeObserver` plus the window `resize` event — otherwise the stage renders at `scale(0)` and the page looks blank.
- **Centre**: the crown logo mark (`assets/logo-mark.png`, 104px) above `BOGLARKA DOBOS` (56px) and `Multimedia designer` (18px, uppercase, letter-spacing .15em, opacity .75). The whole centre block is `pointer-events: none` so it never blocks a flower.
- **Flowers** (each = image + uppercase label underneath at `margin-top:-6px`, whole unit clickable):
  | Label | Position | Flower | Size | Drift |
  | --- | --- | --- | --- | --- |
  | Who Am I | left 9%, top 3% | red | 240px | drift2 7.5s |
  | 2040 | right 11%, top 12% | yellow | 210px | drift1 7s |
  | Contact Me! | left 50% (translateX -50%), top 2% | blue | 150px | drift3 8.5s |
  | Arts | left 1%, top 48% | yellow | 220px | drift3 9.5s |
  | CPHFW | left 14%, bottom 10% | blue | 170px | drift2 9s |
  | Coffee & Connect | left 44%, bottom 4% | yellow | 190px | drift1 8s |
  | Festival App (Blå Sol) | right 13%, bottom 8% | red | 230px | drift2 8.5s |

### 2. Case-study pages (Blå Sol, Coffee & Connect, CPHFW)
Shared structure, all centred:
1. Fixed circular back button, top-left (24px inset), returns Home.
2. Decorative flower, absolutely positioned near the top-right, drifting.
3. Hero: green/red kicker (`h6`), big title, intro paragraph (max 60–62ch).
4. **CPHFW only** — a meta row above the intro divider: five columns `Client / Role / Duration / Date / Tools`, each a green `h6` label + 16px value.
5. "The challenge" / "The goal" in a two-column grid, red headings.
6. **Phase nav**: four flower buttons in a centred wrapping row (190×190 each, image absolutely filling, label centred on top at 20px/700/uppercase), drifting, anchored to `#<page>-discover|define|develop|deliver` with `scroll-margin-top: 24px` on the targets.
7. Four phase sections: kicker, `h3`, intro paragraph (max 70ch), then image figures with captions. Discover = yellow `#EED25D` block, Define = red `#D6493F`, Develop = green `#4B8F7D`, Deliver = plain cream.
   - Image grids must use `grid-template-columns: repeat(2, minmax(0,1fr))` and `min-width:0` on children, or fixed-ratio images force horizontal overflow.
8. CPHFW closes with two prototype buttons and a Team / Course / Deliverables credits row.

#### CPHFW content (the newest case study)
- Meta: Client *Copenhagen Fashion Week*; Role *UX Research, UI/UX Design, Prototyping*; Duration *8 weeks*; Date *November 2025*; Tools *Figma, FigJam, VS Code*. (Duration/Date are unconfirmed — check with Bogi.)
- Images, in order: `cphfw-fieldresearch.png`, `cphfw-brands.png`, `cphfw-persona.png`, `cphfw-moodboard.png` (Discover); `cphfw-vpc.png`, `cphfw-hmw.png`, `cphfw-requirements.png` (Define); `cphfw-lofi-info.png`, `cphfw-lofi-app.png` (Develop); `cphfw-mockup.png`, `cphfw-hifi-info.png`, `cphfw-hifi-app.png` (Deliver).
- Buttons: filled `#2F6154` with `#f4e3d3` label → `https://www.figma.com/proto/SG1uQa9POr3hz2lB50XD4Z/HIFI-CPHFW?node-id=1-28&scaling=scale-down&content-scaling=fixed&starting-point-node-id=1%3A2&page-id=0%3A1` ; outlined ink → `https://www.figma.com/proto/dqiGKoBfv8KPOZU3pp84JD/HIFI-APP?node-id=1-5&scaling=scale-down&content-scaling=fixed&starting-point-node-id=1%3A2&page-id=0%3A1`. Both `target="_blank" rel="noopener"`.

### 3. 2040
Editorial page (not a case study): red hero flower, kicker *Húsznegyven Egyesület · Budaörs, Hungary*, `2040` at 180px in green `#4b8f7d` with a small yellow flower overlapping the numerals, then three text-and-image spreads (Workshops, PIZITÍV, Print) alternating text/image columns.

### 4. Arts
Centred gallery of posters and sketches on cream, back button top-left.

### 5. Who Am I
Sections separated by one identical 1px divider each (no doubles, no gaps):
1. **Header** — back button in flow, top-left. The animated APNG `assets/barista-me-animated.png` (23 frames, plays natively as an `<img>`) sits top-right inside a **172×272 crop window** (`overflow:hidden`) with the image at `width:521px; max-width:none; left:-349px; top:0` so only the drawn figure shows — the source canvas is 1640×2360 and mostly empty. Two drifting flowers on the left. Title `WHO AM I` + tagline *Barista by day, designer always.* centred in ink.
2. **Intro** — flower-clipped photo (`general-photo.jpg`) beside "Hi, my name is BOGI, and I..." and a six-item list, each row a 20px flower bullet: *I'm 21, from Hungary / I'm a barista / I love art / I love working with colors / I enjoy rock music / I have a deep connection with architecture, especially modernism / I speak English, Hungarian and Danish / I study multimedia design*. Below: pill bubbles — bold, happy, art, colors, rock music, architecture, modernism, coffee, community (1px ink border, `border-radius:999px`, 13px).
3. **Why multimedia design?** — red kicker + a single 30px statement, centred, max 34ch.
4. **Team player / Personality / Family** — alternating photo + text sections (`team-photo.jpg`, `family-photo-cropped.jpg`), each with green kicker, `h2`, paragraph and pill bubbles. Team bubbles: good listener, out of the box thinker, good at connecting, empathethic, team builder, creative, good speaker, happy person, designer, colourful.
5. **Toolbox** — green kicker *Toolbox*, `h2` *What I design and build with*, a one-line centred row of **eight 64px logo tiles** (`gap:16px`, `flex-wrap:nowrap`, `padding-bottom:34px`). Each tile: 64×64, `border-radius:18px`, white ground, `overflow:hidden`, with the logo `object-fit:cover` and a per-logo `transform: scale()` that crops the white margin baked into each source file — Figma 1.18, Miro 1.34, Illustrator 1.22, Procreate 1.5, Canva 1.85, VS Code 0.82, GitHub 1.12, Claude 1.02. Hover scales the tile to 1.14 and deepens the shadow; the logo's name fades in underneath (12px uppercase, `opacity 0→1`, `translateY -6px→0`, 200ms). The caption is **absolutely positioned** (`top:100%; left:50%; translateX(-50%)`) so it never affects the tile's width — otherwise the row spaces unevenly.
6. **In five years…** — green kicker *Dreams*, text + flower-clipped photo (`dreams-photo.jpg`), bubbles (colors, design, gamification, architecture, interactive design, interactivity) and a red "Get in touch →" pill button that routes to Contact.

### 6. Contact
Full-height centred view: title *Contact me!*, then five drifting flowers used as links (150px each, image + uppercase label): **Facebook**, **LinkedIn**, **Instagram**, **Glassdoor**, **GitHub**. Facebook/LinkedIn/Instagram point at their sites in a new tab; Glassdoor and GitHub are still `href="#"` — **URLs needed from Bogi**.

### 7. Footer (every view except Home)
Ground `#2F6154`, cream type, `position:relative; overflow:hidden`.
- A 10px colour-block stripe spans the full width at the very top: flex children with `flex: 2 / 1 / 3 / 1 / 2` in yellow, red, cream, yellow, red.
- Two faint drifting flowers (yellow 180px right/top, red 120px right/bottom) at `opacity ~0.17`.
- Three columns (`1.2fr 1fr 1fr`): (a) crown logo mark rendered cream via `filter: brightness(0) invert(1) sepia(0.12) saturate(0.6)`, `BOGLARKA DOBOS` (26px, letter-spacing .06em), *Multimedia designer* in yellow, tagline *Barista by day, **designer always**.* (second half yellow), locations *Aarhus, Denmark · Budaörs, Hungary*; (b) nav — a yellow **Work** toggle (`+`/`−`) that collapses a list of 2040, Blå Sol, Coffee & Connect, CPHFW, plus yellow top-level links Arts, Who am I, Get in touch →; (c) *Contact me!* as a yellow pill, then `dobosbogi@icloud.com`, `+45 28 83 10 54`, Instagram, LinkedIn, Facebook.
- Bottom bar above a 1px cream rule at 25% opacity: `© 2026 Bogi Dobos · Multimedia designer` and *Back to the flowers ↑*.

## Interactions & Behavior
- **Routing**: single `page` state (`'home' | '2040' | 'festival' | 'coffee' | 'cphfw' | 'arts' | 'about' | 'contact'`), one view rendered at a time, no URL routing in the prototype. In a real build, give each view a real route/URL so pages are linkable and the browser back button works.
- **Flower drift** — three infinite keyframe loops, `ease-in-out`, applied with different durations (7–11s) so nothing syncs up:
  - `drift1`: 0/100% `translate(0,0) rotate(-4deg) scale(1)`; 50% `translate(14px,-18px) rotate(4deg) scale(1.08)`
  - `drift2`: 0/100% `translate(0,0) rotate(3deg) scale(1)`; 50% `translate(-16px,16px) rotate(-5deg) scale(0.92)`
  - `drift3`: 0/100% `translate(0,0) rotate(-3deg) scale(1)`; 50% `translate(10px,14px) rotate(5deg) scale(1.1)`
- **Tool tiles**: hover = scale 1.14 + shadow (220ms ease) and caption fade/rise (200ms ease).
- **Footer Work menu**: toggles open/closed, caret `−` when open (open by default).
- **Phase flowers**: in-page anchor jumps to the phase section.
- **Back buttons**: return to Home.
- Respect `prefers-reduced-motion` in the real build: the drifting flowers and the barista APNG should hold still for users who ask for reduced motion.

## State Management
- `page` — current view (see routing above).
- `homeScale` — computed Home stage scale; default 1, never 0.
- `workOpen` — footer Work menu; default `true`.
- `hoveredTool` — name of the hovered Toolbox logo, or `null`; drives that caption's opacity/offset only.
No data fetching, no forms, no auth.

## Assets
All in `assets/`. Everything is Bogi's own work: hand-drawn flowers (`flower-red/yellow/blue.png`), the crown logo (`logo-mark.png`), the animated barista APNG (`barista-me-animated.png`), personal photos (`general-photo.jpg`, `team-photo.jpg`, `family-photo-cropped.jpg`, `dreams-photo.jpg`), and project imagery for 2040, Blå Sol, Coffee & Connect, CPHFW and Arts. Tool logos (`tool-*.jpg`) are third-party brand marks used to indicate software proficiency — swap for official SVGs from each brand's press kit if you want crisper rendering.

## Files
- `Portfolio.dc.html` — the full prototype: all eight views plus the footer, and the logic class at the bottom of the file.
- `assets/` — every image the design uses, at final crop/size.

## Open items
- Glassdoor and GitHub profile URLs for the Contact flowers.
- Confirm the CPHFW duration (8 weeks) and date (November 2025).
