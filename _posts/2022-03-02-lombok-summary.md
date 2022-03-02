---
toc: true
title: "주요 Lombok 에너테이션 정리"
category:
  - Back_End
---
이번 포스트에서는 `Lombok`라이브러리가 제공하는 에너테이션 중 자주 사용되는 에너테이션들을 살펴보도록 하겠다. 😎

## 생성자 자동 생성
Lombok을 통해 생성자를 자동으로 생성할 수 있다.

- NoArgsConstructor
- AllArgsConstructor
- RequiredArgsConstructor

### @NoArgsConstructor
**`@NoArgsConstructor`** 에너테이션은 파라미터가 없는 **기본 생성자**를 생성해 준다.

### @AllArgsConstructor
**`@AllArgsConstructor`** 는 모든 필드 값을 파라미터로 받는 생성자를 생성해준다.

### @RequiredArgsConstructor
**`@RequiredArgsConstructor`** 는 **final**이 거나 **@NonNull**이 붙은 필드 값만 파라미터로 받는 생성자를 생성해준다.

## equlas, hashCode 메서드 자동 생성

equals와 hashCode는 Object 클래스에 정의되어 있다.

### equals 메서드
2개의 객체가 동일한 객체인지 검사하는데 사용. $($2개의 객체가 참조하는 것이 동일한지를 확인.)

```java
public boolean equals(Object obj) {
    return (this == obj);
}
```

두개의 동일한 문자열 객체 "Same"을 생성하였다 해도 이 두개의 문자열은 서로 다른 메모리 주소 공간에 할당되기 때문에 `==` 연산자로는 `False` 결과값이 나온다.

하지만 `equals`를 사용하면 `True`인데, 그 이유는 String 클래스내에서 equals 메서드를 오버라이드하여 문자 비교하는 로직을 포함하고 있기 때문이다.

```java
String test1 = new String("Same");
String test2 = new String("Same");

System.out.println(test1 == test2); // 주소값이 다르므로 false

//String 클래스에 오버라이드된 equals 함수 호출
System.out.println(test1.equals(test2)); // true. 

public boolean equals(Object anObject) { 
    if (this == anObject) { 
        return true; 
    } 
    
    if (anObject instanceof String) { 
        String anotherString = (String)anObject; 
        int n = value.length; 
        if (n == anotherString.value.length) { 
            char v1[] = value; 
            char v2[] = anotherString.value; 
            int i = 0; 
            while (n-- != 0) { 
                if (v1[i] != v2[i]) 
                    return false; 
                i++; 
            } 
            return true; 
        } 
    } 

    return false; 
}
```

### hashCode 메서드
데이터가 저장되는 위치를 결정하기 위해 사용된다.

런타임에 객체의 유일한 integer값을 반환한다. 

더 자세한 내용은 아래의 블로그를 참고하길 바란다.

[망나니개발자 - equls와 hashCode 함수](https://mangkyu.tistory.com/101)

### @EqualsAndHashCode
자동으로 equals, hashCode 메서드를 생성한다.

```java
@EqualsAndHashCode(callSuper = true)
public class Member extends Person {
    
    private String email;
    private String password;
}
```

```java
public class Person {

    private String name;
}
```

- **`callSuper = true`**
부모 클래스 필드 값들도 동일한지 체크.

- **`callSuper = false (기본값)`**
자신 클래스의 필드 값들만 고려.

```java
Member member1 = new Member();
member1.setName("m1");
member1.setEmail("test@gmail.com");
member1.setPassword("1111");

Member member2 = new Member();
member2.setName("m2"); // 부모 클래스의 필드값이 다름
member2.setEmail("test@gmail.com");
member2.setPassword("1111");

member1.equals(member2);
// callSuper = true -> false
// callSuper = false -> true
```

### @ToString
toString 메서드를 자동 생성.

`exclude =` 속성을 사용하여 특정 필드를 toString$($) 결과에서 제외 시키는것 가능.

```java
@ToString(exclude = "password")
public class Member {

    private String name;
    private String password;
}
```

```java
Member member = new Member();

member.setName("test");
member.setPassword("1234");

System.out.println(member);
```

`클래스명(필드A명=필드A값, 필드B명=필드B값, ...)` 식으로 출력된다.

출력결과
```
Member(name=test) 
// password는 toString 결과에서 제외되었다.
```

### @Data
다음의 에너테이션을 모두 포함하며 한번에 설정해준다.

- `@Getter`, `@Setter`
- `@RequiredArgsConstructor`
- `@ToString`
- `EqualsAndHashCode`

### 참고
- https://www.daleseo.com/lombok-popular-annotations/
- https://mangkyu.tistory.com/101