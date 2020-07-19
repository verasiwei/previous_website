---
title: Mixed Effect Model
date: 2019-05-01 11:56:18
tags:
mathjax: true
categories:
- Statistics & Algorithms
---

Assuming there is a continuous outcome measure y, and the interest is to examine the relationship between y and a continuous variable called $x_{1}$ that the model is something like this $y~\beta_{0}+\beta_{1}x_{1}+\epsilon$. And assuming we also have age and sex fixed effects in the model, so the model is like this $y=\beta_{0}+\beta_{1}x_{1}+\beta_{2}age+\beta_{3}sex+\epsilon$. But the situation is that for each subject, we took multiple measures at different timepoints that each subject have multiple responses at different timepoints, so multiple responses from the same subject are not independent. We need to add random effect in the regression model to deal with this situation. Each subject might have different “baseline” y value that subject 1 might have a mean y value1 and subject 2 might have a mean y value2 across three timepoints. Therefore we need to include subjects as one random effect. $y=\beta_{0}+\beta_{1}x_{1}+\beta_{2}age+\beta_{3}sex+\gamma_{1}subject+\epsilon$