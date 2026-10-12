# JavaScript Basic

## 1. JavaScript란?

JavaScript는 Web Page에 동작을 추가할 때 사용하는 Programming Language이다.

HTML이:

```text
구조
```

CSS가:

```text
디자인
```

이라면 JavaScript는:

```text
동작
```

을 담당한다.

---

# 2. JavaScript로 할 수 있는 것

예:

```text
Button Click 처리

Mobile Menu 열기 / 닫기

Image Slider

Popup

Form Validation

계산

화면 내용 변경

Server에 API Request

JSON Data 처리
```

등이 있다.

---

# 3. HTML / CSS / JavaScript

```text
HTML
→ 무엇이 있는가?

CSS
→ 어떻게 보이는가?

JavaScript
→ 어떻게 동작하는가?
```

이다.

---

# 4. JavaScript File

JavaScript File의 확장자는:

```text
.js
```

이다.

예:

```text
script.js
```

---

# 5. 기본 Web Project

```text
my-web/
├── index.html
├── style.css
└── script.js
```

---

# 6. JavaScript 연결

HTML에서:

```html
<script
    src="script.js"
    defer
></script>
```

처럼 JavaScript File을 연결할 수 있다.

---

# 7. `defer`

```html
<script
    src="script.js"
    defer
></script>
```

의 `defer`는 HTML 문서를 Parsing하는 동안 Script 다운로드를 진행하고, 문서 Parsing이 끝난 뒤 Script를 실행하도록 한다.

초보 단계에서는 HTML Element를 JavaScript가 찾지 못하는 문제를 줄이는 데도 도움이 된다.

---

# 8. 첫 JavaScript

```javascript
console.log("Hello JavaScript");
```

---

# 9. `console.log()`

Python의:

```python
print("Hello Python")
```

과 비슷한 역할이다.

JavaScript:

```javascript
console.log("Hello JavaScript");
```

---

# 10. 결과는 어디에서 볼까?

Chrome에서:

```text
F12
↓
Console
```

을 열어 확인할 수 있다.

---

# 11. JavaScript 주석

한 줄 주석:

```javascript
// 한 줄 주석
```

여러 줄:

```javascript
/*
여러 줄
주석
*/
```

---

# 12. 변수

Data를 저장할 이름을 변수라고 한다.

Python:

```python
name = "Juhyeong"
```

JavaScript:

```javascript
let name = "Juhyeong";
```

---

# 13. `let`

값을 나중에 변경할 수 있는 변수를 선언할 때 사용할 수 있다.

```javascript
let age = 30;

age = 31;
```

---

# 14. `const`

다시 대입하지 않을 변수를 선언할 때 사용한다.

```javascript
const name = "Juhyeong";
```

---

# 15. const 재대입

다음은 오류가 발생한다.

```javascript
const name = "Juhyeong";

name = "Kim";
```

---

# 16. 기본적으로 const

값을 다시 대입할 필요가 없다면:

```text
const
```

를 우선 사용하고,

재대입이 필요하면:

```text
let
```

을 사용하는 방식이 흔하다.

---

# 17. `var`

과거 JavaScript Code에서:

```javascript
var name = "Juhyeong";
```

도 많이 볼 수 있다.

하지만 현대 JavaScript에서는 일반적으로:

```text
const

let
```

을 우선 사용한다.

---

# 18. Python과 비교

Python:

```python
name = "Kim"

age = 30
```

JavaScript:

```javascript
const name = "Kim";

let age = 30;
```

---

# 19. Semicolon

JavaScript:

```javascript
const age = 30;
```

처럼 문장 끝에:

```text
;
```

을 사용할 수 있다.

JavaScript에서는 자동 세미콜론 삽입 기능도 있지만, 프로젝트의 Code Style에 맞춰 일관되게 작성하는 것이 중요하다.

---

# 20. String

문자열:

```javascript
const name = "Kim";

const city = 'Seoul';
```

---

# 21. Number

```javascript
const age = 30;

const price = 15000;

const probability = 0.93;
```

JavaScript에서는 일반적인 정수와 실수를 모두 `number` Type으로 다룬다.

---

# 22. Boolean

```javascript
const isLogin = true;

const isAdmin = false;
```

