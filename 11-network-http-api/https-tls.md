# HTTPS & TLS

## 1. HTTP 다시 보기

HTTP는:

```text
HyperText Transfer Protocol
```

이다.

Client와 Server가:

```text
Request
Response
```

를 주고받기 위한 Application Layer Protocol이다.

예:

```text
Client
↓
GET /products
↓
Server
↓
200 OK
↓
Client
```

---

# 2. HTTP의 문제

일반 HTTP 자체는 통신 내용을 보호하는 암호화 계층을 제공하지 않는다.

예를 들어:

```text
ID

Password

Cookie

Token

개인정보

결제 정보
```

같은 Data를 Network로 전달한다고 생각해보자.

통신 경로에서 Data를 보호하기 위한 별도의 보안 계층이 필요하다.

---

# 3. HTTPS란?

HTTPS는 일반적으로:

```text
HTTP over TLS
```

라고 이해할 수 있다.

즉:

```text
HTTP 통신
+
TLS 보호
```

이다.

---

# 4. HTTPS의 S

```text
HTTPS
```

의 `S`는 일반적으로:

```text
Secure
```

를 의미한다고 이해한다.

---

# 5. HTTP vs HTTPS

HTTP:

```text
HTTP
↓
Transport
↓
Network
```

HTTPS:

```text
HTTP
↓
TLS
↓
Transport
↓
Network
```

처럼 개념적으로 생각할 수 있다.

---

# 6. HTTP/1.1 또는 HTTP/2에서

단순화하면:

```text
HTTP
↓
TCP
↓
IP
```

HTTPS는:

```text
HTTP
↓
TLS
↓
TCP
↓
IP
```

로 이해할 수 있다.

---

# 7. HTTP/3

HTTP/3에서는:

```text
HTTP/3
↓
QUIC
↓
UDP
↓
IP
```

구조를 사용한다.

QUIC 자체에 TLS 1.3의 보안 기능이 통합되어 있다.

---

# 8. 기본 Port

일반적으로:

```text
HTTP
→ 80

HTTPS
→ 443
```

Port를 기본값으로 사용한다.

---

# 9. URL

HTTP:

```text
http://example.com
```

HTTPS:

```text
https://example.com
```

---

# 10. 기본 Port 생략

다음 URL에서:

```text
https://example.com
```

Port를 직접 작성하지 않았지만 일반적으로 기본 HTTPS Port:

```text
443
```

을 사용한다.

---

# 11. TLS란?

TLS는:

```text
Transport Layer Security
```

의 약자이다.

Network 통신을 안전하게 보호하기 위한 보안 Protocol이다.

---

# 12. SSL이라는 말도 보게 된다

TLS를 공부하다 보면:

```text
SSL
```

이라는 용어를 자주 볼 수 있다.

SSL은:

```text
Secure Sockets Layer
```

이다.

---

# 13. SSL과 TLS

SSL은 TLS의 이전 기술이다.

현대적인 통신에서는:

```text
TLS
```

를 사용한다.

하지만 관습적으로:

```text
SSL Certificate

SSL 인증서
```

라는 표현을 아직 많이 사용한다.

실제로는 TLS Certificate를 의미하는 경우가 많다.

---

# 14. HTTPS가 제공하는 핵심 보호

HTTPS/TLS에서 중요하게 볼 것은:

```text
Confidentiality

Integrity

Authentication
```

이다.

---

# 15. Confidentiality

```text
기밀성
```

이다.

통신 내용을 암호화하여 제3자가 내용을 쉽게 읽지 못하도록 보호한다.

---

# 16. 예

사용자가:

```text
Password
```

를 Server에 전송한다고 하자.

HTTPS에서는 Network를 통과하는 Data가 TLS로 암호화된다.

---

# 17. 중요한 점

HTTPS라고 해서 Server 내부에서도 Password가 계속 암호문 상태로만 존재한다는 뜻은 아니다.

HTTPS는 주로:

```text
전송 중 Data
```

를 보호한다.

