# # Headline Handoff Refinement

**Landing Page Hero**

_Started 2026-05-02 18:21 UTC_

---

## User



## Assistant

I'll prepare a complete implementation handoff package for Claude Code. The reference shows an original portfolio-style hero design (SNIR. brand) — this appears to be the user's own brand, so I'll create the handoff as specified.

Let me first view the reference image to capture exact details.

_[tool: view_image]_

I'll build a single self-contained HTML handoff document Claude Code can read directly. It includes layer order, exact specs for desktop (1440px) and mobile (390px), HTML/CSS scaffolding, and a QA checklist.

_[tool: write_file]_

_[tool: done]_

_[tool: fork_verifier_agent]_

Layer order (back → front): background → purple glow → BUILD word → rings → knight → text → topbar.

Handoff written to **Hero Handoff.html** with everything specified:

- Exact HTML structure (BEM, no `<br>` in headline)
- CSS architecture rules + skeleton
- Single 768px breakpoint, responsive rules above/below
- Desktop 1440 values: hero 1440×780, headline 104px/0.92/-0.035em, glow 760×760 right:-120/top:-40, BUILD 340px, rings 520×520 right:80/top:60, knight 620w right:-60/bottom:0
- Mobile 390 values: hero 390×720, headline 54px/0.95/-0.03em (4-line via max-width:220px), glow 340×340, BUILD 130px, rings 240×240, knight 280w
- Z-index map (1→7, gap at 5 reserved)
- Positioning specs for all eight layers
- Typography rules including the desktop 2-line vs mobile 4-line headline contract enforced by container max-width
- Color usage tied to tokens (`#F2EFEA`, `#0A0A0A`, `#6E3CFF`, glow rgba stops)
- Asset list (just `chess-knight.png`)
- Delete list + Do-not-touch list
- Desktop and mobile QA checklists

Visual previews of both viewports embedded so Claude Code has a reference render alongside the spec.

## User

Refine the handoff before sending it to Claude Code.

Do NOT redesign anything.

This is a precision correction pass.

---

1. HEADLINE LINE BREAKS (CRITICAL)

Do NOT rely on max-width wrapping.

Make line breaks deterministic.

Desktop MUST render exactly:
Understand deeply.
Build fast.

Mobile MUST render exactly:
Understand
deeply.
Build
fast.

Use explicit control (CSS or markup) so the result cannot change.

---

2. PROTECT TEXT AREA (STRICT RULE)

Add a hard rule:

No visual layer may enter the headline zone.

This includes:

* knight
* rings
* BUILD
* glow

Define a clear boundary:

Desktop:
Text zone = left 60% of viewport

Mobile:
Text zone = top 50% of viewport

Anything entering these zones = failure.

---

3. BUILD POSITION

Update spec:

* BUILD must start below "Bring a problem"
* BUILD must stay in lower half only
* BUILD must not touch headline

---

4. KNIGHT CONSTRAINTS

Add rules:

* Knight must stay bottom-right
* Knight must not cross into text zone
* Knight must not dominate layout
* Knight must be visually secondary to headline

---

5. RINGS CONSTRAINTS

Add rules:

* Rings must stay on right side only
* Rings must not overlap text
* Rings must support composition, not compete

---

6. DOT CONSISTENCY

Ensure purple square dots exist:

* after "deeply"
* after "fast"
* after "Bring a problem" (also on mobile)

---

7. FINAL QA ADDITION

Add explicit QA rule:

"If any element overlaps the headline or reduces readability, the implementation is invalid."

---

After updating the handoff:

Output the FINAL Claude Code prompt.

Claude Code must only implement, not think.

---

No redesign.
No interpretation.
Only enforce the reference.

## Assistant

_[tool: snip]_

I'll apply precision corrections to the handoff and add a final Claude Code prompt.

_[tool: read_file]_

I'll make targeted edits to the handoff to address each correction, then append the final Claude Code prompt.

_[tool: str_replace_edit]_

_[tool: snip]_

_[tool: done]_

