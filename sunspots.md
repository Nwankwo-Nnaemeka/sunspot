
## Visualizing The Data
After importing the libraries we need function to create a plot of the time series
## Model Building
## Split the Data
#### Naive Forecast
- naive forecast is just a naive way of predicting a time series. It assumes the next value </br> is the same as the previous value. As a baseline naive forecast is good to see how other models compares to it, if it does better or worse and how you can improve on the baseline to get better predictive performance.
#### Moving average 
- Another technique you can use is moving average which basically sums up value from a series of time steps and the average will be the prediction of the next time step for example the average value of time `t = 1` to `t = 10` will be the forecast of time step 11 </br> the number of time steps we need to compute the average is called a `window`
the lower the averaging window the performance improved which tells me that points closer to each other have a better signal for the output y
window of 3 gave me a better score than naive forecast and a better mean absolute error, while 4 gave me a better mean squared error compared to each other and not naive forecast.