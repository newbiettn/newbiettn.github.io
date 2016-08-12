---
id: 453
title: Likelihood ratio test
date: 2016-08-09T11:34:05+00:00
layout: post
categories:
  - Uncategorized
---
The likelihood-ratio test is an alternative to $\chi^2$ test in order to assess the \*\*goodness of fit\*\*. It is straight-forward and easy to use.

1. Calculate $G = -2ln\bigg[\frac{likeli \, hood \, of \, the \, replacing \, model \, h\_\theta^{(1)}}{likelihood \, of \, the \, existing \, model \, h\_\theta^{(0)}}\bigg] = 2[loglikelihood \, of \, h\_\theta^{(0)} &#8211; loglikelihood \, of \, h\_\theta^{(1)}\]\$.

&#8211; Notice that $ln$ is removed by performing logarithm.

&#8211; The log-likelihood is usually outputted by R-language.

2. Because the distribution model of likelihood ratio test is nearly the same as $\chi^2$, it allows us to assign $G$-score to $\chi^2$-score.

&#8211; Mathematically we have $G$-score = $\chi^2$-score

2. Find $p$ value based on $\chi^2$ statistic obtained.

3. Determine if $h\_\theta^{(1)}$ is more statistically significant than $h\_\theta^{(0)}$ based on obtained $p$ value and the statistical significance level $\alpha$ (usually $\alpha = 0.05$)

### **Example**:

Suppose we try to fit a given dataset with two alternative models $h\_\theta^{(1)}$ and $h\_\theta^{(0)}$ where in $h\_\theta^{(1)}$ we eliminated some predictors we reckon that they are not sensitive enough. Now we try to assess if the model $h\_\theta^{(1)}$ is more statistically significant than the original model by using the likelihood ratio test.

Suppose $df = 1$.

1. Log-likelihood of

&#8211; $h_\theta^{(1)}$ is 53.6

&#8211; $h_\theta^{(0)}$ is 68.3

Therefore, we have $G = 2[68.3 &#8211; 53.6] = 29.31$

2. $\chi^2 = G = 29.31$

3. Find $p = P[\chi^2(df = 1) = 29.31\]\$ by using R

```
pchisq(29.31, 1, lower.tail = FALSE)
[1] 6.167658e-08
```


4. Because $p$-value << $\alpha = 0.05$, we can reject the null hypothesis $H\_0$ that two models are not significant different, and accept the alternative hypothesis $H\_a$ that $h\_\theta^{(1)}$ fits better than $h\_\theta^{(0)}$.
