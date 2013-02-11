---
layout: mathjax	
title: TriMatch
subtitle: Propensity Score Matching for Non-Binary Treatments
published: true
status: publish
submenu: trimatch
---


#### `as.data.frame.list`: Convert a list of vectors to a data frame. ####

#### Description ####


 This function will convert a list of vectors to a data
 frame. This function will handle three different types of
 lists of vectors. First, if all the elements in the list
 are named vectors, the resulting data frame will have
 have a number of columns equal to the number of unique
 names across all vectors. In cases where some vectors do
 not have names in other vectors, those values will be
 filled with `NA` .


#### Usage ####

     
     as.data.frame.list(x, row.names = NULL, optional = FALSE,
     ...)


#### Arguments ####

a list to convert to a data frame. a vector equal to `length(x)`  corresponding to the row names.  If `NULL` , the row names will be set to `names(x)` . not used. other parameters passed to  [`data.frame`](data.frame.html) .

#### Details ####


 The second case is when all the vectors are of the same
 length. In this case, the resulting data frame is
 equivalent to applying `rbind` across all elements.
 
 The third case handled is when there are varying vector
 lengths and not all the vectors are named. This condition
 should be avoided. However, the function will attempt to
 convert this list to a data frame. The resulting data
 frame will have a number of columns equal to the length
 of the longest vector. For vectors with length less than
 this will fill the row with `NA` s. Note that this
 function will print a warning if this condition occurs.


#### Value ####


 a data frame.


#### Author ####


 Jason Bryer
  [jason@bryer.org](mailto:jason@bryer.org) 


#### References ####


  [http://stackoverflow.com/questions/4227223/r-list-to-data-frame](http://stackoverflow.com/questions/4227223/r-list-to-data-frame) 


#### Examples ####


    
    test1 <- list(c(a = "a", b = "b", c = "c"), c(a = "d", b = "e", c = "f"))
    as.data.frame(test1)

    ##   a b c
    ## 1 a b c
    ## 2 d e f

    
    test2 <- list(c("a", "b", "c"), c(a = "d", b = "e", c = "f"))
    as.data.frame(test2)

    ##   a b c
    ## 1 a b c
    ## 2 d e f

    
    test3 <- list(Row1 = c(a = "a", b = "b", c = "c"), Row2 = c(var1 = "d", var2 = "e", 
        var3 = "f"))
    as.data.frame(test3)

    ##         a    b    c var1 var2 var3
    ## Row1    a    b    c <NA> <NA> <NA>
    ## Row2 <NA> <NA> <NA>    d    e    f

    
    test4 <- list(Row1 = letters[1:5], Row2 = letters[1:7], Row3 = letters[8:14])
    as.data.frame(test4)

    ##      Col1 Col2 Col3 Col4 Col5 Col6 Col7
    ## Row1    a    b    c    d    e <NA> <NA>
    ## Row2    a    b    c    d    e    f    g
    ## Row3    h    i    j    k    l    m    n

    
    test5 <- list(letters[1:10], letters[11:20])
    as.data.frame(test5)

    ##   X1 X2 X3 X4 X5 X6 X7 X8 X9 X10
    ## 1  a  b  c  d  e  f  g  h  i   j
    ## 2  k  l  m  n  o  p  q  r  s   t

    
    test6 <- list(list(letters), letters)
    as.data.frame(test6)
