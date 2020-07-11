---
layout: post
title: Spring Security
---

# SecurityContextHolder
Spring Security의 인증 모델의 핵심이며, `SecurityContext`를 포함한다.

SecurityContextHolder 내부에 SecurityContext가 포함되며, SecurityContext 내부에는 Authentication 객체가 들어 있다.

SecurityContextHolder 내부가 비워져 있지 않으면, 인증이 된 사용자로 판단한다.

SecurityContextHolder는 ThreadLocal을 사용하여 구현되어 있으므로 각 Thread별로 독립적이며, 동일한 Thread에서는 동일한 인증정보를 취득할 수 있다.

SpringSecurity의 FilterProxyChain은 SecurityContext의 안정적인 삭제를 보장한다.

즉, SpringSecurity를 사용하는 입장에서는 동일한 Thread에서는 언제나 인증정보가 동일하다고 판단할 수 있으며, 인증정보의 삭제를 신경쓰지 않아도 된다.

하지만 Web에서만 적절한 방법이며 Swing Client의 경우는 JVM의 모든 Thread가 동일한 SecurityContextHolder를 사용하기 때문에 적절하지 않을 수 있다.


# Authentication
Authentication 객체는 크게 2가지 목적이 있다.

1. 사용자 인증을 진행하기 위해서 AuthenticationManger에 전달하는 Object
    - 아직 인증이 완료되지 않은 상태이므로 `isAuthenticated()`에서는 `false`를 반환한다. 
2. 현재 인증된 사용자를 나타내는 객체

Authentication 객체 내부에는 3가지 요소가 포함되어 있다.
1. Principal
    - Id/Password를 이용해 인증을 진행할때 사용자를 식별하는 Key
    - UserDetails Service의 instance
2. Credentials
    - 암호화 같이 노출되면 안되는 중요한 값이다.
    - 노출되지 않도록 인증이 완료되면 Authentication 내부에서 삭제한다.
3. authorities
    - 권한을 나타낸다. 

# GrantedAuthority
부여된 권한을 나타낸다.

Collection 형태로 여러개의 권한을 가질 수 있다.

이 GrantedAuthority를 통해 다양한 곳에서 권한을 이용한 제어가 가능하다.

일반적으로 `UserDetailsService`에서 load한다.

# AuthenticationManager
SpringSecurity의 Filter가 어떻게 인증을 처리할지에 대한 Interface이다.

AuthenticationManager를 호출한 후 인증 정보를 SecurityContextHolder에 저장한다.

어떻게든 구현이 가능하지만 가장 일반적인 구현은 `ProviderManager`이다. 

# ProviderManager
AuthenticationManager를 구현한 가장 일반적인 구현체이다.

AuthenticationProvider를 Collection형태로 가지고 있으며,

각각의 AuthenticationProvider에서는 인증을 성공, 실패, 또는 보류하며 다른 AuthenticationProvider에 인증을 위임할 수 있다.

즉 AuthenticationManager 마다 서로 다른 인증 방법을 구현할 수 있다.

만약 모든 AuthenticationProvider에서 인증을 진행 할 수 없으면, `ProviderNotFoundException`이 발생한다.

# AuthenticationProvider

인증을 진행하는 실제 구현체이다.

ProviderManager에 여러개의 AuthenticationProvider가 주입되며, 각각의 인증을 담당한다.
