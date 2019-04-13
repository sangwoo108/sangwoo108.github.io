---
layout: post
title: 알고리즘 - 1. 알고리즘의 평가와 점근적 표기법
categories: [Study]
tags: [Algorithm, Asymptotic notation]
description:
---

### 알고리즘

주어진 문제를 풀기위한 절차 / 방법

### 알고리즘의 비교와 평가

하나의 문제를 푸는 방법은 여러가지가 있을 수 있으니, 서로 다른 알고리즘 중 어느 것이 더 좋은 것인지 평가할 기준이 필요함. 또 이 기준을 어떻게 측정할 것인지에 대한 고민이 필요.

### 알고리즘의 평가 기준 - 시간과 공간
얼마나 빠르게 푸는지, 그 알고리즘을 사용할때 추가로 사용하는 공간은 얼마나 많은지.

#### 측정의 어려움
실측값은 알고리즘의 평가에 도움이 안될 수 있음 - 환경에 따라 다를 수 있기 때문에

* 머신 성능
* 시스템 아키텍쳐
* 컴파일러의 최적화
* 메모리 하이어라키

### 점근적 표기법(Asymptotic notation)
이런 환경적인 문제와 독립적으로 알고리즘을 평가하기 위한 방법.
가장 큰 차원의 항의 승수만을 생각함.
**Input이 늘어남에 따라, 알고리즘의 시간/공간은 어떤 추세로 변화하는가(Growth rate)?**
또한 사칙연산의 적용이 가능해지고, 표기가 간결해지는 장점이 있음.

* Big-O : `f(n) = O(g(n))` 
  * n이 충분히 클때 `f(n)`은 `g(n)`보다 오래 걸리지 않는다 / 공간을 많이 쓰지 않는다.
  * `g(n)`은 `f(n)`의 상한(upper bound)이다.
  * 아무리 느려도 `g(n)`보다 느리지는 않다.
  * <img src="https://cdn.kastatic.org/ka-perseus-images/501211c02f4c6765f60f23842450e1151cfd9c89.png"/>
* Big-Ω : `f(n) = Ω(g(n))`
  * n이 충분히 클때, `f(n)`은 `g(n)` 보다 오래 걸린다 / 공간을 많이 쓴다.
  * `g(n)`은 `f(n)`의 하한(lower bound)이다.
  * 최소한 `g(n)`만큼의 시간/공간이 필요하다.
  * <img src="https://cdn.kastatic.org/ka-perseus-images/c02e6916d15bacae7a936381af8c6e5a0068f4fd.png"/>
* Big-Θ: `f(n) = Θ(n)`
  * `f(n)`은 `g(n)`과 점근적으로 같은 실행시간 /  공간을 필요로한다.
  * `g(n)`은 tight bound이다.
  * <img src="https://cdn.kastatic.org/ka-perseus-images/c14a48f24cae3fd563cb3627ee2a74f56c0bcef6.png"/>

* 모든 경우 tight bound를 쓰면 좋겠지만, 주어진 input의 특성에 따라 알고리즘의 평가가 달라지는 경우가 있어,
 Big-O만을 얘기하는 경우가 일반적이다. 그러나 어떤 알고리즘의 Best / Average / Worst case 마다 성능이 다르다면
 이를 잘 알고 있는 것이 도움이 된다.


### Best / Average / Worst case
#### Best case
최선의 경우에 대해서 논하는 것은 대부분의 경우 실질적인 도움이 되지는 않음.

#### Average
모든 가능한 경우의 복잡도를 distribution에 따라 합산. 그러나 distribution을 알수 없는 경우가 많기 때문에, 알 수 없는 경우가 많다. 

e.g. Linear search의 경우, 찾으려는 수가 0~n 각각의 인덱스에 등장할 확률이 uniform 할때,

`linear search = (O(1) + O(2) + ... + O(n)) / n`

이때 1 + 2 + ... n = ((1 + n) * n) / 2 이므로,  O(n)으로 볼수 있음.
그러나 이것은 앞서 언급하였듯 uniform distribution 전제가 필요함.


#### Worst
주로 O(n)과 함께, `최악의 경우에도 g(n) 보다는 빠르다`는 평가를 내리는 것이 일반적.


이와 같이 각 case에 대해 tight하게 평가 가능하다면 theta를, 일반적인 평가를 하자면 O를 사용하는 것이 일반적.


### 분할상환(Amortized) 분석
만약 최악의 경우가 한번 발생한 것이 다음번 최악의 경우가 발생할 확률을 낮춰준다면, 나머지 경우들의 최악의 경우의 비용을 분할 상환하고 있는 것으로 볼 수 있다. e.g. Array의 마지막에 append할때 capacity를 하나씩 늘리지 않고 더 많이 늘리는 경우, 다음번 최악의 경우(가득차있어서 모든 요소를 복사해야하는 경우)를 줄여준다.
Average case의 분석은 이와 달리 독립 시행과 같은 확률적 분석이므로 다르다.

e.g. 동전의 앞이 나오면 10, 뒤가 나오면 1의 비용이 든다고 할때

Average는 `O(head) * 0.5 + O(tail) * 0.5 = 10 * 0.5 + 1 * 0.5 = 5.5` 그러나 실제로는 조금 다를 수 있다.

한편, 만약 동전이 무조건 정해진 시퀀스대로 나온다고 전제한다면, 즉 앞-뒤 로만 나온다면
Amortized는 `(10 + 1) / 2`로 보아 각 경우를 5.5라고 본다. 이 경우는 이값이 항상 실제와 같다.

### 이미지 출처
<a href="https://ko.khanacademy.org/computing/computer-science/algorithms#asymptotic-notation">칸 아카데미 - 점근적 표기법</a>