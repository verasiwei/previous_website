---
title: apply functions in R
date: 2018-10-27 13:12:34
tags:
categories:
- Programming
---

It takes a lot of time to run for loop, apply function is different and based on C language, which saves a lot of time.

# apply family

**apply**
apply() can be applied to matrix, dataframe, array
apply(X,MARGIN,FUN), X: dataframe,matrix,array; MARGIN: applied to rows when =1, applied to columns when =2; FUN: function that apply to the data

**lapply**
apply a given function to every element of a list and obtain a list as result, it can be applied to dataframes, lists or vectors, the output
return to a list
lapply(X,FUN)
lapply cannot be applied to vector or matrix

**sapply**
similar to lapply, but return to vector, not list
sapply(X,FUN,simplify=TRUE,USE.NAMES=TRUE)
X can be array, matrix, dataframe
simplify: whether to array
USE.NAMES: if X is a string, then TRUE set string as the data name
if simplify=FALSE and USE.NAMES=FALSE, then sapply is equal to lapply
if simplify=array then can create matrixes
if USE.NAMES=TRUE, then can create data name

**vapply**
it is similar to sapply
vapply(X,FUN,FUN.VALUE,USE.NAMES=TRUE)
