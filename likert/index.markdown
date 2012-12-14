---
layout: default	
title: likert
subtitle: An R package analyzing and visualizing Likert items
published: true
status: publish
---


 
### Overview
 
The latest version of the `likert` package can be installed from [Github](http://github.com/jbryer/likert) using the `devtools` package:
 
	require(devtools)
	install_github('likert', 'jbryer')
 
The following will analyze the reading attitude items from the [Programme of International Student Assessment](http://www.pisa.oecd.org).
 
 

    require(likert)
    ls("package:likert")

    ## [1] "likert"                "plot.likert.bar"       "plot.likert.matrix"    "print.likert.bar.plot"
    ## [5] "recode"                "xtable.likert"

 

    data(pisaitems)
    items28 = pisaitems[, substr(names(pisaitems), 1, 5) == "ST24Q"]
    items28 <- rename(items28, c(ST24Q01 = "I read only if I have to.", ST24Q02 = "Reading is one of my favorite hobbies.", 
        ST24Q03 = "I like talking about books with other people.", ST24Q04 = "I find it hard to finish books.", ST24Q05 = "I feel happy if I receive a book as a present.", 
        ST24Q06 = "For me, reading is a waste of time.", ST24Q07 = "I enjoy going to a bookstore or a library.", ST24Q08 = "I read only to get information that I need.", 
        ST24Q09 = "I cannot sit still and read for more than a few minutes.", ST24Q10 = "I like to express my opinions about books I have read.", 
        ST24Q11 = "I like to exchange books with my friends"))
    for (i in 1:ncol(items28)) {
        items28[, i] = factor(items28[, i], levels = 1:4, labels = c("Strongly disagree", "Disagree", "Agree", "Strongly Agree"), 
            ordered = TRUE)
    }

 

    l28 <- likert(items28)
    print(l28)

    ##                                                        Item Strongly disagree Disagree Agree Strongly Agree
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

    ##                                                        Item   low  high  mean     sd
    ## 1                                 I read only if I have to. 58.73 41.27 2.292 0.9369
    ## 2                    Reading is one of my favorite hobbies. 56.64 43.36 2.345 0.9277
    ## 3             I like talking about books with other people. 54.99 45.01 2.328 0.9090
    ## 4                           I find it hard to finish books. 65.35 34.65 2.178 0.8992
    ## 5            I feel happy if I receive a book as a present. 46.93 53.07 2.467 0.9447
    ## 6                       For me, reading is a waste of time. 82.89 17.11 1.810 0.8612
    ## 7                I enjoy going to a bookstore or a library. 51.21 48.79 2.429 0.9164
    ## 8               I read only to get information that I need. 50.40 49.60 2.485 0.9090
    ## 9  I cannot sit still and read for more than a few minutes. 76.25 23.75 1.975 0.8793
    ## 10   I like to express my opinions about books I have read. 41.08 58.92 2.605 0.9010
    ## 11                 I like to exchange books with my friends 55.54 44.46 2.343 0.9609

 

    plot(l28, low.color = "maroon", high.color = "burlywood4")

![plot of chunk barchart](/images/likert/barchart1.png) 

    plot(l28, type = "heat")

![plot of chunk barchart](/images/likert/barchart2.png) 

 

    l28.cnt <- likert(items28, pisaitems$CNT)
    summary(l28.cnt)

    ##    Group                                                     Item   low  high  mean     sd
    ## 1    CAN                                I read only if I have to. 60.83 39.17 2.278 1.0002
    ## 2    CAN                   Reading is one of my favorite hobbies. 61.97 38.03 2.247 0.9946
    ## 3    CAN            I like talking about books with other people. 56.91 43.09 2.275 0.9467
    ## 4    CAN                          I find it hard to finish books. 71.77 28.23 2.054 0.9212
    ## 5    CAN           I feel happy if I receive a book as a present. 50.14 49.86 2.384 0.9731
    ## 6    CAN                      For me, reading is a waste of time. 75.72 24.28 1.945 0.9748
    ## 7    CAN               I enjoy going to a bookstore or a library. 48.27 51.73 2.480 1.0039
    ## 8    CAN              I read only to get information that I need. 61.24 38.76 2.293 0.9193
    ## 9    CAN I cannot sit still and read for more than a few minutes. 76.16 23.84 1.951 0.9534
    ## 10   CAN   I like to express my opinions about books I have read. 46.59 53.41 2.496 0.9273
    ## 11   CAN                 I like to exchange books with my friends 59.60 40.40 2.239 0.9931
    ## 12   MEX                                I read only if I have to. 58.64 41.36 2.274 0.8913
    ## 13   MEX                   Reading is one of my favorite hobbies. 51.69 48.31 2.436 0.8727
    ## 14   MEX            I like talking about books with other people. 53.23 46.77 2.372 0.8832
    ## 15   MEX                          I find it hard to finish books. 61.02 38.98 2.258 0.8784
    ## 16   MEX           I feel happy if I receive a book as a present. 42.96 57.04 2.557 0.9166
    ## 17   MEX                      For me, reading is a waste of time. 88.40 11.60 1.699 0.7522
    ## 18   MEX               I enjoy going to a bookstore or a library. 53.62 46.38 2.386 0.8478
    ## 19   MEX              I read only to get information that I need. 43.58 56.42 2.606 0.8836
    ## 20   MEX I cannot sit still and read for more than a few minutes. 76.99 23.01 1.974 0.8196
    ## 21   MEX   I like to express my opinions about books I have read. 36.67 63.33 2.690 0.8728
    ## 22   MEX                 I like to exchange books with my friends 51.75 48.25 2.430 0.9344
    ## 23   USA                                I read only if I have to. 50.17 49.83 2.485 0.9542
    ## 24   USA                   Reading is one of my favorite hobbies. 69.60 30.40 2.107 0.9298
    ## 25   USA            I like talking about books with other people. 59.45 40.55 2.240 0.9077
    ## 26   USA                          I find it hard to finish books. 68.94 31.06 2.138 0.8876
    ## 27   USA           I feel happy if I receive a book as a present. 62.03 37.97 2.166 0.9255
    ## 28   USA                      For me, reading is a waste of time. 74.00 26.00 2.030 0.9463
    ## 29   USA               I enjoy going to a bookstore or a library. 46.56 53.44 2.514 0.9768
    ## 30   USA              I read only to get information that I need. 52.74 47.26 2.439 0.8968
    ## 31   USA I cannot sit still and read for more than a few minutes. 71.15 28.85 2.087 0.9504
    ## 32   USA   I like to express my opinions about books I have read. 49.11 50.89 2.464 0.9178
    ## 33   USA                 I like to exchange books with my friends 65.54 34.46 2.165 0.9386

    plot(l28.cnt)

![plot of chunk likertGrouped](/images/likert/likertGrouped.png) 

 

    plot(l28.cnt, centered=TRUE, low.color='#FF9900', high.color='#660066')

![plot of chunk centeredPlot](/images/likert/centeredPlot.png) 

