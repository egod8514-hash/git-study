# GitHub 기초와 협업

## 1. GitHub란?

GitHub는 Git Repository를 인터넷에 저장하고 다른 개발자와 공유하거나 협업할 수 있도록 제공하는 서비스이다.

Git이 내 컴퓨터에서 버전을 관리하는 도구라면 GitHub는 Git Repository를 원격으로 저장하고 협업할 수 있도록 도와주는 서비스이다.

```text
내 컴퓨터
Local Repository
      │
      │ push / pull
      ▼
GitHub
Remote Repository
```

GitHub에서는 단순히 코드를 저장하는 것뿐만 아니라 다음과 같은 기능도 사용할 수 있다.

- Repository
- Branch
- Issue
- Pull Request
- Code Review
- 프로젝트 협업

---

## 2. Git과 GitHub의 역할

Git:

```text
코드 변경
   ↓
git add
   ↓
git commit
   ↓
버전 기록
```

GitHub:

```text
Git Commit
   ↓
git push
   ↓
GitHub Repository
   ↓
공유 / 협업 / 리뷰
```

따라서 Git과 GitHub는 서로 연결되어 있지만 같은 것은 아니다.

---

## 3. GitHub Repository

GitHub에서 프로젝트를 관리하는 기본 단위가 Repository이다.

하나의 프로젝트를 하나의 Repository로 관리할 수 있다.

예:

```text
git-study
shopping-mall
medical-ai-project
chest-xray-classification
```

Repository 안에는 프로젝트 파일과 Git Commit 기록 등이 저장된다.

---

## 4. Public Repository

Public Repository는 다른 사용자가 Repository를 볼 수 있도록 공개된 저장소이다.

예를 들어 학습 기록이나 공개 프로젝트, 포트폴리오 등을 Public Repository로 관리할 수 있다.

```text
Public Repository
        ↓
다른 사람이 코드 확인 가능
```

단, 공개 Repository에는 비밀번호, API Key, 개인정보와 같은 민감한 정보를 올리지 않도록 주의해야 한다.

---

## 5. Private Repository

Private Repository는 접근 권한이 있는 사용자만 볼 수 있는 저장소이다.

회사 프로젝트나 외부에 공개하면 안 되는 코드 등을 관리할 때 사용할 수 있다.

```text
Private Repository
        ↓
권한이 있는 사용자만 접근
```

---

## 6. README.md

GitHub Repository에서 `README.md`는 프로젝트를 설명하는 대표적인 문서이다.

보통 다음과 같은 내용을 작성할 수 있다.

```text
프로젝트 이름
프로젝트 설명
사용 기술
설치 방법
실행 방법
폴더 구조
주요 기능
결과
```

현재 학습 Repository에서도 `README.md`를 이용하여 전체 공부 내용을 안내할 수 있다.

---

## 7. Issue란?

Issue는 프로젝트에서 해야 할 일, 버그, 개선사항, 질문 등을 기록하고 관리하는 기능이다.

예를 들어 쇼핑몰을 개발한다고 가정한다.

다음과 같은 Issue를 만들 수 있다.

```text
로그인 기능 구현
회원가입 기능 구현
상품 목록 페이지 구현
결제 오류 수정
모바일 화면 수정
```

즉 Issue는:

> 프로젝트에서 무엇을 해야 하는가?

를 기록하고 관리하는 역할을 한다.

---

## 8. Issue를 사용하는 이유

개발해야 할 내용을 기억에만 의존하면 관리하기 어렵다.

Issue를 이용하면:

```text
해야 할 작업
    ↓
Issue 생성
    ↓
담당자 지정
    ↓
개발 진행
    ↓
완료
```

처럼 작업을 관리할 수 있다.

팀 프로젝트에서는 어떤 개발자가 어떤 작업을 담당하는지도 확인하기 쉬워진다.

---

## 9. Issue와 Branch 연결

Issue를 만들었다면 해당 작업을 위한 Branch를 만들 수 있다.

예를 들어 Issue가:

```text
로그인 기능 구현
```

이라면 Branch를:

```text
feature/login
```

처럼 만들 수 있다.

전체 흐름:

```text
Issue
로그인 기능 구현
      ↓
Branch 생성
feature/login
      ↓
코드 작성
      ↓
Commit
```

