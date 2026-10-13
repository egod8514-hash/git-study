# HTTP Method & Status Code

## 1. HTTP Request와 Response 다시 보기

HTTP 통신은 기본적으로:

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

구조이다.

Request에서는:

```text
Method
URL
Headers
Body
```

를 중요하게 본다.

Response에서는:

```text
Status Code
Headers
Body
```

를 중요하게 본다.

---

# 2. HTTP Method란?

HTTP Method는 Client가 Resource에 대해 어떤 종류의 작업을 원하는지 나타낸다.

대표적으로:

```text
GET
POST
PUT
PATCH
DELETE
```

를 자주 사용한다.

---

# 3. 쇼핑몰 예제

상품 API가 있다고 하자.

```text
/products
```

같은 Resource에 Method를 조합할 수 있다.

```text
GET    /products
POST   /products
GET    /products/10
PUT    /products/10
PATCH  /products/10
DELETE /products/10
```

---

# 4. GET

GET은 Resource를 조회할 때 주로 사용한다.

예:

```text
GET /products
```

의미:

```text
상품 목록을 조회한다.
```

---

# 5. 특정 Resource 조회

```text
GET /products/10
```

의미:

```text
10번 상품을 조회한다.
```

---

# 6. Query Parameter와 GET

조회 조건을 추가할 수 있다.

```text
GET /products?category=computer
```

의미:

```text
computer Category의 상품을 조회한다.
```

---

# 7. 검색

```text
GET /search?q=python
```

처럼 검색 조건을 Query Parameter로 전달할 수도 있다.

---

# 8. GET의 핵심

처음에는:

```text
GET
→ 조회
```

라고 기억한다.

---

# 9. GET과 Server Data

GET은 일반적으로 Server Resource의 상태를 변경하기 위한 Method로 사용하지 않는다.

즉:

```text
GET /products/10
```

을 호출했다고 상품 10번이 삭제되도록 설계하는 것은 적절하지 않다.

---

# 10. Safe Method

HTTP에서 GET은:

```text
Safe Method
```

로 정의된다.

이는 Client가 요청한 작업 자체가 Server Resource 상태의 변경을 요구하지 않는다는 의미이다.

---

# 11. 주의

GET Request가 Server에 아무런 변화도 절대로 발생시키지 않는다는 뜻은 아니다.

예:

```text
Access Log 기록

통계 기록

Cache 처리
```

같은 부수적인 변화는 발생할 수 있다.

핵심은:

```text
Client가 Resource 변경을 요청하는 Method가 아니다.
```

라는 것이다.

---

# 12. POST

POST는 Server에 Data를 보내 Resource를 생성하거나 특정 처리를 요청할 때 많이 사용한다.

예:

```text
POST /products
```

---

# 13. POST Body

```json
{
    "name": "Notebook",
    "price": 1500000
}
```

---

# 14. 의미

```text
새 상품을 생성해주세요.
```

와 같은 요청으로 설계할 수 있다.

---

# 15. Login

```text
POST /login
```

도 자주 볼 수 있다.

Body:

```json
{
    "email": "user@example.com",
    "password": "example"
}
```

---

# 16. AI Prediction

AI Service에서도:

```text
POST /predict
```

를 사용할 수 있다.

예:

```text
Image File
↓
POST /predict
↓
AI Model
↓
Prediction
```

---

# 17. 왜 Prediction에 POST를 사용할까?

Prediction 자체가 Database Resource를 반드시 생성하는 것은 아니다.

하지만:

```text
Image

Text

복잡한 JSON
```

등의 Data를 Request Body로 보내 Server가 처리하도록 하는 API에서는 POST를 사용할 수 있다.

---

# 18. POST의 핵심

처음에는:

```text
POST
→ Data를 보내 생성 또는 처리 요청
```

이라고 이해한다.

---

# 19. PUT

PUT은 지정한 Resource를 새 표현으로 전체 교체하는 의미로 사용하는 경우가 많다.

예:

