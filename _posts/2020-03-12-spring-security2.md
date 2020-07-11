---
layout: post
title: Spring Security
---

# Spring Security Flow

1. filterChain 내부에 AbstractAuthenticationProcessingFilter을 통해 user의 request가 전달된다.

2. request를 통해 아직 인증 처리가 되지 않은 Authentication 객체를 AuthenticationManager에 주입한다.

3. AuthenticationManager에서 인증 단계를 거친다.

4. 인증에 성공하면 SecurityContextHolder내부에 인증된 객체를 저장하거나, AuthenticationSuccessHandler를 실행한다.

5. 인증에 실패하면 SecurityContextHolder를 비우고, AuthenticationFailureHandler를 실행한다.

 