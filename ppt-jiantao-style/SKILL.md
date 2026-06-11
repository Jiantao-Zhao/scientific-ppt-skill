---
name: ppt-jiantao-style
description: Use when Jiantao (Jiantao Zhao) asks to create, edit, extend, or restyle any .pptx slide deck — group meetings, lab/progress updates, seminars, English conference talks, thesis-defense decks, literature-sharing decks, self-intros, or posters. Encodes Jiantao's real house style: a FIGURE-FIRST chemistry-report deck where each slide is built around one or a few large scientific figures (reaction schemes, spectra, structures, mechanism diagrams, data plots) that fill the slide, carried by a short conclusion-style title, ONE red take-home line, and a citation — NOT a sparse bullet-and-stat-card "data dashboard." Any chart, table, or text you create must be built as native, editable PowerPoint objects (native chart with its data, native table, text in editable text boxes with adjustable fonts) — never a single flattened, non-editable image; only pre-existing unvectorizable scientific figures are pasted as images. Trigger even if he just says "make slides," "build a deck," "组会 PPT," or names a .pptx — and do NOT produce generic templated AI slides (thin accent rules, big whitespace, bullet walls, stat cards): that is the opposite of his style.
---

# Jiantao's PPT Style

Jiantao is a chemistry researcher (radical / supramolecular chemistry + data-driven catalysis). The single most important thing to get right:

> **His slides are figure-first. The figure does the talking. Text is sparse; figures are dense.**

A typical content slide is **one (or a few) large pasted scientific figures filling most of the slide**, with only a short conclusion-style **title**, **one red take-home line**, and a small **citation**. This is the look of a working chemist's talk — visually full of *figures*, not full of *text*. The most common failure is making clean, sparse, templated "data-dashboard" slides (thin accent rules, big empty margins, bullet lists, KPI/stat cards). **That AI-template look is exactly what to avoid.**

Gold-standard exemplars to match: **thesis-defense / formal decks** and **English academic talks** (e.g. the "Radical-mediated click-clip reactions" series). Read [`reference.md`](reference.md) for exact tokens and slide-construction snippets; [`genres.md`](genres.md) for per-genre deck skeletons.

## The anatomy of a content slide

```
┌────────────────────────────────────────────────────────────┐
│         Conclusion-style title — navy, centered, bold        │  ← states what THIS slide proves
│                                                              │
│   ┌──────────────────────┐   ┌─────────────────────────┐    │
│   │  big scientific图     │   │  another figure / panel  │    │  ← 1–3 pasted figures fill
│   │  (scheme / spectrum / │   │  (often multi-panel)     │    │     the slide; red box/arrow
│   │   structure / plot)   │   │                          │    │     annotates the key species
│   └──────────────────────┘   └─────────────────────────┘    │
│                                                              │
│   One red take-home line (C00000) — the finding in a sentence │  ← the ONE emphasized line
│  Citation: J. Zhao et al., Science 2024, 385, 1354–1359.     │  ← small, bottom
└────────────────────────────────────────────────────────────┘
```

## The rules

**Rule 1 — Paste the figures you can't make natively; make them large.** A scientific figure that *already exists* and cannot be vectorized — a reaction scheme, an EPR/NMR/UV spectrum, a crystal/DFT structure, a mechanism diagram, a micrograph, a gel/plate photo, a molecular rendering, or a plot Jiantao already produced from his own analysis — is **pasted as an image and made large**. Don't shrink it to leave room for text; when you have such a figure, the figure is the slide.

**Rule 2 — Anything *you* generate must be native and editable; never flatten it into a picture.** This is non-negotiable. If you build a chart, build it as a **native PPT chart carrying its data** (pptxgenjs `addChart` / python-pptx `add_chart`) — never render a plot to PNG and paste it. If you build a table, use a **native PPT table** (`addTable` / `add_table`), never a screenshot of one. Every piece of text — the title, the red take-home line, axis / legend / annotation labels, captions, equations — lives in its **own editable text box** with a real, selectable, re-styleable font; never bake text into an image. **Litmus test for anything you created:** can Jiantao click it and change the number, the word, the font, or the color? If you made it, the answer must be *yes*. The only raster you add is a pre-existing scientific figure under Rule 1.

**Rule 3 — One conclusion title + one red take-home line; almost nothing else.** The title states the finding as a short declarative sentence (*"H₂O₂ was generated in clip reactions"*, *"Laser flash photolysis reveals the diradical intermediates"*, *"剪切反应后体系中产生了过氧化氢"*); method/object slides may use a noun phrase. Under or beside the figure, put **exactly one** red (`C00000`) sentence — the take-home. Beyond that, keep text to a citation and at most a tiny numbered list (I / II / III) for mechanism logic. No bullet paragraphs.

**Rule 4 — Dense in figures, not in text; no template chrome.** Fill the slide with the figure(s); avoid big empty margins. Do **not** add the things that read as AI-generated: thin accent rules under titles, KPI/stat cards, rounded "tint" boxes, "Summary —" lead-ins, evenly-spaced bullet lists. Detailed narration goes into **speaker notes** (Jiantao writes the spoken script there), keeping the visible slide visual.

## Look & feel (both apply to new 16:9 decks)

