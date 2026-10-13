# Network Basic

## 1. Network란?

Network는 여러 Computer와 Device가 서로 연결되어 Data를 주고받을 수 있는 구조이다.

예:

```text
Computer
Smartphone
Server
Router
Printer
```

등이 서로 통신할 수 있다.

---

# 2. 가장 간단한 Network

```text
Computer A
    ↕
Computer B
```

두 Computer가 연결되어 Data를 주고받는 것도 Network라고 할 수 있다.

---

# 3. Network가 필요한 이유

Computer 한 대에서 모든 작업을 처리한다면 Network가 필요하지 않을 수도 있다.

하지만 실제 Service에서는:

```text
사용자 Computer

Web Server

Database Server

AI Server

Cloud Server
```

등 여러 System이 서로 Data를 주고받는다.

---

# 4. Web Service와 Network

사용자가 Browser에서 Website에 접속한다고 하자.

```text
사용자 Computer
↓
Network
↓
Web Server
```

Browser가 Server에 Request를 보내려면 두 Computer가 Network를 통해 통신할 수 있어야 한다.

---

# 5. Internet이란?

Internet은 전 세계의 수많은 Network가 서로 연결된 거대한 Network이다.

즉:

```text
Internet
=
Network of Networks
```

라고 이해할 수 있다.

---

# 6. Network와 Internet

Network:

```text
여러 Device가 연결된 구조
```

Internet:

```text
전 세계의 수많은 Network가
서로 연결된 거대한 Network
```

이다.

---

# 7. Web과 Internet은 같은 것인가?

같지 않다.

Internet은:

```text
통신을 위한 기반 Network
```

이고,

Web은:

```text
Internet을 이용하는 Service 중 하나
```

이다.

---

# 8. Internet 위의 Service

Internet에서는 Web뿐 아니라 다양한 통신이 이루어진다.

예:

```text
Web

Email

File Transfer

Online Game

Video Streaming

Messaging
```

등이다.

---

# 9. Web

Web에서는 일반적으로:

```text
Browser
↓
HTTP / HTTPS
↓
Web Server
```

형태로 통신한다.

---

# 10. Client

Network에서 어떤 Service를 요청하는 쪽을:

```text
Client
```

라고 한다.

대표적인 예:

```text
Chrome

Edge

Safari

Mobile App
```

등이 있다.

---

# 11. Server

Client의 Request를 받아 처리하고 Response를 제공하는 쪽을:

```text
Server
```

라고 한다.

---

# 12. 기본 구조

```text
Client
↓
Request
↓
Server

Server
↓
Response
↓
Client
```

이다.

---

# 13. 쇼핑몰 예

사용자가 상품 목록 Page를 연다.

```text
Chrome
↓
상품 목록 Request
↓
쇼핑몰 Server
```

Server는 필요한 Data를 준비한다.

```text
쇼핑몰 Server
↓
상품 Data
↓
Chrome
```

Browser가 이를 화면에 표시한다.

---

# 14. Client = Browser인가?

Browser는 대표적인 Client이지만 Client가 반드시 Browser인 것은 아니다.

예:

```text
Browser

Mobile App

Desktop App

Python Program

curl
```

도 Server에 Request를 보내면 Client 역할을 할 수 있다.

---

# 15. curl도 Client

Terminal에서:

```bash
curl https://example.com
```

을 실행하면 `curl` Program이 Client가 되어 Server에 Request를 보낸다.

---

# 16. Browser와 curl

둘 다 Client가 될 수 있다.

```text
Chrome
↓
HTTP Request
↓
Server
```

```text
curl
↓
HTTP Request
↓
Server
```

차이는 사용 방법과 목적이다.

---

# 17. Browser

Browser는 Response로 받은 HTML, CSS, JavaScript 등을 해석하여 화면으로 보여준다.

---

# 18. curl

curl은 HTTP Request와 Response를 직접 확인하거나 API를 테스트할 때 유용하다.

그래서 Backend/API 공부에서 자주 사용한다.

---

# 19. Request

Client가 Server에 보내는 요청이다.

예:

```text
상품 목록 주세요.

Login 해주세요.

게시글을 저장해주세요.

이 X-ray Image를 분석해주세요.
```

---

# 20. Response

Server가 Request를 처리한 뒤 Client에게 보내는 결과이다.

예:

```text
상품 목록

Login 결과

저장 결과

AI Prediction 결과
```

