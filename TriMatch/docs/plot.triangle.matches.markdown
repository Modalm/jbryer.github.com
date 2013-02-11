---
layout: mathjax	
title: TriMatch
subtitle: Propensity Score Matching for Non-Binary Treatments
published: true
status: publish
submenu: trimatch
---


#### `plot.triangle.matches`: Triangle plot drawing matched triplets. ####

#### Description ####


 This plot function adds a layer to
  [`plot.triangle.psa`](plot.triangle.psa.html) drawing matched triplets.
 If `p` is supplied, this function will simply draw
 on top of the pre-existing plot, otherwise
  [`plot.triangle.psa`](plot.triangle.psa.html) will be called first.


#### Usage ####

     
     plot.triangle.matches(tmatch, sample = 0.05,
     rows = sample(nrow(tmatch), nrow(tmatch) * sample),
     line.color = "black", line.alpha = 0.5,
     point.color = "black", point.size = 3, p, ...)


#### Arguments ####

matched triplets from  `link{triangle.match}` . an number between 0 and 1 representing the percentage of matched triplets to draw. an integer vector corresponding to the rows in `tmatch` to draw. the line color. the alpha for the lines. color of matched triplet points. point size for matched triplets. a ggplot to add the match lines. If NULL, then  [`plot.triangle.psa`](plot.triangle.psa.html) . other parameters passed to  [`plot.triangle.psa`](plot.triangle.psa.html) .

#### Details ####


 If this function calls [`plot.triangle.psa`](plot.triangle.psa.html) ,
 it will only draw line segements and points for those
 data rows that were used in the matching procedure. That
 is, data elements not matched will be excluded from the
 figure. To plot all segments and points regardless if
 used in matching, set `p = plot(tpsa)` .


#### Value ####


 a `ggplot2` graphic.


#### Seealso ####


 plot.triangle.psa
 
 triangle.match


