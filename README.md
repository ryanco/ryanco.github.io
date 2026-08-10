# Handoff: ryancolwell.com redesign

## Overview
A full visual redesign of ryancolwell.com — a personal writing site for Ryan Colwell, a
senior product & engineering leader in healthcare technology. The current site is a 2016-era
Jekyll blog on the Foundation/Sass starter theme. This handoff replaces the look and feel
entirely while keeping the site on **Jekyll + GitHub Pages** (repo: `ryanco/ryanco.github.io`,
branch `master`).

Audience the design targets: healthcare CIOs/CTOs and health-system execs, plus conference
organizers and podcast bookers. Primary job of the site: establish credibility — get them
reading his thinking. Homepage is essay-first.

## About the design files
`Ryan Colwell.dc.html` in this bundle is a **design reference created in HTML** — a prototype
showing intended look and behavior, not production code to copy directly. It opens in a browser
(with `support.js` beside it) and renders the full homepage.

Your task is to **recreate this design in the target codebase** — the existing Jekyll site —
using Jekyll's layouts/includes/data conventions. Do not ship the prototype file. Do lift its
exact values: hex/oklch colors, font stacks, sizes, spacing, and copy.

The prototype uses a small custom runtime (`<x-dc>`, `{{ }}` holes, `<sc-for>`, `<sc-if>`).
Ignore that machinery. The only pieces of it that matter:
- `<sc-for list="{{ archive }}">` = a loop → becomes a Liquid `{% for %}`
- `<sc-if value="{{ archiveOpen }}">` = the collapsible archive → becomes a tiny vanilla-JS toggle
- Everything else is plain HTML with inline styles.

Note: the prototype uses **inline styles on every element** because of how the design tool
streams. In Jekyll you should extract these into a single stylesheet with sensible class names.
The values are what's authoritative, not the delivery mechanism.

## Fidelity
**High-fidelity.** Colors, typography, spacing, and copy are final. Recreate pixel-accurately.

---

## Target implementation: Jekyll structure

Recommended file layout for the repo:

```
_config.yml
_layouts/
  default.html      # head, header nav, footer
  home.html         # hero → featured → archive → about → reading
  post.html         # article template (NOT yet designed — see "Open items")
_includes/
  post-card.html    # secondary post card (used in the 2-col grid)
_data/
  reading.yml       # reading list entries
_posts/             # existing 19 markdown posts
assets/
  css/main.css      # the whole design, one file
  img/ryan-colwell.jpeg
index.html          # layout: home
```

### Cleanup to do first
The repo currently carries the old Foundation/Sass pipeline — `stylesheets/app.css` (~338KB),
the Sass sources, and the associated `_config.yml` plugin config. **Delete all of it.** The new
design needs no framework and no CSS preprocessor; one hand-written `main.css` covers it.
Also bump to the current GitHub Pages Jekyll version.

### Front matter contract
Add these fields to posts so the homepage curates itself with no template edits:

```yaml
---
layout: post
title: "When In Doubt, Re-org"
date: 2013-12-08
featured: true              # promotes into the featured/secondary lists
hero: true                  # at most ONE post: renders in the large 2-col hero slot
category: "Organizational design"
blurb: "Twenty reorganizations in ten years. The good ones fix a named problem…"
---
```

- Hero slot: `site.posts | where: "hero", true | first`
- Secondary cards: `site.posts | where: "featured", true` minus the hero, limit 6
- Archive: all of `site.posts`, newest first

`blurb` is the hand-written summary shown on the homepage — it is deliberately NOT an
auto-excerpt. The copy for the seven curated posts is in "Content" below; paste those into
front matter verbatim.

---

## Design tokens

