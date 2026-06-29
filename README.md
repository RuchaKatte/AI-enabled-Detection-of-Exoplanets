# 🪐 ExoLens — AI Exoplanet Detection Pipeline

**Bharatiya Antariksh Hackathon 2026 | Problem Statement: AI-enabled Detection of Exoplanets from Noisy Astronomical Light Curves**

## Overview

ExoLens is an end-to-end machine learning pipeline that automatically detects and classifies exoplanet transit signals from TESS (Transiting Exoplanet Survey Satellite) light curves. The pipeline mimics NASA's official exoplanet vetting workflow, replacing the manual review step with an ensemble AI classifier.

## Pipeline Stages

1. **Smart Data Ingestion** — Filters the xCTL catalog (280k stars) to the highest-priority planet-search targets and downloads TESS light curves via `lightkurve`
2. **Wotan Biweight Detrending** — Removes stellar variability and instrument systematics while preserving transit dip shapes
3. **TransitLeastSquares Period Search** — Uses a physically realistic limb-darkened transit model (superior to standard BLS) to detect periodic dips and compute SNR, SDE, and MES significance metrics
4. **False Positive Filtering** — Applies 6 discriminators including odd-even depth asymmetry, secondary eclipse check, and duration consistency to reject eclipsing binaries and blends
5. **RF + 1D-CNN Ensemble Classifier** — Classifies candidates into planet transit, eclipsing binary, blend, or noise using a weighted ensemble of a Random Forest (40%) and 1D Convolutional Neural Network (60%)
6. **Parameter Estimation** — Estimates orbital period, transit depth, and transit duration with confidence levels for all planet candidates

## Key Results

* Trained and validated on the Kepler labeled dataset (5,087 stars)
* Random Forest baseline: 87.3% accuracy, 88.3% ROC-AUC
* Ground truth validation: pipeline independently recovers known confirmed planets from TESS Sector 1
* Outputs ranked candidate table (CSV) + light curve plots (PNG) for each detected signal

## Tech Stack

| Component          | Tools                             |
| ------------------ | --------------------------------- |
| Data ingestion     | `lightkurve`, `astropy`, `pandas` |
| Detrending         | `wotan` (biweight filter)         |
| Period search      | `transitleastsquares`             |
| ML classifier      | `scikit-learn`, `PyTorch`         |
| Imbalance handling | `imbalanced-learn` (SMOTE)        |
| Visualization      | `matplotlib`, `seaborn`           |

## Dataset

* **Training:** Kepler labeled time series (Kaggle) — 5,087 stars, 37 confirmed planets
* **Inference:** TESS Sector 1 light curves via MAST archive
* **Target selection:** xCTL v08.01 exoplanet candidate target list

## Team

Built for BAH 2026 — Bharatiya Antariksh Hackathon by ISRO

## How to Run

Open `ExoLens.ipynb` in Google Colab and run all cells top to bottom. All dependencies are installed automatically in Step 0.
