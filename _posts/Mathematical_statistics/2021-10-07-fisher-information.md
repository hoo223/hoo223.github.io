---
title:  "Fisher information, 피셔 정보"
excerpt: ""

categories:
  - Mathematical-statistics
tags:
  - [Fisher information, 피셔 정보]

toc: false
toc_sticky: true
use_math: true
 
date: 2021-10-07
last_modified_at: 2021-10-14
sitemap :
  changefreq : daily
  priority : 1.0
---

TRPO를 공부하다보니 Fisher information matrix라는 개념이 등장했는데 처음 접했던 개념이라 생소했다. 여러 자료들을 통해서 어떤 의미를 가지고 있는지, 논문에서는 어떤 흐름에서 등장했는지를 한번 정리해보고자 한다. 

아래 내용은 reference [1]의 내용을 번역하여 정리한 것이다. 

# Fisher Information

Random variable Y가 어떤 알려진 probability density function $P(Y\mid\theta)$ 에 의해 parameter $\theta$ 와 연관이 있다고 가정하자. 
* 즉, $\theta$는 hypothesis, Y는 data, $P(Y\mid\theta)$ 는 likelihood라고 볼 수 있다. 

Fisher inforamtion의 Goal은 다음과 같다. 
> **Goal**: Measure how well an observation of Y locates the true value of $\theta$.

즉, Fisher information은 Y의 observation을 이용해 얼마나 $\theta$의 실제 값을 잘 찾을 수 있는지 측정하는 지표라고 볼 수 있다. 
 
좀 더 직관적인 이해를 위해 다음의 두 density function 예시를 고려해보자. 
* The Normal Distribution with a *large* variance:
  $$p(Y\mid\theta)=\mathcal{N}(Y\mid\mu,25)$$
* The Normal Distribution with a *small* variance:
  $$p(Y\mid\theta)=\mathcal{N}(Y\mid\mu,1)$$

Observation으로부터 parameter를 추정할 때는 주로 **log likelihood** 함수를 이용한다. 아래 그림은 두 density function의 log likelihood 함수를 나타낸 것이다. 

