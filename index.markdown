---
layout: default
title: Jason.Bryer.org
subtitle: R Packages and Resources by Jason Bryer, Ph.D.
submenu: none
---

### Latest Blog Post ###
{% for post in site.posts limit:1 %}
  <p>
    <strong><a href="{{ post.url }}">{{ post.title | xml_escape }}</a></strong>
    <span>
    	<em><time datetime="{{ post.date | date: "%Y-%m-%d" }}">
    		{{ post.date | date: "%B %d, %Y" }}
    	</time></em>
    </span>
  <br />{{ post.content | split:"<!--more-->" | strip_html | truncatewords: 50 }} <a href="{{ post.url }}">continue reading...</a>
  </p>
{% endfor %}

[More blog posts](/blog.html)

__________


### Table of Contents ###

The primary focus of this website is to provide documentation for R packages I maintain. Not all packages are published on [CRAN](http://cran.r-project.org). For those, see the installation instructions on the corresponding [Github](https://github.com/jbryer) page.

Package                                        | Description                                  | Github Status      | CRAN Version
-----------------------------------------------|----------------------------------------------|:------------------:|:-------------------:
[`PSAboot`](/PSAboot)                          | Bootstrapping for Propensity Score Analysis. | ![Build Status](https://api.travis-ci.org/jbryer/PSAboot.svg) | [![CRAN_Status_Badge](http://www.r-pkg.org/badges/version/PSAboot)](http://cran.r-project.org/package=PSAboot)
[`TriMatch`](/TriMatch)                        | Propensity score matching for non-binary treatments. | ![Build Status](https://travis-ci.org/jbryer/TriMatch.svg)  | [![CRAN_Status_Badge](http://www.r-pkg.org/badges/version/TriMatch)](http://cran.r-project.org/package=TriMatch)
[`multilevelPSA`](/multilevelPSA)              | An R package for estimating and visualizing multilevel propensity score models. | ![Build Status](https://travis-ci.org/jbryer/multilevelPSA.svg)  | [![CRAN_Status_Badge](http://www.r-pkg.org/badges/version/multilevelPSA)](http://cran.r-project.org/package=multilevelPSA)
[`DataCache`](/DataCache)                      | An R package to manage data caching. |  ![Build Status](https://travis-ci.org/jbryer/DataCache.svg) |
[`geocode`](https://github.com/jbryer/geocode) | An R package to geocode IP addresses using GeoIP's database and street addresses using Google or Yahoo. |   |
[`ipeds`](/ipeds)                              | An R package to interface with the Integraded Postsecondary Education Data System. |   |
[`likert`](/likert)                            | An R package to analyze and visualize Likert based items. | ![Build Status](https://api.travis-ci.org/jbryer/likert.svg) | [![CRAN_Status_Badge](http://www.r-pkg.org/badges/version/likert)](http://cran.r-project.org/package=likert)
[`makeR`](/makeR)                              | An R package for managing document building, versioning, and templating. |   |
[`naep`](/naep)                                | An R package to interface with the National Assessment of Educational Progress (NAEP) restricted use databases. This includes access any analyzing data using the replicate weights and multiple plausible values. |   |
[`pisa`](/pisa)                                | A data-only R package for the 2009 [Programme of International Student Assessment](http://www.oecd.org/pisa/) (PISA) conducted by [Organisation for Economic Co-operation and Development](http://www.oecd.org) (OECD). |   |
[`qualtrics`](/qualtrics)                      | An R package to interface with the [Qualtrics.com](http://qualtrics.com) survey system. |   |
[`Rd2markdown`](/Rd2markdown)                  | Functions to convert Rd help files to markdown format. |   |
[`retention`](/retention)                      | An R package for estimating and visualizing retention and completion rates. Designed primarily for online, distant education institutions, the method used can be generalized to traditional institutions. |   |
[`Rgitbook`](/Rgitbook)                        | Creating Gitbook Projects with R Markdown. |   |
[`ruca`](https://github.com/jbryer/ruca)       | An R package to get [Rural-Urban Commuting Area](http://depts.washington.edu/uwruca/index.php) (RUCA) Codes from zip codes. |   |
[`sqlutils`](/sqlutils)                        | An R package to manage a library of SQL queries. |   | [![CRAN_Status_Badge](http://www.r-pkg.org/badges/version/sqlutils)](http://cran.r-project.org/package=sqlutils)
[`timeline`](/timeline)                        | An R package to create timeline figures. |   | [![CRAN_Status_Badge](http://www.r-pkg.org/badges/version/timeline)](http://cran.r-project.org/package=timeline)


