# Database 기초

## 1. Database란?

Database는 데이터를 체계적으로 저장하고 관리하기 위한 데이터 저장소이다.

예를 들어 쇼핑몰 서비스를 만든다고 생각해보자.

쇼핑몰에는 다음과 같은 데이터가 필요하다.

```text
회원 정보
상품 정보
주문 정보
결제 정보
배송 정보
리뷰 정보
```

이 데이터를 단순한 변수에 저장하면 프로그램이 종료될 때 사라질 수 있다.

따라서 데이터를 지속적으로 저장하고 필요할 때 다시 조회할 수 있는 공간이 필요하다.

그 역할을 하는 것이 Database이다.

---

# 2. Database가 필요한 이유

예를 들어 Python에서:

```python
name = "Kim"
age = 30
```

이라고 데이터를 저장했다고 하자.

이 값은 현재 실행 중인 프로그램에서는 사용할 수 있다.

하지만 프로그램이 종료되면 일반적으로 메모리에 있던 데이터는 사라진다.

실제 서비스에서는:

```text
회원가입
    ↓
프로그램 종료
    ↓
다음 날 다시 로그인
```

해도 회원 정보가 남아 있어야 한다.

그래서 데이터를 파일이나 Database 등에 영구적으로 저장한다.

---

# 3. 파일로 저장하면 안 될까?

CSV 파일에도 데이터를 저장할 수 있다.

예:

```text
users.csv

id,name,age
1,Kim,30
2,Lee,25
3,Park,40
```

작은 데이터 분석에서는 CSV만으로도 충분할 수 있다.

하지만 실제 서비스에서는:

```text
여러 사용자가 동시에 데이터 접근

특정 데이터 빠르게 검색

데이터 수정

데이터 삭제

여러 데이터 사이의 관계 관리

권한 관리

데이터 무결성 관리

트랜잭션 처리
```

등이 필요하다.

이러한 작업을 전문적으로 관리하는 것이 Database 시스템이다.

---

# 4. DB와 DBMS

두 용어를 구분해보자.

```text
DB
Database

DBMS
Database Management System
```

Database는:

```text
저장되어 있는 데이터
```

라고 볼 수 있다.

DBMS는:

```text
Database를 관리하는 프로그램
```

이다.

---

# 5. DBMS 예시

대표적인 DBMS:

```text
MySQL

PostgreSQL

Oracle Database

Microsoft SQL Server

SQLite
```

등이 있다.

MongoDB처럼 관계형 구조와 다른 방식으로 데이터를 관리하는 Database 시스템도 있다.

---

# 6. 관계형 Database

많이 사용하는 Database 방식 중 하나가:

```text
Relational Database
관계형 데이터베이스
```

이다.

줄여서:

```text
RDB
```

라고 부르기도 한다.

관계형 Database에서는 데이터를 주로 Table 형태로 관리한다.

---

# 7. RDBMS

RDBMS는:

```text
Relational Database Management System
```

의 약자이다.

즉:

```text
관계형 Database를 관리하는 시스템
```

이다.

대표적으로:

```text
MySQL
PostgreSQL
Oracle
SQL Server
SQLite
```

등이 있다.

---

# 8. Table

관계형 Database에서 데이터는 Table 형태로 저장된다.

예:

```text
users
```

| id | name | age |
|---:|---|---:|
| 1 | Kim | 30 |
| 2 | Lee | 25 |
| 3 | Park | 40 |

Pandas DataFrame과 매우 비슷하게 생겼다.

---

# 9. Column

세로 방향의 항목을 Column이라고 한다.

예:

```text
id
name
age
```

각 Column은 데이터가 어떤 의미를 가지는지 나타낸다.

Pandas에서도:

```python
df.columns
```

를 사용했었다.

Database Table에서도 비슷한 개념이 존재한다.

---

# 10. Row

Table의 가로 한 줄을 Row라고 한다.

예:

```text
1 | Kim | 30
```

이 한 줄은 한 명의 사용자 정보를 나타낸다.

다른 표현으로:

```text
Record
```

라고 부르기도 한다.

---

# 11. Table 구조

다음 Table을 보자.

| id | name | age |
|---:|---|---:|
| 1 | Kim | 30 |
| 2 | Lee | 25 |

여기서:

```text
Table
→ users

Column
→ id, name, age

Row
→ 사용자 한 명의 데이터

Value
→ Kim, 30 등 실제 값
```

이다.

---

# 12. Pandas와 비교

