# Use quantiles. Your editor is right.

For a journalism piece titled "America's hardest-hit counties," **quantiles** are the better choice. Here's why, and what to do about the tradeoffs.

## The short answer

- **Jenks** optimizes for *within-class similarity* in your data. It's a clustering algorithm. Great for analytical/exploratory work where you want to reveal the natural structure of the distribution.
- **Quantiles** guarantee an equal number of counties in each color bin. That's what makes them feel "honest" — every shade on the legend represents the same number of places, so the reader can directly compare "top 20% vs bottom 20%."

For a national choropleth aimed at a general audience, quantiles win on three grounds that matter for your story:

1. **Map readability.** US county COVID rates are heavily right-skewed (a few rural counties had catastrophic per-capita rates because of small denominators and outbreaks in prisons/meatpacking plants/nursing homes). Jenks will spend most of its breakpoints isolating those outliers, leaving 90%+ of the country in one or two indistinguishable colors. Your map will look mostly blank. Quantiles force visual contrast across the whole country, which is what a "hardest-hit counties" map needs.

2. **Comparability and legend clarity.** A quintile legend ("worst 20%, next 20%, ...") is something a reader understands in one second without studying the legend. Jenks breaks land on arbitrary numbers like 4,712 / 8,931 / 17,204 that mean nothing to a reader and can't be compared across maps (e.g., if you do a follow-up map for 2024, Jenks breaks will shift and the maps won't be visually comparable).

3. **Resistance to gaming accusations.** Jenks is a *data-driven* algorithm, which sounds neutral but actually means the breakpoints move based on what makes the map look most differentiated. A skeptical reader (or a county official who's mad about their shading) can't easily audit it. Quantiles are transparent: "this county is in the worst quintile, period."

## Your intuition isn't wrong — it's just for a different job

Jenks is the right call when:
- You're doing analysis, not communication.
- The distribution is roughly unimodal/bell-shaped (COVID rates aren't).
- You're highlighting *clusters* in the data itself ("there are three natural tiers of impact"), not ranking places.
- You're making one definitive map, not a series.

None of those apply here.

## What I'd actually ship

1. **Use quintiles (5 classes).** Quartiles are too coarse for a county map (3,143 counties); deciles are too noisy. Five is the sweet spot and matches what NYT, WaPo, and CDC have used for COVID maps.

2. **Use a sequential colormap**, not diverging. Something like `YlOrRd` or viridis-reversed. Diverging palettes imply a meaningful midpoint, and "median COVID rate" isn't a meaningful midpoint here.

3. **Handle the outliers explicitly.** Because quantiles can hide a county that's 5x worse than the next, add an *annotation layer* on top: label the 10–15 worst counties by name with their actual rate. This gets you the best of both worlds — honest distributional shading + the dramatic specifics that make the story.

4. **Show the actual numbers somewhere.** Either in the legend (write the rate range for each quintile in small type — e.g., "Worst 20%: 14,200–32,400 per 100k") or in a small companion histogram. This neutralizes the main critique of quantiles (that they hide the magnitude of differences between bins).

5. **Consider population-weighted quantiles.** Standard quantiles treat Loving County, TX (population ~60) the same as LA County (population ~10M). For a "hardest-hit" story about *people*, weighting the quantile breaks by population so that each bin contains 20% of Americans (not 20% of counties) is more defensible and avoids the rural-skew problem. This is the move I'd push for if you have time for one refinement.

6. **One more thing — think about denominators.** Cumulative case rate per 100k over a 4-year window mixes very different epidemic phases (pre-vaccine Alpha, Delta, Omicron, post-Omicron). Reported cases also got much less reliable after home testing took over in 2022. If you can, either show the four years as small multiples, or restrict to 2020–2021 when reporting was most consistent. This will affect the story more than the classification choice will.

## Bottom line

Ship quantiles (ideally population-weighted), with a sequential palette, and annotate the true outliers by name. Tell your editor you agree, and that you're going one step further by population-weighting the breaks so the map represents Americans, not acreage.
