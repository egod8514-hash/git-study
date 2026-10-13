# API & REST API

## 1. API란?

API는:

```text
Application Programming Interface
```

의 약자이다.

쉽게 말하면:

```text
Program과 Program이
정해진 방법으로 기능이나 Data를 주고받기 위한 Interface
```

라고 이해할 수 있다.

---

# 2. Interface란?

Interface는 서로 다른 두 대상이 상호작용하기 위한 접점 또는 규칙이라고 생각할 수 있다.

예:

```text
사용자
↓
버튼
↓
Application
```

버튼은 사용자와 Application 사이의 Interface 역할을 한다.

Program끼리도 서로 기능을 사용하기 위한 Interface가 필요할 수 있다.

---

# 3. API 예

쇼핑몰 Frontend가 있다고 하자.

Frontend가 직접 Database에 접속하는 대신:

```text
Frontend
↓
API
↓
Backend
↓
Database
```

구조를 사용할 수 있다.

---

# 4. 상품 조회

Frontend:

```text
상품 목록이 필요함
```

API:

```text
GET /products
```

Backend:

```text
Database에서 상품 조회
```

Response:

```json
[
    {
        "id": 1,
        "name": "Notebook"
    }
]
```

---

# 5. API가 없다면?

Frontend가 Backend 내부 코드나 Database 구조를 모두 알아야 한다면 System 간 결합이 매우 강해질 수 있다.

API를 사용하면:

```text
Frontend

"GET /products로 요청하면
상품 목록을 받을 수 있다."
```

라는 약속만 알고 사용할 수 있다.

---

# 6. API는 약속이다

예:

```text
Method
→ POST

Path
→ /predict

Request
→ Image File

Response
→ JSON
```

이런 규칙을 Frontend와 Backend가 공유할 수 있다.

---

# 7. AI API 예

```text
POST /predict
```

Request:

```text
Image File
```

Response:

```json
{
    "label": "Pneumonia",
    "score": 0.93
}
```

---

# 8. Frontend 입장

Frontend는 내부에서:

```text
PyTorch가 어떻게 실행되는지

CNN 구조가 무엇인지

Tensor를 어떻게 만드는지
```

까지 반드시 알 필요는 없다.

API 사용 방법만 알면 된다.

---

# 9. Backend 입장

Backend 내부에서는:

```text
Image 받기
↓
전처리
↓
Tensor
↓
Model
↓
Prediction
↓
JSON Response
```

를 처리한다.

---

# 10. API의 장점

```text
기능 분리

Frontend / Backend 분리

재사용

유지보수

다른 Application과 연동

내부 구현 숨김
```

등의 장점이 있다.

---

# 11. Web API

HTTP를 이용해 제공되는 API를 Web API라고 부를 수 있다.

예:

```text
GET /products

POST /login

POST /predict
```

---

# 12. 모든 API가 HTTP API인가?

아니다.

API라는 개념은 HTTP보다 넓다.

예를 들어 Programming Library가 제공하는 Function이나 Operating System API 등도 API라고 부를 수 있다.

하지만 지금 우리가 공부하는 것은 주로:

```text
HTTP 기반 Web API
```

이다.

---

# 13. Web API의 주요 요소

지금까지 공부한 것이 그대로 사용된다.

```text
URL

HTTP Method

Headers

Request Body

Status Code

Response Body

JSON
```

---

# 14. API Request 예

```http
POST /products HTTP/1.1
Content-Type: application/json

{
    "name": "Notebook",
    "price": 1500000
}
```

---

# 15. API Response 예

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

# 16. Endpoint란?

API를 공부하면:

```text
Endpoint
```

라는 말을 매우 자주 보게 된다.

Endpoint는 API에서 Client가 접근할 수 있는 특정 지점이라고 이해할 수 있다.

---

# 17. 예

```text
GET /products
```

```text
POST /products
```

```text
GET /products/10
```

```text
POST /predict
```

같은 API 접근 지점을 Endpoint라고 부른다.

---

# 18. Endpoint를 볼 때

처음에는:

```text
Method + Path
```

를 함께 보는 습관을 만든다.

---

# 19. 왜 Method까지 같이 볼까?

다음 두 Request는 Path가 같다.

```text
GET /products

POST /products
```

하지만 동작은 다르다.

---

# 20. 의미