Pandas에서:

```python
import pandas as pd

df = pd.DataFrame({
    "id": [1, 2],
    "name": ["Kim", "Lee"],
    "age": [30, 25]
})
```

결과:

```text
   id  name  age
0   1   Kim   30
1   2   Lee   25
```

Database Table과 매우 비슷하다.

---

# 13. Pandas DataFrame과 Database Table

개념적으로 비교하면:

| Pandas | Database |
|---|---|
| DataFrame | Table |
| Column | Column |
| Row | Row / Record |
| Index | 식별에 사용될 수 있는 별도 개념 |
| Filtering | WHERE |
| sort_values | ORDER BY |
| groupby | GROUP BY |
| merge | JOIN |

완전히 같은 것은 아니지만 기존 Pandas 지식과 연결하면 SQL을 이해하기 쉬워진다.

---

# 14. Schema

Schema는 Database의 구조를 정의하는 개념이다.

예를 들어 users Table이:

```text
id
name
age
email
```

Column을 가지고 있고 각각:

```text
id
→ INTEGER

name
→ VARCHAR

age
→ INTEGER

email
→ VARCHAR
```

형태로 저장된다는 구조를 정의할 수 있다.

---

# 15. Data Type

Database의 Column에도 자료형이 있다.

대표적으로:

```text
INTEGER
→ 정수

FLOAT / REAL / DECIMAL
→ 실수

VARCHAR / TEXT
→ 문자열

DATE
→ 날짜

DATETIME / TIMESTAMP
→ 날짜 + 시간

BOOLEAN
→ True / False 형태
```

DBMS마다 정확한 자료형 이름과 동작에는 차이가 있을 수 있다.

---

# 16. 왜 자료형이 중요할까?

예를 들어:

```text
age
```

Column이 문자열로 저장되어 있다면 숫자 계산이나 정렬에서 문제가 발생할 수 있다.

Pandas에서:

```text
Age가 object인데
실제로는 숫자여야 함
```

을 확인했던 것과 비슷하다.

Database에서도 Column에 적절한 자료형을 설정하는 것이 중요하다.

---

# 17. NULL

Database에서는 값이 없음을:

```text
NULL
```

로 표현할 수 있다.

Pandas에서 자주 봤던:

```text
NaN
```

과 비슷한 역할을 생각할 수 있다.

하지만 `NULL`과 숫자 `0`, 빈 문자열 `''`은 서로 다른 의미이다.

---

# 18. NULL과 0

예를 들어:

```text
score = 0
```

은 실제 점수가 0이라는 뜻일 수 있다.

반면:

```text
score = NULL
```

은 값이 없거나 알려지지 않았다는 의미일 수 있다.

따라서 둘을 구분해야 한다.

---

# 19. Primary Key

Database에서 매우 중요한 개념이다.

Primary Key는 Table의 각 Row를 고유하게 식별하기 위한 값이다.

예:

| user_id | name |
|---:|---|
| 1 | Kim |
| 2 | Lee |
| 3 | Park |

여기서:

```text
user_id
```

를 Primary Key로 사용할 수 있다.

---

# 20. Primary Key가 필요한 이유

이름만으로 사용자를 구분한다고 생각해보자.

```text
Kim
Kim
Kim
```

동일한 이름을 가진 사용자가 여러 명 존재할 수 있다.

따라서:

```text
user_id = 1
user_id = 2
user_id = 3
```

처럼 고유한 식별자를 사용한다.

---

# 21. Primary Key 특징

일반적으로 Primary Key는:

```text
중복될 수 없음

NULL이 될 수 없음

각 Row를 고유하게 구분
```

하는 역할을 한다.

예:

```text
user_id = 1001
```

을 이용하면 특정 사용자를 정확하게 찾을 수 있다.

---

# 22. 중복 Primary Key

다음은 문제가 된다.

| user_id | name |
|---:|---|
| 1 | Kim |
| 1 | Lee |

`user_id = 1`이 두 명이기 때문이다.

Primary Key는 각 Row를 고유하게 구분해야 한다.

---

# 23. Foreign Key

Foreign Key는 다른 Table의 Row와 관계를 연결하기 위해 사용하는 Key이다.

예를 들어:

```text
users
orders
```

두 Table이 있다고 하자.

---

# 24. users Table

| user_id | name |
|---:|---|
| 1 | Kim |
| 2 | Lee |

---

# 25. orders Table

