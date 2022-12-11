# 04. 결과를 전달하는 HTTP 상태 코드

## 1. 상태 코드는 서버로부터 리퀘스트 결과를 전달한다 
1. 클라이언트가 서버로 요청을 보냄
2. 서버는 요청에 대한 응답을 내려줌과 동시에, 상태 코드도 함께 준다. (세자리 숫자임)

### 상태 코드는 아래와 같이 서버에서 지정해서 내려줄 수 있다.
![img.png](img.png)
- 에러 났는데 200OK를 강제로 내려줄 수도 있다. (서버개발자 마음임)


![img_1.png](img_1.png)
- 위와 같이 500 Internal Server Error인데 초록불 뜨게 할수도 있음.



### 상태 코드는 크게 5가지가 있다.
- 1XX : Informational | 요청이 수신되어 처리중 -> 잘안씀
- 2XX : Successful    | 요청을 정상적으로 처라함
- 3XX : Redirection   | 요청을 완료하려면 추가 행동이 필요함 
- 4XX : Client Error  | 서버가 요청을 이해하지 못함
- 5XX : Server Error  | 서버가 요청 처리를 실패함 

---
## 2. 2XX 성공
- 요청이 정상으로 처리되었음
- 2XX와 함께 돌아오는 정보는 메소드에 따라 다름
  - GET -> 요청된 리소스에 대한 엔티티가 보내짐  
  - HEAD -> 요청된 리소스에 대응하는 엔티티 헤더 필드만 돌아옴

### 204 No Content
#### [mdn web docs](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/204)
> - 요청이 성공했지만 클라이언트가 현재 페이지에서 이동할 필요가 없음
> - 예시
>   - 위키 사이트에 대해 "저장하고 계속 편집"기능을 구현할 때 사용할 수 있음
>   - 해당 경우, PUT 요청을 보내 페이지를 저장하고, 204 No Content 응답을 보내 editor가 다른 페이지로 대체되어서는 안됨을 나타냄 

#### 책 
- 요청 처리는 성공 + 응답에 엔티티 바디를 포함하지 않음(돌려줄 리소스가 없기 때문)
- 서버에 정보를 보내는 것으로 만족할 때, 클라이언트에서 새로운 정보를 보낼 필요가 없을 때 사용

#### 갓영한 강의
- save 버튼의 결과로 아무 내용이 없어도 됨
- save 버튼을 눌러도 같은 화면을 유지해야 함
- 결과 내용이 없어도 204만으로 성공 인식 가능 


### 206 Partial Content 
#### [mdn web docs](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/206)
```text
[Single range]
HTTP/1.1 206 Partial Content
Date: Wed, 15 Nov 2015 06:25:24 GMT
Last-Modified: Wed, 15 Nov 2015 04:58:08 GMT
Content-Range: bytes 21010-47021/47022
Content-Length: 26012
Content-Type: image/gif

# 26012 bytes of partial image data…

[Several Range]
HTTP/1.1 206 Partial Content
Date: Wed, 15 Nov 2015 06:25:24 GMT
Last-Modified: Wed, 15 Nov 2015 04:58:08 GMT
Content-Length: 1741
Content-Type: multipart/byteranges; boundary=String_separator

--String_separator
Content-Type: application/pdf
Content-Range: bytes 234-639/8000

# the first range
--String_separator
Content-Type: application/pdf
Content-Range: bytes 4590-7999/8000

# the second range
--String_separator--
```
> - 요청 헤더의 Range 헤더에 설명된 대로, 요청이 성공했고 본문에 요청된 데이터 범위가 포함되었음을 나타냄
> - 범위가 하나인 경우
>   - Content-Type이 document유형으로 설정되고, Content-Range가 제공됨
> - 범위가 다양한 경우
>   - Content-Type이 multipart/byteranges로 설정되고, 각 fragment들은 Content-Range와 Content-Type이 요약된 범위를 포함 

#### 책
- Range에 의해 범위가 지정된 리퀘스트에 의해 서버가 부분적 GET 리퀘스트를 받았음을 나타냄
- 응답에 Content-Range로 지정된 범위의 엔티티가 포함됨 


---

## 3XX 리다이렉트
- 응답이 정상적으로 처리를 종료하기 위해 브라우처 측에서 특별한 처리를 수행해야 함을 나타냄

