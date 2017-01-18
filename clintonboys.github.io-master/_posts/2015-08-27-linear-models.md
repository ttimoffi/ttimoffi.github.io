---
layout: post
title: Regression and stepwise selection
image:
  feature: sample-image-36.jpg
  credit: Crete, Greece, 2015
---

I had quite a bit of trouble learning statistics in my undergraduate degree, mainly because of the dry way theoretical statistics is taught. Because of this, most of my knowledge about statistics comes from what I've learnt through necessity working with data in the last year or so. 

The basics of statistics I use every day: performing tests of statistical validity and significance, using assumptions about normality for sanity checks and excluding outliers, etc. But the real power of statistics is that statistical learning lies behind almost every modern machine learning algorithm. Although these algorithms are getting easier and easier to use out of the box, they have much more meaning and adaptability when you have at least a basic understanding of the statistical mechanisms behind them.

At work recently I had to come up with a basic predictive model for a time-dependent variable. I had an immense collection of data at my disposal to use as inputs in the model: I could easily have come up with forty or fifty features to throw into an out-of-the-box algorithm in scikit-learn 