| order_id | user_id | product |
|---:|---:|---|
| 101 | 1 | Keyboard |
| 102 | 1 | Mouse |
| 103 | 2 | Monitor |

orders의:

```text
user_id
```

는 어떤 사용자가 주문했는지를 나타낸다.

---

# 26. Primary Key와 Foreign Key 관계

users:

```text
user_id
→ Primary Key
```

orders:

```text
user_id
→ Foreign Key
```

개념적으로:

```text
users
----------------
user_id PK
name
     │
     │
     └─────────────┐
                   ↓
orders
----------------
order_id PK
user_id FK
product
```

처럼 연결된다.

---

# 27. 왜 Table을 나눌까?

사용자와 주문 정보를 하나의 Table에 모두 넣을 수도 있다.

예:

| user_id | name | order_id | product |
|---:|---|---:|---|
| 1 | Kim | 101 | Keyboard |
| 1 | Kim | 102 | Mouse |
| 1 | Kim | 104 | Laptop |

그러면:

```text
Kim
Kim
Kim
```

처럼 같은 사용자 정보가 반복된다.

---

# 28. Table 분리

사용자:

```text
users
```

| user_id | name |
|---:|---|
| 1 | Kim |

주문:

```text
orders
```

| order_id | user_id | product |
|---:|---:|---|
| 101 | 1 | Keyboard |
| 102 | 1 | Mouse |
| 104 | 1 | Laptop |

처럼 나누면 사용자 정보를 반복해서 저장할 필요가 줄어든다.

---

# 29. 관계

관계형 Database에서는 Table 사이에 관계를 만들 수 있다.

대표적인 관계:

```text
1 : 1

1 : N

N : M
```

이다.

---

# 30. 1 : 1 관계

한 Row가 다른 Table의 한 Row와 연결되는 관계이다.

예:

```text
사용자
↕
사용자 상세 프로필
```

한 사용자에게 하나의 상세 프로필만 존재하는 구조라면 1:1 관계로 설계할 수 있다.

---

# 31. 1 : N 관계

가장 자주 볼 수 있는 관계 중 하나이다.

예:

```text
사용자 1명
    ↓
여러 주문
```

즉:

```text
users 1
:
orders N
```

이다.

한 사용자가 여러 주문을 할 수 있다.

---

# 32. N : M 관계

예:

```text
학생
↔
수업
```

한 학생은 여러 수업을 들을 수 있고 한 수업에도 여러 학생이 참여할 수 있다.

이것이:

```text
N : M
```

관계이다.

---

# 33. N : M은 어떻게 표현할까?

중간 Table을 만드는 경우가 많다.

예:

```text
students
classes
student_classes
```

student_classes:

| student_id | class_id |
|---:|---:|
| 1 | 10 |
| 1 | 20 |
| 2 | 10 |

이 Table이 학생과 수업 사이의 관계를 연결한다.

---

# 34. 관계를 왜 사용할까?

실제 서비스의 데이터는 서로 연결되어 있기 때문이다.

쇼핑몰:

```text
회원
 ↓
주문
 ↓
주문상품
 ↓
상품
```

병원:

```text
환자
 ↓
진료
 ↓
검사
 ↓
처방
```

웹 서비스:

```text
사용자
 ↓
게시글
 ↓
댓글
```

등의 관계를 표현할 수 있다.

---

# 35. SQL

관계형 Database에서 데이터를 다루기 위해 많이 사용하는 언어가:

```text
SQL
```

이다.

SQL은:

```text
Structured Query Language
```

의 약자이다.

---

# 36. SQL로 무엇을 할까?

SQL을 이용하여:

```text
데이터 조회

데이터 추가

데이터 수정

데이터 삭제

Table 생성

Table 구조 변경

데이터 집계

Table 연결
```

등을 할 수 있다.

---

# 37. Query

Database에:

```text
이 데이터를 보여줘.

이 데이터를 추가해줘.

이 데이터를 수정해줘.
```

같은 명령을 전달하는 것을 Query라고 부른다.

예:

```sql
SELECT *
FROM users;
```

의미:

```text
users Table의 데이터를 조회해줘.
```

이다.

---

# 38. CRUD

웹 개발과 Database에서 매우 자주 등장하는 개념이다.

CRUD는:

```text
Create

Read

Update

Delete
```

의 앞글자를 따서 만든 용어이다.

---

# 39. Create

새로운 데이터를 생성한다.

예:

```text
회원가입

새 게시글 작성

새 주문 생성
```