```text
GET /products
→ 상품 목록 조회

POST /products
→ 상품 생성
```

이다.

---

# 21. API Endpoint

따라서 API를 정리할 때:

| Method | Path | 기능 |
|---|---|---|
| GET | /products | 상품 목록 조회 |
| POST | /products | 상품 생성 |
| GET | /products/{id} | 상품 한 개 조회 |
| PATCH | /products/{id} | 상품 일부 수정 |
| DELETE | /products/{id} | 상품 삭제 |

처럼 표현할 수 있다.

---

# 22. REST란?

REST는:

```text
Representational State Transfer
```

의 약자이다.

Web의 특성을 활용하여 Network Application을 설계하기 위한 Architectural Style이다.

---

# 23. 중요한 점

REST는:

```text
Programming Language

Framework

Library

Protocol
```

이 아니다.

설계 방식:

```text
Architectural Style
```

이다.

---

# 24. REST API

REST의 원칙을 따르도록 설계한 API를 일반적으로:

```text
REST API
```

라고 부른다.

---

# 25. RESTful

REST 원칙을 잘 따르는 API를:

```text
RESTful API
```

라고 표현하기도 한다.

---

# 26. REST에서 Resource

REST에서 중요한 개념 중 하나가:

```text
Resource
```

이다.

예:

```text
User

Product

Order

Post
```

등을 Resource로 생각할 수 있다.

---

# 27. Resource를 URL로 표현

상품 Resource:

```text
/products
```

회원 Resource:

```text
/users
```

주문 Resource:

```text
/orders
```

게시글 Resource:

```text
/posts
```

---

# 28. 특정 Resource

```text
/products/10
```

은:

```text
10번 Product Resource
```

를 나타내도록 설계할 수 있다.

---

# 29. Resource 중심 설계

REST 스타일 API에서는 URL을 동작 이름보다 Resource 중심으로 구성하는 패턴을 많이 사용한다.

예:

```text
/products
```

---

# 30. Method로 동작 표현

```text
GET /products
POST /products
```

처럼 HTTP Method를 사용하여 Resource에 대한 작업의 의미를 표현한다.

---

# 31. 좋지 않은 예라고 단정할 수는 없지만

다음처럼 동작을 URL에 모두 표현하는 방식도 존재할 수 있다.

```text
/getProducts

/createProduct

/deleteProduct
```

하지만 REST 스타일에서는 Resource 중심의 URL을 선호한다.

---

# 32. REST 스타일

```text
GET    /products

POST   /products

GET    /products/10

PATCH  /products/10

DELETE /products/10
```

---

# 33. 읽는 방법

```text
GET
+
/products
```

```text
상품 Resource 조회
```

---

# 34. 생성

```text
POST
+
/products
```

```text
상품 Resource 생성
```

---

# 35. 수정

```text
PATCH
+
/products/10
```

```text
10번 상품 일부 수정
```

---

# 36. 삭제

```text
DELETE
+
/products/10
```

```text
10번 상품 삭제
```

---

# 37. CRUD와 REST API

CRUD:

```text
Create
Read
Update
Delete
```

와 HTTP Method를 연결하면:

```text
Create
→ POST

Read
→ GET

Update
→ PUT / PATCH

Delete
→ DELETE
```

처럼 설계하는 경우가 많다.

---

# 38. CRUD API 예

| CRUD | Method | Path |
|---|---|---|
| Create | POST | /products |
| Read All | GET | /products |
| Read One | GET | /products/{id} |
| Update | PUT/PATCH | /products/{id} |
| Delete | DELETE | /products/{id} |

---

# 39. URL에 동사보다 명사

REST 스타일에서는:

```text
/products
/users
/orders
```

처럼 Resource를 나타내는 명사를 사용하는 패턴이 흔하다.

---

# 40. 예

다음보다:

```text
GET /getProducts
```

다음과 같이 표현할 수 있다.

```text
GET /products
```

---

# 41. 이유

이미:

```text
GET
```

이라는 Method가 조회라는 의미를 가지고 있기 때문에 URL에서 다시:

```text
get
```

을 반복할 필요가 없는 설계가 가능하다.

---

# 42. 삭제도 동일

다음보다:

```text
POST /deleteProduct/10
```

REST 스타일에서는:

```text
DELETE /products/10
```

처럼 설계할 수 있다.

---

# 43. Collection

