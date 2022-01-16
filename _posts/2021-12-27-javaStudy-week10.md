---
toc: true
title: "[Java 기초] 열거형 - Enum"
category:
  - Java
---

이번 포스트에서는 자바의 상수값을 나열하여 저장하는 열거형 Enum에 대해서 알아보도록 하겠다. 👨‍🏫

## 📋 목차
- enum 정의하는 방법
- enum이 제공하는 메소드
- java.lang.Enum
- EnumSet

## enum 정의하는 방법

> enum 열거형이름 { 상수명1, 상수명2, ... }

열거형 사용법 : **열거형이름.상수명**

```java
enum Direction { EAST, SOUTH, WEST, NORTH }

class Unit {
    int x, y;
    Dirction dir; // 열거형을 인스턴스 변수로 선언

    void init() {
        dir = Direction.EAST; // 열거형이름.상수명
    }
}
```

- 열거형 상수 하나하나는 `Direction 객체`이다.
- 열거형 상수간에 == 연산 가능.
- <, > 비교연산자는 불가. compareTo$($)를 사용.


## java.lang.Enum
**열거형 이름.values$($)** :  열거형의 모든 상수를 배열에 담아 반환.

java.lang.Enum은 모든 열거형의 조상이다.

### 열거형에 정의된 모든 상수를 출력하는 방법
```java
Direction[] dArr = Direction.values(); // 열거형 Direction에 정의된 상수를 배열에 담아 반환

for(Direction d : dArr) {
  System.out.pritf("%s = %d%n", d.name(), d.ordinal()); // 열거형 상수와 번호(0부터)가 출력
}
```

## enum이 제공하는 메서드

| 메서드 | 설명 |
|-------|-------|
| Class<<E>E> getDeclaringClass$($) | 열거형의 Class객체를 반환한다. |
|String name$($) | 열거형 상수의 이름을 문자열로 반환한다. |
|int ordinal$($) | 0부터 열거형 상수가 정의된 순서를 반환한다. |
|T valueOf$($Class<<T>T> enumType, String name) | 지정된 열거형에서 name과 일치하는 열거형 상수를 반환한다.|

```java
enum Direction { EAST, SOUTH, WEST, NORTH }

class Example {
    public static void main(String[] args) {
        Direction d1 = Direction.EAST;
        Direction d2 = Direction.valueOf("WEST"); // WEST와 일치하는 상수를 반환
        Direction d3 = Enum.valueOf(Direction.class, "EAST"); // Direction 열거형에서 EAST와 일치하는 상수를 반환

        System.out.println("d1 =" + d1);
        System.out.println("d2 =" + d2);
        System.out.println("d3 =" + d3);

        System.out.println("d1==d2 ? " + (d1==d2)); // false
        System.out.println("d1==d3 ? " + (d1==d3)); // true
        System.out.println("d1.compareTo(d3) ? " + (d1.compareTo(d3))); // 0
        System.out.println("d1.compareTo(d2) ? " + (d1.compareTo(d2))); // -2

        switch (d1) {
            case EAST :
                System.out.println("Thr direction is EAST."); break;
            case SOUTH:
                System.out.println("The direction is SOUTH"); break;
            case WEST :
                System.out.println("The direction is WEST"); break;
            case NORTH:
                System.out.println("The direction is NORTH"); break;
            default :
                System.out.println("Invalid direction."); break;
        }

        Direction[] dArr = Direction.values();

        for(Direction d : dArr) System.out.printf("%s = %d%n", d.name(), d.ordinal()); // 이름 = 순서
    }
}
```

**출력**
```
d1 =EAST
d2 =WEST
d3 =EAST
d1==d2 ? false
d1==d3 ? true
d1.compareTo(d3) ? 0
d1.compareTo(d2) ? -2
Thr direction is EAST.
EAST = 0
SOUTH = 1
WEST = 2
NORTH = 3
```

enum에 멤버를 추가하는것도 가능하다.

```java
enum Direction {
  // 열거형 상수에 value값 추가
  EAST(1), SOUTH(5), WEST(-1), NORTH(10); // 끝에 ; 추가해야함.

  private final int value;
  Direction(int value) { this.value = value; } // 생성자 추가. 열거형의 생성자는 private이다.

  public int getValue() { return value; } // 외부에서 값을 얻을수 있게 하는 메서드
}
```

## EnumSet

`set 컬랙션`에서는 중복된 데이터를 저장하지 못하고 데이터의 저장 순서가 보장되지 않는다.

EnumSet을 만들수 있는 메서드는 다양하다.

먼저 모든 요소를 포함하는 EnumSet은 `allOf()`로 만들 수 있다.

```java
public enum Direction {
  EAST, SOUTH, WEST, NORTH
}

EnumSet<Direction> set = EnumSet.allOf(Direction.class);
set.forEach(System.out::println);
```

**출력**
```
EAST
SOUTH
WEST
NORTH
```

### 그 외의 EnumSet 생성 메서드

#### noneOf$($)
빈 Direction 컬렉션을 갖는 EnumSet을 만든다.

```java
EnumSet<Direction> set = EnumSet.noneOf(Direction.class);
```

#### of$($)
들어갈 요소를 직접 입력하여 EnumSet을 만든다.

```java
EnumSet<Direction> set = EnumSet.of(Direction.NORTHWEST);
```

#### complementOf$($)
원하는 요소를 제거하고 EnumSet을 생성한다.

```java
EnumSet<Direction> set = EnumSet.complementOf(EnumSet.of(Direction.NORTHWEST));
```

#### copyOf$($)
다른 EnumSet의 모든 요소를 복사하여 EnumSet을 만든다.

```java
EnumSet.copyOf(EnumSet.of(Direction.EAST, Direction.WEST));
```

### EnumSet을 다루는 메서드

#### add$($)
EnumSet에 요소를 추가한다.

```java
EnumSet<Direction> set = EnumSet.of(Direction.EAST, Direction.WEST);

set.add(Direction.SOUTH); // EnumSet set에 요소를 추가한다.
```

#### contains$($)
특정 요소가 EnumSet에 포함되어 있는지 확인한다.

```java
EnumSet<Direction> set = EnumSet.of(Direction.EAST, Direction.WEST);

boolean isContain = set.contains(Direction.EAST);

if(isContain) System.out.println("EAST 포함");
```

#### remove$($)
EnumSet에 특정 요소를 삭제한다.

```java
set.remove(Direction.EAST);
```

---

> 참조 <br>
> Java의 정석 $($남궁 성) <br>
> [whiteship 자바 라이브 스터디](https://github.com/whiteship/live-study)