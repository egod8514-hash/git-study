# HTTP Request & Response

## 1. HTTP 통신의 기본

HTTP 통신의 핵심은:

```text
Request
Response
```

이다.

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

Client가 먼저 Request를 보내고 Server가 그 결과를 Response한다.

---

# 2. HTTP Request

HTTP Request는 Client가 Server에게 보내는 Message이다.

예:

```text
상품 목록을 주세요.

Login 해주세요.

새 게시글을 저장해주세요.

Image를 분석해주세요.
```

---

# 3. HTTP Response

HTTP Response는 Server가 Request를 처리한 뒤 Client에게 보내는 Message이다.

예:

```text
상품 목록입니다.

Login 성공입니다.

게시글을 저장했습니다.

AI 분석 결과입니다.
```

---

# 4. Request의 주요 구성

HTTP Request를 이해할 때 크게:

```text
Request Line

Headers

Body
```

로 나눠 생각할 수 있다.

---

# 5. HTTP/1.1 Request 예

```http
POST /products HTTP/1.1
Host: example.com
Content-Type: application/json
Accept: application/json

{
    "name": "Notebook",
    "price": 1500000
}
```

---

# 6. 구조

```text
POST /products HTTP/1.1
→ Request Line

Host: example.com
Content-Type: application/json
Accept: application/json
→ Headers

{
    "name": "Notebook",
    "price": 1500000
}
→ Body
```

---

# 7. Request Line

첫 번째 줄:

```http
POST /products HTTP/1.1
```

을 Request Line이라고 한다.

---

# 8. Request Line 구성

```text
POST
→ HTTP Method

/products
→ Request Target

HTTP/1.1
→ HTTP Version
```

---

# 9. Method

Method는 Request의 의미와 성질을 나타낸다.

대표적으로:

```text
GET
POST
PUT
PATCH
DELETE
```

가 있다.

---

# 10. GET

Resource 조회에 주로 사용한다.

```http
GET /products HTTP/1.1
```

---

# 11. POST

Resource 생성 또는 Server의 특정 처리를 요청할 때 주로 사용한다.

```http
POST /products HTTP/1.1
```

---

# 12. PATCH

Resource 일부를 수정할 때 사용할 수 있다.

```http
PATCH /products/10 HTTP/1.1
```

---

# 13. DELETE

Resource 삭제에 사용할 수 있다.

```http
DELETE /products/10 HTTP/1.1
```

---

# 14. Request Target

다음에서:

```http
GET /products?page=2 HTTP/1.1
```

Request Target은:

```text
/products?page=2
```

이다.

---

# 15. Path

그중:

```text
/products
```

가 Path이다.

---

# 16. Query String

```text
page=2
```

는 Query String이다.

---

# 17. Headers

Request에 대한 추가 정보를 전달한다.

예:

```http
Host: example.com
Accept: application/json
Content-Type: application/json
Authorization: Bearer ...
```

---

# 18. Header 기본 형태

```text
Header-Name: value
```

형태이다.

예:

```http
Content-Type: application/json
```

---

# 19. Host

HTTP/1.1 Request에서:

```http
Host: example.com
```

과 같이 대상 Host 정보를 전달한다.

---

# 20. Accept

Client가 받을 수 있거나 선호하는 Response Media Type을 나타낼 수 있다.

```http
Accept: application/json
```

---

# 21. 의미

```text
가능하면 JSON 형식의 Response를 받고 싶다.
```

라고 이해할 수 있다.

---

# 22. Content-Type

Body의 Media Type을 나타낸다.

```http
Content-Type: application/json
```

---

# 23. 의미

```text
지금 보내는 Body는 JSON 형식이다.
```

라는 의미이다.

---

# 24. Accept와 Content-Type 차이

헷갈리기 쉽다.

```text
Accept
→ 내가 어떤 Response 형식을 받을 수 있는가?

Content-Type
→ 내가 보내는 Body가 어떤 형식인가?
```

---

# 25. 예

```http
Accept: application/json
Content-Type: application/json
```

이면:

```text
Request Body
→ JSON

원하는 Response
→ JSON
```

