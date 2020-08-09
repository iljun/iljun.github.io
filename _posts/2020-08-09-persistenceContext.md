---
layout: post
title: JPA
subtitle: Persistence Context 
---
# Persistence Context
JPA는 스레드가 생성될때마다 EntityManagerFactory를 통해 EntityManager를 생성한다.

이 EntityManager는 내부적으로 DB Connection pool을 이용해 DB에 붙는다.

이 EntityManager는 Transaction 관리 및 영속성 컨텍스트를 관리하기 떄문에 Thread끼리 공유되면 안된다.

## Life Cycle
### new/transient(비영속)
Persistence Context와는 전혀 관계가 없는 상태이다.

즉 Application에서 객체만 생성하고 아직 DB에 save한 상태가 아니다.

### managed(영속)
Persistence Context에 저장된 상태로써,

save method 또는 Select method를 통해 Persistence Context에 객체가 올라온 상태이다.

여기서 save method는 managed 상태가 되어도 바로 Query가 전달된 상태는 아니다.

Persistence Context에 올라가 있는 상태이며, Transaction이 commit 될 때 모든 DB Query가 전달된다.
 
### detached(준영속)
Persistence Conext에 존재하다가 강제로 분리된 상태이다.

detach method를 이용해 떨어뜨릴 수 있다.

### removed(삭제)
Persistence Context와 DB에 모두 삭제 명령을 전달하는 상태이다.


## identity
영속된 Entity의 동일성을 제공한다.

즉 동일한 객체를 조회했을때 `==` 연산자의 상태가 True가 반환된다.

이 의미는 Persistence Context에 존재하지 않는 객체는 DB를 통해 조회해 가져오고,

Persistence Context에 존재하는 객체는 Persistence Context에 저장된 객체를 가져오기 때문에

두 객체는 같은것이라고 판단이 가능하다.

## transactional write-behind
하나의 트랜잭션에서 지연 쓰기를 지원한다.

하나의 Transaction에서 여러번의 save를 실행해도

Persistence Context 내부에 SQL을 쌓아놓고 commit 시점에 한번에 전달한다.

여기서 flush의 개념이 있다.

flush와 commit의 차이점은 flush는 persistence Context 내부에 쌓여있는 SQL을 전달하는 역할이다.

즉, Persistence Context와 DB의 차이점을 sync하는 작업이다.

commit은 현재 Transaction의 상태를 DB에 반영한다.

즉 flush를 한 후 commit을 하지 않으면 DB에 반영되지 않는다.

## Dirty Checking
엔티티의 수정이 필요한 경우에 persistence context에 sync하는 작업이 필요없다.

Persistence Context는 persistence context 내부에 존재하는 객체가 변경될 경우 

자동으로 update query를 전달한다.

이건 persistence context 내부에 snapshot을 저장하기에 가능하다.


