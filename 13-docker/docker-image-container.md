# Docker Image Build와 실행

## Image 만들기

Dockerfile을 이용해 Image를 만드는 것을 **Build**라고 한다.

```bash
docker build -t my-app .
```

## Container 실행하기

만든 Image를 실행한다.

```bash
docker run my-app
```

Port를 연결해서 실행할 수도 있다.

```bash
docker run -p 8000:8000 my-app
```

현재 실행 중인 Container 확인:

```bash
docker ps
```

정리하면:

```text
Dockerfile
   ↓ build
Image
   ↓ run
Container
```