![1](https://user-images.githubusercontent.com/17296297/136661528-1b7bf9a0-5cf4-4afb-a46d-26d8db0b6856.PNG)

*그림 1. log likelihood graphs of two distribution*   
<br/>


그래프에서 하나의 line은 하나의 observation에 대응하는 log likelihood 값이고, 이를 $\mu$ 값의 변화에 따라 나타낸 것이다. 즉, 이 line들은 각 observation이 각각의 $\mu$에서 얼마나 가능성이 있는지 말해준다. 

Data (observation)들은 우리가 모르는 어떤 parameter로부터 생성되었고 이를 **true parameter**라 부르자. 

$$
\text{두 density function 중 어느 쪽이 true parameter를 추측하기 더 쉬울까?}
$$

당연하게도 small variance인 경우가 더 쉽다. 각 observation은 각 line을 통해 true parameter가 어디에 있는지 투표한다고 볼 수 있다 (log prob 값이 가장 큰 위치의 $\mu$값). Small variance의 경우 투표 결과가 더 좁은 범위에 몰려있으므로 실제 값을 예측할 확률이 높아진다. (실제 parameter 값은 그림에서와 같이 $\mu^*=5$이다. )

따라서, small variance인 경우 관측값들이 true parameter $mu^*$에 대한 더 많은 정보(information)를 제공한다고 말할 수 있다.  

**여기서 중요한 것은 information이라는 것이 narrowness와 관련이 있다는 것이다.** 더 큰 narroness는 더 많은 정보를 의미한다고 볼 수 있다. 이러한 관점이 여러분이 Fisher information을 이해하는데 필요한 모든 것이다.

그러나 이와 별개로 다음과 같은 질문들이 생긴다.
* Fisher information은 어떻게 이 narrowness를 수치로 표현하는지?
* 우리는 어떻게 그 information을 측정할 수 있는지?

이 질문에 답하기 위해선 새로운 관점이 필요하다. 

먼저, small variance case에 대해 *그림 2*와 같이 그래프들을 표현해보자.

![2](https://user-images.githubusercontent.com/17296297/136661534-22c1cb56-ac1e-4eb6-bffd-bec5519ed9f6.png)

*그림2.*    
<br/>

(1) $\mu$값에 따른 $log\mathcal{N}(y_i\mid\mu,\sigma^2)$ 그래프    
(2) $\mu_0$에서 각 observation $y_i$에 대응하는 $log\mathcal{N}(y_i\mid\mu,\sigma^2)$의 분포 (density function)   
(3) $log\mathcal{N}(y_i\mid\mu,\sigma^2)$를 $\mu$에 대해 미분한 값 ${\partial \over \partial \mu}log\mathcal{N}(y_i\mid\mu,\sigma^2)$ (=(1)에서의 기울기) 그래프 = score function    
(4) $\mu_0$에서 각 observation $y_i$에 대응하는 ${\partial \over \partial \mu}log\mathcal{N}(y_i\mid\mu,\sigma^2)$들의 분포 (density function)    

* 일반적으로 log likelihood를 parameter로 미분한 값을 *score function*이라 부른다. (3)의 경우, socre functiion이 linear 함수이다. 

이제 $\mu_0$를 움직여보자.

![3](https://user-images.githubusercontent.com/17296297/136661536-8f4c4486-2a15-4038-a577-5b103e54ed67.gif)

*그림 3*    
<br/>

위 그림과 같이 $\mu_0$가 움직임에 따라 (2), (4)의 분포가 변화하는 것을 관찰할 수 있다. 

(4)의 경우, 형태(=분산)는 유지하면서 평균 값만 이동하는데, 이는 score function이 linear 함수이기 때문이다. Normal distribution이 아닌 경우에는 score function이 linear 함수가 아닐 수 있으므로 형태 또한 변화게 된다. 따라서 이 관측 결과는 크게 신경 쓸 필요가 없다. 

모든 경우에 적용되는 중요한 관측은 바로 다음의 내용이다. 

그 내용은 바로 우리가 true parameter value에서 socre function을 평가할 떄 (즉, $\mu_0=\mu^*$일 때), score들의 평균이 **0**이 된다는 것이다. (그림 4)

![4](https://user-images.githubusercontent.com/17296297/136661541-6a488bc4-6d57-4d41-a8f4-a19a943dfecb.png)

*그림 4*    
<br/>

만약 score function의 평균이 0이 아니라면, 우리의 데이터는 true parameter일 가능성이 더 높은 다른 파라미터 값이 있다고 말하는 것과 깉다. 

이제 우리는 Fisher information을 볼 준비가 됐다. 

우선, small variance를 갖는 informative case (그림 4)에서 variance를 증가시켜 large variance를 갖는 uninformative case (그림 5)를 생각해보자.  

![5](https://user-images.githubusercontent.com/17296297/136661983-42d696e0-10b8-44b1-8e47-183daa159dcb.PNG)

*그림 5*    
<br/>

두 경우를 비교해보면 다음과 같은 사실을 알 수 있다.
* informative case가 uninformative case보다 score function의 분포 (4)에서 더 큰 분산 값을 갖는다. 
* 조금 더 직관을 얻기 위해 single small score value를 생각해보자. 만약 score value의 값이 0.001이라면, log likelihood를 1 증가시키기 위해 파라미터를 1000 움직여야 한다. 반대로 -0.001이라면 반대 방향으로 1000을 움직여야 한다. 이는 log likelihood를 변화시키기 위해 더 많이 움직여야 한다는 뜻이고, 다르게 생각하면 true parameter를 찾기 위해 더 넓은 범위를 탐색해야 한다는 뜻이다. 더 넓은 범위를 탐색해야 한다는 건 현재 가진 정보가 부족하다고 볼 수 있다. 
* 정리해보자면,
    * small score value -> large search space -> less information   
    * large score value -> small search space -> more information
* Score value가 0 근처에서는 각각의 observation들이 개별적으로 true parameter가 어디 있는지 광범위하게 추천하지만, 집합적으로는 (평균적으로는) 움직이지 말라고 한다. 
 
이것이 Fisher information 뒤에 숨어있는 핵심 아이디어이다. 

## Definition
Fisher information의 다음과 같이 정의된다.
> True parameter일 때 측정한 score function 분포의 분산 값 

또는

>  score function 분포의 평균이 0에 위치해 있을 때 score function 분포의 분산 값

![6](https://user-images.githubusercontent.com/17296297/136697092-7274b3dc-b392-4fc5-8246-edb3f5dce15c.png)

*그림 6*    
<br/>

그림 6을 보면 score function들의 분포에 양쪽으로 $\mu\pm\sigma$ 범위를 나타내고 있고, $\sigma$는 분산의 제곱근이므로 $\sqrt{FI}$로 대신하였다. 

Informative case와 uninformative case를 비교하면 차이를 더 쉽게 느낄 수 있다 (그림 7). 

![7](https://user-images.githubusercontent.com/17296297/136697763-96a12519-f684-420c-a85d-506633b0b0ef.png)

*그림 7*    
<br/>

다시 한번 정리하자면, Fisher information의 goal은 (log likelihood function들의) narrowness를 측정하는 것이다. Narrowness가 커질수록 기울기들의 분포가 다양해지고 (= 분산이 커지고) Fisher information 값이 증가한다. 

여기서 한 발자국 더 나아가면 추가적인 정보를 얻을 수 있다. Log likelihood 함수의 1차 기울기 뿐만 아니라 2차 기울기까지 고려해보자. 

![8](https://user-images.githubusercontent.com/17296297/136698177-044c6ada-ec94-4910-846c-3f1db9a82182.png)

*그림 8*    
<br/>

(5) $log\mathcal{N}(y_i\mid\mu,\sigma^2)$를 $\mu$에 대해 두 번 미분한 값 ${\partial^2 \over \partial \mu^2}log\mathcal{N}(y_i\mid\mu,\sigma^2)$ (=(3)에서의 기울기) 그래프    
(6) $\mu$에서 각 observation $y_i$에 대응하는 ${\partial^2 \over \partial \mu^2}log\mathcal{N}(y_i\mid\mu,\sigma^2)$들의 분포 (density function)

* 그림 8과 같이 normal distribution인 경우에는 2차 미분 값이 상수이다. 

여기서 알 수 있는 사실은 Fisher information이 score function 분포의 분산이면서 동시에 (만약 존재한다면) 2차 미분 값들의 *negative expected value*와 같다. ([증명](https://en.wikipedia.org/wiki/Fisher_information#:~:text=right%7C%5Ctheta%20%5Cright%5D%2C%7D-,since,-%7B%5Cdisplaystyle%20%7B%5Cfrac%20%7B%5Cpartial))
$$
\text{Fisher information } \mathcal{I}(\theta)=E[({\partial \over \partial \theta} log f(X;\theta))^2 \mid \theta]=-E[{\partial^2 \over \partial \theta^2} log f(X;\theta) \mid \theta]
$$

Normal distribution은 2차 미분 값이 항상 상수이므로 이 사실을 보여주기에 좋은 예제는 아니다. 따라서 조금 더 복잡한 예제로 바꾸어보자 

![9](https://user-images.githubusercontent.com/17296297/136698627-4973ba48-5595-431f-83cd-e2a2532d6d99.png)

*그림 9*    
<br/>

이제 2차 미분 값들의 분포가 좀 더 다양해졌다. 그림 10은 density function의 narrowness의 변화에 따른 거동을 보여준다. Narrowness가 증가하면 2차 미분 값들의 평균 음의 방향으로 커지는 것을 확인할 수 있고, 이는 곧 Fisher information의 증가를 의미한다. 

![10](https://user-images.githubusercontent.com/17296297/136659744-d2d1246d-7fbc-4650-a347-edd40aa6f0c8.gif)

*그림 10*    
<br/>

# 2-dimensional case
지금까지는 1차원 case에 대해 다루어보았다. 이를 좀 더 일반화하여 2차원 case에 대해 다루어보자. 같은 과정을 2d multivariate normal disribution에 대해 진행할 것이다. 이때 parameter는 두 평균으로 이루어진 벡터이고, covariance matrix는 알고 있다고 가정한다. 2차원은 시각화하기가 더 어려워 앞에서 보여준 6개의 그래프를 모두 보여줄 순 없다. 따라서 여기서는 
* (1) log likelihood curve들과 그 slope들     
* (4) 그 slope들의 분포    

두 개만 일반화하여 시각화한다. 

1차원 case에서는 x축이 1차원 parameter space 였지만 여기서는 2차원 parameter space가 필요하다. 따라서 x축, y축의 2차원 평면을 그리고 true parameter point를 표시해보자. 

![11](https://user-images.githubusercontent.com/17296297/136699645-e1d7ca42-99cb-40a1-ba07-b7339140529c.png)

*그림 11*    
<br/>

2차원에서의 log likelihood function은 contour lines로 표현할 수 있다. 

![12](https://user-images.githubusercontent.com/17296297/136700249-5a1f88d6-885d-4870-9711-3552998f76d6.png)

*그림 12*    
<br/>

다음으로, 2d version의 slope가 필요하다. Slope는 한 점에서 parameter value가 어떻게 변화는 지를 말해준다. 이는 gradient vector로 표시할 수 있다. Gradient vector는 함수 값을 가능한 크게 증가시키려면 어떤 방향을 선택해야, 얼만큼 증가하는지 알려준다. 

![13](https://user-images.githubusercontent.com/17296297/136700292-80817fad-a854-4cc7-949d-984ae773b374.png)

*그림 13*    
<br/>

앞서 우리는 하나가 아닌 여러 개의 likelihood function을 이용했다. 하나의 likelihood function은 true parameter로부터 sampling된 observation들 중 하나에 대응한다. 따라서 우리는 여러 개의 contour line들이 필요하다. 

그러나 contour line들을 모두 표현하면 매우 복잡하므로 각 likelihood function 당 하나의 contour line (타원) 만을 사용하여 간단하게 표현하고자 한다. 이렇게 여러 likelihood function을 2차원 평면에 표현하면 다음 그림과 같다. 

![14](https://user-images.githubusercontent.com/17296297/136700998-ba5e4209-b8ba-4b1f-8e7a-9cf8eecfb97d.png)

*그림 14*    
<br/>

1차원 case에서 Fisher information은 log likelihood slope들의 variance를 나타냈다. 좀 더 일반화하여 말하자면 그 slope들의 분포를 묘사한다. 마찬가지로 2차원 case에서는 gradient vector들의 분포를 묘사해야 한다. 이를 표현하기 위해 gradient vector들의 2d histogram을 그려보자. 

![15](https://user-images.githubusercontent.com/17296297/136701734-bb308592-a8f9-4c3e-ac78-64d97d98d9ab.png)

*그림 15*    
<br/>

* Grid의 색깔이 밝을 수록 해당 좌표를 가지는 gradient vector들이 많음을 의미한다. 

2d Fisher information이 무엇을 의미하건 간에 이 histogram을 묘사할 것이다. 2d Fisher information이 무엇인지 얘기하기 전에 한 가지를 짚고 넘어가고자 한다. 

그림 15의 왼쪽 그림을 보면 gradient들이 중심에서부터 랜덤한 방향으로 나오고 있고 어떤 한 방향을 가리키고 있지 않다. 1d case에서 우리는 이러한 현상을 기울기가 0으로 평균화 되는 것으로 보았다. 이러한 현상은 우리가 true parameter 값에서 gradient들을 평가하고 있기 때문이다. 만약 true parameter가 아닌 곳에서 평가를 한다면 이 gradient vector들은 어떤 한 방향을 더 가리키는 경향이 생기고 Fisher information을 적용할 수 없을 것이다. (Fisher information의 정의 자체가 true parameter에서 평가를 하는 것이다.)

![16](https://user-images.githubusercontent.com/17296297/136702814-b1358aac-bf3e-4850-8ec9-b2658879adf6.png)

*그림 16*    
<br/>

이제 다시 돌아와 Fisher infromation이 무엇을 해야하는지 생각해보자. 우리는 이 0에 중심이 있는 2d histogram을 묘사해야 한다. 1d case에서는 variance를 이용해 묘사하였다. 2d에서는 *covariance matrix*를 사용한다. 이 matrix는 한 분포에서 sampling된 vector의 원소들이 어떻게 함께 변하는지를 묘사한 square grid 형태의 숫자들이다. 

따라서 더 높은 차원의 Fisher information은 **gradient vector들의 covariance matrix**이다. 이를 **Fisher information matrix**라고 부른다. 

![17](https://user-images.githubusercontent.com/17296297/136703472-279d341d-df22-4eb4-a6b3-3901ba5590a2.png)

*그림 17*    
<br/>

Fisher information은 우리에게 위 그림에 표시한 타원을 알려준다. Fisher information이 어떻게 타원을 알려주는 지는 중요하지 않다 (eigenvector & eigenvalue). 그냥 이 타원을 histrogram의 간결한 표현이라고 생각하고 histogram에 집중해보자. 

먼저, 가장 간단한 case를 고려해보자. 가장 간단한 case는 gradient vector들이 서로 상관 관계가 없는 경우이다 (uncorrelated). 

![18](https://user-images.githubusercontent.com/17296297/136703730-f0ee468b-6126-4ec3-b729-ba753a8862c7.png)

*그림 18*    
<br/>

이 경우에는 각 gradient vector의 variance만 알면 된다 (covariance matrix의 대각 성분, 나머지는 다 0). 그리고 1d 경우와 거의 같은 방식으로 각각을 해석할 수 있다. 만약 그 gradient vector 요소의 변화가 크다면 쉽게 true parameter를 찾을 수 있다. 

이제 매우 큰 correlation을 갖는 경우를 생각해보자. 

![19](https://user-images.githubusercontent.com/17296297/136704933-e6b9ccf4-eeb7-4b13-a4c6-6f260d1161e1.png)

*그림 19*    

그림 19에서 빨간색 선 위에 있는 parameter들만 고려했을 때 true parameter를 찾기 쉬울까? 

대답은 '그렇다' 이다. 빨간색 선만 본다면 observation들이 좁은 영역에 모여있기 때문이다. 이는 narrowness가 큰다는 의미이다. 이것은 우연이 아니다. 이 빨간색 선의 방향은 histogram의 변화가 큰 방향과 동일하다. 다르게 말하자면, gradient들의 변화가 매우 큰 방향이 true parameter를 더 쉽게 찾을 수 있는 방향이다. 이는 1d case에서 기울기들의 변화, 즉 분산이 더 클 때 true parameter를 찾기 쉬웠던 것과 일맥상통하다. 
* gradient 변화가 큰 방향 = histogram 변화가 큰 방향 = narrowness가 큰 방향 = true parameter를 더 쉽게 찾을 수 있는 방향


![20](https://user-images.githubusercontent.com/17296297/136704936-a15df5d7-1bd6-489f-a91b-f6dafeca1d0f.png)

*그림 20*    

예상한 것과 같이 그림 20의 빨간색 선 방향으로는 true parameter를 찾기가 힘들다. Observation들이 넓은 영역에 분포해 있고, gradient들의 변화가 크지 않다. 

정리하자면 Fisher information은 주어진 방향을 따라 observation들이 true parameter를 얼마나 잘 구분할 수 있는지를 말해준다. 그렇게 하기 위해 true parameter에서의 log likelihood function의 기울기들의 분포를 사용한다. 

![21](https://user-images.githubusercontent.com/17296297/136705074-42b3fc3d-083f-4394-967f-c53a2a3a11ac.gif)

2차 미분에 경우도 마찬가지이다. 이 경우 2차 미분을 hessian matrix로 나타낼 수 있고, negative expected hessian 값이 gradient의 covariance matrix와 동일한 값을 갖는다. 이 둘은 Fisher information을 나타낸다. 

# Summary

![21](https://user-images.githubusercontent.com/17296297/136706767-c2d978cc-9401-4210-84d2-aa18a6ac5f42.png)


# In the view of natural gradient
## natural gradient 관점에서 Fisher information 정리하기

https://julien-vitay.net/deeprl/NaturalGradient.html 

https://www.andrew.cmu.edu/course/10-703/slides/Lecture_NaturalPolicyGradientsTRPOPPO.pdf

Deep RL에서, old policy와 update된 new policy 간의 차이가 크면 policy gradient 값을 추정할 때 old policy로 추정된 Q값들을 사용하기가 어려워 진다. 따라서 actor(policy)는 critic보다 훨씬 느리게 변해야 한다. 

이러한 문제를 해결하기 위해서는 steepest descent와 반대로 policy의 가장 작지만 옳바른 방향으로 변화를 만드는 가장 큰 parameter 변화 $\Delta\theta$를 찾아야 한다. Update 전후 policy의 변화가 크지 않으면 우리는 이전 policy의 Q값들을 활용하는 식으로 과거의 경험을 재사용할 수 있다. 

여기서 말하는 policy는 (action의) distribution을 의미한다. 단순히 loss를 줄이는 방향으로 parameter를 변화시키는 것 뿐만 아니라 두 distribution 간의 변화도 커지지 않도록 신경써야 한다. 결론적으로, 우리는 parameter 변화와 policy 변화 간의 관계에 주목해야 한다. 이를 위해 parameter space 자체에서가 아닌 parameter들에 의해 정의된 distribution의 statistical manifold 상에서 gradient descent를 적용해야 한다. 

어떤 parameterized distribution $p(x;\theta)$를 생각해보자. 어떤 작은 parameter 변화 $\Delta \theta$가 발생하면 $p(x;\theta+\Delta \theta)$가 된다. 그림에서와 같이 parameter space에서의 Euclidean metric $\parallel \theta + \Delta \theta - \theta \parallel^2$은 statistical manifold의 구조를 고려하지 못한다. 

우리는 부분적으로 $\theta$와 $\theta+\Delta \theta$사이의 manifold의 curvature를 고려하는 **Riemannian metric**을 정의할 필요가 있다. Rimannian distance는 dot product로 정의된다:
$$
  \parallel \Delta \theta \parallel^2 = <\Delta\theta,F(\theta),\Delta\theta>
$$
여기서 $F(\theta)$는 Riemannian metric tensor라고 불리고 point $\theta$에서 manifold의 tangent space에서의 inner product이다. 

두 distribution 사이의 거리를 측정하기 위해 symmetric KL divergence (Jensen-Shannon (JS) divergence)를 사용할 때 대응하는 Riemannian metric이 **Fisher Information Matrix** (FIM)이다. $\theta$ 근처에서 KL divergence의 Hessian matrix로 정의된다. 이는 manifold가 $\theta$ 주변에서 국소적으로 어떻게 변화는지를 나타낸다:
$$
  F(\theta)=\nabla^2D_{JS}(p(x;\theta)\mid\mid p(x;\theta + \Delta\theta))\mid_{\Delta\theta=0}
$$
이는 2차 미분에 대한 계산이 필요하고 매우 복잡하고 느리다 (특히 파라미터 수가 많을 때). 다행히도, 좀 더 간단한 버전이 존재한다. log-likelihood의 gradient들 간의 outer product에만 의존한다:
$$
  F(\theta)=\mathbb{E}_{x\sim p(x,\theta)}[\nabla log p(x;\theta)(\nabla log p(x;\theta))^T]
$$

Fisher information matrix가 유용한 이유 중 하나는 가까운 두 distribution 사이의 KL divergence를 국소적으로(locally) 근사 가능하게 해주기 때문이다. 
$$
  D_{JS}(p(x;\theta)\mid\mid p(x;\theta + \Delta\theta)) \approx \Delta\theta^TF(\theta) \Delta\theta
$$

그러면 KL divergence는 국소적으로 quadratic하다. 이는 KL divergence를 gradient descent로 최소화 할 때 얻는 update rule이 linear 하다는 의미이다. 

우리가 $\theta$로 매개변수화된 loss function L을 distribution p를 고려하여 최소화하기 원한다고 가정해보자. **Natural gradient descent**는 KL divergence surface의 local curvature를 사용하여 $L(\theta)$의 gradient를 교정함으로써 p에 의해 정의된 statistical manifold를 따라 움직이려고 한다. 이는 $\tilde{\nabla}_\theta L(\theta)$ 방향을 따라 주어진 거리를 움직이는 것과 같다:
$$
  \tilde{\nabla}_\theta L(\theta)=F(\theta)^{-1}\nabla_\theta L(\theta)
$$

$\tilde{\nabla}_\theta L(\theta)$가 바로 $L(\theta)$의 **natural gradient**이다. Natural gradient descent는 단순히 이 방향으로 step을 진행하는 것이다:
$$
  \Delta \theta = - \eta \tilde{\nabla}_\theta L(\theta)
$$

Manifold가 curve가 아닌 경우,
* $F(\theta) = I$ (identity matrix)
* Natural gradient descent = regular gradient descent

## Advantage
Regular gradient descent의 문제점은 fixed learning rate에 의존한다는 점이다. Loss function이 평평한 부분 (= gradient가 거의 0인 부분)에서는 improvement가 매우 느리다. 반면, natural gradient는 curvature (Fisher)의 역을 고려하기 때문에 평평한 지역에서 gradient의 크기가 더 커져 더 큰 step으로 이어진다. 따라서 regular GD보다 더 빠르게 잘 수렴한다. 

## Drawback
Natural gradient descent의 단점은 Fisher information matrix의 inverse가 필요하다는 점이다. 이 matrix의 크기는 free parameter의 수에 따라 변한다. N개의 parameter가 존재할 경우 N x N matrix의 inverse가 필요하다. 이를 극복하기 위해 Conjugate Gradients 또는 Kronecker-Factored Approximate Curvature (K-FAC)와 같은 근사 방법을 사용한다. 
# In paper

## Natural Gradient Works Efficiently in Learning (1998)

* "The Riemannian structure of the parameter space of a statistical model is defined by the Fisher information (Rao, 1945; Amari, 1985)."
* "This is the only invariant metric to be given to the statistical model (Chentsov, 1972; Campbell, 1985; Amari, 1985)."

## Natural Policy Gradient (2001)

* "As shown by Amari (see [1]), the Fisher information matrix, up to a scale, is an invariant metric on the space of the parameters of probability distributions. It is invariant in the sense that it defines the same distance between two points regardless of the choice of coordinates (ie the param- eterization) used, unlike G = I."

## TRPO

TRPO 논문에서 constrained optimization problem을 푸는 과정은 두 가지 단계로 나누어진다. 첫 번째 단계는 search direction을 찾는 과정이고, 두 번째 단계는 그 방향을 따라 line search를 수행하는 과정이다. 즉, 움직일 방향을 찾고 얼마나 움직일지를 결정하는 것이다.

첫 번째 방향을 찾을 때, objective function의 linear approximation과 constraint의 quadratic approximation을 이용한다. 이때 constarint가 KL divergence이고, 위에서 언급한 것처럼 quadratic approximation이 가능하다. 근사화 후 Lagrangian multiplier를 적용하여 만든 unconstrained objective function은 $\Delta \theta$에 대한 이차식이다. 따라서 unique maximum을 갖고 미분하여 0이 되는 지점이 해가 된다:
$$
  \Delta \theta = {1\over\lambda}F(\theta_{old})^{-1}\nabla_\theta J_{\theta_{old}}(\theta)
$$
위 결과는 **natural gradient descent**와 동일하다! 즉, TRPO에서는 natural gradient descent를 사용한다. (Natural policy gradient 논문과 동일)
> TRPO = natural policy gradient + linesearch + monotomic improvement theorem 

한 가지 문제점은 Fisher information matrix의 inverse를 구하는 것인데, 여기서도 conjugate gradient algorithm을 이용하여 근사한다. 


# 수학적 증명

https://agustinus.kristia.de/techblog/2018/03/11/fisher-information/

Fisher Information과 KL-divergence 간의 관계 https://agustinus.kristia.de/techblog/2018/03/14/natural-gradient/

https://en.wikipedia.org/wiki/Fisher_information

2nd order Taylor expansion of KL divergence http://people.eecs.berkeley.edu/~pabbeel/cs287-fa09/lecture-notes/lecture20-2pp.pdf page6


# Reference
[1] [The Fisher Information - Mutual Information Youtube channel](https://www.youtube.com/watch?v=pneluWj-U-o)

[2] [Fisher Information / Expected Information: Definition - Statistics How To](https://www.statisticshowto.com/fisher-information/)

[3] [Fisher information - Wikipedia](https://en.wikipedia.org/wiki/Fisher_information)

[4] [Fisher Information Matrix - Agustinus Kristiadi's Blog](https://agustinus.kristia.de/techblog/2018/03/11/fisher-information/)

[5] [Ly, A., Marsman, M., Verhagen, J., Grasman, R. P., & Wagenmakers, E. J. (2017). A tutorial on Fisher information. Journal of Mathematical Psychology, 80, 40-55.](https://arxiv.org/pdf/1705.01064.pdf)