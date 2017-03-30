---
title: Statistics
layout: page
---

##### Table of Contents
  * [Paired t\-test](#paired-t-test)
  * [Analysis of Variance (ANOVA)](#analysis-of-variance-anova)
    * [1\. One\-way ANOVA](#1-one-way-anova)

---

### Paired t-test

A paired t-test is used to compare two population means where you have two samples in which observations in one sample can be paired with observations in the other sample. Examples of where this might occur are:

- Before-and-after observations on the same subjects (e.g. students’ diagnostic test results before and after a particular module or course).
- A comparison of two different methods of measurement or two different treatments where the measurements/treatments are applied to the same subjects.

__NOTE__: _For this test to be valid the differences only need to be approximately normally distributed. Therefore, it would not be advisable to use a paired t-test where there were any __extreme outliers__._

__Example:__

Suppose a sample of n students were given a diagnostic test before studying a particular module and then again after completing the module. We want to find out if, in general, our teaching leads to improvements in students’ knowledge/skills (i.e. test scores). We can use the results from our sample of students to draw conclusions about the impact of this module in general.


Let x = test score before the module, y = test score after the module. To test the null hypothesis that the true mean difference is zero, the procedure is as
follows:


1. Calculate the difference ($d_i = y_i − x_i$) between the two observations on each pair, making sure you distinguish between positive and negative differences.


2. Calculate the mean difference, $\bar{d}$.


3. Calculate the standard deviation of the differences, $s_d$, and use this to calculate the standard error of the mean difference, $$SE(\bar{d}) = \frac{s_d}{\sqrt{n}}$$.

4. Calculate the t-statistic, which is given by $$T = \frac{\bar{d}}{SE(d)}$$. Under the null hypothesis, this statistic follows a t-distribution with $n − 1$ degrees of freedom.

5. Use tables of the t-distribution to compare your value for $T$ to the $t_{n−1}$ distribution. This will give the p-value for the paired t-test.

{% include image.html url="/images/paired-t-test-example.png" description="Example data" %}

Calculating the mean and standard deviation of the differences gives: $\bar{d} = 2.05$ and $sd = 2.837$. Therefore, $SE(\bar{d}) = \frac{s_d}{\sqrt{n}} = \frac{2.837}{\sqrt{20}} = 0.634$.

Looking this up in tables gives $p = 0.004$. Therefore, there is strong evidence that, on average, the module does lead to improvements.

```R
> x <- c(18, 21, 16, 22, 19, 24, 17, 21, 23, 18, 14, 16, 16, 19, 18, 20, 12, 22,
+        15, 17)
> y <- c(22, 25, 17, 24, 16, 29, 20, 23, 19, 20, 15, 15, 18, 26, 18, 24, 18, 25,
+        19, 16)
> t.test(x, y, paired = T)

	Paired t-test

data:  x and y
t = -3.2313, df = 19, p-value = 0.004395
alternative hypothesis: true difference in means is not equal to 0
95 percent confidence interval:
 -3.3778749 -0.7221251
sample estimates:
mean of the differences
                  -2.05
```

__Required assumptions for the t-test__

1. __Normality or pseudo-normality__ Dataset with $\gt 30$ instances should be sufficient.

2. __Randomness of the Samples__ Instances are randomly chosen from the population.

3. __Equal variances of the populations__ The t-test assumes that the samples come from populations with equal variance. Can be verified by tests such as F test, Bartlett’s test, Levene’s test, or the Brown–Forsythe test.

---

### Analysis of Variance (ANOVA)

ANOVA is a statistical procedure to __compare  > 2 means__.

#### 1. One-way ANOVA

__Example:__

Test if the amount of __caffeine__ consumed affected __memory__. The participants were divided into __3__ groups randomly (1) Coca-cola classic (34mg), (2) McDonald's coffee (100mg), and (3) Jolt energy (160mg). After drinking the beverage, the participants were given a memory test - words remembered from a list. The data is in __Figure 1__.

{% include image.html url="/images/caffeine-anova-data.png" description="Figure 1. Number of words recalled in memory test" %}

__a) Step 1__:

- __Factor__: cafeine.
- __Levels__: 3.
- __Response variable__: memory.
- __Null hypothesis $H_0$__: means among the groups are the same, i.e., $\mu_A = \mu_B = \mu_C$.
- __Alternative hypothesis $H_a$__: means are not the same.

__b) Step 2__:

There is 2 components of variation in the number of words remembered by the participants.

- __Between-group variation__: variation in the number of words recalled _among the 3 groups_.
- __Within-group variation__: variation in the number of words _among participants of each group_.

To measure 2 components, we will compute __Sum of Squares within Groups (SSWG)__ (Figure 2) and __Sum of Squares between Groups (SSBG)__ (Figure 3).

{% include image.html url="/images/variation-between-group-anova.png" description="Figure 2. We can measure between-group variation by SSBG" %}

{% include image.html url="/images/variation-within-group-anova.png" description="Figure 3. We can measure within-group variation by SSWG" %}

Thus, we have __Total Sum of Squares (TSS)__ = __SSWG__ + __SSBG__ (Figure 4).

{% include image.html url="/images/tss-anova.png" description="Figure 4. Total Sum of Squares" %}

__c) Step 3__:

- Compute __SSWG__.

{% include image.html url="/images/sswg-anova.png" description="Figure 5. Calculate SSWG. Note that 4, 8 and 13 are means of each group respectively." %}

- Compute __SSBG__.

{% include image.html url="/images/ssbg-anova-calculate.png" description="Figure 5. Calculate SSBG. Note that 4, 8 and 13 are means of each group respectively." %}

- Compute __TSS__.

{% include image.html url="/images/tss-anova-calculate.png" description="Figure 6. Calculate TSS. Note that 8.3 is the mean of 3 groups together." %}

__d) Step 4__:

Calculate $F$ score.

{% include image.html url="/images/f-score-anova.png" description="Figure 7. Calculate F-score." %}

{% include image.html url="/images/f-score-anova-calculate.png" description="Figure 7. Find the critical value corresponding to F-score." %}

__References:__

1. https://www.youtube.com/watch?v=-yQb_ZJnFXw
