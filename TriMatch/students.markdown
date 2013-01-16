---
layout: mathjax	
title: TriMatch
subtitle: Propensity Score Matching for Non-Binary Treatments
published: true
status: publish
submenu: trimatch
---
 
#### Example One: New Student Outreach
 


 
The data in example one (`data(students)`) represent newly enrolled students in a distance education program. By the nature of the program all students are considered part-time. Moreover, enrollment in the institution does not necessarily mean students are making active progress towards degree completion. To address this issue the institution began an outreach program whereby academic advisors would regularly contact new students within the first six months of enrollment until either six months have passed, or the student enrolled in some credit earning activity (generally a course or an examination for credit). Since randomization to receive the outreach was not possible, a comparison group was defined as students who enrolled six months prior to the start of the outreach. The treatment group is identified as students who enrolled six months after the start of the outreach and who received at least one academic advisor contact. 
 
Covariates for estimating propensity scores were retrieved from the student information system. The dependent variable of interest is the number of credits attempted within the first seven months of enrollment.
 
During the implementation phase it was identified that the outreach being conducted was substantially different between the two academic advisors responsible for the treatment. As such, it became necessary to treat each academic advisor as a separate treatment. The analysis of propensity score models for more than two groups (i.e. two treatments and one control) has relied on conducting three separate analyses. We outline here an approach to conducting propensity score analysis with three groups. 
 

    data(students)
    names(students)

 
We will create a `treat` variable that identifies our three groups.
 

    treat <- students$TreatBy
    table(treat, useNA = "ifany")

    treat
       Control Treatment1 Treatment2 
           200         83         91 

    describeBy(students$CreditsAttempted, group = list(students$TreatBy), mat = TRUE, skew = FALSE)

       item     group1 var   n  mean    sd median trimmed   mad min max range     se
    11    1    Control   1 200 7.225 7.903      5   6.069 7.413   0  26    26 0.5588
    12    2 Treatment1   1  83 4.361 6.156      0   3.254 0.000   0  27    27 0.6757
    13    3 Treatment2   1  91 5.923 7.148      3   4.767 4.448   0  25    25 0.7493

 
The following boxplot shows unadjusted results.
 

    ggplot(students, aes(x = TreatBy, y = CreditsAttempted, colour = TreatBy)) + geom_boxplot() + 
        coord_flip() + geom_jitter()

![plot of chunk boxplot](/images/trimatch/boxplot.png) 

 
#### Estimate Propensity Scores
 
The `trips` function will estimate three propensity score models.
 

    cols.model <- c("Military", "Income", "Employment", "NativeEnglish", "EdLevelMother", "EdLevelFather", 
        "HasAssocAtEnrollment", "Ethnicity", "Gender", "Age")
    tpsa <- trips(students[, cols.model], treat, ids = 1:nrow(students))

    Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

    Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred

 
 

    head(tpsa)

      id      treat model1 model2 model3       ps1       ps2 ps3 strata1 strata2 strata3
    1  1    Control  FALSE  FALSE     NA 1.462e-01 1.687e-01  NA       1       1    <NA>
    2  2 Treatment1   TRUE     NA   TRUE 8.304e-01        NA   1       5    <NA>       5
    3  3    Control  FALSE  FALSE     NA 3.934e-01 3.693e-01  NA       4       4    <NA>
    4  4    Control  FALSE  FALSE     NA 2.965e-08 1.779e-08  NA       1       1    <NA>
    5  5    Control  FALSE  FALSE     NA 2.965e-08 1.779e-08  NA       1       1    <NA>
    6  6    Control  FALSE  FALSE     NA 2.220e-16 1.650e-08  NA    <NA>       1    <NA>

    (p <- plot(tpsa))

![plot of chunk psaestimates](/images/trimatch/psaestimates.png) 

 
#### Matched Triplets
 

    tmatch <- trimatch(tpsa)

 
Triangle plot of the results. We can see how the propensity scores translate from one model to another.
 

    head(tmatch)

      Treatment2 Treatment1 Control      D.m3      D.m1      D.m2   Dtotal
    1        158        133      79 0.0019572 0.0003234 0.0002349 0.002516
    2        321        105      91 0.0001022 0.0018232 0.0016915 0.003617
    3        340         50     111 0.0006063 0.0032318 0.0027056 0.006544
    4        366        151      13 0.0014745 0.0006807 0.0051495 0.007305
    5        338        334     170 0.0034381 0.0001212 0.0040458 0.007605
    6        340         31     219 0.0016162 0.0050170 0.0011234 0.007757

    plot(tmatch, rows = c(2), line.alpha = 1, draw.segments = TRUE)

