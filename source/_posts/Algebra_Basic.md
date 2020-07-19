---
title: Linear Algebra Basic
date: 2019-06-13 10:40:42
tags:
mathjax: true
categories:
- Statistics & Algorithms
---


This is the review and cheatsheet of some basic linear algebra knowledge during my undergraduate study. 

## 1. Basic
**Determinant**
A scalar value that can be computed from elements of a square matrix. 
$$det(A)=|A|=\sum_{j=1}^{n}(-1)^{i+j}a_{i,j}M_{i,j}$$
where $a_{ij}$ is the element of A and $M_{ij}$ is minor.

**Matrix Inverse**
A square matrix M has an inverse that M is invertible if the determinant $|M| \neq 0$. 
$$A^{-1}A=I$$
The inverse properties are
1.$$(A^{-1})^{-1}=A$$
2.$$(AB)^{-1}=B^{-1}A^{-1}$$
3.$$(A^{-1})^{T}=(A^{T})^{-1}$$

**Orthogonal Matrices**
A square matrix $A \in R^{nn}$ is orthogonal:
$$A^{T}A=I=AA^{T}$$

**Matrix multiplication**
\\(A \in R^{mn}\\) and \\(B \in R^{np}\\) 
$$C=AB=\sum_{k=1}^{n}A_{ik}B_{kj}$$

**Matrix transpose**
$$(A^{T})^T=A$$
$$(AB)^{T}=B^{T}A^{T}$$
$$(A+B)^{T}=A^{T}+B^{T}$$

**Trace**
The trace of a square matrix $A \in R^{nn}$ is denoted as tr(A), which is the sum of diagonal elements in the matrix:
$tr(A)=\sum_{i=1}^{n}A_{ii}$. 

Properties are below: 
1.$$trA=trA^{T}$$
2.$$tr(A+B)=trA+trB$$
3.$$tr(tA)=t tr(A)$$
4.$$tr(AB)=tr(BA)$$
5.$$tr(ABC)=tr(BCA)=tr(CAB)$$

**Rank**
The column rank of a matrix $A \in R^{mn}$ is the size of the largest subset of columns of A that constitute a linearly independent set. The same definition as for row rank. For any matrix, the column rank is equal to the row rank. Both are denoted as rank(A). Properties are below:
1.$$rank(A) \leq min(m,n)$$
2.$$rank(A)=rank(A^{n}$$
3.$$rank(AB) \leq min(rank(A), rank(B))$$
4.$$rank(A+B) \leq rank(A)+rank(B)$$

**Eigenvalue and Eigenvectors**
Given a square matrix A, \\(\lambda\\) is an eigenvalue and x is the corresponding eigenvector if
$$Ax=\lambda x, x \neq 0$$
which equals to $$(\lambda I-A)x=0$$
The properties are
1.$$tr(A)=\sum_{i=1}^{n} \lambda_{i}$$
2.$$|A|=\prod{i=1}^n \lambda_{i}$$
3.The rank of A is equal to the number of non-zero eigenvalues of A
4.If A is non-singular then $1/\lambda_{i}$ is an eigenvalue of $A^{-1}$ with associated eigenvector. 
5.The eigenvalues of a diagonal matrix are just the diagonal entries.

**The gradient**
$f: R^{mn} \to R$ is a function that input a matrix A and returns a value. The gradient of f is the matrix of partial derivatives. 

$$\left[
\begin{matrix}
\alpha f(A)/(\alpha A_{11}) & \alpha f(A)/(\alpha A_{12}) & ... \alpha f(A)/(\alpha A_{1n}) \\\\
\alpha f(A)/(\alpha A_{21}) & \alpha f(A)/(\alpha A_{22}) & ... \alpha f(A)/(\alpha A_{2n}) \\\\
...... \\\\
\alpha f(A)/(\alpha A_{m1}) & \alpha f(A)/(\alpha A_{m2}) & ... \alpha f(A)/(\alpha A_{mn}) \\\\
\end{matrix}
\right]
$$


## 2.Other definitions and calculations
**Laplace Matrix (simple graph)**
Given a simple graph G with n vertices, its Laplace Matrix \\(L_{nn}\\) is defined as: L=D-A, where D is the degree matrix and A is the adjacency matrix. D is a diagonal matrix which includes the information about the degree of each vertex. A is the adjacency matrix which only includes 1 and 0 since G is a simple graph and the diagonal are all 0. 
**symmetrix normalized laplacian**
$$L^{sym}=D^{-1/2}LD^{-1/2}=I-D^{-1/2}AD^{-1/2}$$

**Singular value decomposition**
Assume $A \in R^{mn}$ and all elements in M belongs to real or plural values. There exists a decomposition that 
$$A=U \sum V^{T}$$
where U and V are orthogonal that $U^{T}U=I_{mm}$ and $V^{T}V=I_{nn}$.
$$A^{T}A=V (\sum)^{T} \sum V^{T}$$
$$AA^{T}=U \sum (\sum)^{T} U^{T}$$

**Eigen decomposition**
$A \in R^{nn}$ is a square matrix with n linear independent eigenvectors $q_{i}$(i=1...n). A can be factorized as 
$$A=Q\Lambda Q^{-1}$$
where Q is the square n by n matrix with ith column is the eigenvector of A, and $\Lambda$ is the diagonal matrix with diagonal elements are the corresponding eigenvalues. 
  
