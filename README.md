# Oil Production Anomaly Detection Using XGBoost and EWMA

## Overview

This project focuses on monitoring oil production and identifying unusual changes in well performance using Machine Learning and statistical monitoring techniques.

An XGBoost regression model is used to predict expected oil production for well A01. The prediction errors are then analyzed using the Exponentially Weighted Moving Average (EWMA) method to identify unusual and persistent deviations over time.

## Dataset

The analysis uses daily production data with the following variables:

- `well_id`: Well identifier
- `date`: Observation date
- `actual_oil_bopd`: Actual oil production rate (barrels of oil per day)
- `pressure_psi`: Pressure measurement
- `water_cut_pct`: Water cut percentage

The current analysis focuses on a single well (A01).

## Methodology

### 1. Data Preparation

- Filter production data for well A01.
- Sort observations by date.
- Prepare the dataset for time-series analysis.
- Handle missing values in the selected features.

### 2. Feature Engineering

Create lagged features using previous observations:

- Previous oil production
- Previous pressure
- Previous water cut

These features are used to estimate expected oil production.

### 3. Oil Production Forecasting

An XGBoost regression model is used to predict oil production.

A walk-forward approach is applied, where the model is retrained using historical observations before each prediction.

The first 365 observations are used to initialize the forecasting process.

### 4. Prediction Error Analysis

Prediction errors are calculated as:

`Error = Actual Production - Predicted Production`

Negative errors indicate that actual production is lower than predicted production, while positive errors indicate higher-than-predicted production.

### 5. Calibration and Testing

The out-of-sample prediction results are divided into two periods:

- Calibration: First 180 observations
- Testing: Remaining observations

The calibration period is used to estimate the baseline error level and standard deviation.

### 6. EWMA Monitoring

The Exponentially Weighted Moving Average (EWMA) method is applied to prediction errors to smooth short-term fluctuations and monitor changes over time.

The EWMA is calculated as:

`EWMA(t) = λ × Error(t) + (1 - λ) × EWMA(t-1)`

The smoothing parameter is set to 0.2.

### 7. Anomaly Detection

Upper and Lower Control Limits (UCL and LCL) are calculated using the calibration error statistics.

Observations where EWMA exceeds these limits are flagged as potential anomalies.

These signals may indicate persistent deviations between actual and predicted oil production.

## Technologies

- Python
- Pandas
- NumPy
- XGBoost
- Matplotlib

## Project Limitations

- The current analysis focuses on one well (A01).
- Lagged features use the previous available row, which may not always represent the previous calendar day.
- The model is retrained at each prediction step, which can increase computational time.
- EWMA control limits rely on assumptions about the behavior of calibration errors.
- Detected anomalies do not independently identify their operational or reservoir-related causes.

## Future Improvements

- Extend the analysis to multiple wells.
- Compare EWMA with MAD and CUSUM.
- Evaluate model performance using appropriate forecasting metrics.
- Investigate detected anomalies using additional operational data.
- Improve control limits by evaluating the statistical properties of prediction errors.

## Disclaimer

This project is an educational and analytical study