---

# 23. null

의도적으로 값이 없음을 나타낼 때 사용할 수 있다.

```javascript
const result = null;
```

---

# 24. undefined

값이 아직 지정되지 않은 상태 등에서 볼 수 있다.

```javascript
let result;

console.log(result);
```

결과:

```text
undefined
```

---

# 25. typeof

Data Type을 확인할 수 있다.

```javascript
console.log(
    typeof "hello"
);

console.log(
    typeof 10
);

console.log(
    typeof true
);
```

---

# 26. 결과

```text
string

number

boolean
```

이다.

---

# 27. Python과 Type 비교

```text
Python        JavaScript

str           string

int/float     number

bool          boolean

None          null
```

완전히 같은 개념은 아니지만 처음에는 이렇게 비교하면 이해하기 쉽다.

---

# 28. 문자열 연결

```javascript
const name = "Kim";

console.log(
    "Hello " + name
);
```

---

# 29. Template Literal

JavaScript에서는 Backtick:

```text
`
```

을 이용한 Template Literal을 자주 사용한다.

```javascript
const name = "Kim";

const age = 30;

console.log(
    `이름: ${name}, 나이: ${age}`
);
```

---

# 30. Python f-string과 비교

Python:

```python
print(
    f"이름: {name}, 나이: {age}"
)
```

JavaScript:

```javascript
console.log(
    `이름: ${name}, 나이: ${age}`
);
```

---

# 31. 산술 연산

```javascript
const a = 10;

const b = 3;

console.log(a + b);

console.log(a - b);

console.log(a * b);

console.log(a / b);

console.log(a % b);
```

---

# 32. 비교 연산

```javascript
console.log(
    10 > 5
);

console.log(
    10 < 5
);

console.log(
    10 >= 10
);
```

---

# 33. `===`

JavaScript에서는 값과 Type까지 비교할 때:

```javascript
===
```

을 많이 사용한다.

```javascript
console.log(
    10 === 10
);
```

결과:

```text
true
```

---

# 34. `==`와 `===`

JavaScript에는:

```text
==

===
```

둘 다 존재한다.

---

# 35. `==`

Type 변환을 허용하면서 비교할 수 있다.

```javascript
console.log(
    10 == "10"
);
```

결과:

```text
true
```

가 될 수 있다.

---

# 36. `===`

Type까지 비교한다.

```javascript
console.log(
    10 === "10"
);
```

결과:

```text
false
```

이다.

---

# 37. 초보 단계 권장

예상하지 못한 Type 변환을 줄이기 위해 보통:

```text
===

!==
```

을 사용하는 습관을 들이는 것이 좋다.

---

# 38. 논리 연산

AND:

```javascript
&&
```

OR:

```javascript
||
```

NOT:

```javascript
!
```

---

# 39. 예

```javascript
const age = 25;

const hasTicket = true;


if (
    age >= 20 &&
    hasTicket
) {

    console.log(
        "입장 가능"
    );

}
```

---

# 40. 조건문

Python:

```python
if age >= 20:
    print("성인")
```

JavaScript:

```javascript
if (age >= 20) {

    console.log(
        "성인"
    );

}
```

---

# 41. if / else

```javascript
const age = 18;


if (age >= 20) {

    console.log(
        "성인"
    );

} else {

    console.log(
        "미성년"
    );

}
```

---

# 42. else if

Python의:

```text
elif
```

과 비슷하다.

```javascript
const score = 85;


if (score >= 90) {

    console.log("A");

} else if (score >= 80) {

    console.log("B");

} else {

    console.log("C");

}
```

---

# 43. 중괄호

JavaScript에서는:

```text
{
}
```

를 이용하여 Code Block을 구분한다.

Python의 들여쓰기와 차이가 있다.

---

# 44. 반복문

JavaScript에서도 반복문을 사용할 수 있다.

---

# 45. 기본 for

```javascript
for (
    let i = 0;
    i < 5;
    i++
) {

    console.log(i);

}
```

결과:

```text
0
1
2
3
4
```

---

# 46. 구조

```text
초기값

조건

