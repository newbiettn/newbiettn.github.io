---
title: "Prelude to Python (Part 1 - Pyenv)"
author: "Ngoc Tran Trung"
date: "03 March 2021"
comments: yes
layout: post
---

I did used python around 2013-2015 in some of my previous projects but I never actually took it seriously. In 2016 when I first arrived at Swinburne to pursue my PhD, it occured to me that I had to decide between R and Python as my primary language for data science. Since I had never used R before, I chose it instead of Python. The decision at that moment has taken me to a path on which I rarely got a chance to use Python much over the last few years.

Python is not a tricky language to learn. In fact, it is one of the easiest programming languages that even non-technical persons are still able to learn in a relatively short time. That explains why Python is the chosen language in many ``learn to code'' courses/bootcamps that we can see all over the places.

In this post, I will definitely not try to convey fundamental programming aspects of Python. I instead would like to focus on little things of Python that ones should know in order to make programming and testing Python apps easier for them.

The first topic I would like to concentrate on is how to manage multiple python versions, specifically in macOS Big Sure 11.2.

## Mac does come with multiple Python vers
It is noticeable that macOS BigSure 11.2 is shipped with both Python 2 and 3 vers. We can verify this.

```
% which python
/usr/bin/python
% python --version
Python 2.7.16
% python python3
/usr/local/bin/python3
% python3 --version
Python 3.9.2
```
