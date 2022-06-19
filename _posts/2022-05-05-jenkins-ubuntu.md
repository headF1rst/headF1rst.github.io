---
toc: true
title: "Ubuntu 환경에서의 Jenkins Pipeline 구현"
category:
    - Infra
---

`Promisor` 프로젝트를 진행하면서 저희 팀은 **에자일** 프로세스를 적용하여 협업을 하고 있는데요.

스프린트 단위로 목표를 설정하고 매일 아침마다 간단한 회의를 진행하는 것도 중요하지만 제가 생각하는 에자일의 핵심은 **주기적인 테스트, 빌드, 그리고 배포 사이클**이었습니다.

매번 일일이 빌드하고, 테스트 서버에 배포하는 과정이 귀찮게 느껴지기 시작할 즘에 CI/CD를 프로젝트에 도입하자는 의견을 내보았고, 팀원 모두 동의하여 제가 CI/CD 파이프라인을 구축하게 되었습니다.

## 0. Travis 😭 vs Jenkins 😁

여러 CI/CD tool 중에 후보군이 Travis와 Jenkins로 좁혀지게 되었는데요.

Travis의 경우 다음과 같은 단점이 있었습니다.

- 플러그인이 Jenkins에 비해 다양하지 않으며 레퍼런스가 적다.
- 상용 서비스용 유료 플랜이 별도로 존재한다.
- `.travis.yml` 파일을 수정하고 테스트 하기 위해서는 git push를 반복해야한다.
- Travis-CI는 로컬의 CI 환경과 동일한 빌드 환경을 제공하지 않는다.

## 1. EC2 Docker 설치 및 Jenkins 컨테이너 실행

먼저 도커에 필요한 패키지를 설치해 주었습니다.

```bash
$ sudo apt-get update

// 의존성 패키지 설치
$ sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common

// Docker 패키지 인증 키 추가
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

// Docker 저장소 추가
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs stable"

// 저장소 업데이트
$ sudo apt-get update
```

Docker 패키지가 설치되었는지 확인해 줍니다.

`sudo apt-cache search docker-ce`

