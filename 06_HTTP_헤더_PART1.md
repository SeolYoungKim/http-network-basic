# 6장 - HTTP 헤더

날짜: 2022년 12월 23일

### HTTP 헤더 필드

- HTTP메시지를 구성하는 요소.
- request, response 양쪽에 모두 존재함.
- HTTP메시지에 대한 정보를 가지고 있음.
- [구조] 헤더필드명 : 필드값
  - 하나의 필드가 여러 개의 필드값 가질 수 있음.
  - 같은 헤더 필드명이 두 개 이상인 경우, 어떤 값이 우선인지는 브라우저 따라 다르게 동작함.

<br>
<br>

### HTTP 헤더 필드 4종류

(용도에 따라)

1. 일반적 헤더 필드

   : request메시지와 response메시지 모두에 사용되는 헤더.

2. 리퀘스트 헤더 필드

   : 클라이언트 → 서버로 송신된 request메시지에 사용되는 헤더.

3. 리스폰스 헤더 필드

   : 서버 → 클라이언트로 송신한 response메시지에 사용되는 헤더.

4. 엔티티 헤더 필드

   : request 메시지와 response메시지에 포함된 엔티티에 사용되는 헤더.

5. 그 외

   : 쿠키와 같은 비표준 헤더 필드.

<br>
<br>

### End-to-end헤더, Hob-by-hop 헤더

- 캐시와 비캐시 프록시의 동작을 정의하기 위해 두가지 카테고리로 분류됨.
- End-to-end헤더
  : request, response의 최종 수신자에게 전송됨.
  Hop-by-hop가 아닌 모든 헤더 필드가 포함됨.
- Hop-by-hop헤더
  : 한 번 전송에 대해서만 유효하고 캐시와 프록시에 의해 전송되지 않는 것도 있음.
  Connection, Keep-Alive, Proxy-Authenticate, Proxy-Authorization, Trailer, TE, Transfer-Encoding, Upgrade.

<br>
<br>

### 일반 헤더 필드

1. Cache-Control

   캐시의 동작 지정.

   여러개의 디렉티브를 지정하는 경우 콤마로 구분함.

   - 캐시 사용 가능 여부에 따라
     (1) Cache-Control : public
     다른 유저에게도 돌려줄 수 있는 캐시를 해도 좋다는 의미.
     (2) Cache-Control : private
     특정 유저만을 대상으로 하고 있다는 것을 의미.
     다른 유저로부터 같은 리퀘스트가 오더라도 해당 캐시를 반환하지 않음.
     (3) Cache-Control : no-cache
     캐시로부터 오래된 리소스가 반환되는 것을 막기 위해 사용됨.
     - 클라이언트 request에 no-cache : 캐시된 response를 받지 않음. 캐시 서버가 오리진 서버까지 request를 전송해야함.
     - 서버 response에 no-cache : 캐시 서버는 리소스를 저장할 수 없음. 이후의 request들은 모두 리소스의 유효성을 재확인해야함.
   - 캐시로 보존 가능한 것을 제어
     Cache-Control : no-store
     request, response에 기밀 정보가 포함되어 있을 수 있음.
     로컬 스토리지에 보존X.
   - 캐시 기한이나 검증을 지정
     (1) Cache-Control : s-maxage=604800 (초)
     여러 유저가 사용할 수 있는 공유 캐시 서버에만 적용됨.
     Expires와 max-age 무시.
     (2) Cache-Control : max-age=604800 (초)
     캐시 서버가 유효성을 재확인하지 않고 리소스를 캐시에 보존해두는 최대 시간.
     (3) Cache-Contol : min-fresh=60 (초)
     캐시된 리소스가 적어도 지정된 시간동안은 최신 상태의 것을 반환해야함.
     (4) Cache-Control : max-stale=3600 (초)
     캐시된 리소스의 유효기한이 끝났더라도 받아들일 수 있음.
     값이 지정되어 있지 않은 경우 : 시간이 얼마나 경과했더라도 response를 받아 들임.
     값이 지정되어있는 경우 : 유효기한이 지난 후로부터 지정 시간 내라면 받아 들임.
     (5) Cache-Control : only-if-cached
     목적한 리소스가 로컬 캐시에 있는 경우에만 response를 반환함.
     (6) Cache-Control : must-revalidate
     response의 캐시가 현재도 유효한지 여부를 오리진 서버에 조회함.
     max-stale 무시.
     (7) Cache-Control : proxy-revalidate
     모든 캐시 서버에 대해 반드시 유효성 재확인하도록 요구함.
     (8) Cache-Control : no-transform
     엔티티바디의 미디어 타입을 변경하지 않도록 함.
     이미지가 압축되는 것 방지.

   1. Connection

      Connection : 더 이상 전송하지 않는 헤더 필드 명.

      - 역할
        - 프록시에 더 이상 전송하지 않는 헤더 필드 지정.
        - 지속적 접속 관리.
      - HTTP/1.1에서는 지속적 접근이 디폴트.
        서버측에서 명시적으로 접속을 끊고 싶은 경우, Connection : close

   1. Date

      Date : Tue, 03 Jul 2012 04:40:59 GMT

      - HTTP 메시지를 생성한 날짜를 나타냄.

   1. Pragma

      Pragma : no-cache

      - HTTP/1.1과 HTTP/1.0의 호환성을 위해 정의되어 있음.
      - 모든 서버가 HTTP/1.1이 아닐 수 있기 때문에 아래와 같이 양쪽을 다 보내기도 함.
        Pragma : no-cache
        Cache-Control : no-cache

   1. Trailer (?)

      메시지 바디 뒤에 있는 헤더필드를 미리 전달 가능.

   1. Transfer-Encoding (?)

      메시지 바디의 전송 코딩 형식을 지정.

   1. Upgrade

      HTTP 및 다른 프로토콜의 새로운 버전이 통신에 이용되는 경우에 사용됨.

      Connection : Upgrade 지정 필요.

   1. Via

      클라이언트와 서버 간의 request/response 메시지의 경로를 알기 위해 사용됨.

      프록시/게이트웨이는 자신의 서버 정보를 Via 헤더 필드에 추가한 뒤 메시지 전송함.

      전송된 메시지의 추적과 리퀘스트 루프의 회피 등에 사용되기 때문에 프록시를 경유하는 경우에는 반드시 부가해야함.

   1. Warning

      캐시에 대한 경고를 유저에게 전달함.

      Warining : [경고 코드] [경고한 호스트:포트 번호]”[경고문]” ([날짜])
