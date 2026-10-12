# Web Basic

## 1. Web이란?

Web은 인터넷을 이용하여 문서, 이미지, 영상, 데이터 등을 주고받는 시스템이다.

우리가 Chrome 같은 Browser에서 접속하는:

```text
Google

Naver

YouTube

Shopping Mall

회사 홈페이지
```

등이 모두 Web을 이용한다.

---

# 2. Internet과 Web은 같은 것인가?

완전히 같은 것은 아니다.

```text
Internet
→ Computer들을 연결하는 Network

Web
→ Internet을 이용하는 Service 중 하나
```

이다.

즉:

```text
Internet
    ↓
Web
Email
File Transfer
Online Game
...
```

처럼 생각할 수 있다.

---

# 3. Web의 기본 구조

Web을 이해할 때 가장 먼저 알아야 하는 것은:

```text
Client
↕
Server
```

구조이다.

---

# 4. Client

Client는 Server에 무엇인가를 요청하는 쪽이다.

대표적으로:

```text
Chrome

Edge

Safari

Firefox
```

같은 Web Browser가 있다.

---

# 5. 예

사용자가 Chrome에서 쇼핑몰에 접속한다.

```text
사용자
↓
Chrome
↓
쇼핑몰 Server에 요청
```

여기서 Chrome은:

```text
Client
```

이다.

---

# 6. Server

Server는 Client의 요청을 받고 필요한 작업을 처리한 후 결과를 돌려주는 쪽이다.

예:

```text
Client

상품 목록 보여줘
        ↓

Server

상품 Data 조회
        ↓

Client

상품 목록 표시
```

---

# 7. Request와 Response

Web에서는 Client와 Server가:

```text
Request
Response
```

를 주고받는다.

---

# 8. Request

Client가 Server에게 보내는 요청이다.

예:

```text
상품 목록 보여줘.

로그인해줘.

회원가입해줘.

이 Image 분석해줘.
```

---

# 9. Response

Server가 Request를 처리한 후 Client에게 보내는 결과이다.

예:

```text
상품 목록

로그인 성공

회원가입 완료

AI Prediction 결과
```

---

# 10. 기본 흐름

```text
Client
   │
   │ Request
   ▼
Server
   │
   │ Processing
   ▼
Database
   │
   │ Data
   ▼
Server
   │
   │ Response
   ▼
Client
```

---

# 11. Browser란?

Browser는 Web Page를 사용자에게 보여주는 프로그램이다.

예:

```text
Chrome

Edge

Safari

Firefox
```

---

# 12. Browser가 하는 일

Browser는 Server에서 받은:

```text
HTML

CSS

JavaScript
```

등을 해석해서 화면으로 보여준다.

---

# 13. Web Page의 세 가지 핵심

Frontend Web 개발에서는 가장 먼저:

```text
HTML

CSS

JavaScript
```

를 배운다.

---

# 14. HTML

HTML은 Web Page의:

```text
구조
```

를 만든다.

예:

```html
<h1>강남개발자들</h1>

<p>홈페이지 제작 서비스</p>

<button>문의하기</button>
```

---

# 15. HTML을 건물에 비유하면

HTML은:

```text
건물의 뼈대
```

와 같다.

예:

```text
제목

메뉴

본문

이미지

버튼

Footer
```

등의 구조를 만든다.

---

# 16. CSS

CSS는 HTML에:

```text
디자인
```

을 적용한다.

예:

```css
h1 {
    font-size: 40px;
}

button {
    border-radius: 10px;
}
```

---

# 17. CSS를 건물에 비유하면

CSS는:

```text
인테리어
```

와 비슷하다.

HTML이:

```text
버튼이 있다.
```

를 정의한다면 CSS는:

```text
버튼은 검은색이다.

글자는 흰색이다.

모서리는 둥글다.
```

를 결정한다.

---

# 18. JavaScript

JavaScript는 Web Page에:

```text
동작
```

을 추가한다.

예:

