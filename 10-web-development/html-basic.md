# HTML Basic

## 1. HTML이란?

HTML은:

```text
HyperText Markup Language
```

의 약자이다.

Web Page의:

```text
구조
```

를 만드는 언어이다.

예를 들어 Web Page에:

```text
제목

본문

이미지

링크

메뉴

버튼

입력창
```

등이 어디에 존재하는지를 HTML로 표현한다.

---

# 2. HTML은 Programming Language인가?

일반적으로 HTML은:

```text
Programming Language
```

라기보다:

```text
Markup Language
```

라고 한다.

Python처럼:

```text
조건문

반복문

함수

계산
```

을 수행하는 것이 주목적이 아니기 때문이다.

HTML의 핵심 목적은:

```text
문서의 구조와 의미를 표현하는 것
```

이다.

---

# 3. HTML File

HTML File의 확장자는:

```text
.html
```

이다.

대표적인 첫 Page 이름으로:

```text
index.html
```

을 많이 사용한다.

---

# 4. 기본 Project

예:

```text
my-web/
└── index.html
```

---

# 5. HTML 기본 구조

```html
<!DOCTYPE html>

<html lang="ko">

<head>
    <meta charset="UTF-8">

    <meta
        name="viewport"
        content="width=device-width, initial-scale=1.0"
    >

    <title>My Website</title>
</head>

<body>

    <h1>Hello Web</h1>

</body>

</html>
```

HTML을 배울 때 가장 먼저 익혀야 하는 기본 구조이다.

---

# 6. `<!DOCTYPE html>`

```html
<!DOCTYPE html>
```

은 Browser에게:

```text
이 문서는 HTML5 문서입니다.
```

라는 것을 알려준다.

---

# 7. `<html>`

```html
<html>
</html>
```

은 HTML 문서 전체를 감싸는 가장 바깥쪽 Element이다.

---

# 8. `lang`

```html
<html lang="ko">
```

의:

```text
lang="ko"
```

는 문서의 주 언어가 한국어라는 것을 나타낸다.

---

# 9. `<head>`

```html
<head>
</head>
```

에는 Page 자체에 대한 설정과 정보가 들어간다.

예:

```text
문자 Encoding

Page Title

CSS 연결

Meta 정보
```

등이다.

---

# 10. `<body>`

```html
<body>
</body>
```

에는 실제 Browser 화면에 표시되는 내용이 들어간다.

예:

```text
제목

본문

Image

Button

Menu

Form
```

등이다.

---

# 11. head와 body

```text
HTML
│
├── head
│   └── 문서 설정 / 정보
│
└── body
    └── 실제 화면 내용
```

이라고 생각하면 된다.

---

# 12. `<meta charset>`

```html
<meta charset="UTF-8">
```

은 문자의 Encoding 방식을 지정한다.

한국어 등이 깨지는 문제를 방지하기 위해 일반적으로 UTF-8을 사용한다.

---

# 13. viewport

```html
<meta
    name="viewport"
    content="width=device-width, initial-scale=1.0"
>
```

은 Mobile 환경에서 Page의 크기를 적절하게 처리하기 위한 중요한 설정이다.

---

# 14. 왜 필요한가?

Desktop뿐 아니라:

```text
Mobile

Tablet
```

에서도 적절한 크기로 Web Page를 표시하기 위해 사용한다.

반응형 Web을 만들 때 특히 중요하다.

---

# 15. `<title>`

```html
<title>My Website</title>
```

은 Browser Tab 등에 표시되는 Page 제목이다.

---

# 16. Tag

HTML은:

```text
Tag
```

를 이용하여 구조를 만든다.

예:

```html
<h1>Hello</h1>
```

---

# 17. Opening Tag와 Closing Tag

```html
<h1>
```

은 Opening Tag,

```html
</h1>
```

은 Closing Tag이다.

---

# 18. Content

```html
<h1>Hello</h1>
```

에서:

```text
Hello
```

가 Content이다.

---

# 19. Element

전체:

```html
<h1>Hello</h1>
```