### 301 Move Permanently
#### [mdn web docs](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/301)
```text
[Client]
GET /index.php HTTP/1.1
Host: www.example.org

[Server]
HTTP/1.1 301 Moved Permanently
Location: http://www.example.org/index.asp
```
> - 요청된 리소스가 Location 헤더에 지정된 URL로 최종적으로 이동되었음 
> - 브라우저는 새 URL로 redirection되고, 검색 엔진은 리소스에 대한 링크를 업데이트함 
> - 301 응답은 "메서드 변경이 명시적으로 금지"됨
>   - GET/HEAD 메서드에 대한 응답으로만 사용해야 함
>   - 만약 POST 메서드에 해당 상태가 필요한 경우 308 Permanet Redirect를 사용할 것


#### 책
- 요청된 리소스가 새로운 URI에 부여되어 있음
- 그 리소스를 참조하는 URI를 사용해야 한다는 것을 나타냄
- 301이 발생하는 상황 : 아래와 같이 마지막 부분에 `/`를 붙이는 것을 잊는 경우 등 
  - `http://example.com/sample` 


#### 갓영한 강의 
- 301, 308은 리소스의 URL이 영구 이동했을 때 사용한다.
- 원래 URL을 사용하면 안된다.
- 301은 **아마도(May)** 리다이렉트 시 본문이 제거될 가능성이 있다.
  - 메시지 바디가 사라질 수 있기 때문에 처음부터 다시 입력해야 하는 등 새로운 이벤트 페이지에 대한 입력이 나올 수 있음
- 308은 301과 기능은 같지만, 본문이 제거되는 문제가 해결됨 
  - 리다이렉트 시에도 요청 메서드와 본문을 유지함 


### 302 Found
#### [mdn web docs](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/302)
> - 요청된 리소스가 일시적으로 Location 헤더에 지정된 URL로 이동되었음
> - 브라우저는 이 페이지로 리디렉션 되지만, **검색 엔진은 리소스에 대한 링크를 업데이트 하지 않음**
> - 리디렉션이 수행될 때, 메서드가 변경되지 않도록 요구하더라도 모든 사용자 에이전트가 이를 준수하지 않음
> - 메서드 변경이 명시적으로 금지됨
>   - GET 또는 HEAD 메서드에 대한 응답으로만 설정해야 함
>   - 변경되어야 하는 경우 307 Temporary Redirect를 사용할 것
>   - 만약, 메서드가 다른것에서 GET으로 변경되어야 하는 경우 303 See Other를 사용할 것 
>     - 이런 경우 : PUT 방식에 대해 'XYZ'를 성공적으로 업로드 했다와 같은 확인 메시지로 응답하고 싶을 때 

#### 책
- 해당 응답은, 요청된 리소스에 대해 새로운 URI가 할당되어 있기 때문, 그 URI를 참조해주길 바란다는 의미를 가짐
- 302는 어디까지나 **일시적인 이동**이다.
  - 이동하는 곳의 URI가 변경 될 가능성이 있음 


#### 갓영한 강의
- 302, 307, 303은 **일시적으로 URL이 변경되었을 때 사용**한다 
- 검색엔진 등에서 URL을 변경하면 안된다. (일시적이기 때문)
- 302 Found
  - 불명확함 
  - 리다이렉트 시, 요청 메서드가 GET으로 변하고 본문이 제거될 수 있음 (may be..)
- 307 Temporary Redirect
  - 리다이렉트 시, **요청 메서드와 본문이 유지**됨. 요청 메서드 변경 불가 (must not)
- 303 SeeOther
  - 리다이렉트 시, 요청 메서드가 **확실하게** GET으로 변경됨
> PRG pattern
> - POST -> REDIRECT -> GET 패턴
> - 요청 후 새로고침하면 요청했던 메소드가 계속 전송됨
>   - POST가 마지막인 경우, 계속 POST됨. 중복 글 양산 굿;
> - 그러므로 POST 후에는 Redriect를 이용해 GET 메소드로 다시 요청하게 하자. 이게 PRG!

그래서 302, 307, 303중에 뭐를 써야되는가? 에 대한 답을 주셨다.
- 302 : GET으로 변할 수 있음. 스펙 의도는 HTTP 메서드를 유지하는 것이나, 웹 브라우저들이 대부분 그냥 GET으로 바꿔버림 
  - 모호한 302 대신 명확한 307, 303 등장 
