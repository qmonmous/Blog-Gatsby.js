---
layout: post
title: 🇬🇧 An introduction to Machine Learning with Python
author: Quentin Monmousseau
tags: [Data Sc. & A.I.]
image: img/header-articles.png
date: 2019-03-02T23:46:37.121Z
draft: false
---

*In this article, we are going to see how you should organize your machine learning workflow. It also includes some examples using python's libraries.*

---

Before starting, you need to set up your developing environment. If you're not familiar with Python environments, you can launch a notebook with [Google Colab](https://colab.research.google.com/notebook#create=true&language=python3).  
Also, be aware that some bullets points highlighted below imply a basic understanding of different mathematics concepts. I highly recommend you read/keep aside this article on Statistics Basics if you are not confident with mathematics.

Here are the different steps we are going to go through:

**[I. Data loading and overview](#one)**  
• [a. Loading the data in a dataframe](#one-a)  
• [b. Overview](#one-b)

**[II. Data cleaning](#two)**  
• [a. Duplicated and missing values](#two-a)  
• [b. Deal with outliers](#two-b)

**[III. Features engineering](#three)**  
• [a. Features transformations](#three-a)  
• [b. Dimensional reductions](#three-b)  

**[IV. Model selection](#four)**  
• [a. Split and Metric](#four-a)  
• [b. Try different models](#four-b)  
• [c. Hyperparameters tuning](#four-c)  

**[V. Training and predictions](#six)**

---

<a id="one"></a>
## I. Data loading and overview

In supervised Machine Learning, we want to build a model capable of predicting a variable called the <mark>**target**</mark> thanks to the others, called the <mark>**features**</mark>. To train this model, we need data. We will use *pandas library* to store them is a *dataframe* so we can process them easily.

> Note: When target values are provided (i.e. data are labeled), we talk about **supervised learning**. But sometimes they aren't. In this case, we talk about **unsupervised learning**. We will run **clustering** algorithms designed to be self-determined and capable of building groups by finding statistical properties patterns between the records.

<a id="one-a"></a>
### a. Loading the data in a dataframe

```python
#import the essential libraries we'll need to build an effective model
import os
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
import seaborn as sns

%matplotlib inline
```

```python
FILEPATH = os.path.join('data', 'dataset.csv')

#Store the data from CSV to a pandas dataframe
df = pd.read_csv(FILEPATH, index_col=0)
df.head(3)
```

| Feature 1      | Feature 2      | Target         |
| :------------: | :------------: | :------------: |
| 10             | 20             | 35             |
| 20             | 30             | 5              |
| 15             | 10             | 20             |

<a id="one-b"></a>
### b. Overview

Now that we loaded our dataframe, let's try to understand the problem.

We have rows for the records and columns for the features/target.

**What do we want to predict?**

Excluding **neural networks**, there are 2 kind of algorithms in supervised learning: **classification** and **regression** models.  
If the target is **categorical** (defined number of classes), we will build a classification model. If the target is **numerical** (continuous quantitative value), we will build a regression model.

To summarize:  
- **categorical** target: **classification** model
- **numerical** target: **regression** model

---

<a id="two"></a>
## II. Data cleaning

In this part, we aim at cleaning our dataframe so we can train our model efficiently.

<a id="two-a"></a>
### a. Duplicated and missing values

Sometimes records (=rows) are duplicated. You just need to remove them by running the code bellow.

```python
#Count the number of duplicated rows
df.duplicated().sum()

#Drop the duplicates
df.drop_duplicates()
```

You can also find missing values (NaN) that you can choose to remove, or try to fill (but we won't see how here).

```python
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
- data points that fall outside of 3 standard deviations, using z-score...

Once again keep in mind that it won't work for every data. Let's say we have minimum values of 0. Using IQR would remove them. Now, what if we are working on a number of trips? People who have never traveled won't be legit outliers for sure. Try to stick to the context while removing outliers.

```python
#Delete univariate outliers using sigma-clipping operations
quartiles = np.percentile(df['feature'], [25, 50, 75])
mu = quartiles[1]
sig = 0.74 * (quartiles[2] - quartiles[0])

df = df.query('(feature > @mu - 5 * @sig) & (feature < @mu + 5 * @sig)')

#Delete univariate outliers using IQR
quartiles = np.percentile(df['feature'], [25, 50, 75])
Q1 = quartiles[0]
Q3 = quartiles[2]
IQR = Q3 - Q1

df = df.query('(feature > (@Q1 - 1.5 * @IQR)) & (feature < (@Q3 + 1.5 * @IQR))')
```

---

<a id="three"></a>
## III. Features engineering

<a id="three-a"></a>
#### a. Features transformations

**• Type transformations**: 

**• Features creations**: 

**• Features deletions**: 

**• Features standardization**: 


<a id="three-b"></a>
#### b. Dimensional reductions 

**• Correlations**:  
Une fois les variables catégorielles encodées, on peut faire une matrice de corrélation (de Pearson). Par soucis de performance en terme de vitesse d'execution, il est recommandé de réduire au maximum le nombre de features. Dans ce cas, on conservera des features fortement corréllées à la target et très peu entre elles pour apporter chacune des informations intéressantes.

**• Features creations and deletions**:  
In terms of performance, having data of high dimensionality is problematic because (a) it can mean high computational cost to perform learning and inference and (b) it often leads to overfitting (http://en.wikipedia.org/wiki/Ove...) when learning a model, which means that the model will perform well on the training data but poorly on test data. Dimensionality reduction addresses both of these problems, while (hopefully) preserving most of the relevant information in the data needed to learn accurate, predictive models.

---

<a id="four"></a>
## IV. Modeling

<a id="four-a"></a>
### a. Define a metric

For **regressions problems**, there are basic metrics such as **MSE (Mean Squared Error)** or **accuracy score**.
But these metrics won't fit any supervised learning problem. Let's consider a binary classification model built on a highly imbalanced dataset (90% of class 1, 10% of class 2). In this case, using accuracy score as a performance measure wouldn't be a good idea at all. Indeed, a default prediction of class 1 for all data points would lead to a classifier which is 90% accurate, even though the classifier has not learned anything about the classification problem and will not help to predict anything! In this cases, you have to consider suitable metrics such as *precision-recall* or *ROC-AUC*.

<a id="four-b"></a>
### b. Try different models

```python
#Import and name the Random Forest model
from sklearn.ensemble import RandomForestRegressor
rf_model = RandomForestRegressor()

#Apply the model to the splitted data
rf_model.fit(X_train, y_train)

#Import the Cross Validation method to evaluate the fitting quality
from sklearn.model_selection import cross_val_score

#Evaluate our model with 5 different splits
cv_score = cross_val_score(rf_model, X, y, cv=5)

#Print the scores
print(cv_score), print(np.mean(cv_score))

#Output:
    #[0.77872018 0.7801329  0.77988107 0.78049745 0.77904688]
    #0.7796556968369478
```

<a id="four-c"></a>
### c. Hyperparameters tuning  

```python
#Let's try to find the best combination of hyperparameters with RandomizedSearchCV
from sklearn.model_selection import RandomizedSearchCV

#Set up different hyperparameters to try
random_grid = {
    'n_estimators': [int(x) for x in np.linspace(start = 5, stop = 20, num = 16)],
    'max_features': ['auto', 'sqrt'],
    'max_depth': [int(x) for x in np.linspace(10, 110, num = 11)],
    'min_samples_split': [2, 5, 10],
    'min_samples_leaf': [1, 2, 4],
    'bootstrap': [True, False],
    }

#Launch the search
random_cv = RandomizedSearchCV(estimator = rf_model, param_distributions = random_grid, n_iter = 100, cv = 3, verbose=2, random_state=42, n_jobs = -1)

#Print the best combination of hyperparameters
print(random_cv.best_params_)
```

---

<a id="five"></a>
## V. Training and prediction
