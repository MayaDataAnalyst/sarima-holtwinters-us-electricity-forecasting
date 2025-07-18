# sarima-holtwinters-us-electricity-forecasting
# **Forecasting U.S. Monthly Electricity Generation from Natural Gas (2001–2030) Using SARIMA and Holt-Winters Models**
Long-term energy planning and sustainable resource management depend on sound forecasts of electricity generation. As demonstrated in the chart below, natural gas overtook coal in 2015 and has since become the leading source of electricity generation in the United States. Today, natural gas accounts for approximately 43% of total electricity generation, while coal contributes around 15%, highlighting a significant shift toward cleaner and more flexible energy sources.

<img width="920" height="613" alt="Chart (Jan 2001 - March 2025)" src="https://github.com/user-attachments/assets/6b7073d6-884a-476f-9bd1-3e1e32d503ad" />

---
This project applied two time series methods - **SARIMA** and **Holt-Winters (Triple Exponential Smoothing)** - to model and forecast monthly electricity generation from natural gas in the United States. The data was collected from the U.S. Energy Information Administration (EIA) over a 25-year period between January 2001 and March 2025. Both models were trained on historical data spanning January 2001 to December 2020 and evaluated over a test period from January 2021 to March 2025. Forecasts were then extended five years into the future, covering the period from April 2025 to March 2030.
Evaluation results indicate that both models effectively captured the underlying trend and seasonal patterns in the time series. The Holt-Winters model slightly outperformed SARIMA in terms of Root Mean Squared Error (RMSE) and Mean Absolute Error (MAE), suggesting marginally higher forecasting precision.
These findings demonstrate the practical utility of SARIMA and Holt-Winters models for medium-term energy forecasting, providing valuable insights for stakeholders involved in policy making, infrastructure investment, and energy market operations. For example, in **July 2029**, both models predict electricity generation from natural gas to reach approximately **226,000 thousand megawatt-hours (226 million MWh)**, reflecting peak summer demand.
<br> <br>

---

### **More Details on Data Collection and Applied Time Series Models**

#### **Data Collection and Processing**