증가
```

이다.

```javascript
for (
    let i = 0;   // 초기값
    i < 5;       // 조건
    i++          // 증가
)
```

---

# 47. Python과 비교

Python:

```python
for i in range(5):
    print(i)
```

JavaScript:

```javascript
for (
    let i = 0;
    i < 5;
    i++
) {

    console.log(i);

}
```

---

# 48. while

```javascript
let count = 0;


while (count < 3) {

    console.log(count);

    count++;

}
```

---

# 49. Array

Python의 List와 비슷한 Data Structure이다.

```javascript
const fruits = [
    "apple",
    "banana",
    "orange"
];
```

---

# 50. Array Index

```javascript
console.log(
    fruits[0]
);
```

결과:

```text
apple
```

---

# 51. Index는 0부터

```text
fruits[0]
→ apple

fruits[1]
→ banana

fruits[2]
→ orange
```

이다.

---

# 52. Array 길이

```javascript
console.log(
    fruits.length
);
```

---

# 53. Array에 추가

```javascript
fruits.push(
    "grape"
);
```

---

# 54. 마지막 값 제거

```javascript
fruits.pop();
```

---

# 55. Array 반복

```javascript
for (
    const fruit of fruits
) {

    console.log(
        fruit
    );

}
```

---

# 56. Python과 비교

Python:

```python
for fruit in fruits:
    print(fruit)
```

JavaScript:

```javascript
for (
    const fruit of fruits
) {

    console.log(fruit);

}
```

---

# 57. `forEach`

Array에서는:

```javascript
fruits.forEach(
    function (fruit) {

        console.log(
            fruit
        );

    }
);
```

처럼 사용할 수도 있다.

---

# 58. Arrow Function

위 Code를:

```javascript
fruits.forEach(
    (fruit) => {

        console.log(
            fruit
        );

    }
);
```

처럼 작성할 수 있다.

---

# 59. Object

JavaScript의 Object는 Python Dictionary와 비슷하게 Key와 Value를 저장할 수 있다.

```javascript
const user = {

    name: "Kim",

    age: 30,

    city: "Seoul"

};
```

---

# 60. 값 가져오기

```javascript
console.log(
    user.name
);
```

또는:

```javascript
console.log(
    user["name"]
);
```

---

# 61. 값 변경

```javascript
user.age = 31;
```

---

# 62. const인데 왜 변경 가능?

다음은 가능하다.

```javascript
const user = {
    name: "Kim"
};


user.name = "Lee";
```

---

# 63. 이유

`const`는 변수 자체에 다른 값을 다시 대입하는 것을 막는다.

즉 다음은 안 된다.

```javascript
user = {
    name: "Park"
};
```

하지만 Object 내부 Property 변경까지 모두 막는 것은 아니다.

---

# 64. Array도 동일

```javascript
const numbers = [
    1,
    2
];


numbers.push(3);
```

은 가능하다.

---

# 65. Object와 JSON

JavaScript Object:

```javascript
const product = {

    name: "apple",

    price: 1000

};
```

JSON:

```json
{
    "name": "apple",
    "price": 1000
}
```

모양이 매우 비슷하다.

---

# 66. 하지만 Object와 JSON은 같은 것은 아니다

JavaScript Object는 JavaScript 안에서 사용하는 Data Structure이고,

JSON은 Data를 표현하고 교환하기 위한 Text Format이다.

---

# 67. JSON 문자열 → Object

```javascript
const text =
    '{"name":"apple","price":1000}';


const product =
    JSON.parse(text);


console.log(
    product.name
);
```

---

# 68. Object → JSON 문자열

```javascript
const product = {

    name: "apple",

    price: 1000

};


const text =
    JSON.stringify(product);
```

---

# 69. API와 연결

나중에 Backend가:

```json
{
    "prediction": 1,
    "probability": 0.93
}
```

같은 JSON을 반환하면 JavaScript가 이를 받아 화면에 표시할 수 있다.

---

# 70. Function

반복해서 사용할 Code를 하나의 기능으로 묶을 수 있다.

```javascript
function hello() {

    console.log(
        "Hello"
    );

}
```

---

# 71. Function 호출

```javascript
hello();
```

---

# 72. Parameter

```javascript
function hello(name) {

    console.log(
        `Hello ${name}`
    );

}


