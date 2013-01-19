---
layout: mathjax	
title: TriMatch
subtitle: Propensity Score Matching for Non-Binary Treatments
published: true
status: publish
submenu: trimatch
---
 
#### Example: National Medical Expenditure Survey
 

    ## Loading required package: TriMatch

    ## Loading required package: psych

    ## Attaching package: 'psych'

    ## The following object(s) are masked from 'package:ggplot2':
    ## 
    ## %+%

    ## Loading required package: reshape2

    ## Loading required package: ez

    ## Loading required package: car

    ## Loading required package: MASS

    ## Loading required package: nnet

    ## Attaching package: 'car'

    ## The following object(s) are masked from 'package:psych':
    ## 
    ## logit

    ## Loading required package: lme4

    ## Loading required package: Matrix

    ## Loading required package: lattice

    ## Attaching package: 'Matrix'

    ## The following object(s) are masked from 'package:stats':
    ## 
    ## toeplitz

    ## Attaching package: 'lme4'

    ## The following object(s) are masked from 'package:stats':
    ## 
    ## AIC, BIC

    ## Loading required package: mgcv

    ## This is mgcv 1.7-22. For overview type 'help("mgcv-package")'.

    ## Loading required package: memoise

    ## Loading required package: plyr

    ## Loading required package: scales

    ## Attaching package: 'scales'

    ## The following object(s) are masked from 'package:psych':
    ## 
    ## alpha, rescale

    ## Loading required package: stringr

    ## Attaching package: 'ez'

    ## The following object(s) are masked from 'package:plyr':
    ## 
    ## progress_time

    ## Loading required package: PSAgraphics

    ## Loading required package: rpart

 

    data(nmes)
    names(nmes)

     [1] "PIDX"      "LASTAGE"   "MALE"      "RACE3"     "eversmk"   "current"   "former"   
     [8] "smoke"     "AGESMOKE"  "CIGSSMOK"  "SMOKENOW"  "SMOKED"    "CIGSADAY"  "AGESTOP"  
    [15] "packyears" "yearsince" "INCALPER"  "HSQACCWT"  "TOTALEXP"  "TOTALSP3"  "lc5"      
    [22] "chd5"      "beltuse"   "educate"   "marital"   "SREGION"   "POVSTALB"  "flag"     
    [29] "age"      

 
We will create a `treat` variable that identifies our three groups.
 


 
The following boxplot shows unadjusted results.
 


 
#### Estimate Propensity Scores
 
The `trips` function will estimate three propensity score models.
 


 
 


 
#### Matched Triplets
 
