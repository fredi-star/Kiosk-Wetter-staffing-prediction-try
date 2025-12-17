# 🌤️ Kiosk Staffing Optimization: Weather-Driven Sales Prediction

### ⚠️ Project Context (Vibe Check)
**This is a reconstruction.**
I originally built this project for a live business environment (i was really bored at my store Job, when i was 16). Due to **GDPR restrictions** and loss of the original source files, I cannot publish the production repo.

* **The Logic:** This repo contains a complete re-implementation of the logic I used.
* **The Data:** The dataset here is **synthetic**. I wrote a generator to create dummy data that mimics the statistical distribution and correlations of the original private dataset. This allows you to run the code immediately.
* **The Evidence:** The regression plot screenshot in the "Results" section is the only surviving artifact from the original analysis.

---

## 🎯 The Business Goal
Staffing a high-traffic kiosk is a balancing act.
* **Understaffing** on a sunny day = Long queues, lost revenue, bad customer experience.
* **Overstaffing** on a rainy Tuesday = Burned wages and idle employees.

The goal was to replace "gut feeling" scheduling with a data-driven model. We needed a system to flag specific days where sales would deviate significantly from the weekly trend—specifically driven by weather changes—to trigger an "extra staff" shift.

## ⚙️ Methodology

The project followed a pragmatic data science lifecycle, prioritizing interpretability for non-technical stakeholders.

### 1. Data Cleaning & Anomaly Detection
Real-world sales data is noisy. Before modeling, I applied strict filters:
* **Minimum Operations:** Excluded days with sales < €300 (e.g., half-days or technical issues).
* **Local Holidays:** Holidays in Hamburg create non-standard sales patterns, so they were excluded to avoid skewing the trend.
* **Statistical Anomalies:** Removed days deviating >70% from the 7-day rolling average to filter out unrecorded local events.

### 2. Feature Engineering
Raw temperature and raw sales figures weren't enough. I engineered features to capture **change** rather than absolute values:
* **`residual`:** The difference between today's sales and the *lagged* 7-day moving average. This isolates how much the current day outperformed the recent trend.
* **`temp_change`:** The difference in temperature compared to yesterday. (Insight: A jump from 15°C to 20°C drives more impulse buys than a steady 20°C week).

### 3. Modeling
I utilized **Linear Regression (OLS)** via `statsmodels`.
* *Why?* Interpretability. Store managers need to understand *why* the model predicts a busy day ("Because the temperature jumped 5 degrees"). Black-box models were not suitable for this stakeholder group.

### 4. Backtesting & Threshold Optimization
The model outputs a continuous prediction, but the business decision is binary: *Call extra staff (Yes/No).*
I simulated a backtest on historical data to find the optimal revenue threshold.
* **The Trade-off:** We analyzed Precision (avoiding unnecessary wage costs) vs. Recall (capturing all busy days).
* **Result:** A static threshold based on average sales resulted in zero Recall. We adjusted the sensitivity to capture the revenue upside, accepting a manageable false-positive rate.

## 📊 Results

The analysis proved a strong positive correlation between sudden temperature increases and sales spikes that outperformed the weekly trend.

**Original Regression Analysis:**
*(This screenshot is the original artifact from the live project)*

![Regression Plot](path/to/your/original_screenshot.png)

*Note: The Python notebook in this repo generates a similar plot using the synthetic data to demonstrate the visualization code.*

## 🛠️ Tech Stack
* **Python 3.9+**
* **Pandas:** Data manipulation, rolling window calculations, and time-series handling.
* **Statsmodels:** OLS regression and statistical summaries.
* **Matplotlib/Seaborn:** Visualization.
* **Holidays:** Handling regional public holidays (Germany/Hamburg).

