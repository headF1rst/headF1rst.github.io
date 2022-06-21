---
toc : true
title : "[Spring] ArgumentResolver로 토큰 인증 구현"
category :
    - Spring
---

# ArgumentResolver로 토큰 인증 구현

Controller 메서드에 `@PathVariable` 과 같은 어노테이션을 추가하여 인자 값을 넘겨주고는 합니다.

이때 이런 인자로 들어온 값으로 부터 원하는 객체를 만들어내는 일은 `HandlerMethodArgumentHandler` 에 의해서 간접적으로 처리 될 수 있습니다.

## HandlerMethodArgumentHandler의 역할

`HandlerMethodArgumentHandler` 는 어노테이션이나 타입에 따라서 실제 값을 Controller에게 넘겨주는 역할을 합니다.

스프링에서도 기본적으로 여러 `ArgumentResolver` 가 구현되어 있습니다.

### PathVariableMethodArgumentResolver

- `@PathVariable` 어노테이션으로 선언된 인자를 처리하는 **ArgumentResolver**

### RequestParamMethodArgumentResolver

- `@RequestParam` 어노테이션으로 선언된 인자의 실제 값을 지정해준다.

### RequestHeaderMapMethodArgumentResolver

- `@RequestHeader` 어노테이션으로 선언된 인자의 실제 값을 지정해 준다.

이처럼 `HandlerMethodArgumentHandler` 를 사용하면 코드 중복을 줄이고 공통 기능으로 빼서 사용할 수 있는 장점이 있습니다.

## Jwt Token Argument Resolver 만들기

컨트롤러 메서드에서 @`@JwtAuth` 어노테이션이 추가된 인자를 넘겨주면 토큰에서 유저 이메일 정보만을 추출해 오는 Resolver를 만들어 보도록 하겠습니다.

```java
@RestController
public class UserController {
		
			@GetMapping
			public String getUserInfo(@JwtAuth String email) {
					return ....
			} 
```

### Argument Resolver 인터페이스

![스크린샷 2022-06-19 오후 4.07.27.png](https://i.imgur.com/rL5suls.png)

Argument Resolver 인터페이스는 두가지 메서드를 포함합니다.

1. `supportsParameter(MethodParameter parameter);`
    
    요청받은 메서드의 인자에 원하는 어노테이션을 포함하고 있으면 true 반환.
    
2. `resolveArgument(MethodParameter parameter, @Nullable ModelAndViewContainer mavContainer ....`
    
    supportsParameter에서 true를 받은 경우, parameter가 원하는 형태로 정보를 바인딩하여 반환.
    

![스크린샷 2022-06-19 오후 4.15.38.png](https://i.imgur.com/pIxGbFG.png)

위와 같이 resolveArgument에서 헤더에서 토큰을 추출한 다음, 토큰에서 유저의 이메일을 추출해 온 다음, 이메일 정보를 반환해 주도록 구현하였습니다.

이렇게 구현을 하게 되면 Controller가 토큰 검증이라는 책임까지 담당하지 않아도 됩니다.

게다가 어노테이션만으로 유효한 토큰을 사용하는 것이 검증된 사용자가 필요한 정보를 가지고 필요한 객체로 나오기 때문에 편리하고 간단한 구현을 할 수 있게 됩니다.

## Spring Interceptor

Interceptor를 `HandlerInterceptor` 인터페이스를 구현하여 사용할 수 있습니다.

이름대로 Handler의 실행을 가로체는 인터페이스 입니다.

`HandlerInterceptor` 인터페이스는 `preHandle` , `postHandle` , `afterCompletion` 을 구현하도록 명시되어있습니다.

- **preHandle**
    
    조건에 맞는지 boolean을 반환. true면 실행하고 false면 실행 중단.
    
- **postHandle, afterCompletion**
    
    실행 후에 추가적으로 공통된 처리를 하고 싶을때 사용.
    

Interceptor에는 적용하고자 하는 url을 직접 추가해 주거나 제외할 수 있습니다.

![스크린샷 2022-06-20 오후 4.51.08.png](https://i.imgur.com/ZbEzFur.png)

만일 사용자가 올바른 유저인지, 관리자인지, 작성자인지에 따라 요청에 대한 권한이 다르게 부여된다면, Iterceptor에 메서드를 추가해 검증을 거쳐야만 요청을 실행할 수 있게 할 수 있습니다.

어떠한 요청을 실행한 후에 공통된 처리가 필요한 경우에도, `postHandle` , `afterCompletion` 과 같은 메서드를 적용할 수 있습니다.

인터셉터를 적용하게 되면 코드의 중복을 제거하고, Controller에 직접적인 책임을 주지 않을수 있다는 장점이 있습니다.

## Argument Resolver 동작 방식

내부적으로 Argument Resolver가 어떻게 호출되고 동작하는지 알아보도록 하겠습니다.

1. 요청이 들어온다.
2. filter가 작동한다.
3. DispatcherServlet에 전달된다.
    1. DispatcherServlet이란, Spring의 핵심 객체로, Client의 요청을 받고 응답을 주기까지의 모든 역할을 담당합니다.
4. DispatcherServlet은 HandlerMapping을 통해 요청을 처리할 Controller를 찾는다.
    1. 이때 Controller를 찾고 Interceptor가 확인할 url과 일치하면 Intercptor의 preHandle이 실행된다.
5. DispatcherServlet은 Controller를 실행해줄 HandlerAdapter를 찾는다.
    
    이때, Adapter를 찾고 handle을 실행하기 위해 필요한 파라미터를 생성하기 위해 resolver는 실행된다.
    
6. HandlerAdapter는 Controller를 실행한다.
    1. 이때 Interceptor의 postHandle이 실행된다.
7. DispatcherServlet은 실행한 결과를 ViewResolver에게 전달한다.
8. ViewResolver는 View에 전달한다.
    1. 이때 Interceptor의 afterCompletion이 실행된다.
9. DispatcherServletdms View로 부터 받은 정보를 Client에 전달한다.
10. 응답을 반환한다.

**참고 자료** 📚

- [Custom HandlerMethodArgumentResolver 만들어보기](https://blog.advenoh.pe.kr/spring/HandlerMethodArgumentResolver-%EC%9D%B4%EB%9E%80/)

- [Spring ArgumentResolver와 Interceptor](https://tecoble.techcourse.co.kr/post/2021-05-24-spring-interceptor/)