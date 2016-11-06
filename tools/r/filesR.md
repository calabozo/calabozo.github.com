---
layout: default
title: Manage files in R
excerpt: file, exists, dir, unlink 
---


## Read tables

* read.table: Reads a file in table format and creates a data frame from it. Tge default separator is one or more spaces.
* read.csv: Reads a CSV file in table format and creates a data frame from it.

For reading big tables it is faster:
```
initial <- read.table("file.txt", nrows=100)
classes <- sapply(initial, class)
data <- read.table("file.txt", colClasses= classes)
```

## File management

* getwd : get working directory.
* setwd : set working directory.
* dir, list.files : list files.
* list.firs: list directories.
* file.exists: check if file exists.
* file.info : get file additional information
* file.rename : rename a file
* file.copy : copy a file
* file.remove: removes a file
* dir.create : creates a directory
* unlink: removes a directory