The primary time series used in this project is **monthly electricity generated from natural gas** (measured in **thousand megawatt-hours**). The data was retrieved from the [U.S. Energy Information Administration](https://www.eia.gov/electricity/data/browser/).

*Note: The time series of interest is **nationwide** and not specific to any individual state or region.*

<img width="1186" height="491" alt="image" src="https://github.com/user-attachments/assets/59291c4f-53c9-4323-b63a-8e9ad08660f0" />

---
---
#### **List of Time Series Models Developed and Evaluated in This Project**
The following time series models were developed and compared to forecast monthly electricity generation:
- **SARIMA model**
- **Holt-Winters model (i.e., Triple Exponential Smoothing)**


---
---
# **Train-Test Split for Model Development & Evaluation**

To evaluate the forecasting performance of time series mode, includingas SARIMA and Holt-Winte models, the dataset was split into two subsets:

- **Training Sample (82%)**: All data points before January 2021 were used to train the models.
- **Test Sample (18%)**: Data from January 2021 onward were held out for testing and evaluating the model’s forecast accurarios.

<img width="1031" height="491" alt="image" src="https://github.com/user-attachments/assets/5b21cab8-5ee3-4618-8155-05370d985eba" />

### **Time Series Transformation Plots**

The plots below show:

1. **Original Series:** Monthly electricity generation from natural gas (shows trend and seasonality).  
2. **First-Order Differenced:** Removes trend but seasonal pattern remains.  
3. **Seasonally Differenced:** Removes both trend and seasonality, making the series more stationary.

These transformations help prepare the data for SARIMA modeling.
<img width="1190" height="1189" alt="image" src="https://github.com/user-attachments/assets/84037784-2628-49fc-b249-5d303997ca06" />

### **Interpretation of the Differencing Plots Above for Stationarity**

- The **first-order differenced (d=1) time series** still exhibits a repeating seasonal pattern, indicating that **seasonality remains** after removing the trend.

- The **seasonally differenced (D=1) time series** (after applying both first-order and seasonal differencing) shows that the **seasonal pattern has been removed**, and the fluctuations appear **more stable and random** over time.

- This suggests that the resulting time series is **stationary**, which is a necessary condition for fitting a **SARIMA model**.

---

# **(A) Applying the SARIMA Model to Monthly Electricity Generation Data**
The **Seasonal Autoregressive Integrated Moving Average (SARIMA)** model is an advanced extension of the ARIMA model. It incorporates seasonal components and captures complex patterns—such as trends and periodic fluctuations—in time series data. SARIMA is widely used in fields like economics, energy, and meteorology for forecasting time-dependent phenomena. A key assumption of the model is that the data must be stationary, which often requires differencing techniques to remove trends and seasonality.
<p>

In this project, two techniques were used to identify the optimal parameters `(p, d, q, P, D, Q)` of the SARIMA model:
### **1. Custom Grid Search**  
An iterative algorithm explores combinations of SARIMA parameters `(p, d, q)` and `(P, D, Q)` using the training dataset. For each candidate model,
- **AIC** (Akaike Information Criterion) and **SSE** (Sum of Squared Errors) were computed to assess model fit.  
- The **Ljung-Box test** was applied to ensure residuals are uncorrelated (p-value > 0.10).

The model with the lowest AIC—and the lowest SSE is selected. This iterative algorithm was adapted from an R-based framework developed by Dr. William Thistleton and Dr. Tura Sadigov from the State University of New York Polytechnic Institute.

The custom grid search algorithm identified **SARIMA(1, 1, 1) × (0, 1, 1)[12]** as the best-fit model, with the following performance metrics:
- **AIC**: 4576.52  
- **SSE**: 1.0592 × 10¹⁰  
- **Ljung-Box p-value**: 0.9347

---

### **2. `auto_arima()` from `pmdarima`**  
This function automatically selects the best SARIMA model based on **AIC/BIC**, while internally handling differencing and seasonal component selection.
The `auto_arima()` function selected **ARIMA(0,1,2)(0,1,1)[12]** as the best model according to the lowest AIC = 4579.4537.

## **Selecting the Best-Fit SARIMA Model**
The custom grid search method identified **ARIMA(1,1,1)(0,1,1)[12]** as the best-fit model with an AIC of **4576.52**, while the `auto_arima()` function selected **ARIMA(0,1,2)(0,1,1)[12]** with an AIC of **4579.45**. Since the former yielded a lower AIC, **ARIMA(1,1,1)(0,1,1)[12]** was selected as the final model.

### **The general equation of the SARIMA(1,1,1)(0,1,1)[12] model fitted above is:**

$$(1 - 0.8738B)(1 - B)(1 - B^{12})X_t = (1 - 1.0000B)(1 - 0.6026B^{12})Z_t$$

Where:
- $$ X_{t} $$ is the **predicted value** of the time series (monthly electricity generation from natural gas) at time t,
- $$ B $$ is the **backshift operator:**
 $$BX_t = X_{t-1} \    or   \  B^{12}X_t = X_{t-12} \ $$
- \( Z_t \) is the **white noise error term**, assumed to follow a normal distribution:  
 -  Z_t \sim \mathcal{N}(0,\sigma^2) \quad \text{with} \quad \sigma^2 = 3.13 \times 10^7 $$

<br>

The equation above is expanded using the backshift operator as follows:

$$
\mathbf{X_t} = 1.8738\mathbf{X_{t-1}} - 0.8738\mathbf{X_{t-2}} + \mathbf{X_{t-12}} - 1.8738\mathbf{X_{t-13}} + 0.8738\mathbf{X_{t-14}} + \mathbf{Z_t} - \mathbf{Z_{t-1}} - 0.6026\mathbf{Z_{t-12}}
$$


Where:
 - X_{t-1}, X_{t-2}, X_{t-12}, ... are the **observed values** of the time series from the previous month, two months ago, twelve months ago, and so on.
 - Z_{t-1} represents the **non-seasonal moving average (MA(1))** component.
 - Z_{t-12} represents the **seasonal moving average (SMA(1))** component at lag 12, which captures seasonal shocks repeating every 12 periods (e.g., months).

---
### **Diagnostic Plots for the SARIMAX Model**
This section generates diagnostic plots after fitting the SARIMAX model with the selected optimal parameters. These plots help assess key assumptions, such as the independence of residuals over time and their normality.

<img width="790" height="590" alt="image" src="https://github.com/user-attachments/assets/da126893-67b9-483b-a29e-b61f11beeaca" />

### Inference from the Diagnostic Plots:
- **Standardized Residuals Plot**  
  The residuals are fairly stable and fluctuate around zero, indicating they behave like white noise.

- **Histogram + KDE Plot**  
  The distribution of residuals closely resembles a normal distribution, as reflected by the histogram and the kernel density curve aligning with the theoretical normal curve.

- **Normal Q-Q Plot**  
  Most data points align with the reference line, suggesting that the residuals are approximately normally distributed.

- **Correlogram (ACF of Residuals)**  
  All spikes fall within the 95% confidence intervals (blue shaded bounds), confirming the absence of significant autocorrelation in the residuals and supporting the adequacy of the model. This is also supported by the Ljung-Box test results shown in the SARIMA model output.

---
### **SARIMA Model Prediction & Visualization for the Test Set and Future Forecast**

The SARIMA model was used to predict monthly electricity generation from natural gas for two distinct periods:

- **Out-of-sample test period**: 51 observations spanning from **January 2021 to March 2025**, used to evaluate the model's predictive accuracy on unseen data.
- **Forecast period**: 60 future observations from **April 2025 to March 2030**, providing a five-year projection beyond the available dataset.

In total, the SARIMA model generated **111 monthly predictions**, combining both test and forecast periods.  
The predicted values were then visualized alongside the actual training and test samples to assess the model’s performance and long-term forecasting behavior.
<img width="990" height="490" alt="image" src="https://github.com/user-attachments/assets/b5105009-fac3-40cc-bbf0-c0f5219ecbd9" />

---

# **(B) Applying the Holt-Winters Model to Monthly Electricity Generation Data**
<p align="justify">
The Holt-Winters model, also known as Triple Exponential Smoothing, is an extension of the exponential smoothing technique designed to capture three key components of a time series: level (the baseline value), trend (the direction or slope), and seasonality (regular periodic fluctuations). This makes it particularly well-suited for forecasting time series data that exhibits both trend and seasonal patterns. However, despite its strengths, the Holt-Winters model does not incorporate exogenous variables (i.e., external regressors). In other words, it generates forecasts based solely on the historical values of the target variable, without accounting for the influence of potential external factors. In cases where such external variables significantly contribute to the time series, alternative models like SARIMA with exogenous regressors (SARIMAX) or Facebook Prophet may provide better predictive performance.
<p>

The general equation of the additive Holt-Winters - Triple Exponential Smoothing is expressed as:  
F_{t+k} = L_t + k \times T_t + S_{t+k-12}

<img width="1170" height="374" alt="image" src="https://github.com/user-attachments/assets/49adbd2c-6be7-4f3e-8dc7-60b5171324cb" />

---

## **Holt-Winters Model Prediction & Visualization for the Test Set and Future Forecast**

Similar to the SARIMA model, the Holt-Winters model was used to predict monthly electricity generation from natural gas across two distinct periods:

- **Out-of-sample test period**: 51 observations spanning from **January 2021 to March 2025**  
- **Forecast period**: 60 future observations from **April 2025 to March 2030**

The predicted values were plotted alongside the actual training and test data to evaluate the model’s accuracy and long-term forecasting performance.
<img width="990" height="490" alt="image" src="https://github.com/user-attachments/assets/053da568-1d46-42f2-b05e-c092cc85a72c" />

---
# **Plotting the RMSE and MAE Metrics for the SARIMA and Holt-Winters Models.**
<img width="790" height="390" alt="image" src="https://github.com/user-attachments/assets/fa7ce537-7ba8-4ec6-a4fd-288a8d8d4c21" />

### **Inference from the Barplot of RMSE and MAE:**
The comparison of RMSE and MAE on the test dataset indicates that the Holt-Winters (Triple Exponential Smoothing) model **slightly outperformed** the SARIMA model in terms of prediction accuracy, with approximately **7–9% lower error values**. Although both models exhibited similar performance, the lower RMSE and MAE associated with the Holt-Winters model suggest it provided a **marginally better fit** for modeling electricity generation from natural gas in this context.

---
---
## **Static and Interactive Plots of Actual vs. Forecasted Natural Gas Electricity Generation (SARIMA & Holt-Winters)**
The plots below illustrate the actual (observed) and forecasted monthly electricity generation from natural gas, covering the test period (January 2021 – March 2025) and extended projections through March 2030. The forecasts were generated using the SARIMA and Holt-Winters (Triple Exponential Smoothing) models.
### **Static Plot:
<img width="1187" height="491" alt="image" src="https://github.com/user-attachments/assets/74d1b6aa-110f-4bcf-af95-636b7d375204" />

### **Interactive Plot:
<img width="2596" height="955" alt="image" src="https://github.com/user-attachments/assets/da891158-1ceb-45d8-9f79-67f6227faffe" />

---
---

## **Conclusions & Suggestions for Future Work**
### **Conclusions**
The primary objective of this project was to aply a time series forecasting technique to model and predict monthly electricity generation from natural gas in the United States. To accomplish this objective, two time series models-SARIMA and Holt-Winters, were developed and compared using data from the U.S. EIA spanning a 25-year period from January 2001 to March 2025. The modling comparison results indicated that the Holt-Winters model slightly outperformed the SARIMA in terms. Model comparison results indicated that the Holt-Winters model had similar performance, as measured by RMSE and MAE.
<br>
The findings from this project can support decision-makers and energy planners in developing long-term strategies related to infrastructure investment, resource allocation, and operational planning in the energy sector. For instance, both models forecast an average electricity generation of approximately **218,000 thousand MWh (218 million MWh)** for the peak summer month of July 2029.

### **Recommendations for Future Work**
While this study focused on natural gas, electricity in the U.S. is generated from multiple energy sources such as coal, nuclear, wind, and solar. **For future work**, it is recommended to extend this analysis by developing a **multivariate time series model** that predicts electricity generation from multiple energy sources, enabling a more comprehensive understanding of the national electricity supply mix.
One possible extension of this work is to incorporate **external factors**—such as fuel prices or policy changes—into the forecasting models. Time series techniques like SARIMAX and Facebook Prophet with regressors are well-suited for modeling such relationships, offering deeper insights into the drivers of electricity generation trends.


