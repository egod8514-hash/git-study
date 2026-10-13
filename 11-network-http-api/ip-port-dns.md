# IP, Port, DNS

## 1. Network 통신에서 필요한 정보

Client가 Server와 통신하려면 최소한:

```text
어디에 있는 Server인가?

그 Server의 어떤 Service인가?

어떤 방식으로 통신할 것인가?
```

를 알아야 한다.

이를 이해하기 위해 중요한 개념이:

```text
IP Address

Port

DNS

Protocol
```

이다.

---

# 2. 가장 먼저 보는 예

```text
http://127.0.0.1:8000
```

이를 나누면:

```text
http
→ Protocol

127.0.0.1
→ IP Address

8000
→ Port
```

이다.

---

# 3. IP Address란?

IP는:

```text
Internet Protocol
```

의 약자이다.

IP Address는 IP Network에서 통신 대상 Interface를 식별하고 Data를 목적지로 전달하기 위해 사용하는 Address이다.

---

# 4. 쉽게 이해하면

처음에는:

```text
IP Address
→ Network에서 목적지를 찾기 위한 주소
```

라고 이해하면 된다.

---

# 5. 예

```text
192.168.0.10
```

같은 Address를 볼 수 있다.

---

# 6. IPv4

많이 볼 수 있는 형태:

```text
192.168.0.10
```

은 IPv4 Address 표현이다.

IPv4는:

```text
32 bit
```

Address를 사용한다.

---

# 7. IPv4 구조

IPv4 Address는 보통:

```text
192.168.0.10
```

처럼 4개의 숫자를 점으로 구분하여 표현한다.

각 부분은:

```text
0 ~ 255
```

범위로 표현된다.

---

# 8. IPv6

IPv4 Address 부족 등의 문제를 해결하기 위해 IPv6도 사용된다.

예:

```text
2001:db8::1
```

IPv6는:

```text
128 bit
```

Address를 사용한다.

---

# 9. 지금 당장 중요한 것

초기 Web 개발에서는 우선:

```text
IPv4

localhost

127.0.0.1

Private IP

Public IP
```

개념을 이해한다.

---

# 10. localhost

`localhost`는 일반적으로 현재 자기 Computer를 가리키는 Host Name이다.

예:

```text
http://localhost:8000
```

---

# 11. localhost의 의미

```text
다른 Server
```

를 찾는 것이 아니라:

```text
현재 내 Computer
```

에서 실행 중인 Service에 접근하려는 것이다.

---

# 12. 127.0.0.1

IPv4에서 대표적인 Loopback Address이다.

```text
127.0.0.1
```

은 자기 Computer 자신과 통신할 때 사용한다.

---

# 13. localhost와 127.0.0.1

개발 환경에서:

```text
localhost
```

가:

```text
127.0.0.1
```

또는 IPv6의:

```text
::1
```

같은 Loopback Address로 해석될 수 있다.

---

# 14. 그래서

다음 두 주소를 모두 자주 볼 수 있다.

```text
http://localhost:8000

http://127.0.0.1:8000
```

---

# 15. FastAPI와 연결

FastAPI 개발 Server를 실행하면:

```text
http://127.0.0.1:8000
```

같은 주소를 볼 수 있다.

의미:

```text
127.0.0.1
→ 현재 Computer

8000
→ FastAPI가 Listening 중인 Port
```

이다.

---

# 16. Loopback

Loopback은 Network Data가 외부 Network로 나가지 않고 자기 System으로 돌아오는 통신에 사용된다.

개발과 테스트에서 매우 유용하다.

---

# 17. Private IP

내부 Network에서 사용할 수 있도록 예약된 IP Address 범위가 있다.

IPv4의 대표적인 Private Address 범위:

```text
10.0.0.0/8

172.16.0.0/12

192.168.0.0/16
```

---

# 18. 집의 Network

예:

```text
Router
│
├── Notebook
│   192.168.0.10
│
├── Smartphone
│   192.168.0.11
│
└── TV
    192.168.0.12
```

처럼 각 Device에 내부 Address가 할당될 수 있다.

---

# 19. 같은 LAN에서 접근

Computer에서 Server를 다음과 같이 실행했다고 하자.

```text
192.168.0.10:8000
```

조건이 맞으면 같은 Local Network의 Smartphone 등이:

```text
http://192.168.0.10:8000
```

으로 접근할 수도 있다.

---

