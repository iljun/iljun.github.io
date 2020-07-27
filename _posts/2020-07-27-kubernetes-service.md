---
layout: post
title: Kubernetes
subtitle: Service
---
# Service
Service란 동일한 pod들에 접근하기 위한 정책을 가지고 있는 논리적인 단위이다.

Kubernetes에 running중인 pod들이 언제나 동일하다는 보장은 하지 못한다.
pod자체에 문제가 생겨 삭제되고, 새로운 pod이 생성되었을 수 있다.

Client가 pod에 Request를 날려야하는데 결국 domain이나 IP를 알아야 Request 요청이 가능하다.

하지만 Client가 언제든지 변경될 수 있는 pod의 ip를 알 수 있는 방법이 없다.

이러한 문제를 해결해주는것이 `Service` 객체이다.

Service가 동일한 pod을 판단하는 기준은 바로 `label`이다.

`label`은 식별자 역할은 하며 추후에 정리할 예정이다.

pod의 port와 service의 ports와 mapping할 수 있다.

즉 pod에는 `8080`으로 open되어 있는 port를 service의 ports로 `80`으로 mapping이 가능하다.

service의 기본 protocol은 TCP이며, 다른 protocol로 지정할 수 있다.

결국 Service는 request를 어디로 어떻게 routing할지를 판단하는 Object이다.

## Endpoins
모든 pod들이 kubernetes에 존재하지 않는 경우도 있다.

- 점진적으로 kubernetes로 service를 이관하는경우
- 다른 namespace에 존재하는 service를 참조하느 경우
- 외부 환경의 service와 연동해야할때

이러한 경우에는 `Label`을 지정할 수 없다.

그러면 service객체를 어떻게 이용해야 할까?

`Label`없이도 service객체를 생성 할 수 있다. 하지만 service는 아무 행동을 하지 못한다.

이러한 경우에 `Endpoint` 객체를 생성하여 service와 연결이 가능하다.

subsets을 이용하여 ip와 port를 정의해주면 service객체가 동작이 가능하다.


## Service Types
Service를 cluster 외부 IP주소에 노출해야 할 경우가 있다. ex) FE

Kubernetes는 `Service Types`을 이용해 Service의 종류를 지정하게 해준다.

- ClusterIP (Default)
    - 서비스를 클러스터 내부 IP에 노출시키는 방법이다. 동일 클러스터 내부에서만 접근이 가능하다.
- NodePort
    - 고정 포트로 각 노드의 IP에 서비스를 노출시킨다. `NodeIp:NodePort`를 이용해 접근이 가능하다.
- LoadBalancer
    - 클라우드 환경의 loadbalancer를 사용하여 service를 외부에 노출시키는 방법
- ExternalName
    - CNAME을 이용하여, service를 externalName의 필드에 노출시킨다(foo.bar.example.com)
    - kube-dns 1.7 or CoreDNS 1.7이상 필요
- Ingress
    - service의 type은 아니지만 클래스터 내의 서비스에 대한 접근을 관리하는 object이다.
    - Cluster 외부에서 Cluster 내부로 HTTP와 HTTPS 경로를 노출하는 역할이다.    
