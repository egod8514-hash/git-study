# HTTP Basic

## 1. HTTP란?

HTTP는:

```text
HyperText Transfer Protocol
```

의 약자이다.

Web에서 Client와 Server가 Request와 Response를 주고받기 위한 Application Layer Protocol이다.

가장 기본적인 구조는:

```text
Client
↓
HTTP Request
↓
Server
↓
HTTP Response
↓
Client
```

이다.

---

# 2. Protocol이란?

Protocol은 통신 규칙이다.

두 Computer가 Data를 주고받으려면:

```text
어떤 형식으로 요청할 것인가?

어떤 형식으로 응답할 것인가?

어떤 의미로 해석할 것인가?
```

같은 규칙이 필요하다.

HTTP는 Web 통신에서 이러한 규칙을 정의한다.

---

# 3. HTTP의 핵심

HTTP를 처음 배울 때 가장 중요한 것은:

```text
Request
Response
```

이다.

```text
Client
→ Request

Server
→ Response
```

라고 기억한다.

---

# 4. Client

HTTP Request를 보내는 쪽이다.

예:

```text
Chrome

Edge

Safari

Mobile App

JavaScript fetch()

Python Program

curl
```

등이 Client가 될 수 있다.

---

# 5. Server

Request를 받아 처리하고 Response를 보내는 쪽이다.

예:

```text
Web Server

FastAPI

Django

Spring

Node.js
```

등이 Server Application 역할을 할 수 있다.

---

# 6. Browser에서 Website 접속

사용자가 Browser에:

```text
https://example.com
```

을 입력한다.

단순화하면:

```text
Browser
↓
HTTP Request
↓
Server
↓
HTTP Response
↓
Browser
```

과정이 발생한다.

HTTPS인 경우 실제 통신에는 TLS도 사용된다.

---

# 7. Request란?

Client가 Server에게:

```text
무엇을 원하는지
```

전달하는 Message이다.

예:

```text
상품 목록 주세요.

10번 상품 주세요.

Login 처리해주세요.

새 게시글 저장해주세요.

Image를 분석해주세요.
```

---

# 8. Response란?

Server가 Request를 처리한 후 결과를 Client에게 전달하는 Message이다.

예:

```text
HTML

JSON

Image

File

성공 여부

Error 정보
```

등을 포함할 수 있다.

---

# 9. 쇼핑몰 예

```text
Browser
↓
상품 목록 주세요.
↓
Server
↓
상품 목록입니다.
↓
Browser
```

HTTP 관점에서는:

```text
GET /products
↓
Server
↓
200 OK
+
상품 Data
```

처럼 생각할 수 있다.

---

# 10. HTTP Request의 주요 구성 요소

HTTP Request를 공부할 때 다음을 구분한다.

```text
Method

Target / Path

Headers

Body
```

---

# 11. 예

개념적으로:

```http
POST /products HTTP/1.1
Host: example.com
Content-Type: application/json

{
    "name": "Notebook",
    "price": 1500000
}
```

---

# 12. Method

```text
POST
```

부분이다.

Server에게 어떤 종류의 작업을 요청하는지 표현한다.

대표적으로:

```text
GET

POST

PUT

PATCH

DELETE
```

등이 있다.

---

# 13. Request Target

```text
/products
```

부분이다.

어떤 Resource 또는 Route를 대상으로 Request하는지를 나타낸다.

---

# 14. HTTP Version

```text
HTTP/1.1
```

부분은 사용되는 HTTP Version을 나타낸다.

---

# 15. Header

```text
Host: example.com

Content-Type: application/json
```

같은 추가 정보를 전달한다.

---

# 16. Body

```json
{
    "name": "Notebook",
    "price": 1500000
}
```

같은 실제 Data를 전달할 수 있다.

모든 Request가 Body를 가지는 것은 아니다.

---

# 17. GET 예

```http
GET /products HTTP/1.1
Host: example.com
```

