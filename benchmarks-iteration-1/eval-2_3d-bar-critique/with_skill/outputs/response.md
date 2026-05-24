# Review: 3D bar chart of Weekly Active Users

Short answer: scrap the 3D bar chart. It's the textbook anti-pattern for this exact data — and the code on top of it has three more problems that would bite you even if 3D were OK.

## The structural problems (why no amount of polish saves this)

**1. 3D bars on a 2D screen lie about the numbers.**
You have a Q (quantitative) component — DAU — and you're encoding it as the height of a bar drawn in projected perspective. Bars in the back row are foreshortened, bars in the front row are not, and bars in the middle row are partially occluded by the front row. Two bars that read the same value can render as visibly different heights depending only on where they sit on the Y axis. Tufte's Lie Factor target is 0.95–1.05; perspective 3D blows past that as a property of the projection itself, not of your data. He's blunt: flatten. Bertin would say the same in different words — you've spent a planar dimension (depth) on something that adds no information, and you've made the one variable that *must* be quantitative (height) unreadable as a ratio.

**2. Three components on a single 3D image violates Bertin's image limit, badly.**
Bertin's rule: at most three components can live in a single "image" graspable in one glance, and the third must be an ordered retinal variable, not a third spatial axis. You have:
- Week (O, length 13) — needs an axis
- Product (≠, length 3) — needs categorical separation
- DAU (Q) — needs position or length

That's two axes and a length. The natural construction is 2D: weeks on X, DAU on Y, three lines (or three small-multiple panels) for product. You don't need a third spatial dimension; you have only three products.

**3. `cmap='rainbow'` on `bar3d` is wrong twice over.**
- `bar3d` does not actually accept a `cmap` kwarg the way you've written it — it'll either ignore it or color every bar the same. So whatever you *think* the colors mean, they don't mean it.
- Even if it worked, rainbow is the worst choice for a quantity: hue has no perceptual order (Bertin: color is selective but not ordered, and not quantitative at all). Viewers cannot rank rainbow colors by value without consulting the legend. For quantity, use position or length — which is what bar height already is — so the colormap is redundant *and* misleading.

**4. `dau` is never defined in the snippet.** Minor, but `dau.flatten()` will NameError. I'll assume it's a `(3, 13)` array.

**5. Title says "Weekly Active Users", Z-axis says "Daily Active Users".** Pick one. WAU and DAU are different KPIs and an exec dashboard is exactly where that gets caught and damages trust.

**6. ALL CAPS labels.** Tufte calls these the "computer duck" — they read slower and add nothing. Use mixed case.

## A few smaller code issues
- `figsize=(8, 6)` is roughly 1.33 — fine, but for a time-series 1.5+ wider than tall reads better.
- No units, no source, no `n`, no date range on the caption. An exec chart without "compared to what?" is incomplete.
- Y-axis tick labels for products aren't set — you'd get `0, 1, 2` instead of `Web, iOS, Android`.

## What to draw instead

For 3 products × 13 weeks × 1 quantity, the right construction is a **small multiple of range-framed line charts** (one panel per product, shared Y scale so they're comparable), or a **single line chart with three directly-labeled lines** if the values don't overlap badly. Either one is a true Bertin "image" — graspable in a glance — and has Lie Factor ~1.0.

```python
import matplotlib.pyplot as plt
import numpy as np

# dau shape: (3 products, 13 weeks)
weeks = np.arange(1, 14)
products = ['Web', 'iOS', 'Android']
colors = ['#444444', '#1f77b4', '#d62728']  # neutral + two distinct hues

fig, ax = plt.subplots(figsize=(9, 5))

for i, name in enumerate(products):
    ax.plot(weeks, dau[i], color=colors[i], linewidth=1.8)
    # Direct label at line end — no legend box
    ax.text(weeks[-1] + 0.15, dau[i, -1], name,
            color=colors[i], va='center', fontsize=10)

# Range frame: trim spines to data, drop top/right
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)
ax.spines['left'].set_bounds(dau.min(), dau.max())
ax.spines['bottom'].set_bounds(weeks[0], weeks[-1])

# Real data values as ticks, not round numbers
ax.set_yticks([dau.min(), np.median(dau), dau.max()])
ax.set_yticklabels([f'{int(v):,}' for v in
                    [dau.min(), np.median(dau), dau.max()]])
ax.set_xticks([weeks[0], weeks[-1]])

ax.set_ylabel('Weekly active users')
ax.set_title('Weekly active users by product, last 13 weeks')
fig.text(0.01, 0.01, 'Source: internal analytics. n = WAU per ISO week.',
         fontsize=8, color='gray')

plt.tight_layout()
plt.show()
```

If the three lines cross or overlap (common when one product dwarfs the others), switch to small multiples:

```python
fig, axes = plt.subplots(1, 3, figsize=(11, 3.5), sharey=True)
for ax, name, series in zip(axes, products, dau):
    ax.plot(weeks, series, color='#333', linewidth=1.6)
    ax.set_title(name, loc='left', fontsize=11)
    ax.spines['top'].set_visible(False); ax.spines['right'].set_visible(False)
    ax.set_xticks([weeks[0], weeks[-1]])
fig.suptitle('Weekly active users, last 13 weeks', x=0.02, ha='left')
fig.tight_layout()
```

## The dashboard-level point

An exec dashboard's job is usually **processing** in Bertin's sense — leadership scans for anomalies, trends, and inflection points. A 3D bar chart optimizes for none of that: it's a "message" form (memorable, decorative) used for a "processing" task. The line panel above lets the exec answer "which product is growing, which is flat, which dipped, and when?" in one glance — which is the whole point.

One more thing worth adding for execs: a one-line text annotation on the chart calling out the most important event ("iOS launched v4.2 in week 8") does more work than any color choice.