```text
/products
```

는 Product Resource들의 Collection을 표현할 수 있다.

---

# 44. Individual Resource

```text
/products/10
```

은 특정 Product를 표현할 수 있다.

---

# 45. 복수형

Resource Collection URL에는:

```text
/products

/users

/posts
```

처럼 복수형 명사를 사용하는 Convention이 흔하다.

반드시 모든 API가 그래야 하는 절대 규칙은 아니지만 일관성을 유지하는 것이 중요하다.

---

# 46. Path Parameter

```text
/products/10
```

에서:

```text
10
```

을 Product ID로 사용할 수 있다.

---

# 47. FastAPI에서는

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

처럼 연결할 수 있다.

---

# 48. Request

```text
GET /products/10
```

---

# 49. 결과

```text
10
↓
product_id
↓
Python Function
```

---

# 50. Query Parameter

Resource 목록에 조건을 적용할 때 Query Parameter를 사용할 수 있다.

예:

```text
GET /products?category=computer
```

---

# 51. Pagination

```text
GET /products?page=2&size=20
```

---

# 52. 정렬

```text
GET /products?sort=price
```

---

# 53. 검색

```text
GET /products?q=notebook
```

---

# 54. Path와 Query 구분

```text
/products/10
→ 특정 Resource

/products?category=computer
→ Collection에 조건 적용
```

처럼 사용하는 패턴이 흔하다.

---

# 55. REST에서 Representation

REST의 이름에:

```text
Representational
```

이 들어간다.

Client와 Server는 Resource 자체를 물리적으로 전달하는 것이 아니라 Resource의 표현을 주고받는다.

---

# 56. 예

Product Resource가 Database에 존재한다고 하자.

Database:

```text
id = 10
name = Notebook
price = 1500000
```

---

# 57. JSON Representation

Client에게는:

```json
{
    "id": 10,
    "name": "Notebook",
    "price": 1500000
}
```

처럼 표현하여 전달할 수 있다.

---

# 58. Resource와 JSON은 같은 것인가?

아니다.

```text
Resource
→ 개념적인 대상

JSON
→ Resource를 표현하는 방법 중 하나
```

이다.

---

# 59. REST와 JSON도 같은 것이 아니다

REST API가 반드시 JSON만 사용해야 하는 것은 아니다.

다른 Representation도 가능하다.

하지만 현대적인 Web API에서는 JSON을 매우 많이 사용한다.

---

# 60. REST와 HTTP

REST는 HTTP 자체와 같은 개념이 아니다.

```text
HTTP
→ Protocol

REST
→ Architectural Style
```

이다.

---

# 61. REST API에서 HTTP 활용

REST 스타일 Web API에서는 HTTP의:

```text
Method

URL

Status Code

Headers
```

등을 의미에 맞게 활용한다.

---

# 62. Status Code도 중요

API가 항상:

```text
200
```

만 반환하는 것은 좋은 설계라고 보기 어렵다.

상황에 맞는 Status Code를 사용할 수 있다.

---

# 63. 조회 성공

```text
GET /products/10
↓
200 OK
```

---

# 64. 생성 성공

```text
POST /products
↓
201 Created
```

---

# 65. 삭제 성공

```text
DELETE /products/10
↓
204 No Content
```

처럼 설계할 수 있다.

---

# 66. Resource 없음

```text
GET /products/999
↓
404 Not Found
```

---

# 67. 잘못된 Method

```text
POST /products/10
```

을 Server가 지원하지 않는다면:

```text
405 Method Not Allowed
```

가 될 수 있다.

---

# 68. 인증 필요

```text
GET /mypage
↓
401 Unauthorized
```

같은 Response를 사용할 수 있다.

---

# 69. 권한 없음

```text
DELETE /admin/users/10
↓
403 Forbidden
```

같은 Response를 사용할 수 있다.

---

# 70. REST와 Stateless

REST의 중요한 제약 중 하나는:

```text
Stateless
```

이다.

---

# 71. 의미

각 Request에는 Server가 해당 Request를 이해하고 처리하는 데 필요한 정보가 포함되어야 한다.

Server가 이전 Request의 대화 상태에 의존해서 다음 Request를 이해하는 방식은 REST의 Stateless 제약과 맞지 않는다.

---

# 72. HTTP Stateless와 REST Stateless

