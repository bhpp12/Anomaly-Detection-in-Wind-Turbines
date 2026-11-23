# Anomaly Detection in Wind Turbines

## About the Project

This project was developed for the **Data Mining course at the Federal University of Ceará (UFC)**, with the goal of identifying anomalies in wind turbine telemetry data.

The objective is to analyze turbine operational data, identify unusual behavior, and select a Machine Learning method capable of generating a maximum of 300 potential anomaly alerts.

The project was developed using **Python in Google Colab**, applying exploratory data analysis, statistical methods, and unsupervised anomaly detection algorithms.

## Technologies and Libraries

- Python
- Google Colab
- Pandas and NumPy
- Matplotlib
- Scikit-learn
- SciPy

## Dataset

The dataset contains telemetry information from wind turbines, including:

| Variable | Description |
|---|---|
| `id` | Record identifier. |
| `timestamp` | Date and time of the reading. |
| `turbina` | Turbine identifier. |
| `vento_ms` | Wind speed (m/s). |
| `potencia_kw` | Generated power (kW). |
| `rotor_rpm` | Rotor rotational speed. |
| `temp_caixa_engrenagem_c` | Gearbox temperature (°C). |
| `vibracao_mm_s` | Vibration (mm/s). |
| `temp_ambiente_c` | Ambient temperature (°C). |

## Development Process

### 1. Exploratory Data Analysis (EDA)

Initially, an exploratory analysis of the telemetry variables was performed using scatter plots and time series to investigate relationships between wind speed, generated power, and component temperatures.

### 2. Statistical Anomaly Detection

Two statistical methods were used as baselines:

- **Z-Score:** Identifies observations with values significantly distant from the mean, considering the standard deviation.
- **IQR (Interquartile Range):** Identifies values outside the boundaries defined by the distribution quartiles.

Results:

- Z-Score: 87 alerts.
- IQR: 100 alerts.

These methods have limitations when identifying contextual and multivariate anomalies because they analyze individual variable values independently.

### 3. Feature Engineering

Two new features were created to improve the identification of abnormal behavior:

- `vento_std_1h`: Standard deviation of wind speed within a one-hour rolling window, considering six readings.
- `vib_congelada`: Identifies potential frozen vibration sensors by detecting constant values over a one-hour period.

Data normalization was also performed using `StandardScaler`, preparing the features for the anomaly detection algorithms.

### 4. Machine Learning Models

Four unsupervised learning algorithms were implemented:

**Isolation Forest**

Identifies anomalies through isolation processes, assigning higher anomaly scores to observations considered more unusual.

**k-Nearest Neighbors (k-NN)**

Uses the average distance between each observation and its 10 nearest neighbors to identify isolated records.

**Local Outlier Factor (LOF)**

Analyzes the local density of observations compared to their neighbors to identify potential anomalies.

**One-Class SVM**

Uses a decision function to identify observations that deviate from the expected normal behavior.

Each algorithm was used to select the 300 observations with the highest anomaly scores.

### 5. Results Comparison

All four models generated 300 alerts each.

The intersections between the alert sets were also analyzed to identify records simultaneously flagged by different algorithms.

| Comparison | Common Alerts |
|---|---:|
| Isolation Forest and LOF | 99 |
| Isolation Forest and k-NN | 132 |
| Isolation Forest and One-Class SVM | 160 |
| LOF and k-NN | 152 |

### 6. Anomaly Diagnosis

An analysis was conducted on the three most critical alerts identified by Isolation Forest.

The records presented characteristics such as:

- Wind speed ranging from 2.5 to 2.8 m/s.
- Generated power ranging from 0 to 53 kW.
- Rotor movement despite low wind speed.
- Gearbox temperature above 40 °C.

This behavior may indicate a contextual anomaly related to gearbox overheating under low power generation conditions, raising hypotheses such as cooling system failures or mechanical friction.

## Production Decision

**Selected Method: Isolation Forest.**

The selection was based on three main factors:

1. **Alert Limit:** Allows prioritization of the 300 most anomalous observations, respecting the available investigation capacity.
2. **Computational Performance:** Offers good scalability compared to methods based on distance calculations between observations.
3. **Multivariate Detection:** Can identify unusual combinations of features, including contextual anomalies that univariate statistical methods may fail to detect.

## Results Export

Finally, the 300 observations selected by Isolation Forest are exported to a CSV file containing the identifiers of the flagged records:

`alertas_equipe_bhpp.csv`

The three most critical alerts are also displayed to support further investigation and diagnosis.

## Author

**Bruno Henrique Pacheco Prudêncio**

Data Science Student — Federal University of Ceará (UFC).

[GitHub](https://github.com/bhpp12)
