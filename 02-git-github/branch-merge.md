# Git Branch와 Merge

## 1. Branch란?

Branch는 하나의 프로젝트에서 독립적으로 작업할 수 있도록 만들어진 작업 공간이다.

예를 들어 현재 안정적으로 동작하는 코드가 `main` Branch에 있다고 가정한다.

로그인 기능을 새롭게 개발해야 한다면 `main`에서 바로 코드를 수정할 수도 있지만,
개발 중 오류가 발생하면 기존 코드에도 영향을 줄 수 있다.

따라서 새로운 Branch를 만들어 작업할 수 있다.

```text
main
 │
 ├── 기존 코드
 │
 └── feature/login
         ↓
      로그인 개발
```

로그인 기능 개발이 완료되면 다시 `main`에 합칠 수 있다.

---

## 2. Branch를 사용하는 이유

여러 기능을 하나의 Branch에서 모두 개발하면 코드 관리가 어려워질 수 있다.

예를 들어 다음 기능을 개발한다고 가정한다.

```text
로그인
회원가입
로그아웃
결제
```

기능별로 Branch를 만들면:

```text
main
 ├── feature/login
 ├── feature/signup
 ├── feature/logout
 └── feature/payment
```

처럼 각각 독립적으로 작업할 수 있다.

이를 통해:

- 기능별 작업 분리
- 기존 코드 보호
- 여러 개발자의 동시 작업
- 코드 리뷰
- 문제 발생 시 관리

등이 쉬워진다.

---

## 3. 현재 Branch 확인

Branch 목록을 확인하려면:

```bash
git branch
```

을 사용한다.

예:

```text
* main
  feature/login
  feature/signup
```

`*` 표시가 현재 작업 중인 Branch이다.

위의 경우 현재 Branch는:

```text
main
```

이다.

---

## 4. 새로운 Branch 만들기

새로운 Branch를 만들려면:

```bash
git branch 브랜치이름
```

을 사용한다.

예:

```bash
git branch feature/login
```

이 명령어는 Branch를 만들지만 해당 Branch로 이동하지는 않는다.

확인:

```bash
git branch
```

결과:

```text
  feature/login
* main
```

---

## 5. Branch 이동

다른 Branch로 이동하려면:

```bash
git switch 브랜치이름
```

을 사용한다.

예:

```bash
git switch feature/login
```

확인:

```bash
git branch
```

결과:

```text
* feature/login
  main
```

---

## 6. Branch 생성과 이동을 동시에 하기

Branch를 생성한 후 바로 이동하려면:

```bash
git switch -c feature/login
```

을 사용할 수 있다.

즉 다음 두 명령어:

```bash
git branch feature/login
git switch feature/login
```

을 하나로 줄인 것이다.

```text
git switch -c feature/login
        ↓
Branch 생성
        +
Branch 이동
```

---

## 7. Branch 이름

Branch 이름은 어떤 작업을 하는 Branch인지 알 수 있도록 만드는 것이 좋다.

예:

```text
feature/login
feature/signup
feature/payment
hotfix/login-error
```

`feature`는 새로운 기능 개발에 많이 사용하는 이름이다.

---

## 8. Branch에서 작업하기

예를 들어 로그인 기능을 개발한다고 가정한다.

먼저:

```bash
git switch -c feature/login
```

Branch를 만든다.

코드를 작성한 후:

```bash
git status
git add .
git commit -m "Add login feature"
```

를 실행한다.

그러면 해당 Commit은 `feature/login` Branch에 만들어진다.

```text
main
  │
  A
   \
    B  ← feature/login
```

여기서 B는 로그인 기능을 추가한 Commit이다.

---

## 9. Branch마다 서로 다른 Commit을 가질 수 있다

Branch는 단순한 폴더 복사본이 아니다.

Git의 Commit을 가리키는 이름이라고 이해할 수 있다.

예를 들어:

```text
A --- B --- C  ← main
      \
       D --- E  ← feature/login
```

처럼 서로 다른 Commit을 가리킬 수 있다.

이 때문에 새로운 기능을 별도로 개발할 수 있다.

---

## 10. Merge란?

Merge는 한 Branch의 변경사항을 다른 Branch에 합치는 작업이다.

예를 들어:

```text
main
 │
 └──── feature/login
```

