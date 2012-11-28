--- 
layout: post
title: Fifty Shades of Grey in R
published: true
tags: R R-Bloggers ggplot2
type: post
status: publish
---

My wife went out to her book group tonight and their book of the month was *50 Shades of Grey*. Sadly, I could think of is that plotting 50 shades in R would be a neat exercise.

	require(ggplot2)
	grey50 <- data.frame(
		x = rep(1:10, 5),
		y = rep(1:5, each=10),
		c = unlist(lapply(seq(10,255,5), FUN=function(x) { rgb(x,x,x, max=255) })),
		t = unlist(lapply(seq(10,255,5), FUN=function(x) { ifelse(x > 255/2, 'black', 'white') }))
	)
	ggplot(grey50, aes(x=x, y=y, fill=c, label=c, color=t)) + 
		geom_tile() + geom_text(size=4) +
		scale_fill_identity() + scale_color_identity() + ylab(NULL) + xlab(NULL) + 
		theme(axis.ticks=element_blank(), axis.text=element_blank())

![Fifty Shades of Grey in R](/images/FiftyShadesOfGrey.png)
