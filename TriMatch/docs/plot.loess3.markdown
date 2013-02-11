---
layout: mathjax	
title: TriMatch
subtitle: Propensity Score Matching for Non-Binary Treatments
published: true
status: publish
submenu: trimatch
---


#### `plot.loess3`: Loess plot for matched triplets. ####

#### Description ####


 This function will create a `ggplot2` figure with
 propensity scores on the x-axis and the outcome on the
 y-axis. Three Loess regression lines will be plotted
 based upon the propensity scores from `model` . Since
 each model produces propensity scores for two of the
 three groups, the propensity score for the third group in
 each matched triplet will be the mean of the other two.
 If `model` is not specified, the default will be to
 use the model that estimates the propensity scores for
 the first two groups in the matching order.


#### Usage ####

     
     plot.loess3(tmatch, outcome, model, ylab = "Outcome",
     smooth.method = "auto", plot.connections = FALSE,
     connections.color = "black", connections.alpha = 0.2,
     plot.points = geom_point, points.alpha = 0.1,
     points.palette = "Dark2")


#### Arguments ####

the results of [`trimatch`](trimatch.html) . a vector representing the outcomes. an integer between 1 and 3 indicating from which model the propensity scores will be used. the label for the y-axis. the smoothing method. See  [`stat_smooth`](stat_smooth.html) for more info. boolean indicating whether lines will be drawn connecting each matched triplet. the line color of connections. number between 0 and 1 representing the alpha levels for connection lines. a `ggplot2` function for plotting points. Usually [`geom_point`](geom_point.html) or  [`geom_jitter`](geom_jitter.html) . If `NULL` no points will be drawn. number between 0 and 1 representing the alpha level for the points. the color palette to use. See  [`scale_colour_brewer`](scale_colour_brewer.html) and  [http://colorbrewer2.org/](http://colorbrewer2.org/) for more information.

#### Value ####


 a `ggplot2` figure.


