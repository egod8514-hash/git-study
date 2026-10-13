# URL

## 1. URL이란?

URL은:

```text
Uniform Resource Locator
```

의 약자이다.

Web에서 Resource의 위치와 접근 방법을 표현하는 주소이다.

예:

```text
https://example.com
```

```text
https://example.com/products
```

```text
https://api.example.com/users/10
```

---

# 2. Resource란?

Web에서 Resource는 Server가 제공하는 대상을 의미할 수 있다.

예:

```text
HTML 문서

Image

CSS File

JavaScript File

상품 Data

회원 Data

API 결과
```

등이다.

---

# 3. URL의 기본 구조

예:

```text
https://example.com/products
```

나누면:

```text
https
→ Scheme

example.com
→ Host

/products
→ Path
```

이다.

---

# 4. 조금 더 복잡한 URL

```text
https://api.example.com:443/products/10?category=computer#review
```

나누면:

```text
https
→ Scheme

api.example.com
→ Host

443
→ Port

/products/10
→ Path

category=computer
→ Query

review
→ Fragment
```

---

# 5. 전체 그림

```text
https://api.example.com:443/products/10?category=computer&page=2#review
└─┬─┘   └───────┬───────┘ └┬┘ └─────┬─────┘ └──────────┬─────────┘ └─┬──┘
Scheme          Host        Port      Path              Query          Fragment
```

---

# 6. Scheme

URL의 앞부분:

```text
https://
```

에서:

```text
https
```

가 Scheme이다.

어떤 방식으로 Resource에 접근할지를 나타낸다.

---

# 7. 자주 보는 Scheme

```text
http

https
```

가 대표적이다.

그 외에도:

```text
ftp

mailto

file
```

등 여러 Scheme이 존재한다.

---

# 8. HTTP URL

```text
http://example.com
```

은 HTTP를 이용하여 접근한다.

---

# 9. HTTPS URL

```text
https://example.com
```

은 TLS로 보호되는 HTTP 통신을 사용한다.

---

# 10. `://`

다음과 같이 자주 보인다.

```text
https://
```

여기서:

```text
https
```

가 Scheme이다.

`://`까지 전부 Scheme 이름인 것은 아니다.

---

# 11. Host

```text
https://api.example.com/products
        └───────┬───────┘
                Host
```

여기서:

```text
api.example.com
```

이 Host이다.

---

# 12. Host에는 무엇이 올 수 있는가?

Domain Name:

```text
example.com
```

IP Address:

```text
127.0.0.1
```

등이 올 수 있다.

---

# 13. Domain 사용

```text
https://example.com
```

Browser는 DNS 등을 통해 통신에 필요한 Address 정보를 찾는다.

단순화하면:

```text
example.com
↓
DNS
↓
Server IP
```

이다.

---

# 14. IP 직접 사용

Local 개발에서는:

```text
http://127.0.0.1:8000
```

처럼 IP Address를 직접 사용할 수도 있다.

---

# 15. localhost

```text
http://localhost:8000
```

에서:

```text
localhost
```

도 Host이다.

---

# 16. Subdomain

```text
api.example.com
```

에서:

```text
api
```

는 Subdomain으로 사용할 수 있다.

예:

```text
www.example.com

api.example.com

admin.example.com
```

---

# 17. Service 분리

예를 들어:

```text
www.example.com
→ Website

api.example.com
→ API

admin.example.com
→ 관리자 Service
```

처럼 구성할 수 있다.

---

# 18. Port

```text
http://localhost:8000
                 └─┬─┘
                  Port
```

여기서:

```text
8000
```

이 Port이다.

---

# 19. Port의 역할

```text
Host
→ 어느 Host인가?

Port
→ 그 Host의 어느 Network Service인가?
```

이다.

---

# 20. 기본 Port

HTTP:

```text
80
```

HTTPS:

```text
443
```

이 기본 Port로 사용된다.