hello(
    "Kim"
);
```

---

# 73. return

```javascript
function add(
    a,
    b
) {

    return a + b;

}


const result = add(
    10,
    20
);


console.log(
    result
);
```

---

# 74. Python과 비교

Python:

```python
def add(a, b):
    return a + b
```

JavaScript:

```javascript
function add(
    a,
    b
) {

    return a + b;

}
```

---

# 75. Function Expression

Function을 변수에 저장할 수도 있다.

```javascript
const add = function (
    a,
    b
) {

    return a + b;

};
```

---

# 76. Arrow Function

```javascript
const add = (
    a,
    b
) => {

    return a + b;

};
```

---

# 77. 짧게 작성

```javascript
const add =
    (a, b) => a + b;
```

---

# 78. Arrow Function을 자주 보는 곳

```text
Array 처리

Event Handler

API 처리

React
```

등에서 매우 자주 볼 수 있다.

---

# 79. Scope

변수가 어디에서 사용할 수 있는지를:

```text
Scope
```

라고 한다.

---

# 80. Block Scope

```javascript
if (true) {

    const message =
        "Hello";

    console.log(
        message
    );

}
```

`message`는 이 Block 밖에서는 사용할 수 없다.

---

# 81. 문자열을 숫자로 변환

사용자 Input은 문자열로 들어오는 경우가 많다.

```javascript
const value = "100";
```

숫자로 변환:

```javascript
const number =
    Number(value);
```

---

# 82. Number

```javascript
console.log(
    Number("100")
);
```

결과:

```text
100
```

---

# 83. parseInt

정수 변환:

```javascript
const number =
    parseInt(
        "100",
        10
    );
```

---

# 84. NaN

숫자로 변환할 수 없는 값을 변환하면:

```text
NaN
```

을 볼 수 있다.

```javascript
console.log(
    Number("hello")
);
```

---

# 85. NaN 확인

```javascript
const value =
    Number("hello");


console.log(
    Number.isNaN(value)
);
```

---

# 86. Truthy / Falsy

JavaScript에서는 Boolean이 아니어도 조건문에서 참 또는 거짓처럼 평가되는 값이 있다.

Falsy의 대표적인 예:

```text
false

0

""

null

undefined

NaN
```

---

# 87. 예

```javascript
const username = "";


if (username) {

    console.log(
        "이름 있음"
    );

} else {

    console.log(
        "이름 없음"
    );

}
```

---

# 88. Ternary Operator

간단한 조건은:

```javascript
const result =
    age >= 20
        ? "성인"
        : "미성년";
```

처럼 표현할 수도 있다.

---

# 89. Python과 비교

Python:

```python
result = "성인" if age >= 20 else "미성년"
```

JavaScript:

```javascript
const result =
    age >= 20
        ? "성인"
        : "미성년";
```

---

# 90. Array of Objects

Web API에서 매우 자주 보는 구조이다.

```javascript
const products = [

    {
        id: 1,
        name: "Notebook",
        price: 1500000
    },

    {
        id: 2,
        name: "Mouse",
        price: 30000
    }

];
```

---

# 91. 첫 번째 상품

```javascript
console.log(
    products[0].name
);
```

---

# 92. 반복

```javascript
for (
    const product of products
) {

    console.log(
        product.name
    );

}
```

---

# 93. `map()`

Array의 각 값을 이용해 새로운 Array를 만들 수 있다.

```javascript
const prices =
    products.map(
        (product) => {

            return product.price;

        }
    );
```

---

# 94. 짧게

```javascript
const prices =
    products.map(
        product =>
            product.price
    );
```

---

# 95. `filter()`

조건에 맞는 값만 선택한다.

```javascript
const expensive =
    products.filter(
        product =>
            product.price >= 100000
    );
```

---

# 96. Pandas와 연결

Pandas:

```python
df[
    df["price"] >= 100000
]
```

JavaScript Array:

```javascript
products.filter(
    product =>
        product.price >= 100000
);
```

목적은 비슷하지만 사용하는 Data Structure와 문법이 다르다.

---

# 97. `find()`

조건에 맞는 첫 번째 값을 찾는다.

```javascript
const product =
    products.find(
        product =>
            product.id === 2
    );