# 20. 하지만 localhost는 다르다

Smartphone에서:

```text
localhost
```

를 입력하면 Computer를 의미하지 않는다.

Smartphone 자기 자신을 의미한다.

---

# 21. 매우 중요한 부분

```text
localhost
=
현재 그 주소를 사용하는 Device 자신
```

이라고 이해한다.

따라서:

```text
PC의 localhost
→ PC

Smartphone의 localhost
→ Smartphone
```

이다.

---

# 22. 같은 Wi-Fi라고 무조건 되는 것은 아니다

다른 Device에서 PC의 Server에 접근하려면:

```text
Server Bind Address

Firewall

Network 설정

Port

Router / AP 설정
```

등도 영향을 줄 수 있다.

---

# 23. 0.0.0.0

Server 실행 과정에서:

```text
0.0.0.0
```

을 볼 수 있다.

예:

```bash
uvicorn main:app --host 0.0.0.0 --port 8000
```

---

# 24. 의미

Server의 Bind Address로 사용되는 `0.0.0.0`은 일반적으로 사용 가능한 IPv4 Network Interface들에서 Connection을 받을 수 있도록 Listening하겠다는 의미이다.

---

# 25. 주의

`0.0.0.0`은 일반적인 Client 접속 목적지 주소로 사용하는 개념과는 다르다.

Server가:

```text
0.0.0.0:8000
```

에서 Listening한다고 해서 다른 Device가 Browser에 반드시:

```text
http://0.0.0.0:8000
```

을 입력해야 한다는 의미가 아니다.

---

# 26. 다른 Device에서는

Server Computer의 실제 접근 가능한 IP를 사용한다.

예:

```text
http://192.168.0.10:8000
```

---

# 27. Public IP

Internet에서 외부와 통신할 때 Public IP Address가 사용된다.

집에서는 Router가 Public IPv4 Address를 가지고 여러 내부 Device가 이를 공유하는 구조를 흔히 볼 수 있다.

---

# 28. Private vs Public

```text
Private IP
→ 내부 Network에서 사용

Public IP
→ Internet과의 통신에서 사용 가능한 전역 Address
```

정도로 먼저 이해한다.

---

# 29. NAT

NAT는:

```text
Network Address Translation
```

의 약자이다.

Address 정보를 변환하는 기술이다.

---

# 30. 가정 Network의 단순화

```text
Notebook
192.168.0.10
       \
        \
Smartphone
192.168.0.11
         \
          Router
          NAT
           ↓
       Public IP
           ↓
        Internet
```

처럼 이해할 수 있다.

---

# 31. 왜 Private IP를 그대로 Internet에서 찾지 못할까?

Private IP 범위는 Public Internet에서 직접 Routing하기 위한 Address로 사용하지 않는다.

그래서:

```text
192.168.0.10
```

같은 주소는 서로 다른 집이나 회사에서 동시에 사용될 수 있다.

---

# 32. IP만 있으면 충분할까?

아니다.

한 Computer에서는 여러 Network Service가 동시에 실행될 수 있다.

예:

```text
Web Server

FastAPI

Database

SSH
```

등이다.

그래서:

```text
Port
```

개념이 필요하다.

---

# 33. Port란?

Port는 Network 통신에서 하나의 Host 내부의 특정 Network Service/Process와 연결되는 논리적 번호이다.

---

# 34. 쉽게 이해하면

```text
IP
→ 어느 Host인가?

Port
→ 그 Host의 어느 Service인가?
```

라고 생각한다.

---

# 35. 비유

```text
IP Address
→ 건물 주소

Port
→ 건물 안의 서비스 창구
```

처럼 비유할 수 있다.

완전히 동일한 개념은 아니지만 처음 이해하기에는 도움이 된다.

---

# 36. 예

```text
127.0.0.1:8000
```

에서:

```text
127.0.0.1
→ 내 Computer

8000
→ 8000번 Port
```

이다.

---

# 37. 여러 Service

한 Computer에서:

```text
Frontend
→ 5500

FastAPI
→ 8000

Database
→ 5432
```

처럼 서로 다른 Port를 사용할 수 있다.

실제 Port는 설정에 따라 달라진다.

---

# 38. Port 범위

TCP/UDP Port 번호는:

```text
0 ~ 65535
```

범위를 사용한다.

---

# 39. Well-Known Ports

일부 Service는 관례적으로 잘 알려진 Port 번호를 사용한다.

