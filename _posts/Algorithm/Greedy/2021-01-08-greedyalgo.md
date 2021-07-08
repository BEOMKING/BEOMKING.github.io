---
title:  "그리디 알고리즘"
categories: 
- greedy
tags:
toc: true
toc_sticky: true
show_date: true <!-- 포스트 상단 작성일자 -->
sidebar_main: true  <!-- 포스트에 사이드바 -->
date: 2021-01-08
last_modified_at: 2021-01-08 00:31
---

**현재 상황에서 지금 당장 좋은 것만 고르는 방법**

현재의 선택이 나중에 미칠 영향에 대해서는 고려하지 않음

사전에 외우고 있지 않아도 풀 수 있을 가능성이 높은 문제 유형

**예시 1. 거스름돈**

카운터에 거스름돈으로 사용할 500원, 100원, 50원, 10원이 존재한다고 가정

손님에게 거슬러 줘야 할 돈이 N원일때 동전의 최소 개수

**해결 방법 - 가장 큰 화폐 단위부터 돈을 거슬러주는 것**

```python
n = 1260
count = 0 # 큰 단위의 화폐부터 차례대로 확인하기
coin_types = [500, 100, 50, 10]
for coin in coin_types:
  count += n // coin # 해당 화폐로 거슬러 줄 수 있는 동전의 개수 세기  
  n %= coin 
  print(count)
```
**화폐의 종류가 K라고 할 때 시간 복잡도는 O(K)**

거스름 돈 N은 시간 복잡도와 연관 X

대부분의 문제는 그리디 알고리즘을 이용했을 때 최적의 해를 찾을 수 없을 가능성이 다분

하지만 탐욕적으로 문제에 접근했을 때 정확한 답을 찾을 수 있다는 보장이 있을 때는 효과적이고 직관적

문제의 해법을 찾았을 때는 그 해법이 정당한지 검토 필요

예로 거스름 돈 문제를 그리디로 해결할 수 있는 이유는 가지고 잇는 동전 중에서 큰 단위가 항상 작은 단위의 배수이므로 작은 단위의 동전들을 종합해 다른 해가 나올 수 없기 때문

이처럼 문제 풀이를 위한 최소한의 아이디어를 떠올리고 이것이 정당하진지 검토할 수 있어야 답을 도출할 수 있다.