SQL에서는 대표적으로:

```sql
INSERT
```

를 사용한다.

---

# 40. Read

데이터를 조회한다.

예:

```text
회원 정보 조회

상품 목록 조회

게시글 조회
```

SQL에서는:

```sql
SELECT
```

를 사용한다.

---

# 41. Update

기존 데이터를 수정한다.

예:

```text
회원 이름 수정

배송지 수정

게시글 수정
```

SQL에서는:

```sql
UPDATE
```

를 사용한다.

---

# 42. Delete

기존 데이터를 삭제한다.

예:

```text
회원 탈퇴

게시글 삭제

장바구니 상품 삭제
```

SQL에서는:

```sql
DELETE
```

를 사용한다.

---

# 43. CRUD와 SQL

정리하면:

| CRUD | 의미 | SQL |
|---|---|---|
| Create | 생성 | INSERT |
| Read | 조회 | SELECT |
| Update | 수정 | UPDATE |
| Delete | 삭제 | DELETE |

이 관계는 앞으로 웹 개발과 API에서도 계속 등장한다.

---

# 44. Database와 웹 서비스

웹사이트에서 사용자가 로그인한다고 생각해보자.

```text
사용자
   ↓
로그인 요청
   ↓
Backend
   ↓
Database
```

Backend는 Database에서 사용자의 정보를 조회한다.

개념적으로:

```sql
SELECT *
FROM users
WHERE email = 'user@example.com';
```

같은 작업이 이루어질 수 있다.

---

# 45. 회원가입

회원가입에서는:

```text
사용자가 정보 입력
        ↓
Frontend
        ↓
HTTP Request
        ↓
Backend
        ↓
Database에 사용자 저장
```

이라는 흐름을 생각할 수 있다.

Database 관점에서는:

```text
Create
```

작업이다.

---

# 46. 상품 목록 조회

쇼핑몰에서 상품 페이지를 열면:

```text
Browser
   ↓
Backend에 상품 요청
   ↓
Backend가 Database 조회
   ↓
상품 데이터 반환
   ↓
Browser에 표시
```

Database에서는:

```text
Read
```

작업이다.

---

# 47. 사용자 정보 수정

사용자가 닉네임을 변경한다면:

```text
Frontend
   ↓
Backend
   ↓
Database 수정
```

Database에서는:

```text
Update
```

작업이다.

---

# 48. 게시글 삭제

게시글 삭제 버튼을 누르면:

```text
Frontend
   ↓
Backend
   ↓
Database에서 데이터 삭제
```

Database에서는:

```text
Delete
```

작업이다.

---

# 49. Database와 HTTP 연결

앞에서 배운 HTTP와 연결하면:

```text
Browser
   ↓
HTTP Request
   ↓
Backend
   ↓
SQL
   ↓
Database
   ↓
결과
   ↓
Backend
   ↓
HTTP Response
   ↓
Browser
```

라는 전체 구조가 만들어진다.

---

# 50. API와 Database

예를 들어:

```text
GET /users/1
```

이라는 API 요청이 있다고 하자.

Backend에서는 개념적으로:

```sql
SELECT *
FROM users
WHERE user_id = 1;
```

같은 Database 조회가 이루어질 수 있다.

그 결과를 JSON으로 만들어 Client에게 반환할 수 있다.

---

# 51. Python과 Database

Python 프로그램에서도 Database에 연결할 수 있다.

개념적인 흐름:

```text
Python
   ↓
Database 연결
   ↓
SQL 실행
   ↓
결과 받기
   ↓
Python에서 사용
```

이다.

---

# 52. SQL 결과를 Pandas로 가져오기

Database에서 조회한 데이터를 Pandas DataFrame으로 가져와 분석할 수도 있다.

개념적으로:

```text
Database
   ↓
SQL SELECT
   ↓
Pandas DataFrame
   ↓
EDA
   ↓
전처리
   ↓
머신러닝
```

이라는 흐름을 만들 수 있다.

---

# 53. AI 프로젝트와 Database

AI 서비스에서도 Database가 사용될 수 있다.

예:

```text
사용자
   ↓
의료 데이터 입력
   ↓
Backend
   ↓
AI 모델 예측
   ↓
예측 결과
   ↓
Database 저장
```

이후 사용자가 과거 기록을 조회하면:

```text
Database
   ↓
과거 예측 결과 조회
   ↓
화면에 표시
```

할 수 있다.

---

# 54. Database와 머신러닝

