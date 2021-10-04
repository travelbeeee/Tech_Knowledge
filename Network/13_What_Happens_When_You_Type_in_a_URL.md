# 브라우저에 URL을 입력했을 때 

 제목 그대로 브라우저에 URL을 입력했을 때 네트워크 상에서 어떤 일들이 일어나는지 자세히 정리해보겠습니다.

### 1) 브라우저 URL 파싱

 URL을 입력받은 브라우저는 일단 URL 구조를 해석합니다. 어떤 프로토콜로 어느 도메인의 어떤 포트로 보낼지 해석합니다.

```
//URL구조
http://www.example.com:80/path/to/myfile.html?key1=value1&key2=value2
-프로토콜 : http
-도메인 : www.example.com
-포트번호 : 80
-자원경로 : /path/to/myfile.html
-파라미터 : key1=value1&key2=value2
```

 포트를 명시적으로 적지 않으면, 브라우저가 기본값을 이용하고 HTTP는 80번 포트, HTTPS 는 443 포트가 기본값입니다.

<br>

### 2) HSTS 목록 조회

 HSTS 목록 조회를 통해 해당 요청을 HTTPS로 보낼지 판단합니다. HSTS목록에 해당 URL이 존재한다면 명시적으로 HTTP를 통해 요청한다 해도 브라우저가 이를 HTTPS로 요청합니다.

<br>

### 3) URL 을 IP 주소로 변환

 브라우저에서는 자신의 로컬 hosts 파일과 브라우저 캐시에 해당 URL이 존재하는지 확인합니다. 존재하지 않는다면 도메인 주소를 IP주소로 변환해주는 DNS(Domain Name System) 서버에 요청하여 해당 URL을 IP주소로 변환합니다. 

<br>

### 4) 라우터를 통해 해당 서버의 게이트웨이로 이동

 DNS서버에게 IP주소를 받았으니 이제 해당 서버로 요청을 보냅니다. 요청을 보낼 IP주소로 가야 하는 것은 알지만 어떻게 가야 할지 경로는 알 수 없습니다. 이 요청이 네트워크를 타고 어떻게 이동할지는 네트워크 장비인 라우터의 라우팅을 통해 이루어집니다. 라우터에서는 라우팅 테이블을 통해 해당 요청이 어떤 경로를 통해 가야할지 경로를 지정해줍니다.

<br>

### 5) ARP를 통해 IP 주소를 MAC 주소로 변환

 실질적인 통신을 하기 위해서는 논리 주소인 IP주소를 물리 주소인 MAC 주소로 변환해야 합니다. 이를 위해 해당 네트워크 내에서 ARP를 브로드 캐스팅합니다. 해당 IP주소를 가지고 있는 노드는 자신의 MAC 주소를 응답합니다.

<br>

### 5) 대상 서버와 TCP 소켓 연결

 대상 서버와 통신하기 위해 TCP 소켓 연결을 진행합니다. `3-way-handshake` 과정을 통해 이루어지고, HTTPS 요청인 경우에는 `TLS-handshaking` 작업이 추가됩니다.

<br>

### 6) HTTP 프로토콜로 요청

 이제 연결되었으니 서버에게 요청을 합니다. 서버는 이 요청에 대한 응답을 생성하여 브라우저에게 전달합니다.

<br>

### 7) HTTP 응답 해석

 서버에서 응답한 HTML, CSS, JS 등의 파일들을 브라우저가 렌더링해서 보여줍니다.

<br>

### 8) 번외 - HSTS 란 

 HSTS(HTTP Strict Transport Security) 는 HTTPS Protocol을 지원하는 Web Site에서, 자신은 HTTPS Protocol만 사용해서 통신할 수 있음을, 접속하고자 하는 Web Browser에게 알려 주는 기능입니다. Web Browser에서도 HSTS 기능을 지원해야, HSTS 기능이 제대로 동작합니다. 2010년 이후에 출시된 대부분의 Web Browser 버전에서는 HSTS 기능을 지원하고 있습니다. HSTS를 지원하는 Web Browser는 내부에 HSTS List를 보유하고 있습니다. 즉, HTTPS Protocol을 사용해야만 하는 Web Site에 대한 정보를 가지고 있습니다.

 HSTS를 지원하는 Web Browser는 내부에 HSTS List를 보유하고 있습니다. 즉 HTTPS Protocol을 사용해야만 하는 Web Site에 대한 정보를 가지고 있습니다. 사용자가 Web Browser 주소창에 도메인 이름을 입력하면(또는 실수로 “http://” 를 사용하여 URL 주소를 입력하면), 또는 Link된 URL 주소를 클릭하면, 도메인 주소만 추출하여 HSTS List에 있는지 확인하고 있으면 HTTPS Protocol을 사용하여 접속하게 됩니다. 없다면, HTTP Protocol로 접속하고 "301 Redirect" 응답으로 인해, 다시 HTTPS Protocol로 접속하게 됩니다. 이때, 새로운 도메인 이름을 HSTS List에 추가합니다.

 이미 만들어져 있는 HSTS Lists를 Web Browser에 제공하는 기능도 지원하고 있는 데, 이러한 HSTS Lists를 “Preloaded HSTS Lists” 라고 합니다. Preloaded HSTS Lists는, Web Browser 설치 시 또는 Update 시에 Web Browser Vendor에 의해  제공되어 진다고 합니다. MS사는 분기별로 Update 한다고 합니다.

<br>

### 9) 번외 - HSTS 장점

 HSTS는 보안을 강화하기 위한 목적으로 등장했습니다. 해커와 같은 공격자가 중간자공격(MITM attack)을 하여 중간에 Proxy Server를 두고 사용자와는 HTTP 통신을 하고,실제 Site 와는 HTTPS 통신을 해도 사용자는 전혀 인식을 하지 못하게 됩니다. 즉 사용자가 실제 Site 와 주고 받는 모든 정보는 공격자에게 노출이 되게 됩니다. 이러한 공격을 “SSL Stripping” 공격(attack)이라고 부르며, 이러한 공격을 방지하기 위하여 HSTS 기능을 사용합니다.

<br>



