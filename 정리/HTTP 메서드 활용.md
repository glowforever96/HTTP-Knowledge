## 클라이언트에서 서버로 데이터 전송
데이터 전달 방식은 크게 2가지

* 쿼리 파라미터를 통한 데이터 전송
  * GET
  * 주로 정렬 필터
* 메시지 바디를 통한 데이터 전송
  * POST, PUT, PATCH
  * 회원 가입, 상품 주문, 리소스 등록, 변경

### 4가지 상황
1. 정적 데이터 조회 - 이미지, 정적 텍스트 문서
2. 동적 데이터 조회 - 주로 검색, 게시판 목록에서 정렬 필터
3. HTML Form을 통한 데이터 전송 - 회원 가입, 상품 주문, 데이터 변경
4. HTTP API를 통한 데이터 전송 - 회원 가입, 상품 주문, 데이터 변경<br/>
    서버 to 서버, 앱 클라이언트, 웹 클라이언트(Ajax)

#### 정적 데이터 조회
* 이미지, 정적 텍스트 문서
* 조회는 GET 사용
* 정적 데이터는 일반적으로 리소스 경로로 단순하게 조회 가능

#### 동적 데이터 조회
* 조회 조건을 줄여주는 필터, 조회 결과를 정렬하는 정렬 조건에 주로 사용
* 조회는 GET 사용
* GET은 쿼리 파라미터 사용해 데이터 전달

#### Form 데이터 전송
* HTML Form submit시 POST 전송
* 파일 전송: Content-Type: multipart/form-data
* 참고: HTML Form 전송은 GET, POST만 지원

#### HTTP API를 통한 데이터 전송
* 서버 to 서버
* 앱 클라이언트
* 웹 클라이언트
  * HTML에서 Form 전송 대신 자바스크립트를 통한 통신에 사용
* POST, PUT, PATCH: 메시지 바디를 통해 데이터 전송
* Content-Type: application/json을 주로 사용 (사실상 표준)

## HTTP API 설계 예시
* 회원 조회 /members -> GET
* 회원 등록 /members -> POST
* 회원 수정 /members/{id} -> PATCH, PUT, POST
* 회원 삭제 /members/{id} -> DELETE

### POST - 신규 자원 등록 특징
* 클라이언트는 등록될 리소스의 URI를 모름
  * 회원 등록 /members -> POST
  * POST /members
* 서버가 새로 등록된 리소스 URI를 생성
  * HTTP/1.1 201 Created
  * Location: /members/100
* 컬렉션(Collection)
  * 서버가 관리하는 리소스 디렉토리
  * 서버가 리소스의 URI를 생성하고 관리
  * 여기서 컬렉션은 /members

### 파일 관리 시스템 (PUT 기반 등록)
* 파일 목록 /files -> GET
* 파일 조회 /files/{filename} -> GET
* 파일 등록 /files/{filename} -> PUT
* 파일 삭제 /files/{filename} -> DELETE
* 파일 대량 등록 /files -> POST

***
* 클라이언트가 리소스 URI를 알고 있어야함 
  * 파일 등록 /files/{filename} -> PUT
  * PUT /files/star.jpg
* 클라이언트가 직접 리소스의 URI를 지정
* 스토어(Store)
  * 클라이언트가 관리하는 리소스 저장소
  * 클라이언트가 리소스의 URI를 알고 관리
  * 여기서 스토어는 /files



