---
layout: post
title: Blocking, NonBlocking
subtitle: Blocking, NonBlocking, Synchronous, Asynchronous
---
Blocking, Synchronous

NonBlocking, Asynchronous은 서로 비슷한 개념으로 보인다.

어떠한 차이점이 있는지 알아보자.

## Blocking/NonBlocking
`Blocking과 NonBlocking의 차이점은 바로 호출되는 함수, method가 바로 return이 되는지에 대한 여부이다.`

호출된 함수, method가 바로 return되어 제어권을 넘겨주는 것이 NonBlocking

호출된 함수, method가 모든 작업을 마치기 전까지 제어권을 넘겨주지 않는 것이 Blocking이다.

Blocking의 경우 제어권이 없기 때문에 제어권을 반환받을 때까지 대기하게 된다.

## Synchronous, Asynchronous
`Synchronous과 Asynchronous의 차이점은 호출된 함수의 작업 완료 여부를 누가 신경 쓰냐에 관심사이다.`

호출되는 함수의 작업 완료 여부를 신경 써 return과는 별개로 함수의 작업 완료 여부를 신경 쓰는 것이 Synchronous

호출되는 함수의 작업 완료 여부는 callback으로 전달받고 완료 여부에 신경 쓰지 않는 것이 Asynchronous이다.


동작은 전부 비슷하지만 관심사가 전혀 다르다.

그렇다면 서로 조합되는 4가지의 경우는 어떻게 동작할까?

### Blocking + Synchronous
함수를 실행 후 제어권을 가지지도 못하고 함수가 완료될 때까지 기다리게 된다.

코딩하기에는 가장 직관적인 방법이지만 성능에는 가장 좋지 않다.

### Blocking + Asynchronous
함수를 실행 후 제어권을 가지고 있지 않다가 callback에 의해서 제어권을 가지고 다시 실행하게 된다.

Blocking + Synchronous와 달라질 게 없다.

## NonBlocking + Synchronous
함수를 실행 후 제어권을 가지고 다른 일을 실행 할 수 있다.

하지만 중간중간 함수가 완료되었는지 체크해야 한다.

## NonBlocking + Asynchronous
함수를 실행 후 다른 일을 실행하는 도중 callback을 받아 함수가 완료되었는지 파악한다.

가장 효율적으로 resource를 사용할 수 있다.

하지만 디버깅이 어렵고, 구조 잡기에도 가장 어렵다.

이 조합에서 가장 조심해야 할 점은 바로 synchronous 로직이 포함이 되어있으면 안 된다는 것이다.

많은 Asynchronous 조합에 하나의 synchronous가 끼워져 있기만 해도 전체는 synchronous로 동작하기 때문이다.

![blocking-nonBlocking](/assets/img/blocking-nonBlocking.png)

참조 : https://has3ong.github.io/syncasync-nonblock/