All colors are authored in **oklch**. Hex equivalents given for convenience; prefer the oklch
values (they're what the design was tuned in, and browser support is universal now).

| Token | oklch | ~hex | Use |
|---|---|---|---|
| `--bg` | `oklch(0.975 0.008 85)` | `#faf8f4` | page background, warm off-white |
| `--ink` | `oklch(0.24 0.014 70)` | `#2e2a26` | primary text, links |
| `--ink-2` | `oklch(0.36 0.014 70)` | `#4b463f` | hero-card body text |
| `--ink-3` | `oklch(0.38 0.014 70)` | `#514b44` | body paragraphs |
| `--ink-4` | `oklch(0.42 0.014 70)` | `#5a544c` | card body text |
| `--muted` | `oklch(0.5 0.012 70)` | `#6d675f` | nav, section labels |
| `--muted-2` | `oklch(0.52 0.012 70)` | `#736d64` | meta line under hero |
| `--muted-3` | `oklch(0.55 0.012 70)` | `#7c756c` | author/company sublines |
| `--muted-4` | `oklch(0.58 0.012 70)` | `#847d74` | footer |
| `--muted-5` | `oklch(0.6 0.012 70)` | `#8c857b` | card dates |
| `--muted-6` | `oklch(0.62 0.012 70)` | `#948d83` | timeline labels, counts |
| `--muted-7` | `oklch(0.65 0.012 70)` | `#9d958b` | archive dates |
| `--accent` | `oklch(0.52 0.13 32)` | `#a8503c` | eyebrow, category, link hover |
| `--accent-sel` | `oklch(0.9 0.06 32)` | `#f4d5cb` | `::selection` background |
| `--rule` | `oklch(0.88 0.012 75)` | `#e2ddd5` | primary hairlines |
| `--rule-2` | `oklch(0.9 0.01 75)` | `#e8e3dc` | secondary/interior hairlines |

**Typography**

- Display serif: **Newsreader** — Google Fonts, italic + roman, optical size 6..72, weights 300/400/500/600 + italic 300/400. Fallback `Georgia, serif`.
- Mono/UI label: **JetBrains Mono** — weights 400/500. Fallback `monospace`.
- Body sans: system stack — `'Helvetica Neue', Helvetica, Arial, sans-serif`. (No webfont; intentional.)

Font link:
```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Newsreader:ital,opsz,wght@0,6..72,300;0,400;0,500;0,600;1,6..72,300;1,400&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
```

**Type scale** (family / size / weight / line-height / letter-spacing)

| Role | Value |
|---|---|
| Wordmark | Newsreader / 20px / 400 / — / -0.01em |
| Nav link | sans / 13px / 400 / — / 0.04em, uppercase |
| Eyebrow | JetBrains Mono / 12px / 400 / — / 0.12em, uppercase, accent |
| H1 hero | Newsreader / `clamp(38px, 5.2vw, 64px)` / **300** / 1.1 / -0.02em |
| Hero lede | sans / 18px / 400 / 1.65 / — |
| Hero meta | sans / 15px / 400 / — / — |
| Section label (h2) | JetBrains Mono / 12px / 500 / — / 0.12em, uppercase |
| Featured H3 | Newsreader / `clamp(30px, 3.4vw, 42px)` / 400 / 1.15 / -0.015em |
| Featured body | Newsreader / 19px / 400 / 1.6 / — |
| Card H3 | Newsreader / 26px / 400 / 1.2 / -0.01em |
| Card body | Newsreader / 17px / 400 / 1.6 / — |
| Card date | JetBrains Mono / 11px / 400 / — / 0.06em |
| Archive title | Newsreader / 19px |
| Archive date | JetBrains Mono / 11px |
| About pull-quote | Newsreader / 22px / 400 / 1.6 |
| About body | sans / 16px / 400 / 1.75 |
| Timeline role | sans / 16px |
| Timeline company | sans / 14px |
| Reading title | Newsreader / 21px |
| Reading author | sans / 14px |
| Reading note | sans / 15px / 1.65 |
| Footer | JetBrains Mono / 12px |

**Layout**

- Container: `max-width: 1080px; margin: 0 auto`
- Page gutter: `padding: 0 32px` on the outer wrapper
- Base line-height: 1.5; `text-wrap: pretty` on all headings and body paragraphs
- `-webkit-font-smoothing: antialiased` on `html`
- No border-radius anywhere except the archive toggle button (`999px`)
- **No shadows, no gradients, no cards with fills.** Structure comes entirely from 1px hairlines and whitespace.

**Spacing rhythm** — header `28px` top; hero `88px` top / `72px` bottom; section top padding `64px` with `96px` margin above the rule; grid column gap `56px`; card vertical padding `36px`; reading item padding `24px`; footer `32px` top / `56px` bottom.

---

## Screens

### 1. Homepage (`index.html`, layout `home`) — the only designed screen

**Header** — flex row, `align-items: baseline`, `justify-content: space-between`, gap 24px, wraps.
Left: wordmark "Ryan Colwell" linking to `#top`. Right: nav — Writing / About / Reading / LinkedIn,
gap 26px, uppercase 13px, color `--muted`, hover `--accent`.

**Hero** (`#top`) — bottom rule `--rule`.
- Eyebrow: "Product & engineering leadership in healthcare"
- H1: "Turning complexity into practical decisions." — `max-width: 20ch`
- Lede paragraph, `max-width: 62ch`, color `--ink-3`
- Meta line: "Associate Principal, Product Management & Strategy at Vizient · Milwaukee, WI"

**Writing** (`#writing`)
- Label row: "Writing" (left) / "Selected" (right), both mono 12px, `margin-bottom: 48px`
- **Hero post**: a whole-block `<a>`, `display: grid; grid-template-columns: minmax(0,1fr) minmax(0,1fr); gap: 40px; align-items: start`. Left column = category (mono 12px, accent) + H3. Right column = blurb (Newsreader 19px) + meta line "8 Dec 2013 · Read →" (mono 12px). Bottom padding 44px, bottom rule.
- **Secondary cards**: `grid-template-columns: repeat(auto-fit, minmax(320px, 1fr)); gap: 0 56px`. Six cards, each a block `<a>`: date (mono 11px) → H3 (Newsreader 26px) → blurb (Newsreader 17px). Padding `36px 0`, bottom rule.
- **Archive toggle row**: flex, gap 20px, `padding-top: 32px`. Pill button — mono 12px uppercase, transparent bg, `1px solid --rule` at 0.8 lightness (`oklch(0.8 0.014 75)`), `border-radius: 999px`, `padding: 11px 20px`. Hover: border and text both → `--accent`. Label toggles "Browse full archive" ⇄ "Hide full archive". Beside it: "19 posts, 2013–2016" (mono 12px, `--muted-6`).
- **Archive list** (hidden by default): `grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 0 56px; padding-top: 28px`. Each row is a flex `<a>`, `align-items: baseline`, gap 16px, `padding: 13px 0`, bottom rule `--rule-2`; fixed 78px date column (mono 11px) + title (Newsreader 19px). All 19 posts, **newest first**.

**About** (`#about`) — `margin-top: 96px`, top rule, `padding-top: 64px`.
Two columns: `minmax(240px, 300px) minmax(0, 1fr)`, gap 56px, `align-items: start`.
- Left: square headshot — `aspect-ratio: 1; object-fit: cover; filter: grayscale(1) contrast(1.02)`. Below it, a mono 12px stack (gap 8px) of email / LinkedIn / GitHub links.
- Right: pull-quote (Newsreader 22px), two body paragraphs (`max-width: 66ch`), then the timeline.
- **Timeline**: rows of `grid-template-columns: 110px minmax(0,1fr); gap: 24px; padding: 18px 0`, hairline between rows, top rule on the group, no rule on the last row. Left cell = mono 12px `--muted-6`. Right cell = role (16px) + company/detail (14px, `--muted-3`, `margin-top: 3px`).

**Reading** (`#reading`) — same section chrome as About.
Label row: "Reading" / "Books worth the time". Grid `repeat(auto-fit, minmax(300px, 1fr))`, gap `0 56px`. Each item: `padding: 24px 0`, **top** rule `--rule-2`; title (Newsreader 21px) → author (14px `--muted-3`) → note (15px/1.65 `--ink-4`).
→ Drive this from `_data/reading.yml` with keys `title`, `author`, `note`.

**Footer** — `margin-top: 96px`, top rule, `padding: 32px 0 56px`, flex space-between, wraps, mono 12px `--muted-4`. Left: "Ryan Colwell · West Bend, Wisconsin". Right: Email / LinkedIn / RSS, gap 22px.

### 2. Post page (`_layouts/post.html`) — NOT YET DESIGNED
See "Open items". Until it's designed, build it from the homepage vocabulary: same container and
gutters, title in Newsreader 300 at `clamp(34px, 4.4vw, 52px)`, date in mono 12px above it,
body in Newsreader 19px/1.7 at `max-width: 68ch`, same rules and accent for inline links.

---

## Interactions & behavior

- **Archive toggle** — the only stateful element. Click flips visibility of the archive grid and swaps the button label. One boolean. In Jekyll: render the full list server-side inside a `hidden` container and toggle with ~6 lines of vanilla JS. Do not require JS to reach the content — if you can do it with `<details>`/`<summary>` styled to match the pill, prefer that.
- **Links** — default `--ink`, no underline; hover `--accent`. No transition specified; a `0.15s ease` color transition is acceptable.
- **Post cards** — the whole card is the click target. No hover treatment beyond the inherited link-color change on the title. Resist adding lift/shadow/background-fill hovers; the design is deliberately flat.
- **`::selection`** — background `--accent-sel`.
- **Responsive** — every grid already uses `auto-fit`/`minmax`, so cards reflow to one column naturally. Two things need explicit breakpoints when you write real CSS:
  - Featured hero post: collapse the 2-col grid to one column below ~720px.
  - About: collapse to one column below ~760px (headshot above text).
  Header nav wraps on its own. Type is already fluid via `clamp()`.

## State management
One boolean (`archiveOpen`, default `false`). Nothing else. No data fetching — everything is static at build time.

## Assets
- `img/ryan-colwell.jpeg` — headshot, supplied by Ryan. Rendered square, cropped with `object-fit: cover`, desaturated via CSS `grayscale(1) contrast(1.02)`. **Keep the filter in CSS**, don't bake a grayscale copy — the color original should stay in the repo.
- Fonts load from Google Fonts (link above). If the client prefers self-hosting, both families are OFL-licensed.
- No icons, no logos, no illustrations. The design intentionally has zero decorative imagery.

---

## Content

### Hero
- Eyebrow: **Product & engineering leadership in healthcare**
- H1: **Turning complexity into practical decisions.**
- Lede: *I lead large product and engineering organizations in healthcare and senior living — accountable for delivery predictability, quality, and platform strategy. I write here about building teams that operate with confidence, and about what it actually takes to make software reliable in regulated environments.*
- Meta: *Associate Principal, Product Management & Strategy at Vizient · Milwaukee, WI*

### Featured post — hero slot
**When In Doubt, Re-org** — 8 Dec 2013 — category *Organizational design*
blurb: *Twenty reorganizations in ten years. The good ones fix a named problem; the bad ones are messaged as if they did. What I'd want to see instead: organize teams organically, roll change out inclusively, and hold the last set of changes accountable before announcing the next.*

### Featured posts — secondary cards (in this order)
1. **Tech Conference Talk Anti-Patterns** — 9 Jun 2013 — *War stories, walls of text, and eighty percent spent restating the problem — plus the audience habits that waste a room's time.*
2. **If Disney Ran Your Hospital** — 28 Nov 2013 — *Courtesy over efficiency, and decentralizing the authority to say yes. Notes on Fred Lee's case for changing culture rather than scripts.*
3. **Being Principled** — 21 Jul 2013 — *Practices are cheap to adopt and easy to cargo-cult. Principles are what let a team decide well when the practice doesn't fit.*
4. **Personal Retros** — 3 Jan 2014 — *Running the same inspect-and-adapt loop on yourself that you ask of your teams — and being honest in the write-up.*
5. **The Antiques Roadshow Rule** — 20 Dec 2013 — *What a system is worth has little to do with what it cost to build, and everything to do with what someone will do with it today.*
6. **Practical Practice** — 21 Mar 2016 — *Mastery rarely comes from rote exercises. It comes from iterating on real work you care about — in code or anywhere else.*

### Archive — all 19 posts
Rendered newest-first. Note that four titles were **cleaned up** from the originals; update the
post front matter to match so the archive and featured lists agree:

| Date | Title | Slug |
|---|---|---|
| 2013-06-09 | Tech Conference Talk Anti-Patterns | `2013/06/09/tech-confrence-talk-anti-patterns` |
| 2013-07-21 | Being Principled | `2013/07/21/being-principled` |
| 2013-07-25 | Jeff Testworthy | `2013/07/25/jeff-testworthy` |
| 2013-09-26 | The Phoenix Project | `2013/09/26/The-Phoenix-Project` |
| 2013-10-27 | Migrating From Scriptogram | `2013/10/27/Migrating-From-Scriptogram` |
| 2013-11-10 | Trying Out Sketchnoting | `2013/11/10/trying-out-sketchnoting` |
| 2013-11-17 | Learning About Sass | `2013/11/17/learning-about-sass` |
| 2013-11-21 | Visual Studio 2013 Launch Event | `2013/11/21/visual-studio-2013-launch-event` |
| 2013-11-24 | Customers For Life | `2013/11/24/customers-for-life` |
| 2013-11-26 | The Five Dysfunctions of a Team ← *was "5 Dysfunctions of a Team"* | `2013/11/26/5-dysfunctions-of-a-team` |
| 2013-11-28 | If Disney Ran Your Hospital | `2013/11/28/if-disney-ran-your-hospital` |
| 2013-11-30 | The Lean Startup | `2013/11/30/the-lean-startup` |
| 2013-12-08 | When In Doubt, Re-org ← *was "When In Doubt -- Re-org"* | `2013/12/08/When-In-Doubt--Reorg` |
| 2013-12-12 | Growing | `2013/12/12/Growing` |
| 2013-12-20 | The Antiques Roadshow Rule ← *was "Antiques Roadshow Rule"* | `2013/12/20/Antiques-Roadshow-Rule` |
| 2014-01-03 | Personal Retros | `2014/01/03/Personal-Retros` |
| 2014-01-16 | Just 3 Things — Productivity ← *was "Just 3 Things - Productivity"* | `2014/01/16/Just-3-Things---Productivity` |
| 2014-03-20 | Driven | `2014/03/20/Driven` |
| 2016-03-21 | Practical Practice | `2016/03/21/Practical-Practice` |

**Preserve every existing URL.** The slugs above are live permalinks; keep `_config.yml`'s
permalink style as-is so nothing 404s.

### About
Pull-quote: *I'm a senior product and technology leader who builds organizations that deliver — reliably, at scale, and with purpose.*

Para 1: *My work focuses on turning complexity into durable systems: teams that operate with confidence, products that unlock real customer value, and platforms that compound improvement over time. I'm known for bringing order to chaos — establishing predictable delivery, making quality a first-class engineering responsibility, and building governance models that scale without slowing people down.*

Para 2: *A defining principle in my leadership is being AI-first, but not AI for AI's sake. I use AI where it meaningfully expands capability — automating expert judgment, improving compliance, and unlocking outcomes that weren't practical before — rather than as a novelty. Twenty-five years in regulated healthcare, from patient monitors at GE to AI-driven platforms in senior living, taught me that the interesting problem is rarely the technology.*

Timeline rows:
| Label | Role | Detail |
|---|---|---|
| 2026– | Associate Principal, Product Management & Strategy | Vizient |
| 2011–2026 | Senior Director of Engineering, Spend Management | Direct Supply · 70-person product and engineering org; launched DSSI Menu AI |
| 2001–2011 | Senior Software Engineer | GE Healthcare · patient monitoring, embedded and validation systems |
| Patents | Automated dietary management in healthcare facilities · Managing inventory associated with a facility | |
| Education | B.S. Biomedical Engineering, Marquette University | |

Contact links: `ryangcolwell@gmail.com` · `linkedin.com/in/ryangcolwell` · `github.com/ryanco`

### Reading (`_data/reading.yml`)
```yaml
- title: The Phoenix Project
  author: Gene Kim, Kevin Behr, George Spafford
  note: >-
    The book that makes flow, WIP, and constraints legible to people outside
    engineering. Still the fastest way to align a leadership team.
- title: If Disney Ran Your Hospital
  author: Fred Lee
  note: >-
    Courtesy over efficiency, and pushing the authority to say yes out to
    whoever is facing the customer.
- title: The Five Dysfunctions of a Team
  author: Patrick Lencioni
  note: >-
    A blunt model for why capable teams underperform. Most of the time it
    starts and ends with trust.
- title: The Lean Startup
  author: Eric Ries
  note: >-
    Useful less for startups than for large organizations trying to learn
    something before committing a year of capacity.
- title: Getting to Yes
  author: Roger Fisher, William Ury
  note: >-
    Interests over positions. Most cross-functional deadlock is a positions
    problem wearing a roadmap costume.
- title: Wool
  author: Hugh Howey
  note: >-
    Not a leadership book. A very good one about what systems do to the
    people living inside them.
```

---

## Open items — confirm with Ryan before assuming

1. **Post page is not designed yet.** Build it from the homepage vocabulary (guidance above) or ask for a design pass first.
2. **Post `blurb` values only exist for the 7 curated posts.** The other 12 appear in the archive as title-only, which is by design.
3. **Old theme removal** — the Foundation/Sass teardown is assumed. Confirm nothing else in the repo depends on it.
4. **`feed.xml`** — the footer links to it; make sure it survives the theme swap.
5. **Homepage currently links to the live `http://ryancolwell.com/...` URLs** in the prototype so the links work standalone. In Jekyll these become `{{ post.url | relative_url }}`. Also consider moving the site to `https`.

## Screenshots (`screens/`)
Reference captures of the prototype at ~920px viewport width. Use these for look and feel;
use the token tables above for exact values (the captures are re-rendered and slightly soften
type). Two known capture artifacts, not design intent: the headshot is missing from
`04-about.png` (it renders fine in the browser — see `img/ryan-colwell.jpeg`), and Newsreader
may fall back to Georgia in places.

- `01-hero.png` — header + hero
- `02-writing-featured.png` — Writing label row and the featured hero post
- `03-writing-cards-archive.png` — secondary cards, archive toggle, archive open
- `04-about.png` — About two-column layout
- `05-reading-footer.png` — Reading grid

## Files in this bundle
- `Ryan Colwell.dc.html` — the design prototype. Open in a browser to see the real thing.
- `support.js` — runtime the prototype needs to render. Not part of the deliverable.
- `img/ryan-colwell.jpeg` — headshot asset, use as-is.
- `screens/*.png` — reference screenshots.
