---
layout: default	
title: naep
subtitle: An R package for the National Assessment of Educational Progress (NAEP)
published: true
status: publish
---


 
This document outline getting started analyzing [National Assessment of Educational Progress (NAEP)](http://nces.ed.gov/nationsreportcard/) data with R and the `naep` package. The examples will utilize the [NAEP Primer](http://nces.ed.gov/pubsearch/pubsinfo.asp?pubid=2011463) but can be used with restricted use data.
 

    require(naep)

 
The first step for working with NAEP in R is to determine the base directory for the NAEP data. The `getBaseDirectory` function will provide a dialog box to assist in locating the root directory of the NAEP data, typically stored on DVD as supplied by NCES. This function will also verify that you have indeed selected a readable NAEP data source.
 

    baseDir = getBaseDirectory()

 
![Select base directory](/images/naep/BaseDirectory.png)
 
The `baseDir` variable will be used for all of the data access functions. Once determined, you may wish to hard code the value to make re-running your R script easier.
 

    baseDir <- "/Volumes/PRIMER_FINAL"

 
We can read in the data catalog with the `getNAEPCatalog` function. Note that the `year`, `grade`, `subject`, `sample`, and `directory` parameters are required.
 

    catalog <- getNAEPCatalog(year = 2005, grade = 8, subject = "Math", sample = "PM", 
        directory = baseDir)
    names(catalog)

    ## [1] "FieldName"     "Start"         "Width"         "Decimal"      
    ## [5] "PreselectFlag" "Description"   "NumCodeValues" "CodeValues"

    nrow(catalog)

    ## [1] 301

    head(catalog[, c("FieldName", "Description")], n = 10)

    ##    FieldName                                     Description
    ## 1       YEAR                                 Assessment year
    ## 2     COHORT                                    All students
    ## 3     SCRPSU                   Scrambled PSU and school code
    ## 4       DSEX                                          Gender
    ## 5        IEP Student classified as having a disability (504)
    ## 6        LEP        Student classified as ELL (2 categories)
    ## 7       ELL3   Student classified Eng lang learner (3 categ)
    ## 8    SDRACEM            Race/ethnicity (from school records)
    ## 9      PARED     Parental education level (from 2 questions)
    ## 10   B003501                        Mother's education level

 
There are `301` variables in this dataset, we have printed the first 10.
 

    vars <- selectVariables(year = 2005, grade = 8, subject = "Math", sample = "PM", 
        directory = baseDir)

 
![Variable Selection](/images/naep/VariableSelectScreen.png)
 
However, like with the `getBaseDirectory()` function above, you can set the variables you wish to use directly.
 

    vars <- c("SCRPSU", "DSEX", "IEP", "LEP", "ELL3", "SDRACEM", "PARED", "RPTSAMP", 
        "REPGRP1", "REPGRP2", "JKUNIT", "ORIGWT", "SRWT01", "SRWT02", "SRWT03", "SRWT04", 
        "SRWT05", "SRWT06", "SRWT07", "SRWT08", "SRWT09", "SRWT10", "SRWT11", "SRWT12", 
        "SRWT13", "SRWT14", "SRWT15", "SRWT16", "SRWT17", "SRWT18", "SRWT19", "SRWT20", 
        "SRWT21", "SRWT22", "SRWT23", "SRWT24", "SRWT25", "SRWT26", "SRWT27", "SRWT28", 
        "SRWT29", "SRWT30", "SRWT31", "SRWT32", "SRWT33", "SRWT34", "SRWT35", "SRWT36", 
        "SRWT37", "SRWT38", "SRWT39", "SRWT40", "SRWT41", "SRWT42", "SRWT43", "SRWT44", 
        "SRWT45", "SRWT46", "SRWT47", "SRWT48", "SRWT49", "SRWT50", "SRWT51", "SRWT52", 
        "SRWT53", "SRWT54", "SRWT55", "SRWT56", "SRWT57", "SRWT58", "SRWT59", "SRWT60", 
        "SRWT61", "SRWT62", "SMSRSWT", "MRPCM1", "MRPCM2", "MRPCM3", "MRPCM4", "MRPCM5")

 
Once the base directory and variable list have been determined, we can read data from the NAEP dataset using the `getNAEPData` function.
 

    g8math = getNAEPData(year = 2005, grade = 8, subject = "Math", sample = "PM", directory = baseDir, 
        vars = vars)
    class(g8math)

    ## [1] "naep"

    names(g8math)

    ##  [1] "data"       "catalog"    "repweights" "design"     "year"      
    ##  [6] "grade"      "subject"    "sourceDir"  "sample"     "type"

 
The `getNAEPData` returns a list of type `naep` that contains a number of important variables, not least of which is the actual data and the design. The raw data can be accessed using `g8math$data`. Here we get the name of the variables available in the data frame.
 

    names(g8math$data)

    ##  [1] "SCRPSU"  "DSEX"    "IEP"     "LEP"     "ELL3"    "SDRACEM" "PARED"  
    ##  [8] "RPTSAMP" "REPGRP1" "REPGRP2" "JKUNIT"  "ORIGWT"  "SRWT01"  "SRWT02" 
    ## [15] "SRWT03"  "SRWT04"  "SRWT05"  "SRWT06"  "SRWT07"  "SRWT08"  "SRWT09" 
    ## [22] "SRWT10"  "SRWT11"  "SRWT12"  "SRWT13"  "SRWT14"  "SRWT15"  "SRWT16" 
    ## [29] "SRWT17"  "SRWT18"  "SRWT19"  "SRWT20"  "SRWT21"  "SRWT22"  "SRWT23" 
    ## [36] "SRWT24"  "SRWT25"  "SRWT26"  "SRWT27"  "SRWT28"  "SRWT29"  "SRWT30" 
    ## [43] "SRWT31"  "SRWT32"  "SRWT33"  "SRWT34"  "SRWT35"  "SRWT36"  "SRWT37" 
    ## [50] "SRWT38"  "SRWT39"  "SRWT40"  "SRWT41"  "SRWT42"  "SRWT43"  "SRWT44" 
    ## [57] "SRWT45"  "SRWT46"  "SRWT47"  "SRWT48"  "SRWT49"  "SRWT50"  "SRWT51" 
    ## [64] "SRWT52"  "SRWT53"  "SRWT54"  "SRWT55"  "SRWT56"  "SRWT57"  "SRWT58" 
    ## [71] "SRWT59"  "SRWT60"  "SRWT61"  "SRWT62"  "SMSRSWT" "MRPCM1"  "MRPCM2" 
    ## [78] "MRPCM3"  "MRPCM4"  "MRPCM5"

 
The `naep` package makes substantial use of the [`survey`](http://faculty.washington.edu/tlumley/survey/) package (Lumley, 2004, 2012b) to analyze data using the provided replicate weights. The `design` object in `g8math` represents the survey design. The `naep.model` function is provided to conveniently interface with the `survey` package. There are a number of functions available to perform most types of analysis. By convention, all functions start with `svy`. Here is the current listing of functions:
 

    ls("package:survey")[grep("^svy*", ls("package:survey"))]

    ##  [1] "svrepdesign"    "svrVar"         "svyboxplot"     "svyby"         
    ##  [5] "svycdf"         "svychisq"       "svyciprop"      "svycontrast"   
    ##  [9] "svycoplot"      "svycoxph"       "svyCprod"       "svydesign"     
    ## [13] "svyfactanal"    "svyglm"         "svyhist"        "svykappa"      
    ## [17] "svykm"          "svyloglin"      "svylogrank"     "svymean"       
    ## [21] "svymle"         "svyolr"         "svyplot"        "svyprcomp"     
    ## [25] "svyquantile"    "svyranktest"    "svyratio"       "svyrecvar"     
    ## [29] "svysmooth"      "svystandardize" "svytable"       "svytotal"      
    ## [33] "svyttest"       "svyvar"

 
You can get more information about any of these functions through R's help system by typing `?functionName`.
 
For example, the `svytable` will return a contingency table, here using the IEP variable.
 

    m1 <- naep.model(~IEP, naepData = g8math, svyFUN = svytable, na.action = na.pass)
    round(m1$combined)

    ## IEP
    ##   Yes    No 
    ##  2336 15270

 
And to perform a two-way contigency table.
 

    m2 <- naep.model(~IEP + DSEX, naepData = g8math, svyFUN = svytable, na.action = na.pass)
    round(m2$combined)

    ##      DSEX
    ## IEP   Male Female
    ##   Yes 1482    853
    ##   No  7426   7844

    round(prop.table(m2$combined) * 100, digits = 1)

    ##      DSEX
    ## IEP   Male Female
    ##   Yes  8.4    4.8
    ##   No  42.2   44.6

 
The `naep.model` function will also combine the multiple plausable values using the [`mitools`](http://faculty.washington.edu/tlumley/survey/svymi.html) package (Lumley, 2012a).
 

    m3 <- naep.model(~MRPCM, var = "MRPCM", naepData = g8math, svyFUN = svymean, na.rm = TRUE)
    print(m3)

    ## Multiple imputation results:
    ##       MIcombine.default(result$models)
    ##       results   se
    ## MRPCM   275.9 0.82

    summary(m3)

    ## NAEP results for with svymean function call.
    ## ~MRPCM
    ## Multiple imputation results:
    ##       MIcombine.default(result$models)
    ##       results   se (lower upper) missInfo
    ## MRPCM   275.9 0.82  274.3  277.5      2 %

 
We can show that we indeed do not get the same result when simplying calculating the mean of the multiple plausible values.
 

    avgScores = apply(g8math$data[, paste("MRPCM", 1:5, sep = "")], 1, mean, na.rm = TRUE)
    mean(avgScores, na.rm = TRUE)

    ## [1] 275.7

    rbind(as.data.frame(m3$models[[1]]), as.data.frame(m3$models[[2]]), as.data.frame(m3$models[[3]]), 
        as.data.frame(m3$models[[4]]), as.data.frame(m3$models[[5]]))

    ##         mean     SE
    ## MRPCM1 276.0 0.8131
    ## MRPCM2 275.8 0.8230
    ## MRPCM3 275.9 0.8156
    ## MRPCM4 275.8 0.8051
    ## MRPCM5 275.9 0.8014

 
The following example combines the functionality for using the replicate weights and multiple plausible values in a regression model predicting math scores from gender, IEP, English Language Learner, race, and parents education. Note that since this is Primer data, no inferences or conclusions should be made. This is provided merely to demonstrate how the `naep` package works.
 

    m4 <- naep.model(MRPCM ~ DSEX + IEP + ELL3 + SDRACEM + PARED, var = "MRPCM", naepData = g8math, 
        svyFUN = svyglm)
    m4$svyFUN

    ## svyglm()

    m4$combined

    ## Multiple imputation results:
    ##       MIcombine.default(result$models)
    ##                             results      se
    ## (Intercept)                221.4858  2.8538
    ## DSEXFemale                  -2.8754  0.5876
    ## IEPNo                       34.6996  1.3177
    ## ELL3No                      26.4805  2.1231
    ## ELL3Formerly ELL            27.6795  2.8281
    ## SDRACEMBlack               -30.9818  1.1463
    ## SDRACEMHispanic            -15.9316  1.2928
    ## SDRACEMAsian/Pacific Islan   7.0456  2.3456
    ## SDRACEMAmer Ind/Alaska Nat -14.6062  3.6607
    ## SDRACEMOther                -6.3977  4.9235
    ## PAREDGraduated H.S.         -0.7574  1.4462
    ## PAREDSome ed after H.S.     10.0207  1.3716
    ## PAREDGraduated college      16.9554  1.3753
    ## PAREDI Don't Know           -2.7588  1.7071
    ## PAREDOmitted                -3.1506  3.7250
    ## PAREDMultiple              -25.0073 12.8202

 
We can also examine each of the five models separately. Here we extract the beta coefficents for each model run (i.e. one for each plausible value).
 

    betas <- MIextract(m4$models, fun = coef)
    betas

    ## [[1]]
    ##                (Intercept)                 DSEXFemale 
    ##                    221.573                     -2.754 
    ##                      IEPNo                     ELL3No 
    ##                     34.496                     26.168 
    ##           ELL3Formerly ELL               SDRACEMBlack 
    ##                     29.238                    -30.963 
    ##            SDRACEMHispanic SDRACEMAsian/Pacific Islan 
    ##                    -16.171                      7.197 
    ## SDRACEMAmer Ind/Alaska Nat               SDRACEMOther 
    ##                    -13.269                     -7.678 
    ##        PAREDGraduated H.S.    PAREDSome ed after H.S. 
    ##                     -0.199                     10.157 
    ##     PAREDGraduated college          PAREDI Don't Know 
    ##                     17.571                     -2.269 
    ##               PAREDOmitted              PAREDMultiple 
    ##                     -2.151                    -25.996 
    ## 
    ## [[2]]
    ##                (Intercept)                 DSEXFemale 
    ##                    220.776                     -2.997 
    ##                      IEPNo                     ELL3No 
    ##                     35.174                     27.504 
    ##           ELL3Formerly ELL               SDRACEMBlack 
    ##                     28.448                    -31.135 
    ##            SDRACEMHispanic SDRACEMAsian/Pacific Islan 
    ##                    -16.108                      6.158 
    ## SDRACEMAmer Ind/Alaska Nat               SDRACEMOther 
    ##                    -14.883                     -7.064 
    ##        PAREDGraduated H.S.    PAREDSome ed after H.S. 
    ##                     -1.335                      9.540 
    ##     PAREDGraduated college          PAREDI Don't Know 
    ##                     16.197                     -3.186 
    ##               PAREDOmitted              PAREDMultiple 
    ##                     -4.060                    -22.958 
    ## 
    ## [[3]]
    ##                (Intercept)                 DSEXFemale 
    ##                   221.8211                    -2.7318 
    ##                      IEPNo                     ELL3No 
    ##                    34.7181                    25.8139 
    ##           ELL3Formerly ELL               SDRACEMBlack 
    ##                    25.9226                   -30.9697 
    ##            SDRACEMHispanic SDRACEMAsian/Pacific Islan 
    ##                   -16.0747                     7.4419 
    ## SDRACEMAmer Ind/Alaska Nat               SDRACEMOther 
    ##                   -14.7118                    -6.2783 
    ##        PAREDGraduated H.S.    PAREDSome ed after H.S. 
    ##                    -0.6733                    10.7710 
    ##     PAREDGraduated college          PAREDI Don't Know 
    ##                    17.3430                    -3.0643 
    ##               PAREDOmitted              PAREDMultiple 
    ##                    -3.3702                   -30.1142 
    ## 
    ## [[4]]
    ##                (Intercept)                 DSEXFemale 
    ##                   221.3883                    -3.0579 
    ##                      IEPNo                     ELL3No 
    ##                    34.8386                    26.6162 
    ##           ELL3Formerly ELL               SDRACEMBlack 
    ##                    27.1694                   -31.0923 
    ##            SDRACEMHispanic SDRACEMAsian/Pacific Islan 
    ##                   -15.5642                     7.4676 
    ## SDRACEMAmer Ind/Alaska Nat               SDRACEMOther 
    ##                   -13.6560                    -8.0414 
    ##        PAREDGraduated H.S.    PAREDSome ed after H.S. 
    ##                    -0.8506                     9.6485 
    ##     PAREDGraduated college          PAREDI Don't Know 
    ##                    16.6640                    -3.2077 
    ##               PAREDOmitted              PAREDMultiple 
    ##                    -2.1522                   -25.3367 
    ## 
    ## [[5]]
    ##                (Intercept)                 DSEXFemale 
    ##                   221.8709                    -2.8365 
    ##                      IEPNo                     ELL3No 
    ##                    34.2710                    26.3011 
    ##           ELL3Formerly ELL               SDRACEMBlack 
    ##                    27.6196                   -30.7494 
    ##            SDRACEMHispanic SDRACEMAsian/Pacific Islan 
    ##                   -15.7403                     6.9638 
    ## SDRACEMAmer Ind/Alaska Nat               SDRACEMOther 
    ##                   -16.5113                    -2.9269 
    ##        PAREDGraduated H.S.    PAREDSome ed after H.S. 
    ##                    -0.7296                     9.9871 
    ##     PAREDGraduated college          PAREDI Don't Know 
    ##                    17.0016                    -2.0666 
    ##               PAREDOmitted              PAREDMultiple 
    ##                    -4.0200                   -20.6317

 
#### References
 
Lumley, T. (2004) Analysis of complex survey samples. *Journal of Statistical Software 9*(1): 1-19
 
Lumley, T. (2012a). mitools: Tools for multiple imputation of missing data. R package version 2.2. http://CRAN.R-project.org/package=mitools
 
Lumley, T. (2012b) "survey: Analysis of complex survey samples". R package version 3.28-2.
 
