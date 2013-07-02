---
layout: default	
title: timeline
subtitle: Create timeline Figures in R
published: true
status: publish
submenu: timeline
---

The `timeline` package provides a function to create timeline figures in a style similar to [Preceden](http://www.preceden.com/).

	install.packages('timeline', repos='http://cran.r-project.org')
	require(timeline)
	data(ww2)
	timeline(ww2, ww2.events, event.spots=2, event.label='', event.above=FALSE)

![Timeline of World War II](http://jason.bryer.org/images/timeline/ww2.png)

#### Shiny App

The `ww2` demo (type `demo(ww2)` at the R console to start) provides many variations of the timeline figure. There is also a Shiny app to explore some of the parameters to the `timeline` function.
	
	timelineShinyDemo()

Or try the Shiny App from the [RStudio Server](http://rstudio.com) at [http://spark.rstudio.com/jbryer/timeline/](http://spark.rstudio.com/jbryer/timeline/).


#### Development Version

Install the latest development version using `devtools`:

	require(devtools)
	install_github('timeline','jbryer')
