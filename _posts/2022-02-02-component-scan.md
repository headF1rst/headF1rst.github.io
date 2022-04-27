---
toc: true
title: "수동, 자동 빈 등록 $($컴포넌트 스캔)"
category:
  - Spring
---
**DI 컨테이너**는 객체를 생성하고 관리하며 의존관계를 주입해 주는 역할을 한다.

스프링 컨테이너 또한 이러한 DI 컨테이너이며, 스프링 컨테이너를 통해서 객체를 생성하고 의존관계 주입을 수행한다.

개발자는 스프링 컨테이너에 객체를 스프링 빈으로 등록하고, 스프링 컨테이너에서 스프링 빈을 찾아서$($getBean) 사용하게 된다.

스프링 컨테이너에 객체를 빈으로 등록하는 방법에는 수동으로 등록하는 방법과 스프링이 자동으로 직접 빈을 등록해주는 방법이 있다.

## 수동 빈 등록
구성 정보 클래스 안에 `@Bean` + 메서드로 스프링 빈을 등록.

먼저 제어의 흐름을 갖고 있는 설정 클래스에 `@Configuration` 에너테이션을 붙여서 해당 구성 클래스가 설정 정보 클래스라는 것을 명시해준다.

`@ApplicationContext`: 스프링 컨테이너 $($인터페이스)

`AnnotationConfigApplicationContext(className.class)`
에너테이션 기반의 자바 설정 클래스로 컨테이너를 생성한다는 의미의 구현체이다. $($XML 기반도 가능)

파라미터 값으로는 `@Configuration`이 붙은 설정 클래스가 주어지며 파라미터로 주어진 설정 클래스 또한 빈으로 컨테이너에 등록된다.

`@Bean`
설정 클래스에 `@Bean`을 붙여준 메서드는 설정 클래스가 `AnnotationConfigApplicationContext(className.class)`의 파라미터값으로 주어지면서 컨테이너가 생성될 때, 스프링 빈으로 등록된다.

```java
@Configuration
public class AppConfig {

    @Bean
    public MemberService memberService() {
        return new MemberServiceImpl(memberRepository());
    }
```

`.getBean("빈 이름", 빈 타입)`

해당 메서드를 통해, 스프링 컨테이너에서 필요한 스프링 빈(객체)를 찾는다.

스프링 빈을 통해 DI를 수행한다.

```java
ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);

// 의존관계 주입
MemberService memberService = ac.getBean("memberService", MemberService.class);
```

## 컴포넌트 스캔을 통한 자동 빈 등록
`@ComponentScan`, `@Component` 를 통해, 자동으로 클래스를 빈으로 등록.

컴포넌트 스캔은 스프링이 직접 `@Component` 에너테이션이 붙은 모든 클래스를 조회해서 빈으로 등록해주는 기능이다. 

설정 클래스 위에 `@ComponentScan` 에너테이션을 붙이면 해당 설정 클래스에 `@Component`가 붙은 클래스들을 자동으로 빈 등록해준다.

```java
@Configuration
@ComponentScan(
        excludeFilters = @Filter(type = FilterType.ANNOTATION, classes = Configuration.class))
public class AutoAppConfig {
}

...
```

```java
@Component
public class MemoryMemberRepository implements MemberRepository() {}
```

설정 클래스에 $($Config) 수동으로 빈 등록 해주지 않아도 원하는 클래스를 빈으로 등록할 수 있기 때문에 컴포넌트 스캔을 사용하여 설정 코드를 줄일수 있다.

스프링 빈의 기본 이름은 클래스명이 사용되며 맨 앞 글자만 소문자를 사용한다.

### 컴포넌트 스캔 탐색 시작 위치 지정
별도로 페키지 탐색 위치를 지정해 주지 않으면 `@ComponentScan`이 붙은 설정 정보 클래스의 패키지가 컴포넌트 스캔 탐색 시작 위치가 된다.

보통 `@ComponentScan`을 적용한 설정 정보 클래스의 위치를 프로젝트 최상단에 위치시켜서 하위 패키지들이 모두 컴포넌트 스캔 대상이 되게 한다.

!['ComponentScan Location'](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/9e85f864-64bc-4a9f-801f-189a979afd06/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-01-27_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.35.10.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220202%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220202T145622Z&X-Amz-Expires=86400&X-Amz-Signature=1a3cce86f15bc9aad13dce9073c2b839e737f40b29611d02ecc04db61bacd3d3&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202022-01-27%2520%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB%252011.35.10.png%22&x-id=GetObject)

추가)  @SpringBootApplication 을 프로젝트 시작 루트에 두는 것이 관례다.

### 컴포넌트 스캔 대상

- `@Component`
- `@Controller` : 스프링 MVC 컨트롤러로 인식
- `@Service` : 비즈니스 계층 인식
- `@Repository` : 데이터 접근 계층. 데이터 계층의 예외를 스프링 예외로 변환
- `@Configuration` : 스프링 구성 정보로 인식. 싱글톤 유지

$($위의 에네테이션을 열어보면 컴포넌트 에노테이션이 붙어있다)

### 컴포넌트 스캔 필터

- `includeFilters` : 컴포넌트 스캔 대상을 추가 지정.
- `excludeFilters` : 컴포넌트 스캔 제외 대상을 지정.