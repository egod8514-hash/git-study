# DOM & Event

## 1. DOM이란?

DOM은:

```text
Document Object Model
```

의 약자이다.

Browser가 HTML 문서를 읽고 JavaScript가 다룰 수 있도록 만든 객체 구조이다.

---

# 2. HTML과 DOM

HTML:

```html
<body>

    <h1>Hello</h1>

    <button>Click</button>

</body>
```

Browser는 이를 개념적으로:

```text
Document
└── html
    └── body
        ├── h1
        └── button
```

같은 Tree 구조로 만든다.

이 구조가 DOM이다.

---

# 3. DOM이 필요한 이유

JavaScript는 DOM을 이용하여 HTML Element를:

```text
찾고

읽고

변경하고

추가하고

삭제
```

할 수 있다.

즉:

```text
HTML
↓
DOM
↓
JavaScript
↓
화면 변경
```

이다.

---

# 4. document

JavaScript에서 현재 HTML 문서를 나타내는 대표적인 Object가:

```javascript
document
```

이다.

예:

```javascript
console.log(
    document
);
```

---

# 5. Element 찾기

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
```

---

# 6. getElementById

```javascript
document.getElementById(
    "title"
);
```

은:

```html
id="title"
```

인 Element를 찾는다.

---

# 7. 결과 확인

```javascript
console.log(
    title
);
```

Browser Console에서 선택된 HTML Element를 확인할 수 있다.

---

# 8. querySelector

현대 JavaScript에서 매우 자주 사용하는 방법이다.

```javascript
const title =
    document.querySelector(
        "#title"
    );
```

---

# 9. CSS Selector 사용

`querySelector()` 안에는 CSS Selector를 사용할 수 있다.

ID:

```javascript
document.querySelector(
    "#title"
);
```

Class:

```javascript
document.querySelector(
    ".card"
);
```

Tag:

```javascript
document.querySelector(
    "button"
);
```

---

# 10. querySelector의 특징

조건에 맞는:

```text
첫 번째 Element
```

를 반환한다.

---

# 11. querySelectorAll

조건에 맞는 여러 Element를 선택하려면:

```javascript
const cards =
    document.querySelectorAll(
        ".card"
    );
```

을 사용할 수 있다.

---

# 12. 예

HTML:

```html
<div class="card">
    Card 1
</div>

<div class="card">
    Card 2
</div>

<div class="card">
    Card 3
</div>
```

JavaScript:

```javascript
const cards =
    document.querySelectorAll(
        ".card"
    );


console.log(
    cards
);
```

---

# 13. 여러 Element 반복

```javascript
cards.forEach(
    (card) => {

        console.log(
            card
        );

    }
);
```

---

# 14. Element 선택 정리

```text
getElementById()
→ ID로 하나 선택

querySelector()
→ CSS Selector로 첫 번째 하나 선택

querySelectorAll()
→ CSS Selector로 여러 개 선택
```

이다.

---

# 15. textContent

Element의 Text를 읽거나 변경할 수 있다.

HTML:

```html
<h1 id="title">
    Hello
</h1>
```

JavaScript:

```javascript
const title =
    document.querySelector(
        "#title"
    );


console.log(
    title.textContent
);
```

---

# 16. Text 변경

```javascript
title.textContent =
    "Hello JavaScript";
```

화면의 제목이 변경된다.

---

# 17. innerHTML

HTML 문자열을 Element 내부에 넣을 수도 있다.

```javascript
const container =
    document.querySelector(
        "#container"
    );


container.innerHTML = `
    <h2>Service</h2>
    <p>홈페이지 제작</p>
`;
```

---

# 18. textContent와 innerHTML

```text
textContent
→ Text로 처리

innerHTML
→ HTML로 해석
```

이다.

---

# 19. innerHTML 주의

사용자가 입력한 값을 검증 없이 `innerHTML`에 넣으면 보안 문제가 발생할 수 있다.

특히:

```text
XSS
Cross-Site Scripting
```

위험을 고려해야 한다.

단순 Text를 표시한다면 가능한 경우:

```javascript
textContent
```

를 사용하는 편이 안전하다.

---

# 20. Attribute 가져오기

HTML:

```html
<img
    id="main-image"
    src="images/main.jpg"
    alt="메인 이미지"