예:

```text
HTTP
→ 80

HTTPS
→ 443

SSH
→ 22
```

---

# 40. Database Port 예

대표적인 기본 Port 예:

```text
PostgreSQL
→ 5432

MySQL
→ 3306

MongoDB
→ 27017
```

설정에 따라 변경할 수 있다.

---

# 41. 기본 Port 생략

Browser에서:

```text
http://example.com
```

이라고 작성하면 HTTP의 기본 Port인:

```text
80
```

을 사용하는 것으로 처리될 수 있다.

---

# 42. HTTPS

```text
https://example.com
```

에서는 기본적으로:

```text
443
```

Port를 사용한다.

---

# 43. 그래서 URL에 Port가 안 보일 수 있다

```text
https://example.com
```

에서 `:443`이 보이지 않아도 기본 Port를 사용하고 있을 수 있다.

---

# 44. 개발에서는 Port가 자주 보인다

```text
localhost:3000

localhost:5173

localhost:5500

localhost:8000
```

처럼 개발 Server는 여러 Port를 사용할 수 있다.

---

# 45. Port 충돌

두 Program이 같은 IP/Port 조합에 동시에 Bind하려고 하면 충돌이 발생할 수 있다.

예:

```text
Port 8000 is already in use
```

같은 Error를 볼 수 있다.

---

# 46. 해결 방법

```text
기존 Server 종료

또는

다른 Port 사용
```

을 고려한다.

예:

```bash
uvicorn main:app --port 8001
```

---

# 47. Server가 Port를 연다는 말

정확히는 Application이 특정 Address와 Port에 Bind하고 Connection을 받을 수 있도록 Listening하는 구조를 말하는 경우가 많다.

---

# 48. Listening

Server:

```text
Port 8000에서 Request 기다림
```

Client:

```text
Port 8000으로 연결
```

이라고 생각하면 된다.

---

# 49. Client도 Port를 사용한다

Client 역시 Network 연결에서 Port를 사용한다.

Browser가 Server의 443번 Port에 연결할 때 Client 측에는 운영체제가 임시 Port를 할당할 수 있다.

---

# 50. 예

단순화하면:

```text
Client
192.168.0.10:53000

        ↓

Server
203.0.113.10:443
```

처럼 양쪽에 IP와 Port가 존재한다.

---

# 51. 중요한 구분

우리가 URL에서 주로 신경 쓰는 Port는:

```text
접속하려는 Server의 Port
```

이다.

---

# 52. IP를 외우기 어렵다

예를 들어 Website에 접속할 때마다:

```text
142.x.x.x
```

같은 IP Address를 외워야 한다면 불편하다.

그래서:

```text
Domain Name
```

을 사용한다.

---

# 53. Domain Name

예:

```text
example.com

google.com
```

처럼 사람이 읽고 기억하기 쉬운 이름이다.

---

# 54. DNS

DNS는:

```text
Domain Name System
```

이다.

Domain Name과 IP Address 등의 정보를 연결하여 찾을 수 있도록 하는 분산 시스템이다.

---

# 55. 단순화

```text
example.com
↓
DNS
↓
IP Address
```

라고 이해할 수 있다.

---

# 56. 왜 DNS가 필요한가?

사람은:

```text
example.com
```

같은 이름을 기억하기 쉽다.

Computer Network 통신에는 목적지 Address 정보가 필요하다.

DNS가 그 연결을 돕는다.

---

# 57. Browser에서 Domain 입력

```text
https://example.com
```

을 입력했다고 하자.

단순화하면:

```text
1. Domain 확인

2. DNS 조회

3. IP Address 확인

4. Server 연결

5. HTTPS 통신

6. HTTP Request

7. HTTP Response
```

순서로 생각할 수 있다.

---

# 58. DNS Server

DNS 정보를 질의하고 응답하는 Server들이 존재한다.

사용자의 Device는 설정된 DNS Resolver를 통해 필요한 정보를 조회할 수 있다.

---

# 59. DNS는 하나의 Server가 아니다

전 세계 Domain 정보를 한 Computer가 모두 처리하는 구조가 아니다.

DNS는 계층적이고 분산된 System이다.

---

# 60. DNS 계층

단순화하면:

```text
Root
↓
TLD
↓
Authoritative DNS
```

같은 계층 구조가 존재한다.

---

# 61. TLD

Top-Level Domain이다.

예:

