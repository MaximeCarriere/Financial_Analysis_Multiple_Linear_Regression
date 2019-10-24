# Financial Analysis with Multiple Linear Regression
# Description

The goal of this analysis is to predict the value of the exchange-traded fund SPY which tracks S&P 500. Here, the SPY prediction is based on different predictors in order to forecast its value when the US market opens in the morning. 

Predictors:

- Nikkei (Japan)
- HSI (Hong-Kong)
- DAX (Germany)
- CAC40 (France)
- Dji (US)
- Nasdaq (US)

The US market  is opened from 9:00 AM to 4:00 PM, the European Market is opened from 3:00 AM to 11:30 AM and the Asian Market opens at 8:00 PM and closes at 3:00 AM. Therefore, the Asian Market information is available for the US market at its opening. 

# Data

All data sets have 6 columns:

- Index: the date
- Open: the value of the share at the opening
- High: the highest value of the share
- Low: the lowest value of the share
- Close: the value of the share at the closing
- Adj Close: the adjusted closing price including dispution and corporate actions 
- Volume: the number of shares tradded on that day

# Multiple Linear Regression

1) We will focus on the open price in order to simplify the Multiple Linear Regression, but every variables could be analyse in the same way. 
The first step is to create a new variable called "indicepanel" which is composed of the difference between two following days for every predictor. Obviously the first value in each column is "NaN" since it is not possible to do the difference with the previous day, we therefore drop these values. 

2) The second step is to split the data into train and test data sets. The best way to explore the data is to draw scatter plots between all different combinaisons that we have in our data set. 

![Scatter](https://user-images.githubusercontent.com/55028120/67499099-673bee00-f678-11e9-8bcb-ba97392d9ee4.png)


3) Look at the coefficient of correlation between our predictors and SPY. 

![Correlation](https://user-images.githubusercontent.com/55028120/67499982-e120a700-f679-11e9-9fc2-00f9e8162fb5.png)

4) Built the models. Model 1 includes all predictors.

Model 1 Summary:

![Model_1_Summary](https://user-images.githubusercontent.com/55028120/67503304-4d51d980-f67f-11e9-94fa-4eb6ad003d9f.png)

Since Model 1 is not significant, we can try to drop the less significant variable to obtain a significant model. Thus, Model 2 includes all predictors but Daxi.

Model 2 Summary:

![Model_2_Summary](https://user-images.githubusercontent.com/55028120/67517770-39b46c00-f69b-11e9-9c67-fbfcc927d6d6.png)


Thus, the equation of this model is: SPY = 0.0831 + 0.0022*CAC40 + 0.004*Nikkei - 0.007*Dji - 0.003*Hsi + 0.0018*Nasdaq

5) Make Prediction. To do so we plot the train data set against the test data set. Here, the positive correlation is not really strong. 

![Screen Shot 2019-10-24 at 17 28 26](https://user-images.githubusercontent.com/55028120/67505753-bfc4b880-f683-11e9-8e64-769e59a831af.png)


# Diagnostic of the model

A Multiple Linear Regression model has to satisfy 4 assumptions:

- Linearity
- Independance
- Normality
- Homoscedasticity

1) Linearity defines a linear relationship between our variables SPY and each of our predictors. This can be observed in the scatter plot previously showed. 

2) Independance of variables. Residuals must be uncorrelated. We can use the Durbin-Watson test (https://www.jstor.org/stable/1914122?seq=1#metadata_info_tab_contents). A value below 1.5 corresponds to a positive correlation. A value above 2.5 corresponds to a negative correlation. A value between 1.5 and 2.5 represents the independance of variable. In our model the value of the Durbin-Test is 2.037. Thus, the independance is statisfied. 

![Independance](https://user-images.githubusercontent.com/55028120/67507941-e389fd80-f687-11e9-819b-f53b46864d03.png)

3) Normality, the variable must have a normal distribution. To check this assumption, the quantil-quantil plot (QQ plot) is drawn. If the distribution follows the red line which represents a 'perfect normal distribution', the distribution can be considered as normal. In our model, all predictors and the independant variable seem to be normal.

![Normality](https://user-images.githubusercontent.com/55028120/67519355-9b2a0a00-f69e-11e9-973d-0d16b350b64c.png)


4) Homoscedasticity represents an equal variance between predictors and the independant variable.

If the model violates the assumptions, it is not possible to make statistical inference. However, the accurancy and consistency of model does not rely on these four assumptions.

# Model Evaluation

We can use 2 parameters:
- Root Mean Square Error (RMSE) 
- R-square (R^2)

It is important to compute these two values after training and to compare them to the values obtained for the test data set. The values should be similar in the two conditions. Otherwise it indicates the presence of overfitting. Here the two values are relatively the same.

![Screen Shot 2019-10-24 at 21 10 41](https://user-images.githubusercontent.com/55028120/67521450-d0385b80-f6a2-11e9-8c19-274c7f6ab6c7.png)

# Evaluate the strategy

Now we will estimate the capacity of the model. If the model is positive we are long (share increase) and we will buy shares, if the model is negative we are short (share decrease) and therefore we will sell shares. We then compute the daily profit and the cumulative profit over time. 

The Total profit made in Train:  115.98004799999941. (Train correponds to the train data set of 1000 days). 

Then, we can compare the performance of this strategy, which we call the signal-based strategy, with a passive strategy, which we call buy and hold strategy, which is to buy more shares of SPY initially and hold it for 1000 days. We can see from the plot, signal-based strategy outperforms buy-and-hold strategy. 

![Profit_total](https://user-images.githubusercontent.com/55028120/67523779-42ab3a80-f6a7-11e9-86eb-e30e28aff003.png)

The consistency of performance is very important. Otherwise, it is too risky to apply it in the future. Average daily return is a mirror we can make comparison in finance industry when they use a Sharpe ratio and maximum drawdown.

Sharpe ratio measures excess return per unit of deviation in an investment asset or trading strategy named after William Sharpe. 

- Usually, any Sharpe ratio greater than 1.0 is considered acceptable to good by investors.
- A ratio higher than 2.0 is rated as very good.
- A ratio of 3.0 or higher is considered excellent.
- A ratio under 1.0 is considered sub-optimal.

In the train data set:
Daily Sharpe Ratio is  0.05398637652227286
Yearly Sharpe Ratio is  0.8570071587805803

In the test data set:
Daily Sharpe Ratio is  0.032941642184423775
Yearly Sharpe Ratio is  0.522932357988359

Thus, the Sharp ratio indicates that our model might not be optimal. 

Maximum drawdown is a maximum percentage decline in the strategy from the historical peak profit at each point in time. We first compute drawdown, and then the maximum of all drawdowns in the trading period. Maximum drawdown is that risk of mirror for extreme loss of a strategy. 

One would hope that the maximum drawdown would be as small as possible. If an investment never lost a penny, the maximum drawdown would be zero. The worst possible maximum drawdown would be 100%, meaning the investment is completely worthless. Most maximum drawdowns will fall somewhere between these two extremes. The two most important elements to keep in mind when analyzing maximum drawdown are the asset class and time frame being analysed.

Maximum Drawdown in Train is  0.10943675248590072
Maximum Drawdown in Test is  3.0691683894010597

Here again, the Maximum Drawdown indicates that our model might not be optimal. 