---

# 21. Port 생략

그래서:

```text
https://example.com
```

에서는 일반적으로 `:443`을 생략할 수 있다.

---

# 22. 개발 환경

개발에서는:

```text
http://localhost:5500

http://localhost:8000
```

처럼 Port를 직접 지정하는 경우가 많다.

---

# 23. Path

```text
https://example.com/products/10
                    └────┬────┘
                         Path
```

여기서:

```text
/products/10
```

이 Path이다.

---

# 24. Path의 역할

Server 안에서 어떤 Resource 또는 Route를 요청하는지를 나타낸다.

예:

```text
/

/products

/products/10

/users

/login

/predict
```

---

# 25. Root Path

```text
/
```

는 Root Path이다.

예:

```text
https://example.com/
```

---

# 26. API와 Path

FastAPI에서:

```python
@app.get("/products")
def get_products():

    return []
```

이라고 만들었다면:

```text
/products
```

가 API Path이다.

---

# 27. Frontend에서 요청

```javascript
fetch(
    "http://localhost:8000/products"
);
```

를 실행하면:

```text
Host
localhost

Port
8000

Path
/products
```

로 Request를 보내게 된다.

---

# 28. Path Parameter

다음 URL을 보자.

```text
/products/10
```

여기서:

```text
10
```

을 특정 상품 ID로 사용할 수 있다.

---

# 29. FastAPI 예

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

# 30. Request

```text
GET /products/10
```

이면:

```text
product_id = 10
```

으로 처리할 수 있다.

---

# 31. Path Parameter의 특징

Resource 자체를 식별하는 값에 자주 사용한다.

예:

```text
/users/10

/products/25

/posts/100
```

---

# 32. Query String

URL 뒤에:

```text
?
```

를 이용하여 추가 Parameter를 전달할 수 있다.

예:

```text
/products?category=computer
```

---

# 33. Query Parameter

위 URL에서:

```text
category=computer
```

가 Query Parameter이다.

---

# 34. 구조

```text
?key=value
```

형태를 자주 사용한다.

예:

```text
?category=computer
```

---

# 35. 여러 Query Parameter

여러 개라면:

```text
&
```

로 구분한다.

```text
/products?category=computer&page=2
```

---

# 36. 분해

```text
category = computer

page = 2
```

이다.

---

# 37. Query Parameter의 용도

대표적으로:

```text
검색

필터

정렬

페이지

옵션
```

등에 사용한다.

---

# 38. 검색 예

```text
/search?keyword=python
```

---

# 39. 필터 예

```text
/products?category=computer
```

---

# 40. Pagination

```text
/products?page=2&size=20
```

처럼 사용할 수 있다.

---

# 41. 정렬

```text
/products?sort=price
```

같은 형태도 사용할 수 있다.

---

# 42. Path Parameter와 Query Parameter

둘의 차이를 구분하는 것이 중요하다.

Path Parameter:

```text
/products/10
```

Query Parameter:

```text
/products?category=computer
```

---

# 43. 처음에는 이렇게 구분

```text
Path Parameter
→ 어떤 Resource인가?

Query Parameter
→ 어떤 조건으로 볼 것인가?
```

라고 생각하면 쉽다.

---

# 44. 예

```text
/products/10
```

은:

```text
10번 상품
```

을 의미할 수 있다.

반면:

```text
/products?category=computer
```

은:

```text
상품 목록 중
computer Category만
```

을 의미할 수 있다.

---

# 45. 절대적인 규칙인가?

API 설계에 따라 달라질 수 있다.

하지만 REST 스타일 API에서 흔히 볼 수 있는 패턴이다.

---

# 46. FastAPI Query Parameter

```python
@app.get("/products")
def get_products(
    category: str | None = None
):

    return {
        "category": category
    }
```

---

# 47. Request

```text
GET /products?category=computer
```

이면:

```text
category
=
computer
```

