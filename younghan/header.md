### HTTP 일반 헤더
header-field=field-name: field-value 이런식으로 되어있음.

### 분류
```
General: 메시지 전체에 적용되는 정보
Request: 요청 정보
Response: 응답 정보
Representation: 표현 메타데이터 + 표현 데이터
```

표준 헤더가 너무 많음... 그리고 필요시 임의로 헤더를 추가할 수 있음.

메시지 본문(페이로드)을 통해 표현 데이터를 전달

표현은 요청이나 응답에서 전달할 실제 데이터

표현 헤더는 표현 데이터를 해석할 수 있는 정보 제공

데이터 유형, 데이터 길이, 압축 정보 등등

참고: 표현 헤더는 표현 메타데이터와 페이로드 메시지를 구분해야 하지만, 여기서는 생략

### HTTP 전송에 필요한 모든 부가정보
EX) 메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증, 요청 클라이언트, 서버 정보, 캐시 관리 정보

### 표현
표현 헤더는 요청, 응답 둘다 사용.

Content-Type: 표현 데이터의 형식, 미디어 타입, 문자 인코딩

Content-Encoding: 표현 데이터의 압축 방식, 클라이언트에서 압축 후 인코딩 헤더 추가, 서버에서 인코딩 헤더의 정보로 압축 해제

Content-Language: 표현 데이터의 자연 언어

Content-Length: 표현 데이터의 길이, 바이트 단위

### 협상(컨텐츠 네고시에이션)
클라이언트가 선호하는 표현 요청, 요청시에만 사용

Accept: 클라이언트가 선호하는 미디어 타입 전달

Accept-Charset: 클라이언트가 선호하는 문자 인코딩

Accept-Encoding: 클라이언트가 선호하는 압축 인코딩

Accept-Language: 클라이언트가 선호하는 자연 언어

### 우선순위
Quality Values 값 사용

0~1, 클수록 높은 우선순위

생략하면 1

EX) Accept-Language: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7

### 전송 방식
```
1. 단순 전송
Content-Length를 그대로 내려준다.

2. 압축 전송
Content-Encoding을 사용해서 압축된 데이터를 내려준다.

3. 분할 전송
Transfer-Encoding: chunked을 사용한다.
Content-Length를 입력하지 않는다.

EX)
5
hello
5
world
0
\r\n

4. 범위 전송
범위를 지정하고 전송한다. 중간에 통신 장애가 났을 때 사용
Range: 1001-2000
Content-Range: bytes 1001-2000 / 2000
```

### From
유저 에이전트의 이메일 정보

일반적으로 잘 사용되지 않음

검색 엔진 같은 곳에서 주로 사용

요청에서 사용

### Referer
현재 요청된 페이지의 이전 웹 페이지 주소

A -> B로 이동하는 경우 B를 요청할 때 Referer: A를 포함해서 요청

Referer를 사용해서 유입 경로 분석 가능

요청에서 사용

참고: Referer는 단어 referrer의 오타

### User-Agent
유저 에이전트 애플리케이션 정보

클라이언트의 애플리케이션 정보(웹 브라우저 정보, 등등)

통계 정보

어떤 종류의 브라우저에서 장애가 발생하는지 파악 가능

요청에서 사용

### Server
요청을 처리하는 ORIGIN 서버의 소프트웨어 정보

Server: Apache/2.2.22(Debian)

server: nginx

응답에서 사용

### Date
메시지가 발생한 날짜와 시간

Date: Tue, 15 Nov 1994 08:12:31 GMT

응답에서 사용

### Host
요청한 호스트 정보(도메인)

요청에서 사용하고 필수값임.

하나의 서버가 여러 도메인을 처리해야 할 때

하나의 IP주소에 여러 도메인이 적용되어 있을 때

### Location
페이지 리다이렉션

웹 브라우저는 3XX 응답의 결과에 Location 헤더가 있으면 Location 위치로 자동 이동(리다이렉트)

201 (Created): Location 값은 요청에 의해 생성된 리소스 URI

3XX (Redirection): Location 값은 요청을 자동으로 리다이렉션하기 위한 대상 리소스를 가리킴

### Allow
허용 가능한 HTTP 메서드

405 (Method Not Allowed)에서 응답에 포함해야 함

Allow: GET, HEAD, PUT

### Retry-After
유저 에이전트가 다음 요청을 하기까지 기다려야 하는 시간

503 (Service Unavailable): 서비스가 언제까지 불능인지 알려줄 수 있음

Retry-After: Fri, 31 Dec 1999 23:59:59 GMT (날짜 표기)

Retry-After: 120 (초단위 표기)

### Authorization
클라이언트 인증 정보를 서버에 전달

Authorization: Basic XXXXXXXXXXXXXXXXXX

### WWW-Authenticate
리소스 접근시 필요한 인증 방법 정의

401 Unauthrized 응답과 함께 사용

WWW-Authenticate: Newauth realm="apps", type=1, ...

### 쿠키
Stateless인 HTTP에서 사용자의 정보를 개인화하기 위한 수단이다.

어떤 사용자가 요청을 했는지 알기 위해서 사용한다.

모든 요청에 쿠키 정보를 자동으로 포함한다.

### Set-Cookie
서버에서 클라이언트로 쿠키 전달(응답)

### Cookie
클라이언트가 서버에서 받은 쿠키를 저장하고, HTTP 요청시 서버로 전달

### 사용처
1. 사용자 로그인 세션 관리
2. 광고 정보 트래킹

### 쿠키 정보는 항상 서버에 전송됨
1. 네트워크 트래픽 추가 유발
2. 최소한의 정보만 사용(세션id, 인증 토큰)
3. 서버에 전송하지 않고, 웹 브라우저 내부에 데이터를 저장하고 싶으면 웹 스토리지 참고

### 주의!
보안에 민감한 데이터는 저장하면 안됨(주민번호, 신용카드 번호등등)

### expires(만료 날짜 세팅)
만료일이 되면 쿠키 삭제

### max-age(초 단위 세팅)
0이나 음수를 지정하면 쿠키 삭제

세션 쿠키: 만료날짜를 생략하면 브라우저 종료시까지만 유지

영속 쿠키: 만료날짜를 입력하면 해당 날짜까지 유지

### domain
도메인을 명시하면 쿠키를 생성할 경우 하위 도메인까지 쿠키 접근이 가능함.

### path
이 경로를 포함한 하위 경로 페이지만 쿠키 접근

### secure
쿠키는 HTTPS를 구분하지 않고 전송
이 옵션을 설정하면 HTTPS일 경우에만 전송

### httponly
XSS 공격 방지
JS에서 접근 불가(document.cookie)
HTTP 전송에만 사용

### samesite
XSRF 공격 방지
요청 도메인과 쿠키에 설정된 도메인이 같은 경우만 쿠키 전송
