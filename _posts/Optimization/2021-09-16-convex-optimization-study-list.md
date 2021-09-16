---
title:  "Convex Optimization Study List"
excerpt: "'모두를 위한 컨벡스 최적화' 공부 리스트"

categories:
  - Optimization
tags:
  - [Optimization, Convex]

toc: true
toc_sticky: true
use_math: true
 
date: 2021-09-16
last_modified_at: 2021-09-16
---
# 0. Reference
[모두를 위한 컨벡스 최적화](https://convex-optimization-for-all.github.io/)

# 1. Introduction

## 1-1 Optimization problems?
* 여러개의 선택가능한 후보 중에서 최적의 해(Optimal value) 또는 최적의 해에 근접한 값을 찾는 문제
* Mathematical optimization problems 정의       
    >$$
    \begin{align*} 
        &\min_{x\in D}\ && f(x) \\
        &\text{subject to} && g_i(x) \le 0,\ i = 1, ...m \\
        &&& h_j(x) = 0,\ j = 1,\ ...r 
    \end{align*}
    $$
    * $x \in R^n$ is the optimization variable      
    * $f: R^n \rightarrow R$ is the objective or cost function      
    * $g_i: R^n \rightarrow R, i = 1, ..., m$ are the inequality constraint functions        
    * $h_i: R^n \rightarrow R, j = 1, ..., r$ are the equality constraint functions
* 제약조건의 종류
    1. Explicit 
    2. Implicit
* Applications
    * Portpolio optimization
    * Device sizing in electonic circuits
    * Data fitting

## 1-2 Convex optimization problem
### Convex optimization problem
* Convex optimization problem = optimization problem의 한 종류
    * objective function $f$, inequality constraint function $g_i$ -> convex
    * equality constraint function $h_i$ -> affine function
### Convex sets
* Definition
### Convex functions
* Definition
### Relation between a convex set and a convex function
* Epigraph
### Nice property of convex optimization problems
* Convex 함수의 local minimum은 항상 global minimum
    * 증명
### Convex combination

## 1-3 Goals and Topics
### Goals
### Topics
### Algorithms
## 1-4 Brief history of convex optimization
### Theory
### Algorithms
### Applications

---
# 2. Convex Sets
## Motivation
Convex set -> convex optimization의 근간을 이루는 개념      
* Convex optimization       
    > 문제를 convex function으로 정의해서 최대 또는 최소를 구하는 기법
* Convex set은 convex function과 밀접한 관련이 있음
    * Convex function은 convex set으로 정의됨 
        * 함수의 정의역과 치역이 convex set으로 정의
        * Convex function의 주요 성질들이 convex set에 의해 결정
    * 풀고자 하는 문제가 convex function으로 정의된 것인지 판단할 때        
        => 함수의 epigraph가 convex set인지 확인하면 됨

## Contents
* convex set의 정의와 예제, 주요 속성, convexity를 유지하는 연산

## 2-1 Affine and Convex Sets