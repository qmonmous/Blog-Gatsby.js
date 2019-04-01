---
layout: post
title: An introduction to (supervised) Machine Learning by the workflow
author: Quentin Monmousseau
tags: [Data Science]
image: img/brain.jpg
date: 2019-03-02T23:46:37.121Z
draft: false
---

In this article, we are going to see how you should organize your machine learning workflow. It also includes some examples using python's libraries.

---

Before starting, you need to set up your developing environment. If you didn’t, please follow this easy tutorial to get started.
Also, be aware that some bullets points highlighted below imply a basic understanding of different mathematics concepts. I highly recommend you read/keep aside this article on Statistics Basics if you are not confident with mathematics.

Here are the different steps we are going to go through:

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
- Metrics/Loss function selection
- Model validation

**[V. Hyperparameters tuning](#five)**

**[VI. Training and predictions](#six)**

<a id="one"></a>
## I. Data loading and overview<br><br>

In supervised Machine Learning, we want to build a model capable of predicting a variable called the <mark>**target**</mark> thanks to the others, called the **features**. To train this model, we need data. We will use *pandas library* to store them is a *dataframe* so we can process them easily.

> Note: When target values are provided (i.e. data are labeled), we talk about **supervised learning**. But sometimes there aren't. In this case, we will talk about **unsupervised learning**. We will run **clustering algorithms** to find patterns in data and build groups.

<a id="one-a"></a>
### a. Loading the data<br><br>

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
df.head(2)
```

| Feature 1      | Feature 2      | Target         |
| :------------: | :------------: | :------------: |
| Division 1     | Division 2     | Division 3     |
| Division 1     | Division 2     | Division 3     |
| Division 1     | Division 2     | Division 3     |

<a id="one-b"></a>
### b. Overview

Now that we loaded our dataframe, let's try to understand the problem.

**What do we want to predict?**

Excluding **neural networks**, there are 2 kind of algorithms in supervised learning: **classification** and **regression**. If the target is **categorical**, we will 
- **categorical** (qualitative) : who/what/what kind  
- **numerical** (quantitative) : how much 

---

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

Once again keep in mind that it won't work for every data. Let's say we have minimum values of 0. Using IQR would remove them. Now, what if we are working on a number of trips? People who have never traveled won't be legit outliers for sure. Try to stick to the context while removing outliers.

```python
#Delete univariate outliers using sigma-clipping operations
quartiles = np.percentile(df['feature'], [25, 50, 75])
mu = quartiles[1]
sig = 0.74 * (quartiles[2] - quartiles[0])

df = df.query('(feature > @mu - 5 * @sig) & (feature < @mu + 5 * @sig)')

#Delete univariate outliers using IQR

```

---

## III. Features engineering
- Transformations
  - Categorical data: One-hot encoding, 
  - Standardization
- Features creations and deletions
- Dimensional reductions

Une fois les variables catégorielles encodées, on peut faire une matrice de corrélation (de Pearson). Par soucis de performance en terme de vitesse d'execution, il est recommandé de réduire au maximum le nombre de features. Dans ce cas, on conservera des features fortement corréllées à la target et très peu entre elles pour apporter chacune des informations intéressantes.

---

## IV.

b. Metrics/Loss function



**Classifications problems:**
- **MSE (Mean Squared Error)**:

- **accuracy score**:
Using accuracy as a performance measure for highly imbalanced datasets is not a good idea. For example, if 90% points belong to the true class in a binary classification problem, a default prediction of true for all data points leads to a classifier which is 90% accurate, even though the classifier has not learned anything about the classification problem at hand!

- **Precision Recall**:

- **ROC AUC**:
