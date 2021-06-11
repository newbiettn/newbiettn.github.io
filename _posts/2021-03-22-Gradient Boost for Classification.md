---
title: "Gradient Boost (part 2): Classification"
author: "Ngoc Tran Trung"
date: "22 March 2021"
comments: yes
layout: post
---

## 1. Introduction

In the 1st part of the Gradient Boost (GBM) series, we covered how GBM handles regression. In this following part, we will shed light on how the algorithm works for classification problems.

## 2. Algorithm Exposition

### 2.1. Loss function

The algorithm idea of GBM for classification remains the same as in the previous part, except that we need to use a different kind of loss function. Usually, we use __log loss__, which is defined as follows:


\begin{equation}
\bbox[15px, border:1px solid black]{
\mathcal{L} = \sum_{i=1}^n - y_i log(p_i) - (1-y_i) log(1 - p_i)
}
\end{equation}

where $y_i$ is the actual value and $p_i$ is the predicted probability of the model for $y_i$.

The above loss function is of the predicted probability. We need to make a small tranformation so that it is the function of the predicted __log(odds)__.

$$
\begin{align}
\mathcal{L} &= \sum - y_i log(p_i) - (1-y_i) log(1 -p_i) \\
&= \sum - y_i log(p_i) - log(1-p_i) + y_i log(1-p_i) \\
&= \sum - y_i \Big[ log(p_i) - log(1-p_i) \Big] - log(1-p_i) \\
&= \sum - y_i log(\frac{p_i}{1-p_i}) - log(1-p_i)
\end{align}
$$

Since the definition of odds is $odds = \frac{p}{1-p}$, we have:

$$
\begin{align}
log(odds) = log(\frac{p}{1-p}) \\
\Leftrightarrow e^{log(odds)} = \frac{p}{1-p} \\
\Rightarrow p = \frac{e^{log(odds)}}{1+e^{log(odds)}} 
\end{align}
$$

Thus, our loss function now is:

$$
\begin{align}
\mathcal{L} &= \sum -y_i log(odds) - log(1 - \frac{e^{log(odds)}}{1-e^{log(odds)}}) \\
&= \sum -y_i log(odds) - log(\frac{1}{1-e^{log(odds)}}) \\
&= \sum -y_i log(odds) + log(1 + e^{log(odds)})
\end{align}
$$

The loss function that uses $log(odds)$ is as follows:

\begin{equation} \label{eq:loss-func-with-log-odds}
\bbox[15px, border:1px solid black]{
\mathcal{L} = \sum -y_i log(odds) + log(1 + e^{log(odds)})
}
\end{equation}

### 2.2. The derivative of the loss function

We take the derivative of the above loss function for later use:

$$
\begin{align}
\frac{\partial \mathcal{L}}{\partial log(odds)} &= \sum -y_i log(odds) + log(1 + e^{log(odds)}) \\
&= \sum y_i + \frac{1}{1+e^{log(odds)}} e^{log(odds)} \tag{since $(log(x))' = \frac{1}{x}$ and $(e^x)' = e^x$} \\
&= \sum - y_i + p_i
\end{align}
$$

In general, the derivative of the loss function \eqref{eq:loss-func-with-log-odds} is as follows:

\begin{equation} \label{eq:derivative-loss-func}
\bbox[15px, border:1px solid black]{
\frac{\partial \mathcal{L}}{\partial log(odds)} = \sum y_i + \frac{1}{1+e^{log(odds)}} e^{log(odds)} = \sum - y_i + p_i
}
\end{equation}


### 2.3. Algorithm Steps

#### 2.3.1 Step 1

In this step, we are going to find the initial value $F_0(x)$:

\begin{equation}
F_0(x) = \operatorname*{arg\,min}_\gamma \sum \mathcal{L}(y_i, \gamma) 
\end{equation}

where $\gamma$ is the $log(odds)$.

By reusing the derivative of our loss function in \eqref{eq:derivative-loss-func}, we can have:

