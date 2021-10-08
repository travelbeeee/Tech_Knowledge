# TCP프로토콜

### 1) TCP 프로토콜이란

 TCP 프로토콜은 대용량의 데이터를 보내기 쉽게 작게 분해하여 상대에게 보내고, 정확하게 도착했는지 확인하는 프로토콜입니다. IP프로토콜의 한계를 다 해결해주는 프로토콜로 출발지 PORT, 목적지 PORT, 전송 제어, 순서, 검증 정보 등의 내용을 포함하고 있음.

- TCP 프로토콜은 연결 지향 프로토콜입니다.
- TCP 프로토콜에서는 데이터 송수신을 위해 클라이언트와 서버의 소켓이 연결되어 있어야 하며, 데이터가 유실되면 데이터 재전송을 요청함으로써 신뢰성을 보장합니다.

TCP 프로토콜의 세부 구조는 아래와 같습니다.

![1](https://user-images.githubusercontent.com/59816811/135642263-f9a6215f-dccd-4870-bca8-22734f415238.png)

- Source Port(16 bits) : 출발지(송신) 포트 번호

- Destination Port(16 bits) : 목적지(수신) 포트 번호

- Sequence Number(32 bits) : 송신 데이터 순서 번호

  - 송신 시 전송하는 데이터의 시작 바이트 순번을 담는다. 바이트 순번은 전송하는 데이터의 바이트 단위로 부여하는 연속된 번호를 의미한다.
  - 연결설정 단계에서 초기 순서 번호를 상호간에 주고받는다, 초기 순서 번호는 0부터 시작하는 것이 아니라 임의의 수를 할당해서 사용한다.

- Acknowledgment Number(32 bits) : 상대방이 다음에 전송할 순서 번호

  - 수신 확인 응답(ACK)과 함께 해당 필드에 상대방이 다음에 전송할 순서 번호를 담아서 보낸다.
- HLEN(4 bits) : 헤더 길이

  - 4 bits 워드 단위로 표시(20 ~ 60 bytes)하며 기본 헤더 20 bytes와 옵션 헤더 최대 40 bytes로 구성된다.
- Reserved(4 bits) : 예약
- Control Flags(6 bits)
  - URG(Urgent pointer is valid) : 긴급 데이터 설정
  - ACK(Acknowledgment is valid) : 수신 확인 응답(ACK) 설정
  - PSH(Request for push) : 송수신 버퍼에 있는 데이터를 즉시 처리
  - RST(Reset the connection) : 연결 중단(강제 종료)
  - SYN(Synchronize sequence numbers) : 연결 설정
  - FIN(Terminate the connection) : 연결 종료 (정상 종료)
- Window size(16 bits) 
  - 수신측에서 송신측에 보내는 Receiver window size로 수신버퍼의 여유공간 크기를 의미한다. 송신측에서는 상대방의 여유 공간 크기를 통해서 흐름제어를 수행할 수 있다.따라서 송신측에서는 상대방의 윈도우 사이즈 범위 내에서 수신측의 수신 확인 응답(ACK)을 기다리지 않고 연속적으로 전송할 수 있는데 이를 슬라이딩 윈도우 제어방식이라 한다.
- Checksum(16 bits) : 헤더를 포함한 전체 세그먼트에 대한 오류를 검사하기 위한 필드
- Urgent Pointer(16 bits) : 세그먼트가 긴급 데이터(URG 플래그 설정)를 포함하고 있는 경우에 사용되는 필드로 긴급 데이터의 위치값을 담고 있다.

<br>

### 2) 3-way-handshake

TCP 프로토콜은 연결 성립을 위해 3-way-handshake 방식을 사용합니다. 

![tcpprotocol1](https://user-images.githubusercontent.com/59816811/114971611-a8047400-9eb7-11eb-9f61-8101fc504fd7.png)

​	1) 연결을 하기 전 클라이언트는 `Closed` 상태이고, 서버는 `Listen` 상태입니다.

​	2) 클라이언트는 서버에 접속을 요청하는 SYN 패킷을 보냅니다.

​	3) 서버는 클라이언트의 요청인 SYN 패킷을 받고, 클라이언트에게 요청을 수락한다는 ACK 패킷과 SYN 패킷을 보냅니다.

​		서버는 상대방의 응답을 기다리는 `SYN_RCV`  상태로 변경 됩니다.

​	4) 클라이언트는 서버로부터 ACK 패킷과 SYN 패킷을 받으면, 포트를 열고 이에 대한 확인으로 서버로 ACK 패킷을 보냅니다.

​		클라이언트는 `Established` 상태로 바꿉니다.  

​	5) ACK 패킷을 받은 서버 역시 `Establihsed` 상태로 변경되면서 클라이언트와 서버는 논리적으로 연결이 됩니다.

<br>

### 3) 4-way-handshake

TCP 프로토콜은 연결 해제를 위해 4-way-handshake 방식을 사용합니다. 

![tcpprotocol2](https://user-images.githubusercontent.com/59816811/114971873-3a0c7c80-9eb8-11eb-8d13-8dab66570768.png)

​	1) 서로 통신 상태이기 때문에 클라이언트와 서버 모두 `Establihsed` 상태입니다.

​	2) 클라이언트가 통신을 종료하자는 FIN 플래그를 보내고, 상태를 종료요청 후 서버의 FIN 플래그를 기다리고 있다는 의미로 `FIN_WAIT_1` 상태로 바꿉니다.

​	3) FIN 패킷을 받은 서버는 ACK 패킷을 전송하고 애플리케이션 소켓을 닫습니다.

​	이때, 자원을 정리하는데 시간이 소요되므로, 소켓을 닫지만 기다리고 있는 상태라는 의미에서 `CLOSE_WAIT` 상태로 바꿉니다.

​	클라이언트는 서버의 ACK 패킷은 받았고 FIN 을 아직 기다리고 있다는 의미로 `FIN_WAIT_2` 상태로 바꿉니다.

​	4) 서버는 데이터를 모두 보내고 통신이 끝났으면 연결이 종료되었다고 클라이언트에게 FIN 플래그를 전송합니다.

​	5) 클라이언트는 FIN 플래그를 확인했다는 ACK 패킷을 서버로 보내고 `TIME_WAIT` 상태로 바꾸면서 서버에세 ACK 패킷을 보냅니다.

​	`TIME_WAIT` 상태에서 일정 시간이 되면 `CLOSED` 상태가 됩니다.

​	6) 서버는 ACK 패킷을 받고 `CLOSED` 상태가 되면서 연결이 끊깁니다.

> SYN : Synchronize
>
> ACK : Acknowledgement
>
> FIN : Finish

<br>

