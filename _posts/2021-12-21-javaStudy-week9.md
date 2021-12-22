---
toc: true
title: "[Java 기초] 멀티쓰레드 프로그래밍"
category:
  - Java
---

이번 포스트에서는 자바의 멀티쓰레드 프로그래밍에 대해서 알아보도록 하겠다.

---

## 목차
- Thread 클래스와 Runnable 인터페이스
- 쓰레드의 상태
- 쓰레드의 우선순위
- Main 쓰레드
- 동기화
- 데드락

## Thread 클래스와 Runnable 인터페이스
쓰레드를 구현하는 방법에는 두가지가 있다.
1. Thread 클래스를 상속받는법
2. Runnable 인터페이스를 구현하는법 $($일반적)

간단히 말해 쓰레드의 구현이란 쓰레드를 통해 작업하고자 하는 내용을 run()의 몸통{}부분에 채워주는 작업이다.

### Thread 클래스를 상속
```java
class MyThread extends Thread { // Thread 클래스를 상속
    public void run() { // Thread클래스의 run()을 오버라이딩
        // 작업 내용
    } 
}
```

조상클래스인 Thread클래스의 `run()`을 오버라이딩하여 쓰레드 구현.

**Thread클래스**

```java
public class Thread {
    private Runnable r;

    public Tread(Runnable r) {
        this.r = r;
    }

    public void run() {
        if(r != null) r.run(); // Runnable 인터페이스를 구현한 인스턴스의 run()을 호출
    }

    ... 
}
```

---

### Runnable 인터페이스를 구현
```java
class MyThread implements Runnable { // Runnable 인터페이스 구현
    public void run() { // Runnable인터페이스의 run()을 구현
        // 작업 내용
    }
}
```

오직 `run()`만 정의되어있는 Runnable 인터페이스의 몸통을 만들어줘서 쓰레드 구현.

**Runnable 인터페이스**

```java
public interface Runnable {
    public abstract void run();
}
```

---

### 예제

```java
class Example {
    public static void main(String[] args) {
        ThreadEx1 thread1 = new ThreadEx1();

        Runnable r = new ThreadEx2(); // Runnable을 구현한 클래스의 인스턴스를 생성
        Thread thread2 = new Thread(r); // 생성자 Thread(Runnable target)

        thread1.start();
        thread2.start();
    }
}

class ThreadEx1 extends Thread {
    public void run() {
        for(int i=0; i < 5; i++) {
            System.out.println(getName()); // 쓰레드의 이름을 반환
        }
    }
}

class ThreadEx2 implements Runnable { // Runnable 인터페이스를 구현하는 클래스
    public void run() {
        for(int i=0; i < 5; i++) {
            // 현재 실행중인 쓰레드를 반환한다.
            System.out.println(Thread.currentThread().getName()); // 현재 실행중인 쓰레드의 참조를 반환. 그중에서 이름을 반환.
        }
    }
}
```
**출력**
```
Thread-0
Thread-0
Thread-0
Thread-0
Thread-0
Thread-1
Thread-1
Thread-1
Thread-1
Thread-1
```

쓰레드의 이름을 지정하지 않으면 `Thread-번호` 형식으로 이름이 정해진다.

쓰레드의 이름은 생성자나 메서드를 통해 변경할 수 있다.

## start() vs run()

**start()**
새로운 쓰레드를 생성.

쓰레드가 작업하는데 사용될 호출스택을 생성.

**run()**
생성된 쓰레드가 작업을 수행하게 한다.

## 쓰레드의 상태
| 상태 | 설명 |
|---|----|
|NEW|쓰레드가 생성되고 start()가 호출되지 않은 상태.|
|RUNNABLE|실행 중 또는 실행 가능한 상태|
|BLOCKED|동기화블럭에 의해서 lock이 풀릴 때까지 대기중인 상태|
|WAITING, TIMED_WAITING|쓰레드의 작업이 종료되지 않았지만 일시정지인 상태|
|TERMINATED|쓰레드의 작업이 종료된 상태|



