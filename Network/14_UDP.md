# UDP 프로토콜

### 1) UDP 프로토콜이란

  UDP 프로토콜은 데이터를 처리하는 프로토콜입니다. UDP는 비연결형 서비스이므로, 연결을 설정하고 해제하는 과정이 존재하지 않습니다. 서로 다른 경로로 독립적으로 처리함에도 패킷에 순서를 부여하여 재조립을 하거나 흐름 제어 또는 혼잡 제어와 같은 기능도 처리하지 않기에 TCP보다 속도가 빠르며 네트워크 부하가 적다는 장점이 있지만 신뢰성있는 데이터의 전송을 보장하지는 못합니다. 그렇기 때문에 신뢰성보다는 연속성이 중요한 서비스 예를 들면 실시간 서비스(streaming)에 자주 사용됩니다.

- 비연결형 프로토콜(Connectionless Protocol)
  - 논리적인 연결 설정 과정이 없기 때문에 데이터그램 전송 시 마다 주소 정보를 설정해서 전송한다.
  - 데이터의 순차적 전송을 보장해주지 않는다.
  - **데이터그램 기반의 전송방식**을 사용한다. 즉 데이터를 정해진 크기로 전송하는 방식을 사용한다.
-  신뢰할 수 없는 프로토콜(Unreliable Protocol)
  - 흐름제어(Flow Control), 혼잡제어(Congestion Control) 등을 수행하지 않는다.
  - 실질적으로 IP 기반에 포트 정보를 이용하여 **상위 송수신 어플리케이션을 식별**해주는 역할 정도만 수행한다
  - CheckSum 헤더필드를 이용해 최소한의 오류만 검증한다.
- 단순하고 가벼운 프로토콜로 전송 속도 빠르다.
  - 따라서, 한 번의 패킷 송수신으로 완료되는 서비스에서 많이 사용된다.
  - Streaming 서비스, DNS, NTP, DHCP

> 흐름제어, 혼잡제어
>
> 흐름제어는 데이터를 송신하는 곳과 수신하는 곳의 데이터 처리 속도를 조절하여 수신자의 버퍼 오버플로우를 방지하는 것입니다.
>
> 혼잡제어는 네트워크 내의 패킷 수가 넘치게 증가하지 않도록 방지하는 것입니다.  네트워크 내의 패킷이 많다면 조금만 전송하면 방지합니다.

![다운로드](https://user-images.githubusercontent.com/59816811/136507072-6e67c883-db89-492e-ba6e-d0d590727154.png)

- Source Port (16 bits) : 출발지(송신) 포트 번호
- Destination Port (16 bits) : 목적지(수신) 포트 번호
- Total Length (16 bits) : 헤더와 데이터부를 포함한 전체 길이
- Checksum (16 bits) : 전체 데이터그램에 대한 오류를 검사하기 위한 필드