```text
PUT /products/10
```

---

# 20. 기존 Data

```json
{
    "id": 10,
    "name": "Notebook",
    "price": 1500000,
    "stock": 20
}
```

---

# 21. PUT Request 예

```json
{
    "name": "Gaming Notebook",
    "price": 1800000,
    "stock": 15
}
```

Resource 전체를 갱신하는 의미로 설계할 수 있다.

---

# 22. PUT의 핵심

처음에는:

```text
PUT
→ 전체 교체 / 전체 수정
```

으로 기억한다.

실제 API의 세부 동작은 Server 설계에 따라 달라질 수 있다.

---

# 23. PATCH

PATCH는 Resource의 일부를 수정할 때 주로 사용한다.

예:

```text
PATCH /products/10
```

Body:

```json
{
    "price": 1700000
}
```

---

# 24. 의미

```text
10번 상품에서
price만 수정해주세요.
```

처럼 사용할 수 있다.

---

# 25. PATCH 핵심

```text
PATCH
→ 일부 수정
```

이라고 기억한다.

---

# 26. PUT vs PATCH

초기 학습에서는:

```text
PUT
→ 전체 교체

PATCH
→ 일부 수정
```

으로 구분하면 이해하기 쉽다.

---

# 27. DELETE

DELETE는 Resource 삭제를 요청할 때 사용한다.

예:

```text
DELETE /products/10
```

---

# 28. 의미

```text
10번 상품을 삭제해주세요.
```

이다.

---

# 29. DELETE 핵심

```text
DELETE
→ 삭제
```

라고 기억한다.

---

# 30. CRUD와 연결

Database에서 배운 CRUD와 HTTP Method를 연결해볼 수 있다.

```text
CRUD        HTTP Method

Create  →   POST
Read    →   GET
Update  →   PUT / PATCH
Delete  →   DELETE
```

---

# 31. 하지만 완전히 같은 개념은 아니다

HTTP Method는:

```text
Network / Web Protocol의 의미
```

이고 CRUD는:

```text
Data 처리 관점의 개념
```

이다.

따라서:

```text
GET = SQL SELECT
```

처럼 완전히 동일한 개념으로 생각하면 안 된다.

---

# 32. 일반적인 연결

Web API에서는 다음 흐름이 흔하다.

```text
GET /products
↓
Backend
↓
SELECT
↓
Database
```

---

# 33. 생성

```text
POST /products
↓
Backend
↓
INSERT
↓
Database
```

---

# 34. 수정

```text
PATCH /products/10
↓
Backend
↓
UPDATE
↓
Database
```

---

# 35. 삭제

```text
DELETE /products/10
↓
Backend
↓
DELETE
↓
Database
```

---

# 36. Method 표

| Method | 일반적인 의미 |
|---|---|
| GET | 조회 |
| POST | 생성 / 처리 요청 |
| PUT | 전체 교체 |
| PATCH | 일부 수정 |
| DELETE | 삭제 |

---

# 37. Method만 보면 안 된다

다음은:

```text
GET
```

만으로는 어떤 Data를 조회하는지 알 수 없다.

Path를 같이 봐야 한다.

---

# 38. 예

```text
GET /products
```

```text
GET
→ 조회

/products
→ 상품 Resource
```

---

# 39. 따라서

API의 의도는 흔히:

```text
HTTP Method
+
Path
```

를 함께 보고 이해한다.

---

# 40. Method + Path 예

```text
GET /products
→ 상품 목록 조회

POST /products
→ 상품 생성

GET /products/10
→ 10번 상품 조회

PATCH /products/10
→ 10번 상품 일부 수정

DELETE /products/10
→ 10번 상품 삭제
```

---

# 41. Idempotent란?

HTTP Method를 공부하다 보면:

```text
Idempotent
```

라는 말을 볼 수 있다.

---

# 42. Idempotent 의미

같은 Request를 한 번 실행하든 여러 번 실행하든 의도된 Server Resource 상태가 같아지는 성질을 말한다.