```text
.com

.net

.org

.kr
```

등이다.

---

# 62. Domain 구조

예:

```text
api.example.com
```

을 보면:

```text
com
→ TLD

example
→ Domain

api
→ Subdomain
```

처럼 이해할 수 있다.

---

# 63. Subdomain

하나의 Domain에서 Service를 구분할 때 사용할 수 있다.

예:

```text
www.example.com

api.example.com

admin.example.com
```

---

# 64. AI Service 예

```text
www.example.com
→ Frontend

api.example.com
→ Backend API
```

처럼 구성할 수도 있다.

---

# 65. DNS Record

DNS에는 여러 종류의 Record가 있다.

대표적으로:

```text
A

AAAA

CNAME

MX
```

등이 있다.

---

# 66. A Record

Domain Name을 IPv4 Address와 연결하는 데 사용한다.

---

# 67. AAAA Record

Domain Name을 IPv6 Address와 연결하는 데 사용한다.

---

# 68. CNAME

한 Domain Name을 다른 Domain Name의 별칭으로 연결할 때 사용한다.

---

# 69. MX

Email Server와 관련된 DNS Record이다.

---

# 70. 지금 단계에서 가장 중요한 것

모든 DNS Record를 외울 필요는 없다.

우선:

```text
Domain
↓
DNS
↓
IP
↓
Server
```

흐름을 이해한다.

---

# 71. DNS Cache

매번 같은 Domain을 처음부터 조회하면 비효율적이다.

그래서:

```text
Browser

Operating System

DNS Resolver
```

등 여러 단계에서 DNS 결과가 Cache될 수 있다.

---

# 72. DNS 변경이 바로 안 보이는 이유

Domain 설정을 변경했는데 바로 반영되지 않는 것처럼 보이는 경우 DNS Cache와 TTL 등이 영향을 줄 수 있다.

---

# 73. TTL

DNS Record에는 Cache가 얼마 동안 유효한지와 관련된:

```text
TTL
Time To Live
```

값이 있다.

---

# 74. Windows에서 내 IP 확인

PowerShell:

```powershell
ipconfig
```

---

# 75. 결과 예

다음과 같은 정보를 볼 수 있다.

```text
IPv4 Address

Subnet Mask

Default Gateway
```

---

# 76. IPv4 Address

예:

```text
192.168.0.10
```

이면 Local Network에서 해당 Interface가 사용하는 Address일 수 있다.

---

# 77. Default Gateway

Local Network 밖으로 Data를 전달할 때 사용하는 Gateway가 표시될 수 있다.

가정에서는 Router가 이 역할을 하는 경우가 많다.

---

# 78. DNS 확인

Windows에서는:

```powershell
nslookup example.com
```

을 사용할 수 있다.

---

# 79. nslookup의 목적

```text
이 Domain이 어떤 DNS 정보를 반환하는가?
```

를 확인하는 데 사용할 수 있다.

---

# 80. 예

```powershell
nslookup google.com
```

실행 결과에서 DNS Server와 Address 정보 등을 볼 수 있다.

---

# 81. ping

```powershell
ping example.com
```

으로 Network 도달 가능성을 확인할 수 있다.

---

# 82. 하지만

Ping은:

```text
HTTP Test
```

가 아니다.

ICMP를 사용한다.

---

# 83. 따라서

```text
ping 성공
```

이:

```text
Website 정상
```

을 반드시 의미하지 않는다.

반대로 Server가 ICMP를 차단하면 Website는 정상인데 Ping은 실패할 수도 있다.

---

# 84. HTTP 확인

Web Server가 실제 HTTP Response를 보내는지 확인하려면:

```powershell
curl.exe https://example.com
```

같은 방법이 더 직접적이다.

---

# 85. 도구별 역할

```text
ipconfig
→ 내 Network 설정

nslookup
→ DNS 조회

ping
→ ICMP 기반 Network 확인

curl
→ HTTP / HTTPS 확인
```

이다.

---

# 86. Website가 안 열릴 때

단순히:

```text
Internet이 안 된다.
```

라고 판단하기 전에 구간을 나눠 본다.

---

# 87. 1단계

다른 Website는 열리는가?

```text
YES
→ 전체 Internet 연결 문제일 가능성은 낮아짐

NO
→ Local Network / Internet 연결부터 확인
```

---

# 88. 2단계

Domain을 확인한다.

```powershell
nslookup example.com
```