값을 받을 수 있다.

---

# 48. Path + Query

같이 사용할 수도 있다.

```text
/users/10/posts?page=2
```

나누면:

```text
/users/10/posts
→ Path

page=2
→ Query
```

---

# 49. Query String

`?` 뒤 전체 부분을:

```text
Query String
```

이라고 부를 수 있다.

예:

```text
category=computer&page=2
```

---

# 50. Query Parameter 값에 공백이 있다면?

URL에서는 특정 문자를 그대로 사용할 수 없는 경우가 있다.

그래서:

```text
URL Encoding
```

이 사용된다.

---

# 51. Percent Encoding

예를 들어 공백이:

```text
%20
```

으로 표현되는 경우를 볼 수 있다.

예:

```text
/search?q=hello%20world
```

---

# 52. 한글도 Encoding될 수 있다

예:

```text
검색어=파이썬
```

같은 값을 URL에 넣으면 Browser나 Library가 Percent Encoding된 형태로 처리할 수 있다.

---

# 53. JavaScript에서 Encoding

필요한 경우:

```javascript
const keyword =
    encodeURIComponent(
        "hello world"
    );
```

를 사용할 수 있다.

---

# 54. 결과

```text
hello%20world
```

같은 형태가 될 수 있다.

---

# 55. URLSearchParams

JavaScript에서는 Query String을 만들 때:

```javascript
const params =
    new URLSearchParams({
        keyword: "python",
        page: "2"
    });


console.log(
    params.toString()
);
```

같은 방법을 사용할 수 있다.

---

# 56. 결과

```text
keyword=python&page=2
```

형태가 된다.

---

# 57. 직접 문자열을 붙이는 것보다

Query Parameter가 많아지면:

```javascript
URLSearchParams
```

같은 API를 사용하면 Encoding과 관리가 편리해질 수 있다.

---

# 58. Fragment

URL 마지막에:

```text
#
```

뒤에 오는 부분이다.

예:

```text
https://example.com/page#contact
```

여기서:

```text
contact
```

가 Fragment이다.

---

# 59. HTML Anchor와 연결

HTML:

```html
<section id="contact">

    <h2>
        Contact
    </h2>

</section>
```

URL:

```text
/page#contact
```

을 사용하면 Browser가 해당 Element 위치로 이동하는 데 사용할 수 있다.

---

# 60. Fragment의 중요한 특징

일반적인 HTTP Request에서는 Fragment가 Server에 전송되지 않는다.

Browser Client 측에서 처리된다.

---

# 61. 예

Browser 주소:

```text
https://example.com/page#contact
```

Server Request 대상은 일반적으로:

```text
/page
```

이며:

```text
#contact
```

부분은 Server로 보내지지 않는다.

---

# 62. Query와 Fragment 차이

Query:

```text
?keyword=python
```

은 Server Request에 포함될 수 있다.

Fragment:

```text
#section1
```

은 일반적인 HTTP Request에서 Server로 전달되지 않는다.

---

# 63. URL 전체 분석

```text
https://api.example.com:443/products/10?category=computer&page=2#review
```

---

# 64. Scheme

```text
https
```

---

# 65. Host

```text
api.example.com
```

---

# 66. Port

```text
443
```

---

# 67. Path

```text
/products/10
```

---

# 68. Query

```text
category=computer&page=2
```

---

# 69. Fragment

```text
review
```

---

# 70. 한 번에 보기

```text
https://api.example.com:443/products/10?category=computer&page=2#review
│       │               │   │            │                         │
│       │               │   │            │                         └─ Fragment
│       │               │   │            └─ Query
│       │               │   └─ Path
│       │               └─ Port
│       └─ Host
└─ Scheme
```

---

# 71. URL과 HTTP Method

매우 중요한 점은 URL만으로 Request의 전체 의미가 결정되는 것은 아니라는 것이다.

예:

```text
/products
```

