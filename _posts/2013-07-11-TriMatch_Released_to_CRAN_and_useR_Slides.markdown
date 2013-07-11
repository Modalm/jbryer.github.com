--- 
layout: post
title: TriMatch - useR! 2013 Slides and Version 0.9 Released to CRAN
tags: R R-Bloggers
type: post
published: true
status: publish
---

Our presentation on the `TriMatch` package at the useR! 2013 Conference went fantastically. Thanks for those who attended and the wonderful discussion that followed. The slides can be downloaded [from Github](https://github.com/jbryer/TriMatch/blob/master/useR%202013/Slides/Slides.pdf?raw=true) as well as the [abstract](https://github.com/jbryer/TriMatch/blob/master/useR%202013/Abstract/Bryer.TriMatch.pdf?raw=true). To coincide with our presentation version 0.9 has been released to CRAN. The package includes a [vignette](https://github.com/jbryer/TriMatch/blob/master/inst/doc/TriMatch.pdf?raw=true) and two demos.

	install.packages('TriMatch',repos='http://cran.r-project.org')
	require(TriMatch)
	vignette('TriMatch')
	demo(tutoring)
	demo(nmes)

![Triangle Plot](https://raw.github.com/jbryer/TriMatch/master/useR%202013/Abstract/matches.png)

The package is hosted on Github at [http://github.com/jbryer/TriMatch]. You can always download the latest development version using `devtools`.
	
	require(devtools)
	install_github('TriMatch','jbryer')
	