이라고 볼 수 있다.

---

# 26. Authorization

인증 정보를 전달할 때 사용할 수 있다.

예:

```http
Authorization: Bearer <token>
```

---

# 27. 주의

실제 Token:

```text
API Key
Access Token
Secret
```

등을 GitHub나 학습 문서에 그대로 올리지 않는다.

---

# 28. User-Agent

Client 정보를 전달할 수 있다.

```http
User-Agent: ...
```

Browser에서 Request를 보내면 Browser 관련 정보를 볼 수 있다.

---

# 29. Cookie

Browser가 Cookie를 Server에 전달할 수 있다.

예:

```http
Cookie: session_id=...
```

Login 상태 유지 등에 활용될 수 있다.

---

# 30. Body

Request Body에는 Server에 전달할 실제 Data가 들어갈 수 있다.

예:

```json
{
    "name": "Notebook",
    "price": 1500000
}
```

---

# 31. 모든 Request에 Body가 있는가?

아니다.

예:

```http
GET /products HTTP/1.1
Host: example.com
```

처럼 Body 없이 Request할 수 있다.

---

# 32. POST Body

예:

```http
POST /products HTTP/1.1
Host: example.com
Content-Type: application/json

{
    "name": "Notebook"
}
```

---

# 33. Header와 Body 사이

HTTP/1.1의 Text 표현에서는 Headers와 Body 사이에 빈 줄이 있다.

```text
Headers

Body
```

형태이다.

---

# 34. JSON Body

API에서는 JSON을 많이 사용한다.

```json
{
    "email": "user@example.com",
    "name": "Kim"
}
```

---

# 35. Form Data

HTML Form Data를 전송할 때 다른 Content-Type을 사용할 수도 있다.

예:

```text
application/x-www-form-urlencoded
```

---

# 36. File Upload

File Upload에서는:

```text
multipart/form-data
```

를 자주 사용한다.

---

# 37. AI Image Upload

예:

```text
POST /predict

Content-Type:
multipart/form-data

Body:
X-ray Image File
```

---

# 38. JavaScript FormData

```javascript
const formData =
    new FormData();

formData.append(
    "file",
    file
);
```

---

# 39. fetch

```javascript
const response =
    await fetch(
        "http://localhost:8000/predict",
        {
            method: "POST",
            body: formData
        }
    );
```

---

# 40. 중요한 점

`FormData`를 `fetch()`로 보낼 때는 Browser가 필요한 `multipart/form-data` Header와 boundary를 설정하도록 두는 것이 일반적이다.

따라서 다음처럼 Content-Type을 직접 지정하지 않는 것이 일반적이다.

```javascript
headers: {
    "Content-Type":
        "multipart/form-data"
}
```

---

# 41. 이유

`multipart/form-data`에는 각 Part를 구분하기 위한:

```text
boundary
```

정보가 필요하기 때문이다.

Browser가 이를 자동으로 구성하도록 두는 것이 안전하다.

---

# 42. HTTP Response

이제 Response를 보자.

예:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "id": 10,
    "name": "Notebook"
}
```

---

# 43. Response 구성

크게:

```text
Status Line

Headers

Body
```

로 나누어 생각할 수 있다.

---

# 44. Status Line

```http
HTTP/1.1 200 OK
```

이다.

---

# 45. 구성

```text
HTTP/1.1
→ HTTP Version

200
→ Status Code

OK
→ Reason Phrase
```

---

# 46. Status Code

Server가 Request를 어떻게 처리했는지를 나타내는 숫자이다.

예:

```text
200
201
400
401
403
404
422
500
```

---

# 47. Response Header

예:

```http
Content-Type: application/json
```

Response Body의 Media Type이 JSON임을 나타낸다.

---

# 48. Response Body

실제 결과 Data가 들어갈 수 있다.

```json
{
    "id": 10,
    "name": "Notebook"
}
```

---

# 49. 모든 Response에 Body가 있는가?

아니다.

예를 들어:

```text
204 No Content
```

Response는 Body가 없는 성공 Response에 사용할 수 있다.

---

# 50. 전체 비교

Request:

```text
Request Line
Headers
Body
```

Response:

```text
Status Line
Headers
Body
```

---

# 51. Request와 Response 비교

```text
REQUEST

