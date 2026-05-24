# Modern Chart Types Reference

Charts that didn't exist (or weren't named) when Bertin (1967) and Tufte (1983) wrote. For each: what it is, when to use, when not to, common failure modes, library support.

The skill's principles still apply — these forms can be evaluated for Lie Factor, data-ink ratio, perceptual encoding correctness. None is exempt from "show the data, kill chartjunk."

## Table of contents

1. Beeswarm and strip plots
2. Raincloud plots
3. Ridgeline plots (joyplots)
4. Sankey / alluvial
5. Treemap
6. Calendar heatmap
7. Hex bin map
8. Parallel coordinates
9. Slopegraph
10. Bump chart
11. Waffle chart
12. Bullet chart
13. Violin plot
14. Dot plot variants (Cleveland dot plot, stripchart)
15. Cohort heatmap (retention triangle)
16. Sparkline
17. Streamgraph
18. Forms to mostly avoid (and what to use instead)

---

## 1. Beeswarm and strip plots

A strip plot is a 1-D scatter — every observation gets a dot along a single axis. A beeswarm jitters the dots non-randomly so they pack densely without overlap, revealing distribution shape while keeping every individual visible.

**When to use:** distribution comparison for small-to-medium n (~10 to ~1000) when individual points carry meaning. Better than a boxplot when the distribution is bimodal, has outliers worth labeling, or n is small enough that a boxplot's five-number summary throws away too much.

**When not to:** n > ~5000 (overlap kills it even with jitter); when the question is "where's the median," in which case a boxplot is cleaner.

**Failure modes:** random jitter that obscures rather than reveals shape; insufficient point separation; bins too narrow making it look like a histogram.

**Libraries:** `seaborn.swarmplot`, `seaborn.stripplot`, `matplotlib` with `np.random.uniform` jitter; `ggplot2::geom_jitter`; `d3-force` for custom layouts.

## 2. Raincloud plots

A combination introduced by Allen et al. (2019): half-violin (density), strip plot (individual points), and boxplot (summary), stacked vertically. Gives the reader shape + individuals + summary in one chart.

**When to use:** when you need to show distribution shape, individual variation, and summary stats together — common in scientific publication.

**When not to:** for general audiences who'll be confused by the three-in-one. For a dashboard. When you only need one of the three layers.

**Failure modes:** rendering all three layers at the same prominence creates visual clutter; better to gray the strip plot and emphasize the cloud.

**Libraries:** `ptitprince` (Python, sits on seaborn), `ggdist` + `gghalves` (R), or hand-rolled in d3.

## 3. Ridgeline plots (joyplots)

Stacked density curves, often slightly overlapping, one per category. Popularized as "joy plots" after the Joy Division album cover.

**When to use:** comparing distribution shapes across many categories (10-50). Lets the eye scan for shape differences, modes, skew, without occlusion.

**When not to:** when exact ratios or means matter (use a faceted histogram or dot plot instead). When there are too few categories (small multiples are clearer for ≤ 5).

**Failure modes:** overlap so heavy that the back ridges are invisible; insufficient categorical labeling; using a color hue for the ridges that has no meaning (just use a single neutral or a sequential palette tied to a metric).

**Libraries:** `joypy` (Python), `ggridges` (R), `seaborn.kdeplot` with manual stacking.

## 4. Sankey / alluvial

Flow diagram where the width of each band encodes a quantity (volume, count, frequency). Sources on the left, destinations on the right (or in a chain across several stages).

**When to use:** showing flow between categorical stages — user journey through funnel stages, energy from sources to uses, immigration between countries, budget allocation. Best with ≤ ~10 nodes per stage and ≤ ~3-4 stages.

**When not to:** dense networks (becomes a hairball); when the flow direction isn't meaningful; when the reader needs precise quantitative readouts (use a table).

**Failure modes:** too many nodes; tangled crossings (sort nodes to minimize crossings); using color decoratively rather than to encode category; bands too thin to read.

**Libraries:** `plotly.graph_objects.Sankey`, `networkD3::sankeyNetwork` (R), `d3-sankey`. SankeyMATIC for one-off web tools.