Server에서 Password를 어떻게 저장하고 처리할지는 별도의 보안 문제이다.

---

# 18. Password 저장

Password 저장에는 일반적으로:

```text
Password Hash
```

같은 별도의 보안 처리가 필요하다.

즉:

```text
HTTPS
≠
Password Database 보안 전체
```

이다.

---

# 19. Integrity

```text
무결성
```

이다.

통신 중 Data가 임의로 변경되었는지 탐지할 수 있도록 보호한다.

---

# 20. 예

Client가:

```text
price = 10000
```

이라는 Data를 보냈는데 통신 중 공격자가:

```text
price = 100
```

으로 바꾸는 상황을 생각할 수 있다.

TLS는 통신 Data의 변조를 탐지할 수 있도록 보호한다.

---

# 21. Authentication

TLS는 일반적인 HTTPS 연결에서 Server의 신원을 확인하는 데 인증서를 사용한다.

즉 Client는:

```text
내가 접속한 Server가
정말 example.com의 Server인가?
```

를 검증하는 과정을 거친다.

---

# 22. HTTPS의 세 가지 핵심

```text
기밀성
→ 내용을 보호

무결성
→ 변조를 탐지

인증
→ Server 신원 확인
```

으로 기억한다.

---

# 23. Certificate

HTTPS를 사용하면:

```text
Digital Certificate
```

를 만나게 된다.

한국어로:

```text
디지털 인증서

TLS 인증서

서버 인증서
```

등으로 표현할 수 있다.

---

# 24. 인증서의 역할

인증서는 특정 Domain과 Public Key 등의 정보를 연결하고, 신뢰할 수 있는 발급 기관의 서명을 통해 그 연결을 검증할 수 있도록 한다.

---

# 25. 예

Browser가:

```text
https://example.com
```

에 접속한다고 하자.

Server는 자신의 Certificate를 Client에게 제공한다.

---

# 26. Certificate에는

인증서에는 개념적으로 다음과 같은 정보가 포함된다.

```text
Domain 관련 정보

Public Key

발급자

유효 기간

Digital Signature
```

등이다.

---

# 27. Browser는 무엇을 확인할까?

Browser는 인증서를 검증하면서:

```text
신뢰 가능한 발급자인가?

접속한 Hostname과 인증서가 맞는가?

유효 기간은 정상인가?

인증서 Chain을 신뢰할 수 있는가?
```

등을 확인한다.

---

# 28. CA

CA는:

```text
Certificate Authority
```

의 약자이다.

한국어로:

```text
인증 기관
```

이라고 한다.

---

# 29. CA의 역할

CA는 인증서를 발급하고 Digital Signature를 통해 Certificate의 신뢰 관계를 형성하는 데 중요한 역할을 한다.

---

# 30. Browser가 모든 Website를 직접 아는 것은 아니다

Browser나 Operating System은 신뢰할 수 있는:

```text
Root CA
```

정보를 가지고 있다.

이를 바탕으로 Certificate Chain을 검증한다.

---

# 31. Certificate Chain

개념적으로:

```text
Root CA
↓
Intermediate CA
↓
Server Certificate
```

같은 신뢰 구조를 사용할 수 있다.

---

# 32. Browser 검증

Browser는 이 Chain을 따라가며 Server Certificate를 신뢰할 수 있는지 확인한다.

---

# 33. 인증서가 잘못되면?

예:

```text
인증서 만료

Domain 불일치

신뢰할 수 없는 발급자

Certificate Chain 문제
```

등이 있으면 Browser에서 보안 경고를 표시할 수 있다.

---

# 34. 인증서와 암호화는 같은 말인가?

아니다.

Certificate는 Server 신원 확인과 Public Key 연결 등에 사용된다.

실제 Session Data를 효율적으로 암호화하기 위해서는 TLS Handshake를 통해 공유 비밀을 만들고 대칭키 기반 암호화를 사용한다.

---

# 35. 암호화 방식

TLS를 이해하려면 두 가지 암호화 개념을 알아두면 좋다.

