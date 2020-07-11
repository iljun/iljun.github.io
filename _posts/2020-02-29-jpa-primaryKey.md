---
layout: post
title: JPA Primary Key Mapping
---

JPA Primary Key Mapping 전략은 총 4가지가 있다.

AUTO, IDENTITY, SEQUENCE, TABLE이다.

우선 DB에서 Primary Key를 선택할때는 크게 2가지가 존재한다.

자연키와 대체키이다.

자연키는 의미가 있는 값 또는 비지니스 로직에서 의미가 있는 값이다.  EX) email, 주민번호 등등

자연키의 큰 단점은 언제든지 변경이 가능해 Unique하지 않다.

대체키는 의미가 없는 값 즉 임의로 생성한 값이다.

자연키와는 다르게 의미가 없이 생성한 값이기 때문에 Unique하다.

JPA에서 `@Id`를 이용해 해당 Entity의 고유한 식별자, DB에서의 Primary key가 어떤 filed인지 지정이 가능하며, 

`@GeneratedValue` 이용해 Primary Key Mapping 전략을 선택 가능하다.

---

### AUTO
- Primary Key Mapping 전략을 설정하지 않으면 default로 설정되는 값이다.

Application에서 직접 Primary key값을 할당하는 방법이다.

비지니스 로직에서 직접 ID값을 지정해줘야한다.

언제나 Unique한 값을 생성해야하고, DB에 존재하는지 확인하는 과정이 필요하다.

만약 식별자값을 지정해주지 않는다면 Exception이 발생하며,

JPA에서는 PersistenceException이 발생한다.

AUTO의 장점은 DB가 변경되어도 코드 수정이 필요없다는 점이다.

하지만 Primary key Mapping 전략이 SEQUENCE, TABLE 등으로 변경된다면 변경되기 전 키 생성용 table이나 sequence를 미리 만들어 두어야한다.

##### 자연키를 사용하는 경우에는 사용이 가능하겠지만, 특별한 이유가 있지 않는 이상은 AUTO는 사용하지 않는다.

---
### IDENTITY
- Primary Key 생성을 DB에 위임하는 전략이다.

가장 흔하게 사용하는 방법이며, 나는 거의 IDENTITY를 사용한다.

주로 MySQL, PostgreSQL, SQL Server, H2 등등 auto_increment를 지원하는 DB에서 사용한다.

이 방식을 사용한다면 데이터 insert이후 추가로 DB를 조회한다.

Entity가 persistence화 되려면 식별자가 필수이다.

식별자가 필수이기 때문에 영속화가 이루어지면 바로 INSERT SQL이 DB로 전달된다.

이 전략은 Lazy Transaction이 동작하지 않는다.

왜냐하면 식별자가 필수이기 때문에 DB에서 생성한 식별자가 필수이기 때문이다.

##### 아직 Lazy Transaction을 적용한 경험은 없지만, 이 방식이 가장 효율적이며 가장 보편적인 방법이라고 생각한다.

---

### SEQUENCE
- DB의 Sequence를 이용해 Primary Key값을 할당하는 방법이다.

auto_increment를 지원하지 않는 DB에서 주로 사용한다.
Entity를 사용하기 전 sequence가 존재해야한다.

`@SequenceGeneratr`을 이용해 Entity에 Mapping이 가능하다.
`@SequenceGenerator`의 Option으로는 `name, sequenceName, initialValue, allocationSize, catalog, schema`가 있다.

이 option중 가장 중요한 option은 sequenceName과 initialValue, allocationSize이다.

sequenceName은 실제 사용 할 sequence를 지정하는 option이며,
initialValue는 DDL 생성시에 sequence가 시작할 값을 지정하는 option이다.

allocationSize는 sequence 호출 시 증가하는 숫자를 지정하는 의미이다.

allocationSize의 default 값은 50이다. sequence값이 1씩 증가하도록 설정이 되어있다면,
필수로 1로 지정해야한다.

allocationSize의 default 값이 50인 이유는 insert 성능의 최적화 때문이다.

SEQUENCE 전략은 DB를 통해 식별자값을 조회해야하는 추가적인 query가 필요하다.

JPA는 sequence의 접근을 줄이기 위해 default값을 50으로 설정했다.

allocationSize가 50이면 JPA는 sequence를 조회한 후 allocationSize 값에서 + 50한 값까지 memory에 할당하고 사용한다. 이 최적화 방법은 여러 JVM이 동시에 작동해도 기본 키 값이 충돌하지 않는다는 장점이 있다.

insert Query 성능이 중요하다면 적용해볼만한 방법이지만, 일반적으로는 사용하지 않을것 같다.

---

### TABLE
- 키 생성 전용 Table을 만들어 사용하는 방법이다. sequence를 비슷하게 구현한 방법이다. 하지만 이 table의 primary key를 사용하는 table의 Primary key가 모두 같이 증가하는 단점이 있다.

키 생성 전용 Table을 사용한다는것을 제외하고는 SEQUENCE 전략과 동작 방법은 동일하다.

`@TableGenerator`을 이용해 지정하며, option에는 name, table, pkColumnName, valueColumnName, initialValue, allocationSize, catalog, schema, uniqueConstraints가 존재한다.

TABLE 전략은 SELECT query를 통해 조회 후 UPDATE query를 통해 value를 update 한다.

SEQUENCE 전략보다 query가 한번 더 발생한다는 단점이 존재하지만, 성능 최적화위해서 SEQUENCE와 동일하게 allocationSize를 적용 가능하다.

---