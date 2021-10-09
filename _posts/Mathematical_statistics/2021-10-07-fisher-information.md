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
last_modified_at: 2021-10-07
sitemap :
  changefreq : daily
  priority : 1.0
---

TRPO를 공부하다보니 Fisher information matrix라는 개념이 등장했는데 처음 접했던 개념이라 생소했다. 여러 자료들을 통해서 어떤 의미를 가지고 있는지, 논문에서는 어떤 흐름에서 등장했는지를 한번 정리해보고자 한다. 

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

(1) $\mu$값에 따른 $log\mathcal{N}(y_i|\mu,\sigma^2)$ 그래프   
(2) $\mu_0$에서 각 observation $y_i$에 대응하는 $log\mathcal{N}(y_i|\mu,\sigma^2)$의 분포       
(3) $log\mathcal{N}(y_i|\mu,\sigma^2)$를 $\mu$에 대해 미분한 값 (=(1)에서의 기울기) 그래프 = score function    
(4) $\mu_0$에서 각 observation $y_i$에 대응하는 ${\partial \over \partial \mu}log\mathcal{N}(y_i|\mu,\sigma^2)$의 분포  

일반적으로 log likelihood를 parameter로 미분한 값을 *score function*이라 부른다. (3)의 경우, socre functiion이 linear 함수이다. 

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
* Score value가 0 근처에서는 각각의 observation들이 개별적으로 true parameter가 어디 있는지 광범위하게 추천하지만, 집합적으로는 움직이지 말라고 한다. 



![2](https://user-images.githubusercontent.com/17296297/136659744-d2d1246d-7fbc-4650-a347-edd40aa6f0c8.gif)

# In the paper

TRPO 논문에서 constrained optimization problem을 푸는 과정은 두 가지 단계로 나누어진다. 

첫 번째 단계는 search direction을 찾는 과정이고, 두 번째 단계는 그 방향을 따라 line search를 수행하는 과정이다. 


에서 KL divergence constraint를 quadratic 



# Reference
[1] [The Fisher Information - Mutual Information Youtube channel](https://www.youtube.com/watch?v=pneluWj-U-o)

[2] [Fisher Information / Expected Information: Definition - Statistics How To](https://www.statisticshowto.com/fisher-information/)

[3] [Fisher information - Wikipedia](https://en.wikipedia.org/wiki/Fisher_information)

[4] [Fisher Information Matrix - Agustinus Kristiadi's Blog](https://agustinus.kristia.de/techblog/2018/03/11/fisher-information/)

[5] [Ly, A., Marsman, M., Verhagen, J., Grasman, R. P., & Wagenmakers, E. J. (2017). A tutorial on Fisher information. Journal of Mathematical Psychology, 80, 40-55.](https://arxiv.org/pdf/1705.01064.pdf)