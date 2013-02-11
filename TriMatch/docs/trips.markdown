---
layout: mathjax	
title: TriMatch
subtitle: Propensity Score Matching for Non-Binary Treatments
published: true
status: publish
submenu: trimatch
---


#### `trips`: Estimates propensity scores for three groups ####

#### Description ####


 The propensity score is $e(X)=P({ W }=1|X)$ This
 function will estimate the propensity scores for each
 pair of groups (e.g. two treatments and one control).


#### Usage ####

     
     trips(thedata, treat, formu = ~., groups = unique(treat),
     nstrata = 5, ...)


#### Arguments ####

the data frame. vector or factor indicating the treatment/control assignment for `thedata` . Length must be equal to `nrow(thedata)` . the logistic regression formula. Note that the dependent variable should not be specified and will be modified. a vector of exactly length three corresponding the values in `treat` for each control/treatment. the number of strata marks to plot on the edge. other parameters passed to [`glm`](glm.html) .

#### Details ####


  
$${ PS }_{ 1 }=e({ x }_{ { T }_{ 1 }C })=Pr(z=1|{ x}_{ { T }_{ 1 }C })$$
  
$${ PS }_{ 2 }=e({ x }_{ { T }_{2 }C })=Pr(z=1|{ x }_{ { T }_{ 2 }C })$$
  
$${ PS }_{ 3}=e({ x }_{ { T }_{ 2 }{ T }_{ 1 } })=Pr(z=1|{ x }_{ { T}_{ 2 }{ T }_{ 1 } })$$
 


#### Examples ####


    
    data(students)
    students$Income <- as.integer(students$Income)
    students$Employment <- as.integer(students$Employment)
    students$EdLevelMother <- as.integer(students$EdLevelMother)
    students$EdLevelFather <- as.integer(students$EdLevelFather)
    form <- ~Military + Income + Employment + NativeEnglish + EdLevelMother + EdLevelFather + 
        HasAssocAtEnrollment + Ethnicity + Gender + Age
    tpsa <- trips(students, students$TreatBy, form)
    head(tpsa)

    ##   id      treat model1 model2 model3       ps1    ps2 ps3 strata1 strata2
    ## 1  1    Control  FALSE  FALSE     NA 2.459e-01 0.2602  NA       2       2
    ## 2  2 Treatment1   TRUE     NA   TRUE 7.924e-01     NA   1       5    <NA>
    ## 3  3    Control  FALSE  FALSE     NA 3.626e-01 0.2500  NA       5       1
    ## 4  4    Control  FALSE  FALSE     NA 1.998e-01 0.2292  NA       1       1
    ## 5  5    Control  FALSE  FALSE     NA 1.998e-01 0.2292  NA       1       1
    ## 6  6    Control  FALSE  FALSE     NA 7.001e-08 0.1650  NA       1       1
    ##   strata3
    ## 1    <NA>
    ## 2       5
    ## 3    <NA>
    ## 4    <NA>
    ## 5    <NA>
    ## 6    <NA>
