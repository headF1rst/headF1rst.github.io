---
toc : true
title : "[Network] TCP 흐름제어"
category : 
    - CS
---
TCP란 기본적으로 신뢰성 있는 네트워크를 보장할 수 있도록 하는 프로토콜을 말합니다. 신뢰성있는 네트워크를 보장하기 위해서는 아래의 조건들을 모두 보장해야 합니다.

1. 손실 - 패킷이 손실될 수 있는 문제
2. 순서바뀜 - 패킷의 도착 순서가 바뀌는 문제
3. Congestion - 네트워크가 혼잡한 문제
4. Overload - 수신측이 overload되는 문제

### 흐름제어

송신측의 패킷 전송 속도가 수신측의 패킷 처리 속도보다 빠를 경우, 수신측의 버퍼에 overflow가 발생하여 패킷이 손실되는 문제가 발생합니다. 패킷이 손실되면서 불필요한 응답과 데이터 전송이 송/수신 측 간에 반복되게 됩니다.

따라서 이러한 문제를 방지하기 위해 송신 측의 패킷 전송량을 수신측에 따라 조절해야 하며 이를 `흐름제어`라고 합니다.

### 흐름제어 방식

- **Stop & Wait**

매번 전송한 패킷에 대한 확인 응답을 받아야지만 그 다음 패킷을 전송하는 방법.

![Stop & Wait](/assets/images/ComputerNetwork/Stop&Wait.jpeg)

단순하지만 패킷을 보내고 기다리고를 반복하게 되므로 너무 느리다는 단점이 존재합니다. 실제로 현재 Stop and wait 방식을 사용하는 네트워크는 없습니다.

- **Sliding Window**

수신측에서 설정한 window 크기만큼 송신측에서 확인 응답에 상관없이 패킷을 한번에 전송하여 데이터 흐름을 동적으로 조절하는 제어방식을 말합니다.

window의 크기만큼의 패킷전송이 완료되었다는 ACK가 발생하면 window를 옆으로 옮김$($Slide)으로써 다음 패킷들을 전송합니다.

> Window란? : 
TCP/IP를 사용하는 모든 호스트들은 송/수신을 위한 2개의 window를 갖고 있습니다.<br>
호스트들은 데이터를 보내기 전에 3-way-handshake를 통해 수신측 window size에 송신측 window size를 맞추게 됩니다.

![Sliding-window](/assets/images/ComputerNetwork/Sliding-window.png)

위 그림을 예로들자면 송/수신 측의 window 크기는 7이므로 한번에 최대 7개의 패킷을 동시에 전송할 수 있음을 의미합니다.

송신측에서 Data 0, 1을 보내고 윈도우가 두칸 줄어든 것을 볼 수 있습니다. 수신측에서는 0,1 데이터를 잘 받았다는 ACK2를 한번 보내주고, window 크기가 두칸 왼쪽으로 늘어난 것을 확인할 수 있습니다. 이런식으로 window가 옆으로 미끄러 진다 해서 `Sliding window`라고 부릅니다.

- **Rate Control**

ACK를 사용하는 이상 송/수신 측간의 대기 시간은 항상 존재하게 됩니다. 따라서 고안된 방법이 ACK를 아예 사용하지 않는 rate-base 방법입니다.

송신측에 할당된 패킷 전송률을 주고, 패킷이 전송률을 초과해서 전송되지 않도록 보장하는 방식입니다.

![Rate-control](/assets/images/ComputerNetwork/Rate-base.png)

위의 그림과 같이 Avg data rate, Maximum burst size, Peak등을 설정하여 ACK없이도 흐름제어를 가능케 하는 방법으로 이러한 설정값을 넘어가는지 감시하는 기술을 `Policing` 이라고 합니다. 