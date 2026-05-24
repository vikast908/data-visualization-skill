# Storytelling Reference

Bertin and Tufte teach you to *show the data*. This file is about making it *land*. A correct, integrity-passing chart that nobody reads or acts on has failed its purpose.

Three governing ideas:

1. **A chart answers a question. State the question.** A title that says "Monthly Revenue" is a label; a title that says "Why did revenue drop in March?" is a story.
2. **The eye should land on what matters first.** Visual hierarchy directs attention. Without explicit hierarchy, the eye lands on whatever has the highest visual weight — often the wrong thing.
3. **Annotation is where prose meets data.** Tufte: "Data graphics are paragraphs about data." A graphic without words isn't half a graphic — it's a graphic with no narrator.

## Table of contents

1. Chart titles that work
2. The headline-and-deck pattern
3. Annotation patterns
4. Visual hierarchy on a single chart
5. Sequencing charts into a narrative
6. The opinion of the chart
7. Common storytelling failures

---

## 1. Chart titles that work

The title is the first thing the reader processes. It frames everything below it. Three title styles, ranked:

### Best: title is the takeaway

The title states the conclusion. The chart is the evidence.

- ✗ "Quarterly Revenue, 2020-2024"
- ✓ "Revenue tripled after the 2022 pricing change"

When the title states the takeaway, the reader knows what to look for and can verify the claim against the data. This is the standard for news graphics (NYT, FT, The Economist, Bloomberg). Use it for any chart where you have a point to make.

