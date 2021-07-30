---
toc : true
title : "[Node.js] Node.js와 Express에 대한 기본 설명"
category : 
    - Back_End
---
### Node.js 란?
---
- 구글의 크롬 V8 자바스크립트 엔진을 기반으로 하는 비동기 I/O를 지원하는 서버입니다.

- 자바스크립트를 프로그래밍 언어로 사용합니다.

- 싱글 쓰레드에서 CPU, 메모리 오버헤드를 적게 가져가면서 많은 동시 접속을 처리할 수 있습니다.

### 장점
---
- **빠른 성능의 서버**
<br>비동기 I/O이기 때문에 쓰레드가 파일을 읽어오는 것을 기다리지 않아 성능이 빠릅니다.

- **Evnet handler**
<br>하나의 쓰레드가 큐에 대기중인 request를 하나씩 가져와서 처리합니다. 마우스를 누르는 것과 같은 동작이 이뤄질 때 마다 `event handler`가 작업을 처리하여 성능이 향상됩니다.

- **한가지 언어**
<br>자바스크립트로 서버, 클라이언트 모두 개발 가능합니다.

- **광범위한 커뮤니티**
<br>많은 커뮤니티가 활성화 되어 있습니다.

- **효율적인 생산성**
<br>Java로 Rest API + DB를 사용하여 백엔드를 개발하는 경우보다 Node.js로 개발할때 더 빠르게 개발할 수 있습니다. $($단, 프로젝트의 시나리오에 따라 달라질 수 있습니다.)

- **스트림**
<br>큰 크기의 파일들을 stream 하는데 적합합니다.

### 단점
---
- **Call back**
<br>반복적으로 call back을 사용하게 되면서 nested call back들을 만들어 냅니다.

- **싱글 쓰레드이기 때문에 처리가 오래걸리는 단일 작업에는 성능 저하.**
<br>따라서 Node는 CPU를 많이 사용하지 않는 작업에 적합하다.

### Node.js를 사용하기 적합한 경우
---
<br>Node.js의 특성상 짧은 시간에 대량의 요청을 빠르게 처리하는 웹 어플리케이션 개발에는 적합하나, 하나의 작업 시간이 오래 소요되는, 즉 CPU의 사용량이 높은 어플리케이션에는 성능이 저하 될 수 있다고 볼 수 있습니다.

따라서 다음과 같은 어플리케이션 개발에 사용하는 것이 적합합니다.

- Chat/Messaging
- Real-time Applications
- High Concurrency Applications
- Communication Hubs
- Data Streaming
- Stock exchange software

### Node.js 프레임워크
---
Node.js를 위한 프레임워크에는 AdonisJS, Egg.js등 다양한 프레임워크가 존재하지만 여기서는 대표적인 Express.js에 대해 알아보겠습니다.

- **Express.js**
<br>현재까지 가장 많이 사용되는 대표적인 Node 프레임워크입니다. 미들웨어 구조를 사용하기 때문에 웹 서버를 구축하기 쉽고 강력한 성능을 자랑합니다. 또한 다양한 기능의 미들웨어 중 개발자는 필요로 하는 미들웨어만을 선택하여 express와 결합하여 사용할 수 있습니다.

>Reference <br>
https://www.voidcanvas.com/describing-node-js/ <br>
https://ithub.tistory.com/32