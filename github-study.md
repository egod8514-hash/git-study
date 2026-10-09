# Git / GitHub 학습 정리

## 1. Git Push / Clone / Pull

### git push

`git push`는 로컬 저장소에서 commit한 내용을 GitHub의 원격 저장소에 올리는 명령어이다.

```bash
git push
```

처음 로컬 Branch와 원격 Branch를 연결할 때는 다음과 같이 사용할 수 있다.

```bash
git push -u origin main
```

- `origin` : 원격 저장소의 기본 이름
- `main` : Push할 Branch 이름

---

### git clone

`git clone`은 GitHub에 있는 Repository를 내 컴퓨터로 처음 복제할 때 사용하는 명령어이다.

```bash
git clone Repository주소
```

즉,

```text
GitHub Repository
        ↓
    git clone
        ↓
내 컴퓨터
```

의 과정이다.

---

### git pull

`git pull`은 GitHub의 원격 저장소에서 변경된 최신 내용을 내 로컬 저장소로 가져오는 명령어이다.

```bash
git pull
```

예를 들어 다른 개발자가 코드를 수정하여 GitHub에 Push했다면, `git pull`을 사용하여 변경된 내용을 내 컴퓨터에 가져올 수 있다.

### Push / Clone / Pull 차이

| 명령어 | 의미 |
|---|---|
| `git push` | 내 변경 내용을 GitHub에 올리기 |
| `git clone` | Repository를 처음 내 컴퓨터로 복제하기 |
| `git pull` | 원격 저장소의 최신 변경 내용 가져오기 |

---

## 2. Branch 관리 전략

Branch는 기존 코드와 분리된 공간에서 새로운 기능을 개발하기 위해 사용한다.

예를 들어 로그인 기능을 개발한다면:

```bash
git switch -c feature/login
```

과 같이 새로운 Branch를 만들 수 있다.

### 주요 Branch

- `main` : 실제 배포 가능한 안정적인 코드
- `develop` : 개발 중인 기능들을 통합하는 Branch
- `feature` : 새로운 기능을 개발하는 Branch
- `release` : 배포 전 최종 확인을 위한 Branch
- `hotfix` : 배포 후 긴급한 오류를 수정하는 Branch

예를 들면 다음과 같이 관리할 수 있다.

```text
main
 └── develop
      ├── feature/login
      ├── feature/signup
      └── feature/payment
```

기능별로 Branch를 나누면 여러 개발자가 동시에 작업하면서 기존 코드에 미치는 영향을 줄일 수 있다.

---

## 3. GitHub 기초

Git과 GitHub는 서로 다른 역할을 한다.

**Git**

내 컴퓨터에서 파일의 변경 사항과 버전을 관리하는 도구이다.

**GitHub**

Git으로 관리되는 Repository를 인터넷에 저장하고 다른 개발자와 공유할 수 있는 서비스이다.

```text
Git
→ 로컬에서 버전 관리

GitHub
→ 원격에서 Repository 공유 및 협업
```

---

## 4. Repository 생성 및 관리

Repository는 프로젝트의 파일과 Git 변경 기록을 저장하고 관리하는 공간이다.

로컬 Repository와 GitHub Repository는 다음과 같이 연결할 수 있다.

```bash
git remote add origin Repository주소
```

연결된 원격 저장소는 다음 명령어로 확인할 수 있다.

```bash
git remote -v
```

---

## 5. Issue

GitHub의 `Issue`는 프로젝트에서 해야 할 작업이나 버그, 새로운 기능 등을 기록하고 관리하는 기능이다.

예를 들어:

```text
로그인 기능 추가
회원가입 오류 수정
메인 페이지 디자인 수정
```

등을 Issue로 등록할 수 있다.

이를 통해 어떤 작업이 필요한지 팀원들과 공유할 수 있다.

---

## 6. Pull Request (PR)

Pull Request는 특정 Branch에서 작업한 내용을 다른 Branch에 합치기 전에 검토를 요청하는 기능이다.

예를 들어 `feature/login`에서 로그인 기능을 개발했다면:

```text
feature/login
      ↓
Pull Request
      ↓
코드 리뷰
      ↓
Merge
      ↓
main
```

과 같은 과정을 거칠 수 있다.

Pull Request를 사용하면 다른 개발자가 변경된 코드를 확인하고 문제가 없는지 검토한 후 Merge할 수 있다.

---

## 7. GitHub Branch 관리

새로운 기능을 개발할 때는 별도의 Branch를 만들어 작업할 수 있다.

```bash
git switch -c feature/login
```

작업을 완료한 후:

```bash
git add .
git commit -m "Add login feature"
git push origin feature/login
```

을 실행하면 GitHub에도 `feature/login` Branch가 생성된다.

그 후 GitHub에서 Pull Request를 생성하여 `main` Branch에 Merge할 수 있다.

---

## 8. 전체 작업 흐름

오늘 배운 Git/GitHub 협업 과정을 정리하면 다음과 같다.

```text
GitHub Repository
        ↓
     git clone
        ↓
로컬 Repository
        ↓
Branch 생성
        ↓
코드 작성
        ↓
     git add
        ↓
    git commit
        ↓
     git push
        ↓
GitHub에 코드 업로드
        ↓
Pull Request
        ↓
코드 리뷰
        ↓
      Merge
        ↓
다른 개발자가 git pull
        ↓
최신 코드 반영
```

---

## 9. 오늘 배운 내용

오늘은 `push`, `clone`, `pull`의 차이를 학습하고 GitHub의 원격 저장소와 로컬 저장소가 어떻게 연결되는지 학습했다.

또한 Branch를 기능별로 나누어 개발하는 이유와 `main`, `develop`, `feature`, `release`, `hotfix` 등의 Branch 관리 방법을 학습했다.

GitHub에서는 Repository를 통해 프로젝트를 관리하고, Issue를 이용하여 작업이나 문제를 기록하며, Pull Request를 통해 다른 개발자에게 코드 검토를 요청한 후 Merge할 수 있다는 것을 학습했다.