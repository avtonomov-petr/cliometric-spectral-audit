# Cliometric Spectral Audit: Two-Echelon Diagnostics for Historical & Economic Matrices

[![Open In Colab](https://google.com)](https://google.com)
[![License: MIT](https://shields.io)](https://opensource.org)

A reproducible diagnostic pipeline for auditing empirical data matrices (N × P) in quantitative history and economics (cliometrics) prior to statistical modeling or dimensionality reduction.

---

## Overview & Motivation

Standard econometric and cliometric pipelines (such as *Z-score standardization + SVD/PCA*, Hodrick–Prescott filtering, or naive rolling correlations) frequently conflate **secular trends**, **nested regional duplicates**, and **localized single-entity shocks** ("lightning-rod" outliers) with genuine systemic phase transitions.

This repository implements a **Two-Echelon Spectral Audit Protocol** that separates true structural regime shifts from preprocessing artifacts:

1. **First Echelon (Linear Participation Diagnostics — *Bypass vs. Gated*):**
   * Jointly evaluates the normalized **Participation Ratio** across time (PR(u₁)) and across entities/regions (PR(v₁)) alongside bootstrap subsampling confidence intervals (80% without replacement).
   * Detects **trend masking** (PR(v₁) → 1 with low PR(u₁)), **temporal crisis localization** (\(\text{PR}(u_1) \ll 1\)), and **nested aggregate duplication**.
2. **Second Echelon (Non-Linear Spectral Graph Audit — \(L_{\text{sym}}\) + Leave-One-Out):**
   * Maps log-differences onto a k-nearest-neighbor similarity graph and extracts the Fiedler mode (u₂) of the symmetric normalized Laplacian \(L_{\text{sym}} = I - D^{-1/2} W D^{-1/2}\).
   * Combines blind change-point scanning (Mann–Whitney U and Levene variance tests) with mandatory **Leave-One-Out (LOO) feature stability checks** to reject false global phase transitions driven by a single peripheral outlier.

---

## Empirical Case Studies Included

The notebook `cliometric_spectral_audit.ipynb` reproduces three complete empirical benchmarks:

| Case Study | Dataset & Dimensions | First Echelon (`Bypass`) | Second Echelon (\(L_{\text{sym}}\) + LOO) | Key Diagnostic Finding |
| :--- | :--- | :--- | :--- | :--- |
| **1. Industrial Era GDP** | Maddison Project 2023 (1820–1950, 131 × 12) | PR(u₁) = 0.485<br>PR(v₁) = 0.998 | Apparent shift in 1891 (p = 1.6 × 10⁻⁶); collapses to p = 0.652 without **Peru** | Secular growth masks World Wars; single-country shock (Peru 1890) mimics a global phase transition unless filtered by LOO. |
| **2. European Wheat Markets** | Allen–Unger Prices (1650–1800, 151 × 25) | PR(u₁) = 0.155<br>PR(v₁) = 0.768 | PR(u₂) = 0.585 (3.8× increase in temporal uniformity) | First SVD mode is captured by the 1795–1800 crisis and nested regional duplicates (`England` / `Southern England`); \(L_{\text{sym}}\) recovers 18th-century climatic shocks (1709, 1740, 1789). |
| **3. Fugger Era & Micro-Integration** | Allen–Unger Barley (Augsburg, 1500–1700) | Rolling correlation spike (r = 0.78) in 1625 | Volatility plateau (1500–1557) vs. Thirty Years' War shock | Financial hegemony appears as price stability (1500–1557), whereas high rolling correlation during the 1620s reflects synchronized wartime famine rather than market integration. |

---

## Repository Structure

```text
cliometric-spectral-audit/
├── cliometric_spectral_audit.ipynb   # Main reproducible Google Colab / Jupyter pipeline
├── data/                             # Primary archival datasets (Maddison 2023 & Allen–Unger)
│   ├── mpd2023_web.xlsx
│   └── doi-10.34894-ipvxmw.zip
├── figures/                          # Generated high-resolution diagnostic plots & summary CSV
│   ├── case1_maddison_1820_1950.png
│   ├── case2_allen_unger_wheat_1650_1800.png
│   ├── case3_augsburg_1500_1700.png
│   └── audit_summary_table.csv
└── README.md
```

---

## Quick Start & Data Sources

All required archival datasets are included in the `data/` folder for one-click reproducibility in Google Colab. Original data sources:

* **Maddison Project Database 2023:** `mpd2023_web.xlsx` from the [Groningen Growth and Development Centre (GGDC)](https://rug.nl).
* **Allen–Unger Global Commodity Prices:** `doi-10.34894-ipvxmw.zip` from the [IISH Dataverse Repository](https://iisg.amsterdam) (`DOI: 10.34894/IPVXMW`).

To run the analysis:
1. Click the **Open in Colab** badge at the top of this page.
2. Select **Runtime → Run all** to execute all five diagnostic cells and generate `results_bundle.zip`.

---

## Author & Citation

**Petr Avtonomov**  
Independent Researcher, Theoretical Physicist & Scientific Auditor (St. Petersburg, Russia)  
ORCID: [0009-0003-2816-6782](https://orcid.org)

