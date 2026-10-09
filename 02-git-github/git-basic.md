# Git 기초

## 1. Git이란?

Git은 파일의 변경 이력을 관리하는 버전 관리 시스템(Version Control System)이다.

개발을 하다 보면 코드를 계속 수정하게 된다.

예를 들어:

```text
처음 코드
   ↓
로그인 기능 추가
   ↓
회원가입 기능 추가
   ↓
버그 수정
   ↓
디자인 수정
```

Git을 사용하면 이러한 변경 과정을 기록할 수 있다.

단순히 최신 파일만 저장하는 것이 아니라 과거에 어떤 내용을 변경했는지도 확인할 수 있다.

---

## 2. Git을 사용하는 이유

Git을 사용하는 대표적인 이유는 다음과 같다.

### 변경 기록 관리

코드를 언제, 어떻게 변경했는지 기록할 수 있다.

### 이전 버전 확인

문제가 발생했을 때 과거의 코드와 비교할 수 있다.

### Branch

기존 코드에 영향을 주지 않고 새로운 기능을 따로 개발할 수 있다.

### 협업

여러 개발자가 하나의 프로젝트를 함께 개발할 수 있다.

---

## 3. Git과 GitHub의 차이

Git과 GitHub는 같은 것이 아니다.

### Git

내 컴퓨터에서 파일의 변경사항과 버전을 관리하는 도구이다.

```text
내 컴퓨터
    ↓
   Git
    ↓
버전 관리
```

### GitHub

Git으로 관리하는 Repository를 인터넷에 저장하고 공유할 수 있는 서비스이다.

```text
내 컴퓨터
    ↓
   Git
    ↓
 GitHub
```

따라서 인터넷이 없어도 Git 자체는 사용할 수 있다.

GitHub는 Git Repository를 다른 개발자와 공유하거나 협업할 때 사용할 수 있다.

---

## 4. Repository란?

Repository는 Git이 프로젝트의 파일과 변경 이력을 관리하는 저장소이다.

보통 줄여서 `repo`라고 부르기도 한다.

예를 들어:

```text
my-project/
├── index.html
├── style.css
├── script.js
└── .git/
```

Git Repository로 만들어지면 `.git`이라는 숨김 폴더가 생성된다.

이 `.git` 폴더 안에 Git의 변경 기록과 Repository 정보가 저장된다.

---

## 5. Git 사용자 정보 설정

Git을 처음 사용할 때 Commit을 누가 만들었는지 기록하기 위해 사용자 이름과 이메일을 설정한다.

```bash
git config --global user.name "사용자이름"
```

```bash
git config --global user.email "이메일"
```

현재 설정을 확인하려면:

```bash
git config --list
```

`--global`은 현재 컴퓨터에서 사용하는 Git의 전역 설정이라는 의미이다.

---

## 6. git init

일반 폴더를 Git Repository로 만들 때 사용한다.

```bash
git init
```

예:

```text
project/
```

에서:

```bash
git init
```

을 실행하면 해당 폴더가 Git Repository가 된다.

개념적으로는:

```text
일반 폴더
   ↓
git init
   ↓
Git Repository
```

이다.

---

## 7. main Branch

Git Repository를 만들면 기본 Branch에서 작업하게 된다.

현재 많이 사용하는 기본 Branch 이름은:

```text
main
```

이다.

현재 Branch 이름을 `main`으로 변경하려면:

```bash
git branch -M main
```

새 Repository의 기본 Branch 이름을 앞으로 `main`으로 사용하도록 설정할 수도 있다.

```bash
git config --global init.defaultBranch main
```

---

## 8. git status

현재 Git Repository의 상태를 확인하는 명령어이다.

```bash
git status
```

Git을 사용할 때 매우 자주 사용하는 명령어이다.

예를 들어:

```text
Untracked files:
    study.md
```

가 나온다면 Git이 `study.md` 파일의 존재는 확인했지만 아직 추적하고 있지 않다는 의미이다.

