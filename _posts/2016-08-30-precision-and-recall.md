---
title: Precision and Recall
layout: post
---

Precision and recall are two important concepts in machine learning and pattern recognition. It seems easy to understand but hard to remember, at least to me.

![The Venn diagram of the confusion matrix](/images/precision-recall.jpg){:class="img-center"}

To make it easier to both understand and remember, I plot a Venn diagram and use it to explain the tuition concepts of them.

Mathematically, precison and recall are expressed as follows.

$Precision = \frac{TP}{TP + FP}$

$Recall = \frac{TP}{TP + FN}$

Intuitively, in terms of information retrieval, precision indicates how much irrelevant information the system gives the user. In the same manner, it is safe to understand that recall indicates how much relevant information the system fails to give to the user.
