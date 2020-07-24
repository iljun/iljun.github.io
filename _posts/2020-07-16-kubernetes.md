---
layout: post
title: Kubernetes
subtitle: Kubernetes
---

# Kubernetes 란?
Kubernetes는 컨테이너화된 Application을 배포하고 확장을 편리하게 할 수 있게하는 오픈소스 플랫폼이다.

특징으로는 선언전 구성과 자동화를 모두 지원한다.

또한 컨테이너의 상태의 따라 새로운 컨테이너를 추가 하거나 또는 제거하는등 선언적 설정에 따라 application을 관리하게 해준다.

## 장점
- 효율성
    - 컨테이너화 된 Application을 Node의 상황에 따라 적절히 나누어 배치함으로써 Node의 resource를 효율적으로 사용 가능하다.
- 자동화
    - 어플리케이션 배포 및 업데이트가 자동화 되어있으며, scale-out도 자동화가 되어있다.
- 로깅, 모니터링
    - kubenetes의 제공되는 daemonset 기능을 사용하면 로깅, 모니터링의 기능을 쉽게 구축이 가능하다.    
- 무중단 배포
    - 설정에 따라 다르게 배포 방식은 달라질 수 있으나 readinessprobe를 이용하여 무중단으로 배포가 가능하다.
- secret 관리
    - password, OAuth 토큰, SSH key등을 kubernetes 자체에서 관리 할 수 있습니다.
    - 컨테이너 이미지를 재 구성하지 않고 kubernetes의 설정 변경으로 update가 가능합니다.
- 자가 치유
    - 컨테이너가 정상 상황이 아닐 경우 kubernetes가 현재 상태를 판단해 컨테이너를 재성성, 롤백하는 등의 자동으로 장애 상황을 극복한다.
  
## Cluster   
Cluster란 여러개의 Node를 묶어 관리하는 단위이다.
즉, 여러개의 머신을 하나의 단위로 묶어 관리할 수 있게해준다.

Cluster는 하나의 master cluster와 다수의 Node로 구성된다.
- master cluster
    - 클러스터를 관리합니다. 즉 클러스터의 모든 활동을 조정합니다. (확장, 스케쥴링, 배포 등등)
    - master cluster의 장애 상황은 kubernetes의 장애 상황으로 이어진다.
    - master cluster의 장애 상황을 대비하기 위해서는 HA를 구성해야한다.
- Node
    - Application이 동작하는 머신이다.
    - 하나의 Node에는 여러개의 kubernetes의 Object들이 올라가며, 각 Node에 배치되는 기준은 kubernetes가 결정한다.
 
## Object
kubernetes Object는 kubernetes system에서 영속성을 가지는 개체를 나타낸다.

컨테이너에 대한정보, 네트워크, 디스크 자원, 클러스터가 어떠한 작업을 하는지에 대한 정보를 모두 Object로 표현한다.

중요한 kubernetes의 Object는 아래와같다.
- Pod
- Service
- Namespace
- Deployment
- Daemonset
- StatefulSet
- ReplicaSet

### Pod
Kubernetes Application 실행의 기본 단위이다.

kubernetes Object의 가장 작은 단위이며, 우리가 작성하는 Application의 단위라고 생각해도 된다.

Pod은 하나의 Application으로 구성될 수 있으며, 다수의 Application으로 구성될 수 있다.

다수의 Application으로 구성하는건 추천하지 않는다.

다수의 Application으로 구성된 경우는 Volume 공유가 꼭 필요한 경우가 아니면 결합도가 너무 높아지기 때문이다. 

만약 다수의 Application으로 구성된 하나의 pod은 서로 `network`와 `volume`을 공유하게 된다.

Pod은 위에서 얘기했던 자동 치유, scale out등의 기능을 지원하지 못한다.

자동 치유, scale out은 pod을 관리하는 다른 object에서 담당한다.

Pod은 순수하게 Application 하나의 기능이라고 생각해도 무방할 것 같다.

Pod은 일반적으로 직접 생성하지는 않는다. `Deployment`를 이용하여 Pod을 생성하게 된다.

그 이유는 `Deployment`를 이용해야 Pod의 자가치료, 롤아웃 등등 다양한 기능을 사용할 수 있기 때문이다.

### ReplicaSet
ReplicaSet은 명시된 pod의 개수에 대한 가용성을 보장하기 위해서 사용하는 Object이다. 

ReplicaSet은 필드에 지정된 설정에 따라 pod을 만들고 삭제한다.

새로운 pod을 만들때는 이전에 정의된 template을 따라 pod을 만들게된다.

ReplicaSet은 직접 template으로도 지정이 가능하며, Deployment에도 작성이 가능하다.

### Deployment
Deployment는 pod과 replicaSet에 대한 선언적 설정을 제공한다.

Deployment의 역할은 배포에 관련된 세세한 정보를 정의하고 관리하는 역할은 한다.

배포 전략, 배포시에 몇개의 pod씩 배포를 진행할지, 어떠한 image를 이용한 pod을 생성할것인지 등등 여러가지 설정 정보를 관리한다.

Deployment는 조심히 다루어야한다.

Deployment가 삭제된다면 pod에 대한 관리가 정상적으로 이루어지지 않는다.

Pod이 삭제된다면 Deployment의 정보를 참조해 새로운 pod이 생성되지만, Deployment를 잘못 수정하거나 삭제한다면 한순간에 장애 상황을 마주치게된다.

### StatefulSet
StatefulSet도 마찬가지로 pod을 관리하는 Object이다.

Deployment와 다른 이유는 StatefulSet은 상태를 유지하는점이다.

Deployment는 상태를 유지하지 않는다. 예를들어 Pod이 volume에 적재해놓은 데이터 등등을 언제나 참조해야 한다면 

StatefulSet을 사용하는게 적절하다.

### DaemonSet
Kubernetes는 현재 node의 상태와 pod의 resource 할당량등을 파악해 node에 자동으로 배치한다.

하지만 DaemonSet은 모든 node에 1개씩 동작하도록 구성되는 Object이다.

DaemonSet에는 log수집, 모니터링에 사용하는 pod을 위치시키는것이 적당하다.(APM, fluentd...)

### Service
Kubernetes가 자동으로 pod을 node에 배치하니, 사용자는 어느 node에 pod이 있는지 정확하게 알 수 없다.

그렇다면 loadBalancer나 ip 설정을 어떻게 해야할까? 라는 문제점이 생긴다.

이러한 문제점을 해결할 수 있게 하는 Object가 service이다.

Service는 논리적 단위로 동작하며, 여러 node에 퍼져있는 동일한 pod들의 loadbalancing, 

각각의 Pod을 외부에서 접근이 가능한 상태로 만들어주는 역할을 한다.

Kubernetes에서는 pod에 고유한 IP를 부여하는데 이 IP는 언제 변경될지 모르니 우리는 service를 이용해 pod에 접근하게 된다.

service도 deployment, pod과 마찬가지로 template으로 정의가 가능하다.

### Namespace
물리적으로 동일한 cluster를 기반으로 논리적으로 공간을 분리하는 개념이다.

Namespace를 사용하는 이유는 클러스터의 자원을 여러 사용자들을 구분해 제공하기 위함이다.

쉽게는 java의 package의 개념이라고 생각하면 된다.