## 5. Treemap

Nested rectangles whose areas encode quantity, used for hierarchical data. Originally Ben Shneiderman (1991), refined repeatedly since.

**When to use:** showing the breakdown of a whole into hierarchically-nested parts where you care about both relative size and grouping (e.g., portfolio composition by sector → company, file system disk usage). Best for 3-100 leaf cells.

**When not to:** when exact ratios matter (areas are perceived non-linearly, ~`area^0.8`); when there are too many tiny cells (unreadable); when the hierarchy isn't important (a sorted bar chart wins).

**Failure modes:** comparing non-adjacent cells of similar size (hard); tiny labels in tiny cells; rainbow color encoding category (use a categorical palette and limit to top-level groups).

**Libraries:** `squarify` (Python), `treemap` (R), `d3.treemap`, Plotly. Best layout: squarified (Bruls/Huijing/van Wijk, 2000), which most modern libraries default to.

## 6. Calendar heatmap

Grid of cells representing days, weeks, or months, colored by a daily quantity. Made famous by GitHub's contribution graph.

**When to use:** showing patterns in daily activity over months/years — habits, posting frequency, sales by day, server load. Reveals weekly cycles, seasonality, and gaps.

**When not to:** for non-daily data; for very short time ranges (a line chart is better for < ~30 days); when the absolute quantity matters more than pattern.

**Failure modes:** color scale that's too compressed at the high end (one bright day washes out the rest — use percentile-based binning or log scale for skewed data); missing legend for the color scale; treating Sunday as the week's start when your audience uses Monday (or vice versa).

**Libraries:** `calmap` (Python), `ggcalendar` (R), `cal-heatmap.js`. Easy to hand-roll.

## 7. Hex bin map

A choropleth alternative for point data: divide space into hexagonal bins, color each by the count or aggregate inside it. Hexagons tile evenly with no preferred direction (unlike squares).

**When to use:** dense geographic point data (incidents, observations, events) where individual points would overplot; statistical regions that aren't politically meaningful; when you want spatial pattern without the population-weighted distortion of choropleths.

**When not to:** when administrative boundaries matter (use a choropleth); when n is small enough to show individual points (use a dot map).

**Failure modes:** hex size mismatched to data density (too small = noise, too large = pattern lost); using a non-perceptual color ramp; not showing a legend that maps cell color to count.

**Libraries:** `geopandas.GeoSeries.plot` with `hexbin`, `holoviews.Histogram2d`, `d3-hexbin`, `deck.gl` HexagonLayer.

## 8. Parallel coordinates

Each variable gets its own vertical axis; each observation is a line crossing all axes. Eye reads relationships by parallel/crossing line patterns.

**When to use:** exploring relationships in 4-20 dimensional data when scatterplot matrices become unwieldy; revealing clusters and outliers.

**When not to:** for communication to general audiences (steep learning curve); when n > ~1000 (lines occlude each other); for static publication (interactive sorting/brushing is what makes parallel coordinates useful).

**Failure modes:** axis ordering arbitrary (try several; some orders reveal patterns others hide); no brushing/filtering (parallel coordinates without interaction is half a tool); too many lines without alpha-blending.

**Libraries:** `plotly.graph_objects.Parcoords`, `pandas.plotting.parallel_coordinates`, `GGally::ggparcoord` (R), `d3.parcoords`.

## 9. Slopegraph

Two parallel y-axes (left and right), each item is a line connecting its value at time 1 to its value at time 2. The slope of the line is the change.

**When to use:** comparing change between two time points across many items; ranking changes (who rose, who fell, who stayed). Tufte's own invention for showing tax-rate changes across 15 countries (1970 vs 1979).

**When not to:** more than two time points (use a regular line chart with small multiples); when the change isn't the story.

**Failure modes:** unlabeled lines (label every line at both ends); too many crossings (sort by start value, then by end value); equal-weight lines for trivial and important items (highlight the storytelling lines).

**Libraries:** no built-in in most packages; hand-build with `matplotlib.pyplot.plot` and label_at_end. R: `CGPfunctions::newggslopegraph`. d3: hand-rolled.