이렇게 하면 어떤 작업을 위해 Branch가 만들어졌는지 이해하기 쉬워진다.

---

## 10. Pull Request란?

Pull Request는 보통 줄여서 `PR`이라고 부른다.

Branch에서 작업한 내용을 다른 Branch에 반영하기 전에 검토를 요청하는 GitHub 기능이다.

예:

```text
feature/login
      ↓
로그인 기능 개발 완료
      ↓
GitHub에 Push
      ↓
Pull Request
      ↓
main에 합쳐도 되는지 검토
```

---

## 11. Pull Request가 필요한 이유

혼자 개발한다면 바로 Merge할 수도 있다.

하지만 여러 개발자가 함께 작업할 때는 다른 사람의 코드가 바로 `main`에 들어가면 문제가 발생할 수 있다.

따라서:

```text
개발자 A
로그인 기능 개발
      ↓
Pull Request 생성
      ↓
개발자 B가 코드 확인
      ↓
문제 없음
      ↓
Merge
```

와 같은 과정을 사용할 수 있다.

---

## 12. Pull Request와 git pull은 다르다

이름에 모두 `pull`이 들어가기 때문에 처음에는 혼동할 수 있다.

### git pull

Git 명령어이다.

```bash
git pull
```

Remote Repository의 변경사항을 Local Repository로 가져온다.

```text
GitHub
   ↓
git pull
   ↓
내 컴퓨터
```

### Pull Request

GitHub의 협업 기능이다.

```text
내 Branch의 변경사항
        ↓
다른 Branch에 합쳐주세요
        ↓
검토 요청
```

따라서 둘은 전혀 다른 역할을 한다.

---

## 13. Code Review

Pull Request가 만들어지면 다른 개발자가 코드를 확인할 수 있다.

이 과정을 Code Review라고 한다.

Code Review에서는 다음과 같은 부분을 확인할 수 있다.

```text
코드가 정상적으로 동작하는가?
버그가 있는가?
코드가 이해하기 쉬운가?
중복 코드가 너무 많지는 않은가?
프로젝트 규칙을 지켰는가?
보안 문제가 없는가?
```

필요하면 코드의 특정 부분에 의견을 남길 수도 있다.

---

## 14. 좋은 코드와 Code Review

개발에서는 단순히 실행되는 코드라고 해서 항상 좋은 코드라고 할 수는 없다.

예를 들어 같은 기능을 수행하더라도:

```text
이름이 명확한 변수
이해하기 쉬운 함수
중복이 적은 코드
역할이 잘 분리된 코드
일관된 코드 스타일
필요한 설명이 있는 코드
```

가 유지보수하기 더 쉽다.

Code Review는 이러한 부분을 팀원들과 확인하는 과정이기도 하다.

---

## 15. Merge

Pull Request의 코드가 문제가 없다고 판단되면 대상 Branch에 Merge할 수 있다.

예:

```text
feature/login
      │
      │ Pull Request
      ▼
Code Review
      │
      │ 승인
      ▼
Merge
      │
      ▼
main
```

Merge가 완료되면 로그인 기능이 `main`에 포함된다.

---

## 16. 전체 GitHub 협업 흐름

새로운 기능을 개발한다고 가정한다.

```text
1. Issue 생성
       ↓
2. Branch 생성
       ↓
3. 기능 개발
       ↓
4. git add
       ↓
5. git commit
       ↓
6. git push
       ↓
7. Pull Request 생성
       ↓
8. Code Review
       ↓
9. Merge
       ↓
10. Branch 정리
```

이 흐름을 이해하면 Git과 GitHub의 역할도 구분하기 쉬워진다.

---

## 17. 예제 - 로그인 기능 개발

### 1. Issue

GitHub에서:

```text
로그인 기능 구현
```

Issue를 만든다.

### 2. Branch

```bash
git switch -c feature/login
```

### 3. 코드 작성

로그인 기능을 구현한다.

### 4. 변경사항 확인

```bash
git status
```

### 5. Staging

```bash
git add .
```

### 6. Commit

```bash
git commit -m "Add login feature"
```

### 7. GitHub에 Push

처음 Push하는 Branch라면:

```bash
git push -u origin feature/login
```

### 8. Pull Request

GitHub에서:

```text
feature/login
      ↓
main
```

으로 Pull Request를 만든다.

