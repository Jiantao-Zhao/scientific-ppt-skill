# Secondary Mode — Native Data Slide

Use this **only** when a slide's whole point is a chart of **Jiantao's own numbers** and there is no good figure to paste. This is the exception. Most data in his decks still arrives as a *pasted figure* (a plot exported from his analysis), placed large like any other figure — see `reference.md`. Reach for a native chart only when you genuinely have the numbers and a clean chart communicates better than a pasted plot.

Even then:
- **One honest chart, large.** Not a grid of KPI / stat cards, not "Summary —" lead-ins, not tint boxes. Those read as AI-template chrome and are wrong for his style.
- Keep the rest of the slide identical to a figure slide: navy conclusion title on top, one red take-home line under the chart, citation/provenance (date, plate/batch) bottom-left, page number bottom-right.
- Palette inside the chart: navy `002060` / blue `0000CC` for the primary series, grey `595959` for the baseline/comparison; red `C00000` only to flag a key bar. Fonts Arial.
- 16:9, Arial, same as everything else.

Minimal python-pptx native chart:
```python
from pptx.chart.data import CategoryChartData
from pptx.enum.chart import XL_CHART_TYPE, XL_LEGEND_POSITION
from pptx.util import Inches, Pt
from pptx.dml.color import RGBColor
cd = CategoryChartData(); cd.categories = [...]
cd.add_series('RF', (...)); cd.add_series('Random', (...))
gf = slide.shapes.add_chart(XL_CHART_TYPE.COLUMN_CLUSTERED, Inches(1.2),Inches(1.3),Inches(10.9),Inches(4.6), cd)
ch = gf.chart; ch.has_legend=True; ch.legend.position=XL_LEGEND_POSITION.TOP; ch.legend.include_in_layout=False
for s,c in zip(ch.series, [RGBColor(0,0x20,0x60), RGBColor(0x59,0x59,0x59)]):
    s.format.fill.solid(); s.format.fill.fore_color.rgb=c
ch.plots[0].has_data_labels=True
```

If in doubt, paste the plot as a figure instead — that is the more typical Jiantao slide.
