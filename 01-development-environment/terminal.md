# 터미널 기본 사용법

## 1. 터미널이란?

터미널은 명령어를 입력하여 컴퓨터에 직접 작업을 요청할 수 있는 환경이다.

마우스로 폴더를 만들거나 파일을 이동하는 대신 명령어를 이용하여 같은 작업을 수행할 수 있다.

개발을 하면서 Git, Python, 패키지 설치, 서버 실행 등 다양한 작업에 터미널을 사용한다.

---

## 2. 현재 위치 확인

터미널에서는 현재 내가 어느 폴더에서 작업하고 있는지 확인하는 것이 중요하다.

PowerShell에서는 다음 명령어를 사용할 수 있다.

```powershell
pwd
```

예:

```text
C:\Users\user\git-study
```

현재 `git-study` 폴더에서 작업하고 있다는 의미이다.

---

## 3. 폴더의 파일 확인

현재 폴더에 어떤 파일과 폴더가 있는지 확인할 때 사용한다.

```powershell
dir
```

---

## 4. 폴더 이동

### 특정 폴더로 이동

```powershell
cd 폴더명
```

예:

```powershell
cd git-study
```

### 상위 폴더로 이동

```powershell
cd ..
```

예를 들어 현재 위치가:

```text
C:\Users\user\git-study
```

라면 `cd ..` 실행 후:

```text
C:\Users\user
```

로 이동한다.

---

## 5. 새로운 폴더 만들기

```powershell
mkdir 폴더명
```

예:

```powershell
mkdir project
```

그러면 현재 위치에 `project`라는 새로운 폴더가 생성된다.

---

## 6. 새로운 파일 만들기

PowerShell에서는 다음과 같이 파일을 만들 수 있다.

```powershell
New-Item 파일명
```

예:

```powershell
New-Item study.md
```

Python 파일을 만들 수도 있다.

```powershell
New-Item main.py
```

---

## 7. VS Code에서 파일 열기

터미널에서 VS Code로 파일을 바로 열 수 있다.

```powershell
code 파일명
```

예:

```powershell
code study.md
```

현재 폴더 전체를 VS Code로 열려면:

```powershell
code .
```

을 사용할 수 있다.

`.`은 현재 위치를 의미한다.

---

## 8. CMD와 PowerShell의 차이

Windows에서는 CMD와 PowerShell을 모두 사용할 수 있다.

일부 명령어는 공통으로 사용할 수 있지만 서로 다른 명령어도 존재한다.

예를 들어 화면을 지우는 명령어는 CMD와 PowerShell에서 다음과 같다.

```powershell
cls
```

Linux나 Git Bash에서는 다음 명령어를 주로 사용한다.

```bash
clear
```

파일 생성 방법도 환경에 따라 다를 수 있다.

PowerShell:

```powershell
New-Item test.txt
```

CMD:

```cmd
type nul > test.txt
```

Linux / Git Bash:

```bash
touch test.txt
```

따라서 인터넷에서 명령어를 찾을 때 현재 사용하고 있는 터미널 환경을 확인하는 것이 중요하다.

---

## 9. 터미널에서 자주 사용한 명령어

| 명령어 | 기능 |
|---|---|
| `pwd` | 현재 위치 확인 |
| `dir` | 현재 폴더 내용 확인 |
| `cd 폴더명` | 해당 폴더로 이동 |
| `cd ..` | 상위 폴더로 이동 |
| `mkdir 폴더명` | 새로운 폴더 생성 |
| `New-Item 파일명` | PowerShell에서 파일 생성 |
| `code 파일명` | VS Code에서 파일 열기 |
| `code .` | 현재 폴더를 VS Code로 열기 |
| `cls` | 터미널 화면 정리 |

---

## 10. 배운 점

개발을 할 때는 현재 터미널의 위치를 확인하는 것이 중요하다는 것을 알게 되었다.

특히 파일을 찾지 못하는 오류나 Git Repository 위치 문제 등이 발생할 수 있기 때문에 명령어를 실행하기 전에 현재 작업 위치를 확인하는 습관이 필요하다.

또한 CMD, PowerShell, Git Bash 등 터미널 종류에 따라 사용할 수 있는 명령어가 조금씩 다르다는 것을 학습했다.