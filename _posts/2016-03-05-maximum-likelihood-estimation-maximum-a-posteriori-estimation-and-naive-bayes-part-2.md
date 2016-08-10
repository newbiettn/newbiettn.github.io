---
id: 407
title: Maximum Likelihood Estimation, Maximum a Posteriori Estimation and Naive Bayes (part 2)
date: 2016-03-05T11:23:49+00:00
author: newbiettn
layout: post
guid: http://newbiettn.com/?p=407
permalink: /2016/03/maximum-likelihood-estimation-maximum-a-posteriori-estimation-and-naive-bayes-part-2/
categories:
  - Uncategorized
tags:
  - bayes inference
  - bayes theorem
  - machine learning
  - maximum a posteriori estimation
  - statistics
---
### Maximum a Posteriori Estimation (MAP)

Generally, MAP is a particular extension of MLE where we take in account a biased assumption of data, called **_prior knowledge_**. Specifically Bayes&#8217; theorem (or Bayes&#8217; rule) allows us to incorporate prior probability as follows

[latex display=&#8221;true&#8221;]P(\theta|D) = \frac{P(D|\theta)P(\theta)}{P(D)}[/latex]

where

<li style="margin-left: 150px;">
  [latex]P(D|\theta)[/latex] is the likelihood
</li>
<li style="margin-left: 150px;">
  [latex]P(\theta)[/latex] is the prior probability
</li>
<li style="margin-left: 150px;">
  [latex]P(D)[/latex] is the marginal likelihood
</li>
<li style="margin-left: 150px;">
  [latex]P(\theta|D)[/latex] is the posteriori likelihood
</li>

The principle of MAP is to estimate [latex]\theta[/latex] that maximise the posteriori likelihood [latex]P(\theta)[/latex]. Thus,

[latex display=&#8221;true&#8221;]\hat{\theta}_{MAP} = argmax\ \frac{P(D|\theta)P(\theta)}{P(D)}[/latex]

Notice that the denominator [latex]P(D)[/latex] is independent of [latex]\theta[/latex]. Therefore

[latex display=&#8221;true&#8221;]P(\theta|D) \propto {P(D|\theta)P(\theta)}[/latex]

[latex display=&#8221;true&#8221;] \Rightarrow \hat{\theta}_{MAP} = argmax\ {P(D|\theta)P(\theta)}[/latex]

By assuming attributes [latex]X\_{1}, X\_{2}, &#8230;, X_{n}[/latex] are independent of each other, we have

<p style="text-align: center; font-size: 20px;">
  [latex display=&#8221;true&#8221;]\hat{\theta}_{MAP} = argmax\ P(\theta) \prod_{i=1}^n P(X_i|\theta)[/latex]
</p>

&nbsp;