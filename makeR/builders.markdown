---
layout: makeR
title: makeR
subtitle: An R package for managing document templates and versions
---


### Custom Builders
Builders are R functions that define how a particular project should be built. The package contains builders for building Sweave files (`.rnw`), LaTeX files (`.tex`),  [cacheSweave](http://cran.r-project.org/web/packages/cacheSweave/vignettes/cacheSweave.pdf) (`.rnw`), and [knitr](http://yihui.name/knitr/) files. However, it is relatively easy to create custom builders. The following example is contained in the `stocks` demo (see `demo('stocks')`). A builder function contains two defined parameters, `project` and `theenv`. The former is the `Project` class to be built. The latter is an environment that has had all the appropriate `Project` and `Version` properties set. All execution should occur within that environment as to prevent the user's environment to interfere with the build process. Also, this environment will be saved to the build directory which may be useful for debugging issues in the build process. Additional parameters will passed from the `build` function to the appropriate `builder` function using the unnamed paramater feature in R (i.e. `...`).

The following function, `builder.png`, will create a PNG image file from a R script file.

		builder.png <- function(project, theenv, ...) {
			sourceFile = ifelse(is.null(project$SourceFile), '.r$', project$SourceFile)
			wd = eval(getwd(), envir=theenv)
			files = list.files(path=wd, pattern=sourceFile, ignore.case=TRUE)
			for(i in seq_len(length(files))) {
				cat(paste("Executing ", files[i], "...", sep=''))
				sys.source(files[i], envir=theenv)
			}
			return(list.files(path=wd, pattern=".png", ignore.case=TRUE))
		}

The `stocks.R` file is included in the `inst/stocks` directory. Here is the source of that file:

		require(quantmod)
		require(ggplot2)
		getSymbols(stocks,src=src)
		results = data.frame(Open=numeric(), High=numeric(), Low=numeric(), 
							 Close=numeric(), Volume=numeric(), Adjusted=numeric(), Symbol=character())
		for(s in stocks) {
			df = as.data.frame(get(s)[month])
			names(df) = c('Open', 'High', 'Low', 'Close', 'Volume', 'Adjusted')
			df$Symbol = s
			results = rbind(results, df)
		}
		results$day = as.Date(row.names(results))
		ggplot(results, aes(x=day, y=Close, colour=Symbol)) + geom_line()
		ggsave(paste(month, '.png', sep=''))

Now that the `builder` function and R script are defined, we can create a `makeR` `Project`.

		stocksProject = Project(name="stocks", projectDir=projectDir, 
			properties=list(src = "yahoo", stocks = c("GOOG",'AAPL','AMZN','MSFT')))

Next we will define a new version for December 2011.

		stocksProject$newVersion(name='2011-12', properties=list(month='2011-12'))

Print the project summary

		stocksProject

The output of the summary shows that the version was created successfully.

		Project Directory: /Users/jbryer/R/makeR/demo/stocksDemo
		Source Directory: source
		Build Directory: build
		Current build: 1
		Properties:
		  src = yahoo
		  stocks = GOOG, AAPL, AMZN, MSFT
		There are currently 1 versions defined:
		$`1`
		Version 1.1
		Name: 2011-12
		Properties:
		  month = 2011-12
  
We can override the default builder with the custom builder defined above.

		stocksProject$build(builder=builder.png)

Finally, release the version which copies the final PNG file to the release directory.

		stocksProject$release(version='2011-12')

This is the resulting image.

![Stocks Image](2011-12.png)