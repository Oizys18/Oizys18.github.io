---
layout: post
title: "오차 역전파 Error Backpropagation"
author: "Oizys18"
categories: AI_ML
comments: true
tags: [AI, MachineLearning, BackPropagation, Basic]
---

## 개요

- 일반적인 방법으로 Input -> Output으로 가중치를 업데이트하면서 활성화 함수를 통해 결과값을 업데이트 하는 것을 **순전파(forward)**라고 하며, 반대로 결과 값을 통해서 역으로 input 방향으로 오차를 다시 보내며 가중치를 재업데이트하는 것을 **역전파** 라고 한다.

