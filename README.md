# TimeSeries-FlightPassesngerPrediction
## Multilayer Perceptron Using the Window Method

We can also phrase the problem so that multiple recent time steps can be used to make the prediction for the next time step.

This is called the window method, and the size of the window is a parameter that can be tuned for each problem.

For example, given the current time (t) we want to predict the value at the next time in the sequence (t + 1), we can use the current time (t) as well as the two prior times (t-1 and t-2).

When phrased as a regression problem the input variables are t-2, t-1, t and the output variable is t+1.

You still would have to fine tune the parameters to get the best accuracy!