```text
대칭키 암호화

비대칭키 암호화
```

---

# 36. 대칭키 암호화

같은 Secret Key를 기반으로 Data를 암호화하고 복호화한다.

개념:

```text
Secret Key
↓
암호화

Secret Key
↓
복호화
```

---

# 37. 장점

대칭키 암호화는 많은 Data를 효율적으로 암호화하는 데 적합하다.

---

# 38. 문제

Client와 Server가 같은 Secret을 안전하게 공유해야 한다.

이것이 중요한 문제이다.

---

# 39. 비대칭키

비대칭키 암호 시스템에서는:

```text
Public Key

Private Key
```

Key Pair를 사용한다.

---

# 40. Public Key

이름처럼 공개할 수 있는 Key이다.

---

# 41. Private Key

Server가 안전하게 보호해야 하는 Key이다.

```text
Private Key
```

는 공개하면 안 된다.

---

# 42. Public Key와 Private Key

두 Key는 수학적으로 연결되어 있지만 Public Key만 가지고 Private Key를 실용적으로 계산하기 어렵도록 설계된다.

---

# 43. TLS에서는 둘을 어떻게 사용할까?

현대적인 TLS에서는 Handshake 과정에서 비대칭 암호 기술, Digital Signature, Key Exchange 등을 이용하여 Client와 Server가 안전하게 Session Key를 만들 수 있도록 한다.

---

# 44. 이후 실제 Data 통신

Handshake가 끝난 뒤 실제 Application Data는 효율적인:

```text
대칭키 암호화
```

를 이용해 보호한다.

---

# 45. 핵심

```text
인증서 / 비대칭 암호 기술 / Key Exchange
↓
안전하게 연결 설정
↓
공유 Session Key
↓
대칭키 기반 암호화
↓
HTTP Data 보호
```

라고 크게 이해한다.

---

# 46. 주의

TLS의 실제 Handshake는 Version과 Cipher Suite에 따라 세부 과정이 다르다.

따라서:

```text
Public Key로 Session Key를 단순 암호화해서 전달한다.
```

라고 모든 TLS 연결을 설명하면 정확하지 않다.

현대적인 TLS에서는 ECDHE 같은 Key Exchange가 널리 사용된다.

---

# 47. TLS Handshake

Client와 Server가 안전한 통신을 시작하기 전에:

```text
TLS Handshake
```

를 수행한다.

---

# 48. 매우 단순화한 흐름

```text
Client
↓
TLS 연결 시작

Server
↓
Certificate 제공

Client
↓
Certificate 검증

Client ↔ Server
↓
Key Exchange

Client ↔ Server
↓
공유 Secret 생성

Client ↔ Server
↓
암호화된 통신 시작
```

---

# 49. 실제로는 더 복잡하다

TLS Version에 따라:

```text
지원 Version

Cipher Suite

Key Share

Certificate

Digital Signature

Handshake Messages
```

등 여러 정보가 교환된다.

---

# 50. TLS 1.3

현대적인 HTTPS에서는:

```text
TLS 1.3
```

을 많이 사용한다.

TLS 1.2 역시 환경에 따라 사용될 수 있다.

오래된 SSL/TLS Version은 보안상 사용하지 않는 방향이다.

---

# 51. HTTPS Request

TLS 연결이 설정된 후 HTTP Request를 전송할 수 있다.

예:

```http
POST /login HTTP/1.1
Host: example.com
Content-Type: application/json

{
    "email": "user@example.com",
    "password": "example"
}
```

---

# 52. Network에서는

HTTP Message가 그대로 보호되지 않은 상태로 전송되는 것이 아니라 TLS Record 등을 통해 암호화되어 전달된다.

---

# 53. Server

Server는 TLS 계층에서 Data를 복호화한 뒤 HTTP Request를 처리한다.

---

# 54. Response도 동일

Server:

```text
HTTP Response 생성
↓
TLS 보호
↓
Network
↓
Client
↓
TLS 처리
↓
HTTP Response 사용
```

