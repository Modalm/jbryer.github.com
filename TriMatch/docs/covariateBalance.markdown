---
layout: mathjax	
title: TriMatch
subtitle: Propensity Score Matching for Non-Binary Treatments
published: true
status: publish
submenu: trimatch
---


#### `covariateBalance`: Calculate covariate effect size differences before and after stratification. ####

#### Description ####


 This function is modified from the
  [`cv.bal.psa`](cv.bal.psa.html) function in the
  `PSAgrpahics` package.


#### Usage ####

     
     covariateBalance(covariates, treatment, propensity,
     strata = NULL, int = NULL, tree = FALSE, minsize = 2,
     universal.psd = TRUE, trM = 0, absolute.es = TRUE,
     trt.value = NULL, use.trt.var = FALSE, verbose = FALSE,
     xlim = NULL, plot.strata = TRUE, ...)


#### Arguments ####

dataframe of interest binary vector of 0s and 1s (necessarily? what if character, or 1, 2?) PS scores from some method or other. either a vector of strata number for each row of covariate, or one number n in which case it is attempted to group rows by ps scores into n strata of size approximately 1/n.  This does not seem to work well in the case of few specific propensity values, as from a tree. either a number m used to divide [0,1] into m equal length subintervals, or a vector of cut points between 0 an 1 defining the subintervals (perhaps as suggested by loess.psa).  In either case these subintervals define strata, so strata can be of any size. logical, if unique ps scores are few, as from a recursively partitioned tree, then TRUE will force each ps value to define a stratum. smallest allowable stratum-treatment size. If violated, strata is removed. If 'TRUE', forces standard deviations used to be unadjusted for stratification. trimming proportion for mean calculations. logical, if 'TRUE' routine uses absolute values of all effect sizes. allows user to specify which value is active treatment, if desired. logical, if true then Rubin-Stuart method using only treatment variance with be used in effect size calculations. logical, controls output that is visibly returned. limits for the x-axis. logical indicating whether to print strata. currently unused.

#### Details ####


 Note: effect sizes are calculated as treatment 1 -
 treatment 0, or treatment B - treatment A.


#### Author ####


 Robert M. Pruzek RMPruzek@yahoo.com
 
 James E. Helmreich James.Helmreich@Marist.edu
 
 KuangNan Xiong harryxkn@yahoo.com


