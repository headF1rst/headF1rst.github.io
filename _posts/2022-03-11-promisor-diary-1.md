---
toc: true
title: "[프로젝트 일기 3/5] 사용자 관점에서 요구사항을 분석해 보자!"
category:
  - etc
---

**요구사항 분석 이란?**: 개발하고자 하는 소프트웨어에 대한 요구사항을 고객으로 부터 수집하고 분석하여 명세하는 단계

- 목표
- 프로젝트 개요
    - 배경
- 요구사항
- 기능
    - 기능의 대상이 누구고 대상에게 어떤 이득을 주는지 (기대효과)
- 유스케이스 다이어그램
- 팀 역할
- 사용 기술 스팩
    - 해당 기술을 선택한 이유 : 기술적 특징(제한 사항)과 연결지어서 설명
- 원활한 협업을 위한 팀만의 규칙
- 위험 요소 분석

## 프로젝트 개요

친구들끼리 약속을 정하는데 사람이 많으면 각자의 일정이 있고 

1. 모두가 동시에 메신저에 접속한 상태여야 한다.
2. 모두의 개인적인 일정을 고려해서 약속을 잡기 어렵다. (시간)
3. 모두의 접근성을 고려한 장소를 선정하기 어렵다. (장소)

위의 문제들 때문에 모두의 일정에대한 정보와 장소에 관한 정보를 수집해서 최적의 일정과 장소를 정해주는 플랫폼을 고려해보았고 그에대한 데이터를 시각화 하는 서비스가 필요하다고 생각을 하게 되었다.

## 목표 🏹

다수의 인원이 만날 때, 모두의 일정과 위치 접근성을 고려하여 약속 시간과 장소를 선정할 수 있는 서비스를 제공한다.

가입 시 사용자들이 기입한 이메일을 기반으로 사용자를 검색하고, 친구 추가를 할 수 있다. 친구 추가는 일방적인 방식의 추가로 상대 사용자의 허가를 받지 않아도 되도록 하여 간편화 하였고, 이를 통해 추가된 사용자들 중 원하는 인원들로 그룹을 생성할 수 있다. 그룹 생성 시에는 상대 사용자의 허가가 요구되고, 그룹 생성이 완료되면 이를 통해 약속을 생성/관리할 수 있도록 한다.

그룹원들의 가능 날짜와 불가능한 날짜를 하나의 캘린더에 종합하여 시각화하고, 이를 토대로 약속 날짜를 추천하여 약속 날짜를 선정하는 데에 도움을 준다.

약속 날짜가 이미 결정된 경우에는 그룹장이 약속 날짜로 지정할 수 있고, 해당 약속 뿐만 아니라 사용자가 포함된 모든 그룹에서의 약속 날짜를 자신만의 캘린더로 관리해 줌으로써 사용자의 약속 관리에도 도움을 준다. 약속 당일 아침에는 약속이 있음을 브로드캐스팅으로 알려줌으로써 상기시켜준다.

그룹원들의 주소를 기반으로 중간 지점에 있는 장소를 다른 사용자들의 선택 횟수를 기준으로 장소를 추천하고, 각 장소에 존재하는 맛집이나 가볼만한 플레이스를 사용자에게 제공함으로써 약속 장소를 지정하는 데에 도움을 준다. 이때 맛집, 플레이스에 대한 다른 사용자들의 별점과 코멘트도 사용자의 선택에 영향을 줄 수 있다.

채팅방을 구현하여 그룹원들 간의 소통이 가능하도록 하고, 앞서 장소에서 제공하는 맛집, 플레이스 같은 정보를 채팅방 내에서 공유함으로써 약속의 완성도를 높일 수 있도록 한다.

위와 같은 방식으로 그룹 입장에서 사용자들 간에 즉각적인 의사소통 없이도 입력된 데이터를 통합하고 시각화하여 약속 생성 과정에서의 최선의 선택을 할 수 있도록 도움을 주고, 사용자 개인의 입장에서 자신이 속한 그룹들의 약속을 한눈에 확인하고 관리할 수 있도록 도움을 준다.

## 요구사항 🗣️

1. 그룹원들간의 일정을 데이터화 하여 한눈에 관리 및 파악한다.
2. 서로의 일정을 캘린더를 사용하여 표시하고 공유할 수 있게 한다. (불가, 가능 날짜 표시)
3. 모두가 만족할 만한 일정과 장소를 모아서 후보로 추천한다.
4. 그룹원 개개인의 출발 장소를 모아서 모두에게 이동 시간이 공평한 중간 장소를 찾아준다.
5. 날짜/장소 투표 기능을 만든다.
6. 안되는 날짜에 사유를 남길수 있다.
7. 해당 장소의 맛집, 핫플을 추천해 준다.
8. 사용자들이 방문한 가게에 대한 별점과 코멘트를 남길 수 있다.
9. 회원가입은 자체 로그인과 소셜 로그인으로 한다.
10. 사용자 프로필을 만든다(이름, 프로필 사진 등)
11. 친구를 추가하고 그룹을 만든다.
12. 그룹원들간에 채팅을 할 수 있다.
13. 후보 장소와 후보 날짜를 채팅방에 공유할 수 있다.
14. 사용자가 잡은 약속을 한눈에 볼수있다
15. 약속 당일에 약속에 대한 알림이 온다.

