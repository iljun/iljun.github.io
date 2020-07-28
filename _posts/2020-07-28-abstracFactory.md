---
layout: post
title: Design Pattern
subtitle: Abstract Factory
---
# Abstract Factory(추상 팩토리)
Factory 패턴과 비슷하지만 Factory 패턴을 더 추상화한 패턴이다.

여러개의 객체를 생성해야하는 경우 객체 생성의 일관성이 깨질 수 있다.

이러한 문제점을 해결하기 위해서 제공되는것이 추상 팩토리 패턴이다.

Factory 패턴의 한계를 극복해야 할 때 적용해야 하는 패턴이다.


![Abstract Factory Image](../assets/img/abstract-factory-pattern.png)
출처: https://gmlwjd9405.github.io/2018/08/08/abstract-factory-pattern.html

#### AbstractFactory
Client가 사용하는 Interface이다.

서로 연관있는 객체들을 생성해야 할 경우 Client는 AbstractFactory를 통해 객체를 생성하게 되면,

연관있는 객체들을 생성 가능하다.

즉, Client는 AbstractFactory를 통해 연관있는 모든 객체를 생성하는 패턴이다.
s