>
```

JavaScript:

```javascript
const image =
    document.querySelector(
        "#main-image"
    );


console.log(
    image.getAttribute(
        "src"
    )
);
```

---

# 21. Attribute 변경

```javascript
image.setAttribute(
    "src",
    "images/second.jpg"
);
```

---

# 22. 직접 Property 사용

일부 Attribute는 다음처럼 직접 변경할 수도 있다.

```javascript
image.src =
    "images/second.jpg";
```

---

# 23. classList

CSS Class를 JavaScript에서 다룰 수 있다.

HTML:

```html
<div
    id="menu"
    class="menu"
>
    Menu
</div>
```

---

# 24. Class 추가

```javascript
const menu =
    document.querySelector(
        "#menu"
    );


menu.classList.add(
    "active"
);
```

---

# 25. Class 제거

```javascript
menu.classList.remove(
    "active"
);
```

---

# 26. Class Toggle

```javascript
menu.classList.toggle(
    "active"
);
```

현재 Class가:

```text
있으면 제거

없으면 추가
```

한다.

---

# 27. Class 존재 확인

```javascript
console.log(
    menu.classList.contains(
        "active"
    )
);
```

결과는 Boolean이다.

---

# 28. HTML + CSS + JavaScript 연결

HTML:

```html
<nav
    id="menu"
    class="menu"
>
    Menu
</nav>
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
menu.classList.add(
    "active"
);
```

---

# 29. 역할

```text
HTML
→ Menu 구조

CSS
→ 기본 상태와 active 상태

JavaScript
→ active Class 변경
```

이다.

---

# 30. Event란?

사용자 또는 Browser에서 발생하는 특정 행동을:

```text
Event
```

라고 한다.

예:

```text
Click

Input

Submit

Scroll

Keyboard 입력

Mouse 이동

Page Load
```

등이 있다.

---

# 31. addEventListener

특정 Event가 발생했을 때 실행할 Function을 등록한다.

```javascript
button.addEventListener(
    "click",
    () => {

        console.log(
            "Click"
        );

    }
);
```

---

# 32. 구조

```text
Element
↓
Event 감지
↓
Function 실행
```

이다.

---

# 33. Click Event

HTML:

```html
<button id="button">
    클릭
</button>
```

JavaScript:

```javascript
const button =
    document.querySelector(
        "#button"
    );


button.addEventListener(
    "click",
    () => {

        console.log(
            "버튼을 클릭했습니다."
        );

    }
);
```

---

# 34. Click으로 Text 변경

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
    document.querySelector(
        "#title"
    );


const button =
    document.querySelector(
        "#button"
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

# 35. Event Object

Event Handler는 발생한 Event에 대한 정보를 받을 수 있다.

```javascript
button.addEventListener(
    "click",
    (event) => {

        console.log(
            event
        );

    }
);
```

---

# 36. event.target

실제로 Event가 발생한 Element를 확인할 수 있다.

```javascript
button.addEventListener(
    "click",
    (event) => {

        console.log(
            event.target
        );

    }
);
```

---

# 37. Input Event

HTML:

```html
<input
    id="username"
    type="text"
>
```

JavaScript:

```javascript
const input =
    document.querySelector(
        "#username"
    );


input.addEventListener(
    "input",
    () => {

        console.log(
            input.value
        );

    }
);
```

---

# 38. input Event

사용자가 Input 값을 변경할 때 발생한다.

입력하는 순간마다 값을 확인하는 기능 등에 사용할 수 있다.

---

# 39. change Event

```javascript
input.addEventListener(
    "change",
    () => {

        console.log(
            input.value
        );

    }
);
```

`input`과 `change`는 발생 시점과 사용 목적이 다를 수 있다.

---

# 40. Form

HTML:

```html
<form id="login-form">

    <input
        id="user-id"
        type="text"
    >

    <input
        id="password"
        type="password"
    >

    <button type="submit">
        로그인
    </button>

</form>
```

---

# 41. submit Event

```javascript
const form =
    document.querySelector(
        "#login-form"
    );


