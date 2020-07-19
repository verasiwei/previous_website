---
title: Machine Learning-Classification
date: 2019-06-16 15:21:31
tags:
mathjax: true
categories:
- Statistics & Algorithms
---

## Bagging (Random Forest)
**1.Bagging and Bootstrap**
Given a training set $X=x_{1},x_{2},...x_{n}$ with responses $Y=y_{1}, y_{2},.. y_{n}$, bagging **repeatedly** with K times, each time selects a subset of random samples with replacement from the training set and fits the algorithms like trees to these samples. Assuming we have trained a regression tree $f_{k}$ on $X_{k}$ and $Y_{k}$ where k=1...K. Predictions of unknown samples $X_{unknown}$ are based on the average of the predictions from all the trees $f_{k}$ on $X_{unknown}$.
$$f'=\frac {1}{K} \sum_{k=1}^{K} f_{k}X_{unknown}$$
Or as for classification trees, based on the majority votes of trees. The optimal number of trees K can be found through cross-validation or by out of bag error. 

**2.Decision Tree**
A decision treee is a tree where each node represents a feature, each branch represents a dicision and each leaf represents an outcome. Tree models where the outcome are discrete values are called classification trees and tress models where the outcome take continuous values are called regression trees.

**3.Random Forest**
Random forest selects a random subset of the features at each candidate split in the learning process. If one or a few features are very strong predictors for the response variable, these features will be selected in many of the K trees, which cause them to become correlated.
I will use the package in python and house price data in Kaggle as an implemention example.

**4.Implementation**
```
import pandas as pd
import numpy as np
from sklearn.ensemble import RandomForestClassifier
from sklearn import ensemble, tree, linear_model
from sklearn.model_selection import train_test_split, cross_val_score

# read data in
train = pd.read_csv("D:/work/kaggle/train.csv")
test = pd.read_csv("D:/work/kaggle/test.csv")

# skip the procedures of data manipulation and cleaning
def train_test(estimator, x_trn, x_tst, y_trn, y_tst):
    prediction_train = estimator.predict(x_trn)
    # Printing estimator
    print(estimator)
    # Printing train scores
    get_score(prediction_train, y_trn)
    prediction_test = estimator.predict(x_tst)
    # Printing test scores
    print("Test")
    get_score(prediction_test, y_tst)

RM = ensemble.RandomForestRegressor(n_estimators=1000,
random_state=0,criterion="mse",max_features="sqrt",
max_depth=50).fit(x_train,y_train)
train_test(RM,x_train,x_test,y_train,y_test)

```

## Boosting (Gradient Boosting)
**1.Algorithm**
The aim of the regression is to fit a model F(x) to predict the outcome $\hat{y}$ by minimizing the error. For example, in the situation of least-square regreesion setting, the aim of the regression is to fit a model F(x) to predict the outcome $\hat{y}$ by minimizing the mean squared error $\frac{1}{n} \sum_{i}(\hat{y_{i}}-y_{i})^2$. 

At each stage k, $1 \leq k \leq K$, the function at each stage is $F_{k}$. The gradient boosting improves on the model by constructing a new model to estimate the error: $F_{k+1}(x)=F_{k}(x)+h(x)$. Assuming $\hat{y}=F(x)$ and the loss function is $L(y,\hat{y})$. The error for all samples are $J=\sum_{i=1}^{n} L(y_i,\hat{y_i}$. Based on the chapter of algebra basic, the gradient of J is $\nabla J=\hat{y} -y$. Therefore the algorithm of gradient boosting is:
1.initialize a model $F_{0}(x)$
2.For k=1 to K:
2.1compute gradient
$$r_{ki}=-\frac {\alpha L(y_{i}, F(x_{i}))}{\alpha F(x_{i})}$$

2.2Fit a function $h_{k}$ by using the training dataset

2.3compute $\gamma$ by solving the below equation:
$$\gamma_{k}=argmin \sum_{i=1}^{n}L(y_{i},F_{k-1}(x_{i})+\gamma h_{k}(x_{i}))$$

2.4Update the model
$$F_{k}(x)=F_{k-1}(x)+\gamma_{k}h_{k}(x)$$

**2.Implementation**
I will use package in Python as an example.

```
from sklearn import ensemble, tree, linear_model
from sklearn.ensemble import GradientBoostingRegressor

GBest = ensemble.GradientBoostingRegressor(n_estimators=3000, learning_rate=0.05, max_depth=3, max_features='sqrt',
                                               min_samples_leaf=15, min_samples_split=10, loss='huber').fit(x_train, y_train)
```

