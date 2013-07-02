---
layout: default	
title: likert
subtitle: An R package analyzing and visualizing Likert items
published: true
status: publish
submenu: none
---


 
### Overview
 
The latest version of the `likert` package can be installed from [Github](http://github.com/jbryer/likert) using the `devtools` package:
 
	require(devtools)
	install_github('likert', 'jbryer')
 
The following will analyze the reading attitude items from the [Programme of International Student Assessment](http://www.pisa.oecd.org).
 
 

    require(likert)
    ls("package:likert")

    ## [1] "likert"              "likert.bar.plot"     "likert.density.plot" "recode"

 
#### Item 28: Reading Attitudes
 

    data(pisaitems)
    
    items28 = pisaitems[, substr(names(pisaitems), 1, 5) == "ST24Q"]
    head(items28)

    ##                 ST24Q01           ST24Q02           ST24Q03           ST24Q04           ST24Q05
    ## 68038          Disagree    Strongly agree    Strongly agree Strongly disagree    Strongly agree
    ## 68039             Agree Strongly disagree Strongly disagree    Strongly agree Strongly disagree
    ## 68040    Strongly agree Strongly disagree Strongly disagree             Agree Strongly disagree
    ## 68041          Disagree          Disagree             Agree Strongly disagree          Disagree
    ## 68042 Strongly disagree          Disagree Strongly disagree          Disagree Strongly disagree
    ## 68043             Agree Strongly disagree Strongly disagree             Agree Strongly disagree
    ##                 ST24Q06           ST24Q07           ST24Q08           ST24Q09           ST24Q10
    ## 68038 Strongly disagree             Agree          Disagree Strongly disagree             Agree
    ## 68039             Agree Strongly disagree             Agree    Strongly agree Strongly disagree
    ## 68040    Strongly agree Strongly disagree             Agree          Disagree          Disagree
    ## 68041          Disagree             Agree Strongly disagree Strongly disagree             Agree
    ## 68042          Disagree          Disagree             Agree             Agree             Agree
    ## 68043             Agree Strongly disagree             Agree    Strongly agree Strongly disagree
    ##                 ST24Q11
    ## 68038             Agree
    ## 68039 Strongly disagree
    ## 68040 Strongly disagree
    ## 68041             Agree
    ## 68042 Strongly disagree
    ## 68043 Strongly disagree

    ncol(items28)

    ## [1] 11

    
    items28 <- rename(items28, c(ST24Q01 = "I read only if I have to.", ST24Q02 = "Reading is one of my favorite hobbies.", 
        ST24Q03 = "I like talking about books with other people.", ST24Q04 = "I find it hard to finish books.", ST24Q05 = "I feel happy if I receive a book as a present.", 
        ST24Q06 = "For me, reading is a waste of time.", ST24Q07 = "I enjoy going to a bookstore or a library.", ST24Q08 = "I read only to get information that I need.", 
        ST24Q09 = "I cannot sit still and read for more than a few minutes.", ST24Q10 = "I like to express my opinions about books I have read.", 
        ST24Q11 = "I like to exchange books with my friends"))

 

    l28 = likert(items28)
    l28

    ##                                                        Item Strongly disagree Disagree Agree Strongly agree
    ## 1                                 I read only if I have to.             22.82    35.91 30.54         10.733
    ## 2                    Reading is one of my favorite hobbies.             20.32    36.32 31.93         11.421
    ## 3             I like talking about books with other people.             21.25    33.74 35.96          9.045
    ## 4                           I find it hard to finish books.             24.96    40.39 26.51          8.140
    ## 5            I feel happy if I receive a book as a present.             19.28    27.65 40.17         12.892
    ## 6                       For me, reading is a waste of time.             42.24    40.65 10.98          6.137
    ## 7                I enjoy going to a bookstore or a library.             17.84    33.37 36.88         11.904
    ## 8               I read only to get information that I need.             14.98    35.42 35.76         13.841
    ## 9  I cannot sit still and read for more than a few minutes.             33.12    43.13 16.92          6.836
    ## 10   I like to express my opinions about books I have read.             13.54    27.54 43.82         15.103
    ## 11                 I like to exchange books with my friends             22.52    33.02 32.08         12.379

    summary(l28)

    ##                                                        Item   low neutral  high  mean     sd
    ## 1                                 I read only if I have to. 58.73      NA 41.27 2.292 0.9369
    ## 2                    Reading is one of my favorite hobbies. 56.64      NA 43.36 2.345 0.9277
    ## 3             I like talking about books with other people. 54.99      NA 45.01 2.328 0.9090
    ## 4                           I find it hard to finish books. 65.35      NA 34.65 2.178 0.8992
    ## 5            I feel happy if I receive a book as a present. 46.93      NA 53.07 2.467 0.9447
    ## 6                       For me, reading is a waste of time. 82.89      NA 17.11 1.810 0.8612
    ## 7                I enjoy going to a bookstore or a library. 51.21      NA 48.79 2.429 0.9164
    ## 8               I read only to get information that I need. 50.40      NA 49.60 2.485 0.9090
    ## 9  I cannot sit still and read for more than a few minutes. 76.25      NA 23.75 1.975 0.8793
    ## 10   I like to express my opinions about books I have read. 41.08      NA 58.92 2.605 0.9010
    ## 11                 I like to exchange books with my friends 55.54      NA 44.46 2.343 0.9609

 

    plot(l28)

![plot of chunk barchart](/images/likert/barchart1.png) 

    plot(l28, centered = TRUE, wrap = 30)

![plot of chunk barchart](/images/likert/barchart2.png) 

    plot(l28, type = "density")

![plot of chunk barchart](/images/likert/barchart3.png) 

    plot(l28, type = "heat")

![plot of chunk barchart](/images/likert/barchart4.png) 

 

    l28g = likert(items28, grouping = pisaitems$CNT)

    summary(l28g)

    ##            Group                                                     Item   low neutral  high  mean     sd
    ## 1         Canada                                I read only if I have to. 60.83      NA 39.17 2.278 1.0002
    ## 2         Canada                   Reading is one of my favorite hobbies. 61.97      NA 38.03 2.247 0.9946
    ## 3         Canada            I like talking about books with other people. 56.91      NA 43.09 2.275 0.9467
    ## 4         Canada                          I find it hard to finish books. 71.77      NA 28.23 2.054 0.9212
    ## 5         Canada           I feel happy if I receive a book as a present. 50.14      NA 49.86 2.384 0.9731
    ## 6         Canada                      For me, reading is a waste of time. 75.72      NA 24.28 1.945 0.9748
    ## 7         Canada               I enjoy going to a bookstore or a library. 48.27      NA 51.73 2.480 1.0039
    ## 8         Canada              I read only to get information that I need. 61.24      NA 38.76 2.293 0.9193
    ## 9         Canada I cannot sit still and read for more than a few minutes. 76.16      NA 23.84 1.951 0.9534
    ## 10        Canada   I like to express my opinions about books I have read. 46.59      NA 53.41 2.496 0.9273
    ## 11        Canada                 I like to exchange books with my friends 59.60      NA 40.40 2.239 0.9931
    ## 12        Mexico                                I read only if I have to. 58.64      NA 41.36 2.274 0.8913
    ## 13        Mexico                   Reading is one of my favorite hobbies. 51.69      NA 48.31 2.436 0.8727
    ## 14        Mexico            I like talking about books with other people. 53.23      NA 46.77 2.372 0.8832
    ## 15        Mexico                          I find it hard to finish books. 61.02      NA 38.98 2.258 0.8784
    ## 16        Mexico           I feel happy if I receive a book as a present. 42.96      NA 57.04 2.557 0.9166
    ## 17        Mexico                      For me, reading is a waste of time. 88.40      NA 11.60 1.699 0.7522
    ## 18        Mexico               I enjoy going to a bookstore or a library. 53.62      NA 46.38 2.386 0.8478
    ## 19        Mexico              I read only to get information that I need. 43.58      NA 56.42 2.606 0.8836
    ## 20        Mexico I cannot sit still and read for more than a few minutes. 76.99      NA 23.01 1.974 0.8196
    ## 21        Mexico   I like to express my opinions about books I have read. 36.67      NA 63.33 2.690 0.8728
    ## 22        Mexico                 I like to exchange books with my friends 51.75      NA 48.25 2.430 0.9344
    ## 23 United States                                I read only if I have to. 50.17      NA 49.83 2.485 0.9542
    ## 24 United States                   Reading is one of my favorite hobbies. 69.60      NA 30.40 2.107 0.9298
    ## 25 United States            I like talking about books with other people. 59.45      NA 40.55 2.240 0.9077
    ## 26 United States                          I find it hard to finish books. 68.94      NA 31.06 2.138 0.8876
    ## 27 United States           I feel happy if I receive a book as a present. 62.03      NA 37.97 2.166 0.9255
    ## 28 United States                      For me, reading is a waste of time. 74.00      NA 26.00 2.030 0.9463
    ## 29 United States               I enjoy going to a bookstore or a library. 46.56      NA 53.44 2.514 0.9768
    ## 30 United States              I read only to get information that I need. 52.74      NA 47.26 2.439 0.8968
    ## 31 United States I cannot sit still and read for more than a few minutes. 71.15      NA 28.85 2.087 0.9504
    ## 32 United States   I like to express my opinions about books I have read. 49.11      NA 50.89 2.464 0.9178
    ## 33 United States                 I like to exchange books with my friends 65.54      NA 34.46 2.165 0.9386

    plot(l28g)

![plot of chunk centeredPlot](/images/likert/centeredPlot1.png) 

    plot(l28g, centered = TRUE)

![plot of chunk centeredPlot](/images/likert/centeredPlot2.png) 

    plot(l28g, type = "density")

![plot of chunk centeredPlot](/images/likert/centeredPlot3.png) 

 
#### Item 29: How often do you read these materials because you want to?
 

    title <- "How often do you read these materials because you want to?"
    items29 = pisaitems[, substr(names(pisaitems), 1, 5) == "ST25Q"]
    names(items29) = c("Magazines", "Comic books", "Fiction", "Non-fiction books", "Newspapers")
    l29 = likert(items29)
    print(l29)

    ##                Item Never or almost never A few times a year About once a month Several times a month
    ## 1         Magazines                 10.18              20.03              21.33                 31.02
    ## 2       Comic books                 36.75              25.68              15.78                 14.52
    ## 3           Fiction                 17.04              24.74              19.62                 22.30
    ## 4 Non-fiction books                 30.85              30.57              19.55                 13.47
    ## 5        Newspapers                 18.78              18.51              15.73                 23.86
    ##   Several times a week
    ## 1               17.431
    ## 2                7.268
    ## 3               16.312
    ## 4                5.547
    ## 5               23.124

    summary(l29)

    ##                Item   low neutral  high  mean    sd
    ## 1         Magazines 30.22   21.33 48.45 3.255 1.245
    ## 2       Comic books 62.43   15.78 21.79 2.299 1.293
    ## 3           Fiction 41.77   19.62 38.61 2.961 1.343
    ## 4 Non-fiction books 61.42   19.55 19.02 2.323 1.199
    ## 5        Newspapers 37.29   15.73 46.98 3.140 1.442


    plot(l29) + ggtitle(title)

![plot of chunk barchart29](/images/likert/barchart291.png) 

    plot(l29, centered = TRUE) + ggtitle(title)

![plot of chunk barchart29](/images/likert/barchart292.png) 

    plot(l29, centered = TRUE, center = 2.5) + ggtitle(title)

![plot of chunk barchart29](/images/likert/barchart293.png) 

 

    l29g <- likert(items29, grouping = pisaitems$CNT)
    summary(l29g)

    ##            Group              Item   low neutral  high  mean    sd
    ## 1         Canada         Magazines 27.21  24.886 47.90 3.265 1.233
    ## 2         Canada       Comic books 73.02  13.205 13.78 1.978 1.194
    ## 3         Canada           Fiction 38.42  20.255 41.33 3.081 1.359
    ## 4         Canada Non-fiction books 59.75  20.777 19.47 2.379 1.195
    ## 5         Canada        Newspapers 36.01  18.642 45.35 3.113 1.404
    ## 6         Mexico         Magazines 32.28  18.727 48.99 3.249 1.255
    ## 7         Mexico       Comic books 53.45  18.374 28.17 2.573 1.300
    ## 8         Mexico           Fiction 43.60  19.085 37.31 2.897 1.329
    ## 9         Mexico Non-fiction books 62.91  18.549 18.54 2.275 1.201
    ## 10        Mexico        Newspapers 37.01  13.743 49.25 3.198 1.460
    ## 11 United States         Magazines 28.30  24.802 46.89 3.257 1.225
    ## 12 United States       Comic books 81.66   8.149 10.19 1.699 1.116
    ## 13 United States           Fiction 43.10  20.707 36.19 2.902 1.332
    ## 14 United States Non-fiction books 57.91  21.541 20.55 2.427 1.183
    ## 15 United States        Newspapers 45.01  17.479 37.51 2.837 1.432

 

    plot(l29g) + ggtitle(title)

![plot of chunk barchart29g](/images/likert/barchart29g1.png) 

    plot(l29g, centered = TRUE, center = 2.5) + ggtitle(title)

![plot of chunk barchart29g](/images/likert/barchart29g2.png) 

    plot(l29g, type = "density", legend = "Country") + ggtitle(title)

![plot of chunk barchart29g](/images/likert/barchart29g3.png) 

 
