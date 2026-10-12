# Frontend & Backend

## 1. Web Service의 전체 구조

Web Service를 크게 나누면:

```text
Frontend
+
Backend
```

로 생각할 수 있다.

하지만 실제로는 다음과 같이 여러 부분이 연결된다.

```text
사용자
↓
Browser
↓
Frontend
↓
HTTP Request
↓
Backend
↓
Database / AI Model
↓
Backend
↓
HTTP Response
↓
Frontend
↓
사용자
```

---

# 2. Frontend란?

Frontend는 사용자가 직접 보고 조작하는 영역이다.

예:

```text
화면

Menu

Button

Form

Image

상품 목록

Dashboard

AI 분석 결과
```

등이다.

---

# 3. Frontend의 기본 기술

```text
HTML
CSS
JavaScript
```

이다.

각 역할은:

```text
HTML
→ 구조

CSS
→ 디자인

JavaScript
→ 동작
```

이다.

---

# 4. 예

HTML:

```html
<button id="predict-button">
    분석하기
</button>
```

CSS:

```css
#predict-button {
    padding: 14px 24px;

    background: black;

    color: white;
}
```

JavaScript:

```javascript
const button =
    document.querySelector(
        "#predict-button"
    );


button.addEventListener(
    "click",
    () => {

        console.log(
            "분석 시작"
        );

    }
);
```

---

# 5. Frontend의 역할

Frontend는 단순히 화면만 보여주는 것이 아니다.

다음과 같은 역할을 수행한다.

```text
사용자 입력 받기

Button Event 처리

화면 상태 변경

Form Validation

Backend에 Request 전송

Response 받기

Response를 화면에 표시
```

---

# 6. Backend란?

Backend는 Server에서 동작하면서 Frontend의 Request를 처리하는 영역이다.

사용자가 직접 Backend Code를 보는 것은 아니다.

---

# 7. Backend의 역할

예:

```text
회원가입

Login

Database 조회

Data 저장

권한 확인

Business Logic

File 처리

AI Model 실행

API 제공
```

등이다.

---

# 8. Backend 기술

대표적으로:

```text
Python
FastAPI
Django
Flask

JavaScript
Node.js

Java
Spring

PHP
```

등이 있다.

---

# 9. 우리가 사용할 구조

AI Web Service를 만든다면:

```text
Frontend
HTML / CSS / JavaScript

        ↓

Backend
Python / FastAPI

        ↓

AI
PyTorch

        ↓

Database
SQL / SQLAlchemy
```

처럼 지금까지 공부한 내용이 연결될 수 있다.

---

# 10. Client와 Server

Frontend와 Backend를 이해할 때:

```text
Client
Server
```

개념도 중요하다.

Browser는 대표적인 Client이다.

---

# 11. 예

사용자가 Chrome에서:

```text
상품 목록 보여줘
```

라는 동작을 수행한다.

Browser가 Server에 Request를 보낸다.

```text
Chrome
Client

↓

Request

↓

Server
```

---

# 12. Server

Server는 Request를 처리하고 Response를 반환한다.

```text
Server
↓
상품 Data 조회
↓
Response
↓
Client
```

---

# 13. Frontend = Client인가?

항상 완전히 같은 의미라고 볼 수는 없다.

하지만 일반적인 Web Application을 처음 공부할 때는:

```text
Browser에서 실행되는 Frontend
→ Client 측

Backend Application
→ Server 측
```

으로 이해하면 쉽다.

---

# 14. Request

Frontend는 Backend에 필요한 작업을 요청한다.

예:

```text
상품 목록 주세요.

Login 해주세요.

회원가입 해주세요.

이 Image를 분석해주세요.
```

---

# 15. Response

Backend는 요청을 처리한 후 결과를 반환한다.

예:

```text
상품 목록

Login 성공

회원가입 완료

AI Prediction 결과
```

---

# 16. HTTP

Frontend와 Backend가 Web에서 통신할 때 대표적으로:

```text
HTTP
```

를 사용한다.

---

# 17. 기본 구조

```text
Frontend
↓
HTTP Request
↓
Backend
↓
HTTP Response
↓
Frontend
```

