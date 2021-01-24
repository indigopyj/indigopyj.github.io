---
layout: post
title: Deep Double Descent 요약 
date: '2021-01-21 14:00:00'
description: # Add post description (optional)
img: default.jpg # Add image post (optional)
tags: [Papers]
author: # Add name author (optional)
use_math: true
---

## Deep Double Descent : Where Bigger Models And More Data Hurt(2019)

### Double Descent phenomenon
> Bigger models are better

<img src="./img/ddd1.png" height=300>

- Performance first **improves**, then gets **worse**, and then **improves** again with increasing model size, data size, or training time.
- This phenomenon is called _"Double Descent"_

### EMC(Effective Model Complexity)

<img src="./img/ddd2.png">

- EMC: the maximum number of samples n on which Training procedure T acheives on average ≈ 0 training error



<img src="./img/ddd3.png">

- interpolation threshold : EMC(T) = n
- Critical interval : interval around interpolation threshold
  - Below and above critical interval : complexity ↑, performance ↑
  - Within critical interval : performance ↓

<img src="./img/ddd5.svg" height=500>

### Model-wise double descent

<img src="./img/ddd4.png">

- Double Descent intensifies when there is more label noise.

### Epoch-wise double descent

<img src="./img/ddd6.png">

- (Left) Larger model and intermediate model has double descent.
  - Larger model has double descent earlier than intermediate one.
- (Right) Test error of larger model descreases first, increases, and then descreases again as # of epochs increases.
  - But test error of intermediate model is not. It is better to stop early.

### Sample-wise non-monotonicity

<img src="./img/ddd8.png">

- (Left) Double descent abates when more samples.
- (Right) there is a regime where more samples hurt performance. But more than 10K, smaller model is better.

<img src="./img/ddd7.svg" height=300>

- increasing # of samples shifts the curve downwards towards lower test error.
- More samples require larger models to fit.
- For intermediate model, more samples hurts performance.


### Conclusion

- **In general, the peak of test error appears when models are just barely able to fit the train set.**
- Models at the interpolation threshold is the worst and label noise can easily destroy its gloabl structure.
- However, in the over-parameterized regime, there are many models that fit the train set.
- But authors don't know why this tendency happens.

#### references
- https://arxiv.org/pdf/1912.02292.pdf
- https://openai.com/blog/deep-double-descent/
- https://bluediary8.tistory.com/59

