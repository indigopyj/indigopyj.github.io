---
layout: post
title: 확률과 정보이론 개념정리
date: '2020-11-27 16:30:00'
description: # Add post description (optional)
img: default.jpg # Add image post (optional)
tags: [Study]
author: # Add name author (optional)
use_math: true
---


내용 출처 : <https://www.deeplearningbook.org> Chapter 3. Probability and Information Theory

GAN을 공부하는데 모르는 통계용어들이 자꾸 튀어나와서 결국 통계를 따로 공부해놓기로 마음먹었다.

자료를 제공해준 윤쓰에게 감사의 말을 전합니다.


-----------------------------------------


# Probability

## Frequentist probability(도수 확률) : 

포커 게임에서 특정한 패를 뽑는 사건은 반복적으로(repeatable) 일어난다.

"그 사건이 일어날 확률은 p 이다"
== experiment를 무한 번 반복한다면 결과적으로 p의 비율로 그 사건이 일어날 수 있다는 뜻이다.

단 여기서 이와같은 추론은 **반복한다**는 전제에서만 적용할 수 있다.

정리하자면 **frequentist probability는 어떤 사건이 일어날 비율/빈도**를 말한다.


## Bayesian probability(베이즈 확률):

한 의사가 말하길, 어떤 환자가 감기에 걸릴 확률은 40%다.

근데 여기서는 의사는 무한 명의 환자를 볼수가 없고, 다양한 조건 하에 서로 다른 환자들이 모두 같은 증상을 나타낸다고 할 수도 없다.

그래서 여기서의 확률은 **degree of belief**를 표현한다고 할 수 있다.

즉 1은 환자가 감기에 걸릴 절대적인 확실성, 0은 감기에 걸려있지않을 절대적 확실성이다.

정리하자면 **Bayesian probability는 확실성(certainty)의 정량적인 등급**을 말한다.



# Random Variable

- A **random variable** is a variable that can take on different values randomly.

- Notation : 
  - random variable x가 취할 수 있는 값으로는 $x_1$, $x_2$,.....가 있다.
  - random variable vector x의 value는 $x$라고 표현한다.

- Random variable은 discrete/continuous할 수 있다.
  - discrete random variable은 유한하거나, 셀 수 있는 무한한 개수의 상태들 중 하나이다. 이것은 정수일 필요는 없고 numerical value로 표현할 수 없어도 되는 'state' 형태이면 된다.
  - continuous random variable은 실수 형태이다.


# Probability Distributions:

- **난수 한개나 난수 집합의 각각의 원소가 가능한 states를 취할 수 있을지를 나타낸 것.**

## Discrete variables

### Probability Mass Functions(PMF) : discrete variable에 대한 확률분포

- PMF는 한 random variabled의 state를 probability로 맵핑시켜준다.

- 즉 P에서 x= $x$일때는 $P(x)$ 라 하는데, 이때 x= $x$가 확실하다면 probability는 1이다. 반면에 x= $x$가 불가능하다면 probabilit는 0이다.

- PMF는 동시에 여러 변수에 의해서 동작할 수도 있는데 그것을 **joint probability distribution**이라고 한다.
  - ex. $P(x,y)$

- P가 아래조건을 만족해야만 PMF가 될수있다.
  - P의 도메인은 모든 x에 대한 집합이다.
  - $\forall x \in X, 0\leq  P(x)\leq 1$
  - $\sum _{x\in X} P(x) = 1$

### Uniform distribution : k개의 다른 state에 대해 random variable x가 존재할 때, 모든 i에 대해 아래 식을 만족한다.


\\[P(x_i) = \frac{1}{k}\\]


## Continuous Variables

- Probability Density Functions(PDF): continuous variable에 대한 확률분포

- p가 아래조건을 만족해야만 PDF가 될 수 있다.
  - p의 도메인은 모든 x에 대한 집합이다.
  - $\forall x \in X, p(x) \geq 0$
  - $\int p(x)dx = 1$


## Marginal Probability

- **어떤 부분집합에 대한 확률분포**

### Sum rule
- 어떤 random variable x,y가 있고, P(x,y)를 알고있을때 P(x)를 아래와 같이 구할 수 있음.

\\[\forall x \in X, P(x) = \sum _{y} P(x,y)\\]

\\[p(x) = \int p(x,y)dy\\]



# Conditional Probability(조건부 확률)

