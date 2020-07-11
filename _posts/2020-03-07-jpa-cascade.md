---
layout: post
title: JPA CASCADE
---

# 영속성 전이

Entity를 persistence화 할때 연관된 Entity도 함께 persistence화 할 수 있다.

연관 객체를 함께 영속화하는 option은 `CASCADE` option을 이용해 편리하게 진행 할 수 있다.

cascade의 option은 `ALL`, `PERSIST`, `MERGE`, `REMOVE`, `REFRESH`, `DETACH` 등이 있다.

# 고아 객체

JPA는 연관관계가 끊어진 연관 Entity를 자동으로 삭제하는 기능이 존재한다.

`orphanRemoval = true`를 이용하면 연관관계가 끊어진 객체를 자동으로 삭제한다.

cascade의 REMOVE와 비슷하게 동작한다. 