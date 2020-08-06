---
layout: post
title: Kubernetes
subtitle: Kubernetes Master 
---
# Master
Cluster 전체를 관리 하는 시스템으로, API 서버, 스케쥴러, 컨트롤러 매니져, etcd 로 구성되어 있다.

## API 서버
Kubernetes는 모든 명령과 통신을 API를 통해서 진행한다.

모든 API를 담당하는게 Master의 API 서버이다.

## Etcd
Kubernetes Cluster의 데이터베이스 역할을 담당한다.

클러스터의 설정 정보, 상태 정보가 저장되는 key-value 형식의 저장소이다.

## 스케쥴러 
Kubernetes의 Object들을 적절한 Node에 배치하는 역할을 담당한다.

## Controller Manager
중앙에서 Replica , Service , Volume에 관련되어 있는 Controller를 생성하고 각 Node에 배치하는 역할을 담당한다.
     
## DNS
Kubernetes에는 resource가 생겨날때 마다 새로운 IP를 할당한다.

동적으로 생성되기 때문에 언제나 IP가 동일 할 수 없고 이것을 관리하기 위해서 내부 DNS를 운영한다.

## Kubelet
Node에 배포되는 Agent로 API 서버와 통신하며, 노드에 실행할 명령을 받아들이고, 노드의 상태를 API 서버로 전달한다.

## Kube-proxy
Node에 들어오는 트래픽을 적잘하게 라우팅하고, 로드밸런싱을 한다.

Node의 트래픽을 프록시하여, Node와 master간의 통신을 관리한다.



