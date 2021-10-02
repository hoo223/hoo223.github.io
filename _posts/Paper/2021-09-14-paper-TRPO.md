---
title:  "Trust Region Policy Optimization"
excerpt: "Trust Region Policy Optimization 논문 정리"

categories:
  - In-progress
tags:
  - [Research, RL, Policy-based]

toc: false
toc_sticky: true
use_math: true
 
date: 2021-09-15
last_modified_at: 2021-09-30
sitemap :
  changefreq : daily
  priority : 1.0
---

PPO 논문을 이해하려면 TRPO 논문에 대한 이해가 먼저 필요합니다.  
항상 두 논문에 대해 좀 더 깊게 이해해보고 싶었는데 이번 기회에 한번 정리해보려고 합니다.    
팡요랩 리뷰 영상이 이해하는데 많은 도움이 되었습니다.   
다음의 reference를 참고하여 작성하였습니다.

[[쉽게읽는 강화학습 논문 5화] TRPO 논문 리뷰](https://www.youtube.com/watch?v=XBO4oPChMfI&t=3268s)    
[TRPO 논문](https://arxiv.org/abs/1502.05477)

# 1. Introduction

## Motivation
많은 policy optimization 알고리즘들은 크게 3가지로 분류할 수 있다.    
1. Policy iteration methods
    * 현재 policy 하에서 value function을 추정하고 -> policy를 향상시키는 과정을 반복 (Bertsekas, 2005)   
2. Policy gradient methods    
    * Sample trajectories로부터 얻어지는 expected return (total reward)의 gradient estimator를 사용 (Peters & Schaal, 2008a)    
3. Derivative-free optimization methods   
    * Cross-entropy method (CEM), covariance matrix adaptation (CMA)    
        * Return을 policy parameter에 대해 최적화되어야 할 black box function으로 다룸 (Szita & Lorincz, 2006)

CEM과 CMA와 같은 일반적인 derivative-free stochastic optimization methods는 많은 문제들에서 선호된다. 왜냐하면 이해하고 구현하기가 쉬우면서도 좋은 결과를 내기 때문이다. 
* 예를 들면, 테트리스가 approximate dynamic programming (ADP) 방법들을 위한 전통적인 benchmark 문제임에도 stochatic optimization methods를 이기기 어려웠다 (Gabillon et al., 2013). 
* Continuous control problem에서는, CMA 같은 방법들이 locomotion과 같은 challenging tasks를 위한 control policy를 배우는데 성공을 거둬왔다. 

Gradient-based optimization 알고리즘은 gradient-free 방법보다 더 나은 sample complexity을 보장하지만 성능면에서 일관되게 이기지 못한다는 점에서 만족스럽지 못하다. 

Continuous gradient-based optimization은 수많은 parameter들을 사용하여 supervised learning tasks를 위한 function approximator를 배우는데 매우 큰 성공을 거둬왔고, 이를 강화학습으로 확장하면 복잡하고 강력한 policy를 효과적으로 훈련할 수 있다.


## Contribution
이 논문에서는 
* 먼저, 특정 surrogate objective function을 non-trivial step size로 minimize하는 것이 policy improvement를 보장하는 것을 증명하였다. 
* 그 후 이론적으로 정당화된 (theoretically-justifie) 알고리즘에 일련의 근사화 과정을 거쳐 **trust region policy optimization (TRPO)** 라고 불리는 실용적인 알고리즘을 도출함.
* 이 알고리즘의 두 가지 변형을 묘사함    
    1) *single-path* method
        * can be applied in the model-free setting
    2) *vine* method
        * requires the system to be restored to particular states, which is typically only possible in simulation

These algorithms are scalable and can optimize nonlinear policies with tens of thousands of parameters, which have previ- ously posed a major challenge for model-free policy search (Deisenroth et al., 2013). 

In our experiments, we show that the same TRPO methods can learn complex policies for swimming, hopping, and walking, as well as playing Atari games directly from raw images.

---
---

