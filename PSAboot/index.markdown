---
layout: default	
title: PSAboot
subtitle: Bootstrapping for Propensity Score Analysis
published: true
status: publish
submenu: psaboot
---
 
# Abstract
 
As the popularity of propensity score methods for estimating causal effects in observational studies increase, the choice of specific method has also increased. Rosenbaum (2012) has suggested that there are substantial benefits for testing a hypothesis more than once. Moreover, with the wide availability of high power computers resampling methods such as bootstrapping (Efron, 1979) have become popular for providing more stable estimates of the sampling distribution. This paper introduces the `PSAboot` package for R that provides functions for bootstrapping propensity score methods. It deviates from traditional bootstrapping methods by allowing for different sampling specifications for treatment and control groups. Additionally, this framework will provide estimates using multiple methods for each bootstrap sample. Two examples are discussed: the classic National Work Demonstration and PSID (Lalonde, 1986) study and a study on tutoring effects on student grades.
 
# Installation
 
You can download `PSAboot` from CRAN:
 
 
    install.packages("PSAboot", repos = "http://cran.r-project.org")

 
Or the latest development version of the `PSAboot` package can be downloaded from Github using the `devtools` package.
 

    devtools::install_github("PSAboot", "jbryer")

Then load the package.
 

    require(PSAboot)

 
# Demos
 
The package includes three demos:
 
* `PSAbootPISA` - This demo performs a propensity score analysis of students who attend private and public school in the United States using the Programme of International Student Assessment (PISA). This demo shows how to bootstrap a dataset when the ratio of treated to control units is large. In these examples we use all treated units but many random smaples of control units to reduce the ratio, and therefore increase the propensity score range (especially when using logistic regression).
* `PSAbootLalonde` This demo performs a propensity score analysis of the classic lalonde dataset. Thid demonstrates how to boostrap both the treatment and control units.
* `PSAbootTutoring` - This demo uses the tutoring dataset from the TriMatch package.
 

-----------------------------
 
# Introduction
 
The `PSAboot` function will perform the actual bootstrapping. It has a number of parameters for to specify how the bootstrap samples are drawn.
 
