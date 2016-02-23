---
layout: default
title: Shiny
subtitle: Shiny Applications.
---

Some of the Shiny Apps below are included in the [`IS606`](https://github.com/jbryer/IS606) or [`psa`](https://github.com/jbryer/psa) R package. These packages can be installed from Github using the `devtools` package:

```
devtools::install_github('jbryer/IS606')
devtools::install_github('jbryer/psa')
library(IS606)
```

### Lottery

Web: https://jbryer.shinyapps.io/lottery/

```
shiny_demo('lottery')
```

### Shiny Assessment

Blog post: http://jason.bryer.org/posts/2015-02-22/Assessments_with_Shiny.html

Web: https://jbryer.shinyapps.io/ShinyAssessmentTest/

```
shiny::runGist('a6fb5a3b1d5fd56cff64')
```

### Propensity Score Analysis

Web: https://jbryer.shinyapps.io/psashiny/

```
psa::psa_shiny()
```

### Bayes Billiards Problem

Blog post: http://jason.bryer.org/posts/2015-02-21/Bayes_Billiards_Shiny.html

Web: https://jbryer.shinyapps.io/BayesBilliards/

```
shiny_demo('BayesBilliards')
```

### Gambler's Run


```
shiny_demo('gambler')
```
