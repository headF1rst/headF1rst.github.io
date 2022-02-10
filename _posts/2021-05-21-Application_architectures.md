---
toc : true
title : "[Network] Application architectures"
category : 
    - CS
---

Application architecture에는 예전부터 사용해 오던 client server architecture와 pc와 n/w에 향상이 있는 peer-to-peer $($P2P) architecture, 그리고 두 모델의 Hybrid 형태가 존재합니다.

### Client/Server architecture란?

정보를 사용하고자 하는 `클라이언트`와 정보가 존재하며 이를 제공하는 곳을 `서버`라고 하며 이와 같이 클라이언트와 서버로 분리시킨 구조를 client/server architecture $($2-tier-architecture)라고 합니다.

![client&server](/assets/images/ComputerNetwork/client&server.png)

**Client**

클라이언트는 보통 플랫폼에 따라 구분됩니다.

- 웹 플랫폼 $($브라우저) : client == 웹사이트 / 웹 엡
<br><br>

- 스마트폰/데스크탑 플랫폼 $($iOS, 윈도우) : client == 앱

**Server**

서버는 역할에 따라 구분됩니다.

- 파일 서버 : 파일을 제공하는 앱
    <br><br>
- 웹 서버 : 웹사이트에서 필요로 하는 정보를 제공하는 앱
    <br><br>
- 메일 서버 : 메일을 주고받을수 있도록 도와주는 앱

최근에는 2-tier-architecture에서 `Database`가 추가된 3-tier-architecture가 등장했고 이때 서버는 정보를 전달하고 데이터베이스는 정보의 저장을 맡게 됩니다. 

### Client와 Server 간의 HTTP 프로토콜

원하는 정보를 client가 얻기위해서는 server로 부터 원하는 정보를 요청하고 받아야 합니다. 이를 위해 client와 server는 `HTTP 프로토콜`을 사용하여 연결을 설정합니다.

연결이 완료되었다면 client는 서로가 이해할 수 있는 XML 혹은 JSON 형식으로 서버에 request를 보냅니다.

요청을 받은 server는 request를 이해하고 그에 맞는 적절한 데이터를 response 하게 됩니다.

---

날씨를 예로 들어 봅시다. 만약 서울의 날씨 정보를 알고싶다면 client는 server에 request를 보낼때 "서울" 이라는 장소에 대한 세부 정보를 같이 지정하여 보내줘야 합니다.

server는 request를 해석하여 "서울"의 날씨에 맞는 데이터를 다시 response로 보내줍니다.

![client&server](/assets/images/ComputerNetwork/http_communication.png)

### HTTP 프로토콜

HTTP 프로토콜에 대해 알아보기 전에 웹페이지에 존재하는 `object`이란 단어에 대한 의미를 먼저 알아보겠습니다.

웹페이지의 base HTML-file은 여러 objects들을 포함하고 있습니다. object는 HTML file, JPEG image, audio file 등이 될 수 있습니다.

이러한 object는 URL을 통해 접근 가능합니다. client는 object을 받기위해 원하는 object에 대한 정보를 담고있는 URL을 포함한 HTTP 프로토콜을 사용하여 서버로 부터 object를 요청합니다.

![URL](/assets/images/ComputerNetwork/URL.jpeg)


---

HTTP$($Hypertext Transfer Protocol)는 인터넷상에서 데이터를 주고받기 위한 대표적인 client/server 모델 기반 프로토콜입니다.

Application level 프로토콜이며 TCP/IP위에서 작동합니다.
링크 기반으로 데이터에 접속하며 HTML 문서, 이미지, 동영상, 오디오 등 여러 종류의 데이터를 전송할 수 있습니다.

HTTP에는 연결 유지의 유무에 따라 두가지 버전이 존재합니다. 

버전 1.0은 비지속 HTTP connection, 버전 1.1은 지속 HTTP connection을 의미합니다.

**Non-persistent HTTP $($비지속)**

비지속 HTTP는 말 그대로 서버에 연결하고, 요청한후 응답을 받으면 연결을 끊어버리는 방식을 말합니다.