![쓰레드 상태](/assets/images/Back_End/threadstatus.png)

### sleep(long millis)

> sleep(long millis, int nanos)

쓰레드를 지정시간동안 일시정지 시킨다.

지정된 시간이 다되면 `InterruptedException`이 발생하여 잠에서 깨어나 실행대기 상태가 된다.

때문에 `try-catch`문으로 항상 예외처리를 해줘야 한다.

```java
// 매번 try-catch문을 쓰는게 번거로우므로 다음과 같이 사용.
void delay(long millis) {
    try {
        Thread.sleep(millis);
    } catch (InterrupedException e)
}
```

`sleep()`은 **static 메서드**이다.

즉, 클래스이름을 사용하여 호출해야 한다.

현재 실행 중인 쓰레드에 대해 작동하는 메서드이며 다른 쓰레드를 재울수는 없다.

### 쓰레드의 작업을 취소. interrupt()
`interrupt()`는 쓰레드에게 작업을 멈추라고 요청한다.

쓰레드는 WAITING 상태에서 RUNNABLE$($실행대기) 상태로 바뀐다.

```java
void interrupt() // 쓰레드의 interrupted 상태를 false -> true

static boolean interrupted() // 쓰레드의 interrupted 상태를 반환 후, false로 변경

boolean isInterrupted() // 쓰레드의 interrupted 상태를 반환
```

#### 예제
```java
import javax.swing.*;

class Example {
    public static void main(String[] args) throws Exception {
        Thread th = new Thread(new Thread1());
        th.start();

        String input = JOptionPane.showInputDialog("아무값이나 입력하세요.");
        System.out.println("입력하신 값은 " + input + "입니다.");
        th.interrupt();
        System.out.println("isInterrupted() : " + th.isInterrupted());
    }
}

class Thread1 implements Runnable {
    public void run() {
        int i =10;

        while(i != 0 && !Thread.currentThread().isInterrupted()) {
            System.out.println(i--);
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) { /*Thread.currentThread().interrupt();*/ }
        }
        System.out.println("카운트가 종료되었습니다.");
    }
}
```
**출력**
```
10
9
8
7
6
5
4
입력하신 값은 abcd입니다.
3
isInterrupted() : true    
2
1
카운트가 종료되었습니다.         < - interrupt에 의해 카운트가 종료되지 않았다.
```

우리의 예상과 달리 `interrupt()`에 의해 카운트가 종료되지 않았다.

이는 쓰레드가 `sleep()`에 의해 잠들어있을때 `interrupt()`가 호출되면 interrupted 상태를 `true`로 변경하지만 interrupt$($)에 의해 깨어난 sleep$($)에서 **InterruptedException**을 던지고, 이는 interrupted 상태를 **false**로 초기화 하기 때문이다.

때문에 의도한 결과를 얻으려면 예외 처리 catch구문에 주석과 같이 interrupt를 통해 interrupted상태를 다시 true로 초기화 해주어야 한다.

### suspend(), resume(), stop()

> suspend$($) : 쓰레드를 멈추게 한다.
> resume$($) : suspend된 쓰레드를 실행대기 상태로 바꾼다.
> stop$($) : 쓰레드를 종료시킨다.

위 메서드들은 교착상태$($DeadLock)을 일으키기 쉽기 때문에 사용이 권장되지 않는다. $($Deprecated)

때문에 다음과 같이 오버라이딩을 하여 구현한다.

#### 예시

