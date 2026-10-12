# CSS Basic

## 1. CSS란?

CSS는:

```text
Cascading Style Sheets
```

의 약자이다.

HTML로 만든 구조에:

```text
색상
크기
간격
위치
배경
폰트
Layout
Animation
반응형
```

등을 적용한다.

---

# 2. HTML과 CSS

HTML:

```html
<h1>강남개발자들</h1>
```

CSS:

```css
h1 {
    color: black;
    font-size: 48px;
}
```

HTML은:

```text
무엇이 있는가?
```

를 정의하고,

CSS는:

```text
어떻게 보여줄 것인가?
```

를 정의한다.

---

# 3. CSS 기본 문법

```css
h1 {
    color: red;
}
```

구조는:

```text
h1
→ Selector

color
→ Property

red
→ Value
```

이다.

---

# 4. Selector

어떤 HTML Element에 Style을 적용할지 선택한다.

예:

```css
h1 {
    color: red;
}
```

모든 `h1`에 적용된다.

---

# 5. 여러 Property

```css
h1 {
    color: black;
    font-size: 48px;
    font-weight: 700;
}
```

처럼 여러 Style을 적용할 수 있다.

---

# 6. CSS 연결 방법

HTML에서 외부 CSS File을 연결할 수 있다.

Project:

```text
my-web/
├── index.html
└── style.css
```

HTML:

```html
<head>

    <link
        rel="stylesheet"
        href="style.css"
    >

</head>
```

---

# 7. 왜 CSS File을 분리할까?

HTML과 디자인을 분리하면:

```text
Code 관리

재사용

수정

협업
```

이 쉬워진다.

---

# 8. CSS 주석

```css
/* Header Style */
```

처럼 작성한다.

---

# 9. Tag Selector

```css
p {
    color: gray;
}
```

모든 `p` Element에 적용된다.

---

# 10. Class Selector

HTML:

```html
<div class="card">
    Card
</div>
```

CSS:

```css
.card {
    background: white;
}
```

Class 앞에는:

```text
.
```

을 붙인다.

---

# 11. ID Selector

HTML:

```html
<section id="portfolio">
</section>
```

CSS:

```css
#portfolio {
    padding: 100px;
}
```

ID 앞에는:

```text
#
```

을 붙인다.

---

# 12. 정리

```text
Tag
h1

Class
.card

ID
#portfolio
```

이다.

---

# 13. 여러 Element 선택

```css
h1,
h2,
h3 {
    font-weight: 700;
}
```

---

# 14. 자식 / 후손 Selector

HTML:

```html
<section class="service">

    <h2>Service</h2>

    <p>홈페이지 제작</p>

</section>
```

CSS:

```css
.service p {
    color: gray;
}
```

`.service` 안에 있는 `p`를 선택한다.

---

# 15. 직접 자식

```css
.service > p {
    color: gray;
}
```

`>`를 사용하면 직접 자식인 `p`를 선택한다.

---

# 16. CSS 우선순위

여러 CSS Rule이 같은 Element에 적용될 수 있다.

예:

```css
p {
    color: black;
}

.description {
    color: gray;
}
```

HTML:

```html
<p class="description">
    Hello
</p>
```

여기서는 Class Selector의 Style이 적용될 수 있다.

---

# 17. Specificity

CSS에는 어떤 Rule을 우선 적용할지 결정하는:

```text
Specificity
```

개념이 있다.

초보 단계에서는 대략:

```text
ID
>
Class
>
Tag
```

순으로 선택자가 더 구체적이라고 이해할 수 있다.

실제 CSS 우선순위에는 선언 위치, specificity, source order, `!important` 등도 영향을 준다.

---

# 18. `!important`

```css
.title {
    color: red !important;
}
```

처럼 강제로 우선순위를 높일 수 있다.

하지만 남발하면 CSS 관리가 어려워진다.

가능하면 Selector 구조를 올바르게 설계한다.

---

# 19. Color

```css
.title {
    color: red;
}
```

---

# 20. Hex Color

```css
.title {
    color: #111111;
}
```

---

# 21. RGB

```css
.title {
    color: rgb(255, 0, 0);
}
```

---

# 22. RGBA

```css
.overlay {
    background: rgba(0, 0, 0, 0.5);
}
```

마지막 값은 투명도이다.

---

# 23. Background