Method
Target
Version
Headers
Body

        ↓

      SERVER

        ↓

RESPONSE

Version
Status Code
Headers
Body
```

---

# 52. GET 예제

Request:

```http
GET /products HTTP/1.1
Host: example.com
Accept: application/json
```

Response:

```http
HTTP/1.1 200 OK
Content-Type: application/json

[
    {
        "id": 1,
        "name": "Notebook"
    },
    {
        "id": 2,
        "name": "Mouse"
    }
]
```

---

# 53. 의미

```text
Client
→ 상품 목록 주세요.

Server
→ 성공했습니다.
→ 여기 상품 목록입니다.
```

---

# 54. 특정 상품 조회

Request:

```http
GET /products/10 HTTP/1.1
Host: example.com
Accept: application/json
```

---

# 55. 성공 Response

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "id": 10,
    "name": "Notebook"
}
```

---

# 56. 존재하지 않는 경우

```http
HTTP/1.1 404 Not Found
Content-Type: application/json

{
    "detail": "Product not found"
}
```

---

# 57. 중요한 점

같은 Request라도 처리 결과에 따라:

```text
200

404

500
```

등 서로 다른 Response가 올 수 있다.

---

# 58. POST 예제

Request:

```http
POST /products HTTP/1.1
Host: example.com
Content-Type: application/json
Accept: application/json

{
    "name": "Notebook",
    "price": 1500000
}
```

---

# 59. Response

```http
HTTP/1.1 201 Created
Content-Type: application/json

{
    "id": 10,
    "name": "Notebook",
    "price": 1500000
}
```

---

# 60. 의미

```text
Client
→ 새 상품을 생성해주세요.

Server
→ 생성했습니다.
→ 생성된 상품은 이것입니다.
```

---

# 61. Login Request

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

# 62. Login Response

개념적인 예:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "message": "Login success"
}
```

---

# 63. 실제 Login은 더 복잡하다

실제 Service에서는:

```text
Password Hash

Cookie

Session

Token

CSRF

HTTPS

Authorization
```

등 보안 요소를 고려해야 한다.

---

# 64. Password를 URL에 넣지 않는다

다음과 같은 방식은 피한다.

```text
/login?email=user@example.com&password=1234
```

---

# 65. URL에 민감한 Data를 피하는 이유

URL은:

```text
Browser History

Server Log

Proxy Log

Analytics
```

등에 기록될 가능성이 있다.

---

# 66. Body도 무조건 안전한 것은 아니다

Request Body에 Password를 넣었다고 해서 자동으로 암호화되는 것은 아니다.

통신 보호를 위해:

```text
HTTPS
```

가 중요하다.

---

# 67. Query Parameter Request

```http
GET /products?category=computer&page=2 HTTP/1.1
Host: example.com
```

---

# 68. Query

```text
category=computer

page=2
```

이다.

---

# 69. Path Parameter

```http
GET /products/10 HTTP/1.1
```

여기서:

```text
10
```

을 Path Parameter로 처리할 수 있다.

---

# 70. Query vs Body

Query:

```text
/products?category=computer
```

Body:

```json
{
    "name": "Notebook"
}
```

은 다른 영역이다.

---

# 71. 처음에는 이렇게 구분

```text
Path
→ 어떤 Resource?

Query
→ 어떤 조건?

Body
→ 어떤 Data?
```

라고 생각하면 쉽다.

---

# 72. AI Prediction Request

예를 들어 Image Classification API를 만든다고 하자.

```text
POST /predict
```

---

# 73. Request

개념적으로:

```http
POST /predict HTTP/1.1
Host: localhost:8000
Content-Type: multipart/form-data; boundary=...

[Image File]
```

---

# 74. Backend

```text
FastAPI
↓
Image 받기
↓
Image 전처리
↓
Tensor 변환
↓
PyTorch Model
↓
Prediction
```

---

# 75. Response

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "prediction": 1,
    "label": "Pneumonia",
    "score": 0.93
}
```