## 요구사항 우선 순위 ⬆️

**Client-Driven : 많이 사용되는 기술 우선**

1. 회원가입은 자체 로그인과 소셜 로그인으로 한다.
2. 사용자 프로필을 만든다(이름, 프로필 사진 등)
3. 친구를 추가하고 그룹을 만든다.
4. 서로의 일정을 캘린더를 사용하여 표시하고 공유할 수 있게 한다. (불가, 가능 날짜 표시)
5. 안되는 날짜에 사유를 남길수 있다.
6. 그룹원 개개인의 출발 장소를 모아서 모두에게 이동 시간이 공평한 중간 장소를 찾아준다.
7. 모두가 만족할 만한 일정과 장소를 모아서 후보로 추천한다.
8. 해당 장소의 맛집, 핫플을 추천해 준다.
9. 그룹원들간에 채팅을 할 수 있다.
10. 그룹별로 약속을 한눈에 볼 수 있다.
11. 사용자가 잡은 약속을 한눈에 볼수있다.
12. 후보 장소와 후보 날짜를 채팅방에 공유할 수 있다.
13. 사용자들이 방문한 가게에 대한 별점과 코멘트를 남길 수 있다.
14. 약속 당일에 약속에 대한 알림이 온다.
15. 날짜/장소 투표 기능을 만든다.

## 기능

요구사항을 위해 필요한 기능과 설명을 표로 정리했다. 요구사항 10번까지의 기능을 최우선으로 개발하는 것을 목표로 잡았다.