머신러닝 학습 데이터를 Database에서 가져오는 경우도 있다.

```text
Database
   ↓
SQL
   ↓
데이터 추출
   ↓
Pandas
   ↓
전처리
   ↓
Feature Engineering
   ↓
Machine Learning
```

즉 지금까지 배운 데이터 분석과 Database는 서로 분리된 분야가 아니다.

---

# 55. SQL Database와 NoSQL

Database가 모두 Table 구조인 것은 아니다.

크게 보면:

```text
SQL / Relational Database

NoSQL Database
```

등의 방식이 있다.

---

# 56. SQL Database

대표적인 특징:

```text
Table 구조

Row / Column

관계

Schema

SQL 사용
```

대표적인 시스템:

```text
MySQL
PostgreSQL
SQLite
```

등이 있다.

---

# 57. NoSQL

NoSQL은 관계형 Table 구조만을 전제로 하지 않는 여러 Database 방식을 가리킨다.

예를 들어 MongoDB는 Document 기반 Database이다.

데이터가 다음처럼 저장될 수 있다.

```json
{
    "name": "Kim",
    "age": 30,
    "skills": [
        "Python",
        "SQL"
    ]
}
```

JSON과 비슷한 구조로 이해할 수 있다.

---

# 58. MongoDB

MongoDB에서는 관계형 Database의 Table과 조금 다른 용어를 사용한다.

개념적으로 비교하면:

| RDBMS | MongoDB |
|---|---|
| Database | Database |
| Table | Collection |
| Row | Document |
| Column | Field |

정확히 완전히 같은 개념은 아니지만 처음 이해할 때 이렇게 비교할 수 있다.

---

# 59. SQL vs MongoDB 예

관계형 Database:

```text
users Table

id | name | age
1  | Kim  | 30
```

MongoDB:

```json
{
    "_id": 1,
    "name": "Kim",
    "age": 30
}
```

저장 구조와 데이터 모델링 방식이 다르다.

---

# 60. 어떤 Database를 사용해야 할까?

무조건:

```text
SQL이 좋다.

MongoDB가 좋다.
```

라고 결정할 수 없다.

다음을 고려한다.

```text
데이터 구조

데이터 사이의 관계

일관성 요구

조회 방식

서비스 규모

개발 환경
```

등이다.

---

# 61. 처음에는 SQL부터

Database를 처음 공부할 때는 관계형 Database와 SQL을 먼저 이해하는 것이 좋다.

이유는:

```text
Table
Row
Column
Primary Key
Foreign Key
Relationship
JOIN
```

등의 개념이 Database 구조를 이해하는 데 좋은 기초가 되기 때문이다.

---

# 62. SQLite

Database를 처음 연습할 때 SQLite도 사용할 수 있다.

특징:

```text
별도의 Database Server 설치 없이 사용 가능

하나의 파일에 Database 저장 가능

Python과 함께 사용하기 쉬움

간단한 프로젝트와 학습에 편리
```

하다.

---

# 63. MySQL / PostgreSQL

실제 웹 서비스에서는:

```text
MySQL
PostgreSQL
```

같은 RDBMS를 많이 사용한다.

이들은 별도의 Database Server가 실행되고 Backend가 해당 Server에 연결하는 구조를 사용할 수 있다.

---

# 64. Client와 Database Server

웹 구조와 비슷하게 Database도 Client/Server 개념으로 생각할 수 있다.

```text
Backend
Database Client 역할
       ↓
Database Server
       ↓
데이터 저장
```

Backend가 SQL Query를 Database Server에 보내면 Database Server가 결과를 반환한다.

---

# 65. SQL Injection

Database를 웹과 연결할 때 보안도 중요하다.

사용자가 입력한 문자열을 SQL에 그대로 붙이면 SQL Injection 취약점이 생길 수 있다.

예를 들어 다음과 같은 방식은 피해야 한다.

```python
query = (
    "SELECT * FROM users "
    "WHERE name = '"
    + user_input
    + "'"
)
```

실제 개발에서는:

```text
Parameterized Query

ORM
```

등 안전한 방법을 사용해야 한다.

---

# 66. ORM

ORM은:

```text
Object Relational Mapping
```

의 약자이다.

Python 객체와 관계형 Database Table을 연결해서 SQL을 직접 많이 작성하지 않고도 Database를 다룰 수 있도록 도와주는 방식이다.

Python에서는 대표적으로:

```text
SQLAlchemy
```

