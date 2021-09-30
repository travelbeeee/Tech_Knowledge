# 번외_TCP프로토콜

TCP 프로토콜은 연결 성립을 위해 3-way-handshake 방식을 사용합니다. 또, 연결 해제를 위해서는 4-way-handshake 방식을 사용합니다. 

### 1) 3-way-handshake

![tcpprotocol1](https://user-images.githubusercontent.com/59816811/114971611-a8047400-9eb7-11eb-9f61-8101fc504fd7.png)

​	1-1) 클라이언트는 서버에 접속을 요청하는 SYN 패킷을 보냅니다.

​	1-2) 서버는 클라이언트의 요청인 SYN 패킷을 받고, 클라이언트에게 요청을 수락한다는 ACK 패킷과 SYN 패킷을 보냅니다.

​	1-3) 클라이언트는 서버의 수락 응답인 ACK 패킷과 서버가 보낸 SYN 패킷을 받고 SYN 패킷을 잘 받았다고 ACK 패킷을 서버로 보냅니다.

<br>

### 2) 4-way-handshake

![tcpprotocol2](https://user-images.githubusercontent.com/59816811/114971873-3a0c7c80-9eb8-11eb-8d13-8dab66570768.png)

​	2-1) 클라이언트가 TCP Header의 flags 필드의 FIN 을 셋팅하여 연결을 종료하겠다는 FIN 플래그를 전송합니다.

​	2-2) FIN 플래그를 받은 서버는 ACK 패킷을 전송하고 서버를 CLOSE_WAIT 상태로 TimeOut을 겁니다.

​	2-3) 서버는 데이터를 모두 보내고 통신이 끝났으면 연결이 종료되었다고 클라이언트에게 FIN 플래그를 전송합니다.

​	2-4) 클라이언트는 FIN 플래그를 확인했다는 ACK 패킷을 서버로 보내고 서버는 ACK 패킷을 받고 연결을 close 합니다.

> TCP Header에는 Code Bit(flag bit) 라는 6Bit 짜리 Bit 가 존재하고, Urg-Ack-Psh-Rst-Syn-Fin 순서로 해당 패킷이 어떤 내용을 담고 있는 패킷인지를 나타내는 비트가 존재합니다.
>
> SYN : Synchronize Sequence Number
>
> ACK : ACKnowledgement

<br>