---

# 89. 3단계

HTTP/HTTPS 통신을 확인한다.

```powershell
curl.exe https://example.com
```

---

# 90. 4단계

Browser에서만 문제가 발생하는지도 확인한다.

```text
Browser Cache

Extension

Proxy

Certificate

Browser 설정
```

등도 원인이 될 수 있다.

---

# 91. FastAPI 접속 문제

예:

```text
http://127.0.0.1:8000
```

이 안 열린다고 하자.

확인:

```text
FastAPI Server가 실행 중인가?

8000번 Port가 맞는가?

127.0.0.1에 Bind했는가?

Terminal에 Error가 있는가?
```

---

# 92. Smartphone에서 FastAPI 접속

PC에서:

```text
127.0.0.1:8000
```

으로 Server를 실행했다고 하자.

Smartphone에서:

```text
127.0.0.1:8000
```

을 입력하면 PC로 가지 않는다.

Smartphone 자신을 가리킨다.

---

# 93. 필요한 구조

예:

```text
PC
192.168.0.10

Smartphone
192.168.0.11
```

이라면 Smartphone에서는 조건이 맞을 때:

```text
http://192.168.0.10:8000
```

으로 PC에 접근한다.

---

# 94. Server Bind도 확인

PC의 Server가 외부 Interface에서도 Connection을 받을 수 있도록 설정되어 있어야 한다.

예:

```bash
uvicorn main:app \
    --host 0.0.0.0 \
    --port 8000
```

---

# 95. Firewall

Windows Firewall 등이:

```text
8000번 Port의 Inbound Connection
```

을 막고 있다면 다른 Device에서 접근하지 못할 수 있다.

---

# 96. 그래서 접속 문제는

```text
IP

Port

Server Bind

Firewall

Network
```

을 함께 확인해야 한다.

---

# 97. IP와 Domain의 관계

```text
Domain
example.com

↓

DNS

↓

IP
203.0.113.x
```

처럼 Domain을 이용하여 필요한 Address 정보를 찾는다.

---

# 98. IP와 Port의 관계

```text
IP
→ 어느 Host?

Port
→ 그 Host의 어느 Network Service?
```

이다.

---

# 99. Domain과 Port

Domain 뒤에도 Port를 지정할 수 있다.

```text
example.com:8000
```

---

# 100. URL 전체 예

```text
http://localhost:8000/products
```

분해하면:

```text
http
→ Protocol / Scheme

localhost
→ Host Name

8000
→ Port

/products
→ Path
```

이다.

---

# 101. Domain을 사용하는 경우

```text
https://api.example.com/predict
```

분해하면:

```text
https
→ Scheme

api.example.com
→ Host

443
→ 기본 HTTPS Port라면 생략 가능

/predict
→ Path
```

이다.

---

# 102. AI API

Local 개발:

```text
http://127.0.0.1:8000/predict
```

배포 후:

```text
https://api.example.com/predict
```

처럼 바뀔 수 있다.

---

# 103. 달라지는 것

```text
Host

Protocol

Port
```

등은 환경에 따라 달라질 수 있다.

---

# 104. 변하지 않는 핵심

```text
Client가 특정 Host의 특정 Service로
Request를 보낸다.
```

는 기본 구조는 같다.

---

# 105. Docker와 Port

Docker에서는 다음과 같은 설정을 볼 수 있다.

```text
8000:8000
```

---

# 106. 의미

단순화하면:

```text
Host의 8000
↓
Container의 8000
```

으로 연결한다.

---

# 107. 예

```text
Browser
↓
localhost:8000
↓
Host Port 8000
↓
Docker
↓
Container Port 8000
↓
FastAPI
```

---

# 108. Database와 IP/Port

FastAPI가 Database Server에 연결할 때도 같은 개념이 적용된다.

```text
FastAPI
↓
Database Host
↓
Database Port
↓
Database
```

---

# 109. PostgreSQL 예

```text
localhost:5432
```

이라면:

```text
localhost
→ 현재 Computer

5432
→ PostgreSQL에서 흔히 사용하는 기본 Port
```

이다.

---

# 110. Connection String

Database에서는 다음과 같은 주소를 볼 수 있다.

```text
postgresql://user:password@localhost:5432/mydb
```

여기에도:

```text
Host
Port
Database
```

정보가 들어 있다.

---

# 111. Frontend와 Backend

Frontend:

