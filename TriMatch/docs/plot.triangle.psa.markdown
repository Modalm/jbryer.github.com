---
layout: mathjax	
title: TriMatch
subtitle: Propensity Score Matching for Non-Binary Treatments
published: true
status: publish
submenu: trimatch
---


#### `plot.triangle.psa`: Triangle plot. ####

#### Description ####


 Triangle plot showing the fitted values (propensity
 scores) for three different models.


#### Usage ####

     
     plot.triangle.psa(tpsa, point.alpha = 0.3,
     point.size = 5, legend.title = "Treatment",
     text.size = 4, draw.edges = FALSE,
     draw.segments = TRUE, edge.alpha = 0.2,
     edge.color = "grey",
     edge.labels = c("Model 1", "Model 2", "Model 3"), ...)


#### Arguments ####

the results from [`trips`](trips.html) . alpha level for points. point size. title for the legend. text size. draw edges of the triangle. draw segments connecting points across two models. alpha level for edges if drawn. the color for edges if drawn. the labels to use for each edge of the triangle. currently unused.

#### Value ####


 ggplot2 figure


#### Seealso ####


 triangle.psa


