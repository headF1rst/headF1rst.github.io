---
toc : true
title : "[Server] APM 컴파일 수동 설치하기 3/3 - PHP-7.4.1"
category : 
    - Infra
---

### 환경
아래의 세팅이 완료된 상황에서 진행하였습니다. 

**Installed list**
- VirtualBox 6.1.18
- Ubuntu 20.04
- Apache 2.4.46
- MySQL 8.0.19

### PHP 7.4.1 컴파일 설치

**1. php 컴파일 설치를 위한 패키지 설치**

`/usr/local# apt-get install libxml2-dev` <br>
`/usr/local# apt-get install libjpeg-dev` <br>
`/usr/local# apt-get install libpng-dev` <br>
`/usr/local# apt-get install libsqlite3-dev` <br>

**2. php 설치**

php-7.4.1.tar.gz 파일을 다운받은 후 압축해제하여 줍니다.

`/usr/local# wget https://www.php.net/distributions/php-7.4.1.tar.gz`
`/usr/local# tar xvfz php-7.4.1.tar.gz`


`/usr/local/php-7.4.1# ./configure \` <br>
`> --with-apxs2=/usr/local/apache2.4/bin/apxs \` <br>
`> --enable-mysqlnd \` <br>
`> --with-mysql-sock=mysqlnd \` <br>
`> --with-mysqli=mysqlnd \` <br>
`> --with-pdo-mysql=mysqlnd \` <br>
`> --with-imap-ssl \` <br>
`> --with-iconv \` <br>
`> --enable-gd \` <br>
`> --with-jpeg \` <br>
`> --with-libxml \` <br>
`> --with-openssl` <br>


![kill_error](/assets/images/Back_End/install_php.png) 

`/usr/local/php-7.4.1# make` <br>
`/usr/local/php-7.4.1# make test` <br>
`/usr/local/php-7.4.1# make install` <br>

![kill_error](/assets/images/Back_End/php_install.png) 

![kill_error](/assets/images/Back_End/php-module.png) 

**3. Apache와 php연동**

아파치 설정파일인 $($httpd.conf)를 열어서 php 모듈이 정상적으로 설치되어있는지 확인해 줍니다.

`/usr/local# vi apache2.4/conf/httpd.conf` 

![kill_error](/assets/images/Back_End/php모듈설치확인.png) 

모듈이 잘 설치되어 있다면 `min_module`에 아래와 같은 내용을 AddType 해줍니다.

![kill_error](/assets/images/Back_End/addtype_in_mime_moudle.png) 

**4. php.ini 파일 세팅**

> **php.ini** : php의 설정 파일

프로덕션 시스템용 설정 파일인 php.ini-production 파일을 /usr/local/lib/php.ini에 복사해 줍니다.

`/usr/local/php-7.4.1# cp php.ini-production /usr/local/lib/php.ini` 

**5. 테스트용 php파일 작성**

`/usr/local/apache2.4/htdocs# vi phpinfo.php`

![kill_error](/assets/images/Back_End/phpinfo_edit.png) 

**<? php> 와 같이 띄어쓰기를 하면 phpinfo 페이지가 나오지 않기 때문에 주의해야 합니다**

`/usr/local# apache2.4/bin/httpd -k start` <br>
`/usr/local# ps -ef | grep httpd | grep -v grep` <br>
`/usr/local# netstat -anp | grep httpd` <br>

`/usr/local# curl http://127.0.1.1` <br>

![kill_error](/assets/images/Back_End/php_works.png) 

**결과**

![kill_error](/assets/images/Back_End/php_res.png)
