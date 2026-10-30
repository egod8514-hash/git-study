# Docker 핵심 개념

## Image

컨테이너를 만들기 위한 **설계도**이다.

예:

```text
mysql:8.0
```

## Container

Image를 이용해 **실제로 실행한 것**이다.

```text
Image → 실행 → Container
```

## Volume

Container가 꺼지거나 다시 만들어져도 **데이터를 보관하기 위한 저장 공간**이다.

MySQL 데이터를 유지할 때 사용할 수 있다.

## Port

내 컴퓨터와 Container를 연결한다.

예:

```text
3306:3306
```

앞쪽은 내 컴퓨터의 Port, 뒤쪽은 Container의 Port이다.

같은 Port를 다른 프로그램이 이미 사용하고 있으면 **Port 충돌**이 발생할 수 있다.