![스크린샷 2022-03-10 오후 5.15.13.png](https://i.imgur.com/nECsm5R.png)

## 팀 역할  👨‍👨‍👧‍👦

| 팀원 | 역할 |
| --- | --- |
| 고산하 | Project Leader / Back End |
| 김채은 | Project Manager / Front End |
| 이준석 | Infra / Back End |
| 황승환 | DBA / Back End |

## 사용 기술 🛠️

- Back-end
    - **Spring Boot** 2.5.2
    - **JUnit** 5
    - **Nginx** 1.14.0
    - **MySql**
    - **AWS Ec2** 18.04.1
    - **Jenkins** 2.305
    - **Sonarqube** 9.0.1

객체 지향적으로 프로그램을 설계하는 것은 유지보수 측면에서 중요합니다. 클라이언트의 요구사항은 언제 바뀔지 모르기때문에 객체지향적인 설계를 통해서 어떠한 변수에도 대처할 수 있는 코드가 만들어져야 합니다.

저희는 실제 서비스를 배포하는것 까지 고려를 하고 있기 때문에 실제 서비스를 운영하면서 발생하는 오류들에대해 유지보수를 고려하는 것은 중요하다고 생각했습니다.

자바 기반 프레임 워크인 Spring boot는 객체지향 언어인 자바를 더욱 더 객체지향적으로 프로그램을 작성하는데 도움을 줍니다.

Spring boot의 의존관계 주입(DI)를 통해서 클라이언트 코드를 수정하지 않고, 클라이언트가 호출하는 대상의 타입 인스턴스를 변경할 수 있습니다. 이 덕분에 SOLID 원칙의 OCP를 지키면서 객체지향적으로 프로그램을 설계하는 것이 가능합니다. 

Spring Boot는 여러 기업에서 가장 널리 사용되는 프레임 워크입니다. 때문에 관련 레퍼런스가 많고 참고할 자료가 많다는 점에서 프로젝트 과정에서 발생하는 여러 문제를 해결하는데 도움이 됩니다.

또한 JUnit을 통한 테스트 코드 작성이 원활하기 때문에 “분석, 설계, 개발, 테스팅"의 스프린트를 보다 쉽게 진행할 수 있습니다.

Nginx에 리버스 프록시를 적용하게 되면 클라이언트와 서버는 nginx를 통해 통신을 주고받게 됩니다.

이로인해 몇가지의 이점을 얻게되는데, 첫번째 이점은 로드 밸런싱입니다.

1. **로드 밸런싱** : 요청이 많은 사이트를 운영하는 경우 여러 대의 서버를 두고 운영을 하게 되는데 이 때, 특정 서버에만 요청이 몰리지 않도록 nginx가 역할을 수행합니다. 현재 저희 서비스는 하나의 ec2 서버를 사용하지만 사용자가 늘어나서 서버 Scale-Out을 고려하여 nginx의 로드 밸런싱이 도움이 될것이라 생각하였습니다.
2. **보안성** : 웹 사이트나 서비스에서 실제 서버의 IP 주소를 필요로 하지 않기 때문에 DDoS와 같은 공격으로 부터 실제 서버를 보호할 수 있습니다.
3. **캐싱** : Nginx는 컨텐츠를 캐싱할 수 있기 때문에 결과를 더 빠르게 응답하여 성능을 높일 수 있습니다.

MySql을 사용한 이유는 대표적인 RDBMS 이면서 NoSql에 비해 데이터의 일관성이 더 보장됩니다. 또한 RDBMS에 JPA를 적용하면 관계형 중심의 DB에서 객체 지향적으로 데이터베이스를 설계할 수 있습니다.

- Front-end

**타입스크립트(Typescript)**
타입스크립트는 자바스크립트의 슈퍼셋인 오픈소스 프로그래밍 언어이다. 코드 작성 단계에서 타입을 체크해 오류를 확인할 수 있어서 버그를 사전에 예방할 수 있다. 또한 미리 타입을 결정하기 때문에 메모리 낭비를 줄일 수 있고 실행 속도가 빠르다.

좋은 코드의 중요한 조건 중 하나는 읽기 좋은 코드를 짜는 것이다. 내가 쓴 코드를 언제나 내가 유지보수 한다는 보장은 없다. 타입스크립트는 어떤 데이터 형식이 넘어오는지 코드를 보고 바로 알 수 있고, 바꾼 코드로 인해 생길 수 있는 side effect를 알 수 있다. 따라서 새로운 담당자가 코드를 마주해도 수월하게 기능 개발과 유지보수를 할 수 있다. 현재 프론트엔드 개발자는 한명이지만, 프로젝트를 실제로 운영하며 확장해갈 계획이 있기 때문에 기술 선택에 있어서 유지보수 측면을 가장 중요하게 고려했다.

**리액트(React)**
리액트는 자바스크립트 라이브러리의 하나로, 동적인 웹 페이지 개발을 위해 사용된다. 컴포넌트 단위로 코드를 작성하고, 하나의 컴포넌트를 여러 부분에서 사용할 수 있다.
애플리케이션 개발이라는 문제가 주어졌을 때, 이를 한번에 해결할 수는 없다. 페이지, 조직, 컴포넌트 등 단계에 걸쳐 작게 쪼개어 문제를 해결하고 다시 합치는 과정을 반복했을 때 이 거대하고 복잡한 문제를 해결할 수 있게 된다. 거대하고 복잡한 UI를 컴포넌트를 통해 기능별로 캡슐화하면 유지보수 측면에서 유리하게 작용할 수 있다.
또한 리액트는 매우 널리 사용되는 라이브러리이기 때문에 에러가 발생했을 때 도서, 국내외 사이트 등 도움을 얻을 만한 소스가 다양하다는 장점이 있다.

**스타일 컴포넌트(Styled Components)**
스타일 컴포넌트는 CSS가 지정된 HTML 요소를 생성하는 라이브러리이다. 요소의 이름을 사용자가 설정할 수 있기 때문에 다른 사람이 봤을 때도 이 컴포넌트가 어떤 기능을 하는지 한눈에 알 수 있다. 이는 HTML 요소만으로 코드를 구성하는 것보다 유지보수 측면에서 훨씬 유리하게 작용할 수 있다.
또한 스타일 컴포넌트는 상속을 통해 스타일을 재사용할 수 있다. 본격적으로 애플리케이션을 개발하기 전 필요한 컴포넌트를 설계하고 자주 사용될 것으로 예상되는 컴포넌트 스타일을 일반화해 만들어둔다면, 일반화한 컴포넌트를 재사용함으로써 개발 속도가 굉장히 빨라지고 메모리도 절약할 수 있다.

- Communication
    - Jira
    - slack

![1_1hUw16iwwDx40qDo5ViezA.png](%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%20%2007bec/1_1hUw16iwwDx40qDo5ViezA.png)

## 원활한 협업을 위한 팀만의 규칙 🤝

1. 프로젝트 중 발생한 문제를 함께 그려가자.
    - 혼자 생각하고 있다고 달라지는 것은 없다!
2. 아웃 오브 사이트, 아웃 오브 마인드. 👨‍👨‍👧‍👦
    - 회의는 가능한 오프라인으로 진행한다.
3. 시간은 금이다. ❌
    - 지각은 금물이다.
    - 사정이 있으면 최소한 하루 전에 얘기한다.
4. 저스트 두 잇!
    - 에러 만들 것을 지레 걱정하지 말자.
5. 인생은 즐겁게, 일상은 치열하게.
6. 새롭게 배운 것들은 공유하자.
7. 말 아끼다가 똥 된다. 💩
    - 좋은 아이디어가 있으면 말하자.
8. 밥은 먹고 하자. 🍚
9. 기능을 추가하기 위해서는 모든 사람의 동의가 필요하다. ✅
10. 완성도는 꾸준함에서 온다. 🏃
    - 1일 1커밋 하기