---
title: Boolean Logic Regression Note
date: 2019-07-10 12:20:45
tags:
mathjax: true
categories:
- Statistics & Algorithms
---

In the regression models, we always only include the main effects and some simple interaction terms, which ignore many interactions especially when all the predictors are binary. This is the most important problem in the analysis of SNP microaray data. My master thesis is a kind of study trying to optimize this problem and validate a better approach through Iterative Sure Independence Screening developed by a mathematician, Jianqing Fan. 
Logic regression is a regression methodology that try to find combinations through Boolean logic of binary variables that highly predict the response. Boolean algebra is a kind of algebra that the variables values are either true or false, the operations of Boolean algebra are $\bigwedge (AND)$, $\bigvee (OR)$, $^{c} (NOT)$. The operations are the following:
$$x \bigwedge y, if x=y=1, then x \bigwedge y=1, otherwise x \bigwedge y=0 $$
$$x \bigvee y, if x=y=0, then  x \bigvee y=0, otherwise x \bigvee y=1 $$
$$x^{c}, if $x=1$, x^{c}=0$$
The logic tree: The location of each element is a knot. The knots that do not have a parent is the root and the knots that do not have children are leaves.
​

## LOGIC MODEL
Paper reading: http://kooperberg.fhcrc.org/logic/documents/logic-regression.pdf

Logic regression models: $g(E[Y])=\beta_{0} + \sum_{j=1}^{t} \beta_{j} L_{j}$, where $L_{j}$ is a Boolean expression of $X_{i}$ such like $L_{j}=X_{2} \bigwedge X_{3}$. 

Search for best models: 
1. moving
Alternating a leaf: replace a leaf with another leaf at this position, the leaf cannot be replaced with its sibling or the complement of the sibling.
Changing operators: replace $\bigwedge$ can be replaced with $bigvee$, also true for the reverse.
Growing and pruning: the new branch can be grou and the counter move to grouing is pruning.
Splitting and deleting: any leaf can be split by creating a sibling and any leaf can be deleted.
2. Greedy Search
As mentioned before, the regression model is built, a score function can be defined. For example, if linear regression, use RSS and if logistic model, use binomial deviance. The greedy algorithm is used to search best logic model, of which firstly is to find the single predictor that minimizes the score function. Then its neighbors(single move) are investigated, if a new state has a better score than the original then it will be chosen
3. simulated annealing
The state space S is a collection of individual states and the states $S_{1}$,...,$S_{n}$ are related by a neighborhood system. The set of neighbor pairs in S defines a substructure M in S*S. The elements in M are moves, $(S,S') \in M^{k}$ means two states S,S' are connected via a set of k moves. All tress are fit simultaneously. We need preselect the maximum number t of trees, we select one tree in the logic model and then randomly pick a move from the move set, then refit the parameters for the new model and compare the score of the previous state and the new state, if the score is better, accept the move, if not, accept the move with a probability.
4. model selection
Two types of randomization tests. The first is an overall test for signal in the data. Assuming we first find the best scoring model, and we want to test whether there is association between X and Y. If there is no assocition, then Y are randomly permuted that it should have the same score as the best model. Repeat this procedure and the proportion of scores better than the best model as p value. The second is to determine the optimal model size. We want to test whether the better score obtained by models of larger sizes is due to noise. Assuming the optimal model has score $\epsilon_{j}$ and size j and sssuming the best scoring model has score $\epsilon'$ and size k. For a model with p trees, there can be up to $2^{p}$ fitted classes. Randomly permute the response, If the null hypothesis is true, the model size j is optimal, but other models of size j may have better score $\epsilon''$ than $\epsilon_{j}$, then $\epsilon'$ would be a sample from the same distribution as $\epsilon''$. Otherwise, optimal model had a size larger than j, then the randomization would have average worse scores than $\epsilon'$