![plot of chunk matches](/images/trimatch/matches.png) 

 
We can plot the distances. We can specify other calipers to see how may matched triplets we eliminate if we specify a small caliper to the `trimatch` function.
 

    plot.distances(tmatch, caliper = c(0.15, 0.2, 0.25))

    Standard deviations of propensity scores: 0.17 + 0.15 + 0.23 = 0.55
    Percentage of matches exceding a distance of 0.083 (caliper = 0.15): 7%
    Percentage of matches exceding a distance of 0.11 (caliper = 0.2): 0.72%
    Percentage of matches exceding a distance of 0.14 (caliper = 0.25): 0%

    Mapping a variable to y and also using stat="bin".  With stat="bin", it will attempt to
    set the y value to the count of cases in each group.  This can result in unexpected
    behavior and will not be allowed in a future version of ggplot2.  If you want y to
    represent counts of cases, use stat="bin" and don't map a variable to y.  If you want y
    to represent values in the data, use stat="identity".  See ?geom_bar for examples.
    (Deprecated; last used in version 0.9.2)

    Warning: position_stack requires constant width: output may be incorrect

![plot of chunk distances](/images/trimatch/distances.png) 

 
The numbers on the left edge are the row numbers from `tmatch`. We can then use the `plot.triangle.matches` function with specifying the `rows` parameters to any or all of these values to investigate that matched triplet. The following figures shows that the large distances in due to the fact that only one data point has a very large propensity score in both model 1 and 2.
 

    tmatch[tmatch$Dtotal > 0.11, ]

        Treatment2 Treatment1 Control    D.m3    D.m1    D.m2 Dtotal
    833        128         12     180 0.05792 0.04008 0.01561 0.1136
    834        181         12     180 0.04890 0.04008 0.02495 0.1139
    835        271         12     180 0.05416 0.04008 0.02054 0.1148
    836        372        103     256 0.04075 0.04209 0.03475 0.1176
    837        363        283      25 0.05112 0.03998 0.02767 0.1188
    838        368        103     256 0.04575 0.04209 0.03242 0.1203

    tmatch[838, ]

        Treatment2 Treatment1 Control    D.m3    D.m1    D.m2 Dtotal
    838        368        103     256 0.04575 0.04209 0.03242 0.1203

    plot(tmatch, rows = c(838), line.alpha = 1, draw.segments = TRUE)

![plot of chunk followup](/images/trimatch/followup.png) 

 
#### Checking balance.
 
 

    plot.balance(tmatch, students$Age, label = "Age")

    Warning: Could not determine strata for the following rows: 441

    
    	Friedman rank sum test
    
    data:  Covariate and Treatment and ID 
    Friedman chi-squared = 32.98, df = 2, p-value = 6.89e-08

![plot of chunk balance](/images/trimatch/balance1.png) 

    plot.balance(tmatch, students$Age, label = "Age", nstrata = 8)

    Warning: Could not determine strata for the following rows: 441

    
    	Friedman rank sum test
    
    data:  Covariate and Treatment and ID 
    Friedman chi-squared = 32.98, df = 2, p-value = 6.89e-08

![plot of chunk balance](/images/trimatch/balance2.png) 

    plot.balance(tmatch, students$Military, label = "Military", nstrata = 10)

    Warning: Could not determine strata for the following rows: 441

    
    	Friedman rank sum test
    
    data:  Covariate and Treatment and ID 
    Friedman chi-squared = 1.924, df = 2, p-value = 0.3821

![plot of chunk balance](/images/trimatch/balance3.png) 

    plot.balance(tmatch, students$Gender, label = "Gender")

    Warning: Could not determine strata for the following rows: 441

    
    	Friedman rank sum test
    
    data:  Covariate and Treatment and ID 
    Friedman chi-squared = 9.553, df = 2, p-value = 0.008425

![plot of chunk balance](/images/trimatch/balance4.png) 

    plot.balance(tmatch, students$Ethnicity, label = "Ethnicity")

    Warning: Could not determine strata for the following rows: 441

    
    	Friedman rank sum test
    
    data:  Covariate and Treatment and ID 
    Friedman chi-squared = 6.38, df = 2, p-value = 0.04118