## 10. Bump chart

Like a slopegraph but for ranks over many time periods. Each item's line shows its rank position over time; the bumping pattern reveals position changes.

**When to use:** rank evolution over time (best-selling products over months, sports league standings, song chart positions). Best for ≤ ~15 items, ≤ ~20 time periods.

**When not to:** when absolute values matter more than rank (use a line chart with the actual values); when there are too many items (becomes spaghetti).

**Failure modes:** smoothed curves that don't reflect actual transitions (use straight segments unless there's a reason to smooth); no end labels; all lines the same weight.

**Libraries:** `ggbump` (R), or hand-rolled in matplotlib / d3.

## 11. Waffle chart

A 10×10 grid where each cell represents 1%. Used to show parts of a whole, "1 in 10 people…"-style.

**When to use:** communicating proportions to general audiences ("1 in 4 patients experience side effects"); when you want every value to be countable individually; in infographics where the visual metaphor matters.

**When not to:** when precision matters (you can count cells, but bar charts are faster); for comparison across multiple categories (multiple waffles are noisy); for ratios more granular than 1%.

**Failure modes:** square cells that aren't a perfect 10×10 grid (defeats the "out of 100" interpretation); too many colors; using waffles for change-over-time (small-multiple waffles compare badly).

**Libraries:** `pywaffle` (Python), `waffle` (R), or hand-rolled.

## 12. Bullet chart

Stephen Few's replacement for the speedometer / gauge. A horizontal bar showing the current value, against a background band showing performance tiers (bad/satisfactory/good), with a marker for the target.

**When to use:** any single-metric display with a target or threshold (KPI tiles done right). Sales vs target. Server health vs threshold. NPS vs goal.

**When not to:** when you don't have a meaningful threshold or target (just show the value).

**Failure modes:** too many performance tiers (3 max); rainbow tier colors (use a single sequential ramp from light to dark); tier widths that don't match the actual scale.

**Libraries:** `plotly.graph_objects.Indicator(mode='number+gauge+delta')`, `bulletchartraphael`, or hand-rolled.

## 13. Violin plot

A density plot mirrored around a central axis, often combined with an inner boxplot. Shows distribution shape and summary together.

**When to use:** comparing distributions across categories when shape matters more than individual points; replacement for a boxplot when distributions are bimodal or skewed (a boxplot hides bimodality entirely).

**When not to:** for small n (the kernel density estimate is misleading with few points — show the points instead); when audiences won't read density-based shapes (most non-statistical audiences).

**Failure modes:** showing only the violin without summary marks (median, quartiles); kernel bandwidth too wide (smooths out real features) or too narrow (creates spurious bumps); using a violin where a beeswarm is more honest.

**Libraries:** `seaborn.violinplot`, `matplotlib.pyplot.violinplot`, `ggplot2::geom_violin`, `plotly.violin`.

## 14. Dot plot variants

The Cleveland dot plot (1985) is a horizontal bar chart's better-behaved cousin: a single dot per category instead of a bar, positioned along the x-axis by value. Originally proposed by William Cleveland to avoid the "bar overweight" problem.

**When to use:** ranked comparison of categories (replacing a horizontal bar chart); when bars feel too heavy or you want to overlay multiple metrics per category (multiple dots, different colors).

**When not to:** when zero is meaningful and bars communicate "starts at zero" honestly; when the audience expects bars.

**Failure modes:** dots too small to register; no connecting line for paired/grouped values (use the line as a visual aid); not sorting by value.

**Libraries:** every major library; in matplotlib, `plt.plot(values, categories, 'o')`.

## 15. Cohort heatmap (retention triangle)

A heatmap where rows are signup cohorts (months), columns are observation months since signup, and cells are retention %. Triangular because cohort N can only be observed for `total_months - N` months.

**When to use:** SaaS retention analysis, cohort behavior comparison, any longitudinal-cohort study.

