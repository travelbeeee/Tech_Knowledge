# HTTPNetwork_HTTPS

### 1) HTTP의 약점

 HTTP는 다음과 같은 약점이 있습니다.

- **평문 통신이기 때문에 도청 가능**

   HTTP를 사용한 리퀘스트나 리스폰스 통신 내용은 HTTP 자신을 암호화하는 기능은 없기 때문에 통신 전체가 암호화되지는 않습니다. 또, TCP/IP 구조의 통신 내용은 전부 통신 경로의 도중에 엿볼 수 있습니다

  - 통신 암호화

    HTTP에는 암호화 구조는 없지만 SSL이나 TLS 이라는 다른 프로토콜을 조합함으로써 HTTP의 통신 내용을 암호화 할 수 있습니다. SSL 등을 이용해 안전한 통신로를 확립하고 나서 그 통신로를 이용해 HTTP 통신을 하는 것을 **HTTPS** 라고 합니다.

  - 콘텐츠 암호화

    HTTP 메시지에 포함되는 콘텐츠만 암호화해서 사용하는 방법입니다. 콘텐츠의 암호화를 유효하게 하기 위해서는 클라이언트와 서버가 콘텐츠의 암호화나 복호화 구조를 가지고 있어야되므로 평상시에 유저가 사용하는 브라우저와 웹 서버에서는 이용하는 것이 어렵다.

- **통신 상대를 확인하지 않기 때문에 위장 가능**

   HTTP를 사용한 리퀘스트나 리스폰스에서는 통신 상대를 확인하지 않습니다. 또, 누구이던 간에 리퀘스트를 보내면 리스폰스가 반환되는 매우 심플한 구조를 가지고 있습니다. 따라서, 다음과 같은 문제가 있습니다.

  - 리퀘스트를 보낸 곳의 웹 서버가 원래 의도한 리스폰스를 보내야 하는 웹 서버인지 아닌지를 확인할 수 없다.
  - 리스폰스를 반환한 곳의 클라이언트가 원래 의도한 리퀘스트를 보낸 클라이언트인지 아닌지를 확인할 수 없다.
  - 통신하고 있는 상대가 접근이 허가된 상대인지 아닌지를 확인할 수 없다.
  - 누가 리퀘스트를 했는지 확인할 수 없다.
  - 의미없는 리퀘스트라도 수신하게 되므로 Dos 공격을 방지할 수 없다.

- **완전성을 증명할 수 없기 때문에 변조 가능**

   완전성이란 정보의 정확성을 가리킵니다. 그것을 증명할 수 없다는 것은 정보가 정확한지 아닌지를 확인할 수 없음을 의미합니다. HTTP가 완전성을 증명할 수 없으므로 리퀘스트나 리스폰스가 발신된 후에 상대가 수신할 때까지의 사이에 변조되었다고 하더라도 이 사실을 알 수 없다는 뜻입니다.

<br>

### 2) HTTPS

위에서 언급한 HTTP의 문제점을 해결한 것이 HTTPS 입니다.  

`HTTP + 암호화 + 인증 + 완전성보호 = HTTPS` 

 HTTPS는 새로운 애플리케이션 계층의 프로토콜이 아니라 **HTTP 통신을 하는 소켓 부분을 SSL이나 TLS 프로토콜로 대체하고 있을 뿐**입니다. 보통 HTTP는 TCP 와 직접 통신하지만, HTTPS 를 이용하면 HTTP는 SSL과 SSL이 TCP와 통신하게 됩니다.

 HTTPS 는 공개키와 공통키 암호를 조합한 하이브리드 암호 시스템으로 암호화를 진행해줍니다. 키를 교환하는 곳에서는 공개키 암호를 사용하고 그 후의 통신에서 메시지를 교환하는 곳에서는 공통키 암호를 사용합니다.

 또, 공개키가 진짜인지 증명하기 위해서 인증 기관과 그 기관이 발행하는 공개키 증명서가 이용되고 있습니다.

<br>

### 3) HTTPS 인증

 HTTPS는 `SSL 인증서` 를 통해 서버를 검증합니다. `SSL 인증서` 는 신뢰할 수 있는 인증 기관에서 발급을 진행하고, 웹브라우저는 서버가 보유한 `SSL 인증서`를 인증 기관을 통해 검증을 받습니다. 또, `SSL 인증서`를 통해 인증 받는 동안 암호화와 복호화를 위한 `공개키`, `개인키`를 주고받습니다.

![image-20211011113045860](https://user-images.githubusercontent.com/59816811/136725129-542ce860-c0f1-4cf1-81be-95053fc40af6.png)

- 서버는 자신의 `공개키`와 사이트 정보를 인증기관에게 전달해 인증서를 받습니다.
- 인증기관은 `SSL인증서`를 발급하며 `서버의 공개키`를 담고, 인증기관의 `개인키`로 암호화하여 서버에게 전달합니다.
- 웹 브라우저는 `인증기관의 공개키`를 가져와 서버가 보여준 `SSL 인증서`를 복호화하고 인증서를 확인합니다. 또, `SSL인증서`안에 담긴 `서버의 공개키`를 얻습니다.
- 웹 브라우저는 이제 서버를 신뢰할 수 있으므로 `서버의 공개키`로 데이터 암호화에 사용할 `비밀키`를 암호화해서 전달합니다.
- 서버는 웹 브라우저가 보낸 데이터를 `서버의 개인키`로 복호화해서 `비밀키`를 얻습니다.
- 이제 웹 브라우저와 서버는 `비밀키`를 이용해 데이터를 암호화해서 소통합니다.

> 서버, 인증기관이 처음에 인증 작업을 위해서는 `비대칭키 알고리즘`을 사용해 `공개키`, `개인키`를 만들고 암호화를 진행합니다. 속도는 `대칭키 알고리즘`이 빠르지만 키를 공유해야된다는 문제를 `비대칭키 알고리즘`을 통해 해결하고 있습니다. 그 후, 실제 데이터를 암호화하는 방식은 `대칭키 알고리즘`을 이용하고 있습니다. 키를 공유해야된다는 문제를 `비대칭키 알고리즘`으로 암호화해서 해결하고 있습니다. 이처럼 `대칭키 알고리즘` 과 `비대칭키 알고리즘`의 장점들을 활용한 하이브리드 암호화 방식을 이용하고 있습니다.

<br>

### 4) HTTPS 의 약점

 HTTPS 는 HTTP 프로토콜에 SSL 프로토콜이 하나 더 추가됩니다. 즉, SSL 프로토콜 처리를 위한 과정이 추가됩니다. 암호화와 복호화를 위해 CPU나 메모리 등의 리소스를 다량으로 소비하게됩니다. 따라서, HTTPS 는 HTTP에 비해 네트워크의 부하가 2 ~ 100배 정도 느려지게 됩니다.

 결과적으로 HTTP / HTTPS 를 적절한 경우에 맞춰서 사용하는 것이 중요하다.

<br>

### 5) HTTPS 암호화

 HTTPS를 이용하면 HTTP 헤더, 바디 모두 암호화를 하게 되고, 결과적으로 `IP주소`를 제외하고 모두 암호화 하게 됩니다. `IP주소`를 모르면 패킷을 전달할 수 없으므로, `IP주소`는 암호화되지 않습니다.
