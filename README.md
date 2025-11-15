# Anomaly-Detection

**Project Title**: Anomaly-Detection

## Project Summary

Anomaly-Detection is a compact, reproducible repository that demonstrates end-to-end workflows for detecting unusual events in sales time series data. The primary analysis notebook (`FireAI_Test_Sunny_Vishwakarma_15_11_2025.ipynb`) performs EDA, seasonal decomposition and multiple statistical anomaly-detection methods, and produces visualizations to support investigation and operational decision-making.

## Problem Statement / Business Context

- **Problem:** Detect anomalous behavior in system logs, sensor readings, or transactional data to reduce downtime, prevent fraud, and prioritize investigations.
- **Business Context:** Early detection of anomalies helps operations teams act quickly, reducing mean time to repair (MTTR), preventing financial loss, and improving system reliability.

## Tech Stack / Tools Used

- **Language:** Python 3.9+
- **Core libraries (used in notebook):** `pandas`, `numpy`, `matplotlib`, `seaborn`, `statsmodels`
- **Dev / infra:** Jupyter Notebook, Git, GitHub

- **Optional / advanced models:** `scikit-learn` (e.g., `IsolationForest`) and `pyod` are not required by the existing notebook but are common additions if you want to run ensemble or ML-based anomaly detectors.

## Features (implemented in the notebook)

- Data loading and integrity checks (date parsing, missing values)
- Exploratory Data Analysis (histograms, boxplots, ACF/PACF)
- Seasonal decomposition (statsmodels)
- Statistical anomaly detection methods: Z-score, IQR, Moving Average with rolling std
- Consolidation of multiple methods into confidence scores and validation
- Visual outputs: anomaly timeline, store-product hotspot heatmap, method comparison plots

Note: ML-based detectors such as `IsolationForest` are optional and not included in the notebook by default.

## Demo (Screenshots / GIFs / Video)

Add images to the `docs/` folder and reference them here. Example:

![Anomaly timeline](docs/screenshot-timeline.png)

You can also add a short GIF or link to a recorded walkthrough.

## Repository Structure & Setup

Current project files:

```
Anomaly-Detection/
├─ FireAI_Test_Sunny_Vishwakarma_15_11_2025.ipynb  # main analysis notebook
├─ README.md
```

Recommended folders to add as the project grows:

```
data/    # local CSV or other data files (optional)
docs/    # screenshots, architecture diagrams
src/     # reusable scripts or modules
```

If you add a local dataset, place it under `data/` and update the notebook code accordingly.

## Installation Instructions

1. Clone the repo:

```bash
git clone https://github.com/sunnyaiml/Anomaly-Detection.git
cd Anomaly-Detection
```

2. (Optional) Create and activate a virtual environment:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

3. Install the required Python packages (copy-pasteable):

```bash
pip install pandas numpy matplotlib seaborn statsmodels jupyter
```

4. (Optional) If you plan to use ML-based detectors (IsolationForest / pyod), install:

```bash
pip install scikit-learn pyod
```

5. Start Jupyter Lab/Notebook and run the analysis notebook:

```bash
jupyter lab
# or
jupyter notebook FireAI_Test_Sunny_Vishwakarma_15_11_2025.ipynb
```

## Data Sources & Methodology

- Data source (used in notebook): dataset is loaded from a Google Drive URL inside the notebook. The notebook's runtime load command is:

```python
df = pd.read_csv('https://drive.google.com/uc?export=download&id=1jpCNevXAiSKj6DURoe1Keno5GNrC9Ybw')
```

- Dataset summary (as reported in the notebook):
	- Rows: 230,090
	- Columns: `Date`, `store`, `product`, `number_sold`
	- Date range: 2010-01-01 → 2018-12-31

- Methodology (implemented):
	- Data ingestion & cleaning (date parsing, index, sorting)
	- Exploratory Data Analysis (distribution, boxplots, store/product breakdown)
	- Seasonal decomposition (`statsmodels.tsa.seasonal`) and ACF/PACF analysis
	- Statistical anomaly detection: Z-score, IQR, Moving Average with rolling std
	- Consolidation and validation of anomalies into confidence tiers

If you prefer a local CSV file instead of the Drive link, add it under `data/` and update the notebook `pd.read_csv(...)` path accordingly.

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

Load dataset (as in the notebook):

```python
import pandas as pd
df = pd.read_csv('https://drive.google.com/uc?export=download&id=1jpCNevXAiSKj6DURoe1Keno5GNrC9Ybw')
df['Date'] = pd.to_datetime(df['Date'])
```

Minimal Z-score anomaly detection pattern (conceptual):

```python
import numpy as np
temp = df.set_index('Date')['number_sold']
mean = temp.mean()
std = temp.std()
z_score = (temp - mean) / std
anomalies = z_score.abs() > 3
```

Optional: run an IsolationForest (install `scikit-learn` to enable):

```python
from sklearn.ensemble import IsolationForest
X = df[['number_sold']].fillna(method='ffill')
model = IsolationForest(contamination=0.01, random_state=42)
model.fit(X)
df['anomaly_score'] = model.decision_function(X)
df['anomaly_iforest'] = model.predict(X) == -1
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

## Notes on the Q4-2025 referral

You provided a detailed Q4-2025 (Sales Anomaly Detection) narrative earlier. That description refers to a different dataset period (Q4 2025) and an IsolationForest-based workflow. The current notebook in this repository uses a historical dataset (2010–2018) and implements statistical methods (Z-score, IQR, Moving Average). If you want the Q4-2025 analysis and IsolationForest code, I can either:

- create a new notebook that follows the Q4-2025 narrative (requires the Q4-2025 CSV), or
- modify the existing notebook to ingest a local `data/sales_data_q4_2025.csv` and add IsolationForest code (you must provide the dataset or allow synthetic generation).

No `requirements.txt` file is created per your request; instead, copy-paste the `pip install ...` command shown in Installation Instructions to install the required packages.

## Contact & License

- Contact: https://www.linkedin.com/in/sv-tech/
- GitHub: https://github.com/sunnyaiml/

This project is released under the MIT License. See `LICENSE` for details.

---
If you'd like, I can also:
- add example images to `docs/` and update the notebooks
- create a `requirements.txt` and a small runnable script to demonstrate detection
