---
title: "Gradient Boost (part 1): Regression"
author: "Ngoc Tran Trung"
date: "18 March 2021"
comments: yes
layout: post
---

# 1. Introduction

Gradient boost or gradient boosted machines (GBM) are a widely used ML algorithm. There are two parts of GBM: "gradient" and "boost". Here the term "gradient" refers to the use of gradient descent in the algorithm while "boost" reflects the boosting mechanism of the algorithm, that is, using an ensemble of weak successive trees to _boost_ the prediction performance.

While an algorithm like Random Forest (RF) builds __an ensemble of deep & independent trees using bagging approach__, GBM builds __an ensemble of weak & successive tree using boosting approach__ where each tree takes into account the error of the previous trees.

# 2. Algorithm

In the following, we describe GBM *for regression*:

__Input:__ a training dataset ${(x_i, y_i)}_{i=1}^n$ and a differentiable loss function $\mathcal{L} = (y_i, F(x_i))$.

__Step 1:__ Initialize model with a constant value:

$$
F_0(x) = \operatorname*{arg\,min}_\gamma \sum_{i=1}^{n} \mathcal{L}(y_i, \gamma)
$$

__Step 2:__ For $m = 1$ to $M$ (maximum number of trees):

__(A)__ Compute pseudo-residual 

$$
r_{im} = - \Bigg[ \frac{\partial L(y_i, F(x_i))}{\partial F(x_i)} \Bigg]_{F(x) = F_{m-1}(x)} \tag{for $i = 1, ..., n$}
$$

__(B)__ Fit a weaker learner to the values $r_{im}$ and create terminal regions $R_{jm}$, for $j = 1 ... J_m$

__(C)__ For $j = 1 ... J_m$ compute:

$$
\gamma_{jm} = \operatorname*{arg\,min}_\gamma \sum_{x_i \in R_{ij}} \mathcal{L}(y_i, F_{m-1}(x) + \gamma)
$$

__(D)__ Update $F_{m}(x) = F_{m-1}(x) + \nu \sum_{j=1}^{J_m} \gamma_m I(x \in R_{jm})$

# 3. Algorithm Exposition

# 3.1. Input

The input is a training dataset and a __loss function__ $\mathcal{L}$, which is often squared error for regression:

\begin{equation} \label{eq:loss-func}
\bbox[15px, border:1px solid black]{
\mathcal{L}(y_i, F(x_i)) = \frac{1}{2} \sum_{i=0}^{n} \bigg[ y_i - F(x_i) \bigg]^2
}
\end{equation}

We also take the derivative of the loss function shown in \eqref{eq:loss-func} for later use:

\begin{equation} \label{eq:derivative-loss-func}
\bbox[15px, border:1px solid black]{
\frac{\partial \mathcal{L}}{\partial F(x_i)} = \frac{\partial}{\partial F(x_i)} \frac{1}{2} \sum_{i=0}^{n} \bigg[ y_i - F(x_i) \bigg]^2 = - \sum_{i=0}^{n} y_i - F(x_i) 
}
\end{equation}

# 3.2. Step 1:

This step indicates that **a GBM model always starts with a single leaf of a constant value**. It is a important difference when we compare GBM with other boosting learners such as AdaBoost when Adaboost always starts with a stump, *i.e.* a very short tree!

In this step, $\gamma$ is a single prediction. By the term $\operatorname*{arg\,min}_\gamma$, it means that we have to find a value for $\gamma$ that $\mathcal{L}(y_i, \gamma)$ attains its minimum. 

$$
F_{0}(x) = \frac{1}{2} \sum_{i=1}^n (y_i - \gamma)^2
$$

To do that, we have to solve $\frac{\partial F_{0}}{\partial \gamma} = 0$. Using the derivative we prepared in \eqref{eq:derivative-loss-func}, we can have:

$$
\begin{align}
\frac{\partial F_{0}}{\partial \gamma} &= 0 \\
- \sum_{i=0}^{n} y_i - \gamma &= 0 \\
\end{align}
$$

\begin{equation}
\bbox[15px, border:1px solid black]{
\gamma = \frac{\sum_{i=0}^{n} y_i}{n} 
}
\end{equation}

So, as it turns out that, when we use mean squared error as a loss function, $F_{0}(x)$ is simply the average value of all target values!

## Step 2: 

In this step, we construct $M$ sequential trees.

### (A) 

$r_{im} = - \Bigg[ \frac{\partial L(y_i, F(x_i))}{\partial F(x_i)} \Bigg]$ is basically a derivative of the loss function $\mathcal{L}$ with respect to the predicted value $F(x_i)$. 

Using the derivative shown in \eqref{eq:derivative-loss-func}, we have 

$$
\begin{align}
r_{im} &= - \Bigg[ \frac{\partial L(y_i, F(x_i))}{\partial F(x_i)} \Bigg] \\
r_{im} &= - \bigg[ -(y_i - F(x_i)) \bigg] \\
\end{align}
$$

\begin{equation}
\bbox[15px, border:1px solid black]{
r_im = y_i - F(x)
}
\end{equation}

Here, $r_{im}$ __is basically a residual__, *i.e.* a difference between an actual and a predicated value.

Also, the definition of $r_{im}$ indicates that $F(x) = F_{m-1}(x)$, which is the predicated value made by the previous tree. Note that when $m=1$ (the first tree), $F(x) = F_0(x)$, which is the average value we have mentioned above.

__In general, the purpose of step (A) is to calculate residual for each data sample $x_i$ in the training set.__ And because $r_{im}$ is a gradient, that's why this algorithm is named Gradient Boosting.

### (B) 

The goal of step (B) is to build a weak learner -- a regression tree -- **that predicts the residuals found in step (A)**. 

Next, terminal regions $R_{jm}$ refer to leaves of the tree, *i.e.* each leaf corresponding to each terminal region, where $J_m$ is the total number of leaves: if the tree has 2 leaves then $J_m = 2$, or 3 leaves $J_m = 3$, etc.

__In short, the general idea of this step is to fit a regression tree to the residuals we have calculated in step (A), and finally label the leaves of the new tree.__

### (C)

For each leaf $j$ of $J_m$ leaves of the new tree, we compute $\gamma_{jm}$, which is the predicted value for the leaf $j$.

Note that the loss function in this step $\mathcal{L}(y_i, F_{m-1}(x) + \gamma)$ is slightly different to our original loss function $\mathcal{L} = (y_i, F(x_i))$. In particular, in this new loss function, the predicted value is $F_{m-1}(x) + \gamma$ where $F_{m-1}(x)$ is the most recent predicted value.

After some calculation, we have:

\begin{equation} \label{gamma_jm}
\bbox[15px, border:1px solid black]{
\gamma_{jm} = \frac{\sum_{x_i \in R_{ij}} F_{m-1}(x) - y_i}{k} \\
}
\end{equation}

where $k$ is the total number of residuals in the $j$ leaf.

Basically $\gamma_{jm}$ is the average value of the residuals of the considering leaf $j$.

__In general, step (C) is to compute the output residual $\gamma_{jm}$ for each leaf $j$ of the current tree $m$.__

### (D)

In step (D), we make a new prediction $F_m(x)$. 

Here $F_m(x)$ is a sum of the previous prediction $F_{m-1}(x)$ (made by the previous tree $m-1$) and the new prediction made by the tree $m$ we have constructed.

In the equation, we have $\nu$, which is a learning rate. This parameter $\nu$ is to scale the effect of the new tree on the final prediction $F_m(x)$.