---

# 55. HTTPS 전체 흐름

```text
Browser
↓
https://example.com
↓
DNS
↓
Server Address
↓
Connection
↓
TLS Handshake
↓
Certificate 검증
↓
암호화된 연결 설정
↓
HTTP Request
↓
Server 처리
↓
HTTP Response
↓
TLS로 보호되어 전달
↓
Browser
```

---

# 56. URL도 암호화될까?

HTTPS라고 해서 URL의 모든 정보가 동일하게 숨겨지는 것은 아니다.

---

# 57. Domain

Network 연결을 만들기 위해 Destination Address 정보가 필요하고, TLS에서는 Server Name 정보가 노출될 수 있는 경우도 있다.

DNS 조회도 별도의 보호 기술을 사용하지 않는다면 관찰될 수 있다.

따라서:

```text
HTTPS니까 어떤 Website에 접속했는지까지
항상 완전히 숨겨진다.
```

라고 생각하면 안 된다.

---

# 58. Path와 Query

HTTPS에서는 일반적으로 HTTP Request의:

```text
Path

Query String

Headers

Body
```

가 TLS 보호 범위 안에 들어간다.

---

# 59. 하지만 Query에 Password를 넣어도 되는가?

아니다.

예:

```text
https://example.com/login?password=1234
```

는 피해야 한다.

---

# 60. 이유

HTTPS가 Network 전송을 보호하더라도 URL은:

```text
Browser History

Application Log

Server Log

Analytics

Screenshot

공유된 URL
```

등 다른 위치에 남을 수 있다.

---

# 61. HTTPS는 모든 보안을 해결하지 않는다

매우 중요하다.

```text
HTTPS 사용
≠
Website가 완전히 안전함
```

이다.

---

# 62. HTTPS가 있어도 발생 가능한 문제

예:

```text
SQL Injection

XSS

취약한 Password

잘못된 권한 설정

Server 해킹

Secret 노출

악성 File Upload

Application Logic Bug
```

등은 별도로 방어해야 한다.

---

# 63. HTTPS의 역할

HTTPS는 주로:

```text
Client ↔ Server
```

사이의 전송 구간을 보호한다.

---

# 64. Password 저장은 별도

```text
HTTPS
→ 전송 보호

Password Hash
→ 저장 보호
```

처럼 역할이 다르다.

---

# 65. Database 보안도 별도

```text
Database Access Control

Encryption at Rest

Backup Security

Network Policy
```

등은 HTTPS와 별도로 고려한다.

---

# 66. API Key도 별도

HTTPS를 사용하더라도 Source Code에:

```text
API_KEY = "secret..."
```

을 넣고 GitHub에 Push하면 Secret이 노출될 수 있다.

---

# 67. 따라서

Secret은:

```text
Environment Variable

Secret Manager
```

등을 이용해 관리할 수 있다.

---

# 68. HTTP 개발 환경

Local 개발에서는:

```text
http://127.0.0.1:8000
```

처럼 HTTP를 사용하는 경우가 많다.

---

# 69. FastAPI Local

예:

```powershell
uvicorn main:app --reload
```

기본적인 개발 Server를 실행하면 환경에 따라:

```text
http://127.0.0.1:8000
```

형태로 접근한다.

---

# 70. 배포 환경

실제 Internet에 공개하는 Service에서는 일반적으로:

```text
HTTPS
```

를 사용한다.

---

# 71. HTTPS를 누가 처리할까?

실제 배포에서는 FastAPI Application 자체가 TLS를 직접 처리할 수도 있지만:

```text
Reverse Proxy

Load Balancer

Cloud Service
```

등이 HTTPS/TLS를 처리하는 구조도 흔하다.

---

# 72. 예

```text
Internet
↓
HTTPS
↓
Reverse Proxy
↓
FastAPI
```

---

# 73. TLS Termination

Reverse Proxy나 Load Balancer에서 TLS 연결을 종료하고 내부 Application으로 Request를 전달하는 구성을:

