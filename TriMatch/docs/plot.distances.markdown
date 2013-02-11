---
layout: mathjax	
title: TriMatch
subtitle: Propensity Score Matching for Non-Binary Treatments
published: true
status: publish
submenu: trimatch
---


#### `plot.distances`: Barplot for the sum of distances. ####

#### Description ####


 Barplot for the sum of distances.


#### Usage ####

     
     plot.distances(tmatch, caliper = 0.25, label = FALSE)


#### Arguments ####

the results of [`trimatch`](trimatch.html) . a vector indicating where vertical lines should be drawn as a factor of the standard deviation. Rosenbaum and Rubin (1985) suggested one quarter of one standard deviation. label the bars that exceed the minimum caliper.

#### Seealso ####


 triangle.match