- **Aspect ratio: 16:9** (13.33 × 7.5 in). His archive is mostly 4:3, but new decks should be 16:9.
- **Fonts:** **Arial** for everything (Latin *and* inside Chinese sentences); **黑体 (SimHei)** for Chinese headings; **Times New Roman** for citations/equations. (Calibri/Consolas are NOT this style.)
- **Type sizes (fixed):** content-slide **title 26 pt**, **body text 22 pt**, **table text 22 pt**. (Cover/divider larger; citation/page-number smaller — see `reference.md`.)
- **Color — restrained, navy + red:**
  - Title / headings: navy **`002060`** (formal) or deep blue **`000099`** (talks); blue **`0000CC`** for secondary emphasis.
  - **Red `C00000`** — the one take-home line, and inline highlight of a key species/reagent (e.g. *CP[6]*, *MPTZ²⁺*) or a **red box/arrow** on a figure. Used sparingly (a handful of times per deck).
  - Body text near-black; white only on a colored band.
  - Do **not** use Material-Indigo, teal, or pastel tint palettes.
- **No bottom-left grey footnote / method-note line.** Do **not** add a small grey caption at the bottom of a slide (assay conditions, "n = …, grouped CV", model settings, dataset notes). If the audience needs that detail it goes in **speaker notes**, never as a grey line on the slide. The *only* bottom-left small text allowed is a **citation**, and only when the slide shows someone else's **published** figure (Times New Roman, ~14–16 pt, for attribution) — never on a slide of Jiantao's own data.
- **Concept diagrams — light, not heavy.** When you draw a flow / concept diagram from boxes and arrows, make each box an **outline only: a thin colored border, NO fill (transparent), with the text in that same color.** A solid-filled box looks clunky and heavy. Keep connectors **thin and light** — a hairline arrow or a slim line in a muted/light tone; **never a chunky solid dark-navy arrow block.**
- **Page numbers** bottom-right on multi-slide decks.
- **Bilingual is normal:** Chinese narrative + English chemical terms / figure labels / citations. Match the language Jiantao writes in.

## Secondary mode — native data slide (use sparingly)

Only when the slide's point IS a chart of **Jiantao's own numbers** and there is no good figure to paste, build a clean native chart — see [`data-mode.md`](data-mode.md). Even then, prefer one honest chart over a grid of stat cards. This is the exception, not the default; most data still arrives as a pasted figure.

## Workflow

### Creating from scratch
1. **Gather the figures first.** Ask for / locate the schemes, spectra, structures, and plots — the deck is built around them. One key finding per slide.
2. Pick the genre skeleton in [`genres.md`](genres.md) (defense / English talk / lit-share / progress / self-intro).
3. For each content slide: place the hero figure(s) large → add the conclusion title (navy) → add the one red take-home line → add the citation → annotate the key species with a red box/arrow if useful.
4. Set the deck to **16:9 (13.33 × 7.5)**, Arial, navy/red. Build with `python-pptx` (best for placing images + annotations) or `pptxgenjs`. Snippets in [`reference.md`](reference.md).
5. Write the spoken narration into speaker notes; keep slides visual.

### Editing an existing deck
1. Render it first (see Verification) and **look** — match the deck's own figures, colors, and density. Don't impose a cleaner template on it.
2. Edit via `python-pptx`, preserving pasted figures and native object types.

### Verification (render and LOOK — never judge from XML alone)
```bash
/Applications/LibreOffice.app/Contents/MacOS/soffice --headless --convert-to pdf deck.pptx
pdftoppm -png -r 70 deck.pdf slide       # then view the images
```
For each slide confirm:
- [ ] The slide is built around the figure(s), which are **large and fill the space** — not shrunk to make room for text
- [ ] **Everything you generated is native & editable** — charts are native PPT charts (click → edit data), tables are native tables, all text is in editable text boxes with selectable fonts; no flattened picture of a chart/table/text. Raster = only pre-existing scientific figures.
- [ ] At most a title + one red take-home line + citation (+ optional tiny I/II/III list); no bullet walls
- [ ] No AI-template chrome (thin accent rule, stat cards, tint boxes, "Summary —", even bullet grids)
- [ ] Title is a conclusion sentence (results) or a clean noun phrase (method/divider), navy/deep-blue
- [ ] Exactly one red `C00000` take-home line; red also only on key-species highlights / annotation boxes
- [ ] Arial (+ 黑体 for Chinese headings, Times New Roman for citations); navy/red palette only
- [ ] **No bottom-left grey method/footnote line** (conditions, n, model settings → speaker notes); a citation appears only for someone else's published figure
- [ ] **Concept-diagram boxes are outlined (colored border, no fill, colored text), not solid-filled; arrows are thin/light, not chunky dark-navy**
- [ ] 16:9; page numbers; backup slides after "Thank you"
- [ ] Nothing clipped or overlapping; figures not stretched out of aspect

If a slide looks like a clean corporate template, it is wrong — rebuild it around the figure.

## Reference files
- [`reference.md`](reference.md) — exact color/font/size tokens, the figure-first slide-construction recipe, annotation craft (red box/arrow on a figure), citation format, and python-pptx / pptxgenjs snippets.
- [`genres.md`](genres.md) — deck skeletons: English academic talk, thesis defense, literature sharing, periodic progress report, self-introduction, poster.
- [`data-mode.md`](data-mode.md) — the secondary native-data-slide style, for the rare slide whose point is a chart of Jiantao's own numbers.
