---
layout: default
title: Shiny
subtitle: Shiny Applications.
---

This page has links to some [Shiny](http://shiny.rstudio.com) applications I have developed. Where available, I have included links to the app running on [shinyapps.io](http://shinyapps.io) as well as R code to run the apps locally.

Some of the Shiny Apps below are included in the [`IS606`](https://github.com/jbryer/IS606) or [`psa`](https://github.com/jbryer/psa) R package. These packages can be installed from Github using the `devtools` package:

```
devtools::install_github('jbryer/IS606')
devtools::install_github('jbryer/psa')
```

### Lottery

Web: [jbryer.shinyapps.io/lottery/](https://jbryer.shinyapps.io/lottery/)

```
library(IS606)
shiny_demo('lottery')
```

### Shiny Assessment

Blog post: [jason.bryer.org/posts/2015-02-22/Assessments_with_Shiny.html](http://jason.bryer.org/posts/2015-02-22/Assessments_with_Shiny.html)

Web: [jbryer.shinyapps.io/ShinyAssessmentTest/](https://jbryer.shinyapps.io/ShinyAssessmentTest/)

```
shiny::runGist('a6fb5a3b1d5fd56cff64')
```

### Propensity Score Analysis

Web: [jbryer.shinyapps.io/psashiny/](https://jbryer.shinyapps.io/psashiny/)

```
psa::psa_shiny()
```

### Bayes Billiards Problem

Blog post: [http://jason.bryer.org/posts/2015-02-21/Bayes_Billiards_Shiny.html](http://jason.bryer.org/posts/2015-02-21/Bayes_Billiards_Shiny.html)

Web: [jbryer.shinyapps.io/BayesBilliards/](http://jason.bryer.org/posts/2015-02-21/Bayes_Billiards_Shiny.html)

```
library(IS606)
shiny_demo('BayesBilliards')
```

### Gambler's Run


```
library(IS606)
shiny_demo('gambler')
```
