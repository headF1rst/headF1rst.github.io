---
toc : true
title : "[Network] Network Layer"
category : 
    - 컴퓨터네트워크
---
### 패킷망의 과제
응용 프로세스 간에 메시지를 교환하기 위해서는 패킷망을 통해 전달되어야 합니다. 전달 과정에서는 패킷의 목적지는 어떻게 확인 할 것인지, 경로를 어떻게 찾아갈 것인지, 체증이 생기면 어떻게 처리할 것인지 등 여러 문제를 해결해야만 송신측의 패킷이 수신측으로 잘 전달될 수 있습니다. 

이러한 수많은 문제들을 해결하기 위해 Divide and conquer 방법을 사용하여 목적지 까지 메시지를 전달하기 위해 해야할 모든 일들을 `계층`으로 구분합니다.

각 계층은 상호 독립적으로 각기 맡은 기능을 수행하며 상위 계층과 하위 계층 사이에는 정해진 정보 만을 전달합니다. 따라서 오류가 발생하였을 시에도 어느 계층에서 문제가 발생했는지 파악하기 쉽다는 장점이 있습니다.

### Encapsulation & Decapsulation

![Encapsulation & Decapsulation](/assets/images/ComputerNetwork/en&decapsulation.png)

- Encapsulation : 데이터를 전송할 때 각각의 레이어마다 정보를 담고 있는 헤더를 붙이는 과정
- Decapsulation : 수신된 데이터가 각각의 레이어를 따라 올라가면서 헤더가 벗겨지는 과정

### OSI 7 Layer

국제표준화기구 ISO에서 만든 네트워크 통신의 7단계 과정을 OSI 7 계층이라고 합니다. 각각의 7계층에 대해서 Top-Down 방식으로 알아보겠습니다.

![OSI 7 Layer](/assets/images/ComputerNetwork/OSI_Model.png)

### 7. 응용 계층$($Application)

최상위 계층인 응용계층은 사용자 또는 어플리케이션이 네트워크에 접근할 수 있도록 해줍니다.

HTTP, FTP, SMTP, POP3, IMAP, Telnet 등과 같은 프로토콜이 존재하며 통신 패킷들은 이와 같은 프로토콜에 의해 모두 처리됩니다. 우리가 사용하는 브라우저나, 메일 프로그램은 프로토콜을 보다 쉽게 사용하게 해주는 `응용프로그램`일 뿐입니다. 즉 **모든 통신의 양 끝단은 HTTP와 같은 프로토콜**이지 응용프로그램이 아닙니다.

사용자를 위한 인터페이스를 지원하며 메일 전송, 인터넷 접속등의 작업을 수행할 수 있습니다.

>PDU$($Process Data Unit) : 데이터 전송 단위

PDU = Data

### 6. 표현 계층$($Presentation)

표현계층에서는 응용계층으로 부터 전달 받은 데이터의 디코딩 혹은 응용계층으로 전송하는 데이터의 인코딩이 이루어 지는 계층입니다.
MIME 인코딩 또는 암호화 동작이 이루어 지게 됩니다.

응용계층에서 Data를 이해할 수 있도록 응용프로그램에 맞춰 변환하게 됩니다. 
EBCDIC로 인코딩된 문서 파일을 ASCII로 인코딩된 파일로 바꿔 주는 것, 해당 데이터가 택스트인지 GIF인지 JPG인지 구분하는 작업 등이 표현 계층의 역할입니다.

PDU = Data

### 5. 세션 계층$($Session)

**데이터가 통신하기 위한 논리적 연결**을 말합니다. 통신을 하기위한 대문이라고 볼 수 있겠습니다. 연결을 맺고 종료할 수 있는 비슷한 기능의 전송 계층$($4계층)과 무관하게 응용 프로그램 관점에서 봐야합니다. 세션 설정, 유지, 종료, 전송 중단시 복구 등의 기능이 있습니다.

세션 계층은 양 끝단의 응용 프로세스가 통신을 관리하기 위한 방법을 제공합니다. **동시 송수신 방식$($duplex), 반이중 방식$($half-duplex), 전이중 방식$($Full duplex)**의 동신과 함께, 체크 포인팅과 유휴, 종료, 다시 시작 과정 등을 수행합니다. 또한 TCP/IP 세션을 생성 및 삭제하는 책임을 집니다.

### 4. 전송 계층$($Transport Layer)

전송 계층에서는 데이터를 전송하고 전송 속도를 조절하며 오류가 발생된 부분은 다시 맞춰주는 계층입니다. 보통 TCP 프로토콜을 주로 사용하며 데이터를 전송받은 경우, 전송 계층에서 데이터를 합산하여 세션 계층으로 보내주게 됩니다. $($흐름제어, 시퀀스 넘버 기반 오류 제어, 중복검사 등을 수행)

헤더에는 송, 수신지 포트번호를 포함하여 전달하고 데이터 전송 단위는 TCP의 경우 Segment, UDP의 경우 Datagram이 됩니다.

전송 계층은 종단간$($End to end)의 사용자들이 신뢰성 있는 데이터를 주고받게 해주어 상위 계층들이 데이터 전달의 유효성이나 효율성을 생각하지 않도록 해준다.

