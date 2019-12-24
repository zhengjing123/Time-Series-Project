
# Time Series Project <br>
## Modeling the temperature in Melbourne from 1971 to 1990 <br>

Author: Zheng Jing <br>
Programming Language: R <br>


## Question: 

Given the Melbourne temperatre data from 1971 to 1990, analyze its pattern (trend & seasonality), perform anomaly detection, fit SARIMA model, then make predictions for Melbourne temperature in 1991. <br>


## Data:
https://pkg.yangzhuoranyang./tsdl/ <br>
The dataset was released by Australian Bureau of Meteorology, which records the monthly data of mean maximum temperature in Melbourne from Jan. 1971 to Dec. 1990.


## Methodology:

​ a) split data into training and testing <br>

​ b) plot original time series; plot autocorrelation & partial autocorrelation <br>

​ c) transform the data; difference the data <br>

​ d) fit SARIMA model <br>

​ e) diagnose candidate models- checking unit roots, residual behavior... <br>

​ f) evaluate the final model on testig set <br>

​ g) predict the temperature in 1991 with C.I. <br>


## Conclusion:

Our final model - SARIMA(2,2,2)*(0,1,1) as follows:

(1 - 1.3636B - 0.5909B^{2})\diff_s 12X_{t} = (1 - 1.3647 + 0.6943B^{2})(1-0.9308B^{12})Z_{t}

Based on the model, we predict the temperature in 1991 to share the same pattern as previous years, which is warm at the beginning and end of the year, and turning cold in the middle. We also make predictions on mean max temp for each month with plus/minus three bound.

For example, we predict the mean max temperature in Melbourne in Jan. 1991  to be 26 Celcius(+/- 4), and 10 Celcius(+/-3) in July.
