# Docker란?

Docker는 프로그램을 실행하는 데 필요한 환경을 **컨테이너(Container)**로 만들어 실행하는 도구이다.

컴퓨터마다 Python 버전이나 설치된 프로그램이 달라서 생기는 문제를 줄일 수 있다.

쉽게 말하면,

```text
"내 컴퓨터에서는 되는데 다른 컴퓨터에서는 안 돼요"
```

같은 문제를 줄이기 위해 사용한다.

## 기본 흐름

```text
Dockerfile → Image → Container
```

- **Dockerfile** : 실행 환경을 어떻게 만들지 작성한 파일
- **Image** : 실행 환경을 저장한 설계도
- **Container** : Image를 실제로 실행한 상태