를 HTML Element라고 부를 수 있다.

---

# 20. 기본 구조

```text
Opening Tag
    ↓
<h1>Hello</h1>
     ↑      ↑
 Content   Closing Tag
```

이다.

---

# 21. 모든 Tag에 Closing Tag가 있는 것은 아니다

예:

```html
<img>

<input>

<br>

<meta>

<link>
```

같은 Void Element는 별도의 Closing Tag를 사용하지 않는다.

---

# 22. Attribute

HTML Tag에는 추가 정보를 넣을 수 있다.

이를:

```text
Attribute
```

라고 한다.

예:

```html
<a href="https://example.com">
    이동
</a>
```

---

# 23. 여기서

```text
a
→ Tag

href
→ Attribute

https://example.com
→ Attribute Value
```

이다.

---

# 24. Heading

제목은:

```html
<h1>제목 1</h1>

<h2>제목 2</h2>

<h3>제목 3</h3>

<h4>제목 4</h4>

<h5>제목 5</h5>

<h6>제목 6</h6>
```

처럼 사용할 수 있다.

---

# 25. h1 ~ h6

숫자는 단순히 글자 크기만 의미하는 것이 아니다.

문서의:

```text
Heading Level
```

을 표현한다.

---

# 26. 예

```html
<h1>강남개발자들</h1>

<h2>서비스</h2>

<h3>홈페이지 제작</h3>

<h3>앱 제작</h3>

<h2>포트폴리오</h2>
```

처럼 문서 구조를 표현할 수 있다.

---

# 27. Paragraph

문단은:

```html
<p>
    홈페이지 및 앱 제작 서비스를 제공합니다.
</p>
```

처럼 작성한다.

`p`는:

```text
Paragraph
```

을 의미한다.

---

# 28. 줄바꿈

```html
<br>
```

을 사용할 수 있다.

예:

```html
<p>
    홈페이지 제작<br>
    앱 제작
</p>
```

---

# 29. 하지만

화면 간격을 만들기 위해 `<br>`를 여러 번 사용하는 것은 좋은 방법이 아니다.

간격과 Layout은 이후:

```text
CSS
```

로 처리하는 것이 좋다.

---

# 30. `<div>`

HTML에서 매우 자주 보게 되는 Tag이다.

```html
<div>
    내용
</div>
```

---

# 31. div란?

`div`는 여러 Element를 하나의 Group으로 묶을 때 사용할 수 있는 일반적인 Container이다.

---

# 32. 예

```html
<div>
    <h2>홈페이지 제작</h2>

    <p>
        기업 홈페이지를 제작합니다.
    </p>

    <button>
        문의하기
    </button>
</div>
```

---

# 33. 구조

```text
div
├── h2
├── p
└── button
```

처럼 생각할 수 있다.

---

# 34. div를 사용하는 이유

여러 Element를 하나로 묶으면 이후 CSS나 JavaScript로 함께 다루기 편해진다.

---

# 35. 하지만 div만 사용하지 않는다

HTML에는 의미를 표현하는 Semantic Tag도 있다.

예:

```html
<header>
</header>

<nav>
</nav>

<main>
</main>

<section>
</section>

<article>
</article>

<footer>
</footer>
```

---

# 36. Semantic HTML

Element 이름만 보고도 역할을 어느 정도 알 수 있도록 HTML을 작성하는 것을 생각할 수 있다.

---

# 37. 예

```html
<header>
    <h1>강남개발자들</h1>
</header>

<main>

    <section>
        <h2>서비스</h2>
    </section>

</main>

<footer>
    Copyright
</footer>
```

---

# 38. `<header>`

Page 또는 Section의 Header 영역에 사용할 수 있다.

예:

```text
Logo

Navigation

Page Title
```

등이다.

---

# 39. `<nav>`

주요 Navigation Link를 묶을 때 사용할 수 있다.

```html
<nav>
    <a href="/">HOME</a>

    <a href="/portfolio">
        PORTFOLIO
    </a>
</nav>
```

---

# 40. `<main>`

