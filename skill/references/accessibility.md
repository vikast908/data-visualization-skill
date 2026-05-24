# Accessibility Reference

Bertin and Tufte wrote before accessibility was rigorously defined. This file covers what they left out.

The principle that ties it together: **never let color (or any single channel) be the only encoding of meaning.** If your chart works in grayscale, color is doing its job. If it doesn't, color is the only encoding and you've made the chart inaccessible to 5–10% of viewers, every printer, every dark-mode user, and every screen reader.

## Table of contents

1. Color-vision deficiency — what to design for
2. Color-blind safe palettes (with hex codes)
3. The "would it work in grayscale?" test
4. WCAG contrast ratios
5. Text labels on the data (Tufte's "friendly graphic")
6. Alt text for data visualizations
7. Screen-reader-friendly tables
8. Motion and animation
9. Dark mode adaptation
10. Font size and reading distance
11. Production checklist

---

## 1. Color-vision deficiency — what to design for

Frequencies in the general population (assuming a roughly 50/50 sex split):

| Type | Affects | Prevalence |
| --- | --- | --- |
| **Deuteranomaly** (red-green, green-weak) | both | ~6% of men, ~0.4% of women |
| **Deuteranopia** (red-green, no green) | both | ~1% of men |
| **Protanomaly / protanopia** (red-green, red-weak/missing) | both | ~2% of men |
| **Tritanomaly / tritanopia** (blue-yellow) | both | <0.01% |
| **Monochromacy** (no color discrimination) | both | very rare |

About **1 in 12 men** has some form of color-vision deficiency. If your dashboard, report, or article reaches more than 12 men, you have a CVD user. Design accordingly.

**Practical implication:** the red-green axis is the failure case for ~8% of male readers. Never use red and green as the *only* distinguishing channel for two categories that matter ("good" vs "bad," "above target" vs "below target," "team A" vs "team B"). Pair them with shape, position, or labels.

## 2. Color-blind safe palettes (with hex codes)

Memorize these.

### Categorical — Okabe-Ito (2008)

Designed for color-blind safety; every pair is distinguishable across all common deficiencies.

```
#000000  black
#E69F00  orange
#56B4E9  sky blue
#009E73  bluish green
#F0E442  yellow
#0072B2  blue
#D55E00  vermillion
#CC79A7  reddish purple
```

### Categorical — Tol Bright (Paul Tol, 2018)

Slightly more saturated than Okabe-Ito. CVD-safe.

```
#4477AA  blue
#EE6677  red
#228833  green
#CCBB44  yellow
#66CCEE  cyan
#AA3377  purple
#BBBBBB  gray
```

### Sequential — the viridis family

`viridis`, `magma`, `inferno`, `plasma`, `cividis`. All CVD-safe and perceptually uniform. **`cividis`** is specifically optimized for tritanopia.

### Sequential — ColorBrewer

Filter at colorbrewer2.org with the "color-blind safe" checkbox. Reliable choices: `Blues`, `Greens`, `YlOrRd`, `YlGnBu`, `BuPu`.

### Diverging — CVD-safe

- `BrBG` — brown ↔ blue-green (best general choice)
- `PuOr` — purple ↔ orange
- `RdBu` — red ↔ blue (technically CVD-safe; red and blue are still distinguishable for deuter/prot)

Avoid: `RdYlGn` (the textbook failure), `Spectral` (rainbow-derived), any palette that puts red and green at the endpoints with green at the "good" end.

## 3. The "would it work in grayscale?" test

Before shipping, view your chart in grayscale. If categories blur together, color was doing the whole encoding and you need a second channel.

**How to test:**

- **matplotlib:** save as PNG, open in image editor, desaturate. Or use `from colorspacious import cspace_convert; cspace_convert(rgb, "sRGB1", "JCh")` and read off the J (luminance) values.
- **Browser/screen:** macOS → Accessibility → Display → Color Filters → Grayscale. Windows → Magnifier or third-party "grayscale" filter.
- **Figma:** "Grayscale" plugin.
- **Coblis:** [color-blindness.com/coblis-color-blindness-simulator/](https://www.color-blindness.com/coblis-color-blindness-simulator/) — upload an image, see it under each deficiency type.

**If categories are indistinguishable in grayscale**, add one of:

- **Shape** — different marker shapes in scatterplots (circle/square/triangle/diamond, NOT pentagon/hexagon which look the same at small sizes)
- **Pattern / texture** — solid vs hatched fills (use sparingly; Tufte warns of moiré)
- **Direct labels on the data** — Tufte's "friendly graphic" approach
- **Small multiples** — separate each category into its own panel
- **Line style** — solid/dashed/dotted for line charts (use sparingly; dashed lines read as "less important" or "projected")

## 4. WCAG contrast ratios

Web Content Accessibility Guidelines for text and meaningful graphics:

| Use | Minimum (AA) | Enhanced (AAA) |
| --- | --- | --- |
| Normal text (< 18pt or < 14pt bold) on background | 4.5:1 | 7:1 |
| Large text (≥ 18pt or ≥ 14pt bold) on background | 3:1 | 4.5:1 |
| Graphical objects (icons, important chart elements) | 3:1 | — |
| UI components and meaningful borders | 3:1 | — |

**Practical floors:**

- Body text on white: at least `#595959` (4.5:1). Default `#666` is borderline.
- Chart axis labels: `#333` to `#555` on white is safe.
- Light gray gridlines: keep them light (`#E0E0E0`) so they're clearly "background"; that's fine because they aren't text or meaningful objects.
- Annotation text on a chart's background: must hit 4.5:1.

**Test:** [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/), browser DevTools accessibility panel, [Color Review](https://color.review/).

**Pitfall:** sufficient contrast on a white background may be insufficient on a brand-color or photographic background. Test in context.

## 5. Text labels on the data (Tufte's "friendly graphic")

Tufte's "friendly graphic" criteria (p. 176) double as accessibility guidelines:

- Words spelled out, not encoded in legend keys.
- Run text left-to-right (no vertical y-axis labels).
- Label important events directly on the data.
- Avoid encoded shadings with legends elsewhere.
- Upper-and-lower-case serif type beats all-caps sans-serif.
- Accommodate color-deficient viewers (blue distinguishes for most).

The accessibility translation: **the more you label directly, the less the reader has to integrate** — across legend, axis, and data. Direct labels reduce cognitive load, work in grayscale, work for screen readers (via alt text or table fallback), and reduce dependence on color.

## 6. Alt text for data visualizations

A screen reader announces alt text in place of the image. Three-tier rule (from Amy Cesal's [Sunlight Foundation guide](https://www.urbaninstitute.org/sites/default/files/publication/97308/do-no-harm-guide_4.pdf)):

1. **Chart type and what it shows.** "Line chart of monthly active users for Web, iOS, and Android from Jan 2024 to Dec 2024."
2. **The headline takeaway.** "iOS overtook Web in August 2024 and continues to lead by ~15K users."
3. **A reference to the underlying data** if the chart can't be fully described. "Full data in the accompanying table."

What NOT to put in alt text:

- "Image of a chart." (Useless.)
- The legend list. (Already in the chart.)
- Marketing copy. (Alt text is for content equivalence.)

For complex charts (maps, multi-panel small multiples), pair alt text with a `<figcaption>` or linked detailed description that lays out the key patterns.

For interactive web charts, ensure:

- Each data series has an accessible label.
- Tooltips are keyboard-accessible (not just mouse).
- Focus states are visible.
- Tab order makes sense.

## 7. Screen-reader-friendly tables

For any non-trivial chart, **publish the data table alongside the chart**. This is the single most powerful accessibility move: it works for screen readers, search engines, copy-paste, citation, and re-analysis.

HTML pattern:

```html
<figure>
  <img src="chart.png" alt="Line chart of monthly active users for Web, iOS,
       and Android from Jan to Dec 2024. iOS overtook Web in August and
       continues to lead by ~15K users." />
  <figcaption>Monthly active users by platform, 2024.
    Source: internal analytics.</figcaption>
  <details>
    <summary>Data table</summary>
    <table>
      <caption>Monthly active users, 2024</caption>
      <thead>
        <tr><th scope="col">Month</th><th scope="col">Web</th>
            <th scope="col">iOS</th><th scope="col">Android</th></tr>
      </thead>
      <tbody>
        <tr><th scope="row">Jan</th><td>120,400</td><td>98,200</td><td>74,500</td></tr>
        <!-- ... -->
      </tbody>
    </table>
  </details>
</figure>
```

Use proper `<th scope>` attributes. Don't use tables for layout (they confuse screen readers when they're not actual data).

## 8. Motion and animation

The WCAG rule: respect `prefers-reduced-motion`. Implementation:

```css
@media (prefers-reduced-motion: reduce) {
  .chart * { animation: none !important; transition: none !important; }
}
```

For data viz specifically:

- **Animated transitions on quantitative data are usually a mistake** (modern extension of Tufte's "design variation, not data variation"). The reader cannot read a bar's height while it's transitioning.
- **Acceptable:** brief enter/exit transitions when the data hasn't changed (e.g., on hover, on chart load).
- **Acceptable:** small-multiple frame transitions used to *show* time-evolution (Hans Rosling's bubbles), provided they can be paused, scrubbed, and seen as a still grid of frames.
- **Avoid:** auto-rotating carousels of charts; charts that auto-animate without user control; "growing" bars on every dashboard reload.

For seizure safety: no flashing or strobing > 3Hz. Test with PEAT (Photosensitive Epilepsy Analysis Tool).

## 9. Dark mode adaptation

See `color.md` §9 for the color side. Accessibility-specific:

- Pure white (`#FFFFFF`) text on pure black (`#000000`) is too harsh; use `#FAFAFA` or `#E8E8E8` on `#1A1A1A` or `#0F0F0F`.
- Saturated colors on dark backgrounds appear to vibrate — drop saturation by 10-20% for accent colors in dark mode.
- Sequential ramps should reverse: dark-to-light in light mode → light-to-dark in dark mode.
- Provide a manual toggle in addition to `prefers-color-scheme` for users who want to override OS settings.

## 10. Font size and reading distance

The default chart axis-label font in matplotlib is 10pt. That's too small for projection screens, board decks viewed across a room, and many users with low vision.

Heuristic font sizes by context:

| Context | Body | Annotations | Axis labels |
| --- | --- | --- | --- |
| Detailed PDF report (read at desk) | 10pt | 8-9pt | 8-9pt |
| Print article (magazine, newspaper) | 9-10pt | 7-8pt | 8pt |
| Web article | 16px equivalent (~12pt) | 14px | 12-14px |
| Dashboard (read at desk) | 14-16px | 12-14px | 12-14px |
| Slide deck | 24pt+ | 18-24pt | 18pt+ |
| Boardroom / TV display | 32pt+ | 24-32pt | 24pt+ |

**Body text under 9pt is borderline accessible for anyone over 40.** Be generous.

## 11. Production checklist

Before shipping a chart to anywhere with users:

- [ ] Categories distinguishable in grayscale (test it)
- [ ] If quantitative color is used, it's a perceptually-uniform sequential or diverging palette (viridis family, ColorBrewer, BrBG/PuOr/RdBu for diverging)
- [ ] Categorical color palette is Okabe-Ito, Tol Bright, or another CVD-safe set
- [ ] Text contrast ≥ 4.5:1 (3:1 for large text or chart graphical elements)
- [ ] Axis labels and annotations spelled out, not encoded in a legend you have to look up
- [ ] Y-axis label is horizontal (above the axis), not vertical
- [ ] Alt text describes type, content, and headline takeaway
- [ ] If interactive, the data is also published as an HTML `<table>`
- [ ] Reduced-motion preference respected for any transitions
- [ ] Font size appropriate for the reading context (see table above)
- [ ] Tested at the actual rendered size (a chart that looks fine at 800×600 may be unreadable when embedded at 400×300)

## Resources

- [WAI Web Accessibility Tutorials — Complex Images](https://www.w3.org/WAI/tutorials/images/complex/) — official W3C guidance, gold standard.
- [Do No Harm Guide (Urban Institute / Cesal)](https://www.urban.org/research/publication/do-no-harm-guide-applying-equity-awareness-data-visualization) — accessibility + equity in data viz.
- [Highcharts accessibility module docs](https://www.highcharts.com/docs/accessibility/accessibility-module) — even if you don't use Highcharts, the docs explain the principles well.
- [matplotlib alt text best practices](https://matplotlib.org/stable/users/explain/figure/figure_intro.html#accessibility) — for static charts.
- [Sara Wachter-Boettcher, "Design for Real Life"](http://abookapart.com/products/design-for-real-life) — broader frame.
- [Sarah L. Fossheim's "Practical accessibility for data visualization" series](https://fossheim.io/writing/posts/accessible-dataviz-design/) — current best practices.

## When the user disagrees

- *"Our users don't have these problems."* — You can't know that. ~8% of male users have CVD by default. Designing for accessibility costs little and helps everyone (grayscale printing, dark mode, small screens, fatigue, projection rooms).
- *"Brand requires red/green."* — Use brand colors for non-data accents. The data series uses a CVD-safe palette. Frame it as: "the data must be readable for our user base; the chrome is where brand expression goes."
- *"Alt text is just for screen readers."* — Alt text also helps SEO, image-loading failures, and printouts. Treat it as captioning, not just assistive tech.
- *"Animations look more impressive."* — Impressive to whom? They are unreadable for the data they represent and hostile to motion-sensitive users. Static charts read better.