서로 관련이 있지만 완전히 같은 문장으로만 외우기보다는:

```text
HTTP
→ 기본적으로 Stateless한 Request/Response Protocol

REST
→ Stateless를 Architectural Constraint로 요구
```

라고 구분한다.

---

# 73. Login은 불가능한가?

아니다.

Stateless라고 해서 Login을 만들 수 없다는 뜻이 아니다.

인증 정보를:

```text
Cookie

Token

Authorization Header
```

등을 통해 Request와 함께 전달할 수 있다.

---

# 74. API Contract

Frontend와 Backend는 API 사용 방법을 약속해야 한다.

예:

```text
Endpoint
POST /products

Request
application/json

{
    "name": string,
    "price": number
}

Response
201

{
    "id": number,
    "name": string,
    "price": number
}
```

---

# 75. 이것이 API Contract

즉:

```text
어디로 요청?

어떤 Method?

무엇을 보내야 함?

무엇을 받을 수 있음?

성공/실패는 어떻게 표현?
```

등을 정의한다.

---

# 76. Frontend와 Backend 협업

Frontend 개발자는:

```text
POST /products

name 필요

price 필요

성공하면 201

JSON Response
```

를 알면 된다.

---

# 77. Backend 개발자는

그 Contract에 맞춰 API를 구현한다.

---

# 78. Contract가 맞지 않으면

Frontend:

```json
{
    "productName": "Notebook"
}
```

Backend 기대:

```json
{
    "name": "Notebook"
}
```

이면 문제가 발생할 수 있다.

---

# 79. API Documentation

그래서 API 문서가 중요하다.

API 문서에는 보통:

```text
Method

Path

Parameters

Request Body

Response

Status Code

Authentication
```

등을 작성한다.

---

# 80. FastAPI

FastAPI는 API 문서를 자동으로 생성하는 기능을 제공한다.

개발 Server에서는 일반적으로:

```text
/docs
```

를 통해 Swagger UI를 확인할 수 있다.

---

# 81. 예

```text
http://127.0.0.1:8000/docs
```

---

# 82. Swagger UI

Swagger UI에서는 API의:

```text
Method

Path

Parameter

Request Body

Response
```

등을 확인하고 직접 Request를 보내볼 수 있다.

---

# 83. FastAPI와 OpenAPI

FastAPI는 API Schema를:

```text
OpenAPI
```

형식으로 생성할 수 있다.

Swagger UI는 이를 이용해 대화형 API 문서를 제공한다.

---

# 84. API 테스트 방법

API는 여러 방법으로 테스트할 수 있다.

```text
Browser

JavaScript fetch()

curl

Swagger UI

API Client
```

---

# 85. GET은 Browser에서도 쉽게 테스트

예:

```text
http://127.0.0.1:8000/products
```

---

# 86. POST는?

Browser 주소창은 Page Navigation에서 주로 GET을 사용한다.

POST Request를 직접 테스트하려면:

```text
curl

Swagger UI

Frontend fetch()

API Client
```

등을 사용할 수 있다.

---

# 87. curl 예

```powershell
curl.exe -X POST "http://127.0.0.1:8000/products" `
    -H "Content-Type: application/json" `
    -d '{"name":"Notebook","price":1500000}'
```

---

# 88. 해석

```text
-X POST
→ Method

URL
→ Endpoint

-H
→ Header

-d
→ JSON Body
```

---

# 89. REST API 전체 예

상품 API:

```text
GET /products
→ 전체 상품 조회

POST /products
→ 상품 생성

GET /products/{id}
→ 특정 상품 조회

PUT /products/{id}
→ 상품 전체 교체

PATCH /products/{id}
→ 상품 일부 수정

DELETE /products/{id}
→ 상품 삭제
```

---

# 90. FastAPI로 표현

```python
from fastapi import FastAPI


app = FastAPI()


@app.get("/products")
def get_products():

    return []


@app.post("/products")
def create_product():

    return {
        "message":
            "created"
    }


@app.get(
    "/products/{product_id}"
)
def get_product(
    product_id: int
):

    return {
        "product_id":
            product_id
    }


@app.patch(
    "/products/{product_id}"
)
def update_product(
    product_id: int
):

    return {
        "product_id":
            product_id
    }


@app.delete(
    "/products/{product_id}"
)
def delete_product(
    product_id: int
):

    return {
        "message":
            "deleted"
    }
```

