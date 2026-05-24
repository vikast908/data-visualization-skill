Drop the heatmap. It's the wrong *function* for a board deck.

Heatmaps are a recording graphic — they let an analyst scan 288 cells looking for anomalies. A board deck needs a communicating graphic: one image, one glance, one takeaway. Also, your retention rate is quantitative, but heatmap shading (Bertin's "value") only encodes order — the CFO can see "darker = worse" but not "Q3 2024 is 1.4x better than Q3 2023." The triangle of empty cells is the giveaway that the format is fighting the data.

**Ship this instead: small-multiple retention curves, grouped by quarter.**

- 8 panels (one per signup quarter, 2024 Q1 → 2025 Q4), arranged 2×4 or 4×2.
- X = months since signup (0–12), Y = % retained.
- Each panel shows the 3 monthly cohorts in that quarter as thin gray lines, with the quarter's average overlaid darker.
- Identical axes across all panels — that's what makes the eye see the trend.
- The triangular-data problem vanishes: recent cohorts just have shorter curves. No empty cells, no awkward void.

Position encodes the quantitative retention rate honestly (Bertin: Q needs position, size, or value; position is the only one that supports ratio reading). Small multiples carry the time dimension without overloading any single panel. Your designer gets something that looks composed rather than computed.

**Then add a 5-column table beneath the panels:**

| Cohort quarter | n signups | M1 | M3 | M6 | M12 |

Eight rows, ~40 numbers — under Tufte's "table beats chart for ≤20 numbers" threshold per row group, and it gives the board the exact figures they'll want to quote. The chart shows the shape; the table delivers the numbers. This is the semi-graphic pattern and it's almost always what finance teams actually want.

**If you only have space for one image**, do this single panel instead: all 24 monthly cohort curves overlaid, the oldest 20 in light gray (#CCC), the most recent 4 in a saturated color, labeled directly at the right end of each line. No legend. Range-framed axes. One sentence as title: "Month-6 retention has improved from 62% to 74% over the last year" — or whatever the actual finding is. That's Tufte's "data graphics are paragraphs about data."

**A few things to avoid as you build it:**

- No rainbow colormap on the heatmap if your designer insists on keeping it — viridis or a single-hue sequential at minimum, but you'll still have the function-mismatch problem.
- Don't stack-area the cohorts. Only the bottom series has a real baseline.
- No 3-D anything. No gradient fills behind the curves.
- Y-axis: 0–100%, not auto-scaled to 60–80%, or the improvement will look like a Lie-Factor cliff.
- Annotate one or two real events on the curves (pricing change, churn-save program launch) — that's what makes finance trust the chart.

The heatmap isn't wrong, it's just answering the wrong question. Heatmaps answer "where are the anomalies?" Boards ask "is it getting better?" Retention curves answer that in one glance.
