---
title:  "[Modern Robotics] 3. Rigid-Body Motions"
excerpt: "'Modern Robotics: Mechanics, Planning, and Control' 정리"

categories:
  - In-progress
tags:
  - [Rigid-body, Position, Orientation, Spatial velocity, Twist]

toc: false
toc_sticky: true
use_math: true
 
date: 2021-09-27
last_modified_at: 2021-09-29
sitemap :
  changefreq : daily
  priority : 1.0
---

이번 챕터에서는 rigid body에 reference frame을 붙여 position과 orientation을 묘사하는 체계적인 방법을 다룬다. 고정된 기준 frame에 대한 이 frame의 configuration은 4 $\times$ 4 matrix로 표현된다. Rigid-body configuration의 실제 6-dimensional space는 4 $\times$ 4 matrix의 16-dimensional space에 10개의 constraints를 적용하여 얻을 수 있다. 

이 matrix는 frame의 configuration을 표현할 뿐만 아니라, 다음에 사용될 수 있다.
1) translate and rotate a vector or a frame
2) change the representation of a vector or a frame from coordinates in one frame to coordinates in another frame

이런 연산들은 간단한 선형대수에 의해 수행될 수 있고, 이것이 바로 configuration을 4 $\times$ 4 matrix로 표현하는 이유이다. 

C-space의 position과 orientation의 non-Euclidean (i.e. non-"flat") 성질 때문에 matrix 표현을 사용한다. 그러나 Rigid-body의 속도(velocity)는 간단하게 $\mathbb{R}^6$ 상의 한 점으로 표현할 수 있고, 3개의 각속도(angular velocity)와 선속도(linear velocity)로 정의된다. 이 둘을 합쳐서 **spatial velocity** 또는 **twist**라고 부른다. 좀 더 일반적으로, 로봇의 C-space가 vector로 표현될 수 없다고 하더라도 C-space 상의 어떤 point에서의 feasible velocities의 집합은 항상 vector space를 형성한다. 
* ex) C-space가 sphere $S^2$인 로봇 -> sphere 표면 상의 한 점에서의 velocity space는 그 점에 tangent plane으로 생각할 수 있다. 

모든 rigid-body configuration은 fixed (home) reference frame에서 시작해서 constant twist를 특정 시간동안 적분하여 도달할 수 있다. 이러한 motion은 같은 고정된 축을 따라 translation과 rotation을 하는 screw motion과 비슷하다. 모든 configuration이 screw motion으로 얻어질 수 있다는 사실로부터 configuration을 6개의 parameter로 표현하는 **exponential coordinates**가 나오게 되었다. 이 6개의 parameter는 다음의 두 부분으로 나눠질 수 있다.
1) the parameters describing the direction of the screw axis
2) a scalar to indicate how far the screw motion must be followed to achieve the desired configuration

이번 챕터는 forces에 대한 논의로 마무리한다. 각속도와 선속도가 함께 묶여 하나의 vector를 구성한 것처럼, moments(torques)와 forces도 함께 묶여 하나의 vector를 구성하고 **spatial force** 또는 **wrench**라고 불린다. 

몇 가지 vector 표기법에 대한 설명 후, planar example을 통해 개념들을 설명하고 이번 장의 개요를 제공하고자 한다. 
 
## A Word about Vectors and Reference Frames
### **free vector**
### **coordinate free**
### **fixed frame or space frame {s}**
### **body frame {b}**

# 3.1 Rigid-Body Motions in the Plane
### **rotation matrix**
### **rigid-body displacement or rigid-body motion**
### **screw motion**
### **screw axis**
### **exponential coordinates**
### **twist**
### **Euler an- gles**
### **roll–pitch–yaw angles**
### **Cayley–Rodrigues parameters**
### **unit quaternions**


# 3.2 Rotation and Angular Velocities
## 3.2.1 Rotation Matrices
### **3.2.1.1 Properties of Rotation Matrices**
### **3.2.1.2 Uses of Rotation Matrices**
**Representing an orientation**

**Changing the reference frame**

**Rotating a vector or a frame**

## 3.2.2 Angular Velocities

## 3.2.3 Exponential Coordinate Representation of Rotation
### **3.2.3.1 Essential Results from Linear Differential Equations Theory**

### **3.2.3.2 Exponential Coordinates of Rotations**

### **3.2.3.3 Matrix Logarithm of Rotations**

# 3.3 Rigid-Body Motions and Twists
## 3.3.1 Homogeneous Transformation Matrices
### **3.3.1.1 Properties of Transformation Matrices**

### **3.3.1.2 Uses of Transformation Matrices**
**Representing a configuration**

**Changing the reference frame of a vector or a frame**

**Displacing (rotating and translating) a vector or a frame**

## 3.3.2 Twists
**spatial velocity in the body frame (body twist)**

**spatial velocity in the space frame (spatial twist)**

### **3.3.2.1 Summary of Results on Twists**

### **3.3.2.2 The Screw Interpretation of a Twist**

## 3.3.3 Exponential Coordinate Representation of Rigid-Body Motions

### **3.3.3.1 Exponential Coordinates of Rigid-Body Motions**

### **3.3.3.2 Matrix Logarithm of Rigid-Body Motions**

# 3.4 Wrenches

# 3.5 Summary