- 307 : 메서드가 변하면 안됨
- 303 : 메서드가 GET으로 변경
> *현실적으로...*
> - 307, 303을 권장하지만, 이미 많은 앱들이 302를 기본적으로 쓰는 중 
> - 그래서 우리 웹브라우저에 Redirection(노란불)뜨면 거의 다 302인거임 
> - 자동 리디렉션 시, GET으로 변해도 된다면 302를 사용해도 별 탈 없음 


### 303 See Other
302 설명 시 대부분이 설명되었기 때문에, 책 내용만 담았다.
- 해당 응답도 리디렉션이다.
- 요청에 대한 리소스가 다른 URI에 있기 때문에, GET 메소드를 사용해서 얻어야 된다는 뜻을 내포하고 있다.
- 무조건 리디렉션 방법으로 GET을 쓴다.
> 책 부연 설명
> - 301, 302, 303 리스폰스 코드가 되돌아 오면 대부분의 브라우저에서는 POST를 GET으로 바꾼다.
> - 바꾼 후, 요청의 엔티티 바디를 삭제하고, 리퀘스트를 자동적으로 재송신 하도록 되어있다.
> - 301, 302의 사양은 메소드 변경을 금지하고 있지만, 대부분 다 이렇게 되어있다.


### 304 Not Modified
#### [mdn web docs](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/304)
> - 요청된 리소스를 재전송할 필요가 없음을 의미 
> - 캐시된 리소스에 대한 암시적인 리디렉션 
> - 요청 방법이 GET 또는 HEAD와 같은 안전한 방법이거나, 요청이 조건부이고 If-None-Match 또는 If-Modified-Since 헤더를 사용하는 경우 발생

-> 어떤 예제를 살펴봤는데, 304 Not Modified 응답이 올 때, 브라우저의 캐시를 전부 지우고 재요청하면 200OK가 날아온다고 함 

#### 책
- 클라이언트가 조건부 리퀘스트를 했을 때 리소스에 대한 액세스는 허락하지만, 조건이 충족되지 않음을 표시
- 304를 되돌려 줄 때는 응답 바디에 아무것도 없어야함 
- 304는 3xx에 분류는 되어있으나, 리디렉션과 관계가 없음  

-> 솔직히 무슨소리인지 모르겠음

#### 갓영한 강의
- 캐시를 목적으로 사용
- 클라이언트에게 리소스가 수정되지 않았음을 알려줌
  - 클라이언트는 로컬 PC에 저장된 캐시를 재사용하여, 캐시로 리디렉션을 한다
- 로컬 캐시를 사용해야 하기 때문에 응답에 메세지 바디가 포함되면 안된다 
- 조건부 GET, HEAD 요청 시 사용한다 


### 307 Temporary Redirect
이 또한 위에 전부 정리가 되어버렸기 때문에, 책 내용만 간단히 정리하였음 
- 302는 POST -> GET 치환 금지되어있음에도 불구하고, 치환 많이 함 
- 307은 명시적으로, 브라우저 사양에 따라 POST -> GET 치환을 하지 않는다.
- 단지, 브라우저마다 응답이 달라질 수 있다. -> 쓰면 안되는거 아닌지.... 

---

## 4XX 클라이언트 에러
- 클라이언트의 원인으로 에러가 발생하였음 
- 클라이언트가 이미 잘못된 요청과 데이터를 보내는 중 -> 똑같은 재시도는 똑같이 실패함

### 400 Bad Request
#### [mdn web docs](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/400)
> - 서버가 클라이언트 에러로 인해 요청을 처리할 수 없거나, 처리하지 않을 것임을 의미 
> - 예시
>   - 잘못된 요청 구문, 잘못된 요청 메세지, 사기성 요청 등 


#### 책
- 리퀘스트 구문이 잘못되었음을 나타냄 
- 리퀘스트 내용을 재검토하고 나서 재송신 할 필요가 있음 
- 브라우저는 이걸 200OK와 같이 취급함 


### 401 Unauthorized
#### [mdn web docs](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/401)
```text
HTTP/1.1 401 Unauthorized
Date: Wed, 21 Oct 2015 07:28:00 GMT
WWW-Authenticate: Basic realm="Access to staging site"
```
> - 요청된 리소스에 대한 유효한 인증 자격 증명이 없음 
> - 클라이언트 요청이 완료되지 않음 
> - 해당 상태 코드는 사용자에게 **인증 자격 증명**을 요청
> - 클라이언트가 리소스를 다시 요청할 수 있는 방법에 대한 정보가 포함된 **HTTP WWW-Authenticate**응답 헤더와 함께 전송됨 
> - 403과 비슷하나, 401이 발생하는 상황에서 사용자 인증이 리소스에 대한 액세스를 허용할 수 있다는 점이 다름


