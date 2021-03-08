---
title: "Gradient Boosting"
author: "Ngoc Tran Trung"
date: "08 March 2021"
comments: yes
layout: post
---

# Introduction

Gradient boosting (GBM) is a highly popular machine learning (ML) algorithm that can be used for both classification and regression. It also has been known that GB can be used in ranking problems, such as retrieving top-$n$ web pages or documents given a search query.

There are two parts of GBM: "gradient" and "boosting". Here the term "gradient" refers to the use of gradient descent in the algorithm while "boosting" reflects the boosting mechanism of the algorithm, that is, using an ensemble of weak algorithms (usually Decision Tree) to *boost* the prediction performance.

# Algorithm

In the following, we have a formal description of GBM:

**Input:** a training dataset ${\{(x_i, y_i)\}}_{i=1}^n$ and a differentiable loss function $\mathcal{L} = (y_i, F(x_i))$.

**Step 1:** Initialize model with a constant value:

\begin{equation}
F_0(x) = \operatorname*{arg\,min}_\gamma \sum_{i=1}^{n} \mathcal{L}((y_i, \gamma)
\end{equation}

**Step 2:** For $m = 1$ to $M$ (maximum number of trees):

**(A)** Compute pseudo-residual 

\begin{equation}
$r_{im} = - \left[ \frac{\partial L(y_i, F(x_i))}{\partial F(x_i) \right]
\end{equation}