* `Tr` - a numeric (0 for control and 1 for treatment) or logical vector of treatment indicators.
* `Y` - a numeric vector with the outcome of interest.
* `X` - a data frame of covaraites.
* `M` (default is 100) - the number of bootstrap samples to draw.
* `formu` - the formula for estimating the propensity scores in phase I. Note that the dependent variable does not need to be specified as it will be replaced when combining the `Tr` vector and `X` data frame.
* `control.ratio` (default is 5) - This specifies the sample size of control units as a ratio of treatment units. For example, with a default value of 5 and 100 treatment units, this methods will sample 500 control units for each sample, or the number of control units if smaller than 500. When the ratio of treatment-to-control units increases, the range of propensity scores (using logistic regression) shrinks. Randomly selecting a subset of control units often results in wider and better overlapping distribution of propensity scores. See the the [`PSranges`](http://jason.bryer.org/multilevelPSA/psranges.html) function in the [`multilevelPSA`](http://jason.bryer.org/multilevelPSA) package for more information or this page: [http://jason.bryer.org/multilevelPSA/psranges.html](http://jason.bryer.org/multilevelPSA/psranges.html).
* `control.sample.size` (default is 5 times the number of treatment units) - The number of control units to sample for each bootstrap sample. If specified, this overrides the `control.ratio` parameter.
* `control.replace` (default is `TRUE`) - Specify whether random sampling of control units does so with replacement.
* `treated.sample.size` (default is the number of treatment units) - The number of treatment units to sample for each bootstrap sample.
* `treated.replace` (default is `TRUE`) - Specify whether random sampling of treatment units does so with replacement.
* `methods` - A list of functions to perform a propensity score analysis for each bootstrap sample. See the methods section below.
* `parallel` (default is `TRUE`) - Whether the bootstrapping procedure should be run in parallel.
* `seed` - Seed used for the random number generator. If specified, the random seed will be set to `seed + i` where `i` is the current bootstrap sample in (1, M).
 
Other parameters can be passed to `methods` using the `...` parameter.
 
## Methods
 
The `methods` parameter on the `PSAboot` function specifies the different propensity score methods that will be used. Specifically, for each bootstrap sample drawn, each function will be called. This allows for a comparison of methods across all bootstrap samples. Five methods are included, they are:
 
* `boot.strata` - This method estimates propensity scores using logistic regression and stratifies using quintiles on the propensity scores. Effects within each strata are estimated and aggregated.
* `boot.ctree` - This method creates strata using conditional inference trees vis-a-vis the `ctree` function in the `party` package. Effects within each strata (i.e. leaf node) are estimated and aggregated.
* `boot.rpart` - This method creates strata using classification trees vis-a-vis the `rpart` function. Effects within each strata (i.e. leaf node) are estimated and aggregated.
* `boot.matching` - This method finds matched pairs using the `Match` function in the `Matching` package. A paired dependent sample t-test is used to estimate effect sizes.
* `boot.matchit` - This method finds match pairs using the `matchit` function in the `MatchIt` package. A paired dependent sample t-ttest is used to estimate effect sizes.
 
### Defining Custom Methods
 
It is possible to define a custom method. Each method is a function with, at minimum, the following six parameters:
 
* `Tr` - A logical or integer (0 and 1) vector with treatment indicators.
* `Y` - A numeric vector representing the outcome.
* `X` - A data frame with the covariates.
* `X.trans` - A data frame with factor levels dummy coded.
* `formu` - A formula for estimating propensity scores in phase one.
* `...` - Other parameters passed through from the `PSAboot` function.
 
Each method must return a `list` with three elements:
 
* `summary` - This must be a named numeric vector with at minimum `estimate`, `ci.min`, and `ci.max`, however other values allowed.
* `balance` - This must be a named numeric vector with one element per covariate listed in `X.trans` representing a balance statistic. It is recommended, and the implementation for the built-in methods, to use an absolute standardized effect size. As will be shown below, the summary and plotting functions will include an adjusted balance statistic (i.e. effect size) before adjustment for comparison.
* `details` - This can be an arbitrary object, typically the result of the underlying method used.
 
For example, the `boot.matching.1to3` function below wraps the built-in `boot.matching` method but sets the `M` parameter to 3, thereby performing 1-to-3 matching instead of the default 1-to-1 matching. This framework simplifies the process of using, and comparing, slight variations of different propensity score methods. 
 

    boot.matching.1to3 <- function(Tr, Y, X, X.trans, formu, ...) {
        return(boot.matching(Tr = Tr, Y = Y, X = X, X.trans = X.trans, formu = formu, M = 3, ...))
    }

 
The `PSAboot` function returns an object of class `PSAboot`. The following S3 methods are implemented: `print`, `summary`, `plot`, `boxplot`, and `matrixplot`.
 
-----------------------------
 
# Example: National Work Demonstration and PSID
 
The `lalonde` (Lalonde, 1986) has become the *de defacto* teaching dataset in PSA since Dehejia and Wahba's (1999) re-examination of the National Supported Work Demonstration (NSW) and the Current Population Survey (CPS). 
 
The `lalonde` data set is included in the `MatchIt` package. The crosstab shows that there are 429 control units and 185 treatment units.
 

    data(lalonde, package = "MatchIt")
    table(lalonde$treat)

    
      0   1 
    429 185 


    lalonde.formu <- treat ~ age + I(age^2) + educ + I(educ^2) + black + hispan + married + nodegree + 
        re74 + I(re74^2) + re75 + I(re75^2) + re74 + re75
    boot.lalonde <- PSAboot(Tr = lalonde$treat, Y = lalonde$re78, X = lalonde, formu = lalonde.formu, 
        M = 100, seed = 2112)

    100 bootstrap samples using 5 methods.
    Bootstrap sample sizes:
       Treated=185 (100%) with replacement.
       Control=429 (100%) with replacement.

 
The `summary` function provides numeric results for each method including the overall estimate and confidence interval using the complete sample as well as the pooled estimates and confidence intervals with percentages of the number of confidence intervals that do not span zero.
 

    summary(boot.lalonde)

    Stratification Results:
       Complete estimate = 550
       Complete CI = [-1168, 2269]
       Bootstrap pooled estimate = 636
       Bootstrap pooled CI = [-1123, 2395]
       16% of bootstrap samples have confidence intervals that do not span zero.
          15% positive.
          1% negative.
    ctree Results:
       Complete estimate = 169
       Complete CI = [-1552, 1891]
       Bootstrap pooled estimate = 679
       Bootstrap pooled CI = [-1528, 2886]
       22% of bootstrap samples have confidence intervals that do not span zero.
          18% positive.
          4% negative.
    rpart Results:
       Complete estimate = -875
       Complete CI = [-2567, 816]
       Bootstrap pooled estimate = -825
       Bootstrap pooled CI = [-3997, 2348]
       21% of bootstrap samples have confidence intervals that do not span zero.
          4% positive.
          17% negative.
    Matching Results:
       Complete estimate = -606
       Complete CI = [-1296, 83.2]
       Bootstrap pooled estimate = -577
       Bootstrap pooled CI = [-2814, 1659]
       75% of bootstrap samples have confidence intervals that do not span zero.
          16% positive.
          59% negative.
    MatchIt Results:
       Complete estimate = 617
       Complete CI = [-801, 2034]
       Bootstrap pooled estimate = 365
       Bootstrap pooled CI = [-1287, 2018]
       10% of bootstrap samples have confidence intervals that do not span zero.
          8% positive.
          2% negative.

 
The `plot` function plots the estimate (mean difference) for each bootstrap sample. The default is to sort from largest to smallest estimate for each method separately. That is, rows do not correspond across methods. The `sort` parameter can be set to `none` for no sorting or the name of any `method` to sort only based upon the results of that method. In these cases the rows then correspond to matching bootstrap samples. The blue points correspond to the the estimate for each bootstrap sample and the horizontal line to the confidence interval. Confidence intervals that do not span zero are colored red. The vertical blue line and green lines correspond to the overall pooled estimate and confidence for each method, respectively.
 

    plot(boot.lalonde)

![plot of chunk lalonde.plot](/images/PSAboot/lalonde_plot.png) 

 
The `hist` function plots a histogram of the estimates across all bootstrap samples for each method.
 

    hist(boot.lalonde)

![plot of chunk lalonde.histogram](/images/PSAboot/lalonde_histogram.png) 

 
The `boxplot` function depicts the distribution of estimates for each method along with confidence intervals in green. Additionally, the overall pooled estimate and confidence interval across all bootstrap samples and methods are represented by the vertical blue and green lines, respectively.
 

    boxplot(boot.lalonde)

![plot of chunk lalonde.boxplot](/images/PSAboot/lalonde_boxplot.png) 

 
The `matrixplot` summarizes the estimates across methods for each bootstrap sample. The lower half of the matrix are scatter plots where each point represents the one bootstrap sample. The red line is a Loess regression line. The main diagonal depicts the distribution of effects and the upper half provides the correlation of estimates. In the following example we see that the `ctree` and `Stratification` methods have the strongest agreement with a correlation of 0.63 whereas the `rpart` and `MatchIt` methods have the least agreement with a correlation of 0.22.
 

    matrixplot(boot.lalonde)

![plot of chunk lalonde.matrixplot](/images/PSAboot/lalonde_matrixplot.png) 

 
## Evaluating Balance
 
The strength of propensity score analysis relies on achieving good balance. Typically one would evaluate each covariate separately to ensure that sufficient balance has been achieved. We recommend Helmreich and Pruzek (2009) for a more complete discussion of visualizations to evaluate balance. Given the large number of samples and methods used, it is desirable to have a single metric to evaluate balance. Drawing on the principles of the multiple covariate balance assessment plot (Helmreich & Pruzek, 2009), the `balance` function will estimate the effect of each covariate before and after adjustment. Moreover, to provide a single metric for each sample and method, the mean standardized effect size will be used.
 

    lalonde.bal <- balance(boot.lalonde)
    lalonde.bal

    Unadjusted balance: 0.443234083160092
                   Complete Bootstrap
    Stratification   0.1487    0.1447
    ctree            0.2013    0.1608
    rpart            0.2938    0.2886
    Matching         0.1296    0.1917
    MatchIt          0.2787    0.2744

 
The `plot` function provides density plots of the balance statistics across all bootstrap samples for each method. The mean overall balance is represented by the vertical black lines, the overall balance for the complete dataset is represented by the vertical blue line, and the adjusted balance is represented by the vertical red line. Although no specific guidelines are recommended in the literature, achieving a balance of less than 0.1 is desirable. In this example we see that PSA has reduce the bias in all methods although `rpart`, and to a lesser extent `MatchIt`, did not reduce the bias by as much as would be typically desired.
 

    plot(lalonde.bal)

![plot of chunk lalonde.balance.plot](/images/PSAboot/lalonde_balance_plot.png) 

 
The `boxplot` function provides a more nuanced depiction of the balance by separating on the distribution of balance statistics by individual covariate. In addition to the boxplot of balance statistics, the mean balance statistic is represented by the blue point and the unadjusted balance statistic by the red point. We see that the largest imbalance was in the `black`, `married`, and `re74` (real earnings in 1974) before adjustment. All of the estimates reduced the bias in these covariates although it is worth noting that the `MatchIt` method did not reduce the bias in the `black` covariate to a desirable level.
 
This figure, in conjunction with the density plot above, show that the relatively large mean balance for `rpart` is likely due to some outlier samples where the adjusted balance for `educ`, `re75`, and `hispan` are fairly large. It should be noted that with these exceptions, balance for each covariates are generally less than the unadjusted balance.
 

    boxplot(lalonde.bal)

![plot of chunk lalonde.balance.boxplot](/images/PSAboot/lalonde_balance_boxplot.png) 

 