---

# 43. GET

예:

```text
GET /products/10
```

을 여러 번 조회한다고 상품 자체의 의도된 상태가 달라지지 않는다.

따라서 GET은 Idempotent Method이다.

---

# 44. PUT

예:

```text
PUT /products/10
```

으로 같은 Resource 표현을 반복해서 적용한다.

한 번 실행하든 여러 번 실행하든 최종적으로 동일한 Resource 상태가 되도록 사용할 수 있다.

PUT도 Idempotent이다.

---

# 45. DELETE

```text
DELETE /products/10
```

을 한 번 실행하면 Resource가 삭제된다.

다시 실행했을 때 Response Status는 달라질 수 있지만:

```text
Resource가 존재하지 않는 상태
```

라는 의도된 최종 상태는 같다.

DELETE도 Idempotent이다.

---

# 46. POST

POST는 일반적으로 Idempotent로 정의되지 않는다.

예:

```text
POST /orders
```

를 두 번 보내면:

```text
주문 2개
```

가 생성될 수도 있다.

---

# 47. 중요한 이유

결제, 주문 같은 API에서는 같은 Request가 중복 전송되는 문제를 고려해야 한다.

실제 Service에서는:

```text
Idempotency Key
```

등의 추가 설계를 사용하기도 한다.

---

# 48. 지금은

처음에는 다음 정도만 기억한다.

```text
GET
→ Safe + Idempotent

PUT
→ Idempotent

DELETE
→ Idempotent

POST
→ 일반적으로 Idempotent 아님
```

PATCH의 Idempotency는 PATCH 문서와 실제 처리 방식에 따라 달라질 수 있다.

---

# 49. HTTP Status Code

이제 Response의:

```text
Status Code
```

를 보자.

Status Code는 Server가 Request를 어떻게 처리했는지를 Client에게 알려준다.

---

# 50. 예

```http
HTTP/1.1 200 OK
```

여기서:

```text
200
```

이 Status Code이다.

---

# 51. Status Code 범위

크게:

```text
1xx
2xx
3xx
4xx
5xx
```

로 나눈다.

---

# 52. 1xx

```text
Informational
```

Request 처리 과정의 정보를 나타낸다.

초기 학습에서는 자주 직접 다룰 일은 많지 않다.

---

# 53. 2xx

```text
Success
```

Request가 성공적으로 처리되었음을 나타낸다.

---

# 54. 3xx

```text
Redirection
```

다른 위치나 Cache된 Resource 사용 등 추가 동작이 필요한 상황과 관련된다.

---

# 55. 4xx

```text
Client Error
```

Client Request 쪽의 문제와 관련된 상태이다.

---

# 56. 5xx

```text
Server Error
```

Server가 Request를 처리하는 과정에서 문제가 발생한 상황과 관련된다.

---

# 57. 가장 먼저 기억

```text
2xx
→ 성공

3xx
→ Redirect 등

4xx
→ Client Request 쪽 문제

5xx
→ Server 처리 쪽 문제
```

---

# 58. 200 OK

가장 많이 보는 성공 Status Code이다.

```text
200 OK
```

---

# 59. GET 성공

```text
GET /products
↓
200 OK
```

---

# 60. Response

```json
[
    {
        "id": 1,
        "name": "Notebook"
    }
]
```

---

# 61. AI Prediction 성공

```text
POST /predict
↓
200 OK
```

Response:

```json
{
    "label": "Pneumonia",
    "score": 0.93
}
```

---

# 62. 201 Created

새 Resource가 성공적으로 생성되었을 때 사용할 수 있다.

```text
POST /products
↓
201 Created
```

---

# 63. 예

Response:

```json
{
    "id": 10,
    "name": "Notebook"
}
```

---

# 64. 200 vs 201

```text
200
→ 일반적인 성공

201
→ Resource 생성 성공
```

이라고 먼저 이해한다.

---

# 65. 204 No Content

