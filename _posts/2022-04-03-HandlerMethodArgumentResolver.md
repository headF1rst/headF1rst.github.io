---
toc : true
title : "[Spring] HandlerMethodArgumentResolver로 Header값 받기"
category : 
    - Back_End
---

`HandlerMethodArgumentResolver` : Controller 단에서 특정 조건에 맞는 파라미터가 있을 때 원하는 값을 바인딩 해주는 인터페이스.

## HandlerMethodArgument 사용하여 Jwt 엑세스 토큰 받기

### 1. Annotation 작성

`JwtAuth` 에너테이션을 작성하였다.

![JwtAuth](https://i.imgur.com/OzIDzlp.png)

