# Tufte Reference — Integrity, Density, and Craft

The full canon from *The Visual Display of Quantitative Information* (Edward R. Tufte, 2nd ed., 1983/2001), distilled. Page numbers are 2nd edition.

Where Bertin gives you a grammar, Tufte gives you a code of conduct. Both are needed; one without the other produces either dull-honest or dishonest-clever work.

## Table of contents

1. The creed and the corollaries
2. Graphical integrity — six principles and the Lie Factor
3. The data-ink ratio and standard erasures
4. Multifunctioning graphical elements
5. Data density and small multiples
6. Chartjunk: moiré, the grid, the duck
7. Format choice ladder
8. Anti-patterns catalogue
9. Code defaults (matplotlib/plotly/etc.)
10. Numeric rules of thumb
11. The Pantheon: people to invoke
12. Canonical positive exemplars (the canon to imitate)
13. Canonical negative exemplars (the cautionary canon)
14. Quotes by theme

---

## 1. The creed and the corollaries

The one-line creed (p. 13):

> "Excellence in statistical graphics consists of complex ideas communicated with clarity, precision, and efficiency."

Three working corollaries:

- **Above all else, show the data** (p. 100). The data is the point. Ink that does not serve the data is suspect.
- **If the data is boring, you have the wrong numbers** (p. 76) — not a decoration problem. Decoration cannot rescue a thin data set.
- **Design is choice** (p. 184). These principles generate options; they do not dictate answers. "It is better to violate any principle than to place graceless or inelegant marks on paper."

## 2. Graphical integrity — six principles and the Lie Factor

(p. 74, verbatim)

1. Representation of numbers on the surface should be **directly proportional** to the quantities represented.
2. Clear, detailed, thorough **labeling**; write explanations on the graphic; label important events in the data.
3. **Show data variation, not design variation.**
4. **Deflated, standardized monetary units** in time-series.
5. **Information-carrying dimensions ≤ data dimensions.**
6. **Don't quote data out of context.**

### Lie Factor

`Lie Factor = (size of effect shown in graphic) / (size of effect in data)` (p. 57).

Acceptable range: **0.95 – 1.05**. Outside that band is "substantial distortion, far beyond minor inaccuracies in plotting."

In practice almost all distortions overstate. The catalogued lies:

