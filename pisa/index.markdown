---
layout: pisa	
title: pisa
subtitle: An R data package for the Programme of International Student Assessment (PISA)
---

### Overview

The `pisa` package is a data package that provides R formatted data frames for the 2009 [Programme of International Student Assessment](http://www.pisa.oecd.org) (PISA). This package is approximately 75mb and can be installed using the `install_github` function in the `devtools` package.

	require(devtools)
	install_github('pisa', 'jbryer')

The available data frames are (`data(package='pisa')`):

* `pisa.catalog.cognitive` Data catalog for cognitive file.
* `pisa.catalog.parent` Data catalog for parent file.
* `pisa.catalog.school` Data catalog for school file.
* `pisa.catalog.scoredcognitive` Data catalog for scored cognitive file.
* `pisa.catalog.student` Data catalog for the student file.
* `pisa.countries` Data frame containing two columns, one with three letter country abbreviation and the other with the full country name.
* `pisa.items` Item results of the 2009 Programm of International Student Assessment.
* `pisa.parent` Parent survey results of the 2009 Programm of International Student Assessment.
* `pisa.school` School results of the 2009 Programm of International Student Assessment.
* `pisa.student` Student results of the 2009 Programm of International Student Assessment.

Data dictionaries and questions can be downloaded at [http://pisa2009.acer.edu.au/downloads.php](http://pisa2009.acer.edu.au/downloads.php).
