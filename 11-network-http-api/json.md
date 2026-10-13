# JSON

## 1. JSON이란?

JSON은:

```text
JavaScript Object Notation
```

의 약자이다.

Data를 표현하고 교환하기 위해 널리 사용하는 Text 기반 형식이다.

예:

```json
{
    "name": "Notebook",
    "price": 1500000
}
```

---

# 2. JSON은 왜 사용하는가?

Frontend와 Backend는 서로 다른 Programming Language를 사용할 수 있다.

예:

```text
Frontend
→ JavaScript

Backend
→ Python
```

JavaScript Object 자체와 Python dict 자체를 Network로 그대로 전달하는 것이 아니라 공통으로 해석할 수 있는 Data 형식으로 변환해서 전달할 수 있다.

대표적인 형식이:

```text
JSON
```

이다.

---

# 3. 기본 흐름

```text
JavaScript Object
↓
JSON
↓
HTTP Request
↓
Python Backend
↓
Python Data
```

반대 방향도 가능하다.

```text
Python Data
↓
JSON
↓
HTTP Response
↓
JavaScript
↓
JavaScript Object
```

---

# 4. JSON 기본 구조

가장 기본적인 JSON Object:

```json
{
    "name": "Notebook",
    "price": 1500000
}
```

---

# 5. Key와 Value

```json
{
    "name": "Notebook"
}
```

에서:

```text
"name"
→ Key

"Notebook"
→ Value
```

이다.

---

# 6. JSON Object

중괄호:

```text
{
}
```

를 이용하여 Object를 표현한다.

예:

```json
{
    "name": "Kim",
    "age": 30
}
```

---

# 7. 여러 Data

각 항목은:

```text
,
```

로 구분한다.

```json
{
    "name": "Kim",
    "age": 30,
    "city": "Seoul"
}
```

---

# 8. JSON Key

JSON Object의 Key는 큰따옴표를 사용하는 String이다.

올바른 예:

```json
{
    "name": "Kim"
}
```

---

# 9. 다음은 JSON이 아니다

```text
{
    name: "Kim"
}
```

JavaScript Object 문법으로는 사용할 수 있지만 엄밀한 JSON 문법은 아니다.

---

# 10. 작은따옴표

다음 역시 엄밀한 JSON 문법이 아니다.

```text
{
    'name': 'Kim'
}
```

JSON String은:

```text
"
```

큰따옴표를 사용한다.

---

# 11. JSON Data Type

JSON에서 대표적으로 사용할 수 있는 값:

```text
String

Number

Boolean

null

Object

Array
```

이다.

---

# 12. String

```json
{
    "name": "Notebook"
}
```

---

# 13. Number

```json
{
    "price": 1500000
}
```

---

# 14. Boolean

JSON에서는:

```text
true

false
```

를 사용한다.

예:

```json
{
    "is_active": true
}
```

---

# 15. Python과 차이

Python:

```python
True
False
```

JSON:

```json
true
false
```

이다.

대소문자가 다르다.

---

# 16. null

JSON에서는 값이 없음을:

```text
null
```

로 표현할 수 있다.

```json
{
    "nickname": null
}
```

---

# 17. Python과 비교

Python:

```python
None
```

JSON:

```text
null
```

JavaScript:

```javascript
null
```

이다.

---

# 18. Array

JSON Array는:

```text
[
]
```

를 사용한다.

예:

```json
[
    "Python",
    "JavaScript",
    "FastAPI"
]
```

---

# 19. Object 안의 Array

```json
{
    "name": "Kim",
    "skills": [
        "Python",
        "JavaScript",
        "FastAPI"
    ]
}
```

---

# 20. Array 안의 Object

API에서 매우 자주 보는 형태이다.

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

---

# 21. 상품 목록 API

예:

```text
GET /products
```

Response:

```json
[
    {
        "id": 1,
        "name": "Notebook",
        "price": 1500000
    },
    {
        "id": 2,
        "name": "Mouse",
        "price": 50000
    }
]
```

---

# 22. 중첩 Object

JSON Object 안에 다른 Object를 넣을 수 있다.

```json
{
    "user": {
        "id": 10,
        "name": "Kim"
    }
}
```

---

# 23. 복잡한 구조

```json
{
    "user": {
        "id": 10,
        "name": "Kim"
    },
    "products": [
        {
            "id": 1,
            "name": "Notebook"
        },
        {
            "id": 2,
            "name": "Mouse"
        }
    ]
}
```