Refinements applied. Section 0 now defines protected text zones as a hard rule (desktop left 60%, mobile top 50%); decorative numbers were re-tuned so knight, rings, BUILD, and glow peak all sit inside the right 40% / bottom 50% with explicit boundary math. Headline line breaks are now deterministic via per-word `<span>` + `flex-basis:100%` (no `<br>`, no max-width). BUILD repositioned bottom-right, anchored below the lead-in. Mobile lead-in now requires the purple square dot to match desktop. Master QA rule added: any overlap with headline = invalid.

Section 15 is the copy-paste Claude Code prompt — implementation only, all values inline, with the QA gates baked in.

## User

Final correction before implementation.

The handoff is strong, but there is one remaining contradiction.

You must remove ALL references to:

* max-width based line breaking
* natural wrapping

Line breaking must be 100% deterministic using flex-basis only.

---

Update rule:

Desktop:

* break happens ONLY before "Build" using flex-basis:100%

Mobile:

* every word has flex-basis:100%

There must be ZERO dependency on:

* container width
* font rendering
* browser wrapping behavior

---

2. KNIGHT SAFETY

Add one strict rule:

The knight must never visually dominate the headline.

Even if within bounds, if it feels heavier than the text → it is invalid.

---

3. BUILD VISUAL RULE

Add:

BUILD must feel like background depth, not a foreground word.

If it is readable before the headline → it is too strong.

---

After this, output ONLY the final Claude Code prompt.

Do not rewrite the whole document again.

## Assistant

_[tool: snip]_

Let me apply the corrections to the handoff document, then output only the final Claude Code prompt.

_[tool: grep]_

_[tool: str_replace_edit]_

_[tool: snip]_

