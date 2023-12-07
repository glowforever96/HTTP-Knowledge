## 일반 헤더
**HTTP 헤더 용도**
* HTTP 전송에 필요한 모든 정보
* 표준 헤더 많음
* 필요시 임의의 헤더 추가 가능

* 헤더 분류
  * General 헤더 : 메시지 전체 적용 정보
  * Request 헤더 : 요청 정보
  * Response 헤더 : 응답 정보
  * Entity 헤더 : 엔티티 바디 정보

### RFC723x 변화
* 엔티티 -> 표현(Representaion)
* Representation = representation Metadata + Representation Data
* 표현 = 표현 메타 데이터 + 표현 데이터

## HTTP BODY
**RFC7230 최신**
* 메시지 본문을 통해 표현 데이터 전달
* 메시지 본문 = 페이로드
* 표현은 요청이나 응답에서 전달할 실제 데이터
* 표현 헤더는 표현 데이터를 해석할 수 있는 정보 제공
  * 데이터 유형(html, json), 데이터 길이, 압축 정보 등

## 표현
* Content-Type : 표현 데이터의 형식 ex) text/html; charset=utf-8, application/json
* Content-Encoding: 표현 데이터의 압축 방식, 데이터를 전달하는 곳에서 압축후 인코딩 헤더 추가 ex)gzip, deflate
* Content-Langauage : 표현 데이터의 자연 언어 ex) ko, en, en-US
* Content-Length : 표현 데이터의 길이

## 협상(콘텐츠 네고시에이션)
**클라이언트가 선호하는 표현 요청**
* Accpet: 클라이언트가 선호하는 미디어 타입 전달
* Accept-Charset: 클라이언트가 선호하는 문자 인코딩
* Accept-Encoding: 클라이언트가 선호하는 압축 인코딩
* Accpet-Language: 클라이언트가 선호하는 자연 언어
* 협상 헤더는 요청시에만 사용

### 협상과 우선순위1
* Quality Values(q) 값 사용
* 0 ~ 1, 클수록 높은 우선순위
* 생략하면 1
* Accept-Language: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7

### 협상과 우선순위2
* 구체적인 것이 우선
  * Accpet: text/*, text/plain, text/plain;format=flowed, */*

### 협상과 우선순위3
* 구체적인 것을 기준으로 미디어타입을 맞춤
* Accpet: text/*;q=0.3, text/html;1=0.7, text/html;level=1, text/html;level=2;q=0.4, /*/;q=0.5

## 전송 방식
* 단순 전송 (Content-Length)
* 압축 전송 (Content-Encoding)
* 분할 전송 (Transfer-Encoding : chunked 분할해서 보내게 됨)
* 범위 전송 (Range, Content-Range)

## 일반 정보
* From: 유저 에이전트의 이메일 정보
  * 일반적으로 잘 사용 X
  * 검색 엔진 같은 곳에서 주로 사용
  * 요청에서 사용
* Referer: 이전 웹 페이지 주소
  * 현재 요청된 페이지의 이전 웹 페이지 주소
  * A->B로 이동하는 경우 B를 요청할 때 Referer: A를 포함해서 요청
  * 유입 경로 분석 가능
  * 요청에서 사용
* User-Agent: 유저 에이전트 애플리케이션 정보
  * 클라이언트의 애플리케이션 정보(웹 브라우저 정보..)
  * 통계 정보
  * 어떤 종류의 브라우저에서 장애가 발생하는지 파악 가능
  * 요청에서 사용
* Server: 요청을 처리하는 ORIGIN 서버의 소프트웨어 정보
  * Server: Apache/2.2.22 (Debian)
  * server: nginx
  * 응답에서 사용
* Date: 메시지가 발생한 날짜와 시간
  * 응답에서 사용

## 특별한 정보
* HOST: 요청한 호스트 정보
  * 요청에서 사용, 필수 값
  * 하나의 서버가 여러 도메인을 처리해야 할 때 (하나의 IP 주소에 여러 도메인이 적용)
* Location: 페이지 리다이렉션
  * Location 헤더가 있으면, Location 위치로 자동 이동
* Allow: 허용 가능한 HTTP 메서드
  * 405 에서 응답에 포함해야함
  * Allow: GET, HEAD, PUT
* Retry-After: 유저 에이전트가 다음 요청을 하기까지 기다려야 하는 시간
  * 503: 서비스가 언제까지 불능인지 알려줌

## 인증
* Authorization: 클라이언트 인증 정보를 서버에 전달
  * Authorization: Basic xxxxxxxxxxxxxxxxx
* WWW-Authenticate: 리소스 접근시 필요한 인증 방법 정의
  * 401 Unauthorized 응답과 함께 사용

## 쿠키
* Set-Cookie: 서버에서 클라이언트로 쿠키 전달(응답)
* Cookie: 클라이언트가 서버에서 받은 쿠키를 저장하고, HTTP 요청시 서버로 전달

* 사용처: 사용자 로그인 세션 관리, 광고 정보 트래킹
* 쿠키 정보는 항상 서버에 저장됨
  * 네트워크 트래픽 추가 유발
  * 최소한의 정보만 사용(세션 ID, 인증 토큰)
  * 서버에 전송하지 않고 웹 브라우저 내부에 데이터를 저장하고 싶으면 웹 스토리지(localStorage, sessionStorage)

### 쿠키 - 생명 주기
* Set-Cookie: expires=날짜
  * 만료일이 되면 쿠키 삭제
* Set-Cookie: max-age=3600 (3600초)
  * 0이나 음수를 지정하면 쿠키 삭제
* 세션 쿠키: 만료 날짜를 생략하면 브라우저 종료시 까지만 유지
* 영속 쿠키: 만료 날짜를 입력하면 해당 날짜까지 유지

### 쿠키 - 도메인
* domain=example.org
* 명시: 명시한 문서 기준 도메인 + 서브 도메인 포함
  * domain=example.org를 지정해 쿠키 생성

* 생략: 현재 문서 기준 도메인에만 적용
  * example.org에서 쿠키를 생성해고 domain 지정을 생략
    * example.org 에서만 쿠키 접근
    * dev.example.org는 쿠키 미접근

### 쿠키 - 경로
* path=/home
* 이 경로를 포함한 하위 경로 페이지만 쿠키 접근
* 일반적으로 path=/ 루트로 지정

### 쿠키 - 보안
* Secure
  * Secure를 적용하면 https 경우에만 전송
* HttpOnly
  * XSS 공격 방지
  * 자바스크립트에서 접근 불가
  * http 전송에만 사용
* SameSite
  * XSRF 공격 방지
  * 요청 도메인과 쿠키에 설정된 도메인이 같은 경우만 쿠키 전송