---

# 21. Web 통신 기본 흐름

```text
Client
↓
Request
↓
Network
↓
Server
↓
Processing
↓
Response
↓
Network
↓
Client
```

이다.

---

# 22. Server는 반드시 다른 Computer인가?

개발 중에는 반드시 그렇지 않다.

자신의 Computer에서 Client와 Server를 모두 실행할 수도 있다.

---

# 23. Local 개발

예:

```text
Browser
http://localhost:5500

FastAPI
http://localhost:8000
```

둘 다 자신의 Computer에서 실행할 수 있다.

---

# 24. localhost

`localhost`는 일반적으로 현재 자기 Computer를 가리키는 Host Name이다.

즉:

```text
http://localhost:8000
```

은 개발 중 자신의 Computer에서 8000번 Port를 사용하는 Service에 접근하는 형태이다.

---

# 25. Network 통신에 필요한 것

상대방과 통신하려면 여러 정보와 규칙이 필요하다.

대표적으로:

```text
IP Address

Port

Protocol
```

이 중요하다.

---

# 26. IP Address

Network에서 통신할 Device 또는 Interface를 식별하기 위한 Address이다.

예:

```text
192.168.0.10
```

또는:

```text
127.0.0.1
```

같은 형태를 볼 수 있다.

---

# 27. Port

하나의 Computer에서는 여러 Program이 Network 통신을 할 수 있다.

Port를 이용하여 어떤 Service와 통신할지를 구분할 수 있다.

예:

```text
localhost:5500
→ Frontend 개발 Server

localhost:8000
→ FastAPI Server
```

---

# 28. 비유

IP를 건물 주소라고 생각하면 Port는 그 건물 안에서 찾아갈 서비스 창구나 방 번호처럼 비유할 수 있다.

```text
IP
→ 어느 Computer?

Port
→ 그 Computer의 어느 Network Service?
```

정도로 이해하면 된다.

---

# 29. Protocol

통신할 때 사용하는 규칙이다.

예:

```text
HTTP

HTTPS

TCP

UDP
```

등이 있다.

---

# 30. 왜 규칙이 필요한가?

두 Computer가 서로 다른 방식으로 Data를 해석하면 통신할 수 없다.

그래서:

```text
어떤 형식으로 보내는가?

어떤 순서로 처리하는가?

Error는 어떻게 처리하는가?
```

등에 대한 규칙이 필요하다.

---

# 31. HTTP

Web에서 Client와 Server가 Request와 Response를 주고받기 위해 사용하는 Application Layer Protocol이다.

```text
Client
↓
HTTP Request
↓
Server

Server
↓
HTTP Response
↓
Client
```

---

# 32. HTTPS

HTTPS는 HTTP 통신을 TLS를 이용해 보호한다.

```text
HTTP
+
TLS
=
HTTPS
```

라고 단순화해서 이해할 수 있다.

---

# 33. Network Layer 개념

Network 통신은 여러 단계의 Protocol이 함께 동작한다.

Web Request를 단순화하면:

```text
HTTP
↓
TCP
↓
IP
↓
Network
```

같이 생각할 수 있다.

HTTPS라면 TLS도 관련된다.

---

# 34. HTTP의 역할

```text
Web에서 어떤 Request와 Response를
주고받을 것인가?
```

와 관련된다.

---

# 35. TCP의 역할

대표적으로:

```text
신뢰성 있는 Data 전달

순서 보장

재전송
```

등을 제공한다.

HTTP/1.1과 HTTP/2는 일반적으로 TCP 위에서 동작한다.

---

# 36. IP의 역할

Data가 Network에서 목적지까지 전달될 수 있도록 Addressing과 Routing에 관여한다.

---

# 37. Packet

Network에서는 큰 Data도 작은 단위로 나뉘어 전달될 수 있다.

이러한 Network Data 단위를 설명할 때:

```text
Packet
```

이라는 표현을 자주 사용한다.

정확한 명칭은 Network Layer에 따라 달라질 수 있지만 처음에는 Data가 작은 단위로 전달된다는 개념을 이해한다.

---

# 38. Router

서로 다른 Network 사이에서 Data가 목적지로 갈 수 있도록 전달 경로를 결정하는 Network Device이다.

---

# 39. 집에서 Internet 사용

단순화하면:

```text
Computer / Smartphone
↓
Wi-Fi
↓
Router
↓
ISP
↓
Internet
```

