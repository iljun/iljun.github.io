---
layout: post
title: Kubernetes
subtitle: pod
---
# Pod
pod이란 kubernetes에서 생성되고 관리될 수 있는 가장 작은 단위이다.

## Life Cycle

### Phase
pod은 상태를 나타내는 `PodStatus`라는 Object로 표현된다.

- pending
    - kubernetes에 의해 pod이 생성되었지만, 컨테이너 이미지 생성이 완료되지 않은 상태(network를 통한 이미지 다운로드 시간도 포함)
- running
    - pod의 container가 정상 생성이 되었으며, node에서 동작중이란걸 표현
- Succeeded
    - container가 성공적으로 종료되었으며, restart되지 않는다.
- Failed
    - container가 실패로 종료된 경우를 의미한다.
- Unknown
    - pod의 상태를 정확하게 파악 할 수 없는 경우, 일반적으로 pod과의 통신의 문제일 가능성이 높다.
    
### Condition
Pod이 가지는 `PodStatus`가 변경된 조건에 대하여 `PodConditions`를 가진다.
- lastProbeTime
    - condition이 변경된 마지막 시간을 기록
- lastTransitionTime
    - podStatus가 변경된 시점의 timestamp
- message
    - 세부 정보를 표시한 message
- reason
    - condition이 변경된 이유 
- status
    - `true`, `false`, `Unknown`으로 표시되는 상태값
- type
    - PodScheduled
        - pod이 node에 schedule이 완료된 상태
    - Ready
        - pod이 request를 받아들일 수 있으며, 로드밸런싱 poll에 추가된 상태
    - Initialized
        - 초기화 container가 완료된 상태
    - ContainersReady
        - pod의 container가 준비된 상태

### Probe
pod의 상태가 어떠한지 판단하는 action이다.

상태를 파악하기 위한 type은 3가지이다. 
- ExecAction
    - 컨테이너 내부의 명령어를 실행해 확인하는 방법이다. 상태코드가 0이라면 성공으로 간주한다.
- TCPSocketAction
    - 컨테이너의 IP주소에 대해서 TCP검사를 진행, port가 활성화 되어 있으면 성공으로 판단
- HttpGetAction
    - HttpGet 요청을 통해 컨테이너의 상태를 파악, 응답이 200보다 크고 400보다 작으며 성공으로 판단
    
probe는 3가지 상태를 가진다.
- Success
    - 컨테이너의 상태가 정상
- Failure
    - 컨테이너의 상태가 비정상
- Unknown
    - 컨테이너의 상태가 알 수 없음
    
kubelet은 3가지 종류의 probe가 가능하다.
- livenessProbe
    - 컨테이너가 동작 중인지를 판단한다.
    - 만약 livenessProbe가 실패하면 pod을 삭제하고 새롭게 생성한다.
- readinessProbe
    - 컨테이너가 요청을 처리할 준비가 되었는지를 확인하는 probe이다.
    - readinessProbe가 실패한다면, pod이 속해있는 모든 서비스에서 pod의 IP를 제거해 request가 들어오지 않게 만들어 버린다.
- startupProbe
    - 컨테이너 내부의 어플리케이션이 모두 시작되었는지를 파악한다.
    - startupProbe가 성공이 아닐경우 다른 모든 probe는 활성화되지 않는다.(default 30s)
    - startupProbe가 실패하면 pod을 죽이고 새롭게 생성한다.
    
### RestartPolicy
pod이 문제가 생겼을때 재시작에 대한 정책을 설정이 가능하다.

재시작에 대한 정책은 3가지이다. 
- Always
    - 언제나 재시작을 한다.
- OnFailure
    - 실패시에만 재시작을 한다.
- Never
    - 어느 상황에서도 재시작을 하지 않는다.

pod은 언제나 재시작에 대한 정책을 가지고 있다.


