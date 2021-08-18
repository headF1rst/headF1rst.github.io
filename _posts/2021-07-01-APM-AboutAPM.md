---
toc : true
title : "[Server] APM $($Apache, PHP, Mysql) 이란?"
category : 
    - Back_End
---

APM 이란 하나의 소프트웨어를 특정 짓는 것이 아닌 웹 서버 구축을 위한 Apache, Php, Mysql을 의미하는 것입니다. 

물론 APM $($Application Performance Management)과 같은 다른 의미의 APM도 존재하지만 여기서는 전자의 의미에 대해서 다뤄보겠습니다.

### Apache $($웹 서버)

<center><img src = "/assets/images/Back_End/apache.png"></center>

<br>
아파치는 클라이언트에게 웹 서비스를 제공해주는 웹 서버입니다.

웹 서버는 클라이언트의 웹 브라우저에게 요청을 받으며 해당 요청에 알맞는 응답을 제공해 주는 역할을 담당합니다.

예를 들어 클라이언트가 웹 사이트에 접속하게 되면 웹 서버에게 HTML 파일을 요청합니다. 이때 웹서버는 요청 받은 HTML파일을 클라이언트 컴퓨터에 제공하게 되며 클라이언트는 웹 사이트를 볼 수 있게 됩니다.

웹 서버에는 아파치 뿐만 아니라 nginx, Lighttpd, IIS, node.js - $($자체 웹 서버 내장) 등 다양한 웹 서버가 존재합니다.

### PHP $($웹 프로그래밍 언어)

<center><img src = "/assets/images/Back_End/PHP-logo.svg" width="300" height="200"></center>

<br>
PHP $($Personal Home Page)는 서버에서 실행되는 웹 프로그래밍 언어입니다. 

PHP는 Apache 웹 서버와 연동하여 동작하며 PHP는 .php 파일을 Apache 는 HTML 파일을 처리합니다.

PHP는 서버에서 소스코드를 해석하여 HTML 코드로 만들어 브라우저에 전달합니다. 그러면 브라우저는 PHP가 HTML로 해석해준 소스를 해석하여 디스플레이 하게 됩니다. 

### MySQL $($데이터베이스)

<center><img src = "/assets/images/Back_End/mysql_image.png" width="300" height="150"></center>

<br>
MySQL은 가장 널리 쓰이고 있는 관계형 데이터베이스 관리 시스템입니다$($Relational DBMS). 

오픈소스 데이터베이스 이며 현재는 상업용인 MySQL과 무료 버전인 MariaDB로 나뉘어 집니다.

### APM 구동 원리

**1. 클라이언트가 URL을 통해 서버에 원하는 정보를 request 한다.**

**2. 아파치는 승인된 접속자에게 해당 정보를 제공하기 위해서 PHP에게 스크립트 실행을 요청한다.**

**3. PHP는 미리 작성된 프로그램을 통해 MySQL에 알맞는 쿼리를 질의한다.**

**4. MySQL은 질의에 알맞는 데이터를 추출하여 PHP에게 response 한다**

**5. PHP는 스크립트 실행 결과를 HTML 형태로 변경한 다음 아파치에 전달한다.**

**6. 아파치는 HTML파일을 클라이언트 측의 웹브라우저에 전달한다.**

<center><img src = "/assets/images/Back_End/apmWorkflow.png" width="450" height="350"></center>
