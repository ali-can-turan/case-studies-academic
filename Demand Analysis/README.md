# Demand Forecasting Showcase: MS Excel (OLS) vs. Python (Lasso) Regression

> **Executive Summary:** This project evaluates a 24-month demand dataset to forecast the subsequent 3 months using two distinct approaches: **MS Excel Data Analysis Toolpak (OLS)** and **Python Machine Learning (`scikit-learn` Lasso & `statsmodels`)**. Models are benchmarked on both training and test sets using $R^2$, MAE, and WMAPE metrics to compare traditional linear regression with regularized machine learning.

<img width="1285" height="546" alt="demand" src="https://github.com/user-attachments/assets/e772a15e-4e94-4db4-a16a-f90dd8856db7" />

## Model Architecture & Comparison

| Benchmark Feature | MS Excel (OLS) | Python Machine Learning (Lasso) |
| :--- | :--- | :--- |
| **Tool / Engine** | Data Analysis Toolpak | `scikit-learn`, `statsmodels` |
| **Model Type** | Ordinary Least Squares (OLS) | L1 Regularized Linear Regression (Lasso) |
| **Variable Selection** | Manual / All variables retained | Automated via L1 penalty (feature drop-off) |
| **Continuous Variables** | 2: Time Index ($t$) + Price | 2: Time Index ($t$) + Price |
| **Categorical Variables** | 10: Month Dummies (12-month cycle) | 11: Month Dummies + Seasonal Peak/Valley Indicator |
| **Data Split** | Train: 24 months / Test: 3 months | Train: 24 months / Test: 3 months |
| **Train Performance** | Strong fit ($R^2$, MAE) | Strong fit ($R^2$, MAE) |
| **Test Performance** | High metric variance ($n=3$) | High metric variance ($n=3$) |

<img width="1278" height="500" alt="demand3" src="https://github.com/user-attachments/assets/13d396a7-9d4d-4140-8c6a-92a3d510874e" />

## Results & Model Evaluation

<img width="1306" height="717" alt="demand2" src="https://github.com/user-attachments/assets/880f51f7-b68c-4510-b504-011efb54d2f1" />

---

## Strategic Recommendations & High-Frequency Seasonality

For monthly data, categorical dummy variables effectively capture seasonal shifts. However, for **high-frequency demand data (daily or weekly)**, adding 52 or 365 dummy variables creates extreme dimensionality and severe overfitting risk.

Though they cannot explain the sharp peaks and troughs in demand in full efficiency due to their smooth responsiveness; to address high-cardinality seasonality efficiently without inflating model parameters, **Fourier Terms (Sin/Cos Harmonic Transformations)** are recommended:

$$
x_{\text{sin}} = \sin\left(\frac{2\pi \cdot k \cdot t}{P}\right) \quad \text{and} \quad x_{\text{cos}} = \cos\left(\frac{2\pi \cdot k \cdot t}{P}\right)
$$

* **$P$**: Length of the seasonal period (e.g., $P=12$ for monthly, $P=52$ for weekly, $P=365$ for daily).
* **$t$**: Sequential time index.
* **$k$**: Harmonic order.

*By replacing dozens of dummy variables with just 4 to 8 Fourier terms, models preserve degrees of freedom while smoothly capturing complex cyclical patterns.*