이다.

---

# 18. URL

어디에 Request를 보낼지 나타낸다.

예:

```text
http://localhost:8000/products
```

---

# 19. HTTP Method

무엇을 하려는 Request인지 표현한다.

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

# 20. 예

```text
GET /products
→ 상품 목록 조회

GET /products/10
→ 10번 상품 조회

POST /products
→ 상품 생성

PATCH /products/10
→ 10번 상품 수정

DELETE /products/10
→ 10번 상품 삭제
```

---

# 21. URL + Method

Web API에서는:

```text
URL
+
HTTP Method
```

를 함께 보고 Request의 의도를 이해한다.

---

# 22. API

API는:

```text
Application Programming Interface
```

의 약자이다.

Frontend가 Backend의 기능을 사용할 수 있도록 정해 놓은 Interface라고 생각할 수 있다.

---

# 23. API 예

쇼핑몰 Backend에 다음 API가 있다고 하자.

```text
GET /products

GET /products/{id}

POST /products

POST /login
```

Frontend는 이 API를 이용한다.

---

# 24. Frontend가 Database에 직접 접근하지 않는 이유

일반적인 Web Application에서는:

```text
Frontend
↓
Backend
↓
Database
```

구조를 사용한다.

다음처럼 Browser가 Database에 직접 접근하도록 만들지 않는다.

```text
Frontend
↓
Database
```

---

# 25. Backend가 중간에 필요한 이유

Backend에서는:

```text
인증

권한 검사

입력값 검증

Business Logic

보안

Database 처리
```

등을 수행할 수 있다.

---

# 26. Database

Database는 Service에 필요한 Data를 저장한다.

예:

```text
회원

상품

게시글

주문

Prediction 기록
```

등이다.

---

# 27. Database 흐름

```text
Frontend
↓
GET /products
↓
Backend
↓
SQL Query
↓
Database
↓
상품 Data
↓
Backend
↓
JSON Response
↓
Frontend
```

---

# 28. SQL과 연결

Backend에서는 Database에:

```sql
SELECT
    *
FROM
    products;
```

같은 Query를 실행할 수 있다.

---

# 29. SQLAlchemy와 연결

Python Backend에서는 SQLAlchemy를 이용하여 Database를 다룰 수도 있다.

개념적으로:

```text
FastAPI
↓
SQLAlchemy
↓
Database
```

구조가 된다.

---

# 30. JSON

Frontend와 Backend가 Data를 주고받을 때 자주 사용하는 형식이다.

예:

```json
{
    "name": "Notebook",
    "price": 1500000
}
```

---

# 31. 여러 Data

```json
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

처럼 Array 형태의 Data도 전달할 수 있다.

---

# 32. JavaScript Object와 JSON

JavaScript:

```javascript
const user = {

    name: "Kim",

    age: 30

};
```

JSON:

```json
{
    "name": "Kim",
    "age": 30
}
```

모양은 비슷하지만 같은 것은 아니다.

---

# 33. fetch

Browser JavaScript에서는 `fetch()`를 이용하여 HTTP Request를 보낼 수 있다.

```javascript
const response =
    await fetch(
        "http://localhost:8000/products"
    );
```

---

# 34. Response를 JSON으로 변환

```javascript
const data =
    await response.json();
```

---

# 35. 전체 GET 예

```javascript
async function getProducts() {

    const response =
        await fetch(
            "http://localhost:8000/products"
        );


    const data =
        await response.json();


    console.log(
        data
    );

}
```

---

# 36. 흐름

```text
getProducts()
↓
fetch()
↓
HTTP GET Request
↓
Backend
↓
Response
↓
response.json()
↓
JavaScript Object / Array
```

이다.

---

# 37. 화면에 출력

```javascript
const result =
    document.querySelector(
        "#result"
    );


result.textContent =
    data.name;
```

---

# 38. Backend 예

나중에 FastAPI에서는 다음과 비슷한 Code를 작성할 수 있다.

```python
from fastapi import FastAPI


app = FastAPI()