상품 목록을 요청하는 API라고 가정할 수 있다.

---

# 18. GET과 Body

GET Request는 일반적으로 조회 조건을 URL의 Path나 Query Parameter로 표현하는 경우가 많다.

예:

```text
GET /products?page=2
```

GET Request Body에 의존하는 API 설계는 일반적인 Web API에서는 피하는 편이다.

---

# 19. POST 예

```http
POST /products HTTP/1.1
Host: example.com
Content-Type: application/json

{
    "name": "Notebook"
}
```

새 Resource를 생성하는 API로 설계할 수 있다.

---

# 20. Response의 주요 구성 요소

HTTP Response에서는 다음을 중요하게 본다.

```text
HTTP Version

Status Code

Status

Headers

Body
```

---

# 21. Response 예

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "id": 10,
    "name": "Notebook"
}
```

---

# 22. Status Code

```text
200
```

부분이다.

Request 처리 결과를 숫자로 표현한다.

---

# 23. Reason Phrase

HTTP/1.1 예제에서:

```text
OK
```

처럼 Status Code의 의미를 사람이 읽기 쉽게 표현한 부분을 볼 수 있다.

---

# 24. Response Header

```text
Content-Type: application/json
```

같은 Response 관련 정보를 포함한다.

---

# 25. Response Body

```json
{
    "id": 10,
    "name": "Notebook"
}
```

처럼 Client가 실제로 사용할 Data가 들어갈 수 있다.

---

# 26. HTTP 전체 흐름

```text
Client
↓
Method
Path
Headers
Body
↓
HTTP Request
↓
Server
↓
Application Logic
↓
HTTP Response
↓
Status Code
Headers
Body
↓
Client
```

---

# 27. HTTP Method

대표적인 Method:

```text
GET
POST
PUT
PATCH
DELETE
```

---

# 28. GET

주로 Resource를 조회할 때 사용한다.

```text
GET /products
```

```text
상품 목록 조회
```

---

# 29. POST

주로 Resource를 생성하거나 특정 처리를 요청할 때 사용한다.

```text
POST /products
```

```text
상품 생성
```

---

# 30. PUT

Resource 전체를 교체하거나 갱신하는 의미로 사용하는 경우가 많다.

```text
PUT /products/10
```

---

# 31. PATCH

Resource의 일부를 수정하는 의미로 사용하는 경우가 많다.

```text
PATCH /products/10
```

---

# 32. DELETE

Resource를 삭제하는 데 사용한다.

```text
DELETE /products/10
```

---

# 33. 중요한 점

HTTP 자체가:

```text
GET이면 무조건 DB SELECT

POST면 무조건 DB INSERT
```

라고 강제하는 것은 아니다.

Method에는 표준적인 의미와 성질이 있고, API는 이를 적절하게 사용하도록 설계해야 한다.

---

# 34. URL + Method

같은 Path라도 Method에 따라 의미가 달라질 수 있다.

```text
GET /products

POST /products
```

Path는 동일하다.

하지만 Method가 다르다.

---

# 35. 의미

```text
GET /products
→ 상품 목록 조회

POST /products
→ 상품 생성
```

처럼 설계할 수 있다.

---

# 36. 그래서

API Request를 볼 때:

```text
URL만 보지 않는다.

Method도 같이 본다.
```

---

# 37. HTTP Header

Header는 Request 또는 Response에 대한 부가 정보를 전달한다.

예:

```text
Content-Type

Accept

Authorization

User-Agent