를 사용할 수 있다.

---

# 67. SQL과 ORM 관계

예를 들어 SQL에서는:

```sql
SELECT *
FROM users;
```

처럼 작성한다.

ORM에서는 Python 코드로 비슷한 Database 작업을 수행할 수 있다.

하지만 ORM을 사용하더라도:

```text
Table
Primary Key
Foreign Key
SELECT
JOIN
Transaction
```

같은 Database와 SQL 개념을 이해하는 것이 중요하다.

---

# 68. Database 공부 흐름

앞으로 다음 순서로 공부한다.

```text
Database 기본
      ↓
Table / Row / Column
      ↓
Primary Key / Foreign Key
      ↓
SQL 기본
      ↓
SELECT
      ↓
WHERE
      ↓
ORDER BY
      ↓
GROUP BY
      ↓
JOIN
      ↓
INSERT / UPDATE / DELETE
      ↓
Python 연결
      ↓
SQLAlchemy
      ↓
MongoDB
```

---

# 69. 전체 서비스에서 Database 위치

지금까지 배운 내용을 합치면:

```text
사용자
   ↓
Browser / App
   ↓
HTTP Request
   ↓
Backend API
   ↓
Database
   ↓
데이터
```

AI가 추가되면:

```text
사용자
   ↓
Frontend
   ↓
HTTP / API
   ↓
Backend
   ├────────→ Database
   │
   └────────→ AI Model
                 ↓
              Prediction
                 ↓
              Database
```

같은 구조를 만들 수 있다.

---

# 70. 핵심 용어 정리

```text
Database
→ 데이터를 저장하는 공간

DBMS
→ Database를 관리하는 시스템

RDBMS
→ 관계형 Database 관리 시스템

Table
→ 데이터가 저장되는 표 형태의 구조

Column
→ 데이터의 항목

Row
→ 하나의 데이터 Record

Primary Key
→ Row를 고유하게 구분하는 Key

Foreign Key
→ 다른 Table과 관계를 연결하는 Key

SQL
→ 관계형 Database를 다루는 언어

CRUD
→ Create / Read / Update / Delete

ORM
→ 객체와 관계형 Database를 연결하는 방식
```

---

# 71. Pandas와 연결해서 기억하기

이미 Pandas를 공부했다면 다음처럼 연결하면 이해하기 쉽다.

```text
Pandas DataFrame
      ↕
Database Table
```

그리고:

```text
df["Age"]
↔
Column

df.loc[조건]
↔
WHERE

df.sort_values()
↔
ORDER BY

df.groupby()
↔
GROUP BY

pd.merge()
↔
JOIN
```

앞으로 SQL을 배우면서 이 관계를 하나씩 확인한다.

---

# 72. 배운 점

Database는 데이터를 지속적으로 저장하고 관리하기 위한 시스템이다.

관계형 Database에서는 데이터를:

```text
Table
Row
Column
```

구조로 관리한다.

각 Row를 고유하게 구분하기 위해:

```text
Primary Key
```

를 사용할 수 있고 다른 Table과 관계를 연결하기 위해:

```text
Foreign Key
```

를 사용할 수 있다.

Database에서 가장 기본적인 데이터 작업은:

```text
Create
Read
Update
Delete
```

이며 이를:

```text
CRUD
```

라고 한다.

SQL에서는 대표적으로:

```text
Create → INSERT
Read   → SELECT
Update → UPDATE
Delete → DELETE
```

와 연결된다.

그리고 실제 웹 서비스에서는:

```text
Frontend
   ↓
HTTP Request
   ↓
Backend
   ↓
Database
```

라는 흐름으로 Database가 사용될 수 있다.

AI 서비스에서는:

```text
Database
   ↓
Pandas
   ↓
전처리
   ↓
Machine Learning

또는

사용자
   ↓
Backend
   ↓
AI Model
   ↓
Prediction
   ↓
Database 저장
```

같은 구조로 지금까지 배운 기술들이 서로 연결될 수 있다.

Database 공부에서 가장 먼저 이해해야 할 것은 SQL 문법 자체보다:

```text
데이터를 왜 Database에 저장하는가?

Table은 무엇인가?

Row와 Column은 무엇인가?

Primary Key는 왜 필요한가?

Table들은 어떻게 연결되는가?
```

이다.

이 구조를 이해하면 이후 SQL의 SELECT, WHERE, GROUP BY, JOIN을 훨씬 쉽게 이해할 수 있다.