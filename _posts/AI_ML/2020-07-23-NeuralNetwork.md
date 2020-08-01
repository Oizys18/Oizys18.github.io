---
layout: post
title: "신경망 NeuralNetwork"
author: "Oizys18"
categories: AI_ML
comments: true
tags: [AI, MachineLearning,NeuralNetwork, Basic]
---


## 구조
- Input(입력층), Hidden(은닉층), Output(출력층)
- 퍼셉트론은 활성화함수로 계단함수(step) 사용, 신경망은 주로 시그모이드 함수 사용 
    - 시그모이드 함수 https://icim.nims.re.kr/post/easyMath/64
    - 시그모이드 함수는 값을 실수형으로 가지는 것을 볼 수 있다. 시그모이드 함수의 매끄러움은 가중치 값을 전달할 때 좀 더 부드럽게 양을 조절해서 전달할 수 있다는 점이 계단 함수와 다른 점이다.

```
왜 비선형 함수를 사용해야 하는가?
선형함수를 사용했을 때는 은닉층을 사용하는 이점이 없기 때문이다. 다시 말해 선형함수를 여러층으로 구성한다 하더라도 이는 선형함수를 세번 연속 반복한 것에 지나지 않는다는 의미와 같기 때문이다. y = ax라는 선형함수가 있다고 한다면 이 것을 3층으로 구성하면 y = a(a(a(x))) 와 동일한 것으로 이는 y = a3(x)와 같다. 굳이 은닉층 없이 선형함수로 네트워크를 구성하는 것은 의미가 없다는 뜻입니다.
```
```
회귀 --> 항등 함수 (출력 값을 그대로 반환하는 함수) identity function
분류(0/1) --> 시그모이드 함수 sigmoid function
분류(multiple) --> 소프트맥스 함수 softmax function
```
```
시그모이드, 퍼셉트론 둘 다 0 or 1의 리턴을 한다.
```