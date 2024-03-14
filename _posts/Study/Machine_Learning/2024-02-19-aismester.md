---
title: "AI 단기강좌"
excerpt: ""
categories: [MachineLearning]
tags: [ML]
toc: true
toc_sticky: true
---

트렌스포머

입력값과 weight의 operation

CNN
* 컨볼루션 연산 사용
* 저수준의 특징에서 고수준의 특징으로
* CNN의 단점을 해소한게 residual learning

RNN
* 반복적인 연산
* 똑같은 weight를 연산을 시켜 결과를 냄

* hidden state 값과 인풋과 연산
* 동일한 weight를 반복적으로 사용

LSTM
* RNN이 학습에 문제가 있는 문제점(RNN은 이전 값만 너무 반영함)을 개선
* 게이트 3개
  * 이전 데이터 반영 정도 게이트
  * 현제 데이터 반영 정도 게이트
  * output 게이트(얼마나 출력할 것인가)
* 전 hidden state에 여전히 가장 많은 영향을 받음


Attention
* 중요도를 선별할 수 있음(각각의 입력값들에 따라서 weight가 있음) 주목하는 정도
* 어텐션맵: 각 단거사이의 관계를 나타냄

Attention Mechanism
* 입력 단어들에서 출력값들 사이의 유사도를 판별, 가중치 부여
* 단점: 입력 간의 관계성을 고려하지 않음


Transformer

Multi-Head Attention
* 서로 다른 헤더끼리의 다른 관점을 가질 수 있음 
* 병렬 처리 가능