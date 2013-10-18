--- 
layout: post
title: Version 1.1 of the likert Pakcage Released to CRAN
tags: R R-Bloggers
type: post
published: true
status: publish
---

After some delay, we are happy to finally get version 1.1 of the `likert` package on [CRAN](http://cran.r-project.org/web/packages/likert/index.html). Although labeled 1.1, this is actually the first version of the package released to CRAN. After receiving some wonderful feedback from [useR! this year](http://www.edii.uclm.es/~useR-2013/), we held back releasing until we implemented many of the feature suggestions. The [NEWS](http://cran.r-project.org/web/packages/likert/NEWS) file details most of what is in this release, but here are some highlights:

* Simplify analyzing and visualizing Likert type items using R's familiar `print`, `summary`, and `plot` functions.
* Create LaTeX and HTML formatted tables using the [`xtable`](http://cran.r-project.org/web/packages/xtable/index.html) package. See the [`likert-xtable` vignette](https://github.com/jbryer/likert/blob/master/vignettes/likert-xtable.pdf?raw=true) for sample output.

There are four demos available:

* [`likert`](https://github.com/jbryer/likert/blob/master/demo/likert.R) - Shows most of the features of the package using data from the Programme of International Student Assessment (PISA).
* [`PreSummarized`](https://github.com/jbryer/likert/blob/master/demo/PreSummarized.R) - *Currently only in the development version* Shows how to use the `likert` function with pre-summarized data.
* [`RecodeFactors`](https://github.com/jbryer/likert/blob/master/demo/RecodeFactors.R) - Shows how to recode factors. Useful for setting up your data before calling `likert`.
* [`UnusedLevels`](https://github.com/jbryer/likert/blob/master/demo/UnusedLevels.R) - This demo shows how to address the "All items (columns) must have the same number of levels" error that occurs when the items passed to `likert` do not have the same number of levels.

The useR! 2013 slides can be downloaded [from Github](https://github.com/jbryer/likert/blob/master/useR%202013/Slides/Slides.pdf?raw=true) as well as the [abstract](https://github.com/jbryer/likert/blob/master/useR%202013/Abstract/Speerschneider.Bryer.likert.pdf?raw=true). More documentation is available at http://jason.bryer.org/likert and be sure to request feature requests or bug reports on the Github page at https://github.com/jbryer/likert

	install.packages('likert',repos='http://cran.r-project.org')
	require(likert)

![likert Plot](http://jason.bryer.org/images/likert/centeredPlot1.png)

The package is hosted on Github at [http://github.com/jbryer/likert]. You can always download the latest development version using `devtools`.
	
	require(devtools)
	install_github('likert','jbryer')

We have already started work on version 1.2 which will include:

* The ability to work with pre-summarized data (already on Github, see the `summary` parameter on the `likert` function and `demo(PreSummarized)` for more details).
* A Shiny App to explore the features of the `likert` package and to work with your own data.
* A more complete vignette.

![likert Plot with histogram](http://jason.bryer.org/images/likert/centeredPlot2.png)