- **어떤 사건들이 일어나고 있을 때, 한 사건이 일어날 확률**


\\[P(y \vert x) = \frac{P(y,x)}{P(x)}\\]


단, $P(x) > 0$


# The chain rule of conditional probabilities

<img src="https://user-images.githubusercontent.com/17904547/100418961-ef37c700-30c6-11eb-8c64-98707d01c340.png" height=50>

여러변수에 대한 어떤 joint probability distribution은 한 변수에 대한 조건부확률로 분해될 수 있다.
이것을 **chain rule**, or **product rule** 이라고 한다.


# Independence and conditional independence

- 아래와 같이 표현될 수 있을 떄 두 랜덤변수 x,y는 **독립적(independent)**이다. 
  - notation : $x \perp y$

<img width="500" alt="스크린샷 2020-11-27 오후 3 53 33" src="https://user-images.githubusercontent.com/17904547/100419853-b698ed00-30c8-11eb-8572-6ca39d20df01.png">

​
- 랜덤변수 z에 대해서 두 랜덤변수 x,y는 **조건부 독립적(conditional independent)**이다.
  - notation : $x \perp y \vert z$

<img width="741" alt="스크린샷 2020-11-27 오후 3 56 48" src="https://user-images.githubusercontent.com/17904547/100420097-2a3afa00-30c9-11eb-96e1-1aa051e06b81.png">



# Expectation, variance and covariance

## **Expectation** : f(x)에 대한 기댓값, 평균값, mean, average 

이산확률분포에서는 

<img width="265" alt="스크린샷 2020-11-27 오후 4 05 42" src="https://user-images.githubusercontent.com/17904547/100420798-69b61600-30ca-11eb-9a16-31756f2bf9b4.png">

연속확률분포에서는

<img width="270" alt="스크린샷 2020-11-27 오후 4 09 09" src="https://user-images.githubusercontent.com/17904547/100421002-e47f3100-30ca-11eb-8a49-a939d32af243.png">

기대값은 linear한 성질이 있어서 아래와 같이 전개가 가능하다.

<img width="413" alt="스크린샷 2020-11-27 오후 4 15 18" src="https://user-images.githubusercontent.com/17904547/100421453-c1a14c80-30cb-11eb-9f15-a7edaab847fd.png">


## **variance(분산)** : 랜덤변수 x가 같은 확률분포에서 샘플링한 다른 x의 함수값과 얼마나 다른지를 나타내는 척도
  - variance이 작으면 f(x)의 값들은 평균쪽에 몰려있다.
  - **variance의 제곱근 = standard deviation(표준편차)**

<img width="300" alt="스크린샷 2020-12-02 오후 2 16 18" src="https://user-images.githubusercontent.com/17904547/100831346-f38a2880-34a8-11eb-9616-2078813d78ec.png">

## **covariance(공분산)** : 서로 다른 두 값이 얼마나 선형적으로 연관이 있는지와 두 값의 스케일을 나타내는 척도.
  - 공분산의 절대값이 크다는 것은 두 값이 매우 다르고 두 값은 각자의 평균으로부터 매우 떨어져있음을 의미함.
  - 공분산이 양수이면, 두 비교값이 둘다 매우 큰 값임을 추측할 수 있음.
  - 공분산이 음수이면, 한 값은 매우 작고 다른 한 값은 매우 큰 값임.
  - **correlation** : 스케일에 관계없이 오직 "얼마나 두 값이 연관이 되어있는지"만 보기위해 normalize를 하는 것.

<img width="400" alt="스크린샷 2020-12-02 오후 2 20 01" src="https://user-images.githubusercontent.com/17904547/100831604-78754200-34a9-11eb-80be-b4f258a03f29.png">


## Dependence와 Covariance

- 두 변수가 독립이면 공분산은 0이다.
- 두 변수의 공분산이 0이 아니면 두 변수는 의존적이다.
- 두 변수가 공분산이 0이면, _반드시_ 두 변수사이에는 linear dependence가 존재하지 않는다.
  - 즉 Independence가 zero-covariance보다 더 강한 조건임. 그 이유는 독립성은 nonlinear relationship도 제외하기 때문.
  - 즉 dependent한 두 변수가 zero-covariance일 수 있다.
- **covariance matrix** : x가 n차원의 벡터일때, $Cov(x)_{i,j} = Cov(x_{i}, x_{j})$ 을 만족하는 n x n 행렬이다.
- covariance magrix의 대각선원소는 variance와 같다.

