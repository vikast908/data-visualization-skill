# Color Reference

Color is the single most common place data visualization goes wrong. It is also the place where Bertin and Tufte are weakest — both wrote before perceptually-uniform colormaps existed, before color-blind accessibility was rigorous, and before display gamuts varied as widely as they do today. This file is the modern supplement.

The core rule from Bertin still holds: **color (hue) is selective and associative, but not ordered and not quantitative.** The eye cannot rank hues by value or read ratios between them. Anything quantitative or ordered must go through *value* (light-to-dark), not hue.

## Table of contents

1. The four color jobs (and the colormap family that fits each)
2. Why "rainbow" / `jet` / `hsv` is wrong, every time
3. Perceptually-uniform sequential palettes (viridis & relatives)
4. Diverging palettes
5. Categorical palettes (qualitative)
6. Cyclic palettes
7. ColorBrewer at a glance
8. Color-blind safety
9. Dark mode
10. Brand color integration
11. Practical defaults for code
12. Tools

---

## 1. The four color jobs

Pick the family by data type first. Picking a specific palette comes second.

| Job | Data is… | Use | Examples |
| --- | --- | --- | --- |
| **Sequential** | Quantitative, single direction (small → large) | A perceptually-uniform light→dark ramp in one hue (or hue-shifted) | viridis, magma, cividis, Blues, YlOrRd |
| **Diverging** | Quantitative, anchored on a meaningful midpoint (above/below average, gain/loss, change vs target) | Two sequential ramps meeting at a neutral middle | RdBu, BrBG, PiYG, PuOr |
| **Categorical (qualitative)** | Nominal categories with no order | Distinct hues at roughly equal value/saturation | Okabe-Ito, Tableau 10, Set2 |
| **Cyclic** | Quantitative but wraps (hour of day, day of year, angle, phase) | A loop with no apparent start | twilight, hsv (cyclic case only), HCL-cyclic |

If you can't pick the family, the data type isn't clear yet — go back and decide.

## 2. Why rainbow / jet / hsv is wrong, every time

The classic "rainbow" colormap (matplotlib's `jet`, MATLAB's old default, `hsv`) violates every modern principle:

- **Not perceptually uniform.** Equal data steps produce unequal perceived color jumps. Bands of yellow look brighter than they should; bands of cyan look like a separate feature when they aren't.
- **Not monotonic in luminance.** Yellow is light, blue and red are dark — but green is medium, so the order isn't readable as a ramp.
- **Not color-blind safe.** Red/green confusion (the most common deficiency) destroys both ends of the scale at once.
- **Implies false structure.** Sharp hue transitions (green→yellow, blue→cyan) read as data boundaries that aren't in the data — the eye invents "edges" between bands.

Use rainbow only for the case where the data *is* an angle or phase that genuinely wraps — i.e., when you actually need a cyclic colormap, and even then prefer a perceptually-uniform cyclic one.

In matplotlib: `cmap='viridis'` is the safe default, never `cmap='jet'`. The 2.0 default change in matplotlib (2017) was specifically to fix this.

## 3. Perceptually-uniform sequential palettes

The "viridis family" is the modern default for quantitative data. Designed by Stéfan van der Walt and Nathaniel Smith specifically to be perceptually uniform, color-blind safe, and printable in grayscale.

| Palette | Hue range | Color-blind safe | Best for |
| --- | --- | --- | --- |
| **viridis** | Purple → green → yellow | Yes (deuter, prot) | The safe default; almost everything |
| **magma** | Black → purple → orange → yellow | Yes | When you want black at zero (denser dark range) |
| **inferno** | Black → red → yellow | Yes | Heat / temperature / emergency framing |
| **plasma** | Dark purple → magenta → yellow | Yes | Higher visual energy than viridis |
| **cividis** | Dark blue → light yellow | Yes (best for tritanopia) | When you want maximum color-blind fidelity |
| **turbo** | Improved rainbow (Google, 2019) | Better than jet but still hue-encoded | Only when you need to highlight extrema sharply |