# 2. Preliminaries
## MDP
![1](https://user-images.githubusercontent.com/17296297/133378285-c8df6fc8-4087-4455-9ab7-d60c39a4eb42.png)

---

## Stochastic policy
![2](https://user-images.githubusercontent.com/17296297/133378284-849f68f6-70d8-489a-9c50-3758da605a22.png)

---

## Expected (cumulative) discounted reward
![3](https://user-images.githubusercontent.com/17296297/133378282-f0b327ea-6223-43dd-8b2b-6de486e1910c.png)

---

## Action value, state value, advantage function
![4](https://user-images.githubusercontent.com/17296297/133378280-4a8e3437-8c49-4e07-aebc-7c5cb56748a2.png)

---

## Useful identity from "Kakade & Langford (2002)"
![5](https://user-images.githubusercontent.com/17296297/133378277-26b76044-f0ce-4c5b-a9cf-a10cc2cf3a3d.png)   
식 (1)은 한 policy π의 expected return과 다른 policy π ̃의 expected return 간의 관계를 표현한 식이다. 
* the expected return of another policy $\tilde{\pi}$ in terms of the advantage over $\pi$, accumulated over timesteps
* 이에 대한 증명은 본 논문의 Appendix A를 참조

이 식의 의미는 다음과 같이 해석할 수 있다. 
  * 모든 state s에서 nonnegative expected advantage를 갖는 모든 policy update $\pi \rightarrow \tilde{\pi}$ 는 policy performance $\eta$가 증가하거나 유지하는 것을 보장함 (expected advantage가 모두 0인 경우) = policy 성능이 떨어지지 않는 것을 보장함    
  * 이는 Deterministic policy $\tilde{\pi}=argmax_a A_\pi (s,a)$를 사용하는 policy iteration에 의해 수행되는 update의 결과와 동일하다.
    * 만약 positive advantage value와 nonzero state visitation probability를 갖는 state-action pair가 적어도 하나 존재한다면 policy가 향상됨
    * 그렇지 않을 경우, optimal policy로 수렴

그러나 다음과 문제점이 존재한다. 
  * 일반적으로 approximate setting에서는, 추정/근사 error에 의해 어떤 state s에서 expected advantage가 **음수**인 경우들이 존재    
  ![6](https://user-images.githubusercontent.com/17296297/133378275-5bf9667b-4fda-4f16-904f-b3fde85c234c.png)
  * $\rho_{\tilde{\pi}}(s)$에서 $\pi$ ̃에 대한 의존성이 존재해 직접적인 최적화를 어렵게 만듦
* Solution
  * Local approximation to $\eta$    
    ![7](https://user-images.githubusercontent.com/17296297/133378274-264562cf-be65-4173-a16e-03538a644e9d.png)
    * policy 변화에 따른 state visitation density의 변화를 무시 -> 이래도 되는가? 
    * 가능하다. θ로 미분 가능한 parameterized policy $\pi_\theta$ 를 고려할 경우 -> $\theta_0 (= \theta_{old})$에서 first order까지 일치 (see Kakade & Langford (2002))    
    ![8](https://user-images.githubusercontent.com/17296297/133378272-c25da6c6-cb80-4d5a-86da-45d4105423e7.png)
      * 의미 : $L_{\pi_{\theta_{old}}}$를 향상시키는 충분히 작은 step $\pi_{\theta_0} \rightarrow \tilde{\pi}$는 $\eta$ 또한 향상시킴    
      **"$L_{\pi_{\theta_{old}}}$를 증가 시키는 것이 곧 $\eta$ 를 증가시키는 것과 같다!"**
      * 문제점 : step 크기가 얼마나 작아야 하는지는 모름

---

## Conservative policy iteration
* 위 문제를 다루기 위해 Kakade & Langford (2002)가 제안한 policy updating scheme    
  ![9](https://user-images.githubusercontent.com/17296297/133378268-3f09ed77-bdd4-4dfa-be3c-400cf919eacd.png)
  * $\pi_{old}$  : current policy
  * $\pi'=argmax_{\pi'}L_{\pi_{\theta_{old}}}(\pi')$
* $\eta$의 improvement에 대한 명시적인 lower bounds를 제공   
  ![10](https://user-images.githubusercontent.com/17296297/133378266-51dc2e26-35fc-411a-bb84-f6c947ab5ea7.png)
* 의미
  * Right-hand side를 향상시키는 policy update가 true performance 의 향상을 보장함
* 문제점
  * 식 (5)로 생성된 mixture policy들에만 적용 가능 -> 실용적이지 못함   
  => 모든 general stochastic policy classes에 적용 가능한 실용적인 policy update scheme이 필요

---
---

# 3. Monotonic Improvement Guarantee for General Stochastic Policies
* $\alpha$를 $\pi$와 $\tilde{\pi}$ 간의 distance measure로 대체하고 상수 를 적절하게 변경
  * Distance measure = total variation divergence for discrete probability distribution p, q  
  ![11](https://user-images.githubusercontent.com/17296297/133378263-024a308d-cb4a-4ee9-ba11-2547c0c81319.png)
  ![12](https://user-images.githubusercontent.com/17296297/133378260-06dbacd6-c7c2-4b68-859b-32fcf65aebd2.png)
    * 모든 state에 대한 최대값 
* Theorem 1 (proved in the appendix)
![13](https://user-images.githubusercontent.com/17296297/133378259-1f2b38a1-a7a9-4c1a-a885-ca024b7a958d.png)

* Relationship between the total variation divergence and the KL divergence (Pollard (2000), Ch. 3)   
![14](https://user-images.githubusercontent.com/17296297/133378258-97b2cbfb-1c6c-499c-8995-14f711cd4a1a.png)    
![15](https://user-images.githubusercontent.com/17296297/133378256-352f5ef7-c579-4205-99a7-36b70e73bac8.png)

* Modified bound with KL divergence   
![16](https://user-images.githubusercontent.com/17296297/133378254-dbe09313-53b6-4e90-b6ef-34ad4e46eb31.png)

---

## 지금까지의 흐름
![17](https://user-images.githubusercontent.com/17296297/133378252-68a94bd0-9d74-48cf-bf90-02010c9feac7.png)    
출처: [[쉽게읽는 강화학습 논문 5화] TRPO 논문 리뷰](https://www.youtube.com/watch?v=XBO4oPChMfI&t=3268s)

---

## Algorithm 1: Policy iteration algorithm guaranteeing non- decreasing expected return η
![18](https://user-images.githubusercontent.com/17296297/133378251-64ff33ee-201c-415e-ae75-9e4a99f35efa.png)    
* Equation (9)의 policy improvement bound 기반의 policy iteration scheme
* 여기서는 advantage values $A_\pi$ 의 정확한 계산를 가정
* policy들의 monotonically improving sequence의 생성을 보장   
  ![19](https://user-images.githubusercontent.com/17296297/133378250-151d4f90-d312-47fe-87fc-0cb0553addf1.png)
  * Proof   
  ![20](https://user-images.githubusercontent.com/17296297/133378249-854668a3-4f66-432f-a24f-68fdf0d35fe0.png)
  * 매 iteration에서 $M_i$ 를 최대화함으로써, true objective η의 non-decreasing을 보장
* Minorization-maximization (MM) algorithm의 한 종류
  * MM algorithm의 용어에서,    
    $M_i$  : surrogate function that minorizes η with equality at $\pi_i$



---

# 4. Optimization of Parameterized Policies
* 앞에서는 모든 state에서 policy를 평가할 수 있다는 가정하에 π의 parameterization과 독립적인 policy optimization 문제를 고려함
* Parameterized policies π_θ (a|s) with parameter vector θ 고려   
  ![21](https://user-images.githubusercontent.com/17296297/133378248-2895607a-a985-487b-a075-5d3af025c1a7.png)    
  ![22](https://user-images.githubusercontent.com/17296297/133378246-88ecbf01-77b3-48e3-91db-bf08d0e2a2c3.png)    
  θ_old  : previous policy parameters that we want to improve upon    
  ![23](https://user-images.githubusercontent.com/17296297/133378243-a0a3b1c2-744b-4862-8a1e-5bfd0f5233a4.png)    
 	아래의 maximization을 수행함으로써 true objective η의 향상을 보장   
  ![24](https://user-images.githubusercontent.com/17296297/133378242-cc1d861c-0bb4-4f99-9ce9-da64ba4378b4.png)
* 문제점
  * 위 이론에서 추천하는 penalty coefficient C를 사용하면 step size가 매우 작아질 수 있음
* Robust하게 좀더 큰 step을 취하기 위한 이 논문의 해결방안
  * Constraint 형태로 변환 => trust region constraint
  * 새로운 policy와 기존 policy 간의 KL divergence에 대한 constraint를 사용   
    ![25](https://user-images.githubusercontent.com/17296297/133378240-a41f56df-cfa6-4111-a1c4-ca4bd031328a.png)
  * 이 때의 문제점 -> state space의 모든 point에 대하여 고려해야 함
  * 해결방안 -> max값 대신 평균 값을 사용하자 => average KL divergence    
    ![26](https://user-images.githubusercontent.com/17296297/133378239-7c03e950-8a10-417b-b955-f9e00bd33374.png)    
    ![27](https://user-images.githubusercontent.com/17296297/133378233-dc2e331a-be52-434c-821e-d5cc1b15d8e5.png)
  * 실험을 통해 maximum KL divergence와 비슷한 성능임을 보임

# 5. Sample-Based Estimation of the Objective and Constraint
이전 section에서는 policy parameter에 대한 constrained optimization problem을 제안했다 (Equation (12)). 이 optimization problem은 매 update마다 policy의 변화에 대한 constraint 하에서 expected total reward $\eta$의 추정값을 최적화한다.

 Monte Carlo simulation을 사용해 objective와 constraint function을 근사    
...