```javascript
button.addEventListener("click", function () {
    alert("문의가 접수되었습니다.");
});
```

---

# 19. JavaScript를 건물에 비유하면

JavaScript는:

```text
전기와 장치
```

같은 역할을 한다.

예:

```text
버튼 클릭

메뉴 열기

Popup 표시

Image Slider

Server에 Data 요청
```

등이다.

---

# 20. HTML + CSS + JavaScript

세 가지를 합치면:

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

# 21. 예

쇼핑몰 상품 Card가 있다고 하자.

HTML:

```text
상품 이름

가격

구매 버튼
```

을 만든다.

CSS:

```text
Card 크기

색상

글자 크기

Button 디자인
```

을 만든다.

JavaScript:

```text
구매 버튼 클릭

장바구니 추가

수량 변경
```

등을 처리한다.

---

# 22. Frontend

사용자가 직접 보고 사용하는 부분을:

```text
Frontend
```

라고 한다.

---

# 23. Frontend 예

```text
홈페이지 화면

메뉴

Button

Login Form

상품 목록

Image Slider

Popup

Dashboard
```

등이다.

---

# 24. Frontend 기술

기본적으로:

```text
HTML

CSS

JavaScript
```

가 있다.

그 이후에는:

```text
React

Vue

Next.js
```

같은 기술로 발전할 수 있다.

---

# 25. Backend

Backend는 사용자가 직접 보지는 않지만 Server에서 동작하는 부분이다.

---

# 26. Backend가 하는 일

예:

```text
Login 처리

회원가입 처리

Database 조회

상품 저장

게시글 저장

결제 처리

AI Model 실행

API 제공
```

등이다.

---

# 27. Backend 기술

예:

```text
Python

FastAPI

Django

Flask

Node.js

Java Spring

PHP
```

등이 있다.

---

# 28. Frontend와 Backend

```text
사용자
↓
Frontend
↓
Request
↓
Backend
↓
Database
↓
Backend
↓
Response
↓
Frontend
↓
사용자
```

구조로 연결된다.

---

# 29. 예 - Login

사용자가 Login Form에 입력한다.

```text
ID
user01

Password
1234
```

---

# 30. Frontend

HTML Form에서 입력값을 받는다.

```html
<input type="text">

<input type="password">

<button>로그인</button>
```

---

# 31. JavaScript

입력한 Data를 Backend에 보낸다.

개념적으로:

```text
POST /login
```

같은 Request를 보낼 수 있다.

---

# 32. Backend

Backend는:

```text
ID 확인

Password 확인

Database 조회
```

등을 수행한다.

---

# 33. Response

성공하면:

```text
로그인 성공
```

실패하면:

```text
아이디 또는 비밀번호가 잘못되었습니다.
```

같은 Response를 보낸다.

---

# 34. Database

Web Service에서는 Data를 저장해야 하는 경우가 많다.

예:

```text
회원정보

상품정보

게시글

주문정보

AI Prediction 결과
```

등이다.

---

# 35. Database 연결

```text
Frontend
↓
Backend
↓
Database
```

구조이다.

Frontend에서 Database에 직접 접근하는 구조보다는 Backend를 통해 접근하는 것이 일반적이다.

---

# 36. 지금까지 배운 Database와 연결

앞에서 공부한:

```text
SQL

SQLAlchemy

MongoDB
```

가 여기서 사용된다.

---

# 37. 예

```text
Browser
↓
상품 목록 요청
↓
Backend
↓
SQL
↓
Database
↓
상품 Data
↓
Backend
↓
Browser
```

이다.

---

# 38. URL

Browser에서 Web Site에 접속할 때 사용하는 주소를:

```text
URL
```

이라고 한다.

URL은:

```text
Uniform Resource Locator
```

의 약자이다.

---

# 39. 예

```text
https://example.com/products
```

---

# 40. URL을 단순하게 나누면

```text
https
://
example.com
/products
```

로 볼 수 있다.

---

# 41. Protocol

```text
https
```

부분은 어떤 방식으로 통신할 것인지 나타낸다.

