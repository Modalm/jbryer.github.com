---
layout: default	
title: likert
subtitle: An R package analyzing and visualizing Likert items
published: true
status: publish
submenu: none
---

 
### Overview
 
The latest version of the `likert` package can be installed from [Github](http://github.com/jbryer/likert) using the `devtools` package:
 
	require(devtools)
	install_github('likert', 'jbryer')
 
The following will analyze the reading attitude items from the [Programme of International Student Assessment](http://www.pisa.oecd.org).
 
 

    require(likert)
    ls("package:likert")

    ## [1] "likert"              "likert.bar.plot"     "likert.density.plot" "recode"

 

