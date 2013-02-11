---
layout: mathjax	
title: TriMatch
subtitle: Propensity Score Matching for Non-Binary Treatments
published: true
status: publish
submenu: trimatch
---


#### `unmatched`: Returns rows from [`trips`](trips.html) that were not matched by [`trimatch`](trimatch.html) . ####

#### Description ####


 This function returns a subset of [`trips`](trips.html) 
 that were not matched by [`trimatch`](trimatch.html) . All data
 frame methods work with the returned object but special
  `summary` function will provided relevant
 information.


#### Usage ####

     
     unmatched(tmatch)


#### Arguments ####

the results of [`trimatch`](trimatch.html) .

#### Value ####


 a data frame of unmatched rows.