$$
\begin{align}
\frac{\partial \mathcal{L}}{\partial log(odds)} &= 0 \\
\Leftrightarrow \sum y_i + p &= 0 \\
\Leftrightarrow p &= \frac{\sum y_i}{n}
\end{align}
$$

With $p$, we can calculate the initial predicted value in terms of __log(odds)__ with:

\begin{equation}
\bbox[15px, border:1px solid black]{
F_0(x) = \frac{p}{1 - p}
}
\end{equation}


#### 2.3.1 Step 2

#### (A) 

In this step, we are going to compute the residuals $r_{im} = - \Bigg[ \frac{\partial L(y_i, F(x_i))}{\partial F(x_i)} \Bigg]$.

By using the derivative of our loss function shown in \eqref{eq:derivative-loss-func}, we have:

$$
\begin{align}
r_{im} &= - \Big[ -y_i + \frac{1}{1+e^{log(odds)}} e^{log(odds)}) \Big] \\
&= y_i - p_i \tag{since $p = \frac{e^{log(odds)}}{1+e^{log(odds)}}$}
\end{align}
$$

As a result, we have:

\begin{equation}
\bbox[15px, border:1px solid black]{
r_{im} = y_i - p_i
}
\end{equation}

#### (B) 

The goal of step (B) is to build a weak learner -- a regression tree -- **that predicts the residuals found in step (A)**. This step is identical for both regression and classification.

#### (C)

In this step, we compute the __output value__ $\gamma_{jm}$ for each leaf that minimizes the following equation:

$$
\begin{align} 
\gamma_{jm} &= \operatorname*{arg\,min}_\gamma \sum_{x_i \in R_{ij}} \mathcal{L}(y_i, F_{m-1}(x) + \gamma) \\
&= \operatorname*{arg\,min}_\gamma \sum - y_i \Big[ F_{m-1}(x) + \gamma \Big] + log(1 + e^{F_{m-1}(x) + \gamma}) \label{eq:gamma_jm_1}
\end{align}
$$

Taking the derivative of \eqref{eq:gamma_jm_1} is hard. Instead, we use Taylor Polynomial to approximate that derivation:

$$
\mathcal{L}(y_i, F_{m-1}(x) + \gamma) \approx \mathcal{L}(y_i, F_{m-1}(x))  + \frac{\partial}{\partial F} \mathcal{L}(y_i, F_{m-1}(x)) \gamma + \frac{1}{2} \frac{\partial^2}{\partial^2 F} \mathcal{L}(y_i, F_{m-1}(x)) \gamma^2 \\
$$

Therefore,

$$
\begin{align}
\frac{\partial}{\partial \gamma} \mathcal{L}(y_i, F_{m-1}(x) + \gamma) \approx 0 + \frac{\partial}{\partial F} \mathcal{L}(y_i, F_{m-1}(x)) + \frac{\partial^2}{\partial^2 F} \mathcal{L}(y_i, F_{m-1}(x)) \gamma
\end{align}
$$

Set the derivative to 0, we can find $\gamma_jm$ as follows:

$$
\begin{align}
\Rightarrow \gamma_jm  &= -\frac{\frac{\partial}{\partial F} \mathcal{L}(y_i, F_{m-1}(x))}{\frac{\partial^2}{\partial^2 F} \mathcal{L}(y_i, F_{m-1}(x))} \\
\end{align}
$$

After some calculation of the 1st-order and 2nd-order derivatives, we can have:

\begin{equation}
\bbox[15px, border:1px solid black]{
\gamma_{jm} = \frac{y_i - p_i}{p_i (1-p_i)} = \frac{residual}{p_i (1-p_i)}
}
\end{equation}

For a leaf with multiple residuals, we have:

\begin{equation}
\bbox[15px, border:1px solid black]{
\gamma_{jm} = \frac{\sum y_i - p_i}{\sum p_i (1-p_i)} = \frac{\sum residual}{\sum p_i (1-p_i)}
}
\end{equation}


#### (D)

In step (D), we make a new prediction $F_m(x)$. 

