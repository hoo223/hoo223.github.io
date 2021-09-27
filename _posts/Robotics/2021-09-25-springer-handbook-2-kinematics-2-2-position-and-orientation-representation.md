---
title:  "[Springer handbook] 2. Kinematics - 2.2 Position and Orientation Representation"
excerpt: "'Springer handbook of robotics' 정리"

categories:
  - Robotics
tags:
  - [Kinematics, In-progress]

toc: false
toc_sticky: true
use_math: true
 
date: 2021-09-25
last_modified_at: 2021-09-25
sitemap :
  changefreq : daily
  priority : 1.0
---

## Spatial, rigid-body kinematics
* Body의 pose를 표현하는 다양한 방법들을 비교하는 연구라고 볼 수 있다. 
* Translation과 rotation은 합쳐서 rigid-body displacements라 불리고, 이런 다양한 방법들을 통해 표현된다. 
* 각 방법은 모든 목적에 대해 최적일 수는 없지만, 각각의 장점들을 적절히 활용하여 서로 다른 문제들을 해결할 수 있다. 
* Euclidean 공간에서 하나의 body를 위치시키는데 필요한 좌표(coordinates)의 최소 개수는 6개이다. 
* Spatial pose (공간에서의 자세)를 표현하는 많은 방법들은 매우 풍부한 좌표 집합을 사용하는데, 이 좌표들 사이에는 보조 관계(auxiliary relationships)들이 존재한다. 이때 독립적인 보조 관계들은 집합 내의 좌표의 수와 6과의 차이만큼 존재한다.  

## Coordiante frame
이번 챕터와 따라오는 챕터들에서는 *coordinate frames* (줄여서 *frames*)를 자주 사용한다. 
* Coordinate frame *i*은 다음을 포함한다. 
  * 원점(origin) $O_i$
  * 서로 수직인 basis vector $(\hat{x}_i, \hat{y}_i, \hat{z}_i)$ 
    * Mutually orthogonal
  * 모두 특정 body에 고정되어 있음 -> 각 body에는 고유의 coordinate frame이 존재 
* 각 body에는 고유의 coordinate frame이 존재하므로 각 body의 pose는 coordinate frame의 pose로 표현될 수 있다. 
  * Body의 pose는 **항상 어떤 다른 body에 대해 상대적으로 표현**된다. 따라서 body의 pose를 표현하기 위해서는 다른 coordinate frame을 기준으로 해당 body의 coordinate frame의 상대적인 pose를 구해야 한다.  
* 비슷하게, rigid-body의 displacement(변위)는 두 coordinate frame 사이의 displacement로 표현될 수 있다. 이때 한 frame은 ***moving***, 다른 frame은 ***fixed***로 불린다. 
  * 이는 관찰자가 fixed frame에서 정지 위치에 있음을 나타내는 것이지, 어떤 절대적인 fixed frame이 있음을 나타내는 것은 아니다. 

*(그림 - coordinate frame)*

# 2.2.1 Position and Displacement

## Poistion
Coordinate frame *i*의 원점의 coordinate frame *j*에 대한 상대적인 위치(position)는 $3 \times 1$ vector로 나타낼 수 있다. 
$$^{j}\mathbf{p}_{i}=(^{j}p_i^x, ^{j}p_i^y, ^{j}p_i^z)^T$$

이 vector의 원소들은 *j* frame 기준으로 본 $O_i$의 **Cartesian** coordinate이다.
* 이는 vector $^{j}\mathbf{p}_{i}$를 *j* frame의 각 basis vector로 projection한 값이다. 

마찬가지로 **spherical** 또는 **cylindrical** coordinates로도 표현할 수 있다. 
* 이는 spherical과 cylindrical joints를 포함하는 robotic mechanism을 분석할 때 장점이 있다.

## Displacement
### **Translation**
> Rigid body 상의 어떤 point도 초기 위치를 유지하지 않고, rigid body 상의 모든 직선이 처음의 방향과 평행을 유지하는 displacement 
* 어떤 position에 대한 표현도 translation (displacement)을 표현하는데 사용될 수 있고, 그 반대도 마찬가지. (position $\Leftrightarrow$ translation)

# 2.2.2 Orientation and Rotation
Position에 대한 표현보다 orientation에 대한 표현이 훨씬더 광범위하다. 이 섹션에서는 robotic mechanism에 가장 일반적으로 적용되는 표현들에 중점을 둔다.


### **Rotation**
> Rigid body 상의 적어도 한 점이 초기 위치를 유지하고, body 상의 모든 직선이 초기 방향과의 평행을 유지하지 않는 displacement
* 예를 들면, 어떤 body가 있다고 회전 축(axis)을 중심으로 회전할 때 회전 축 상에 존재하는 body 상의 point들은 초기 위치를 유지한다. 
* Position과 translation의 경우와 마찬가지로, 어떤 orientation에 대한 표현도 rotation (displacement)을 표현하는데 사용될 수 있고, 그 반대도 마찬가지. (orientation $\Leftrightarrow$ rotation)


## Rotation Matrices

## Euler Angles

## Fixed Angles

## Angle-Axis

## Quaternions

# 2.2.3 Homogeneous Transformations

# 2.2.4 Screw Transformations

## Chasles' Theorem

## Rodrigues’ Equation
어떤 body에 대해 다음 3가지가 주어지면 그 body 상의 임의의 점의 displacement를 찾을 수 있다. 
1) Screw axis 
2) Angular displacement of a body about it 
3) Translation of the body along it 

Matrix transformation이 body의 displacement를 표현한다는 관점에서, 이 문제는 주어진 screw displacement에 대응하는 matrix transformation을 찾는 문제와 같다. 

 
# 2.2.5 Matrix Exponential Parameterization
한 body의 position과 orientation은 또한 exponential mapping과 함께 통합된 방식으로 표현될 수 있다. 이 방법은 먼저 rotation만 존재하는 application에 소개되었고 rigid-body motion으로 확장되었다. 

## Exponential Coordinates for Rotation

## Exponential Coordinates for Rigid-Body Motion

# 2.2.6 Plücker Coordinates