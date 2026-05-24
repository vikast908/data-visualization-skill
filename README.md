# 📊 data-visualization

> A [Claude Code](https://claude.com/claude-code) skill that distills two foundational texts on quantitative graphics —  
> **Jacques Bertin's *Semiology of Graphics*** (the grammar) and **Edward Tufte's *Visual Display of Quantitative Information*** (the standards) —  
> into a single, actionable workflow that fires whenever you build, review, or critique any chart, dashboard, or map.

<p align="center">
  <em>"Above all else show the data."</em> &nbsp;·&nbsp; Tufte, 1983<br>
  <em>"One does not 'read' a graphic; one asks three questions of it."</em> &nbsp;·&nbsp; Bertin, 1967
</p>

---

## ⚡ Quick start

Three ways to install. Pick one.

**Option A — install the bundle:**
```bash
# Drop the .skill file into your skills directory
cp data-visualization.skill ~/.claude/skills/
```

**Option B — copy the folder:**
```bash
# Linux / macOS
cp -r skill ~/.claude/skills/data-visualization

# Windows PowerShell
Copy-Item -Recurse skill $HOME\.claude\skills\data-visualization
```

**Option C — symlink while developing:**
```bash
ln -s "$(pwd)/skill" ~/.claude/skills/data-visualization
```

Then restart Claude Code. The skill auto-loads on any visualization task — you don't need to invoke it explicitly.

---

## 🎯 What it does

When you ask Claude something like *"what chart should I use for…"*, *"review this matplotlib code"*, *"is this graph misleading?"*, or *"how should I lay out this dashboard?"*, this skill walks Claude through a 4-phase workflow grounded in Bertin's perceptual grammar and Tufte's integrity standards.

```mermaid
flowchart LR
    A[📋 Phase 1<br/>Analyze<br/><sub>Bertin</sub>] --> B[🎨 Phase 2<br/>Encode<br/><sub>Bertin</sub>]
    B --> C[⚖️ Phase 3<br/>Check Integrity<br/><sub>Tufte</sub>]
    C --> D[✂️ Phase 4<br/>Clean & Refine<br/><sub>Tufte</sub>]

    A -.- A1["• Invariant?<br/>• Components?<br/>• ≠ / O / Q level?"]
    B -.- B1["• 8 visual variables<br/>• Match level to variable<br/>• Reserve plane for Q"]
    C -.- C1["• Lie Factor ≤ 1.05<br/>• Deflate money<br/>• Compared to what?"]
    D -.- D1["• Erase non-data-ink<br/>• Kill chartjunk<br/>• Format choice ladder"]
```

The result is concrete, defensible visualization advice — not generic "use the right chart" platitudes.

---

## 📈 Benchmark results

The skill was evaluated against a non-skill baseline on 4 realistic prompts (cohort retention, 3D bar critique, choropleth classification, customer-success dashboard). Each prompt was graded on 5 substantive assertions.

| Metric | 🟢 With skill | ⚪ Without skill | Δ |
|---|---:|---:|---:|
| **Pass rate** | **100%** (20/20) | 80% (16/20) | **+20%** |
| Avg time | 78.5s | 63.4s | +15s |
| Avg tokens | 29,675 | 20,733 | +43% |

### Per-eval breakdown

| # | Test | With | Without | What the skill caught that the baseline missed |
|---|---|---|---|---|
| 1 | Cohort retention chart choice | 5/5 | 5/5 | *(tie — baseline strong here)* |
| 2 | 3D bar chart code review | 5/5 | 4/5 | Called out ALL-CAPS labels as Tufte's "computer duck" |
| 3 | Choropleth: Jenks vs quantiles | 5/5 | 4/5 | Raised Bertin's *Q → O* loss-of-resolution critique |
| 4 | CS dashboard layout | **5/5** | **3/5** | Led with table-as-primary-view; rejected KPI tiles as message-form misused for processing |

> **TL;DR** — the skill's marginal value is largely in pushing the model toward *second-order* rigor (typography, function-fit, systematic anti-patterns) rather than first-order chart-type selection, which the baseline already handles well.

📁 Full responses + the interactive comparison viewer live in `benchmarks-iteration-1/`. Open `benchmarks-iteration-1/review.html` in any browser.

---

## 🗂️ Repository layout

```
data-visualization-skill/
├── 📄 README.md                          ← you are here
├── 📦 data-visualization.skill           ← installable bundle (36 KB)
├── 🛡️  .gitignore
│
├── 📁 skill/                              ← the source of truth
│   ├── SKILL.md                          ← unified workflow (the "always-loaded" doc)
│   ├── references/
│   │   ├── bertin.md                     ← full grammar (visual variables, image theory, maps, networks)
│   │   └── tufte.md                      ← full canon (Lie Factor, data-ink, chartjunk, examples)
│   └── evals/
│       ├── evals.json                    ← 4 test cases with 5 graded assertions each
│       └── trigger_eval_set.json         ← 20 queries for description-trigger optimization
│
└── 📁 benchmarks-iteration-1/             ← evaluation artifacts
    ├── benchmark.md                      ← summary table
    ├── benchmark.json                    ← raw scores
    ├── review.html                       ← side-by-side comparison (open in browser)
    └── eval-*/                           ← 4 evals × 2 conditions × { response, grading, timing }
```

---

## 🧬 The grammar at a glance

Bertin's 8 visual variables and their 4 perceptual properties (the encoding lookup table the skill walks every time):

| Visual variable | Selective (≠) | Associative (≡) | Ordered (O) | Quantitative (Q) |
| --- | :---: | :---: | :---: | :---: |
| **Position** (X, Y) | ✓ | ✓ | ✓ | ✓ |
| **Size** | ✓ (limited) | ✗ (dissociative) | ✓ | ✓ |
| **Value** (light→dark) | ✓ | ✗ (dissociative) | ✓ | ✗ |
| **Texture** | ✓ | ✓ | ✓ | ✗ |
| **Color (hue)** | ✓ | ✓ | ✗ | ✗ |
| **Orientation** | ✓ (point/line) | ✓ | ✗ | ✗ |
| **Shape** | ✗ | ✓ | ✗ | ✗ |

The decisive rule: **the variable's perceptual capacity must equal or exceed the component's level**. Encode a quantity with color, and the eye can't read the ratio. Encode an order with hue, and the eye won't see the sequence.

The full table with derived rules, plus Tufte's Lie Factor, data-ink ratio, chartjunk catalogue, and format-choice ladder, lives in `skill/SKILL.md`.

---

## 🛠️ Provenance

This skill was built using [Claude Code](https://claude.com/claude-code)'s `skill-creator` plugin via the standard workflow:

1. **Extract** — two scanned PDFs were OCR'd and text-extracted with PyMuPDF, then split into ~30-page chunks.
2. **Read in parallel** — 6 sub-agents on Tufte (191 pages), 10 sub-agents on Bertin (462 pages), each returning structured digests.
3. **Synthesize** — digests were unified into a 4-phase workflow: Bertin's grammar drives encoding choice; Tufte's principles drive the integrity/cleanup pass.
4. **Test** — 4 realistic test prompts run twice (with skill, without skill) in parallel sub-agents; assertions graded by an independent grader sub-agent.
5. **Benchmark** — 100% vs 80% pass rate on iteration 1, no further iterations needed.
6. **Package** — bundled as `data-visualization.skill` for distribution.

Source books (not in this repo per `.gitignore`):

- Bertin, J. (1967, English transl. 1983, 2nd ed. 2010). *Semiology of Graphics: Diagrams, Networks, Maps*. ESRI Press.
- Tufte, E. R. (1983, 2nd ed. 2001). *The Visual Display of Quantitative Information*. Graphics Press.

---

## ❓ When the skill triggers

Anything that looks like working with data visualization, including these phrasings that don't name a chart library:

- *"how should I show this data?"*
- *"this dashboard feels cluttered"*
- *"is this graph misleading?"*
- *"my boss wants a 3D pie chart, talk me out of it"*
- *"what kind of map should I use for county-level census data?"*
- *"review this matplotlib code"*

Tested against 20 trigger eval queries in `skill/evals/trigger_eval_set.json` (10 should-trigger, 10 tricky near-misses like "optimize this BigQuery" or "figma checkout mockups" that share keywords but need other expertise).

---

## 📜 License

The skill content (the synthesis, workflow, and reference notes) is released under the **MIT License**. Tufte's and Bertin's underlying ideas are theirs — this skill is a study aid that distills and reorganizes their published frameworks for use inside an AI coding assistant. Read the originals.

---

<p align="center">
  <sub>Built with Claude Code · synthesized from two classics that should be on every analyst's shelf</sub>
</p>