---

# 24. Python dict

Python에서는:

```python
product = {
    "name": "Notebook",
    "price": 1500000
}
```

처럼 `dict`를 사용할 수 있다.

---

# 25. JSON과 비슷하다

JSON:

```json
{
    "name": "Notebook",
    "price": 1500000
}
```

Python:

```python
{
    "name": "Notebook",
    "price": 1500000
}
```

단순한 경우 매우 비슷하게 보인다.

---

# 26. 하지만 같은 것은 아니다

```text
Python dict
≠
JSON
```

이다.

Python dict는:

```text
Python Object
```

이고 JSON은:

```text
Text 기반 Data Format
```

이다.

---

# 27. 중요한 차이

Python:

```python
{
    "active": True,
    "value": None
}
```

JSON:

```json
{
    "active": true,
    "value": null
}
```

이다.

---

# 28. Python에서 JSON 사용

Python 표준 Library:

```python
import json
```

을 사용할 수 있다.

---

# 29. Python dict → JSON String

```python
import json


data = {
    "name": "Notebook",
    "price": 1500000
}


json_text = json.dumps(data)


print(json_text)
print(type(json_text))
```

---

# 30. dumps

```python
json.dumps()
```

는 Python Object를 JSON 형식의 String으로 직렬화하는 데 사용한다.

```text
Python dict
↓
json.dumps()
↓
JSON String
```

---

# 31. 결과

개념적으로:

```text
{"name": "Notebook", "price": 1500000}
```

이다.

Type은:

```text
str
```

이다.

---

# 32. JSON String → Python

```python
import json


json_text = '''
{
    "name": "Notebook",
    "price": 1500000
}
'''


data = json.loads(json_text)


print(data)
print(type(data))
```

---

# 33. loads

```python
json.loads()
```

는 JSON 형식의 String을 Python Object로 역직렬화하는 데 사용한다.

```text
JSON String
↓
json.loads()
↓
Python dict
```

---

# 34. dumps vs loads

```text
dumps
→ Python → JSON String

loads
→ JSON String → Python
```

---

# 35. 기억 방법

```text
dump
→ 내보낸다.

load
→ 불러온다.
```

정도로 기억할 수 있다.

---

# 36. JSON File 저장

Python에서는:

```python
json.dump()
```

를 이용하여 File에 JSON Data를 기록할 수 있다.

---

# 37. 예

```python
import json


data = {
    "name": "Notebook",
    "price": 1500000
}


with open(
    "product.json",
    "w",
    encoding="utf-8"
) as f:

    json.dump(
        data,
        f,
        ensure_ascii=False,
        indent=4
    )
```

---

# 38. JSON File 읽기

```python
import json


with open(
    "product.json",
    "r",
    encoding="utf-8"
) as f:

    data = json.load(f)


print(data)
```

---

# 39. dump vs dumps

```text
json.dump()
→ File에 작성

json.dumps()
→ JSON String 생성
```

---

# 40. load vs loads

```text
json.load()
→ File에서 읽기

json.loads()
→ JSON String에서 읽기
```

---

# 41. 한 번에 정리

```text
dump
→ Python → JSON File

dumps
→ Python → JSON String

load
→ JSON File → Python

loads
→ JSON String → Python
```

---

# 42. JavaScript Object

JavaScript:

```javascript
const product = {
    name: "Notebook",
    price: 1500000
};
```

이다.

---

# 43. JavaScript Object와 JSON

JavaScript Object:

```javascript
{
    name: "Notebook",
    price: 1500000
}
```

JSON:

```json
{
    "name": "Notebook",
    "price": 1500000
}
```

비슷하지만 같은 것은 아니다.

---

# 44. JavaScript Object → JSON String

```javascript
const product = {
    name: "Notebook",
    price: 1500000
};


const jsonText =
    JSON.stringify(product);


console.log(jsonText);
```

---

# 45. JSON.stringify

```text
JavaScript Object
↓
JSON.stringify()
↓
JSON String
```

이다.

---

# 46. JSON String → JavaScript Object

```javascript
const jsonText =
    '{"name":"Notebook","price":1500000}';


const product =
    JSON.parse(jsonText);


console.log(product);
```

---

# 47. JSON.parse

```text
JSON String
↓
JSON.parse()
↓
JavaScript Object
```

이다.

---

# 48. JavaScript 정리