```css
section {
    background-color: #f5f5f5;
}
```

---

# 24. Background Image

```css
.hero {
    background-image: url("images/main.jpg");
}
```

---

# 25. 배경 Image 설정

```css
.hero {
    background-image: url("images/main.jpg");

    background-size: cover;

    background-position: center;

    background-repeat: no-repeat;
}
```

---

# 26. `background-size: cover`

Container를 배경 Image로 채운다.

필요하면 Image 일부가 잘릴 수 있다.

---

# 27. `background-position`

```css
background-position: center;
```

처럼 배경 Image의 기준 위치를 설정한다.

---

# 28. Font Size

```css
h1 {
    font-size: 60px;
}
```

---

# 29. Font Weight

```css
h1 {
    font-weight: 700;
}
```

예:

```text
400
→ 일반

700
→ 굵게
```

Font에 따라 지원하는 Weight가 다르다.

---

# 30. Line Height

```css
p {
    line-height: 1.7;
}
```

Text 줄 간격을 조절한다.

---

# 31. Letter Spacing

```css
h1 {
    letter-spacing: -1px;
}
```

글자 사이 간격을 조절한다.

---

# 32. Text Align

```css
.hero {
    text-align: center;
}
```

---

# 33. Box Model

CSS에서 매우 중요한 개념이다.

모든 Element를 하나의 Box라고 생각한다.

```text
┌───────────────────────────┐
│          Margin           │
│   ┌───────────────────┐   │
│   │      Border       │   │
│   │   ┌───────────┐   │   │
│   │   │  Padding  │   │   │
│   │   │ ┌───────┐ │   │   │
│   │   │ │Content│ │   │   │
│   │   │ └───────┘ │   │   │
│   │   └───────────┘   │   │
│   └───────────────────┘   │
└───────────────────────────┘
```

---

# 34. Content

실제 내용 영역이다.

예:

```text
Text

Image
```

---

# 35. Padding

Content와 Border 사이의 내부 여백이다.

```css
.card {
    padding: 30px;
}
```

---

# 36. Margin

Element 바깥쪽 여백이다.

```css
.card {
    margin: 20px;
}
```

---

# 37. Border

Element의 테두리이다.

```css
.card {
    border: 1px solid #ddd;
}
```

---

# 38. Border Radius

모서리를 둥글게 만든다.

```css
.card {
    border-radius: 20px;
}
```

---

# 39. Box Shadow

그림자를 만들 수 있다.

```css
.card {
    box-shadow:
        0 10px 30px rgba(0, 0, 0, 0.1);
}
```

---

# 40. Width

```css
.card {
    width: 400px;
}
```

---

# 41. Height

```css
.hero {
    height: 700px;
}
```

---

# 42. `max-width`

반응형 Web에서 매우 자주 사용한다.

```css
.container {
    max-width: 1200px;
}
```

화면이 커져도 Container가 지나치게 넓어지는 것을 막는다.

---

# 43. 가운데 정렬

```css
.container {
    max-width: 1200px;

    margin: 0 auto;
}
```

좌우 Margin을 `auto`로 설정하여 Block Container를 가운데 정렬할 수 있다.

---

# 44. `box-sizing`

많이 사용하는 초기 설정:

```css
* {
    box-sizing: border-box;
}
```

---

# 45. 왜 사용할까?

기본 Box Model에서는 Width 계산에 Padding과 Border 때문에 혼란이 생길 수 있다.

```text
border-box
```

를 사용하면 지정한 Width 안에 Padding과 Border가 포함되어 Layout 계산이 편해진다.

---

# 46. 기본 Reset 예

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
```

---

# 47. 주의

모든 기본 Margin과 Padding을 제거하므로 필요한 간격을 직접 다시 설정해야 한다.

---

# 48. Display

Element가 화면에서 어떻게 배치되는지를 결정하는 중요한 Property이다.

대표적으로:

```text
block

inline

inline-block

flex

grid

none
```

등이 있다.

---

# 49. Block

대표적으로:

```text
div

section

p
```

등은 기본적으로 Block 성격을 가진다.

일반적으로 한 줄의 영역을 차지한다.

---

# 50. Inline

대표적으로:

```text
a