```

---

# 98. Destructuring

Object에서 필요한 값을 꺼낼 수 있다.

```javascript
const user = {

    name: "Kim",

    age: 30

};


const {
    name,
    age
} = user;
```

---

# 99. Array Destructuring

```javascript
const numbers = [
    10,
    20
];


const [
    first,
    second
] = numbers;
```

---

# 100. Spread Syntax

```javascript
const a = [
    1,
    2
];


const b = [
    ...a,
    3
];
```

결과:

```text
[1, 2, 3]
```

---

# 101. Object Spread

```javascript
const user = {

    name: "Kim",

    age: 30

};


const updatedUser = {

    ...user,

    age: 31

};
```

---

# 102. Error

JavaScript 실행 중 문제가 발생하면 Browser Console에서 Error를 확인한다.

예:

```text
ReferenceError

TypeError

SyntaxError
```

---

# 103. ReferenceError

정의하지 않은 변수를 사용했을 때 볼 수 있다.

```javascript
console.log(
    username
);
```

`username`이 선언되지 않았다면 Error가 발생한다.

Python의 `NameError`와 비슷하게 생각할 수 있다.

---

# 104. TypeError

Data Type에 맞지 않는 동작을 시도할 때 발생할 수 있다.

예:

```javascript
const user = null;