Request가 성공했지만 Response Body를 보낼 필요가 없을 때 사용할 수 있다.

예:

```text
DELETE /products/10
↓
204 No Content
```

---

# 66. 204 특징

```text
성공
+
Response Body 없음
```

이다.

---

# 67. 3xx

Redirect와 관련된 Code를 살펴보자.

---

# 68. 301 Moved Permanently

Resource가 영구적으로 다른 위치로 이동했다는 의미로 사용할 수 있다.

---

# 69. 302 Found

임시적인 Redirect 등에 사용할 수 있다.

---

# 70. Redirect

예:

```text
Client
↓
GET /old-page
↓
Server
↓
301
Location: /new-page
↓
Client
↓
GET /new-page
```

---

# 71. Browser에서는

Redirect를 자동으로 따라가서 사용자가 중간 과정을 눈치채지 못하는 경우도 많다.

---

# 72. 400 Bad Request

Server가 Request를 잘못된 Request로 판단한 경우 사용할 수 있다.

```text
400 Bad Request
```

---

# 73. 예

잘못된 Request 형식이나 Server가 처리할 수 없는 Request 등의 상황에서 볼 수 있다.

---

# 74. 401 Unauthorized

이름 때문에 헷갈리기 쉽다.

일반적으로:

```text
인증이 필요하거나

유효한 인증 자격 증명이 없음
```

과 관련된다.

---

# 75. 예

```text
GET /mypage
```

인증 정보가 없다.

```text
↓
401 Unauthorized
```

---

# 76. 401을 쉽게 기억

```text
너 누구야?
```

에 가깝게 생각하면 이해하기 쉽다.

즉:

```text
Authentication
```

문제와 연결한다.

---

# 77. Authentication

```text
사용자가 누구인지 확인
```

하는 것이다.

예:

```text
Login

Token 확인
```

---

# 78. 403 Forbidden

Server가 Request를 이해했지만 접근을 허용하지 않는 경우 등에 사용된다.

---

# 79. 예

일반 사용자가 관리자 Page에 접근:

```text
GET /admin
↓
403 Forbidden
```

---

# 80. 403을 쉽게 기억

```text
누군지는 알겠는데
여기는 들어오면 안 돼.
```

와 가깝게 이해할 수 있다.

---

# 81. Authorization

```text
이 사용자가 이 작업을 할 권한이 있는가?
```

를 판단하는 것이다.

---

# 82. 401 vs 403

처음에는:

```text
401
→ Authentication 문제
→ 누구인지 인증이 필요

403
→ Authorization 문제
→ 권한이 없음
```

으로 구분한다.

---

# 83. 404 Not Found

요청한 Resource를 찾지 못한 경우이다.

```text
404 Not Found
```

---

# 84. 예

Backend:

```text
GET /products
```

만 존재한다.

Client:

```text
GET /product
```

요청.

결과:

```text
404
```

가 될 수 있다.

---

# 85. 404가 의미하는 중요한 점

404 Response를 받았다는 것은 일반적으로:

```text
Server와 HTTP 통신 자체는 이루어졌지만

요청한 Resource를 찾지 못했다.
```

는 의미이다.

---

# 86. 따라서

```text
404
```

와:

```text
Server 연결 자체 실패
```

는 다른 문제이다.

---

# 87. 405 Method Not Allowed

Path는 존재하지만 해당 HTTP Method를 지원하지 않는 경우이다.

---

# 88. 예

Backend:

```text
GET /products
```

만 존재한다.

Client:

```text
POST /products
```

요청.

결과:

```text
405 Method Not Allowed
```

가 될 수 있다.

---

# 89. 404 vs 405

```text
404
→ Resource / Path를 찾지 못함

405
→ Path는 있지만 Method가 허용되지 않음
```

으로 구분한다.

---

# 90. 409 Conflict

현재 Resource 상태와 Request가 충돌하는 상황에 사용할 수 있다.

예:

```text
이미 존재하는 값

현재 상태와 충돌하는 변경
```

