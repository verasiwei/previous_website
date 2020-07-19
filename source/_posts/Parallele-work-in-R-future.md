---
title: 'Parallel work in R'
date: 2019-06-18 10:01:33
tags:
mathjax: true
categories:
- Programming
---

# Parallel processing
References: https://github.com/berkeley-scf/parallelR-biostat-2015/blob/master/parallel.pdf
cores: the different processing units on a single node
nodes: each like one computer, each with their own memory
process: tasks on a machine 
threads: multiple paths of execution within a single process
sockets: creating new R processes

With shared memory, multiple processors share the same memory. With the distributed memory, many nodes and each node will run its own memory. Running on the cores of a single node using shared memory will be faster than using the same number of cores across multiple nodes. 

# 1.future
## future
The difficiency of R is that R has less API. But *future* is a good implementation of doing parallel computing in R. 
*Future* package can use a sequential strategy, which means it will run in the current R sessions. Other strategies can be used to run parallely on the current machine or on a cluster. During parallel working, the current R session does not block, and the *future* are being resolved in separate processes running in the background. The source of *future* is here: https://github.com/HenrikBengtsson/future/tree/master. The future package implements these types of work:

**1.sequential**: sequentially and run in the current R session
**2.multisession**: multiple background R sessions on current machine. The package will launch a set of R sessions in the background. If all background sessions are busy serving other futures, the creation of the next multisession future is blocked until a backgoround session becomes available again. The total number of background processes is decided by the number of available cores on your machine. 
```
availableCores()
plan(multisession)
```
**3.multicore**: forking processes. Faster than multisession but unstable. For instance, when running R from within Rstudio process forking may resulting in crashed R sessions. The future package disables multicore by default. This will cause `plan(multicore)` to `plan(sequential)` and `plan(multiprocess)` to `plan(multisession)`.
```
#default FALSE
supportsMulticore()
plan(multicore)
```
**4.multiprocess**: multicore if supported otherwise multisession. Multisession will be used unless multicore evaluation is supported.
```
availableCores()
plan(multiprocess)
```
*Global* objects have to be passed exactly as they were at the time the future was created.

## future.apply
*future.apply* is to parallelize the apply family
I will implement asynchronous futures:
```
library(future)
#firstly check how many cores in your machine
availableCores()

#this will actually run multisession using up to 20 parallel futures, also 20 cores.
plan(multiprocess,workers=20)
resuls <- future.apply::future_lapply(1:10,function(x) createfunction(x))
```

# 2.foreach and doParallel
How about another option of using foreach and doParallel packages? Inside the doParallel package, **Pay attention to** there are two types of parallel working, one is *multicore* functionality which runs tasks on a single computer, not a cluster of computers, and another is *snow* functionality. And **pay attention to** that the packages should be load within the *foreach*. When using *multicore*, 

```
library(doParallel)
library(foreach)

#multicore functionality
registerDoParallel(cores=20)
getDoParWorkers()
getDoParName()

#%do% is to run sequentially and %dopar% is to run parallely
result <- foreach(i=1:10) %dopar% {lapply(list,function)}

#snow functionality
mycluster <- makeCluster(5)
registerDoParallel(mycluster,cores=5)
getDoParWorkers()
result <- foreach(i=1:10) %dopar% {lapply(list,function)}

```