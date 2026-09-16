# Eniac Webpage Button A/B Test — Chi-Square Analysis

Hypothesis-testing case study on a 4-way A/B test of the Eniac homepage's primary call-to-action button. Uses a chi-square test of independence, followed by post-hoc pairwise testing with a Bonferroni correction, to determine which button version drives the highest click-through rate (CTR).

## Business Question

Eniac tested four homepage variants that differ in call-to-action copy and color:

| Version | Button copy | Color |
|---|---|---|
| A | SHOP NOW | White |
| B | SHOP NOW | Red |
| C | SEE DEALS | White |
| D | SEE DEALS | Red |

Each variant was shown to a separate group of visitors over a comparable ~14-day window. The question: **does CTR differ significantly across the four versions, and if so, which version should Eniac ship?**

## Data

Each `eniac_[a|b|c|d].csv` file is a click-tracking export for one homepage variant, listing every tracked page element (buttons, links, images, form fields) with its click count and visibility. The dataset used for this analysis is the single row per file for the primary CTA element (`SHOP NOW` for A/B, `SEE DEALS` for C/D), plus the total visit count embedded in the `Snapshot information` field of each file.

| Version | Visits | CTA clicks | CTR |
|---|---:|---:|---:|
| A | 25,326 | 512 | 2.02% |
| B | 24,747 | 281 | 1.14% |
| C | 24,876 | 527 | 2.12% |
| D | 25,233 | 193 | 0.76% |

## Method

1. **Hypotheses** — H₀: all four versions have the same CTR. H₁: at least one version differs.
2. **Significance level** — α = 0.05.
3. **Omnibus test** — a chi-square test of independence on the 2×4 contingency table (click / no-click × version A–D).
4. **Post-hoc testing** — since a significant omnibus result only shows that *some* version differs, not *which*, every pairwise combination (6 total) is re-tested with its own chi-square test, using a Bonferroni-corrected alpha (0.05 / 6) to control the family-wise error rate.

## Results

- Chi-square statistic: **224.02** (df = 3), p ≈ 2.72 × 10⁻⁴⁸ — **H₀ is rejected**. CTR is not the same across all four versions.
- Post-hoc pairwise tests: every pair is significantly different **except A vs. C**, which are statistically tied.

| Comparison | p-value | Significant (α = 0.0083) |
|---|---:|:---:|
| A vs B | 2.67 × 10⁻¹⁵ | Yes |
| A vs C | 0.465 | No |
| A vs D | 3.08 × 10⁻³³ | Yes |
| B vs C | 6.96 × 10⁻¹⁸ | Yes |
| B vs D | 2.36 × 10⁻⁵ | Yes |
| C vs D | 6.45 × 10⁻³⁷ | Yes |

## Conclusion

Versions **A** and **C** are statistically tied for the highest CTR and both significantly outperform B and D. Version **D** performs the worst of all four. Since A and C perform equivalently, Eniac can choose between "SHOP NOW" and "SEE DEALS" copy on business/branding grounds — but should avoid the red button variants (B and D) used with either copy.

## Repo Contents

```
data/
  eniac_a.csv              click-tracking export, Version A (white, SHOP NOW)
  eniac_b.csv              click-tracking export, Version B (red, SHOP NOW)
  eniac_c.csv              click-tracking export, Version C (white, SEE DEALS)
  eniac_d.csv              click-tracking export, Version D (red, SEE DEALS)
eniac_case_analysis.ipynb  full analysis notebook
```

## Tools

Python · pandas · SciPy (`chi2_contingency`) · Matplotlib

## How to Run

```
pip install pandas numpy scipy matplotlib jupyter
jupyter notebook eniac_case_analysis.ipynb
```