```text
JSON.stringify()
→ JavaScript Object → JSON String

JSON.parse()
→ JSON String → JavaScript Object
```

---

# 49. Python과 JavaScript 비교

```text
Python

json.dumps()
Python → JSON String

json.loads()
JSON String → Python
```

```text
JavaScript

JSON.stringify()
Object → JSON String

JSON.parse()
JSON String → Object
```

---

# 50. 전체 연결

```text
Python dict
       ↓
  json.dumps()
       ↓
  JSON String
       ↓
     Network
       ↓
  JSON String
       ↓
 JSON.parse()
       ↓
JavaScript Object
```

반대 방향도 가능하다.

---

# 51. HTTP와 JSON

JSON 자체는 HTTP가 아니다.

```text
HTTP
→ 통신 Protocol

JSON
→ Data 표현 형식
```

이다.

---

# 52. 둘을 함께 사용

예:

```http
POST /products HTTP/1.1
Content-Type: application/json

{
    "name": "Notebook",
    "price": 1500000
}
```

---

# 53. HTTP 부분

```text
POST

/products

Content-Type
```

등은 HTTP와 관련된다.

---

# 54. JSON 부분

```json
{
    "name": "Notebook",
    "price": 1500000
}
```

은 Body에 담긴 JSON Data이다.

---

# 55. Content-Type

JSON Body를 보내면 일반적으로:

```http
Content-Type: application/json
```

을 사용한다.

---

# 56. 의미

```text
이 HTTP Message Body는
JSON 형식의 Data이다.
```

라고 알려준다.

---

# 57. Frontend → Backend

JavaScript에서 상품을 생성한다고 하자.

```javascript
const product = {
    name: "Notebook",
    price: 1500000
};
```

---

# 58. fetch

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

            body:
                JSON.stringify(product)
        }
    );
```

---

# 59. 여기서 중요한 부분

```javascript
JSON.stringify(product)
```

이다.

JavaScript Object를 JSON String으로 변환한다.

---

# 60. 왜 그냥 Object를 넣지 않을까?

HTTP Request Body로 전송하기 위한 형식으로 직렬화해야 하기 때문이다.

```text
JavaScript Object
↓
JSON.stringify()
↓
JSON Text
↓
HTTP Request Body
```

---

# 61. Backend

FastAPI 같은 Framework는:

```text
HTTP Request
↓
JSON Body
↓
Parsing / Validation
↓
Python Data
```

과정을 도와준다.

---

# 62. FastAPI 간단 예

```python
from fastapi import FastAPI


app = FastAPI()


@app.post("/products")
def create_product(
    product: dict
):

    return product
```

---

# 63. Request

```json
{
    "name": "Notebook",
    "price": 1500000
}
```

---

# 64. Python에서는

개념적으로:

```python
{
    "name": "Notebook",
    "price": 1500000
}
```

같은 Python Data로 처리할 수 있다.

---

# 65. 실제 FastAPI에서는

Request Body Schema를 명확하게 정의하기 위해 Pydantic Model을 사용하는 경우가 많다.

예:

```python
from fastapi import FastAPI
from pydantic import BaseModel


app = FastAPI()


class Product(BaseModel):

    name: str
    price: int


@app.post("/products")
def create_product(
    product: Product
):

    return product
```

---

# 66. 왜 Model을 만들까?

Backend가 기대하는 Data를 명확하게 정의할 수 있다.

```text
name
→ str

price
→ int
```

---

# 67. 올바른 Request

```json
{
    "name": "Notebook",
    "price": 1500000
}
```

---

# 68. Validation

FastAPI와 Pydantic이 입력 Data가 정의한 Schema에 맞는지 검사할 수 있다.

---

# 69. Response

Python에서:

```python
return {
    "message": "success"
}
```

처럼 반환할 수 있다.

FastAPI가 이를 JSON Response로 직렬화하는 과정을 처리해준다.

---

# 70. 즉

개념적으로:

```text
Python dict
↓
FastAPI
↓
JSON Response
↓
HTTP
↓
Client
```

이다.

---

# 71. JavaScript에서 Response 받기

```javascript
const response =
    await fetch(
        "http://localhost:8000/products"
    );


const data =
    await response.json();
