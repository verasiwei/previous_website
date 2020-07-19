---
title: Self-Study-Rcpp
date: 2020-03-16 15:38:07
tags:
categories:
- Programming
---


Combine C++ code with R.

```
#include <Rcpp.h>

RcppExport SEXP convolve3cpp(SEXP a, SEXP b){
    Rcpp::NumericVector xa(a);
    Rcpp::NumericVector xb(b);
    int n_xa=xa.size(), n_xb=xb.size();
    int nab=n_xa+n_xb-1;
    Rcpp::NumericVector xab(nab);

    for (int i=0; i < n_xa;i++)
        for (int j=0; j < n_xb; j++)
            xab[i+j] += xa[i] * xb[j];

    return xab;
}


```