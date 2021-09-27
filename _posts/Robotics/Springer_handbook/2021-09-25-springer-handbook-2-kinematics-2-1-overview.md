---
title:  "[Springer handbook] 2. Kinematics - 2.1 Overview"
excerpt: "'Springer handbook of robotics' 정리"

categories:
  - Robotics
tags:
  - [Kinematics]

toc: false
toc_sticky: true
use_math: true
 
date: 2021-09-25
last_modified_at: 2021-09-25
sitemap :
  changefreq : daily
  priority : 1.0
---

## Robotic mechanisms
따로 언급이 있지 않는 한 여기서는
> *systems of rigid bodies connected by joints*

를 의미함

## Pose 
> ***position** and **orientation** of a rigid body in space*

## Robot Kinematics
Robot kinematics는 pose, velocity, acceleration, 그리고 mechanism을 이루는 body들의 pose의 모든 higher-order derivatives를 묘사함

Kinmatics는 motion을 유도하는 forces/torques를 다루지 않기 때문에, 이번 챕터에서는 **pose**와 **velocity**를 묘사하는데 초점을 맞춤

이는 앞으로 나올 dynamics (Chap. 3), motion planning (Chap. 7), motion control (Chap. 8) algorithms의 **기본요소**임 

## Serial chain, Parallel mechanism, Tree structure
Body들이 연결될 수 있는 수 많은 topology들 가운데 로보틱스에서는 serial chain과 fully parallel mechanism 두 가지가 특히 중요함
### **Serial chain**
* A system of rigid bodies in which each member is connected to two others, except for the first and last members that are each connected to only one other member.
* ex) 6-DOF manipulator

### **Fully parallel mechanism**
* One in which there are two members that are connected together by multiple chains of other members and joints.
* In practice, each of these chains is often itself a serial chain.
* ex) Stewart platform

이번 챕터에서는 serial chain에 적용할 수 있는 알고리즘들에 중점을 둠 (parallel mechanism은 Chap. 18을 참조)

또 다른 중요한 topology로는 tree structure가 있음

### **Tree structure** (Chap. 3)
* Closed loop가 없다는 점에서 serial chain과 비슷하지만, 한 링크에 여러 링크가 연결되어 **multiple *branches***를 형성할 수 있다는 점에서 다름
* Serial chain은 tree structure의 special case라고 볼 수 있음 (tree structure with no branches)
* ex) Quadruped robot


