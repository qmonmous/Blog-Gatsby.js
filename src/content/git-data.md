---
layout: post
title: An introduction to Git for beginners
author: Quentin Monmousseau
tags: [Data Science]
image: img/brain.jpg
date: 2019-03-02T23:46:37.121Z
draft: false
---

In this article, we are going to see how you should organize your machine learning workflow. It also includes some examples using python's libraries.

**[I. Get started with Git](#one)**
- [a. Create your first repository](#one-a)
- [b. The wedding analogy](#one-b)

**[II. Collaborate with your team](#two)**
- [a. Branchs](#two-a)
- [b. Pull requests](#two-b)

---

<a id="one"></a>
## I. Get started with Git

<a id="one-a"></a>
### a. Branchs

We 
`git checkout -b newbranch`

We create an index.html in newbranch
Check if the the file is here
Now let's move back to our master branch
The file isn't here, we are designing a new feature on our new branch.
Let's add some stuff in our feature and push it to github. Now let's say it's over and we want it on the branch master. .......?......

<a id="one-b"></a>
### b. Overview

TODO

---

<a id="two"></a>
## Collaborate with your team

<a id="two-a"></a>
### a. Duplicated and missing values

TODO

```python
#Count the number of duplicated rows
> df.duplicated().sum()
```

<a id="two-b"></a>
### b. Deal with outliers

TODO

... or by considering outliers as:
- mistyped data points, using sigma-clipping operations.
- data points that fall outside of 1,5*IQR above the 3rd quartile and below the 1st quartile.
- data points that fall outside of 3 standard deviations, using z-score.

Once again keep in mind that it won't work for every data. Let's say we have minimum values of 0. Using IQR would remove them. Now what if we are working on a number of trips? People who have never traveled won't be legit outliers for sure. Try to stick to the context while removing outliers.

```python
#Delete univariate outliers using sigma-clipping operations
quartiles = np.percentile(df['feature'], [25, 50, 75])
mu = quartiles[1]
sig = 0.74 * (quartiles[2] - quartiles[0])

df = df.query('(feature > @mu - 5 * @sig) & (feature < @mu + 5 * @sig)')

#Delete univariate outliers using IQR

```

## III. Features engineering
- Transformations
  - Categorical datas: One-hot encoding, 
  - Standardization
- Features creations and deletions
- Dimensional reductions

Une fois les variables catégorielles encodées, on peut faire une matrice de corrélation (de Pearson). Par soucis de performance en terme de vitesse d'execution, il est recommandé de réduire au maximum le nombre de features. Dans ce cas, on conservera des features fortement corréllées à la target et très peu entre elles pour apporter chacune des informations intéressantes.


## IV.

b. Metrics/Loss function



**Classifications problems:**
- **MSE (Mean Squared Error)**:

- **accuracy score**:
Using accuracy as a performance measure for highly imbalanced datasets is not a good idea. For example, if 90% points belong to the true class in a binary classification problem, a default prediction of true for all data poimts leads to a classifier which is 90% accurate, even though the classifier has not learnt anything about the classification problem at hand!

- **Precision Recall**:

- **ROC AUC**: