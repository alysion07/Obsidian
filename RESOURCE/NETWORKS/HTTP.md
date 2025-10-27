## HTTP 응답 코드

HTTP 응답 코드는 서버가 클라이언트 요청을 처리한 결과를 나타내는 세 자리 숫자로 구성됩니다. 주요 코드와 의미는 다음과 같습니다:

### 1xx (정보 응답)
- 100 Continue: 클라이언트가 요청의 나머지 부분을 계속 보내도 됨.
- 101 Switching Protocols: 서버가 프로토콜 전환을 수락함.
### 2xx (성공)
- 200 OK: 요청이 성공적으로 처리됨.
- 201 Created: 요청이 성공적으로 처리되었고 새로운 리소스가 생성됨.
### 3xx (리다이렉션)
- 301 Moved Permanently: 요청한 리소스가 영구적으로 이동됨.
- 302 Found: 요청한 리소스가 일시적으로 다른 위치에 있음.
- 304 Not Modified: 캐시된 리소스를 사용해도 됨.

### 4xx (클라이언트 오류)
- 400 Bad Request: 잘못된 요청.
- 401 Unauthorized: 인증 필요.
- 403 Forbidden: 접근 권한 없음.
- 404 Not Found: 요청한 리소스를 찾을 수 없음.
### 5xx (서버 오류)
- 500 Internal Server Error: 서버 오류로 인해 요청을 처리할 수 없음.
- 502 Bad Gateway: 잘못된 게이트웨이.
- 503 Service Unavailable: 서버가 일시적으로 사용할 수 없음.