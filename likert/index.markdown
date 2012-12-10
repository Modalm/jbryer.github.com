---
layout: default	
title: likert
subtitle: An R package analyzing and visualizing Likert items
---

### Overview

The latest version of the `likert` package can be installed from [Github](http://github.com/jbryer/likert) using the `devtools` package:

	require(devtools)
	install_github('likert', 'jbryer')

The following will analyze the reading attitude items from the [Programme of International Student Assessment](http://www.pisa.oecd.org).

	require(likert)
	data(pisa)
	items28 = pisa[,substr(names(pisa), 1,5) == 'ST24Q']
	names(items28) = c("I read only if I have to.",
	   "Reading is one of my favorite hobbies.",
	   "I like talking about books with other people.",
	   "I find it hard to finish books.",
	   "I feel happy if I receive a book as a present.",
	   "For me, reading is a waste of time.",
	   "I enjoy going to a bookstore or a library.",
	   "I read only to get information that I need.",
	   "I cannot sit still and read for more than a few minutes.",
	   "I like to express my opinions about books I have read.",
	   "I like to exchange books with my friends")
	for(i in 1:ncol(items28)) {
		items28[,i] = factor(items28[,i], levels=1:4, 
			labels=c('Strongly disagree', 'Disagree', 'Agree', 'Strongly Agree'),
			ordered=TRUE)
	}

	plotBarchartTable(items28, low.color='maroon', high.color='burlywood4')

![Likert Bar Chart](PISA29BarchartTable.png)

	plotBarchartTable(items28, grouping=pisa$CNT, low.color='maroon', high.color='burlywood4')

![Likert Bar Chart Grouped by Country](PISA29BarchartTable2.png)


![Likert Heat Map](PISA29HeatmapTable.png)