```

---

# 72. response.json()

여기서:

```javascript
response.json()
```

은 JSON Response Body를 읽고 JavaScript에서 사용할 수 있는 값으로 Parsing한다.

---

# 73. 중요한 구분

```javascript
JSON.parse()
```

와:

```javascript
response.json()
```

은 관련은 있지만 같은 API가 아니다.

---

# 74. JSON.parse()

이미 가지고 있는 JSON String을 Parsing한다.

```javascript
const data =
    JSON.parse(jsonText);
```

---

# 75. response.json()

HTTP Response의 Body를 읽어 JSON으로 Parsing한다.

```javascript
const data =
    await response.json();
```

---

# 76. await가 필요한 이유

Response Body를 읽는 과정은 비동기적으로 처리되므로:

```javascript
await response.json()
```

형태를 사용한다.

---

# 77. 전체 Frontend 흐름

```javascript
const response =
    await fetch(
        "http://localhost:8000/products"
    );


if (!response.ok) {

    throw new Error(
        `HTTP Error: ${response.status}`
    );

}


const data =
    await response.json();


console.log(data);
```

---

# 78. 순서

```text
fetch()
↓
HTTP Request

response
↓
HTTP Response

response.ok
↓
성공 여부 확인

response.json()
↓
JSON Body Parsing

data
↓
JavaScript에서 사용
```

---

# 79. AI Prediction JSON

AI Model 결과도 JSON으로 반환할 수 있다.

예:

```json
{
    "prediction": 1,
    "label": "Pneumonia",
    "score": 0.93
}
```

---

# 80. Python Backend

```python
return {
    "prediction": 1,
    "label": "Pneumonia",
    "score": 0.93
}
```

---

# 81. Frontend

```javascript
const data =
    await response.json();


console.log(
    data.label
);


console.log(
    data.score
);
```

---

# 82. DOM 출력

```javascript
result.textContent =
    `${data.label} (${data.score})`;
```

---

# 83. 전체 AI Service

```text
Image
↓
Frontend
↓
POST /predict
↓
FastAPI
↓
PyTorch
↓
Python Prediction Result
↓
JSON
↓
HTTP Response
↓
JavaScript Object
↓
DOM
↓
화면 출력
```

---

# 84. Image 자체는 JSON으로 보내야 할까?

반드시 그렇지 않다.

Image File은:

```text
multipart/form-data
```

로 보내고 결과는 JSON으로 받을 수 있다.

---

# 85. 예

Request:

```text
POST /predict

Content-Type:
multipart/form-data

Body:
Image File
```

Response:

```text
200 OK

Content-Type:
application/json
```

Body:

```json
{
    "label": "Normal",
    "score": 0.98
}
```

---

# 86. Request와 Response 형식은 같을 필요가 없다

Request:

```text
multipart/form-data
```

Response:

```text
application/json
```

이어도 된다.

---

# 87. JSON에서 사용할 수 없는 Python Object

모든 Python Object가 바로 JSON으로 표현될 수 있는 것은 아니다.

예:

```text
NumPy ndarray

PyTorch Tensor

datetime

Custom Class
```

등은 별도의 변환이 필요할 수 있다.

---

# 88. PyTorch Tensor 예

다음 값을 그대로 Response에 넣기보다:

```python
tensor
```

Python 기본 Type으로 변환하는 것이 필요할 수 있다.

---

# 89. Tensor Scalar

예:

```python
score = tensor.item()
```

---

# 90. Tensor → List

예:

```python
values = tensor.tolist()
```

---

# 91. NumPy

NumPy Scalar도 필요한 경우:

```python
value = np_value.item()
```

등으로 Python 기본 Type으로 변환할 수 있다.

---

# 92. 왜 변환할까?

JSON은 제한된 Data Type만 표현하기 때문이다.

```text
String

Number

Boolean

null

Object