```text
TLS Termination
```

이라고 부를 수 있다.

---

# 74. 예시 구조

```text
Browser
↓
HTTPS
↓
Nginx
↓
HTTP 또는 HTTPS
↓
FastAPI
```

내부 구간을 어떻게 보호할지는 배포 Architecture에 따라 결정한다.

---

# 75. Cloud 배포

Cloud 환경에서는:

```text
Load Balancer

API Gateway

Reverse Proxy

CDN
```

등이 Certificate와 HTTPS 처리를 담당할 수도 있다.

---

# 76. 인증서 발급

실제 Domain에 HTTPS를 적용하려면 신뢰할 수 있는 Certificate가 필요하다.

무료 인증서를 제공하는 CA 서비스도 존재한다.

---

# 77. Certificate 갱신

Certificate에는 유효 기간이 있다.

따라서:

```text
발급
↓
설치
↓
갱신
```

관리가 필요하다.

실제 운영에서는 자동 갱신을 사용하는 경우가 많다.

---

# 78. Browser 자물쇠 표시

Browser UI는 Version에 따라 표현 방식이 달라질 수 있지만 HTTPS Connection과 Website 보안 정보를 확인할 수 있는 기능을 제공한다.

---

# 79. 중요한 오해

HTTPS 표시가 있다고 해서:

```text
이 Website 회사가 무조건 믿을 만하다.

이 Website의 내용이 무조건 안전하다.
```

는 뜻은 아니다.

---

# 80. HTTPS가 보장하는 것

주로:

```text
현재 연결의 전송 보호

Certificate를 통한 Server Identity 검증
```

이다.

Website 사업 자체의 신뢰성을 보증하는 제도는 아니다.

---

# 81. HTTP와 HTTPS 비교

| 항목 | HTTP | HTTPS |
|---|---|---|
| Application Protocol | HTTP | HTTP |
| TLS 보호 | 없음 | 있음 |
| 기본 Port | 80 | 443 |
| URL Scheme | http | https |
| 전송 암호화 | 자체 제공 안 함 | TLS 사용 |
| Server 인증 | 자체 제공 안 함 | Certificate 사용 |
| 무결성 보호 | 자체 제공 안 함 | TLS 사용 |

---

# 82. HTTP Method는 달라지는가?

HTTPS를 사용한다고:

```text
GET
POST
PUT
PATCH
DELETE
```

가 달라지는 것은 아니다.

---

# 83. 예

HTTP:

```text
GET http://example.com/products
```

HTTPS:

```text
GET https://example.com/products
```

Application 수준의 HTTP Method 의미는 동일하다.

---

# 84. Status Code도 동일

HTTPS에서도:

```text
200

404

422

500
```

같은 HTTP Status Code를 사용한다.

---

# 85. JSON도 동일

HTTPS에서도 JSON을 사용할 수 있다.

```text
HTTPS
↓
HTTP Response
↓
JSON Body
```

---

# 86. 즉 HTTPS는

HTTP의:

```text
Method

Path

Header

Body

Status Code
```

라는 Application 의미를 바꾸는 것이 아니다.

통신을 TLS로 보호하는 것이다.

---

# 87. API에서도 HTTPS

배포된 API:

```text
https://api.example.com/products
```

처럼 사용할 수 있다.

---

# 88. AI API

```text
https://api.example.com/predict
```

---

# 89. 흐름

```text
Frontend
↓
HTTPS POST /predict
↓
TLS
↓
Backend
↓
AI Model
↓
JSON Response
↓
TLS
↓
Frontend
```

---

# 90. Mixed Content

HTTPS Page에서 보호되지 않은 HTTP Resource를 불러오려고 하면 Browser의:

```text
Mixed Content
```

보안 정책에 의해 문제가 발생할 수 있다.

---

# 91. 예

Frontend:

```text
https://myservice.com
```

API:

```text
http://api.myservice.com
```

처럼 HTTPS Page가 HTTP Resource에 접근하면 Browser가 차단할 수 있다.

