---
layout: post
title: JPA Proxy & FetchType
---

# JPA Proxy

`JPA Proxy`는 연관 관계에 속한 Entity를 사용할 때만 조회할 수 있게 하는 역할을 한다.

Entity의 자유로운 객체 탐색을 위해서는 언제나 연관 관계에 속한 Entity를 한번에 조회해야한다.

이건 성능상에 치명적인 문제가 될 수 있다.

Proxy는 이러한 문제를 모두 해결한다.

Proxy 객체는 Entity 객체를 상속받은 Object이다.

이 Proxy 객체가 동작하는 방법은 아직 Persistence Context에 올라와 있지 않은 Entity는 Proxy객체라고 생각하면 된다.

그렇기 때문에 아직 조회가 안된 상태로 존재하며 객체 탐색을 할때 호출할때는 Proxy 객체의 Method를 호출하게 된다.

Proxy 객체 Method에서 아직 영속화가 되지 않은 상태라면 이때 DB를 조회하게 된다.

이 Proxy 객체가 초기화 되는 시기는 Persistence Context에 조회하려는 Entity가 존재하지 않을 때 이다.

이미 Persistence Context에 Entity가 존재한다면 Proxy 객체가 아닌 실제 Entity 객체를 사용하게 된다.

`이미 영속화된 Entity가 아니라면, 우리는 Proxy 객체를 통해 Entity에 접근하고 있는 것이다.`

Proxy 객체가 사용되는 이유는 주로 `Lazy Loading`이다. 필요한 순간에만 Entity를 조회하는 방법이다.

## Lazy Loading, Eager Loading
JPA는 객체의 Loading 시점을 결정할 수 있다.

- Lazy Loading 
    - 연관된 Entity를 실제 사용할 때 조회
- Eager Loading
    - 연관된 Entity를 한번에 조회
    
`JPA는 성능 최적화를 위해서 Eager Loading일 경우 Join을 사용해 조회한다.`

`Lazy Loading의 장점은 필요한 경우에만 query를 하기 떄문에 성능에 최적해 보일 수 있지만 언제나 함께 사용하는 Entity라면 Eager Loading이 훨씬 적절하다.`

Lazy Loading을 사용하려면 FetchType.LAZY로 지정하면 간단하게 사용 가능하다.

Eager Loading은 마찬가지로 FetchType.EAGER로 지정 가능하다.

### JPA Join 전략

JPA는 외래키의 여부에 따라 `INNER JOIN`, `OUTER JOIN`을 선택적으로 사용한다.

INNER JOIN이 성능에 조금 더 최적화 되어 있지만, OUTER JOIN을 선택적으로 사용하는 이유는

바로 외래키의 NULL 여부이다.

외래키가 NULL값을 허용 한다면, Entity끼리의 객체 탐색에서 원치 않은 데이터가 삽입 되거나, 필요한 데이터가 삽입이 되지 않을 수 있다.

이 문제점을 해결하기 위해서 `OUTER JOIN`을 선택적으로 사용하는데,

INNER JOIN으로 변경하기 위해서는 간단하다. `@JoinColumn`에 option을 nullable = false로 설정하면
JPA는 INNER JOIN을 사용한다.

또는 `@ManyToOne.optional = false`를 이용해도 가능하다.

### JPA default Fetch 전략

1. 연관된 Entity가 1개일때는 Eager Loading
2. Collection이라면 Lazy Loading이다.
    - Collection을 Loading하는 경우는 매우 비용이 크며, 잘못하면 너무 많은 데이터를 Loading하기 때문이다.
    
    