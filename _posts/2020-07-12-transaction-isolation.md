---
layout: post
title: Transaction Isolation Level
subtitle: 트랜잭션 격리 수준
---

RDB의 가장 큰 장점이자 성능에 이슈가 생길 수 있는 부분은 Transaction이다.

동시성 제어가 필요한 데이터에는 Transaction이 필요하며, 나도 Transaction을 이용해 데이터의 정합성 및 동시성 제어를 하고 있다.

동시성 제어를 하면서 알아야 하는 개념은 Transaction Isolation Level인데 크게 4가지로 구분된다.

- READ UNCOMMITTED
- READ COMMITTED
- REPEATABLE READ
- SERIALIZABLE

순서대로 Transaction의 고립 정도가 높아지며, 일반적으로 성능이 떨어진다.

일반적으로는 READ COMMITTED, REPEATABLE READ를 사용한다. (Mysql은 REPEATABLE READ를 default로 사용한다.)

## READ UNCOMMITTED
다른 트랜잭션이 진행중인 경우에도 데이터를 읽을 수 있는 level이다. 

즉 COMMIT하기 전의 데이터를 조회가 가능하며, `dirty read`라고 한다.

데이터의 정합성에 문제가 많으며, 권장하지 않는 방법이다.

## READ COMMITTED
다른 트랜잭션이 COMMIT 또는 ROLLBACK을 실행해야 데이터를 읽을 수 있는 수준이다.

하지만 하나의 Transaction에서 서로 다른 데이터가 조회될 수 있는 `NON-REPEATABLE READ`가 발생 할 수 있다.

일반적으로는 문제가 되지 않을 수 있으나 금융권등의 민감한 정보에는 문제가 생길 수 있다. 

입금과 출금이 연속되는 트랜잭션이 이루어져 있다면 순간 NON-REPEATABLE READ에 의해서 잔고가 다르게 변경 될 수 있다.

## REPEATABLE READ
트랜잭션이 시작하기 전 데이터만 조회가 가능하다.

즉 하나의 트랜잭션이 점유하고 있는 row에 대해서는 다른 트랜잭션은 READ도 불가능한 접근 Level이다.

`NON-REPEATABLE READ`까지 방지 할 수 있는 Level이나, 하나의 트랜잭션이 매우 길어질 경우 성능에 치명적 일 수 있다.

트랜잭션 단위가 커지면 성능에 치명적일 수 있기 때문에 트랜잭션의 단위를 작게 유지하는게 핵심이라고 생각한다.

REPEATABLE READ도 `PHANTOM READ`라는 문제점이 존재한다.

현재 SELECT한 조건에 일치하는 다르 데이터가 다른 트랜잭션에 의해서 생기거나 삭제가 된다면,

동일한 트랜잭션에서 SELECT한 결과가 존재하다가 없어지거나, 없다가 존재하게 된다.

이러한 현상을 해결하기 위해서는 write lock을 걸어야한다.

## SERIALIZABLE
가장 높은 수준의 격리 수준이다.

SERIALIZABLE일 경우 일반적인 SELECT 구문에도 잠금을 설정하며, 동시에 여러 Transaction이 데이터를 변경하지 못하게 된다. 


