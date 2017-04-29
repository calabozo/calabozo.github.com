---
layout: page
title: Text Mining
excerpt: Text Mining
search_omit: false
---

# Installing tm

The package **tm** is the most important text mining package in R. To install the package like other R packages just write: 
'''R
install.packages('tm')
'''
But it is possible that you have a problem installing it, if you get this error:
'''
ERROR: dependency ‘slam’ is not available for package ‘tm’
* removing ‘/usr/local/lib/R/site-library/tm’
'''
Try to install the r-cran-slam package first:
'''
apt-get install r-cran-slam
'''



install.packages("SnowballC")

# Data import

### Read PDF files

To show the PDF files in a dir write:
'''R
files <- list.files(pattern = "pdf$")
'''

To read the PDF into a document object use the *readPDF* function:
'''R
doc <- readPDF(control = list(text = "-layout"))(elem = list(uri = "mytext.pdf"), language = "en", id = "id1")
'''

The output will be a *Document* object, you can access the text by looking at  *doc$content*.

###  Corpus

A Corpus is a larger set of texts. The object *Corpus* in R is able to convert text from files of variables into an object Corpus.





opinions.tdm <- TermDocumentMatrix(opinions, control = list(removePunctuation = TRUE,
                                                         stopwords = TRUE,
                                                         tolower = TRUE,
                                                         stemming = TRUE,
                                                         removeNumbers = TRUE,
                                                         bounds = list(global = c(3, Inf)))) 



 



docs <- Corpus(DirSource(cname))  

docs <- Corpus(VectorSource(df$description))



To see the content of one doc:
'''R
docs[[1]]$content
'''

You can remove puntuation points, convert to lowercase, 
docs <- tm_map(docs, removePunctuation) 
docs <- tm_map(docs, tolower) 
docs <- tm_map(docs, removeWords, c("saludos","estimados"))
docs <- tm_map(docs, removeWords, stopwords("spanish"))
docs <- tm_map(docs, stripWhitespace) 


docs <- tm_map(docs, PlainTextDocument)   


docsTDM <- DocumentTermMatrix(docs)

https://www.r-bloggers.com/text-mining-in-r-automatic-categorization-of-wikipedia-articles/
https://rstudio-pubs-static.s3.amazonaws.com/31867_8236987cf0a8444e962ccd2aec46d9c3.html

http://mukom.mondragon.edu/ict/mineria-de-textos-para-la-clasificacion-de-documentos-en-espanol-con-r/