등에서 API 설계에 따라 사용할 수 있다.

---

# 91. 415 Unsupported Media Type

Server가 Request Body의 Media Type을 지원하지 않을 때 사용할 수 있다.

예:

Server가 JSON을 기대하는데 적절하지 않은 Content-Type으로 보내는 경우 등을 생각할 수 있다.

---

# 92. 422

FastAPI에서 특히 자주 보게 될 수 있다.

Request Data가 예상한 Schema 또는 Validation 조건을 만족하지 못할 때:

```text
422
```

Response를 볼 수 있다.

---

# 93. FastAPI 예

Backend:

```python
@app.get("/products/{product_id}")
def get_product(
    product_id: int
):

    return {
        "product_id": product_id
    }
```

---

# 94. 정상 Request

```text
GET /products/10
```

`10`은 Integer로 변환 가능하다.

---

# 95. 잘못된 Request

```text
GET /products/hello
```

Backend는:

```text
product_id: int
```

를 기대한다.

`hello`는 Integer로 처리할 수 없다.

FastAPI의 Validation 과정에서 오류 Response가 발생한다.

---

# 96. FastAPI와 422

FastAPI/Pydantic의 버전과 Validation 처리에 따라 세부 Response 형태는 달라질 수 있지만 입력 Validation 오류를 만났다는 점이 핵심이다.

---

# 97. 429 Too Many Requests

Client가 너무 많은 Request를 보낸 상황에서 Rate Limiting과 관련하여 사용할 수 있다.

```text
429 Too Many Requests
```

---

# 98. AI API와 429

외부 AI API 등을 사용할 때 사용량 제한을 넘으면:

```text
429
```

관련 Error를 볼 수도 있다.

---

# 99. 500 Internal Server Error

Server 내부에서 예상하지 못한 Error가 발생한 경우이다.

---

# 100. 예

```text
POST /predict
↓
FastAPI
↓
Model 실행
↓
RuntimeError
↓
500
```

---

# 101. 500이면

Frontend HTML을 먼저 고치는 것이 아니라 Backend Terminal의 Error Log를 확인해야 할 가능성이 높다.

---

# 102. 예

```text
500 Internal Server Error
```

그리고 Terminal:

```text
FileNotFoundError
```

가 있다면 Backend에서 필요한 File을 찾지 못한 것이다.

---

# 103. 502 Bad Gateway

Gateway 또는 Proxy 역할을 하는 Server가 Upstream Server에서 정상적인 Response를 받지 못한 경우 등에 볼 수 있다.

---

# 104. 503 Service Unavailable

Server가 현재 Request를 처리할 수 없는 상황에서 사용할 수 있다.

예:

```text
일시적인 과부하

Maintenance

Backend Service unavailable
```

등이다.

---

# 105. 504 Gateway Timeout

Gateway/Proxy가 Upstream Server의 Response를 제시간에 받지 못한 경우 등에 볼 수 있다.

---

# 106. Status Code 표

| Code | 의미 |
|---|---|
| 200 | OK |
| 201 | Created |
| 204 | No Content |
| 301 | Moved Permanently |
| 302 | Found |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 405 | Method Not Allowed |
| 409 | Conflict |
| 415 | Unsupported Media Type |
| 422 | Validation 관련 오류에서 자주 접함 |
| 429 | Too Many Requests |
| 500 | Internal Server Error |
| 502 | Bad Gateway |
| 503 | Service Unavailable |
| 504 | Gateway Timeout |

---

# 107. 모든 Code를 외울 필요는 없다

처음에는:

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
429

