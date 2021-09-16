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
## 1-3 Goals and Topics
## 1-4 Brief history of convex optimization



---
# 2. Convex Sets