같은 구조로 생각할 수 있다.

---

# 40. ISP

ISP는:

```text
Internet Service Provider
```

이다.

Internet 연결 서비스를 제공하는 사업자를 의미한다.

---

# 41. LAN

LAN은:

```text
Local Area Network
```

이다.

비교적 제한된 공간의 Network를 의미한다.

예:

```text
집

회사

학교
```

등의 내부 Network이다.

---

# 42. WAN

WAN은:

```text
Wide Area Network
```

이다.

더 넓은 지역의 Network를 연결하는 개념이다.

---

# 43. Private IP

집이나 회사 내부 Network에서 사용하는 Private IP Address가 있다.

대표적인 IPv4 Private Address 범위는:

```text
10.0.0.0/8

172.16.0.0/12

192.168.0.0/16
```

이다.

---

# 44. Public IP

Internet에서 외부와 통신할 때 Public IP Address가 사용될 수 있다.

가정 Network에서는 여러 Device가 Router를 통해 하나의 Public IPv4 Address를 공유하는 구조도 흔하다.

---

# 45. NAT

내부 Network의 Private Address와 외부 Network의 Address 사이에서 Address 정보를 변환하는 기술을:

```text
NAT
Network Address Translation
```

이라고 한다.

---

# 46. 집의 Network 예

```text
Notebook
192.168.0.10
        \
         \
Smartphone
192.168.0.11
          \
           Router
             ↓
          Public IP
             ↓
          Internet
```

처럼 생각할 수 있다.

---

# 47. 127.0.0.1

IPv4에서:

```text
127.0.0.1
```

은 대표적인 Loopback Address이다.

자기 Computer 자신과 통신할 때 사용한다.

---

# 48. localhost와 127.0.0.1

개발 환경에서는 흔히:

```text
localhost
```

가 Loopback Address로 해석된다.

그래서:

```text
http://localhost:8000
```

과:

```text
http://127.0.0.1:8000
```

을 모두 자주 볼 수 있다.

환경에 따라 `localhost`가 IPv6 `::1`로 해석될 수도 있다.

---

# 49. Server 실행 예

FastAPI를 실행했을 때:

```text
http://127.0.0.1:8000
```

같은 주소를 보게 될 수 있다.

의미는:

```text
127.0.0.1
→ 자기 Computer

8000
→ 해당 Server가 사용하는 Port
```

이다.

---

# 50. Domain

사람이 IP Address를 매번 기억하기는 어렵다.

그래서:

```text
google.com
example.com
```

같은 Domain Name을 사용한다.

---

# 51. DNS

DNS는:

```text
Domain Name System
```

이다.

Domain Name을 통신에 필요한 IP Address 정보로 찾는 과정에 사용된다.

단순화하면:

```text
example.com
↓
DNS 조회
↓
IP Address
```

이다.

---

# 52. Browser에 주소 입력

사용자가:

```text
https://example.com
```

을 입력한다.

단순화하면 다음과 같은 일이 일어난다.

```text
URL 확인
↓
Domain 확인
↓
DNS 조회
↓
Server IP 확인
↓
Server와 연결
↓
HTTPS 통신
↓
HTTP Request
↓
HTTP Response
↓
Browser Rendering
```

---

# 53. 실제 과정은 더 복잡하다

실제 Internet 통신에서는:

```text
DNS Cache

CDN

Proxy

Load Balancer

Firewall

TLS

HTTP Version

Browser Cache
```

등 다양한 요소가 개입할 수 있다.

하지만 처음에는 기본 흐름을 이해하는 것이 중요하다.

---

# 54. Frontend Server

HTML/CSS/JavaScript File을 제공하는 Server가 있을 수 있다.

```text
Browser
↓
Frontend Server
↓
HTML / CSS / JavaScript
```

---

# 55. Backend Server

API를 처리하는 Server가 따로 있을 수도 있다.

```text
Browser
↓
Backend API
↓
Database / AI Model
```

---

# 56. 전체 구조

```text
Browser
│
├── Frontend 요청
│       ↓
│   Frontend Server
│
└── API 요청
        ↓
    Backend Server
        ↓
    Database
        또는
    AI Model
```

---

# 57. 같은 Server에서 처리할 수도 있다

모든 Service가 반드시 여러 Server로 분리되어야 하는 것은 아니다.

작은 Application에서는 하나의 Server가 여러 역할을 담당할 수도 있다.

