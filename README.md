# Aggregation Operators for Composite Air Quality Indices

A research project exploring axiomatic aggregation operators, Power Means, OWA families, and their parametric extensions, for constructing composite environmental performance indices. Developed during an internship at the **International Laboratory of Game Theory and Decision Making**, HSE University - St. Petersburg.

## Overview

Standard air quality indices (e.g., Russia's IZA) typically rely on simple arithmetic averages, which mask critical pollutant peaks. This project:

1. **Formalises** seven families of aggregation operators (AM, GM, HM, Exponential Mean, Power OWA, Geometric OWA, Slide OWA, Window OWA) within a unified axiomatic framework.
2. **Proves** key properties — monotonicity, idempotency, continuity, boundary behaviour, and veto — analytically and verifies them numerically.
3. **Applies** the operators to real emission data from ~100 Russian cities (2007–2022) and compares them against the official IZA index.

## Simulation Notebook — `sim_fixed_v3.ipynb`

The notebook is the computational backbone of the project. It is structured in three layers:

### Layer 1 — Mathematical Primitives (Cells 1–8)

Self-contained implementations and unit tests for every aggregation family:

| Cell | Content |
|------|---------|
| 1 | Global seeds (`PRIMARY_SEED = 42`) and constants |
| 2 | Cost-to-performance affine transform; Power Mean validation |
| 3 | GM / HM — veto property and singularity handling |
| 4 | Exponential Mean — log-sum-exp stabilisation |
| 5 | Power OWA (POWA) — convergence to max |
| 6 | Geometric OWA (GOWA) — weight concentration |
| 7 | Slide OWA (SOWA) — linear pessimism interpolation |
| 8 | Window OWA (WOWA) — tail selectivity via RDM quantifier |

### Layer 2 — Data Pipeline (Cells 9–11)

- **Cell 9**: Load CSV, clean special codes (`-1`, `-9`, `-99`), min-max normalise emissions to `[0, 1]`, transform to performance scale.
- **Cell 10**: Compute Shannon entropy weights for the four pollutant indicators.
- **Cell 11**: Apply all aggregation methods with equal and entropy weights; compute Spearman / Kendall rank correlations.

### Layer 3 — Analysis and Validation (Cells 12–18)

- Rank divergence and distributional analysis
- Comparison with official IZA classification (1–4 scale)
- Random Forest classification to validate index discriminative power

### Running the Notebook

```bash
jupyter notebook simulations/sim_fixed_v3.ipynb
# or
jupyter lab simulations/sim_fixed_v3.ipynb
```

> **Important:** Cells must be executed top-to-bottom — later stages depend on variables defined earlier (`df_final`, weight vectors, etc.).

### Key Invariants

- `air_general_level` is the **validation target only** — never used as an input feature.
- The scale constant `M` is always a fixed parameter passed explicitly — never inferred from sample data.
- Entropy weights are computed in-sample; this is documented in the report.

## Dataset

**Source:** `data_air_cities_100_v20231129.csv` (https://tochno.st/datasets/air_cities) — semicolon-delimited, Russian-language headers.

**Feature columns** (four emission indicators):
- `air_solid_emissions` — solid particulate emissions
- `air_so_emissions` — sulphur oxide emissions
- `air_no_emissions` — nitrogen oxide emissions
- `air_co_emissions` — carbon monoxide emissions

**Validation label:** `air_general_level` — official IZA air quality level (1–4 scale).

## Key Results

- **Pessimism-sensitive operators** (GM, HM, SOWA, WOWA) detect high-pollution outliers that the arithmetic mean conceals.
- **Rank stability:** Spearman correlations between operator families exceed 0.95 for moderate pessimism, diverging only at extreme parameter values.
- **IZA comparison:** The proposed indices achieve higher classification accuracy than the official IZA formula, particularly for boundary cases between quality levels 2 and 3.

## Author

**Ivan D. Zamyatin**
BSc International Business and Economics, HSE University — St. Petersburg
Group 2201

## License

This repository accompanies an academic internship report. Please cite appropriately if reusing any part of the code or methodology.
