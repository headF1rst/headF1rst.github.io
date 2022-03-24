---
toc: true
title: "[프로젝트 일기 3/8] '남들이 다 쓰니까'라는 이유는 없다! - 백엔드 편"
category:
  - etc
---
이번 프로젝트를 진행하기 위한 기술 스택들을 정해 보았다. 내가 이 기술을 왜 채택했는지 알고 적용해야 더 효율적으로 사용하고 공부할 수 있을 것이라고 생각한다.

## 사용 기술 🛠️

### Back-end

|Category|Stack|
|---|---|
|Framework|- Spring Boot 2.6.3|
|Test|- JUnit 5|
|Infra|- Nginx 1.14.0 <br> - AWS EC2 18.04.1 <br> - Jenkins 2.305 <br> - Sonarqube 9.0.1|
|Database|- MySql <br> - 공공데이터|

- **Spring Boot**

의존관계 주입을 통해 SOLID 원칙을 지키면서 보다 객체지향적인 프로그래밍이 가능하다.

**Java기반의 좋은 개발 컨텐츠가 많다.**

우리나라는 흔히 ‘자바공화국'이라 불릴만큼 빅테크 기업들이 메인 기술로 스프링을 채택하고 
있다.

매일 수천만 트래픽을 받는 기업들은 매년 컨퍼런스와 기술 블로그에 자신들이 해결한 
노하우를 끊임없이 쏟아낸다. 이를 통해 좋은 코드 디자인과 테스트 코드 작성법, 아키텍처 
등을 참고할 수 있다는 장점이 있다.

책, 강의도 마찬가지다. 당장에 인프런의 유명 강의들만 해도 스프링 부트 강의들이고 ‘클린 
코드', ‘단위 테스트' ‘Effective Unit Testing’ 등 자바 기반의 좋은 책들이 많이 
출시되고 있다.

- **JUnit5**

또한 JUnit을 통한 테스트 코드 작성이 원활하기 때문에 “분석, 설계, 개발, 테스팅"의 
스프린트를 보다 쉽게 진행할 수 있다.

- **Nginx**

Nginx에 리버스 프록시를 적용하게 되면 클라이언트와 서버는 nginx를 통해 통신을 
주고받게 된다.

이로인해 몇가지의 이점을 얻게되는데, 첫번째 이점은 로드 밸런싱이다.

1. **로드 밸런싱** : 요청이 많은 사이트를 운영하는 경우 여러 대의 서버를 두고 
운영을 하게 되는데 이 때, 특정 서버에만 요청이 몰리지 않도록 nginx가 역할을 수행. 
현재 서비스는 하나의 ec2 서버를 사용하지만 사용자가 늘어나고 서버 Scale-Out을 
고려하여 nginx의 로드 밸런싱이 도움이 될것이라 생각하였다.
2. **보안성** : 웹 사이트나 서비스에서 실제 서버의 IP 주소를 필요로 하지 않기 
때문에 DDoS와 같은 공격으로 부터 실제 서버를 보호할 수 있다.
3. **캐싱** : Nginx는 컨텐츠를 캐싱할 수 있기 때문에 결과를 더 빠르게 응답하여 
성능을 높일 수 있다.

- **MySql**

MySql을 사용한 이유는 대표적인 RDBMS 이면서 NoSql에 비해 데이터의 일관성이 더 
보장된다는 장점이 있다. 또한 RDBMS에 JPA를 적용하면 관계형 중심의 DB에서 객체 
지향적으로 데이터베이스를 설계할 수 있게된다.

- **공공 데이터**

공공 데이터는 공공기관에서 제공하는 DB로, 서비스나 애플리케이션, 플랫폼 등을 개발 하는 
데에 사용할 수 있다. 파일 형식이나 Open API 형식으로 제공된 공공 데이터를 개발에 
활용할 경우, 공공기관에서 제공한 데이터인 만큼 신뢰성을 높일 수 있고, 방대한 양의 
데이터를 쉽게 이용할 수 있게 된다.

### Front-end

|Category|Stack|
|---|---|
|Language|Typescript|
|Framework(Library)|React|
|CSS|Styled Components|

- 프론트엔드 기술은 프론트엔드 파트에서 작성했다.
['남들이 다 쓰니까'라는 이유는 없다! - 프론트엔드 편](https://velog.io/@chchaeun/%EB%82%A8%EB%93%A4%EC%9D%B4-%EB%8B%A4-%EC%93%B0%EB%8B%88%EA%B9%8C%EB%9D%BC%EB%8A%94-%EC%9D%B4%EC%9C%A0%EB%8A%94-%EC%97%86%EB%8B%A4-%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C-%ED%8E%B8)

## TEAM 우아한 남매들  👨‍👨‍👧‍👦

|팀원|역할|Github|
|---|---|---|
|고산하|Project Leader / Back End|[@headF1rst](https://github.com/headf1rst)|
|김채은|Project Manager / Front End|[@chchaeun](https://github.com/chchaeun)|
|이준석|Infra / Back End|[@juy4556](https://github.com/juy4556)|
|황승환|DBA / Back End|[@xxOhn](https://github.com/xx0hn)|

![Promisor](https://media.vlpt.us/images/chchaeun/post/1280bfb6-6a99-4654-a16f-91224003006a/edit.png)