# 🌞 **Sunspot Forecasting Project Report**

<sub> _To view the plots and charts, please visit my [Kaggle Notebook](https://www.kaggle.com/code/mohammadehsani/sunspot-with-arima-sarima-and-rnns)._ </sub>

### 📌 **Objective**
To analyze historical sunspot data from 1749 to 2019 and forecast future values using time series models—**ARMA, SARIMA**, and a **Recurrent Neural Network (RNN)** using LSTM architecture.

---

## 📚 **1. Data Overview & Preparation**

**Source:** Sunspots.csv  
**Period Covered:** January 1749 – December 2019  
**Features:**
- Date
- Monthly Mean Total Sunspot Number

**Steps Taken:**
- Loaded the dataset using `pandas` and `csv`.
- Converted `Date` to datetime format and set it as index.
- Removed unnecessary columns (`Unnamed: 0`).
- Checked for missing values – ✅ *None found*.

---

## 📈 **2. Data Visualization**

To capture the long-term trend, the x-axis was converted to represent **collective months** since January 1749.

A full series plot was created to understand:
- Seasonal cycles (~11 years),
- Amplitude fluctuations,
- Periodic bursts in solar activity.

---

## 🧹 **3. Pre-Processing**

### 📊 Train-Test Split:
- **Train:** 80% of the dataset
- **Validation:** 20%

Both segments were visualized to ensure temporal separation.

---

## 🔍 **4. Stationarity Check**

Using **ADF** and **KPSS** tests:

| Test     | p-value   | Interpretation                   |
|----------|-----------|----------------------------------|
| ADF      | 3.03e-15  | Reject H₀ → Stationary ✅       |
| KPSS     | 0.1       | Fail to Reject H₀ → Stationary ✅|

**Conclusion:** The series is stationary.

---

## 🧠 **5. Forecasting Models**

---

### 📉 **Model 1: ARMA (ARIMA with d=0)**

**Configuration:** ARIMA(p=1, d=0, q=2)

**Performance:**
- **MSE:** 4901.65
- **MAE:** 58.75

❌ *Model failed to capture seasonality* → **Suboptimal forecasts**

---

### 📉 **Model 2: SARIMA (Seasonal ARIMA)**

**Parameters:**
- ARIMA order: (1, 0, 1)
- Seasonal order: (1, 1, 1, 132) → *11-year cycle = 132 months*

**Seasonal Decomposition:** Clearly identified trend and strong cyclic behavior.

**Performance:**
- **MSE:** 3228.30
- **MAE:** 45.09

✅ *Much better than ARMA due to its ability to model seasonality.*

---

## 🔁 **6. Deep Learning Approach: RNN (LSTM)**

---

### 🔧 **Data Preparation:**
- **Window Size:** 64 (5 years and 4 months worth of monthly data)
- **Normalization:** Using `MinMaxScaler`
- **Sequence Generation:** Sliding window approach to build `(X, y)` pairs for supervised learning.

---

### 🧪 **Model Architecture:**

| Layer     | Description                                |
|-----------|--------------------------------------------|
| LSTM #1   | 50 units, returns sequences                |
| LSTM #2   | 50 units                                   |
| Dense     | Output layer with 1 unit                   |

**Optimizer:** Adam  
**Loss Function:** Mean Squared Error  
**EarlyStopping:** Patience = 10  
**LearningRateScheduler:** Exponential decay after 10 epochs

---

### 📉 **Training & Evaluation**

- **Training Epochs:** 62
- **Final Validation Loss:** 0.00315

✅ The model showed strong learning dynamics, with gradual convergence.

---

### 📊 **Results Visualization**

Predictions from the RNN were **denormalized** and plotted alongside the actual sunspot values.

**Outcome:**
- The RNN successfully captured the **wave-like** seasonal trend.
- Smooth and accurate long-term predictions.

---

## 📊 **7. Model Comparison Summary**

| Model   | MSE     | MAE     | Comments                             |
|---------|---------|---------|--------------------------------------|
| ARMA    | 4901.65 | 58.75   | Fails to capture seasonality         |
| SARIMA  | 3228.30 | 45.09   | Good balance between trend & season  |
| RNN     | ~Low (0.003 val loss) | N/A | Best visual fit and trend capture ✅|

---

## ✅ **Conclusion**

- **RNN with LSTM** demonstrated superior forecasting capability in modeling **nonlinear**, **seasonal** time series like sunspot activity.
- **SARIMA** offers a solid statistical baseline and is easier to interpret.
- **ARMA** is not well-suited for this cyclical, seasonal data.
