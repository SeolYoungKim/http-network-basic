# HTTP 정보는 HTTP 메시지에 있다.

<br/>

# 3.1 HTTP 메시지

<br/>

HTTP 메서지는 HTTP에서 **교환하는 정보**를 뜻한다.  
Request 측 HTTP 메시지를 `리퀘스트 메시지` Response 측 HTTP 메시지를 `리스폰스 메시지` 라고 한다.

<br/>

## HTTP 메시지 구성

- HTTP 메시지는 복수 행의 데이터로 구성된 텍스트 문자열이다.
- 크게 구분하면 `메시지 헤더`와 `메시지 바디`로 구성
- 개행 문자는``CR+LF`` 사용
- 메시지 바디가 항상 존재한다고는 할 수 없다.

<img width="400px" height="450px" src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/e809be3d-524c-4a65-8b3a-24f3ec8fea64/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221215%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221215T151838Z&X-Amz-Expires=86400&X-Amz-Signature=7e75c5f9c599aaf699d79d3c75f1f55fad7b24d5f7467880505e8e2479ef111e&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject" alt="img"/>

<br/>

### [메시지 헤더]

- 서버와 클라이언트가 꼭 처리해야 하는 리퀘스트와 리스폰스 내용과 속성 등

### [CR+LF]

- CR(Carriage Return) : 커서를 행의 제일 왼쪽으로 이동
- LF(Line Feed) : 커서를 현재 행의 다음행으로 이동, 즉 아래로 줄바꿈

### [메시지 바디]

- 꼭 전송되는 데이터 그 자체

<br/>
<br/>

# 3.2 리퀘스트 메시지와 리스폰스 메시지의 구조

<br/>

## 리퀘스트 메시지

- 리퀘스트 라인
- 리퀘스트 헤더 필드
- 일반 헤더 필드
- 엔티티헤더 필드
- 그외

<br/>

## 리스폰스 메시지

- 상태 라인
- 리스폰스 헤더 필드
- 일반 헤더 필드
- 엔티티헤더 필드
- 그외

<br/>

## 리퀘스트 라인

> 리퀘스트에 사용하는 메소드와 리퀘스트 URI와 사용하는 HTTP 버전이 포함

```http
GET /test.html HTTP/1.1
[HTTP Method] [Request target] [HTTP version]
```

<br/>

## 상태 라인

> 리스폰스 결과를 나타내는 상태 코드와 설명, 사용하는 HTTP 버전이 포함

```http
HTTP/1.1 200 OK
[HTTP version] [Status Code] [Status Text]
```

<br/>

## 헤더 필드

> 리퀘스트와 리스폰스의 여러 조건과 속성 등을 나타내는 각종 헤더 필드가 포함  
> `일반 헤더 필드`, `리퀘스트 헤더 필드`, `리스폰스 헤더 필드`, `엔티티 헤더 필드`

<br/>

## 그 외

> HTTP의 RFC에는 없는 헤더 필드(쿠키 등)가 포함되는 경우가 있다.

```http
GET /home.html HTTP/1.1
Host: developer.mozilla.org
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.9; rv:50.0) Gecko/20100101 Firefox/50.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/ *;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://developer.mozilla.org/testpage.html
Connection: keep-alive
Upgrade-Insecure-Requests: 1
If-Modified-Since: Mon, 18 Jul 2016 02:36:04 GMT
If-None-Match: "c561c68d0ba92bbeb8b0fff2a9199f722e3a621a"
Cache-Control: max-age=0
https://gmlwjd9405.github.io/2019/01/28/http-header-types.html
```

```http
POST /myform.html HTTP/1.1
Host: developer.mozilla.org
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.9; rv:50.0) Gecko/20100101 Firefox/50.0
Content-Length: 128
https://gmlwjd9405.github.io/2019/01/28/http-header-types.html
```

<br/>
<br/>

# 3.3 인코딩으로 전송 효율을 높이다

<br/>