에서 로그인 기능 개발이 완료되었다면 `feature/login`을 `main`에 합칠 수 있다.

먼저 합칠 대상 Branch인 `main`으로 이동한다.

```bash
git switch main
```

그다음:

```bash
git merge feature/login
```

을 실행한다.

중요한 점은:

> 어떤 Branch에서 `git merge`를 실행하는지가 중요하다.

`feature/login`을 `main`에 넣고 싶다면:

```text
main으로 이동
    ↓
feature/login을 merge
```

해야 한다.

```bash
git switch main
git merge feature/login
```

---

## 11. Fast-forward Merge

Branch가 나뉜 이후 `main`에는 새로운 Commit이 없고 feature Branch에만 Commit이 추가되었다고 가정한다.

```text
A --- B  ← main
       \
        C --- D  ← feature/login
```

`main`에서:

```bash
git merge feature/login
```

을 하면 `main`이 앞으로 이동할 수 있다.

```text
A --- B --- C --- D
                   ↑
          main, feature/login
```

이것을 Fast-forward Merge라고 한다.

새로운 Merge Commit을 만들 필요 없이 Branch의 위치만 앞으로 이동할 수 있기 때문이다.

---

## 12. 3-way Merge

Branch가 나뉜 이후 두 Branch 모두에서 새로운 Commit이 만들어질 수도 있다.

예:

```text
        C --- D  ← feature/login
       /
A --- B
       \
        E --- F  ← main
```

이 경우 `main`과 `feature/login` 모두 변경되었기 때문에 단순히 Branch를 앞으로 이동시킬 수 없다.

Git은 다음 세 가지를 비교한다.

```text
공통 조상
main의 변경사항
feature/login의 변경사항
```

그리고 두 Branch의 변경사항을 합쳐 새로운 Merge Commit을 만든다.

```text
        C --- D
       /       \
A --- B         M  ← main
       \       /
        E --- F
```

이러한 방식을 3-way Merge라고 한다.

---

## 13. Fast-forward와 3-way Merge 차이

### Fast-forward

```text
main에는 추가 작업 없음
        +
feature에만 새로운 Commit
```

→ Branch 위치를 앞으로 이동

### 3-way Merge

```text
main에서도 작업
        +
feature에서도 작업
```

→ 변경사항을 합친 새로운 Merge Commit 생성

---

## 14. Merge Conflict

두 Branch에서 같은 파일의 같은 부분을 서로 다르게 수정하면 Git이 어떤 내용을 선택해야 하는지 판단하지 못할 수 있다.

이를 Merge Conflict라고 한다.

예를 들어 `main`에서:

```text
Hello Main
```

으로 수정하고,

`feature/login`에서 같은 줄을:

```text
Hello Login
```

으로 수정했다면 Merge 과정에서 충돌할 수 있다.

---

## 15. Conflict가 발생하면

Git은 충돌이 발생한 파일에 다음과 비슷한 표시를 만든다.

```text
<<<<<<< HEAD
Hello Main
=======
Hello Login
>>>>>>> feature/login
```

의미는 다음과 같다.

```text
<<<<<<< HEAD

현재 Branch의 내용

=======

합치려고 하는 Branch의 내용

>>>>>>> feature/login
```

개발자가 어떤 내용을 사용할 것인지 직접 결정해야 한다.

---

## 16. Conflict 해결

원하는 코드로 직접 수정한 후 Conflict 표시를 제거한다.

예를 들어:

```text
Hello
```

로 최종 결정했다면 파일을 수정한 후:

```bash
git add 충돌파일
```

그리고:

```bash
git commit
```

을 통해 Merge 작업을 완료할 수 있다.

즉:

```text
Merge
  ↓
Conflict 발생
  ↓
코드 확인
  ↓
직접 수정
  ↓
git add
  ↓
git commit
```

과정이다.

---

## 17. Branch 삭제

기능 개발이 끝나고 Merge까지 완료했다면 더 이상 필요하지 않은 Branch를 삭제할 수 있다.

```bash
git branch -d feature/login
```

`-d`는 일반적으로 Merge가 완료된 Branch를 안전하게 삭제할 때 사용한다.

---

## 18. Branch 이름을 잘못 만들었을 때

Branch 이름을 잘못 입력할 수도 있다.

