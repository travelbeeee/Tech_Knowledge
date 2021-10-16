# HTTP2 프로토콜

 HTTP2 프토토콜은 HTTP1.1 프로토콜을 개선한 것으로 2015년부터 웹에서 사용되고 있는 프로토콜입니다.

"HTTP/2 is a replacement for how HTTP is expressed “on the wire.” It is not a ground-up rewrite of the protocol; HTTP methods, status codes and semantics are the same, and it should be possible to use the same APIs as HTTP/1.x (possibly with some small additions) to represent the protocol.

 HTTP/2는 프로토콜을 새롭게 다시 작성하는 것이 아니다.

The focus of the protocol is on performance; specifically, end-user perceived latency, network and server resource usage. One major goal is to allow the use of a single connection from browsers to a Web site."

 HTTP/2의 초점은 성능에 있다.

 즉, HTTP/1.1의 성능 개선에 초점을 맞추고 완전히 새로운 프로토콜을 만든 것이 아니라 HTTP/1.1을 개선한 프로토콜입니다.

![http2_02](https://user-images.githubusercontent.com/59816811/137593726-8650ff73-9642-45cc-9e4e-1a4b0009f401.png)

<br>

### 1) HTTP/1.1 문제점

 HTTP는 TCP 연결 기반 위에서 동작하는 프로토콜입니다. 신뢰성 확보를 위해 연결을 맺고 끊는 데 있어서 핸드 셰이크가 이루어집니다. 거기에다 HTTP는 비연결성 프로토콜이기 때문에 한 번 연결로 한 번의 요청과 응답을 하고 응답이 끝나면 연결을 끊어 버립니다. 연결을 맺고 끊을 때마다 핸드 셰이크를 하기 때문에 비연결성 프로토콜에선 오버헤드가 생깁니다. 특히 요즘 웹서비스들은 하이퍼 텍스트라기보다는 많은 정적 데이터로 이루어진 하이퍼 미디어로 발전했기 때문에 오버헤드가 커질 수 있습니다. 그래서 HTTP/1.1에서 **Keep-alive** 기능이 추가되어 한 번 맺어졌던 연결을 끊지 않고 지속적으로 유지하여 불필요한 핸드 셰이크를 줄여 성능을 개선할 수 있습니다. 이런 식으로 웹페이지가 예전의 텍스트 위주와는 다르게 점점 미디어들이 추가되고 상태(쿠키, 세션 등)를 유지하려는 기술들이 요구되다 보니 성능 개선이 반드시 필요하게 되었고 이를 위해 부가적인 기능들이 추가되다 H2까지 발전하게 된 것입니다.

 HTTP/1.1에서 성능 개선을 위해 파이프라이닝이라는 기술이 도입되었습니다. 하나의 커넥션에서 한 번에 순차적인 여러 요청을 연속적으로 하고 그 순서에 맞춰 응답을 받는 방식으로 지연 시간을 줄이는 방법입니다. 순차적으로 데이터를 요청하고 받아야 하다 보니 먼저 받은 요청이 끝나지 않으면 그 뒤에 있는 요청의 처리가 아무리 빨리 끝나도 먼저 온 요청이 끝날 때까지 기다려야 합니다. 이를 HTTP의 **HOL(Head Of Line)** Blocking 문제라고 하고 파이프라이닝의 큰 문제입니다. 그래서 모던 브라우저들은 대부분은 파이프라이닝을 사용하지 못하도록 막아 놓았습니다. 그래서 HTTP/1.1으로 통신할 때 클라이언트(브라우저)가 요청을 병렬로 하기 위해서 6-8개(브라우저마다 다름)의 커넥션을 이용해 데이터를 가져오는 방식으로 성능을 개선하고 있습니다. 하지만, 이 문제는 여러 TCP 연결이 사용되어야 한다는 문제가 있습니다.

- HOL Blocking, 특정 응답의 지연

  pipelining을 통해 하나의 Connection에서 다수 개의 요청, 응답을 처리하게 개선되었으나 첫 번째 응답이 지연되면 뒤에 있는 요청들도 모두 대기하게되는 HOL Blocking 문제가 아직 남아있습니다.

- RTT(Round Trip Time) 증가

  매 요청마다 3-Way Handshake, 4-Way Handshake를 하므로

- 무거운 Header 구조 (특히 Cookie)

<br>

### 2) HTTP/2 Binary Frame

 HTTP/2의 핵심은 바이너리 프레이밍 계층입니다.바이너리 프레이밍 메커니즘이 도임되면서 클라이언트와 서버 간에 데이터 교환 방식이 바뀌었습니다.

- 스트림 : 구성된 연결 내에서 전달되는 바이트의 양방향 흐름이며, 하나 이상의 메시지가 전달될 수 있습니다.
- 메시지 : 논리적 요청 또는 응답 메시지에 매핑되는 프레임의 전체 시퀀스입니다.
- 프레임 : HTTP/2에서 통신의 최소 단위이며 각 최소 단위에는 하나의 프레임 헤더가 포함됩니다. 이 프레임 헤더는 최소한으로 프레임이 속하는 스트림을 식별합니다.
- 모든 통신은 단일 TCP 연결을 통해 수행되며 전달될 수 있는 양방향 스트림의 수는 제한이 없습니다.
- 각 스트림에는 양방향 메시지 전달에 사용되는 고유 식별자와 우선순위 정보(선택 사항)가 있습니다.
- 각 메시지는 하나의 논리적 HTTP 메시지(예: 요청 또는 응답)이며 하나 이상의 프레임으로 구성됩니다.
- 프레임은 통신의 최소 단위이며 특정 유형의 데이터(예: HTTP 헤더, 메시지 페이로드 등)를 전달합니다. 다른 스트림들의 프레임을 인터리빙한 다음, 각 프레임의 헤더에 삽입된 스트림 식별자를 통해 이 프레임을 다시 조립할 수 있습니다.

![화면 캡처 2021-10-17 005401](https://user-images.githubusercontent.com/59816811/137594029-4c1f2a76-4ff4-4463-9357-c5d5bdb75d05.png)

<br>

### 3) HTTP/2 Multiplexed Streams

 HTTP String 메시지를 Binary Frame으로 캡슐화해 클라이언트와 서버 사이에서 전송합니다. 이로 인해 요청과 응답의 멀티플렉싱을 지원해줍니다. HTTP 메시지를 바이너리 형태의 프레임으로 나누고 이를 전송 후 받은 쪽에서 다시 조립합니다. 요청과 응답이 동시에 이루어지니 하나의 연결에 여러 요청과 응답이 뒤섞여 있습니다. 프레이밍 작업은 서버와 클라이언트(브라우저)에서 해주기 때문에 큰 변경사항을 고려하지 않아도 됩니다. 바이너리 프레이밍과 멀티플렉싱을 이용해 여러 개의 연결 없이 병렬 처리도 할 수 있고 파이프라이닝과 달리 HOL문제를 해결한 것입니다.

![화면 캡처 2021-10-17 010137](https://user-images.githubusercontent.com/59816811/137594248-c195c37b-0e95-41e4-bc30-1dede89d64aa.png)

<br>

### 4) HTTP/2 Stream Prioritization

 HTTP/2 는 Binary Frame 안에 우선 순위 정보를 담을 수 있습니다. 리소스 간의 의존 관계를 통해 렌더링을 지연 방지 할 수 있습니다.

- 각 스트림에는 1~256 사이의 정수 가중치가 할당될 수 있습니다.
- 각 스트림에는 다른 스트림에 대한 명시적 종속성이 부여될 수 있습니다.

![화면 캡처 2021-10-17 010038](https://user-images.githubusercontent.com/59816811/137594210-64a2cd8b-691d-4554-90af-9b335c6bd4d9.png)

<br>

### 5) HTTP/2 Server Push

 HTTP/2는 서버가 클라이언트 요청에 대해 여러 응답을 보낼 수 있습니다. 즉, 서버는 원래 요청에 응답할 뿐만 아니라 클라이언트가 명시적으로 요청하지 않아도 서버가 추가적인 리소스를 클라이언트에 푸시할 수 있습니다. HTML 파일을 요청했으면 필요한 CSS, JS 파일들도 같이 Server가 보냅니다.

![화면 캡처 2021-10-17 010421](https://user-images.githubusercontent.com/59816811/137594324-a1a27710-80b7-4dd4-8ae4-7598c3191425.png)

<br>

### 6) HTTP/2 Header Data Compression

 HTTP/2 에서는 Header 를 Plain Text가 아니라 `Huffman Encoding`을 사용하는 `HPACK`을 이용해 압축을 하고, 중복되는 필드는 재전송하지 않습니다. 

![image](https://user-images.githubusercontent.com/59816811/137594485-55075a5e-116e-4f8e-a670-1e07fbd39226.png)

<br>

### 7) Spring에서 HTTP2 이용하기

 톰캣 8.5버전 이상부터 HTTP/2 를 지원하고 있습니다. 또, HTTP/2를 사용하기 위해서는 HTTPS 설정도 되어 있어야 합니다. 스프링 부트에서는 `server.http2.enabled` 설정만 true로 바꿔주면 됩니다. 빈을 직접 등록해주는 방법도 있습니다.

```properties
server.http2.enabled = true
```

```java
@Bean
public ConfigurableServletWebServerFactory tomcatCustomizer() {
    TomcatServletWebServerFactory factory = new TomcatServletWebServerFactory();
    factory.addConnectorCustomizers(connector -> connector.addUpgradeProtocol(new Http2Protocol()));
    return factory;
}
```

