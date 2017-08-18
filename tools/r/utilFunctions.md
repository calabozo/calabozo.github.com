---
layout: default
title: Useful R Functions
excerpt: 
---

```R
    cleanOutlier<-function(df){
        markOutlierInColumn<-function(x){
            lowerq = quantile(x)[2]
            upperq = quantile(x)[4]            
            iqr = upperq - lowerq #Or use IQR(data)
            extreme.threshold.upper = (iqr * 12) + upperq
            extreme.threshold.lower = lowerq - (iqr * 12)
            x[x>extreme.threshold.upper]<-NA
            x[x<extreme.threshold.lower]<-NA
            x
        }
        df<-data.frame(sapply(df,markOutlierInColumn))
        na.omit(df)
    }
```

```R
      localMaxima <- function(x) {
        x<-x/max(x,na.rm=T)
        y <- diff(c(-.Machine$integer.max, x)) > 0L
        y <- cumsum(rle(y)$lengths)
        y <- y[seq.int(1L, length(y), 2L)]
        if (x[[1]] == x[[2]]) {
          y <- y[-1]
        }
        y
      }      
```