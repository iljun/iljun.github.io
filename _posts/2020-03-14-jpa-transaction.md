---
layout: post
title: JPA Transaction
---

# 트랜잭션 범위의 Persistence Context

## Spring Container의 Default 전략

Spring Container는 트랜잭션의 범위와 Persistence Context의 life Cycle이 동일하다.

일반적으로 `@Transactional`을 이용해서 Transaction을 시작한다.

@Transactional이 선언된 Method를 실행하면 Spring Transaction AOP가 먼저 동작하고,

Method가 정상적으로 종료되면 flush를 호출한다.

## 준영속 상태와 Lazy Loading

Spring Container의 Default 전략을 사용하게 되면 준영속 상태에서는 Lazy Loading이 정상적으로 동작하지 않는다.

준영속 상태란 Persistence Context가 종료된 이후 Entity가 준영속 상태가 된다.

이러한 준영속 상태에서는 Lazy Loading이 동작하지 않고, Exception을 발생시킨다.

준영속 상태에서 Lazy Loading을 해결하는 방법은 2개가 존재한다.

- 필요한 Entity를 미리 로딩해두는방법
- OSIV를 사용해 Entity를 항상 영속 상태로 유지하는 방법

#### 필요한 Entity를 미리 로딩해두는 방법
- fetchType 수정
    - fetchType을 Lazy에서 Eager로 변경하는 방법
    - 언제나 Eager로 변경되기 떄문에 성능에 큰 영향을 미칠 수 있다.
    - 필요하지 않은 Entity를 조회한다.
    - N + 1 문제가 발생한다. (N + 1 문제란 조회한 Entity만ㅋ틈 SQL을 추가로 여러번 실행하는 문제이다.)    
- JPQL fetch Join
    - N + 1 문제를 해결할수 있는 방법이다.
    - 
- 강제로 초기화 

다른 방법으로는 FACADE 계층 추가가 존재한다.

FACADE 계층은 Presentation 계층과 도메인 모델 계층간의 의존성 분리를 위해 생성한다.

Presentation 계층에서 필요한 Proxy객체를 초기화해 Lazy Loading이 가능하게 한다.

### OSIV(open session in view)
Persistence Context를 view까지 열어둔다는 의미이다.

Persistence Context가 살아 있기 때문에 view단에서도 Lazy Loading이 가능해진다.

OSIV의 큰 문제점은 Presentation계층, View에서 Entity를 변경할 수 있다는 점이다.

Presentation, View계층에서 Entity값을 변경해 버리면 뷰를 렌더링 한다음 flush를 호출하게 된다.

그러면 원하지 않는 결과가 데이터로 저장되게 된다.

이 문제를 해결하기 위해서는 몇가지 방법이 존재하는데,

Entity를 readOnly만 가능한 Interface로 제공하는 방법

Entity를 readOnly만 가능한 객체로 Wrapping하는 방법

Entity를 DTO로 변환해 전달하는 방법이 있다.

이런 문제들때문에 OSIV를 사용하지 않는 편이 좋아보인다.

### Spring의 OSIV를 알아보자
Spring FrameWork를 OSIV를 따로 제공한다.

Sprign OSIV는 OSIV를 제공하긴 하지만 트랜잭션은 비지니스 계층에서만 사용한다.

결국 Filter, Controller, View에서는 영속상태이긴하나 수정이 불가능하며,

Service와 Repository단에서는 수정까지 가능하다.

Spring OSIV에서도 주의할점이 있다.

바로 Presentation 계층에서 엔티티를 수정한 다음, 다른 Transaction을 실행하면 이전과 동일한 문제가 발생한다.

이점은 유의해서 작업을 진행해야한다.

`차라리 OSIV를 사용하지 않는 방법도 괜찮은 방법이라고 생각한다.`


