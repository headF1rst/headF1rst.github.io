---
toc: true
title: "[Java 기초] 멀티쓰레드 프로그래밍"
category:
  - Java
---

이번 포스트에서는 자바의 멀티쓰레드 프로그래밍에 대해서 알아보도록 하겠다. 👨‍🏫

## 📋 목차
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
| --- | --- |
| NEW | 쓰레드가 생성되고 start()가 호출되지 않은 상태. |
| RUNNABLE | 실행 중 또는 실행 가능한 상태 |
| BLOCKED | 동기화블럭에 의해서 lock이 풀릴 때까지 대기중인 상태 |
| WAITING, TIMED_WAITING | 쓰레드의 작업이 종료되지 않았지만 일시정지인 상태 |
| TERMINATED | 쓰레드의 작업이 종료된 상태 |



![쓰레드 상태](/assets/images/Back_End/threadstatus.png)

### sleep$($long millis)

> sleep$($long millis, int nanos)

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

### yield$($)
`yield()`가 호출되면 자신에게 남은 수행시간을 포기하고 실행대기 상태가된다.

```java
while(!stopped) {
    if(!suspended) {
        ...
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) { }
    } else {
        Thread.yield(); // static 메서드이다.
    }
} // while
```

만약 suspended == true 이고 `else {Thread.yield;}` 문이 생략되어 있었다면 쓰레드는 while문을 의미없이 돌면서 `busy-waiting`상태에 빠지게 될것이다.

`Thread.yield()`를 통해 while문에서 시간을 낭비하지 않고 다른 쓰레드에게 양보하므로 코드가 더 효율적이게 된다.

$($yield는 OS 스케줄러에 의존하기 때문에 정확한 반환 시간을 기대하기 어렵다.)

### join$($)

```java
try {
    th1.join() // 현재 실행중인 쓰레드가 쓰레드 th1의 작업이 끝날때까지 기다린다.
} catch (InterruptedException e) {}
```

- 작업중에 다른 쓰레드의 작업이 먼저 수행되어야할 필요가 있을때 사용.

- interrupt$($)에 의해 대기상태에서 벗어날 수 있다.

- 깨어날때 InterruptedException 발생

- static 메서드가 아니다.

## 쓰레드의 우선순위
쓰레드의 `priority`속성값을 변경하여 특정 쓰레드가 더 많은 수행시간을 부여받도록 할 수 있다.

> void setPriority$($int newPriority) 
쓰레드의 우선순위를 지정한 값으로 변경하는 메서드.

> int getPriority$($)
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

## 쓰레드의 동기화
멀티 쓰레드 프로세스에서는 

- 여러 쓰레드가 같은 자원을 공유한다. $($메모리)

- 한 쓰레드가 다른 쓰레드의 작업에 영향을 미칠 수 있다.

- 다른 쓰레드에게 진행중인 작업이 간섭받지 않게 하기위해 **동기화** 필요.

> **쓰레드의 동기화** : 한 쓰레드가 진행중인 작업을 다른 쓰레드가 간섭하지 못하게 막는 것.

- 간섭받지 않아야 하는 문장을 **임계 영역으로 설정**

- Lock $($열쇠) 을 얻은 단 하나의 쓰레드만 임계영역에 출입가능

- 객체 1개에 Lock 1개

### synchronized 동기화 방법
`synchronized` 키워드를 통해 임계 영역을 설정할 수 있다.

**1. 메서드 전체를 임계 영역으로 지정**
```java
public synchronized void calcSum() {
    // ... 
} // 임계 영역 끝
```

두개의 쓰레드가 출금하는 예제에서의 임계영역 설정

변수 `balance`에 여러 쓰레드가 동시에 접근하지 않도록 동기화 해줘야 한다.

```java
public synchronized void withdraw(int money) {
    if(balance >= money) {
        try { Thread.sleep(1000); } catch(Exception e) { }
        balance -= money;
    }
}
```

**2. 특정한 영역을 임계 영역으로 지정**
```java
synchronized(객체의 참조변수) { // lock을 걸고자 하는 객체를 참조하는 참조변수
    // ...
} // 임계 영역 끝
```

두개의 쓰레드가 출금하는 예제에서의 임계영역 설정

```java
public void withdraw(int money) {
    synchronized(this) {
        if(balance >= money) {
            try { Thread.sleep(1000); } catch(Exception e) { }
            balance -= money;
        }
    }
}
```

동기화 해야되는 객체를 읽고 쓰는 메서드에는 전부 `synchronized`로 동기화 해주어야 한다.