**ColorBrewer sequential** palettes (Cynthia Brewer's classic set) are also excellent and well-tested:

- Single-hue: `Blues`, `Greens`, `Reds`, `Purples`, `Greys`
- Multi-hue: `YlOrRd`, `YlGnBu`, `BuPu`, `OrRd`

Single-hue ramps are quieter and integrate better with branded layouts. Multi-hue ramps span a wider luminance range and discriminate more steps but are harder to keep "on brand."

## 4. Diverging palettes

Use only when the midpoint is meaningful. "Above average" / "below average," "gain/loss," "change vs forecast," "yes/no/abstain."

| Palette | Endpoints | When to pick |
| --- | --- | --- |
| **RdBu** | Red → white → blue | The default; politically neutral if you map Red=negative |
| **BrBG** | Brown → white → blue-green | When red has unwanted political/financial connotations |
| **PiYG** | Pink → white → yellow-green | When values cross a biological threshold |
| **PuOr** | Purple → white → orange | When the data is non-political and you want distinctness |
| **RdYlBu** | Red → yellow → blue | Three-tone; only when the midpoint is genuinely "warning" |

**Critical rule for diverging:** anchor the midpoint at the *meaningful* zero, not the data's median. A "change vs forecast" map's midpoint must be 0% change, not the median change across regions — otherwise half the regions look "above average" by construction.

If your data has a true zero but is one-sided (e.g., revenue), it's sequential, not diverging. Diverging maps invented for one-sided data make readers hunt for a midpoint that doesn't exist.

## 5. Categorical (qualitative) palettes

For nominal categories: products, regions, species. The goal is distinct hues at roughly equal luminance and saturation so no category dominates visually.

| Palette | n | Notes |
| --- | --- | --- |
| **Okabe-Ito (2008)** | 8 | The gold standard. Designed for color-blind safety; every pair is distinguishable across all common deficiencies. Memorize these hex codes: `#000000 #E69F00 #56B4E9 #009E73 #F0E442 #0072B2 #D55E00 #CC79A7` |
| **Tableau 10 / Tableau 20** | 10 / 20 | Friendly defaults; widely used so audiences are habituated. |
| **ColorBrewer Set2 / Set3 / Dark2 / Pastel1** | 8–12 | Cynthia Brewer's qualitative sets; well-tested for print and screen. |
| **D3 schemeCategory10 / schemeTableau10** | 10 | Modern d3 defaults; reasonable. |

**Practical cap:** if you have more than ~7 categories, hue alone cannot distinguish them all. Either group lesser categories into "Other," use a small-multiple instead of overlaying, or pair hue with shape/orientation.

**Never use a sequential or diverging palette for nominal data** — it implies an order that isn't there. Picking the 5 colors of viridis for 5 products silently tells the reader that "Product C" is between "B" and "D" on some scale. It isn't.

## 6. Cyclic palettes

For data that wraps: hour of day (0 = 24), day of year, compass direction, phase angle.

The defining property: the start equals the end visually. matplotlib's `twilight`, `twilight_shifted`, and `hsv` (used cyclically) qualify. Avoid `hsv` if any perceptual uniformity matters — use a designed cyclic palette.

## 7. ColorBrewer at a glance

ColorBrewer ([colorbrewer2.org](https://colorbrewer2.org)) is the most-tested palette collection. Filter by:

- **Number of data classes** (3–12)
- **Nature** (sequential, diverging, qualitative)
- **Color-blind safe** (filter on)
- **Print friendly** (filter on)
- **Photocopy safe** (filter on)

If you're picking a palette and don't already have one in mind: open ColorBrewer, filter for the data type and "color-blind safe," and use whatever it suggests. You will not regret it.

## 8. Color-blind safety

About 8% of men and 0.5% of women have color-vision deficiency. The breakdown:

- **Deuteranomaly / deuteranopia** (red-green, green-weak): ~6% of men. The most common.
- **Protanomaly / protanopia** (red-green, red-weak): ~2% of men.
- **Tritanomaly / tritanopia** (blue-yellow): rare (<0.01%).
- **Achromatopsia** (full color blindness): very rare.

**Safe defaults:**

- For categorical: Okabe-Ito (designed for this).
- For sequential: viridis family (all designed for this), cividis specifically for tritanopia.
- For diverging: BrBG, PuOr (avoid RdGn which is the textbook failure case).

**Test your charts** with one of:

- Coblis (online): [color-blindness.com/coblis-color-blindness-simulator/](https://www.color-blindness.com/coblis-color-blindness-simulator/)
- Sim Daltonism (macOS)
- Stark (Figma plugin)
- matplotlib + `colorspacious`: `cspace_convert(colors, "sRGB1", "JCh")` for direct simulation

**Belt-and-suspenders rule:** never rely on color alone to distinguish important categories. Pair color with shape (in scatterplots), pattern (in fills if you must), position (in small multiples), or direct labels. If the chart still works in grayscale, color cannot have been the only encoding.

## 9. Dark mode

Dark mode is not just inverting colors. Pitfalls:

- **Pure white text on pure black** is too harsh; eyes fatigue from contrast. Use #FAFAFA or #E8E8E8 on #1A1A1A or similar.
- **Sequential palettes** designed light-to-dark stop working when the background is dark. You want dark-to-light, which usually means a different palette (e.g., reverse viridis: `viridis_r`).
- **Saturation** of accent colors needs to drop on dark backgrounds — saturated colors on dark bg look like they're vibrating (a real visual artifact).
- **Diverging palettes** that pivot on white look fine on light bg, terrible on dark — the white midpoint becomes the loudest point. Use a palette pivoting on a mid-gray that matches the bg luminance.

If your viz must work in both modes: design for both, test both, and accept that some palette choices won't carry over. CSS `prefers-color-scheme` and design-token-driven palettes are the only sane approach for production dashboards.

## 10. Brand color integration

When brand requires you to use specific colors (the company red, the product blue), and the data needs a sequential ramp:

- **Treat brand color as the dark end** of a sequential ramp ending at near-white (or near-bg).
- Generate the ramp in HCL/OKLCH space, not RGB, so intermediate steps have equal perceived contrast.
- Tools: [chroma.js scale generator](https://gka.github.io/palettes/), [hclwizard.org](https://hclwizard.org/color-picker/), [Leonardo](https://leonardocolor.io/) (Adobe).

If brand has multiple colors and you need a categorical palette: check whether the brand palette is color-blind safe (often it isn't). If it isn't, push back — getting two colors wrong in a logo is forgivable; getting two categories indistinguishable in every quarterly report is not.

## 11. Practical defaults for code

```python
# matplotlib: viridis is already the default for imshow/contourf since 2.0
import matplotlib.pyplot as plt
plt.imshow(data, cmap='viridis')           # sequential
plt.imshow(data, cmap='RdBu_r', vmin=-1, vmax=1, center=0)  # diverging, anchored

# Categorical: explicit Okabe-Ito
okabe_ito = ['#000000', '#E69F00', '#56B4E9', '#009E73',
             '#F0E442', '#0072B2', '#D55E00', '#CC79A7']
plt.rcParams['axes.prop_cycle'] = plt.cycler('color', okabe_ito)

# Quick palettable usage
from palettable.cartocolors.qualitative import Safe_10
colors = Safe_10.mpl_colors

# Color-blind-safe diverging
from palettable.colorbrewer.diverging import BrBG_11
cmap = BrBG_11.mpl_colormap
```

```r
# ggplot2 — scale_color_viridis_d / _c for the family
library(ggplot2)
ggplot(df, aes(x, y, color = z)) + geom_point() +
  scale_color_viridis_c()

# Okabe-Ito as a manual palette
okabe_ito <- c("#000000", "#E69F00", "#56B4E9", "#009E73",
               "#F0E442", "#0072B2", "#D55E00", "#CC79A7")
scale_color_manual(values = okabe_ito)
```

```javascript
// d3 — d3-scale-chromatic ships every ColorBrewer + viridis + Okabe-Ito
import * as d3 from 'd3';
const seq = d3.scaleSequential(d3.interpolateViridis).domain([0, 100]);
const div = d3.scaleDiverging(d3.interpolateRdBu).domain([-1, 0, 1]);
const cat = d3.scaleOrdinal(d3.schemeTableau10);
```

```css
/* Tailwind v4 / CSS modern: define design tokens in OKLCH */
:root {
  --color-data-1: oklch(0.6 0.15 250);   /* blue */
  --color-data-2: oklch(0.6 0.15 30);    /* warm red — equal perceptual contrast */
  --color-data-3: oklch(0.6 0.15 140);   /* green */
}
```

## 12. Tools

- **[ColorBrewer 2.0](https://colorbrewer2.org)** — pick a palette by data type; filter for color-blind / print safe.
- **[Viz Palette](https://projects.susielu.com/viz-palette)** — Susie Lu's tool: paste hex codes, see them simulated for each color-blindness type.
- **[Chroma.js scale](https://gka.github.io/palettes/)** — generate a palette in perceptual color space from any starting hex.
- **[OKLCH Picker](https://oklch.com/)** — design palettes in OKLCH (perceptually uniform).
- **[Leonardo](https://leonardocolor.io/)** — Adobe's contrast-driven palette generator.
- **[Color Review](https://color.review/)** — check pairwise contrast for all combinations.
- **[Coblis](https://www.color-blindness.com/coblis-color-blindness-simulator/)** — simulate any image under each deficiency.
- **[palettable (Python)](https://jiffyclub.github.io/palettable/)** — programmatic access to every common palette.
- **[paletteer (R)](https://github.com/EmilHvitfeldt/paletteer)** — same for R; 2500+ palettes.

## When the user disagrees

The most common pushback you'll get on color advice:

- *"But our brand uses red and green."* — Push back that red/green is the textbook color-blind failure case for ~6% of male readers. Suggest using brand colors for non-data accents (titles, dividers) and a color-blind-safe palette for the data itself.
- *"Rainbow looks more visual / dramatic."* — It looks more visual because the eye is inventing structure that isn't in the data. Show them their data in viridis and ask which one matches the actual distribution shape.
- *"I want to use red for bad and green for good."* — Fine as a categorical signal (annotations, status pills), bad for a continuous quantitative encoding. The "compromise" diverging palette is BrBG or RdBu, both color-blind-safe and culturally legible.
- *"The chart looks washed out."* — Increase saturation, not contrast range. A perceptually uniform ramp can still be saturated; e.g., `magma` is more saturated than `viridis`.

## Two principles to repeat

1. **Color encodes category, not quantity** — for quantity, use position or length; for category, use distinct hues. Bertin's table on this is non-negotiable.
2. **If it works in grayscale, color is doing its job; if it doesn't, color is the only encoding and you've made an accessibility hole.** Test grayscale early.
