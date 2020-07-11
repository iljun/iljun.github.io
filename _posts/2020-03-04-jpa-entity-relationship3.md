---
layout: post
title: JPA RelationShip part 3
---

# 복합 키, 식별 관계 Mapping

관계형 데이터베이스에서 식별 관계는 식별 관계와 비식별 관계로 표현된다.

식별 관계는 부모 테이블의 기본키를 이용해 자식 테이블의 기본키이자 부모 테이블과의 외래키로 사용하는 방법이다.

비 식별 관계는 부모 테이블의 기본키를 자식 테이블과 부모 테이블의 외래키로 사용되지만, 자식 테이블은 기본키를 따로 가지고 있다.

식별 관계
- 장점
    - 인덱스를 활용하기 좋고, 상위 테이블의 기본 키를 하위 테이블에서 가지고 있으니 특정 상황에서 Join없이 Query가 가능하다.
- 단점
    - 하위 테이블의 depth가 깊어질때마다 기본 키 컬럼이 늘어나게 된다.
    - 복합키를 만들어야 하는 경우가 많다.

비식별 관계
- 장점 
    - 비지니스 모델이 변경되었을때 변경점이 적다.
    - 인조키를 사용하기 적절하다.
- 단점
    - 조회 query시 까다로울 수 있다.
    - 제약조건 설정이 잘 못 되었을 시 고아 데이터가 생성될 수 있다.
    
## 비 식별 관계 Mapping
JPA는 비 식별관계를 Mapping하기 위한 Annotation이 존재한다.

`@IdClass`, `@EmbeddedId`이다.

주의점으로는 식별자가 2개 이상이라면 별도의 클래스로 구분해야한다.

만약 별도의 클래스로 구분하지 않는다면 JPA는 인식하지 못한다.

또한 `equlas` 와 `hashCode`를 재정의 해야한다.

JPA는 Persistence Context에서 식별자를 Key로 이용한다.

별도의 class로 분리한 식별자의 `equals`, `hashCode`를 재정의 하지 않는다면 default인 Object의 `equals`, `hashCode`를 이용해

비교를 하게 될테고, 정확한 식별자 구분을 할 수가 없다.

`Serializable` Interface를 구현해야한다.

`@IdClass`와 `@EmbeddedId`의 차이점은

@IdClass는 별도로 분리한 class의 filed와 entity의 filed를 Mapiing

@EmbeddedId는 별도로 분리한 class를 그대로 사용하는 개념이다.

# Join Table
데이터 베이스에서 테이블의 연관 관계를 설정하는 방법은 2가지이다.

- `Join Column`
- `Join Table`
 
JPA에서는 이 두가지를 이용하는 방법은 `@JoinColumn`, `@JoinTable` 이 두개로 간단하게 가능하다.

1:1, 1:N, N:N 모두 사용 방법은 동일하다.

하지만 `@JoinTable`을 사용하는 경우 추가 filed를 추가하지 못한다.

Mapping Table이더라도 추가적인 정보가 필요한 경우는 다른 Mapping Entity를 생성해야한다.

`@JoinTable`은 사용 빈도수가 낮을 것으로 예상한다.

# Multiple table Mapping
하나의 엔티티에 여러 Table을 Mapping하는 방법이다.

`@SecondaryTable`로 하나의 Entity에 존재하는 데이터를 여러 Table에 나누어 저장할 수 있다.

option으로는 `name`, `pkJoinColumns`가 존재한다.

name에는 Mapping 할 테이블명, pkJoinColumns에는 다른 테이블의 기본키 컬럼을 지정한다.

Entity 내부에 다른 테이블에 저장될 컬럼은 `@Column(talbe=""")`으로 사용 가능하다.
