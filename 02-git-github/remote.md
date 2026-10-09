# Git Remote - Push / Pull / Clone

## 1. Local Repository와 Remote Repository

Git Repository는 크게 로컬 저장소와 원격 저장소로 구분할 수 있다.

### Local Repository

내 컴퓨터에 있는 Git Repository이다.

```text
내 컴퓨터
C:\Users\user\git-study
```

여기에서:

```bash
git add
git commit
git branch
git merge
```

등의 Git 작업을 수행할 수 있다.

### Remote Repository

인터넷에 존재하는 Git Repository이다.

GitHub Repository가 대표적인 예이다.

```text
내 컴퓨터
Local Repository

       ↕
       
GitHub
Remote Repository
```

---

## 2. Git과 GitHub의 연결

`git init`을 했다고 해서 자동으로 GitHub와 연결되는 것은 아니다.

```text
git init
   ↓
Local Git Repository 생성
```

여기에 GitHub Repository를 연결해야 한다.

```text
Local Repository
       ↓
Remote 설정
       ↓
GitHub Repository
```

---

## 3. git remote

Remote는 현재 Local Repository와 연결된 원격 저장소를 의미한다.

현재 연결된 Remote를 확인하려면:

```bash
git remote
```

조금 더 자세히 확인하려면:

```bash
git remote -v
```

를 사용한다.

예:

```text
origin  https://github.com/username/project.git (fetch)
origin  https://github.com/username/project.git (push)
```

---

## 4. origin이란?

`origin`은 원격 Repository에 붙이는 이름이다.

일반적으로 첫 번째 원격 Repository의 이름으로 `origin`을 많이 사용한다.

즉:

```text
origin
   ↓
GitHub Repository의 별명
```

이라고 이해할 수 있다.

`origin`이 GitHub 자체를 의미하는 것은 아니다.

원격 Repository 주소에 붙인 이름이다.

---

## 5. 원격 Repository 연결

GitHub에서 Repository를 만든 후 Local Repository와 연결하려면:

```bash
git remote add origin Repository주소
```

를 사용한다.

예:

```bash
git remote add origin https://github.com/username/git-study.git
```

그다음:

```bash
git remote -v
```

를 사용하여 제대로 연결되었는지 확인할 수 있다.

---

## 6. git push

`git push`는 Local Repository의 Commit을 Remote Repository에 보내는 명령어이다.

```text
내 컴퓨터
Local Repository
      │
      │ git push
      ▼
GitHub
Remote Repository
```

예:

```bash
git push origin main
```

의 의미는:

```text
origin → 어느 원격 저장소에?

main → 어떤 Branch를?
```

Push할 것인지 지정한 것이다.

---

## 7. 첫 Push와 -u

처음 Push할 때 다음과 같이 사용할 수 있다.

```bash
git push -u origin main
```

`-u` 옵션을 사용하면 Local `main`과 Remote의 `main` 사이에 추적 관계를 설정한다.

개념적으로:

```text
Local main
    ↕
origin/main
```

이 연결이 만들어진다.

그 이후에는:

```bash
git push
```

만 입력해도 Git이 어디로 Push해야 하는지 알 수 있다.

---

## 8. origin/main이란?

Git을 사용하면서 다음과 같은 표현을 볼 수 있다.

```text
origin/main
```

`origin/main`은 원격 저장소 `origin`의 `main` Branch 상태를 나타낸다.

단순화하면:

```text
main
→ 내 Local Branch

origin/main
→ 내가 알고 있는 Remote main의 상태
```

라고 이해할 수 있다.

---

## 9. ahead by 1 commit

Commit을 만들고 아직 Push하지 않았을 때:

```bash
git status
```

에서 다음과 같은 메시지가 나타날 수 있다.

```text
Your branch is ahead of 'origin/main' by 1 commit.
```

이것은 오류가 아니다.

현재:

```text
Local main
    │
    └── 새로운 Commit
             ↓
       아직 Push 안 됨

origin/main
```

이라는 의미이다.

즉 Local에는 새로운 Commit이 있지만 Remote에는 아직 없는 상태이다.

이때:

```bash
git push
```

하면 된다.

---

## 10. up to date

Push가 완료된 후:

```bash
git status
```

를 실행하면:

```text
Your branch is up to date with 'origin/main'.
```

이라고 나타날 수 있다.

이는 현재 Local Branch와 Remote Branch 사이에 Push할 새로운 Commit이 없다는 의미이다.

---

## 11. git clone

`git clone`은 이미 존재하는 Remote Repository를 내 컴퓨터로 처음 가져올 때 사용한다.

```bash
git clone Repository주소
```

예:

```bash
git clone https://github.com/username/project.git
```

구조는:

```text
GitHub Repository
        │
        │ git clone
        ▼
내 컴퓨터
Local Repository
```

이다.

---

## 12. clone은 단순 파일 다운로드와 다르다

GitHub에서 ZIP 파일을 다운로드하는 것과 `git clone`은 다르다.

`git clone`을 사용하면 프로젝트 파일뿐만 아니라 Git Repository의 정보도 함께 가져온다.

따라서 Clone 후:

```bash
git status
git log
git branch
```

등의 Git 명령어를 사용할 수 있다.

---

## 13. git clone과 git init의 차이

새로운 프로젝트를 내가 처음 만든다면:

```bash
git init
```

을 사용할 수 있다.

반대로 이미 GitHub에 존재하는 프로젝트를 가져온다면:

```bash
git clone
```

을 사용한다.

```text
내가 새로운 프로젝트 시작
        ↓
     git init


GitHub에 기존 프로젝트 존재
        ↓
     git clone
```

---

## 14. git pull

`git pull`은 Remote Repository에 있는 최신 변경사항을 Local Repository로 가져와 반영할 때 사용한다.

