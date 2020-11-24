---
layout: post
title: ReMixMatch &#58; Semi-Supervised Learning with Distribution Alignment and Augmentation Anchoring 논문요약
date: '2020-11-25 00:50:00'
description: # Add post description (optional)
img: remixmatch.png # Add image post (optional)
tags: [Papers]
author: # Add name author (optional)
---


이미지 출처:
http://dsba.korea.ac.kr/seminar/?mod=document&uid=248

https://amitness.com/2020/07/semi-supervised-learning/


----------------------------------- 



## MixMatch

![image](https://user-images.githubusercontent.com/17904547/100116756-744c9180-2eb7-11eb-8cbf-2a33679c39c4.png)

![image](https://user-images.githubusercontent.com/17904547/100117034-c1c8fe80-2eb7-11eb-8593-c6cc73ed7e02.png)


- unlabeled data에 대해 각각 K 번의 standard augmentation을진행
- K개의 prediction distribution 평균내기 & sharpening → pseudo label
- 모든 데이터(labeled & unlabeled)를 섞은 후 labeled group과 unlabeled group을 나눔
- 각 그룹의 데이터와 레이블에 대해 MixUp을 진행
- MixUp 데이터에 대한 prediction output과 MixUp 레이블간의 loss를 계산
- 모든 로스를 더해줌.

​

​

## ReMixMatch

- MixMatch에 두 가지 요소를 추가함

1) **Distribution alignment**

2) **Augmentation anchoring**

​

​

### Distribution alignment

<img src="https://user-images.githubusercontent.com/17904547/100117207-e7560800-2eb7-11eb-96c7-b8b96f491f13.png" width=500>

- unlabeled data의 prediction distribution을 labeled data의 distribution에 맞춰서 조정하는 것.


![image](https://user-images.githubusercontent.com/17904547/100117229-ed4be900-2eb7-11eb-8a1a-1b1c88bf54a7.png)

- Bridle et.al (1992) : unlabeled data에 대해서 모델의 input과 output은 상호의존적이여야함.
- 상호의존도 I(y;x)를 최대화 시키는 방향으로 모델을 학습시켜야함.
- Minimize E_x(H(~~)):
     - Entropy Minimization → Sharpening in MixMatch
- Maximize H(E_x(~~)) :
     - Entropy ↑ : 분포가 평평해야함
- 즉, 모든 training data에 대해, 모델이 각 class에 대해 Equal frequency를 가질 때 Entropy가 최대가 된다.


※ 그러나, 실제 클래스에 대한 분포가 동일한 경우가 아니라면 학습에 안 좋은 영향을 끼침
☞ 개선책 : **Distribution Alignment**



![image](https://user-images.githubusercontent.com/17904547/100117347-0c4a7b00-2eb8-11eb-89ca-603808e77ede.png)

- q : unlabeled data의 prediction distribution
- p(y) : labeled data의 class 분포의 average
- tilde_p(y) : q의 moving average
unlabeled data가 labeled data의 분포를 따라가도록 조정시킴.

​

​

### Augmentation Anchoring

​

- AutoAugment : best accuracy를 가지는 최적의 (strong) augmentation을 찾는 강화학습 기반의 파이썬 라이브러리

   - 문제점1 : 성능은 올라갔지만 training 수렴X
   → sol) weakly aug. image의 prediction을 strongly aug. images의 guessed label로 사용하자

  - 문제점2 : 강화학습은 시간과 자원이 많이듬. labeled data가 어느정도 필요함.
   → sol) CTAugment(Control Theory Augment)

#### CTAugment:

<img src="https://user-images.githubusercontent.com/17904547/100117565-4ae03580-2eb8-11eb-8026-e8a97c7e89e2.png" width=400>

- magnitude val를 bin으로 나누어주고, 각 bin에게 가중치 부여.
- two transformations를 랜덤하게 선택 후, 각 transformation에 대해 가중치가 부여된 magnitude bin을 랜덤하게 선택.
- 가중치를 갱신하기 위해, **labeled data**를 두 transformation에 통과시킴.
- label의 정보를 훼손하지 않는 augmentation을 생성하는 가능성을 학습.
- 예측결과와 실제 label(One-hot vector)을 비교하여, bin weight를 갱신시킴.

![image](https://user-images.githubusercontent.com/17904547/100118656-64ce4800-2eb9-11eb-97b1-2ee2bfef29c9.png)

#### Augmentation Anchoring :

- unlabeled data에 대해 strong/weak augmentation을 적용
- weakly augmented image의 predictions가 여러개의 strong augmented images의 guessed label이 됨.

​
![image](https://user-images.githubusercontent.com/17904547/100118697-70ba0a00-2eb9-11eb-8b06-1c452b65f146.png)


​


​

## Loss Function

### MixMatch Loss

![image](https://user-images.githubusercontent.com/17904547/100118724-79124500-2eb9-11eb-9a31-82458049b799.png)


### ReMixMatch Loss

![image](https://user-images.githubusercontent.com/17904547/100118755-7ca5cc00-2eb9-11eb-8d2c-650913d3b68e.png)

- labeled group과 unlabeled group 모두 Cross Entropy loss 사용
- Pre-mixup unlabeled loss : MixUp 이전의 데이터 u1에 대해서 cross entropy loss 계산
- Rotation Loss: {0,90,180,270} 중 하나의 각도로 회전시킨 뒤 회전된 각도를 예측한 결과에 대한 loss

​

​

​

## Results

![image](https://user-images.githubusercontent.com/17904547/100118828-8b8c7e80-2eb9-11eb-8d21-48860e61e0a1.png)

​ 