```java
class Example {
    public static void main(String[] args) {
        Thread1 th1 = new Thread1("*");
        Thread1 th2 = new Thread1("**");
        Thread1 th3 = new Thread1("***");

        th1.start();
        th2.start();
        th3.start();

        try {
            Thread.sleep(2000);
            th1.suspend();
            Thread.sleep(2000);
            th2.suspend();
            Thread.sleep(3000);
            th1.resume();
            Thread.sleep(3000);
            th1.stop();
            th2.stop();
            Thread.sleep(2000);
            th3.stop();
        } catch (InterruptedException e) { }
    }
}

class Thread1 implements Runnable {
    volatile boolean suspended = false;
    volatile boolean stopped = false;

    Thread th;

    Thread1(String name) {
        // Thread(Runnable r, String name)
        th = new Thread(this, name);
    }

    public void run() {
        while(!stopped) {
            if(!suspended) {
                System.out.println(Thread.currentThread().getName());
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) { }
            }
        }
        System.out.println(Thread.currentThread().getName() + " - stopped");
    }

    public void suspend() { suspended = true; }
    public void resume() { suspended = false; }
    public void stop() { stopped = true; }
    public void start() { th.start(); }
}
```
**출력**
```
*
**
***
***
**
*
***
**
**
***
***
***
***
*
***
*
***
*
***
** - stopped
* - stopped
***
***
*** - stopped
```



## 쓰레드의 우선순위
쓰레드의 `priority`속성값을 변경하여 특정 쓰레드가 더 많은 수행시간을 부여받도록 할 수 있다.

> void setPriority(int newPriority) 
쓰레드의 우선순위를 지정한 값으로 변경하는 메서드.

> int getPriority()
쓰레드의 우선순위를 반환하는 메서드.

우선순위 범위 : 1 ~ 10

쓰레드의 우선순위는 쓰레드를 생성한 쓰레드로부터 상속받는다.

### 예제 
```java
class Example {
    public static void main(String[] args) {
        Thread th1 = new Thread(new Thread1());
        Thread th2 = new Thread(new Thread2());

        th1.setPriority(7);

        System.out.println("Priority of th1 (-) : " + th1.getPriority());
        System.out.println("Priority of th2 (|) : " + th2.getPriority());
        th1.start();
        th2.start();
    }
}

class Thread1 implements Runnable {
    public void run() {
        for(int i=0; i < 300; i++) {
            System.out.print("-");
            for(int x=0; x < 10000000; x++); // 한 번에 작업이 끝나버리는걸 막기위한 지연
        }
    }
}

class Thread2 implements Runnable {
    public void run() {
        for(int i=0; i < 300; i++) {
            System.out.print("|");
            for(int x=0; x < 10000000; x++);
        }
    }
}
```

하지만 멀티코어에서는 쓰레드의 우선순위에 따른 차이가 없다.

물론 OS와 JVM에 따라 멀티코어에서의 실행 결과가 달라질수 있지만 결과의 예측이 어렵다.

때문에 쓰레드에 우선순위를 부여하는 대신 작업에 우선순위를 두어 `PriorityQueue`에 저장해 놓고, 우선순위가 높은 작업을 먼저 처리하도록 하는 편이 좋다.


## Main 쓰레드
main 메서드를 호출해서 main메서드를 수행하는 쓰레드를 `main쓰레드`라고 한다.

### 예제

```java
class Example {
    public static void main(String[] args) throws Exception {
        Thread thread1 = new Thread(new ThreadEx1());
        thread1.start();
        // thread1.run();
    }
}

class ThreadEx1 implements Runnable {
    public void run() {
        throwException();
    }

    public void throwException() {
        try {
            throw new Exception();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
**출력**
```
java.lang.Exception
	at week1.ThreadEx1.throwException(Example.java:17)
	at week1.ThreadEx1.run(Example.java:12)
	at java.base/java.lang.Thread.run(Thread.java:832)
```

호출스택에는 `run()`메서드와 `throwException()`만 남아있고 `main`쓰레드가 예외에 의해 종료되어 main메서드가 호출스택에 존재하지 않는다.

즉, 한 쓰레드가 예외에 의해 종료되어도 다른 쓰레드의 실행에는 영향을 미치지 못한다.

