# Anomaly-Detection

**Project Title**: Anomaly-Detection

## Project Summary

Anomaly-Detection is a compact, reproducible repository that demonstrates end-to-end workflows for detecting unusual events in time series and tabular datasets. The project includes data processing, modeling (unsupervised and semi-supervised), evaluation, and visualizations to support actionable insights for operations and monitoring use cases.

## Problem Statement / Business Context

- **Problem:** Detect anomalous behavior in system logs, sensor readings, or transactional data to reduce downtime, prevent fraud, and prioritize investigations.
- **Business Context:** Early detection of anomalies helps operations teams act quickly, reducing mean time to repair (MTTR), preventing financial loss, and improving system reliability.

## Tech Stack / Tools Used

- **Language:** Python 3.9+
- **Core libraries:** pandas, numpy, scikit-learn, matplotlib, seaborn
- **Anomaly libraries (examples):** pyod, isolation-forest (sklearn), prophet for seasonal decomposition
- **Dev / infra:** Jupyter Notebook, Git, GitHub

## Features

- Preprocessing pipelines for time series and tabular data
- Unsupervised models (Isolation Forest, One-Class SVM, LOF)
- Semi-supervised and statistical baselines (Z-score, rolling statistics)
- Visual dashboards for anomaly timelines and distributions
- Evaluation metrics and comparison reports

## Demo (Screenshots / GIFs / Video)

Add images to the `docs/` folder and reference them here. Example:

![Anomaly timeline](docs/screenshot-timeline.png)

You can also add a short GIF or link to a recorded walkthrough.

## Repository Structure & Setup

Suggested structure:

```
Anomaly-Detection/
├─ FireAI_Test_Sunny_Vishwakarma_15_11_2025.ipynb
├─ README.md
├─ data/                  # (optional) raw and processed data
├─ notebooks/             # exploratory notebooks
├─ src/                   # scripts and modules
├─ docs/                  # screenshots, diagrams
└─ requirements.txt       # python dependencies
```

## Installation Instructions

1. Clone the repo:

```bash
git clone https://github.com/sunnyaiml/Anomaly-Detection.git
cd Anomaly-Detection
```

2. (Optional) Create virtual environment and install dependencies:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

3. Start the notebook:

```bash
jupyter lab
```

If `requirements.txt` is not present, install core packages manually:

```bash
pip install pandas numpy scikit-learn matplotlib seaborn pyod jupyter
```

## Data Sources & Methodology

- Example public datasets you can try: NAB (Numenta Anomaly Benchmark), Yahoo Webscope S5, KDD Cup variations.
- Methodology:
	- Data ingestion & cleaning (resample, impute, smoothing)
	- Feature engineering (lags, rolling stats, seasonal decomposition)
	- Train unsupervised detectors and tune contamination/thresholds
	- Evaluate using precision/recall, F1, ROC (where labels exist) and time-to-detect metrics

## Key Visualizations

- Time series with anomaly overlays (timestamps highlighted)
- Distribution plots of anomaly scores
- Confusion matrix (when labels available)
- ROC / Precision-Recall curves

## Model Performance / Metrics

- When labels exist: precision, recall, F1-score, AUC-ROC
- When unlabeled: use Manually-validated precision, detection latency, and score stability
- Example table format:

| Model | Precision | Recall | F1 | Notes |
|-------|-----------:|-------:|---:|-------|
| IsolationForest | 0.84 | 0.72 | 0.77 | robust to noise |

## Key Code Snippets

Minimal example: run Isolation Forest on a single feature

```python
import pandas as pd
from sklearn.ensemble import IsolationForest

df = pd.read_csv('data/sample.csv', parse_dates=['timestamp'])
X = df[['value']].fillna(method='ffill')
model = IsolationForest(contamination=0.01, random_state=42)
model.fit(X)
df['anomaly_score'] = model.decision_function(X)
df['anomaly'] = model.predict(X) == -1
```

## Architecture Diagram / Workflow

Place an architecture image at `docs/architecture.png` or add a diagram here. Typical workflow:

- Data ingestion -> preprocessing -> feature extraction -> model training -> scoring -> evaluation -> visualization & alerts

## Key Findings & Actionable Insights

- Example finding: periodic spikes at 02:00 daily correspond to batch job — investigate job duration and resource usage.
- Actionable insight: set alert thresholds on aggregated anomaly counts per hour to reduce false positives.

## Challenges Faced & How They Were Solved

- Challenge: Seasonal patterns causing false positives. Solution: use seasonal decomposition / detrending before scoring.
- Challenge: Rare labels. Solution: use manual labeling + active learning and tune thresholds conservatively.

## Future Improvements

- Add streaming / online detection (Kafka + incremental models)
- Build a dashboard (Streamlit / Grafana) for live monitoring and investigation
- Add more detectors and ensemble voting

## FAQs

- Q: Do I need labeled data? A: No — many detectors work unsupervised, but labels help evaluation and thresholding.
- Q: Can I run this in production? A: The notebooks are a prototype; wrap preprocessing and models into services for production.

## Contributors / Acknowledgements

- Repository owner: `sunnyaiml` — https://github.com/sunnyaiml/
- Connect: https://www.linkedin.com/in/sv-tech/

## Contact & License

- Contact: https://www.linkedin.com/in/sv-tech/
- GitHub: https://github.com/sunnyaiml/

This project is released under the MIT License. See `LICENSE` for details.

---
If you'd like, I can also:
- add example images to `docs/` and update the notebooks
- create a `requirements.txt` and a small runnable script to demonstrate detection