- HTTP로 데이터를 전송시 인코딩(변환)을 하면 전송 효율을 높일 수 있다.
- 다량의 엑세스를 효율 좋게 처리할 수 있다.
- 컴퓨터에서 인코딩 처리를 하기 때문에 CPU등의 리소스는 보다 많이 소비하게 된다.

<br/>
<br/>

## 3.3.1 메시지 바디와 엔티티 바디의 차이

<br/>

### 메시지(message)

HTTP 통신의 기본 단위로 옥텟 시퀀스(Octet sequence, 8비트)로 구성되고 통신을 통해 전송된다.

<br/>

### 엔티티(entity)

리퀘스트랑 리스폰스의 페이로드(payload)로 전송되는 정보로 엔티티 헤더 필드와 엔티티 바디로 구성된다.

<br/>

> HTTP 메시지 바디의 역할은 리퀘스트랑 리스폰스에 관한 엔티티 바디를 운반하는 일이다.  
> 기본적으로 메시지 바디와 엔티티 바디는 같지만 전송 코딩이 적용된 경우에는 엔티티 바디의 내용이 변하기 떄문에 메시지 바디와 달라진다.

<br/>
<br/>

## 3.3.2 압축해서 보내는 콘텐츠 코딩

<br/>

HTTP에는 전송할 때 압축해서 보낼 수 있는 `콘텐츠 코딩(Content Coding)`이라고 불리는 기능이 구현되어 있다.  
콘텐츠 코딩은 엔티티에 적용하는 인코딩을 가르키는데 엔티티 정보를 유지한 채로 압축한다.

- 서버 [인코딩] -> 클라이언트 [디코딩]

<br/>

### 콘텐츠 압축 방식

- gzip(GNU zip)
- compress(UNIX의 표준 압축)
- deflate(zlib)
- identity(인코딩 없음)

<br/>
<br/>

## 3.3.3 분해해서 보내는 청크 전송 코딩

<br/>

사이즈가 큰 데이터를 전송하는 경우에 모든 리소스의 전송이 완료되지 않으면 브라우저 표시되지 않았다.  
하지만 청크 전송 코딩을 사용하면 데이터를 분할하여 조금씩 표시할 수 있다.

<br/>

청크 전송 코딩은 엔티티 바디를 `청크(덩어리)`로 분해한다. 이때 다음 청크 사이즈를 16진수로 사용하여 단락을 표시, 엔티티 바디 끝에는 "0(CR+LF)"를 기록해 둔다.

<br/>

청크 전송 코딩된 엔티티 바디는 수신한 클라이언트 측에서 원래의 엔티티 바디로 디코딩 한다.

<br/>
<br/>

# 3.4 여러 데이터를 보내는 멀티파트

<br/>

MIME(Multipurpose Internet Mail Extensions : 다목적 인터넷 메일 확장 사양)는 메일로 텍스트, 영상, 이미지와 같은 여러 다른 데이터를 다루기 위한 기능을 사용한다.  
이와같이 여러 데이터를 수용하는 방법을 MIME의 확장 사양인 멀티파트(Multipart)라고 한다.

<br/>

멀티파트를 사용하면 하나의 메시지 바디 내부에 엔티티를 여러 개 포함시켜 보낼 수 있다.  
주로 이미지나 텍스트 파일 등을 업로드할 때 사용된다.

<br/>

## multipart/form-data

- Web 폼으로부터 파일 업로드에 사용된다.

```http
Content-Type: multipart/form-data; boundary=AaB03x

--AaB03x
Content-Deisposition: form-data; name="field1"

Joe Blow
--AaB03x
Content-Disposition: form-data; name="pics"; filename="fiel1.txt"
Content-Type: text/plain
... (file1.txt데이터) ...
--AaB03x--
```

<br/>

## multipart/byteranges

- 상태코드 206(Partial Content) 리스폰스 메시지가 복수 범위의 내용을 포함하는 때에 사용된다.