---

# 91. Decorator

다음:

```python
@app.get("/products")
```

은 FastAPI에게:

```text
GET /products Request가 들어오면
아래 Function을 사용한다.
```

라는 Routing 정보를 등록한다.

---

# 92. Function

```python
def get_products():

    return []
```

는 실제 Backend Logic을 처리한다.

---

# 93. 연결

```text
GET /products
↓
FastAPI Router
↓
get_products()
↓
Python Result
↓
JSON Response
```

---

# 94. API와 Database

실제 Service에서는:

```text
GET /products
↓
FastAPI
↓
SQLAlchemy
↓
Database
↓
Product Data
↓
FastAPI
↓
JSON
↓
Frontend
```

처럼 연결할 수 있다.

---

# 95. API와 AI

AI Service에서는:

```text
POST /predict
↓
FastAPI
↓
PyTorch
↓
Prediction
↓
JSON
↓
Frontend
```

구조를 만들 수 있다.

---

# 96. AI API 설계

예:

```text
POST /predict
```

Request:

```text
multipart/form-data

file
→ Image
```

Response:

```json
{
    "prediction": 1,
    "label": "Pneumonia",
    "probability": 0.93
}
```

---

# 97. 왜 /predict인가?

AI Prediction은 전형적인 CRUD Resource 작업과는 조금 다르게:

```text
Prediction이라는 처리 작업
```

을 나타낸다.

이런 Action-oriented Endpoint가 실제 API에서 사용되기도 한다.

---

# 98. 중요한 점

REST를 배웠다고 모든 API를 억지로 CRUD 형태로 만들 필요는 없다.

API의 목적과 Domain에 맞게 명확하게 설계하는 것이 중요하다.

---

# 99. AI API 예

다음과 같이 설계할 수 있다.

```text
POST /predict
```

또는 Service 구조에 따라:

```text
POST /predictions
```

처럼 Prediction Resource 생성 관점으로 설계할 수도 있다.

---

# 100. 둘 중 무조건 하나만 정답은 아니다

API의:

```text
목적

일관성

사용성

Domain
```

을 고려해 설계한다.

---

# 101. Versioning

API가 변경될 때 Version을 URL에 표현하는 방식도 있다.

예:

```text
/api/v1/products
```

---

# 102. 다음 Version

```text
/api/v2/products
```

---

# 103. 왜 Version을 사용할까?

기존 Client가 사용하는 API와 호환성을 유지하면서 새로운 API를 제공해야 할 수 있기 때문이다.

---

# 104. Base URL

API에서 공통 앞부분을:

```text
Base URL
```

이라고 부를 수 있다.

예:

```text
https://api.example.com
```

---

# 105. Endpoint Path

```text
/products
```

---

# 106. 전체 URL

```text
https://api.example.com/products
```

---

# 107. Version 포함

```text
https://api.example.com/api/v1/products
```

처럼 구성할 수도 있다.

---

# 108. Frontend 설정

JavaScript:

```javascript
const API_BASE_URL =
    "http://localhost:8000";
```

---

# 109. 사용

```javascript
const response =
    await fetch(
        `${API_BASE_URL}/products`
    );
```

---

# 110. 장점

Backend 주소가 바뀌면:

```javascript
API_BASE_URL
```

을 중심으로 관리할 수 있다.

---

# 111. Local 개발

```text
http://localhost:8000
```

---

# 112. 배포

```text
https://api.example.com
```

처럼 달라질 수 있다.

---

# 113. CORS

Frontend와 Backend의 Origin이 다르면 Browser에서 CORS 정책을 만나게 될 수 있다.

예:

```text
Frontend
http://localhost:5500

Backend
http://localhost:8000
```

---

# 114. Origin

Origin은 기본적으로:

```text
Scheme
+
Host
+
Port
```

로 결정된다.

---

# 115. 위 예에서는

Port가:

```text
5500
8000
```

으로 다르므로 다른 Origin이다.

---

# 116. CORS는 Backend에서 다룰 수 있다

FastAPI에서는 Middleware 등을 사용해 허용할 Origin을 설정할 수 있다.

이 부분은 FastAPI Chapter에서 자세히 공부한다.

---

# 117. API와 Authentication

모든 API를 누구나 사용하도록 만들지는 않는다.

예:

```text
GET /mypage

POST /orders

DELETE /users/10
```

등은 인증이나 권한 확인이 필요할 수 있다.

---

# 118. Authorization Header

예:

```http
Authorization: Bearer <token>
```

형태를 사용할 수 있다.

---

# 119. API Key

외부 API에서는:

```text
API Key
```

를 이용해 Client를 식별하거나 사용 권한을 확인하는 경우도 있다.

---

# 120. Secret 주의

다음 정보는 Frontend Source Code나 GitHub에 그대로 올리지 않는다.

```text
API Key

Database Password

Secret Key

Access Token
```

---

# 121. Frontend는 사용자에게 공개된다

Browser에 전달되는 JavaScript는 사용자가 확인할 수 있다.

따라서 비밀 API Key를 Frontend JavaScript에 하드코딩하는 방식은 피해야 한다.

---

# 122. 외부 AI API를 사용한다면

일반적으로:

```text
Browser
↓
우리 Backend
↓
External AI API
```

구조를 사용하여 Secret을 Server 측에서 관리하는 방식을 고려한다.

---

# 123. API Error Handling

Frontend는 성공 Response만 생각하면 안 된다.

예:

```text
200

400

401

403

404

422

429

500
```

등을 처리해야 한다.

---

# 124. JavaScript

```javascript
const response =
    await fetch(url);


if (!response.ok) {

    const message =
        await response.text();

    throw new Error(
        `${response.status}: ${message}`
    );

}


const data =
    await response.json();
```

---

# 125. Backend도 적절한 Error Response

예:

```text
상품 없음
→ 404

인증 필요
→ 401

권한 없음
→ 403

Server 내부 Error
→ 500
```

등을 상황에 맞게 사용한다.

---

# 126. 좋은 API의 특징

좋은 API를 만들 때는:

```text
일관된 URL

적절한 HTTP Method

적절한 Status Code

명확한 Request Schema

명확한 Response Schema

Error 처리

Documentation

Security
```

등을 고려한다.

---

# 127. 이름도 중요

다음처럼 혼합하면 이해하기 어려워질 수 있다.

```text
/getProduct

/products/create

/delete_product

userList
```

---

# 128. 일관성

예:

```text
/products

/products/{id}

/users

/users/{id}
```

처럼 일정한 규칙을 유지하면 API를 이해하기 쉽다.

---

# 129. API 설계에서 가장 중요한 질문

Endpoint를 만들 때:

```text
어떤 Resource / 기능인가?

어떤 Method인가?

어떤 Path인가?

어떤 Parameter가 필요한가?

어떤 Body가 필요한가?

성공하면 무엇을 반환하는가?

실패하면 어떤 Status Code인가?

인증이 필요한가?
```

를 생각한다.

---

# 130. API 설계표 예

| Method | Path | Request | Response | Status |
|---|---|---|---|---|
| GET | /products | - | 상품 목록 | 200 |
| POST | /products | 상품 JSON | 생성 상품 | 201 |
| GET | /products/{id} | id | 상품 | 200/404 |
| PATCH | /products/{id} | 수정 JSON | 수정 상품 | 200/404 |
| DELETE | /products/{id} | id | 없음 | 204/404 |

---

# 131. AI API 설계표

| Method | Path | Request | Response |
|---|---|---|---|
| POST | /predict | Image File | Prediction JSON |
| GET | /health | 없음 | Server 상태 |
| GET | /model-info | 없음 | Model 정보 |

---

# 132. Health Check

Server가 정상적으로 실행 중인지 확인하기 위한 Endpoint를 만들 수 있다.

예:

```text
GET /health
```

Response:

```json
{
    "status": "ok"
}
```

---

# 133. 왜 필요한가?

배포 환경에서:

```text
Server가 살아 있는가?

Application이 Response 가능한가?
```

등을 확인하는 데 활용할 수 있다.

---

# 134. Model Info

AI Service라면:

```text
GET /model-info
```

같은 Endpoint를 만들 수도 있다.

Response 예:

```json
{
    "model": "ChestXrayClassifier",
    "classes": [
        "Normal",
        "Pneumonia"
    ]
}
```

---

# 135. Prediction

```text
POST /predict
```

Response:

```json
{
    "label": "Pneumonia",
    "probability": 0.93
}
```

---

# 136. AI Web Service API