console.log(
    user.name
);
```

---

# 105. SyntaxError

JavaScript 문법이 잘못된 경우 발생한다.

예:

```javascript
if (
    age > 20
```

처럼 괄호나 문법이 완성되지 않은 경우이다.

---

# 106. Debugging

가장 기본적인 방법:

```javascript
console.log(
    variable
);
```

---

# 107. 여러 값 확인

```javascript
console.log(
    "name:",
    name
);

console.log(
    "age:",
    age
);
```

---

# 108. console.table

Array of Objects를 확인할 때 유용하다.

```javascript
console.table(
    products
);
```

---

# 109. Browser Developer Tools

```text
F12
↓
Console
```

에서 JavaScript Error를 확인한다.

---

# 110. HTML과 연결

HTML:

```html
<h1 id="title">
    Hello
</h1>
```

JavaScript:

```javascript
const title =
    document.getElementById(
        "title"
    );


console.log(
    title
);
```

---

# 111. `document`

현재 Web Page의 HTML Document를 JavaScript에서 다룰 수 있도록 제공되는 Object이다.

---

# 112. Element 선택

```javascript
document.getElementById(
    "title"
);
```

을 이용하면:

```html
<h1 id="title">
```

Element를 찾을 수 있다.

---

# 113. Text 변경

```javascript
const title =
    document.getElementById(
        "title"
    );


title.textContent =
    "JavaScript로 변경";
```

---

# 114. 결과

HTML 원본:

```html
<h1 id="title">
    Hello
</h1>
```

화면:

```text
JavaScript로 변경
```

으로 바뀐다.

---

# 115. 중요한 흐름

```text
HTML
↓
Browser가 DOM 생성
↓
JavaScript가 Element 선택
↓
JavaScript가 DOM 변경
↓
화면 변경
```

이다.

---

# 116. Button

HTML:

```html
<button id="button">
    클릭
</button>
```

JavaScript:

```javascript
const button =
    document.getElementById(
        "button"
    );
```

---

# 117. Click Event

```javascript
button.addEventListener(
    "click",
    function () {

        console.log(
            "버튼 클릭"
        );

    }
);
```

---

# 118. Arrow Function

```javascript
button.addEventListener(
    "click",
    () => {

        console.log(
            "버튼 클릭"
        );

    }
);
```

---

# 119. 화면 변경

HTML:

```html
<h1 id="title">
    Hello
</h1>

<button id="button">
    변경
</button>
```

JavaScript:

```javascript
const title =
    document.getElementById(
        "title"
    );


const button =
    document.getElementById(
        "button"
    );


button.addEventListener(
    "click",
    () => {

        title.textContent =
            "버튼을 클릭했습니다.";

    }
);
```

---

# 120. 이것이 JavaScript의 핵심

```text
Element 찾기

↓

Event 감지

↓

Data 처리

↓

Element 변경
```

이다.

---

# 121. CSS Class와 연결

HTML:

```html
<div
    id="menu"
    class="menu"
>
    Menu
</div>
```

CSS:

```css
.menu {
    display: none;
}


.menu.active {
    display: block;
}
```

JavaScript:

```javascript
const menu =
    document.getElementById(
        "menu"
    );


menu.classList.add(
    "active"
);
```

---

# 122. Class 제거

```javascript
menu.classList.remove(
    "active"
);
```

---

# 123. Toggle

```javascript
menu.classList.toggle(
    "active"
);
```

있으면 제거하고 없으면 추가한다.

---

# 124. Mobile Menu

HTML:

```html
<button id="menu-button">
    MENU
</button>

<nav
    id="menu"
    class="menu"
>
    ...
</nav>
```

JavaScript:

```javascript
const menuButton =
    document.getElementById(
        "menu-button"
    );


const menu =
    document.getElementById(
        "menu"
    );


menuButton.addEventListener(
    "click",
    () => {

        menu.classList.toggle(
            "active"
        );

    }
);
```

---

# 125. 지금까지의 연결

```text
HTML
→ Menu 구조

CSS
→ Menu Style

JavaScript
→ Menu 열기 / 닫기
```

이다.

---

# 126. 사용자 Input

HTML:

```html
<input
    id="username"
    type="text"
>

<button id="submit-button">
    확인
</button>
```

JavaScript:

```javascript
const input =
    document.getElementById(
        "username"
    );


const button =
    document.getElementById(
        "submit-button"
    );


button.addEventListener(
    "click",
    () => {

        console.log(
            input.value
        );

    }
);
```

---

# 127. `.value`

Input에 사용자가 입력한 값을 가져올 수 있다.

---

# 128. Form Validation

```javascript
if (
    input.value === ""
) {

    alert(
        "이름을 입력해주세요."
    );

}
```

---

# 129. `alert()`

Browser에 간단한 Alert 창을 표시할 수 있다.

```javascript
alert(
    "문의가 접수되었습니다."
);
```

---

# 130. API와 JavaScript

앞으로 JavaScript는 Backend API에 Request를 보내게 된다.

예:

```text
Frontend JavaScript
↓
POST /predict
↓
FastAPI
↓
AI Model
↓
JSON Response
↓
JavaScript
↓
화면 출력
```

---

# 131. `fetch()`

Browser JavaScript에서는 `fetch()`를 이용하여 HTTP Request를 보낼 수 있다.

예:

```javascript
fetch(
    "http://localhost:8000/products"
);
```

---

# 132. 아직 중요한 것은

지금 단계에서는 `fetch()` 문법을 외우기보다:

```text
JavaScript가 Server에
HTTP Request를 보낼 수 있다.
```

는 구조를 이해한다.

---

# 133. Promise

API Request는 즉시 결과가 나오는 것이 아니라 시간이 걸릴 수 있다.

JavaScript에는 이러한 비동기 작업을 처리하기 위한:

```text
Promise

async

await
```

개념이 있다.

---

# 134. async / await 예

```javascript
async function getProducts() {

    const response =
        await fetch(
            "/products"
        );


    const data =
        await response.json();


    console.log(
        data
    );

}
```

---

# 135. 흐름

```text
fetch()
↓
Server Request
↓
Response 대기
↓
response
↓
JSON 변환
↓
data
```

이다.

---

# 136. POST 개념

```javascript
async function sendData() {

    const response =
        await fetch(
            "/predict",
            {
                method: "POST",

                headers: {
                    "Content-Type":
                        "application/json"
                },

                body: JSON.stringify(
                    {
                        age: 30,
                        bmi: 24.5
                    }
                )
            }
        );


    const result =
        await response.json();


    console.log(
        result
    );

}
```

---

# 137. 지금까지 공부한 HTTP와 연결

```javascript
method: "POST"
```

는 HTTP Method이다.

```javascript
"/predict"
```

는 URL Path이다.

```javascript
headers
```

는 HTTP Header이다.

```javascript
body
```

는 Request Body이다.

---

# 138. Backend Response

FastAPI가:

```json
{
    "prediction": 1,
    "probability": 0.93
}
```

을 반환했다고 하자.

JavaScript에서는:

```javascript
console.log(
    result.prediction
);

console.log(
    result.probability
);
```

처럼 사용할 수 있다.

---

# 139. AI Web Service

최종적으로:

```text
HTML
↓
사용자 입력

JavaScript
↓
Input Data 수집

fetch()
↓
HTTP Request

FastAPI
↓
AI Model

JSON
↓
Prediction

JavaScript
↓
DOM 변경

HTML
↓
결과 화면
```

으로 연결된다.

---

# 140. Python과 JavaScript 비교

```text
Python                 JavaScript

print()                console.log()

None                   null

list                   Array

dict                   Object

def                    function

elif                   else if

and                    &&

or                     ||

not                    !

len(data)              data.length

f"{name}"              `${name}`
```

완전히 동일한 언어는 아니지만 기존 Python 지식을 이용하면 이해하기 쉽다.

---

# 141. 중요한 차이

Python은 지금까지 주로:

```text
Data Analysis

Machine Learning

Deep Learning

Backend
```

에서 사용했다.

JavaScript는 이번 과정에서 주로:

```text
Browser

Frontend

DOM

Event

API Request
```

에서 사용한다.

---

# 142. JavaScript를 공부하는 목적

현재 목표에서는 JavaScript의 모든 문법을 외우는 것이 우선이 아니다.

먼저 다음을 할 수 있으면 된다.

```text
변수 만들기

조건문 사용

반복문 사용

Array / Object 다루기

Function 만들기

HTML Element 찾기

Click Event 처리

Class 변경

Input 값 가져오기

API Request 보내기

Response를 화면에 표시하기
```

---

# 143. JavaScript Debugging 순서

JavaScript가 동작하지 않으면:

```text
1. script.js가 HTML에 연결됐는가?

2. File을 저장했는가?

3. Console Error가 있는가?

4. Element ID / Class가 맞는가?

5. JavaScript가 Element보다 적절한 시점에 실행되는가?

6. 변수 값이 무엇인가?

7. Event가 실제 발생하는가?

8. Network Request가 성공하는가?
```

를 확인한다.

---

# 144. `console.log()`를 적극적으로 사용

예:

```javascript
console.log(
    "script 시작"
);


console.log(
    "button:",
    button
);


console.log(
    "input:",
    input.value
);
```

이렇게 한 단계씩 확인한다.

---

# 145. JavaScript의 핵심 흐름

Frontend에서 JavaScript는 크게:

```text
Data
↓
Logic
↓
Event
↓
DOM
↓
Network
```

을 다룬다.

---

# 146. Data

```text
String

Number

Boolean

Array

Object
```

등을 다룬다.

---

# 147. Logic

```text
if

for

function
```

등을 이용한다.

---

# 148. Event

```text
click

input

submit
```

등 사용자 행동을 감지한다.

---

# 149. DOM

HTML Element를:

```text
찾고

읽고

변경하고

추가하고

삭제
```

한다.

---

# 150. Network

```text
fetch()
```

를 이용하여 Backend API와 통신할 수 있다.

---

# 151. 전체 연결

```text
HTML
구조
    ↓

CSS
디자인
    ↓

JavaScript
동작
    ↓

DOM
화면 조작
    ↓

Event
사용자 행동
    ↓

fetch
HTTP Request
    ↓

FastAPI
Backend
    ↓

AI Model / Database
```

---

# 152. 한 문장으로 정리

JavaScript는:

```text
Browser에서 Data와 Logic을 처리하고,
사용자의 Event에 반응하며,
DOM을 변경하고,
Backend API와 통신하여
Web Page에 실제 기능을 추가하는 Programming Language
```

라고 이해할 수 있다.

---

# 153. 가장 먼저 기억할 것

```javascript
const name = "Kim";


if (
    name === "Kim"
) {

    console.log(
        "Hello Kim"
    );

}
```

그리고 Web에서는:

```javascript
const button =
    document.getElementById(
        "button"
    );


button.addEventListener(
    "click",
    () => {

        console.log(
            "Click"
        );

    }
);
```

이 두 흐름을 이해하면 JavaScript 기초에서 중요한 출발점을 잡은 것이다.