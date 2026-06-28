# Drift-Aware Streaming Machine Learning Pipeline

A production-style streaming machine learning project that detects **concept drift** in non-stationary electricity market data and adapts the model automatically using ADWIN-based monitoring.

This project is designed for entry-level **Machine Learning Engineer**, **Applied ML**, and **Data Science** roles. It demonstrates online learning, streaming evaluation, concept drift detection, model adaptation, and experiment comparison.

---

## Project Overview

Traditional machine learning models assume that the relationship between features and labels remains stable over time. In real-world systems, this assumption often fails because user behavior, prices, market conditions, or external environments change. This problem is known as **concept drift**.

In this project, I built a streaming ML pipeline using the ELEC2 electricity dataset to:

- Train and evaluate a model one event at a time.
- Detect concept drift using ADWIN on the prediction error stream.
- Trigger automated model adaptation when drift is detected.
- Compare baseline, reset-only, and buffer-retraining strategies.
- Track final accuracy, drift count, and model stability.

---

## Dataset

**Dataset:** ELEC2 Electricity Market Dataset  
**Task:** Binary classification  
**Goal:** Predict electricity price movement in a streaming environment.

The dataset represents a non-stationary data stream where patterns can change over time, making it suitable for concept drift detection and online learning experiments.

---

## Methodology

The pipeline follows a **predict-then-learn** streaming evaluation approach:

1. Receive one data instance from the stream.
2. Predict the class label before training on the instance.
3. Compare prediction with the true label.
4. Send prediction error to ADWIN drift detector.
5. If drift is detected, trigger adaptation logic.
6. Train the model on the current instance.
7. Update online performance metrics.

---

## Experiment Versions

| Version | Description |
|---|---|
| Baseline | Hoeffding Tree model without drift detection or adaptation |
| Version 1 | ADWIN drift detection with model reset after drift |
| Version 2 | ADWIN drift detection with model reset and buffer retraining |

---

## Best Result

| Configuration | ADWIN Delta | Buffer Size | Final Accuracy | Total Drifts | Balanced Score |
|---|---:|---:|---:|---:|---:|
| Best Balanced | 0.001 | 100 | 0.8337 | 37 | 0.8263 |

Balanced Score was calculated as:

```text
Balanced Score = Final Accuracy - 0.00005 × Total Drifts
```

This helped select a configuration that balanced predictive performance with drift-detection stability.

---

## Tech Stack

- **Language:** Python
- **Streaming ML:** River
- **Drift Detection:** ADWIN
- **Model:** Hoeffding Tree Classifier
- **Evaluation:** Online Accuracy, drift count, balanced score
- **Experiment Tracking:** CSV result summaries

---

## Repository Structure

```text
.
├── README.md
├── requirements.txt
├── .gitignore
├── src/
│   ├── baseline.py
│   ├── adwin_reset.py
│   ├── adwin_buffer_retraining.py
│   └── utils.py
├── notebooks/
│   └── .gitkeep
├── results/
│   └── results_summary.csv
├── reports/
│   └── project_report.md
├── data/
│   ├── raw/
│   └── processed/
└── assets/
```

---

## How to Run

### 1. Clone the repository

```bash
git clone https://github.com/your-username/drift-aware-streaming-ml.git
cd drift-aware-streaming-ml
```

### 2. Create a virtual environment

```bash
python -m venv .venv
```

Windows:

```bash
.venv\Scripts\activate
```

Mac/Linux:

```bash
source .venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Run experiments

```bash
python src/baseline.py
python src/adwin_reset.py
python src/adwin_buffer_retraining.py
```

---

## Resume Bullet

Built a self-adaptive streaming ML pipeline using Python, River, ADWIN, and Hoeffding Tree to detect concept drift in the ELEC2 electricity dataset, achieving **83.37% final accuracy** while detecting **37 drift events** using an optimized ADWIN configuration.

---

## Future Improvements

- Add dashboard visualizations for drift points and accuracy over time.
- Add MLflow or Weights & Biases for experiment tracking.
- Compare additional online models such as Adaptive Random Forest.
- Add Docker support for reproducible execution.
- Deploy a Streamlit interface to visualize streaming predictions.