결국 다음 구조를 만들 수 있다.

```text
Frontend
↓
POST /predict
↓
FastAPI
↓
Image Validation
↓
Preprocessing
↓
PyTorch Model
↓
Prediction
↓
JSON Response
↓
Frontend
```

---

# 137. API는 Model과 Frontend 사이의 다리

```text
Frontend
      ↕
     API
      ↕
Backend
      ↕
AI Model
```

이라고 생각할 수 있다.

---

# 138. Database가 추가되면

```text
Frontend
      ↕
     API
      ↕
FastAPI
   ↙      ↘
AI Model   Database
```

같은 구조도 만들 수 있다.

---

# 139. Docker가 추가되면

나중에는:

```text
Frontend
↓
HTTP
↓
FastAPI Container
↓
AI Model
↓
Database
```

등을 Docker 환경에서 실행할 수도 있다.

---

# 140. 지금까지 배운 것 연결

```text
Network
→ Computer 간 Data 통신

IP
→ Network의 Address

Port
→ Application / Service 구분

DNS
→ Domain을 Address와 연결

URL
→ Resource 위치

HTTP
→ Request / Response Protocol

Method
→ Request의 의미

Status Code
→ Response 처리 결과

JSON
→ Data 표현 형식

API
→ Program 간 기능 사용 Interface

REST
→ Web API 설계 Architectural Style
```

---

# 141. 한 번에 보기

```text
JavaScript
↓
POST
↓
http://localhost:8000/products
↓
HTTP
↓
JSON
↓
FastAPI
↓
Python
↓
Database
↓
Python
↓
JSON
↓
HTTP Response
↓
JavaScript
```

---

# 142. AI라면

```text
JavaScript
↓
POST /predict
↓
HTTP
↓
Image
↓
FastAPI
↓
PyTorch
↓
Prediction
↓
JSON
↓
HTTP Response
↓
JavaScript
↓
DOM
```

---

# 143. 핵심 구분

```text
API
→ Program 간 기능을 사용하기 위한 Interface

Web API
→ HTTP 등을 이용하는 Web 기반 API

REST
→ API 설계에 사용할 수 있는 Architectural Style

REST API
→ REST 원칙을 적용한 API

FastAPI
→ Python으로 Web API를 만드는 Framework
```

---

# 144. API와 FastAPI는 같은 것이 아니다

```text
API
→ 개념 / Interface

FastAPI
→ API를 구현하는 Python Framework
```

이다.

---

# 145. REST와 FastAPI도 같은 것이 아니다

FastAPI로:

```text
REST 스타일 API
```

를 만들 수도 있고 다른 형태의 HTTP API도 만들 수 있다.

---

# 146. HTTP와 API도 같은 것이 아니다

```text
HTTP
→ 통신 Protocol

API
→ Program이 기능을 제공하고 사용하는 Interface
```

이다.

Web API에서 HTTP를 많이 사용하는 것이다.

---

# 147. JSON과 API도 같은 것이 아니다

```text
JSON
→ Data Format

API
→ Interface
```

이다.

API에서 JSON을 많이 사용하는 것이다.

---

# 148. URL과 API도 같은 것이 아니다

```text
URL
→ Resource 위치 표현

API Endpoint
→ Client가 API 기능을 호출하는 접근 지점
```

이다.

---

# 149. 최종 구조

```text
Client
↓
URL
↓
HTTP Method
↓
Headers
↓
Body
↓
API Endpoint
↓
Backend Logic
↓
Database / AI Model
↓
Status Code
↓
JSON Response
↓
Client
```

---

# 150. 한 문장으로 정리

API는:

```text
Program과 Program이 정해진 규칙에 따라
기능과 Data를 주고받기 위한 Interface
```

이고,

REST API는:

```text
Resource를 중심으로 URL을 구성하고
HTTP의 Method, Status Code 등의 의미를 활용하여
설계하는 Web API 방식
```

이라고 이해할 수 있다.

---

# 151. 다음 단계

이제:

```text
Network
IP
Port
DNS
URL
HTTP
Request
Response
Method
Status Code
JSON
API
REST
```

의 기본 흐름을 공부했다.

다음 단계에서는 Python으로 직접:

```text
GET /products

POST /products

POST /predict
```

같은 API를 만들어본다.

그때 사용할 Framework가:

```text
FastAPI
```

이다.