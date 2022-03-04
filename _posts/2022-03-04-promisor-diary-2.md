---
toc: true
title: "프로젝트 일기 2/16 패키지 구조는 어떻게 가져가야 할까?"
category:
  - etc
---
프로젝트 환경설정을 진행하면서 패키지 구성을 어떻게 하는게 좋을지 고민이 되었다.  🤔

패키지 구성은 크게 계층형과 도메인형이 있는데 각각의 구조와 장단점을 간단하게 알아보자면 다음과 같다.

## 계층 구조

```json
└── src
      ├── main
      │   ├── java
      │   │   └── com
      │   │       └── example
      │   │           └── demo
      │   │               ├── DemoApplication.java
      │   │               ├── config
      │   │               ├── controller
      │   │               ├── dao
      │   │               ├── domain
      │   │               ├── exception
      │   │               └── service
      │   └── resources
      │       └── application.properties
```

### 장점 👍

- 프로젝트의 전체적인 구조를 빠르게 파악 가능.
- 각 패키지간 cyclic dependency 발생 가능성 적음.
- 중복 제거 용이

### 단점 👎

- 디렉터리에 클래스가 너무 많이 모임.
- 기능 단위 분리가 어려움.

## 도메인 구조

```json
└── src
      ├── main
      │   ├── java
      │   │   └── com
      │   │       └── example
      │   │           └── demo
      │   │               ├── DemoApplication.java
      │   │               ├── coupon
      │   │               │   ├── controller
      │   │               │   ├── domain
      │   │               │   ├── exception
      │   │               │   ├── repository
      │   │               │   └── service
      │   │               ├── member
      │   │               │   ├── controller
      │   │               │   ├── domain
      │   │               │   ├── exception
      │   │               │   ├── repository
      │   │               │   └── service
      │   │               └── order
      │   │                   ├── controller
      │   │                   ├── domain
      │   │                   ├── exception
      │   │                   ├── repository
      │   │                   └── service
      │   └── resources
      │       └── application.properties
```

### 장점 👍

- 관련된 코드들이 응집.
- 기능 단위 분리에 유리.

### 단점 👎

- 프로젝트 이해도가 낮으면 구조파악 어려움.
- 중복 코드 발생 가능.
- 패키지간에 cyclic dependency 발생 가능성 높음.

## 다른 프로젝트는 어떤 구조일까? 👀

이런 고민이 생길때면 다른 사람의 코드를 자주 참고하는 편이다.

먼저 스프링 공식 예제 프로젝트인 `petclinic` 을 살펴보았다.

```json
└── petclinic
						├── model
						|			├── BaseEntity.java
						|			├── NamedEntity.java
						|			└── Person.java
						|
				    ├── owner
						|			├── Owner.java
						|			├── OwnerController.java
						|			├── OwnerRepository.java
						|     ├── ...
						|
						|
						├── system
						├── vet
						├── PetClinicApplication.java
```

petclinic 패키지 구조 또한 ower 패키지 내에 도메인, 컨트롤러, 레포지토리로 구성된 도메인형 구조였다.

이 외에도 여러 사람들의 프로젝트를 살펴보았고 대부분이 도메인 구조를 체택해서 사용하고 있었다.

## 결론

결론부터 말하자면 **도메인형**으로 패키지 구성을 가져는것으로 정했다.

과거에는 monolithic 하게 개발하는 추세였고 계층간의 역할을 구분하는게 더 중요시 생각되던 아키텍쳐 기반으로 계발이 이루어졌기 때문에 계층형이 더 적합했을 것이라 생각하지만  최근에 MSA(Micro Service Architecture)에 대한 논의도 많이 되고있고, 각 기능별로 응집력을 높인다는 관점에서 기능 단위로 나눈 도메인 구조가 더 적합하다고 생각된다.

MSA로 도메인 별로 서로 다른 팀이 분할을 해서 작업을 한다 해도 패키지 별로 나누어서 작업을 할수 있기 때문에 더 편리할듯 하다.

도메인 구조는 도메인 별로 기능이 응집되어 있고 좀더 객체지향적 설계와 ORM에 적합하다고 생각한다.

뿐만 아니라 패키지가 도메인 별로 관련 코드가 응집되어 있기 때문에 원하는 클래스를 찾기가 쉬워졌다.

```json
── src
      ├── main
      │   ├── java
      │   │     └── promisor
      │   │           └── promisor
      │   │               ├── PromisorApplication.java
      │   │               ├── domain
      │   │               │   ├── group
      │   │               │   │   ├── api
      │   │               │   │   ├── application
      │   │               │   │   ├── dao
      │   │               │   │   ├── domain
      │   │               │   │   ├── dto
      │   │               │   │   └── exception
      │   │               │   ├── member
      │   │               │   │   ├── api
      │   │               │   │   ├── application
      │   │               │   │   ├── dao
      │   │               │   │   ├── domain
      │   │               │   │   ├── dto
      │   │               │   │   └── exception
      │   │               │   └── model
      │   │               │       ├── Address.java
      │   │               │       ├── Email.java
      │   │               │       └── Name.java
      │   │               ├── global
      │   │               │   ├── common
      │   │               │   │   ├── request
      │   │               │   │   └── resonse
      │   │               │   ├── config
      │   │               │   │   ├── SwaggerConfig.java
      │   │               │   │   ├── properties
      │   │               │   │   ├── resttemplate
      │   │               │   │   └── security
      │   │               │   ├── error
      │   │               │   │   ├── ErrorResponse.java
      │   │               │   │   ├── GlobalExceptionHandler.java
      │   │               │   │   └── exception
      │   │               │   └── util
      │   │               └── infra
      │   │                   ├── email
      │   │                   └── 
      │   │                       ├── 
      │   │                       ├── 
      │   │                       └── d
      │   └── resources
      │       ├── application-dev.yml
      │       ├── application-local.yml
      │       ├── application-prod.yml
      │       └── application.yml
```

## 프로젝트 참고 자료 📚

- [https://www.popit.kr/spring-guide-directory-가이드/](https://www.popit.kr/spring-guide-directory-%EA%B0%80%EC%9D%B4%EB%93%9C/)
- [https://slipp.net/questions/36](https://slipp.net/questions/36)
- [http://www.javapractices.com/topic/TopicAction.do?Id=205](http://www.javapractices.com/topic/TopicAction.do?Id=205)