---

# 76. Frontend

JavaScript는 Response를 받는다.

```javascript
const data =
    await response.json();
```

---

# 77. 중요한 부분

`response.json()`은:

```text
HTTP Response 자체
```

를 만드는 것이 아니다.

Response Body에 들어 있는 JSON Data를 JavaScript에서 사용할 수 있는 값으로 읽는 과정이다.

---

# 78. fetch의 response

```javascript
const response =
    await fetch(url);
```

여기서 `response`에는 HTTP Response와 관련된 정보가 들어 있다.

---

# 79. Status

```javascript
console.log(
    response.status
);
```

예:

```text
200
```

---

# 80. 성공 여부

```javascript
console.log(
    response.ok
);
```

`response.ok`는 일반적으로 Status Code가:

```text
200 ~ 299
```

범위인지 확인하는 데 사용할 수 있다.

---

# 81. JSON 읽기

```javascript
const data =
    await response.json();
```

---

# 82. Text 읽기

Response Body가 일반 Text라면:

```javascript
const text =
    await response.text();
```

같은 방법을 사용할 수 있다.

---

# 83. Response Header 읽기

```javascript
const contentType =
    response.headers.get(
        "content-type"
    );
```

같은 방법도 사용할 수 있다.

---

# 84. Error 처리

```javascript
if (!response.ok) {

    throw new Error(
        `HTTP Error: ${response.status}`
    );

}
```

---

# 85. 중요한 점

`fetch()`는 404나 500 Response를 받았다고 해서 항상 자동으로 JavaScript Exception을 발생시키는 방식은 아니다.

따라서:

```javascript
response.ok
```

또는:

```javascript
response.status
```

를 확인하는 습관이 중요하다.

---

# 86. Network Error

Server 자체에 연결하지 못하는 등의 Network Error라면 `fetch()` 자체가 실패할 수 있다.

---

# 87. HTTP Error와 Network Error

구분한다.

```text
HTTP Error

404
500
등의 HTTP Response를 Server로부터 받음
```

반면:

```text
Network Error

Server에 연결 자체가 되지 않음
DNS 문제
Connection 문제
등
```

이다.

---

# 88. 404의 의미

```text
Server까지 Request가 도착할 수 있었고

Server가 Response를 보냈지만

요청한 Resource를 찾지 못했다.
```

는 상황일 수 있다.

---

# 89. Connection Refused

```text
Server Process가 실행되지 않음

Port가 틀림
```

등의 이유로 Connection 자체가 만들어지지 않은 상황일 수 있다.

---

# 90. 둘은 다른 문제

```text
404
≠
Server가 꺼져 있음
```

이다.

---

# 91. Browser DevTools

HTTP Request/Response를 확인하는 가장 중요한 도구 중 하나이다.

```text
F12
↓
Network
```

---

# 92. Request 선택

Network 목록에서 Request 하나를 선택한다.

---

# 93. Headers

여기서:

```text
Request URL

Request Method

Status Code

Request Headers

Response Headers
```

등을 확인할 수 있다.

---

# 94. Payload

POST Request 등에 포함된:

```text
JSON

Form Data

Request Payload
```

를 확인할 수 있다.

---

# 95. Response

Server가 보내준 Response Body를 확인할 수 있다.

---

# 96. Preview

JSON Response 등을 Browser가 보기 편한 형태로 보여줄 수 있다.

---

# 97. Debugging 핵심

API가 이상하면:

```text
Console만 보지 말고

Network도 확인한다.
```

---

# 98. Console

주로:

```text
JavaScript Error

console.log()
```

등을 확인한다.

---

# 99. Network

주로:

```text
HTTP Request

HTTP Response

Status Code

Headers

Payload
```

를 확인한다.

---

# 100. curl로 Request 확인

Browser 없이 Request를 보낼 수도 있다.

```powershell
curl.exe -X GET "https://example.com"
```

---

# 101. Response Header 확인

curl에서는:

```powershell
curl.exe -i "https://example.com"
```

