---
toc: true
title: "[Spring security] 스프링 세큐리티 기본 개념 정리"
category:
  - Back_End
---
## UserDetails 인터페이스
사용자의 정보를 담는 인터페이스.

Spring Security는 UserDetails를 구현한 클래스(사용자 정보 엔티티)를 대상으로 인증 작업을 
수행한다.

사용자의 정보를 저장하는 엔티티를 구현해야 한다.

오버라이드 되는 메소드들은 다음과 같다.

**getAuthorities()**

리턴 타입 - `Collection<? extends GrantedAuthority>`

계정이 갖고있는 권한 목록을 리턴.

**getPassword()**

리턴 타입 - `String`

계정의 비밀번호를 리턴.

**getUsername()**

리턴 타입 - `String` 

계정의 아이디 혹은 이메일을 리턴.

**isAccountNonExpired()**

리턴 타입 - `boolean`

계정 만료 여부를 리턴. (true: 만료 안됨)

이러한 여부 확인할 필요 없다면 true로 설정.

**isAccountNonLocked()**

리턴 타입 - `boolean`

계정이 잠겨있는지 여부를 리턴 (true: 잠기지 않음)

**isCredentialNonExpired()**

리턴 타입 - `boolean`

비밀번호 만료 여부를 리턴 (true: 만료안됨)

**isEnabled()**

리턴 타입 - `boolean`

계정이 활성화 상태인지 여부를 리턴. (true: 활성화)

현재 진행중인 프로젝트에서의 DB에는 계정의 상태 정보 “ACTIVE”, “INACTIVE”, “DELETED”가 
존재하기 때문에 `isEnable()` 메서드는 상태를 나타내는 필드 값에 따라 조건문을 사용해서 맞는 
값을 리턴해 주었다.

### [User.java](http://User.java)

```java
@Entity
@Getter
public class User extends Person implements UserDetails {

    @Lob
    @NotEmpty
    private String email;

    @Lob
    @NotEmpty
    private String password;

    @Enumerated(EnumType.STRING)
    private UserRole userRole;

    @Lob
    private String imageUrl;

    private int mannerPoint;

    @Column(length = 100)
    private String location;

    @Column
    @NotEmpty
    @Digits(fraction = 0, integer = 11)
    private String telephone;

    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        SimpleGrantedAuthority authority = new SimpleGrantedAuthority
        (userRole.name());
        return Collections.singletonList(authority);
    }

    @Override
    public String getUsername() {
        return getName();
    }

    @Override
    public boolean isAccountNonExpired() {
        return true;
    }

    @Override
    public boolean isAccountNonLocked() {
        if (getStatus() == "ACTIVE") {
            return ture;
        } else {
            return false;
        }
    }

    @Override
    public boolean isCredentialsNonExpired() {
        return true;
    }

    @Override
    public boolean isEnabled() {
        if (getStatus() == "ACTIVE") {
            return true;
        } else {
            return false;
        }
    }
}
```

## UserDetailsService 인터페이스란?

UserDetailsService 인터페이스를 구현한 클래스는 DB에서 유저 정보를 직접 가져온다.

### loadUserByUsername(String email)

DB에서 유저 정보를 가져와서 리턴해주는 작업을 수행한다.

DB로 부터 가져온 유저 정보를 UserDetails 형으로 가져온다.

```java
@Service
@Transactional(readOnly = true)
@RequiredArgsConstructor
public class UserService implements UserDetailsService {

    private final UserRepository userRepository; // 삭제 생각해 봐야함
    private final UserDAO userDao;

    @Override
    public UserDetails loadUserByUsername(String email) throws 
    UsernameNotFoundException {
        return userDao.findByEmail(email)
                .orElseThrow(() -> new UsernameNotFoundException("해당 
                이메일의 사용자를 찾을수 없습니다."));
    }
```

## WebSecurityConfigurerAdapter

**void configure(HttpSecurity http)**

`.anyMatchers()` : 로그인이 필요한 url을 정의

`.formLogin()` : form 태그 기반의 로그인을 지원하겠다는 설정. 별도의 로그인 페이지를 
제작하지 않아도 된다 (’/login’ 경로를 호출하면 스프링 시큐리티에서 제공하는 기본 로그인 화면을 
볼 수 있게 된다)  

로그인 페이지와 로그인 성공시 보내줄 url 정의

`.loginPage(”loginpage”)`, `.defaultSuccessUrl("logininsuccess", true)`

`.csrf()` : CSRF 공격은 사이트간 위조 요청으로, 정상적인 사용자가 의도치 않은 위조 요청을 
보내는 것을 의미한다.

REST api를 이용한 서버는 stateless하기 때문에 서버에 인증정보를 저장하지 않기 떄문에 csrf 
공격으로부터 안전하고 매번 api 요청으로 부터 csrf 토큰을 받지 않아도 되기 때문에 disable()
로 설정 하는것이 좋다.

`.authorizeRequests()` : 시큐리티 처리에 HttpServletRequest를 이용.

`.antMatchers()` : 특정한 경로를 지정.