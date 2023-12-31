# Tensorflow Lending Club Example

Example of using tensor flow to predict bad loans
for [this Kaggle dataset,](https://www.kaggle.com/datasets/deependraverma13/lending-club-loan-data-analysis-deep-learning?resource=download)

## Features

There are several features including FICO score and interest rate.
For details see
[this link.](https://www.kaggle.com/datasets/deependraverma13/lending-club-loan-data-analysis-deep-learning?resource=download)

# Split Data Program

*split\_data.py*

This program splits the data into train, test, and validation.
It also replaces periods in column names to underscores.

| split      | fraction |
|:----------:|:---------|
| train      | 80%      |
| test       | 15%      |
| validation | 5%       |

Note that if we took a random sample of 5% of all of the data for
validation, there is a high probability that all or nearly all of the
validation set would be good loans since the vast majority of all
loans are good.

So we take a random sample of 5% of the good loans and 5% of the bad
loans for validation. The same technique is used for the test and
train splits.

# Train and Predict Program

*train\_and\_predict.py*

Builds and trains a neural network to predict if the loan
was fully paid. The neural network looks like this:

```
_________________________________________________________________
 Layer (type)                Output Shape              Param #   
=================================================================
 input_1 (InputLayer)        [(None, 19)]              0         
                                                                 
 dense (Dense)               (None, 20)                400       
                                                                 
 tf.nn.relu (TFOpLambda)     (None, 20)                0         
                                                                 
 dense_1 (Dense)             (None, 20)                420       
                                                                 
 tf.nn.relu_1 (TFOpLambda)   (None, 20)                0         
                                                                 
 dense_2 (Dense)             (None, 20)                420       
                                                                 
 tf.nn.relu_2 (TFOpLambda)   (None, 20)                0         
                                                                 
 dense_3 (Dense)             (None, 20)                420       
                                                                 
 tf.nn.relu_3 (TFOpLambda)   (None, 20)                0         
                                                                 
 dense_4 (Dense)             (None, 1)                 21        
                                                                 
=================================================================
Total params: 1681 (6.57 KB)
Trainable params: 1681 (6.57 KB)
Non-trainable params: 0 (0.00 Byte)

```

# Normalizing Features

The features are normalized so they will fit a neural network better.

## Categorical Features

Categorical features are converted to one-hot representation.

## Binary Features

If the feature is always 0 or 1, we leave it as-is.

## Floating Point Features

These features are transformed using
[quantile mapping.](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.quantile_transform.html)

# Balancing Training Data

Most of the loans are good so training data is highly imbalanced. We used
class weights for this but they didn't seem to help.

# Plotting Program

*plot\_data.py*

Reads prediction data from the database and plots ROC curves.


# Prediction Results

This ROC curve shows true positive versus false positive.

* true positive: a bad loan was classified as bad
* false positive: a good loan was classified as bad

In this case, you would get slightly better results using the FICO
score by itself. Either way, the prediction is not very good.

![](images/roc_curve.svg)