#### 책
- 송신한 리퀘스트에 HTTP 인증(BASIC 인증, DIGEST 인증...) 정보가 필요함을 나타냄 
  - HTTP 인증은 여러가지가 있다.
  - BASIC : 사용자 이름과 암호를 Base64로 인코딩된 문자열을 주고받는 인증 방식 -> 보안 약함. 요즘 이거 쓰면 다 뚫림 안씀.
  - DIGEST : 사용자명, 패스워드 등을 조합하여 MD5 값으로 인증 ... (알아보십시오.. 이것도 잘 안쓰는듯 합니다.)
  - 잘쓰는 인증 방식
    - Form base : 여러분이 사용하는 ID/PW 인증 방식
    - token 인증 방식 : 대표적인게 Bearer Token을 이용한 인증 방식(JWT). 이 또한 Authorization header로 주고받음. -> 추후 여러분을 괴롭힐 존재임 
- 최초 리스폰스 : 인증이 필요하여 로그인 창 등을 응답한다 
- 이후 리스폰스 : 인증에 실패했음을 의미함 
- 401 리스폰스 -> WWW-Authenticate 헤더 필드가 필요함을 의미 
- 여러분이 더 알면 좋은것 
  - 인증(Authentication) : 로그인 (본인이 누구인지 확인)
  - 인가(Authorization)  : 권한 부여 (등급 같은것. 특정 리소스에 접근할 수 있는 권한과 인증 방식)



### 403 Forbidden
#### [mdn web docs](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/403)
```text
HTTP/1.1 403 Forbidden
Date: Wed, 21 Oct 2015 07:28:00 GMT
```
> - 서버에 요청이 전달되었지만, 권한 때문에 거절됨 
> - 401과 비슷하나, 재인증을 한다 하더라도 권한이 없으면 지속적으로 접속을 거절함 


#### 책
- 리퀘스트된 리소스의 액세스가 거부되었음 
- 거부 이유가 명확할 경우, 엔티티 바디에 기재해서 유저측에 표시 
- 퍼미션이 부여되지 않은 경우, 액세스 권한 문제 등으로 발생 



### 404 Not Found 
#### [mdn web docs](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/404)
```text
[커스텀 에러 페이지]
ErrorDocument 404 /notfound.html
```
> - 요청받은 리소스를 찾을 수 없음 
> - 브로큰 링크 or 데드 링크라고 부름
> - 리소스가 일시적, 또는 영구적으로 사라졌다는 것을 의미하지 않음 
> - 영구히 삭제된 경우 410 Gone 을 사용할 것


#### 책
- 요청된 리소스가 서버상에 없음을 의미
- 그 외에도, 서버 측에 해당 요청을 거부하고 싶은 이유를 분명히 하고싶지 않은 경우(숨기고 싶을 경우)에도 이용할 수 있음 

---

## 5XX 서버 에러
- 서버 원인으로 에러가 발생하고 있음 (진짜 서버에 문제가 있을 때만 내야 함) 
- 서버에 문제가 있기 때문에, 재시도 하면 성공 할수도 있음 (복구가 되거나..)

### 500 Internal Server Error
#### [mdn web docs](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/500)
> - 요청을 처리하는 과정에서 서버가 예상하지 못한 상황에 놓임을 나타냄 
> - 서버 에러를 총칭하는, **구체적이지 않은** 응답임
> - 애매하면 500 오류 (by 갓영한)

#### 책
- 서버에서 요청을 처리하는 도중에 에러가 발생함 


### 503 Service Unavailable
#### [mdn web docs](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/503)
> - 서버가 요청을 처리할 준비가 되어있지 않음 
> - 유지 관리를 위해 다운되었거나 과부화된 서버인 경우를 나타냄
> - 해당 응답은 임시 조건에 사용해야 함
> - Retry-After HTTP 헤더에는 가능한 경우 서비스 복구 예상 시간이 포함되어야 함 
> - 예시 : 4대 명검... (긴급 점검, 임시 점검, 정기 점검, 연장 점검)


#### 책
- mdb web docs와 동일 

---

> 상태 코드는 현재 상황과 불일치 할 수도 있다.
> - 웹 애플리케이션에서 에러가 발생한 경우에도 상태 코드는 200이 올 수도 있음       