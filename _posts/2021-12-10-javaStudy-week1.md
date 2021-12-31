---
toc: true
title: "[Java 기초] JVM은 무엇이며 자바 코드는 어떻게 실행하는 것인가."
category:
  - Java
---

이번 포스트에서는 자바 소스 파일 $($.java)을 JVM으로 실행하는 과정을 알아보도록 하겠다.

## 목차

- JVM이란 무엇인가
- 컴파일 하는 방법
- 실행하는 방법
- 바이트코드란 무엇인가
- JIT 컴파일러란 무엇이며 어떻게 동작하는가
- JVM 구성 요소
- JDK와 JRE의 차이

<small>해당 목차는 백기선님의 자바 스터디 1주차 과제에서 가져왔습니다.</small>

---

## JVM이란 무엇인가

> "Write once, run anywhere. (한 번 작성하면 어디서든 실행된다.)"

JVM은 **Java Virtual Machine**의 줄임말로 Java 어플리케이션이 OS에 독립적으로 되는것을 가능하게 한다.

Java 어플리케이션은 JVM하고만 통신하고 JVM이 Java 어플리케이션으로 부터 전달받은 명령을 해당 OS가 이해할 수 있도록 변환하여 OS에 전달한다.

Java로 작성된 프로그램은 JVM 덕분에 OS에 독립적이지만 JVM은 OS에 종속적이어서 OS에 맞는 JVM을 설치해 주어야 한다.

![JVM](/assets/images/Back_End/JVMimage.png)

이와 같이 JVM 덕분에 한 번 Java 코드를 작성하고 나면 프로그램의 변경 없이 다양한 OS 어디서든 어플리케이션의 실행이 가능하다.

다만 Java 어플리케이션의 경우 JVM을 한 번 더 거치는 과정이 필요하고 실행 시에 해석$($interpret)되기 때문에 속도가 느리다는 단점을 가지고 있었지만 최근에는 바이트코드 $($컴파일된 자바코드)를 기계어로 바로 변환해주는 **JIT**컴파일러와 향상된 최적화 기술이 적용되어 속도를 많이 개선시켰다.

## 컴파일 하는 방법

JDK의 javac 사용

`javac hello.c`

## 실행하는 방법

`java hello.java`

or

`java hello`

## 바이트코드란 무엇인가

JVM이 이해할 수 있는 기계어.

컴파일러를 통해 자바 소스파일 $($.java)이 바이트코드 $($.class)로 변환되고 JVM은 바이트코드를 해당 OS의 기계어로 변환하여 OS로 전달한다.

## JVM 구성 요소

![JVM Architecture](https://www.guru99.com/images/1/2.png)

<small>출처: https://www.guru99.com/java-virtual-machine-jvm.html</small>

### 1. ClassLoader
JVM에서 ClassLoader는 바이트코드 $($.class 파일)들을 메모리상으로 로드하는 역할을 한다. 

class 파일들은 해당 클래스가 실행될때 메모리에 올라가게 된다.

클래스로더는 **로딩$($클래스 읽기), 링킹$($레퍼런스 연결), 초기화.** 3가지 중요한 기능을 수행한다.

### 2. Method Area
Method Area에는 클래스의 필드, 메소드 정보, 메소드와 생성자의 바이트코드, 각 클래스, 인터페이스에 관련된 런타임 상수풀이 저장된다.

### 3. Heap
모든 객체, 연관된 인스턴스 변수, 배열이 힙에 저장된다. 

할당된 오직 가비지 컬렉터에 의해서 해제된다.

힙 메모리는 공유 공간이며 여러 쓰레드에 공유된다.

### 4. Java language Stacks
Java laguage Stacks은 지역 변수를 저장한다. 각각의 쓰레드는 고유의 JVM 스택을 보유하고 있으며 쓰레드가 생성됨과 동시에 JVM도 생성된다.

메소드가 호출되면 새로운 프레임이 생성되고 메소드가 끝나면 삭제된다.

### 5. PC Registers
PC 레지스터는 현재 수행중인 JVM 명령의 주소를 저장하며 context switch가 발생할때 사용된다. 자바에서 각각의 쓰레드는 고유의 PC 레지스터를 보유한다.

### 6. Native Method Stacks
자바 이외의 언어로 작성된 코드를 실행할 때, Native Method Stack이 할당된다.

### 7. Execution Engine
JVM이 컴파일한 바이트코드 $($.class)를 ClassLoader를 통해 메모리에 실행 가능한 상태로 로드해 놓으면 **Execution Engine**은 메모리에 할당된 바이트코드를 실행하는 역할을 한다.

Execution Engine은 바이트코드를 한 줄씩 읽으며 기계어로 변환하는 작업을 거친다.

Execution Engine의 구성요소에는 **Interpreter, JIT, Garbage Collector**가 있다.

#### Interpreter
바이트코드를 한 줄씩 컴파일해서 기계가 이해할 수 있는 Native Code로 변환하는 작업을 한다.

중복되는 바이트코드들을 매번 컴파일하게 되면 Running Time이 길어지고 비효율적이다.

때문에 중복되는 바이트코드는 JIT 컴파일러를 사용한다.

#### JIT 컴파일러
Interpreter의 성능을 높히기 위해 Interpreter가 반복되는 코드를 발견하면 JIT 컴파일러로 반복되는 코드를 모두 Native Code로 변환한다.

반복된 바이트코드는 이미 Native Code로 바뀌어 있기 때문에 Interpreter가 바로 해석할 수 있게 된다.

#### Garbage Collector
힙 영역에서 더 이상 참조되지 않는 객체를 삭제한다.

## JDK와 JRE의 차이

![JDKvsJRE](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fc00klf%2FbtqAjMzLyF2%2F6sU1VGp5vqAYIPLsXpakpK%2Fimg.png)

<small>출처: https://cryptosalamander.tistory.com/4</small>

### JDK
JDK $($Java Development Kit)의 약자로, 자바 개발 키트를 의미한다.

JRE를 포함하여 javac 등의 컴파일러, 디버거등을 포함하는 프로그램이다.

### JRE
JRE $($Java Runtime Environment)는 자바 실행 환경을 의미한다.

JVM을 동작하는데 필요한 자바 라이브러리를 포함하고 있다.

---

> 참조 <br>
> Java의 정석 $($남궁 성) <br>
> [whiteship 자바 라이브 스터디](https://github.com/whiteship/live-study)