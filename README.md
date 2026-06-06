# Power Studio

**Sample size & power analysis for preclinical oncology — in your browser.**

Power Studio computes statistically rigorous animal numbers for PDX (patient-derived xenograft) and other in-vivo efficacy studies, then writes the IACUC/ethics justification paragraph for you. It runs as a single HTML file — no installation, no server, and nothing leaves your machine.

> Companion to **TGI Studio** (PDX tumor growth analysis).

**© 2026 Alexey Sorokin**

---

## Table of Contents

1. [Quick Start](#quick-start)
2. [Calculation Modes](#calculation-modes)
3. [Study Designs](#study-designs)
4. [Statistical Engine](#statistical-engine)
5. [Inputs](#inputs)
6. [Outputs](#outputs)
7. [IACUC Justification](#iacuc-justification)
8. [Budget Estimator](#budget-estimator)
9. [Formulas](#formulas)
10. [Notes & Disclaimer](#notes--disclaimer)

---

## Quick Start

1. Download `Power_Studio.html`.
2. Open it in any modern browser (Chrome, Edge, Firefox, Safari).
3. Enter your group means, SDs, and desired power.
4. Everything updates **live** as you type.
5. Click **Copy** to grab the justification text for your protocol.

Nothing to install, nothing to configure.

---

## Calculation Modes

Pick what you want to solve for:

| Mode | Calculates | You provide |
|------|-----------|-------------|
| **Sample size** | n per group | means, SDs, α, power |
| **Power** | achieved power | means, SDs, α, n |
| **Minimum Δ** | smallest detectable difference | SDs, α, power, n |

---

## Study Designs

Power Studio supports the three standard hypothesis frameworks (two-group t-test):

| Design | Question | How it's powered |
|--------|----------|------------------|
| **Superiority** *(default)* | Is there a difference? | Two-sided (or one-sided) test on Δ = \|μ₁ − μ₂\|. |
| **Non-inferiority** | Is treatment not worse by more than a margin? | One-sided test; effect size = assumed difference + margin. |
| **Equivalence** | Do the groups differ by less than ± margin? | Two one-sided tests (TOST); effect size = margin − \|assumed difference\|. |

Selecting **Non-inferiority** or **Equivalence** reveals a **Margin** field (in the same units as your means). Equivalence correctly reports an infeasible design if the assumed difference exceeds the margin.

> Study design applies to two-group t-tests. In ANOVA mode the selector is hidden.

---

## Statistical Engine

Power Studio uses the **Student's t-distribution**, not the simpler normal (z) approximation. Because the t critical value depends on the degrees of freedom — which depend on n — the sample size is solved **iteratively**.

**Why this matters:** the z-only shortcut undersizes studies at the small group sizes typical in animal work. Power Studio's results match **G\*Power** exactly:

| Scenario (Δ=60, SD=50, α=0.05) | Power Studio | G\*Power |
|-------------------------------|:------------:|:--------:|
| Two-sided, 90% power | **16** / group | 16 |
| Two-sided, 80% power | **12** / group | 12 |
| Non-inferiority, margin 20, 80% power | **79** / group | ~79 |

**Variance assumption** is now honored correctly:
- **Welch (unpooled)** — keeps σ₁² and σ₂² separate (default; robust to unequal SDs).
- **Pooled** — assumes equal variances, uses the pooled form 2σ².

Numerical methods: standard-normal quantile/CDF (Beasley-Springer / Zelen-Severo) plus the regularized incomplete beta function for the t-distribution CDF, with the t-quantile obtained by bisection. All validated against published critical-value tables.

---

## Inputs

**Group parameters**
- Group 1 & 2 means and SDs.
- **CV% helper** — enter a coefficient of variation to back-calculate SD if you only have CV.

**Statistical parameters**
- **α** (significance level) and **power** (1 − β).
- **Test type:** two-sided / one-sided t-test, or one-way ANOVA (multi-arm).
- **Variance assumption:** Welch or pooled.
- **Study design:** superiority / non-inferiority / equivalence (+ margin).

**PDX variability sensitivity**
- A slider that inflates SD by a chosen CV% to show how the required n grows under more realistic xenograft variability — with a live "n increases from X to Y" readout and a note in the justification.

**Study logistics**
- **Dropout / attrition %** — inflates n, always rounding up by at least one animal (you can't lose a fraction of a mouse).
- **Number of arms** — drives Bonferroni correction in ANOVA mode and total-animal counts.
- **Models, engraftment/take rate, study name.**

---

## Outputs

- **Primary result** — the headline n, power, or minimum Δ for your mode.
- **A / B / C scenarios** — sample size at 50% / 80% / 95% power so you can see the trade-off.
- **α × power table** — n across a grid of α and power values.
- **Study design visual** — mean ± SD bands for both groups, with dashed overlays showing the inflated-variability scenario.
- **Power curve**, **sensitivity analysis**, and **SD heatmap** for exploring how assumptions drive n.
- **ANOVA card** (multi-arm mode) — Cohen's f and per-arm sizing.

---

## IACUC Justification

One click copies a complete, protocol-ready paragraph that states the test and design, α and power, SDs and Cohen's d (with effect-size label), Bonferroni correction (ANOVA), dropout allowance, PD/satellite animals, and the total-animal calculation across arms, models, and take rate. The text adapts automatically to your design type and margin.

---

## Budget Estimator

Translate the study into cost: enter price per animal, per-diem housing, study duration, supplies, and shipping, and Power Studio returns a per-arm and total estimate that tracks your current sample size — handy for grant and protocol budgets.

---

## Formulas

**Sample size (Welch / unpooled):**
```
n = (t_{α/2,df} + t_{β,df})² × (σ₁² + σ₂²) / Δ²,   df = 2(n − 1)
```

**Sample size (pooled):**
```
n = (t_{α/2,df} + t_{β,df})² × 2σ² / Δ²,   σ² = (σ₁² + σ₂²) / 2
```

**Non-inferiority:** effect = assumed difference + margin, one-sided α.
**Equivalence (TOST):** effect = margin − |assumed difference|, one-sided α each side.

**Bonferroni (ANOVA):** α divided by k(k−1)/2 pairwise comparisons.

**Dropout:** n_adjusted = ceil(n / (1 − dropout%/100)), minimum +1 animal.

Critical values t are solved iteratively because df depends on n.

---

## Notes & Disclaimer

- All computation is client-side; your inputs never leave the browser.
- Best viewed on desktop given the data-dense charts.
- Power Studio is a planning aid, validated against G\*Power for the supported designs. For regulatory submissions, confirm final numbers in a validated statistical package.

---

Power Studio © 2026 Alexey Sorokin.
