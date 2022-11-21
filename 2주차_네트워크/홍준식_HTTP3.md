# HTTP 역사
## HTTP/0.9
단일라인 요청 (가능한 메서드는 GET 이 유일)  
```
GET /mypage.html
```  
응답 또한 파일 내용 자체로 구성되어있다. (오직 HTML 파일만 전송 가능)  
```
<HTML>
A very simple HTML page
</HTML>
```
## HTTP/1.0
- 버전 정보 포함 (`GET /mypage.html HTTP/1.0`)  
- 응답 상태 코드 추가 (200, 201, 404...)  -> 캐시 제어 가능
- HTTP 헤더 개념 추가 -> HTML 외의 파일 전송 가능 (Content-Type)  
## HTTP/1.1 - 표준 프로토콜
- 커넷션 재사용 가능  
- 파이프라이닝 도입 (첫번째 요청이 종료되기 전 두번째 요청 전송 가능)  
- Chunked Message 가능 (큰 메세지를 분할하여 전송)  
- 캐시 제어 매커니즘 도입  
- 언어, 인코등, 타입 등 컨텐츠 협상 도입  
- Host 헤더 (Host + Port, default 80) 도입으로 동일 IP 다른 도메인 호스트 가능  
## HTTP/2
- 텍스트 프로토콜이 아닌 이진 프로토콜 -> 사람이 읽을 수 없음, 더 가벼움  
- 동일한 커넥션 내부 병렬 요청  
- 연속된 내용의 매우 유사한 헤더 압축
- 사전에 클라이언트 캐시를 서버 푸쉬를 통해 채워넣을 수 있음  
### HOLB(Head-of-line Blocking)  
HOLB 문제는 여러 요청 중 하나의 요청에 문제가 있는 경우 후속 요청들이 모두 지연되는 문제이다.  
HTTP/2는 multiplexing으로 이 문제를 해결하였는데 여전히 TCP 레벨에서의 문제점인 모든 스트림이 연관되어있고 모든 패킷은 정확한 순서대로 처리되어야하기때문에 하나의 패킷 손실이 해당 패킷이 재전송될때까지 모든 스트림을 멈추는 문제가 여전히 존재한다.  
![TCP HTTP](https://blog.cloudflare.com/content/images/2018/07/http-request-over-tcp-tls@2x.png)  
![UDP HTTP](https://blog.cloudflare.com/content/images/2018/07/http-request-over-quic@2x.png)  
## HTTP/3 (QUIC)
TPC는 핸드쉐이크와 slow start 방식 때문에 굉장히 느리다.  
QUIC는 하나의 QUIC 연결을 통해 여러 스트림을 전송할 수 있고 추가적인 핸드 쉐이크와 slow start가 필요 없다.  
또한 각각의 스트림은 독립적으로 전달되어서, 하나의 패킷 손실이 다른 스트림에 영향을 주지 않는다. 즉, HTTP/2의 HOLB 문제를 해결한다.  
또한 클라이언트의 IP가 바뀌어도 연결을 유지할 수 있는데, 클라이언트와 서버의 연결을 IP + Port가 아닌 랜덤한 Connection ID를 통해 맺으므로 IP가 변경되어도 연경을 유지할 수 있어 모바일 환경에서 용이하다.
또한 UDP는 TCP처럼 OS에 많은 것이 종속되어있지 않아 OS 변화에 무관하다.  

CloudFlare 등 CDN 에서 HTTP3를 일반 사용자에게 제공하고 있다.
NGINX 또한 [NGINX-QUIC](https://quic.nginx.org) 을 개발중이다.
2022년 11월 21일 기준 25.8%의 웹사이트에서 사용되고 있다. [참조](https://w3techs.com/technologies/details/ce-http3)  


## 참조
https://stackoverflow.com/questions/45583861/how-does-http2-solve-head-of-line-blocking-hol-issue  
https://blog.cloudflare.com/http3-the-past-present-and-future/  
https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP/Evolution_of_HTTP  
https://developer.mozilla.org/ko/docs/Glossary/QUIC  
