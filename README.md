# TrAISformer: Maritime Trajectory Prediction

[![Status](https://img.shields.io/badge/Status-Complete-success)](https://github.com/pollyxinjei-byte/TrAISformer_Project)
[![Python](https://img.shields.io/badge/Python-3.12-blue)](https://www.python.org/)

## 📖 Project Overview

**TrAISformer** is a Transformer-based deep learning architecture designed to predict vessel trajectories in complex maritime environments. Unlike traditional regression models that often predict physically impossible paths (e.g., crossing land), this project implements a **classification approach** using "Four-hot" encoding (Latitude, Longitude, Speed, Course) to generate physically valid trajectory tokens.

This repository contains the full reproduction of the architecture, validated against the **Danish Maritime Authority (DMA)** dataset.

**Developer:** Polly Chen-Goujard  
**Course:** Machine Learning and Deep Learning (Master AI for Business, 2025)

## 🎯 Key Results

| Prediction Horizon | My Result (nmi) | Original Paper (nmi) | Status |
|--------------------|-----------------|----------------------|--------|
| 1 Hour | 0.48 | 0.48 | ✅ Exact Match |
| 2 Hours | 0.92 | 0.94 | ✅ Validated |
| 3 Hours | 1.51 | 1.64 | ✅ Validated |

## 📂 Repository Structure
```text
TrAISformer_Project/
├── report.pdf          # Full technical report
├── presentation.pdf    # Executive summary slides
├── logbook.pdf         # Experiment logbook
└── README.md
```

## 🚀 Quick Start

Run on Google Colab with T4 GPU:
```python
# Mount Google Drive
from google.colab import drive
drive.mount('/content/drive')

# Clone original TrAISformer repository
!git clone https://github.com/CIA-Oceanix/TrAISformer.git
%cd TrAISformer

# Fix Python 3.12 compatibility
!sed -i 's/.next()/.__next__()/g' trainers.py

# Save results to Google Drive
!sed -i 's|./results|/content/drive/MyDrive/TrAISformer_results|g' config_trAISformer.py
!mkdir -p /content/drive/MyDrive/TrAISformer_results

# Run training (~100 minutes)
!python trAISformer.py
```

## 🔧 Technical Details

| Component | Specification |
|-----------|---------------|
| Architecture | Transformer Decoder (GPT-style) |
| Encoding | Four-hot (Lat, Lon, SOG, COG) |
| Loss Function | Cross-Entropy |
| Optimizer | AdamW + Cyclic LR |
| Parameters | ~57.4 Million |
| Training | 50 epochs (~100 min on T4 GPU) |
| Best Epoch | 10 (Early Stopping) |

## 📊 Dataset

| Split | Trajectories |
|-------|--------------|
| Training | 9,144 |
| Validation | 1,291 |
| Test | 1,453 |

**Source:** [Danish Maritime Authority (DMA)](https://dma.dk/safety-at-sea/navigational-information/ais-data)

# 🤝 Contributor Expectations
This project was developed as an individual academic assignment.

* **Reporting Bugs:** Please use the GitHub Issues tab.
* **Pull Requests:** Ensure your code follows the existing style and includes comments explaining any architecture changes.

## ⚠️ Known Issues
During the reproduction process, the following limitations were observed:

* **Overfitting:** The model begins to overfit significantly after **Epoch 10**. While Training Loss continues to decrease, Validation Loss spikes (1.38 -> 3.91).
    * *Workaround:* Early Stopping is strictly applied at Epoch 10.
* **Python Compatibility:** The original codebase uses deprecated iterator syntax (`.next()`). This reproduction has patched it to `.__next__()` for Python 3.12 compatibility.

## ☕ Support
If you found this project helpful for your AIS research or coursework, please consider supporting it by:

* Giving this repository a **Star** ⭐️ on GitHub!
* Citing the original TrAISformer paper by Nguyen et al.