Host
```

등이 있다.

---

# 38. Content-Type

전송하는 Body의 Media Type을 나타낸다.

예:

```text
Content-Type: application/json
```

은:

```text
Body가 JSON 형식
```

이라는 의미이다.

---

# 39. Accept

Client가 어떤 Response 형식을 받을 수 있거나 선호하는지 Server에 전달할 때 사용한다.

예:

```text
Accept: application/json
```

---

# 40. Authorization

인증 정보 등을 전달할 때 사용될 수 있다.

예:

```text
Authorization: Bearer <token>
```

실제 Token이나 Secret은 학습 노트나 GitHub에 그대로 저장하지 않는다.

---

# 41. User-Agent

Client Application에 대한 정보를 전달할 수 있다.

Browser가 Request를 보내면 Browser 관련 User-Agent Header를 볼 수 있다.

---

# 42. Host

HTTP/1.1 Request에서는:

```text
Host: example.com
```

처럼 대상 Host 정보를 전달한다.

하나의 Server IP에서 여러 Domain을 서비스하는 구성에도 중요하다.

---

# 43. Body

Request Body에는 Server가 처리해야 하는 Data가 들어갈 수 있다.

예:

```json
{
    "email": "user@example.com",
    "password": "example"
}
```

---

# 44. JSON

Web API에서는 Body Data 형식으로 JSON을 매우 자주 사용한다.

```json
{
    "name": "Notebook",
    "price": 1500000
}
```

---

# 45. File Upload

File을 보낼 때는 JSON이 아니라:

```text
multipart/form-data
```

를 사용할 수 있다.

예:

```text
X-ray Image

Profile Image

Document
```

등이다.

---

# 46. AI API

Image 분석 API라면:

```text
POST /predict
```

Request에 Image File을 포함시킬 수 있다.

---

# 47. AI API 흐름

```text
Browser
↓
POST /predict
↓
Image File
↓
FastAPI
↓
PyTorch Model
↓
Prediction
↓
JSON Response
↓
Browser
```

---

# 48. Response Status Code

Status Code는 크게 다음 범위로 구분한다.

```text
1xx
→ Informational

2xx
→ Success

3xx
→ Redirection

4xx
→ Client Error

5xx
→ Server Error
```

---

# 49. 200

```text
200 OK
```

Request가 정상적으로 처리된 경우 자주 볼 수 있다.

---

# 50. 201

```text
201 Created
```

새 Resource가 성공적으로 생성된 경우 사용할 수 있다.

---

# 51. 204

```text
204 No Content
```

Request는 성공했지만 Response Body가 필요하지 않은 경우 사용할 수 있다.

---

# 52. 301 / 302

Redirect와 관련된 Status Code이다.

Client에게 다른 Location으로 이동하도록 안내할 수 있다.

---

# 53. 400

```text
400 Bad Request
```

잘못된 Request와 관련하여 사용할 수 있다.

---

# 54. 401

```text
401 Unauthorized
```

이름 때문에 헷갈릴 수 있지만 일반적으로 유효한 인증 자격 증명이 없거나 인증이 필요한 상황과 관련된다.

---

# 55. 403

```text
403 Forbidden
```

Server가 Request를 이해했지만 해당 Resource에 대한 접근을 허용하지 않는 경우 등에 사용된다.

---

# 56. 404

```text
404 Not Found
```

요청한 Resource 또는 Route를 찾을 수 없는 경우 자주 볼 수 있다.

---

# 57. 405

```text
405 Method Not Allowed
```

해당 Resource에서 요청한 HTTP Method가 허용되지 않는 경우이다.

예:

```text
Backend
GET /products만 있음

Client
POST /products 요청
```

같은 상황에서 볼 수 있다.

---

# 58. 422

FastAPI를 사용할 때 자주 만나게 될 수 있다.

입력 Data Validation 문제 등에:

```text
422
```

Response를 볼 수 있다.

---

# 59. 500

```text
500 Internal Server Error
```

Server 내부에서 처리 중 문제가 발생한 경우이다.

---

# 60. Error를 보면

Status Code를 먼저 확인한다.

```text
404
→ URL / Path?

405
→ Method?

422
→ 입력 Data?

