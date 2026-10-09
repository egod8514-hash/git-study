# VS Code 기본 사용법

## 1. VS Code란?

Visual Studio Code(VS Code)는 코드를 작성하고 실행하는 데 사용하는 코드 에디터이다.

Python, HTML, CSS, JavaScript 등 다양한 언어를 사용할 수 있으며 Extension을 설치하여 필요한 기능을 추가할 수 있다.

---

## 2. VS Code의 기본 화면

VS Code는 크게 다음과 같은 영역으로 구성되어 있다.

- Explorer : 프로젝트의 파일과 폴더 확인
- Editor : 코드 작성
- Terminal : 명령어 실행
- Extensions : 확장 프로그램 설치 및 관리
- Source Control : Git 변경사항 확인 및 관리

---

## 3. 프로젝트 폴더 열기

VS Code에서는 파일 하나만 여는 것보다 프로젝트 폴더 전체를 열어 작업하는 경우가 많다.

터미널에서 현재 폴더를 VS Code로 열려면:

```powershell
code .
```

`.`은 현재 폴더를 의미한다.

특정 파일만 열 수도 있다.

```powershell
code README.md
```

---

## 4. VS Code 터미널

VS Code 안에서도 터미널을 사용할 수 있다.

단축키:

```text
Ctrl + `
```

터미널에서는 Git 명령어, Python 실행, 패키지 설치 등 다양한 작업을 할 수 있다.

예:

```powershell
git status
python --version
```

---

## 5. 파일의 언어 모드

VS Code는 파일 확장자를 이용하여 어떤 언어의 파일인지 판단한다.

예:

```text
main.py       → Python
index.html    → HTML
style.css     → CSS
script.js     → JavaScript
README.md     → Markdown
```

VS Code 오른쪽 아래에서도 현재 파일의 언어 모드를 확인할 수 있다.

파일이 잘못된 언어로 인식되면 자동완성이나 문법 강조 기능이 제대로 동작하지 않을 수 있다.

---

## 6. Extension

VS Code는 Extension을 설치하여 기능을 추가할 수 있다.

개발하면서 사용한 대표적인 Extension은 다음과 같다.

### Python

VS Code에서 Python 코드를 작성하고 실행할 수 있도록 도와준다.

### Jupyter

VS Code에서 `.ipynb` Jupyter Notebook을 사용할 수 있도록 도와준다.

Git과 GitHub 관련 기능도 VS Code에서 사용할 수 있다.

---

## 7. Python Interpreter

컴퓨터에 여러 Python 환경이 설치되어 있으면 어떤 Python을 사용할 것인지 선택해야 한다.

VS Code에서는 Python Interpreter를 선택하여 현재 프로젝트에서 사용할 Python 환경을 지정할 수 있다.

예:

```text
Python 3.x
ml_env
base
```

잘못된 Interpreter를 선택하면 설치했던 라이브러리를 찾지 못하는 문제가 발생할 수 있다.

예:

```text
ModuleNotFoundError
```

따라서 Python 관련 오류가 발생하면 현재 선택된 Interpreter가 올바른지 확인하는 것이 중요하다.

---

## 8. Jupyter Notebook Kernel

Jupyter Notebook에서는 Interpreter와 함께 Kernel을 선택하게 된다.

Kernel은 Notebook의 Python 코드를 실제로 실행하는 환경이라고 이해할 수 있다.

예:

```text
mission3.ipynb
      ↓
ml_env Kernel
      ↓
Python 3.10
      ↓
코드 실행
```

Kernel 연결에 문제가 있으면 셀을 실행해도 계속 실행 중이거나 다음과 같은 상태가 나타날 수 있다.

```text
Connecting to kernel...
```

이 경우 Python 자체의 문제인지 VS Code/Jupyter의 문제인지 구분하여 확인해야 한다.

---

## 9. Command Palette

VS Code에서는 Command Palette를 이용하여 여러 기능을 검색하고 실행할 수 있다.

단축키:

```text
Ctrl + Shift + P
```

예를 들어 다음과 같은 기능을 검색할 수 있다.

```text
Developer: Reload Window
Python: Select Interpreter
Preferences: Open User Settings
```

메뉴의 위치를 모를 때 Command Palette에서 기능 이름을 검색하면 편리하다.

---

## 10. 자동완성과 Emmet

HTML을 작성할 때 VS Code의 Emmet 기능을 이용하면 태그를 빠르게 작성할 수 있다.

예를 들어:

```text
div
```

을 Emmet으로 확장하면:

```html
<div></div>
```

가 생성된다.

또한:

```text
!
```

을 Emmet으로 확장하면 HTML의 기본 구조를 빠르게 만들 수 있다.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>

</body>
</html>
```

---

## 11. 주석 단축키

VS Code에서는:

```text
Ctrl + /
```

를 이용하여 현재 언어에 맞는 주석을 만들거나 해제할 수 있다.

예를 들어 Python에서는:

```python
# Python 주석
```

HTML에서는:

```html
<!-- HTML 주석 -->
```

JavaScript에서는:

```javascript
// JavaScript 주석
```

처럼 언어에 맞는 주석 문법이 적용된다.

---

## 12. Markdown 미리보기

`.md` 파일은 Markdown 문서이다.

VS Code에서는 다음 단축키로 Markdown 결과를 미리 확인할 수 있다.

```text
Ctrl + Shift + V
```

이를 이용하면 GitHub에 Push하기 전에 제목, 코드 블록, 표 등이 제대로 작성되었는지 확인할 수 있다.

---

## 13. 오류가 발생했을 때 확인할 것

VS Code에서 코드가 실행되지 않는다고 해서 항상 코드 자체가 잘못된 것은 아니다.

다음과 같은 부분도 확인해야 한다.

1. 올바른 프로젝트 폴더를 열었는지
2. 파일을 저장했는지
3. 현재 파일의 언어 모드가 올바른지
4. Python Interpreter가 올바른지
5. Jupyter Kernel이 올바른지
6. 필요한 Extension이 설치되어 있는지
7. 터미널의 현재 위치가 올바른지

---

## 14. 배운 점

VS Code는 단순히 코드를 입력하는 프로그램이 아니라 파일 관리, 터미널, Git, Python 환경, Jupyter Notebook 등을 함께 사용할 수 있는 개발 도구라는 것을 학습했다.

특히 Python이나 Jupyter 코드가 실행되지 않을 때 코드만 확인하는 것이 아니라 Interpreter, Kernel, Extension, 현재 작업 경로 등 개발 환경도 함께 확인해야 한다는 것을 알게 되었다.