Array
```

---

# 93. AI API에서 흔한 실수

```python
return {
    "score": output_tensor
}
```

처럼 Tensor 자체를 반환하려고 하면 Serialization 문제가 발생할 수 있다.

---

# 94. 대신

```python
return {
    "score":
        output_tensor.item()
}
```

처럼 JSON으로 표현 가능한 Python Type으로 변환한다.

실제 Tensor Shape에 따라 변환 방법은 달라진다.

---

# 95. Prediction Class

PyTorch에서:

```python
pred = output.argmax(
    dim=1
)
```

결과가 Tensor라면:

```python
pred.item()
```

등을 통해 Python Integer로 변환할 수 있다.

---

# 96. Probability

```python
probability = 0.93
```

처럼 Python `float`로 만들면 JSON Number로 표현할 수 있다.

---

# 97. JSON Response 설계

AI API Response를:

```json
{
    "prediction": 1,
    "label": "Pneumonia",
    "probability": 0.93
}
```

처럼 만들 수 있다.

---

# 98. 이름을 명확하게

다음보다:

```json
{
    "a": 1,
    "b": 0.93
}
```

다음이 이해하기 쉽다.

```json
{
    "prediction": 1,
    "probability": 0.93
}
```

---

# 99. 중첩 Response

```json
{
    "result": {
        "prediction": 1,
        "label": "Pneumonia",
        "probability": 0.93
    },
    "model": {
        "name": "ChestXrayClassifier"
    }
}
```

같이 구성할 수도 있다.

---

# 100. List Response

여러 Prediction을 반환한다면:

```json
{
    "predictions": [
        {
            "label": "Normal",
            "score": 0.10
        },
        {
            "label": "Pneumonia",
            "score": 0.90
        }
    ]
}
```

같은 구조도 가능하다.

---

# 101. JSON은 순수 Data

JSON 안에:

```text
Function

Python Code

JavaScript Function
```

같은 실행 Logic을 직접 표현하는 형식이 아니다.

Data를 표현하기 위한 형식이다.

---

# 102. JSON Comment

표준 JSON에는 일반적인 Comment 문법이 없다.

다음과 같이 작성하면 표준 JSON이 아니다.

```text
{
    // 사용자 이름
    "name": "Kim"
}
```

---

# 103. Trailing Comma

다음 역시 표준 JSON에서는 잘못된 형식이다.

```text
{
    "name": "Kim",
    "age": 30,
}
```

마지막 항목 뒤의:

```text
,
```

를 제거해야 한다.

---

# 104. 올바른 JSON

```json
{
    "name": "Kim",
    "age": 30
}
```

---

# 105. 흔한 JSON Error

```text
Key 큰따옴표 누락

String 작은따옴표 사용

마지막 Comma

true / false 대소문자 오류

null 표현 오류

괄호 불일치
```

등이 있다.

---

# 106. Python 문법과 섞지 않는다

잘못된 JSON:

```text
{
    "active": True,
    "value": None
}
```

---

# 107. 올바른 JSON

```json
{
    "active": true,
    "value": null
}
```

---

# 108. JavaScript Object와도 섞지 않는다

JavaScript에서는:

```javascript
const user = {
    name: "Kim"
};
```

가 가능하다.

하지만 JSON은:

```json
{
    "name": "Kim"
}
```

이다.

---

# 109. JSON과 API

Web API에서는 JSON을 Request/Response Data 형식으로 매우 자주 사용한다.

예:

```text
Client
↓
HTTP + JSON
↓
API
```

---

# 110. REST API 예

```text
GET /products
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

# 111. POST

```text
POST /products
```

Request:

```json
{
    "name": "Notebook",
    "price": 1500000
}
```

---

# 112. Response

```json
{
    "id": 10,
    "name": "Notebook",
    "price": 1500000
}
```

---

# 113. JSON과 Database

Database Data도 Backend에서 Python Object로 변환된 뒤 JSON Response로 전달될 수 있다.

```text
Database
↓
SQLAlchemy
↓
Python
↓
FastAPI
↓
JSON
↓
Frontend
```

---

# 114. JSON과 AI

```text
PyTorch
↓
Prediction
↓
Python Number / String
↓
JSON
↓
Frontend
```

---

# 115. JSON과 HTTP 구분

다시 기억한다.

```text
HTTP
→ 어떻게 주고받을 것인가?

JSON
→ 어떤 형식의 Data를 주고받을 것인가?
```

---

# 116. URL과 JSON 구분

```text
URL
→ 어디로 보낼 것인가?

JSON
→ 무엇을 보낼 것인가?
```

---

# 117. Method와 JSON

```text
Method
→ 무엇을 하려는가?

JSON Body
→ 작업에 필요한 Data
```

---

# 118. 전체 Request

```http
POST /products HTTP/1.1
Host: localhost:8000
Content-Type: application/json
Accept: application/json

{
    "name": "Notebook",
    "price": 1500000
}
```

---

# 119. 읽는 방법

```text
POST
→ 생성/처리 요청

/products
→ 상품 Resource

Content-Type
→ 보내는 Body는 JSON

Accept
→ JSON Response를 받을 수 있음

Body
→ 실제 상품 Data
```

---

# 120. Response

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

