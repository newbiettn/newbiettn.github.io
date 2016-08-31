---
title: Variables selection using Fractional Polynomials
date: 2016-08-12
layout: post
---
I had quite a time with using the algorithm Fractional Polynomial (FP) to purposefully select variables for regression model. The algorithm's idea is thoroughly explained in the paper "Multivariable regression model building by using fractional polynomials: Description of SAS, STATA and R programs". Nevertheless, the authors mainly use SAS to demonstrate, as a result making it quite a bit different when compare the output from R-language to SAS. But after a few hours, I accomplished reproducing the example using our familiar R-language.

In short, FP is an algorithm to check if the variable is linear or non-linear. 
