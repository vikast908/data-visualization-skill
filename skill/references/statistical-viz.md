# Statistical Visualization Reference

Bertin and Tufte both predate most of modern statistical visualization. This file covers the patterns that come up in analytical and scientific work: uncertainty, comparisons, distributions, A/B tests, regression diagnostics, survival, time-to-event.

The principle that unifies them: **show the uncertainty.** A point estimate without an interval is a half-truth; a p-value without an effect size is a misleading truth; a regression line without residuals is a confident-looking lie.

## Table of contents

1. Uncertainty visualization
2. Error bars: when, when not, which kind
3. Confidence intervals
4. Distribution comparison
5. A/B test result visualization
6. Forest plots
7. Regression diagnostics
8. Kaplan-Meier survival curves
9. Calibration plots
10. ROC and precision-recall curves
11. Bayesian posterior visualization
12. P-value display (or rather, when not to)

---

## 1. Uncertainty visualization

The single most important rule in statistical graphics: **show the uncertainty around every estimate.** A bar chart with point estimates and no error bars is presenting a guess as a fact.

Three major approaches:

### Intervals (error bars, ribbons, bands)

The most common. A line for the estimate; a bracket or band around it for the uncertainty range. Works for time series (ribbons), regression lines (confidence bands), and point estimates (error bars).

### Distributions (densities, eye plots, gradient plots)

Shows the full posterior or sampling distribution, not just a summary. Better than intervals when the distribution is non-Gaussian (skewed, multimodal) and the reader is statistically literate.

### Frequency framings (quantile dotplots, HOPs)

Quantile dotplots from Kay et al. (2016): represent the posterior as 20 or 50 dots whose positions sample the distribution. "1 in 20 outcomes look like this."

HOPs (Hypothetical Outcome Plots) from Hullman et al. (2015): animate samples from the posterior. The reader sees the variability directly.

These work well for general audiences who struggle with the abstract idea of intervals.

## 2. Error bars: when, when not, which kind

**When to use:** any chart with sample-based point estimates (means, proportions, regression coefficients). Default to showing them.

**When not to:** when you're presenting raw observations (a histogram, a strip plot) — the data *is* its own uncertainty. When the audience will misread (executive dashboards sometimes get error bars stripped because they "confuse people"; the right answer is education, not removal).

**Which kind:**

| Kind | Shows | When to use |
| --- | --- | --- |
| **Standard deviation (SD)** | Spread of the underlying data | When you want the reader to see population variability, not estimation precision. Almost never the right choice on a summary chart. |
| **Standard error (SE)** | Precision of the estimate (typically SD/√n) | Common in scientific publication, but misleading on its own — readers often misread SE bars as CIs. **Always state which you used.** |
| **95% confidence interval (CI)** | Range of plausible parameter values | Almost always what you want. State it: "95% CI." |
| **Bootstrap percentile interval** | Same as CI but without parametric assumptions | When the sampling distribution is non-Gaussian. State the method. |
| **Credible interval (Bayesian)** | Range of plausible parameter values given the prior | When you've done Bayesian inference. State the credible level. |
| **Prediction interval** | Range plausible for a *future observation* | When the reader will ask "what's the next value likely to be?" — broader than a CI because it includes the residual variance. |

**Always label which interval you're showing**, in the figure caption or legend. "Error bars" with no specification is a footgun.

### Common error-bar mistakes

- **Asymmetric data shown with symmetric bars** — a right-skewed count distribution shown with ±SE bars implies values can go negative.
- **Overlapping CIs interpreted as "not significantly different"** — that's not how it works. Two 95% CIs can overlap and the difference can still be significant; conversely, non-overlapping CIs are sufficient but not necessary for p < 0.05.
- **Tukey-style "T" caps** that visually dominate — use thin lines, optionally with no caps at all (a clean line beats a T-shape on a busy chart).

## 3. Confidence intervals

The default uncertainty interval. Some patterns:

### CI on a single estimate

Display: a dot at the estimate, a horizontal line for the CI, both ends marked. If multiple estimates, sort by estimate and stack vertically (Cleveland dot plot with CIs).

### CI band on a regression line

Display: regression line + shaded ribbon for pointwise CI. Use lighter alpha for the ribbon than the line.

In ggplot2: `geom_smooth(method = "lm")` does this by default.

In matplotlib/seaborn: `sns.regplot(x, y, ci=95)`.

### CI on a curve over time

Display: line for the estimate, shaded ribbon for the CI. Two CIs (50% and 95%) overlaid in different alphas gives the reader both "most likely" and "plausible" at once.

### Distinguish CI from prediction interval

If the chart shows both, label them. A 95% prediction band will be much wider than a 95% confidence band, and the reader needs to know which they're looking at.