Page의 주요 Content를 나타낸다.

```html
<main>
    ...
</main>
```

---

# 41. `<section>`

관련 있는 Content를 Section 단위로 나눌 때 사용할 수 있다.

```html
<section>
    <h2>서비스</h2>

    <p>
        서비스 소개
    </p>
</section>
```

---

# 42. `<article>`

독립적으로 의미를 가질 수 있는 Content에 사용할 수 있다.

예:

```text
Blog Post

News Article

게시글
```

등이다.

---

# 43. `<footer>`

Page 또는 Section의 Footer 영역을 표현한다.

예:

```text
Copyright

회사 정보

연락처

약관 Link
```

등이다.

---

# 44. Link

다른 Page로 이동하려면:

```html
<a href="https://example.com">
    Example
</a>
```

를 사용한다.

---

# 45. `a`

`a`는:

```text
Anchor
```

Element이다.

---

# 46. `href`

```html
href="..."
```

는 Link가 이동할 위치를 지정한다.

---

# 47. 외부 Link

```html
<a href="https://example.com">
    Example
</a>
```

---

# 48. 내부 Page

```html
<a href="about.html">
    회사소개
</a>
```

---

# 49. 같은 Page의 특정 위치

```html
<a href="#portfolio">
    포트폴리오
</a>
```

그리고:

```html
<section id="portfolio">
    ...
</section>
```

처럼 연결할 수 있다.

---

# 50. `id`

```html
id="portfolio"
```

는 특정 Element를 식별하는 데 사용할 수 있다.

---

# 51. 새 Tab

```html
<a
    href="https://example.com"
    target="_blank"
    rel="noopener noreferrer"
>
    Example
</a>
```

처럼 사용할 수 있다.

---

# 52. Image

Image를 표시할 때:

```html
<img
    src="image.jpg"
    alt="서비스 소개 이미지"
>
```

를 사용한다.

---

# 53. `src`

```text
Source
```

즉 Image File의 위치이다.

---

# 54. `alt`

Image를 표시할 수 없거나 보조 기술을 사용하는 상황 등에서 Image의 의미를 전달하는 대체 Text이다.

```html
alt="강남개발자들 홈페이지 제작 서비스"
```

처럼 Image의 의미를 설명한다.

---

# 55. Project 구조

```text
my-web/
├── index.html
└── images/
    └── main.jpg
```

이라면:

```html
<img
    src="images/main.jpg"
    alt="메인 이미지"
>
```

처럼 사용할 수 있다.

---

# 56. 상대 경로

현재 File 위치를 기준으로 File을 찾는 방식을:

```text
Relative Path
```

라고 한다.

---

# 57. 예

```text
images/main.jpg
```

---

# 58. 상위 Folder

```text
../
```

를 사용한다.

예:

```html
<img
    src="../images/main.jpg"
    alt="이미지"
>
```

---

# 59. Path Error

Web 개발에서 Image가 보이지 않는다면 가장 먼저:

```text
File 이름

Folder 이름

현재 HTML 위치

상대 경로
```

를 확인한다.

---

# 60. List

목록을 만들 수 있다.

---

# 61. Unordered List

순서가 중요하지 않은 목록:

```html
<ul>
    <li>HTML</li>
    <li>CSS</li>
    <li>JavaScript</li>
</ul>
```

---

# 62. 구조

```text
ul
├── li
├── li
└── li
```

이다.

---

# 63. Ordered List

순서가 있는 목록:

```html
<ol>
    <li>HTML 공부</li>
    <li>CSS 공부</li>
    <li>JavaScript 공부</li>
</ol>
```

---

# 64. `ul`과 `ol`

```text
ul
→ Unordered List

ol
→ Ordered List

li
→ List Item
```

이다.

---

# 65. Button

```html
<button>
    문의하기
</button>
```

---

# 66. HTML Button 자체

Button을 만들었다고 자동으로 원하는 기능이 실행되는 것은 아니다.

동작은 이후:

```text
JavaScript
```

와 연결할 수 있다.

---

# 67. Input