form.addEventListener(
    "submit",
    () => {

        console.log(
            "Form 제출"
        );

    }
);
```

---

# 42. Form 기본 동작

Form을 제출하면 Browser가 Page 이동이나 새 요청 같은 기본 동작을 수행할 수 있다.

JavaScript로 직접 처리하고 싶다면 이를 막아야 할 때가 있다.

---

# 43. preventDefault

```javascript
form.addEventListener(
    "submit",
    (event) => {

        event.preventDefault();


        console.log(
            "JavaScript에서 Form 처리"
        );

    }
);
```

---

# 44. 의미

```text
기본 Form 제출 동작 중지

↓

JavaScript에서 직접 처리
```

이다.

---

# 45. Login Form 값 가져오기

```javascript
const form =
    document.querySelector(
        "#login-form"
    );


const userId =
    document.querySelector(
        "#user-id"
    );


const password =
    document.querySelector(
        "#password"
    );


form.addEventListener(
    "submit",
    (event) => {

        event.preventDefault();


        console.log(
            userId.value
        );


        console.log(
            password.value
        );

    }
);
```

---

# 46. Validation

사용자가 필요한 값을 입력했는지 확인할 수 있다.

```javascript
if (
    userId.value === ""
) {

    alert(
        "아이디를 입력해주세요."
    );

    return;

}
```

---

# 47. Password 검사

```javascript
if (
    password.value === ""
) {

    alert(
        "비밀번호를 입력해주세요."
    );

    return;

}
```

---

# 48. 전체 Form Validation

```javascript
form.addEventListener(
    "submit",
    (event) => {

        event.preventDefault();


        if (
            userId.value === ""
        ) {

            alert(
                "아이디를 입력해주세요."
            );

            return;

        }


        if (
            password.value === ""
        ) {

            alert(
                "비밀번호를 입력해주세요."
            );

            return;

        }


        console.log(
            "입력 확인 완료"
        );

    }
);
```

---

# 49. Frontend Validation만 믿으면 안 된다

JavaScript Validation은 사용자 편의를 위해 중요하지만 보안을 위해 Backend에서도 반드시 Data를 검증해야 한다.

```text
Frontend Validation
+
Backend Validation
```

둘 다 필요하다.

---

# 50. Mobile Menu 예제

HTML:

```html
<header>

    <h1>
        Logo
    </h1>


    <button
        id="menu-button"
        type="button"
    >
        MENU
    </button>


    <nav
        id="menu"
        class="menu"
    >

        <a href="#home">
            HOME
        </a>

        <a href="#service">
            SERVICE
        </a>

        <a href="#portfolio">
            PORTFOLIO
        </a>

    </nav>

</header>
```

---

# 51. CSS

```css
.menu {
    display: none;
}


.menu.active {
    display: flex;
}
```

---

# 52. JavaScript

```javascript
const menuButton =
    document.querySelector(
        "#menu-button"
    );