500
→ Backend 내부 Error?
```

처럼 문제 범위를 좁힐 수 있다.

---

# 61. HTTP는 Stateless

HTTP는 기본적으로:

```text
Stateless
```

한 Protocol이다.

---

# 62. Stateless란?

각 Request를 기본적으로 독립적인 Request로 취급한다는 의미이다.

Server가 HTTP 자체만으로 이전 Request의 사용자 상태를 자동으로 기억해주는 구조는 아니다.

---

# 63. 예

첫 번째 Request:

```text
GET /products
```

두 번째 Request:

```text
GET /cart
```

HTTP 자체는 이 두 Request 사이의 사용자 상태를 자동으로 연결해서 기억하지 않는다.

---

# 64. 그런데 Login은 어떻게 유지할까?

Web Service에서는 별도의 Mechanism을 사용한다.

예:

```text
Cookie

Session

Token
```

등이다.

---

# 65. Cookie

Browser에 저장될 수 있는 작은 Data이다.

Server와의 HTTP 통신에서 Cookie를 주고받을 수 있다.

---

# 66. Session

Server 측에서 사용자 상태를 저장하고, Client의 Session Identifier 등을 통해 연결하는 방식으로 사용할 수 있다.

---

# 67. Token

Client가 인증 정보를 나타내는 Token을 Request와 함께 보내는 방식도 있다.

예:

```text
Authorization: Bearer ...
```

---

# 68. Stateless와 Connection은 다른 개념

HTTP가 Stateless라는 것은:

```text
Application Request 상태
```

에 대한 개념이다.

Network Connection을 항상 매 Request마다 반드시 새로 만든다는 의미와 동일하지 않다.

---

# 69. HTTP Connection

HTTP Version에 따라 Connection을 사용하는 방식이 다를 수 있다.

---

# 70. HTTP/1.0

초기의 HTTP에서는 Request마다 Connection을 새로 만드는 방식이 흔했다.

---

# 71. HTTP/1.1

Persistent Connection을 기본적으로 활용할 수 있어 하나의 TCP Connection을 여러 Request에 재사용할 수 있다.

---

# 72. Keep-Alive

Connection을 재사용하는 개념과 관련하여:

```text
Keep-Alive
```

라는 표현을 자주 볼 수 있다.

---

# 73. 왜 Connection을 재사용할까?

매번:

```text
Connection 생성
↓
Request
↓
Response
↓
Connection 종료
```

를 반복하면 추가 비용이 발생할 수 있다.

Connection을 재사용하면 이러한 비용을 줄일 수 있다.

---

# 74. HTTP/1.1

HTTP/1.1은 오랫동안 Web에서 널리 사용된 Version이다.

Text 형태의 HTTP Message 예제를 공부할 때 자주 사용한다.

---

# 75. HTTP/2

HTTP/2에서는 하나의 Connection에서 여러 Request/Response Stream을 효율적으로 처리할 수 있는 Multiplexing 등의 기능이 도입되었다.

---

# 76. HTTP/2의 핵심

단순화하면:

```text
HTTP/1.1
→ 여러 Request 처리에서 제약이 존재

HTTP/2
→ 하나의 Connection에서 여러 Stream을 Multiplexing 가능
```

이라고 이해할 수 있다.

---

# 77. HTTP/3

HTTP/3는 TCP 대신:

```text
QUIC
```

을 Transport로 사용한다.

QUIC은 UDP를 기반으로 설계된 Transport Protocol이다.

---

# 78. 단순 비교

```text
HTTP/1.1
↓
TCP

HTTP/2
↓
TCP

HTTP/3
↓
QUIC
↓
UDP
```

---

# 79. HTTPS

HTTPS는 별개의 Web Application Protocol이라고 생각하기보다:

```text
HTTP 통신을 TLS로 보호
```

하는 구조로 이해하면 된다.

---

# 80. HTTP

일반적인 HTTP 통신은 전송되는 내용을 보호하는 암호화 계층을 자체적으로 제공하지 않는다.

---

# 81. HTTPS

TLS를 사용하여:

```text
암호화