사용자 입력을 받을 때:

```html
<input type="text">
```

를 사용할 수 있다.

---

# 68. 여러 Input Type

```html
<input type="text">

<input type="password">

<input type="email">

<input type="number">

<input type="checkbox">

<input type="radio">

<input type="file">
```

등이 있다.

---

# 69. Label

Input이 무엇을 입력하는 곳인지 설명할 때:

```html
<label for="username">
    이름
</label>

<input
    id="username"
    type="text"
>
```

처럼 연결할 수 있다.

---

# 70. `for`와 `id`

```text
label의 for
=
input의 id
```

로 연결한다.

---

# 71. Form

여러 입력값을 묶어서 제출할 때:

```html
<form>
    ...
</form>
```

을 사용한다.

---

# 72. Login Form 예

```html
<form>

    <label for="user-id">
        아이디
    </label>

    <input
        id="user-id"
        type="text"
    >


    <label for="password">
        비밀번호
    </label>

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

# 73. Form과 Backend

나중에는 Form의 Data를:

```text
Frontend
↓
HTTP Request
↓
Backend
```

로 보낼 수 있다.

---

# 74. Textarea

긴 Text를 입력할 때:

```html
<textarea>
</textarea>
```

를 사용할 수 있다.

---

# 75. 문의 Form 예

```html
<label for="message">
    문의 내용
</label>

<textarea
    id="message"
></textarea>
```

---

# 76. Select

선택 목록:

```html
<select>

    <option>
        홈페이지 제작
    </option>

    <option>
        앱 제작
    </option>

</select>
```

---

# 77. Table

표 형태의 Data는:

```html
<table>
</table>
```

을 사용할 수 있다.

---

# 78. 기본 Table

```html
<table>

    <thead>

        <tr>
            <th>상품</th>
            <th>가격</th>
        </tr>

    </thead>


    <tbody>

        <tr>
            <td>홈페이지</td>
            <td>문의</td>
        </tr>

    </tbody>

</table>
```

---

# 79. Table Tag

```text
table
→ 전체 Table

thead
→ Header 영역

tbody
→ Data 영역

tr
→ Table Row

th
→ Header Cell

td
→ Data Cell
```

이다.

---

# 80. HTML 주석

HTML에서는:

```html
<!-- 주석입니다 -->
```

처럼 작성한다.

---

# 81. 예

```html
<!-- 메인 배너 -->

<section>
    ...
</section>
```

---

# 82. 주석은 화면에 보이지 않는다

Browser가 Page Content로 표시하지 않는다.

Code를 설명하거나 임시로 표시할 때 사용할 수 있다.

---

# 83. 언어마다 주석이 다르다

HTML:

```html
<!-- HTML Comment -->
```

CSS:

```css
/* CSS Comment */
```

JavaScript:

```javascript
// JavaScript Comment
```

Python:

```python
# Python Comment
```

이다.

---

# 84. VS Code Comment Shortcut

VS Code에서는 일반적으로:

```text
Ctrl + /
```

을 이용하여 현재 언어에 맞는 주석을 빠르게 사용할 수 있다.

---

# 85. Nesting

HTML Element 안에 다른 Element를 넣을 수 있다.

```html
<div>

    <h2>
        서비스
    </h2>

    <p>
        홈페이지 제작
    </p>

</div>
```

---

# 86. Parent / Child

위 구조에서:

```text
div
→ Parent

h2
→ Child

p
→ Child
```

로 볼 수 있다.

---

# 87. Tree Structure

HTML은:

```text
Tree
```

구조로 생각하는 것이 중요하다.

```text
body
├── header
│   ├── h1
│   └── nav
│       ├── a
│       └── a
│
├── main
│   └── section
│       ├── h2
│       └── p
│
└── footer
```

---

# 88. 왜 Tree가 중요할까?

나중에:

```text
CSS Selector

