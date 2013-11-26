---
layout: default
title: Jason.Bryer.org
subtitle: R Packages and Resources by Jason Bryer
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

The primary focus of this website is to provide documentation for R packages I maintain. These include:

* [`PSAboot`](/PSAboot) Bootstrapping for Propensity Score Analysis.
* [`TriMatch`](/TriMatch) Propensity score matching for non-binary treatments.
* [`multilevelPSA`](/multilevelPSA) An R package for estimating and visualizing multilelel propensity score models.

* [`geocode`](https://github.com/jbryer/geocode) An R package to geocode IP addresses using GeoIP's database and street addresses using Google or Yahoo.
* [`ipeds`](/ipeds) An R package to interface with the Integraded Postsecondary Education Data System.
* [`irutils`](/irutils) An R package containing a collection of useful functions for Institutional Researchers.
* [`likert`](/likert) An R package to analyze and visualize Likert based items.
* [`makeR`](/makeR) An R package for managing document building, versioning, and templating.
* [`naep`](/naep) An R package to interface with the National Assessment of Educational Progress (NAEP) restricted use databases. This includes access any analyzing data using the replicate weights and multiple plausible values.
* [`pisa`](/pisa) A data-only R package for the 2009 [Programme of International Student Assessment](http://www.oecd.org/pisa/) (PISA) conducted by [Organisation for Economic Co-operation and Development](http://www.oecd.org) (OECD).
* [`qualtrics`](/qualtrics) An R package to interface with the [Qualtrics.com](http://qualtrics.com) survey system.
* [`Rd2markdown`](/Rd2markdown) Functions to convert Rd help files to markdown format.
* [`retention`](/retention) An R package for estimating and visualizing retention and completion rates. Designed primarily for online, distant education institutions, the method used can be generalized to traditional institutions.
* [`ruca`](https://github.com/jbryer/ruca) An R package to get [Rural-Urban Commuting Area](http://depts.washington.edu/uwruca/index.php) (RUCA) Codes from zip codes.
* [`sqlutils`](/sqlutils) An R package to manage a library of SQL queries.
* [`timeline`](/timeline) An R package to create timeline figures.
