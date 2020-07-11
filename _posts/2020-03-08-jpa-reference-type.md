---
layout: post
title: JPA Reference Type
---

# JPA의 값 타입

JPA는 값 타입이 2가지가 존재한다. Java에서 제공하는 기본 값 (String, Integer ...) 과 Entity가 존재한다.

Entity를 설계할 때 동일한 기능을 하는 field가 존재할것이다. 이 filed에 따라 동일한 Method도 존재할 수 있다.

이러한 field를 Entity가 생성 될 때마다 계속 재정의를 하게 된다면, 중복 코드는 물론 응집력이 높아지게 된다.

객체 지향적인 코드를 작성하기 어렵다.

이런 문제점을 해결하기 위해서 JPA는 `embedded type`을 제공한다.

embedded type이란 Entity, Java 기본 타입을 제외하고 새로운 Type을 생성하는 것이다.

이 embedded type은 중복키를 선언하는데도 사용된다.

`@Embeddable` 은 값 타입을 정의하는 곳에 선언하는 Annotation이며,

`@Embedded`는 값 타입을 사용하는곳에 선언하는 Annotation이다.

## AttributeOverride
Embedded 타입에 매핑된 정보를 재정의할 필요가 있다.

이떄 사용하는 Annotation이 `@AttributeOverride`를 사용한다.

재정의와 동일하게 field를 추가 수정 삭제도 가능하다.

# 값 타입 객체의 불변
Embedded Type을 여러 Entity에서 공유하게 된다면, 원치 않는 결과를 얻을 수 있다.

Persistence Context에 영속화 되어있는 Embedded Type이 변경되게 된다면, 이 Embedded Type을 참조하는 Entity의 값이 모두 변경되게 된다.

Embedded Type은 공유하지 않고 값을 복사해서 사용하는게 안전하다.
(Embedded Type은 결국 객체이고, 객체는 실제 값이 아닌 주소값이 전달된다)

Java에서는 `clone()`을 사용해 쉽게 값을 복사해서 사용 가능하다.

하지만 Java에서는 참조 값, 즉 객체의 주소값을 직접 전달하는 방법을 막을 방법이 없다.

이런 문제를 해결하기 위해 `immutable Object`로 설계하는 방법도 존재한다.

Setter Method를 만들지 않고, 생성자에서 초기 값을 세팅한다면 객체가 생성된 이후에 값을 변경 할 수 없다.

가장 쉽게 불변의 객체를 만드는 방법이다.

## ElementCollection
여러개의 값을 저장하려면 ElementCollection을 사용하면 된다.

이 ElementCollection은 Java의 기본값과, Embedded Type에서 사용 가능하다.

ElementCollection도 CASCADE, ORPHAN_REMOVE option을 동일하게 사용 가능하다.

ElementCollection은 데이터가 변경되었을때 테이블의 기본키를 이용해 변경된 내용만 반영하려고 하지만,

언제나 테이블의 기본키를 식별하지는 못할 수 도 있다.

왜냐하면 Entity는 기본 식별값(ID)를 이용해 쉽게 찾을 수 있지만, Embedded Type은 단순한 값의 모음이기 때문에 
값이 변경되면 DB에서 찾기 어려울 수 있다.


그렇기 때문에 ElementCollection을 사용할 떄는 데이터를 모두 삭제하고 새로운 값을 Insert하는 시나리오까지 고려해야한다.

