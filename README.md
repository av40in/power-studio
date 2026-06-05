# Power Studio

**Power Studio** is a self-contained, browser-based sample size and power analysis tool designed for preclinical oncology research, particularly PDX (Patient-Derived Xenograft) studies. It generates statistically rigorous animal number justifications ready for IACUC / ethics board submissions — with no installation, no server, and no internet required after loading.

> Companion tool to **TGI Studio** (PDX tumor growth analysis)

**© 2026 Alexey Sorokin**

---

## Table of Contents

1. [Quick Start](#quick-start)
2. [Calculation Modes](#calculation-modes)
3. [Sidebar Inputs](#sidebar-inputs)
4. [Output Cards](#output-cards)
5. [Statistical Justification](#statistical-justification)
6. [Budget Estimator](#budget-estimator)
7. [Formulas Reference](#formulas-reference)
8. [Tips](#tips)

---

## Quick Start

1. Download `Power_Studio.html`
2. Open in any modern browser (Chrome recommended)
3. Enter your group means, SDs, and desired power
4. All outputs update **live** as you type
5. Click **📋 Copy** to copy the justification text for your IACUC submission

No installation. No server. No internet required after first load.

---

## Calculation Modes

Select via the **Study Design** toggle at the top of the sidebar:

| Mode | What it calculates | What you provide |
|------|--------------------|------------------|
| **Sample Size** | n per group | Means, SDs, α, power |
| **Power** | Achieved power % | Means, SDs, α, known n |
| **Min. Δ** | Smallest detectable difference | Means, SDs, α, power, known n |

---

## Sidebar Inputs

### Study Design
- **Calculation mode** — Sample Size / Power / Min. Δ
- **Test type:**
  - t-test two-sided — standard, most common for PDX efficacy
  - t-test one-sided — directional hypothesis
  - One-way ANOVA — multi-group comparison (reveals the ANOVA card)
- **Variance assumption** — Unpooled (Welch, σ₁ ≠ σ₂) or Pooled (σ₁ = σ₂)

### Statistical Parameters
- **Significance level α** — 0.10 / 0.05 / 0.02 / 0.01
- **Desired power (1−β)** — slider from 50% to 99%
- **Known n per group** — shown in Power and Min. Δ modes

### Group Parameters
- **Group 1 / Group 2 mean** — expected endpoint values (e.g. tumor volume)
- **Group 1 / Group 2 SD** — standard deviation from pilot data or literature
- **Don't know SD? Calculate from CV%** — enter a coefficient of variation, click Apply to fill SD fields (SD = mean × CV / 100)

### PDX Variability Sensitivity
- Slider from +0% to +100% above baseline CV
- Shows how n changes if your PDX variability is higher than expected
- The Study Design Visual updates with dashed amber bands showing the wider spread
- Justification text includes a sensitivity analysis note

### Study Setup (for Justification)
All fields feed directly into the Statistical Justification text and total animal calculation:

| Field | Description |
|-------|-------------|
| **Efficacy animals per arm** | Auto-filled from sample size calculation (read-only) |
| **Dropout / attrition rate (%, if needed)** | Inflates n to account for expected losses. Minimum: +1 animal |
| **PD / pharmacodynamic animals per arm** | Early-euthanized cohort for tissue/biomarker collection |
| **Additional sampling timepoints (if needed)** | Extra animals for interim bleeds or PK/PD timepoints — specify n timepoints, n animals per timepoint, and n selected arms |
| **Animals per arm summary** | Live breakdown: Efficacy + Dropout + PD + Sampling = Total per arm |
| **Experimental arms** | Number of treatment groups (also used for ANOVA Bonferroni correction) |
| **Models** | Number of PDX models |
| **Engraftment / take rate (%)** | Adjusts total for expected non-engraftment |
| **Study name / drug** | Optional label included in justification text |

**Total animal formula:**
```
Total = (base_n/arm × all_arms + sampling_n × selected_arms) × models / take_rate
```

---

## Output Cards

### Sample Size Results
- **A (minimum)** — Power 50%: lowest justifiable n, high false-negative risk
- **B (optimum, highlighted)** — Power at your selection: standard recommendation
- **C (maximum)** — Power 95%: upper bound before over-powering
- **Total animals** — n × number of groups
- **Cohen's d** — effect size with qualitative label (small/medium/large)
- **Δ%** — difference as % of Group 1 mean

### Full α × Power Table
Matrix of n per group across 4 α levels (0.10/0.05/0.02/0.01) and 4 power levels (50%/80%/90%/95%). Your current selection is highlighted in teal. Changes live as you adjust inputs.

### Interpretation Box
Plain-language summary: "To detect a difference of X between Group 1 (μ=Y, σ=Z) and Group 2... you need N animals per group."

### Study Design Visual
Schematic line chart showing expected group trajectories:
- Solid bands — current SD
- Dashed amber bands — simulated variability (when CV slider > 0)
- Legend shows SD and CV values with → arrows when variability is simulated

### Power Curve
Power vs n for three α levels simultaneously. Your current selection marked with a vertical line. Updates live.

### Sensitivity Analysis
Required n vs detectable difference (% of Group 1 mean). Useful for understanding trade-offs between effect size and animal numbers.

### SD Sensitivity Heatmap
6×5 grid: n per group as SD multiplier (0.5×–2×) and Δ multiplier (0.5×–1.5×) vary. Current inputs highlighted. Teal = low n, coral = high n.

### Multi-group ANOVA (shown when ANOVA mode selected)
- Cohen's f effect size and qualitative label
- Required n per group for k = 2, 3, 4, 5, 6, 8, 10 groups
- Bar chart: n per group vs total animals as k scales
- Your current k highlighted

---

## Statistical Justification

Click **📋 Copy** to copy a complete, IACUC-ready paragraph to your clipboard.

The text auto-generates from all inputs and includes:

- **Test type** and variance assumption
- **α level** and power
- **SD range** and Cohen's d with effect size label
- **Bonferroni correction** note (ANOVA mode only)
- **Minimum detectable difference** as % of Group 1 mean
- **PD and sampling timepoint** animal breakdown (if entered)
- **Dropout adjustment** note (including "*adjusted to minimum +1 animal" when fractional)
- **Variability sensitivity** note (when CV slider > 0)
- **Total animal calculation** formula: models × arms × n/arm / take rate

**Example output:**
```
Statistical justification [Generated by Power Studio · 6/5/2026]:

Tumor volumes from treated mice will be compared with those from control
mice using a two-sided Students t-test, using Welchs correction for
unequal variances. The study is powered at α = 0.05 with 80% power,
assuming a standard deviation of 30.0–60.0 (Group 1 SD = 60, Group 2
SD = 30; Cohen's d = 1.49, large effect size).

A sample size of 8 animals per group provides sufficient power to detect
a minimum difference of approximately 50% between group means
(|Δ| = 100.0, Group 1 mean = 200, Group 2 mean = 100).

Total animal estimate:
  4 arms × 8 animals/arm / 70% take rate = 46 animals total.
```

---

## Budget Estimator

Located at the bottom of the page. Updates automatically when study design changes.

| Input | Default | Description |
|-------|---------|-------------|
| Total animals | Auto | Pulled from study design total |
| Price per animal | $150 | Purchase cost per mouse |
| Supplies per animal | $30 | Consumables, reagents, etc. |
| Study duration | 80 days | Used for housing calculation |
| Housing cost per cage per day | $1.50 | Institutional per diem rate |
| Animals per cage | 5 | Standard mouse housing density |
| Shipping cost per box | $100 | Cost per shipping container |
| Animals per shipping box | 50 | Standard box capacity |
| Additional costs | $0 | Imaging, histology, genotyping, etc. |

**Outputs:**
- Total estimated cost
- Breakdown by category with % of total
- Color-coded proportional bar chart
- Number of cages and shipping boxes
- Cost per animal

---

## Formulas Reference

### Sample size (unpooled, two-sided t-test)
```
n = (Z_α/2 + Z_β)² × (σ₁² + σ₂²) / Δ²
```

### Sample size (pooled, two-sided t-test)
```
n = (Z_α/2 + Z_β)² × 2σ² / Δ²
where σ² = (σ₁² + σ₂²) / 2
```

### ANOVA effect size (Cohen's f)
```
f = (Δ / σ_pooled) / √k
```

### Dropout adjustment
```
n_adjusted = max(ceil(n_raw / (1 − dropout/100)), n_raw + 1)
```
Guarantees at least +1 animal when dropout > 0.

### Bonferroni correction (ANOVA mode)
```
α_adjusted = α / (k × (k−1) / 2)
```

### Total animal count
```
Total = ceil((n_efficacy × k_arms + n_sampling × k_selected_arms) × n_models / take_rate)
```

### Z quantiles used
| Parameter | Z value |
|-----------|---------|
| α = 0.05 two-sided | 1.960 |
| α = 0.01 two-sided | 2.576 |
| Power = 80% | 0.842 |
| Power = 90% | 1.282 |
| Power = 95% | 1.645 |

---

## Tips

| Tip | Detail |
|-----|--------|
| **Live updates** | All outputs recalculate instantly — no Calculate button needed |
| **CV shortcut** | Enter CV% and click Apply if you only know the coefficient of variation |
| **Variability slider** | Drag to simulate higher PDX variability — SD bands widen on the design visual |
| **Minimum dropout** | Even 5% dropout on small groups is automatically rounded up to +1 animal |
| **ANOVA mode** | Switch test type to ANOVA to see how n scales with number of groups |
| **Copy justification** | One click copies the full IACUC paragraph to clipboard |
| **Budget** | Adjust prices to your institution — defaults are approximate US academic rates |
| **Take rate** | PDX typical range: 60–90% depending on model and passage number |
| **PD cohort** | These animals are euthanized early — they don't contribute to efficacy statistics |

---

## Browser Compatibility

| Browser | Status |
|---------|--------|
| Chrome / Edge | ✅ Recommended |
| Firefox | ✅ Supported |
| Safari | ✅ Supported |
| Mobile | ⚠️ Usable, sidebar may be cramped |

---

## License & Citation

Power Studio © 2026 Alexey Sorokin. All rights reserved.

If you use Power Studio for sample size justification in a publication, please acknowledge it in your methods section.
