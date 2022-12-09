## 2.1 Http는 서버와 클라이언트 간에 통신을 한다.

클라이언트는 서버에게 리소스를 요청하고 서버는 클라이언트에게 리소스를 제공한다.

## 2.2 리퀘스트와 리스폰스를 교환하여 성립

클라이언트가 Request를 보내면 서버는 Response를 보낸다.

### 💬 예시

클라이언트 측) 리퀘스트 송신
```rest
GET /index.html HTTP/1.1
Host: www.youngjin.com
```
서버 측) 리스폰스 송신
```
HTTP/1.1 200 OK
Date: Tue, 10 Jul 2012 06:50:15 GMT
Content-Length: 362
Content-Type: text/html
<html>
....
```

### 🧩 리퀘스트 메시지 구성

<img style="background-color:white" width="300" src="https://user-images.githubusercontent.com/7973448/206637000-4a9b520e-3bdd-46a6-9a9d-bc081a022227.png"/>

### 🧩 리스폰스 메시지 구성

<img style="background-color:white" width="300" src="https://user-images.githubusercontent.com/7973448/206638938-d0fade72-473d-4f68-95cb-44ab32512340.png">

## 2.3 HTTP는 상태를 유지하지 않는 프로토콜

HTTP는 stateless 프로토콜로, 리퀘스트와 리스폰스를 교환하면서 이전의 송/수신 정보는 기록하지 않습니다.  
그러나 로그인과 같은 경우에 상태를 유지해야 할 필요가 있어 쿠키(Cookie)라는 기술이 도입되었습니다.

## 2.4 리퀘스트 URI로 리소스를 식별

HTTP는 리퀘스트에 URI을 담아 리소스를 식별합니다.  
리퀘스트 URI를 지정하는 방법에는 여러 종류가 있습니다.

### 모든 URI를 리퀘스트 URI에 포함하는 방법.

```rest
GET http://shopping.naver.com/home HTTP/1.1
```

### Host 헤더 필드에 네트워크 로케이션을 포함하는 방법.

```rest
GET /home HTTP/1.1
Host: shopping.naver.com
```

### 서버가 자신에게 리퀘스트 보내기

서버가 자신에게 리퀘스트를 보낼 때는 리퀘스트 URI에 *를 지정할 수 있습니다.
```rest
OPTIONS * HTTP/1.1
```

## 2.5 서버에 임무를 부여하는 HTTP 메소드

### GET: 리소스 획득

리퀘스트 URI로 식별된 리소스를 가져올 수 있도록 요구합니다.  
아래와 같이 부트스트랩의 css를 받아올 수 있습니다.

#### 리퀘스트
```rest
GET http://maxcdn.bootstrapcdn.com/bootstrap/3.3.2/css/bootstrap.min.css HTTP/1.1
```

#### 리스폰스
```
HTTP/1.1 200 OK
Date: Fri, 09 Dec 2022 08:05:03 GMT
Content-Type: text/css; charset=utf-8
...

/*! * Bootstrap v3.3.2 (http://getbootstrap.com) * Copyright 2011-2015 Twitter, Inc. * Licensed under MIT (https://github.com/twbs/bootstrap/blob/master/LICENSE) */
/*! normalize.css v3.0.2 | MIT License | git.io/normalize */
html{
  font-family:sans-serif;
  -webkit-text-size-adjust:100%;
  -ms-text-size-adjust:100%
...
```

### POST: 엔티티 전송

POST 메서드는 엔티티를 전송하기 위해서 사용합니다.  
아래와 같이 전송합니다.

#### 리퀘스트

Content-Length는 데이터의 크기를 의미하며 바이트 단위입니다.

```rest
POST /submit.cgi HTTP/1.1
Host: www.hackr.jp
Content-Length: 1560

....
```

### PUT: 파일 전송

PUT 메소드는 파일(리소스)을 전송하기 위해서 사용합니다.  

