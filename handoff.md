# Handoff — Luisa Monâ Website

## Goal
Build a static website for the fashion brand **Luisa Monâ**. The site is aesthetic-first: ASCII art, Courier New 900-weight fonts, off-white (#F1ECE4) background. No frameworks, no build tools — pure HTML/CSS/JS.

---

## What We're Building

**Two main states on the landing page (index.html):**
1. **Front page** — full-screen looping ASCII art GIF (`new.gif`, 2044×1152px), centered logo, "CLICK TO EXPLORE" cursor label that follows the mouse and snaps to a character grid, CONTACT (top-right) and MENU (top-left) buttons.
2. **Store page** — plain #F1ECE4 background with just the logo, CONTACT, and MENU. Revealed after a wave animation triggered by clicking while "CLICK TO EXPLORE" is visible.

**Category stub pages** (MEN, WOMEN, KIDS, BAGS, ACCESSORIES, COLLECTIONS, SCENTS, FURNITURE, EXHIBITIONS) — each is a minimal page with the same header (logo → index.html, MENU, CONTACT) and a centered page title. All share the same sidebar nav.

---

## Current State (end of last session)

- `index.html` — fully implemented: GIF background, cursor label with grid-snapping and edge-stacking behavior, wave transition animation, store page reveal, sidebar, MENU/CONTACT buttons
- All 9 stub pages exist and are working
- Sidebar slides from the **left**, dims the page behind it (rgba 0,0,0,0.4 overlay)
- MENU and CONTACT are vertically aligned with the logo center (−15px offset)
- Logo links back to index.html on all pages
- CLAUDE.md exists to remind Claude to conserve credits

**Everything appears complete and working.** No outstanding bugs were open at the end of the session.

---

## Active / Recently Modified Files

| File | Status |
|------|--------|
| `index.html` | Primary file — all logic lives here |
| `men.html` | Stub — last fixed (MENU bold bug) |
| `women.html` | Stub — fixed same session |
| `kids.html` | Stub — fixed same session |
| `bags.html` | Stub — fixed same session |
| `accessories.html` | Stub — fixed same session |
| `collections.html` | Stub — fixed same session |
| `scents.html` | Stub — fixed same session |
| `furniture.html` | Stub — fixed same session |
| `exhibitions.html` | Stub — fixed same session |

---

## What Was Changed (chronological summary)

1. Built index.html from scratch: GIF background, logo, cursor label
2. Added character grid snapping (CELL_W=10, CELL_H=16, OFFSET_X=137, OFFSET_Y=311)
3. Added CONTACT button (top-right), MENU button (top-left) with hamburger lines
4. Added left-slide sidebar with full nav + overlay dimming
5. Added wave transition: sin-wave canvas sweeps up from bottom, reveals store page
6. Wave oscillates side-to-side (one full cycle in ~4s), extra canvas padding prevents edge gaps
7. Added all 9 stub pages from a bash heredoc template
8. Logo wraps in `<a href="index.html">` on all pages
9. Fixed: `sed` substitution corrupted "MENU" → "KIDSu" on kids.html (rebuilt from template)
10. Fixed: sidebar was opening on the right — changed to `left:0; transform:translateX(-101%)`
11. Fixed: "MENU" label inside sidebar was not bold on stub pages — added combined `#sidebar-label, #sidebar-close` CSS rule to all stub pages

---

## What Failed and Why

| Problem | Root Cause | Fix |
|---------|-----------|-----|
| `kids.html` showed "KIDSu" in sidebar | `sed s/MEN/${page^^}/g` matched inside "MENU" → "KIDS" + leftover "u" | Rebuilt all stubs from a clean heredoc that never substitutes within "MENU" |
| Sidebar opened on the right | Initial CSS used `right:0; translateX(101%)` | Changed to `left:0; right:auto; translateX(-101%)` |
| "MENU" text in sidebar not bold on stub pages | Stubs only had `#sidebar-close` CSS, no `#sidebar-label` rule | Added combined `#sidebar-label, #sidebar-close { font-weight:900; display:inline-block; vertical-align:middle; }` to all stubs |
| Duplicate CSS rule after fix | Old separate `#sidebar-label` rule was left in index.html after adding the combined rule | Removed the duplicate |
| `elapsed` declared twice in wave script | Accidentally added a second `var elapsed` inside the draw loop | Removed the duplicate declaration |

---

## Next Steps

No open bugs at handoff. Possible directions:

- **Populate category pages** — add actual product images/grids to MEN, WOMEN, etc.
- **CONTACT button** — currently has no action; could open a modal or link to a contact page
- **Mobile responsiveness** — site was built desktop-first; sidebar and cursor label may need mobile handling (touch events, different layout)
- **Performance** — `new.gif` is large; consider replacing with a video (`<video autoplay loop muted>`) for better performance
- **Animations on stub pages** — currently stub pages are plain; could add entrance animations

---

## Key Constants / Values to Know

```
GIF size:        2044 × 1152 px
Grid cell:       CELL_W=10, CELL_H=16
Grid offset:     OFFSET_X=137, OFFSET_Y=311  (in GIF-space)
Ease factor:     0.12 (lerp for cursor label)
Wave duration:   PHASE_DUR=700ms per phase (total ~1.4s)
Wave constants:  SIN_FREQ=0.38, SIN_AMP_TOP=3, SIN_AMP_BOT=2
                 RIPPLE_SPEED=0.004, OSC_AMP=160, OSC_FREQ=(2π)/4000
                 EXTRA_PX=200 (canvas overhang each side)
Background:      #F1ECE4
Font:            'Courier New', Courier, monospace; weight 900
z-index layers:  GIF=1, store=2, cursor label=5, logo/buttons=10,
                 sidebar overlay=20, sidebar=30, wave canvas=50
```