### wait$($) & notify$($)

- 동기화 통해 데이터의 안정성은 보장.

- **But!** 한번에 하나의 쓰레드밖에 임계영역에서 동작하지 못하기 때문에 프로그램의 효율성 저하.

- 동기화 효율 향상을 위해 `wait()`, `notify()` 사용.

- Object클래스내에 정의되어 있다.

- 동기화 블록 내에서만 사용가능.


> **wait$($) :** 객체의 lock을 반납하고 쓰레드를 해당 객체의 waiting pool에 넣는다.

> **notify$($) :** waiting pool에 대기중인 임의의 쓰레드 하나를 깨운다. <br>
> **notifyAll$($) :** waiting pool에 대기중인 모든 쓰레드를 깨운다.

은행 입출금 예제를 통해 wait, notify에 대해 알아보겠다.

계좌에 출금할 돈이 부족해서 한 쓰레드가 lock을 보유한 채로 돈이 입금될 때까지 다른 작업들 마저 진행되지 못하게 된다.

때문에 wait과 notify를 이용해서 다음과 같이 효율을 개선해 주어야 한다.

```java
class Account {
    private int balance = 1000;

    public synchronized void withdraw(int money) {
        while(balance < money) {
            try {
                wait(); // 락을 반납하고 기다린다.
                // 통지를 받으면 락을 재획득(ReEntrance)
            } catch(InterruptedException e) { }
        }
        balance -= money;
    }

    public synchronized void deposit(int money) {
        balance += money;
        notify(); // 대기중인 임의의 쓰레드 하나를 깨움.
    }
}
```

notify$($)를 사용해서 하나의 임의의 쓰레드를 깨우게 되면 최악의 경우 어느 한 쓰레드는 오랫동안 깨어나지 못하는 **Starvation** 현상이 발생할 수 있다.

이를 해결하고자 보통 notifyAll$($)을 사용하지만 이러한 경우 starvation은 막았지만 한번에 깨어난 여러 쓰레드가 lock을 얻기 위해 서로 경쟁하는 **Race condition**이 발생하게 된다.

때문에 쓰레드 간의 선별적인 통지가 이루어지는것이 필요하고 자바에서 제공하는 lock클래스인 **Lock**과 **Condition**을 사용하여 해결 가능하다.

### ReentrantLock
**생성자**

> **ReentrantLock$($)** <br>
> **ReentrantLock$($boolean fair)**

매개변수 `fair`에 `true`를 주게되면 가장 오래 기다린 쓰레드가 lock을 획득한다.

하지만 가장 오래 기다린 쓰레드를 찾는 과정 떄문에 성능저하가 발생한다.

`ReentrantLock`은 synchronized 블럭과 달리 메서드를 호출하여 수동으로 lock을 잠그고 해제해야 한다.

> **void lock$($)**     lock을 잠금. <br>
> **void unlock$($)**   lock을 해지. <br>
> **boolean isLocked$($)** lock이 잠겼는지 확인

임계 영역 내에서 예외가 발생하거나 return문으로 빠져나갈 경우 lock이 풀리지 않을 수 있기 때문에 unlock$($)을 try - finally문으로 감싼다.

```java
lock.lock() // ReentrantLock lock = new ReentrantLock();
try {
    // 임계 영역
} finally {
    lock.unlock();
}
```

### Condition
`Condition`은 대기$($await)와 통지$($signal)의 대상을 명확히 구분해준다.

기존의 wait & notify에서는 공유 객체의 waiting pool에 모든 쓰레드가 대기했었지만 각 쓰레드의 Condition을 만들어서 각각의 waiting pool에서 대기상태에 있도록 하였다.

Object|Condition|
----|----|
void wait$($) | void await$($) |
void notify$($) | void signal$($) |
void notifyAll$($) | void signalAll$($) |

```java
public void add(String dish) {
    lock.lock();

    try {
        while(dishes.size() >= MAX_FOOD) {
            String name = Thread.currentThread().getName();
            System.out.println(name + "is waiting");

            try {
                forCook.await(); // Cook 쓰레드를 기다리게 한다.
            } catch (InterruptedException e) {}
        }

        dishes.add(dish);
        forCust.signal(); // 기다리고 있는 CUST를 깨운다.
        System.out.println("Dishes:" + dishes.toString());
    } finally {
        lock.unlock();
    }
}
```

---

> 참조 <br>
> Java의 정석 $($남궁 성) <br>
> [whiteship 자바 라이브 스터디](https://github.com/whiteship/live-study)