*(추가) HTTP PUT 메서드는 요청 페이로드를 사용해 새로운 리소스를 생성하거나, 대상 리소스를 나타내는 데이터를 대체합니다. 따라서 "리소스 덮어쓰기" 라고 생각하면 되겠습니다.*

#### 리퀘스트

```rest
PUT /example.html HTTP/1.1
Host: www.hackr.jp
Content-type: text/html
Content-Length: 1560

...
```

#### 리스폰스

204 No Content는 리스폰스가 유의미한 바디를 가지고 있지 않기 때문입니다.

```
HTTP/1.1 204 No Content
...
```

### DELETE: 파일 삭제

DELETE 메소드는 파일(리소스)을 삭제하기 위해 사용됩니다.  
리퀘스트 URI로 지정된 리소스의 삭제를 요구합니다.

#### 리퀘스트

```rest
DELETE /example.html HTTP/1.1
Host: www.hackr.jp
```

#### 리스폰스

```
HTTP/1.1 204 No Content
...
```

### HEAD: 메시지 헤더 취득

HEAD 메소드는 GET과 비슷하지만 헤더만 돌려준다는 특징이 있습니다.  
URI유효성과 리소스 갱신 시간을 확인하는 목적 등으로 사용합니다.

리스폰스는 바디를 가져서는 안되며 가진다고 하더라도 무시해야 합니다.  
다만, Content-Length와 같은 헤더는 포함 될 수 있습니다.

#### 리퀘스트

```rest
HEAD http://maxcdn.bootstrapcdn.com/bootstrap/3.3.2/css/bootstrap.min.css HTTP/1.1
```

#### 리스폰스
```
HTTP/1.1 200 OK
Date: Fri, 09 Dec 2022 08:05:03 GMT
Content-Type: text/css; charset=utf-8
...
```

### OPTIONS: 제공하고 있는 메소드 문의

OPTIONS 메소드는 리퀘스트 URI로 지정한 리소스가 제공하고 있는 메소드를 알아보기 위해 사용합니다.

#### 리퀘스트
```rest
OPTIONS shopping.naver.com HTTP/1.1
```

#### 리스폰스

shoopping.naver.com가 제공하고 있는 메서드 들을 볼 수 있다.  
(GET, HEAD, POST, PUT, DELETE, TRACE, OPTIONS)

```
HTTP/1.1 200 
date: Fri, 09 Dec 2022 08:07:35 GMT
content-length: 0
allow: GET, HEAD, POST, PUT, DELETE, TRACE, OPTIONS
referrer-policy: unsafe-url
server: nfront
connection: close
```

### TRACE: 경로 조사

TRACE 메소드는 Web 서버에 접속한 뒤, 자신에게 통신을 되돌려 받는 루프백(loop-back)을 발생시킵니다.  

이 메소드는 원본(origin) 서버에 접속할때 까지 프록시를 얼마나 거치는지, 어떻게 리퀘스트가 가공되는지를 확인하기 위해서 사용합니다.

Max-Forwards라는 헤더필드를 넣어 보내는데, 원본 서버를 포함한 탐색할 프록시 서버 개수입니다.

**보안상의 이유로 거의 대부분 사용할 수 없습니다.**

#### 리퀘스트

```rest
TRACE / HTTP/1.1
Host: hackr.jp
Max-Forwards: 2
```

#### 리스폰스

리스폰스의 바디에는 리퀘스트 내용을 다시 되돌려줍니다.

```
HTTP/1.1 200 OK
Content-Type: message/http
Content-Length: 1024

TRACE / HTTP/1.1
Host: hackr.jp
Max-Forwards: 2
```

### (추가) PATCH: 파일 수정

*본문에는 없지만 중요한 메서드라고 생각하여 추가했습니다.*