### 9. Code Review

팀원이 코드를 확인한다.

### 10. Merge

문제가 없다면 `main`에 Merge한다.

---

## 18. Branch를 바로 main에 Push하는 것과 PR의 차이

혼자 공부할 때는:

```text
main에서 작업
   ↓
commit
   ↓
push
```

처럼 사용할 수도 있다.

하지만 협업에서는:

```text
main
 ↓
feature Branch
 ↓
개발
 ↓
Push
 ↓
Pull Request
 ↓
Review
 ↓
Merge
```

와 같은 흐름을 사용할 수 있다.

이렇게 하면 `main`에 들어가기 전에 변경사항을 검토할 수 있다.

---

## 19. Repository를 관리할 때 주의할 점

GitHub에 모든 파일을 무조건 올리는 것은 좋지 않다.

예를 들어 다음과 같은 파일은 공개 Repository에 올리지 않도록 주의해야 한다.

```text
비밀번호
API Key
Access Token
개인정보
환경설정의 민감한 값
```

필요한 경우 `.gitignore`를 이용하여 Git이 특정 파일을 추적하지 않도록 설정할 수 있다.

예:

```gitignore
.env
__pycache__/
.venv/
```

---

## 20. .gitignore

`.gitignore`는 Git이 추적하지 않아야 하는 파일이나 폴더를 지정하는 파일이다.

예를 들어 Python 프로젝트에서는:

```gitignore
__pycache__/
*.pyc
.venv/
.env
```

등을 지정할 수 있다.

머신러닝이나 딥러닝 프로젝트에서는 데이터나 모델 파일의 크기가 매우 클 수 있으므로 어떤 파일을 Git으로 관리할 것인지 판단하는 것도 중요하다.

---

## 21. GitHub에서 Commit 확인

Push한 Commit은 GitHub Repository에서도 확인할 수 있다.

예:

```text
Add Git study README
Add GitHub study notes
Add development environment notes
```

Commit 기록을 보면 프로젝트가 어떤 과정을 거쳐 변경되었는지 확인할 수 있다.

따라서 Commit 메시지를 이해하기 쉽게 작성하는 것이 중요하다.

---

## 22. 학습 Repository 관리 방법

현재 `git-study` Repository에서는 공부한 내용을 단원별로 Markdown 파일로 정리하고 있다.

예:

```text
01-development-environment
02-git-github
03-python
04-data-analysis
...
```

각 단원을 작성한 후:

```bash
git add 단원폴더
git commit -m "단원 작업 내용"
```

으로 기록한다.

그리고 여러 Local Commit을:

```bash
git push
```

하여 GitHub에 반영할 수 있다.

이렇게 하면 GitHub의 Commit 기록 자체가 학습 과정이 된다.

---

## 23. Git과 GitHub 전체 관계

지금까지 배운 내용을 하나로 연결하면 다음과 같다.

```text
해야 할 작업
    ↓
GitHub Issue
    ↓
Branch 생성
    ↓
코드 작성
    ↓
git status
    ↓
git add
    ↓
git commit
    ↓
Local Repository
    ↓
git push
    ↓
GitHub Repository
    ↓
Pull Request
    ↓
Code Review
    ↓
Merge
    ↓
main
```

그리고 다른 개발자는 최신 내용을:

```bash
git pull
```

하여 자신의 Local Repository에 반영할 수 있다.

---

## 24. 배운 점

GitHub는 단순히 코드를 업로드하는 저장 공간이 아니라 Git을 기반으로 프로젝트를 공유하고 협업할 수 있는 플랫폼이라는 것을 학습했다.

Repository에서 코드를 관리하고 Issue를 이용하여 해야 할 작업을 기록하며 Branch를 만들어 기능을 독립적으로 개발할 수 있다.

개발이 완료되면 Pull Request를 이용하여 변경사항을 검토받고 Code Review를 거친 후 Merge할 수 있다.

전체 협업 흐름을 정리하면:

```text
Issue
 ↓
Branch
 ↓
Development
 ↓
Commit
 ↓
Push
 ↓
Pull Request
 ↓
Code Review
 ↓
Merge
```

라고 이해할 수 있다.

또한 `git pull`과 GitHub의 `Pull Request`는 이름은 비슷하지만 서로 다른 기능이라는 점도 학습했다.