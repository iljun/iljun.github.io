---
layout: post
title: @Transactional
subtitle: Spring Transactional Annotation
---
# @Transactional 
AOP 방식으로 동작하며, 메소드, 클래스에 적용하여 트랜잭션이 적용되는 Annotation이다.

## Option
### isolation
격리수준을 의미하며 트랜잭션이 일관성이 없는 데이터를 허용하는 수준을 뜻한다.

별도로 설정하지 않으면 DB의 isolation level을 따라간다.

- READ_UNCOMMITTED
    - 아직 COMMIT 되지 않은 데이터에 접근이 가능하다.
    - DIRTY READ가 발생하는 단계이며, 데이터의 일관성이 매우 떨어지는 단계이다.
    
- READ_COMMITTED
    - COMMIT이 확정된 데이터만 READ가 가능한 단계이다.
    - 하나의 ROW에 접근하여 데이터를 수정하는 동안 다른 Transaction이 침범이 불가능하다.

- REPEATABLE_READ
    - 하나의 트랜잭션이 끝나기 전까지 모든 데이터에 Shared Lock이 걸린다.
    - 하나의 SELECT에도 Lock이 걸리기 때문에 쓰기도 대기하게 된다.

- SERIALIZABLE
    - 완벽한 일관성을 제공한다.
    - 트랜잭션이 완료될때까지 SELECT 문장이 적용되는 모든 ROW에 Shared Lock이 걸리고 그 영역에 대한 수정이 불가능하다.
    
    
### propagation
Transaction의 전파 범위를 의미한다.
- REQUIRED(default)
    - 부모 트랜잭션 내에서 실행하며, 부모 트랜잭션이 없으면 새롭게 생성한다.
- REQUIRES_NEW
    - 부모 트랜잭션의 여부에 관계없이, 새로운 트랜잭션을 생성한다.
- SUPPORT
    - 부모 트랜잭션이 있으면 사용하고 없다면 nonTransactionally로 실행한다.
- MANDATORY
    - 부모 트랜잭션에서 실행하며 부모 트랜잭션이 없다면 exception이 발생한다.
- NOT_SUPPORT
    - nonTransactionally로 실행되며, 부모 트랜잭션 내부에서 실행되면 일시 정지 된다.
- NEVER
    - nonTransactionally로 실행되며, 부모 트랜잭션이 있다면 exception이 발생한다.
- NESTED
    - 부모 트랜잭션에서 실행됨다면 별개로 커밋 or 롤백이 가능.
    - 부모 트랜잭션이 없다면 REQUIRED와 동일하게 작동한다.
    
### no-rollback-for-(exception)
특정 exception이 발생해도 롤백되지 않는다.