span
```

등은 Inline 성격을 가진다.

Text 흐름 안에서 배치된다.

---

# 51. `display: none`

```css
.menu {
    display: none;
}
```

Element를 Layout에서 보이지 않게 할 수 있다.

JavaScript 메뉴 Toggle에서도 자주 볼 수 있다.

---

# 52. Flexbox

Web Layout에서 매우 중요하다.

HTML:

```html
<div class="cards">

    <div class="card">
        Card 1
    </div>

    <div class="card">
        Card 2
    </div>

    <div class="card">
        Card 3
    </div>

</div>
```

CSS:

```css
.cards {
    display: flex;
}
```

---

# 53. Flex의 핵심

```text
Parent
→ display: flex

Children
→ Flex Item
```

이다.

---

# 54. 가로 배치

기본적으로 Flex Item이 Main Axis 방향으로 배치된다.

```css
.cards {
    display: flex;
}
```

---

# 55. `gap`

Item 사이의 간격:

```css
.cards {
    display: flex;

    gap: 20px;
}
```

---

# 56. `justify-content`

Main Axis 방향의 배치를 조절한다.

```css
.cards {
    display: flex;

    justify-content: center;
}
```

---

# 57. 예

```text
flex-start

center

flex-end

space-between

space-around

space-evenly
```

등이 있다.

---

# 58. `align-items`

Cross Axis 방향 정렬을 조절한다.

```css
.cards {
    display: flex;

    align-items: center;
}
```

---

# 59. 가운데 정렬

```css
.hero {
    display: flex;

    justify-content: center;

    align-items: center;
}
```

Container 안의 Content를 가로·세로 가운데에 배치할 때 자주 사용한다.

---

# 60. Flex Direction

```css
.container {
    display: flex;

    flex-direction: column;
}
```

---

# 61. `row`

```text
→ → →
```

방향이다.

기본값이다.

---

# 62. `column`

```text
↓
↓
↓
```

방향으로 배치한다.

---

# 63. Flex Wrap

```css
.cards {
    display: flex;

    flex-wrap: wrap;
}
```

공간이 부족할 때 Item이 다음 줄로 넘어갈 수 있도록 한다.

---

# 64. `flex: 1`

```css
.card {
    flex: 1;
}
```

남은 공간을 Flex Item들이 나누어 사용할 때 자주 사용한다.

---

# 65. Flex를 이해하는 핵심

항상:

```text
누가 Parent인가?

누가 Child인가?
```

를 먼저 확인한다.

---

# 66. 잘못 생각하기 쉬운 부분

Child를 가로로 배치하고 싶은데 Child에:

```css
display: flex;
```

를 주는 것이 아니다.

그 Child들을 가지고 있는:

```text
Parent
```

에 Flex를 적용해야 한다.

---

# 67. CSS Grid

2차원 Layout을 만들 때 유용하다.

```css
.portfolio-grid {
    display: grid;

    grid-template-columns:
        repeat(3, 1fr);

    gap: 30px;
}
```

---

# 68. 의미

```text
3개의 동일한 Column
```

을 만든다.

---

# 69. `fr`

Grid에서 사용 가능한 공간의 비율을 나타낼 때 사용한다.

```text
1fr 1fr 1fr
```

이면 세 Column이 공간을 동일하게 나눠 가진다.

---

# 70. Flex vs Grid

간단하게:

```text
Flex
→ 한 방향 Layout에 강함

Grid
→ 행과 열을 함께 다루는 Layout에 강함
```

이라고 이해할 수 있다.

둘 중 하나만 사용해야 하는 것은 아니다.

---

# 71. Position

Element의 위치를 제어한다.

대표적으로:

```text
static

relative

absolute

fixed

sticky
```

가 있다.

---

# 72. `position: static`

기본적인 문서 흐름에 따라 배치된다.

---

# 73. `position: relative`

```css
.card {
    position: relative;
}
```

Element가 원래 자리 기준을 유지하면서 위치 기준점 역할을 할 수 있다.

---

# 74. `position: absolute`

```css
.badge {
    position: absolute;

    top: 10px;

    right: 10px;
}
```

---

# 75. Absolute 기준

가까운 조상 중 Position이 설정된 Element 등을 기준으로 위치가 정해질 수 있다.

그래서 자주:

```css
.card {
    position: relative;
}

