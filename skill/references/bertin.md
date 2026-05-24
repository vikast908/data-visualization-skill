# Bertin Reference — The Grammar of Graphics

The full systematic theory from *Semiology of Graphics: Diagrams, Networks, Maps* (Jacques Bertin, 1967; English translation 1983; 2010 reprint with the author's 2004 epilogue).

Bertin's framework is the closest thing visualization has to a periodic table. Everything in it is derived from one premise: graphics is a **monosemic** sign-system whose efficiency is governed by the properties of human visual perception.

## Table of contents

1. The three semiotic categories
2. Information: invariant, components, length, level
3. The three levels of organization (≠, O, Q)
4. The two perceptual approaches (selective, associative) and their four levels
5. The eight visual variables — properties table
6. The eight visual variables — detailed treatment
7. Implantation: point, line, area
8. Imposition: the four groups of construction
9. Image theory and the three-component limit
10. The reorderable matrix (matrix permutation)
11. The three functions of graphics
12. Diagrams: types and construction rules
13. Networks and trees
14. Maps: types, projections, scale, generalization
15. Cartographic quantitative representation (GEO Q)
16. Construction rules (cross-cutting)
17. Legibility rules (rules of separation)
18. The matrix theory synthesis (2004)
19. Canonical examples
20. Quotable lines

---

## 1. The three semiotic categories

Bertin places graphics within a taxonomy of sign-systems (p. 2, 437):

- **Pansemic** (music, nonfigurative image) — the sign means "everything" and thus nothing precise.
- **Polysemic** (verbal language, figurative image) — meaning emerges from context and varies by reader.
- **Monosemic** (mathematics, graphics) — "the meaning of each sign is known prior to observation of the collection of signs."

Graphics' power comes from monosemy: it is the *rational* part of the world of images. This is also its limit — graphics cannot do what poetry, photography, or music can.

## 2. Information: invariant, components, length, level

**Information** (p. 16): "a series of correspondences observed within a finite set of variational concepts or 'components.' All the correspondences must relate to an invariable common ground, which we will term the 'invariant.'"

Three analytical attributes of any information set:

- **Invariant** — the constant subject (e.g., "stock X, daily closing price").
- **Components** — the variational dimensions (e.g., time, price).
- **Length** — the number of distinguishable categories within a component.
- **Level** — the type of organization (qualitative, ordered, quantitative).

**Range** is distinct from length: range = ratio of smallest to largest in a quantitative series. A series 0.07–32 and one from 22–10054 share the same range (1:457) but imply very different graphic treatments.

Bertin's notation:
- `≡` = same / similar
- `≠` = different (qualitative)
- `O` = ordered
- `Q` = quantitative

## 3. The three levels of organization

(pp. 36-38, 54-57)

**≠ Qualitative / nominal:** categories not ordered universally; reorderable; equidistant. Examples: trades, products, religions. Generates two perceptual operations: "this is similar to that" (association) and "this is different" (selection).

**O Ordered:** single universally acknowledged order; categories defined as equidistant. Examples: temporal (year 1, year 2…); sensory (cold, warm, hot); moral (good, mediocre, bad); social rank.

**Q Quantitative (interval-ratio):** a countable unit exists. Statements like "this is double, half, four times that" become possible.

**Nesting rule:** Q ⊃ O ⊃ ≠. A quantitative variable can be read at all lower levels. A merely ordered variable cannot answer quantitative questions. A qualitative variable can be reordered arbitrarily but answers neither order nor quantity questions.

**Critical implication:** the visual variable used to encode a component must have at least the component's level. Encoding a Q component with a non-quantitative variable (e.g., color) forces the reader to read point-by-point through the legend — defeating the image.

## 4. The four perceptual approaches

For any visual variable, ask which of these the eye can do with it:

- **Selective (≠)** — "where are all the X's?" Immediately isolate one category.
- **Associative (≡)** — "where is everything, all categories combined?" Immediately group across the variable.
- **Ordered (O)** — "which is more, which is less?" Immediately rank.
- **Quantitative (Q)** — "this is twice that." Read the visual distance as a numerical ratio.

A **dissociative** variable is the opposite of associative — it makes some marks less visible than others, so it dominates any combination it enters. Size and value are dissociative.

## 5. The eight visual variables — properties table

(consolidated from pp. 69/87, 114-115, 439)

| Variable | Selective | Associative | Ordered | Quantitative | Notes |
| --- | :---: | :---: | :---: | :---: | --- |
| **Plane (X, Y)** | ✓ | ✓ | ✓ | ✓ | The only variables with all four properties. Reserve for most important components. |
| **Size** | ✓ (limited ~4-5 steps) | ✗ dissociative | ✓ | ✓ | The only retinal variable that is quantitative. Dominates combinations. |
| **Value** (light-dark) | ✓ (≤6-7 steps) | ✗ dissociative | ✓ | ✗ | "White cannot serve as a unit for measuring gray." |
| **Texture** | ✓ | ✓ | ✓ | ✗ | Ordered + selective + associative. Underused. |
| **Color (hue)** | ✓ | ✓ | ✗ | ✗ | No innate order. Strongly conditioned by value. |
| **Orientation** | ✓ point/line, ✗ area | ✓ | ✗ | ✗ | 4 selective steps in point; 2 in linear. |
| **Shape** | ✗ | ✓ | ✗ | ✗ | Infinite possibilities, but only 3 selective marks: point, dash, cross. |

**Classing sequence by descending perceptual power:** plane → size → value → texture → color → orientation → shape.

**Image variables vs. differential variables** (2004 epilogue, p. 433):
- **Image variables:** plane, size, value. They modulate the *quantity* of light energy and can carry quantitative meaning.
- **Differential variables:** texture, color, orientation, shape. They modulate the *quality* of energy, not quantity.

## 6. The eight visual variables — detailed treatment

### Position (the plane, X and Y)

The two planar dimensions are the most powerful variable. They alone display all four perceptual properties. The plane is continuous, homogeneous, and *meaningful* — "absence of signs signifies absence of phenomena" (p. 46/64). Empty regions are not empty; they assert that nothing is there.

Consequences:
- Cannot use "white = missing data" without ambiguity.
- An "inset" is a supplementary image, never a replacement.
- Same concept cannot use different implantations (point vs. line vs. area) in one image — differences in implantation are themselves selective.

### Size

Variation in mark size. The only retinal variable that carries quantitative meaning, because the eye reads area as ratio.

Limits:
- ~20 ordered steps in ratio 1:10.
- ~4-5 selective steps (more steps lose category distinction).
- Dissociative: smaller marks become invisible, so size dominates any combination.
- For *area* encoding of a quantity Q, the area should be proportional to Q — never the radius. If sector angles are equal in a polar diagram, radii must be proportional to √Q so areas are proportional to Q.

The **natural series of graduated sizes** (p. 369, fully developed in the 2004 epilogue): 20 steps between 1 and 10, progression = 20th root of 10 ≈ 1.122. Avoids arbitrary step choices and yields perceptually true proportional symbols. Larger circles use radii drawn directly; small ones use preprinted patterns.

### Value (light-to-dark)

Ratio of black to white perceived on a surface. Ordered and selective, but not quantitative — the eye cannot read value as ratio.

Limits:
- ≤ 6-7 selective steps including pure black and white.
- 11 equidistant steps available via texture-supported value scales.
- Dissociative — like size, dominates combinations.

Choice between value and texture for ordered area data: value is faster to read but flattens density perception across the whole image. Texture preserves density but reads more slowly.

### Texture

Number of separable marks within a unit area, at constant value. Ordered, selective, associative.

The **vibratory effect** (p. 98): central values (~50%) with marks > 1 mm produce shimmer from the collusion of physiological eye tremor and figure/ground hesitation. "The designer's duty is to flirt with ambiguity without succumbing to it."

This is the point where Bertin and Tufte explicitly disagree — Tufte calls all moiré chartjunk (Tufte p. 107). Default to Tufte; reach for vibration only when emphasis is the explicit goal.

Texture also degrades under photographic reduction — fine textures vanish, coarse textures become solid. Designers of atlases must anticipate microfilm-grade reproduction.

### Color (hue)

Difference between uniform areas of the same value. Selective and associative. Not ordered, not quantitative.

Key facts the eye establishes:
- The saturated tone of each hue lives at a different value — yellow saturates light, violet dark. *"The saturated tone is not of constant value, but varies in value according to the color."*
- The spectrum is NOT an ordered series. Eye reads colors by value: blue and red (both dark) fuse perceptually against yellow (light center).
- Color fusion: smaller marks lose color. Marks must be ≥ 1.5 mm to support color variation.
- Selectivity by value: light values use green→orange; medium values are most selective (blue vs. red diametrically opposed); dark values use blue→red through violet/purple.

Practical rules:
- **Color is never strictly indispensable** for analysis/processing — color is selective but information processing involves order, which color cannot encode.
- Always combine color with another variable so a B/W photo of the graphic remains decipherable.
- For ordered/quantitative components, use **gray scale** (which is ordered by value) — not the spectrum.
- 5-10% of viewers have color-deficient vision; blue distinguishes for most.

### Orientation

Angular direction of linear marks. Selective for point and line implantations; *not* selective for area.

Limits: ~4 selective orientations in point (oblique 30°/60°, not 45° which reads as in-between); 2 in linear (axis + perpendicular). Requires marks of height/base ≥ 4:1.

### Shape

Infinite morphological variation at constant area. **Associative only** — not selective, not ordered, not quantitative.

Implication: shape answers "is this similar?" but cannot answer "where are all the X?" Use only at the elementary reading level — for revealing similarity and for external identification via mimetic/symbolic shapes (when the domain is restricted and habit is established).

"There is no universal shape signification. The meaning of a symbol becomes familiar to us only by habit" (p. 113).

In practice, only three shapes survive at small sizes and remain selective: **point, dash, cross**.

## 7. Implantation: point, line, area

(p. 44/62)

Every mark on the plane has an implantation:

- **Point** — represents a location with no theoretical length or area.
- **Line** — measurable length, no area.
- **Area** — measurable size; signification applies to the entire visible area.

Implantation governs which retinal variables are even available, and how many selective steps each variable affords:

- A point can take size, value, texture (limited), color (small marks fuse), orientation, shape (if ≥ 2mm).
- A line can take size (= width), value, texture, color, orientation (if not the line's own axis), shape (limited).
- An area can take size (changes meaning, since area = quantity), value, texture, color, orientation (weakly), shape (poorly).

**Same concept cannot use different implantations in one image** — switching implantation is itself a categorical signal.

## 8. Imposition: the four groups of construction

(p. 50/68)

How the two planar dimensions are used divides every graphic into one of four families:

- **Diagram** — correspondences between divisions of two *different* components (price × time; trade × department).
- **Network** — correspondences among divisions of the *same* component (friend-of relationships; supply chains).
- **Map** — a network arranged in observed geographic order; a special case of network where the plane order is given by geography.
- **Symbol** — correspondence between an element and the reader, exterior to the graphic (road signs, military symbols).

A map is, in Bertin's pithy summary, "an ordered network" (p. 285).

## 9. Image theory and the three-component limit

(pp. 11-12, 154-160, 440)

**Image** (p. 160): "the meaningful visual form perceptible in the minimum instant of vision." The entire graphic is grasped in a single glance.

**Figuration** (p. 163): a construction requiring multiple images to extract the answer — multiple charts, multiple views, multiple legends to consult. Far less efficient than an image.

**The capacity limit:** an image can carry at most **three variables** (p. 154). Two planar dimensions + one ordered retinal variable. A fourth meaningful variable cannot be perceived in one image.

This is why **small multiples** (Tufte's term; Bertin's "collections of comparable images") are the standard solution for >3-component data: keep one image at a time small enough to grasp, then array many of them together.

The three questions a graphic should answer in one glance (preface p. xiv, restated p. 441):
1. What are the X, Y, Z components?
2. What groups in X and Y are formed by the Z data?
3. What are the exceptions?

A graphic that fails any of these three is a figuration, not an image — even if pretty.

**Three levels of reading** (p. 10, 159):
- **Elementary** — one element of a component (one row).
- **Intermediate** — a group of elements.
- **Overall** — the whole component (global pattern).

A graphic optimized only for elementary reading is impoverished. The eye wants all three.

## 10. The reorderable matrix (matrix permutation)

(pp. 168-169, 254-257, 440-443 — the centerpiece of Bertin's late synthesis)

A **reorderable matrix** is a 2-D table where both axes are reorderable (≠ × ≠), cells carry quantity via value/size, and the user permutes rows and columns until similarity blocks emerge on the diagonal.

The key insight: **diagonalization preserves all correspondences** — it is lossless simplification by ordering. Different from smoothing or aggregation, which discard information.

Use whenever:
- Data table has > 3 rows.
- Both axes are reorderable categorical components.
- Discovery of structure is the goal (not communication of a known structure).

Tools (historical):
- The **Permutator** (Pagés & Lévy-Schoen, C.N.R.S.) — a magnetic table holding up to 80×80 categories for free permutation.
- **Domino** (Laboratoire de Cartographie, E.P.H.E.) — extends to a third quantitative component.

Modern equivalents: any tool that lets you re-sort rows and columns of a heatmap by clustering, by manual drag, or by a chosen variable. Most BI tools fail at this; specialized tools (Seriation in R, ordering in Vega-Lite, manual permutation) succeed.

Validation: any subjective typology can be tested by re-ordering the matrix per that typology and checking whether ≥ 5% of criteria align (p. 257).

## 11. The three functions of graphics

(pp. 160-164, 437)

Match the construction to the function:

- **Recording / inventory** — comprehensive storage replacing memory. Need not be memorizable. A topographic atlas, a complete census table. Optimize for completeness.
- **Communicating / message** — memorizable image inscribed in memory. Need not be comprehensive. A presentation slide, a headline chart. Optimize for retention.
- **Processing** — must be both comprehensive AND image-graspable, because the graphic is the instrument of discovery. Optimize for transformability — reorderable matrices, image-files, small multiples that can be reordered to test hypotheses.

Bertin: *"Information processing entails comprehensivity. Communication involves simplification."*

A common dashboard failure: using a *message* form (a single KPI tile, a simplified pie chart) for a *processing* task (looking for anomalies, trends, correlations). The user needs to see the full surface and rearrange it; the message form has thrown that information away.

## 12. Diagrams: types and construction rules

(pp. 211-235, classified by component levels)

**By component levels:**

- **≠ × ≠ (two reorderable):** diagonalization / scalogram. Permute both axes to make similarity cluster on the diagonal. Carneiro's cultural-traits scalogram yields Guttman's reproducibility coefficient C = 1 − (differences)/(L₁ × L₂).
- **# × ~ (one reorderable, one ordered):** order the reorderable against the ordered to approach the diagonal.
- **# × # (two ordered):** no permutation possible; read existing correspondence.
- **Q × ~ (limited):** allows "special constructions" (pie sectors, polar, linear) since number of images ≤ length of ~.
- **Q × ~ (extensive, repartitions):** standard construction is mandatory. Classes of values reveal levels, distributions, concentrations.
- **Q × Q (time series, scatter):** processing graphic seeking structure, categories, correlation.
- **Q × Q × Q:** standard construction (isarithms, perspective, age pyramids).

**Construction rules:**

1. Axes orthogonal; both planar dimensions used homogeneously.
2. Third component goes to an ordered retinal variable (size, value, or texture).
3. Diagonalize at construction time, not after.
4. **Angular optimum:** ~70° on the elementary level; diagonal of a square on the overall level — compromise between them.
5. **5-10% of "black"** in the meaningful area. Too little = invisible; too much = blurred.
6. **Figure must separate from background** at the first perceptible step.
7. **Square format** (or rectangle ≤ 1/2 ratio) maximizes angular legibility.
8. **Use the full perceptible range** — under-utilization wastes capacity.

**Repartition / distribution / concentration** (pp. 223-229) — three transformations of the same Q × O data:
- **Repartition** = ordered diagram revealing levels.
- **Distribution** = number of observations per equal class (histogram).
- **Concentration** = cumulative curve. "The integral of the repartition."

Means, medians, medials, modes, quartiles all have graphic definitions on these forms (pp. 226-227).

**Time-series comparison** (pp. 252-257): when comparing two or more series:
- Free choice of scale is illegitimate; slope must be rigorously represented.
- **Index from a common base** (Q_n / Q_i × 100) eliminates scale arbitrariness.
- For very large ranges or small relative differences, switch to **log scale** — only slope is meaningful on it; curves can slide along the log axis to improve legibility.
- Always show common origin in linear comparisons.

## 13. Networks and trees

(pp. 269-283, 287)

**Network** = correspondences among elements of the same component. The plane has no inherent meaning at the start — you plot first, then seek the arrangement that minimizes meaningless intersections.

**Construction process:**
1. Plot on a meaningless plane.
2. Start with **circular construction** — best visual point of departure.
3. Permute to reduce intersections. (Two intersections may be preferable to none if they expose meaningful sub-groupings.)
4. If the element set has an ordering (size, time, hierarchy), give the plane that order — convert to a rectilinear construction.
5. For very complex data, run a **permutable matrix** first, then build the network.
6. For very dense networks, an **adjacency matrix with permutation** often beats a node-link diagram.

**Tree** = a network in which there is only one path between any two nodes. Can be drawn:
- Points + lines (standard).
- Lines replacing points so direction-change = node (line tree).
- Circular trees for genealogies; ordered circular web for very large ones.
- Where lines = individuals, length can encode lifespan.

**Inclusive relationships** = areas containing other areas (set diagrams, organizational hierarchies). The 1875 French Constitution's plane "spontaneously reads as ordered/hierarchical"; the 1795's "as disorganized" — the plane itself can suggest a meaning of "nonorganization."

## 14. Maps: types, projections, scale, generalization

(pp. 285-330+)

**Map** (p. 285): "A graphic is a geographic map when the elements of a geographic component are arranged on a plane in the manner of their observed geographic order on the surface of the earth." Or: **"a map is an ordered network."**

### Projection choice

(p. 293, summary)

Choose by:
1. **Condition to preserve** — conformal (angles), equivalent (areas), or compromise (aphylactic).
2. **Shape of region** — azimuthal/zenithal (circular/square regions), conic (small-circle arcs; temperate-zone topo maps), cylindrical (great-circle arcs; Mercator).
3. **Tangent point** — polar / equatorial / oblique.

Place the zone of least deformation under the most pertinent region; dump deformation into the Pacific or wherever the data isn't.

**Two generations of planispheres:** "classic" (axes coincide with terrestrial coordinates) vs. "oblique" (least-deformation zone coincides with the continents).

### Scale

(p. 296)

- Express scale graphically (line + units), since fractionary scales (1:25,000) become meaningless after rescaling/microfilming.
- **Scale unity in a series** — when reference space is constant across a series of maps, scale must be too. Exceptions point to methodological error.
- Plot at larger scale than you'll publish; never compile a map at the same scale as the source. Rule of thumb: source ~10× larger than final.

### Generalization

(pp. 300-307)

**Generalization** = "the spatial equivalent of simplification" = regionalization.

Two types:
- **Conceptual** generalization changes implantation and level (mines → coal field).
- **Structural** generalization preserves implantation, simplifies distribution (smooth the river outline, keep it a river).

Rules:
- Any correct generalization must stem from the original comprehensive document, which must remain under the designer's eyes.
- For lines: distinguish symmetric lines (rivers, roads — symmetric loops) from boundary lines (coastlines, contours — asymmetric: angle toward the sea for coasts, toward the landform for contours).
- **Use graphic error positively**: displacements ≤ 1/4 mm are imperceptible as dimensional error, so spend them on encoding *relational* truths (city border on river, foot of mountain).

### Base maps

(p. 308)

- A base map is "the set of known reference points which are necessary and sufficient for situating the as yet unknown elements of the new information being mapped."
- Choose reference points by (a) proximity, (b) stability (peaks/cliffs beat rivers/roads), (c) field of knowledge of the average reader.
- **Reference signs must never be more visible than the new information.**
- **Two-stage drafting:** rough sketch with all reference points, then a simplified definitive drawing.

## 15. Cartographic quantitative representation (GEO Q)

(pp. 374-376+ — central to Bertin's modern impact on data visualization)

A **GEO Q** map represents geographic data whose interesting component is quantitative. The graphic must let the viewer perceive *the variation in distance among categories* — not merely some pre-classed grouping.

**The honest representations:**
- Proportional symbols (regular pattern of proportional points or circles).
- Dot maps with equal-sized dots.
- Relief / perspective 3-D.

**The dishonest default: choropleth maps with pre-classed intervals.** Choosing class intervals before plotting "transforms the component Q into an interpretation O" (p. 374). This is fine for *communication* of a known result; it is forbidden for *discovery*.

Bertin's preferred construction (the most powerful innovation in his framework): **a series of proportional circles applied to a regular pattern of points** across the map. Avoids the trap of pre-classing, preserves quantitative perception at every reading level.

For absolute quantities by area (population, GDP), always reduce to densities (Q/S) before mapping — never plot QS on area as area, since the visual encoding would compound to QS².

**Multi-component cartographic problems** (p. 397):
- **Inventory maps** answer "what's at this place?" — comprehensive, point-by-point read.
- **Processing maps** = collections of comparable images, one per component, classable.
- **Cartographic messages** = simplified superimpositions of the essential aspects.

A *single* loaded map cannot do all three. Choose the function first.

**Chartmaps** (diagrams scattered on a geographic base) are "hardly ever efficient" beyond binary categorical components. The Ministère de la Construction's five-component workforce map would require "120 × 3 × 6 × 2 = 4320 successive images" to grasp (p. 395).

**Movement maps** (GEO O with time):
- Three solutions: series of images, trace + direction (arrow), or ordered retinal variable.
- Use **arrows with weight at the head** for vectors.
- For flow maps that don't show direction, line width = density of ships at sea, not direction of trade.

**Isarithms (contour lines)** are weak: they show slope only, not absolute height/volume/altitude/direction in a single image (p. 385). Use only when slope is the answer.

## 16. Construction rules (cross-cutting)

(pp. 189-192, 207)

- **Single-image rule:** "Represent the information in a single image, or in the minimum number of images necessary."
- **Standard schema:** orthogonal planar layout + ordered retinal variable for the third component.
- **Simplification by ordering** (lossless) for reorderable components.
- **Simplification by reduction** (lossy) only for ordered components that cannot be reordered.
- **Redundant combination** = several variables for the same component (boosts selectivity, basis of selective legibility).
- **Meaningful combination** = two variables encoding two different components, with the image based on size or value.
- Familiar component on the easier axis: lateral discrimination is more developed than vertical, so put the familiar reference (time, scale) on the horizontal.

## 17. Legibility rules (rules of separation)

(pp. 193-208)

Where construction rules are like grammar, legibility rules are like pronunciation and sound quality.

- **Graphic density** (p. 194): "Ten signs per cm² represents a maximum limit" for figurations. For true images (homogeneous), no maximum.
- **Angular legibility** (p. 196): optimum near 70° elementary, square-diagonal overall. Compromise.
- **Retinal legibility** (p. 198): differentiation among steps of a retinal variable; depends on size of marks and step count.
- **5-10% black rule** (p. 198): total "black" on a meaningful area.
- **Figure / ground** (p. 199): subject matter must dominate the background; new information must dominate reference.
- **Subject vs. base contrast**: the base map's lines as light as possible; new information stronger. The lighter the first step, the less it is necessary to enlarge the lines of the second.

## 18. The matrix theory synthesis (2004 epilogue)

(pp. 432-452 — Bertin's own late summary)

By the 2004 epilogue, Bertin reduced his entire theory to:

> "Graphics utilizes the properties of the visual image to bring out relationships of similarity and order among the data."

Three questions (the same as the preface, refined):
1. What are the XYZ components of the table?
2. What groups in X and Y are formed by the Z data?
3. What are the exceptions?

Two objectives:
1. Data processing — understand and derive.
2. Communication of derived insight.

These are governed by **comprehensivity** (processing) vs. **simplification** (communication).

The **reorderable matrix** is the basic graphic construction because it "gives concrete form to a chain of logical operations: data-matrix-groups-exceptions-discussion-decision-action."

The **3-dimension limit** (X, Y, Z) is why "synthetic interdisciplinary science is structurally impossible" — each discipline is defined by its choice of X (space for geographers, time for historians, individuals for psychologists, social categories for sociologists). Time is the fourth dimension, "which is precisely what should be minimized."

Closing aphorism of the book (p. 452):
> "Useful information processing can exist only within the framework of a finite set: the data table. But there are an infinite number of finite sets. Whatever our efforts at rational thinking, they will always be drowned in the infinite sea of the irrational."

## 19. Canonical examples

| Example | Pages | Lesson |
| --- | --- | --- |
| Stock X on the Paris exchange | 16 | Canonical 2-component example introducing invariant + components |
| Cuban Missile Crisis | 18, 282 | 5 components defeat single-image construction; example of array-of-curves |
| Tougan Region ethnic distribution | 341 | Same data three ways (value-ordered, size-ordered, equal-visibility) shows selectivity tradeoffs |
| British household expenditures | 28-29 | Reordering rows transforms useless figuration into meaningful image |
| Spectrum vs. value-scale demonstration | 102-106 | Hue alone cannot order; value can |
| Anthropometric map of Europe | 154-155 | Three-variable limit forces split into 3 maps or one figuration |
| French working population by sector | 142-156 | Hundred different graphics from one data set, only a few are images |
| Stanciu theft × age × amount | 244-247 | Pipeline: percentages → expected → observed → deviation → tendency |
| Carneiro South American scalogram | 215 | Diagonalization revealing cumulative cultural sequence |
| Theodorescu ionic capitals (82×80 matrix) | 256-257 | Reorderable matrix → 3 systems, 3 types, 9 subtypes from chaos |
| Tulle region rural planning | 286 | 62 communes × 108 indicators reduced via matrix + collection of maps |
| EU meat production (5×5) | 440-441 | Canonical reorderable-matrix demo |
| Da Silva Spanish economic censuses | 397-401 | 1500 data points × 10 components → 36 images on one page |
| Siegfried Ardèche electoral geography | 401-405 | Graphic processing reduces 53 maps × 4 components to 2 questions / 2 answers |
| US Chemical Industry raw materials | 448-449 | One map answers "what's in Florida?"; collection of maps answers "where's the petroleum?"; simplified synthesis maps satisfy both |
| Eurasia 1936 population dots | 361 | Proportional equal-sized dots |
| Malinvaud economic circuit | 290 | Standard circular network construction |
| Elector movement 1953 | 290-291 | Conversion from circular to linear ordered network reveals migration pattern |
| Jenkins squadron morale | 292 | Irregular network with value distinguishing friendship/enmity |
| Genghis Khan genealogy (1230 persons) | 276-277 | Circular tree, ordered vs. unordered constructions |
| Djebel Ansarine genealogy | 280-281 | Superimposed male tree + female trellis reveals parallel-cousin marriage |
| French Constitutions 1795 vs 1875 | 282 | Inclusive areas + vectors; plane suggests "organization" or "nonorganization" |
| Dombes lakes | 304-307 | Nine published maps fail because generalization derived from already-generalized sources |
| Lyon-Chambéry protohistoric atlas | 311 | Publishable positional map using proximity-chosen references |
| Hotel monthly indicators image-file | 258 | 20 × 12 image-file; 10% revenue lift from acting on revealed cycles |
| Mossi colonization, Bwa country | 263 | Matrix-file with 350 × 27 and four successive classings |

## 20. Quotable lines

**On the framework:**
- "Graphics utilizes the properties of the visual image to bring out relationships of similarity and order among the data." (p. 437)
- "Graphics is the visual means of resolving logical problems." (preface)
- "One does not 'read' a graphic; one asks three questions of it." (preface p. xiv)
- "A graphic should not show only the leaves; it should show the branches as well as the entire tree." (preface p. xiv)
- "Information processing entails comprehensivity. Communication involves simplification." (p. 437)

**On the visual variables:**
- "An order will not be perceptible if the variable is not ordered; a ratio will not be perceptible if the variable is not quantitative." (p. 32/52)
- "White cannot serve as a unit for measuring gray." (p. 87)
- "A dissociative variable dominates all combinations made with it." (p. 65/83)
- "There is no universal shape signification. The meaning of a symbol becomes familiar to us only by habit." (p. 113)
- "Any effort at coloration is a struggle against natural mixture, against color 'fusion.'" (p. 107)
- "Color is never strictly indispensable in the stages of research, analysis, and processing." (p. 109)
- "We see only ratios." (p. 370)

**On the plane:**
- "In a signifying space absence of signs signifies absence of phenomena." (p. 46/64)
- "Any visual variable appears as meaningful." (p. 46/64)
- "The surface of the paper signifies the surface of the earth; an excellent analogy, since space is utilized to signify space." (p. 58/76)

**On the image:**
- "The meaningful visual form, perceptible in the minimum instant of vision, will be called the IMAGE." (p. 160)
- "The most efficient constructions are those in which any question, whatever its type and level, can be answered in a single instant of perception, in a single image." (p. 11)
- "Visual efficiency is inversely proportional to the number of images necessary for the perception of the data." (p. 12)
- "The image will not accommodate the representation of a meaningful fourth variable." (p. 154)

**On simplification:**
- "Simplification of the image by ordering does not eliminate any correspondence and preserves the integrated totality of the information." (p. 166)
- "The discovery of an ordered concept thus appears as the ultimate point in logical simplification." (p. 166)
- "To class 200 communal maps of France (38 000 communes) is relatively easy, and yet this represents the processing of 7 600 000 pieces of information." (p. 255)

**On maps:**
- "A map is an ordered network." (p. 285)
- "Cartography is the sole means of simplifying the geographic component as a function of spatial relations." (p. 285)
- "We must have at our disposal information on a scale about ten times larger than the definitive drawing." (p. 303)
- "Any correct generalization must stem from the original comprehensive document." (p. 307)
- "A map which is loaded with new information is a priceless document." (p. 308)
- "Determining the steps is the goal of the graphic operation, not its means." (p. 374)
- "Any choice of steps made prior to the construction of the 'relief' will transform the component Q into an interpretation O." (p. 374)
- "Visual habit has thus assumed the power of law, even though it does not conform with the fundamental conditions of the problem being posed." (p. 374)
- "The reader who probes such maps for the nature of the coastline commits an error in reading. These maps are not made to answer such questions." (p. 330)
- "The lines of a map should not evoke a plate of spaghetti, but rather the marvelous structures which any designer can now admire during the course of air travel." (p. 333)

**On craft:**
- "It is the designer's duty to flirt with ambiguity without succumbing to it." (p. 98)
- "Showy but useless graphics do a real disservice to graphics. Experiments have shown that readers do not even look at them." (p. 260)
- "The graphic representation of erroneous data is not to be feared during the course of research. On the contrary, it always yields useful information." (p. 400)
- "Useful information processing can exist only within the framework of a finite set: the data table. But there are an infinite number of finite sets. Whatever our efforts at rational thinking, they will always be drowned in the infinite sea of the irrational." (p. 452 — closing)

**On the contribution of graphics to thought:**
- "With hindsight, the principal contribution of GRAPHICS seems to me to be in the realm of reasoning, with the precise and constructive visualization of the different stages of a study, even before that study is undertaken." (p. 435)
- "In the absence of adequate education concerning graphics, the power of visual perception is still underappreciated." (p. 434)
- "Graphics is learned, not inherited." (preface p. xv)
