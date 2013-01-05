--- 
layout: post
title: Function for Generating LaTeX Tables with Decimal Aligned Numbers
tags: R R-Bloggers LaTeX tables
type: post
published: true
status: publish
---

The [`xtable`](http://xtable.r-forge.r-project.org/) package is tremendously useful for generating LaTeX tables from data frames. It is also pretty easy to customize the output to handle some special cases of LaTeX formatting. The `xtable.decimal` function will create a LaTeX table where  numeric columns will be vertically aligned on the decimal point. In addition to specifying the LaTeX alignment code it will also create appropriate column titles so that the column name spans the two resulting columns. In the following example, we create a data frame with five columns, three of which are `numeric` that we want to display with aligned decimal places. We also have a column of type `character` and another of type `integer`.

	> df <- data.frame(Id=letters[1:10], 
	 		Split1=rnorm(10, mean=0, sd=10),
	 		Numbers=1:10,
	 		Split2=rnorm(10, mean=-1, sd=.5),
	 		Split3=rnorm(10, mean=10, sd=.75))
	#A whole number because prettyNum will not print anything after the decimal point.
	> df[5,'Split1'] = 2 
	> df
	    Id      Split1 Numbers     Split2    Split3
	 1   a -11.4957434       1 -1.0974334 10.895100
	 2   b  11.5985173       2 -1.6314018  9.539108
	 3   c   0.1062397       3 -1.1795816 10.788935
	 4   d   1.1586916       4 -0.4612825 10.462340
	 5   e   2.0000000       5 -0.5154210  9.279148
	 6   f  14.1508151       6 -1.1308398 10.001278
	 7   g  -6.7793246       7  0.3325152 10.026043
	 8   h   1.4984447       8 -1.3638784 10.373774
	 9   i   4.1131184       9 -1.2390835 10.996286
	10   j   5.4228637      10 -1.8332372  9.881770
	> str(df)
	'data.frame':	10 obs. of  5 variables:
	 $ Id     : Factor w/ 10 levels "a","b","c","d",..: 1 2 3 4 5 6 7 8 9 10
	 $ Split1 : num  -0.5072 13.3672 -0.0906 -9.5944 2 ...
	 $ Numbers: int  1 2 3 4 5 6 7 8 9 10
	 $ Split2 : num  -1.359 -0.722 -1.341 -0.359 -1.022 ...
	 $ Split3 : num  9.87 10.29 9.55 8.51 10.36 ...
	 
The `xtable.decimal` function (source code below) has five parameters:

- `x` the data frame to convert.
- `cols` the columns to align. This defaults to columns of type `numeric` but can be specified explicitly as a numeric vector specifying the column position within `x`.
- `colAlignment` all non-aligned columns will be aligned left (i.e. `l`) by default. If you wish to align any columns right (`r`) or centered (`c`), then create a named vector where the name corresponds to the column name (as identified by `names(x)`) and the value the new alignment.
- `tocharFun` the function that will be used to convert the column to a character vector. This is `prettyNum` by default.
- `...` other parameters that are passed to `tocharFun`, `xtable`, and `print.xtable`.

Here we will create the LaTeX table for the data frame created above.

	 xtable.decimal(df, digits=3, 
		 colAlignment=c(Numbers='c'), 
		 caption.placement='bottom', 
		 caption='Test Data Frame')

And the resulting table as it appears in the PDF:

![xtable Output](/images/xtable-decimal.png)

<script src="https://gist.github.com/4458674.js"></script>