JavaScript DOM
```

을 이해할 때 그대로 연결된다.

---

# 89. 들여쓰기

Browser는 들여쓰기 자체를 보고 구조를 판단하는 것은 아니지만 개발자가 Code 구조를 이해하기 쉽게 만들어준다.

---

# 90. 나쁜 예

```html
<div><h2>서비스</h2><p>홈페이지 제작</p><button>문의</button></div>
```

---

# 91. 읽기 좋은 예

```html
<div>

    <h2>
        서비스
    </h2>

    <p>
        홈페이지 제작
    </p>

    <button>
        문의
    </button>

</div>
```

---

# 92. `class`

여러 Element를 같은 Group으로 지정할 때 자주 사용한다.

```html
<div class="card">
    ...
</div>
```

---

# 93. 여러 Element

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

---

# 94. CSS에서

나중에:

```css
.card {
    ...
}
```

처럼 같은 Class를 가진 Element에 공통 Style을 적용할 수 있다.

---

# 95. `id`와 `class`

간단하게:

```text
id
→ 특정 Element를 식별

class
→ 여러 Element를 Group화
```

라고 이해할 수 있다.

---

# 96. 예

```html
<section
    id="portfolio"
    class="section"
>
    ...
</section>
```

한 Element에 `id`와 `class`를 같이 사용할 수도 있다.

---

# 97. Emmet

VS Code에서는 HTML을 빠르게 작성하기 위해:

```text
Emmet
```

을 사용할 수 있다.

---

# 98. HTML 기본 구조

HTML File에서:

```text
!
```

를 입력한 후 Emmet 확장을 실행하면 HTML 기본 구조를 빠르게 생성할 수 있다.

환경에 따라 일반적으로 `Tab` 또는 자동완성 선택을 사용할 수 있다.

---

# 99. div

```text
div
```

입력 후 Emmet을 확장하면:

```html
<div></div>
```

를 빠르게 만들 수 있다.

---

# 100. h1

```text
h1
```

→

```html
<h1></h1>
```

---

# 101. List Emmet

```text
ul>li
```

→

```html
<ul>
    <li></li>
</ul>
```

---

# 102. 여러 개 만들기

```text
ul>li*3
```

→

```html
<ul>
    <li></li>
    <li></li>
    <li></li>
</ul>
```

---

# 103. class Emmet

```text
div.card
```

→

```html
<div class="card"></div>
```

---

# 104. id Emmet

```text
section#portfolio
```

→

```html
<section id="portfolio"></section>
```

---

# 105. 조합

```text
section.service>h2+p
```

같은 방식으로 여러 Element 구조를 빠르게 생성할 수도 있다.

---

# 106. Emmet의 목적

HTML 문법을 대신 공부하는 기능이 아니다.

이미 알고 있는 HTML 구조를:

```text
더 빠르게 작성하기 위한 도구
```

이다.

---

# 107. VS Code에서 Emmet이 안 될 때

`Enter`가 항상 Emmet 확장 키로 동작하는 것은 아니다.

환경이나 자동완성 상태에 따라 단순 줄바꿈이 될 수 있다.

우선:

```text
Tab
```

또는 자동완성 목록에서 Emmet 항목을 선택해 확장하는 방법을 사용할 수 있다.

---

# 108. CSS File 연결

HTML에서 CSS File을 연결한다.

```html
<link
    rel="stylesheet"
    href="style.css"
>
```

일반적으로 `<head>` 안에 작성한다.

---

# 109. JavaScript File 연결

```html
<script src="script.js"></script>
```

처럼 연결한다.

---

# 110. script 위치

초보 단계에서는 `</body>` 직전에 두는 방법을 자주 볼 수 있다.

```html
<body>

    ...

    <script src="script.js"></script>

</body>
```

---

# 111. defer

`head`에서 Script를 연결하면서:

```html
<script
    src="script.js"
    defer
></script>
```

처럼 `defer`를 사용하는 방법도 있다.

---

# 112. `defer`

HTML Parsing을 불필요하게 막지 않으면서 문서 Parsing 후 Script를 실행할 수 있도록 사용하는 Attribute이다.

---

# 113. 완성된 기본 구조

```html
<!DOCTYPE html>

<html lang="ko">

