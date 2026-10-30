# Docker Compose

Docker Compose는 **여러 Container를 한 번에 관리하는 방법**이다.

예를 들어 프로젝트에:

```text
FastAPI
MySQL
```

이 필요하다면 Docker Compose로 같이 실행할 수 있다.

## 실행

```bash
docker compose up -d
```

## 상태 확인

```bash
docker compose ps
```

## 종료

```bash
docker compose down
```

정리하면:

```text
up   → 실행
ps   → 상태 확인
down → 종료
```

이번 프로젝트에서는 FastAPI와 MySQL을 함께 관리하기 위해 Docker Compose를 사용했다.