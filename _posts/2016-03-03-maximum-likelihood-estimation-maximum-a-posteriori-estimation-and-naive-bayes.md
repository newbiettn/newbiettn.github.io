---
id: 329
title: Maximum Likelihood Estimation, Maximum a Posteriori Estimation and Naive Bayes (part 1)
date: 2016-03-03T16:48:25+00:00
layout: post
tags:
  - least squares
  - likelihood
  - maximum a posteriori
  - maximum likelihood
  - naive bayes
  - probability
---
There are some notes with regards to three important concepts &#8211; Maximum Likelihood Estimation (MLE), Maximum a Posterior Estimation (MAP), and Naive Bayes (NB) &#8211; that I would like to put here in order to remind me in case necessary. I also can see that my note can be fruitful to anyone having a hard time in grasping these concepts, because we&#8217;re gonna unfold plot twists of them that confused me while learning in this writing.

### Maximum Likelihood Estimation (MLE)

#### Probability vs Likelihood

First of all, before diving deep, it is essentially important to understand differences between two terms &#8220;likelihood&#8221; and &#8220;probability&#8221;, at least for the given concept. Thank to <a href="http://statgen.iop.kcl.ac.uk/bgim/mle/sslike_3.html" target="_blank">this article</a>, we can have a good explanation as follows.

  * **Likelihood**: Observation of data -> Estimation of parameters **(I)**
  * **Probability**: Knowing parameters  -> Prediction of outcome **(II)**

If it&#8217;s still unclear, take another orthogonal but obvious example that involves another estimation method, called Least Squares Estimation (LSE), which is more preferable than MLE in case the data is linear. The way LSE works is straightforward: to estimate unknown parameters $\theta\_{j}$ of fitting hypothesis functions $ h\_{\theta_{j}} (x)$ for given a set of data by minimising the sum of squared errors. In **Figure 1** we have a good illustration of this idea. In this example, by using linear regression, which is the method of least squares, we are able to _**approximate**_ locally optimal values of parameters $\theta\_{0} = 3.5$ and $\theta\_{1} = 1.4$ for a linear function $ h\_{\theta\_{j}} (x) = \theta\_{0} + \theta\_{1}x$. By substituting values of $\theta\_{j}$ we would have a particular hypothesis function $ h\_{\theta_{j}} (x) = 3.5 + 1.4x$ corresponding to the blue line in the figure. Now we can use the hypothesis function we have found to **_predict_** a new value $y$ for a given $x$. Briefly we have two distinct steps as follows.

  * **Approximation**: Observation of data -> Estimation of parameters **(I&#8217;)**
  * **Prediction**: Knowing parameters  -> Prediction of value **(II&#8217;)**

Now if we roughly put steps **I** and **I&#8217;**, **II** and **II&#8217;** into comparison, we can see that they share common traits. Particularly the goal of steps **I **and **I&#8217; **is to estimate $\theta_{j}$ that either best explains or best fits the data. In contrast, in both steps **II **and **II&#8217;, **the target is to predict either value or probability by using given parameters $\theta_{j}$.

<div id="attachment_334" style="width: 310px" class="wp-caption aligncenter">
  <img class="wp-image-334 size-medium" src="http://newbiettn.com/wp-content/uploads/2016/03/Screen-Shot-2016-03-03-at-4.29.41-PM-300x290.png" alt="Figure 1 " width="300" height="290" srcset="http://newbiettn.com/wp-content/uploads/2016/03/Screen-Shot-2016-03-03-at-4.29.41-PM-300x290.png 300w, http://newbiettn.com/wp-content/uploads/2016/03/Screen-Shot-2016-03-03-at-4.29.41-PM-768x742.png 768w, http://newbiettn.com/wp-content/uploads/2016/03/Screen-Shot-2016-03-03-at-4.29.41-PM-1024x989.png 1024w, http://newbiettn.com/wp-content/uploads/2016/03/Screen-Shot-2016-03-03-at-4.29.41-PM.png 1474w" sizes="(max-width: 300px) 100vw, 300px" />

  <p class="wp-caption-text">
    Figure 1 [Photo courtesy of Wikipedia]
  </p>
</div>

I know I was messing up a bit above when using an example of LSE to illustrate the differences between Probability and Likelihood. But that&#8217;s the thing I had not realised at first when I started to learn MLE. I was wondering why the hell we need to estimate a parameter $\theta$ for a given data $D$. It did not make sense because I assumed we already have a data, we can estimate any probability with ease&#8230; It was only when I started to approach MLE from a perspective of LSE as I illustrated above, things turn to be more clear to me.

#### Definition

As said, the ultimate purpose of MLE is to choose parameter $\theta$ that best explains observed data $D$. In other words, MLE is a method to search in a space $\Omega$ to find a $\theta$ that maximise the probability of data $D$. We can formulated MLE mathematically as follows.

$$
\hat{\theta_{MLE}} = argmax \ P(D|\theta) = argmax \ P(X_{1}, X_{2}, ..., X_{n}|\theta)
$$

Given that $D$ has $n$ attributes, we can obtain

$$
P(D|\theta) = P(X_{1}, X_{2}, ..., X_{n}|\theta)
$$

By using <a href="https://en.wikipedia.org/wiki/Chain_rule_(probability)" target="_blank">chain rule</a>, we can also have

$$
P(X_{1}, X_{2}, ..., X_{n}|\theta) = P(X_{1}|X_{2}, ..., X_{n}, \theta) P(X_2, ..., X_{n} |\theta)
$$

$$
= P(X_{1}|X_{2}, ..., X_{n}, \theta) P(X_{2}|X_{3}, ..., X_{n}, \theta) P(X_3, ..., X_{n}|\theta)
$$

If repeat this process, we would obtain

$$
P(D|\theta) = P(X_{1}|X_{2}, ..., X_{n}, \theta) P(X_{2}|X_{3}, ..., X_{n},\theta)\ ...\ P(X_{n-1} | X_{n}, \theta) P(X_n|\theta)
$$

According to this equation, we can see there is an extremely big problem of computation complexity.

Given $P(X_{1}, X_{2}, X_{n}\|\theta)$, we would need $2(2^{n}-1) + 1$ parameters to describe. In practice, it is common to work with a dataset of more than 100 attributes, if not thousands. To reduce the number of parameters, MLE, MAP as well as NB fundamentally rely on an assumption that attributes $X_{1}, X_{2}, ..., X_{n}$ are independent of each other. By doing this, we can reduce a number of parameters to $2n + 1$. Thus,

$$
P(D|\theta) = P(X_{1}|\theta) P(X_{2}|\theta)\ ...\ P(X_{n-1} |\theta) P(X_n|\theta)
$$

$$
= \prod_{i=1}^n P(X_i|\theta)
$$

Finally we have
$$
\hat{\theta_{MLE}} = argmax \prod_{i=1}^n P(X_i|\theta)
$$