# 121. 읽는 방법

```text
201
→ Resource 생성 성공

Content-Type
→ Response Body는 JSON

Body
→ 생성된 상품 Data
```

---

# 122. Browser Network에서 JSON 확인

Chrome DevTools:

```text
F12
↓
Network
↓
Request 선택
```

---

# 123. Payload

Request JSON Data를 확인할 수 있다.

---

# 124. Response

Server가 반환한 JSON Data를 확인할 수 있다.

---

# 125. Headers

다음도 같이 확인한다.

```text
Content-Type
```

예:

```text
application/json
```

---

# 126. API Debugging

JSON API가 실패하면:

```text
Method가 맞는가?

URL이 맞는가?

Content-Type이 맞는가?

JSON 문법이 맞는가?

Key 이름이 맞는가?

Data Type이 맞는가?

필수 값이 있는가?
```

를 확인한다.

---

# 127. FastAPI 422와 연결

Backend가:

```text
name: str
price: int
```

를 요구한다고 하자.

Client가 필요한 Field를 누락하거나 적절하지 않은 값을 보내면 Validation Error가 발생할 수 있다.

---

# 128. 따라서 422가 나오면

```text
Request JSON

Field Name

Data Type

필수 Field

Request Body 구조
```

를 확인한다.

---

# 129. 예

Backend 기대:

```json
{
    "name": "Notebook",
    "price": 1500000
}
```

Client 전송:

```json
{
    "product_name": "Notebook"
}
```

이면 기대한 Schema와 다르다.

---

# 130. API Contract

Frontend와 Backend는:

```text
어떤 URL

어떤 Method

어떤 Request 형식

어떤 Response 형식
```

을 서로 약속해야 한다.

이를 API Contract의 일부라고 생각할 수 있다.

---

# 131. 예

```text
POST /predict

Request:
Image File

Response:
{
    "label": String,
    "score": Number
}
```

---

# 132. Frontend는 이 약속에 맞춰 개발

```javascript
console.log(
    data.label
);

console.log(
    data.score
);
```

---

# 133. Backend가 이름을 바꾸면

Backend:

```json
{
    "prediction_label": "Normal"
}
```

Frontend:

```javascript
data.label
```

이라면 값이 맞지 않는다.

---

# 134. 따라서

Frontend와 Backend의 JSON 구조를 맞추는 것이 중요하다.

---

# 135. 핵심 변환 관계

```text
Python

dict
↓
JSON
↓
HTTP
↓
JavaScript Object
```

---

# 136. 반대

```text
JavaScript Object
↓
JSON.stringify()
↓
HTTP
↓
JSON Parsing
↓
Python Data
```

---

# 137. 핵심 용어

```text
JSON
→ Text 기반 Data 형식

Object
→ { }

Array
→ [ ]

String
→ "text"

Boolean
→ true / false

Null
→ null
```

---

# 138. Python 변환

```text
json.dumps()
→ Python → JSON String

json.loads()
→ JSON String → Python

json.dump()
→ Python → JSON File

json.load()
→ JSON File → Python
```

---

# 139. JavaScript 변환

```text
JSON.stringify()
→ JavaScript Object → JSON String

JSON.parse()
→ JSON String → JavaScript Object

response.json()
→ HTTP JSON Response Body를 Parsing
```

---

# 140. 가장 중요한 구분

```text
Python dict
≠ JSON

JavaScript Object
≠ JSON

HTTP
≠ JSON
```

하지만 서로 변환하여 함께 사용할 수 있다.

---

# 141. 전체 AI Service

```text
Frontend
JavaScript

↓ Image Upload

HTTP POST /predict

↓

FastAPI
Python

↓

PyTorch Model

↓

Python Prediction

↓

JSON Response

{
    "label": "Pneumonia",
    "score": 0.93
}

↓

HTTP

↓

JavaScript

↓

response.json()

↓

JavaScript Object

↓

DOM

↓

사용자 화면
```

---

# 142. 한 문장으로 정리

JSON은:

```text
Frontend와 Backend 등 서로 다른 System과 Programming Language 사이에서
구조화된 Data를 표현하고 교환하기 위해 널리 사용하는
Text 기반 Data Format
```

이라고 이해할 수 있다.

---

# 143. 다음 단계

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
```

을 연결했다.

다음에는 이것들을 실제 Web Service의 기능으로 묶어주는:

```text
API
REST API
```

를 공부한다.