![plot of chunk balance](/images/trimatch/balance5.png) 

 
#### Examine unmatched students.
 

    # Look at the subjects that could not be matched
    unmatched <- attr(tmatch, "unmatched")
    nrow(unmatched)/nrow(tpsa) * 100

    [1] 18.45

    # Percentage of each group not matched
    table(unmatched$treat)/table(tpsa$treat) * 100

    
       Control Treatment1 Treatment2 
         23.00      13.25      13.19 

    unmatched[unmatched$treat != "Control", ]

         id      treat model1 model2 model3    ps1    ps2       ps3 strata1 strata2 strata3
    2     2 Treatment1   TRUE     NA   TRUE 0.8304     NA 1.000e+00       5    <NA>       5
    23   23 Treatment1   TRUE     NA   TRUE 1.0000     NA 1.000e+00       5    <NA>       5
    65   65 Treatment1   TRUE     NA   TRUE 0.7518     NA 1.000e+00       5    <NA>       5
    80   80 Treatment1   TRUE     NA   TRUE 0.5788     NA 8.082e-01       5    <NA>       5
    83   83 Treatment1   TRUE     NA   TRUE 0.7625     NA 1.000e+00       5    <NA>       5
    119 119 Treatment2     NA   TRUE  FALSE     NA 0.5751 1.641e-01    <NA>       5       1
    123 123 Treatment2     NA   TRUE  FALSE     NA 0.6137 1.316e-01    <NA>       5       1
    125 125 Treatment2     NA   TRUE  FALSE     NA 0.5315 1.413e-08    <NA>       5       1
    139 139 Treatment2     NA   TRUE  FALSE     NA 0.3737 3.892e-08    <NA>       4       1
    144 144 Treatment1   TRUE     NA   TRUE 0.5993     NA 7.979e-01       5    <NA>       5
    214 214 Treatment1   TRUE     NA   TRUE 0.5998     NA 8.225e-01       5    <NA>       5
    284 284 Treatment2     NA   TRUE  FALSE     NA 0.4910 2.230e-08    <NA>       5       1
    285 285 Treatment2     NA   TRUE  FALSE     NA 0.6094 1.195e-01    <NA>       5       1
    302 302 Treatment2     NA   TRUE  FALSE     NA 0.3743 6.308e-08    <NA>       4       1
    319 319 Treatment2     NA   TRUE  FALSE     NA 0.5894 1.260e-01    <NA>       5       1
    323 323 Treatment2     NA   TRUE  FALSE     NA 0.3476 2.220e-16    <NA>       4    <NA>
    324 324 Treatment2     NA   TRUE  FALSE     NA 0.3517 6.188e-08    <NA>       4       1
    332 332 Treatment2     NA   TRUE  FALSE     NA 0.6212 3.449e-01    <NA>       5       2
    335 335 Treatment1   TRUE     NA   TRUE 1.0000     NA 1.000e+00       5    <NA>       5
    339 339 Treatment2     NA   TRUE  FALSE     NA 0.6239 1.097e-01    <NA>       5       1
    365 365 Treatment1   TRUE     NA   TRUE 0.8931     NA 1.000e+00       5    <NA>       5
    367 367 Treatment1   TRUE     NA   TRUE 0.3604     NA 1.000e+00       4    <NA>       5
    369 369 Treatment1   TRUE     NA   TRUE 0.4517     NA 1.000e+00       5    <NA>       5

 
We can create a triangle plot of only the unmatched students by subsetting `tpsa` with those students in the `unmatched` data frame.
 

    plot(tpsa[tpsa$id %in% unmatched$id, ])

![plot of chunk plotunmatched](/images/trimatch/plotunmatched.png) 

 
#### Loess Plot
 

    plot.loess3(tmatch, students$CreditsAttempted, plot.points = geom_jitter, ylab = "Credits Attempted")

    geom_smooth: method="auto" and size of largest group is <1000, so using loess. Use
    'method = x' to change the smoothing method.

![plot of chunk loess](/images/trimatch/loess.png) 

 
#### Parrellel Plot
 

    tmatch.out <- merge(x = tmatch, y = students[, c("CreditsAttempted")])
    names(tmatch.out)

     [1] "Treatment2"     "Treatment1"     "Control"        "D.m3"           "D.m1"          
     [6] "D.m2"           "Dtotal"         "Treatment2.out" "Treatment1.out" "Control.out"   

    plot.parallel(tmatch.out)

![plot of chunk merge](/images/trimatch/merge.png) 

 
#### Friedman Rank Sum Test
 

    outcomes <- grep(".out$", names(tmatch.out), perl = TRUE)
    tmatch.out$id <- 1:nrow(tmatch.out)
    out <- melt(tmatch.out[, c(outcomes, ncol(tmatch.out))], id.vars = "id")
    names(out) <- c("ID", "Treatment", "Outcome")
    head(out)

      ID      Treatment Outcome
    1  1 Treatment2.out       4
    2  2 Treatment2.out      17
    3  3 Treatment2.out       3
    4  4 Treatment2.out       0
    5  5 Treatment2.out       0
    6  6 Treatment2.out       3

    set.seed(2112)
    friedman.test(Outcome ~ Treatment | ID, out)

    
    	Friedman rank sum test
    
    data:  Outcome and Treatment and ID 
    Friedman chi-squared = 70.65, df = 2, p-value = 4.546e-16

 
