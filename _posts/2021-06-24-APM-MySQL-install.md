---
toc : true
title : "[Server] APM 컴파일 수동 설치하기 2/3 - MySQL-8.0.19"
category : 
    - Back-End
---
### 환경
아래의 세팅이 완료된 상황에서 진행하였습니다.

**Installed list**
- VirtualBox 6.1.18
- Ubuntu 20.04
- Apache 2.4.46

### MySQL 설치

**1. MySQL 컴파일 설치를 위한 패키지 설치** <br>
`$ sudo su` <br>
`/usr/local# apt-get update` <br>
`/usr/local# apt-get install cmake` <br>
`/usr/local# apt-get install libssl-dev` <br>
`/usr/local# apt-get install libboost-all-dev` <br>
`/usr/local# apt-get install libncurses5-dev libncursesw5-dev` <br>

[MySQL 의존성 패키지 설치 공식 문서](https://dev.mysql.com/doc/refman/8.0/en/source-installation-prerequisites.html)를 참고하였습니다.

**2. MySQL Community Server 8.0.19 설치** <br>
Apache 설치 과정과 마찬가지로 `wget`을 사용하여 mysql-8.0.19 압축 파일을 사이트로 부터 다운받은 다음 `tar xvfz`로 압축을 해제해 줍니다. <br>

`/usr/local# wget https://dev.mysql.com/get/Downloads/MySQL-8.0/mysql-8.0.19.tar.gz` <br>
`/usr/local# tar xvfz mysql-8.0.19.tar.gz` <br>

**3. MySQL 설치**

> **cmake** : 설치 옵션 부여 <br>
> **make** : build, 컴파일 <br>
> **make install** : 컴파일한 파일 설치 진행 <br>

mysql-8.0.19 디렉토리 내에 새로운 디렉토리 생성후 그안에서 cmake을 수행해야 합니다... <br> $($저의 경우 "springmysql" 이라는 이름의 디렉토리를 생성해 주었습니다.)


`/usr/local/mysql-8.0.19/springmysql# cmake \` <br>
`> .. \` <br>
`> -DCMAKE_INSTALL_PREFIX=/usr/local/mysql \` <br>
`> -DMYSQL_DATADIR=/usr/local/mysql/data \` <br>
`> -DMYSQL_UNIX_ADDR=/usr/local/mysql/mysql.sock \` <br>
`> -DMYSQL_TCP_PORT=3306 \` <br>
`> -DDEFAULT_CHARSET=utf8 \` <br>
`> -DDEFAULT_COLLATION=utf8_general_ci \` <br>
`> -DSYSCONFDIR=/etc \` <br>
`> -DWITH_EXTRA_CHARSETS=all \` <br>
`> -DWITH_INNOBASE_STORAGE_ENGINE=1 \` <br>
`> -DWITH_ARCHIVE_STORAGE_ENGINE=1 \` <br>
`> -DWITH_BLACKHOLE_STORAGE_ENGINE=1 \` <br>
`> -DDOWNLOAD_BOOST=1 \` <br>
`> -DWITH_BOOST=/usr/local/mysql/boost` <br>

`/usr/local/mysql-8.0.19/springmysql# make` <br>
`/usr/local/mysql-8.0.19/springmysql# make install`


만약 springmysql과 같은 디렉토리를 새로 생성해 주지 않았다면 다음과 같은 오류가 발생합니다.

![dforce](/assets/images/Back_End/dforce.png) 

에러 메시지가 알려주는대로 cmake 옵션에 `-DFORCE_INSOURCE_BUILD=1` 을 추가해도 되지만 후에 문제가 발생할 이유가 될 수 있기 때문에 권장 드리지는 않습니다.

디렉토리를 생성하고 cmake를 잘하여도 다음과 같은 에러가 발생하였습니다.

![dforce](/assets/images/Back_End/cmake-boost-error.jpg) 

이는 boost_1_73_0.tar.gz 파일을 `/usr/local/mysql/boost` 경로에 다운받는 것을 실패하여 발생하는 것이었습니다.

에러 메세지에서 boost파일을 다운받기 권장하는 사이트는 Forbidden 되어 있기 때문에 "Sourceforge" 사이트에서 에러 메세지에 알맞는 버전의 boost 파일을 다운받으면 해결됩니다.

`/usr/local/mysql/boost# wget https://sourceforge.net/projects/boost/files/boost/1.73.0/boost_1_73_0.tar.gz` <br>

`/usr/local/mysql/boost# tar xvfz https://sourceforge.net/projects/boost/files/boost/1.73.0/boost_1_73_0.tar.gz` <br>

![sol](/assets/images/Back_End/cmake_error_sol.png) 

그다음 `make`로 컴파일 하는 과정이 큰 관문이었습니다. <br>
일단 컴파일 하는데 상당히 오랜시간이 소요되었고, 컴파일 [69%]에서 다음과 같은 오류가 발생하였습니다.

![kill_error](/assets/images/Back_End/signal_9_error.png) 

너무 많은 프로세스에 의해 메모리의 과부하가 생겨 시스템이 프로세스를 일부 죽이게 되면서 발생하는 오류였습니다.

**첫번째 시도**

첫번째로 시도했던 방법은 `vm.overcommit` 입니다.

overcommit은 순간적으로 많은 양의 메모리를 필요로 하는 작업을 할때 필요합니다.
![kill_error](/assets/images/Back_End/vm.overcommit.png) 

2로 세팅하여 가용한 메모리 안에서만 할당 가능하게 설정 하였지만 이번엔 우분투 자체에 이상이 생기는 현상이 발생하였습니다.

**두번쨰 시도**

`df -h`를 사용하여 디스크의 용량 상태를 확인해 보았습니다.

![kill_error](/assets/images/Back_End/df-h.png) 

sda5 디스크 파티션을 보면 제가 초기에 설정한 VM 이미지의 디스크 용량인 24GB를 전부 사용하고 있는것을 알아낼 수 있었습니다.

journal도 지워보고 swap공간도 늘려보고 vmware 세팅에서 디스크 크기를 동적으로 변경도 해보았지만 해결이 안되었고 결국 기존 이미지를 지우고 40GB로 설정하여 다시 새로 다운 받았습니다. $($덕분에 apache 설치부터 전부 다시하게 되었습니다...)

**디스크를 40GB로 다시 다운받고 make**

`cmake`로 설치옵션을 부여하는 과정을 다시 마치고 나서 `make`로 컴파일 해주었습니다. 

![kill_error](/assets/images/Back_End/make_success.png) 

`make install`까지 성공적으로 설치할 수 있었습니다.

### MySQL 데이터베이스 초기화
<br>

**mysql 그룹 및 유저 생성**

`# groupadd mysql` <br>
`# useradd -r -g mysql -s /bin/false mysql` <br>

**mysql-files 디렉토리 생성**

`/usr/local/# cd mysql` <br>
`/usr/local/mysql# mkdir mysql-files` <br>

**권한 설정**

> **chown** : change own, 파일의 소유권자 변경 <br>
> **chmod** : change mod, 파일과 디렉토리의 사용 권한 변경

`/usr/local/mysql# chown -R mysql:mysql /usr/local/mysql` <br>
`/usr/local/mysql# chown mysql:mysql mysql-files` <br>
`/usr/local/mysql# chmod 750 mysql-files` <br>

**기본 데이터베이스 설정**


`/usr/local/mysql/bin# ./mysqld --initialize --user=mysql \` <br>
`> --basedir=/usr/local/mysql \` <br>
`> --datadir=/usr/local/mysql/data` <br>

![kill_error](/assets/images/Back_End/db-setting.jpg)

서버에 접속하기 위한 임시 비밀번호가 생성된 것을 볼 수 있습니다.

### 서버 실행 및 종료
<br>

**서버 실행**

`/usr/local/mysql/bin# ./mysqld_safe --user=mysql &`

![kill_error](/assets/images/Back_End/server-sql-start.png)

&로 백그라운드에서 프로세스를 수행시켜 주었습니다.

**서버 실행됐는지 확인**

`$ ps -ef | grep mysqld` 

![kill_error](/assets/images/Back_End/server-sql-check.png) 

**서버 연결**

`/usr/local/mysql/bin# ./mysql -u root -p` 

발급받은 임시 비밀번호를 입력하고 아래의 명령어를 통해 비밀번호를 변경해 주어야 합니다.

**비밀번호 변경**

`mysql> alter user 'root'@'localhost' identified with mysql_native_password by '바꿀 패스워드';` 


![kill_error](/assets/images/Back_End/pw-ch.png)

**서버 종료**

`/usr/local/mysql/bin# ./mysqladmin -u root -p shutdown` 

![kill_error](/assets/images/Back_End/server-end-check.png)

`ps -ef | grep mysqld` 명령어로 서버가 종료된 것을 확인할 수 있습니다.

### Mysql service 등록

`/usr/local/# cp /usr/local/mysql/support-files/mysql.server /etc/init.d/mysqld` <br>
`/usr/local/# vi /etc/init.d/mysqld` <br>

아래와 같이 basedir, datadir 뒤에 경로를 추가해 줍니다.

![kill_error](/assets/images/Back_End/base&datadir.png)

**mysql service 등록**

`/usr/local# update-rc.d mysqld defaults`

**service 명령어 실행**

service 명령어를 통해 간단하게 서버를 실행시키고 종료시킬 수 있습니다.

`/usr/local# service mysql start` <br>
`/usr/local# service mysql stop` <br>
`/usr/local# service mysql restart` <br>
`/usr/local# service mysql status` <br>

나가기 위해서는 q를 입력하시면 됩니다.

![kill_error](/assets/images/Back_End/mysql_works.png)

**환경 설정**

`/usr/local/# vi /etc/my.cnf` 

my.cnf 파일에 다음을 입력해 줍니다.

```
[mysqld]
bind-address=0.0.0.0
port=3306
basedir=/usr/local/mysql
datadir=/usr/local/mysql/data
``` 