Server 인증

Data 무결성 보호
```

등을 제공한다.

---

# 82. 그래서 실제 Service에서는

민감한 Data뿐만 아니라 현대적인 Web Service 전반에서 HTTPS 사용이 기본적이다.

---

# 83. HTTP와 TCP/IP

단순화하면 HTTP/1.1 또는 HTTP/2 통신은:

```text
HTTP
↓
TCP
↓
IP
↓
Network
```

구조로 생각할 수 있다.

HTTPS라면:

```text
HTTP
↓
TLS
↓
TCP
↓
IP
```

처럼 이해할 수 있다.

---

# 84. HTTP/3

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

---

# 85. Browser 주소 입력

사용자가:

```text
https://example.com/products
```

를 입력한다.

---

# 86. 전체 흐름

매우 단순화하면:

```text
URL 분석
↓
Host 확인
↓
DNS 조회
↓
Server Address 확인
↓
Connection 설정
↓
TLS 설정
↓
HTTP Request
↓
Server 처리
↓
HTTP Response
↓
Browser 처리
```

이다.

---

# 87. Browser Rendering

Response가 HTML이라면 Browser는 HTML을 분석한다.

그 과정에서 추가 Resource가 필요할 수 있다.

예:

```text
CSS

JavaScript

Image

Font
```

---

# 88. 추가 HTTP Request

HTML:

```html
<link
    rel="stylesheet"
    href="style.css"
>
```

가 있다면 Browser는:

```text
style.css
```

를 받기 위해 추가 Request를 보낼 수 있다.

---

# 89. Image도 동일

```html
<img
    src="image.jpg"
    alt="Image"
>
```

가 있다면:

```text
image.jpg
```

에 대한 Request가 발생할 수 있다.

---

# 90. Website 한 번 열었는데 Request가 많은 이유

하나의 Page에서도:

```text
HTML

CSS

JavaScript

Images

Fonts

API Data
```

등 여러 Resource가 필요하기 때문이다.

---

# 91. Network Tab

Chrome DevTools:

```text
F12
↓
Network
```

에서 실제 Request들을 확인할 수 있다.

---

# 92. Network에서 볼 것

```text
Name

Method

Status

Type

Size

Time
```

등을 확인할 수 있다.

---

# 93. Request 상세

Request 하나를 선택하면:

```text
Request URL

Request Method

Status Code

Request Headers

Response Headers

Payload

Response
```

등을 확인할 수 있다.

---

# 94. Frontend API Request

JavaScript:

```javascript
const response =
    await fetch(
        "http://localhost:8000/products"
    );
```

---

# 95. fetch 기본 Method

`fetch()`에서 Method를 지정하지 않으면 기본적으로:

```text
GET
```

을 사용한다.

---

# 96. POST

```javascript
const response =
    await fetch(
        "http://localhost:8000/products",
        {
            method: "POST",

            headers: {
                "Content-Type":
                    "application/json"
            },

            body: JSON.stringify({
                name: "Notebook",
                price: 1500000
            })
        }
    );
```

---

# 97. 이 코드를 HTTP 관점에서 보기

```text
URL
→ http://localhost:8000/products

Method
→ POST

Header
→ Content-Type: application/json

Body
→ 상품 JSON
```

이다.

---

# 98. FastAPI

Backend:

```python
@app.post("/products")
def create_product():

    ...
```

---

# 99. 연결

Frontend:

```text
POST /products
```

Backend:

```text
POST /products
```

가 일치해야 한다.

---

# 100. Method가 다르면

Frontend:

```text
POST /products
```

Backend:

```text
GET /products
```

만 정의되어 있다면 같은 Path라도 다른 Endpoint 처리이다.

---

# 101. Path가 다르면

Frontend:

```text
GET /product
```

Backend:

```text
GET /products
```

라면 다른 Path이다.

---

# 102. 그래서 Debugging

API가 동작하지 않으면:

```text
URL

