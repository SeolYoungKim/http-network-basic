# HTTP와 연계하는 웹서버

<br>

### **가상 호스트**

- 물리적으로는 서버가 1대지만 가상으로 여러대가 있는 것처럼 설정하는 것이 가능.
- 같은 IP주소에서 다른 호스트명과 도메인명을 가진 여러 개의 웹사이트가 실행될 수 있게 해줌
- 가상 호스트 시스템 안에서 구분하는 방법

<br>

(1) HTTP Request를 보내는 경우 호스트명과 도메인명을 완전하게 포함한 URI지정.

(2) Host 헤더 필드에서 지정.

![img1](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/6afc99ea-faa3-45e9-b795-fff875301b77/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221216%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221216T151510Z&X-Amz-Expires=86400&X-Amz-Signature=40603a94d9a319e06c9e982fd59b3c66a4fc7b2a84c98372ea598022f4c4b28b&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

![img2](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/7e1ad4ee-72c6-45a3-8510-898435d4f6e5/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221216%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221216T151517Z&X-Amz-Expires=86400&X-Amz-Signature=734c97e5fea17dd6757ecec06d1385728b027026232ea6253dd9e8a1951d93e6&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

<br>
<br>
<br>

### **통신을 중계하는 프로그램**

: 클라이언트와 서버 이외에 통신을 중계하는 친구들.

1. **프록시**

- 클라이언트-서버 사이에서 데이터를 전달해주는 서버.
- request를 가로챈 뒤 response를 돌려줌.

  가로챈 request는

        전달,
        전달 X (캐시인 경우),
        수정 (서로 다른 두 네트워크 간 경계에서 헤더를 바꾼 경우)

        모두 가능함.

- 클라이언트로부터 받은 Request URI를 변경하지 않음.
- 프록시 서버를 경유할 때마다 Via 헤더필드에 정보 추가.
- 사용자의 로컬 컴퓨터, 또는 사용자의 컴퓨터와 오리진 서버 사이 그 어디든 존재 가능함.
- 크게 2가지 종류
  (1) 포워드 프록시 : 인터넷 상에서 어디로든지 request 전송.
  ![img3](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/de3833a1-c9fb-47ad-8a55-c2c7f1552888/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221216%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221216T151521Z&X-Amz-Expires=86400&X-Amz-Signature=7d42216971bc0cc25b5999c68e27a1a6049288dbe2c71ed356e7027e342c9cf4&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)
  - 익명 프록시 : 서버ID 공개, 클라이언트의 IP 비공개.
  - 투명 프록시 : 자신을 프록시 서버로 식별, HTTP 헤더 필드 지원. IP주소 검색 가능.
    (2) 리버스 프록시 : 인터넷에서 request를 받으면, 내부망 내의 서버로 전송.
    ![img4](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/f70c6e90-027c-4c24-a57f-5e4ac719fc54/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221216%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221216T151523Z&X-Amz-Expires=86400&X-Amz-Signature=b2a5ee38c992d9662f3ca0957958998e1c9ea4ca0dd74a284e6cd00a43c5d391&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)
- 프록시 서버 사용 이유
  - 인터넷 속도 향상에 도움이 되는 경우가 존재. (캐시 저장 가능)
  - 보안. (서버 IP 숨기기 가능)
  - 우회.
- 프록시 사용 방법

  1. 캐싱 프록시 : 서버 상에 리소스 캐시를 보존해두는 타입. (response 중계할 때)

     같은 리소스에 대한 요청일 경우, 오리진 서버로부터 리소스를 얻는 것이 아니라 캐시로부터 얻음.

  2. 투명 프록시 : 중계 시 요청/응답메시지 변경을 하지 않는 타입.
  3. 비투과 프록시 : 요청/응답메시지에 변경을 가하는 타입.

+) 오리진 서버 : 리소스 본체를 가진 서버.

+) Via : 프록시 자체에 대한 정보(클라이언트 정보X) 제공 위해 사용하는 헤더.

포워드/리버스 프록시 둘 모두에 의해 추가되며 요청/응답 헤더에 나타날 수 있음.

<br>

2. **게이트웨이**

- 프록시와 동작이 유사.
- 다음에 있는 서버가 HTTP서버 이외의 서비스를 제공하는 서버임.
- 클라이언트와 게이트웨이 사이를 암호화하여 안전하게 접속함. (안정성 높임)

<br>

3. **터널**

- 요구에 따라 다른 서버와의 통신 경로 확립.
- 클라이언트는 SSL같은 암호화 통신을 통해 서버와 안전하게 통신 가능.
- 리퀘스트 그대로 다음 서버에 중계.
- 통신하고 있는 양쪽 끝의 접속이 끊어질 때 터널도 종료함.
- 터널 자체는 투명한 존재. 클라이언트는 터널을 의식할 필요 X.
  안전한 통신로 확립용.

<br>
<br>
<br>

### **캐시**

- 캐시 : 프록시 서버와 클라이언트의 로컬 디스크에 보관된 리소스의 사본.
- 캐시를 사용하면 리소스를 가진 서버로의 액세스를 줄이는 것이 가능 → 통신 양/시간 절약
- 캐시 서버 : 프록시 서버의 하나. (== 캐싱 프록시)

프록시가 서버로부터의 응답을 중계할 때 프록시 서버 상에 리소스의 사본이 등록됨.

- 장점 :
  - 같은 데이터는 가까운 서버로부터 얻을 수 있게 되어 오리진 서버는 동일한 응답을 매번 처리하지 않아도 됨.
  - 인터넷 서비스 속도가 높아짐.
- 주의 : 오리진 서버의 원래 리소스가 갱신되는 경우, 캐시 서버는 낡은 리소스를 보낼 수도 있음.

  클라이언트의 요구/캐시의 유효 기간에 따라 오리진 서버로 리소스의 유효성 확인/리소스 갱신하러 감.

- 클라이언트(브라우저)도 캐시를 가지고 있음.

  같은 데이터는 서버에 액세스하지 않고 로컬 디스크로부터 불러옴.

  리소스가 오래된 경우, 오리진 서버로 리소스의 유효성 확인/리소스 갱신하러 감.

    <br>
    <br>
    <br>

### **호스트 네임/도메인 네임**

![img5](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/9111bf54-f591-4120-a986-fffa3d040b53/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221216%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221216T151527Z&X-Amz-Expires=86400&X-Amz-Signature=f1c69cfe779c742451ec035df5f4a86ac7cb5a8903d1412cfca54612206204c5&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

- 호스트 네임

: 네트워크에 연결된 장치들에게 부여되는 각각의 고유한 이름.

- 도메인 네임

: 네트워크 상에서 컴퓨터를 식별.

- ex)

  www.naver.com

  mail.naver.com

  → www와 mail은 naver.com이라는 도메인 네임에서 각각의 서비스를 구분하기 위한 호스트네임.

- DNS에서 주소를 찾아가는 과정

![img6](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/1b25df29-dec1-4fa0-9123-069e82351241/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221216%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221216T151529Z&X-Amz-Expires=86400&X-Amz-Signature=fc268e7053a6e59d85add67cb2f6b18d26cec8a225f049c19f29cdf508f63ac8&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

com → naver.com → www.naver.com 순으로 DNS 서버에서 검색함.

<br>
<br>
<br>

[참고]

[https://developer.mozilla.org/ko/docs/Glossary/Proxy_server](https://developer.mozilla.org/ko/docs/Glossary/Proxy_server)

[https://real-dongsoo7.tistory.com/77](https://real-dongsoo7.tistory.com/77)
