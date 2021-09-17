---
title:  "Convex Optimization Study List"
excerpt: "'모두를 위한 컨벡스 최적화' 공부 리스트"

categories:
  - Optimization
tags:
  - [Optimization, Convex]

toc: false
toc_sticky: true
use_math: true
 
date: 2021-09-16
last_modified_at: 2021-09-16
---
# 0. Reference
[모두를 위한 컨벡스 최적화](https://convex-optimization-for-all.github.io/)     
[CMU 강의](http://www.stat.cmu.edu/~ryantibs/convexopt-F16/#miscellaneous)      
[Stanford 강의](https://web.stanford.edu/~boyd/cvxbook/)        
[UBC 강의 (CPSC406)](https://friedlander.io/19T2-406/notes/Constrained_optimization/)


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

---

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

---

## 1-3 Goals and Topics
### Goals
### Topics
### Algorithms

---

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

---

## Contents
* convex set의 정의와 예제, 주요 속성, convexity를 유지하는 연산

---

## 2-1 Affine and Convex Sets
### 2-1-1 Line, line segment, ray

### 2-1-2 Afiine set
* Affine set
* Affine combination
* Affine hull
* Affine set과 subspace의 관계

### 2-1-3 Convex set
* Convex set
* Convex combination
* Convex hull

### 2-1-4 Cone
* Cone
    > A set C is called a **cone** if $x \in C \Rightarrow \theta x \in C, \;\; \forall \theta \geq 0.$
    
    위 정의로부터 2차원 평면에서의 cone의 예시는 다음과 같다고 생각한다.     

    ![Cone example](https://user-images.githubusercontent.com/17296297/133630072-251f3976-b34a-43e5-9474-acc06be119c9.PNG){: width="640" height="320"}      
    *2-D cone example*   

    그림의 6가지 예시 모두 집합 C에서 한 점 x (= 벡터)를 선텍하고 $\theta$ 배를 한 점 $\theta x$ 도 집합 C에 포함되므로 위 정의를 만족한다.

* Convex cone
    > A set C is a **convex cone** if it is convex and a cone, i.e.,
    > $$
    \begin{align} 
        &x_1, x_2 \in C \Rightarrow \theta_1 x_1 + \theta_2 x_2 \in C && \forall \theta_1, \theta_2 \geq 0
    \end{align}
    $$

    * Convex cone은 convex 조건을 만족하는 cone을 말한다. 
        * 즉, 집합 C에서 두 점 $x_1, x_2$를 고르고 선형 결합(linear combination)한 결과로 나온 점 또한 집합 C에 포함될 경우 convex cone이 된다. 
        * 이때 계수 $\theta_1, \theta_2$는 조건 ($\forall \theta_1, \theta_2 \geq 0$)을 만족해야 한다. 
    * 위 그림에서는 1, 2, 3, 6번에 convex cone이다. 

        ![convex cone](https://user-images.githubusercontent.com/17296297/133630077-853a6159-75de-41eb-b61a-8eb9628e893b.PNG){: width="640" height="320"}       
        *2-D convex cone example*  

        * 왼쪽 그림 -> 두 점을 집합 내부에서 선택한 경우
        * 오른쪽 그림 -> 두 점을 집합 경계에서 선택한 경우
            * $\theta_1, \theta_2$ 모두 0인 경우 -> 원점
            * $\theta_1, \theta_2$ 중 하나만 0인 경우 -> 경계
            * $\theta_1, \theta_2$ 둘 다 0이 아닌 경우 -> 내부

        ![non-convex cone](https://user-images.githubusercontent.com/17296297/133630067-e13106b8-2af4-4637-933d-7d29e7e55dea.PNG){: width="640" height="320"}       
        *2-D non-convex cone example*  
        * 두 점의 선형 결합이 집합에 포함되지 않는 경우가 존재

* Conic combination     
    > The point $\sum_{i=1}^{k}\theta_i x_i$ is called a conic combination of $x_1,\cdots, x_k.$
    * 선형 결합 (linear combination) 할 때 모든 계수가 0 이상인 경우        
    = **Conic combination** or **nonnegative linear combination**
* Conic hull

---

## 2-2 Some important examples
Convex set의 주요 예제들        
* Trivial ones: empty set, point, line, line segment, ray
* Affine space
* Convex set
    * Hyperplanes, halfspaces, Euclidean balls, ellipsoids, norm balls, polyhedron, simplexes
* Convex cone
    * Norm cone, normal cone, positive semidefinite cone

### 2-2-1 Convex set examples

### 2-2-2 Convex cone examples
* Norm cone
    > $$C = \{(x, t) : \|x\| \le t\} \subseteq R^{n+1}, \; \text{for a norm} \; \|·\|, \; t \in \mathbb R $$
    * 
* Normal cone
    * Noraml cone은 tangent cone으로부터 정의될 수도 있음
    * Normal cone, tangent cone 예시
* Positive semidefinite cone

---

## 2-3 Operations that preserve convexity
* Intersection
* Affine functions
* Perspective function
* Linear-fractional functions

---

## 2-4 Generalized inequalities

---

## 2-5 Separating and supporting hyperplanes

---

## 2-6 Dual cones and generalized inequalities
### 2-6-1 Dual cones

### 2-6-2 Dual generalized inequalities