![스크린샷 2022-04-29 오후 6.34.21.png](https://i.imgur.com/lkGsVz4.png)

성공적으로 설치가 된것을 확인하고 Docker를 본격적으로 설치해 주었습니다.

``` bash
// Docker 설치
$ sudo apt-get install docker-ce docker-ce-cli containerd.io

// 사용자를 Docker 그룹에 포함
$ sudo usermod -aG docker $USER

// 권한 부여 반영
$ newgrp docker
```

사용자를 Docker 그룹에 포함시켜서 sudo 권한 없이도 Docker 명령어를 실행할 수 있게 됩니다.

### Jenkins 컨테이너 준비

Docker Hub에서 Jenkins 이미지를 확보하였습니다.

``` bash
$ docker pull jenkins/jenkins:lts
```

docker images 명령어를 통해서 이미지가 성공적으로 받아와졌는지 확인하였습니다.

``` bash
$ docker images
```

![스크린샷 2022-04-29 오후 7.11.39.png](https://i.imgur.com/6U92DVJ.png)

받아온 이미지를 컨테이너에 적재하였습니다.

``` bash
$ docker run --name jenkins-docker -d -p 8080:8080 -p 50000:50000 -v /home/jenkins:/var/jenkins_home -u root jenkins/jenkins:lts
```

![스크린샷 2022-04-29 오후 9.59.25.png](https://i.imgur.com/eoRDPhm.png)

- **-d**: 백그라운드 실행
- **-p**: 컨테이너와 호스트 PC 연결을 위한 내부 포트, 외부 포트 연결
- **-v**: 이미지의 /var/jenkins_home 디렉토리를 Host PC 내에 마운트

$($Jenkins 설치 시 ssh 키값 생성, 저장소 참조 등에 용이합니다.)

home 디렉터리에 jenkins 폴더가 생성되었습니다.

![스크린샷 2022-04-29 오후 10.04.09.png](https://i.imgur.com/RNbJArS.png)

Jenkins 포트가 정상적으로 할당됐는지 확인해 보았습니다.

``` bash
$ sudo netstat -antp
```

![스크린샷 2022-04-29 오후 10.12.44.png](https://i.imgur.com/gdJpd7n.png)

80포트로 들어온 요청이 8080으로 연결 되는지 확인해 줍니다.

`curl localhost`

![스크린샷 2022-04-29 오후 5.05.27.png](https://i.imgur.com/nGMRTAC.png)

EC2 인스턴스의 탄력적 IP로 부터 발급 받은 퍼블릭 IP로 Jenkins에 접속하였습니다.

![스크린샷 2022-04-29 오후 5.11.58.png](https://i.imgur.com/UHPEbkQ.png)

비밀번호를 얻기 위해 터미널에 다음 명령어를 입력해 주었습니다.

``` bash
$ sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

(만약 해당 디렉토리를 찾을수 없다는 메시지가 출력된다면 다음을 명령어를 입력해 줍니다)

``` bash
$ sudo cat /home/jenkins/secrets/initialAdminPassword
```

미리 구성해둔 설정이 있는 게 아니어서 추천 플러그인을 설치하였습니다.

![스크린샷 2022-04-29 오후 5.17.51.png](https://i.imgur.com/Ep991KE.png)

관리자 계정을 만들어주고 URL을 설정해 주어 Jenkins를 사용할 준비를 마쳤습니다.

Jenkins가 운영 서버에 접근하기 위해서는 Publish Over SSH 패키지가 필요합니다.

왼편 Dashboard → Jenkins 관리 → 플러그인 관리로 이동하였습니다.

`설치 가능` 탭을 누르고 오른편의 검색 탭에 `ssh` 를 검색, `Publish Over SSH` 를 체크하여 `Download now and install after restart` 를 눌러 설치해 주었습니다.

![스크린샷 2022-04-29 오후 10.30.42.png](https://i.imgur.com/Vd2VY8k.png)

### Service 서버 컨테이너 준비

Dockerfile을 통해 서버 컨테이너를 생성해 주었습니다.

이때 Dockerfile은 어떠한 베이스 이미지를 기반으로 사용자에게 필요한 것들을 사전에 세팅해 새로운 이미지를 생성할 때 사용하는 명세서라고 볼 수 있습니다.

Dockerfile을 관리하기 위한 디렉터리와 파일을 생성하였습니다.

``` bash
$ cd

$ mkdir service_server

$ cd service_server

$ vim Dockerfile
```

다음과 같이 Dockerfile을 작성하였습니다.

```bash
FROM ubuntu:18.04 

# default user 
ENV USER serve 

# packages install 
RUN apt-get update && apt-get upgrade -y 
RUN apt-get install -y sudo vim net-tools ssh openssh-server openjdk-8-jdk-headless 

# Access Option 
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config 
RUN sed -i 's/UsePAM yes/#UserPAM yes/g' /etc/ssh/sshd_config 

#user add & set 
RUN groupadd -g 999 $USER 
RUN useradd -m -r -u 999 -g $USER $USER

RUN sed -ri '20a'$USER' ALL=(ALL) NOPASSWD:ALL' /etc/sudoers 

#set root & user passwd 
RUN echo 'root:root' | chpasswd 
RUN echo $USER':serve123' | chpasswd 

# java 환경변수 
ENV JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64 

ENTRYPOINT sudo service ssh restart && bash 

USER $USER
```

- Base OS: Ubuntu 18.04
- jdk, vim 설치
- Jenkins에서 사용할 사용자 계정 생성
- ssh 접속 키 관리를 위한 .ssh 디렉터리 생성
- 프로젝트 배포를 위한 JAVA 환경 변수 지정

사용자 계정을 serve, 패스워드를 serve123으로 설정하였습니다.

생성한 Dockerfile을 이용하여 이미지를 빌드해 주었습니다.

``` bash
$ docker build -t service-server .
```

빌드를 성공적으로 마치고 이미지 컨테이너에 적재하였습니다.

``` bash
$ docker run --name spring-server -itd -p 9000:9000 -p 9022:22 service-server:latest
```

- **-p 9000:9000** = 스프링 부트에게 할당할 포트
- **-p 9022:22** = 젠킨스에서 ssh로 접근하기 위한 포트

9000번, 9022번 포트가 개방된 서버 컨테이너에 올라갔는지 확인하였습니다.

``` bash
$ docker ps
```

![스크린샷 2022-04-29 오후 11.34.22.png](https://i.imgur.com/BoZwBmC.png)

## 5. Jenkins, Github - SSH 키 생성 및 등록

Jenkins에서 Github에 접근하여 프로젝트를 pull 해오기 위해서 SSH 키를 깃허브에 등록해 주었습니다.

``` bash
$ sudo mkdir /home/jenkins/.ssh

$ sudo chmod 700 /home/jenkins/.ssh

$ sudo ssh-keygen -t rsa

$ Enter file in which to save the key (/root/.ssh/id_rsa): /home/jenkins/.ssh/id_rsa
```

![스크린샷 2022-04-29 오후 11.56.36.png](https://i.imgur.com/e8pijkf.png)

발급받은 ssh key를 GitHub에 등록해 주기 위해서 아래 명령어를 사용하여 키 값을 확보하였습니다.

``` bash
$ sudo su

$ cat /home/jenkins/.ssh/id_rsa.pub
```

![스크린샷 2022-04-29 오후 11.57.58.png](https://i.imgur.com/uwWLXta.png)

Jenkins Public SSH Key를 확보하였습니다.

확보한 키를 GitHub에 등록해 주었습니다.

Settings → SSH and GPG keys → New SSH key 

![스크린샷 2022-04-30 오전 12.00.23.png](https://i.imgur.com/RtpasWO.png)

### spring-server에 Jenkins의 Public SSH Key 등록

Jenkins에서 빌드한 프로젝트를 `spring-server` 에 배포하기 위해서 jenkins의 Public SSH Key를 등록해 주었습니다.

spring-server 컨테이너 터미널에 접근하여 키 값을 ~/.ssh/authorized_keys 경로에 등록해 주었습니다.

``` bash
$ docker exec -it spring-server /bin/bash

// Jenkins의 Public SSH Key 입력
$ sudo vim ~/.ssh/authorized_keys
```

그다음 Jenkins 대시보드 → Jenkins 관리 → 시스템 설정으로 이동해 주었습니다.

맨 아래의 Publish over SSH 항목으로 이동합니다.

만약 Publish over SSH 항목이 존재하지 않는다면 Jenkins를 종료했다가 재시작 해줍니다

```bash
$ sudo systemctl stop jenkins
$ sudo systemctl start jenkins
```

> ❗️ jenkinsurl/restart 를 통해서 젠킨스를 재시작 하게 되면 restarting창이 계속 띄워져 있는 오류가 생겼습니다.

창을 닫고 `sudo netstat -antp` 결과 할당되어있던 젠킨스의 8080 포트가 할당되지 않은것을 발견하였습니다.

![스크린샷 2022-04-30 오후 7.56.23.png](https://i.imgur.com/6EQxyF1.png)

다음 과정을 통해서 문제를 해결하였습니다.

```bash
$ docker run --name jenkins-docker -d -p 8080:8080 -p 50000:50000 -v /home/jenkins:/var/jenkins_home -u root jenkins/jenkins:lts
```

만약 다음과 같은 애러 메시지가 출력된다면

`docker: Error response from daemon: Conflict. The container name "/jenkins-docker" is already in use by container "13f8979d8f97c246f1136676d7e2895486073f304c2e20d6e08b8ae3a55d391e". You have to remove (or rename) that container to be able to reuse that name.`

다음 명령어로 충돌이 발생하는 컨테이너를 삭제하고 다시 시도해 줍니다.

```bash
$ docker rm /jenkins-docker

$ docker run --name jenkins-docker -d -p 8080:8080 -p 50000:50000 -v /home/jenkins:/var/jenkins_home -u root jenkins/jenkins:lts
```

![스크린샷 2022-04-30 오후 7.58.49.png](https://i.imgur.com/3v9FzlF.png)

Jenkins의 8080 포트가 다시 할당된 것을 확인할 수 있었습니다.

다시 Jenkins의 시스템 설정에 들어가서 Publish over SSH를 등록해 주었습니다.

![스크린샷 2022-04-30 오후 10.27.24.png](https://i.imgur.com/n1bpOB5.png)

![스크린샷 2022-04-30 오후 8.04.43.png](https://i.imgur.com/iQiLZX9.png)

- Name: Jenkins에서 식별할 서버 이름 (사용자 지정 이름)
- Hostname: 운영 서버의 ip 주소
- Username: 운영 서버의 사용자 계정을 넣습니다.
- Remote Directory: 사용자 루트
- Passphrase / Password: 운영 서버의 비밀번호를 입력
- Port: 운영 서버의 ssh 포트인 22를 입력

위 항목들을 입력하고 Test Configuration을 사용하여 접속 상태를 체크해 주었습니다.

## 6. Github 저장소 추가

![스크린샷 2022-04-30 오후 11.07.45.png](https://i.imgur.com/lUbDfhO.png)

프로젝트 레포지토리의 Code를 눌러 저장소의 주소를 SSH 형식으로 확보해 주었습니다.

### Jenkins에 프로젝트 등록

Jenkins 대시보드 → 새로운 Item

프로젝트 이름을 promisor-jenkins로 하고 Freestyle project를 클릭한 뒤 하단의 OK 버튼을 눌러주었습니다.

![스크린샷 2022-04-30 오후 10.35.04.png](https://i.imgur.com/Tti7es2.png)

![스크린샷 2022-04-30 오후 10.35.20.png](https://i.imgur.com/wWlbSbI.png)

![스크린샷 2022-04-30 오후 10.37.29.png](https://i.imgur.com/iUqYMRG.png)

그 다음 ssh 연동을 하였기 때문에 옆의 “고급" 버튼을 눌러서 Name, Refspec, 브랜치 관련 정보를 추가해 주었습니다. 

![스크린샷 2022-04-30 오후 10.40.57.png](https://i.imgur.com/0AI6k2m.png)

**참고자료 📚**

[https://goddaehee.tistory.com/259](https://goddaehee.tistory.com/259)

[https://dev-overload.tistory.com/40](https://dev-overload.tistory.com/40)