# Cohort retention for a board deck — what to use instead of a heatmap

Short answer: **don't replace the heatmap, redesign it, and pair it with one summary line chart.** For a board deck, the combo that reads best is:

1. A **retention curves overlay** (small set of cohorts as lines, x = months since signup) as the hero chart.
2. A **polished triangular table-heatmap** (numbers in cells, color as a secondary encoding) below or on the facing page as the reference.

Skip the standalone heatmap-as-hero. Heatmaps are great for *you* to spot patterns in 24x12 grids; they're poor for a finance audience that wants to read a number and a trend in 5 seconds.

---

## Why the plain heatmap fails for this audience

- Triangular missingness looks like a bug to non-analysts ("why is the bottom half empty?").
- Color-to-percentage decoding is slow and imprecise — board members will ask "what's the number?" anyway.
- 24 rows is too many to label cleanly on a slide; you end up with unreadable y-axis ticks.
- Sequential color ramps compress the interesting range (most retention action happens between 60–95%, but the ramp spans 0–100%).

## Option A (recommended hero): Retention curves, overlaid, with cohort highlighting

- X-axis: months since signup (0–11).
- Y-axis: % retained.
- One line per cohort, but **don't draw all 24**. Draw them in three tiers:
  - **Gray, thin, low-opacity** for the 20 "context" cohorts.
  - **Two or three colored, thick lines** for the cohorts that tell the story (e.g., oldest complete cohort, the cohort right before a pricing/product change, most recent cohort with >=3 months of data).
  - Label those colored lines directly at the right end — no legend.
- Add a **median or "typical cohort" line** in a distinct neutral (dark gray, dashed) as the benchmark.
- Annotate one or two inflection points in plain English ("Self-serve onboarding launched — Aug 2024 cohort retains 8pts higher at M3").

This is the chart that answers the board's actual question: *"Is retention getting better or worse, and by how much?"* The triangular structure disappears entirely because every cohort starts at month 0.

Variations worth considering:
- **Cohort age vs. retention, colored by cohort vintage on a continuous scale** (light = old, dark = new, or vice versa). Reads as a "fan" — if newer cohorts sit above older ones, retention is improving; you see it instantly.
- **Small multiples** (4 rows x 6 cols of mini line charts, one per cohort). Good if the designer wants editorial polish and you have a full page. Each panel is tiny, no axes repeated, shared y-scale, with the cohort label and the M3/M6/M12 retention number printed in the corner.

## Option B (recommended companion): Polished triangular table

Treat it as a **table first, color second** — this is the Tufte-style move and what designers usually mean by "polished":

- Print the actual retention number in every cell (e.g., `87`, no `%` sign, right-aligned, tabular numerals).
- Use a **muted, narrow-range color scale** (e.g., a single hue from white to a brand color, mapped to the 60–100% range, not 0–100%). Don't use red-green diverging — finance will read that as good/bad and it'll dominate.
- Leave the lower triangle truly empty (no gray fill, no "N/A"). The diagonal edge is information.
- Bold the diagonal (month 0 = 100%) or omit it entirely — it's noise.
- Add a **right-margin column** with the cohort size (N) in light gray. Board will ask.
- Add a **bottom row** with the average retention at each month-since-signup across all cohorts that have reached it — this is the "average curve" reference.
- Group cohort rows in quarters with a hairline separator every 3 rows; label the quarter on the left ("Q1 2024", "Q2 2024"...). Makes 24 rows scannable.

## Option C (only if you have one big story): Layer cake / stacked area

If churn itself is the headline (not retention by cohort age), a **stacked area where each band is a signup cohort and height = active customers in that calendar month** is very effective. X-axis is calendar time, not cohort age. The triangular issue vanishes because you're looking at the present-day stack, and the board sees total active customers + the contribution of each vintage. Pair with a single churn-rate line on a secondary axis. This answers "are we growing net of churn?" rather than "how does each cohort behave?"

## Concrete recommendation for the deck

Slide structure:
- **Hero chart**: Option A (retention curves overlay), one sentence headline like *"Cohorts since the Q3 onboarding revamp retain ~8pts better at month 3."*
- **Reference table**: Option B (triangular table-heatmap), smaller, captioned *"Monthly retention by signup cohort. Cells are % of cohort still active."*
- **Footnote**: cohort definition, what counts as "retained" (logged in? paid? non-zero usage?), and N per cohort.

## Implementation notes

- **Tooling**: If you're in Python, Plotly or Altair both do the overlay cleanly; matplotlib is fine but you'll hand-roll the direct labels. For the table-heatmap, `great_tables` (Python) or `gt` (R) give you typography-grade output that actually looks like a board-deck table, not a Jupyter screenshot. Avoid seaborn's default heatmap — it screams "analyst notebook."
- **Color**: pick one brand hue, two tints. Don't use viridis on a board deck.
- **Typography**: tabular/lining figures, right-aligned numbers, no gridlines inside the table.
- **Export**: render to SVG or high-DPI PNG. Match the deck's font (almost always not the chart library default).
- **Smoothing**: don't. Cohort retention has real month-to-month structure (billing cycles, seasonality) — smoothing hides it and finance will notice.

## One thing to push back on with the designer

"Polished" sometimes means "fewer numbers." For a finance board deck, resist that. The hero chart should be visual; the companion table should have every number visible. Boards trust tables; they tolerate charts.
