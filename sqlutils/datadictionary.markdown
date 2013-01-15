---
layout: default
title: sqlutils
subtitle: An R package for managing a library of database queries
submenu: sqlutils
---

### Data Dictionary

The `sqlutils` contains a data dictionary script as a vignette that can be recreated reflecting your own library of SQL files. You can view the [source on Github](https://github.com/jbryer/sqlutils/blob/master/inst/doc/DataDictionary.Rnw) and the results of running the Sweave file for the two included SQL scripts is provided below (and can also be [downloaded here](https://github.com/jbryer/sqlutils/blob/master/inst/doc/DataDictionary.pdf?raw=true)). The script will loop over each query returned by `getQueries()` and for each attempting to execute the query using the default values (see `?sqldoc`). By executing the query the script can determine the structure of the data frame returned for the database. That information is included in the resulting PDF file. 


<embed src="DataDictionary.pdf" width="100%" height="450" type='application/pdf' />

