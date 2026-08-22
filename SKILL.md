---
name: grasshopper-icons
description: Draw or review icons in the Grasshopper style, David Rutten's 24x24 pixel-grid method. Use when creating component icons for Grasshopper or Rhino plugins, or any icon set that should match the original Grasshopper look. Also the guide to fetching any original Grasshopper icon from this repo by component name.
---

# How the Grasshopper Icons Were Drawn

Every component icon in Grasshopper was drawn by one person, **David Rutten**, by hand, on a 24-pixel canvas, in Xara. We recovered his original vector sources, converted them to SVG, and pulled together everything he's written about how he does it. This document is the result, written for humans and agents alike.

**How to read this:** anything under a **"David's rules"** heading is paraphrased from his own writing, with the source linked every time. The occasional phrase in quotation marks is his exact wording. Anything under **"Our notes"** is us: analysis of his source files, or our own additions. When in doubt, assume the good ideas are his and the mistakes are ours.

His main writings on this, all worth reading in full:

- ["On Icons"](https://ieatbugsforbreakfast.com/2012/07/12/on-icons/): his blog, 2012. The philosophy, his rules, and a step-by-step walkthrough.
- [Grasshopper Icons](https://developer.rhino3d.com/guides/grasshopper/grasshopper-icons/): the official guide page where he published the Xara sources.
- [How to draw a grasshopper style icon?](https://discourse.mcneel.com/t/how-to-draw-a-grasshopper-style-icon/74786): his most compact summary of the method, on the McNeel forum.
- [Font used in GH component icons](https://www.grasshopper3d.com/forum/topics/font-used-in-gh-component-icons): how he handles text at 24px.
- [Grasshopper 2 Icon Reference Chart](https://discourse.mcneel.com/t/grasshopper-2-icon-reference-chart/169859): the newer scriptable pipeline inside Rhino.

---

## What an icon is for

**David's rules** (from ["On Icons"](https://ieatbugsforbreakfast.com/2012/07/12/on-icons/)):

- Icons are functional, not decorative. They help you *find* things, they *tell* you when something happens, and they *show* you the state of the app.
- An icon doesn't need to describe its feature. It needs to be **memorable and recognizable**. Describing things is what tooltips are for. The floppy-disk Save icon outlived floppy disks and nobody minds.
- **Consistency beats metaphor.** Almost any distinctive shape works if you apply it consistently, and metaphors don't travel across languages and cultures anyway.
- Icons are not art. Every pixel either communicates or gets out of the way.
- Colour is for navigation. He criticizes greyscale toolbars for throwing that away, and colour that only shows up on hover arrives *after* you've found the tool, which defeats the point.
- The line of his we keep coming back to: "icons are conduits of love from developers to users." Users never see your code. The pixels are the only part of your work they actually touch.

## The canvas

**David's rules** (from the [official guide](https://developer.rhino3d.com/guides/grasshopper/grasshopper-icons/) and [his forum post](https://discourse.mcneel.com/t/how-to-draw-a-grasshopper-style-icon/74786)):

- **24x24 pixels, fixed.** Keep roughly a 2px empty border on all sides, so the artwork lives in about 20x20.
- Work vector, but on top of a pixel grid: **every vertex sits on a pixel centre**, never on a boundary between pixels. This is the whole secret to crisp 1px lines.
- **Redraw for every target size.** Never draw one big master and scale it down.

## Line

**David's rules:**

- Lines are "one or two pixel thick lines, aligned with the pixel grid" ([forum post](https://discourse.mcneel.com/t/how-to-draw-a-grasshopper-style-icon/74786)).
- **Never near-vertical or near-horizontal lines.** Anti-aliasing can't save them, they render as stepped fuzz. True verticals, true horizontals, honest diagonals.
- **No black outlines.** Outline each region in a **darker version of its own fill colour**. (His earlier icons used black. He explicitly stopped recommending it.)
- **Outline the silhouette only.** Interior creases and face boundaries are separated by fill value, not by line.
- His signature flourish, which he calls personal taste rather than a rule: outlines taper thinner where they terminate.
- For a harsh edge that still won't smooth: a one-pixel edge at 5-10% opacity along the transition, which he calls his "uber-anti-aliasing" trick.

**Our notes:** the pixel-centre rule applies to interior detail as much as to
silhouettes. A 1px stroke that lands between pixels renders as two grey ones,
and at 24px a few of those make the whole icon look muddy. Detail has a hard
budget: one bold object, at most a few interior marks, nothing thinner than
1px, dots at least 3px across. When a drawing looks worse than the originals,
the fix is almost always removing strokes, not refining them.

## Colour

**David's rules:** one colour family per icon. Two is already pushing it.

**Our notes:** these are the dominant values we extracted from his actual Xara files, per category:

| Category | Values | What they are |
|---|---|---|
| Surfaces / Transform / Vector | `#FFC200` to `#FF7900`, `#ED6B00`, outlines `#380000` / `#6E0000` | the signature amber to burnt-orange gradient bodies |
| Meshes | `#FF9E05` to `#D72000`, `#8E1500` | hotter, shifted toward red |
| Math | `#00C860`, `#024F00`, `#252C6E`, `#E10000` | greens, ink blue, signal red |
| Params | `#454545` to `#C5C5C5`, black, white | the near-greyscale hexagons |
| Shared accents | `#FFEA00` (selected), `#C9F99A` / `#85DA19` (preview green), `#FF7F7F` (annotation pink) | |

Also worth noticing: no saturated RGB primaries as body colours, and his area-covering "blacks" are usually a warm `#191919` rather than `#000000`.

## Light and depth

**David's rules** (from ["On Icons"](https://ieatbugsforbreakfast.com/2012/07/12/on-icons/) and the [forum post](https://discourse.mcneel.com/t/how-to-draw-a-grasshopper-style-icon/74786)):

- **Light always comes from the upper left.** Never varies across the set.
- **Large regions get subtle gradients**, never flat fills. The gradients indicate lighting and shadowing, and "keep it subtle."
- Shadows are very faint, fade out gradually, and don't need to be geometrically correct. A wrong-but-pleasing shadow beats a correct-but-ugly one.
- Fake soft curvature with one-pixel transition lines: one in an in-between tone, one as a highlight.
- Real materials (cardboard, wood, paper) get a faint noise pass at the end.

**The drop shadow** is applied *after* export, in a pixel editor, never in the vector tool (vector shadow effects look wrong at this size):

```
offset:  +1px right, +1px down
blur:     2px
colour:   black
opacity:  65-85 / 255  (about 25-33%)
```

**Our notes:** he published 65/255 in the official guide and quoted 85/255 later from memory on the forum. We treat 65 as canonical and anything in that band as authentic.

## The details he sets by hand

**David's rules:**

- **Dashed lines and checkerboards are placed pixel by pixel**: individual squares, positioned exactly. No dash tools, no pattern fills.
- Avoid text if you can ("icons without text work best" is written right on his template). When unavoidable: big letters are Times New Roman, hand-nudged onto the pixel grid. Small letters and numerals use a **custom font he drew at exactly 7px tall** (squeezed to 6 or 5px when needed). The full character set is in the template legend, `icons/template.svg`.

## Families and mimicry

**David's rules** (from the [official guide](https://developer.rhino3d.com/guides/grasshopper/grasshopper-icons/) and [this thread](https://discourse.mcneel.com/t/why-the-icons-use-in-grasshopper-tutorials-and-not-the-full-names-display-mode/55799)):

- Related components share icon *structure* while the data type varies. Curve offset, surface offset and plane offset visibly read as the same operation on different things, which tells you more than the word "Offset" ever could.
- Recurring concepts reuse the exact same glyphs everywhere: the arrows, the white-dot control points, the planes.
- If you're making a third-party icon that should blend in: **mimic** the set's line weights, contrast, saturation, and its arrow and point symbols. Don't invent new vocabulary.

**Our notes,** the vocabulary the set already settles, worth reusing verbatim:

- Serializable data types live on the Params sheet's black bevelled hexagons,
  with the type drawn as a white glyph inside. Measured from his sources, the
  hexagon is the one shape that ignores the 2px border: full bleed (0.5 to
  23.5 wide, 1.5 to 21.5 tall, flat top and bottom), a radial fill from
  `#454545` at the upper left down to `#000000`, a 1px black outline, a
  bright white inner rim along the lit top-left edges and a faint one
  opposite. A smaller or flat-filled hexagon reads wrong next to his.
- His verbs have grammar, visible in Construct Point, Construct Domain and
  their partners. Construct shows the parts, drawn as themselves (the XYZ
  letters, the 0 and 1 digits), converging along faint dashed lines down
  into the assembled thing. Deconstruct is the exact vertical mirror: the
  thing on top, giving up its parts below. Deconstruct Mesh shows the other
  form: black arrows springing out of the assembled thing. Never mark these
  verbs with plus or minus badges. The composition is the verb.
- He never draws an arrow to mean "converts to". Look at Project and Map to
  Surface: the source sits above and its imprint lands on the target below,
  or the two fuse into one object, related by a dashed leader at most.
  Arrows are reserved for actual motion or data flow (Move, Orient).
- Move is an amber arrow with a white dot at each end. Grids are a light
  patch with a dark lattice crossing past its edges and white node dots.
  Polylines are dark runs with white dots at the vertices. Planes are
  perspective quads with an origin dot.
- When a plugin adds its own concepts, give each exactly one glyph and hold
  it across every icon: the same thing must never appear as a dot in one
  icon and a globe in the next. Related components then form a family
  automatically, the way his do.
- New subject matter still deserves a real drawing, not a collage of stock
  glyphs: a road drawn as a road beats an abstraction assembled from parts.
  Reserve the shared vocabulary for the concepts it already covers.
- When the world already has a sign, use it: the info circle, waves for
  water, mountains for terrain. An invented composition loses to a
  universal one, and a metaphor nobody shares reads as noise.
- Objects float. His subjects stand on nothing unless the ground is the
  subject; bases, plinths and ground lines read as clutter at 24px.
- Depth is extrusion: a lit top face, a shaded side face, a gradient front,
  exactly his boxes. A flat front-on shape reads as a sticker beside them.
- Fill the frame. The subject should own the ~20x20 artwork area; an icon
  that huddles small or drifts to a corner reads broken among his.
- Take the host's colours for the host's meanings: geometry a plugin
  returns wears the amber. A set's identity comes from its held glyphs,
  not from novel colours on everything.
- A ribbon category icon is its own drawing at 16px, and greyscale, the
  way the host draws its tab icons. Never a scaled or tinted copy.
- Use round line joins on filled shapes. Mitred joins spike past the
  silhouette at acute corners and read as stray pixels at 24px.

## His workflow

**David's rules** (assembled from all the sources above):

1. Draw vectors in **Xara** over a pixel grid, every vertex on a pixel centre.
2. Model genuinely complex 3D forms in **Rhino** first, then trace.
3. Export at 24x24 using his template jig: a mint-green 24x24 rectangle on a non-exporting layer, selected together with the icon, guarantees registration.
4. Post-process in **Paint.NET**: the drop shadow, and noise for materials.

For Grasshopper 2 he moved to a scriptable vector pipeline inside Rhino itself (`G2IconSetup`, `G2IconSymbol`, `G2ExportIcons`).

**Our notes, a modern equivalent:** keep **SVG as the truth base** (that's what this repo is). Draw in Figma, Inkscape or Affinity with pixel snapping at 24px. Script the shadow pass. Every original icon is in this repo to learn from, trace against, or extend: one folder per sheet, each icon its own SVG named after its component, with `icons/manifest.json` as the lookup.

Judge nothing from the vector view: render at an honest 24px after every
change and decide from that, next to a few originals at the same size. The
first version of a new icon is usually overdrawn, and the 24px render is
what tells you which strokes to delete.

## The checklist

**Our notes:** this is our compression of everything above into a review pass, for a human or an AI. An icon in this style should satisfy all of it:

- [ ] 24x24 canvas, ~2px empty border, vertices on pixel centres
- [ ] Lines 1-2px, only true horizontals/verticals and honest diagonals
- [ ] One colour family (two max), category palette respected
- [ ] Outlines in a darker version of the fill, silhouette only
- [ ] Light from the upper left, subtle gradients on large regions
- [ ] Shadow: +1/+1px, 2px blur, black at 25-33%, applied in raster after export
- [ ] Dashes and checkers placed as individual pixels
- [ ] Shared glyphs (arrows, points, planes) reused, not reinvented
- [ ] Subject fills the artwork area and stands on nothing it does not need
- [ ] Memorable silhouette, recognizable at a glance among 40 neighbours
- [ ] None of: black outlines, unnecessary text, photorealism, uncaused perspective, meaningless transparency, downscaled masters

## Getting the originals

Whether you're a person or an agent, pull reference icons from this repo rather than redrawing from memory. The repo is the source of truth. The [README](README.md) is the full clickable directory, and everything is fetchable over plain HTTPS:

1. Find an icon by component name in the manifest. Entries carry `name`, `nickname`, `category`, `description`, `guid`, and the `svg` and `png` paths under `icons/`:
   `https://raw.githubusercontent.com/aidannewsome/grasshopper-icons/main/icons/manifest.json`
2. Download the vector original (24x24 SVG, no shadow):
   `https://raw.githubusercontent.com/aidannewsome/grasshopper-icons/main/icons/<svg path>`
3. Variants, all under `icons/<sheet>/` (sheets: curves, math, meshes, parameters, surfaces, transform, vector):
   - `png/<name>.png` has David's drop shadow baked in.
   - `<sheet>.svg` is the whole sheet on a spaced grid, every icon a named group, and `<sheet>.png` is its shadowed render.
   - `<sheet>.xar` is the untouched Xara original, and `<sheet>.zip` bundles the sheet.
   - `icons/template.svg` is his sheet template (alignment grid, export widget, pixel font), and `icons/grasshopper-icons.zip` is everything at once.