## 4. Distribution comparison

When the question is "how do these distributions compare across groups?" the right form depends on n and on what aspect of the distribution matters.

| Form | Best for | Worst for |
| --- | --- | --- |
| **Boxplot** | Quick summary across many groups; outliers visible | Bimodal distributions (the box hides this) |
| **Violin** | Distribution shape across groups | Small n (kernel density misleads); audiences unfamiliar with density |
| **Beeswarm / strip** | Small-to-medium n; individual points matter | Large n (overlap kills it) |
| **Raincloud** | Publication-quality combination of all of the above | General audiences |
| **Overlaid density** | 2-3 groups; shape comparison | More than 4 groups (becomes unreadable) |
| **Ridgeline** | 10-50 groups; shape comparison | Precise comparison; small n |
| **Histogram small multiples** | Any number of groups; shape comparison; you want bins visible | When you want a single overview |
| **ECDF (empirical CDF) overlay** | Statistical comparisons; tail behavior | General audiences |

**Underused choice: ECDF.** For 2-5 groups, overlay their empirical CDFs. Every shift, scale, and tail difference is visible. Used widely in statistics, rare in business.

## 5. A/B test result visualization

The wrong way: bar chart of variant means with no error bars. The right way: show the effect estimate with its uncertainty.

### Single-metric A/B test

- **Plot:** dot for the effect estimate (variant − control), horizontal CI line. Vertical reference line at zero.
- **Annotate:** the estimate value, the CI bounds, the % relative change, and the p-value (or Bayesian probability of improvement).
- **Don't:** plot the two variants' raw means with no uncertainty.

### Multi-variant test

Forest plot (see §6).

### Multi-metric test

Small-multiples of single-metric forest plots, one panel per metric. Sort metrics by importance (primary first, guardrails after).

### "Did the test work?" dashboard

The classic 4-panel:

1. Estimate ± CI for the primary metric
2. Time series of cumulative effect (with CI ribbon) — shows convergence
3. Distribution of the metric in each variant
4. Sample size accumulation over time

## 6. Forest plots

Pioneered in clinical meta-analysis; useful any time you're presenting many effect estimates side-by-side.

**Anatomy:** one row per study/variant/cohort. A dot for the effect estimate (sized by sample size or weight). A horizontal line for the CI. A vertical reference line at the null effect (usually 0 or 1).

**When to use:** A/B tests with many variants; meta-analyses; subgroup analyses; any "here are 20 estimates with their uncertainty" situation.

**Failure modes:** rows in arbitrary order (sort by estimate or effect size); dot size that obscures the line; no reference line at the null.

**Libraries:** `forestplot` (Python), `forestplot` / `metafor::forest` (R), or hand-roll with a horizontal dot+line plot.

## 7. Regression diagnostics

A regression isn't done until you've looked at its diagnostic plots. The standard four:

1. **Residuals vs. fitted** — should be patternless. Pattern means model misspecification.
2. **Q-Q plot of residuals** — should be approximately linear. Curvature means non-normal residuals.
3. **Scale-location** (√|residuals| vs. fitted) — should be flat. Slope means heteroscedasticity.
4. **Residuals vs. leverage** — identifies high-leverage points, including Cook's distance contours.

These don't go in the report. They go in your analysis notebook. The report shows the fitted model with a CI band and a brief mention that diagnostics were inspected.

In R: `plot(model)` returns all four. In Python: `statsmodels.graphics.regressionplots`.

## 8. Kaplan-Meier survival curves

For time-to-event data with censoring (customer churn, mechanical failure, clinical survival).

**Anatomy:** step function showing survival probability over time. Tick marks at censoring times. Shaded CI ribbon. "At-risk" table beneath the x-axis showing how many subjects remain at risk at each time point.

**When to use:** any survival analysis. Tells the reader what proportion are still "alive" (un-churned, working, etc.) at any given time point.

**Common failure:** no censoring marks, no at-risk table, two groups overlaid with no log-rank test result. These three additions are mandatory in clinical work and a good idea everywhere.

**Libraries:** `lifelines.KaplanMeierFitter` (Python), `survival::survfit` + `survminer::ggsurvplot` (R, the best survival plotting).

## 9. Calibration plots

For probabilistic classifiers. Does the model predict 70% when it should?

**Anatomy:** x-axis = predicted probability (binned); y-axis = observed frequency in that bin. Diagonal y=x reference line. Histogram of predictions along the bottom.

A well-calibrated model lies on the diagonal. A model that's "too confident" (predictions extreme) falls below at high probs and above at low probs. A model that's "too cautious" does the opposite.

**Libraries:** `sklearn.calibration.CalibrationDisplay`, `caret::calibration`.

## 10. ROC and precision-recall curves

