---
name: voxel-portfolio-hero
description: Build a single-file portfolio / landing page whose hero is an animated isometric voxel scene that endlessly collapses into scattered cubes and re-assembles into new shapes (towers, a letter monogram, a cloud, or any logo/icon), styled in a warm cream + brick-red editorial look with flourish script headlines, ink-wash colour blooms and monospace HUD labels. Use this whenever the user wants a personal portfolio, studio site or product landing page with a voxel / isometric / particle collapse-and-reform / "pixel cube" hero, asks to recreate the style of thinkclub.in, wants cubes that scatter and re-form into a logo, or wants to swap the shapes in an existing Phantom Epic page. Also use it when they ask to redesign the rotating shapes or restyle this page, even if they do not say "voxel".
---

# Voxel portfolio hero

A ready-made one-file page (`assets/template.html`, ~16 KB, no build step) with:

- **Hero**: headline in a flourish script font + serif italic subtitle, mono HUD labels, two buttons, and a canvas on the right that loops through 3 voxel shapes. Between shapes every cube drops to a random point on the floor (collapse), then flies to its slot in the next shape (reform). Click the canvas to trigger the next transition early. **Pointer lens**: a mouse or finger over the canvas bends space around it (cubes bulge and twist, faces deform) and washes the colours lighter and bluish, easing in and out.
- **Sections**: 4 work cards, a 4-step process row, a contact CTA, footer.
- **Look**: cream paper background, brick red accent, faint monospace glyph texture behind the hero, three drifting "ink-bleed" colour blooms, light + dark mode, safe-area padding for phones.
- Default content is the placeholder portfolio **Phantom Epic**; default shapes are *towers → PE monogram → cloud*.

## Workflow

1. Copy `assets/template.html` to the outputs folder under a sensible name. Do not rewrite it from scratch; edit it.
2. Personalise (see checklist below). Ask the user for name, projects and contact only if they have not given them; otherwise keep the placeholders and say so.
3. If the user wants different shapes, read `references/shapes.md` for the shape format and ready recipes (ghost, forest, floating castle, bitmap logos, italic wordmark), then replace the shape block in the `<script>`.
4. Syntax-check the script (extract it and run `node --check` if node exists).
5. Deliver: if an Artifact/publish tool is available, publish the single HTML (it is self-contained; the only external resource is the Google Fonts stylesheet, which is allowed). Otherwise present the file. For later tweaks prefer small targeted edits over regenerating.
6. Say plainly that you have not seen it rendered unless you actually did, and offer to adjust proportions.

## Personalisation checklist

| What | Where in the template |
|---|---|
| Site name | `<title>`, `.logo` (`PHANTOM<b>EPIC</b>`), `h1 .sc` (script word) and `h1 .se` (serif word), footer, `aria-label` |
| Eyebrow / lede / buttons | `.eyebrow`, `.lede`, `.cta` in `.hero` |
| Projects | the four `<a class="card">`; `--w` is the card's bloom colour, `.n` is the script numeral |
| Process steps | `.steps` |
| Contact | `mailto:` link in `#contact` |
| HUD text | `Module: …` line and `<span id="st">` initial state; keep them equal to the `NM` array in the script |
| Colours | CSS variables at the top (`--bg --ink --red --fa --fb` …), dark variants repeated twice (media query and `[data-theme="dark"]`) |
| Shapes | block between `function tower` and `const S=…` in the script |

Keep the script-font word short (one word, 5-9 letters); long words overflow on phones.

## How the animation works (needed when changing shapes)

- Grid is `G=16` cells. A voxel is `[x,y,z,colourKey]` with integer coordinates; colour keys are the keys of `PAL`, and each gets three pre-shaded faces (top ×1.06, left ×0.88, right ×0.70).
- Projection: `sx = ox + (x-y)*s`, `sy = oy + (x+y-G)*s/2 - z*s*1.1`. The camera sees the **top**, the **+y face (screen left)** and the **+x face (screen right)**. Draw order is ascending `x+y+z`.
- Flat things (logos, letters, text) live in the **x–z plane** and are extruded 2–4 cells in y; their front is the +y side, so text slopes down to the right. Use x from 0 to 15 only.
- Only build **shells** (hidden interior cubes are wasted). Duplicates are removed by the `S=[…].map(Map)` line, later voxels win.
- Each shape is sorted by `z` then `x+y`, and particle *i* is the *i*-th voxel of every shape, so columns rise to columns. Shapes with fewer voxels than `N` (the maximum) park the spare particles as small grey dust on the floor.
- Timing: `H=3.4` s hold, `M=2.7` s move. In the move each particle has a delay `d*0.3`, goes to its random floor point in the first half and to the next slot in the second half with a small arc.
- Vertical budget: scale is `min(w*.46/G, h/(G+10.5))`; keep the tallest voxel at `z ≤ 15`. Keep the largest shape under about 500 voxels for smooth 60 fps.
- Pointer lens: `warp(X,Y)` remaps every projected vertex inside radius `R=min(w,h)*.21` (magnify `d'=R*(d/R)^(1/(1+.9K))` plus a twist angle and a faint ripple); `poly()` mixes each face colour toward `LT` by up to 70% using the face centre's falloff; `glow()` adds a soft light disc. `K` eases toward `Kt` (1 while the pointer is down/over, 0 otherwise). Tune: radius factor `.063`, magnify `.9`, twist `.6`, lightness `.7`. Colours in `PAL`, `fa`, `fb`, `RD` are RGB arrays, not CSS strings. `.stage` uses `touch-action:pan-y` so touch can still scroll the page.
- `prefers-reduced-motion` renders one still frame of shape 0. Pointer events redraw that frame too.

## Hard-won rules

- Never add `localStorage`-style state or network calls; the page must stay self-contained.
- Fonts: Cormorant Garamond (serif), Pinyon Script (flourish), Inter (body), IBM Plex Mono (HUD), each with a real fallback stack. Do not use other hosts.
- Typographic apostrophes and quotes in visible text (’ “ ”).
- When turning a user's screenshot into voxels, say that it is an approximation (grid of 12–16 cells) and ask before using third-party logos on a public site (trademark).
- If the user writes Chinese, reply in Chinese; the page text stays in the language they ask for (default English).