처럼 Response Header까지 확인할 수 있다.

---

# 102. 상세 통신 확인

```powershell
curl.exe -v "https://example.com"
```

을 사용하면 Connection과 Request/Response 관련 더 자세한 정보를 확인할 수 있다.

---

# 103. POST JSON

```powershell
curl.exe -X POST "https://example.com/products" `
    -H "Content-Type: application/json" `
    -d '{"name":"Notebook","price":1500000}'
```

---

# 104. 각각의 의미

```text
-X POST
→ Method

URL
→ Request 대상

-H
→ Header

-d
→ Body Data
```

---

# 105. curl이 중요한 이유

Frontend 없이도:

```text
Backend API만 따로 테스트
```

할 수 있기 때문이다.

---

# 106. 예

Frontend에서 API가 안 된다.

먼저 curl로:

```text
POST /predict
```

를 테스트한다.

---

# 107. curl 성공

```text
Backend API
→ 정상일 가능성이 높음
```

그다음:

```text
Frontend fetch

CORS

JavaScript

FormData
```

등을 확인한다.

---

# 108. curl도 실패

```text
Backend

URL

Method

Body

Server
```

등을 먼저 확인한다.

---

# 109. Postman 같은 도구

HTTP API를 테스트할 때 GUI 기반 API Client를 사용할 수도 있다.

하지만 기본 원리는 동일하다.

```text
Method

URL

Headers

Body
```

를 설정하고 Request를 보낸다.

---

# 110. FastAPI Swagger UI

FastAPI에서는 기본 설정에서 API 문서를 Browser에서 테스트할 수 있는 Swagger UI가 제공된다.

보통 개발 Server에서:

```text
/docs
```

Path를 통해 접근한다.

---

# 111. 예

```text
http://127.0.0.1:8000/docs
```

---

# 112. Swagger UI에서도

결국 내부적으로:

```text
Method

URL

Parameters

Body
```

를 구성하여 HTTP Request를 보내는 것이다.

---

# 113. 즉 도구만 다르다

```text
Browser fetch

curl

Swagger UI

API Client
```

모두 HTTP Client 역할을 할 수 있다.

---

# 114. 핵심은 같다

```text
Request

↓

Response
```

이다.

---

# 115. Request Header와 Response Header

같은 Header 이름이 Request와 Response 양쪽에서 사용되는 경우도 있고 특정 방향에서 주로 사용하는 Header도 있다.

따라서:

```text
Request Header

Response Header
```

문맥을 보고 구분한다.

---

# 116. Content-Type 예

Request:

```http
Content-Type: application/json
```

이면:

```text
Request Body가 JSON
```

이다.

Response:

```http
Content-Type: application/json
```

이면:

```text
Response Body가 JSON
```

이다.

---

# 117. Content-Length

Message Body의 크기와 관련된 Header를 볼 수 있다.

예:

```http
Content-Length: 123
```

---

# 118. Location

Redirect 또는 Resource 생성 등에서:

```http
Location: /products/10
```

같은 Header를 볼 수 있다.

---

# 119. Cache 관련 Header

Browser Cache 동작과 관련하여:

```text
Cache-Control

ETag

Last-Modified
```

등의 Header도 볼 수 있다.

---

# 120. 지금 다 외울 필요는 없다

우선 중요한 Header:

```text
Host

Content-Type

Accept

Authorization

Cookie
```

정도를 이해한다.

---

# 121. HTTP Request를 읽는 순서

Request를 보면:

```text
1. Method는?

2. Path는?

3. Query는?

4. Header는?

5. Body는?
```

순서로 본다.

---

# 122. HTTP Response를 읽는 순서

Response를 보면:

```text
1. Status Code는?

2. Header는?

3. Body는?
```

순서로 본다.

---

# 123. 예제 분석

Request:

```http
POST /predict HTTP/1.1
Host: localhost:8000
Accept: application/json
Content-Type: multipart/form-data; boundary=...

[Image]
```

---

# 124. 분석

