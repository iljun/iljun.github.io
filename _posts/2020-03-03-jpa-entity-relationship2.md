---
layout: post
title: JPA RelationShip part 2
---

# 상속 관계 Mapping

관계형 데이터 베이스에는 상속의 개념이 존재하지 않는다.

관계형 데이터 베이스에서 객체 지향의 상속을 비슷하게 사용 할 수 있는 방법은 `Super-Type Sub-Type Relationship`이라는 기법이 있다.

이 `Super-Type Sub-Type RelationShip`을 실제 구현 할 때는 3가지 방법을 사용 할 수 있다.

1. 각각의 테이블 설계
- super-Type의 table에는 공통적인 field만 생성하며, sub-Type에 해당하는 데이터들을 각각 다른 Table로 생성하고 조회시에 join을 이용하여 조회한다.
- 장점
  - 데이터가 정규화되어 데이터 공간의 낭비가 없다.
  - 외래 키 참조 무결성 제약 조건을 사용할 수 있다.
- 단점
  - Join Query가 많아지게 된다.
  - Insert Query도 언제나 2번이 실행되어야 한다.

2. 통합 테이블로 설계
- 하나의 테이블로 설계한다. 하나의 테이블의 모든 컬럼이 존재하는 형태이다.
- 장점 
  - join query가 없으므로 일반적으로 조회 성능이 빠르다.
- 단점
  - 데이터가 정규화 되어 있지 않은 상태로 존재
  - null인 데이터가 언제나 존재 할 수 있다
  - Table의 크기가 커진다.

3. 각각의 테이블로 설계
- 각각 모든 테이블로 설계하는 방법이다.
- 장점 
  - 서브 타입을 구분해서 처리할때 효과적이다.
- 단점
  - 여러 테이블을 동시에 조회할 때 성능이 느리다 (Union 사용)
  - 테이블을 통합해 Query하기 어렵다.

## JPA 상속 관계 전략 

1. Join 전략 
  - 엔티티를 각각의 테이블로 생성하고, 자식 테이블의 부모 테이블의 기본키를 이어받아 기본 키 + 외래 키로 사용하는 방법이다.
  - 여기서 객체 지향과 관계형 데이터 베이스의 패러다임 불일치에 대한 문제점이 하나 존재한다.
  - 바로 객체는 Type이 존재한다는 점이다. 관계형 데이터 베이스에는 Type이 존재하지 않아 패러다임의 불일치가 생긴다.
  - JPA는 이런 패러다임의 불일치를 해결하기 위해 테이블의 Type을 추가하여 패러다임의 불일치를 해결했다.
  - 부모 테이블의 Type이 존재해 어느 자식 테이블과 join을 할 지 알 수 있게한다.

2. Single Table 전략
  - 테이블 하나만을 사용하여 상속 구조를 표현하며, 어떤 데이터가 저장이 되어있는지 특정 Column을 이용해 구분한다.

3. Table Per Concrete Class
  - 자식 엔티티마다 테이블로 표현하는 방법이다.

## 상속 관계를 표현하기 위한 Annotation

- `@Inheritance`
  - super class에 선언해야 하는 Annotation이다.
  - option으로는 `Inheritance.JOINED`, `Inheritance.SINGLE_TABLE`, `Inheritance.TABLE_PER_CLASS`이다.
  
- `@DiscriminatorColumn`
  - 자식 클래스들을 구별할 수 있는 컬럼값을 설정 할 수 있는 Annotation이다.
  - 기본값은 DTYPE이다.
  - option으로 `name`을 사용할 수 있다.
  
- `@DiscriminatorValue`
  - 데이터가 Insert 될 때 자식 DiscriminatorColumn에 들어갈 값을 명시하는 Annotation이다.
  - 자식 클래스에 존재하야하는 Annotation이다.
  
- `@MappedSuperclass`
  - 앞에 설명한 Annotation은 모두 Entity와 Table을 Mapping하는 Annotation이다.
  - 이 Annotation은 Table Mapping이 아닌 자식 클래스에게 Mapping 정보를 제공하고 싶을때 사용하는 어노테이션이다.
  - 테이블의 공통으로 들어가는 Column이 있을때 `@MappedSuperclass`를 이용하여 중복 코드를 줄일 수 있다.
  - EX) created_at, created_by, modified_at, modified_by, deleted_at, deleted_by 등 공통으로 필요한 column에 주로 사용한다.
  
- `@AttributeOverrides`
  - `@MappedSuperclass`를 이용해 상속받은 정보를 재정의 할 때 사용한다.
  - option으로는 `name`, `column`이 있다.
  - EX) `@AttributeOverride(name = "id", column = @Column(name = "MEMBER_ID"))`
  
- `@AssociationOverrides`, `@AssociationOverrides`
  - 상위 클래스에서 정의된 연관관게를 재정의 할 때 사용하는 Annotation이다.
  - 실제 사용할 case는 정확하게 모르겠다.
  
  