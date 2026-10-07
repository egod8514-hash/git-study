
# Git / GitHub 학습 정리

## 1. Git / GitHub

### Git과 GitHub를 사용하는 이유

Git은 파일의 변경 이력을 기록하고 관리하기 위한 버전 관리 시스템이다.

개발 과정에서 코드를 수정했을 때 변경 기록을 남길 수 있으며,
필요한 경우 이전 상태를 확인하거나 되돌릴 수 있다.

GitHub는 Git으로 관리하는 프로젝트를 인터넷에 저장하고
다른 개발자와 공유하거나 협업할 수 있도록 해주는 서비스이다.

### Git과 GitHub의 차이

- Git: 내 컴퓨터에서 파일의 버전과 변경 이력을 관리한다.
- GitHub: Git 저장소를 인터넷에 저장하고 공유할 수 있는 서비스이다.

### Repository

Repository는 Git이 프로젝트의 파일과 변경 이력을 관리하는 저장소이다.

일반적으로 하나의 프로젝트를 하나의 Repository로 관리할 수 있다.

### Commit

Commit은 변경된 파일의 상태를 하나의 기록으로 저장하는 것이다.

```bash
git commit -m "Add login"
```

커밋 메시지를 통해 어떤 작업을 했는지 기록할 수 있다.

### Branch

Branch는 기존 코드에 영향을 최소화하면서
새로운 기능을 독립적으로 개발하기 위해 사용하는 작업 공간이다.

예를 들어 로그인 기능을 개발한다면 다음과 같은 브랜치를 만들수 있다.
git branch feat/login

## 2. Git 기본 명령어

### git config

Git 사용자 정보를 설정한다
```bash
git config --global user.name "사용자이름"
git config --global user.email "이메일"
```

### git init

현재 폴더를 Git Repository로 만든다.
```bash
git init
```

### git status

현재 Git의 상태를 확인한다.
```bash
git status
```
어떤 파일이 수정되었는지, 어떤 파일이 커밋될 예정인지 등을 확인할 수 있다.

### git add

변경사항을 다음 커밋에 포함하도록 추가한다.
```bash
git add README.md
```

모든 변경 파일을 추가하려면 다음과 같이 사용할 수 있다.
```bash
git add .
```

### git commit

git add로 추가한 변경사항을 하나의 기록으로 저장한다.
```bash
git commit -m "Add README"
```

### git push

내 컴퓨터의 Git 커밋을 GitHub와 같은 원격 Repository에 업로드한다.
```bash
git push
```

### git pull

원격 Repository의 변경사항을 내 컴퓨터로 가져와 반영한다.
```bash
git pull
```

## 3. Branch

### git branch 명령어

현재 존재하는 브랜치를 확인한다.
```bash
git branch
```
새로운 브랜치를 생성할 수도 있다.
```bash
git branch feat/login
```
브랜치를 이동하려면 다음과 같이 사용할 수 있다.
```bash
git switch feat/login
```
새로운 브랜치를 만들면서 바로 이동도 가능하다.
```bash
git switch -c feat/login
```

### 브랜치 관리

프로젝트에서는 목적에 따라 브랜치를 나누어 관리할 수 있다.
main : 배포 가능한 안정적인 코드
develop : 개발 중인 기능들을 통합하는 브랜치
feature : 새로운 기능 개발
hotfix : 긴급한 오류 수정
release : 배포를 준비하는 브랜치
프로잭트의 방식에 따라 사용하는 브랜치 구조는 달라질 수 있다.

### Fast-forward

브랜치가 분기된 이후 main에 새로운 커밋이 없다면,
브랜치 포인터를 앞으로 이동하는 방식으로 병합할 수 있다.

이를 Fast-forward merge라고 한다.

### 3-way merge

두 브랜치가 서로 다른 커밋을 가지고 있는 경우
공통 조상을 기준으로 두 브랜치의 변경사항을 합친다.

이 과정에서 새로운 Merge Commit이 만들어질 수 있다.

### Merge Conflict

서로 다른 브랜치에서 같은 파일의 같은 부분을 다르게 수정하면
Git이 어떤 내용을 선택해야 하는지 자동으로 판단하지 못할 수 있다.

이를 Merge Conflict 라고 한다.
충돌이 발생하면 개발자가 직접 코드를 확인하고
어떤 내용을 사용할지 결정한 후 다시 커밋해야 한다.


