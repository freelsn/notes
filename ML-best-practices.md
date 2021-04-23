https://www.cs.toronto.edu/~hinton/absps/guideTR.pdf

## The size of a mini-batch

10 to 100
divide the total gradient computed on a mini-batch by the size of the mini-batch
for classification, a single mini-batch should better contain one example of each class

## Monitoring the progress of learning

use multiple histograms and graphic displays

## Monitoring overfitting

After every few epochs, compute the metrics of a representative subset of the training data and compare it with the metrics of the validation set. If the gap starts growing, the model is overfitting.

## Initial values of the weights and biases

Use small random values for the weights chosen from a zero-mean Gaussian with a standard deviation of 0.01
Set the hidden biases to 0

## Regularization

the weight-cost coefficient for L2 typically range from 0.01 to 0.00001, try an initial weight-cost of 0.0001
weight cost is typically not applied to the hidden and visible biases


overfitting

	1. reduce network capacity
	2. apply regularization
	3. add dropout layer
	4. batch normalization

https://stats.stackexchange.com/questions/426931/how-to-deal-with-biased-dataset-for-both-training-and-testing-data

Handle imbalanced dataset
	- duplicating data with low frequencies on training set
	- https://www.tensorflow.org/tutorials/structured_data/imbalanced_data