#### Repeated Measures ANOVA
 

    rmanova <- ezANOVA(data = out, dv = Outcome, wid = ID, within = Treatment)

    Warning: Converting "ID" to factor for ANOVA.

    print(rmanova)

    $ANOVA
         Effect DFn  DFd     F         p p<.05     ges
    2 Treatment   2 1674 58.33 3.251e-25     * 0.04424
    
    $`Mauchly's Test for Sphericity`
         Effect      W         p p<.05
    2 Treatment 0.9769 5.685e-05     *
    
    $`Sphericity Corrections`
         Effect    GGe     p[GG] p[GG]<.05    HFe    p[HF] p[HF]<.05
    2 Treatment 0.9774 1.048e-24         * 0.9797 9.32e-25         *

 
#### Pairwise Wilcoxon Rank Sum Tests
 

    pairwise.wilcox.test(x = out$Outcome, g = out$Treatment, paired = TRUE, p.adjust.method = "bonferroni")

    
    	Pairwise comparisons using Wilcoxon signed rank test 
    
    data:  out$Outcome and out$Treatment 
    
                   Treatment2.out Treatment1.out
    Treatment1.out 4.2e-10        -             
    Control.out    1.4e-05        < 2e-16       
    
    P value adjustment method: bonferroni 

 
#### Posthoc *t*-tests
 

    (t1 <- t.test(x = tmatch.out$Treatment1.out, y = tmatch.out$Control.out, paired = TRUE))

    
    	Paired t-test
    
    data:  tmatch.out$Treatment1.out and tmatch.out$Control.out 
    t = -10.76, df = 837, p-value < 2.2e-16
    alternative hypothesis: true difference in means is not equal to 0 
    95 percent confidence interval:
     -4.385 -3.032 
    sample estimates:
    mean of the differences 
                     -3.709 

    (t2 <- t.test(x = tmatch.out$Treatment2.out, y = tmatch.out$Control.out, paired = TRUE))

    
    	Paired t-test
    
    data:  tmatch.out$Treatment2.out and tmatch.out$Control.out 
    t = -4.863, df = 837, p-value = 1.379e-06
    alternative hypothesis: true difference in means is not equal to 0 
    95 percent confidence interval:
     -2.491 -1.058 
    sample estimates:
    mean of the differences 
                     -1.774 

    (t3 <- t.test(x = tmatch.out$Treatment2.out, y = tmatch.out$Treatment1.out, paired = TRUE))

    
    	Paired t-test
    
    data:  tmatch.out$Treatment2.out and tmatch.out$Treatment1.out 
    t = 6.054, df = 837, p-value = 2.13e-09
    alternative hypothesis: true difference in means is not equal to 0 
    95 percent confidence interval:
     1.307 2.562 
    sample estimates:
    mean of the differences 
                      1.934 

 
#### Boxplot of differences
 

    tmatch.out$Treat1_Control <- tmatch.out$Treatment1.out - tmatch.out$Control.out
    tmatch.out$Treat2_Control <- tmatch.out$Treatment2.out - tmatch.out$Control.out
    tmatch.out$Treat2_Treat1 <- tmatch.out$Treatment2.out - tmatch.out$Treatment1.out
    out.box <- melt(tmatch.out[, c("id", "Treat1_Control", "Treat2_Control", "Treat2_Treat1")], 
        id.vars = "id")
    names(out.box) <- c("Student", "Treatment", "Difference")
    
    ci <- as.data.frame(rbind(t1$conf.int, t2$conf.int, t3$conf.int))
    ci$Treatment <- names(tmatch.out)[12:14]
    ci$estimate <- c(t1$estimate, t2$estimate, t3$estimate)
    
    ggplot(out.box, aes(x = Treatment, y = Difference)) + geom_boxplot() + geom_hline(yintercept = 0) + 
        geom_crossbar(data = ci, aes(x = Treatment, ymin = V1, ymax = V2, y = estimate), color = "green", 
            fill = "green", width = 0.72, alpha = 0.6) + scale_x_discrete(NULL, labels = c(Treat1_Control = "Treat1 - Control", 
        Treat2_Control = "Treat2 - Control", Treat2_Treat1 = "Treat2 - Treat1")) + xlab(NULL)

![plot of chunk boxplotdiff](/images/trimatch/boxplotdiff.png) 

