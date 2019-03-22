---
layout: post
title: Introduction to Machine Learning by its workflow
author: Quentin Monmousseau
tags: [Data Science]
image: img/blog-cover.jpg
date: 2015-02-02T23:46:37.121Z
draft: false
---

# Introduction to Machine Learning by its workflow

In this article, we are going to see how you should organize your machine learning workflow. It also includes some examples using python's libraries.

Here are the different steps we are going to go through.

**[Intro. Requirements](#zero)**

**[I. Data loading and overview](#one)**
- [a. Loading the data](#one-a)
- [b. Overview](#one-b)

**[II. Data cleaning](#two)**
- [a. Duplicated and missing values](#two-a)
- [b. Deal with outliers](#two-b)

**[III. Features engineering](#three)**
- Transformations
- Features creations and deletions
- Dimensional reductions

**[IV. Model selection](#four)**
- Split
- Metrics/Loss function
- Models stability (Cross-Validation)
- Check over/underfit

**[V. Hyperparameters tuning](#five)**

**[VI. Training and predictions](#six)**

<a id="zero"></a>
## Intro. Requirements

Before starting, you need to set up your developing environment. If you didn’t, please follow this easy tutorial to get started.
Also, be aware that some bullets points highlighted below imply a basic understanding of different mathematics concepts. I highly recommend you read/keep aside this article on Statistics Basics if you are not confident at all with mathematics.

<a id="one"></a>
## I. Data loading and overview

<a id="one-a"></a>
### a. Loading the data
The very first step is to import essential libraries.

```python
import os

import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
import seaborn as sns

%matplotlib inline
```

Now we load the data.

```python
FILEPATH = os.path.join('data', 'dataset.csv')

df = pd.read_csv(FILEPATH, index_col=0)
df.head(2)
```
<a id="one-b"></a>
### b. Overview

In Machine Learning, we want to build a model capable of predicting one of these variables (called the target) thanks to the others (called the features). Here, our target will be variable3 and our features to do it variable1 and 2. We say that our model has currently two dimensions (i.e. two features).

First of all, we want to know what kind of values we have to predict. This will tell us what kind of algorithms use to build our model. When target values are provided (i.e. labeled data), we talk about supervised learning. When there aren't, we talk about unsupervised learning. Targets can be:
- **categoricals** (qualitative) : who/what/what kind  
- **numericals** (quantitative) : how much 

Excluding **neural networks**, there are 3 big types of ML algorithms:  

When supervised learning:  
- **classification** for categorical targets.  
- **regression** for numerical targets.  

When unsupervised learning:  
- **clustering** that will build clusters for us.

In our example, the target is provided so our model will learn from its values. We want to predict a numerical target so we will use regression algorithms.

<a id="two"></a>
## II. Data cleaning

<a id="two-a"></a>
### a. Duplicated and missing values

Sometimes rows are duplicated so you just need to remove the duplications.  
You can also find missing values that you can choose to remove or try to fill (by doing a mean imputation/mod input (If numerical) or binarization (if categorical)).

```python
#Count the number of duplicated rows
df.duplicated().sum()

#Drop the duplicates
df.drop_duplicates()

#Count the number of NaN values for each column
df.isna().sum()

#Drop all NaN values
df = df.dropna()
```
<a id="two-b"></a>
### b. Deal with outliers

Outliers are extreme values that can damage our model. We can find univariate outliers on a single variable or multivariate outliers in the relationship between two variables.  
You'll have to determine whether they are errors or if they are possible (here you’ll probably need a specific business knowledge). If they aren't legit, you'll have to delete them.

We can detect univariate outliers visually by plotting a boxplot...

```python
#Visualize univariate outliers with a boxplot
plt.subplots(figsize=(18,6))
plt.title("Outliers visualisation")
df.boxplot();
```

... or by considering outliers as:
- mistyped data points, using sigma-clipping operations.
- data points that fall outside of 1,5*IQR above the 3rd quartile and below the 1st quartile.
- data points that fall outside of 3 standard deviations, using z-score.

```python
#Delete univariate outliers using sigma-clipping operations
quartiles = np.percentile(df['feature'], [25, 50, 75])
mu = quartiles[1]
sig = 0.74 * (quartiles[2] - quartiles[0])

df = df.query('(feature > @mu - 5 * @sig) & (feature < @mu + 5 * @sig)')

#Delete univariate outliers using IQR
quartiles = np.percentile(df['feature'], [25, 50, 75])
mu = quartiles[1]
sig = 0.74 * (quartiles[2] - quartiles[0])

df = df.query('(feature > @mu - 5 * @sig) & (feature < @mu + 5 * @sig)')
````

III. Features engineering](#three)**
- Transformations
  - Categorical datas: One-hot encoding, 
  - Standardization
- Features creations and deletions
- Dimensional reductions

Une fois les variables catégorielles encodées, on peut faire une matrice de corrélation (de Pearson). Par soucis de performance en terme de vitesse d'execution, il est recommandé de réduire au maximum le nombre de features. Dans ce cas, on conservera des features fortement corréllées à la target et très peu entre elles pour apporter chacune des informations intéressantes.


IV.

b. Metrics/Loss function



#### Classifications problems:
- **MSE (Mean Squared Error)**:

- **accuracy score**:
Using accuracy as a performance measure for highly imbalanced datasets is not a good idea. For example, if 90% points belong to the true class in a binary classification problem, a default prediction of true for all data poimts leads to a classifier which is 90% accurate, even though the classifier has not learnt anything about the classification problem at hand!

- **Precision Recall**:

- **ROC AUC**: