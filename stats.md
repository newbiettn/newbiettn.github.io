---
title: Statistics
layout: page
---

##### Table of Contents
  * [Paired t\-test](#paired-t-test)
  * [Analysis of Variance (ANOVA)](#analysis-of-variance-anova)
    * [1\. One\-way ANOVA](#1-one-way-anova)
    * [2\. Two\-way ANOVA](#2-two-way-anova)
  * [Chi\-squared test](#chi-squared-test)
  * [McNemar's test](#mcnemars-test)

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

__Implementation in R:__

```R
> a <- c(2,3,7,2,6)
> b <- c(10, 8, 7, 5, 10)
> c <- c(10, 13, 14, 13, 15)
>
> val <- c(a, b, c)
> cat <- c(rep("A", 5), rep("B", 5), rep("C", 5))
> dt <- data.frame(val, cat)
> rslt <- aov(val ~ cat, data = dt)
> summary(rslt)
            Df Sum Sq Mean Sq F value   Pr(>F)    
cat          2  203.3   101.7   22.59 8.54e-05 ***
Residuals   12   54.0     4.5                     
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
```

According to the founded p-value, we reject the null hypothesis $H_0$, which suggests that there are differences among groups. _However, it is important to know that ANOVA does not tell which groups differ, only that at least 2 groups among 3 differ._

#### 2. Two-way ANOVA

Use two-way ANOVA when we have __2 factors__ in the data.

```R
> delivery.df = data.frame(
+   Service = c(rep("Carrier 1", 15), rep("Carrier 2", 15),
+               rep("Carrier 3", 15)),
+   Destination = c(rep(c("Office 1", "Office 2", "Office 3",
+                         "Office 4", "Office 5"), 9)),
+   Time = c(15.23, 14.32, 14.77, 15.12, 14.05,
+            15.48, 14.13, 14.46, 15.62, 14.23, 15.19, 14.67, 14.48, 15.34, 14.22,
+            16.66, 16.27, 16.35, 16.93, 15.05, 16.98, 16.43, 15.95, 16.73, 15.62,
+            16.53, 16.26, 15.69, 16.97, 15.37, 17.12, 16.65, 15.73, 17.77, 15.52,
+            16.15, 16.86, 15.18, 17.96, 15.26, 16.36, 16.44, 14.82, 17.62, 15.04)
+ )
> head(delivery.df)
    Service Destination  Time
1 Carrier 1    Office 1 15.23
2 Carrier 1    Office 2 14.32
3 Carrier 1    Office 3 14.77
4 Carrier 1    Office 4 15.12
5 Carrier 1    Office 5 14.05
6 Carrier 1    Office 1 15.48
> rslt <- aov(Time ~ Service * Destination, data = delivery.df)
> summary(rslt)
                    Df Sum Sq Mean Sq F value   Pr(>F)    
Service              2 23.171  11.585 161.560  < 2e-16 ***
Destination          4 17.542   4.385  61.155 5.41e-14 ***
Service:Destination  8  4.189   0.524   7.302 2.36e-05 ***
Residuals           30  2.151   0.072                     
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
```

__References:__

1. [https://www.youtube.com/watch?v=-yQb_ZJnFXw](https://www.youtube.com/watch?v=-yQb_ZJnFXw)

2. [https://www.learner.org/courses/againstallodds/pdfs/AgainstAllOdds_StudentGuide_Unit31.pdf](https://www.learner.org/courses/againstallodds/pdfs/AgainstAllOdds_StudentGuide_Unit31.pdf)

3. [http://www.stat.columbia.edu/~martin/W2024/R3.pdf](http://www.stat.columbia.edu/~martin/W2024/R3.pdf)


---

### Chi-squared test

The Chi-squared test is for categorial data to test how likely it is that an observed distribution is due to chance. The test is also known as __goodness of fit test__, because it measures how well the observed distribution fit the distribution that is expected if the variables are independent.

__Example:__

{% include image.html url="/images/sample-data-chisquare.png" description="Figure 1. Contingency table of the data" %}

Suppose we wish to determine if there is a relationship between _attending the class_ and _passing the course_. We will use Chi-squared test to figure out.

What Chi-squared test actually does is making a comparison between the percentage of the <span style="color: red; font-weight: bold">red</span> and <span style="color: green; font-weight: bold">green</span> percentages, which means comparing the percentage of students who attended the class but still fails, and the percentage of student skipped the class and fail. If those percentages are equal, the chi-squared test statistic is zero. It would mean that there is no relationship between attending the class and failing the course.

In our example, we can see that two percentages are not the same, so we could expect that there is an underlying relationship between two given variables.

- __Null hypothesis $H_0$__: _Passing the class_ and _Attending the course_ are independent, which implies there is no association between two variables.
- __Alternative hypothesis $H_A$__: two variables are dependent, which implies there is a relationship between them.

```R
> mtrx <- matrix(c(25, 6, 8, 15), byrow = T, nrow = 2, ncol = 2)
> colnames(mtrx) <- c("Pass", "Fail")
> rownames(mtrx) <- c("Attended", "Skipped")
> tbl <- as.table(mtrx)
> tbl
         Pass Fail
Attended   25    6
Skipped     8   15
> chisq.test(tbl, correct = F)

	Pearson's Chi-squared test

data:  tbl
X-squared = 11.686, df = 1, p-value = 0.0006297
```

Because p-value << 0.05, we reject the null hypothesis and accept the alternative hypothesis. The outcome of the test suggests that __attending the class__ and __passing the course__ are two dependent variables.

__References:__

1. [http://www.ling.upenn.edu/~clight/chisquared.htm](http://www.ling.upenn.edu/~clight/chisquared.htm)
2. [http://www.theanalysisfactor.com/difference-between-chi-square-test-and-mcnemar-test/](http://www.theanalysisfactor.com/difference-between-chi-square-test-and-mcnemar-test/)

---

### McNemar's test

McNemar's test is essentially __a paired version of Chi-squared test__. We, for example, can use McNemar's test to test if the number of participants were significantly changed after and before an experiment.

McNemar's test can only be applied to 2x2 table, rather than larger tables like Chi-squared test. More importantly, unlike Chi-squared test by which we can use to test the independence/dependence between two categorial variables, we use McNemar's test to _test for consistency in response across two variables_.

Our example is introduced in Figure 1. We will use McNemar's test to verify whether or not the treatment is effective. What McNemar's test essentially does is recognizing if the number of people move from Yes to No and vice versa randomly. To do that, McNemar's test ignores the number of patients who are consistently Yes and No before and after the treatment. Instead the test steers its focus on the number of patients changing answers. In Figure 1, the focus is <span style="color: purple">purple cells</span>.

__Note:__ we do not use _row percentage_ in McNemar's test as in Chi-squared test. McNemar's test directly compares the numbers.

{% include image.html url="/images/joint-pain-mcnemar-test.png" description="Figure 1" %}

- __Null hypothesis $H_0$__: The treatment has no effect.
- __Alternative hypothesis $H_A$__: The treatment has some effects.

```R
> mtrx <- matrix(c(215, 75, 785, 380), byrow = T, nrow = 2, ncol = 2)
> colnames(mtrx) <- c("No", "Yes")
> rownames(mtrx) <- c("No", "Yes")
> tbl <- as.table(mtrx)
> tbl
     No Yes
No  215  75
Yes 785 380
> mcnemar.test(tbl, correct = F)

	McNemar's Chi-squared test

data:  tbl
McNemar's chi-squared = 586.16, df = 1, p-value < 2.2e-16
```

Because the p-value is $\ll 0.05$, we reject $H_0$ and accept $H_A$, which suggests that there were some effects of the treatment.

__References:__

1. [http://www.theanalysisfactor.com/difference-between-chi-square-test-and-mcnemar-test/](http://www.theanalysisfactor.com/difference-between-chi-square-test-and-mcnemar-test/)
2. [https://en.wikipedia.org/wiki/McNemar%27s_test](https://en.wikipedia.org/wiki/McNemar%27s_test)
3. [http://yatani.jp/teaching/doku.php?id=hcistats:chisquare](http://yatani.jp/teaching/doku.php?id=hcistats:chisquare)