---

# 58. Network와 Backend

FastAPI를 실행한다는 것은 단순히 Python Function을 실행하는 것만을 의미하지 않는다.

외부 또는 Local Client의 Network Request를 받을 수 있는 Web Application을 실행하는 것이다.

---

# 59. FastAPI 예

```python
from fastapi import FastAPI


app = FastAPI()


@app.get("/")
def home():

    return {
        "message": "Hello"
    }
```

---

# 60. Browser Request

Browser가:

```text
GET /
```

Request를 보낸다.

---

# 61. FastAPI 처리

```python
@app.get("/")
```

에 해당하는 Route가 Request를 처리한다.

---

# 62. Response

FastAPI가:

```json
{
    "message": "Hello"
}
```

같은 Response를 반환한다.

---

# 63. 전체 흐름

```text
Browser
↓
Network
↓
HTTP Request
↓
FastAPI
↓
Python Function
↓
Response
↓
Network
↓
Browser
```

이다.

---

# 64. AI API

AI Service에서도 구조는 동일하다.

```text
Browser
↓
POST /predict
↓
Network
↓
FastAPI
↓
PyTorch
↓
Prediction
↓
JSON Response
↓
Network
↓
Browser
```

---

# 65. Network가 없다면

AI Model이 Server에 있어도 사용자의 Browser가 그 Server와 통신할 수 없다면 Web Service로 사용할 수 없다.

그래서 AI Web Service에도 Network 기초가 필요하다.

---

# 66. Database Server

Database도 별도 Network Service로 실행될 수 있다.

예:

```text
FastAPI
↓
Network
↓
Database Server
```

---

# 67. Port가 다른 이유

하나의 Computer에서:

```text
Frontend

Backend

Database
```

등 여러 Service가 동시에 실행될 수 있다.

각 Network Service는 서로 다른 Port를 사용할 수 있다.

예:

```text
Frontend
localhost:5500

FastAPI
localhost:8000

Database
localhost:5432
```

Port 번호는 실제 설정에 따라 달라진다.

---

# 68. Docker와 Network

Docker Container를 사용하면:

```text
Frontend Container

Backend Container

Database Container
```

등이 서로 Network를 통해 통신하는 구조를 만들 수 있다.

---

# 69. Docker Port

Docker에서는:

```text
Host Port
↔
Container Port
```

를 연결하는 개념도 배우게 된다.

예:

```text
8000:8000
```

---

# 70. Cloud Server

Application을 Internet에 공개하면 자신의 Computer가 아니라 Cloud Server 등에서 Backend를 실행할 수 있다.

```text
사용자
↓
Internet
↓
Cloud Server
↓
FastAPI
```

---

# 71. Localhost와 배포 차이

개발:

```text
localhost:8000
```

배포:

```text
https://api.example.com
```

처럼 바뀔 수 있다.

---

# 72. 하지만 기본 구조는 동일

```text
Client
↓
Request
↓
Server
↓
Response
↓
Client
```

이라는 기본 원리는 같다.

---

# 73. Network Error

Network가 연결되지 않으면 여러 Error가 발생할 수 있다.

예:

```text
Server가 실행되지 않음

잘못된 IP

잘못된 Port

DNS 문제

Firewall

Timeout

Internet 연결 문제
```

등이다.

---

# 74. Connection Refused

예를 들어:

```text
localhost:8000
```

에 Server가 실행되고 있지 않은데 접속하면 연결 거부와 관련된 Error를 볼 수 있다.

---

# 75. Timeout

Server나 Network에서 일정 시간 안에 Response를 받지 못하면 Timeout이 발생할 수 있다.

---

# 76. DNS 문제

Domain Name을 올바른 IP Address로 해석하지 못하면 Server에 접근하지 못할 수 있다.

---

# 77. Network 문제를 확인하는 관점

```text
Internet 연결은 되는가?

Domain은 올바른가?

DNS는 정상인가?

Server가 실행 중인가?

IP가 맞는가?

Port가 맞는가?

Protocol이 맞는가?

Firewall이 막고 있지 않은가?
```

등을 단계적으로 확인한다.

---

# 78. ping

Network 연결 상태를 확인할 때:

```bash
ping example.com
```

같은 명령을 사용할 수 있다.

다만 `ping`은 ICMP를 사용하며, Server가 ICMP 응답을 차단할 수도 있다.