대표적으로:

```text
HTTP

HTTPS
```

가 있다.

---

# 42. Domain

```text
example.com
```

은 Domain이다.

사람이 Server를 쉽게 찾을 수 있도록 사용하는 이름이다.

---

# 43. Path

```text
/products
```

는 Server에서 어떤 Resource를 요청하는지 나타내는 Path이다.

---

# 44. URL 예

```text
https://shop.example.com/products/10
```

개념적으로:

```text
Protocol
https

Domain
shop.example.com

Path
/products/10
```

으로 볼 수 있다.

---

# 45. Domain과 IP

Computer Network에서는 실제로 IP Address를 이용하여 Computer를 찾는다.

하지만 사람이:

```text
142.xxx.xxx.xxx
```

같은 IP를 모두 외우기 어렵다.

그래서:

```text
example.com
```

같은 Domain을 사용한다.

---

# 46. DNS

Domain을 IP Address로 찾아주는 시스템을:

```text
DNS
```

라고 한다.

---

# 47. DNS 흐름

```text
example.com
↓
DNS
↓
IP Address
↓
Server
```

처럼 생각할 수 있다.

---

# 48. Server와 Port

한 Computer에서 여러 Service가 동작할 수 있다.

이때 어떤 Program과 통신할지 구분하기 위해:

```text
Port
```

를 사용한다.

---

# 49. 예

개발 중 FastAPI를 실행하면:

```text
127.0.0.1:8000
```

같은 주소를 볼 수 있다.

---

# 50. 여기서

```text
127.0.0.1
→ IP

8000
→ Port
```

이다.

---

# 51. localhost

자기 Computer를 의미하는 주소로:

```text
localhost
```

를 많이 사용한다.

예:

```text
http://localhost:8000
```

---

# 52. 127.0.0.1

```text
127.0.0.1
```

도 자신의 Computer를 가리키는 Loopback Address로 많이 사용한다.

---

# 53. 개발할 때

Frontend를:

```text
localhost:3000
```

Backend를:

```text
localhost:8000
```

처럼 서로 다른 Port에서 실행할 수도 있다.

---

# 54. HTTP

Client와 Server가 Web에서 Request와 Response를 주고받기 위해 사용하는 대표적인 Protocol이다.

```text
HTTP
=
HyperText Transfer Protocol
```

---

# 55. HTTPS

HTTPS는 HTTP 통신을 TLS를 이용하여 보호하는 방식이다.

```text
HTTP
+
TLS
=
HTTPS
```

처럼 이해할 수 있다.

---

# 56. HTTPS가 필요한 이유

Network 중간에서 Data가 그대로 노출되거나 변조되는 위험을 줄이기 위해 통신을 암호화하고 Server의 신원을 검증하는 데 사용된다.

---

# 57. HTTP Method

Request의 의도를 표현하기 위해 Method를 사용한다.

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

# 58. GET

Data를 조회할 때 주로 사용한다.

```text
GET /products
```

개념적으로:

```text
상품 목록을 보여줘.
```

이다.

---

# 59. POST

새로운 Data를 보내거나 생성하는 작업에 주로 사용한다.

```text
POST /users
```

개념적으로:

```text
새로운 회원을 만들어줘.
```

이다.

---

# 60. PUT / PATCH

기존 Data를 수정할 때 사용한다.

```text
PATCH /users/10
```

개념적으로:

```text
10번 회원 정보를 수정해줘.
```

이다.

---

# 61. DELETE

Data를 삭제할 때 사용한다.

```text
DELETE /users/10
```

개념적으로:

```text
10번 회원을 삭제해줘.
```

이다.

---

# 62. URL + HTTP Method

같은 URL이라도 Method에 따라 의도가 달라질 수 있다.

```text
GET /products
→ 상품 조회

POST /products
→ 상품 생성
```

이다.

---

# 63. API

Frontend와 Backend가 Data를 주고받을 수 있도록 정의한 Interface를 API라고 부른다.