500
```

정도를 익숙하게 만든다.

---

# 108. Status Code를 보면 범위를 먼저 본다

예:

```text
422
```

라면 첫 숫자가:

```text
4
```

이다.

따라서:

```text
4xx
→ Client Request와 관련된 문제
```

범위부터 생각한다.

---

# 109. 500

첫 숫자:

```text
5
```

이므로:

```text
Server 처리 문제
```

부터 의심한다.

---

# 110. Status Code와 Error Message

Status Code만 보지 않는다.

Response Body에 추가 Error 정보가 있을 수 있다.

예:

```json
{
    "detail": "Not Found"
}
```

---

# 111. FastAPI Error

예:

```json
{
    "detail": [
        {
            "loc": [
                "path",
                "product_id"
            ],
            "msg": "...",
            "type": "..."
        }
    ]
}
```

처럼 어떤 입력에서 Validation이 실패했는지 정보가 포함될 수 있다.

세부 형식은 버전에 따라 달라질 수 있다.

---

# 112. 그래서 Error를 볼 때

```text
Status Code
+
Response Body
+
Backend Log
```

를 함께 본다.

---

# 113. Browser Network Tab

```text
F12
↓
Network
```

에서 Request를 선택한다.

확인:

```text
Request URL

Request Method

Status Code

Request Headers

Request Payload

Response
```

---

# 114. 예

Network에서:

```text
Request URL
http://localhost:8000/predict

Request Method
POST

Status Code
422
```

를 봤다고 하자.

---

# 115. 생각 순서

```text
Server 연결은 됐나?
→ Response를 받았으므로 통신은 이루어짐

Path는?
→ /predict

Method는?
→ POST

422?
→ 입력 Validation 확인

Payload는?
→ Backend가 기대하는 형태인가?
```

---

# 116. 404 예

```text
POST http://localhost:8000/predcit
```

오타:

```text
/predcit
```

Backend:

```text
/predict
```

이면:

```text
404
```

를 볼 수 있다.

---

# 117. 405 예

Backend:

```python
@app.post("/predict")
```

Frontend:

```text
GET /predict
```

이면:

```text
405
```

를 볼 수 있다.

---

# 118. 422 예

Backend가:

```text
file
```

이라는 Image File을 기대하는데 Client가 필요한 File을 보내지 않았다.

FastAPI Validation에서:

```text
422
```

를 볼 수 있다.

---

# 119. 500 예

Request는 정상적으로 들어왔다.

하지만:

```text
Model File 없음

Tensor Shape Error

Python Exception
```

등이 Backend 내부에서 발생한다.

그러면:

```text
500
```

이 발생할 수 있다.

---

# 120. AI Service Debugging

```text
POST /predict
```

가 실패했다고 하자.

---

# 121. Step 1

Browser Network:

```text
Status Code 확인
```

---

# 122. Step 2

404:

```text
URL / Path 확인
```

---

# 123. Step 3

405:

```text
HTTP Method 확인
```

---

# 124. Step 4

422:

```text
Request Body

File Name

Field Name

Data Type
```

확인.

---

# 125. Step 5

500:

```text
FastAPI Terminal

Python Traceback

Model

Preprocessing
```

확인.

---

# 126. Status Code는 문제 위치를 알려주는 힌트

```text
404
→ Route?

405
→ Method?

422
→ Input?

500
→ Backend?
```

처럼 활용한다.

---

# 127. Frontend fetch

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

# 128. Status 확인

```javascript
console.log(
    response.status
);
```

---

# 129. 성공 확인

```javascript
if (!response.ok) {

    console.error(
        "Request failed:",
        response.status
    );

}
```

---

# 130. response.ok

일반적으로:

```text
200 ~ 299
```

Status Code에 대해 `true`가 된다.

---

# 131. Error 처리 예

```javascript
const response =
    await fetch(
        "http://localhost:8000/predict",
        {
            method: "POST",
            body: formData
        }
    );


if (!response.ok) {

    const errorText =
        await response.text();

    throw new Error(
        `${response.status}: ${errorText}`
    );

}


const data =
    await response.json();
