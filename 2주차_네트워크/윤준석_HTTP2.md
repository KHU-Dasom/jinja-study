# HTTP2

## 😡 HTTP/1.1의 문제점

### Head-of-Line(HoL) blocking

![hol_blocking](https://user-images.githubusercontent.com/39883609/202840047-f382d003-b80a-4ff4-8026-6ebb212ce56d.png)

- 클라이언트가 서버로 복수의 request 전송 시, 서버는 요청 받은 순서대로 response를 보내야 함.
- 서버가 중간 response 작성에 문제가 있는 경우, 후속 request들에 대한 response들은 클라이언트에 전송되지 못하고 지연됨.

### Fat Message Headers

- HTTP/1.1에서는 header에 모든 정보를 담아야 함.
- 매 request 마다 중복된 헤더를 전송하고 해당 도메인에 설정된 쿠키 정보도 매 요청 시 마다 헤더에 포함시켜 전송해야 함.
- 전송하려는 Body의 크기보다 헤더의 크기가 더 큰 경우도 발생함.

### Limited Priorities

- request의 우선순위를 설정할 수 없음.
- 필요 없는 데이터로 인해 우선순위가 높은 데이터가 blocking될 수 있음.

### Client-Driven Transmission

- 클라이언트가 서버로 request를 보내야만 통신이 가능한 구조임.
- 서버 입장에선 클라이언트의 request가 없으면 통신이 불가능함.

## 💨 SPDY

- Google에서 개발한 비표준 네트워크 프로토콜임.

![spdy](https://d2.naver.com/content/images/2015/06/helloworld-140351-1.png)

- HTTP/1.1의 문제점을 해결하기 위해 개발되었으며, HTTP/2.0의 기반이 되었음.
- 주요 목표는 다음과 같음:
  - 항상 TLS 위에서 동작 -> 암호화되지 않은 연결은 지원하지 않음
  - 응답 다중화 (Multiplexing) -> 여러 request를 동시에 보낼 수 있음
  - 헤더 압축을 통한 프로토콜 오버헤드 최소화
  - request 별 우선순위 지정 가능 -> 중요한 request에 더 많은 리소스를 할당할 수 있음
  - 서버 푸시 -> 클라이언트가 요청하지 않은 리소스를 미리 보낼 수 있음
  - 기존 어플리케이션의 수정 없는 지원 -> HTTP/1.1과 호환됨

## 🤔 왜 HTTP/1.2가 아닌 HTTP/2.0인가?

![http1.1_vs_http2](https://user-images.githubusercontent.com/39883609/202841152-125b9c54-a257-49c2-9c1e-341409646940.png)

- Text 기반의 통신이 아닌 Binary framing 계층을 도입함.
- 해당 binary frame은 사람이 읽을 수 없는 데이터임.

![http2_frame](https://cheapsslsecurity.com/p/wp-content/uploads/2019/07/binary-framing-layer-1024x529.png)

- HTTP/1.1과 달리 HTTP/2는 더 작은 메시지와 프레임으로 분할되며 바이너리 형식으로 인코딩됨.
- 클라이언트와 서버는 새 바이너리 인코딩 메커니즘을 사용해야 함.

## 🌐 HTTP/2.0의 특징

### 