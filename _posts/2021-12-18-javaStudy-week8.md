---
toc: true
title: "[Java 기초] 예외처리"
category:
  - Java
---

이번 포스트에서는 자바의 예외처리에 대해서 알아보도록 하겠다. 👨‍🏫

## 📋 목차
- 예외 처리 방법
- Exception과 Error의 차이는?
- 예외 정보 출력
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

try블럭에서 예외가 발생하면, 예외가 발생한 이후의 try블럭의 문장들은 수행되지 않는다.

#### catch 블럭
`try`블럭에서 예외가 발생하면 발생한 예외에 해당하는 클래스의 인스턴스가 만들어진다.

`catch`블럭을 차례로 내려가면서 `catch`블럭의 괄호$($) 내에 선언된 참조변수의 종류와 생성된 예외클래스의 인스턴스에 `instanceof`연산자를 통해 검사를 하며 `true`인 검사결과의 catch블럭을 수행하게 된다.

```java
catch (ArithmeticException e) {
  // ArithmeticException이 발생하면 실행되는 코드
  System.out.println("0");
}
```

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

### throw로 예외 발생시키기
> 1. Exception e = new Exception("고의로 예외 발생");
> 2. throw e;

> 1. throw new Exception("고의로 예외 발생");

`try`블럭 내에서 Exception 참조변수로 인스턴스를 생성하고 `throw`로 에러를 던져주면 예외가 발생한다.

인스턴스를 생성할때 문자열 형태의 에러메세지를 생성자에 넣어 주면 인스턴스에 메시지가 저장된다.

#### 예제

```java
class Example {
    public static void main(String[] args) {
        try {
            throw new Exception("고의로 예외 발생.");
        } catch(Exception e) {
            System.out.println("에러 메시지 : " + e.getMessage());
            e.printStackTrace();
        }
        System.out.println("시스템이 정상적으로 종료되었습니다.");
    }
}
```
**출력**
```
에러 메시지 : 고의로 예외 발생.
시스템이 정상적으로 종료되었습니다.
java.lang.Exception: 고의로 예외 발생.
	at week1.Example.main(Example.java:6)
```

## throws로 메서드에 예외 선언하기
`try-catch`문이 아닌 메서드에 예외를 선언하기 위해서는 메서드 선언부에 `throws`를 사용해서 발생할 수 있는 예외를 적어줘야한다.

> void method() **throws** Exception1, Exception2, ... ExceptionN{
  // 메서드 내용
}

### 예제
```java
class Example {
    public static void main(String[] args) {
        try {
            method1();
        } catch (Exception e) {
            System.out.println("main메서드에서 예외가 처리되었습니다.");
            e.printStackTrace();
        }
    }

    static void method1() throws Exception {
        throw new Exception();
    }
}
```
**출력**
```
main메서드에서 예외가 처리되었습니다.
java.lang.Exception
	at week1.Example.method1(Example.java:14)
	at week1.Example.main(Example.java:6)
```

위의 출력 결과로 보아 2개의 메서드가 호출스택에 있었고, 제일 윗줄 method1에서 예외가 발생하였으며 method1에서 자신을 호출한 main 메서드에 예외를 전달하였으며 main 메서드에서 try-catch문으로 예외를 처리한 것을 볼수있다.

예외 처리는 예외가 발생한 메서드에서 처리할수도 있고 호출한 메서드에서 처리할수도 있고 혹은 두 메서드가 예외처리를 분담할 수도 있다.

### finally 블럭
예외 발생여부 상관없이 실행되어야할 코드를 포함.

`try-catch-finally`순으로 구성된다.

#### 예제
```java
class Example {
    public static void main(String[] args) {

        Example.method1();
        System.out.println("method1() 수행이 끝나고 main으로 돌아왔습니다.");
    }

    static void method1() {
        try {
            System.out.println("method1");
            return;
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            System.out.println("method1()의 finally가 수행되었습니다.");
        }
    }
}
```
**출력**
```
method1
method1()의 finally가 수행되었습니다.
method1() 수행이 끝나고 main으로 돌아왔습니다.
```

try블럭에서 return문이 실행되더라도 finally블럭이 수행된 다음에 실행 중인 메서드를 종료한다.

## 예외 정보 출력

### printStackTrace()
예외발생 당시의 Call Stack에 있던 메서드의 정보와 예외 메시지를 출력.

### getMessage()
발생한 예외클래스의 인스턴스에 저장된 메세지를 출력

## 예외 계층 구조

![예외 계층 구조](/assets/images/Back_End/javaExceptionFlow.png)


**Exception class들 :** `사용자`의 실수와 같은 외적인 요인에 의해 발생하는 예외.

컴파일러가 예외처리를 확인하여 **checked예외** 라고도 한다.

반드시 try-catch문을 통해 예외처리를 해주어야 한다.

**RuntimeException class들 :** `프로그래머`의 실수로 발생하는 예외

컴파일러가 예외처리를 확인하지 않아 **unchecked예외** 라고도 한다.

---

모든 예외 클래스는 `Exception`클래스의 자손이기 때문에 Exception클래스의 참조변수를 catch$($)에 선언하면 어떤 예외던지 처리가 가능하다.

```java
class Example {
    public static void main(String[] args) {
        System.out.println(1);
        System.out.println(2);
        try {
            System.out.println(3);
            System.out.println(0 / 0);
            System.out.println(4); // 실행되지 않는다.
        } catch (ArithmeticException e) {
            if (e instanceof ArithmeticException) {
                System.out.println("true");
            }
            e.printStackTrace();
            System.out.println("예외 메세지 :" + e.getMessage());
        } catch (Exception e) { // ArithmeticException을 제외한 모든 예외가 처리된다.
            e.printStackTrace();
            System.out.println("예외 메세지 :" + e.getMessage());
        }
        System.out.println(6);
        System.out.println("시스템이 정상적으로 종료되었습니다.");
    }
}
```
**출력**
```
1
2
3
true
예외 메세지 :/ by zero
6
java.lang.ArithmeticException: / by zero
	at week1.Example.main(Example.java:9)
시스템이 정상적으로 종료되었습니다.
```

## 커스텀한 예외 만드는 방법
기존의 예외클래스를 상속받는 새로운 사용자 지정 예외클래스를 생성할 수 있다.

```java
class MyException extends RuntimeException {
    private final int ERR_CODE;
    
    MyException(String msg, int errCode) { // 생성자
        super(msg); // 조상인 RuntimeException클래스의 생성자를 호출
        ERR_CODE = errCode;
    }
    
    MyException(String msg) {
        this(msg, 100);
    }
    
    public int getErrCode() { // 에러코드를 얻을 수 있는 메서드
        return ERR_CODE;
    }
}
```

---

> 참조 <br>
> Java의 정석 $($남궁 성) <br>
> [whiteship 자바 라이브 스터디](https://github.com/whiteship/live-study)