.badge {
    position: absolute;

    top: 10px;

    right: 10px;
}
```

처럼 사용한다.

---

# 76. `position: fixed`

Viewport를 기준으로 고정할 때 사용할 수 있다.

```css
.contact-button {
    position: fixed;

    right: 30px;

    bottom: 30px;
}
```

---

# 77. Floating 문의 Button

화면을 Scroll해도 계속 같은 위치에 보이는 문의 Button 등을 만들 때 사용할 수 있다.

---

# 78. `position: sticky`

특정 Scroll 범위에서 고정되는 UI를 만들 때 사용할 수 있다.

```css
header {
    position: sticky;

    top: 0;
}
```

---

# 79. `z-index`

Element가 겹쳤을 때 쌓임 순서에 영향을 줄 수 있다.

```css
header {
    position: fixed;

    z-index: 1000;
}
```

단, `z-index`는 stacking context 등의 영향을 받으므로 단순히 숫자가 크다고 모든 상황에서 항상 위에 오는 것은 아니다.

---

# 80. Overflow

Container보다 Content가 커질 때 처리한다.

```css
.card {
    overflow: hidden;
}
```

---

# 81. Image 크기

```css
img {
    width: 100%;
}
```

처럼 Parent 크기에 맞출 수 있다.

---

# 82. Image 비율

```css
img {
    width: 100%;

    height: 300px;

    object-fit: cover;
}
```

---

# 83. `object-fit: cover`

지정된 영역을 Image로 채우면서 원본 비율을 유지한다.

일부 Image가 잘릴 수 있다.

---

# 84. Button Style

```css
.btn {
    padding: 14px 24px;

    border: none;

    border-radius: 8px;

    background: #111;

    color: white;

    cursor: pointer;
}
```

---

# 85. `cursor`

```css
cursor: pointer;
```

를 사용하면 마우스를 올렸을 때 Pointer Cursor를 표시한다.

---

# 86. Pseudo-class

특정 상태의 Element를 선택할 수 있다.

대표적으로:

```text
:hover

:focus

:active
```

등이 있다.

---

# 87. Hover

```css
.btn:hover {
    background: #333;
}
```

마우스를 올렸을 때 Style이 변경된다.

---

# 88. Transition

Style 변화가 부드럽게 보이도록 할 수 있다.

```css
.btn {
    transition: 0.3s;
}

.btn:hover {
    transform: translateY(-3px);
}
```

---

# 89. `transform`

Element의 위치, 크기, 회전 등을 시각적으로 변경할 수 있다.

```css
.card:hover {
    transform: scale(1.03);
}
```

---

# 90. Transition을 구체적으로 지정

```css
.card {
    transition:
        transform 0.3s ease;
}
```

처럼 어떤 Property를 Transition할지 지정하는 편이 관리에 좋다.

---

# 91. Opacity

```css
.overlay {
    opacity: 0.5;
}
```

Element 전체의 투명도를 조절한다.

---

# 92. Responsive Web

Desktop뿐 아니라:

```text
Tablet

Mobile
```

에서도 적절하게 보이도록 만드는 것을:

```text
Responsive Web Design
```

이라고 한다.

---

# 93. Media Query

화면 조건에 따라 CSS를 변경한다.

```css
@media (
    max-width: 768px
) {

    .cards {
        flex-direction: column;
    }

}
```

---

# 94. 의미

화면 너비가:

```text
768px 이하
```

일 때 내부 CSS를 적용한다.

---

# 95. Desktop

```text
Card  Card  Card
```

였다면 Mobile에서는:

```text
Card

Card

Card
```

처럼 변경할 수 있다.

---

# 96. 반응형에서 중요한 것

단순히 글자 크기만 줄이는 것이 아니다.

```text
Layout

Navigation

Image

Button

Padding

Font Size

Grid Column
```

등을 화면 크기에 맞게 조정한다.

---

# 97. Grid 반응형

Desktop:

```css
.portfolio-grid {
    display: grid;

    grid-template-columns:
        repeat(3, 1fr);
}
```

Mobile:

```css
@media (
    max-width: 768px
) {

    .portfolio-grid {
        grid-template-columns:
            1fr;
    }

}
```

---

# 98. 상대 단위

CSS에서는:

```text
px

%

rem

em

vw

