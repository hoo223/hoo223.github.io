---
title:  "Parameter sharing between policy and value network"
excerpt: ""

categories:
  - RL
tags:
  - [Parameter sharing, Policy network, Value network]

toc: false
toc_sticky: true
use_math: true
 
date: 2021-09-23
last_modified_at: 2021-09-23
sitemap :
  changefreq : daily
  priority : 1.0
---

Policy network와 value network가 결합된 Actor-Critic 알고리즘에서 두 network의 parameter를 공유하는 경우가 있다. 
* [1]에 parameter sharing의 장/단점이 설명되어 있다.

* AlphaZero 논문에서는 residual network와 parameter sharing의 유무에 따른 성능 비교를 하였는데 둘 다 존재할 때 가장 성능이 좋았다고 한다. [2]

* PPO 논문 [3]에서도 policy function과 value function 간 parameter를 공유하는 neural network 구조를 사용하고, 이를 위해 policy loss (surrogate loss)와 value loss (value function error)를 결합한다.

* [4]에서는 5개의 locomotion 환경(HalfCheetah,
Hopper, Ant, Walker2D, and Humanoid)과 3개의 robotic manipulation 환경(Reacher, Pusher, and Kuka)에 parameter sharing 유무에 따른 성능 비교 실험을 진행하였다. Pusher를 제외한 나머지 환경에서는 parameter sharing이 **더 나쁜 성능**을 보였다. 

* [5], [6] -> 관련 stack overflow 질문


# Reference
[1] [Foundations of Deep Reinforcement Learning: Theory and Practice in Python - 6.5 Network Architecture](https://www.informit.com/articles/article.aspx?p=2995356&seqNum=5)        
[2] [A Simple Alpha(Go) Zero Tutorial](https://web.stanford.edu/~surag/posts/alphazero.html)        
[3] [Schulman, J., Wolski, F., Dhariwal, P., Radford, A., & Klimov, O. (2017). Proximal policy optimization algorithms. arXiv preprint arXiv:1707.06347.](https://arxiv.org/pdf/1707.06347.pdf)       
[4] [Gronauer, S., Gottwald, M., & Diepold, K. The Successful Ingredients of Policy Gradient Algorithms.](https://www.ijcai.org/proceedings/2021/0338.pdf)      
[5] [How do shared parameters in actor-critic models work?](https://stackoverflow.com/questions/56312962/how-do-shared-parameters-in-actor-critic-models-work)      
[6] [How does sharing parameters between the policy and value functions help in PPO?](https://ai.stackexchange.com/questions/27932/how-does-sharing-parameters-between-the-policy-and-value-functions-help-in-ppo)