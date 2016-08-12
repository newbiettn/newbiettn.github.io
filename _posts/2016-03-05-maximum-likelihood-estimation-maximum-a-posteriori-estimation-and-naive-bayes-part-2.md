---
id: 407
title: Maximum Likelihood Estimation, Maximum a Posteriori Estimation and Naive Bayes (part 2)
date: 2016-03-05T11:23:49+00:00
layout: post
tags:
  - bayes inference
  - bayes theorem
  - machine learning
  - maximum a posteriori estimation
  - statistics
---
### Maximum a Posteriori Estimation (MAP)

Generally, MAP is a particular extension of MLE where we take in account a biased assumption of data, called **_prior knowledge_**. Specifically Bayes&#8217; theorem (or Bayes&#8217; rule) allows us to incorporate prior probability as follows

$$
P(\theta|D) = \frac{P(D|\theta)P(\theta)}{P(D)}
$$

In which, $P(D\|\theta)$ is the likelihood, $P(\theta)$ is the prior probability, $P(D)$ is the marginal likelihood, and $P(\theta\|D)$ is the posteriori likelihood.

The principle of MAP is to estimate $\theta$ that maximise the posteriori likelihood $P(\theta)$. Thus,

$$
\hat{\theta}_{MAP} = argmax\ \frac{P(D|\theta)P(\theta)}{P(D)}
$$

Notice that the denominator $P(D)$ is independent of $\theta$. Therefore

$$
P(\theta|D) \propto {P(D|\theta)P(\theta)}
$$

$$
\Rightarrow \hat{\theta}_{MAP} = argmax\ {P(D|\theta)P(\theta)}
$$

By assuming attributes $X\_{1}, X\_{2}, &#8230;, X_{n}$ are independent of each other, we have

$$
\hat{\theta}_{MAP} = argmax\ P(\theta) \prod_{i=1}^n P(X_i|\theta)
$$