# Common Probability Distributions

## Bernoulli Distribution

- single binary random variable에 대한 분포
- single parameter $\phi \in [0,1]$에 의해 제어됨.
- 아래와 같은 성질이 있음.

<p align="center"><img width="279" alt="스크린샷 2020-12-02 오후 3 11 58" src="https://user-images.githubusercontent.com/17904547/100835288-ba55b680-34b0-11eb-83f7-9ef0aa48c0d6.png"></p>

## Multinoulli Distribution

- **multinoulli or categorical distribution**는 유한한 k에 대해서, k개의 다른 state에 대한 분포이다.
- 파라미터 벡터 $p \in [0,1]^{k-1}$에 의해 제어됨. 이때 $p_{i}$는 i번째 state에 대한 확률을 말한다.
- k번째 state의 확률은 $1 - 1^{\top }p$이다. (단, $1^{\top }p \leq 1$)

## Gaussian Distribution( = Normal distribution)

<p align="center"><img width="350" alt="스크린샷 2020-12-02 오후 3 30 18" src="https://user-images.githubusercontent.com/17904547/100836800-4a94fb00-34b3-11eb-9e9c-67f562eb1d33.png"></p>

- precision : variance의 역수 $1^{\top }p \leq 1$.  PDF evaluation을 여러번할때 사용됨.
  precision으로 나타낸 가우시안 분포
<p align="center"><img width="414" alt="스크린샷 2020-12-02 오후 3 50 53" src="https://user-images.githubusercontent.com/17904547/100838649-2ab30680-34b6-11eb-92bc-7286295cd805.png"></p>

<img width="500" alt="스크린샷 2020-12-02 오후 3 52 33" src="https://user-images.githubusercontent.com/17904547/100838767-65b53a00-34b6-11eb-862f-090daefb03f4.png">

- 일반적으로 이와같은 종모양의 형태의 분포를 가지고, 중심은 𝞵이고 너비는 𝞼에 의해 결정된다.
- 어떤 실수에 대한 분포를 알아내고 싶을때, 디폴트로 가우시안 분포를 선택하는 것이 좋다.
  - 일반적으로 많은 분포들이 가우시안분포와 비슷하다. central limit theorem은 많은 독립랜덤변수들의 합이 대략적으로 normal distributed라는 것을 보였다. 
  - 같은 분산을 가지는 모든 가능한 분포들 중에서, normal distribution은 최대의 불확실성을 인코딩한다(?). 그 말은 즉슨, 우리가 어떤 모델링을 할때 normal distribution에다가는 최소한의 정보만 주입해주면 된다는 것이다.

- $\mathbb{R}^{n}$에 대해서 일반화된 가우시안 분포는 **multivariate normal distribution**이라고 한다. 이는 양의 유한대칭행렬(positive definite symmetric matriz) 𝞢에 대해 통제된다. 𝞢는 분포의 공분산행렬이다. 이도 마찬가지로 precision matrix 𝞫 에 대해서 표현할 수 있다.

<p align="center"><img width="550" alt="스크린샷 2020-12-02 오후 4 07 08" src="https://user-images.githubusercontent.com/17904547/100839951-71096500-34b8-11eb-95bb-d99665290c3f.png"></p>

<p align="center"><img width="550" alt="스크린샷 2020-12-02 오후 4 10 34" src="https://user-images.githubusercontent.com/17904547/100840218-eaa15300-34b8-11eb-93a2-9654cbd4265f.png"></p>


## Exponential and Laplace Distributions


### Exponential distribution
- 어떤 확률분포가 x=0에서 sharp point(첨점)을 가지도록 하고 싶을때 다음과 같이 나타낼 수 있다.
<p align="center">
<img width="274" alt="스크린샷 2020-12-02 오후 4 20 45" src="https://user-images.githubusercontent.com/17904547/100841222-56d08680-34ba-11eb-8163-16152f8a74d0.png">
</p>

### Laplace distribution
- 임의의 점 𝞵에서 sharp peak point를 가지게 하고 싶을 때 다음과 같이 나타낼 수 있다.

<p align="center">
<img width="388" alt="스크린샷 2020-12-02 오후 4 21 11" src="https://user-images.githubusercontent.com/17904547/100841269-65b73900-34ba-11eb-8ddf-5370df01bde4.png">
</p>

�
