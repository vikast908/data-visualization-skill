# At-Risk Accounts Dashboard — Layout

## The core insight

CS isn't asking "what's our average NPS?" — they're asking "**which 15 of these 500 accounts should I call this week?**" That's a Bertin **processing** task (find anomalies in a population), not a **message** task (communicate a known KPI). Almost every Looker dashboard built for this gets it wrong by leading with tiles ("Avg NPS: 42", "Total MRR: $2.4M") that tell you nothing about which account to open.

So the dashboard is **one big sorted table with sparklines**, supported by two diagnostic charts. Not six KPI tiles across the top.

---

## Layout (top to bottom)

### Row 1 — Risk-ranked account table (the whole point of the dashboard)

This is ~80% of the screen. One row per account, sortable, but **defaulted to descending by a composite Risk Score**. ~25–30 rows visible without scrolling; the remaining 470 are one scroll away.

Columns, left to right, in the order a CSM's eye would walk them:

| Column | Encoding | Why |
|---|---|---|
| Account name | text, left-aligned | the identifier |
| **Risk score** | number + a 6-cell heat strip (light gray → dark gray, never red/green as the *only* channel) | the sort key; value (light-to-dark) is Bertin's ordered variable |
| MRR | right-aligned number, $ formatted | quantity → position on a number line (the column itself) |
| MRR rank percentile | tiny inline bar, 0–100% | gives the MRR number context — "compared to what?" (Tufte) |
| Days since last login | right-aligned number, colored only if > 30 | the eye should jump to the outliers, not scan every cell |
| NPS (90d) | number + a 1-row sparkline of the last ~6 surveys | the sparkline shows whether a 40 is rising from 20 or falling from 70 — totally different stories |
| Open tickets | number; small dot-plot beside it of last 8 weeks | trend matters more than the snapshot |
| Days to renewal | number, with a thin horizontal bar showing position in the year | "90 days out + score of 78" is the panic cell |

A few craft notes:

- **No vertical gridlines.** Use whitespace + a hairline rule every 5 rows (Tufte: "cut thin white gridlines through the bars").
- **Right-align all numerics** so decimal points stack and the eye can compare down a column.
- **Use actual data values for the tick endpoints** on sparklines (min / max / current), not round numbers.
- **One neutral color** (dark gray) for all data. Reserve a single accent (a warm tone, *not* red — accessibility) for the Risk-score heat strip and for the "stale login" highlight.
- **Direct labels on sparklines** — no legend.

This single table is what Tufte calls a **semi-graphic**: text-tables enriched with word-sized graphics. For 500 rows × 8 columns, nothing else comes close on data density.

### Row 2 — Two diagnostic panels (side by side, equal width)

Now that the user has a ranked list, two charts help them *understand the population* and spot patterns the sort might hide.

**Panel A — Renewal-risk scatter** (the most important chart on the dashboard)

- X: days to renewal (0 → 365)
- Y: risk score
- Mark size: MRR (so the eye is drawn to large at-risk accounts near renewal)
- Range-framed, no grid, no chart border.
- Annotate the **upper-left quadrant** ("renewing soon AND high risk") with a label — that's the panic zone.
- Each dot is clickable to filter the table above.

Tufte: "the scatterplot is the greatest of all graphical designs." Three components in one image (Bertin's max for a single-glance read): two planar dimensions + one ordered retinal variable (size). This is the sweet spot.

**Panel B — Small multiples of NPS trend, faceted by MRR tier**

- 4 small panels: Enterprise / Mid-market / SMB / Self-serve
- Each shows the *median* NPS line + a thin band for 25–75th percentile, last 12 months
- Constant Y axis across all four (-100 to +100), constant X axis (12 months)
- Range frame, no grid.

Design constancy across the four panels is the entire point — the eye reads "Mid-market is collapsing while Enterprise is stable" in one second. Stacking these into a single multi-color line chart would destroy that.

---

## What I'm explicitly *not* recommending

- **No KPI tiles at the top.** "Avg MRR: $4,800" answers nothing CS can act on. A KPI tile is a *message* construction misused for a *processing* task.
- **No gauges, donuts, or pie charts** for NPS distribution. A sorted bar or a histogram would be better, but neither earns its space here — the table already shows every NPS score in context.
- **No heatmap of accounts × metrics.** Tempting, but you have one quantitative metric per cell and 500 rows; the table-with-sparklines is denser and more honest.
- **No color-coded "health = red/yellow/green" pill.** Color hue has no innate order (Bertin) and ~8% of your users can't reliably distinguish red from green. Use a single light-to-dark gray ramp for the risk score and let position in the sorted table do the heavy lifting.
- **No "MRR over time" line chart at the top.** It's a vanity metric for CS — they cannot act on aggregate MRR; they act on individual accounts.

---

## The Risk Score itself

Since you didn't define one, here's a defensible starting formula (z-score each component, then weight). You'll iterate, but expose it transparently in a tooltip:

```
risk = 0.30 * z(days_since_last_login)
     + 0.25 * z(-nps_90d)
     + 0.20 * z(open_tickets)
     + 0.15 * z(-days_to_renewal)     // closer renewal = higher risk
     + 0.10 * z(mrr)                  // bigger accounts get a small boost
                                       // because they're more painful to lose
```

Show the score *and* let CSMs sort by any individual column. The score is a starting filter, not a verdict.

---

## The closing test

Bertin: in one glance, can a CSM answer "which accounts are at risk, and what kind of risk?" — yes, the sort + the sparklines reveal both magnitude and trajectory.

Tufte: if I removed everything that isn't data, what would I lose? The hairline row dividers, the renewal-bar background, the heat strip — all earn their ink because they encode something. Strip the rest.
