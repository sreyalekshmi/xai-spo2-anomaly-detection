# XAI for Unsupervised SpO2 Anomaly Detection

> **IEEE 2026** | Unsupervised anomaly detection in wearable SpO2 monitoring using
> Isolation Forest + SHAP-based explainability



---

## Overview

Traditional SpO2 monitors alert only when readings drop below 90% — missing subtle,
clinically significant desaturation patterns. This project presents an **unsupervised
anomaly detection framework** that:

- Detects anomalies **without labeled data** using Isolation Forest
- Captures temporal variability patterns (rolling mean, std, min, max, diff)
- Explains *why* a segment is anomalous using **SHAP (SHapley Additive Explanations)**
- Demonstrates that **signal variability** is more diagnostic than raw threshold values

Tested on the **OSV PhysioNet dataset** (36 patients, 3,600 readings/patient at 1 Hz).

---

## Key Results

| Metric | Value |
|--------|-------|
| Dataset | OSV PhysioNet (36 patients × 3,600 points) |
| Anomaly threshold tested | 1%, 3%, 5% contamination |
| Anomalies detected (patient 160217C) | 46 within 95–97% SpO2 range |
| Missed by static 90% threshold | All 46 (0% detection) |
| Top anomaly driver (SHAP) | `diff` — first-order rate of change |

---

## Methodology

```
Raw SpO2 signal
      ↓
Data Preprocessing (long format, time index, validity checks)
      ↓
Feature Engineering (rolling_mean, rolling_std, rolling_min, rolling_max, diff)
      ↓
Isolation Forest (unsupervised, no labels required)
      ↓
SHAP Explainability (TreeSHAP — beeswarm + waterfall plots per patient)
```

---



## Repository Structure

## 📁 Repository Structure

```plaintext
xai-spo2-anomaly-detection/
│
├── data/
│   └── raw/
│       ├── 3D_Patient_Oxygen_Data.csv
│       └── pattern-analysis-of-oxygen-saturation-variability-1.0.0/
│
├── notebooks/
│   └── anomaly_detection_spo2.ipynb
│
├── results/
│   ├── anamoly_score_patient_160217C.png
│   ├── beeswarm.png
│   ├── dist of spo2 reading.png
│   └── waterfall.png
│
├── .gitignore
├── LICENSE
└── README.md
```






## Results & Visualisations

### SHAP Beeswarm — Feature Importance
> The `diff` (rate of change) feature contributes most to anomaly detection,
> significantly outweighing raw SpO2 threshold values.

![SHAP Beeswarm](results/figures/shap_beeswarm.png)

### Anomaly Detection vs. Static Threshold
> 46 anomalous events detected within the clinically "normal" 95–97% range —
> none of which would be flagged by a standard 90% alert.

![Anomaly Detection](results/figures/anomaly_timeline.png)

---


## Authors

- **Achuth G** — Department of ECE, Amrita Vishwa Vidyapeetham
- **Sreya Lekshmi S** — Department of ECE, Amrita Vishwa Vidyapeetham
- **Sangeeth Kumar** — Center for Wireless Networks & Applications (WNA), Amrita

---

