---
layout: post
title: YOLACT: Real-time Instance Segmentation 논문요약
date: '2020-11-23 15:15:00 +0900'
description: # Add post description (optional)
img: YOLACT.png # Add image post (optional)
tags: [Papers]
author: # Add name author (optional)
---

## Introduction

- 2-stage detector(ex. Mark R-CNN) :

  - localization(RPN) -> 각 영역에 대해 classification & segmentation

- 1-stage detector(ex. FCIS):

  - Classification-localization을 병렬적으로 동시 수행.

     => 문제점: 순차적으로 가속화하기가 어렵고 localization 후 많은 양의 post-processing이 필요하기 때문에 실시간 작동이 어려움

- **YOLACT: fast & high quality masks**

Instance segmentation을 두 가지 task로 나누고 병렬적으로 수행

    1) 이미지 전체에 대해 prototype mask를 생성함

    2) 각 instance에 대한 mask coefficient를 예측함

그리고 각 instance의 예측된 coefficients와 prototype을 합쳐서 full-image instance segmentation 수행.
-> _Fully convolutional network가 instance mask의 localization을 학습함._


## YOLACT Architecture
![image](https://user-images.githubusercontent.com/17904547/99934283-ea120980-2da0-11eb-9a9a-ca86b424509e.png)

### Backbone
ResNet101 + FPN


### Protonet :
Prototype을 생성함.
![image](https://user-images.githubusercontent.com/17904547/99934154-8a1b6300-2da0-11eb-86d1-23225888fc60.png)

  - Input : The deepest feature map P3, C3부터 C5까지의 feature map이 모두 반영됨.
  - 3x3 conv, 마지막에는 k channels->K개의 prototype mask생성.
  - k <= 32 까지가 적절함. K가 너무 크면 coefficient를 예측하는 것이 어려워지고, 한 coefficients에서 large error 발생 시 mask가 사라지거나 leakage발생 가능.
  - Loss를 명확하게 나타내지는 않음->확실한 것(ex. Background)에 대해서 과하게 표현할 수 있도록 하기 위해.

![image](https://user-images.githubusercontent.com/17904547/99934215-b636e400-2da0-11eb-815b-448f784caa2c.png)
예시) k=6
- 1~3: boundary의 한 쪽에 있는 객체를 activate
- 4: 객체 왼쪽 하단에서 activate
- 5 : 배경과 객체의 boundary activate
- 6: 땅 부분 activate




### Prediction Head : Mask coefficient를 생성함.
- Three branches: class confidence(c) + bbox regression(4) + coefficients(k)
- applying tanh to the k mask coefficients-> more stable outputs


### Mask Assembly

<img src="https://user-images.githubusercontent.com/17904547/99934334-13329a00-2da1-11eb-8493-0d82000d8111.png" width=300>

P : prototype masks, C: mask coefficients

### Loss

<p align="center"><img src="https://user-images.githubusercontent.com/17904547/99934347-1c236b80-2da1-11eb-9a82-02c1974a8519.png" width=400>
<img src="https://user-images.githubusercontent.com/17904547/99934373-25acd380-2da1-11eb-84d1-f450ce75aed7.png" width=300></p>

#### ※ How to learn localizing instances?
- Padding -> 한 픽셀이 얼마나 이미지의 가장자리로부터 떨어져있는지를 나타나게 할 수 있음.
ex) 중앙에 위치한 픽셀은 padding value가 convolution 연산에 포함되지않아 output value가 비교적 큼.

![image](https://user-images.githubusercontent.com/17904547/99934407-36f5e000-2da1-11eb-88c9-a7ddb86d8193.png)

- pad=1과 kernel [1,0]을 사용하였을 때 activation map의 오른쪽부분이 활성화되는 것을 볼 수 있음.
  => padding을 이용하여 특정 지역만 activate시키는 것이 가능함.
- 특정 부분이 activate된 mask를 총 k개 생성함.


### Fast NMS

**기존 NMS :**

   1. 각 class에 대해 confidence기준으로 내림차순 정렬
   2. 그중에서 confidence가 가장 높은 anchor box부터 for문돌면서 전체 anchor box들과 IOU를 비교
   3. IOU가 threshold보다 크다면 둘 중 confidence가 더 낮은 박스를 제거  

**Fast NMS:**
- 병렬적으로 연산이 이루어짐.
- 이미 지워진 anchor box가 또 다른 anchor box를 지우기 위해서 비교를 계속함.
- 행렬연산이라 GPU에 최적(기존 NMS보다 15.0 ms faster)
- 문제점: box가 너무 많이 제거될 수도 있음.


## Results

![image](https://user-images.githubusercontent.com/17904547/99934583-baafcc80-2da1-11eb-824d-e94422a729e1.png)
![image](https://user-images.githubusercontent.com/17904547/99934595-c26f7100-2da1-11eb-8aae-f4952f2217ae.png)

- mask quality가 다른 모델과 비교했을 때 훨씬 좋음.

![image](https://user-images.githubusercontent.com/17904547/99934604-c9967f00-2da1-11eb-8722-0b6733b03c29.png)
- 다른 모델과 비교하였을 때 FPS가 월등히 크고 빠르다는 것을 보여주고 있음.
- image size가 클수록 FPS는 감소하고, AP는 증가하는 것을 보여줌.


![image](https://user-images.githubusercontent.com/17904547/99934632-dadf8b80-2da1-11eb-8d5f-fc0d6b611c05.png)

- Fast NMS를 사용했을 때, FPS는 증가하고 AP는 조금 감소하는 것을 보임.
- k가 64이상 일때 AP가 감소하고 있음. FPS와 함께 고려했을 때 k=32일때가 최적임.
- speed-accuracy의 trade-off가 다른 모델에 비해 비교적 적은 편임.

## Discussion


### 문제점
- Localization Failure : 한 곳에 많은 객체가 몰려있는 경우 prototype에서 모든 객체를 localize하지 못할 수 있음.
- Leakage : YOLACT는 cropped region에 대해 노이즈를 제거하지 않기 때문에 bbox가 정확하지 않을 경우 노이즈가 mask에 포함될 수 있음.