```text
GitHub
Remote Repository
      │
      │ git pull
      ▼
내 컴퓨터
Local Repository
```

예:

```bash
git pull
```

---

## 15. pull이 필요한 상황

여러 개발자가 하나의 Repository에서 작업한다고 가정한다.

개발자 A가:

```text
새로운 기능 작성
↓
commit
↓
push
```

했다.

그러면 GitHub에는 새로운 코드가 있지만 개발자 B의 컴퓨터에는 아직 없다.

개발자 B는:

```bash
git pull
```

을 이용하여 최신 변경사항을 가져올 수 있다.

---

## 16. Push / Pull / Clone 차이

| 명령어 | 방향 | 용도 |
|---|---|---|
| `git push` | Local → Remote | 내가 만든 Commit을 원격에 올림 |
| `git pull` | Remote → Local | 원격의 최신 변경사항을 가져옴 |
| `git clone` | Remote → Local | Repository를 처음 통째로 가져옴 |

그림으로 보면:

```text
              git push
Local ----------------------> GitHub

Local <---------------------- GitHub
              git pull


GitHub
   │
   │ git clone
   ▼
새로운 Local Repository
```

---

## 17. clone과 pull의 차이

둘 다 GitHub에서 내용을 가져오는 것처럼 보이지만 사용하는 시점이 다르다.

### clone

내 컴퓨터에 아직 Repository가 없을 때 사용한다.

```text
Repository 없음
      ↓
git clone
      ↓
Repository 생성
```

### pull

이미 Clone했거나 Remote와 연결된 Repository가 있을 때 최신 변경사항을 가져온다.

```text
Repository 이미 있음
      ↓
git pull
      ↓
최신 상태 반영
```

---

## 18. 실제로 git-study를 GitHub에 연결한 과정

학습 내용을 저장하기 위해 Local에서 `git-study` Repository를 만들었다.

먼저:

```bash
git init
```

으로 Git Repository를 만들었다.

Branch를:

```bash
git branch -M main
```

으로 설정했다.

README 파일을 작성하고:

```bash
git add README.md
git commit -m "Add Git study README"
```

로 Commit했다.

그 후 GitHub에 Public Repository를 만들었다.

Local Repository와 GitHub를 연결했다.

```bash
git remote add origin https://github.com/username/git-study.git
```

연결 확인:

```bash
git remote -v
```

첫 Push:

```bash
git push -u origin main
```

그 이후부터는:

```bash
git push
```

만으로 새로운 Commit을 GitHub에 올릴 수 있게 되었다.

---

## 19. 새로운 파일을 GitHub에 올리는 과정

Repository가 이미 연결되어 있다면 매번 Remote를 다시 설정할 필요가 없다.

예를 들어:

```text
study.md 작성
```

후:

```bash
git status
```

변경사항을 확인한다.

```bash
git add study.md
```

Staging Area에 올린다.

```bash
git commit -m "Add study notes"
```

Commit한다.

마지막으로:

```bash
git push
```

하면 GitHub에 반영된다.

---

## 20. 전체 흐름

새로운 Local 프로젝트를 GitHub에 처음 올리는 과정:

```text
프로젝트 생성
     ↓
git init
     ↓
파일 작성
     ↓
git add
     ↓
git commit
     ↓
GitHub Repository 생성
     ↓
git remote add origin ...
     ↓
git push -u origin main
```

이미 연결된 프로젝트에서 계속 작업할 때:

```text
파일 수정
   ↓
git status
   ↓
git add
   ↓
git commit
   ↓
git push
```

다른 개발자가 변경한 내용을 가져올 때:

```text
GitHub에 새로운 변경사항
        ↓
     git pull
        ↓
내 Local Repository 반영
```

---

## 21. Git 상태를 확인하는 습관

Remote 작업에서도 `git status`를 자주 확인하면 현재 상황을 이해하는 데 도움이 된다.

예:

```text
Your branch is ahead of 'origin/main' by 1 commit.
```

→ Commit은 했지만 Push하지 않은 상태

```text
Your branch is up to date with 'origin/main'.
```

→ Local과 Remote 사이에 Push할 새로운 Commit이 없는 상태

따라서 단순히 명령어를 외우기보다 Git이 보여주는 상태 메시지를 읽는 것이 중요하다.

---

## 22. Remote 관련 자주 사용하는 명령어

```bash
git remote
```

Remote 이름 확인

```bash
git remote -v
```

Remote 주소 확인

```bash
git remote add origin Repository주소
```

Remote Repository 연결

```bash
git push -u origin main
```

처음 Push하면서 Branch 추적 관계 설정

```bash
git push
```

Local Commit을 Remote에 Push

```bash
git pull
```

Remote 변경사항을 Local에 반영

```bash
git clone Repository주소
```

Remote Repository를 처음 복제

---

## 23. 배운 점

Git과 GitHub를 사용할 때 Local Repository와 Remote Repository가 서로 다른 저장소라는 것을 이해하는 것이 중요하다는 것을 학습했다.

`git commit`을 했다고 바로 GitHub에 올라가는 것이 아니며 Commit은 먼저 Local Repository에 저장된다.

그 후:

```bash
git push
```

를 실행해야 Remote Repository인 GitHub에 반영된다.

또한 `git clone`은 Repository를 처음 가져올 때 사용하고 `git pull`은 이미 가지고 있는 Repository의 최신 변경사항을 가져올 때 사용한다는 차이를 학습했다.

전체 흐름을 정리하면:

```text
Local에서 작업
      ↓
git add
      ↓
git commit
      ↓
git push
      ↓
GitHub

GitHub의 변경사항
      ↓
git pull
      ↓
Local
```

이라고 이해할 수 있다.