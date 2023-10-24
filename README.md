# Libraries
To work with a time series there are some essential libraries we have to import in other to explore, train and make predictions from our data.
After importing the libraries we need function to create a plot of the time series
# Download and Preview Dataset
The dataset was downloaded from Kaggle it show the average monthly sunspots from the year 1749 to january 2021 you can get it from Kaggle [Sunspot Data](https://www.kaggle.com/datasets/robervalt/sunspots) which spans over 272 years.
- Created a utitlity function in other to facilitate visualizing our data.</br> 
- Read in the data and split the data into training and validation sets 
# Statiscal Analysis
### Naive Forecast
- naive forecast is just a naive way of predicting a time series. It assumes the next value is the same as the previous value. As a baseline naive forecast is good to see how other models compares to it, if it does better or worse and how you can improve on the baseline to get better predictive performance.
### Moving average 
- Another technique you can use is moving average which basically sums up value from a series of time steps and the average will be the prediction of the next time step for example the average value of time `t = 1` to `t = 10` will be the forecast of time step 11 </br> the number of time steps we need to compute the average is called a `window`
### Differencing
moving average above does not anticipate trend or seasonality, so one good thing to do is to try to remove the trend and seasonality  by subtracting the value at time `t- 132` and see if the prediction gets better. Since the seasonality period as stated by some science is 11 years some even suggest 22 years [reference](https://en.wikipedia.org/wiki/Solar_cycle)
After that we add the trend and seasonality back by adding the past values from `t- 132`
the lower the averaging window the performance improved which tells me that points closer to each other have a better signal for the output y
window of 3 gave me a better score than naive forecast and a better mean absolute error, while 4 gave me a better mean squared error compared to each other and not naive forecast.
#### Smoothing
You can also use the moving average to smooth out the past values before adding them back to the time difference there's two ways to that by either using:
* Trailing window - getting the mean of the past value to smooth out the value at the current time step eg `t=0` to `t=3` to get value at **`t=3`**
* Centered window - getting the mean of the past and future to get the value at the current time step e.g `t=0` to `t=4` to get value at **`t=2`**
I used centered window in this to smooth it out.
# Neural Network
### Data Preparation
To train a neural network we have to set our data to have both features and label to be able to carry out supervised learning of our data. Our features are the values of our monthly sunspot of a particular time step we set with the label being the next value in that time step. for example if we take 30 values as the time step or window size we choose the 30 values are our feature with the 31st value being our label.
### Create Model 
**I used:**
* 1 conv1D layer - one important parameter in the conv1D layer is the `padding`. For time series data it is important not to include future values in the current time stamp when you are doing your calculation. you can read more on [casual padding](https://www.tensorflow.org/api_docs/python/tf/keras/layers/Conv1D?authuser=1#args)
- 2 LSTM layers
- 3 Dense layers
- lambda layer as the last layer scaling the outputs can help training.- LSTM's uses tanh function from [-1, 1].

### Training and Tuning
From the `keras.callbacks` you can use the learning rate scheduler to find an optimal parameter for the learning rate then reinitialize the model and use the selected learning rate to train the model.

### Forecasting and Prediction
You can now make predictions to evaluate model peformance. We do that by generating batch of a windowed data. I did this with two methods
#### 1. Using A For Loop
* Slice a window from the original dataset that are aligned with the validation set and add batch dimension dimension before passing it to the model. We can do that by indexing using numpy's `np.newaxis` or `np.expand_dims`. you can also use tensorflows own version pof these methods. 
* Using this method takes a while to run so using the next method makes the code run faster
#### 2. Using Tensoflow batching capabilities
* The for loop runs a single winodw at a time, but here you can pass the entire batch and the model will process them in parallel. Predictions are generated almost instantly.
* The function created is almost a copy of the create_window_dataset function we used to preprocess the data. But it is not shuffled so as to maintain the order with the validation set.