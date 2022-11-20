# **HTTP2**

## **😡 HTTP/1.1의 문제점**

### **Head-of-Line(HoL) blocking**

![hol_blocking](https://user-images.githubusercontent.com/39883609/202840047-f382d003-b80a-4ff4-8026-6ebb212ce56d.png)

- 클라이언트가 서버로 복수의 request 전송 시, 서버는 요청 받은 순서대로 response를 보내야 함.
- 서버가 중간 response 작성에 문제가 있는 경우, 후속 request들에 대한 response들은 클라이언트에 전송되지 못하고 지연됨.

### **Fat Message Headers**

- HTTP/1.1에서는 header에 모든 정보를 담아야 함.
- 매 request 마다 중복된 헤더를 전송하고 해당 도메인에 설정된 쿠키 정보도 매 요청 시 마다 헤더에 포함시켜 전송해야 함.
- 전송하려는 Body의 크기보다 헤더의 크기가 더 큰 경우도 발생함.

### **Limited Priorities**

- request의 우선순위를 설정할 수 없음.
- 필요 없는 데이터로 인해 우선순위가 높은 데이터가 blocking될 수 있음.

### **Client-Driven Transmission**

- 클라이언트가 서버로 request를 보내야만 통신이 가능한 구조임.
- 서버 입장에선 클라이언트의 request가 없으면 통신이 불가능함.

## **💨 SPDY**

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

## **🤔 왜 HTTP/1.2가 아닌 HTTP/2.0인가?**

![http1.1_vs_http2](https://user-images.githubusercontent.com/39883609/202841152-125b9c54-a257-49c2-9c1e-341409646940.png)

- Text 기반의 통신이 아닌 Binary framing 계층을 도입함.
- 해당 binary frame은 사람이 읽을 수 없는 데이터임.

![http2_frame](https://cheapsslsecurity.com/p/wp-content/uploads/2019/07/binary-framing-layer-1024x529.png)

- HTTP/1.1과 달리 HTTP/2는 더 작은 메시지와 프레임으로 분할되며 바이너리 형식으로 인코딩됨.
- 클라이언트와 서버는 새 바이너리 인코딩 메커니즘을 사용해야 함.

## **🌐 HTTP/2.0의 특징**

### **Stream, Message, Frame**

![http2_stream](https://user-images.githubusercontent.com/39883609/202897928-1497adb2-cb78-49c4-b0b3-b6dc208cbb2e.png)

- Stream: 하나의 TCP 연결 내에서 전달되는 바이트의 흐름, 하나 이상의 Message가 전달될 수 있음.
- Message: Request 또는 Response에 매칭되는 프레임의 전체 시퀀스임.
- Frame: HTTP/2.0에서 전달되는 최소 단위의 데이터 블록임. 각각의 프레임에는 하나의 프레임 헤더가 포함됨.

### **Multiplexing**

- HTTP/1.1에서는 하나의 TCP 연결에 하나의 request만을 보낼 수 있었음.
- 성능 개선을 위해 병렬 요청을 수행하려는 경우 여러 개의 TCP 연결이 사용되어야 함.

![HTTP2_connection](https://user-images.githubusercontent.com/39883609/202898170-412fe061-a7be-473a-959e-e7f90006b865.png)

- HTTP/2.0에서는 하나의 TCP 연결 내에서 여러 개의 request를 동시에 보낼 수 있음.
- 클라이언트와 서버 모두 HTTP 메세지를 독립된 프레임으로 세분화하고, 이 프레임들을 인터리빙한 다음, 다른 쪽에서 다시 조립함.

![HTTP_mux](https://user-images.githubusercontent.com/39883609/202898232-1faf6206-9563-4755-aed4-f4a2ad062ea2.png)

- 다수의 Request와 Response들을 병렬로 인터리빙.
- 단일 TCP 연결에서 병렬 전달.
- 불필요한 지연 시간을 제거하고 성능을 향상시킴.

### **Stream Prioritization**

- HTTP/2.0에서는 클라이언트가 서버에게 어떤 요청이 더 중요한지 알려줄 수 있음.
- 각각의 스트림이 가중치와 종속성을 가지고 있음.
  - 가중치: 중요한 정도, 1~256 사이의 정수
  - 종속성: 상하 관계, 다른 스트림에 대한 명시적 종속성 부여
- 스트림의 종속성 및 가중치 조합을 이용하여 클라이언트가 `우선순위 지정 트리`를 구성할 수 있음.

![Stream_prioritization](https://sunjoong85.github.io/assets/http2/h2_stream_prioritization.png)

- 동일한 상위 요소를 가진 스트림은 가중치에 비례하여 리소스가 할당됨.

> e.g. CloudFlare의 HTTP/2.0 우선순위 지정: https://blog.cloudflare.com/better-http-2-prioritization-for-a-faster-web/

### **Server Push**

- HTTP/1.1에서 불가능한 기능으로, 서버가 클라이언트의 Request에 Response를 보낼 뿐만 아니라 클라이언트가 명시적으로 요청하지 않아도 서버가 추가적인 리소스를 클라이언트에 보낼 수 있음.

![Server_push](https://web-dev.imgix.net/image/C47gYyWYVMMhDmtYSLOWazuyePF2/o6lgFK9DiyJmdWwyJnyy.svg)

- `push promise`: 클라이언트가 요청하지 않았지만 필요할 것이라고 생각하여 서버에서 푸시함.
- 서버 푸시 동작 방식
  - `PUSH_PROMISE`를 통해 서버에서 푸시하겠다고 알려줌.
  - 클라이언트는 `PUSH_PROMISE`받아 해당 리소스가 필요한지 필요하지 않은지 판단.
  - `PUSH_PROMISE` 프레임은 HTTP header 정보를 가짐.
  - 클라이언트가 `PUSH_PROMISE`를 거절하는 경우 `RST_STREAM` 프레임을 보냄.
  - 수락하는 경우 클라이언트에서 동시에 몇개를 받을지, 흐름 제어 Window의 크기를 어떻게 할지 설정 가능함.

### **Header Compression**

- Data Frame은 이미 압축되어 전송되므로 압축이 불필요함.
- HTTP/2.0에선 HPACK 알고리즘을 사용하여 Header Frame을 압축함.

![header_compression](https://web-dev.imgix.net/image/C47gYyWYVMMhDmtYSLOWazuyePF2/IYfczfC6ZCTxUVboaEZy.svg)

- 허프만 코드를 통해 인코딩하여 개별 전송 속도를 줄임.
- 하나의 TCP 위에서 스트림을 통해 통신하기 때문에 이전에 전송한 header 정보를 알고 있음.
- 이전 정보와 달라진 정보만 다음 메세지의 헤더에서 보냄.

## **🖥️ HTTP/2.0 서버 구현**

### **TLS 인증을 위한 인증서 발급 및 CA 생성**

```bash
$ brew install mkcert
$ mkcert -install
$ mkcert localhost
```

![cert_result](https://user-images.githubusercontent.com/39883609/202900196-d5882223-e540-4dec-92dc-a5112f38126b.png)

### **HTTP/2.0 서버 구현**

- go의 http 모듈에서 기본적으로 HTTP/2.0 프로토콜을 지원함.
- 간단한 `form`과 이미지들을 보여주는 템플릿 페이지 구현.
- 디렉토리 구조

```text
.
├── go.mod
├── http2.go
├── index.html
├── localhost-key.pem
├── localhost.pem
└── static
    ├── css
    │   ├── gallery.css
    │   ├── main.css
    │   ├── normalizeLogin.css
    │   └── styleLogin.css
    ├── image
    │   ├── img_1.png
    │   ├── img_10.png
    │   ├── img_11.png
    │   ├── img_12.png
    │   ├── img_13.png
    │   ├── img_14.png
    │   ├── img_15.png
    │   ├── img_16.png
    │   ├── img_17.png
    │   ├── img_18.png
    │   ├── img_19.png
    │   ├── img_2.png
    │   ├── img_20.png
    │   ├── img_21.png
    │   ├── img_22.png
    │   ├── img_23.png
    │   ├── img_24.png
    │   ├── img_25.png
    │   ├── img_26.png
    │   ├── img_27.png
    │   ├── img_28.png
    │   ├── img_29.png
    │   ├── img_3.png
    │   ├── img_30.png
    │   ├── img_31.png
    │   ├── img_32.png
    │   ├── img_33.png
    │   ├── img_34.png
    │   ├── img_35.png
    │   ├── img_36.png
    │   ├── img_37.png
    │   ├── img_38.png
    │   ├── img_39.png
    │   ├── img_4.png
    │   ├── img_40.png
    │   ├── img_5.png
    │   ├── img_6.png
    │   ├── img_7.png
    │   ├── img_8.png
    │   └── img_9.png
    └── js
        └── login.js

4 directories, 50 files
```

```go
package main

import (
	"crypto/tls"
	"fmt"
	"html/template"
	"log"
	"net/http"
	"os"
	"time"
)

var tpl *template.Template

func init() {
	tpl = template.Must(template.ParseGlob("./*.html"))
}

func main() {
	server := &http.Server{
		Addr:         ":8080",
		ReadTimeout:  5 * time.Second,
		WriteTimeout: 10 * time.Second,
		TLSConfig:    tlsConfig(),
	}

	log.Printf("Create Server on https://localhost:8080")

	http.Handle("/static/",
		http.StripPrefix("/static",
			http.FileServer(http.Dir("./static"))))
	http.HandleFunc("/", index)

	if err := server.ListenAndServeTLS("", ""); err != nil {
		log.Fatal(err)
	}
}

func index(w http.ResponseWriter, r *http.Request) {
	if pusher, ok := w.(http.Pusher); ok {
		options := &http.PushOptions{
			Header: http.Header{
				"Accept-Encoding": r.Header["Accept-Encoding"],
			},
		}

		checkServerPush(pusher.Push("/static/js/login.js", options))
		checkServerPush(pusher.Push("/static/css/normalizeLogin.css", options))
		checkServerPush(pusher.Push("/static/css/styleLogin.css", options))
	} else {
		fmt.Println("SERVER PUSH NOT ALLOWED")
	}

	tpl.ExecuteTemplate(w, "index.html", nil)
}

func tlsConfig() *tls.Config {
	crt, err := os.ReadFile("./localhost.pem")
	checkErr(err)

	key, err := os.ReadFile("./localhost-key.pem")
	checkErr(err)

	cert, err := tls.X509KeyPair(crt, key)
	checkErr(err)

	return &tls.Config{
		Certificates: []tls.Certificate{cert},
		ServerName:   "localhost",
	}
}

func checkErr(err error) {
	if err != nil {
		log.Fatal(err)
	}
}

func checkServerPush(err error) {
	if err != nil {
		log.Printf("FAILED TO SERVER PUSH: %v", err)
	}
}
```

### **결과**

![http2_server](https://user-images.githubusercontent.com/39883609/202904736-1c310088-045d-411d-80a9-5b52c838d42c.png)

- h2 프로토콜로 전송된 것을 볼 수 있음.
- 해당 요청들이 `HTTP/2.0` 프로토콜로 전송되었기 때문에, `HTTP/1.1` 프로토콜로 전송되는 요청들과는 달리, `HTTP/2.0` 프로토콜에서는 `Multiplexing`이 가능함.

### **HTTP/1.1 서버와 비교**

![http1_server](https://user-images.githubusercontent.com/39883609/202904865-71af4807-0e92-43a8-89c4-feab4e2ccd64.png)

- http1.1 프로토콜로 전송된 것을 볼 수 있음.
- `HTTP/1.1` 프로토콜에서는 `Multiplexing`이 불가능하기 때문에, 요청이 많아지면, `Connection`이 많아지고, `Connection`이 많아지면, `Latency`가 증가함.

### **Removing Server Push from Chrome**

![Server_push_error](https://user-images.githubusercontent.com/39883609/202904991-cfa77cb7-1085-4bd9-be39-585f945f6295.png)

- Chrome 106 버전부터 Chrome 및 Chromium 기반 브라우저에서는 `Server Push`를 지원하지 않음.
- 본래 기획된 의도와는 다르게 잘 사용되지 않았으며, 성능 저하를 유발하는 원인이 되었기 때문.
- Server Push의 대안으로는 `103 Early Hints`가 있음.

> https://developer.chrome.com/blog/removing-push/