---

# 92. 따라서 배포 시

Frontend와 API 모두 HTTPS를 사용하는 구성이 중요하다.

---

# 93. CORS와 HTTPS는 다른 문제

```text
HTTPS
→ 통신 보호

CORS
→ Browser의 Cross-Origin Request 제어
```

이다.

---

# 94. 예

```text
https://frontend.example.com

https://api.example.com
```

둘 다 HTTPS라도 Host가 다르기 때문에 서로 다른 Origin이다.

따라서 CORS 설정이 필요할 수 있다.

---

# 95. HTTPS라고 CORS가 없어지는 것이 아니다

두 문제는 서로 다르다.

```text
TLS
→ Security of Connection

CORS
→ Browser Origin Policy
```

---

# 96. Certificate Error와 HTTP Error

둘도 구분한다.

Certificate 검증에 실패하면 정상적인 HTTP Request를 보내기 전에 TLS 연결 단계에서 문제가 발생할 수 있다.

---

# 97. 반면 404

```text
404 Not Found
```

는 HTTP Response이다.

즉 TLS 연결 이후 Server와 HTTP 통신이 이루어진 것이다.

---

# 98. 비교

```text
Certificate Error
→ TLS 단계 문제

404
→ HTTP 단계 Response

500
→ Server Application 처리 문제
```

---

# 99. Debugging 단계

HTTPS Website가 안 열린다면:

```text
1. DNS가 정상인가?

2. Server에 연결 가능한가?

3. Port 443이 열려 있는가?

4. TLS Handshake가 되는가?

5. Certificate가 유효한가?

6. HTTP Response는 오는가?

7. Application은 정상인가?
```

처럼 단계별로 생각할 수 있다.

---

# 100. curl

HTTPS 연결도 curl로 확인할 수 있다.

```powershell
curl.exe -v https://example.com
```

---

# 101. -v

```text
Verbose
```

정보를 출력한다.

Connection과 TLS, HTTP Request/Response 관련 정보를 확인하는 데 도움이 된다.

---

# 102. Header만 확인

환경과 목적에 따라:

```powershell
curl.exe -I https://example.com
```

처럼 Header 중심으로 확인할 수도 있다.

---

# 103. 인증서 검증 끄기

curl에는 Certificate 검증을 무시하는 Option도 존재하지만 실제 Service에서 무분별하게 사용하면 안 된다.

인증서 Error가 발생했다면:

```text
왜 검증이 실패했는지
```

를 먼저 해결해야 한다.

---

# 104. 개발 환경의 Self-Signed Certificate

Local 개발이나 내부 환경에서는:

```text
Self-Signed Certificate
```

를 접할 수 있다.

---

# 105. Self-Signed

공개적으로 신뢰되는 CA Chain이 아니라 스스로 서명한 Certificate이다.

Browser가 기본적으로 신뢰하지 않아 경고가 발생할 수 있다.

---

# 106. Production

실제 공개 Service에서는 Client가 신뢰할 수 있는 Certificate Chain을 제공하도록 구성하는 것이 중요하다.

---

# 107. HTTPS와 Domain

Certificate는 일반적으로 특정 Hostname과 연결된다.

예:

```text
api.example.com
```

용 Certificate가:

```text
another.example.com
```

과 일치하지 않는다면 Hostname 검증 문제가 발생할 수 있다.

---

# 108. Wildcard Certificate

예:

```text
*.example.com
```

처럼 여러 Subdomain에 사용할 수 있도록 발급되는 Certificate도 있다.

적용 범위에는 규칙이 있으므로 모든 Domain에 무제한 사용할 수 있는 것은 아니다.

---

# 109. Public Key Infrastructure

Certificate, CA, Digital Signature 등의 신뢰 구조를 이해하다 보면:

```text
PKI
```

라는 용어를 볼 수 있다.

---

# 110. PKI

```text
Public Key Infrastructure
```

이다.

