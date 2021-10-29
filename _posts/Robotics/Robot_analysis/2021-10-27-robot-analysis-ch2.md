---
title:  "[Robot Analysis] 2. Position Analysis of Serial Manipulators"
excerpt: ""

categories:
  - In-progress
tags:
  - []

toc: false
toc_sticky: true
use_math: true
 
date: 2021-10-27
last_modified_at: 2021-10-29
sitemap :
  changefreq : daily
  priority : 1.0
---

# 2.1 Introduction
## Serial manipulator, base, end-effector
Serial manipulator는 다양한 종류의 조인트들(일반적으로 revolute 또는 prismatic)을 이용해 순차적으로 연결된 여러 개의 링크들로 구성되어 있다. Manipulator의 한쪽 끝은 지면에 붙어있고, 다른 쪽 끝은 공간에서 자유롭게 움직일 수 있다. 이러한 이유로 serial mainpulator는 *open-loop mainpulator*라고 불리기도 한다. 지면에 고정된 링크는 *base*, gripper나 mechanical hand가 부착되는 반대쪽 free end는 *end-effector*라 부른다.

## Position Analysis Problem
로봇이 특정한 task를 수행하기 위해서는, 먼저 base에 대한 end effector의 상대적인 위치가 규정되어야 한다. 이를 *position analysis problem*이라고 부른다. Position analysis problem에는 두 가지 종류가 있다:
* *Direct position* or *direct kinematics* problems
  > 조인트 값이 주어졌을 때 end effector의 위치를 찾아야 한다.
* *Inverse position* or *inverse kinematics* problems
  > End effector의 위치가 주어졌을 때 해당 위치로 end effector를 보낼 수 있는 조인트 값을 찾아야 한다.

### Serial vs parallel
Serial manipulator에서는 direct kinematics가 복잡하지 않은 반면, inverse kinematics가 매우 어렵다. 반대로, parallel manipulator에서는 inverse kinematics가 매우 간단한 반면, direct kinematics를 풀기가 매우 어렵다. 

### Deficient vs redundant 
Deficient manipulator는 end effector가 공간 상에 자유롭게 위치할 수 없고 (특정 자유도가 제한됨), redundant manipulator는 redundancy 자유도에 따라 주어진 end-effector 위치에 대응하는 inverse kinematic solution이 무한대로 존재할 수 있다. 

Inverse kinematics 문제를 풀때, 우리는 closed-form solution을 얻는 것, 즉, 이 문제를 end-effector 위치와 조인트 변수를 연관짓는 algebraic equation으로 줄이는 것에 관심이 있다. 이러한 방식으로 모든 가능한 solution들과 manipulator 자세들을 설명할 수 있다. Closed-form solution을 얻기 위해 다양한 formulation 방법들이 제안되어 왔다. 
* Vector algebra method (Chase, 1963; Lee and Liang, 1988a,b)
* Geometric method (Duffy and Rooney, 1975; Duffy, 1980)
* $4\times 4$ matrix method (Denavit and Hartenberg, 1955)
* $3\times 3$ dual matrix method (Yang, 1969; Pennonck and Yang, 1985)
* Iterative method (Uicker et al., 1964, Albala and Angeles, 1979)
* Screw algebra method (Yuan and Freudenstein, 1971; Kohli and Soni, 1975)
* Quarternian algebra method (Yang and Freudenstein, 1964)

가능한 inverse kinematics 해의 개수는 manipulator 타입과 위치에 따라 정해진다. 일반적으로, closed-form solution들은 다음과 같이 간단한 형태를 가지는 manipulator에 대해 찾을 수 있다. 
* with three consecutive joint axes intersecting at a common point
* with three consecutive joint axes parallel to on another

위와 같은 형태가 아닌 일반적인 manipulator들은 inverse kinematics 문제가 매우 어려워 진다. 

A brief review of the historical development of the inverse kinematics problem
* Pieper and Roth (1969)
* Roth, Rastegar, and Scheinman (1972)
* Freudenstein (1973)
* Duffy and Crane (1980)
* Albala (1982)
* Tsai and Morgan (1985)
* Primrose (1986)
* Lee and Liang (1988a,b)
* Raghavan and Roth (1989, 1990)

이번 챕터에서는 serial manipulator의 position 문제에 초점을 맞춘다. 일반적으로 사용되는 두 가지 방법, Denavit and Hartenberg's method와 successive screw displacements 방법이 소개된다. 두 방법들은 본질적으로 체계적이고 serial manipulator의 kinematic analysis에 더 적합하다. 방법들의 활용을 보이기 위해 몇몇 자주 사용되는 로봇 구조의 kinematics을 분석한다. Kinematics에서 가장 어려운 문제인 일반적인 형태의 6R manipulator의 inverse kinematics가 Appendix C에서 제공된다. 기하학적 방법(geometric method)은 일부 연구자나 엔지니어에 의해 종종 사용되지만, 비교적 간단한 구조의 serial manipulator와 parallel manipulator의 분석에 더 적합하다고 판단된다. 

# 2.2 Link Parameters and Link Coordinate Systems
# 2.3 Denavit-Hartenberg Homogeneous Transformation
# 2.4 Loop-Closure Equations
# 2.5 Other Coordinate Systems
# 2.6 Denavit-Hartenberg Method
# 2.7 Method of Successive Screw Displacements
# 2.8 Summary