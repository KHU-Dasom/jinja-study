# **HTTP2**

## **๐ก HTTP/1.1์ ๋ฌธ์ ์ **

### **Head-of-Line(HoL) blocking**

![hol_blocking](https://user-images.githubusercontent.com/39883609/202840047-f382d003-b80a-4ff4-8026-6ebb212ce56d.png)

- ํด๋ผ์ด์ธํธ๊ฐ ์๋ฒ๋ก ๋ณต์์ request ์ ์ก ์, ์๋ฒ๋ ์์ฒญ ๋ฐ์ ์์๋๋ก response๋ฅผ ๋ณด๋ด์ผ ํจ.
- ์๋ฒ๊ฐ ์ค๊ฐ response ์์ฑ์ ๋ฌธ์ ๊ฐ ์๋ ๊ฒฝ์ฐ, ํ์ request๋ค์ ๋ํ response๋ค์ ํด๋ผ์ด์ธํธ์ ์ ์ก๋์ง ๋ชปํ๊ณ  ์ง์ฐ๋จ.

### **Fat Message Headers**

- HTTP/1.1์์๋ header์ ๋ชจ๋  ์ ๋ณด๋ฅผ ๋ด์์ผ ํจ.
- ๋งค request ๋ง๋ค ์ค๋ณต๋ ํค๋๋ฅผ ์ ์กํ๊ณ  ํด๋น ๋๋ฉ์ธ์ ์ค์ ๋ ์ฟ ํค ์ ๋ณด๋ ๋งค ์์ฒญ ์ ๋ง๋ค ํค๋์ ํฌํจ์์ผ ์ ์กํด์ผ ํจ.
- ์ ์กํ๋ ค๋ Body์ ํฌ๊ธฐ๋ณด๋ค ํค๋์ ํฌ๊ธฐ๊ฐ ๋ ํฐ ๊ฒฝ์ฐ๋ ๋ฐ์ํจ.

### **Limited Priorities**

- request์ ์ฐ์ ์์๋ฅผ ์ค์ ํ  ์ ์์.
- ํ์ ์๋ ๋ฐ์ดํฐ๋ก ์ธํด ์ฐ์ ์์๊ฐ ๋์ ๋ฐ์ดํฐ๊ฐ blocking๋  ์ ์์.

### **Client-Driven Transmission**

- ํด๋ผ์ด์ธํธ๊ฐ ์๋ฒ๋ก request๋ฅผ ๋ณด๋ด์ผ๋ง ํต์ ์ด ๊ฐ๋ฅํ ๊ตฌ์กฐ์.
- ์๋ฒ ์์ฅ์์  ํด๋ผ์ด์ธํธ์ request๊ฐ ์์ผ๋ฉด ํต์ ์ด ๋ถ๊ฐ๋ฅํจ.

## **๐จ SPDY**

- Google์์ ๊ฐ๋ฐํ ๋นํ์ค ๋คํธ์ํฌ ํ๋กํ ์ฝ์.

![spdy](https://d2.naver.com/content/images/2015/06/helloworld-140351-1.png)

- HTTP/1.1์ ๋ฌธ์ ์ ์ ํด๊ฒฐํ๊ธฐ ์ํด ๊ฐ๋ฐ๋์์ผ๋ฉฐ, HTTP/2.0์ ๊ธฐ๋ฐ์ด ๋์์.
- ์ฃผ์ ๋ชฉํ๋ ๋ค์๊ณผ ๊ฐ์:
  - ํญ์ TLS ์์์ ๋์ -> ์ํธํ๋์ง ์์ ์ฐ๊ฒฐ์ ์ง์ํ์ง ์์
  - ์๋ต ๋ค์คํ (Multiplexing) -> ์ฌ๋ฌ request๋ฅผ ๋์์ ๋ณด๋ผ ์ ์์
  - ํค๋ ์์ถ์ ํตํ ํ๋กํ ์ฝ ์ค๋ฒํค๋ ์ต์ํ
  - request ๋ณ ์ฐ์ ์์ ์ง์  ๊ฐ๋ฅ -> ์ค์ํ request์ ๋ ๋ง์ ๋ฆฌ์์ค๋ฅผ ํ ๋นํ  ์ ์์
  - ์๋ฒ ํธ์ -> ํด๋ผ์ด์ธํธ๊ฐ ์์ฒญํ์ง ์์ ๋ฆฌ์์ค๋ฅผ ๋ฏธ๋ฆฌ ๋ณด๋ผ ์ ์์
  - ๊ธฐ์กด ์ดํ๋ฆฌ์ผ์ด์์ ์์  ์๋ ์ง์ -> HTTP/1.1๊ณผ ํธํ๋จ

## **๐ค ์ HTTP/1.2๊ฐ ์๋ HTTP/2.0์ธ๊ฐ?**

![http1.1_vs_http2](https://user-images.githubusercontent.com/39883609/202841152-125b9c54-a257-49c2-9c1e-341409646940.png)

- Text ๊ธฐ๋ฐ์ ํต์ ์ด ์๋ Binary framing ๊ณ์ธต์ ๋์ํจ.
- ํด๋น binary frame์ ์ฌ๋์ด ์ฝ์ ์ ์๋ ๋ฐ์ดํฐ์.

![http2_frame](https://cheapsslsecurity.com/p/wp-content/uploads/2019/07/binary-framing-layer-1024x529.png)

- HTTP/1.1๊ณผ ๋ฌ๋ฆฌ HTTP/2๋ ๋ ์์ ๋ฉ์์ง์ ํ๋ ์์ผ๋ก ๋ถํ ๋๋ฉฐ ๋ฐ์ด๋๋ฆฌ ํ์์ผ๋ก ์ธ์ฝ๋ฉ๋จ.
- ํด๋ผ์ด์ธํธ์ ์๋ฒ๋ ์ ๋ฐ์ด๋๋ฆฌ ์ธ์ฝ๋ฉ ๋ฉ์ปค๋์ฆ์ ์ฌ์ฉํด์ผ ํจ.

## **๐ HTTP/2.0์ ํน์ง**

### **Stream, Message, Frame**

![http2_stream](https://user-images.githubusercontent.com/39883609/202897928-1497adb2-cb78-49c4-b0b3-b6dc208cbb2e.png)

- Stream: ํ๋์ TCP ์ฐ๊ฒฐ ๋ด์์ ์ ๋ฌ๋๋ ๋ฐ์ดํธ์ ํ๋ฆ, ํ๋ ์ด์์ Message๊ฐ ์ ๋ฌ๋  ์ ์์.
- Message: Request ๋๋ Response์ ๋งค์นญ๋๋ ํ๋ ์์ ์ ์ฒด ์ํ์ค์.
- Frame: HTTP/2.0์์ ์ ๋ฌ๋๋ ์ต์ ๋จ์์ ๋ฐ์ดํฐ ๋ธ๋ก์. ๊ฐ๊ฐ์ ํ๋ ์์๋ ํ๋์ ํ๋ ์ ํค๋๊ฐ ํฌํจ๋จ.

### **Multiplexing**

- HTTP/1.1์์๋ ํ๋์ TCP ์ฐ๊ฒฐ์ ํ๋์ request๋ง์ ๋ณด๋ผ ์ ์์์.
- ์ฑ๋ฅ ๊ฐ์ ์ ์ํด ๋ณ๋ ฌ ์์ฒญ์ ์ํํ๋ ค๋ ๊ฒฝ์ฐ ์ฌ๋ฌ ๊ฐ์ TCP ์ฐ๊ฒฐ์ด ์ฌ์ฉ๋์ด์ผ ํจ.

![HTTP2_connection](https://user-images.githubusercontent.com/39883609/202898170-412fe061-a7be-473a-959e-e7f90006b865.png)

- HTTP/2.0์์๋ ํ๋์ TCP ์ฐ๊ฒฐ ๋ด์์ ์ฌ๋ฌ ๊ฐ์ request๋ฅผ ๋์์ ๋ณด๋ผ ์ ์์.
- ํด๋ผ์ด์ธํธ์ ์๋ฒ ๋ชจ๋ HTTP ๋ฉ์ธ์ง๋ฅผ ๋๋ฆฝ๋ ํ๋ ์์ผ๋ก ์ธ๋ถํํ๊ณ , ์ด ํ๋ ์๋ค์ ์ธํฐ๋ฆฌ๋นํ ๋ค์, ๋ค๋ฅธ ์ชฝ์์ ๋ค์ ์กฐ๋ฆฝํจ.

![HTTP_mux](https://user-images.githubusercontent.com/39883609/202898232-1faf6206-9563-4755-aed4-f4a2ad062ea2.png)

- ๋ค์์ Request์ Response๋ค์ ๋ณ๋ ฌ๋ก ์ธํฐ๋ฆฌ๋น.
- ๋จ์ผ TCP ์ฐ๊ฒฐ์์ ๋ณ๋ ฌ ์ ๋ฌ.
- ๋ถํ์ํ ์ง์ฐ ์๊ฐ์ ์ ๊ฑฐํ๊ณ  ์ฑ๋ฅ์ ํฅ์์ํด.

### **Stream Prioritization**

- HTTP/2.0์์๋ ํด๋ผ์ด์ธํธ๊ฐ ์๋ฒ์๊ฒ ์ด๋ค ์์ฒญ์ด ๋ ์ค์ํ์ง ์๋ ค์ค ์ ์์.
- ๊ฐ๊ฐ์ ์คํธ๋ฆผ์ด ๊ฐ์ค์น์ ์ข์์ฑ์ ๊ฐ์ง๊ณ  ์์.
  - ๊ฐ์ค์น: ์ค์ํ ์ ๋, 1~256 ์ฌ์ด์ ์ ์
  - ์ข์์ฑ: ์ํ ๊ด๊ณ, ๋ค๋ฅธ ์คํธ๋ฆผ์ ๋ํ ๋ช์์  ์ข์์ฑ ๋ถ์ฌ
- ์คํธ๋ฆผ์ ์ข์์ฑ ๋ฐ ๊ฐ์ค์น ์กฐํฉ์ ์ด์ฉํ์ฌ ํด๋ผ์ด์ธํธ๊ฐ `์ฐ์ ์์ ์ง์  ํธ๋ฆฌ`๋ฅผ ๊ตฌ์ฑํ  ์ ์์.

![Stream_prioritization](https://sunjoong85.github.io/assets/http2/h2_stream_prioritization.png)

- ๋์ผํ ์์ ์์๋ฅผ ๊ฐ์ง ์คํธ๋ฆผ์ ๊ฐ์ค์น์ ๋น๋กํ์ฌ ๋ฆฌ์์ค๊ฐ ํ ๋น๋จ.

> e.g. CloudFlare์ HTTP/2.0 ์ฐ์ ์์ ์ง์ : https://blog.cloudflare.com/better-http-2-prioritization-for-a-faster-web/

### **Server Push**

- HTTP/1.1์์ ๋ถ๊ฐ๋ฅํ ๊ธฐ๋ฅ์ผ๋ก, ์๋ฒ๊ฐ ํด๋ผ์ด์ธํธ์ Request์ Response๋ฅผ ๋ณด๋ผ ๋ฟ๋ง ์๋๋ผ ํด๋ผ์ด์ธํธ๊ฐ ๋ช์์ ์ผ๋ก ์์ฒญํ์ง ์์๋ ์๋ฒ๊ฐ ์ถ๊ฐ์ ์ธ ๋ฆฌ์์ค๋ฅผ ํด๋ผ์ด์ธํธ์ ๋ณด๋ผ ์ ์์.

![Server_push](https://web-dev.imgix.net/image/C47gYyWYVMMhDmtYSLOWazuyePF2/o6lgFK9DiyJmdWwyJnyy.svg)

- `push promise`: ํด๋ผ์ด์ธํธ๊ฐ ์์ฒญํ์ง ์์์ง๋ง ํ์ํ  ๊ฒ์ด๋ผ๊ณ  ์๊ฐํ์ฌ ์๋ฒ์์ ํธ์ํจ.
- ์๋ฒ ํธ์ ๋์ ๋ฐฉ์
  - `PUSH_PROMISE`๋ฅผ ํตํด ์๋ฒ์์ ํธ์ํ๊ฒ ๋ค๊ณ  ์๋ ค์ค.
  - ํด๋ผ์ด์ธํธ๋ `PUSH_PROMISE`๋ฐ์ ํด๋น ๋ฆฌ์์ค๊ฐ ํ์ํ์ง ํ์ํ์ง ์์์ง ํ๋จ.
  - `PUSH_PROMISE` ํ๋ ์์ HTTP header ์ ๋ณด๋ฅผ ๊ฐ์ง.
  - ํด๋ผ์ด์ธํธ๊ฐ `PUSH_PROMISE`๋ฅผ ๊ฑฐ์ ํ๋ ๊ฒฝ์ฐ `RST_STREAM` ํ๋ ์์ ๋ณด๋.
  - ์๋ฝํ๋ ๊ฒฝ์ฐ ํด๋ผ์ด์ธํธ์์ ๋์์ ๋ช๊ฐ๋ฅผ ๋ฐ์์ง, ํ๋ฆ ์ ์ด Window์ ํฌ๊ธฐ๋ฅผ ์ด๋ป๊ฒ ํ ์ง ์ค์  ๊ฐ๋ฅํจ.

### **Header Compression**

- Data Frame์ ์ด๋ฏธ ์์ถ๋์ด ์ ์ก๋๋ฏ๋ก ์์ถ์ด ๋ถํ์ํจ.
- HTTP/2.0์์  HPACK ์๊ณ ๋ฆฌ์ฆ์ ์ฌ์ฉํ์ฌ Header Frame์ ์์ถํจ.

![header_compression](https://web-dev.imgix.net/image/C47gYyWYVMMhDmtYSLOWazuyePF2/IYfczfC6ZCTxUVboaEZy.svg)

- ํํ๋ง ์ฝ๋๋ฅผ ํตํด ์ธ์ฝ๋ฉํ์ฌ ๊ฐ๋ณ ์ ์ก ์๋๋ฅผ ์ค์.
- ํ๋์ TCP ์์์ ์คํธ๋ฆผ์ ํตํด ํต์ ํ๊ธฐ ๋๋ฌธ์ ์ด์ ์ ์ ์กํ header ์ ๋ณด๋ฅผ ์๊ณ  ์์.
- ์ด์  ์ ๋ณด์ ๋ฌ๋ผ์ง ์ ๋ณด๋ง ๋ค์ ๋ฉ์ธ์ง์ ํค๋์์ ๋ณด๋.

## **๐ฅ๏ธ HTTP/2.0 ์๋ฒ ๊ตฌํ**

### **TLS ์ธ์ฆ์ ์ํ ์ธ์ฆ์ ๋ฐ๊ธ ๋ฐ CA ์์ฑ**

```bash
$ brew install mkcert
$ mkcert -install
$ mkcert localhost
```

![cert_result](https://user-images.githubusercontent.com/39883609/202900196-d5882223-e540-4dec-92dc-a5112f38126b.png)

### **HTTP/2.0 ์๋ฒ ๊ตฌํ**

- go์ http ๋ชจ๋์์ ๊ธฐ๋ณธ์ ์ผ๋ก HTTP/2.0 ํ๋กํ ์ฝ์ ์ง์ํจ.
- ๊ฐ๋จํ `form`๊ณผ ์ด๋ฏธ์ง๋ค์ ๋ณด์ฌ์ฃผ๋ ํํ๋ฆฟ ํ์ด์ง ๊ตฌํ.
- ๋๋ ํ ๋ฆฌ ๊ตฌ์กฐ

```text
.
โโโ go.mod
โโโ http2.go
โโโ index.html
โโโ localhost-key.pem
โโโ localhost.pem
โโโ static
    โโโ css
    โย ย  โโโ gallery.css
    โย ย  โโโ main.css
    โย ย  โโโ normalizeLogin.css
    โย ย  โโโ styleLogin.css
    โโโ image
    โย ย  โโโ img_1.png
    โย ย  โโโ img_10.png
    โย ย  โโโ img_11.png
    โย ย  โโโ img_12.png
    โย ย  โโโ img_13.png
    โย ย  โโโ img_14.png
    โย ย  โโโ img_15.png
    โย ย  โโโ img_16.png
    โย ย  โโโ img_17.png
    โย ย  โโโ img_18.png
    โย ย  โโโ img_19.png
    โย ย  โโโ img_2.png
    โย ย  โโโ img_20.png
    โย ย  โโโ img_21.png
    โย ย  โโโ img_22.png
    โย ย  โโโ img_23.png
    โย ย  โโโ img_24.png
    โย ย  โโโ img_25.png
    โย ย  โโโ img_26.png
    โย ย  โโโ img_27.png
    โย ย  โโโ img_28.png
    โย ย  โโโ img_29.png
    โย ย  โโโ img_3.png
    โย ย  โโโ img_30.png
    โย ย  โโโ img_31.png
    โย ย  โโโ img_32.png
    โย ย  โโโ img_33.png
    โย ย  โโโ img_34.png
    โย ย  โโโ img_35.png
    โย ย  โโโ img_36.png
    โย ย  โโโ img_37.png
    โย ย  โโโ img_38.png
    โย ย  โโโ img_39.png
    โย ย  โโโ img_4.png
    โย ย  โโโ img_40.png
    โย ย  โโโ img_5.png
    โย ย  โโโ img_6.png
    โย ย  โโโ img_7.png
    โย ย  โโโ img_8.png
    โย ย  โโโ img_9.png
    โโโ js
        โโโ login.js

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

### **๊ฒฐ๊ณผ**

![http2_server](https://user-images.githubusercontent.com/39883609/202904736-1c310088-045d-411d-80a9-5b52c838d42c.png)

- h2 ํ๋กํ ์ฝ๋ก ์ ์ก๋ ๊ฒ์ ๋ณผ ์ ์์.
- ํด๋น ์์ฒญ๋ค์ด `HTTP/2.0` ํ๋กํ ์ฝ๋ก ์ ์ก๋์๊ธฐ ๋๋ฌธ์, `HTTP/1.1` ํ๋กํ ์ฝ๋ก ์ ์ก๋๋ ์์ฒญ๋ค๊ณผ๋ ๋ฌ๋ฆฌ, `HTTP/2.0` ํ๋กํ ์ฝ์์๋ `Multiplexing`์ด ๊ฐ๋ฅํจ.

### **HTTP/1.1 ์๋ฒ์ ๋น๊ต**

![http1_server](https://user-images.githubusercontent.com/39883609/202904865-71af4807-0e92-43a8-89c4-feab4e2ccd64.png)

- http1.1 ํ๋กํ ์ฝ๋ก ์ ์ก๋ ๊ฒ์ ๋ณผ ์ ์์.
- `HTTP/1.1` ํ๋กํ ์ฝ์์๋ `Multiplexing`์ด ๋ถ๊ฐ๋ฅํ๊ธฐ ๋๋ฌธ์, ์์ฒญ์ด ๋ง์์ง๋ฉด, `Connection`์ด ๋ง์์ง๊ณ , `Connection`์ด ๋ง์์ง๋ฉด, `Latency`๊ฐ ์ฆ๊ฐํจ.

### **Removing Server Push from Chrome**

![Server_push_error](https://user-images.githubusercontent.com/39883609/202904991-cfa77cb7-1085-4bd9-be39-585f945f6295.png)

- Chrome 106 ๋ฒ์ ๋ถํฐ Chrome ๋ฐ Chromium ๊ธฐ๋ฐ ๋ธ๋ผ์ฐ์ ์์๋ `Server Push`๋ฅผ ์ง์ํ์ง ์์.
- ๋ณธ๋ ๊ธฐํ๋ ์๋์๋ ๋ค๋ฅด๊ฒ ์ ์ฌ์ฉ๋์ง ์์์ผ๋ฉฐ, ์ฑ๋ฅ ์ ํ๋ฅผ ์ ๋ฐํ๋ ์์ธ์ด ๋์๊ธฐ ๋๋ฌธ.
- Server Push์ ๋์์ผ๋ก๋ `103 Early Hints`๊ฐ ์์.

> https://developer.chrome.com/blog/removing-push/

## HTTP/1.1 vs HTTP/2.0

![http1_vs_http2](https://assets.website-files.com/5ff66329429d880392f6cba2/6149cbd7fd4bdd7c82f55cc6_http1%20vs%20http2.png)

![http1_vs_http2_2](https://assets.website-files.com/5ff66329429d880392f6cba2/6149cc01573487fea0af8519_http1%20vs%20http2%20vs%20http3.png)