When *not* to use it: exploratory analysis (you don't know the takeaway yet); reference documentation (the reader is looking up a value); the chart is one of many small multiples (the takeaway is across the grid, not in each panel).

### Good: title is the question

The title states the question; the chart is the answer.

- "Did the pricing change accelerate growth?"
- "Where in the US is COVID still climbing?"
- "Which products are most likely to churn?"

Questions invite reading. The reader engages because they want to know. Use this when the takeaway is genuinely contested or nuanced.

### Acceptable: title is the subject

The title labels what the chart is. The reader has to figure out the takeaway.

- "Quarterly revenue, 2020-2024"

Acceptable in dashboards and reference documents (where the same chart is consulted repeatedly and a takeaway-style title would lie about a single reading). Bad as the default for narrative reports.

## 2. The headline-and-deck pattern

Newspaper-style two-line title. The first line is the headline (the takeaway); the second is the deck (a supporting clarification).

- **Headline:** "Revenue tripled after the 2022 pricing change"
- **Deck:** "Quarterly revenue, 2020-2024. Pricing rolled out March 2022."

The deck handles: units, time range, n, source, methodology. The headline carries the story. This pattern lets you write a punchy headline without sacrificing the precision the deck supplies.

Implementation:

```python
# matplotlib
fig.suptitle("Revenue tripled after the 2022 pricing change",
             fontsize=14, fontweight='bold', y=0.97)
ax.set_title("Quarterly revenue, 2020-2024. Pricing rolled out March 2022.",
             fontsize=10, color='#666', y=1.0)
```

```r
# ggplot2
ggplot(...) +
  labs(title = "Revenue tripled after the 2022 pricing change",
       subtitle = "Quarterly revenue, 2020-2024. Pricing rolled out March 2022.",
       caption = "Source: internal billing system. n = 12,400 customers.")
```

## 3. Annotation patterns

The cheapest, most underused storytelling move: write on the data.

### Inline event annotation

Mark important moments on the chart with a vertical reference line + a short label.

- "Pricing change rolled out" ← at March 2022
- "iOS v4.2 launched" ← at week 8
- "Lockdown begins" ← at March 2020

Without the annotation, the viewer sees a wiggle and wonders. With it, the wiggle has a cause and the chart has a story.

Implementation: a thin gray vertical line + 8-10pt text rotated 0° if there's room, 90° if there isn't. Don't use red — annotations shouldn't compete with the data.

### Highlight + callout

For "the one data point that matters," set every other point to gray (or 30% alpha) and highlight the focal point with full color + a label.

- A scatterplot of 200 customers, with "Acme Corp — $480K MRR, 0 logins/wk" labeled in orange while the other 199 are gray.
- A line chart of 50 stocks, with "NVDA" highlighted in red while the other 49 are gray.

Tufte calls this differential weighting. The eye lands where you tell it to.

### Range bands

Shade a horizontal/vertical region to mark a meaningful threshold zone.

- "Normal range" shaded behind a temperature time series.
- "Acceptable error" shaded around a control chart's centerline.
- "Healthy NPS (≥50)" shaded green-tinted on an NPS chart.

A range band is more honest than a single threshold line when the threshold itself has uncertainty.

### Arrow + text "what's the story"

When a pattern needs explicit naming, draw an arrow to it and write what it is.

- "← outliers driving the average"
- "← seasonal pattern repeats"
- "← gap = missing data, not zero"

Keep arrows short and text under 10 words. If you need more, write a paragraph below the chart, not on it.

## 4. Visual hierarchy on a single chart

When the reader's eye scans the chart, what should it see first? Second? Third? Design the hierarchy explicitly.

**Tier 1 — the focal data:** the one or two series, regions, or points that carry the story. Highest contrast, largest mark, often colored. Direct labels.

**Tier 2 — the context data:** the comparison set. Gray (#888 or so), unlabeled or sparsely labeled. Visible but quiet.

**Tier 3 — the chart frame:** axes, gridlines, ticks, legend. Lightest gray (#CCC or #DDD). Necessary but recessive. If the reader notices the gridlines before the data, the hierarchy is wrong.

A line chart with five lines, all the same color and weight, has no hierarchy. Make four of them gray and one of them dark, and the reader knows where to look.

## 5. Sequencing charts into a narrative

A report or article is rarely a single chart. The sequence matters as much as each chart's design.

### The classic sequence

1. **Establishing shot** — a single chart that sets the context. ("Here's revenue over five years.")
2. **Tightening focus** — a chart that narrows in on the question. ("Here's revenue by quarter, with March 2022 marked.")
3. **The evidence** — the chart that proves the point. ("Here's the rate-of-change before and after, with the change point explicit.")
4. **Optional: implications** — a chart showing what this means going forward. ("Here's the forecast under the new pricing.")

Each chart should answer one question. Don't try to make one chart answer all four.

### Small-multiple narrative

When the story is "the same pattern repeats across many subgroups," small multiples are themselves the narrative. The constancy of the design carries the message that the *data* differs, not the design.

### The before/after pair

When the story is a redesign, fix, or transformation, place the before chart on the left and the after on the right (or top/bottom). Keep them at identical scale. Annotate what changed.

This pattern is also how to argue against a bad chart someone else made: redraw it next to the original. Cabrera's magnetic monopole event (Tufte p. 36) is the canonical case.

### The expanding-comparison narrative

"Here's what one cohort looks like" → "Here's all cohorts at once" → "Here's the cross-cohort summary." Each chart adds context the previous chart couldn't show.

This is the structure of the iteration-1 evaluation in this repo's `benchmarks-iteration-1/`: per-eval responses are read first, then the cross-eval benchmark adds the pattern.

## 6. The opinion of the chart

Tufte's principles are mostly about restraint, but excellence in graphics is not the same as neutrality. **A good chart has an opinion.** The opinion shows up in:

- **What you chose to plot** (and what you didn't).
- **Where you chose to start the axes** (years, baselines, ranges).
- **What you annotated** vs. what you left as background.
- **Which series you emphasized** with color vs. left in gray.
- **The title** (especially if it states the takeaway).

Pretending a chart is neutral is itself an editorial choice — the choice to hide the editor. Better to be transparent about the angle and let the reader engage with it.

The line between "having an opinion" and "lying" is the Lie Factor: your visual emphasis can match your interpretive emphasis, but it cannot misrepresent the magnitudes.

## 7. Common storytelling failures

- **Generic titles** ("Sales Dashboard") — the title was the cheapest opportunity to communicate and you wasted it.
- **No annotations** — every spike, dip, and discontinuity is a question; without annotations the reader has to invent answers.
- **Equal visual weight for all series** — when everything is highlighted, nothing is.
- **A wall of identical KPI tiles** — the eye has nowhere to land first.
- **Charts captioned with the chart type** ("Bar chart of sales by region") — caption tells the reader what to *think*, not what they're looking at.
- **The "and here's another chart" sequence** without narrative through-line — each chart is fine in isolation; the report has no thesis.
- **Color that decorates rather than encodes** — bright colors that don't mean anything add noise without information.
- **Burying the lede** — the most important chart is on slide 23; the audience is gone by slide 8.
- **Fake precision** — labeling values to two decimals when the underlying data is ±10%.
- **No "compared to what?"** — every KPI tile that shows "$2.4M MRR" without a target, prior period, or benchmark.

## Implementation checklist

For any chart that will be read by a human you don't supervise:

- [ ] Title states either the takeaway or the question (not just the subject)
- [ ] Important events on the data are annotated inline
- [ ] Focal series/points have higher visual weight than context
- [ ] Chart frame (axes, grid) is the lightest layer
- [ ] The "compared to what?" is on the chart (target, baseline, prior period, normal range)
- [ ] Units, source, n, and date are in the caption or near the chart
- [ ] The chart's place in the sequence is clear (this is the establishing shot / the evidence / the implication)
- [ ] If the chart appears in a deck, the slide's headline matches the chart's takeaway

## A note on storytelling vs honesty

The goal is to *help* the reader see the pattern, not to *force* them to. The Lie Factor still applies. The deflation rule still applies. The context rule ("compared to what?") still applies — it's actually the most fundamental storytelling rule, because the entire reason annotation works is that it provides the comparison.

Storytelling without integrity is propaganda. Integrity without storytelling is boredom. Both are failures.

The Tufte epilogue line — "the revelation of the complex" — is the goal. Storytelling is how you make complexity revealed-rather-than-merely-displayed.