vh
```

등 다양한 단위를 사용한다.

---

# 99. `px`

고정적인 Pixel 단위이다.

```css
font-size: 16px;
```

---

# 100. `%`

Parent 등의 기준에 상대적인 크기를 사용할 때 쓰인다.

```css
width: 100%;
```

---

# 101. `rem`

Root Element의 Font Size를 기준으로 하는 상대 단위이다.

```css
font-size: 2rem;
```

---

# 102. `vh`

Viewport Height를 기준으로 한다.

```css
.hero {
    min-height: 100vh;
}
```

화면 높이만큼 Hero Section을 만들 때 자주 볼 수 있다.

---

# 103. `vw`

Viewport Width를 기준으로 한다.

---

# 104. `min-height`

```css
.hero {
    min-height: 100vh;
}
```

처럼 최소 높이를 지정할 수 있다.

Content가 많아지면 필요한 만큼 더 커질 수 있다.

---

# 105. Header 예

HTML:

```html
<header class="header">

    <h1 class="logo">
        강남개발자들
    </h1>

    <nav class="nav">

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

# 106. Header CSS

```css
.header {
    display: flex;

    justify-content: space-between;

    align-items: center;

    padding: 20px 40px;
}
```

---

# 107. Navigation

```css
.nav {
    display: flex;

    gap: 30px;
}
```

---

# 108. Link Style

```css
.nav a {
    color: #111;

    text-decoration: none;
}
```

---

# 109. Hero HTML

```html
<section class="hero">

    <div class="hero-content">

        <h2>
            홈페이지 & 앱 제작
        </h2>

        <p>
            필요한 서비스를 함께 만듭니다.
        </p>

        <button class="btn">
            문의하기
        </button>

    </div>

</section>
```

---

# 110. Hero CSS

```css
.hero {
    min-height: 100vh;

    display: flex;

    justify-content: center;

    align-items: center;

    text-align: center;
}
```

---

# 111. Section 공통 Style

```css
.section {
    padding: 100px 20px;
}
```

---

# 112. Container Pattern

실제 홈페이지에서 자주 사용하는 구조이다.

HTML:

```html
<section class="section">

    <div class="container">

        Content

    </div>

</section>
```

CSS:

```css
.container {
    width: 100%;

    max-width: 1200px;

    margin: 0 auto;
}
```

---

# 113. 왜 이렇게 나눌까?

```text
section
→ 배경 / 위아래 공간

container
→ 실제 Content 최대 너비
```

를 각각 관리하기 좋다.

---

# 114. Card Layout

```css
.cards {
    display: grid;

    grid-template-columns:
        repeat(3, 1fr);

    gap: 30px;
}
```

---

# 115. Card

```css
.card {
    padding: 30px;

    border-radius: 20px;

    background: white;

    box-shadow:
        0 10px 30px rgba(0, 0, 0, 0.08);
}
```

---

# 116. Mobile

```css
@media (
    max-width: 768px
) {

    .header {
        padding: 16px 20px;
    }


    .nav {
        display: none;
    }


    .cards {
        grid-template-columns:
            1fr;
    }


    .hero h2 {
        font-size: 36px;
    }

}
```

---

# 117. CSS 전체 예제

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}


body {
    font-family:
        Arial,
        sans-serif;

    color: #111;

    background: #fff;
}


a {
    color: inherit;
    text-decoration: none;
}


img {
    display: block;
    max-width: 100%;
}


.header {
    display: flex;

    justify-content: space-between;

    align-items: center;

    padding: 20px 40px;
}


.logo {
    font-size: 24px;
}


.nav {
    display: flex;

    gap: 30px;
}


.hero {
    min-height: 100vh;

    display: flex;

    justify-content: center;

    align-items: center;

    padding: 40px 20px;

    text-align: center;

    background: #f5f5f5;
}


.hero h2 {
    font-size: 60px;

    margin-bottom: 20px;
}


.hero p {
    color: #666;

    margin-bottom: 30px;

    line-height: 1.7;
}


.btn {
    padding: 14px 24px;

    border: none;

    border-radius: 8px;

    background: #111;

    color: #fff;

    cursor: pointer;

    transition:
        transform 0.3s ease;
}


.btn:hover {
    transform:
        translateY(-3px);
}


.section {
    padding: 100px 20px;
}


.container {
    width: 100%;

    max-width: 1200px;

    margin: 0 auto;
}


.cards {
    display: grid;

    grid-template-columns:
        repeat(3, 1fr);

    gap: 30px;
}


