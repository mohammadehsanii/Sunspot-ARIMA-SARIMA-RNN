# **Sunspot Activity Forecasting**

## **Introduction**
This project focuses on analyzing and forecasting sunspot activity from 1749 to 2019 using a mix of statistical and machine learning methodologies. The primary goals include visualizing time-series data, evaluating stationarity, and applying models like ARIMA, SARIMA, and Recurrent Neural Networks (RNNs) to predict sunspot counts effectively.

---

## **1. Data Preparation**
**Dataset**: Monthly mean total sunspot numbers.

**Steps Performed**:
1. **Datetime Conversion**: Converted the 'Date' column to datetime format and set it as the index.
2. **Handling Missing Values**: Verified no missing values in the dataset.
3. **Visualization**: Plotted a time-series graph to observe trends and periodic patterns.
4. **Split**: Divided the data into training (80%) and validation (20%) sets.

---

## **2. Exploratory Analysis**
- **Stationarity Testing**:
  - **ADF Test**: p-value near 0, confirming stationarity.
  - **KPSS Test**: Indicated stationarity with a p-value of 0.1.
- **Seasonality**: Decomposition analysis revealed an 11-year periodic cycle, consistent with known sunspot trends.

---

## **3. Statistical Models**

### **3.1. ARMA Model**
- **Order Selection**: Based on ACF and PACF plots, initial values were set as (p=1, d=0, q=2).
- **Performance**:
  - MSE: High
  - MAE: High
- **Observation**: The ARMA model struggled with capturing seasonality, leading to suboptimal performance.

### **3.2. SARIMA Model**
- **Order Selection**:
  - Seasonal Order: (P=1, D=1, Q=1, S=132), reflecting the 11-year cycle.
- **Performance**:
  - MSE: Significantly lower than ARMA.
  - MAE: Lower, indicating better predictive accuracy.
- **Visualization**: The SARIMA model effectively captured both trend and seasonality.

---

## **4. Machine Learning Approach**

### **Recurrent Neural Networks (RNNs)**
**Data Preparation**:
- **Normalization**: Scaled data using MinMaxScaler.
- **Windowing**: Created input-output pairs with a window size of 64 months.

**Model Architecture**:
- **Layers**:
  - Two LSTM layers with 50 units each.
  - Dense layer for output.
- **Optimizer**: Adam
- **Loss Function**: Mean Squared Error (MSE)

**Training**:
- **Epochs**: 100 (with early stopping).
- **Learning Rate Scheduler**: Dynamically adjusted learning rate.

**Performance**:
- **Validation Loss**: Moderate, indicating reasonable generalization.
- **Predictions**: Successfully captured short-term variations but struggled with long-term seasonality compared to SARIMA.

**Visualization**:
- Predicted sunspot counts closely matched actual data with minor deviations.

---

## **5. Model Comparison**

| Model   | MSE   | MAE   | Strengths                       | Weaknesses                    |
|---------|-------|-------|----------------------------------|--------------------------------|
| ARMA    | High  | High  | Simple and interpretable        | Fails to model seasonality    |
| SARIMA  | Low   | Low   | Captures seasonality and trends | Requires parameter tuning     |
| RNN     | Moderate | Moderate | Handles non-linear patterns      | Computationally intensive     |

---

## **6. Conclusion**
- **Best Performing Model**: SARIMA provided the most accurate forecasts, effectively modeling seasonality and trends.
- **RNN Performance**: While promising for short-term predictions, further optimization is needed for long-term accuracy.
- **Future Directions**:
  - Explore hybrid models combining SARIMA for seasonal trends and RNNs for short-term variations.
  - Enhance RNNs with GRU layers or attention mechanisms.
  - Integrate additional exogenous variables to improve model robustness.

---

## **Appendix**
- **Code Efficiency**: Modularize the code to improve readability and reusability.
- **Data Sources**: Ensure comprehensive documentation of data preprocessing and lineage for reproducibility.

---
