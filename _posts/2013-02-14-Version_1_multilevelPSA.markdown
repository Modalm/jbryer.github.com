--- 
layout: post
title: Version 1.0 of multilevelPSA Available on CRAN
tags: R R-Bloggers psa
type: post
published: true
status: publish
---

Version 1.0 of `multilevelPSA` has been released to CRAN. The `multilevelPSA` package provides functions to estimate and visualize propensity score models with multilevel, or clustered, data. The graphics are an extension of [`PSAgraphics`](http://www.jstatsoft.org/v29/i06/paper) package by Helmreich and Pruzek. The example below will investigate the differences between private and public school internationally using the Programme of International Student Assessment (PISA). The `multilevelPSA` package includes a subset of the full 2009 PISA dataset including North American countries (i.e. Canada, Mexico, & United States). However, the full dataset is available in the [`pisa`](/pisa) R package and can be downloaded using the `install_github` function in the `devtools` package (note that the package is approximately 80mb and as such, is not available on CRAN).

	> install.packages(c('multilevelPSA','devtools'), repos='http://cran.r-project.org')
	> require(devtools)
	> install_github('pisa','jbryer')
	> require(multilevelPSA)
	> require(pisa)

Load and setup the data. If the `pisa` package is installed we will load and subset from the full PISA dataset, otherwise we will use only the North American countries.

	> data(pisa.colnames) #Data catalog
	> data(pisa.psa.cols) #Character vector listing the covariates we will use in phase I
	> student <- NULL
	if(require(pisa, quietly=TRUE)) {
		data(pisa.student)
		data(pisa.school)
		student = pisa.student[,c('CNT', 'SCHOOLID',
		                          paste0('PV', 1:5, 'MATH'),
		                          paste0('PV', 1:5, 'READ'),
		                          paste0('PV', 1:5, 'SCIE'),
		                          pisa.psa.cols)]
		school = pisa.school[,c('COUNTRY', "CNT", "SCHOOLID",
		                        'SC02Q01', #Public (1) or private (2)
		                        'STRATIO' #Student-teacher ratio 
		)]
		names(school) = c('COUNTRY', 'CNT', 'SCHOOLID', 'PUBPRIV', 'STRATIO')
		school$SCHOOLID = as.integer(school$SCHOOLID)
		school$CNT = as.character(school$CNT)
		student$SCHOOLID = as.integer(student$SCHOOLID)
		student$CNT = as.character(student$CNT)
		student = merge(student, school, by=c('CNT', 'SCHOOLID'), all.x=TRUE)
		student = student[!is.na(student$PUBPRIV),] #Remove rows with missing PUBPRRIV
		rm(pisa.student)
		rm(pisa.school)
	} else {
		data(pisana)
		student = pisana
		rm(pisana)
	}


#### Phase I

Propensity score analysis is generally conducted in two phases. In phase one, the dependent measure of interest is treatment placement. In this example, we will consider attending private school to be the treatment. There are a variety of methods we can use including logistic regression (see the `mlpsa.logistic` function) and classification trees (see the `mlpsa.ctree` function). In this example we will use the `ctree` function in the `party` package to model private school attendance using the conditional inference tree framework.

	> mlctree = mlpsa.ctree(student[,c('CNT','PUBPRIV',pisa.psa.cols)], 
	                        formula=PUBPRIV ~ ., level2='CNT')
	> student.party = getStrata(mlctree, student, level2='CNT')

The `mlpsa.ctree` estimates separate models for each level. As a result, different sets of covariates are likely to be used within each level. The `tree.plot` function creates a heat map of covariate use by level. The shading indicates the shallowest depth each covariate appears. That is, if a covariate is utilized more than once within a tree, then the smallest depth will be used to determine the shading.
	
	> tree.plot(mlctree, level2Col=student$CNT)
	
![Multilevel PSA Tree Plot](/images/multilevelPSA/pisatree.png)

#### Phase II

Phase two involves comparing students between the two groups with *similar* covariate profiles. In the case of classification trees, we consider students in the same leaf node to have sufficient similar covariate balance. However, it is important to verify that sufficient covariate balance has been achieved and the functions in the `PSAgraphics` can assist with that.

First, we will calculate a mean math score of the five plausible values provided. This is not entirely correct and final tabular results should utilize all the plausible values separately and combined to provide a pooled estimate (see the [`survey`](http://faculty.washington.edu/tlumley/survey/) package for example). However, for visualization purposes the mean value will provide a sufficient estimate of the final results.

	> student.party$mathscore = apply(student.party[,paste0('PV', 1:5, 'MATH')], 1, sum) / 5

	> results.psa.math = mlpsa(response=student.party$mathscore, 
	                           treatment=student.party$PUBPRIV, 
	                           strata=student.party$strata, 
	                           level2=student.party$CNT, minN=5)

	 > summary(results.psa.math)
	 Multilevel PSA Model of 694 strata for 64 levels.
	 Approx t: 3.17
	 Confidence Interval: 15.71, 18.41

The `mlpsa` function returns an object of class type `mlpsa`. The S3 methods for `summary`, `print`, `plot`, and `xtable` are implemented. Additionally, the returned object contains the following elements:

* `approx.t` The approximate overall *t*-value.
* `level1.summary` A data frame containing the results of each individual strata.
* `level2.summary` A data frame with the overall results for the clustering level two variable (country in this example).
* `overall.ci` An integer vector with two values for the overall confidence interval.
* `overall.mnx` The overall adjusted mean for the control group (i.e. public schools).
* `overall.mny` The overall adjusted mean for the treatment group (i.e. private schools).
* `overall.n` The overall *n*.
* `overall.nx` The overall *n* for the control group (i.e. public schools).
* `overall.ny` The overall *n* for the treatment group (i.e. private schools).
* `overall.se.wtd` The overall weighted standard error of the mean difference.
* `overall.wtd` The overall weighted difference.
* `removed` An integer vector with the positions of rows from the original data frame that were removed do to insufficiently small strata (default minimum strata size is five, see `minN` parameter of `mlpsa` function).
* `unweighted.summary` Data frame containing the overall unadjusted means for the treatment and control groups.

Other fields not listed above are used for plotting purposes.

	 > results.psa.math$level2.summary[,c('level2','n','Private','Private.n','Public','Public.n',
	                                      'diffwtd','ci.min','ci.max','df')]

	    level2     n  Private Private.n   Public Public.n      diffwtd     ci.min     ci.max    df
	 1     ALB  4596 420.8655       395 373.4865     4201  47.37901925  37.112435  57.645604  4578
	 2     ARG  4707 409.9844      1535 381.7359     3172  28.24847947  19.860348  36.636611  4681
	 3     AUS 14251 525.7220      5536 497.6153     8715  28.10669008  23.477031  32.736349 14193
	 4     AUT  6405 494.3873       842 500.5522     5563  -6.16487922 -16.364671   4.034913  6377
	 5     AZE  2520 489.5733        80 437.1517     2440  52.42154979  41.202358  63.640742  2514
	 6     BEL  8488 530.4688      5830 490.3529     2658  40.11586405  32.093483  48.138245  8460
	 7     BGR  4488 510.5927        59 426.6689     4429  83.92386223  64.984555 102.863169  4480
	 8     BRA 19112 420.6211      2265 370.9140    16847  49.70712118  44.353349  55.060893 19032
	 9     CAN 23035 574.6481      1609 512.7957    21426  61.85247013  54.140194  69.564746 22981
	 10    CHE 11645 538.9332       383 529.5514    11262   9.38179653  -4.692227  23.455820 11631
	 11    CHL  5161 432.2037      3022 414.2283     2139  17.97548415   8.280336  27.670632  5135
	 12    COL  7695 410.2222      1448 381.9332     6247  28.28900773  22.479379  34.098637  7651
	 13    CZE  5751 513.4700       270 510.6293     5481   2.84071242  -8.075300  13.756725  5745
	 14    DEU  4555 529.8054       241 510.5953     4314  19.21013318   3.206619  35.213647  4545
	 15    DNK  5839 500.7247      1041 486.4226     4798  14.30205768   5.809685  22.794430  5825
	 16    ESP 25363 501.6931     10034 484.6639    15329  17.02913971  13.582441  20.475839 25295
	 17    EST  4727 514.0800       127 513.8796     4600   0.20041004 -14.952982  15.353802  4723
	 18    FIN  5755 533.7971       279 539.0404     5476  -5.24326787 -18.901244   8.414708  5743
	 19    GBR  8202 538.5366       439 501.2581     7763  37.27846710  27.933956  46.622978  8176
	 20    GRC  4665 489.9827       321 468.1922     4344  21.79050984  10.808237  32.772783  4643
	 21    HKG  4804 552.7403      4486 591.3133      318 -38.57307922 -50.860842 -26.285316  4800
	 22    HRV  3059 468.1499        66 468.1106     2993   0.03929578 -19.559319  19.637911  3051
	 23    HUN  4583 501.5931       539 494.2722     4044   7.32087259  -4.284299  18.926044  4571
	 24    IDN  5136 362.4735      2375 381.7147     2761 -19.24121312 -23.520458 -14.961968  5118
	 25    IRL  3928 493.7999      2462 478.9441     1466  14.85584371   3.175379  26.536309  3916
	 26    ISL  3207 538.7074        17 505.9170     3190  32.79040612  -7.925951  73.506763  3205
	 27    ISR  5607 471.0903       977 445.0270     4630  26.06329628  15.921608  36.204985  5585
	 28    ITA 30234 464.5795      1641 491.9211    28593 -27.34161124 -37.826591 -16.856631 30208
	 29    JOR  6439 419.7438       890 389.4549     5549  30.28893975  20.712494  39.865385  6411
	 30    JPN  6088 521.8295      1672 532.6987     4416 -10.86927112 -20.465999  -1.272544  6070
	 31    KAZ  3688 439.8842       140 415.6892     3548  24.19497667   5.874004  42.515949  3670
	 32    KGZ  4128 418.3577       111 334.0767     4017  84.28102627  70.077098  98.484955  4118
	 33    KOR  4989 549.2523      1898 548.4127     3091   0.83959029  -7.121606   8.800787  4961
	 34    LIE   329 523.4837        18 536.4461      311 -12.96248124 -58.831205  32.906242   327
	 35    LTU  4500 471.7728        44 476.7377     4456  -4.96495842 -26.761449  16.831532  4498
	 36    LUX  4613 498.1891       666 493.4815     3947   4.70766110  -4.990032  14.405354  4599
	 37    LVA  4343 469.8542        30 488.0522     4313 -18.19794867 -49.563973  13.168076  4339
	 38    MAC  5628 527.8113      5392 480.8967      236  46.91466591  34.677379  59.151953  5618
	 39    MEX 38124 430.2105      4044 423.0054    34080   7.20509210   4.038334  10.371850 38038
	 40    MNE    41 386.5525        13 369.8808       28  16.67167582 -19.164385  52.507737    39
	 41    NLD  4667 531.9771      2872 535.8870     1795  -3.90982011 -18.107778  10.288137  4657
	 42    NOR  4353 462.8531        49 499.1450     4304 -36.29191402 -59.376655 -13.207173  4345
	 43    NZL  4643 573.8157       242 519.8993     4401  53.91640732  44.564926  63.267889  4631
	 44    PAN  3608 409.4171       919 344.0597     2689  65.35745641  56.961259  73.753654  3568
	 45    PER  5985 387.2319      1155 357.5425     4830  29.68934040  19.766241  39.612440  5941
	 46    POL  4803 522.3807       328 496.4239     4475  25.95684445  14.719257  37.194432  4787
	 47    PRT  6298 503.2090       682 483.6007     5616  19.60823945  11.927275  27.289204  6286
	 48    QAR  4435 444.2281      3281 384.2296     1154  59.99852251  51.270428  68.726617  4371
	 49    QAT  7856 410.2191      2244 344.6134     5612  65.60569937  58.660056  72.551343  7742
	 50    QCN  4966 627.8917       512 597.5703     4454  30.32137741  15.221133  45.421622  4954
	 51    ROU  1017 370.9295        15 387.7845     1002 -16.85494354 -46.227241  12.517354  1013
	 52    RUS  5308 469.4605        12 469.5508     5296  -0.09026699 -42.681943  42.501409  5306
	 53    SGP  4981 524.9804       126 557.3697     4855 -32.38926291 -53.436208 -11.342317  4973
	 54    SRB  5353 410.7185        53 442.6754     5300 -31.95684080 -57.341095  -6.572587  5347
	 55    SVK  4555 516.3498       343 494.3370     4212  22.01279817   6.597103  37.428493  4547
	 56    SVN  6155 566.1863       129 477.3868     6026  88.79944245  73.648893 103.949992  6145
	 57    SWE  4567 516.5581       543 492.3527     4024  24.20539406  14.103703  34.307085  4555
	 58    TAP  5785 518.3554      2225 566.2069     3560 -47.85153187 -54.453618 -41.249446  5759
	 59    THA  6209 406.9979       800 429.9237     5409 -22.92581091 -35.658631 -10.192991  6197
	 60    TTO  4604 402.7190       815 420.4209     3789 -17.70190852 -25.938477  -9.465340  4594
	 61    TUN  2414 301.4003        95 377.2423     2319 -75.84192626 -88.562235 -63.121618  2404
	 62    TUR   125 578.1132        17 515.9970      108  62.11617396  32.725739  91.506609   121
	 63    URY  5462 469.0636      1018 416.3180     4444  52.74565867  44.245235  61.246083  5414
	 64    USA  5233 504.5025       345 484.7964     4888  19.70610175   7.267340  32.144863  5215

The *Multilevel PSA Assessment Plot* provides a detailed visualization of the results. The default `plot` method is comprised of three panels (each panel can be plotted separately, see the commands below). The panels to the left and bottom represent the means for private and public schools, respectively. Grey dots correspond to individual strata and the colored dots to the overall mean for that country. The main plot is a scatter plot with the overall of public school mean on the *x*-axis and the private school mean on the *y*-axis. The size of the dots are proportional to the number of students sampled within each country. Rug plots on the right and top represent the distribution of scores. For each point, a line is projected parallel to the unit line (i.e. *y=x*) to another line perpendicular to the unit line. The tick marks along that line correspond to the distribution of differences. That is, the distance of each tick mark to the unit line is equal to the difference between private and public school scores for that country (a separate plot for differences is provided below). Therefore, the unit line indicates no difference such that points that lie above the line indicate a difference favoring private schools and points that lie below the line indicate a difference favoring public schools. The dashed blue line parallel to the unit line represents the overall mean difference across all countries and the dashed green line represents the confidence interval of that difference. In this example, given the relatively large *n*, the confidence interval is so narrow that it is practically overlaps the mean.

	> plot(results.psa.math)

![Multilevel PSA Assessment Plot](/images/multilevelPSA/pisaAssessmentPlot.png)

[Click here](/images/multilevelPSA/pisaAssessmentPlotLarge.png) for a larger version of the PSA Assessment Plot.

From this figure, we can conclude that there is a small, statistically significant, effect in favor of private schools. However, for many countries, that difference is small as exemplified by the fact that many of the points cluster around the unit line. And there are some countries where public schools outperform private schools. The largest of these is Tunisia, although the overall performance of that country is also the lowest when adjusted for private or public school attendance.

The difference plot below provides more detail with regard to the distribution of differences. In this plot, the grey points correspond to the difference of each strata (i.e. the leaf nodes from phase I above in this example). The blue dots correspond to the overall difference for each country, and like above, the size is proportional to the number of students sampled within each country. However, this figure also included confidence intervals for each country as well as overall. Since the standard deviation was specified vis-à-vis the `sd` parameter, the scale of the *x*-axis is in standardized units.

	> mlpsa.difference.plot(results.psa.math, sd=mean(student.party$mathscore, na.rm=TRUE))

![Multilevel Difference Plot](/images/multilevelPSA/pisaDiffPlot.png)

You can plot the individual parts of the Multilevel PSA Assessment plot with the following functions.

	> mlpsa.circ.plot(results.psa.math, legendlab=FALSE)
	> mlpsa.distribution.plot(results.psa.math, 'Public')
	> mlpsa.distribution.plot(results.psa.math, 'Private')