라는 같은 URL이 있어도 Method가 다를 수 있다.

---

# 72. GET

```text
GET /products
```

상품 목록 조회로 설계할 수 있다.

---

# 73. POST

```text
POST /products
```

새 상품 생성으로 설계할 수 있다.

---

# 74. 같은 Path

```text
/products
```

는 같지만:

```text
GET

POST
```

Method가 다르다.

---

# 75. 그래서 API Request 의도

API에서는 흔히:

```text
HTTP Method
+
URL / Path
```

를 함께 보고 Request의 의도를 이해한다.

---

# 76. 예

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

# 77. URL은 동작 이름만 쓰는 것이 아니다

예를 들어 REST 스타일에서는:

```text
GET /getProducts

POST /createProduct

POST /deleteProduct
```

처럼 모든 동작을 Path 이름으로 표현하기보다 Resource 중심으로 설계하는 패턴을 자주 사용한다.

---

# 78. Resource 중심

```text
/products
```

라는 Resource에:

```text
GET
POST
PATCH
DELETE
```

등 Method를 조합한다.

---

# 79. 하지만

모든 API가 반드시 REST 방식으로 만들어지는 것은 아니다.

Service 설계에 따라 다른 API 스타일도 존재한다.

---

# 80. URL과 Request Body

다음 Request를 생각해보자.

```text
POST /products
```

새 상품 Data는 URL에 전부 넣기보다 Request Body에 JSON으로 전달할 수 있다.

예:

```json
{
    "name": "Notebook",
    "price": 1500000
}
```

---

# 81. URL

```text
POST /products
```

---

# 82. Body

```json
{
    "name": "Notebook",
    "price": 1500000
}
```

---

# 83. 의미

```text
URL / Path
→ 어느 Resource?

Method
→ 무엇을 할 것인가?

Body
→ 필요한 실제 Data
```

라고 이해할 수 있다.

---

# 84. Query와 Body는 다르다

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

은 서로 다른 Request 구성 요소이다.

---

# 85. 민감한 Data와 URL

Password, Token 등의 민감한 값을 URL Query에 넣는 것은 피해야 한다.

URL은:

```text
Browser History

Server Log

Proxy Log

Analytics
```

등 여러 위치에 남을 가능성이 있기 때문이다.

---

# 86. Password 예

다음과 같이 만들지 않는 것이 좋다.

```text
/login?password=1234
```

---

# 87. Login Data

Login 같은 Data는 일반적으로 HTTPS를 사용하고 Request Body 등을 통해 전달하는 구조를 사용한다.

단, Body에 넣는 것만으로 암호화되는 것은 아니다.

통신 보호에는 HTTPS가 중요하다.

---

# 88. URL과 Browser 주소창

Browser 주소창에 URL을 입력하면 기본적으로 Page Navigation을 위한 Request가 발생한다.

---

# 89. fetch의 URL

JavaScript에서도 URL을 사용한다.

```javascript
fetch(
    "https://api.example.com/products"
);
```

---

# 90. Image URL

HTML에서도 사용한다.

```html
<img
    src="https://example.com/images/main.jpg"
    alt="Main"
>
```

---

# 91. CSS URL

CSS에서도 Resource URL을 사용할 수 있다.

```css
.hero {
    background-image:
        url("./images/hero.jpg");
}
```

---

# 92. Link

```html
<a href="/products">
    상품 보기
</a>
```

에도 URL 또는 상대 경로를 사용한다.

---

# 93. Absolute URL

전체 주소를 포함하는 형태:

```text
https://example.com/products
```

---

# 94. Relative URL

현재 문서의 위치를 기준으로 해석되는 형태:

```text
/products
```

또는:

```text
images/photo.jpg
```

같은 표현을 사용할 수 있다.

---

# 95. Root-relative

```text
/products
```

처럼 `/`로 시작하면 일반적으로 현재 Origin의 Root를 기준으로 한다.