```text
API
=
Application Programming Interface
```

---

# 64. Web API

예:

```text
GET /products

POST /login

POST /predict
```

등이다.

---

# 65. AI와 API

지금까지 만든 AI Model도 API로 연결할 수 있다.

---

# 66. 예 - Chest X-ray AI

```text
사용자
↓
X-ray Image Upload
↓
Frontend
↓
POST /predict
↓
FastAPI
↓
PyTorch Model
↓
Prediction
↓
JSON Response
↓
Frontend
↓
결과 표시
```

---

# 67. 여기서 지금까지 공부한 것이 연결된다

```text
Python
→ AI Model

PyTorch
→ Image Classification

FastAPI
→ Backend API

HTML / CSS / JavaScript
→ Frontend

HTTP
→ Client와 Server 통신

Docker
→ 실행 환경 구성
```

---

# 68. JSON

Frontend와 Backend가 Data를 주고받을 때 자주 사용하는 형식이다.

예:

```json
{
    "name": "apple",
    "price": 1000
}
```

---

# 69. Python Dictionary와 비슷하다

Python:

```python
product = {
    "name": "apple",
    "price": 1000
}
```

JSON:

```json
{
    "name": "apple",
    "price": 1000
}
```

모양이 비슷하다.

---

# 70. AI Response 예

Backend가 다음과 같은 JSON을 반환할 수 있다.

```json
{
    "prediction": 1,
    "probability": 0.93
}
```

Frontend는 이 Data를 받아:

```text
Prediction
Pneumonia

Probability
93%
```

처럼 화면에 보여줄 수 있다.

---

# 71. Static Web Page

Server Data와 복잡하게 연결하지 않고 미리 만들어진 HTML, CSS, JavaScript 중심으로 보여주는 Page를 생각할 수 있다.

예:

```text
회사 소개 Page

Portfolio

Landing Page
```

---

# 72. Dynamic Web Application

사용자나 Data에 따라 내용이 바뀌는 Web Service이다.

예:

```text
Shopping Mall

SNS

Dashboard

AI Prediction Service

Admin Page
```

---

# 73. Website와 Web Application

명확하게 완전히 분리되는 용어는 아니지만 초보 단계에서는:

```text
Website
→ 정보 제공 중심

Web Application
→ 사용자 Interaction과 기능 중심
```

정도로 이해할 수 있다.

---

# 74. Web Server

Client의 HTTP Request를 받아 Web Content를 제공하는 Server Software가 있다.

예:

```text
Nginx

Apache
```

등이다.

---

# 75. Application Server

실제 Business Logic을 처리하는 Application이 있다.

예:

```text
FastAPI

Django

Node.js
```

등이다.

---

# 76. 단순화해서 보면

```text
Browser
↓
Web Server
↓
Backend Application
↓
Database / AI Model
```

같은 구조를 생각할 수 있다.

실제 Architecture는 서비스에 따라 달라질 수 있다.

---

# 77. Frontend와 Backend를 나누는 이유

역할을 분리하면:

```text
화면 개발

Server Logic

Database

AI Model
```

을 각각 관리하기 쉬워진다.

---

# 78. Full Stack

Frontend와 Backend를 모두 다루는 개발을 흔히:

```text
Full Stack Development
```

라고 부른다.

---

# 79. 개발자가 Web Page를 만드는 과정

간단하게:

```text
HTML 작성
↓
CSS 작성
↓
JavaScript 작성
↓
Browser에서 확인
↓
Backend 연결
↓
Database 연결
↓
API 연결
↓
배포
```

순서로 발전할 수 있다.

---

# 80. VS Code

HTML, CSS, JavaScript File도 VS Code에서 작성할 수 있다.

예:

```text
index.html

style.css

script.js
```

---

# 81. 기본 Project 구조

```text
my-web/
├── index.html
├── style.css
└── script.js
```

---

# 82. 역할

```text
index.html
→ 구조

style.css
→ 디자인

script.js
→ 동작
```

이다.

---

