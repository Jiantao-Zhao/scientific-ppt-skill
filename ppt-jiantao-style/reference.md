# Figure-First Chemistry Report — Reference

Authoritative tokens and slide-construction recipe for Jiantao's **default** style. Use with `SKILL.md`. The point of every content slide is the **figure**; everything here is in service of placing a figure large and labelling it with the least text that does the job.

**Native & editable (Rule 2):** the only image you add is a pre-existing scientific figure you can't vectorize. Everything *you* produce is built native — charts via `addChart` / `add_chart` (carrying their data), tables via `addTable` / `add_table`, and **all** text (titles, take-home line, labels, captions, equations) in editable text boxes with selectable fonts. Never paste a PNG of a chart, table, or text you made. Litmus test: if you created it, Jiantao must be able to click it and edit the value/word/font/color.

## 1. Color tokens (restrained navy + red)

```js
const C = {
  navy:   '002060',  // primary title / heading (formal, defense). Dominant.
  blue:   '000099',  // title color for talks; deep blue
  blue2:  '0000CC',  // secondary emphasis text
  red:    'C00000',  // THE take-home line + key-species highlight + annotation box/arrow. Sparing.
  redDeep:'A50021',
  black:  '222222',  // body / labels (near-black)
  grey:   '595959',  // captions, page numbers
  white:  'FFFFFF',  // text on a colored band only
};
```
One navy/blue title color + red for the *one* take-home line and a few highlights. No Material-Indigo, teal, or pastel tints. (Older lit-share decks occasionally use a single purple `7030A0` title accent — match a deck you're editing, but default to navy/blue + red.)

## 2. Font tokens

```js
const F = {
  latin: 'Arial',           // titles + body + English terms, even inside Chinese sentences
  cjk:   '黑体',            // SimHei — Chinese headings
  cite:  'Times New Roman', // citations + equations only
};
```
Arial is the everywhere font. 黑体 for Chinese headings. Times New Roman *only* for citations/equations.

## 3. Size tokens (pt) — for 16:9 (13.33" wide)

```js
const S = {
  cover:    36,   // cover title (34–40)
  divider:  36,   // section-divider title (32–40, bold, centered)
  outline:  30,   // 目录 / outline header
  title:    26,   // content-slide title — 26, bold, centered (fixed)
  keyline:  22,   // the red take-home line
  body:     22,   // body / labels — 22 (fixed)
  table:    22,   // table cell text — 22 (fixed)
  small:    16,   // sub-labels, numbered I/II/III
  cite:     15,   // citation / footnote (14–16)
  page:     12,
};
```
**Fixed sizes (use these): title 26, body 22, table 22.** (His archive decks are 4:3 at slightly smaller pt; these are for 16:9. Don't shrink figures to hit a text size — text yields to the figure.)
(His archive decks are 4:3 at slightly smaller pt; these are scaled for 16:9. Don't shrink figures to hit a text size — text yields to the figure.)

## 4. Geometry

- **16:9 — 13.33 × 7.5 in.**
  ```js
  p.defineLayout({ name:'JT', width:13.333, height:7.5 }); p.layout='JT';   // pptxgenjs
  ```
  ```python
  from pptx.util import Inches
  prs.slide_width  = Inches(13.333); prs.slide_height = Inches(7.5)          # python-pptx
  ```
- Title band: centered, top, y ≈ 0.2–0.7". **Title box width shrinks to fit / is centered** (not full-bleed) — a house tell.
- Figure zone: the broad middle, y ≈ 1.1–6.2", using most of the width.
- Take-home line: just under the figure (or bottom-left), red. Citation: bottom-left, small. Page number: bottom-right.

## 5. Building a content slide (python-pptx — best for images + annotation)

```python
from pptx.util import Inches, Pt
from pptx.dml.color import RGBColor
from pptx.enum.text import PP_ALIGN
NAVY=RGBColor(0,0x20,0x60); RED=RGBColor(0xC0,0,0); GREY=RGBColor(0x59,0x59,0x59)

def title(slide, text, size=26, color=NAVY):
    tb=slide.shapes.add_textbox(Inches(0.4), Inches(0.22), Inches(12.5), Inches(0.8))
    p=tb.text_frame.paragraphs[0]; p.alignment=PP_ALIGN.CENTER
    r=p.add_run(); r.text=text; r.font.name='Arial'; r.font.size=Pt(size); r.font.bold=True; r.font.color.rgb=color

def hero_figure(slide, img, x, y, w):           # paste the science LARGE; preserve aspect
    return slide.shapes.add_picture(img, Inches(x), Inches(y), width=Inches(w))

def keyline(slide, text, y=6.35):               # the ONE red take-home line
    tb=slide.shapes.add_textbox(Inches(0.5), Inches(y), Inches(12.3), Inches(0.5))
    p=tb.text_frame.paragraphs[0]
    r=p.add_run(); r.text=text; r.font.name='Arial'; r.font.size=Pt(22); r.font.bold=True; r.font.color.rgb=RED

def citation(slide, text, y=6.95):
    tb=slide.shapes.add_textbox(Inches(0.4), Inches(y), Inches(9.0), Inches(0.4))
    p=tb.text_frame.paragraphs[0]
    r=p.add_run(); r.text=text; r.font.name='Times New Roman'; r.font.size=Pt(15); r.font.color.rgb=GREY
```

A results slide is then just: `title(...)` (a conclusion sentence) → one or two `hero_figure(...)` placed large → `keyline(...)`, and `citation(...)` **only if the figure is from someone else's published paper**. **Never add a bottom grey method-note line** (conditions, *n*, model settings, dataset notes) — that detail goes in speaker notes (`slide.notes_slide.notes_text_frame.text = "…"`), not on the slide.

## 6. Annotation craft — red box / arrow on a figure

Point at the key species/intermediate the way Jiantao does (e.g. a red box around `PTZ1•`, an arrow to a peak):

```python
from pptx.enum.shapes import MSO_SHAPE
def red_box(slide, x, y, w, h):
    sp=slide.shapes.add_shape(MSO_SHAPE.RECTANGLE, Inches(x),Inches(y),Inches(w),Inches(h))
    sp.fill.background()                     # transparent
    sp.line.color.rgb=RGBColor(0xC0,0,0); sp.line.width=Pt(2.25)
def red_arrow(slide, x, y, w, h):
    sp=slide.shapes.add_shape(MSO_SHAPE.RIGHT_ARROW, Inches(x),Inches(y),Inches(w),Inches(h))
    sp.fill.solid(); sp.fill.fore_color.rgb=RGBColor(0xC0,0,0); sp.line.fill.background()
```

## 6b. Concept diagrams — outlined boxes + light arrows (清爽, not 笨重)

When you build a flow / concept diagram from boxes, make each box an **outline only**: a thin colored border, **no fill**, with the text in that same color. A solid-filled box looks heavy and clunky. Keep the connectors **thin and light** — a hairline line in a muted tone — **never a chunky solid dark-navy arrow block**.

```python
def concept_box(slide, x, y, w, h, text, color=NAVY):   # outline only, transparent, colored text
    sp=slide.shapes.add_shape(MSO_SHAPE.ROUNDED_RECTANGLE, Inches(x),Inches(y),Inches(w),Inches(h))
    sp.fill.background()                                  # NO fill — this is the key to "清爽"
    sp.line.color.rgb=color; sp.line.width=Pt(1.25); sp.shadow.inherit=False
    tf=sp.text_frame; tf.word_wrap=True; p=tf.paragraphs[0]; p.alignment=PP_ALIGN.CENTER
    r=p.add_run(); r.text=text; r.font.name='Arial'; r.font.size=Pt(15); r.font.bold=True
    r.font.color.rgb=color                               # text in the border color

def light_arrow(slide, x, y, w, color=RGBColor(0x90,0x90,0x90)):  # thin grey line, slim arrowhead
    cn=slide.shapes.add_connector(2, Inches(x),Inches(y),Inches(x+w),Inches(y))  # 2 = straight
    cn.line.color.rgb=color; cn.line.width=Pt(1.25)
    # add a small arrowhead on the line end:
    ln=cn.line._get_or_add_ln()
    from pptx.oxml.ns import qn
    he=ln.makeelement(qn('a:tailEnd'), {'type':'triangle','w':'sm','len':'sm'}); ln.append(he)
    cn.shadow.inherit=False
```

Use a muted grey (or a thin same-color line) for connectors, not navy `002060` fill. The diagram should read as light line-art, not a row of heavy blocks.
Highlight a key reagent/species inline by coloring just that run red (`run.font.color.rgb = RED`) — used for things like *CP[6]*, *MPTZ²⁺*.

## 7. pptxgenjs equivalent (if building in Node)

```js
const p=new PptxGenJS(); p.defineLayout({name:'JT',width:13.333,height:7.5}); p.layout='JT';
s.addText(titleSentence,{ x:0.4,y:0.22,w:12.5,h:0.8, fontFace:'Arial', fontSize:28, bold:true, color:C.navy, align:'center' });
s.addImage({ path:fig, x:1.2, y:1.2, w:10.9, h:4.6 });          // hero figure, large, keep aspect
s.addText(takeHome,{ x:0.5,y:6.35,w:12.3,h:0.5, fontFace:'Arial', fontSize:22, bold:true, color:C.red });
s.addText(cite,{ x:0.4,y:6.95,w:9,h:0.4, fontFace:'Times New Roman', fontSize:15, color:C.grey });
// red annotation box: s.addShape(p.ShapeType.rect,{ x,y,w,h, line:{color:C.red,width:2.25}, fill:{type:'none'} });
```

## 8. Citation format

`Author A, Author B, … Journal Year, Vol, Pages.` in Times New Roman, e.g.
*J. Zhao, Y. Hu, X. Jin, B. Qin, S. Mao, J. Zhang. Science 2024, 385, 1354–1359.*
Bottom-left on any slide carrying a published figure (own or others').

## 9. Rendering & verification (always LOOK)

```bash
/Applications/LibreOffice.app/Contents/MacOS/soffice --headless --convert-to pdf deck.pptx
pdftoppm -png -r 70 deck.pdf slide          # then view slide-*.png
```
On macOS, if a deck has Chinese in its filename, drive soffice from python (`subprocess`) — bash mis-handles NFD filenames. Then eyeball every slide against the `SKILL.md` checklist — a slide that looks like a clean corporate template is wrong.
