# TimeSeries-FlightPassesngerPrediction
The problem we are going to look at in this post is the international airline passengers prediction problem.

This is a problem where given a year and a month, the task is to predict the number of international airline passengers in units of 1,000. The data ranges from January 1949 to December 1960 or 12 years, with 144 observations.

## Below is a sample of the first few lines of the file.

|"Month"|"International airline passengers: monthly totals in thousands.|
|-------|---------------------------------------------------------------|
|"1949-01"|112|
|"1949-02"|118|
|"1949-03"|132|
|"1949-04"|129|
|"1949-05"|121|

We can load this dataset easily using the Pandas library. We are not interested in the date, given that each observation is separated by the same interval of one month. Therefore when we load the dataset we can exclude the first column.
## Multilayer Preceptron Regression
We want to phrase the time series prediction problem as a regression problem.
That is, given the number of passengers (in units of thousands) this month, what is the number of passengers next month.

We can write a simple function to convert our single column of data into a two-column dataset. The first column containing this month’s (t) passenger count and the second column containing next month’s (t+1) passenger count, to be predicted.
With time series data, the sequence of values is important. A simple method that we can use is to split the ordered dataset into train and test datasets. 

The function takes two arguments, the dataset which is a NumPy array that we want to convert into a dataset and the look_back which is the number of previous time steps to use as input variables to predict the next time period, in this case, defaulted to 1.

|X	|	Y|
|---|--|
|112|		118|
|118|		132|
|132|		129|
|129|		121|
|121|		135|

If you compare these first 5 rows to the original dataset sample listed in the previous section, you can see the X=t and Y=t+1 pattern in the numbers.
We can now fit a Multilayer Perceptron model to the training data.

We use a simple network with 1 input, 1 hidden layer with 8 neurons and an output layer. The model is fit using mean squared error, which if we take the square root gives us an error score in the units of the dataset.

Once the model is fit, we can estimate the performance of the model on the train and test datasets. This will give us a point of comparison for new models.
Finally, we can generate predictions using the model for both the train and test dataset to get a visual indication of the skill of the model.
Taking the square root of the performance estimates, we can see that the model has an average error of 23 passengers (in thousands) on the training dataset and 48 passengers (in thousands) on the test dataset.

Train Score: 532.59 MSE (23.08 RMSE)
Test Score: 2358.07 MSE (48.56 RMSE)


## Multilayer Perceptron Using the Window Method

We can also phrase the problem so that multiple recent time steps can be used to make the prediction for the next time step.

This is called the window method, and the size of the window is a parameter that can be tuned for each problem.

For example, given the current time (t) we want to predict the value at the next time in the sequence (t + 1), we can use the current time (t) as well as the two prior times (t-1 and t-2).

When phrased as a regression problem the input variables are t-2, t-1, t and the output variable is t+1.

|X1|	X2|	X3|	Y|
|--|----|---|--|
|112|	118|	132|	129|
|118|	132|	129|	121|
|132|	129|	121|	135|
|129|	121|	135|	148|
|121|	135|	148|	148|

We can re-run the example in the previous section with the larger window size. We will increase the network capacity to handle the additional information. The first hidden layer is increased to 14 neurons and a second hidden layer is added with 8 neurons. The number of epochs is also increased to 400.
![Alt text](http://Window-Method-for-Time-Series-Prediction-With-Neural-Networks.png "Optional title")
Looking at the graph, we can see more structure in the predictions.

You still would have to fine tune the parameters to get the best accuracy!