예를 들어:

```text
feature/sign-up
```

을 만들려고 했는데:

```text
feature/sing-up
```

으로 잘못 만들었다고 가정한다.

Branch 목록을 먼저 확인한다.

```bash
git branch
```

필요 없는 Branch라면:

```bash
git branch -d feature/sing-up
```

으로 삭제할 수 있다.

따라서 Branch를 생성한 후에는:

```bash
git branch
```

로 이름을 확인하는 습관이 도움이 된다.

---

## 19. 존재하지 않는 Branch를 Merge하려고 하면

예를 들어:

```bash
git merge feature/login
```

을 실행했지만 `feature/login` Branch가 실제로 존재하지 않는다면 Merge할 수 없다.

따라서 Merge 전에:

```bash
git branch
```

로 현재 존재하는 Branch를 확인하는 것이 좋다.

필요하면 전체 Git 기록도 확인할 수 있다.

```bash
git log --all --oneline --graph --decorate
```

이 명령어를 사용하면 Branch와 Commit의 관계를 그래프 형태로 확인할 수 있다.

---

## 20. Branch 관리 전략

실제 프로젝트에서는 Branch에 역할을 부여하여 관리하기도 한다.

대표적인 이름은 다음과 같다.

### main

실제 배포 가능한 안정적인 코드를 관리한다.

### develop

여러 개발 기능을 통합하는 개발용 Branch이다.

### feature

새로운 기능을 개발한다.

예:

```text
feature/login
feature/signup
feature/payment
```

### release

새로운 버전을 배포하기 전에 최종 테스트와 준비를 진행한다.

### hotfix

이미 배포된 서비스에서 긴급한 문제가 발생했을 때 수정하기 위해 사용한다.

---

## 21. Branch 관리 구조 예시

```text
main
 │
 └── develop
      │
      ├── feature/login
      │
      ├── feature/signup
      │
      └── feature/payment
```

기능 개발이 완료되면 feature Branch를 develop에 합치고,
배포 준비가 끝나면 최종적으로 main에 반영하는 방식으로 사용할 수 있다.

모든 프로젝트가 반드시 이 구조를 사용해야 하는 것은 아니며 프로젝트와 팀의 개발 방식에 따라 Branch 전략은 달라질 수 있다.

---

## 22. Branch 작업 흐름

새로운 로그인 기능을 만든다고 가정하면:

```text
main
 ↓
새로운 Branch 생성
 ↓
feature/login
 ↓
코드 작성
 ↓
git add
 ↓
git commit
 ↓
main으로 이동
 ↓
git merge feature/login
 ↓
기능 통합
```

명령어로 보면:

```bash
git switch -c feature/login

# 코드 작성

git add .
git commit -m "Add login feature"

git switch main
git merge feature/login
```

---

## 23. Branch와 GitHub 협업

실제 협업에서는 로컬에서 바로 `main`에 Merge하지 않고 GitHub에 Branch를 Push한 후 Pull Request를 이용하는 경우가 많다.

```text
main
 ↓
feature/login 생성
 ↓
기능 개발
 ↓
Commit
 ↓
GitHub에 Push
 ↓
Pull Request
 ↓
Code Review
 ↓
Merge
```

이 과정은 GitHub 협업 단원에서 자세히 정리한다.

---

## 24. 배운 점

Branch는 단순히 파일을 복사하여 따로 작업하는 기능이 아니라 Git의 Commit을 기준으로 독립적인 개발 흐름을 만드는 기능이라는 것을 학습했다.

새로운 기능을 개발할 때 Branch를 사용하면 안정적인 `main`에 직접 영향을 주지 않고 작업할 수 있다.

또한 Merge에는 Fast-forward와 3-way Merge 등의 방식이 있으며 같은 부분을 서로 다르게 수정하면 Merge Conflict가 발생할 수 있다는 것을 알게 되었다.

Branch 작업에서는 현재 어떤 Branch에 있는지 확인하는 것이 중요하다.

따라서 다음 명령어를 자주 확인하는 습관이 필요하다.

```bash
git branch
git status
git log --all --oneline --graph --decorate
```

특히 Branch를 만들거나 Merge하기 전에 Branch 이름과 현재 위치를 확인하는 것이 실수를 줄이는 데 도움이 된다.