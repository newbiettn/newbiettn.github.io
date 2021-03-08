---
title: "Gradient Boosting"
author: "Ngoc Tran Trung"
date: "08 March 2021"
comments: yes
layout: post
---

# 1. Introduction

Gradient boosting (GBM) is a highly popular machine learning (ML) algorithm that can be used for both classification and regression. It also has been known that GB can be used in ranking problems, such as retrieving top-$n$ web pages or documents given a search query.

There are two parts of GBM: "gradient" and "boosting". Here the term "gradient" refers to the use of gradient descent in the algorithm while "boosting" reflects the boosting mechanism of the algorithm, that is, using an ensemble of weak algorithms (usually Decision Tree) to _boost_ the prediction performance.

# 2. Algorithm

In the following, we have a formal description of GBM:


\begin{equation}
F_0(x) = \operatorname*{arg\,min}_\gamma \sum_{i=1}^{n} \mathcal{L}((y_i, \gamma)
\end{equation}

__Step 2:__ For $m = 1$ to $M$ (maximum number of trees):

__(A)__ Compute pseudo-residual 



\begin{equation}
$r_{im} = - \left[ \frac{\partial L(y_i, F(x_i))}{\partial F(x_i) \right]
\end{equation}




__(B)__ Fit a weaker learner to the values $r_{im}$ and create terminal regions $R_{jm}$, for $j = 1 ... J_m$

__(C)__ For $j = 1 ... J_m$ compute:


\begin{equation}
\gamma_{jm} = \operatorname*{arg\,min}_\gamma \sum_{x_i \in R_{ij}} \mathcal{L}(y_i, F_{m-1}(x) + \gamma)
\end{equation}