.card {
    padding: 30px;

    border-radius: 20px;

    background: #fff;

    box-shadow:
        0 10px 30px rgba(0, 0, 0, 0.08);
}


@media (
    max-width: 768px
) {

    .header {
        padding: 16px 20px;
    }


    .nav {
        display: none;
    }


    .hero h2 {
        font-size: 36px;
    }


    .cards {
        grid-template-columns:
            1fr;
    }

}
```

---

# 118. CSS를 작성하는 순서

처음부터 세부 디자인을 모두 넣지 않는다.

```text
1. 전체 Reset

2. Body

3. 큰 Layout

4. Header

5. Section

6. Container

7. Card

8. Text

9. Button

10. Hover / Animation

11. Responsive
```

처럼 큰 구조부터 작성하면 관리하기 쉽다.

---

# 119. Layout Debugging

화면 배치가 이상하면 먼저 Parent를 확인한다.

```text
Parent의 display는?

flex인가?

grid인가?

width는?

height는?

padding은?

Child width는?
```

를 확인한다.

---

# 120. 임시 Border

Layout을 확인할 때:

```css
* {
    outline: 1px solid red;
}
```

같은 방법으로 각 Box 영역을 눈으로 확인할 수 있다.

Debugging 후 제거한다.

---

# 121. Developer Tools

Chrome의 Elements에서 Element를 선택하면:

```text
Styles

Computed

Box Model
```

등을 확인할 수 있다.

---

# 122. CSS가 적용되지 않을 때

다음 순서로 확인한다.

```text
1. CSS File을 저장했는가?

2. HTML에서 CSS를 연결했는가?

3. href 경로가 맞는가?

4. Selector가 맞는가?

5. 다른 CSS가 덮어쓰고 있는가?

6. Developer Tools에서 Rule이 보이는가?
```

---

# 123. Path 확인

HTML:

```html
<link
    rel="stylesheet"
    href="style.css"
>
```

인데 실제 File이:

```text
css/style.css
```

에 있다면 적용되지 않는다.

이 경우:

```html
<link
    rel="stylesheet"
    href="css/style.css"
>
```

처럼 맞춰야 한다.

---

# 124. 취소선

Developer Tools의 Styles에서 CSS Property에 취소선이 있다면 다른 Rule에 의해 덮어쓰였을 가능성이 있다.

---

# 125. CSS를 무작정 추가하지 않는다

화면이 안 맞는다고:

```text
margin 추가

padding 추가

position absolute 추가

!important 추가
```

를 계속 반복하면 구조가 복잡해질 수 있다.

---

# 126. 먼저 구조 확인

```text
HTML Parent / Child

Flex / Grid

Box Model

Width

Padding

Position
```

을 먼저 확인한다.

---

# 127. Absolute 남발 주의

모든 Element를:

```css
position: absolute;
```

로 배치하면 특정 화면에서는 맞아 보여도 화면 크기가 바뀌면 쉽게 깨질 수 있다.

---

# 128. 일반 Layout

먼저:

```text
Normal Flow

Flex

Grid
```

를 사용하고 정말 필요한 UI에 Position을 사용한다.

---

# 129. 반응형의 핵심

Desktop 디자인을 그대로 작게 축소하는 것이 아니다.

화면 크기에 따라 Layout 자체를 바꿀 수 있다.

---

# 130. 예

Desktop:

```text
Logo        HOME SERVICE PORTFOLIO
```

Mobile:

```text
Logo                       ☰
```

처럼 Navigation 구조 자체가 달라질 수 있다.

---

# 131. JavaScript와 연결

Mobile Menu는 CSS만으로 숨겨두고 JavaScript로 Class를 추가해서 표시할 수 있다.

예:

```css
.mobile-menu {
    display: none;
}


.mobile-menu.active {
    display: block;
}
```

---

# 132. JavaScript

나중에:

```javascript
menu.classList.toggle(
    "active"
);
```

처럼 `active` Class를 추가하거나 제거한다.

---

# 133. 역할 분리

```text
HTML
→ Menu 구조

CSS
→ Menu 디자인과 active 상태