@app.get("/products")
def get_products():

    return [
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

# 39. 연결

Frontend:

```javascript
fetch(
    "http://localhost:8000/products"
);
```

Backend:

```python
@app.get("/products")
```

가 연결된다.

---

# 40. 매우 중요한 부분

Frontend에서:

```text
GET /products
```

를 요청한다.

Backend에는:

```text
GET /products
```

를 처리할 Code가 있어야 한다.

---

# 41. URL이 다르면?

Frontend:

```text
GET /products
```

Backend:

```text
GET /product
```

처럼 다르면 원하는 API를 찾지 못할 수 있다.

---

# 42. Method가 다르면?

Frontend:

```text
POST /products
```

Backend:

```text
GET /products
```

만 있다면 서로 다른 Request이다.

---

# 43. 그래서 확인할 것

```text
URL이 같은가?

HTTP Method가 같은가?

보내는 Data 형식이 맞는가?

Backend가 기대하는 Data가 맞는가?
```

를 확인한다.

---

# 44. POST

Frontend에서 Backend로 Data를 보낼 수 있다.

```javascript
async function createProduct() {

    const response =
        await fetch(
            "http://localhost:8000/products",
            {
                method: "POST",

                headers: {
                    "Content-Type":
                        "application/json"
                },

                body: JSON.stringify(
                    {
                        name: "Notebook",
                        price: 1500000
                    }
                )
            }
        );


    const data =
        await response.json();


    console.log(
        data
    );

}
```

---

# 45. POST 구조

```text
URL
→ 어디로?

Method
→ 무엇을 할 것인가?

Headers
→ 어떤 Data인가?

Body
→ 실제 Data
```

이다.

---

# 46. Content-Type

```text
Content-Type: application/json
```

은 Request Body가 JSON 형식이라는 것을 Server에 알려준다.

---

# 47. Backend는 Request를 받는다

개념적으로:

```text
Frontend

{
    "name": "Notebook",
    "price": 1500000
}

↓

Backend
```

이다.

---

# 48. Backend Validation

Backend는 받은 Data가 올바른지 검사해야 한다.

예:

```text
name이 있는가?

price가 숫자인가?

price가 음수가 아닌가?

사용자가 권한이 있는가?
```

등이다.

---

# 49. Frontend Validation vs Backend Validation

Frontend:

```text
사용자 편의

빠른 피드백
```

Backend:

```text
실제 Data 검증

보안

Data 무결성
```

을 담당한다.

Backend Validation은 생략하면 안 된다.

---

# 50. HTTP Status Code

Backend Response에는 처리 결과를 나타내는 Status Code가 있다.

대표적으로:

```text
200
→ 성공

201
→ 생성 성공

400
→ 잘못된 Request

401
→ 인증 필요 또는 인증 실패

403
→ 권한 없음

404
→ Resource를 찾을 수 없음

422
→ 입력 Data 검증 실패 등

500
→ Server 내부 Error
```

가 있다.

---

# 51. 404 예

Frontend가:

```text
GET /product
```

를 요청했는데 Backend에는 해당 Route가 없다면:

```text
404
```

를 볼 수 있다.

---

# 52. 500

Backend Code에서 예상하지 못한 Error가 발생하면:

```text
500 Internal Server Error
```

를 볼 수 있다.

---

# 53. FastAPI에서 자주 볼 수 있는 422

Backend가 기대하는 Data 구조와 실제 Request Data가 맞지 않는 경우 등에:

```text
422
```

를 볼 수 있다.

이때 Request Body와 Backend Schema를 확인한다.

---

# 54. Browser Network Tab

Frontend와 Backend 연결 문제를 확인할 때 매우 중요하다.

Chrome:

```text
F12
↓
Network
```

---

# 55. Network에서 확인할 것

```text
Request URL

Request Method

Status Code

Request Headers

Request Payload

Response
```

등이다.

---

# 56. Console과 Network

JavaScript Error:

```text
Console
```

HTTP 통신 문제:

```text
Network
```

를 우선 확인한다.

---

# 57. localhost

개발 중에는 자기 Computer에서 Server를 실행할 수 있다.

예:

```text
Frontend

http://localhost:5500
```

Backend:

```text
http://localhost:8000
```

---

# 58. 같은 Computer인데 왜 주소가 다를까?

Port가 다르기 때문이다.

```text
localhost:5500
localhost:8000
```

은 같은 Computer를 가리킬 수 있지만 서로 다른 Port의 Service이다.

---

# 59. Origin

Web에서 Origin은 기본적으로:

```text
Protocol
+
Host
+
Port
```

조합으로 구분한다.

예:

```text
http://localhost:5500
```

과:

```text
http://localhost:8000
```

은 Port가 다르기 때문에 서로 다른 Origin이다.

---

# 60. CORS

Frontend와 Backend가 서로 다른 Origin에 있을 때 Browser의 Same-Origin Policy와 관련하여:

```text
CORS
```

설정이 필요할 수 있다.

CORS는:

```text
Cross-Origin Resource Sharing
```

의 약자이다.

---

# 61. 흔한 상황

Frontend:

```text
http://localhost:5500
```

Backend:

```text
http://localhost:8000
```

에서 개발한다.

Browser가 Cross-Origin Request를 수행하므로 Backend의 CORS 설정이 필요할 수 있다.

---

# 62. FastAPI CORS 예

나중에 FastAPI에서:

```python
from fastapi.middleware.cors import (
    CORSMiddleware
)


app.add_middleware(
    CORSMiddleware,

    allow_origins=[
        "http://localhost:5500"
    ],

    allow_credentials=True,

    allow_methods=["*"],

    allow_headers=["*"]
)
```

같은 설정을 볼 수 있다.

---

# 63. 개발 중 `*`

개발 편의를 위해 모든 Origin을 허용하는 예제를 볼 수도 있지만 실제 Service에서는 필요한 Origin만 허용하도록 설계하는 것이 좋다.

---

# 64. Frontend와 AI Model

지금까지 학습한 AI Model도 Backend에 연결할 수 있다.

```text
Frontend
↓
Backend
↓
AI Model
```

---

# 65. AI Model을 Browser에서 직접 실행해야 할까?

가능한 기술도 있지만 지금 학습할 구조에서는 Python AI Model을 Backend에서 실행하는 방식으로 이해한다.

```text
Browser
↓
FastAPI
↓
PyTorch Model
```

---

# 66. AI Prediction API

예:

```text
POST /predict
```

---

# 67. Image Upload

사용자가 X-ray Image를 선택한다.

HTML:

```html
<input
    id="image-input"
    type="file"
    accept="image/*"
>
```

---

# 68. 분석 Button

```html
<button
    id="predict-button"
    type="button"
>
    분석하기
</button>
```

---

# 69. 결과 영역

```html
<div id="result">
</div>
```

---

# 70. JavaScript

개념적으로:

```javascript
const imageInput =
    document.querySelector(
        "#image-input"
    );


const button =
    document.querySelector(
        "#predict-button"
    );


const result =
    document.querySelector(
        "#result"
    );
```

---

# 71. Click

```javascript
button.addEventListener(
    "click",
    async () => {

        const file =
            imageInput.files[0];


        if (!file) {

            result.textContent =
                "이미지를 선택해주세요.";

            return;

        }

    }
);
```

---

# 72. FormData

Image 같은 File을 전송할 때:

```javascript
const formData =
    new FormData();


formData.append(
    "file",
    file
);
```

처럼 사용할 수 있다.

---

# 73. API Request

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

# 74. File Upload에서 Content-Type

`FormData`를 `fetch()`로 보낼 때는 Browser가 필요한 `multipart/form-data` Header와 boundary를 설정하도록 두는 것이 일반적이다.

직접:

```text
Content-Type: multipart/form-data
```

를 수동 지정하면 boundary 문제가 생길 수 있다.

---

# 75. Response

```javascript
const data =
    await response.json();
```

---

# 76. 화면 표시

```javascript
result.textContent =
    `예측: ${data.prediction}`;
```

---

# 77. 전체 흐름

```text
Image 선택
↓
Button Click
↓
JavaScript
↓
FormData
↓
POST /predict
↓
FastAPI
↓
Image 전처리
↓
PyTorch Model
↓
Prediction
↓
JSON
↓
JavaScript
↓
DOM
↓
결과 화면
```

---

# 78. AI Backend에서 중요한 것

Model만 있다고 바로 Service가 되는 것은 아니다.

Backend에서:

```text
Model Load

Input Validation

Image 읽기

Resize

Channel 변환

Tensor 변환

Normalize

Device 이동

Inference

Probability 계산

Response 생성
```

등이 필요할 수 있다.

---

# 79. 학습과 추론의 전처리

AI Model 학습에서 사용한 전처리와 Service 추론에서 사용하는 전처리가 일치해야 한다.

예:

```text
Resize

RGB 변환

Normalize

Class Mapping
```

등이다.

---

# 80. 그렇지 않으면

Model 자체는 같아도 입력 Data 처리 방식이 달라져 Prediction이 잘못될 수 있다.

---

# 81. Class Mapping

Model Output:

```text
0
1
```

이라면 Backend 또는 Frontend에서:

```text
0 → Normal

1 → Pneumonia
```

처럼 의미를 연결해야 한다.

---

# 82. Probability

Backend가 다음과 같이 반환할 수도 있다.

```json
{
    "prediction": 1,
    "label": "Pneumonia",
    "probability": 0.93
}
```

---

# 83. Frontend

```javascript
result.textContent =
    `${data.label} / ${data.probability}`;
```

처럼 표시할 수 있다.

---

# 84. 실제 화면에서는

```text
분석 결과

Pneumonia

Confidence: 93%
```

같이 사용자에게 이해하기 쉽게 표시할 수 있다.

---

# 85. 하지만 의료 AI에서는

Model Probability를 그대로:

```text
질병 확률
```

이라고 단정해서 표현하는 것은 주의해야 한다.

Model Score 또는 Confidence가 실제 임상적 질병 확률과 동일하다고 볼 수 없기 때문이다.

---

# 86. Loading 상태

AI Prediction에는 시간이 걸릴 수 있다.

그래서 Frontend에서:

```javascript
result.textContent =
    "분석 중...";
```

같은 상태를 표시할 수 있다.

---

# 87. Button 비활성화

```javascript
button.disabled =
    true;
```

Request가 끝나면:

```javascript
button.disabled =
    false;
```

로 복원할 수 있다.

---

# 88. Error 처리

Network나 Backend에서 Error가 발생할 수 있다.

```javascript
try {

    // API Request

} catch (error) {

    console.error(
        error
    );

}
```

---

# 89. response.ok

HTTP Response가 성공 범위인지 확인할 수 있다.

```javascript
if (!response.ok) {

    throw new Error(
        "API 요청 실패"
    );

}
```

---

# 90. AI Request 예

```javascript
button.addEventListener(
    "click",
    async () => {

        const file =
            imageInput.files[0];


        if (!file) {

            result.textContent =
                "이미지를 선택해주세요.";

            return;

        }


        const formData =
            new FormData();


        formData.append(
            "file",
            file
        );


        button.disabled =
            true;


        result.textContent =
            "분석 중...";


        try {

            const response =
                await fetch(
                    "http://localhost:8000/predict",
                    {
                        method: "POST",
                        body: formData
                    }
                );


            if (!response.ok) {

                throw new Error(
                    "Prediction 요청 실패"
                );

            }


            const data =
                await response.json();


            result.textContent =
                `${data.label} / ${data.probability}`;


        } catch (error) {

            console.error(
                error
            );


            result.textContent =
                "분석 중 오류가 발생했습니다.";


        } finally {

            button.disabled =
                false;

        }

    }
);
```

---

# 91. try / catch / finally

```text
try
→ 실행할 작업

catch
→ Error 처리

finally
→ 성공/실패와 관계없이 마지막에 실행
```

이다.

---

# 92. Frontend 상태

API와 연결된 Frontend에서는 최소한:

```text
Initial

Loading

Success

Error
```

상태를 생각하는 것이 좋다.

---

# 93. 예

```text
Initial
이미지를 선택해주세요.

↓

Loading
분석 중...

↓

Success
Pneumonia / 93%

또는

Error
분석 중 오류가 발생했습니다.
```

---

# 94. Authentication

Login이 필요한 Service에서는 사용자가 누구인지 확인해야 한다.

이를:

```text
Authentication
인증
```

이라고 한다.

---

# 95. Authorization

인증된 사용자가 특정 기능을 사용할 권한이 있는지 확인하는 것을:

```text
Authorization
인가
```

라고 한다.

---

# 96. 차이

```text
Authentication
→ 누구인가?

Authorization
→ 무엇을 할 수 있는가?
```

이다.

---

# 97. 예

```text
Login
→ Authentication

관리자 Page 접근
→ Authorization
```

---

# 98. Password

Password를 Database에 평문 그대로 저장하면 안 된다.

실제 Backend에서는 안전한 Password Hashing 방식을 사용해야 한다.

---

# 99. Secret

다음과 같은 정보도 Frontend Code에 직접 넣으면 안 된다.

```text
Database Password

Private API Key

Secret Key
```

---

# 100. 이유

Browser에 전달된 Frontend Code는 사용자가 확인할 수 있기 때문이다.

---

# 101. Secret은 어디에?

민감한 Secret은 일반적으로 Backend 환경 변수나 Secret 관리 시스템 등에서 관리한다.

---

# 102. Frontend에 있어도 되는 것

공개되어도 문제가 없는:

```text
API Endpoint

화면 Text

Public 설정값
```

등은 Frontend에서 사용할 수 있다.

단, 서비스별 공개 키의 보안 모델은 별도로 확인해야 한다.

---

# 103. Static Website

Backend 없이 HTML/CSS/JavaScript 중심으로 제공되는 Website를 만들 수도 있다.

예:

```text
Portfolio

회사 소개

Landing Page
```

---

# 104. Dynamic Web Application

Server Data와 상호작용하는 Service이다.

예:

```text
Login

Shopping Mall

Dashboard

AI Prediction

SNS
```

---

# 105. SPA

JavaScript Framework를 사용하는 Web에서:

```text
SPA
Single Page Application
```

이라는 구조도 자주 볼 수 있다.

Page 전체를 매번 새로 받기보다 JavaScript가 필요한 Data와 UI를 동적으로 갱신하는 방식이다.

---

# 106. React / Vue

규모가 커지면 순수 JavaScript로 DOM을 직접 관리하는 것이 복잡해질 수 있다.

그래서:

```text
React

Vue
```

같은 Frontend Library / Framework를 사용할 수 있다.

---

# 107. Next.js

React 기반으로 Routing, Rendering, Server 기능 등 여러 기능을 제공하는 Framework이다.

---

# 108. 하지만 기본기가 중요한 이유

React를 사용하더라도:

```text
HTML 구조

CSS

JavaScript

DOM 개념

Event

HTTP

API

JSON

Frontend / Backend
```

을 이해해야 한다.

---

# 109. 배포

Localhost에서만 실행하면 자신의 Computer에서만 개발용으로 접근하는 상태이다.

다른 사용자가 인터넷에서 접근할 수 있도록 Service를 공개하는 과정을:

```text
Deployment
배포
```

라고 한다.

---

# 110. 배포 후 구조

```text
사용자 Browser
↓
Internet
↓
Domain
↓
Server
↓
Backend
↓
Database / AI Model
```

같은 형태가 된다.

---

# 111. Domain

예:

```text
example.com
```

같은 사람이 읽기 쉬운 주소를 사용한다.

---

# 112. HTTPS

실제 Service에서는 일반적으로:

```text
HTTPS
```

를 사용하여 통신을 보호한다.

---

# 113. Docker

Backend와 AI Model을 실행하기 위해 필요한 환경을 Container로 구성할 수 있다.

예:

```text
Python Version

Package

FastAPI

PyTorch

Application Code
```

등을 Docker Image에 구성할 수 있다.

---

# 114. Docker 연결

```text
GitHub
↓
Source Code
↓
Docker Image
↓
Server
↓
Container 실행
↓
FastAPI
↓
AI Model
```

같은 배포 흐름으로 발전할 수 있다.

---

# 115. GitHub

Web Project도 Git으로 Version Control할 수 있다.

```text
HTML

CSS

JavaScript

Python

FastAPI

Dockerfile

README
```

등을 관리한다.

---

# 116. 단, 올리면 안 되는 것

예:

```text
.env

Password

API Secret

Private Key

민감한 의료 Data

불필요한 대용량 Model
```

등은 주의한다.

---

# 117. `.gitignore`

Git에 포함하지 않을 File을 설정한다.

예:

```gitignore
.env

__pycache__/

.venv/

node_modules/
```

프로젝트 환경에 맞게 작성한다.

---

# 118. 전체 AI Web Service 구조

```text
User
↓
Browser
↓
HTML / CSS
↓
JavaScript
↓
Event
↓
fetch()
↓
HTTP Request
↓
FastAPI
↓
Input Validation
↓
Image Preprocessing
↓
PyTorch Model
↓
Prediction
↓
JSON Response
↓
JavaScript
↓
DOM
↓
Result UI
```

---

# 119. Database까지 추가

```text
User
↓
Frontend
↓
FastAPI
↓
AI Model
↓
Prediction
↓
Database 저장
↓
JSON Response
↓
Frontend
```

같은 구조도 만들 수 있다.

---

# 120. 예

AI 분석 기록을:

```text
사용자 ID

Image 이름

Prediction

Confidence

분석 시간
```

등으로 Database에 저장할 수도 있다.

단, 실제 의료 데이터는 개인정보 및 의료정보 처리 요구사항을 반드시 고려해야 한다.

---

# 121. Frontend Error

대표적으로:

```text
JavaScript Syntax Error

DOM Selector Error

잘못된 File Path

잘못된 API URL

CORS Error
```

등이 있다.

---

# 122. Backend Error

대표적으로:

```text
Python Error

Validation Error

Database Error

Model Load Error

File Path Error

Tensor Shape Error

CUDA Error
```

등이 있다.

---

# 123. 연결 Error

Frontend와 Backend 각각은 정상인데 연결에서 문제가 생길 수도 있다.

예:

```text
URL 불일치

Method 불일치

Port 불일치

JSON 구조 불일치

Field 이름 불일치

CORS

Backend Server 미실행
```

등이다.

---

# 124. Debugging 순서

Frontend와 Backend가 연결되지 않으면 무작정 Code 전체를 수정하지 않는다.

다음 순서로 확인한다.

```text
1. Backend Server가 실행 중인가?

2. API URL이 맞는가?

3. HTTP Method가 맞는가?

4. Browser Network에 Request가 보이는가?

5. Status Code는 무엇인가?

6. Request Body가 맞는가?

7. Backend Terminal에 Error가 있는가?

8. Response 내용은 무엇인가?

9. Frontend JavaScript가 Response를 제대로 처리하는가?

10. DOM에 제대로 표시하는가?
```

---

# 125. 문제를 구간으로 나누기

```text
Frontend
↓
Network
↓
Backend
↓
Database / Model
```

어느 구간에서 문제가 발생했는지 먼저 찾는다.

---

# 126. 예

Button을 눌러도 아무 일도 없다.

먼저:

```text
Click Event가 발생하는가?
```

를 확인한다.

---

# 127. Click은 된다

그다음:

```text
fetch()가 실행되는가?
```

를 Network에서 확인한다.

---

# 128. Request는 갔다

그다음:

```text
Status Code는?
```

를 확인한다.

---

# 129. 500이다

이 경우 Frontend보다:

```text
Backend Terminal Error
```

를 확인해야 한다.

---

# 130. 200이다

Response도 정상이라면:

```text
JavaScript가 Response를 제대로 읽는가?

DOM 출력 Code가 맞는가?
```

를 확인한다.

---

# 131. 이 방식이 중요한 이유

Web Application은 여러 기술이 연결되어 있기 때문에:

```text
어디가 문제인지
```

를 먼저 찾는 능력이 중요하다.

---

# 132. Frontend 개발자의 관점

```text
사용자는 무엇을 보는가?

어떤 입력을 하는가?

어떤 Event가 발생하는가?

어떤 Request를 보내는가?

Response를 어떻게 보여줄 것인가?
```

를 생각한다.

---

# 133. Backend 개발자의 관점

```text
어떤 Request를 받을 것인가?

어떤 Data가 필요한가?

Data가 올바른가?

어떤 Logic을 실행할 것인가?

Database가 필요한가?

어떤 Response를 반환할 것인가?
```

를 생각한다.

---

# 134. AI 개발자의 관점

```text
Model Input은 무엇인가?

Preprocessing은 무엇인가?

Model Output은 무엇인가?

Threshold는 무엇인가?

Class Mapping은 무엇인가?

Inference Device는 무엇인가?
```

를 생각한다.

---

# 135. Full Stack + AI

이 세 영역을 연결하면:

```text
Frontend
+
Backend
+
AI
+
Database
```

가 하나의 Service가 된다.

---

# 136. 지금까지 공부한 내용 연결

```text
Python
↓
Data Analysis
↓
Machine Learning
↓
Deep Learning
↓
Computer Vision
↓
AI Model

SQL / SQLAlchemy
↓
Database

HTML / CSS / JavaScript
↓
Frontend

HTTP / API
↓
통신

FastAPI
↓
Backend

Docker
↓
배포 환경

Git / GitHub
↓
Version Control
```

---

# 137. 최종적으로 만들 수 있는 구조

```text
AI Web Service
│
├── Frontend
│   ├── HTML
│   ├── CSS
│   └── JavaScript
│
├── Backend
│   └── FastAPI
│
├── AI
│   └── PyTorch
│
├── Database
│   └── SQLAlchemy
│
├── Deployment
│   └── Docker
│
└── Version Control
    └── Git / GitHub
```

---

# 138. Chapter 10 전체 흐름

```text
web-basic.md
↓
Web / Client / Server

html-basic.md
↓
화면 구조

css-basic.md
↓
디자인 / Layout / Responsive

javascript-basic.md
↓
Programming Logic

dom-event.md
↓
화면 조작 / 사용자 Event

frontend-backend.md
↓
Frontend와 Server 연결
```

---

# 139. Chapter 11과 연결

다음 Chapter에서는 지금 여러 번 등장한:

```text
Network

IP

Port

DNS

URL

HTTP

HTTPS

Request

Response

Method

Header

Body

Status Code

JSON

API
```

를 더 자세히 공부한다.

---

# 140. 이후 FastAPI

HTTP와 API 구조를 이해한 뒤 Python으로 실제 Backend를 만든다.

```text
Frontend
↓
HTTP
↓
FastAPI
```

를 직접 구현한다.

---

# 141. 이후 Docker

FastAPI와 AI Model이 동작하면:

```text
Application
↓
Docker
↓
Server
↓
Deployment
```

로 발전시킬 수 있다.

---

# 142. 최종 목표

지금까지의 학습은 각각 따로 존재하는 것이 아니다.

```text
Python
AI
Computer Vision
Database
Web
HTTP
FastAPI
Docker
GitHub
```

를 연결하여 하나의 실제 Service를 만드는 것이 가능하다.

---

# 143. 가장 중요한 흐름

다음 구조를 기억한다.

```text
사용자
↓
Frontend
↓
HTTP Request
↓
Backend
↓
Database / AI Model
↓
HTTP Response
↓
Frontend
↓
사용자
```

---

# 144. AI Service에서는

```text
사용자
↓
Image Upload
↓
JavaScript
↓
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
JavaScript
↓
DOM
↓
Prediction Result
```

이다.

---

# 145. 한 문장으로 정리

Frontend와 Backend의 관계는:

```text
Frontend가 사용자의 입력과 화면을 담당하고,

HTTP를 통해 Backend에 필요한 작업을 요청하며,

Backend가 Database 또는 AI Model 등의
실제 Logic을 처리한 뒤 Response를 반환하고,

Frontend가 그 결과를 다시 사용자에게 보여주는 구조
```

라고 이해할 수 있다.