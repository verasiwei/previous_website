---
title: ggplot cheat sheet
date: 2018-10-27 13:13:58
tags:
categories:
- Programming
---


ggplot(data=NULL,mapping=aes(x=,y=,color=,),environment=parent.frame())

**one variable**
ggplot(data=NULL,mapping=aes(x=,y=,alpha=,color=,fill=,linetype=,size=,))+
geom_area(aes(y=..density..),stat="bin") #with shadow

ggplot(data=NULL,mapping=aes(x=,y=,alpha=,color=,fill=,linetype=,size=,weight=))+
geom_density(aes(y=..county..,),kernel="gaussian") #without shadow

ggplot(data=NULL,mapping=aes(x=,y=,alpha=,color=,fill=,))+
+geom_dotplot()#plot dots

ggplot(data=NULL,mapping=aes(x=,y=,alpha=,color=,fill=,linetype=,size=,weight-))+
+geom_histogram(aes(y=..density..),binwidth=5)

**two variables**
*continuous X, continuous Y*
ggplot(data=NULL,mapping=aes(x=,y=,alpha=,color=,fill=,shape=,size=,))+
geom_jitter() #jittering is adding a small amount of random noise to data, it is often used to spread out points that would otherwise
be overplotted

ggplot(data=NULL,mapping=aes(x=,y=,alpha=,color=,fill=,shape=,size=,))+
geom_point()

ggplot(data=NULL,mapping=aes(x=,y=,alpha=,color=,linetype=,size=,weight=))+
geom_quantile()

ggplot(data=NULL,mapping=aes(x=,y=,alpha=,color=,lintype=,size=,))+
geom_rug(sides="bl")

ggplot(data=NULL,mapping=aes(x=,y=,alpha=,color=,fill=,linetype=,size=,weight=))+
geom_smooth(model=lm)

ggplot(data=NULL,mapping=aes(x=,y=,label=,alpha=,angle=,color=,family=,fontface=,hjust=,lineheight=,size=,vjust=))+
geom_text(aes(label=cty))

*continuous bivariate distribution*
ggplot(data=NULL,mapping=aes(xmax=,xmin=,ymax=,ymin=,alpha=,color=,fill=,linetype=,size=,weight=))+
geom_bin2d()

ggplot(data=NULL,mapping=aes(x=,y=,alpha=,color=,linetype=,size=,))+
geom_density2d()

ggplot(data=NULL,mapping=aes(x=,y=,alpha=,color=,fill=,size=,))+
geom_hex()

*discrete X, continuous Y*
ggplot(data=NULL,mapping=aes(x=,y=,alpha=,color=,fill=,lintype=,size=,weight=))+
geom_bar(stat="identity")

ggplot(data=NULL,mapping=aes(x=,y=,alpha=,color=,fill=,shape=,size=,))+
geom_boxplot(lower=,middle=,upper=,x=,ymax=,ymin=,alpha=,color=,fill=,linetype=,shape=,size=,weight=)

ggplot(data=NULL,mapping=aes(x=,y=,alpha=,color=,fill=))+
geom_dotplot(binaxis="y",stackdir="center")

**visualizing error**
ggplot(data=NULL,aes(x=,y=,ymin=,ymax=,alpha=,color=,fill=,linetype=,size=))+
geom_crossbar(fatten=2)

ggplot(data=NULL,aes(x=,ymin=,ymax=,alpha=,color=,linetype=,size=,width=))+
geom_errorbar()

ggplot(data=NULL,aes(x=,ymin=,ymax=,alpha=,color=,linetype=,size=))+
geom_linerange()

ggplot(data=NULL,aes(x=,y=,ymin=,ymax=,alpha=,color=,fill=,linetype=,shape=,size=))+
geom_pointrange()

**Stats**
some plots visualize a transformation of the original data set, each stats creates additional variables to map aesthetics to
stat_smooth(method=,formula=)
method: datasets with n<1000, use loess, n>1000, use gam
how to define smooths in gam formulae???
s(x,k=1,fx=FALSE,bs="tp")
x represents a list of variables that are the covariates that this smooth is a function of
k is the dimension of the basis used to represent the smooth term
fx is whether the term is a fixed d.f. regression spline
bs is a two letter charater string indicating the penalized smoothing basis(smooth terms in GAM)
thin plate regression spline: tp;
duchon splines: ds
cubic regression splines: cr,cs,cc
splines on the sphere: sos
P-splines: ps
Random effects: re
Markov Random Fields: mrf
Gaussian process smooths: gp
soap film smooths: so, sf, sw

thin plate regression splines gives the best MSE performance but slower, the knot based penalized cubic regression splines is the second
best
https://www.rdocumentation.org/packages/mgcv/versions/1.8-24/topics/smooth.terms

**Scales**
scales control how a plot maps data values to the visual values of an aesthetic
scale_fill_manual(
values=c(),
limits=c(),
breaks=c(),
name=" ",
labels=c())specify the own set of mappings from levels in the data to aesthetic values;
scale_x_continuous():
set the range and the breaks for xaxis, map x values to visual values
scale_color_manual():
mapped colors for values

**themes**
theme_bw():
a theme with white background and black gridlines
theme():
theme(axis.text.x=element_text(size= , angle= , hjust= ):
when there are many x axis coordinates, a big issue is that they will be overlapped, so change the angle of x axis coordinates and horizontal justification

**Faceting**
divide a plot into subplots based on the values of one or more discrete variables
facet_wrap(~fl): wraps a 1d sequence of panels into 2d(wrap facets into a rectangular layout)

**Legends**
place legend at bottom,top,left, or right
ggplot()+geom_point()+theme(legend.position)
ggplot()+geom_point()+guides(color="none")
ggplot()+geom_point()+scale_fill_discrete(name=Title,labels=c("A","B","C"))

**some other functions not related to ggplot**
gsub(): replaces all matches of a string
with(): for example, with(mtcars,summary(mpg)) > to calculate the summary statistics of mpg in mtcars data, return the summary statistic
cut():cut into different intervals
melt():transform the wide format to long format; when does it need to transform to long format? when there are group factors
subset():subset datas of what you are interested in

