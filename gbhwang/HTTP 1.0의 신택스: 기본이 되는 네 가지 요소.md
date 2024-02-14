# 1장. HTTP/1.0의 신택스: 기본이 되는 네 가지 요소

# HTTP의 기본 요소

- 메서드와 경로
- 헤더
- 바디
- 상태 코드

# HTTP 역사

## 버전별 출시 연도

- 1990년 : HTTP/0.9 ⇒ 다큐먼트를 요청하여 가져오는 단순한 프로토콜이었음
- 1996년 : HTTP/1.0 ⇒ 요청 시 메서드/헤더 추가, 응답 헤더 추가 및 상태 코드 추가
- 1997년 : HTTP/1.1
- 2005년 : HTTP/2

0.9 버전은 현행 프로토콜과 하위 호환성이 없음

# URL

## 형식

> **스키마://사용자:패스워드@호스트명:포트/경로#프래그먼트?쿼리**
> 
- 스키마 해석은 브라우저의 책임, 스키마를 보고 적절한 접속 방법을 선택해야 함

## URI 와의 차이

- URI에는 `URN`이라는 이름 부여 규칙도 포함됨
- URL은 장소로, 문서 등의 리소스를 특정하는 수단 제공

# HTTP Method

- `GET` : 서버에 헤더와 콘텐츠 요청
- `HEAD` : 서버에 헤더만 요청
- `POST` : 새로운 문서 추가
- `PUT` : 이미 존재하는 URL 문서 갱신
- `DELETE` : 지정된 URL의 문서 삭제

# HTTP Header

## Request Header

- `User-Agent` : 클라이언트의 애플리케이션명
- `Referer` : 클라이언트가 요청을 보낼 때 보고 있던 페이지 URL
- `Authorization` : 인증 정보 전달

⇒ 같은 헤더를 여러 번 보내는 것도 허용

## Response Header

- `Content-Type` : 파일 종류 지정, 지정된 MIME 타입 사용
- `Content-Length` : 페이로드 크기
- `Content-Encoding` : 압축 시 압축 형식
- `Date` : 문서 날짜

# HTTP Body

- 1.0 부터 헤더와 바디의 분리가 필요해졌음
- 응답 바디에는 한 파일만 반환
- 응답 바디의 길이는 `Content-Length` 헤더로 지정
- 통신 속도를 위해 바디 압축 가능
- GET, HEAD, DELETE, OPTIONS, CONNECT 메서드는 요청 바디를 가질 수 있으나 서버에서 거부할 수 있음 (요청 바디 전송 지양)

# Status Code

## 카테고리

- `1xx` : 처리 진행중
- `2xx` : 성공
- `3xx` : 서버에서 클라이언트로의 명령, 리다이렉트나 캐시 이용 지시
- `4xx` : 클라이언트가 보낸 요청에 오류 존재
- `5xx` : 서버 내부 오류 발생

## 리다이렉트

- `301`, `308` : 요청된 페이지가 다른 페이지로 영구적으로 이동되었을 때 사용
- `302`, `307` : 일시적 이동
- `303` : 요청된 페이지에 반환할 컨텐츠가 없거나 원래 반환할 페이지가 따로 있는 경우 사용

클라이언트는 위 상태 코드를 받고 `Location` 헤더 값을 확인하여 재요청

# `curl` 주요 옵션

- `-G`, `—get` : GET 메서드로 요청
- `-X` : HTTP 메서드 지정
- `-v` : 자세한 정보 표시
- `-H` : 헤더행 추가
- `-L` : 리다이렉트 시 허용
- `-d` : 송신 시 바디 전송

# 참고 자료

## HTTP Header 목록

[Message Headers](http://www.iana.org/assignments/message-headers/message-headers.xhtml)

## HTTP Method와 Request Body 전송 관련

```
   The presence of a message-body in a request is signaled by the
   inclusion of a Content-Length or Transfer-Encoding header field in
   the request's message-headers. A message-body MUST NOT be included in
   a request if the specification of the request method (section 5.1.1)
   does not allow sending an entity-body in requests. A server SHOULD
   read and forward a message-body on any request; if the request method
   does not include defined semantics for an entity-body, then the
   message-body SHOULD be ignored when handling the request.                                
```

```java
요청에 메시지 본문이 있다는 것은 요청에 요청의 메시지 헤더에 콘텐츠 길이 또는 전송 인코딩 헤더 필드가 포함되어 있으면 요청의 메시지 헤더에 포함됩니다. 메시지 본문은 요청 메서드의 사양이 요청 메소드의 사양(5.1.1절)이 엔티티 전송을 허용하지 않는 경우 에서 요청에 엔티티-본문 전송을 허용하지 않는 경우 요청에 메시지-본문을 포함해서는 안 됩니다. 서버는 다음과 같이 해야 합니다.
모든 요청에서 메시지 본문을 읽고 전달해야 하며, 만약 요청 메서드에 엔티티 바디에 대한 정의된 시맨틱이 포함되어 있지 않으면 메시지 본문은 요청을 처리할 때 무시해야 합니다.
```

[RFC 2616: Hypertext Transfer Protocol -- HTTP/1.1](https://datatracker.ietf.org/doc/html/rfc2616)