따라서 Ping 실패만으로 Web Server가 죽었다고 단정할 수는 없다.

---

# 79. ipconfig

Windows에서는:

```powershell
ipconfig
```

를 사용하여 Network Interface의 IP 설정 등을 확인할 수 있다.

---

# 80. nslookup

DNS 조회를 확인할 때:

```powershell
nslookup example.com
```

을 사용할 수 있다.

---

# 81. curl

HTTP 통신을 직접 확인할 때:

```powershell
curl.exe https://example.com
```

을 사용할 수 있다.

---

# 82. 도구의 차이

```text
ipconfig
→ 내 Network 설정 확인

ping
→ ICMP 기반 연결 확인

nslookup
→ DNS 확인

curl
→ HTTP / HTTPS 통신 확인
```

정도로 구분할 수 있다.

---

# 83. Browser만 보지 않는 이유

Website가 안 열린다고 해서 항상 Browser 문제는 아니다.

문제는:

```text
Browser

DNS

Network

Server

HTTP

TLS
```

등 여러 단계에 있을 수 있다.

---

# 84. 문제를 구간으로 나눈다

```text
Client
↓
DNS
↓
Network
↓
Server
↓
Application
```

어느 부분에서 문제가 발생했는지 확인한다.

---

# 85. Web Developer에게 Network가 중요한 이유

Frontend와 Backend를 연결할 때:

```text
localhost

IP

Port

Domain

HTTP

HTTPS

CORS
```

같은 개념을 계속 만나기 때문이다.

---

# 86. AI Developer에게도 필요한 이유

Model을 Notebook에서 학습하는 것과 실제 사용자에게 Service하는 것은 다르다.

```text
Notebook
↓
Model

```

에서 끝나는 것이 아니라:

```text
User
↓
Network
↓
API
↓
Model
↓
Response
```

까지 연결해야 Service가 된다.

---

# 87. 지금까지 배운 기술 연결

```text
HTML / CSS / JavaScript
→ Client

FastAPI
→ Server Application

PyTorch
→ AI Model

SQLAlchemy
→ Database 연결

HTTP
→ Client / Server 통신

Network
→ 통신 기반
```

---

# 88. 전체 AI Service

```text
User
↓
Browser
↓
Frontend
↓
Network
↓
HTTP Request
↓
FastAPI
↓
PyTorch
↓
Prediction
↓
HTTP Response
↓
Network
↓
JavaScript
↓
DOM
↓
User
```

---

# 89. 핵심 질문

Network를 공부할 때 다음 질문을 계속 생각한다.

```text
누가 Client인가?

누가 Server인가?

어디로 연결하는가?

어떤 IP인가?

어떤 Port인가?

어떤 Protocol을 사용하는가?

무엇을 Request하는가?

무엇을 Response하는가?
```

---

# 90. 핵심 정리

```text
Network
→ Device들이 Data를 주고받을 수 있도록 연결된 구조

Internet
→ 전 세계 Network들이 연결된 거대한 Network

Client
→ Service를 요청하는 쪽

Server
→ Request를 처리하고 Response하는 쪽

IP
→ Network상의 Address

Port
→ Computer에서 Network Service를 구분

Protocol
→ 통신 규칙

DNS
→ Domain Name과 IP Address 정보를 연결하는 시스템

HTTP
→ Web Request / Response Protocol

HTTPS
→ TLS로 보호되는 HTTP 통신

localhost
→ 자기 Computer를 가리키는 Host Name
```

---

# 91. 가장 중요한 구조

```text
Client
↓
Network
↓
Server
```

그리고 Web에서는:

```text
Client
↓
HTTP Request
↓
Network
↓
Server
↓
HTTP Response
↓
Network
↓
Client
```

이다.

---

# 92. 한 문장으로 정리

Network는:

```text
Client와 Server를 포함한 여러 Computer와 Device가
IP Address, Port, Protocol 등의 규칙과 정보를 이용하여
서로 Data를 주고받을 수 있도록 연결하는 기반
```

이라고 이해할 수 있다.

---

# 93. 다음 단계

Network의 전체 그림을 이해했다면 다음에는:

```text
IP
Port
DNS
```

를 각각 더 자세히 공부한다.

그다음:

```text
URL
↓
HTTP
↓
Request / Response
↓
Method / Status Code
↓
HTTPS / TLS
↓
JSON
↓
API / REST
```

순서로 연결한다.