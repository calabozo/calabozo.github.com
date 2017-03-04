---
layout: default
title: Optimization tricks
---

# Microbenchmark

The [microbenchmark](https://cran.r-project.org/web/packages/microbenchmark/) package allows to test the execution time of simple commands in R. It is also posible to graphically compare the results using plot or autoplot (with ggplot2). The firstone uses a box diagram and the second one generates a violin diagram.
For example:
```R
> library(microbenchmark)
> df<-data.frame(a=rnorm(1000),b=rexp(1000))
> mk<-microbenchmark(df[df$a>0.5,],subset(df,a>0.5),times=100)
> mk
Unit: microseconds
                expr     min      lq      mean   median      uq     max neval
    df[df$a > 0.5, ]  66.737  73.092  82.24333  76.8135  82.116 417.909   100
 subset(df, a > 0.5) 118.021 124.352 138.77408 131.1465 144.617 380.429   100
> plot(mk)
> library(ggplot)
> autoplot(mk)
```




# Faster searchs in data frame:

http://stackoverflow.com/questions/27303534/faster-way-to-subset-on-rows-of-a-data-frame-in-r

Expected time comparison (from fast to slow)
```R
> df<-data.frame(val1=rnorm(1000*length(letters)),l=letters)
> dft<-data.table(df)
> microbenchmark(dft[dft$val1>0.5,],df[which(df$val1>0.5),],df[df$val1>0.5,],subset(df,val1>0.5),times=1000)
Unit: microseconds
                       expr      min        lq      mean    median       uq       max  neval
      dft[dft$val1 > 0.5, ]  553.412  689.8770  951.0451  744.9750  824.997  48782.01  1000
 df[which(df$val1 > 0.5), ]  820.468  871.7765 1140.8142  897.9575 1003.746  45858.08  1000
        df[df$val1 > 0.5, ] 1063.283 1117.5755 1459.8197 1161.0700 1427.850  46410.95  1000
     subset(df, val1 > 0.5) 1802.162 1872.8540 2429.1417 1938.1930 2460.903  48076.26  1000
> setkey(dft,l)
> microbenchmark(dft["b",],df[which(df$l=="b"),],df[df$l=="b",],subset(df,l=="b"),times=1000)
Unit: microseconds
                     expr      min       lq     mean   median       uq      max  neval
               dft["b", ]  779.820 1052.383 1208.314 1158.503 1277.401  4550.64  1000
 df[which(df$l == "b"), ] 1695.645 1773.638 2256.367 1825.460 2108.171 48292.90  1000
        df[df$l == "b", ] 1864.640 1953.710 2457.619 1999.205 2719.698 54131.61  1000
     subset(df, l == "b") 2522.657 2644.773 3433.870 2755.842 3862.699 50394.92  1000
```


| command | relative time |
|---------|---------------|
| as.data.table | 1 |
| df[which(expr)] | 1.2-1.8 |
| df[expr,] | 1.5-2 |
| subset(df, expr) | 2.5-2.8 |

http://www.milanor.net/blog/access-data-quickly-and-easily-data-table-package/

# HashMap for search

http://stackoverflow.com/documentation/r/5179/hashmaps#t=201702131022504441359

It is possible to emulate a hashtable usign a new environment, access to the variables of the environment is done via a hash table.
The methods to insert or retreive elements are [[ or $. For example:

```R
> H <- new.env(hash=TRUE)
> H[["key"]] <- c("a","b")
> key2 <- "myKey"
> H[[key2]] <- data.frame(x = 1:5, y = rexp(5))
> H$key
[1] "a" "b"
> H[["myKey"]]
  x         y
1 1 0.6271936
2 2 2.5107182
3 3 1.0051368
4 4 0.6161979
5 5 0.9121217
```

You can inspect the elements in the environment has table with the same commands you use to see the variables in the default environment.
```R
> names(H)
[1] "myKey" "key"  
> ls.str(H)
key :  chr [1:2] "a" "b"
myKey : 'data.frame':	5 obs. of  2 variables:
 $ x: int  1 2 3 4 5
 $ y: num  0.627 2.511 1.005 0.616 0.912
```

To remove an elment you have to use _rm_ with the _envir_ option:
```R
> rm("myKey", envir=H)
```


# Create a data frame dinamically

It is possible to create a data frame and grow it dinamically:
```R
> df<-data.frame(height=numeric(0),name=character(0))
> df<-rbind(df,data.frame(height=11,name="def",stringsAsFactors=FALSE))
> df<-rbind(df,data.frame(height=7,name="abc",stringsAsFactors=FALSE))
> df
  height name
1     11  def
2      7  abc

```

But this is slow, a faster option instead of the use of *rbind* is the use of *bind_rows* from the *dplyr* package.
```R
> library(dplyr)
> df<-data.frame(height=numeric(0),name=character(0))
> df<-bind_rows(df,data.frame(height=11,name="def",stringsAsFactors=FALSE))
> df<-bind_rows(df,data.frame(height=7,name="abc",stringsAsFactors=FALSE))
> df
  height name
1     11  def
2      7  abc
```

If we plan to store a big amount of rows in the data the fastest option available is to preallocate a list with the same numer of elements as rows your data frame will have and combine them with rbind. For example:
```R
> datalist <- vector(mode = "list", length = 2)
> datalist[[1]] <- c(11,"def")
> datalist[[2]] <- c(7,"abc")
> df<-do.call(rbind,datalist)
> df<-as.data.frame(df,stringsAsFactors=FALSE)
> colnames(df)<-c("height","name")
> df
  height name
1    11  def
2     7  abc
```