```text
http://localhost:5500
```

Backend:

```text
http://localhost:8000
```

일 수 있다.

---

# 112. 같은 localhost인데 다른 Service

둘 다 같은 Computer지만:

```text
5500

8000
```

이라는 다른 Port를 사용하므로 서로 다른 Network Endpoint이다.

---

# 113. Origin과 연결

Browser의 Origin은 기본적으로:

```text
Scheme
+
Host
+
Port
```

로 구분한다.

---

# 114. 따라서

```text
http://localhost:5500
```

과:

```text
http://localhost:8000
```

은 Port가 다르므로 다른 Origin이다.

---

# 115. CORS와 연결

Frontend와 Backend가 서로 다른 Origin에 있다면 Browser의 Cross-Origin 정책과 CORS 설정을 만나게 될 수 있다.

즉 Port 개념은 CORS를 이해할 때도 중요하다.

---

# 116. Endpoint

Network/API 공부에서:

```text
Endpoint
```

라는 말을 자주 사용한다.

API에서는 Client가 Request를 보낼 수 있는 특정 API 접근 지점을 의미하는 식으로 사용한다.

예:

```text
GET /products

POST /predict
```

---

# 117. Network Endpoint 관점

더 넓은 Network 관점에서는:

```text
IP Address
+
Port
```

조합을 Endpoint로 설명하기도 한다.

문맥을 보고 이해한다.

---

# 118. 가장 중요한 질문

주소를 볼 때 다음을 나눠 본다.

```text
Protocol은?

Host는?

IP인가 Domain인가?

Port는?

Path는?
```

---

# 119. 예

```text
http://127.0.0.1:8000/users
```

```text
Protocol
→ HTTP

Host
→ 127.0.0.1

Port
→ 8000

Path
→ /users
```

---

# 120. 예

```text
https://api.example.com/predict
```

```text
Protocol
→ HTTPS

Host
→ api.example.com

Port
→ 기본 HTTPS Port라면 443

Path
→ /predict
```

---

# 121. 문제 해결 관점

접속이 안 된다면:

```text
Host가 맞는가?

DNS가 정상인가?

IP가 맞는가?

Port가 맞는가?

Server가 Listening 중인가?

Firewall이 막는가?

Protocol이 맞는가?
```

순서로 생각할 수 있다.

---

# 122. 핵심 정리

```text
IP Address
→ Network에서 목적지를 식별하는 Address

127.0.0.1
→ IPv4 Loopback Address

localhost
→ 자기 Computer를 가리키는 Host Name

Private IP
→ 내부 Network에서 사용하는 Address

Public IP
→ Internet에서 사용할 수 있는 전역 Address

Port
→ Host 내부 Network Service를 구분

DNS
→ Domain Name과 IP 등의 정보를 연결하는 시스템

Domain
→ 사람이 사용하기 쉬운 이름

0.0.0.0
→ Server Bind에서 모든 IPv4 Interface를 의미할 수 있음
```

---

# 123. 한 번에 연결

```text
https://api.example.com:443/predict
```

를 보면:

```text
HTTPS
↓
api.example.com
↓
DNS
↓
Server IP
↓
Port 443
↓
/predict
↓
Backend API
```

처럼 연결해서 생각한다.

---

# 124. Local 개발에서는

```text
http://127.0.0.1:8000/predict
```

```text
127.0.0.1
→ 내 Computer

8000
→ FastAPI Port

/predict
→ API Path
```

이다.

---

# 125. 가장 중요한 구분

```text
IP
→ 어디에 있는 Host인가?

Port
→ 그 Host의 어느 Service인가?

DNS
→ Domain 이름으로 필요한 Address 정보를 어떻게 찾는가?
```

이다.

---

# 126. 한 문장으로 정리

IP, Port, DNS는:

```text
DNS를 통해 사람이 사용하는 Domain Name에서
통신에 필요한 Address 정보를 찾고,

IP를 이용해 Network상의 목적지를 식별하며,

Port를 이용해 해당 Host에서
어떤 Network Service와 통신할지를 구분하는
Network 통신의 핵심 개념
```

이라고 이해할 수 있다.

---

# 127. 다음 단계

이제:

```text
IP
Port
DNS
```

를 이해했으므로 다음에는:

```text
https://api.example.com:443/predict?id=10
```

같은 주소 전체가 무엇을 의미하는지:

```text
URL
```

을 자세히 공부한다.