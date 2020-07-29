---
layout: post
title: Reactive Stream
subtitle: BackPressure & Interface
---
## BackPressure
Reactive Stream에서 중요한 개념이다.

Reactive Stream도 publisher/subscribe 패턴이지만 BackPressure를 구현한 publisher/subscribe 패턴이다. 

BackPressure는 일반적인 publisher/subscribe 패턴에서 나타난 문제점을 해결한 패턴이다.

publisher/subscribe 패턴인 옵저버 패턴(observer pattern)을 알아보자.

### Observer pattern
Observer pattern은 객체의 상태 변화를 관찰하여 객체의 상태가 변경되면, 옵저버들에게 이벤트를 전달하는 패턴이다.

button 클릭과 같은 이벤트 핸들링을 처리하기 위해서 사용하는 패턴이다.

Observer Pattern은 2가지 요소가 존재한다.

- Observable
    - 등록된 Observer를 관리하며, 새로운 이벤트가 발생하면 관리하던 Observer에게 데이터를 전달하는 역할(Publisher)
- Observer
    - Observable로부터 이벤트를 받는 역할이다.(Subscriber)
    
이 Observer Pattern의 가장 큰 문제점은 `Observer가 처리할 수 있는 이벤트보다 많은 양의 이벤트`이다.

Observer Pattern은 Observable이 Observer에게 event를 push 하는 패턴이다.

Observer의 이벤트 처리량보다 많은 이벤트가 전달된다면 언제든지 OOM이 발생할 문제점이 있는 패턴이다.

그렇다면 BackPressure는?

Publisher가 Subscribe에게 event를 push 하는 방식이 아닌 Subscribe가 소모할 수 있는 만큼만 `pull`하는 방식이다.

이러한 방식은 `dynamic pull`이라고 부르며 BackPressure의 기본 원리이다. 

그럼 Reactive Stream에는 어떠한 Interface가 있고, 어떻게 동작하는지를 알아보자.

## Reactive Stream Interface
Reactive Stream은 4가지 Interface가 존재한다.
- Subscriber
    - 데이터를 전달 받는 주체
- Subscription
    - Publisher와 Subscriber의 중계자 역할
- Publisher
    - 데이터를 제공하는 주체
- Processor
    - Subscriber와 Publisher를 모두 상속받은 인터페이스로 데이터를 받고, 전달하는 중계자 역할에 사용한다.
    
### Interface의 동작 방법
- Subscriber는 Publisher를 `subscribe`한다.
- Publisher는 Subscription을 통해 Subscriber의 `OnSubscribe()` Method를 호출한다.
- Subscriber는 Subscription의 `request()` method를 통해 한 번에 얼마만큼의 데이터를 받을지 설정한다.
- Publisher는 설정된 데이터량 만큼 `onNext()` method를 통해 데이터를 전달한다.
- 완료는 `onComplete()` Method를 통해 알린다.

![ReactiveStreamSequence](/assets/img/interface.PNG)
출처: https://brunch.co.kr/@springboot/153

Reactive Stream의 Mono와 Flux는 Publisher Interface를 구현한 구현체이다.