const menu =
    document.querySelector(
        "#menu"
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

# 53. 흐름

```text
사용자 MENU 클릭
↓
click Event
↓
Event Handler 실행
↓
classList.toggle()
↓
active 추가 또는 제거
↓
CSS 변경
↓
Menu 표시 또는 숨김
```

---

# 54. DOM Element 생성

JavaScript로 새로운 HTML Element를 만들 수도 있다.

```javascript
const item =
    document.createElement(
        "li"
    );
```

---

# 55. Text 넣기

```javascript
item.textContent =
    "새로운 메뉴";
```

---

# 56. DOM에 추가

HTML:

```html
<ul id="list">
</ul>
```

JavaScript:

```javascript
const list =
    document.querySelector(
        "#list"
    );


const item =
    document.createElement(
        "li"
    );


item.textContent =
    "HTML";


list.appendChild(
    item
);
```

---

# 57. 결과

Browser DOM은:

```html
<ul id="list">

    <li>
        HTML
    </li>

</ul>
```

처럼 변경된다.

---

# 58. 여러 Data를 화면에 표시

```javascript
const skills = [
    "HTML",
    "CSS",
    "JavaScript"
];


const list =
    document.querySelector(
        "#list"
    );


skills.forEach(
    (skill) => {

        const item =
            document.createElement(
                "li"
            );


        item.textContent =
            skill;


        list.appendChild(
            item
        );

    }
);
```

---

# 59. 중요한 개념

JavaScript Array의 Data를:

```text
DOM Element
```

로 변환하여 화면에 표시하고 있다.

---

# 60. API와 연결하면

Backend에서:

```json
[
    {
        "name": "상품 A",
        "price": 10000
    },
    {
        "name": "상품 B",
        "price": 20000
    }
]
```

같은 Data를 받았다고 하자.

JavaScript는 이 Array를 반복하면서 Product Card를 만들 수 있다.

---

# 61. Product Card 예

HTML:

```html
<div id="products">
</div>
```

JavaScript:

```javascript
const products = [

    {
        name: "상품 A",
        price: 10000
    },

    {
        name: "상품 B",
        price: 20000
    }

];


const container =
    document.querySelector(
        "#products"
    );


products.forEach(
    (product) => {

        const card =
            document.createElement(
                "div"
            );


        card.classList.add(
            "card"
        );


        const title =
            document.createElement(
                "h3"
            );


        title.textContent =
            product.name;


        const price =
            document.createElement(
                "p"
            );


        price.textContent =
            `${product.price}원`;


        card.appendChild(
            title
        );


        card.appendChild(
            price
        );


        container.appendChild(
            card
        );

    }
);
```

---

# 62. 이것이 중요한 이유

Frontend에서는 매우 자주:

```text
Server Data
↓
JavaScript Array / Object
↓
반복
↓
DOM Element 생성
↓
화면 출력
```

과정을 사용한다.

---

# 63. Element 삭제

```javascript
item.remove();
```

처럼 DOM에서 Element를 제거할 수 있다.

---

# 64. Element Style 직접 변경

```javascript
title.style.color =
    "red";
```

처럼 JavaScript에서 직접 Style을 변경할 수도 있다.

---

# 65. 하지만

복잡한 디자인 변경은:

```javascript
element.style...
```

을 계속 사용하는 것보다 CSS Class를 만들고 JavaScript에서 Class를 변경하는 방식이 관리하기 좋은 경우가 많다.

---

# 66. 권장 구조

CSS:

```css
.message {
    color: black;
}


.message.error {
    color: red;
}
```

JavaScript:

```javascript
message.classList.add(
    "error"
);
```

---

# 67. 역할 분리

```text
HTML
→ 구조

CSS
→ 디자인

JavaScript
→ 상태 변경
```

을 유지할 수 있다.

---

# 68. Scroll Event

```javascript
window.addEventListener(
    "scroll",
    () => {

        console.log(
            window.scrollY
        );

    }
);
```

---

# 69. 활용

Scroll 위치에 따라:

```text
Header 색상 변경

Animation 시작

Top Button 표시
```

등을 구현할 수 있다.

---

# 70. 하지만 Scroll Event 주의

Scroll Event는 매우 자주 발생할 수 있다.

무거운 작업을 매번 실행하면 성능 문제가 생길 수 있다.

복잡한 Scroll UI에서는:

```text
IntersectionObserver

throttle

debounce
```

같은 방법을 고려할 수 있다.

---

# 71. IntersectionObserver

Element가 화면에 들어오는지 감지할 때 사용할 수 있는 Browser API이다.

예를 들어 Scroll Animation 구현에 활용할 수 있다.

---

# 72. Keyboard Event

```javascript
document.addEventListener(
    "keydown",
    (event) => {

        console.log(
            event.key
        );

    }
);
```

---

# 73. Escape 처리 예

```javascript
document.addEventListener(
    "keydown",
    (event) => {

        if (
            event.key === "Escape"
        ) {

            menu.classList.remove(
                "active"
            );

        }

    }
);
```

---

# 74. Event Bubbling

HTML:

```html
<div id="parent">

    <button id="child">
        Click
    </button>

</div>
```

Child에서 발생한 Event가 Parent 방향으로 전달되는 현상을:

```text
Event Bubbling
```

이라고 한다.

---

# 75. 예

```javascript
parent.addEventListener(
    "click",
    () => {

        console.log(
            "Parent"
        );

    }
);


child.addEventListener(
    "click",
    () => {

        console.log(
            "Child"
        );

    }
);
```

Child를 클릭했는데 Parent Handler도 실행될 수 있다.

---

# 76. stopPropagation

필요한 경우 Event 전파를 막을 수 있다.

```javascript
child.addEventListener(
    "click",
    (event) => {

        event.stopPropagation();

    }
);
```

하지만 이유 없이 남용하지 않는다.

---

# 77. Event Delegation

여러 Child 각각에 Event를 등록하는 대신 Parent 하나에서 Event를 처리할 수도 있다.

---

# 78. 예

HTML:

```html
<ul id="menu-list">

    <li>HOME</li>

    <li>SERVICE</li>

    <li>PORTFOLIO</li>

</ul>
```

JavaScript:

```javascript
const menuList =
    document.querySelector(
        "#menu-list"
    );


menuList.addEventListener(
    "click",
    (event) => {

        if (
            event.target.tagName
            === "LI"
        ) {

            console.log(
                event.target.textContent
            );

        }

    }
);
```

---

# 79. 왜 가능한가?

Event Bubbling 때문에 Child의 Event가 Parent까지 전달되기 때문이다.

---

# 80. 장점

동적으로 추가되는 Child까지 처리해야 하는 상황 등에서 유용할 수 있다.

---

# 81. DOMContentLoaded

HTML 문서가 Parsing된 뒤 실행하고 싶다면:

```javascript
document.addEventListener(
    "DOMContentLoaded",
    () => {

        console.log(
            "DOM 준비 완료"
        );

    }
);
```

를 사용할 수 있다.

---

# 82. defer와 관계

Script에:

```html
<script
    src="script.js"
    defer
></script>
```

를 사용하면 DOM Parsing 이후 실행되므로 기본적인 DOM 조작에서는 편리하다.

---

# 83. Element가 null인 경우

```javascript
const button =
    document.querySelector(
        "#button"
    );
```

해당 Element가 없다면:

```text
null
```

이 반환된다.

---

# 84. 흔한 Error

```javascript
button.addEventListener(
    "click",
    ...
);
```

에서 `button`이 `null`이라면 Error가 발생할 수 있다.

---

# 85. 확인

```javascript
console.log(
    button
);
```

결과가:

```text
null
```

이면 HTML Selector를 확인한다.

---

# 86. 자주 확인할 것

```text
HTML id가 맞는가?

class 이름이 맞는가?

# 또는 . 을 제대로 사용했는가?

JavaScript가 너무 일찍 실행되고 있지 않은가?

올바른 HTML File을 열었는가?
```

---

# 87. Optional Chaining

필요한 경우:

```javascript
button?.addEventListener(
    "click",
    () => {

        console.log(
            "Click"
        );

    }
);
```

처럼 작성할 수도 있다.

하지만 단순히 Error를 숨기기보다 왜 Element가 없는지 확인하는 것이 먼저이다.

---

# 88. FormData

Form Data를 다룰 때 Browser의 `FormData`를 사용할 수도 있다.

HTML:

```html
<form id="contact-form">

    <input
        name="name"
        type="text"
    >

    <input
        name="email"
        type="email"
    >

    <button type="submit">
        보내기
    </button>

</form>
```

---

# 89. JavaScript

```javascript
const form =
    document.querySelector(
        "#contact-form"
    );


form.addEventListener(
    "submit",
    (event) => {

        event.preventDefault();


        const formData =
            new FormData(
                form
            );


        console.log(
            formData.get(
                "name"
            )
        );


        console.log(
            formData.get(
                "email"
            )
        );

    }
);
```

---

# 90. Image Upload와 연결

AI Image Service에서는:

```html
<input
    type="file"
    name="image"
>
```

를 사용할 수 있다.

그리고 JavaScript에서 `FormData`를 이용하여 Image File을 Backend로 보낼 수 있다.

---

# 91. AI Service 흐름

```text
사용자
↓
Image 선택
↓
File Input
↓
JavaScript
↓
FormData
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
DOM
↓
화면에 결과 표시
```

---

# 92. 간단한 Image 선택 확인

HTML:

```html
<input
    id="image-input"
    type="file"
    accept="image/*"
>
```

JavaScript:

```javascript
const imageInput =
    document.querySelector(
        "#image-input"
    );


imageInput.addEventListener(
    "change",
    () => {

        const file =
            imageInput.files[0];


        console.log(
            file
        );

    }
);
```

---

# 93. `files`

File Input에서 사용자가 선택한 File 정보를 확인할 수 있다.

---

# 94. Image Preview

```javascript
const file =
    imageInput.files[0];


if (file) {

    const imageUrl =
        URL.createObjectURL(
            file
        );


    preview.src =
        imageUrl;

}
```

---

# 95. 실제 AI UI

이 구조를 이용하면:

```text
X-ray Image 선택
↓
Preview 표시
↓
분석 Button 클릭
↓
Backend 전송
↓
Prediction 표시
```

같은 UI를 만들 수 있다.

---

# 96. Event Handler Function 분리

Code가 길어지면 Handler를 별도 Function으로 분리한다.

```javascript
function handleMenuClick() {

    menu.classList.toggle(
        "active"
    );

}


menuButton.addEventListener(
    "click",
    handleMenuClick
);
```

---

# 97. 장점

```text
읽기 쉬움

재사용

Debugging

유지보수
```

에 도움이 된다.

---

# 98. 이름 짓기

Event Handler에는:

```text
handleClick

handleSubmit

handleChange

handleMenuToggle
```

같은 이름을 자주 볼 수 있다.

---

# 99. 상태

Web UI는 어떤 상태를 가지고 있을 수 있다.

예:

```text
Menu가 열렸는가?

Login 되었는가?

Loading 중인가?

Error가 있는가?

Prediction 결과가 있는가?
```

---

# 100. 간단한 상태

```javascript
let isOpen =
    false;
```

---

# 101. 변경

```javascript
isOpen =
    !isOpen;
```

---

# 102. 하지만 UI 상태는 Class로 표현할 수도 있다

```javascript
menu.classList.toggle(
    "active"
);
```

처럼 DOM 상태와 CSS Class를 연결할 수 있다.

---

# 103. Loading 상태

API Request를 보낼 때:

```text
분석 중...
```

같은 상태를 표시할 수 있다.

```javascript
result.textContent =
    "분석 중...";
```

---

# 104. 성공 상태

```javascript
result.textContent =
    "분석 완료";
```

---

# 105. Error 상태

```javascript
result.textContent =
    "분석 중 오류가 발생했습니다.";
```

---

# 106. 사용자 경험

JavaScript에서는 기능만 실행되는 것이 아니라 사용자에게 현재 상태를 알려주는 것도 중요하다.

예:

```text
Loading

Success

Error

Disabled Button
```

등이다.

---

# 107. Button 비활성화

```javascript
button.disabled =
    true;
```

다시 활성화:

```javascript
button.disabled =
    false;
```

---

# 108. API 요청 중

```text
분석 Button 클릭
↓
Button Disabled
↓
Loading 표시
↓
API Request
↓
Response
↓
결과 표시
↓
Button Enabled
```

같은 흐름을 만들 수 있다.

---

# 109. DOM Debugging

화면이 변경되지 않으면:

```text
1. JavaScript File이 연결됐는가?

2. Console Error가 있는가?

3. Selector가 맞는가?

4. Element가 null인가?

5. Event가 발생하는가?

6. Handler가 실행되는가?

7. 값은 무엇인가?

8. Class가 실제 추가됐는가?

9. CSS가 해당 Class에 적용되는가?
```

순서로 확인한다.

---

# 110. Debugging 예

```javascript
console.log(
    "script 시작"
);


const button =
    document.querySelector(
        "#button"
    );


console.log(
    "button:",
    button
);


button.addEventListener(
    "click",
    () => {

        console.log(
            "click 발생"
        );

    }
);
```

---

# 111. 한 단계씩 확인

결과가:

```text
script 시작
```

까지만 나온다면:

```text
Element 선택
```

단계를 확인한다.

`button`까지 나오지만 클릭 로그가 없다면:

```text
Event
```

단계를 확인한다.

---

# 112. DOM과 Event 핵심 패턴

```javascript
const element =
    document.querySelector(
        "selector"
    );


element.addEventListener(
    "event",
    () => {

        // 원하는 동작

    }
);
```

이 구조를 기억한다.

---

# 113. 실제 Web 기능 대부분

많은 Frontend 기능은 기본적으로:

```text
Element 찾기
↓
Event 기다리기
↓
Function 실행
↓
Data 변경
↓
DOM 변경
```

으로 생각할 수 있다.

---

# 114. Menu Toggle

```text
Button 찾기
↓
click 기다리기
↓
active Class Toggle
↓
CSS 변경
↓
Menu 열기 / 닫기
```

---

# 115. Form

```text
Form 찾기
↓
submit 기다리기
↓
preventDefault
↓
Input 값 확인
↓
Validation
↓
Server Request
```

---

# 116. AI Prediction

```text
Image Input
↓
change
↓
Image 선택
↓
분석 Button click
↓
FormData 생성
↓
API Request
↓
Prediction Response
↓
DOM 변경
↓
결과 표시
```

---

# 117. DOM과 Framework

나중에 React, Vue 같은 Framework를 사용하면 DOM을 직접 조작하는 방식이 줄어들 수 있다.

하지만:

```text
DOM

Event

State

Rendering
```

개념은 그대로 중요하다.

---

# 118. DOM을 먼저 배우는 이유

Framework가 내부적으로 해결해 주는 문제가 무엇인지 이해하기 쉬워진다.

---

# 119. HTML과 연결

```text
HTML
→ Element 생성
```

---

# 120. CSS와 연결

```text
CSS
→ Element의 Style과 상태 표현
```

---

# 121. JavaScript와 연결

```text
JavaScript
→ Element와 상태 변경
```

---

# 122. Event와 연결

```text
Event
→ 언제 JavaScript를 실행할지 결정
```

---

# 123. API와 연결

```text
fetch()
→ Backend와 Data 통신
```

---

# 124. 전체 흐름

```text
사용자
↓
HTML Element
↓
Event
↓
JavaScript
↓
Data 처리
↓
DOM 변경
↓
화면 변경
```

또는:

```text
사용자
↓
Event
↓
JavaScript
↓
HTTP Request
↓
Backend
↓
Response
↓
JavaScript
↓
DOM 변경
↓
사용자
```

이다.

---

# 125. 핵심 정리

```text
DOM
→ Browser가 만든 HTML 객체 구조

document
→ 현재 HTML 문서

querySelector()
→ Element 하나 찾기

querySelectorAll()
→ 여러 Element 찾기

textContent
→ Text 변경

classList
→ CSS Class 변경

createElement()
→ Element 생성

appendChild()
→ Element 추가

remove()
→ Element 삭제

addEventListener()
→ Event 처리

event.target
→ Event 발생 Element

preventDefault()
→ Browser 기본 동작 중지
```

---

# 126. 가장 중요한 Code

```javascript
const button =
    document.querySelector(
        "#button"
    );


const title =
    document.querySelector(
        "#title"
    );


button.addEventListener(
    "click",
    () => {

        title.textContent =
            "Hello JavaScript";

    }
);
```

---

# 127. 한 문장으로 정리

DOM과 Event는:

```text
Browser가 HTML로 만든 DOM Element를
JavaScript로 선택하고,

사용자의 Click, Input, Submit 등의 Event가 발생했을 때
원하는 Logic을 실행하여

HTML의 내용, Class, Element 등을 변경함으로써
Web Page를 동적으로 만드는 핵심 개념
```

이다.

---

# 128. 지금까지 Chapter 10 연결

```text
Web Basic
↓
Client / Server / Request / Response

HTML
↓
구조

CSS
↓
디자인 / Layout / Responsive

JavaScript
↓
Data / Logic / Function

DOM
↓
HTML 조작

Event
↓
사용자 행동 처리
```

이제 다음 단계에서는 이 Frontend가 Backend와 어떻게 연결되는지 정리한다.