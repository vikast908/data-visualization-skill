# Chart Picker — A Decision Aid

"What chart should I use?" is the most common visualization question. This file is the fast lookup.

Two warnings before the table:

1. **The right answer is often "not a chart."** Re-read SKILL.md's "Format Choice — pick the lightest container" section. If you have ≤ 20 numbers, a table almost always wins (Tufte p. 56).
2. **Pick by the *question*, not by the data shape.** A scatter and a line chart can plot the same X-Y pairs; the question is whether the reader should think "what's the relationship?" (scatter) or "how does this change over time?" (line). Same data, different question, different chart.

## Table of contents

1. Pick by question
2. Pick by data shape
3. Common wrong picks and what to use instead
4. The fast decision tree
5. Niche cases that need their own answer

---

## 1. Pick by question

The most reliable way to choose. Identify the question your reader will ask out loud when they see the chart.

| Reader's question | Default chart | Why |
| --- | --- | --- |
| "How does X change over time?" | **Line chart** (range-framed, labeled at ends) | Position on the X axis encodes time order; slope encodes rate. |
| "How do these N things compare in size?" | **Sorted bar chart** (horizontal if labels long) | Position encodes quantity; sorting carries the story. |
| "What is the distribution of X?" | **Histogram** (or strip / beeswarm for small n) | Reveals shape: skew, modes, outliers. |
| "How do the distributions of X across groups compare?" | **Small multiples of histograms / density / ridgeline**, or **boxplot strip** | Shape comparison demands constant scale + co-located displays. |
| "What is the relationship between X and Y?" | **Scatterplot** (Tufte: "the greatest of all graphical designs") | Position encodes both variables; eye sees the joint distribution. |
| "What is the relationship between X and Y, controlling for Z?" | **Small multiples of scatterplots** (faceted by Z) or **scatterplot with Z encoded as color/shape/size** | Faceting reads cleanly; encoding Z reads densely. |
| "What composes this whole?" (parts of 100%) | **Sorted bar chart** or **stacked bar (single-bar)** or **table** | Pie charts fail comparison; bars compare. |
| "How has composition changed over time?" | **100% stacked area** (use sparingly) or **small-multiple bars** | Both work; small multiples read cleaner for >4 categories. |
| "How do these N items rank?" | **Sorted dot plot** or **bar chart** | Position on a number line is the most precise rank encoding. |
| "Where on the map is X concentrated?" | **Proportional symbols** (preferred) or **dot density** or **choropleth** (if pre-classed is OK) | See SKILL.md "Map advice"; choropleth converts Q to O. |
| "How does X flow from source to destination?" | **Sankey/alluvial** for ≤ ~10 nodes per side; **chord diagram** for matrices; **arrow map** for geographic flow | Match the topology. |
| "How are these things connected?" | **Adjacency matrix** for dense networks; **force-directed** only when sparse and small (<50 nodes); **arc diagram** for ordered nodes | Force-directed hairballs are the most common viz failure for networks. |
| "Is this number above or below the goal?" | **Bullet chart** (Stephen Few) or labeled line with target reference | One quantity + one threshold = bullet, not gauge. |
| "How is this metric trending right now?" | **Sparkline in-line with the current value** | Tufte's invention. Word-sized intense graphic; no axis needed. |
| "What is the breakdown of these N items by two categorical dimensions?" | **Heatmap with cells colored by value** (reorderable matrix if both axes are reorderable — Bertin) | Two planar dimensions + one ordered retinal variable = single image. |
| "How has the cohort retention/funnel/conversion shape evolved?" | **Cohort heatmap** with proper triangle handling, or **small multiples of retention curves** | Bertin's three-component limit; either layout works. |
| "What does the same metric look like across many subgroups?" | **Small multiples** | "The constancy of the design allows the viewer to focus on changes in the data" (Tufte p. 42). |
| "Is this dataset normally distributed / what's the shape?" | **Histogram + QQ plot** | Histogram for shape, QQ for tail behavior; both, not one. |
| "How do these two distributions compare?" | **Overlaid density plots** or **paired histograms (back-to-back)** or **boxplot strip** | Density for shape, boxplot for summary, overlaid for direct comparison. |
| "What changed between two time points across many items?" | **Slopegraph** (Tufte's invention) | Two parallel y-axes; line per item; slope = change. Beats grouped bars. |

## 2. Pick by data shape

Same idea, but indexed by what's in your data frame.

| Data | Default chart |
| --- | --- |
| One number, one comparison | A sentence. ("MRR grew from $1.2M to $1.5M.") |
| 2-5 numbers | A text-table or labeled dot. |
| 5-20 numbers | A real table with row/column grouping. |
| 20-100 numbers | Bar / dot plot if comparing categories; small multiples; semi-graphic table with sparklines. |
| Many numbers, one variable, one dimension | Histogram, strip plot, or beeswarm. |
| Many numbers, two continuous variables | Scatterplot (range-framed). |
| Many numbers, time + one variable | Line chart. |
| Many numbers, time + multiple series | Small-multiple lines, or single line chart with direct-labeled series (≤ 5 series). |
| Many numbers, two categorical dimensions, one quantity | Heatmap (reorderable matrix), or grouped bar chart. |
| Many numbers, three+ continuous variables | Scatterplot matrix, small multiples of scatters, or parallel coordinates. |
| Many numbers, geographic point data | Map with proportional symbols. |
| Many numbers, geographic areal data | Proportional symbols overlaid on areas (Bertin's preference), choropleth (for communication), or dot density. |
| Many numbers, network / relationship | Adjacency matrix (dense), force-directed (small + sparse), arc/chord (ordered). |
| Many numbers, hierarchy | Treemap, sunburst, or indented table. Treemap for area-as-quantity; indented table for navigation. |
| Many numbers, flow | Sankey/alluvial. |
| Time series with multiple seasonalities | STL decomposition (Cleveland & Terpenning, Tufte p. 35); separate trend, seasonal, residual. |

## 3. Common wrong picks and what to use instead

| Common pick | Problem | Use instead |
| --- | --- | --- |
| 3-D bar chart | Occludes; Lie Factor inflation; perspective distortion | Flat bar chart, or small multiples |
| 3-D pie chart | All the above + pie's existing problems | Sorted bar chart, or table |
| Pie chart (>5 slices) | Hard to compare angle/area; no order | Sorted bar chart |
| Pie chart (≤5 slices) | Still hard to compare; takes more space than a sentence or labeled stacked bar | Sentence, text-table, or single stacked bar |
| Donut chart | All pie chart problems + a hole that adds nothing | Sorted bar chart |
| Gauge / speedometer | Single value + threshold could be a sentence ("87% of target") or a bullet chart | Bullet chart, or annotated sparkline |
| Stacked bar (many categories, comparing absolutes) | Only the bottom category has a real baseline | Small-multiple bars, or grouped bars |
| Stacked area (many categories) | Same — only bottom has a baseline | Small-multiple line charts, or single 100%-normalized stacked area |
| Spider/radar chart | Area changes nonlinearly with values; axis ordering is arbitrary | Small-multiple bars or a parallel coordinates plot |
| Word cloud | Size encodes frequency badly (perceived area is non-linear); position is meaningless | Sorted bar chart of top terms |
| Treemap (many small cells) | Tiny cells unreadable; comparison of non-adjacent cells hard | Sorted bar of top N + "other" |
| Bubble chart (3 variables: x, y, size) | Bubble area is non-linearly perceived | Use only when size is a rough magnitude (e.g., GDP); never for precise quantities |
| Connected scatter | Mixes "trajectory" and "scatter" semantics; readers misinterpret | Two charts (one scatter, one line over time) |
| Polar area / Coxcomb / Nightingale rose | Area encoding for angular categories — Bertin's dimensional mismatch | Sorted bar chart |
| Choropleth of pre-classed intervals (when discovering, not communicating) | Bertin's "Q → O" critique | Proportional symbols on points; or accept that choropleth is for communication only |
| Force-directed graph of >50 nodes | Hairball | Adjacency matrix with row/column permutation |
| Heatmap with rainbow palette | Perceptual non-uniformity; color-blind unsafe | Viridis / magma / sequential ColorBrewer |
| Bar chart sorted alphabetically | Wastes the only ordering dimension you have | Sort by value (descending if "biggest matters") |
| Y-axis truncated to exaggerate | Tufte's Lie Factor violation | Start at zero for bars (always); be honest about line-chart range with a clear axis label |
| Two-y-axis chart | Reader can't compare slopes between scales | Two stacked small multiples with shared x |
| KPI tile + bare big number | "Compared to what?" — no context | Sparkline + current value + change vs prior period |

## 4. The fast decision tree

```
Start: "What is my reader's question?"

├─ "Is this number good or bad?"
│    → Sparkline + current value + target/benchmark + change
│
├─ "How does X change over time?"
│    ├─ 1 series → range-framed line chart with annotations
│    ├─ 2-5 series → single chart, direct-labeled at line ends
│    └─ >5 series → small multiples (one panel per series, shared y)
│
├─ "How do N things compare?"
│    ├─ N ≤ 6 and the comparison is one number each → text-table
│    ├─ N ≤ 6 and you need a chart → sorted horizontal bar
│    ├─ 6 < N ≤ 30 → sorted horizontal bar (horizontal so labels fit)
│    └─ N > 30 → ranked dot plot, or top-N + "other"
│
├─ "What is the distribution of X?"
│    ├─ n ≤ ~100 → strip plot or beeswarm (every point visible)
│    ├─ 100 < n ≤ ~10K → histogram (with overlay density if needed)
│    └─ n > 10K → density plot, or histogram with log y, or 2D density if X-Y
│
├─ "What is the relationship between X and Y?"
│    ├─ 1 dataset, no groups → scatterplot (range-framed)
│    ├─ groups → scatterplot with color/shape per group, or faceted
│    └─ very dense → hexbin, 2D histogram, or smoothed density
│
├─ "Where on the map is X?"
│    ├─ Discovery / quantitative → proportional symbols (Bertin)
│    ├─ Communication of pre-classed → choropleth with sequential palette
│    ├─ Dense point events → dot density or hex bin map
│    └─ Flow → arrow map or chord diagram (if non-geographic)
│
├─ "How are these connected?"
│    ├─ Sparse, < 50 nodes → force-directed or arc diagram
│    ├─ Dense or > 50 nodes → adjacency matrix with permutation
│    └─ Hierarchy → tree, treemap, or indented table
│
├─ "What is the composition?"
│    ├─ 1 whole → sorted bar (NOT pie, NOT donut)
│    ├─ Composition over time → small-multiple bars, OR 100% stacked area
│    └─ Two-dimensional breakdown → heatmap / reorderable matrix
│
└─ "How did things change between two points?"
     └─ Slopegraph (Tufte): two parallel y-axes, lines connecting paired values
```

## 5. Niche cases that need their own answer

- **Survival curves** → Kaplan-Meier plot. Step function, range-framed, censoring marks visible.
- **Funnel conversion** → A horizontal bar chart showing absolute counts at each stage *and* a table of stage-to-stage conversion %. Avoid the literal "funnel chart" — the slope encoding is misleading.
- **A/B test results** → Confidence intervals around the effect estimate, not p-values. Forest plot if multiple variants.
- **Forecast with uncertainty** → Line for point forecast; fan/ribbon for prediction interval(s); show 50% and 90% bands.
- **Cohort retention** → Triangular heatmap with diagonal cells faded; OR small-multiples of retention curves (every cohort starts at month 0).
- **Calendar of activity** (e.g., GitHub contribution graph) → Calendar heatmap. Sequential single-hue palette.
- **Hour × day of week patterns** → 2D heatmap (rows = hour, columns = day). The classic "subway ridership" chart.
- **Multi-metric comparison across N items** (e.g., countries on GDP, life expectancy, education) → Slopegraph for two points; parallel coordinates for many; small-multiple bar for snapshot.
- **Distribution-free uncertainty visualization** → Quantile dotplot or HOPs (Hypothetical Outcome Plots).
- **Rare-event time series** (e.g., earthquakes, crashes) → Rug plot below a smooth density; OR event timeline with markers.
- **Likert / agreement data** → Centered (diverging) stacked bar with the neutral category split around zero.

## Two principles that govern every pick

1. **Match the chart to the reader's question, not to the dataset's shape.** The same data answers different questions with different charts.
2. **The lightest container that fits wins** — sentence > table > semi-graphic > chart. Going up the ladder when the lower rung would do is the most common visualization mistake.
