# HTTP/2


### HTTP/2

HTTP/2의 주요 목적은 전체 요청 및 `응답 다중화`를 활성화 하여 대기 시간을 줄이고 HTTP `헤더 필드의 효율적인 압축`을 통해 프로토콜 오버헤드를 최소화하며 요청 우선 순위 지정 및 `서버 푸시`에 대한 지원을 추가하는 것.

HTTP2는 HTTP 프로토콜의 핵심 개념은 변경되지 않았음 대신 전체 프로세스를 관리하는 클라이언트와 서버 간에 데이터 형식이 프레임으로 지정하고 스트림이라는 연결단위를 통해 헤더 프레임 혹은 본문 프레임을 보낸다. 결과적으로는 기존에 사용하던 HTTP 프로토콜의 버전을 HTTP/2로 변경하여도 애플리케이션은 수정하지 않아도 된다.

기존 HTTP/1.1은 파이프라인을 추가하여 응답을 대기하지 않고 요청을 여러번 보낼 수 있게 되었지만, 응답은 순차적으로 이뤄지기 때문에 후순위 응답은 지연될 수밖에 없다. (네트워크 병목 발생 가능성 높음) 그리고 중복적인 헤더 필드가 매 요청마다 반복될 수 있기 때문에 불필요한 네트워크 트래픽을 유발할 수 있다.

### 바이너리 프레이밍 레이어

HTTP/2의 성능 향상의 핵심은 HTTP 메세지가 캡슐화되고 클라이언트 서버간에 전송되는 방법을 지시하는 새로운 바이너리 프레이밍 계층의 등장 덕이다.

줄바꿈으로 구분된 일반 텍스트 HTTP/1.x 프로토콜과 달리 HTTP/2 통신은 더 작은 메시지가 프레임으로 분할되며 각각은 이진 형식으로 인코딩된다.

이런 특별한 계층이 추가되었기 때문에 클라이언트와 서버 모두 바이너리 인코딩 메커니즘을 사용하여 서로의 메세지를 이해해야 한다. HTTP/1.x 클라이언트는 HTTP/2 전용 서버를 이해하지 못하고 그 반대도 마찬가지이다.

클라이언트와 서버가 바이너리 프레이밍 작업을 수행하기 때문에 응용 프로그램은 이러한 변화를 인식하지 못한다. (신경쓰지 않아도 된다는 얘기)

### 헤더압축

HTTP/1.x 에선 헤더의 메타데이터를 항상 일반 텍스트로 전송한다. 헤더에 쿠키등의 정보를 추가하면 HTTP Header의 크기는 HTTP Body의 크기와 별 다를 것 없어진다고 한다. 문제는 HTTP는 무상태 특성을 가지고 있기 때문에 매 요청마다 중복된 Header Field를 전송해야 한다는 것이다. HTTP/2는 이러한 오버헤드를 줄이고 성능을 개선하기 위해 HPACK 압축 형식을 사용하여 요청 및 응답 헤더를 압축한다.

<img width="661" alt="image" src="https://user-images.githubusercontent.com/56028408/196042198-62ea9260-41d9-4f30-af90-c52c97e325fc.png">


**주절주절**

아직 제대로 이해 못했지만 ,, Static Table과 Dynamic Table이 존재하는데 Static Table에는 HTTP/2 Spec에 정의된 자주 사용되는 Key-Value 값 쌍을 저장하고 있고 Dynamic Table은 한번 송수신한 Header의 Key-Value를 저장해둔다고 한다.

첫 요청에서 Static Table에 저장된 key 값으로 헤더의 바이트 크기를 줄이고 Static Table에 저장되어있지 않은 값은 Dynamic Table에 저장해두는 것 같다.

그리고 이후 요청에서 Static Table의 값과 Dynamic Table의 값으로 전부 대체할 수 있다면 헤더의 크기는 상당히 줄어드는 것으로 보인다.

### 응답 다중화(Multiplexing)

HTTP/1.x는 클라이언트가 다중 병렬 요청을 하려면 다중 TCP 연결을 사용해야 한다. 이 경우 연결당 한번의 하나의 응답만 전달된다. 이는 `Head of Line Blocking`이 발생하고 TCP 연결이 비효율적으로 사용될 수 있다는 문제점이 있다.

HTTP/2는 이러한 문제점을 해결하기 위해 `Multiplexing` 방법을 도입하였다. `Multiplexing` 방식은 기존 HTTP/1.1과 동일하게 여러개의 요청을 받지만 Stream channel을 이용하여 하나의 Connection 만으로 여러개의 응답을 받을 수 있다. `MultiConnection`과 `Multiplexing`의 차이인 듯 하다. 동작 방식은 아직 잘 이해 못하겠다. 어렵다.

### 서버 푸쉬(Server Push)

HTTP/1.1 이전 버전에서는 클라이언트가 서버에 요청을 보내야만 원하는 데이터를 응답받을 수 있었다.

HTTP/2 서버푸쉬를 이용하면 클라이언트가 요청하기 전에 HTTP/2 호환 서버가 리소스를 HTTP/2 호환 클라이언트에게 보낼 수 있다. 서버 푸쉬의 목적은 클라이언트가 리소스가 필요한지 알기도 전에 미리 리소스를 로드하여 대기 시간을 줄이는 것을 목표로 하는 기술이다. 이때 한 페이지 렌더링에 필요한 여러 리소스를 전달하더라도 한 라운드의 HTTP 통신만 필요하기 때문에 성능이 향상된다고 한다.

그런데 Google 크롬 팀에 따르면 HTTP/2의 서버 푸쉬는 거의 사용되지 않는다고 한다. 실제로 푸시된 리소스를 사용하는 경우보다 사용하지 않는 이유 때문이라고 한다.

### HTTP/2 버전 식별

- h2 = TLS 사용하는 HTTP/2
- h2c = TLS 사용하지 않는 HTTP/2 ALPN 프로토콜 식별자로 직렬화


# Reference
- [https://web.dev/performance-http2/](https://web.dev/performance-http2/)
- [https://medium.com/@devfallingstar/network-http-2%EC%97%90%EC%84%9C-multiplexing%EC%9D%B4%EB%9E%80-565a7b184c](https://medium.com/@devfallingstar/network-http-2%EC%97%90%EC%84%9C-multiplexing%EC%9D%B4%EB%9E%80-565a7b184c)
- [https://luavis.me/http2/http2-overall-operation](https://luavis.me/http2/http2-overall-operation)
- [https://http2.github.io/faq/](https://http2.github.io/faq/)