Method

Headers

Body

Status Code

Response
```

를 확인한다.

---

# 103. curl

Browser나 Frontend Code 없이도 HTTP Request를 직접 보내볼 수 있다.

예:

```powershell
curl.exe https://example.com
```

---

# 104. GET 명시

```powershell
curl.exe -X GET https://example.com
```

---

# 105. POST

예:

```powershell
curl.exe -X POST "https://example.com/products" `
    -H "Content-Type: application/json" `
    -d '{"name":"Notebook","price":1500000}'
```

---

# 106. curl 옵션

```text
-X
→ HTTP Method 지정

-H
→ Header 추가

-d
→ Request Data / Body
```

이다.

---

# 107. 그래서 curl을 배우는 이유

Frontend 화면 없이도:

```text
URL

Method

Header

Body
```

를 직접 지정해서 HTTP Request를 테스트할 수 있기 때문이다.

---

# 108. API 문제를 분리할 수 있다

Frontend에서 API가 안 될 때:

```text
Frontend 문제인가?

Backend API 문제인가?
```

헷갈릴 수 있다.

---

# 109. curl로 API 테스트

curl에서는 정상인데 Frontend에서 실패한다면:

```text
JavaScript

CORS

Frontend URL

Frontend Data 처리
```

등을 추가로 확인할 수 있다.

---

# 110. curl에서도 실패

그렇다면:

```text
Backend

API URL

Method

Request Data

Server
```

등을 우선 확인할 수 있다.

---

# 111. HTTP와 REST API

REST 스타일 API에서는 HTTP의 개념을 활용한다.

예:

```text
GET /products

POST /products

GET /products/10

PATCH /products/10

DELETE /products/10
```

---

# 112. HTTP와 Database

API Request가 Database 작업으로 이어질 수 있다.

```text
GET /products
↓
FastAPI
↓
SELECT
↓
Database
↓
JSON Response
```

---

# 113. HTTP와 AI

```text
POST /predict
↓
FastAPI
↓
PyTorch
↓
Prediction
↓
JSON Response
```

---

# 114. HTTP는 AI를 모른다

중요한 점:

HTTP는:

```text
AI Model을 실행하는 Protocol
```

이 아니다.

HTTP는 Client와 Server가 Request/Response를 주고받는 통신 규칙이다.

---

# 115. AI 실행은 Backend의 역할

```text
HTTP Request
↓
Backend
↓
Python
↓
PyTorch
↓
Prediction
↓
Backend
↓
HTTP Response
```

이다.

---

# 116. HTTP와 JSON도 같은 것이 아니다

```text
HTTP
→ 통신 Protocol

JSON
→ Data 표현 형식
```

이다.

---

# 117. HTTP + JSON

두 개를 함께 사용하는 경우가 많다.

```text
HTTP Request
+
JSON Body
```

```text
HTTP Response
+
JSON Body
```

---

# 118. HTTP와 URL도 같은 것이 아니다

```text
URL
→ 어디에 접근할 것인가?

HTTP
→ 어떻게 Request / Response를 주고받을 것인가?
```

이다.

---

# 119. IP와 HTTP

```text
IP
→ Network에서 목적지를 찾는 데 사용