- **TCP 프로토콜 $($Transmission Control Protocol)**
    
    - 프로세스간 통신의 신뢰성이 보장된다.
    - flow control을 통해서 송신측이 수신측에 부하를 주지 않는다.
    - 네트워크 과부화 상태면 혼잡제어를 통해 수신측을 멈춘다.
    - server와 client간에 사전 handshake가 필요하다.

패킷 손실과 중복, 순서바뀜이 없도록 신뢰성이 보장됩니다.

- **UDP 프로토콜 $($User Datagram Protocol)**

    - 프로세스간 데이터 통신의 신뢰성이 보장되지 않는다.
    - flow control, 혼잡제어, handshake 등을 제공하지 않는다.
    - 데이터 전송 속도가 빠르다.
    - 헤더가 단순하여 헤더처리가 빠름

메세지가 제대로 수신되었는지 확인하는 절차가 없으며 수신 메세지의 순서가 보장되지 않습니다. 하지만 $($1:多) 전송이 가능하며 빠른 요청과 응답이 필요한 실시간 응용에 적합하다는 장점이 존재합니다.

데이터 전송 신뢰성과 전송 시간은 서로 trade off 관계 입니다. ex) email vs call

### 3. 네트워크 계층$($Network)

네트워크 계층은 `라우팅`, `혼잡제어`, `Internetworking` 의 기능을 담당하는 계층입니다.

먼저 `라우팅 (routing)`이란 어떤 네트워크 상에서 통신 데이터를 보낼 경로를 선택하는 과정을 얘기합니다.

네트워크 계층은 적합한 라우팅 알고리즘을 통해 여러개의 노드를 거칠때 마다 경로를 선택하고 주소를 정하여 해당 경로에 따라 패킷을 전달해 주는 역할을 합니다. 

`혼잡`이란 서브넷에 너무 많은 패킷이 들어와서 수행능력이 감소하는것을 의미합니다. 트래픽이 너무 많이 증가하게 되면 라우터가 더 이상 이를 대체할 수 없게 되며 패킷손실이 발생하기 시작합니다.

이러한 혼잡을 제어하기 위한 기능을 `혼잡제어`라고 합니다. 

전달 과정에서 전송계층이 요구하는 서비스 품질$($Qos)를 제공하기 위한 기능적, 절차적 수단을 제공합니다.

`Internetworking`이란 네트워크 간의 통신하는 개념 혹은 기술을 말합니다. 즉, 독립적으로 움직이는 개개의 네트워크간의 접속을 의미합니다.

### 2. 데이터 링크계층 $($DataLink Layer)

데이터 링크 계층에서는 4가지 주요 기능을 담당하게 됩니다.

- **Service to neighbour layer**
    데이터 링크계층과 이웃해있는 물리계층과 네트워크 계층에게 잘 정의된 서비스 인터페이스를 제공해줍니다. 
    <br><br>
    보통 확인되지 않은 비연결형 서비스 $($Unacknowledged connectionless serviece), 확인된 비연결형 서비스 $($Acknowledged connectionless service), 확인된 연결형기반 서비스 $($Acknowledged connection-oriented service), 이 세가지 서비스를 제공합니다.
    <br><br>

- **Framing**
    데이터 링크계층은 물리계층으로 부터 비트 스트림 $($bit stream)을 제공받아 네트워크 계층에 서비스를 제공하게 됩니다. 하지만 이 비트 스트림은 수신된 비트의 갯수가 송신된 비트의 수와 다른 오류가 생길수 있으며 이러한 오류를 찾아내고 수정하기 위해서 데이터 링크는 비트 스트림을 분리된 프레임으로 나누어 그룹화 하는 `Framing`을 수행합니다.
    <br><br>
    그룹화된 프레임에 대해서 checksum을 계산하고 프래임이 목적지에 도착하였을때 다시한번 checksum을 계산합니다. 
    <br><br>
    만약 사전의 checksum값과 도착후의 checksum값이 다르다면 데이터 링크계층은 오류를 감지하고 이를 수정하는 과정을 거치게 됩니다.
    <br><br>

- **흐름제어 $($Flow control)**
    데이터를 보내는 측과 받는측의 속도를 파악하고 이를 조절하는 것을 흐름제어라고 합니다.
    <br><br>
    데이터를 보내는 측의 속도가 상대적으로 빠를 때, 전송에 오류가 없다고 하더라도 특정 시점에서 수신하는 측은 프레임이 도착하자마자 처리할 수 있는 능력이 되지 않아서 일부 프레임을 손실하게 됩니다.
    <br><br>
    따라서 이와같은 경우에서 데이터 링크계층은 데이터를 받는 측의 속도가 느릴때 데이터 전송측의 속도에 의해서 막히지 않도록 속도를 규제합니다.
    <br><br>

### 1. 물리계층 $($Physical Layer)

데이터를 전기적 신호로 변환해서 통신 케이블을 통해 주고 받으며 통신 단위는 비트$($0 또는 1) 입니다. 

물리계층에서는 데이터를 전달 할뿐 전송하거나 받으려는 데이터가 무엇이고 어떤 에러가 존재하는 지는 신경쓰지 않습니다.

이 계층에 속하는 대표적 장비들에는 통신 케이블, 리피터, 허브 등이 있으며 이러한 장비들을 통해 데이터를 전송하고 수신받습니다.

참고 자료
- https://shlee0882.tistory.com/110
- https://doorbw.tistory.com/51 