PATCH 메서드는 파일(리소스)을 수정하기 위해 사용됩니다.  
PUT은 멱등성을 가지는데, PATCH는 멱등성을 가지지 않습니다.  
이는 동일한 PATCH 요청이 다른 결과로 이어질 수 있다는 것을 의미합니다.

#### 리퀘스트

*출처 : https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/PATCH*

```rest
PATCH /file.txt HTTP/1.1
Host: www.example.com
Content-Type: application/example
If-Match: "e0023aa4e"
Content-Length: 100

[description of changes]
```

#### 리스폰스

```
HTTP/1.1 204 No Content
...
```

### CONNECT: 프록시에 터널링 요구

CONNECT 메소드는 TCP 통신을 터널링 시키기 위해서 사용합니다.  
주로 SSL과 TLS 등의 프로토콜로 암호화 된 것을 터널링 시키기 위해서 사용됩니다.

*요청한 리소스에 대해 양방향 연결을 시작하는 메소드입니다. 이는 터널을 열기 위해서 사용될 수 있습니다.*

#### 리퀘스트

```rest
CONNECT proxy.hackr.jp:8080 HTTP/1.1
host: proxy.hackr.jp
```

#### 리스폰스

```
HTTP/1.1 200 OK
```

## 2.6 메소드를 사용해서 지시를 내리다

메소드는 지시를 내리기 위해서 사용합니다.  
메소드는 대문자와 소문자를 구별하기 때문에 대문자로 기재해야 합니다.

~~LINK와 UNLINK는 HTTP/1.1에서 폐지되었습니다.~~

## 2.7 지속 연결로 접속량을 절약

아래와 같이 하나의 HTML 문서에 여러 리소스가 포함되어 있으면 리퀘스트를 많이 보내게 됩니다.  
리퀘스트를 보낼 때 마다 매번 TCP 연결을 해야 해서 오버헤드가 크게 발생합니다.

<img width="664" alt="image" src="https://user-images.githubusercontent.com/7973448/206660038-2d7c3f42-8f68-42bd-b90b-551c376162b8.png">

### 2.7.1 지속 연결

HTTP/1.1에서는 TCP연결 문제를 해결하기 위해 지속 연결이란 방안을 고안했습니다.  
TCP 커넥션을 반복해서 사용하는 방식입니다.

### 2.7.2 파이프라인화

파이프라인화는 여러 리퀘스트를 병행해서 보낼 수 있게 합니다.  
이전에는 리퀘스트 송신 후에 리스폰스를 수신할 때까지 기다린 뒤에 리퀘스트를 발행할 수 있었는데, 파이프라인화에 의해서 바로 기다리지 않고 송신할 수 있습니다.

## 2.8 쿠키를 이용한 상태 관리

HTTP는 stateless 프로토콜이기 때문에, 과거에 교환했던 리퀘스트와 리스폰스의 상태를 저장하지 않습니다.  
하지만 상태관리가 필요한 웹 페이지(예: 인증)를 위해 쿠키라는 시스템이 도입되었습니다.

쿠키는 리스폰스에 Set-Cookie라는 헤더 필드로 클라이언트에게 보내지고 클라이언트는 이를 저장해서 다음번에 리퀘스트를 보낼 때, 쿠키 값을 넣어서 송신합니다.

### 리퀘스트(쿠키를 가지고 있지 않은 상태)
```rest
GET /reader/ HTTP/1.1
Host: www.youngjin.com
```

### 리스폰스(서버가 쿠키를 발행)
```
HTTP /1.1 200 OK
Date: Thu, 12 Jul 2012 07:12:20 GMT
Server: Apache
<Set-Cookie: sid=1342077140226724; path=/;expires=Wed, => 10-Oct-12 07:12:20 GMT>
Content-Type: text/plain; charset=UTF-8
```

### 리퀘스트(보관하고 있던 쿠키를 자동 송신)
```rest
GET /image/ HTTP/1.1
Cookie: sid=1342077140226724
```