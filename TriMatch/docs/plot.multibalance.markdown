---
layout: mathjax	
title: TriMatch
subtitle: Propensity Score Matching for Non-Binary Treatments
published: true
status: publish
submenu: trimatch
---


#### `plot.multibalance`: Multiple covariate blance assessment plot. ####

#### Description ####


 A graphic based upon [`cv.bal.psa`](cv.bal.psa.html) function in
 the `PSAgraphics` package. This graphic plots the
 effect sizes for multiple covariated before and after
 propensity score andjustement.


#### Usage ####

     
     plot.multibalance(tpsa, covs, grid = TRUE)


#### Arguments ####

results of [`trips`](trips.html) . a vector or data frame of covariates. if TRUE, then a grid of three plots for each model will be displayed.

#### Value ####


 a `ggplot2` figure.


