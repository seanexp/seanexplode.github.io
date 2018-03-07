---
layout: post
title: Optimization as a Model for Few-Shot Learning 리뷰
date: 2018-03-06
description: ICLR 2017 논문인 Optimization as a Model for Few-Shot Learning 을 리뷰한다.
tags: [Meta Learning, Few-shot Learning]
---

최근 One-Shot/Few-shot Learning 관련 논문들을 읽다보면 [Optimization as a Model for Few-Shot Learning](https://openreview.net/pdf?id=rJY0-Kcll) 과의 성능 비교를 하는 부분들이 종종 보였다. 그래서 이번 포스트에서는 본 논문을 읽고 리뷰를 해보려고 한다.

## One-Shot/Few-Shot Learning

딥러닝은 많은 분야에서 큰 성공을 거두었다. 그러나 딥러닝의 성공은 많은 데이터가 축적된 환경에서만 이루어졌으며, 적은 수의 데이터만으로 빠르게 일반화해야 하는 상황에서는 실망스러운 성능을 보였다. 많은 연구자들이 하나 혹은 적은 수의 데이터만으로 빠르게 일반화하여 학습시킬 수는 없을까 고민하기 시작했고, 하나 혹은 적은 수의 데이터만으로 빠르게 일반화하여 학습하는 하는 것을 **One-Shot/Few-Shot Learning** 이라고 명명했다. 많은 딥러닝 방법론들이 인간 수준의 지능을 따라잡았지만 One-Shot/Few-Shot Learning 분야에서는 아직 인간 수준의 성능을 보이지는 못하는 것 같다.

## 본 논문이 풀려고 하는 문제

논문 제목에서 쓰여있듯이, 이 논문이 풀고 싶은 문제는 One-Shot/Few-Shot Learning 이다. 저자들은 이 문제를 풀기 위해 다음과 같은 의문을 던졌다.

> 왜 딥러닝은 많은 수의 데이터를 필요로 할까?

저자들은 딥러닝 모델들의 성공이 많은 그레디언트 업데이트를 필요로 하기 때문이라고 말했다. 또, 적은 수의 라벨링이 된 예시들만으로 그레디언트 기반 최적화가 실패하는 이유는 다음과 같이 두 가지가 있다고 보았다.

1. momentum, Adagrad, Adadelta 그리고 ADAM 과 같은 그레디언트 기반 최적화 알고리즘들은 그레디언트 업데이트 수가 제한되었을 때 잘 작동하도록 만들어지지 않았다.
2. Random initialization을 한다. 이것은 적은 수의 그레디언트 업데이트로 좋은 해를 얻기 어렵게 만든다.

그렇다면 이제부터 풀어야 하는 문제는 명확하다. **1)** 적은 수의 그레디언트 업데이트로도 학습이 잘 될 수 있도록 하는 업데이트 규칙을 만든다. **2)** 적은 수의 그레디언트 업데이트만으로 최적해가 될 수 있는 초기값을 찾는다.