```http
HTTP/1.1 206 Partial Content
Date: Fri, 13 Jul 2012 02:45:26 GMT
Last-Modified: Fri, 31 Aug 2007 02:02:20 GMT
Content-Type: multipart/byteranges; boundary=THIS_STRING_SEPARATES

--THIS_STRING_SEPARATES
Content-Type: application/pdf
Content-Range: bytes 500-999/8000

...(지정한 범위의 데이터)...
--THIS_STRING_SEPARATES
Content-Type: application/pdf
Content-Range: bytes 7000-7999/8000
--THIS_STRING_SEPARATES--
```

<br/>

HTTP 메시지로 멀티파트를 사용할 때에는 Content-type 헤더 필드를 사용한다.
멀티파트에서 각각의 엔티티를 구분하기 위해 `boundary` 문자열을 사용한다.
각 엔티티 앞에는 `--` 뒤에 `boundary` 문자열을 붙이고, 멀티파트의 마지막에는 `boundary` 앞뒤로 `--` 를 붙여 마무리 한다.

<br/>

> 멀티파트는 파트마다 헤더 필드가 포함되는데 자세한 내용은 RFC2046을 참고하자.

<br/>
<br/>

# 3.5 일부분만 받는 레인지 리퀘스트

<br/>

레인지 리퀘스트(Range Request)는 엔티티의 범위를 지정해서 다운로드를 할 수 있다.  
이러한 기능을 리줌(resume)이라고 한다.

```http
GET /tip.jpg HTTP/1.1
Host: www.usagidesign.jp
Range: bytes=5001-10000
```

위와같이 레인지 리퀘스트를 사용하면 전체 10000바이트 데이터 중 5001 ~ 10000 바이트 범위의 데이터만 받을 수 있다.

<br/>

## Range 헤더 필드

<br/>

- 5001 ~ 10000 바이트

```http
Range: byte = 5001-10000
```

- 5001 바이트 이상

```http
Range: byte = 5001-
```

- 3000 바이트까지, 그리고 5000~7000 바이트까지의 복수 범위

```http
Range: byte = -3000, 5000-7000
```

리스폰스상태의 코드는 `206 Partial Content`, 복수범위에 대해서는 `content-type: multipart/byteranges` 리스폰스가 돌아온다.

> 만약 서버가 레인지 리퀘스트를 지원하지 않는다면 `200 OK` 상태코드와 완전한 엔티티가 반환된다.

<br/>
<br/>

# 3.6 최적의 콘텐츠를 돌려주는 콘텐츠 네고시에이션

<br/>

내용은 같지만 영어판과 한국어판과 같이 표시되는 언어가 다른 경우 같은 URI로 접속 시 브라우저가 선호하는 언어의 웹 페이지를 표시한다.  
이러한 구조를 `콘텐츠 네고시에이션(Content Negotiation)`이라고 한다.

<br/>

> 콘텐츠 네고시에이션은 클라이언트와 서버가 제공하는 리소스의 내용에 대해서 협상 하는 것이다.  
> 제공하는 리소스를 언어와 문자 세트, 인코딩 방식 등을 기준으로 판단한다.  
> 판단 기준은 리퀘스트 메시지에 포함된 다음과 같은 리퀘스트 헤더 필드이다.
> - Accept
> - Accept-Charset
> - Accept-Encoding
> - Accept-Language
> - Content-Language

<br/>

## 콘텐츠 네고시에이션의 종류

<br/>

### 서버 구동형 네고시에이션(Server-driven Negotiation)

- 서버 측에서 콘텐츠 네고시에이션을 하는 방식
- 서버 측에서 리퀘스트 헤더 필드의 정보를 참고해서 자동적으로 처리

<br/>

### 에이전트 구동형 네고시에이션(Agent-driven Negotiation)

- 클라이언트 측에서 콘텐츠 네고시에이션을 하는 방식
- 브라우저에 표시된 선택지 중에서 유저가 수동으로 선택

<br/>

### 트랜스페어런트 네고시에이션(Transparent Negotiation)

- 서버 구동형과 에이전트 구동형을 혼합한 형태
- 서버와 클라이언트가 각각 콘텐츠 네고시에이션을 하는 방식
