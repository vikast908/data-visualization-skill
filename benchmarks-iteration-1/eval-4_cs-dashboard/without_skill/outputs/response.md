# CS At-Risk Accounts Dashboard — Layout Recommendation

With 500 accounts and 5 signals per account, the dashboard's job is **funneling CSMs from "who do I look at first?" down to "what do I do about this one?"** in under 30 seconds. Build it as a single page with three vertical zones.

---

## Zone 1 (top strip) — Health Triage Scorecards

A row of 4 single-value tiles. These exist so a CSM opening the dashboard at 9am knows immediately if today is a normal day or a fire drill.

| Tile | Metric | Conditional formatting |
|---|---|---|
| 1 | **# Accounts at Risk** (composite: see scoring below) | Red if > 10% of book |
| 2 | **MRR at Risk** ($ sum of at-risk accounts) | Red if > 15% of total MRR |
| 3 | **Renewals in next 60 days** (count + $ MRR) | Amber always — it's a watch metric |
| 4 | **Avg NPS (last 90d)** with sparkline of weekly trend | Red if < 30, amber 30-50 |

Keep these tiles 1 row tall. Don't add a 5th — five tiles in a row stops being scannable.

---

## Zone 2 (middle) — The Risk Matrix (the centerpiece)

This is what the team will actually use. **Scatter plot, full width**, takes ~40% of the page:

- **X-axis:** Days since last login (0 on the left, 90+ on the right)
- **Y-axis:** MRR per account (log scale — your top accounts will dominate otherwise)
- **Dot color:** NPS bucket (green ≥ 50, grey 30-49, red < 30, light grey = no NPS)
- **Dot size:** Open support tickets (capped at ~6 tiers so a 40-ticket account doesn't eat the chart)
- **Dot border:** Bold red ring if contract renews in < 90 days

The top-right quadrant — high MRR, hasn't logged in recently — is your "drop everything" zone. Add a faint quadrant shading or a dashed reference line at "30 days since login" and at the median MRR so the quadrant reads instantly.

Make every dot click-through to that account's record in your CRM (Looker's URL drill works fine for this).

---

## Zone 3 (bottom) — The Action Queue Table

A sorted, filterable table — this is what CSMs work *from* during the day. One row per account, sorted by a **composite risk score** (descending). Compute the score in LookML so it's reusable:

```
risk_score =
  (days_since_login / 7)              -- 1 pt per week dark
  + (90 - nps_score) / 10              -- detractors hurt more
  + (open_tickets * 2)                 -- tickets weighted heavily
  + (case when days_to_renewal < 90 then 20 else 0 end)
  + log(mrr)                           -- bigger accounts float up
```

Columns, left to right (this order matters — it mirrors how a CSM reads the row):

1. **Account name** (link to CRM)
2. **Owner / CSM** (so people can filter to themselves)
3. **MRR** — right-aligned, with a tiny inline bar (Looker's `html:` formatter) for at-a-glance scale
4. **Days since login** — heatmap cell: green 0-7, yellow 8-30, red 31+
5. **NPS** — colored pill (the actual number, not just a color)
6. **Open tickets** — number, red bg if ≥ 3
7. **Days to renewal** — red bg if < 60, amber if < 120
8. **Risk score** — the sort key, shown so people trust the order

Default filter: `risk_score > X` so the table opens showing only the ~30-50 accounts worth attention, not all 500. Add filters at the top for CSM owner, segment, and renewal window.

---

## What I'd deliberately NOT do

- **No pie charts** anywhere. NPS distribution as a pie is the classic mistake here — use a stacked bar or just the avg-NPS tile.
- **No gauges** for the headline metrics. They eat space and only show one number.
- **Don't show all 500 accounts in the table by default.** Filter to risk_score > threshold; let CSMs lift the filter if they want the full book.
- **Don't put the scatter and the table side-by-side.** Full-width each — the scatter needs horizontal room for the login axis to be readable, and the table needs room for 8 columns without truncation.

---

## Two small things that punch above their weight

1. **Sparkline column** in the table for "NPS trend last 4 quarters" — Looker supports this via a measure with `html:` rendering. A flat-then-dropping line tells a story a single NPS number can't.
2. **A "Newly at risk this week" callout** above the table — accounts whose risk_score crossed the threshold in the last 7 days. These are the highest-leverage interventions because the relationship hasn't fully decayed yet.

Build Zone 1 + Zone 2 first, ship it to two CSMs, watch them use it for a week, then build Zone 3 based on which columns they actually asked you to add. The scoring formula will need tuning against your real data — start with the weights above and adjust after you see which accounts it flags vs. which ones actually churned last quarter.