---

# 96. 상대 경로

```text
images/photo.jpg
```

는 현재 문서 위치를 기준으로 해석될 수 있다.

---

# 97. 상위 폴더

```text
../images/photo.jpg
```

에서:

```text
..
```

는 상위 경로를 의미한다.

HTML에서 Image 경로를 다룰 때도 이미 사용한 개념이다.

---

# 98. Frontend와 Backend URL

개발 중:

```text
Frontend
http://localhost:5500

Backend
http://localhost:8000
```

일 수 있다.

---

# 99. Frontend fetch

```javascript
fetch(
    "http://localhost:8000/products"
);
```

---

# 100. URL을 분해

```text
http
→ Scheme

localhost
→ Host

8000
→ Port

/products
→ Path
```

---

# 101. 다른 Origin

다음 두 주소는:

```text
http://localhost:5500

http://localhost:8000
```

Port가 다르다.

따라서 서로 다른 Origin이다.

---

# 102. Origin

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

# 103. 예

```text
https://example.com:443
```

Origin의 구성:

```text
https

example.com

443
```

이다.

---

# 104. Path는 Origin에 포함되지 않는다

```text
https://example.com/products

https://example.com/users
```

는 Path가 다르지만 같은 Origin일 수 있다.

---

# 105. Query도 Origin에 포함되지 않는다

```text
https://example.com/products?page=1

https://example.com/products?page=2
```

도 같은 Origin이다.

---

# 106. CORS와 URL

Frontend:

```text
http://localhost:5500
```

Backend:

```text
http://localhost:8000
```

처럼 Origin이 다르면 Browser의 Cross-Origin 정책과 CORS 설정을 만나게 될 수 있다.

---

# 107. API Endpoint

API에서:

```text
GET /products
```

같은 특정 접근 지점을 API Endpoint라고 부르는 경우가 많다.

---

# 108. 예

```text
GET /users

POST /users

POST /login

POST /predict
```

등이다.

---

# 109. AI API URL

Local:

```text
http://localhost:8000/predict
```

---

# 110. 배포

```text
https://api.example.com/predict
```

---

# 111. Frontend 코드

```javascript
const response =
    await fetch(
        "https://api.example.com/predict",
        {
            method: "POST",
            body: formData
        }
    );
```

---

# 112. 여기서 URL의 역할

```text
어느 Server의

어느 API Path로

Request를 보낼 것인가?
```

를 결정한다.

---

# 113. Method의 역할

```text
그 Resource에 대해

무엇을 하려는가?
```

를 표현한다.

---

# 114. AI Prediction

```text
POST /predict
```

처럼 Prediction Request를 설계할 수 있다.

---

# 115. Image Data

Image 자체를:

```text
/predict?image=...
```

처럼 URL에 억지로 넣기보다 일반적으로 File Upload Request Body를 사용한다.

---

# 116. FormData

Frontend:

```javascript
const formData =
    new FormData();


formData.append(
    "file",
    file
);
```

---

# 117. Request

```javascript
fetch(
    "http://localhost:8000/predict",
    {
        method: "POST",
        body: formData
    }
);
```

---

# 118. 전체 흐름

```text
URL
http://localhost:8000/predict
            ↓
Host + Port
            ↓
FastAPI Server
            ↓
Path
/predict
            ↓
Prediction Route
            ↓
AI Model
```

---

# 119. Browser Network Tab

F12:

```text
Network
```

에서 Request의 URL을 확인할 수 있다.

---

# 120. 확인할 항목

```text
Request URL

Request Method

Status Code

Query String Parameters

Request Headers

Request Payload

Response
```

등을 볼 수 있다.

---

# 121. URL Error

Frontend에서 API가 안 된다면 먼저 URL을 확인한다.

예:

```javascript
fetch(
    "http://localhost:8000/product"
);
```

그런데 Backend가:

```text
/products
```