For binary classifier evaluation. ROC = true-positive rate vs false-positive rate at different thresholds. PR = precision vs recall.

**When to use which:**

- **ROC** when the class balance is roughly even or when both classes matter equally.
- **PR** when the positive class is rare (class imbalance ≥ ~95/5). ROC looks great on imbalanced problems because the false-positive rate stays low even when the classifier is bad at precision.

**Always plot both** unless you know the application demands one or the other.

**Failure modes:** comparing ROC AUCs across datasets with different class balance (apples-to-oranges); reading "good AUC" as "useful classifier" without checking calibration; reporting a single threshold's performance without showing the ROC/PR curve.

## 11. Bayesian posterior visualization

For Bayesian inference. The estimate is a distribution, not a point.

**Patterns:**

- **Density** of the posterior with credible-interval shading.
- **Eye plot** — a violin on its side with interval marks.
- **Quantile dotplot** — 20 or 50 dots positioned at quantiles of the posterior. Communicates uncertainty as frequency ("1 in 20 plausible outcomes look like this").
- **Posterior predictive check** — overlay simulated draws against observed data to see if the model reproduces the data shape.

**Libraries:** `arviz` (Python; the standard for PyMC and Stan), `bayesplot` (R; built for Stan output), `tidybayes` (R; for grammar-of-graphics workflows).

## 12. P-value display (or rather, when not to)

P-values, displayed as starred asterisks on bar charts, are one of the worst visualization conventions still in widespread use. They:

- Conflate effect size with statistical significance.
- Encourage threshold thinking (p < 0.05 = real, p > 0.05 = not real).
- Hide effect-size and CI information that would be more informative.
- Visually clutter the chart with marks that aren't data.

**Better practice:**

1. **Show the effect estimate with its CI.** This includes the p-value information implicitly (CI excludes the null → p < the corresponding threshold) plus the magnitude.
2. **If you must show p-values, put them in the caption or a table**, not as asterisks on the chart.
3. **For multiple comparisons, show the effect-size forest plot.** Don't sprinkle "***" across a bar chart.

The American Statistical Association's 2016 statement on p-values explicitly recommends moving away from threshold-based interpretation. Match the visualization.

## Implementation defaults

```python
# Confidence interval around a mean (matplotlib + seaborn)
import seaborn as sns
sns.pointplot(data=df, x='group', y='value', errorbar=('ci', 95),
              capsize=0, linestyle='none', marker='o')

# Effect estimate with CI (forest plot style, hand-rolled)
import matplotlib.pyplot as plt
estimates = [(0.12, 0.04, 0.20, 'Variant A vs Control'),
             (-0.03, -0.08, 0.02, 'Variant B vs Control')]
fig, ax = plt.subplots(figsize=(8, 3))
for i, (est, lo, hi, label) in enumerate(estimates):
    ax.errorbar(est, i, xerr=[[est-lo], [hi-est]], fmt='o', color='#333',
                capsize=3, markersize=6)
    ax.text(hi + 0.005, i, label, va='center', fontsize=10)
ax.axvline(0, color='#888', linestyle='--', linewidth=1)
ax.set_yticks([])
ax.set_xlabel('Effect size (95% CI)')
ax.spines[['top', 'right', 'left']].set_visible(False)
```

```r
# ggplot2: A/B test result
ggplot(results, aes(x = effect, y = variant)) +
  geom_vline(xintercept = 0, linetype = 'dashed', color = '#888') +
  geom_errorbarh(aes(xmin = ci_lo, xmax = ci_hi), height = 0.2) +
  geom_point(size = 3) +
  labs(x = 'Effect on conversion rate (95% CI)', y = NULL,
       title = 'A/B test results, week 4',
       subtitle = 'Positive = improvement over control')
```

## When the user disagrees

- *"Error bars confuse our exec audience."* — Then teach them once. Bare point estimates are misleading; the cost of one confused exec moment is less than the cost of a confident decision made on noise.
- *"Just give me the p-value, I'll figure out the rest."* — Show the effect estimate with CI; the p-value is derivable but the magnitude isn't.
- *"This chart is for a journal; reviewers want the stars."* — Some journals do; some don't. Default to CI + effect size; add stars only if the journal style sheet requires.
- *"The CI is huge; it looks bad."* — A huge CI is the truth. Hiding it doesn't shrink it; it just lets the reader form a false confidence. If the CI is too wide to be useful, the message is "we need more data," not "let's not show the CI."

## Two principles

1. **A point estimate alone is a half-truth.** Always pair with an uncertainty range.
2. **Effect size beats significance.** A statistically significant effect of 0.1% may be useless; a non-significant effect of 30% with a wide CI may be worth investigating. Show the effect; show the uncertainty.
