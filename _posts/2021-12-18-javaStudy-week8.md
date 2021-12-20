---
toc: true
title: "[Java 기초] 예외처리"
category:
  - Java
---

이번 포스트에서는 자바의 예외처리에 대해서 알아보도록 하겠다.

---

## 목차
- 예외 처리 방법
- Exception과 Error의 차이는?
- 자바가 제공하는 예외 계층 구조
- RuntimeException과 RE가 아닌 것의 차이는?
- 커스텀한 예외 만드는 방법

## 예외 처리 하는 방법
예외처리의 목적은 프로그램의 비정상적인 종료를 막고 정상적인 실행상태를 유지할 수 있도록 하는것이다.

- **런타임 에러 :** 프로그램 실행시에 발생하는 에러
  - **에러 :** 코드에 의해 수습될 수 없는 심각한 오류
  - **예외 :** 코드에 의해 수습될 수 있는 미약한 오류 

### try - catch문

```java
try {
  // 예외가 발생할 가능성이 있는 문장을 넣는다.
} catch (Exception1 e1) {
  // Exception1이 발생했을 경우, 이를 처리하기 위한 문장이 온다.
} catch (Exception2 e2) {
  // Exception2가 발생했을 경우, 이를 처리하기 위한 문장이 온다.
} catch (ExceptionN eN) {
  // ExceptionN이 발생했을 경우, 이를 처리하기 위한 문장이 온다.
}
```

하나의 `try`블럭 다음에는 여러개의 `catch`블럭이 올 수 있다.

이 중 발생한 예외와 일치하는 하나의 `catch`블럭만 수행된다.

catch블럭 내의 코드에서도 예외가 발생할 수 있기 때문에 try, catch블럭에 또 다른 try - catch문이 포함될 수 있다.

#### 예제

다음은 100을 0~9의 랜덤한 값으로 나눈 결과를 출력하는 코드이다.

이때 실수를 0으로 나누는것은 금지되어 있기 때문에 예외 $($ArithmeticException)가 발생하고 프로그램이 비정상적으로 종료된다.

때문에 예외처리구문을 추가해서 프로그램이 예외에 의해 종료되지 않도록 수정해야한다. 

```java
class Example {
    public static void main(String[] args) {
        int number = 100;
        int result = 0;

        for (int i = 0; i < 10; i++) {
            try {
                result = number / (int) (Math.random() * 10);
                System.out.println(result);
            } catch (ArithmeticException e) {
              // ArithmeticException이 발생하면 실행되는 코드
                System.out.println("0");
            }
        }
    }
}
```

**출력**
```
50
0 <- ArithmeticException이 발생해서 0이 출력됨.
100
0
14
11
0
100
20
11
```

예외처리를 하지 않았다면 첫번째 줄만 수행되고 예외가 발생한 두번째 줄 부터 프로그램이 비정상적으로 종료되었을거다.

## 예외 계층 구조

![예외 계층 구조](/assets/images/Back_End/javaExceptionFlow.png)


**Exception class들 :** `사용자`의 실수와 같은 외적인 요인에 의해 발생하는 예외

**RuntimeException class들 :** `프로그래머`의 실수로 발생하는 예외