| Graphic | Lie Factor |
| --- | --- |
| Shrinking Family Doctor (LA Times 1979) | 2.8 |
| NYT NY State budget chart (1976) | high (multiple tricks) |
| Oil price (Time / Washington Post 1978) | 9.4 – 9.5 |
| NYT fuel economy "road" chart (1978) | 14.8 |
| NYT oil-barrel chart (1981, by volume) | 59.4 (Tufte's record) |

### Perceived-area power law

Perceived area = `actual_area^x` where `x = 0.8 ± 0.3` (p. 55). Viewers cannot read 2-D or 3-D quantitative encodings reliably. Never encode a 1-D quantity with 2-D area or 3-D volume.

### Context defeats truncation

The Connecticut traffic deaths example (pp. 71-72) is the canonical case. Two data points (1955 → 1956) make the speeding crackdown look decisive. Widening to nine years plus adjacent states reveals the decline was regional and already underway. Whenever a user shows you two data points, widen the window.

### Honesty over time

For money: always inflation-adjust. Nominal dollars silently change scale. Same logic for any rate normalized by a changing denominator (population, area, sample size).

## 3. The data-ink ratio and standard erasures

`Data-ink ratio = data-ink / total ink` (p. 88), or equivalently `1 − (fraction erasable without information loss)`.

The five concluding principles of data-graphic design (p. 100):

> 1. Above all else show the data.
> 2. Maximize the data-ink ratio.
> 3. Erase non-data-ink.
> 4. Erase redundant data-ink.
> 5. Revise and edit.

**"Within reason"** matters (p. 119). The maximization principle suggests designs; the final choice rests on statistical and aesthetic criteria — the procedure is one of *reasonable* data-ink maximizing, not slavish minimization.

### Standard erasures (always or almost-always improvements)

- Heavy chart frame → **range-frame**: lines extend only between the data's min and max (p. 125).
- Dark grid → mute to light gray or erase. *"Dark grid lines are chartjunk"* (p. 108).
- Tick marks on bar charts → cut thin white gridlines *through* the bars (p. 122).
- Round-number tick labels (0, 10, 20…) → use the actual data values (min, Q1, median, Q3, max) as labels.
- Legend box → label series directly on the data.
- Cross-hatching, dot screens, dense parallel rulings → flat gray tints.
- Boxes drawn around charts → erase.
- Redundant axis lines when bars already define the baseline → erase or thin.
- Redundant encoding (bar height + label + tick + shade encoding the same value) → pick one channel.
- Decorative asterisks when every comparison is significant → push to caption.

### Heuristic

Vigorous pruning can erase **~65% of original ink** without information loss (p. 96, Kuznicki & McCutcheon redesign).

The data-ink principle "makes good sense … for perhaps two-thirds of all statistical graphics"; for the rest the ratio is ill-defined or inappropriate (p. 91).

## 4. Multifunctioning graphical elements

(p. 133)

> "Mobilize every graphical element, perhaps several times over, to show the data."

Same ink, multiple jobs:

- **Frames carry data** → range-frame, quartile-frame (p. 127). A range-frame's lines tell you the data extent; a quartile-frame's small shift in the line tells you median and quartiles too — "ten extra numbers" at near-zero ink cost.
- **Axis labels are the actual data values**, not round numbers (p. 143).
- **Box plot** = edges encode quartiles; whiskers encode min/max; one chart element does five jobs.
- **Stem-and-leaf** = the digits *are* the data (Tukey's invention; if we're going to make a mark, make a meaningful one).
- **Grid lines, when used, fall at meaningful data events** (Playfair's debt graphic at war years), not at convenient round intervals.
- **Dot-dash-plot** replaces axes with marginal-distribution ticks (p. 128).
- **Data-based grid** — grid positioned by significant data events rather than at regular intervals.
- **Double-functioning labels** — coordinate labels relocated to also serve as data measures or marginal-distribution markers.
- **Friendly graphics** spell out words, run text left-to-right, place labels on the graphic itself, avoid encoded shadings/legends, accommodate color-deficient viewers, use upper-and-lower-case serif type.

## 5. Data density and small multiples

`Data density = entries in the data matrix / area of the graphic` (p. 155).

### Benchmarks (median entries/sq in, ca. 1980)

| Publication | Density |
| --- | --- |
| Nature | 48 |
| JRSS-B | 27 |
| Science | 21 |
| Wall Street Journal | 19 |
| Fortune / Times of London | 18 |
| JASA | 17 |
| NEJM | 12 |
| Economist | 9 |
| Le Monde | 8 |
| JAMA / NYT | 7 |
| Business Week / Newsweek / Scientific American / US Statistical Abstract | 5-6 |
| American Political Science Review | 2 |
| Pravda | 0.2 |

Records: NYC weather chart 181 numbers/sq in; sunshine map ~1,000/sq in; commune map ~9,000/sq in; galaxies map 110,000/sq in. USGS topographic quadrangles approach 250,000/sq in.

Most published business and news graphics waste 90%+ of the eye's resolving power (~625 distinguishable points/sq in).

### Small multiples

(pp. 42, 163-168)

Same chart design repeated for each slice (time, group, region). *"The constancy of the design allows the viewer to focus on changes in the data rather than on changes in graphical design"* (p. 42).

Small multiples are:
- Inevitably comparative
- Deftly multivariate
- Shrunken and high-density
- Based on a large data matrix
- Drawn almost entirely with data-ink
- Efficient to interpret
- Often narrative

This is Tufte's name for what Bertin calls "collections of comparable images." It is the default solution for any data with more than three dimensions.

### The Shrink Principle

(p. 162) Most graphics can be reduced to half their published size with no real loss of legibility. Applied repeatedly, this *is* the small multiple.

## 6. Chartjunk: moiré, the grid, the duck

(pp. 102-116)

> "Forgo chartjunk, including moiré vibration, the grid, and the duck." (p. 116)

### The three banned species

- **Moiré vibration** — cross-hatching, dot screens, dense parallel lines that shimmer against the eye's tremor. Replace with flat gray tints. (Tufte disagrees here with Bertin, who allows controlled vibration for emphasis — Tufte p. 107, Bertin p. 98. Default Tufte.)
- **The grid** — when dark or prominent. Suppress, or use a light gray under-layer only when the graphic also functions as a look-up table.
- **The duck** — a graphic so taken over by decoration or 3-D illustration that the data become design elements. Named for the duck-shaped roadside building in Flanders, NY, via Venturi/Scott Brown/Izenour's *Learning from Las Vegas*. Includes "we-used-a-computer-to-build-a-duck" graphics whose virtue is showing off the plotter.

### Survey of moiré

Tufte's 1980-82 sample (p. 105) found moiré in 21% of NEJM graphics, 17% of Science, 15% of Lancet, 12% of PNAS, 11% of Nature; 68% of SAS/GRAPH User's Guide, 53% of Tell-A-Graf Manual. Computer plotting industrialized chartjunk.

### Decoration vs. distortion

Decoration is allowed when it editorializes about substance. **Distorting the data-ink itself to fit a decorative scheme is not** (p. 59). The line is sharp: a decorative border or background pattern is acceptable; a 3-D bar chart whose perspective skews data is not.

Venturi/Scott Brown/Izenour, quoted approvingly (p. 112): *"It is all right to decorate construction but never construct decoration."*

## 7. Format choice ladder

(pp. 171-172)

Before drawing anything, pick the lightest container that fits. Going up this ladder when the lower rung would do is the most common visualization error:

1. **A sentence** — for one or two numbers. "Profit grew from $1.2M to $1.5M" beats a two-bar chart.
2. **A text-table** — sentences aligned in columns. Better than prose for ≤6 comparisons.
3. **A real table** — almost always beats a chart for **≤ 20 numbers** (p. 56). Sort rows by content (not alphabetically); group with horizontal rules into topical paragraphs.
4. **A semi-graphic** — table with sparklines, bullets, dot-plots, or in-line bars inside cells. The Consumer Reports frequency-of-repair tables are exemplary.
5. **A graphic** — when the data has real variability, many entries, or you need to reveal pattern. Reach for a **scatterplot** first. Tufte calls it "the greatest of all graphical designs" (p. 47).

**Supertable** (p. 172): large, elaborate table organized into topical "paragraphs" by horizontal rules, with rows ordered to tell a story. Tufte: *"One supertable is far better than a hundred little bar charts."* The NYT 410-number 1976/1980 presidential vote supertable he designed is the canonical example.

If the honest answer is "table," don't draw a bar chart to look more visual.

## 8. Anti-patterns catalogue

(with the *why*)

- **Pie charts** — no ordering along a visual dimension, low density, weak comparison. "Pie charts should never be used" (p. 171). A sorted bar chart or a table almost always wins. Multiple pies are worse.
- **3-D bar / pie / perspective charts** — Lie Factor explosions, occluded data points, Necker-illusion flips. Flatten.
- **Stacked area / stacked bar with many series** — only the bottom series has a real baseline; the others encode their value *plus* everything below. Switch to small multiples or a line chart.
- **Hidden, shifted, or truncated baselines** that hide negative values or compress to dramatize growth. The Day Mines, Inc., 1974 annual report hid a 1970 loss by starting bars at −$4.2M.
- **Tall, narrow aspect ratios for growth data** — engineered to feel like a skyrocket. Default toward horizontal (Playfair: 92% of his graphics were wider than tall, p. 181). Tufte criticizes the Fiorina federal-spending chart at 2.7× taller than wide.
- **Color used to encode quantity without a natural order** — color has no innate ordering. Use a gray scale (ordered) or a perceptually-uniform sequential palette. Two-variable color maps are usually puzzles.
- **Two-point before/after charts** — strip the context that would tell the true story. Always widen the window.
- **"Ordinal" charts that get only the direction right** — the Pravda School (p. 73). Fantasy magnitudes with the correct sign are still lies.
- **Encoded shadings with the legend elsewhere on the page** — force the eye to ping-pong. Label on the data.
- **All-caps sans-serif crinkly computer labels** — the computer-duck symptom (p. 115). Use upper-and-lower-case serif text where possible.
- **Vertical Y-axis labels** — make the reader turn their head. Rewrite horizontal.
- **Pictograms that scale 2-D or 3-D for a 1-D quantity** ("shrinking doctor", oil barrels) — Lie Factors 2.8 to 59 in Tufte's catalogue.
- **Redundant encoding** — bar height labeled with its number *and* shaded *and* with a tick *and*… Pick one channel.
- **Cosmetic asterisks** in stat charts when every comparison is significant — note it in the caption (p. 96).
- **Heavy grid "sea" that swamps the points** — the Hayward/Pauling atomic-volume chart (p. 89) before redesign.
- **Frame lines extending past the data** to the next round number — wastes the frame's potential as a data element.
- **Bilateral symmetry** that doubles design space without adding information (Chernoff faces, open bars, box plots with symmetric whiskers).
- **Animated transitions on quantitative data** (modern extension) — design variation masquerading as data variation; viewers cannot read it.

## 9. Code defaults (matplotlib / plotly / ggplot / d3 / vega-lite / recharts / chart.js / observable)

When generating chart code:

- **Range-frame look**: hide top and right spines; trim bottom and left spines to data range.
- **Grid**: off by default. If needed, light gray, behind the data, with as few lines as possible.
- **Tick labels**: actual min/max (and median or other landmarks) — not round numbers.
- **Color**: a single neutral (dark gray) for the data. Reserve color for grouping or emphasis. Never for quantity unless using a perceptually-uniform sequential scale.
- **Aspect ratio**: ~1.5 wider than tall, unless the data dictates otherwise. Tukey's rule of thumb: wiggly curves wider than tall; smooth curves can be taller.
- **Legends**: label series directly at the line end when there are ≤ 5 series.
- **Annotations**: label important events on the data itself, not in a separate caption block.
- **Caption**: short — units, source, n, date.
- **Never**: 3-D, pies, gradient fills, drop shadows, beveled bars, all-caps body text.

## 10. Numeric rules of thumb

| Quantity | Page | Value |
| --- | --- | --- |
| Acceptable Lie Factor | 57 | 0.95 – 1.05 |
| Table vs. graphic threshold | 56 | ≤ 20 numbers → table |
| Eye's resolution (with light grid) | 154 | ~625 distinguishable points/sq in |
| Type density benchmark (US Statistical Abstract) | 154 | 276 chars/sq in maximum |
| Default aspect ratio | (Playfair) | ~1.5 wider than tall |
| Color-blind population | 176 | 5 – 10% |
| Data-density benchmark (top journals) | 160 | 17 – 48 entries/sq in |
| Erasable ink in a typical redesign | 96 | ~65% |
| Perceived-area exponent | 55 | ~0.8 ± 0.3 |
| Perspective-design Lie Factor in NYT fuel chart | 57-58 | 14.8 |
| 3-D oil-barrel Lie Factor | 68 | 59.4 |
| Relational-graphics share in modern science | 47 | ~40% |

## 11. The Pantheon — people to invoke

- **Playfair** — inventor of most modern chart types (line, bar, pie); the standard for what one chart can carry.
- **Lambert** — pioneer of scientific time-series (1779).
- **Minard** — quantitative thematic maps; the Napoleon march.
- **Marey** — multivariate space-time narratives; the train schedule.
- **Snow** — analytic mapping for epidemiology.
- **Halley** — first true data maps (1686 trade winds).
- **Tukey** — box plot, stem-and-leaf, exploratory data analysis as a discipline.
- **Cleveland** — modern grammar of statistical graphics; perceptual studies.
- **Bertin** — semiology of graphics (a contemporary; Tufte critiques his defense of moiré, p. 107).

## 12. Canonical positive exemplars (the canon to imitate)

### Minard's *Carte Figurative* of Napoleon's Russian campaign (1869) — pp. 40-41

Six variables on one image: army size, 2-D position, direction of movement, temperature, dates. **"It may well be the best statistical graphic ever drawn."** Cite when arguing for unobtrusive multivariate design — many variables can fit on one chart when each is given its own clean line of sight.

### John Snow's cholera map, London, September 1854 — p. 24

Dots for cholera deaths, crosses for water pumps. Clustering around the Broad Street pump → handle removed → epidemic ended. The canonical case that *"graphical analysis testifies about the data far more efficiently than calculation."* Cite when arguing that *plotting on the right substrate* (a map) reveals what a table never would.

### Anscombe's quartet — pp. 13-14

Four data sets with identical N, means, regression, and r², but radically different scatterplots. The strongest single argument for **always plot before trusting summary statistics**. Cite when reviewing analyses or dashboards that report only aggregates.

### Playfair's *Commercial and Political Atlas* (1786) — pp. 32-34, 86-87

First known economic time-series. Balance of trade shown as the *difference* between import and export lines (graphical arithmetic). Also contains the first bar chart — invented reluctantly because year-over-year data was missing. The 1785 → 1786 redesign within a year (stripping grid apparatus) is the origin story for "Above all else show the data."

### Marey's Paris-Lyon train schedule (1880s) — pp. 31, 93

Horizontal = time, vertical = station (spaced by actual distance). Line slope = train speed; intersections = where opposing trains meet; horizontal segments = stop length. Cite as the template for **dense, multifunctioning, instantly-readable scheduling/flow graphics.**

### Voyager 2 Jupiter radio emissions, 1979 — p. 29

Multi-band, 10-hour rotation cycle. Dual horizontal labels (date *and* Jupiter-radii distance); third panel labels the spacecraft's orientation. **3.6M bits reduced to 18,900 plotted points** with no analytic loss. The template for combining multi-band overlays, dual-purpose axis labels, and a context strip beneath the main panel.

### NYT NYC weather, 1980 — pp. 30, 157

1,888 numbers — daily high/low overlaid on the normal range, plus humidity. **181 numbers/sq in.** The template for layering a baseline/normal band behind actuals so the viewer can read both data and forecast.

### Cleveland & Terpenning seasonal decomposition — p. 35

1,296 numbers; top series broken into seasonal + trading-day components to reveal real inflation-adjusted trend. The template for **decomposing a time series into interpretable components on one page.**

### LA air pollution small multiples (McRae) — pp. 42, 163

23 hourly frames of reactive hydrocarbon distribution; design held constant so attention goes to data shifts. **3 pollutants × 4 times × 2,400 cells = 28,800 readings on a page.** The canonical small-multiple.

### USGS topographic quadrangles — p. 161

~100 million bits per 17×23 inch sheet, approaching **250,000 bits/sq in**. The aspirational ceiling for high-information graphics.

### Consumer Reports frequency-of-repair tables — p. 167

"Particularly ingenious mix of table and graphic." The template for **semi-graphic tables** that beat any standalone chart for the same data.

### NYT 410-number 1976/1980 presidential vote supertable — p. 172

Designed by Tufte. Horizontal rules group into topical paragraphs; rows ordered to tell a story. *"One supertable is far better than a hundred little bar charts."* Cite whenever the user is about to build a dashboard of small charts.

### Connecticut traffic deaths, 1955-56, in regional context — pp. 71-72

The canonical "compared to what?" example. Two-point chart is misleading; nine-year series with adjacent states is honest. Cite whenever a user shows two data points.

### Cabrera's magnetic monopole event — p. 36

Before-after time series; flux jump in a superconducting loop stands out against an earlier liquid-nitrogen disturbance. The template for the **vivid before/after** when the data merits it.

### Doll's cigarettes vs. lung cancer scatterplot — p. 47

Classic example of relational graphics confronting causal theory with empirical evidence. Cite when arguing for **scatterplots over bar charts** in any analysis that touches causation.

## 13. Canonical negative exemplars (the cautionary canon)

### NYT 1978 fuel-economy "road" chart — pp. 57-58

**Lie Factor 14.8.** A perspective road drawing where a 53% actual change shows as a 783% visual change. Conventions reversed; dates stay constant size while road shrinks. Cite when criticizing perspective or 3-D quantitative encoding.

### NYT 1981 oil-barrel chart — p. 68

3-D barrels: a 454% price increase shown as a **27,000% volume increase** (Lie Factor 59.4 — Tufte's record). The reductio of dimensional mismatch.

### Day Mines, Inc., 1974 annual report — p. 54

"Disappearing baseline" — bars start at ~ -$4.2M to hide a 1970 loss, same lie repeated four times. Cite when a user proposes any custom baseline.

### Pravda industrial-production chart, 1982 — p. 73

**The Pravda School of Ordinal Graphics** — every chart with a crystal-clear direction and fantasy magnitudes. Cite when a chart's direction is right but the magnitudes are off.

### "The Shrinking Family Doctor," LA Times 1979 — p. 66

Area encodes a 1-D ratio. Lie Factor 2.8. The textbook case of the dimensional-mismatch pictogram.

### American Education 1972-76 age-structure chart — p. 113

"May well be the worst graphic ever to find its way into print": five colors, fake 3-D perspective, **five real data points**. The reductio of duck-form.

### Brazilian textile-industry duck chart (Rio, 1929) — p. 103

Stack of moiré-screened bars; archetypal vibrating bad graphic. Cite for any cross-hatched fill.

### California Water Atlas crop-water duck — p. 114

A "superbly produced duck" — proves that even excellent execution does not redeem the duck form.

### JASA "proper form" graphic (1976 style sheet) — p. 105

**131 line-strokes and 15 digits to communicate trivial information.** Indicts the statistical profession's own house standard. Cite when defending a redesign against "but that's the convention."

### Hayward's atomic-volume vs. atomic-number plot (Pauling 1947) — pp. 97-100

*"63 dark grid marks arrayed over the data plane like a precision marching band of 63 mosquitoes."* Original data-ink ratio < 0.6; redesigned ≈ 0.9 by erasing ticks and frame. The textbook redesign sequence.

### NYT 1976 New York State budget chart — pp. 66-67

"Hyperactive design": three parallelepipeds pushed onto a forward optical plane; type stretched for old years, squeezed for recent; horizontal arrows for old, vertical arrows for new. Actual budget didn't increase over the last nine years shown. Cite when any chart uses multiple decorative tricks at once.

### Vauthier's "mountain-to-the-sea" color scheme — p. 148

Anti-pattern: forces mnemonics; arbitrary color order. Use to argue against arbitrary quantitative color encodings.

### OMB 5-color 3-bar chart — p. 155

**4 numbers in 26.5 sq in = 0.15 numbers/sq in.** The canonical low-density failure mode.

### Israel Atlas pie charts — p. 171

Heavily encoded multi-pie example. The textbook indictment of pies, esp. multiple pies.

### Tufte's own 1973 *APSR* "Seats and Votes" graphic — p. 177

Self-criticized as heavy-handed, clotted ink, puffed-up small dataset, mismatched lettering. Use as evidence that even Tufte fails his own principles when he over-decorates.

## 14. Quotes by theme

### The creed

- "Excellence in statistical graphics consists of complex ideas communicated with clarity, precision, and efficiency." (p. 13)
- "Graphical excellence is that which gives to the viewer the greatest number of ideas in the shortest time with the least ink in the smallest space." (p. 51)
- "Graphical excellence is nearly always multivariate." (p. 51)
- "Graphical excellence requires telling the truth about the data." (p. 51)
- "Graphics reveal data. Indeed graphics can be more precise and revealing than conventional statistical computations." (p. 13)
- "At their best, graphics are instruments for reasoning about quantitative information." (p. 7)

### Honesty

- "Show data variation, not design variation." (p. 61)
- "A steady canvas makes for a clearer picture." (p. 61)
- "Graphics must not quote data out of context." (p. 71)
- "Graphics often lie by omission, leaving out data sufficient for comparisons." (p. 71)
- "The number of information-carrying (variable) dimensions depicted should not exceed the number of dimensions in the data." (p. 68)
- "When a chart on television lies, it lies tens of millions of times over." (p. 73)
- "Every chart has a crystal clear direction coupled with fantasy magnitudes." — on the Pravda School (p. 73)
- "It is wrong to distort the data measures — the ink locating values of numbers — in order to make an editorial comment or fit a decorative scheme." (p. 59)

### Show the data

- "Above all else show the data." (p. 87, 100)
- "A large share of ink on a graphic should present data-information, the ink changing as the data change." (p. 88)
- "Every bit of ink on a graphic requires a reason. And nearly always that reason should be that the ink presents new information." (p. 91)
- "Maximize the data-ink ratio, within reason." (p. 91)
- "Erase non-data-ink, within reason." / "Erase redundant data-ink, within reason." (pp. 91, 95)
- "Just as a good editor of prose ruthlessly prunes out unnecessary words, so a designer of statistical graphics should prune out ink that fails to present fresh data-information." (p. 95)

### Chartjunk

- "Forgo chartjunk, including moiré vibration, the grid, and the duck." (p. 116)
- "Dark grid lines are chartjunk." (p. 108)
- "Moiré vibration is an undisciplined ambiguity, with an illusive, eye-straining quality that contaminates the entire graphic. It has no place in data graphical design." (p. 107)
- "Chartjunk can turn bores into disasters, but it can never rescue a thin data set." (p. 116)
- "No information, no sense of discovery, no wonder, no substance is generated by chartjunk." (p. 116)
- "Like weeds, many varieties of chartjunk flourish." (p. 102)
- Venturi/Scott Brown/Izenour (quoted approvingly): "It is all right to decorate construction but never construct decoration." (p. 112)

### Density and small multiples

- "Mobilize every graphical element, perhaps several times over, to show the data." (p. 133)
- "The constancy of the design allows the viewer to focus on changes in the data rather than on changes in graphical design." (p. 42)
- "For non-data-ink, less is more. For data-ink, less is a bore." (p. 168)
- Tukey (quoted): "If we are going to make a mark, it may as well be a meaningful one." (p. 134)

### Format choice

- "Tables usually outperform graphics in reporting on small data sets of 20 numbers or less." (p. 56)
- "Small, noncomparative, highly labeled data sets usually belong in tables." (p. 33)
- "Pie charts should never be used." (p. 171)
- "The scatterplot and its variants is the greatest of all graphical designs." (p. 47)
- "Time-series displays are at their best for big data sets with real variability." (p. 30)

### Audience and craft

- "If the statistics are boring, then you've got the wrong numbers." (p. 76)
- "Graphics should be as intelligent and sophisticated as the accompanying text." (p. 131)
- "Graphical competence demands three quite different skills: the substantive, statistical, and artistic." (p. 83)
- "Allowing artist-illustrators to control the design and content of statistical graphics is almost like allowing typographers to control the content, style, and editing of prose." (p. 83)
- "A silly theory means a silly graphic." (p. 15)
- E. B. White (quoted): "No one can write decently who is distrustful of the reader's intelligence, or whose attitude is patronizing." (p. 77)

### Words and graphics together

- "Data graphics are paragraphs about data and should be treated as such." (p. 174)
- "Words and pictures belong together." (p. 173)
- "The contrast in line weight represents contrast in meaning." (p. 179)
- Valéry (quoted): "Seeing is forgetting the name of the thing one sees." (p. 147)
- "A sure sign of a puzzle is that the graphic must be interpreted through a verbal rather than a visual process." (p. 147)

### Reading complex graphics

- "How can the data-information be arranged so that the viewer is able to peel back layer after layer of data from a graphic?" (p. 148)
- "Each separate line of sight should remain unchanging (preferably horizontal or vertical) as the eye watches for data variation off the flat of the line of sight." (p. 149)
- "Graphical elegance is often found in simplicity of design and complexity of data." (p. 170)
- "Beautiful graphics do not traffic with the trivial." (p. 170)

### The Epilogue synthesis (p. 184)

- "Design is choice."
- "It is better to violate any principle than to place graceless or inelegant marks on paper."
- "What is to be sought in designs for the display of information is the clear portrayal of complexity … the revelation of the complex."