---

## 9. Untracked File

새로운 파일을 만들었다고 Git이 바로 Commit 대상으로 관리하는 것은 아니다.

예를 들어:

```text
study.md
```

를 새로 만들고:

```bash
git status
```

를 실행하면:

```text
Untracked files:
    study.md
```

라고 나타날 수 있다.

즉:

```text
파일 생성
   ↓
Git이 파일 발견
   ↓
Untracked
```

상태이다.

---

## 10. git add

변경된 파일을 다음 Commit에 포함하기 위해 Staging Area에 올리는 명령어이다.

특정 파일 하나:

```bash
git add study.md
```

특정 폴더:

```bash
git add 01-development-environment
```

현재 위치의 모든 변경사항:

```bash
git add .
```

---

## 11. Staging Area

`git add`를 이해하려면 Staging Area를 이해하는 것이 중요하다.

Git의 기본 흐름은:

```text
Working Directory
      ↓
   git add
      ↓
Staging Area
      ↓
  git commit
      ↓
Repository
```

### Working Directory

현재 실제로 파일을 작성하고 수정하는 공간이다.

### Staging Area

다음 Commit에 포함할 변경사항을 준비하는 공간이다.

### Repository

Commit된 변경 기록이 저장되는 공간이다.

---

## 12. git add 전과 후

새로운 파일을 만들고:

```bash
git status
```

를 실행했을 때:

```text
Untracked files:
    study.md
```

라고 나올 수 있다.

그다음:

```bash
git add study.md
```

를 실행하고 다시:

```bash
git status
```

를 확인하면:

```text
Changes to be committed:

    new file: study.md
```

로 변경된다.

즉 `git add`를 통해 다음 Commit에 들어갈 파일을 선택한 것이다.

---

## 13. git commit

Staging Area에 있는 변경사항을 하나의 버전으로 기록한다.

```bash
git commit -m "커밋 메시지"
```

예:

```bash
git commit -m "Add Git study notes"
```

`-m` 뒤에는 해당 Commit에서 무엇을 변경했는지 설명하는 메시지를 작성한다.

---

## 14. Commit이란?

Commit은 프로젝트의 특정 변경사항을 하나의 기록으로 저장한 것이다.

예:

```text
Commit 1
Add README

      ↓

Commit 2
Add login feature

      ↓

Commit 3
Fix login bug
```

이렇게 Commit을 계속 만들면 프로젝트가 어떻게 변경되었는지 기록으로 남는다.

---

## 15. 좋은 Commit 메시지

Commit 메시지를 보면 어떤 작업을 했는지 어느 정도 알 수 있도록 작성하는 것이 좋다.

예:

```text
Add login feature
Fix login error
Update README
Add machine learning notes
```

다음과 같이 의미가 불분명한 메시지보다는:

```text
test
aaa
수정
123
```

작업 내용을 알 수 있는 메시지를 사용하는 것이 좋다.

---

## 16. git log

Commit 기록을 확인할 때 사용한다.

```bash
git log
```

Commit을 간단하게 확인하려면:

```bash
git log --oneline
```

예:

```text
7349ba7 Add development environment notes
1acdec0 Add GitHub study notes
383f7e6 Add Git study README
```

앞부분은 Commit을 구분하는 Commit ID이고 뒤에는 Commit 메시지이다.

---

## 17. HEAD

Git에서 `HEAD`는 현재 내가 보고 있거나 작업하고 있는 Commit을 가리킨다.

예:

```text
A
↓
B
↓
C  ← HEAD
```

현재 C Commit에서 작업하고 있다는 의미이다.

Branch를 함께 보면:

```text
A
↓
B
↓
C  ← main ← HEAD
```

처럼 이해할 수 있다.

---

## 18. Git의 기본 작업 흐름

가장 기본적인 Git 사용 과정은 다음과 같다.