Public Key와 Digital Certificate를 이용한 신뢰 체계를 구축하고 관리하기 위한 구조이다.

---

# 111. 지금 단계에서는

PKI 전체를 깊게 외우기보다:

```text
Browser
↓
Certificate 확인
↓
CA Chain 검증
↓
Server Identity 확인
↓
TLS 연결
```

정도로 이해한다.

---

# 112. HTTPS를 사용하는 이유

주요 이유:

```text
Data 암호화

Data 무결성

Server 인증

사용자 정보 보호

API 통신 보호
```

이다.

---

# 113. Login

```text
Browser
↓
HTTPS
↓
POST /login
↓
Server
```

---

# 114. API

```text
Frontend
↓
HTTPS
↓
REST API
↓
Backend
```

---

# 115. AI Service

```text
사용자
↓
HTTPS
↓
Frontend / API
↓
FastAPI
↓
PyTorch
```

---

# 116. 지금까지 전체 연결

```text
Domain
↓
DNS
↓
IP
↓
Port 443
↓
Connection
↓
TLS Handshake
↓
Certificate 검증
↓
Encrypted Connection
↓
HTTP Request
↓
API
↓
Backend
↓
HTTP Response
↓
TLS
↓
Client
```

---

# 117. HTTP와 HTTPS 핵심 차이

```text
HTTP
→ HTTP 통신

HTTPS
→ TLS로 보호되는 HTTP 통신
```

이다.

---

# 118. TLS 핵심

```text
Confidentiality
→ 기밀성

Integrity
→ 무결성

Authentication
→ 인증
```

을 제공한다.

---

# 119. Certificate 핵심

```text
Server Identity

Public Key

CA Signature

Validity
```

등과 관련된다.

---

# 120. TLS Handshake 핵심

```text
Client
↕
Server

Certificate 검증
+
Key Exchange
↓
공유 Secret
↓
암호화된 통신
```

이다.

---

# 121. 꼭 구분

```text
HTTPS
≠ HTTP와 완전히 다른 Application 기능

HTTPS
= HTTP 통신을 TLS로 보호
```

---

# 122. 또 구분

```text
HTTPS
≠ Website 전체 보안

HTTPS
= Client와 Server 사이의 통신 보호의 핵심 요소
```

---

# 123. 또 구분

```text
TLS
≠ CORS

TLS
→ Connection Security

CORS
→ Browser Origin Policy
```

---

# 124. 또 구분

```text
TLS Error
→ HTTPS 연결 단계

HTTP 404
→ HTTP Request/Response 단계

HTTP 500
→ Server Application 처리 단계
```

---

# 125. 최종 정리

```text
HTTP
→ Request / Response Protocol

HTTPS
→ TLS로 보호되는 HTTP

TLS
→ Network 통신 보안 Protocol

Certificate
→ Server Identity와 Public Key를 검증하는 데 사용

CA
→ Certificate 신뢰 체계를 구성하는 인증 기관
```

---

# 126. 한 문장으로 정리

HTTPS는:

```text
HTTP 통신에 TLS를 적용하여
Client와 Server 사이의 Data에 대해
기밀성, 무결성, Server 인증 등을 제공하는
안전한 Web 통신 방식
```

이라고 이해할 수 있다.

---

# 127. Chapter 11 전체 정리

이번 Chapter에서 배운 흐름:

```text
Network
↓
IP / Port / DNS
↓
URL
↓
HTTP
↓
Request / Response
↓
Method / Status Code
↓
HTTPS / TLS
↓
JSON
↓
API / REST API
```

---

# 128. 다음 Chapter

이제 Network와 HTTP/API의 기본 원리를 이해했으므로 다음에는 Python을 이용해 직접 Web API를 만든다.

사용할 Framework:

```text
FastAPI
```

다음 Chapter에서는:

```text
FastAPI 설치

Server 실행

GET API

POST API

Path Parameter

Query Parameter

Request Body

Pydantic

Status Code

Error Handling

CORS

File Upload

AI Model 연결
```

등을 직접 구현한다.