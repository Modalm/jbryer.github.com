---
layout: makeR
title: makeR
subtitle: An R package for managing document templates and versions
---

### Overview

`makeR` is an R package to help manage R projects (e.g. Sweave reports, R scripts, LaTeX documents) where multiple versions are created based upon a single source repository. For example, a monthly report where each versions is identitcal with the exception of easily definable parameters (e.g. date ranges for data extraction, title, etc.). This package is not meant to assist with package development or more complex data analysis projects. For those types of projects, consider [`devtools`](http://github.com/hadley/devtools) or [`ProjectTemplate`](http://projecttemplate.net), respectively.

To install the latest development version using `devtools`, type following in R:

		require(devtools)
		install_github('makeR', 'jbryer')

There are two demos that demonstrate the many of the features of the `makeR` package.

		require(makeR)
		demo('rbloggers')
		demo('stocks')


