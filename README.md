# sarima-holtwinters-us-electricity-forecasting
# **Forecasting U.S. Monthly Electricity Generation from Natural Gas (2001–2030) Using SARIMA and Holt-Winters**
Long-term energy planning and sustainable resource management depend on sound forecasts of electricity generation. As demonstrated in the chart below, natural gas overtook coal in 2015 and has since become the leading source of electricity generation in the United States. Today, natural gas accounts for approximately 43% of total electricity generation, while coal contributes around 15%, highlighting a significant shift toward cleaner and more flexible energy sources.

<img width="920" height="613" alt="Chart (Jan 2001 - March 2025)" src="https://github.com/user-attachments/assets/6b7073d6-884a-476f-9bd1-3e1e32d503ad" />

This project applies two time series methods - **SARIMA** and **Holt-Winters (Triple Exponential Smoothing)** - to model and forecast monthly electricity generation from natural gas in the United States. The data was collected from the U.S. Energy Information Administration (EIA) over a 25-year period between January 2001 and March 2025. Both models were trained on historical data spanning January 2001 to December 2020 and evaluated over a test period from January 2021 to March 2025. Forecasts were then extended five years into the future, covering the period from April 2025 to March 2030.
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
