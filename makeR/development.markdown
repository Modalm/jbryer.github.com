---
layout: default
title: makeR
subtitle: An R package for managing document templates and versions
submenu: makeR
---

### Development
The `makeR` package was developing using the `devtools` package. The following commands provide an easy and efficient approach to package development.

	setwd("~/Project") #Change to your working directory
	document("makeR")
	check_doc("makeR")
	build("makeR")
	install("makeR")
	check("makeR")
	library(makeR)
	ls('package:makeR')
