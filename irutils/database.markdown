---
layout: irutils
title: irutils
subtitle: Utilities for Institutional Research
---

### Database Access

For many Institutional Research offices the institutions student information system (SIS) is the most common source of data. Since virtually all SIS systems are backed by a database, extracting data requires extracting data using queries. Typically the language used to extract data is called structured query language (SQL) regardless if the database is provided by Oracle, Microsoft, or an open source options such as MySQL and PostgreSQL. There are a number of functions in this package that will faciliate extracting data from these databases directly into R.

The database access functions provide an interface to a directory of SQL scripts. SQL scripts are simply a plain text file containing the query. The directory containing these files can be determined or set using the `getSQLRepos` and `setSQLRepos` functions, repsectively.

		getSQLRepos()
		[1] "C:/Program Files/R/R-2.14.0/library/irutils/data"

		setSQLRepos("O:/Resources/R Packages/ecir/data")

By convention, all SQL files must use a .sql file extension. The getQueries function will return a list of all the queries available in the current reposistory.

		getQueries()
		 [1] "Alumni"                             "CourseCancelations"
		 [3] "EnrolledStudents"                   "graduates"
		 [5] "GraduatesWithinRange"               "PendingGrades"
		 [7] "TransferCredit"                     "TransferCreditDetail"
		 [9] "WarehouseData"                      "WarehouseDataWithCredits"
		[11] "WarehouseDataWithCreditsNoINLCCS"   "WarehouseDataWithDemographics"
		[13] "warehouseSummary"

The `getQueryDesc` and `getParameters` functions will provide some details about the query in question. In particular, the latter will return the parameters that are required for the query to execute.

		getQueryDesc('GraduatesWithinRange')
		[1] " This function returns all graduates in the given date range."
		getParameters('GraduatesWithinRange')
		[1] "endDate" "startDate"

There are two functions available for executing the query. The `execQuery` will execute the query and return a data frame. The `cacheQuery` however, will first look in the specified directory (by default the dir parameter is set to `getwd()`) for a CSV file that matches the currently request query. That is, the file name (which is returned when this function is executed) is built using a combination of the query name and parameters to uniquely identify it. This is useful when using Sweave and LaTeX for document preparation where the function may be executed multiple times but the data does not change. It is considerably faster to read data from a flat file then it is to query the database each time.

		graduates = execQuery('GraduatesWithinRange', startDate='01-JUL-2010', endDate='30-JUN-2011')
		graduates = cacheQuery('GraduatesWithinRange', startDate='01-JUL-2010', endDate='30-JUN-2011')
		[1] "Cached query file: C:/GraduatesWithinRange.endDate.30-JUN-2011.startDate.01-JUL-2010.csv

#### Creating Your Own Query

To create your own query, simply place the SQL statement in its own text file ending with .sql. Comments can be placed in the file using the # symbol. Placing informative comments at the beginning of the file will be useful since the `getQueryDesc` function will return these comments to the user. Parameters can be placed anywhere in the file and must be enclosed within colons (i.e. the : character). Additionally, parameter names must begin with a letter, contain only letters and numbers, and cannot have any spaces.
