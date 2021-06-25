---
toc : true
title : "[Server] APM 컴파일 수동 설치하기 1/3 - Apache-2.4.46"
category : 
    - Back-End
---
### Virtual Box + Ubuntu 20.04 설치
리눅스 환경에서 과제를 수행하기 위해 가상머신을 사용해야 했습니다.
가상환경으로는 `VirtualBox`, 우분투는 20.04 버전을 다운받아 사용하였습니다. Mac OS 베이스 환경에서 작업을 하였고 VirtualBox와 우분투 다운로드 방법은 아래의 유튜브 링크를 참고해 주시면 되겠습니다. + $($우분투 이미지의 디스크 용량은 40GB 이상을 권장드립니다.)

- [우분투, 버추얼 박스 설치 및 세팅](https://www.youtube.com/watch?v=Hzji7w882OY)

### Apache 2.4.46 컴파일 설치
- /usr/local 디렉토리에 설치하는 것이 관례입니다.
- 소스파일은 /usr/local/src 에 저장합니다.

다음은 컴파일 설치를 위한 필수 패키지 입니다.<br>
`$ sudo su` <br>
`# apt-get install gcc zlib1g zlib1g-dev libssl-dev openssl libxml2-dev ncurses-dev` <br>

**zlib1g 에서 b와 g사이의 수는 영어 L의 소문자 l 이 아닌 숫자 "1" 인것을 주의하시기 바랍니다.**

>**sudo su** : sudo $($Super User DO)는 유닉스 계열 OS에서 슈퍼유저로 프로그램을 구동할 수 있도록 권한을 부여하는 명령어 입니다. 즉 관리자 모드로 전환되어 프로그램을 수행합니다.
<br>

>**apt** : apt $($Advanced Packaging Tool)는 소프트웨어 패키지의 확인,구성,설치를 자동화 해줍니다.

### 의존성 패키지 설치 $($APR, PCRE)

- **APR $($Apache Portable Runtime)** : 아파치 HTTP 서버 2.x.의 핵심이며 휴대용 라이브러리

- **PCRE $($Perl Compatible Regular Expressions)** : 펄 호환 정규 표현식으로서, 정규식 패턴 일치를 구현하는 함수의 집합

**1. /usr/local에 apache 디렉토리 생성**

`# cd usr` <br>
`# cd local` <br>
`# mkdir apache` <br>

**2. apr, apr-util 다운 및 압축 해제**

`# wget http://mirror.navercorp.com/apache//apr/apr-1.7.0.tar.gz` <br>
`# wget http://mirror.navercorp.com/apache//apr/apr-util-1.6.1.tar.gz` <br>
`# tar xvfz apr-1.7.0.tar.gz` <br>
`# tar xvfz apr-util-1.6.1.tar.gz` <br>

>**wget** : wget$($web get), 웹상의 파일을 다운받을때 사용

>**tar xvfz** : tar.gz 파일 압축해제

**3. apr, apr-util 설치**

다음은 apr 설치 <br>
`# cd usr/local/apr-1.7.0` <br>
`# ./configure --prefix=/usr/local/apr` <br>
`# make` <br>
`# make install` <br>

다음은 apr-util 설치 <br> 
`# cd usr/local/apr-util-1.6.1` <br>
`# ./configure --with-apr=/usr/local/apr --prefix=/usr/local/apr-util` <br>
`# make` <br>
`# make install` <br>

![apr-util-error](/assets/images/Back_End/apr-util-error.png) 

apr-util 설치 중 위와같은 에러가 발생하였고  libexpat1-dev 패키지가 없어서 발생한 것이었습니다.

`# apt-get install libexpat1-dev`

다음과 같이 패키지를 설치해 주어 해결할 수 있었습니다.

### PCRE 다운 및 압축해제, 설치

`# cd usr/local` <br>
`# wget ftp://ftp.pcre.org/pub/pcre/pcre-8.43.tar.gz`<br>
`# tar xvfz pcre-8.43.tar.gz` <br>
`# cd usr/local/pcre-8.43` <br>
`# ./configure --prefix=/usr/local/pcre` <br>
`# make` <br>
`# make install` <br>

### Apache 2.4.46 설치

아파치 다운 및 압축해제 <br>
`# cd usr/local` <br>
`# wget http://apache.tt.co.kr//httpd/httpd-2.4.46.tar.gz` <br>
`# tar xvfz httpd-2.4.46.tar.gz` <br>

아파치 설치 <br>
`# cd httpd-2.4.46` <br>
`# ./configure --prefix=/usr/local/apache2 \` <br>
`> --enable-module=so --enable-rewrite --enable-so \` <br>
`> --with-apr=/usr/local/apr \` <br>
`> --with-apr-util=/usr/local/apr-util \` <br>
`> --with-pcre=/usr/local/pcre \` <br>
`> --enable-mods-shared=all` <br>
`# make` <br>
`# make install` <br>

make를 하는 과정에 에러가 발생하였습니다.

![no make file found](/assets/images/Back_End/no_makefile_found.png) 

apr-util 부분에서 오류가 발생하는 것을 알아냈고 `make clean`을 통해 apr-util을 삭제 후 다시 설치하고 실행하였더니 make를 수행할 수 있었습니다. 

![apr-util-clean](/assets/images/Back_End/apr-util-clean.png)

![apr-util-error](/assets/images/Back_End/apr-util-install.png) 

### Apache 실행

`# sudo /usr/local/apache2/bin/httpd -k start` <br>
`# ps -ef | grep httpd | grep -v grep` <br>
`# netstat -anp | grep httpd` <br>
`# curl http://127.0.0.1` <br>

> **httpd -k start** : apache 실행

> **httpd -k stop** : apache 종료

![apr-util-error](/assets/images/Back_End/apache_execute.png) 

![apr-util-error](/assets/images/Back_End/result-apache.png) 

![apr-util-error](/assets/images/Back_End/ItWorks.png) 