사용자가 `www.someSchool.edu/someDepartment/home.index`라는 URL에 접속하고자 한다고 가정해 봅시다. + 서버의 포트 번호는 80입니다.

![non-persistent-HTTP](/assets/images/ComputerNetwork/non-persistent-HTTP.png)

위와 같은 과정은 자원 하나당 하나의 연결이 만들어 지면서 반복됩니다. 이러한 비지속 HTTP는 아래와 같은 장단점을 갖습니다.

**Pro)**

- 불특정 다수를 대상으로 하는 서비스에 적합하다.
- 많은 인원이 서비스를 사용하더라도 접속 유지는 최소한으로 유지하므로 더 많은 사용자의 요청을 처리할수 있다.

**Cons)**

- 한번에 하나의 object 밖에 TCP 연결을 통해 전송하지 못한다.
- 하나의 object당 2 RTT가 요구된다.
- 매 TCP 연결마다 OS에 overhead 발생

> RTT $($Return Transfer Time) : 하나의 패킷이 client에서 server로 갔다가 다시 client까지 돌아오는데 걸리는 시간을 의미합니다.

<br><br>

**Persistent HTTP $($지속)**

지속 HTTP에서 server는 response를 보내고 나서도 client와의 연결을 유지합니다. 따라서 단일 TCP연결을 통해 여러 objects들을 전송할 수 있게 됩니다.

또한 object에 대한 GET 메세지는 병렬로 요청하게 됩니다.

### HTTP request message

General format

![HTTP request format](/assets/images/ComputerNetwork/http_request_format.png)

**1) Method**

메서드는 요청의 종류를 서버에 알려주기 위해서 사용됩니다.

- GET : 정보를 요청하기 위해서 사용 $($SELECT)
- POST : 정보를 삽입하기 위해서 사용 $($INSERT)
- PUT : 정보를 업데이트하기 위해서 사용 $($UPDATE)
- DELETE : 정보를 삭제하기 위해서 사용 $($DELETE)

- OPTIONS : 응답 가능한 HTTP method 요청
- HEAD : HTTP Header 정보만 수신
- TRACE : Request의 loop back 테스트
- CONNECT : 터널링의 목적으로 연결요청

사실 GET, POST 만으로 다른 메소드를 사용할 필요없이 모든 요청을 처리할 수 있습니다.

**+ POST method VS GET method**

POST 와 GET 메소드는 HTTP 프로토콜에서 데이터를 client에서 server로 전송한다는 공통점이 존재하지만 분명한 차이점 또한 존재하며 상황에 알맞는 메소드를 사용하는것이 중요합니다.

[GET vs POST 글로 이동합니다](https://headf1rst.github.io/cs/GETvsPOST/)

**2) URL**

요청하는 자원의 위치를 표시합니다.

**3) Version**

웹 브라우저가 사용하는 프로토콜 버전을 의미합니다.

1.0 : non-persistent HTTP connection
1.1 : persistent HTTP connection

### HTTP response message

![non-persistent-HTTP](/assets/images/ComputerNetwork/HTTPresponse_message.pbm)

- 프로토콜과 응답코드 : $($HTTP/1.1 200 OK)
- 날짜 : $($Sun, 28 Aug 2017 08:56:53 GMT)
- 서버 프로그램 및 스크립트 정보 : $($Apache/2.4.27 (Linux))
    응답헤더에는 다양한 정보를 추가할 수 있습니다.
- 컨텐츠 마지막 수정일
- 캐시 제어 방식
- 컨텐츠 길이 
- Keep-Alive 기능 설정

|     코드번호    |     설명        |       비고        |
|----------------|---------------|------------------|
|200|성공|서버가 요청을 제대로 처리했다.|
|301|영구적으로 이동했다|요청한 리소스가 Location (en-US) 헤더에 주어진 URL로 완전히 옮겨졌다는 것을 나타낸다. |
|400|잘못된 요청|서버가 요청한 메세지를 해석하지 못했다.|
|403|금지|서버가 요청을 거부하고 있다.|
|404|찾을 수 없음|요청한 자원이 서버에 존재하지 않는다.|
|505|지원하지 않는 HTTP 버전| 해당 버전의 HTTP를 지원하지 않는다|