JavaScript
→ 클릭했을 때 active 변경
```

이다.

---

# 134. CSS와 Animation

CSS만으로 간단한 Animation을 만들 수도 있다.

```css
@keyframes fadeIn {

    from {
        opacity: 0;
    }

    to {
        opacity: 1;
    }

}
```

---

# 135. 적용

```css
.hero-content {
    animation:
        fadeIn 1s ease;
}
```

---

# 136. Animation 남용 주의

Animation은 사용자 경험을 돕는 수준으로 사용하는 것이 좋다.

너무 많은 움직임은:

```text
가독성

성능

사용성
```

을 떨어뜨릴 수 있다.

---

# 137. CSS Framework

CSS를 직접 작성하는 방법 외에도:

```text
Tailwind CSS

Bootstrap
```

같은 도구가 있다.

---

# 138. Tailwind CSS

예:

```html
<button
    class="
        px-6
        py-3
        rounded-lg
        bg-black
        text-white
    "
>
    문의하기
</button>
```

처럼 Utility Class를 조합하여 Style을 적용한다.

---

# 139. 하지만 기본 CSS가 중요한 이유

Tailwind를 사용하더라도:

```text
Flex

Grid

Padding

Margin

Position

Responsive

Box Model
```

개념은 그대로 사용된다.

---

# 140. CSS 학습 순서

```text
Selector
↓
Color / Font
↓
Box Model
↓
Width / Height
↓
Display
↓
Flex
↓
Grid
↓
Position
↓
Responsive
↓
Transition / Animation
```

순으로 이해하면 좋다.

---

# 141. 가장 중요한 개념 1

```text
HTML Element는 Box이다.
```

따라서:

```text
Content
Padding
Border
Margin
```

을 이해해야 한다.

---

# 142. 가장 중요한 개념 2

```text
Layout은 Parent가 중요하다.
```

특히 Flex와 Grid에서는:

```text
Parent
↓
Children 배치
```

관계를 먼저 본다.

---

# 143. 가장 중요한 개념 3

```text
Desktop에서 맞는 것
≠
Mobile에서도 맞는 것
```

이다.

그래서 Responsive Design이 필요하다.

---

# 144. HTML + CSS

HTML:

```html
<section class="service">

    <div class="container">

        <h2>
            Service
        </h2>


        <div class="cards">

            <article class="card">

                <h3>
                    홈페이지 제작
                </h3>

                <p>
                    기업과 브랜드를 위한
                    반응형 홈페이지를 제작합니다.
                </p>

            </article>


            <article class="card">

                <h3>
                    앱 제작
                </h3>

                <p>
                    필요한 기능을 중심으로
                    앱 서비스를 제작합니다.
                </p>

            </article>

        </div>

    </div>

</section>
```

CSS:

```css
.service {
    padding:
        100px 20px;
}


.container {
    max-width: 1200px;

    margin:
        0 auto;
}


.cards {
    display: grid;

    grid-template-columns:
        repeat(2, 1fr);

    gap: 30px;
}


.card {
    padding: 40px;

    border-radius: 20px;

    background: #f5f5f5;
}


@media (
    max-width: 768px
) {

    .cards {
        grid-template-columns:
            1fr;
    }

}
```

---

# 145. Browser가 보는 흐름

```text
HTML
↓
Element 구조 생성
↓
CSS File 읽기
↓
Selector Matching
↓
Style 계산
↓
Layout 계산
↓
화면 Rendering
```

으로 생각할 수 있다.

---

# 146. CSS의 역할을 한 문장으로 정리

CSS는:

```text
HTML Element가
화면에서 어떤 크기, 색상, 간격, 위치,
Layout과 반응형 형태로 표시될지를 정의하는
Style Language
```

이다.

---

# 147. 핵심 정리

```text
Selector
→ 무엇을 꾸밀 것인가?

Property
→ 무엇을 변경할 것인가?

Value
→ 어떻게 변경할 것인가?


Box Model
→ Content / Padding / Border / Margin

Flex
→ 한 방향 Layout

Grid
→ 행과 열 Layout

Position
→ 특수한 위치 배치

Media Query
→ 반응형

Transition
→ 상태 변화 효과

Animation
→ 움직임
```

을 기억한다.

---

# 148. HTML / CSS 관계

```text
HTML
↓
구조

CSS
↓
디자인 + Layout

JavaScript
↓
동작
```

이다.

다음 단계에서는 JavaScript를 이용해 정적인 HTML/CSS Page에 실제 동작을 추가한다.