만 제공한다면 다른 Path이다.

---

# 122. 흔한 실수

```text
/product
/products
```

처럼 한 글자 차이도 다른 URL Path이다.

---

# 123. Port 실수

Backend:

```text
localhost:8000
```

인데:

```text
localhost:8080
```

으로 요청하면 다른 Port이다.

---

# 124. Protocol 실수

Server가 HTTPS로 제공되는데 잘못된 Protocol로 접근하면 문제가 발생할 수 있다.

---

# 125. Query Parameter 이름 실수

Backend:

```text
?category=
```

를 기대하는데 Frontend에서:

```text
?categories=
```

를 보내면 서로 다른 Parameter이다.

---

# 126. Debugging

API 연결 문제가 생기면 URL을 다음처럼 분해한다.

```text
Scheme 맞는가?

Host 맞는가?

Port 맞는가?

Path 맞는가?

Query Parameter 맞는가?

HTTP Method 맞는가?
```

---

# 127. URL만 보고 생각하는 연습

다음 URL:

```text
http://localhost:8000/users/10?detail=true
```

---

# 128. 분석

```text
http
→ Scheme

localhost
→ Host

8000
→ Port

/users/10
→ Path

detail=true
→ Query
```

---

# 129. 다음 URL

```text
https://api.example.com/search?q=python&page=3
```

---

# 130. 분석

```text
https
→ Scheme

api.example.com
→ Host

/search
→ Path

q=python
page=3
→ Query Parameters
```

---

# 131. 다음 URL

```text
https://example.com/about#team
```

---

# 132. 분석

```text
https
→ Scheme

example.com
→ Host

/about
→ Path

team
→ Fragment
```

---

# 133. URL과 DNS 연결

```text
URL
↓
Host 확인
↓
Domain이라면 DNS 조회
↓
IP Address 확인
↓
Server 연결
```

---

# 134. URL과 Port 연결

```text
Host
↓
Server Computer

Port
↓
Server의 Network Service
```

---

# 135. URL과 HTTP 연결

```text
URL
↓
어디로 Request?

HTTP Method
↓
무엇을 요청?

Header
↓
추가 정보

Body
↓
전달 Data
```

---

# 136. URL과 API 연결

```text
GET /products
```

```text
GET
→ Method

/products
→ Path
```

둘을 합쳐 어떤 API 동작인지 이해한다.

---

# 137. 핵심 정리

```text
URL
→ Resource의 위치와 접근 방법을 나타내는 주소

Scheme
→ 접근 방식

Host
→ 접속 대상 Host

Port
→ Host의 Network Service

Path
→ Resource / Route 위치

Query
→ 추가 Parameter

Fragment
→ 문서 내부 위치 등을 나타내는 Client 측 식별자
```

---

# 138. 가장 중요한 구조

```text
https://api.example.com:443/products/10?page=2#review
│       │               │   │            │      │
│       │               │   │            │      └─ Fragment
│       │               │   │            └─ Query
│       │               │   └─ Path
│       │               └─ Port
│       └─ Host
└─ Scheme
```

---

# 139. API에서 기억할 것

```text
URL
→ 어디에 요청할 것인가?

Method
→ 무엇을 할 것인가?

Query
→ 어떤 조건을 추가할 것인가?

Body
→ 어떤 Data를 보낼 것인가?
```

---

# 140. 한 문장으로 정리

URL은:

```text
Scheme, Host, Port, Path, Query, Fragment 등의 정보를 이용하여
Web에서 어떤 Resource에 어떤 위치로 접근할지를 표현하는 주소
```

라고 이해할 수 있다.

---

# 141. 다음 단계

이제 URL을 이해했으므로 다음에는:

```text
GET /products HTTP/1.1
Host: example.com
Accept: application/json
```

같은 실제 HTTP Request가 무엇인지 공부한다.

즉 다음 단계는:

```text
HTTP
```

이다.