<head>

    <meta charset="UTF-8">

    <meta
        name="viewport"
        content="width=device-width, initial-scale=1.0"
    >

    <title>
        강남개발자들
    </title>

    <link
        rel="stylesheet"
        href="style.css"
    >

    <script
        src="script.js"
        defer
    ></script>

</head>


<body>

    <header>

        <h1>
            강남개발자들
        </h1>

        <nav>

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


    <main>

        <section id="home">

            <h2>
                홈페이지 & 앱 제작
            </h2>

            <p>
                필요한 서비스를 함께 만듭니다.
            </p>

            <button>
                문의하기
            </button>

        </section>


        <section id="service">

            <h2>
                Service
            </h2>

            <ul>
                <li>기업 홈페이지</li>
                <li>Landing Page</li>
                <li>Web Application</li>
                <li>App Development</li>
            </ul>

        </section>


        <section id="portfolio">

            <h2>
                Portfolio
            </h2>

        </section>

    </main>


    <footer>

        <p>
            © Gangnam Developers
        </p>

    </footer>

</body>

</html>
```

---

# 114. 이 HTML의 Tree

```text
html
├── head
│   ├── meta
│   ├── meta
│   ├── title
│   ├── link
│   └── script
│
└── body
    ├── header
    │   ├── h1
    │   └── nav
    │       ├── a
    │       ├── a
    │       └── a
    │
    ├── main
    │   ├── section#home
    │   │   ├── h2
    │   │   ├── p
    │   │   └── button
    │   │
    │   ├── section#service
    │   │   ├── h2
    │   │   └── ul
    │   │       └── li
    │   │
    │   └── section#portfolio
    │       └── h2
    │
    └── footer
        └── p
```

---

# 115. HTML을 작성할 때 생각하는 방법

디자인을 보고 바로 Code부터 작성하지 않는다.

먼저 화면을 영역으로 나눈다.

```text
Header

Main

Hero

Service

Portfolio

Contact

Footer
```

---

# 116. 그리고 구조를 만든다

```text
header
main
    section
    section
    section
footer
```

---

# 117. 그 다음 세부 Element

```text
section
├── h2
├── p
├── img
└── button
```

처럼 만든다.

---

# 118. 그 다음 CSS

HTML 구조가 만들어지면 CSS를 이용해:

```text
크기

위치

색상

간격

반응형
```

을 적용한다.

---

# 119. 그 다음 JavaScript

필요한 부분에:

```text
Menu Toggle

Slider

Popup

Button Event

API Request
```

같은 동작을 추가한다.

---

# 120. HTML 작성 순서

```text
화면 분석
↓
큰 Section 구분
↓
Semantic HTML
↓
세부 Element
↓
class / id
↓
CSS
↓
JavaScript
```

로 생각할 수 있다.

---

# 121. 자주 사용하는 HTML Tag

```text
html
head
body

header
nav
main
section
article
footer

div

h1 ~ h6
p

a
img

ul
ol
li

form
label
input
textarea
select
option
button

table
thead
tbody
tr
th
td
```

---

# 122. HTML에서 중요한 것

Tag를 전부 외우는 것이 가장 중요한 것은 아니다.

더 중요한 것은:

```text
화면을 구조로 나누는 능력
```

이다.

---

# 123. HTML과 CSS 역할 구분

HTML에서:

```html
<h1>
    Hello
</h1>
```

은:

```text
이 Text는 중요한 제목이다.
```

라는 구조와 의미를 표현한다.

---

# 124. CSS에서

```css
h1 {
    font-size: 60px;
}
```

은:

```text
그 제목을 어떻게 보여줄 것인가?
```

를 결정한다.

---

# 125. HTML에 디자인을 몰아넣지 않는다

가능하면:

```text
HTML
→ 구조와 의미

CSS
→ 디자인