**When not to:** for executive summary (it's a recording graphic, not a message graphic — Bertin); when the triangular missing cells confuse non-analyst audiences.

**Failure modes:** white cells for missing data (read as 0); no diagonal cutoff (cells beyond the observation window shown as zero); color scale that compresses the typical retention range (~10-60%) into one shade; mixing different cohort sizes without showing n.

**Libraries:** `seaborn.heatmap` with a triangle mask, `ggplot2::geom_tile`. For board-deck communication, switch to small-multiple retention curves instead — the triangle is for analysis, curves are for the message.

## 16. Sparkline

Tufte's own invention (post-VDQI, in *Beautiful Evidence* 2006): an intense, word-sized graphic — a line chart small enough to embed inline with text or in a table cell.

**When to use:** alongside a current value in a table or dashboard, to show recent trend. "MRR: $2.4M ▁▂▃▅▆▇" gives current state and trajectory in one glance.

**When not to:** standalone (a sparkline without context is just a tiny line chart); when precision matters (no axis labels).

**Failure modes:** too small to read; not aligned with the value it accompanies; using color to encode a separate dimension (sparklines should be a single hairline).

**Libraries:** `sparkline` (Python/CLI), `ggplot2` with `theme_void()` and small figure size, Excel's built-in sparklines, `react-sparklines` for React, HTML/CSS hand-rolled.

## 17. Streamgraph

A stacked area chart with no axis baseline — the layers flow around a center, with the total reaching above and below. Popularized by Lee Byron's "The Ebb and Flow of Movies" (NYT, 2008).

**When to use:** showing composition change over time when the total isn't important but the relative shape and timing are (music charts, app categories, popularity over years).

**When not to:** when the total matters (use a stacked area chart with an axis); when the absolute values per category matter (only the bottom and top of each band are readable; middles are not).

**Failure modes:** too many categories (the bands become threadlike); decorative color use; not letting the reader find a starting baseline.

**Libraries:** `d3.stack` with `d3.stackOffsetWiggle`, `plotly.area` with offsets; hand-rolled in matplotlib.

## 18. Forms to mostly avoid (and what to use instead)

| Trendy form | Why it underperforms | Use instead |
| --- | --- | --- |
| **Word cloud** | Size encodes frequency badly (perceived area is non-linear); position carries no information | Sorted bar chart of top N terms with frequencies |
| **3-D anything** | Occlusion, perspective distortion, Lie Factor inflation | Flat equivalent |
| **Spider / radar chart** | Area changes nonlinearly with values; axis ordering arbitrary; closed shape adds meaning that isn't there | Small-multiple bars, or parallel coordinates |
| **Polar / coxcomb / Nightingale rose** | Area encoding for angles is Bertin's dimensional mismatch | Sorted bar chart |
| **Funnel chart** (the literal trapezoid) | Slope encoding misleads about conversion rate | Horizontal bar chart of absolute counts + table of stage-to-stage % |
| **Donut chart** | Pie chart problems + a hole that adds nothing | Sorted bar chart |
| **Connected scatter** (line through scatter points colored by time) | Mixes "trajectory" and "scatter" semantics; readers misinterpret | Two charts: one scatter, one line over time |
| **Animated bubble chart** | Bubble area perception is bad; animation hides individual moments | Small multiples of scatterplots at chosen time slices |
| **Tag cloud / weighted text on a map** | Size + position both encoded; reads as ad copy | Choropleth or proportional symbols, with top-N labels |
| **Marimekko / mosaic chart** | Two-dimensional area encoding is hard to read; few audiences understand it | Two faceted bar charts or a heatmap |
| **Circular bar chart / radial bar** | Bars of different angular position have different perceived weight; closure adds visual noise | Standard horizontal bar chart, sorted |

## When the user wants a trendy chart

Most "trendy" forms are tools that have a narrow correct application and a wide misuse pattern. The skill should:

1. Ask what the reader's question is.
2. Recommend the form that best answers it, with a brief explanation of why the trendy form is or isn't the right pick.
3. If the trendy form *is* right (e.g., raincloud for a scientific publication comparing distribution shapes across treatments), apply the form's specific best practices.
4. If the trendy form is wrong, name the failure mode the reader would hit, and offer the alternative.

The principle from Tufte's epilogue applies: "Design is choice." A skill's job is to make the choice deliberate, not to forbid forms by reputation.
