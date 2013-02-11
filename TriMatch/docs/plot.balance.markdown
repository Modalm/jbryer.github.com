---
layout: mathjax	
title: TriMatch
subtitle: Propensity Score Matching for Non-Binary Treatments
published: true
status: publish
submenu: trimatch
---


#### `plot.balance`: Balance plot for the given covariate. ####

#### Description ####


 If the covariate is numeric, boxplots will be drawn with
 red points for the mean and green error bars for the
 standard error. For non-numeric covariates a barplot will
 be drawn.


#### Usage ####

     
     plot.balance(tmatch, covar, model,
     nstrata = attr(attr(tmatch, "triangle.psa"), "nstrata"),
     label = "Covariate", ylab = "", se.ratio = 2)


#### Arguments ####

results from [`trimatch`](trimatch.html) . vector of the covaraite to check balance of. an integer between 1 and 3 indicating from which model the propensity scores will be used. number of strata to use. label of the y-axis. a multiplier for how large standard error bars will be. label for the legend.

#### Details ####


 A Friedman rank sum test will be performed for all
 covariate types, printed, and stored as an attribute to
 the returned object named `friedman` . If a
 continuous covariate a repeated measures ANOVA will also
 be performed, printed, and returned as an attribute named
  `rmanova` .


#### Value ####


 a `ggplot2` figure.