HTTP
→ Application 수준의 Request / Response 규칙
```

이다.

---

# 120. Port와 HTTP

HTTP Server가 특정 Port에서 Request를 기다릴 수 있다.

예:

```text
FastAPI
127.0.0.1:8000
```

---

# 121. 지금까지 전부 연결

```text
URL
↓
Host
↓
DNS
↓
IP
↓
Port
↓
Server 연결
↓
HTTP Request
↓
Backend
↓
HTTP Response
↓
Client
```

---

# 122. AI Web Service까지 연결

```text
사용자
↓
Browser
↓
JavaScript
↓
HTTP POST /predict
↓
Network
↓
FastAPI
↓
PyTorch
↓
Prediction
↓
HTTP 200 Response
↓
JSON
↓
JavaScript
↓
DOM
↓
사용자
```

---

# 123. HTTP Error와 Application Error

HTTP Request 자체는 성공적으로 Server에 도착했지만 Application 처리 결과가 실패할 수도 있다.

예:

```text
POST /predict
↓
FastAPI까지 도착
↓
Image 처리 중 Error
↓
500 Response
```

---

# 124. 그래서

```text
Network가 연결됐다.
```

와:

```text
Application이 정상 동작한다.
```

는 같은 의미가 아니다.

---

# 125. 200인데 Data가 이상할 수도 있다

HTTP 통신 자체는 성공했지만 Application Logic에 문제가 있을 수 있다.

예:

```text
200 OK

하지만 Prediction 결과가 잘못됨
```

이 경우:

```text
Model

Preprocessing

Business Logic
```

등을 확인해야 한다.

---

# 126. Status Code만으로 모든 것을 판단하지 않는다

Status Code는 HTTP 처리 결과를 이해하는 중요한 정보지만 Application 결과의 품질까지 보장하지는 않는다.

---

# 127. HTTP Debugging 순서

API가 안 된다면:

```text
1. Server가 실행 중인가?

2. URL이 맞는가?

3. Method가 맞는가?

4. Header가 맞는가?

5. Body가 맞는가?

6. Status Code는?

7. Response는?

8. Backend Log는?

9. Application Logic은?
```

순서로 확인한다.

---

# 128. HTTP를 공부할 때 계속 볼 구조

```text
Request

Method
URL
Headers
Body

↓

Server

↓

Response

Status Code
Headers
Body
```

---

# 129. Request 한 줄로 보기

```text
POST /predict
```

만 보더라도:

```text
POST
→ 어떤 종류의 작업?

/predict
→ 어느 Route?
```

라고 생각한다.

---

# 130. Request 전체로 보기

```text
POST /predict

Content-Type: multipart/form-data

[Image File]
```

---

# 131. Response

```text
200 OK

Content-Type: application/json

{
    "label": "Pneumonia",
    "score": 0.93
}
```

---

# 132. 전체 해석

```text
Client
→ /predict에 Image 분석 요청

Server
→ Image 처리

AI Model
→ Prediction

Server
→ JSON Response

Client
→ 화면 출력
```

---

# 133. 핵심 용어

```text
HTTP
→ Web Request / Response Protocol

Client
→ Request를 보내는 쪽

Server
→ Request를 처리하는 쪽

Method
→ 요청의 의미/성질

URL
→ 요청 대상 위치

Header
→ Request / Response 부가 정보

Body
→ 전달 Data

Status Code
→ 처리 결과

Stateless
→ 각 Request를 기본적으로 독립적으로 처리

HTTPS
→ TLS로 보호되는 HTTP 통신
```

---

# 134. 가장 중요한 구조

```text
HTTP Request

Method
URL
Headers
Body

        ↓

      Server

        ↓

HTTP Response

Status Code
Headers
Body
```

---

# 135. 한 문장으로 정리

HTTP는:

```text
Client가 Server에게
Method, URL, Header, Body 등을 이용하여 Request를 보내고,

Server가 이를 처리한 뒤
Status Code, Header, Body 등을 포함한 Response를 반환하기 위한

Web의 Application Layer 통신 Protocol
```

이라고 이해할 수 있다.

---

# 136. 다음 단계

이제 HTTP 전체 구조를 이해했으므로 다음에는:

```text
HTTP Request
HTTP Response
```

를 더 자세히 분해한다.

특히:

```text
Request Line

Request Header

Request Body

Status Line

Response Header

Response Body

Content-Type

Accept

Authorization

Cookie
```

등을 실제 Message 형태로 살펴본다.