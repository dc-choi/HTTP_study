### 클라이언트에서 서버로 데이터 전송
1. 쿼리 파라미터를 통한 데이터 전송

	GET, 주로 정렬 필터(검색어)

2. 메시지 바디를 통한 데이터 전송

	POST, PUT, PATCH

	회원 가입, 상품 주문, 리소스 등록, 리소스 변경

### 데이터를 전송하는 4가지 상황
1. 정적 데이터 조회

	이미지, 정적 텍스트 문서

	GET을 사용하고, 일반적으로 쿼리 파라미터 없이 리소스 경로로 단순하게 조회 가능

2. 동적 데이터 조회

	검색, 게시판 목록에서 정렬 필터

	GET을 사용하며, 쿼리 파라미터를 이용해 데이터를 동적으로 생성할 수 있도록 한다.

	서버는 전달된 쿼리 파라미터로 데이터를 동적으로 생성해서 전달한다.

3. HTML Form을 통한 데이터 전송

	회원 가입, 상품 주문, 데이터 변경

	동적으로 데이터를 조회하는 부분과 다르게 Content-Type이 application/x-www-form-urlencoded로 전송된다.

	메시지의 바디 부분에는 쿼리 파라미터와 비슷하게 생성되어서 전달된다.

	파일 전송을 위해서는 Content-Type을 multipart/form-data로 바꿔서 전송하면 된다.

	multipart/form-data로 전송하게 될 경우 각각의 파라미터를 나눠서 보낸다.

	```
	-----XXX
	Content-Disposition: form-data; name="username"

	kim
	-----XXX
	Content-Disposition: form-data; name="file"; filename="intro.jpg"
	Content-Type: image/jpg

	12asdf31wwqer2312qwerqwer31231qwerweqrfweq893219e8h9hbewfkjsb...
	```

	참고로 HTML Form은 GET, POST만 지원한다고 합니다.

4. HTTP API를 통한 데이터 전송
	회원 가입, 상품 주문, 데이터 변경

	서버 TO 서버, 앱 클라이언트, 웹 클라이언트(AJAX)
	
	Content-Type이 application/json으로 전송된다. (사실상 표준)
	
	GET, POST, PUT, PATCH, DELETE를 전부 지원한다.

### HTTP API 설계 예시
1. 컬렉션

	POST 기반 등록

	EX) 회원 관리 API

	회원 목록 GET /members

	회원 등록 POST /members

	회원 조회 GET /members/{id}

	회원 수정 PATCH, PUT, POST /members/{id}

	회원 삭제 DELETE /members/{id}

	클라이언트는 등록될 리소스의 URI를 모른다.

	서버가 새로 등록된 리소스 URI를 생성해준다.

2. 스토어

	PUT 기반 등록

	EX) 정적 컨텐츠 관리, 원격 파일 관리

	파일 목록 GET /files

	파일 조회 GET /files/{filename}

	파일 등록 PUT /files/{filename}

	파일 삭제 DELETE /files/{filename}

	파일 대량 등록 POST /files

	클라이언트가 리소스 URI를 알고 있어야 한다.

	직접 리소스의 URI를 지정한다.

3. HTML FORM 사용

	웹 페이지 회원 관리 (GET, POST만 지원)

	회원 목록 GET /members

	회원 등록 폼 GET /members/new

	회원 등록 POST /members/new

	회원 조회 GET /members/{id}

	회원 수정 폼 GET /members/{id}/edit

	회원 수정 POST /members/{id}/edit

	회원 삭제 POST /members/{id}/delete

	위처럼 URI를 표현하는걸 컨트롤 URI라고 부름

	HTTP 메서드로 해결하기 애매하거나 제약이 걸린경우 사용한다고 함.

### URI 설계 개념 정리
1. 문서(document)

	단일 개념(파일 하나, 객체 인스턴스, 데이터베이스 row)

	EX) /members/100, /files/star.jpg

2. 컬렉션(collection)

	서버가 관리하는 리소스 디렉토리

	서버가 리소스의 URI를 생성하고 관리

	EX) /members

3. 스토어(store)

	클라이언트가 관리하는 리소스 저장소

	클라이언트가 리소스의 URI를 알고 관리

	EX) /files

4. 컨트롤러, 컨트롤 URI

	문서, 컬렉션, 스토어로 해결하기 어려운 추가 프로세스 실행

	동사를 직접 사용

	EX) /members/{id}/delete
