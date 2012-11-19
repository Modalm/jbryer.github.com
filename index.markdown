---
layout: default
title: jason.bryer.org
subtitle: R Packages and Resources by Jason Bryer
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
  <br />{{ post.content | strip_html | truncatewords: 50 }} <a href="{{ post.url }}">continue reading...</a>
  </p>
{% endfor %}

[More blog posts](/blog.html)

__________

### Table of Contents ###

The primary focus of this website is to provide documentation for R packages maintained by [Jason Bryer](jason@bryer.org). These include:

* [`makeR`](/makeR) An R package for managing document building, versioning, and templating.
* [`multilevelPSA`](/multilevelPSA) An R package for estimating and visualizing multilelel propensity score models.
* `sqlutils` An R package to manage a library of SQL queries.
* `pisa` A data-only R package for the 2009 [Programme of International Student Assessment](http://www.oecd.org/pisa/) (PISA) conducted by [Organisation for Economic Co-operation and Development](http://www.oecd.org) (OECD).
* `retention` An R package for estimating and visualizing retention and completion rates. Designed primarily for online, distant education institutions, the method used can be generalized to traditional institutions.
* [`ipeds`](/ipeds) An R package to interface with the Integraded Postsecondary Education Data System.
* [`qualtrics`](/qualtrics) An R package to interface with the [Qualtrics.com](http://qualtrics.com) survey system.
* `likert` An R package to analyze and visualize Likert based items.
* [`irutils`](irutils) An R package containing a collection of useful functions for Institutional Researchers.
* `geocode` An R package to geocode IP addresses using GeoIP's database and street addresses using Google or Yahoo.
