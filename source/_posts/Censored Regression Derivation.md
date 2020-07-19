---
title: Censored Regression Derivation
date: 2019-12-15 11:19:20
tags:
categories:
- Statistics & Algorithms
---


It seems no prediction function in R for censored regression. Here is my derivation and the corresponding R function.

Reference: http://people.stern.nyu.edu/wgreene/Lugano2013/Greene-Chapter-19.pdf

![](/images/jak2.jpg)

```
pred.censor <- function(model,data,side){
  #get beta and sigma
  coefficients <- summary(model)$estimate[1:(length(summary(model)$estimate[,1])-1),1]
  names <- names(coefficients)
  indices <- unlist(lapply(names[-1], function(x) which(colnames(data)==x)))
  data_pred <- cbind(rep(1, length(data[,1])), do.call(cbind, lapply(indices, function(x) data[,x])))
  fitted <- (data_pred %*% coefficients)[,1]
  sigma <- exp(summary(model)$estimate[length(summary(model)$estimate[,1]),1])
  
  #predict
  if (side=="right") {
    right <- model$right
    truncated <- fitted - sigma*(dnorm((right - fitted)/sigma)/pnorm((right - fitted)/sigma))
    prob_trunc <- pnorm((right - fitted)/sigma)
    censored <- right*(1-pnorm((right - fitted)/sigma)) + prob_trunc*truncated
  } else if (side=="left") {
    left <- model$left
    truncated <- fitted + sigma*(dnorm((left - fitted)/sigma)/(1-pnorm((left - fitted)/sigma)))
    prob_trunc <- 1-pnorm((left - fitted)/sigma)
    censored <- left*pnorm((left-fitted)/sigma) + prob_trunc*truncated 
  } else {
    left <- model$left
    right <- model$right
    truncated <- fitted + sigma*((dnorm((left - fitted)/sigma)-(dnorm((right - fitted)/sigma)))/(pnorm((right - fitted)/sigma)-pnorm((left - fitted)/sigma)))
    prob_trunc <- pnorm((right - fitted)/sigma) - pnorm((left - fitted)/sigma)
    censored <- left*pnorm((left-fitted)/sigma) + prob_trunc*truncated + right*(1-pnorm((right - fitted)/sigma))
  }
  return(censored)
}

```