# 83. Browser는 어떻게 연결할까?

HTML에서 CSS를 연결한다.

```html
<link
    rel="stylesheet"
    href="style.css"
>
```

---

# 84. JavaScript 연결

```html
<script src="script.js"></script>
```

---

# 85. 결과

```text
index.html
   │
   ├── style.css
   │
   └── script.js
```

처럼 연결된다.

---

# 86. HTML을 Browser에서 열면

Browser가 HTML을 읽고:

```text
HTML
↓
구조 생성

CSS
↓
디자인 적용

JavaScript
↓
동작 적용
```

을 수행한다.

---

# 87. 개발자 도구

Chrome에서는:

```text
F12
```

또는:

```text
Ctrl + Shift + I
```

로 Developer Tools를 열 수 있다.

---

# 88. Developer Tools

대표적으로:

```text
Elements

Console

Network

Application
```

등을 확인할 수 있다.

---

# 89. Elements

현재 Page의:

```text
HTML

CSS
```

구조를 확인한다.

---

# 90. Console

JavaScript의:

```text
print 결과

Error

Warning
```

등을 확인한다.

Python의 Terminal 출력과 비슷한 Debugging 역할을 한다.

---

# 91. Network

Browser가 Server와 주고받은:

```text
Request

Response
```

를 확인할 수 있다.

---

# 92. Network Tab의 중요성

나중에 API를 공부할 때:

```text
어떤 URL로 요청했는가?

GET인가 POST인가?

Status Code는?

어떤 JSON을 보냈는가?

무슨 Response가 왔는가?
```

를 직접 확인할 수 있다.

---

# 93. Web Debugging

Web 개발에서도 Error가 발생하면 한 단계씩 확인한다.

```text
HTML 구조가 맞는가?

CSS File이 연결됐는가?

JavaScript File이 연결됐는가?

Console Error가 있는가?

Network Request가 성공했는가?

Backend가 실행 중인가?
```

순서로 확인한다.

---

# 94. AI Debugging과 비슷하다

AI에서는:

```text
Data
↓
Shape
↓
Model
↓
Loss
```

를 확인했다.

Web에서는:

```text
HTML
↓
CSS
↓
JavaScript
↓
Network
↓
Backend
```

를 확인한다.

---

# 95. Web 전체 구조

```text
사용자
        ↓
Browser
        ↓
Frontend
HTML / CSS / JavaScript
        ↓
HTTP Request
        ↓
Backend
FastAPI / Node / etc.
        ↓
Database
        +
AI Model
        ↓
Backend
        ↓
HTTP Response
        ↓
Frontend
        ↓
Browser
        ↓
사용자
```

---

# 96. 지금 Chapter에서 배울 것

```text
Web Basic
↓
HTML
↓
CSS
↓
JavaScript
↓
DOM / Event
↓
Frontend / Backend
```

이다.

---

# 97. 그 다음 Chapter

Web의 화면 구조를 이해한 뒤:

```text
Network

HTTP

API
```

를 더 자세히 공부한다.

---

# 98. 그 다음

Python으로:

```text
FastAPI
```

Backend를 만든다.

---

# 99. 최종 연결

```text
HTML / CSS / JavaScript
        ↓
Frontend
        ↓
HTTP / API
        ↓
FastAPI
        ↓
Python
        ↓
AI Model
        ↓
Prediction
```

으로 연결한다.

---

# 100. 가장 중요한 개념

Web을 처음 배울 때 다음 구조를 기억한다.

```text
Client
↓
Request
↓
Server
↓
Processing
↓
Response
↓
Client
```

그리고 화면에서는:

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

# 101. 한 문장으로 정리

Web Application은:

```text
Browser에서 동작하는 Frontend가
HTTP를 이용하여 Backend Server에 Request를 보내고,

Backend가 Database 또는 AI Model 등의 작업을 수행한 뒤
Response를 반환하여

사용자에게 필요한 화면과 기능을 제공하는 시스템
```

이라고 이해할 수 있다.