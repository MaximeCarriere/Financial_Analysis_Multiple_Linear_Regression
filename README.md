# Financial Analysis with Multiple Linear Regression
# Description

The goal of this analysis is to predict the value of the exchange-traded fund SPY which tracks S&P 500. Here, the SPY prediction is based on different predictors in order to forecast its value when the US market opens in the morning. 

Predictors:

- Nikkei (Japan)
- HSI (Hong-Kong)
- DAX (Germany)
- CAC40 (France)
- S&P 500 (US)
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

1) We will focus on the open price in order to simplify the Multiple Linear Regression, but every variables could be analyse in the same way. 
The first step is to create a new variable called "indicepanel" which is composed of the difference between two following days for every predictor. Obviously the first value in each column is "NaN" since it is not possible to do the difference with the previous day, we therefore drop these values. 

2) The second step is to split the data into train and test data sets. The best way to explore the data is to draw scatter plots between all different combinaisons that we have in our data set. 

![Scatter](https://user-images.githubusercontent.com/55028120/67499099-673bee00-f678-11e9-8bcb-ba97392d9ee4.png)






