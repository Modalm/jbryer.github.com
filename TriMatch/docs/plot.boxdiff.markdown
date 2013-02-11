---
layout: mathjax	
title: TriMatch
subtitle: Propensity Score Matching for Non-Binary Treatments
published: true
status: publish
submenu: trimatch
---


#### `plot.boxdiff`: Returns a `ggplot2` box plot of the differences. ####

#### Description ####


 A boxplot of differences between each pair of treatments.


#### Usage ####

     
     plot.boxdiff(tmatch, out, plot.mean = TRUE)


#### Arguments ####

the results from [`trimatch`](trimatch.html) . a vector of the outcome measure of interest. logical indicating whether the means should be plotted.

#### Value ####


 a `ggplot2` boxplot of the differences.