```
You are implementing the SNIR. landing-page hero from a fixed spec.
DO NOT redesign. DO NOT improve. DO NOT reinterpret. DO NOT add elements.
DO NOT think about layout choices — every value below is binding.

Files to write:
  /styles/hero.css
  /index.html  (hero section only — leave the rest of the page untouched)

Required asset (must already exist):
  /assets/hero/chess-knight.png   (transparent PNG, black 3D chess knight)

=== TOKENS (CSS custom properties on :root) ===
--paper:#F2EFEA;
--ink:#0A0A0A;
--purple:#6E3CFF;
--purple-soft:#B9A4FF;
--muted:#5A5A5A;
Font stack: "Helvetica Neue", Helvetica, Arial, sans-serif.

=== PROTECTED ZONES (HARD RULE — read first) ===
Desktop 1440x780: text zone = left 0..864 (left 60%). NO decoration may have any visible pixel inside it.
Mobile  390x720:  text zone = top  0..360 (top  50%). NO decoration may have any visible pixel inside it.
Decoration = .hero__glow (peak alpha >= 0.20), .hero__build, .hero__rings, .hero__knight.
If any of those crosses the boundary, the implementation is INVALID.

=== LINE-BREAK RULE (100% DETERMINISTIC — no exceptions) ===
Line breaks come ONLY from `flex-basis:100%`. ZERO dependency on:
  - container width
  - font rendering
  - browser wrapping behavior
Forbidden: <br>, max-width-based wrapping, white-space tricks, word-break, content-visibility.
Desktop: exactly ONE break, before "Build", via `.w-build { flex-basis:100% }`.
Mobile:  every `.word` gets `flex-basis:100%` so each word is on its own line.

=== KNIGHT SAFETY (HARD RULE) ===
The knight must NEVER visually dominate the headline.
Even if every numeric bound is met, if the knight feels heavier than the headline → INVALID.
The headline is the primary element on the page. The knight is supporting imagery only.

=== BUILD VISUAL RULE (HARD RULE) ===
BUILD must read as background depth, not as a foreground word.
If "BUILD" is legible before the headline on first glance → it is too strong → INVALID.
Color is locked at rgba(110,60,255,0.10). Do not raise opacity, add stroke, add shadow, or add weight.

=== HTML (place exactly this; no extra wrappers, no reorder) ===
<section class="hero">
  <div class="hero__glow" aria-hidden="true"></div>
  <span class="hero__build" aria-hidden="true">BUILD</span>
  <svg class="hero__rings" viewBox="0 0 500 500" aria-hidden="true">
    <circle cx="250" cy="250" r="248" fill="none" stroke="#0A0A0A" stroke-width="1.25"/>
    <circle cx="250" cy="250" r="184" fill="none" stroke="#6E3CFF"  stroke-width="1"/>
  </svg>
  <img class="hero__knight" src="/assets/hero/chess-knight.png" alt="" aria-hidden="true"/>
  <div class="hero__topbar">
    <a class="hero__logo" href="/">SNIR<span class="dot"></span></a>
    <nav class="hero__nav">
      <a href="#">What I build</a><a href="#">Tools</a>
      <a href="#">Tracking</a><a href="#">How I operate</a>
    </nav>
    <a class="hero__cta" href="#contact">Let's talk<span class="dot"></span></a>
    <button class="hero__menu" aria-label="Open menu"><span></span><span></span><span></span></button>
  </div>
  <div class="hero__copy">
    <h1 class="hero__title">
      <span class="word">Understand</span>
      <span class="word w-deeply">deeply<span class="dot"></span></span>
      <span class="word w-build">Build</span>
      <span class="word w-fast">fast<span class="dot"></span></span>
    </h1>
    <p class="hero__sub">Paid media. <a href="#systems">Systems</a>. Scale.</p>
    <a class="hero__lead" href="#contact">Bring a problem<span class="dot"></span></a>
  </div>
</section>

=== CSS — DESKTOP (default) ===
.hero{position:relative;background:var(--paper);color:var(--ink);
  height:780px;overflow:hidden;
  font-family:"Helvetica Neue",Helvetica,Arial,sans-serif;}

/* TOPBAR (z 7) */
.hero__topbar{position:absolute;inset:0 0 auto 0;height:72px;padding:0 56px;
  display:flex;align-items:center;justify-content:space-between;z-index:7;}
.hero__logo{font-weight:900;font-size:22px;letter-spacing:-.02em;color:var(--ink);text-decoration:none;}
.hero__logo .dot{display:inline-block;width:8px;height:8px;background:var(--purple);margin-left:2px;transform:translateY(-2px);}
.hero__nav{display:flex;gap:32px;}
.hero__nav a{font-size:14px;font-weight:500;color:var(--ink);text-decoration:none;}
.hero__cta{display:inline-flex;align-items:center;gap:6px;background:var(--ink);color:#fff;
  padding:10px 18px;border-radius:6px;font-size:14px;font-weight:600;text-decoration:none;}
.hero__cta .dot{width:6px;height:6px;background:var(--purple);display:inline-block;}
.hero__menu{display:none;}

/* COPY (z 6) — NO max-width; width is content-driven only */
.hero__copy{position:absolute;left:96px;top:208px;z-index:6;width:max-content;}
.hero__title{margin:0;font-weight:900;font-size:104px;line-height:.92;letter-spacing:-.035em;
  display:flex;flex-wrap:wrap;column-gap:.22em;row-gap:0;}
.hero__title .word{display:inline-block;}
.hero__title .w-build{flex-basis:100%;} /* the ONLY line break */
.hero__title .dot{display:inline-block;width:14px;height:14px;background:var(--purple);margin-left:6px;transform:translateY(-2px);}
.hero__sub{margin:28px 0 0;font-size:20px;line-height:1.45;color:#2A2A2A;font-weight:400;}
.hero__sub a{color:var(--purple);font-weight:500;text-decoration:none;}
.hero__lead{display:inline-flex;align-items:center;gap:14px;margin-top:40px;
  font-size:14px;font-weight:500;color:var(--ink);text-decoration:none;}
.hero__lead::before{content:"";display:inline-block;width:32px;height:1.5px;background:var(--ink);}
.hero__lead .dot{width:6px;height:6px;background:var(--purple);display:inline-block;margin-left:4px;}

/* DECORATION (z 1..4) */
.hero__glow{position:absolute;z-index:1;pointer-events:none;
  width:640px;height:640px;right:-80px;top:60px;
  background:radial-gradient(closest-side,
    rgba(110,60,255,.55) 0%,
    rgba(110,60,255,.28) 38%,
    rgba(185,164,255,.08) 70%,
    transparent 100%);
  filter:blur(2px);}
.hero__build{position:absolute;z-index:2;pointer-events:none;
  right:80px;bottom:40px;
  font-weight:900;font-size:280px;line-height:.85;letter-spacing:-.05em;
  color:rgba(110,60,255,.10);} /* locked — do not strengthen */
.hero__rings{position:absolute;z-index:3;pointer-events:none;
  width:500px;height:500px;right:40px;top:80px;}
.hero__knight{position:absolute;z-index:4;pointer-events:none;
  width:540px;height:auto;right:-40px;bottom:0;object-fit:contain;display:block;}

=== CSS — MOBILE (@media (max-width:768px)) ===
.hero{height:720px;}
.hero__topbar{height:64px;padding:0 20px;}
.hero__logo{font-size:20px;}
.hero__logo .dot{width:6px;height:6px;}
.hero__nav, .hero__cta{display:none;}
.hero__menu{display:flex;flex-direction:column;justify-content:center;gap:4px;
  width:24px;height:16px;background:none;border:0;padding:0;margin-left:auto;}
.hero__menu span{display:block;height:2px;background:var(--ink);}

.hero__copy{left:24px;top:96px;} /* NO max-width */
.hero__title{font-size:54px;line-height:.95;letter-spacing:-.03em;column-gap:0;}
.hero__title .word{flex-basis:100%;}        /* every word on its own line */
.hero__title .dot{width:9px;height:9px;margin-left:4px;}
.hero__sub{margin-top:20px;font-size:14px;}
.hero__lead{margin-top:24px;font-size:12px;gap:10px;}
.hero__lead::before{width:24px;}
.hero__lead .dot{width:5px;height:5px;}     /* purple square IS present on mobile */

.hero__glow{width:360px;height:360px;right:-40px;top:380px;}
.hero__build{right:8px;bottom:300px;font-size:110px;}
.hero__rings{width:280px;height:280px;right:-40px;top:380px;}
.hero__knight{width:260px;right:-30px;}

=== Z-INDEX ===
glow=1, build=2, rings=3, knight=4, copy=6, topbar=7. Index 5 unused.

=== DELETE FROM CURRENT BUILD ===
- any background gradient on body or .hero
- any <br> inside the headline
- any max-width or width constraint on .hero__copy or .hero__title used to drive line breaks
- any framework grid wrappers (.container, .row, .col-*) inside .hero
- any animation, transition, parallax, reveal on hero children
- any backdrop-filter, mix-blend-mode, box-shadow on glow/rings/knight/build
- inline styles on hero markup
- position:fixed or position:sticky on the topbar
- any decorative SVG that is not the two-circle .hero__rings

=== DO NOT TOUCH ===
- headline wording, casing, punctuation
- ring SVG geometry (cx/cy/r/stroke values)
- token color values
- z-index map
- the 768px breakpoint (single breakpoint, no others)
- the deterministic flex-basis line-break technique
- BUILD opacity (rgba(110,60,255,0.10))

=== QA — must all pass ===
Zone integrity:
[ ] Desktop: no part of knight/rings/BUILD/glow-peak crosses x=864 going leftward.
[ ] Mobile:  no part of knight/rings/BUILD/glow-peak crosses y=360 going upward.
[ ] BUILD top edge sits below the lead-in line on desktop (y >= 480).

Headline:
[ ] Desktop renders exactly two lines: "Understand deeply." / "Build fast."
[ ] Mobile  renders exactly four lines: Understand / deeply. / Build / fast.
[ ] Line breaks come ONLY from flex-basis:100% — verified by deleting any container width and re-rendering: layout must be identical.
[ ] Purple square dots present after "deeply", "fast", "Bring a problem", logo, CTA label.

Visual hierarchy:
[ ] Headline is the dominant element on first glance.
[ ] Knight does NOT visually dominate the headline.
[ ] BUILD reads as background depth, NOT as a foreground word — it should be barely-there at first glance.

System:
[ ] Topbar (desktop): logo + 4 nav links + "Let's talk" CTA.
[ ] Topbar (mobile):  logo + hamburger only.
[ ] No animations, transitions, parallax, scroll effects.
[ ] No horizontal scroll at 1440 or 390.

MASTER RULE:
If ANY element overlaps the headline, reduces readability, dominates the headline, or makes BUILD readable before the headline → the implementation is INVALID. Re-tune until it passes.

Build it. Do not deviate.
```

