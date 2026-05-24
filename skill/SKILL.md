---
name: data-visualization
description: Apply Bertin's *Semiology of Graphics* (visual-encoding grammar) and Tufte's *Visual Display of Quantitative Information* (integrity, density, craft) to any chart, plot, dashboard, infographic, table, map, or network diagram. Use whenever the user designs, builds, reviews, or critiques data visualizations — matplotlib/plotly/d3/ggplot/vega/recharts/chart.js code, BI dashboards (Looker/Tableau/Power BI/Metabase/Superset), report figures, scientific plots, financial charts, exec slides with numbers, or choropleth maps. Trigger even when the user doesn't name Bertin or Tufte and only describes a chart in prose ("make a bar chart of …", "this dashboard feels cluttered", "is this graph misleading?", "what kind of map should I use?"). Walks Bertin's encoding workflow (components → visual variables → perceptual levels), then applies Tufte's filters (Lie Factor, data-ink ratio, chartjunk removal, format choice) for concrete, defensible advice.
---

# Data Visualization — Bertin + Tufte

A working synthesis of the two foundational texts on quantitative graphics:

- **Jacques Bertin, *Semiology of Graphics: Diagrams, Networks, Maps* (1967)** — the *grammar*. A systematic theory of which visual encoding fits which data, derived from the perceptual properties of the eye.
- **Edward Tufte, *The Visual Display of Quantitative Information* (1983/2001)** — the *standards*. A prescriptive set of integrity, density, and craft rules built on a critique of how published graphics actually fail.

Bertin tells you **which marks to make**. Tufte tells you **whether the marks are honest, dense, and clean**.

This skill loads only the most-used material in the body. Read `references/bertin.md` for the full grammar (visual variables, image theory, matrix permutation, networks, maps). Read `references/tufte.md` for the full Tufte canon (Lie Factor, data-ink, chartjunk, anti-patterns, examples). Cite an author by name when it sharpens the argument.

## The Combined Workflow

For any visualization request — designing, building, reviewing, or critiquing — walk these four phases:

### Phase 1 — Analyze the information (Bertin)

Before any mark is drawn, answer three questions about the **data itself**:

1. **What is the invariant?** The constant subject the data is *about* (e.g., "stock X, daily closing price"). It is not a variable; it is the unchanging frame.
2. **What are the components?** The dimensions along which the data varies. Count them. Each one will need a visual variable.
3. **For each component, classify its level of organization:**
   - **≠ Qualitative / nominal** — categories without universal order (industries, regions, species). Re-orderable.
   - **O Ordered** — single universal order, equidistant categories (cold/warm/hot, low/medium/high). Not re-orderable without losing meaning.
   - **Q Quantitative** — countable units permitting ratio statements ("twice as big", "1/4 of").
   - Bertin's nesting rule: Q ⊃ O ⊃ ≠. A quantitative variable can be read at all lower levels; the reverse is not true.

Also note each component's **length** — the number of distinguishable categories. Length drives which visual variables are even available.

### Phase 2 — Choose the encoding (Bertin)

You have **eight visual variables** and must assign each component to one. The variable's perceptual capacity must equal or exceed the component's level.

| Visual variable | Selective (≠) | Associative (≡) | Ordered (O) | Quantitative (Q) |
| --- | :---: | :---: | :---: | :---: |
| **Position (the plane, X and Y)** | ✓ | ✓ | ✓ | ✓ |
| **Size** | ✓ (limited) | ✗ (dissociative) | ✓ | ✓ |
| **Value** (light-to-dark) | ✓ | ✗ (dissociative) | ✓ | ✗ |
| **Texture** | ✓ | ✓ | ✓ | ✗ |
| **Color (hue)** | ✓ | ✓ | ✗ | ✗ |
| **Orientation** | ✓ (point/line) | ✓ | ✗ | ✗ |
| **Shape** | ✗ | ✓ | ✗ | ✗ |

Rules that follow from this table:

- **Quantitative components must use position, size, or value.** Color and shape cannot carry quantity; the eye does not read them as ratios.
- **Ordered components must use position, size, value, or texture.** Color hue has no innate order — only red ≈ "hot" is universal.
- **Reserve the two planar dimensions (X, Y) for the most important components**, because position is the only variable that does all four jobs.
- **Size and value are "dissociative"**: when you use them, smaller/lighter marks lose visibility, so they dominate any combination they enter. Be aware before combining.
- **Shape has no selectivity**: it answers "is this similar?" but not "where are all the X?" — use sparingly and only at the elementary reading level.
- **For three components, an "image" is achievable** — perceived in a single glance: two planar dimensions plus one ordered retinal variable. Beyond three, you need **small multiples** (Tufte's term) or what Bertin calls "collections of comparable images."

### Phase 3 — Check the integrity (Tufte)

Once the encoding is chosen, audit for the six principles of graphical excellence (Tufte pp. 51, 74):

1. **Lie Factor in [0.95, 1.05].** `Lie Factor = (size of effect shown in graphic) / (size of effect in data)` (Tufte p. 57). Anything bigger is dishonest.
2. **Visual dimensions ≤ data dimensions.** A 1-D number gets length or position, not area or volume. Perceived area scales as `actual_area^0.8` — viewers cannot read 2-D/3-D quantitative encodings reliably (Tufte p. 55).
3. **Show data variation, not design variation.** One scale per axis. No shifting baselines, no irregular tick intervals, no perspective shifts, no mid-chart redesigns.
4. **For money over time, always deflate.** Nominal dollars silently change scale. Same for any rate normalized by a changing denominator (population, area).
5. **Quote data in context.** Two-point before/after charts are usually lies. Widen the window until the comparison is fair ("compared to what?").
6. **Maximize the data-ink ratio, within reason** (Tufte p. 88, 91). `Data-ink ratio = data-ink / total ink`. Erase anything that, if removed, costs no data-information.

### Phase 4 — Clean and refine (Tufte)

Standard erasures that almost always improve a chart:

- Heavy frame → **range-frame** (lines extend only between the data's min and max).
- Dark grid → mute to light gray or erase. "Dark grid lines are chartjunk" (Tufte p. 108).
- Tick marks on bar charts → cut thin white gridlines *through* the bars instead.
- Round-number tick labels → use the actual data values (min, median, Q1, Q3, max).
- Legend box → label series directly on the data.
- Cross-hatching, dot screens, dense parallel rulings → flat gray tints. (Tufte criticized Bertin's defense of moiré here — Tufte p. 107; Bertin replied that controlled vibration can emphasize, p. 98. In practice, default to Tufte; reach for vibration only when emphasis is the *point*.)
- Boxes drawn around the chart → erase.

The three banned species of chartjunk (Tufte p. 116): **moiré vibration, the grid, the duck**. A "duck" is a graphic so taken over by decoration that the data become design elements.

Heuristic: vigorous pruning can erase ~65% of original ink with no data loss.

## Format Choice — pick the lightest container

Before drawing anything, choose the right vehicle. Going up this ladder when the lower rung would do is the most common visualization error (Tufte pp. 171–172):

1. **A sentence** — for one or two numbers. "Profit grew from $1.2M to $1.5M" beats a two-bar chart.
2. **A text-table** — sentences aligned in columns.
3. **A real table** — almost always beats a chart for **≤ 20 numbers** (Tufte p. 56). Sort rows by content; group with rules into topical paragraphs.
4. **A semi-graphic** — table with sparklines, bullets, or in-line dot-plots.
5. **A graphic** — when the data has real variability, many entries, or you need to reveal pattern. Reach for a **scatterplot** first (Tufte p. 47: "the greatest of all graphical designs").

Bertin's parallel rule (p. 254): when the data has > 3 rows and both axes are reorderable, the right construction is often a **reorderable matrix** — permute rows and columns until similarity blocks emerge. This is the "lossless simplification by ordering" that almost no business tool offers.

## Bertin's three graphic functions

Match the construction to the function (Bertin pp. 160–164):

- **Recording / inventory** — comprehensive, need not be memorizable. Use dense matrices, supertables, full-resolution maps. Optimize for completeness.
- **Communicating / message** — memorizable, need not be comprehensive. Simple, one-glance image. Optimize for retention.
- **Processing** — must be both comprehensive *and* image-graspable, because you are using the graphic to discover patterns. Optimize for transformability (reorderable matrices, image-files, small multiples).

A common dashboard failure is using a *message* form (a single KPI tile) for a *processing* task (looking for trends, anomalies, correlations).

## Map advice (Bertin's specialty)

When the data is geographic:

- **The plane = the surface of the earth.** This commits both X and Y. Any other component must be encoded with a retinal variable.
- **For quantitative geographic data, the only honest representations are:** proportional symbols (Bertin's "regular pattern of proportional points"), dot maps, or relief/3D — *not* choropleths of pre-classed intervals if you want to discover structure rather than communicate a known one (Bertin p. 374).
- **Choropleths convert Q into O.** Choosing class intervals before plotting is "transform[ing] the component Q into an interpretation O." It's fine for *communication*, dishonest for *discovery*.
- **For movement, prefer arrows** with weight at the head; ordered retinal variables for sequence; small multiples for time-series of maps.
- **Cartograms** (warp space to encode magnitude) are sometimes acceptable when *the* question is non-geographic — but the reader loses geography.
- **Chartmaps** (diagrams scattered on a map) are "hardly ever efficient" when the inset has more than ~3 components (Bertin p. 392).
- **Isarithms (contour lines) are weak**: they show only slope, not absolute height, volume, or even direction. Use only when slope *is* the answer.

For the full cartographic ruleset (projections, scale, generalization, base maps), see `references/bertin.md`.

## Network and relationship advice

When the data describes relationships among elements of the same set (people, websites, citations, parts of a system):

- The plane is **meaningless at the start** — plot first, then seek the arrangement that **minimizes meaningless intersections** (Bertin p. 287).
- **Start with a circular layout** to see the problem; permute to reduce intersections.
- If the elements have an ordering (size, time, hierarchy), give the plane that order (e.g., rectilinear layout with ordered axis).
- For dense networks, an **adjacency matrix with a reorderable permutation** often beats a node-link diagram. The matrix scales further and reveals block structure.
- **A map is just an ordered network** — the geographic order is fixed and given, that is all.

## Anti-Patterns (and why)

- **Pie charts** — Bertin's framework allows them in principle (sectors are positional), but they cannot be re-permuted or compared, and Tufte is explicit: "pie charts should never be used" (Tufte p. 171). For categorical breakdowns, use a sorted bar chart or a table.
- **3-D bar / pie / perspective charts** — Lie Factor explosions, occluded data, Necker-illusion flips. Always flatten.
- **Stacked area / stacked bar with many series** — only the bottom series has a real baseline; switch to small multiples or a line chart.
- **Hidden, shifted, or truncated baselines.**
- **Tall, narrow aspect ratios for growth data** — engineered to look like a skyrocket. Default toward ~1.5 wider than tall (Tufte; Playfair: 92% of his graphics were wider than tall).
- **Color used to encode quantity** without a natural order — color has no innate ordering. Use a gray scale or a perceptually-uniform sequential palette.
- **Two-variable color maps** — Bertin and Tufte agree these are usually puzzles (Tufte p. 147: "a sure sign of a puzzle is that the graphic must be interpreted through a verbal rather than a visual process").
- **Two-point before/after charts** — strip context. Widen the window.
- **Encoded shadings with the legend elsewhere on the page** — force the eye to ping-pong. Label on the data.
- **All-caps sans-serif crinkly computer labels** — the "computer duck" (Tufte p. 115).
- **Vertical Y-axis labels** — rewrite horizontal.
- **Pictograms that scale 2-D or 3-D for 1-D data** ("shrinking doctor", oil barrels).
- **Redundant encoding** — bar height labeled with its number *and* shaded *and* with a tick. Pick one.
- **Pre-classed choropleth maps used for discovery** — Bertin's "transforms Q into O" critique.
- **Chartmaps with >3 components** — overload; switch to small-multiple maps.
- **Networks with random layout** — start from circular, then permute.

## Where Bertin and Tufte disagree

- **Moiré vibration**: Bertin says a designer must "flirt with ambiguity without succumbing to it" — controlled vibration can emphasize (p. 98). Tufte says moiré "has no place in data graphical design" (p. 107). Default to Tufte. Use vibration only when emphasis is the explicit purpose, not as a fill pattern.
- **Color**: Bertin treats hue as one variable among eight with definable selectivity/associativity. Tufte is more aphoristic and skeptical of color in quantitative encoding. Both agree color cannot encode order or quantity reliably.
- **Pie charts**: Tufte bans them. Bertin's framework permits them as a "special construction" for limited categorical data, but his analysis shows tables almost always win.
- **Style**: Bertin is systematic and scientific; Tufte is aphoristic and aesthetic. In practice, use Bertin to *decide what to draw* and Tufte to *judge whether it's drawn well*.

## When the user asks you to write chart code

Defaults to apply to any matplotlib / plotly / ggplot / d3 / vega-lite / recharts / chart.js / observable output:

- **Range-frame look**: hide top and right spines; trim bottom and left spines to data range.
- **Grid**: off by default. If needed, light gray, behind the data, with as few lines as possible.
- **Tick labels**: actual min/max (and median or other landmarks) — not round numbers.
- **Color**: a single neutral (dark gray) for the data. Reserve color for grouping or emphasis. Never for quantity unless using a perceptually-uniform sequential scale.
- **Aspect ratio**: ~1.5 wider than tall, unless the data dictates otherwise (Tukey: wiggly curves wider, smooth curves can be taller).
- **Legends**: label series directly at the line end when there are ≤ 5 series.
- **Annotations**: label important events on the data itself.
- **Caption**: short — units, source, n, date.
- **Never**: 3-D, pies, gradient fills, drop shadows, beveled bars, all-caps body text.

## The Review Checklist

When reviewing or critiquing any visualization, walk this out loud:

1. **What is the invariant and what are the components?** (Bertin Phase 1) Count them. Classify each as ≠/O/Q. Note each length.
2. **Which visual variable encodes each component?** (Bertin Phase 2) Does the variable's perceptual capacity meet the component's level?
3. **Could a permutation of rows/columns/categories reveal structure that the current order hides?** (Bertin matrix-permutation)
4. **Estimate the Lie Factor.** Outside [0.95, 1.05]? Fix.
5. **Visual dimensions vs. data dimensions.** Is a 1-D number encoded with area or volume? Flatten.
6. **Honest scale?** Inflation-adjusted? Per-capita? Log if relationship is multiplicative? Consistent baselines and tick intervals?
7. **In-context comparison?** ("Compared to what?")
8. **Non-data-ink to erase.** Frame, grid, ticks, legend, gradients, shadows, 3-D, hatching, decorative icons.
9. **Redundant data-ink.** Label + height + tick is two redundancies.
10. **Format choice.** Could a table, a sentence, or a small multiple carry the same load better?
11. **Aspect ratio.** Default ~1.5 wider than tall.
12. **Color use.** Quantitative → gray scale / perceptual sequential. Categorical → distinct hues, accessible to 5–10% color-blind viewers (avoid red/green for essential contrasts).
13. **Small-multiple check.** Most multi-series charts are clearer as 4–12 panels with a constant design.
14. **Read the words on the chart.** Title, caption, annotations, units, source. "Data graphics are paragraphs about data" (Tufte p. 174).
15. **Function fit.** Is this a recording, a message, or a processing graphic? Does the construction match the function? (Bertin)

## Dashboards specifically

Dashboards are a special case neither author wrote about directly, but their principles apply with these emphases:

- A dashboard is a **small-multiple of small-multiples** — design constancy across panels frees the eye to see data.
- Each panel should answer one question; the title is that question.
- **Sparklines** (intense, word-sized graphics) belong in tables alongside the current value — almost always better than a separate "trend" chart.
- The default panel is a range-framed line or dot-plot, not a gauge, not a donut, not a 3-D bar.
- Color carries category, never quantity — use position or length for quantity.
- "Compared to what?" governs every KPI tile — a number alone is design malpractice. Add a real-units comparison (target, prior period, baseline, percentile).
- High data density on a dashboard means *more numbers in less space*, not more panels.
- The dashboard's job is usually **processing** (Bertin's third function): the user is looking for anomalies. Optimize for transformability, not retention.

## Numeric rules of thumb

| Quantity | Source | Value |
| --- | --- | --- |
| Acceptable Lie Factor | Tufte p. 57 | 0.95 – 1.05 |
| Table vs. graphic threshold | Tufte p. 56 | ≤ 20 numbers → table |
| Eye's resolution (with light grid) | Tufte p. 154 | ~625 distinguishable points/sq in |
| Default aspect ratio | Tufte / Playfair | ~1.5 wider than tall |
| Color-blind population | Tufte p. 176 | 5 – 10% |
| Data-density benchmark (top journals) | Tufte p. 160 | 17 – 48 entries/sq in |
| Erasable ink in a typical redesign | Tufte p. 96 | ~65% |
| Maximum components in a single image | Bertin p. 154 | 3 |
| Minimum mark size for color discrimination | Bertin p. 107 | 1.5 mm |
| Selective color steps at maximum saturation | Bertin pp. 105–106 | ~6 with adjusted values |
| Selective shape variants | Bertin pp. 113, 196 | 3 (point, dash, cross) |
| Optimal % black in a meaningful area | Bertin p. 198 | 5 – 10% |
| Optimal angle for elementary legibility | Bertin p. 196 | ~70° |
| Max signs/cm² in a figuration | Bertin p. 194 | ~10 |
| Max signs/cm² in a true image | Bertin p. 194 | "practically no maximum" |

## When the user pushes back

Useful one-liners from each:

**Bertin:**
- "One does not 'read' a graphic; one asks three questions of it." (preface)
- "A graphic should not show only the leaves; it should show the branches as well as the entire tree." (preface)
- "An order will not be perceptible if the variable is not ordered; a ratio will not be perceptible if the variable is not quantitative." (p. 32/52)
- "The plane is meaningful — absence of signs signifies absence of phenomena." (p. 46/64)
- "Determining the steps is the goal of the graphic operation, not its means." (p. 374)

**Tufte:**
- "Above all else show the data." (p. 100)
- "Show data variation, not design variation." (p. 61)
- "Graphics must not quote data out of context." (p. 71)
- "If the statistics are boring, then you've got the wrong numbers." (p. 76)
- "For non-data-ink, less is more. For data-ink, less is a bore." (p. 168)
- "It is better to violate any principle than to place graceless or inelegant marks on paper." (p. 184)

## Deeper material — load on demand

Read these only when the user's task touches the topic. Don't preload everything.

| File | Read when… |
| --- | --- |
| `references/bertin.md` | The task needs the full grammar — visual-variable selection beyond the SKILL.md table, image theory, matrix permutation, networks, projections, cartographic generalization. |
| `references/tufte.md` | The task needs the full canon — the Lie Factor catalogue, data-ink redesigns, chartjunk taxonomy, small multiples, named examples (Minard, Snow, Anscombe, Playfair, Marey, Connecticut speeding, NYT lies), or quotes by theme. |
| `references/color.md` | The user is choosing a palette, asking about color, has a rainbow/jet colormap, is building for color-blind users, or is moving to dark mode. Covers viridis family, ColorBrewer, Okabe-Ito, OKLCH, diverging anchoring, dark mode. |
| `references/chart-picker.md` | The user asks "what chart should I use?" or describes data and asks for the right form. A decision tree indexed by question and by data shape, plus common wrong picks and what to use instead. |
| `references/accessibility.md` | The user is shipping to production, asks about color-blind safety, alt text, screen readers, contrast ratios, dark mode, or animation/motion concerns. |
| `references/modern-charts.md` | The user mentions or considers a chart type post-1983: beeswarm, raincloud, ridgeline, sankey, treemap, calendar heatmap, hex bins, parallel coordinates, slopegraph, bump chart, waffle, bullet, violin, streamgraph, sparkline. Or asks whether a trendy form is the right pick. |
| `references/storytelling.md` | The user is writing a report, building a deck, designing a dashboard headline, or asking how to make a chart "land." Covers takeaway titles, the headline-and-deck pattern, annotation patterns, visual hierarchy, chart sequencing, and the opinion-of-the-chart. |
| `references/statistical-viz.md` | The work touches uncertainty (error bars, CIs, prediction intervals), A/B tests, distribution comparison beyond shape, regression diagnostics, survival curves, ROC/PR, Bayesian posteriors, or p-value display. The skill's modern-statistics supplement. |

Cite by author/source when the citation sharpens the critique. Don't drown the user in attribution.

## Closing test

Before declaring a visualization finished, ask both authors' closing tests:

- **Bertin:** Can the three questions be answered in a single glance? *What are the X and Y components? What groups in X and Y are constructed by Z? What are the exceptions?*
- **Tufte:** If I removed everything that isn't data, what would I lose? If the answer is "nothing important," remove it.
