---
toc: true
title: "[Java 기초] 애노테이션"
category:
  - Java
---

이번 포스트에서는 자바의 애노테이션에 대해서 알아보도록 하겠다.

## 목차
- 애노테이션 정의하는 방법
- @Retention
- @Target
- @Documented
- 애노테이션 프로세서

## 애너테이션 정의하는 방법
애너테이션은 평소에는 주석과 다르지 않지만 javadoc.exe에 의해서 읽어지면 특정 프로그램에게 유의미한 정보를 제공한다.

애너테이션을 정의하는 방법은 인터페이스를 정의하는 것과 동일하다.

애너테이션 내의 메서드를 애너테이션의 요소라고 한다.

```java
@interface 애너테이션이름 {
    타입 요소이름(); // 애너테이션의 요소를 선언.
}
```

- 요소의 타입은 기본형, String, enum, Class, 애너테이션만 가능하다.
- $($)안에 매개변수 선언 불가
- 예외 선언 불가
- 요소를 타입 매개변수로 정의 불가
- 애너테이션을 적용할 때 반드시 요소들의 값을 모두 지정.

```java
@interface TestInfo {
    int count();
    String testedBy();
    String[] testTools();
    DateTime testDate(); // 다른 애너테이션을 포함할 수 있다.
}

@interface DateTime {
    String yymmdd();
    String hhmmss();
}

// 요소들의 값 모두 지정
@TestInfo(
    count = 3, testedBy = "Ko",
    testTools = { "JUnit", "AutoTester" },
    testDate = @DateTime(yymmdd = "211227", hhmmss = "214023")
)
public class Example { ... }
```

## 메타 애너테이션
메타 에너테이션이란 애너테이션을 위한 애너테이션이다.

애너테이션으로 애너테이션을 정의할 때 애너테이션의 적용대상, 유지기간등을 지정하는데 사용된다.

### @retention - 애너테이션 유지 기간 지정

**애너테이션 유지 정책**

| 유지 정책 | 의미 |
|---|---|
| RetentionPolicy.**SOURCE** | 소스 파일에만 존재. 클래스파일에는 존재하지 않음. |
| RetentionPolicy.**CLASS** | 클래스 파일에 존재. 기본값. 실행시에 사용불가. |
| RetentionPolicy.**RUNTIME** | 클래스 파일에 존재. 실행시에 사용가능. |

- `SOURCE`
    - 컴파일 시에만 애너테이션을 사용하겠다.
    - 컴파일 후 애너테이션은 없어진다.

- `CLASS`
    - 애너테이션 정보를 바이트 코드에도 남겨두겠다.
    - 클래스파일을 JVM이 실행하면서 클래스로더가 클래스 정보를 메모리에 적재하고 이후에 메모리에서 읽어올 때는 애너테이션 정보를 제외하고 읽어온다.

- `RUNTIME`
    - 애너테이션 정보를 바이트 코드에도 남겨두겠다.
    - 메모리에 적재된 클래스 정보를 읽어올 때 에너테이션 정보를 포함하여 읽어온다.

```java
@Retention(RetentionPolicy.RUNTIME)
```

### @Target - 애너테이션 적용 대상 지정
`@Target`은 애너테이션이 적용가능한 대상을 지정하는데 사용된다.

**@Target으로 지정할 수 있는 애너테이션 적용대상의 종류**

| 대상 타입 | 의미 |
|---------|------|
| ANNOTATION_TYPE | 애너테이션 |
| CONSTRUCTOR | 생성자 |
| FIELD | 멤버변수 $($기본형), enum상수 |
| LOCAL_VARIABLE | 지역변수 |
| METHOD | 메서드 |
| PACKAGE | 패키지 |
| PARAMETER | 매개변수 |
| TYPE | 클래스, 인터페이스, enum |
| TYPE_PARAMETER | 타입 매개변수 |
| TYPE_USE | 타입이 사용되는 모든 곳 $($참조형 매개변수) |

```java
import static java.lang.annotation.ElementType.*;

@Target({FIELD, TYPE, TYPE_USE}) // 적용대상이 FIELD, TYPE, TYPE_USE
public @interface MyAnnotation { } // MyAnnotation을 정의

@MyAnnotation // 적용대상이 TYPE인 경우
class MyClass {
    @MyAnnotation // 적용대상이 FIELD인 경우
    int i;

    @MyAnnotation // 적용대상이 TYPE_USE인 경우
    MyClass mc;
}
```

### @Documented - 애너테이션 정보를 문서에 포함시킨다.
애너테이션에 대한 정보가 `javadoc`으로 작성한 문서에 포함되도록 한다.

기본 애너테이션에는 모두 @Documented 애너테이션이 붙어있다.
