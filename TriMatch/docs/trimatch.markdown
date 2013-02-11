---
layout: mathjax	
title: TriMatch
subtitle: Propensity Score Matching for Non-Binary Treatments
published: true
status: publish
submenu: trimatch
---


#### `trimatch`: Creates matched triplets. ####

#### Description ####


 Create matched triplets by minimizing the total distance
 between matched triplets within a specified caliper.


#### Usage ####

     
     trimatch(tpsa, caliper = 0.25, nmatch = c(25),
     match.order, M1 = 2, M2 = 1, exact, status = TRUE, ...)


#### Arguments ####

the results from [`trips`](trips.html) a vector of length one or three indicating the caliper to use for matching within each step. This is expressed in standardized units such that .25 means that matches must be within .25 of one standard deviation to be kept, otherwise the match is dropped. number of closest matches to retain before moving to next edge. This can be `Inf` in which case all matches within the caliper will be retained through to the next step. For large datasets, evaluating all possible matches within the caliper could be time consuming. character vector of length three indicating the order in which the matching algorithm will processes. The default is to use start with the group the middle number of subjects, followed by the smallest, and then the largest. a scaler indicating the number of unique subjects in group one to retain. This applies only to the first group in the matching order. a scaler indicating the number of unique matches to retain. This applies to the first two groups in the matching order. a vector or data frame of representing covariates for exact matching.  That is, matched triplets will first be matched exactly on these covariates before evalutating distances. currently unused. whether to print a status bar while executing.

#### Details ####


 The [`trips`](trips.html) function will estimate the
 propensity scores for three models. This method will then
 find the best matched triplets based upon minimizing the
 summed differences between propensity scores across the
 three models. That is, the algorithm works as follows:
 
   

 *  The first subject from model 1 is selected. 

 *  The `nmatch` smallest distances are selected using propensity scores from model 1. 

 *  For each of the matches identified, the subjects propensity score from model 2 is retrieved. 

 *  The `nmatch`  smallest distances are selected using propensity score from model 3. 

 *  For each of those matches identified, the subjects propensity score from model 2 is retrieved.  

 *  The distances is calculated from the first and last subjects propensity scores from model 2. 

 *  The three distances are summed. 

 *  The triplet with the smallest overall distance is selected and returned. 


#### Examples ####


    
    data(students)
    students$Income <- as.integer(students$Income)
    students$Employment <- as.integer(students$Employment)
    students$EdLevelMother <- as.integer(students$EdLevelMother)
    students$EdLevelFather <- as.integer(students$EdLevelFather)
    form <- ~Military + Income + Employment + NativeEnglish + EdLevelMother + EdLevelFather + 
        HasAssocAtEnrollment + Ethnicity + Gender + Age
    tpsa <- trips(students, students$TreatBy, form)
    tmatch <- trimatch(tpsa, M1 = 3, M2 = 1, status = FALSE)
    head(tmatch)

    ##   Treatment2 Treatment1 Control     D.m3     D.m1     D.m2  Dtotal
    ## 1        321        341     196 0.002053 0.005125 0.003299 0.01048
    ## 2         30         54     266 0.002237 0.006129 0.002445 0.01081
    ## 3        304        355     145 0.007630 0.015860 0.002943 0.02643
    ## 4        104        341     174 0.013443 0.016193 0.002124 0.03176
    ## 5        292        283     205 0.005717 0.019473 0.006671 0.03186
    ## 6        181        281     155 0.021590 0.004147 0.007150 0.03289

    plot(tmatch, rows = c(3), line.alpha = 1, draw.segments = TRUE)

![plot of chunk trimatch-example](trimatch-example.png) 