```text
POST
→ Prediction 처리 요청

/predict
→ Prediction API

Host
→ localhost:8000

Accept
→ JSON Response 희망

Content-Type
→ multipart/form-data

Body
→ Image
```

---

# 125. Response

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "label": "Pneumonia",
    "score": 0.93
}
```

---

# 126. 분석

```text
200
→ 성공

Content-Type
→ JSON

Body
→ Prediction 결과
```

---

# 127. 전체 AI 흐름

```text
사용자
↓
Image 선택
↓
JavaScript
↓
HTTP Request

POST /predict
Headers
Image Body

↓
FastAPI
↓
Image Preprocessing
↓
PyTorch
↓
Prediction
↓
HTTP Response

200
Headers
JSON Body

↓
JavaScript
↓
DOM
↓
사용자
```

---

# 128. HTTP와 Backend Function

HTTP Request가 들어오면 Backend Framework가 Request를 분석하고 적절한 Function을 실행한다.

예:

```python
@app.get("/products")
def get_products():

    return [
        {
            "id": 1,
            "name": "Notebook"
        }
    ]
```

---

# 129. Request

```text
GET /products
```

---

# 130. Function 실행

```python
get_products()
```

---

# 131. Response

FastAPI가 Python Data를 HTTP Response 형태로 변환하여 Client에 전달할 수 있다.

---

# 132. 이것이 중요한 이유

나중에 FastAPI Code를 보면:

```python
@app.post("/predict")
```

가 단순한 Python Decorator처럼만 보일 수 있다.

하지만 실제 의미는:

```text
POST /predict HTTP Request가 들어오면

이 Function으로 처리한다.
```

라는 Web Routing과 연결되어 있다.

---

# 133. Request와 Function Argument

FastAPI에서는:

```text
Path Parameter

Query Parameter

Request Body

Header

Cookie

File
```

등을 Python Function Parameter와 연결할 수 있다.

---

# 134. 예

```python
@app.get("/products/{product_id}")
def get_product(
    product_id: int
):

    return {
        "product_id":
            product_id
    }
```

---

# 135. Request

```text
GET /products/10
```

---

# 136. 연결

```text
10
↓
product_id
↓
Python Function
```

---

# 137. Query 예

```python
@app.get("/products")
def get_products(
    page: int = 1
):

    return {
        "page": page
    }
```

---

# 138. Request

```text
GET /products?page=2
```

---

# 139. 연결

```text
page=2
↓
page Parameter
↓
Python Function
```

---

# 140. Request Body도 연결

나중에는 Pydantic Model 등을 이용하여 JSON Request Body를 Python Object로 Validation하고 처리할 수 있다.

---

# 141. 결국 FastAPI도

```text
HTTP Request
↓
Python Data로 변환
↓
Python Function 실행
↓
Python 결과
↓
HTTP Response로 변환
```

하는 역할을 도와주는 Framework라고 볼 수 있다.

---

# 142. 핵심 비교

```text
HTTP Request

Method
Target
Headers
Body

        ↓

Backend

        ↓

HTTP Response

Status Code
Headers
Body
```

---

# 143. 가장 중요한 질문

Request를 볼 때:

```text
누가 보냈는가?

어디로 보냈는가?

어떤 Method인가?

어떤 Header인가?

어떤 Data를 보냈는가?
```

Response를 볼 때:

```text
성공했는가?

Status Code는?

어떤 형식인가?

어떤 Data가 왔는가?
```

를 생각한다.

---

# 144. 한 문장으로 정리

HTTP Request와 Response는:

```text
Client가 Method, Target, Header, Body 등을 이용해
Server에게 작업을 요청하고,

Server가 이를 처리한 뒤
Status Code, Header, Body 등을 이용해
결과를 Client에게 반환하는 HTTP의 핵심 통신 구조
```

라고 이해할 수 있다.

---

# 145. 다음 단계

이제 Request와 Response의 구조를 이해했으므로 다음에는:

```text
GET
POST
PUT
PATCH
DELETE
```

각 Method의 차이와:

```text
200
201
204
400
401
403
404
405
422
500
```

Status Code를 더 자세히 공부한다.