```

---

# 132. Method 선택 연습

상품 전체 목록 조회:

```text
GET /products
```

---

# 133. 상품 한 개 조회

```text
GET /products/10
```

---

# 134. 상품 생성

```text
POST /products
```

---

# 135. 상품 전체 교체

```text
PUT /products/10
```

---

# 136. 상품 가격만 수정

```text
PATCH /products/10
```

---

# 137. 상품 삭제

```text
DELETE /products/10
```

---

# 138. AI Image 분석

```text
POST /predict
```

---

# 139. 회원 목록 조회

```text
GET /users
```

---

# 140. 특정 회원 조회

```text
GET /users/10
```

---

# 141. API 의도를 읽는 공식

```text
Method
+
Path
```

를 함께 읽는다.

예:

```text
DELETE /users/10
```

---

# 142. 해석

```text
DELETE
→ 삭제

/users/10
→ 10번 User

결과
→ 10번 User 삭제 요청
```

---

# 143. Response까지 연결

```text
DELETE /users/10
↓
204 No Content
```

이면:

```text
삭제 요청을 성공적으로 처리했고
Response Body는 없다.
```

라고 해석할 수 있다.

---

# 144. 또 다른 경우

```text
DELETE /users/999
↓
404 Not Found
```

이면:

```text
삭제하려는 Resource를 찾지 못했다.
```

라고 이해할 수 있다.

---

# 145. Request와 Response를 한 문장으로 읽기

```text
POST /products
↓
201 Created
```

```text
상품 생성을 요청했고
Server가 성공적으로 생성했다.
```

---

# 146. 또 다른 예

```text
GET /products/999
↓
404 Not Found
```

```text
999번 상품을 요청했지만
Server가 해당 Resource를 찾지 못했다.
```

---

# 147. AI 예

```text
POST /predict
↓
422
```

```text
Prediction Request는 Server에 도착했지만
입력 Data가 Backend의 Validation 조건에 맞지 않았다.
```

---

# 148. AI Backend Error

```text
POST /predict
↓
500
```

```text
Request는 Server에 도착했지만
Backend가 처리하는 과정에서 내부 Error가 발생했다.
```

---

# 149. 핵심 정리

HTTP Method:

```text
GET
→ 조회

POST
→ 생성 / 처리 요청

PUT
→ 전체 교체

PATCH
→ 일부 수정

DELETE
→ 삭제
```

---

# 150. Status Code

```text
2xx
→ 성공

3xx
→ Redirect 등

4xx
→ Client Request 쪽 문제

5xx
→ Server 처리 쪽 문제
```

---

# 151. 자주 보는 Code

```text
200
→ OK

201
→ Created

204
→ No Content

400
→ Bad Request

401
→ Authentication 필요/실패

403
→ 접근 권한 없음

404
→ Resource 없음

405
→ Method가 허용되지 않음

422
→ 입력 Validation 문제에서 자주 접함

429
→ Request가 너무 많음

500
→ Server 내부 Error
```

---

# 152. 가장 중요한 Debugging 공식

```text
404
→ URL / Path

405
→ Method

422
→ Input Data

500
→ Backend
```

먼저 이 네 가지를 구분할 수 있도록 한다.

---

# 153. 전체 흐름

```text
Client

POST /predict
Image

        ↓

FastAPI

        ↓

PyTorch Model

        ↓

Response

200
Prediction JSON
```

문제가 생기면:

```text
404
405
422
500
```

등의 Status Code가 문제 위치를 파악하는 중요한 단서가 된다.

---

# 154. 한 문장으로 정리

HTTP Method와 Status Code는:

```text
Method가 Client가 Resource에 대해
어떤 작업을 원하는지를 표현하고,

Status Code가 Server가 그 Request를
어떻게 처리했는지를 알려주는

HTTP Request / Response의 핵심 정보
```

라고 이해할 수 있다.

---

# 155. 다음 단계

이제:

```text
URL
HTTP
Request
Response
Method
Status Code
```

를 이해했다.

다음에는 Web API에서 매우 자주 사용하는:

```text
JSON
```

을 공부한다.

특히:

```text
Python dict
↕
JSON
↕
JavaScript Object
```

가 API에서 어떻게 연결되는지 살펴본다.