JavaScript
→ 동작
```

역할을 구분한다.

---

# 126. Browser에서 확인

HTML File을 Browser에서 열면 결과를 확인할 수 있다.

개발 중에는 VS Code의 Live Server 같은 개발 도구를 사용할 수도 있다.

---

# 127. Developer Tools

Chrome에서:

```text
F12
```

를 눌러 HTML 구조를 확인할 수 있다.

---

# 128. Elements

Elements Tab에서는 Browser가 해석한 HTML 구조를 확인할 수 있다.

---

# 129. HTML Error

HTML은 일부 문법 오류를 Browser가 어느 정도 보정하기도 한다.

그래서 화면이 나온다고 해서 HTML 구조가 항상 올바른 것은 아니다.

---

# 130. 자주 발생하는 실수

```text
Closing Tag 누락

잘못된 Nesting

중복 id

Image Path 오류

잘못된 href

CSS File Path 오류

JavaScript File Path 오류
```

등이다.

---

# 131. 중복 id

다음처럼 작성하는 것은 피한다.

```html
<div id="card">
</div>

<div id="card">
</div>
```

`id`는 문서 안에서 고유하게 사용하는 것이 기본이다.

---

# 132. 같은 Group은 class

```html
<div class="card">
</div>

<div class="card">
</div>
```

처럼 사용한다.

---

# 133. HTML Debugging

문제가 생기면:

```text
1. File이 저장됐는가?

2. 올바른 HTML File을 열었는가?

3. Tag가 제대로 닫혔는가?

4. File Path가 맞는가?

5. Developer Tools에서 구조가 어떻게 보이는가?

6. Console Error가 있는가?
```

를 확인한다.

---

# 134. HTML과 DOM

Browser는 HTML을 읽은 후 내부적으로 문서 구조를 만든다.

이를:

```text
DOM
```

이라고 한다.

DOM은:

```text
Document Object Model
```

의 약자이다.

---

# 135. 예

HTML:

```html
<body>

    <h1>
        Hello
    </h1>

    <button>
        Click
    </button>

</body>
```

Browser는 개념적으로:

```text
Document
└── html
    └── body
        ├── h1
        └── button
```

같은 Tree로 이해한다.

---

# 136. 왜 DOM이 중요할까?

JavaScript는 이 DOM을 이용하여 HTML Element를 찾고 변경한다.

---

# 137. 예

HTML:

```html
<h1 id="title">
    Hello
</h1>
```

JavaScript:

```javascript
document.getElementById("title");
```

로 해당 Element를 찾을 수 있다.

---

# 138. 그리고 변경

```javascript
document.getElementById(
    "title"
).textContent = "Hello JavaScript";
```

처럼 화면 내용을 바꿀 수 있다.

---

# 139. 즉

```text
HTML
↓
Browser
↓
DOM
↓
JavaScript가 DOM 조작
↓
화면 변경
```

이다.

---

# 140. 앞으로의 연결

```text
HTML
→ 구조

CSS
→ 구조를 디자인

JavaScript
→ DOM을 조작

Event
→ 사용자 행동 처리

HTTP / API
→ Server와 Data 통신
```

으로 이어진다.

---

# 141. 한 문장으로 정리

HTML은:

```text
Web Page를 구성하는
제목, 문단, Image, Link, Button, Form 등의
구조와 의미를 Tag를 이용하여 표현하는
Markup Language
```

이다.

---

# 142. 가장 중요한 구조

HTML을 공부하면서 다음 구조를 기억한다.

```text
<!DOCTYPE html>
        ↓
<html>
        ↓
┌───────────────────────┐
│                       │
head                   body
│                       │
설정                  화면
                        │
              ┌─────────┼─────────┐
            header     main      footer
                        │
                     section
                        │
                h1 / p / img /
                a / button ...
```

---

# 143. 핵심 정리

```text
HTML
→ 구조

Tag
→ Element의 종류

Attribute
→ Element의 추가 정보

div
→ 일반 Container

Semantic Tag
→ 의미가 있는 구조

class
→ Group

id
→ 특정 Element 식별

Form
→ 사용자 입력

DOM
→ Browser가 만든 HTML Tree
```

이 개념들이 이후 CSS와 JavaScript의 기반이 된다.