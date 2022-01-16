---
toc: true
title: "[Java 기초] 인터페이스"
category:
  - Java
---

이번 포스트에서는 자바의 인터페이스에 대해서 알아보도록 하겠다. 👨‍🏫

## 📋 목차

- 인터페이스 정의하는 방법
- 인터페이스 구현하는 방법
- 인터페이스 레퍼런스를 통해 구현체를 사용하는 방법
- 인터페이스 상속
- 인터페이스의 기본 메소드 $($Default Method)
- 인터페이스의 static 메소드
- 인터페이스의 private 메소드

---

## 인터페이스 정의하는 방법
인터페이스란

- 일종의 추상클래스. $($추상화 정도가 더 높은)
- 밑그림만 그려진 기본 설계도. $($선언부만 존재)
- 자체적으로 사용되지 못하고 다른 클래스 작성에 도움을 준다.
- **추상메서드와 상수만을 멤버로 가질수 있다.**
    - 모든 멤버변수는 `public static final`이어야 하며 이를 생략 가능.
    - 모든 메서드는 `public abstract` 이어야 하며 이를 생략 가능.

만약 제어자를 생략할 경우 컴파일러가 자동적으로 생략된 제어자를 컴파일타임에 추가해준다.

인터페이스 정의는 기본적으로 클래스를 작성하는것과 같다. $($class 대신 interface 키워드 사용.)

```java
interface 인터페이스이름 {
    // 추상메서드와 상수만을 멤버로 갖는다.
    public static final 타입 상수이름 = 값; // 상수
    public abstract 메서드이름(매개변수목록); // 추상메서드

    final int DIAMOND = 3; // public static final int DIAMOND = 3;
}
```

## 인터페이스 구현하는 방법 implements
추상클래스와 마찬가지로 인터페이스 자체로는 인스턴스를 생성할 수 없다.

인터페이스는 자신의 추상메서드의 몸통을 만들어주는 클래스를 작성하여 인스턴스를 생성한다. $($**implements** 키워드 사용.)

implemnets 사전적 의미 = 구현하다.

```java
class 클래스이름 implements 인터페이스이름 {
    // 인터페이스에 정의된 추상메서드를 구현해야한다.
}
```

### 예제

```java
class Example {
    public static void main(String[] args) {
        Fighter f = new Fighter();

        if(f instanceof Unit) System.out.println("f는 Unit클래스의 자손입니다.");
        if(f instanceof Fightable) System.out.println("f는 Fightable인터페이스를 구현했습니다.");
        if(f instanceof Movable) System.out.println("f는 Movable 인터페이스를 구현했습니다.");
        if(f instanceof Attackable) System.out.println("f는 Attackable 인터페이스를 구현했습니다.");
        if(f instanceof Object) System.out.println("f는 Object클래스의 자손입니다.");
    }
}

class Unit {
    int currentHp;
    int x, y;
}

interface Movable { void move(int x, int y); }
interface Attackable { void attack(Unit u); }
interface Fightable extends Movable, Attackable { }

class Fighter extends Unit implements Fightable {
    public void move(int x, int y) { /* 내용 */ }
    public void attack(Unit u) { /* 내용 */ }
}
```

## 인터페이스 레퍼런스를 통해 구현체 사용하는 법
인터페이스를 사용하면 클래스와 클래스간의 직접적인 관계를 간접적인 관계로 변경하여 독립적인 프로그래밍을 가능하게 한다.

예를들어 SCV에게 상속관계에서 공통점 없는 Tank, Dropship 유닛을 수리할 수 있는 기능을 제공하기 위해 `repair` 메서드를 정의해야 하는 경우, 인터페이스를 사용하여 공통점을 부여해준다.

`Repairable`이라는 인터페이스를 정의한 다음 SCV, Tank, Dropship에 인터페이스를 구현하도록 한다. 

이제 인터페이스의 레퍼런스를 통해 `repair`메서드의 매개변수에 SCV, Tank, Dropship이 들어올 수 있다.

> void repair(**Repairable r**) { ... }

### 예제

```java
class A {
    void autoPlay(I i) {
        i.play();
    }
}

interface I {
    public abstract void play();
}

class B implements I {
    public void play() {
        System.out.println("play in B class");
    }
}

class C implements I {
    public void play() {
        System.out.println("play in C class");
    }
}

class Example {
    public static void main(String[] args) {
        A a = new A();
        a.autoPlay(new B());
        a.autoPlay(new C());
    }
}
```

**출력**
```
play in B class
play in C class
```

**리턴타입이 인터페이스이다** => **해당 인터페이스를 구현한 클래스의 인스턴스를 반환한다.**

## 인터페이스 상속

- 인터페이스 간에서만 상속가능.
- 다중상속. $($여러 인터페이스를 상속받는것이 가능.)

```java
interface Dribblable {
    void move(int x, int y);
}

interface Attackable {
    void attack(Unit u);
}

interface Playable extends Dribblable, Attackable { }
```

## 인터페이스의 default method
아무리 인터페이스 설계를 초기에 잘해도 변경사항이 생기기 마련이다.

인터페이스에 메서드를 추가해야 한다는 것은 이 인터페이스를 구현한 모든 클래스들이 새로 추가된 메서드를 구현해야 하기 때문에 보통 쉬운일이 아니다. 😨

때문에 **default method**가 고안되었다.

디폴트 메서드는 추상 메서드가 아니기 때문에 **새로 추가되어도 해당 인터페이스를 구현한 클래스를 변경하지 않아도 된다**.

- 메서드 앞에 default를 붙인다.
- 몸통 {}이 존재해야한다.
- public 접근제어자 이며 생략가능하다.

```java
intaerface Playable {
    void method();
    // default method
    default void newMethod() {} // 새로 추가된 메서드
    // Playable 인터페이스의 메서드를 구현한 클래스들에 newMethod()를 일일이 추가 구현해주지 않아도 된다.
}
```

## 인터페이스의 static method
기본적으로 default 메서드와 동일하지만 구현체에서 **재정의가 불가능**하다.

## 인터페이스의 private method
인터페이스 내부에서만 private 메서드를 사용할 수 있다.

```java
interface Playable {
    void dribbling();

    default void shooting() { 
        System.out.println("차면 골인 슛입니다.");
    }

    private void tackling() {
        System.out.println("태클로 공을 뺏습니다.");
    }
} 
```

default, static 메서드에 사용하기 위한 메서드이다.

인터페이스를 구현하는 클래스쪽에서 재정의하거나 사용할 수 없다.

---

> 참조 <br>
> Java의 정석 $($남궁 성) <br>
> [whiteship 자바 라이브 스터디](https://github.com/whiteship/live-study)