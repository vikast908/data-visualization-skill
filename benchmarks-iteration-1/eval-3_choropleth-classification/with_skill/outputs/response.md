# Jenks vs. Quantiles for "America's Hardest-Hit Counties"

Short answer: **your editor is right for the wrong reason, and you're wrong for the right reason.** For this specific article, use **neither as your default** — use a **perceptually-uniform sequential scale with a small number of fixed, meaningful breakpoints** (e.g., 0, 5k, 10k, 20k, 30k, 40k+ per 100k). If you must pick one of the two, **quantiles** — but understand what you're giving up.

Here's the reasoning, in the order it should actually drive your decision.

## 1. The framing problem comes first, before classification

Your headline is "America's hardest-hit counties." That is a **message graphic** (Bertin's term) — memorizable, one-glance, communicating a known finding. It is not a discovery/processing graphic. That matters because:

- **Jenks** optimizes within-class homogeneity. It will find the "natural" cliffs in *your* dataset. Change the dataset (add 2024, drop a state, switch from cumulative to annual) and the breaks move. Readers comparing your map to anyone else's map of the same phenomenon will see different geographies. That is the opposite of honest for a published article.
- **Quantiles** guarantee equal-area color usage — every bin has the same number of counties. The map will *always* look dramatic, even if COVID had been uniformly mild, because the top quintile is, by construction, the top 20%. That's the editor's "honest" intuition backwards: equal *visual* weight per bin masks the *actual* magnitude differences between bins.

Neither method ties the color to a real-world threshold. Both are data-dependent.

## 2. Bertin's deeper objection — and why it matters here

Choropleths take a quantitative variable (cases per 100k) and convert it into an ordered one (5 or 7 bins). Bertin calls this "transforming Q into O" and warns it's fine for *communication* of a known message, dishonest for *discovery*. Since you're communicating a known message, you've earned the right to bin — but you should bin on **meaningful thresholds**, not on the shape of your sample.

Concretely, for COVID cases per 100k 2020-2023, cumulative county rates run roughly 0 to 50,000+. Pick breakpoints a reader can reason about: 0–5k, 5–10k, 10–20k, 20–30k, 30k+. These are round, memorable, and a reader looking at any other COVID map can compare. Jenks breaks of "0–4,237 / 4,238–8,914 / ..." cannot be compared to anything.

## 3. If you're forced to choose Jenks vs. quantiles

| | Jenks | Quantiles |
|---|---|---|
| Reveals real clusters in *this* dataset | Yes | No |
| Comparable across datasets/time | No | No |
| Equal visual weight per bin | No | Yes |
| Tends to flatter the "shocking" headline | Yes (cliffs at outliers) | Variable |
| Defensible as "honest" to a skeptical editor | Harder | Easier |

Jenks will produce a more visually striking map because COVID rates have a long right tail — Jenks will pull the top class to capture the outlier counties, and they'll glow. That's what your intuition is picking up: it *looks* like the story. But it's the data-driven equivalent of choosing the aspect ratio that makes the line steepest. Tufte's Lie Factor isn't formally violated, but the spirit is: design variation (the algorithm chose the breaks) is doing work the data should do.

Quantiles avoid that specific manipulation, which is what your editor means by "honest." But quantile maps universally over-emphasize differences between near-identical counties at percentile boundaries, and under-emphasize real gaps inside a bin.

## 4. What to actually ship

1. **Sequential gray or single-hue scale** (viridis, or a single-hue blue/orange ramp). Never a diverging scale unless you have a meaningful midpoint, which you don't.
2. **5 bins on round, externally-defensible thresholds.** Document the choice in the caption: "Bins: 0, 5k, 10k, 20k, 30k cases per 100k. Source: ___."
3. **Show the distribution next to the map.** A thin histogram strip under the legend, with the breakpoints marked, lets a reader see what each color actually contains. This is the single most honest thing you can do, and it neutralizes the entire Jenks-vs-quantiles argument — the reader sees the raw shape.
4. **Population-normalize** (you already did — per 100k) and **state the time window in the title**, not the caption. "Cumulative COVID-19 cases per 100,000 residents, U.S. counties, 2020–2023."
5. **Counties with < ~1,000 residents:** consider suppressing or visually muting. Rate denominators that small produce noise that dominates color, and the noise lives in places like rural Nebraska that will hijack "hardest-hit" framing.
6. **Inset or table for the top 10 named counties.** A map shows the geography; a small table next to it shows the actual numbers — the table does the work the choropleth can't (exact ratios). Tufte: ≤20 numbers, prefer a table.

## 5. Tell your editor

"You're right that quantiles are more defensible against the charge of picking flattering breaks. But both methods hide the real magnitudes behind algorithm-chosen bins. I'm using fixed round-number thresholds so the reader can compare our map to any other COVID map, plus a small histogram under the legend so they can see the underlying distribution. That's more honest than either of us was proposing."

That's the actionable answer. Bertin gets you to the framing (Q→O is a communication choice, not a discovery tool); Tufte gets you to "show the data" (the histogram strip) and "compared to what?" (round, externally meaningful thresholds).