```text
파일 작성 또는 수정
        ↓
    git status
        ↓
     git add
        ↓
    git status
        ↓
    git commit
        ↓
     git log
```

실제로 명령어로 보면:

```bash
git status

git add README.md

git status

git commit -m "Update README"

git log --oneline
```

이다.

---

## 19. 실제로 사용한 과정

Git 학습 내용을 정리하기 위해 `git-study` Repository를 만들면서 다음 과정을 직접 사용했다.

먼저 프로젝트 폴더에서:

```bash
git init
```

Repository를 만들었다.

기본 Branch를:

```bash
git branch -M main
```

으로 설정했다.

`README.md`를 작성한 후:

```bash
git status
```

로 변경사항을 확인했다.

그다음:

```bash
git add README.md
```

로 Staging Area에 올렸다.

그리고:

```bash
git commit -m "Add Git study README"
```

로 첫 번째 학습 기록을 만들었다.

---

## 20. git status를 자주 확인하는 이유

Git 명령어를 사용할 때 현재 상태를 알고 있는 것이 중요하다.

예를 들어:

```text
Untracked
```

인지,

```text
Changes to be committed
```

인지,

```text
nothing to commit, working tree clean
```

인지에 따라 다음에 실행할 명령어가 달라진다.

따라서 처음 Git을 사용할 때는 각 작업 사이에:

```bash
git status
```

를 자주 실행하면서 상태 변화를 확인하는 것이 도움이 된다.

---

## 21. Working Tree Clean

다음과 같은 메시지가 나타날 수 있다.

```text
nothing to commit, working tree clean
```

현재 수정되거나 새롭게 추가된 변경사항이 없다는 의미이다.

즉 현재 작업 내용이 모두 Commit되어 있는 상태이다.

---

## 22. Git과 GitHub를 처음 사용할 때의 흐름

로컬에서 처음 프로젝트를 만들 경우:

```text
프로젝트 폴더 생성
        ↓
      git init
        ↓
파일 작성
        ↓
    git status
        ↓
     git add
        ↓
    git commit
        ↓
로컬 Git 기록 생성
```

여기까지는 **Git**의 영역이다.

그다음 GitHub와 연결하면:

```text
로컬 Git Repository
        ↓
GitHub Repository 연결
        ↓
      git push
        ↓
GitHub에 코드 저장
```

으로 확장된다.

GitHub 연결과 `push`, `pull`, `clone`은 별도의 Remote 단원에서 정리한다.

---

## 23. 지금 학습 Repository에서도 사용 중인 Git

현재 개발 공부 내용을 정리하는 Repository에서도 같은 과정을 사용하고 있다.

예:

```text
Markdown 작성
     ↓
git status
     ↓
git add
     ↓
git commit
     ↓
git push
```

예를 들어 개발 환경 단원을 작성한 후:

```bash
git add 01-development-environment
```

을 사용하여 해당 단원의 파일들을 Staging Area에 올렸다.

그리고:

```bash
git commit -m "Add development environment notes"
```

로 하나의 Commit으로 기록했다.

이를 통해 Git을 공부하는 동시에 실제 Git을 이용하여 학습 기록을 관리하고 있다.

---

## 24. 배운 점

Git은 단순히 파일을 GitHub에 업로드하는 프로그램이 아니라 프로젝트의 변경사항을 버전으로 관리하는 도구라는 것을 학습했다.

특히:

```text
Working Directory
      ↓
git add
      ↓
Staging Area
      ↓
git commit
      ↓
Repository
```

의 흐름을 이해하는 것이 Git의 기본이라는 것을 알게 되었다.

또한 `git status`를 이용하여 현재 상태를 계속 확인하면 Git에서 어떤 작업을 해야 하는지 이해하기 쉬워진다는 것을 학습했다.

Git과 GitHub는 서로 다른 역할을 가지고 있으며 Git으로 로컬 버전을 관리한 후 GitHub를 이용하여 원격 저장 및 협업을 할 수 있다는 것도 알게 되었다.