# Docker-file이란?

Dockerfile은 **Docker Image를 어떻게 만들지 작성하는 파일**이다.

예:

```dockerfile
FROM python:3.13

WORKDIR /app

COPY . /app

RUN pip install fastapi uvicorn

CMD ["uvicorn", "main:app", "--host", "0.0.0.0"]
```

주요 명령어:

- `FROM` : 어떤 Image를 기반으로 만들지 설정
- `WORKDIR` : 작업할 폴더 설정
- `COPY` : 파일 복사
- `RUN` : Image를 만들면서 명령 실행
- `CMD` : Container 실행 시 실행할 명령

정리하면:

```text
Dockerfile
    ↓
Image 생성
```