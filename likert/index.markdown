---
layout: default	
title: likert
subtitle: An R package analyzing and visualizing Likert items
published: true
status: publish
submenu: likert
---
 


 
### Overview
 
The latest version of the `likert` package can be installed from [Github](http://github.com/jbryer/likert) using the `devtools` package:
 
	require(devtools)
	install_github('likert', 'jbryer')
 
The following will analyze the reading attitude items from the [Programme of International Student Assessment](http://www.pisa.oecd.org).
 
 

    require(likert)
    ls("package:likert")

    ## [1] "likert"                "likert.bar.plot"       "likert.density.plot"   "likert.heat.plot"     
    ## [5] "likert.histogram.plot" "recode"                "reverse.levels"        "shinyLikert"

 
#### Item 28: Reading Attitudes
 

    data(pisaitems)
    
    items28 <- pisaitems[, substr(names(pisaitems), 1, 5) == "ST24Q"]
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

 

    l28 <- likert(items28)
    summary(l28)

    ##                                                        Item   low neutral  high  mean     sd
    ## 10   I like to express my opinions about books I have read. 41.08      NA 58.92 2.605 0.9010
    ## 5            I feel happy if I receive a book as a present. 46.93      NA 53.07 2.467 0.9447
    ## 8               I read only to get information that I need. 50.40      NA 49.60 2.485 0.9090
    ## 7                I enjoy going to a bookstore or a library. 51.21      NA 48.79 2.429 0.9164
    ## 3             I like talking about books with other people. 54.99      NA 45.01 2.328 0.9090
    ## 11                 I like to exchange books with my friends 55.54      NA 44.46 2.343 0.9609
    ## 2                    Reading is one of my favorite hobbies. 56.64      NA 43.36 2.345 0.9277
    ## 1                                 I read only if I have to. 58.73      NA 41.27 2.292 0.9369
    ## 4                           I find it hard to finish books. 65.35      NA 34.65 2.178 0.8992
    ## 9  I cannot sit still and read for more than a few minutes. 76.25      NA 23.75 1.975 0.8793
    ## 6                       For me, reading is a waste of time. 82.89      NA 17.11 1.810 0.8612

 
Some plots...
 

    plot(l28)

![plot of chunk barchart](/images/likert/barchart1.png) 

    plot(l28, centered = FALSE, wrap = 30)

![plot of chunk barchart](/images/likert/barchart2.png) 

    plot(l28, type = "density")

![plot of chunk barchart](/images/likert/barchart3.png) 

    plot(l28, type = "heat")

![plot of chunk barchart](/images/likert/barchart4.png) 

 
Grouped by country...
 

    l28g <- likert(items28, grouping = pisaitems$CNT)
    print(l28g)

    ##            Group                                                     Item Strongly disagree Disagree  Agree
    ## 1         Canada                                I read only if I have to.             25.70    35.13 24.884
    ## 2         Canada                   Reading is one of my favorite hobbies.             26.78    35.19 24.636
    ## 3         Canada            I like talking about books with other people.             25.23    31.68 33.471
    ## 4         Canada                          I find it hard to finish books.             31.33    40.44 19.698
    ## 5         Canada           I feel happy if I receive a book as a present.             23.48    26.66 37.827
    ## 6         Canada                      For me, reading is a waste of time.             40.10    35.62 13.962
    ## 7         Canada               I enjoy going to a bookstore or a library.             20.81    27.45 34.641
    ## 8         Canada              I read only to get information that I need.             20.70    40.54 27.558
    ## 9         Canada I cannot sit still and read for more than a few minutes.             38.41    37.75 14.207
    ## 10        Canada   I like to express my opinions about books I have read.             17.17    29.42 40.089
    ## 11        Canada                 I like to exchange books with my friends             28.38    31.22 28.563
    ## 12        Mexico                                I read only if I have to.             21.88    36.77 33.455
    ## 13        Mexico                   Reading is one of my favorite hobbies.             15.26    36.43 37.791
    ## 14        Mexico            I like talking about books with other people.             18.44    34.79 37.891
    ## 15        Mexico                          I find it hard to finish books.             21.09    39.92 31.072
    ## 16        Mexico           I feel happy if I receive a book as a present.             15.49    27.47 42.867
    ## 17        Mexico                      For me, reading is a waste of time.             44.76    43.64  8.519
    ## 18        Mexico               I enjoy going to a bookstore or a library.             15.95    37.68 38.232
    ## 19        Mexico              I read only to get information that I need.             11.46    32.12 40.781
    ## 20        Mexico I cannot sit still and read for more than a few minutes.             30.28    46.71 18.355
    ## 21        Mexico   I like to express my opinions about books I have read.             10.87    25.80 46.809
    ## 22        Mexico                 I like to exchange books with my friends             18.32    33.43 35.174
    ## 23 United States                                I read only if I have to.             17.17    33.00 33.972
    ## 24 United States                   Reading is one of my favorite hobbies.             29.08    40.52 21.033
    ## 25 United States            I like talking about books with other people.             24.32    35.14 32.790
    ## 26 United States                          I find it hard to finish books.             25.32    43.62 22.961
    ## 27 United States           I feel happy if I receive a book as a present.             28.66    33.37 30.716
    ## 28 United States                      For me, reading is a waste of time.             33.18    40.82 15.852
    ## 29 United States               I enjoy going to a bookstore or a library.             18.64    27.91 36.877
    ## 30 United States              I read only to get information that I need.             15.64    37.11 34.995
    ## 31 United States I cannot sit still and read for more than a few minutes.             30.64    40.51 18.321
    ## 32 United States   I like to express my opinions about books I have read.             17.10    32.01 38.321
    ## 33 United States                 I like to exchange books with my friends             27.55    37.99 24.859
    ##    Strongly agree
    ## 1          14.290
    ## 2          13.398
    ## 3           9.619
    ## 4           8.530
    ## 5          12.033
    ## 6          10.315
    ## 7          17.093
    ## 8          11.204
    ## 9           9.632
    ## 10         13.317
    ## 11         11.842
    ## 12          7.901
    ## 13         10.519
    ## 14          8.878
    ## 15          7.912
    ## 16         14.175
    ## 17          3.085
    ## 18          8.147
    ## 19         15.639
    ## 20          4.652
    ## 21         16.518
    ## 22         13.080
    ## 23         15.854
    ## 24          9.365
    ## 25          7.756
    ## 26          8.099
    ## 27          7.252
    ## 28         10.147
    ## 29         16.566
    ## 30         12.260
    ## 31         10.530
    ## 32         12.573
    ## 33          9.599

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

    plot(l28g, include.histogram = TRUE)

![plot of chunk centeredPlot](/images/likert/centeredPlot2.png) 

    plot(l28g, centered = FALSE)

![plot of chunk centeredPlot](/images/likert/centeredPlot3.png) 

    plot(l28g, type = "density")

![plot of chunk centeredPlot](/images/likert/centeredPlot4.png) 

 
#### Item 29: How often do you read these materials because you want to?
 

    title <- "How often do you read these materials because you want to?"
    items29 <- pisaitems[, substr(names(pisaitems), 1, 5) == "ST25Q"]
    names(items29) <- c("Magazines", "Comic books", "Fiction", "Non-fiction books", "Newspapers")
    l29 <- likert(items29)
    summary(l29)

    ##                Item   low neutral  high  mean    sd
    ## 1         Magazines 30.22   21.33 48.45 3.255 1.245
    ## 5        Newspapers 37.29   15.73 46.98 3.140 1.442
    ## 3           Fiction 41.77   19.62 38.61 2.961 1.343
    ## 2       Comic books 62.43   15.78 21.79 2.299 1.293
    ## 4 Non-fiction books 61.42   19.55 19.02 2.323 1.199

 
Some plots...
 

    plot(l29) + ggtitle(title)

![plot of chunk barchart29](/images/likert/barchart291.png) 

    plot(l29, centered = FALSE) + ggtitle(title)

![plot of chunk barchart29](/images/likert/barchart292.png) 

    plot(l29, center = 2.5) + ggtitle(title)

![plot of chunk barchart29](/images/likert/barchart293.png) 

 
Grouped by Country...
 

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

    plot(l29g, centered = FALSE, center = 2.5) + ggtitle(title)

![plot of chunk barchart29g](/images/likert/barchart29g2.png) 

    plot(l29g, type = "density", legend = "Country") + ggtitle(title)

![plot of chunk barchart29g](/images/likert/barchart29g3.png) 

 
