# SQL 기초

## 1. SQL이란?

SQL은:

```text
Structured Query Language
```

의 약자이다.

관계형 Database에서 데이터를 다루기 위해 사용하는 언어이다.

SQL을 이용하면:

```text
Table 생성
데이터 추가
데이터 조회
데이터 수정
데이터 삭제
```

등을 할 수 있다.

---

# 2. SQL의 기본 흐름

이번에는 `users`라는 Table을 하나 만든다고 생각해보자.

전체 흐름은:

```text
Database
   ↓
Table 생성
   ↓
데이터 추가
   ↓
데이터 조회
   ↓
조건 검색
   ↓
데이터 수정
   ↓
데이터 삭제
```

이다.

SQL로 보면:

```text
CREATE TABLE
     ↓
INSERT
     ↓
SELECT
     ↓
WHERE
     ↓
UPDATE
     ↓
DELETE
```

가 된다.

---

# 3. SQL 문장의 기본 형태

SQL은 다음처럼 작성한다.

```sql
SELECT *
FROM users;
```

마지막에:

```text
;
```

세미콜론을 붙인다.

세미콜론은:

```text
SQL 문장이 여기서 끝난다.
```

는 의미이다.

---

# 4. SQL 대문자와 소문자

다음 두 SQL은 일반적으로 같은 의미이다.

```sql
SELECT *
FROM users;
```

```sql
select *
from users;
```

SQL Keyword는 대소문자를 구분하지 않는 경우가 많다.

하지만 가독성을 위해:

```text
SELECT
FROM
WHERE
INSERT
UPDATE
DELETE
```

같은 SQL Keyword를 대문자로 작성하는 경우가 많다.

---

# 5. Table 만들기

먼저 users Table을 만들어보자.

```sql
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    name VARCHAR(50),
    age INTEGER,
    email VARCHAR(100)
);
```

---

# 6. CREATE TABLE

```sql
CREATE TABLE users
```

의 의미는:

```text
users라는 이름의 Table을 생성한다.
```

이다.

---

# 7. Column 정의

다음 부분:

```sql
id INTEGER,
name VARCHAR(50),
age INTEGER,
email VARCHAR(100)
```

은 Table에 어떤 Column을 만들 것인지 정의한다.

결과적으로:

| id | name | age | email |
|---|---|---|---|

형태의 Table이 만들어진다.

---

# 8. INTEGER

```sql
id INTEGER
```

에서 `INTEGER`는 정수 자료형이다.

예:

```text
1
2
10
100
```

같은 값을 저장할 수 있다.

---

# 9. VARCHAR

```sql
name VARCHAR(50)
```

은 문자열을 저장하는 Column이다.

`50`은 저장할 문자열 길이에 대한 제한을 나타낸다.

DBMS에 따라 문자열 자료형과 동작에는 차이가 있을 수 있다.

---

# 10. PRIMARY KEY

```sql
id INTEGER PRIMARY KEY
```

에서:

```text
PRIMARY KEY
```

는 각 Row를 고유하게 식별한다.

예:

| id | name |
|---:|---|
| 1 | Kim |
| 2 | Lee |
| 3 | Park |

`id`가 각각 다르기 때문에 사용자를 구분할 수 있다.

---

# 11. Table에 데이터 추가

데이터를 추가할 때는:

```text
INSERT
```

를 사용한다.

```sql
INSERT INTO users (
    id,
    name,
    age,
    email
)
VALUES (
    1,
    'Kim',
    30,
    'kim@example.com'
);
```

---

# 12. INSERT INTO

```sql
INSERT INTO users
```

의 의미는:

```text
users Table에 데이터를 추가한다.
```

이다.

---

# 13. VALUES

```sql
VALUES (
    1,
    'Kim',
    30,
    'kim@example.com'
);
```

은 실제로 저장할 값이다.

Column 순서:

```text
id
name
age
email
```

와 VALUES 순서:

```text
1
Kim
30
kim@example.com
```

가 서로 대응한다.

---

# 14. 문자열은 따옴표

숫자:

```sql
30
```

문자열:

```sql
'Kim'
```

처럼 문자열은 일반적으로 작은따옴표로 감싼다.

---

# 15. 여러 사용자 추가

```sql
INSERT INTO users
VALUES
    (2, 'Lee', 25, 'lee@example.com'),
    (3, 'Park', 40, 'park@example.com'),
    (4, 'Choi', 35, 'choi@example.com');
```

그러면 Table은:

| id | name | age | email |
|---:|---|---:|---|
| 1 | Kim | 30 | kim@example.com |
| 2 | Lee | 25 | lee@example.com |
| 3 | Park | 40 | park@example.com |
| 4 | Choi | 35 | choi@example.com |

처럼 된다.

실제 코드에서는 Column 이름을 명시하는 습관이 더 안전하다.

```sql
INSERT INTO users (
    id,
    name,
    age,
    email
)
VALUES
    (2, 'Lee', 25, 'lee@example.com'),
    (3, 'Park', 40, 'park@example.com');
```

---

# 16. 데이터 조회

데이터를 조회할 때는:

```text
SELECT
```

를 사용한다.

```sql
SELECT *
FROM users;
```

---

# 17. SELECT의 의미

```sql
SELECT *
```

에서 `SELECT`는:

```text
데이터를 조회한다.
```

는 뜻이다.

`*`는:

```text
모든 Column
```

을 의미한다.

따라서:

```sql
SELECT *
FROM users;
```

는:

```text
users Table에서 모든 Column을 조회해줘.
```

라는 뜻이다.

---

# 18. FROM

```sql
FROM users
```

는:

```text
users Table에서
```

라는 의미이다.

따라서:

```sql
SELECT *
FROM users;
```

를 문장처럼 읽으면:

```text
SELECT *
→ 모든 Column을 조회한다.

FROM users
→ users Table에서.
```

이다.

---

# 19. 특정 Column만 조회

모든 Column이 필요하지 않을 수도 있다.

```sql
SELECT name
FROM users;
```

결과:

| name |
|---|
| Kim |
| Lee |
| Park |
| Choi |

---

# 20. 여러 Column 조회

```sql
SELECT name, age
FROM users;
```

결과:

| name | age |
|---|---:|
| Kim | 30 |
| Lee | 25 |
| Park | 40 |
| Choi | 35 |

---

# 21. Pandas와 비교

Pandas:

```python
df[
    ["name", "age"]
]
```

SQL:

```sql
SELECT name, age
FROM users;
```

목적은 비슷하다.

```text
필요한 Column을 선택한다.
```

---

# 22. WHERE

특정 조건의 데이터만 조회하려면:

```text
WHERE
```

를 사용한다.

예:

```sql
SELECT *
FROM users
WHERE age >= 30;
```

의미:

```text
users Table에서
age가 30 이상인 사용자만 조회
```

이다.

---

# 23. Pandas WHERE 비교

Pandas:

```python
df.loc[
    df["age"] >= 30
]
```

SQL:

```sql
SELECT *
FROM users
WHERE age >= 30;
```

둘 다:

```text
age >= 30
```

조건을 만족하는 데이터만 선택한다.

---

# 24. 비교 연산자

SQL에서도 비교 연산자를 사용한다.

```text
=     같다

!=    같지 않다

>     크다

<     작다

>=    크거나 같다

<=    작거나 같다
```

예:

```sql
SELECT *
FROM users
WHERE age = 30;
```

---

# 25. Python과 SQL의 = 차이

Python에서는:

```python
age == 30
```

처럼 같은지 비교할 때 `==`를 사용한다.

SQL에서는:

```sql
age = 30
```

처럼 `=`를 사용한다.

헷갈리기 쉬운 부분이다.

```text
Python 비교
==

SQL 비교
=
```

---

# 26. 문자열 조건

```sql
SELECT *
FROM users
WHERE name = 'Kim';
```

의미:

```text
name이 Kim인 사용자 조회
```

이다.

---

# 27. AND

조건을 모두 만족해야 한다면:

```sql
SELECT *
FROM users
WHERE age >= 30
AND name = 'Kim';
```

의미:

```text
age가 30 이상이고
name이 Kim
```

이다.

---

# 28. OR

둘 중 하나만 만족해도 된다면:

```sql
SELECT *
FROM users
WHERE age < 30
OR age >= 40;
```

---

# 29. NOT

조건을 반대로 만들 수 있다.

```sql
SELECT *
FROM users
WHERE NOT age >= 30;
```

즉:

```text
age >= 30이 아닌 사용자
```

를 조회한다.

---

# 30. Pandas 논리 조건과 비교

Pandas:

```python
df.loc[
    (df["age"] >= 30)
    &
    (df["name"] == "Kim")
]
```

SQL:

```sql
SELECT *
FROM users
WHERE age >= 30
AND name = 'Kim';
```

SQL에서는 조건을 문장처럼 읽기 쉽다.

---

# 31. BETWEEN

특정 범위의 데이터를 조회할 수 있다.

```sql
SELECT *
FROM users
WHERE age BETWEEN 30 AND 40;
```

의미:

```text
age가 30부터 40 사이
```

이다.

일반적으로 양 끝 값도 포함한다.

---

# 32. IN

여러 값 중 하나에 해당하는 데이터를 찾을 수 있다.

```sql
SELECT *
FROM users
WHERE age IN (25, 30, 40);
```

의미:

```text
age가
25 또는 30 또는 40
```

인 데이터이다.

---

# 33. LIKE

문자열 패턴을 검색할 때 사용할 수 있다.

```sql
SELECT *
FROM users
WHERE name LIKE 'K%';
```

`%`는 여러 문자를 나타내는 Wildcard로 사용할 수 있다.

즉:

```text
K로 시작하는 name
```

을 찾는다.

---

# 34. LIKE 예시

```sql
WHERE name LIKE 'K%'
```

```text
K로 시작
```

```sql
WHERE name LIKE '%m'
```

```text
m으로 끝남
```

```sql
WHERE name LIKE '%im%'
```

```text
중간에 im이 포함
```

---

# 35. NULL 검색

NULL을 찾을 때는:

```sql
SELECT *
FROM users
WHERE email IS NULL;
```

을 사용한다.

---

# 36. NULL이 아닌 값

```sql
SELECT *
FROM users
WHERE email IS NOT NULL;
```

을 사용한다.

---

# 37. NULL 비교 주의

다음처럼 사용하지 않는다.

```sql
WHERE email = NULL
```

NULL은 일반적인 값과 다른 방식으로 다뤄진다.

사용:

```sql
IS NULL
```

또는:

```sql
IS NOT NULL
```

이다.

---

# 38. Pandas 결측값과 비교

Pandas:

```python
df[
    df["email"].isnull()
]
```

SQL:

```sql
SELECT *
FROM users
WHERE email IS NULL;
```

비슷한 목적이다.

---

# 39. ORDER BY

조회 결과를 정렬할 수 있다.

```sql
SELECT *
FROM users
ORDER BY age;
```

기본적으로 오름차순 정렬이다.

---

# 40. ASC

오름차순:

```sql
SELECT *
FROM users
ORDER BY age ASC;
```

예:

```text
25
30
35
40
```

---

# 41. DESC

내림차순:

```sql
SELECT *
FROM users
ORDER BY age DESC;
```

결과:

```text
40
35
30
25
```

---

# 42. Pandas 정렬과 비교

Pandas:

```python
df.sort_values(
    "age",
    ascending=False
)
```

SQL:

```sql
SELECT *
FROM users
ORDER BY age DESC;
```

둘 다 age 기준 내림차순 정렬이다.

---

# 43. LIMIT

조회할 데이터 개수를 제한할 수 있다.

```sql
SELECT *
FROM users
LIMIT 3;
```

의미:

```text
최대 3개의 Row만 조회
```

이다.

DBMS에 따라 행 제한 문법에는 차이가 있을 수 있다.

---

# 44. 조건 + 정렬 + 제한

여러 문법을 같이 사용할 수 있다.

```sql
SELECT name, age
FROM users
WHERE age >= 30
ORDER BY age DESC
LIMIT 2;
```

순서대로 읽어보자.

```text
SELECT name, age
→ name과 age 조회

FROM users
→ users에서

WHERE age >= 30
→ 30세 이상만

ORDER BY age DESC
→ 나이 내림차순

LIMIT 2
→ 2명만
```

---

# 45. SQL을 읽는 방법

SQL은 작성 순서대로 읽어도 이해할 수 있지만 실제 처리 개념을 이해할 때는:

```text
FROM
→ 어디에서?

WHERE
→ 어떤 데이터를?

SELECT
→ 어떤 Column으로?

ORDER BY
→ 어떤 순서로?

LIMIT
→ 몇 개?
```

처럼 생각하면 이해하기 쉽다.

예:

```sql
SELECT name, age
FROM users
WHERE age >= 30
ORDER BY age DESC;
```

개념적으로:

```text
users에서
    ↓
age >= 30인 Row를 찾고
    ↓
name, age를 선택하고
    ↓
age 내림차순으로 정렬
```

한다.

---

# 46. DISTINCT

중복 값을 제거하여 조회할 수 있다.

예:

```sql
SELECT DISTINCT age
FROM users;
```

age가:

```text
30
30
25
40
40
```

이었다면 결과는:

```text
30
25
40
```

처럼 중복되지 않은 값들을 조회할 수 있다.

---

# 47. Pandas와 DISTINCT

Pandas:

```python
df["age"].unique()
```

SQL:

```sql
SELECT DISTINCT age
FROM users;
```

비슷한 목적으로 사용할 수 있다.

---

# 48. UPDATE

기존 데이터를 수정할 때:

```text
UPDATE
```

를 사용한다.

예:

```sql
UPDATE users
SET age = 31
WHERE id = 1;
```

의미:

```text
id가 1인 사용자의
age를 31로 변경
```

이다.

---

# 49. UPDATE 구조

```sql
UPDATE users
```

```text
users Table을 수정한다.
```

```sql
SET age = 31
```

```text
age 값을 31로 바꾼다.
```

```sql
WHERE id = 1
```

```text
id가 1인 Row만.
```

이다.

---

# 50. UPDATE에서 WHERE가 중요한 이유

다음 코드를 보자.

```sql
UPDATE users
SET age = 31;
```

`WHERE`가 없다.

그러면 여러 DBMS에서 users의 모든 Row에 대해 age가 31로 변경될 수 있다.

따라서 UPDATE를 사용할 때는:

```text
어떤 Row를 수정하는가?
```

를 반드시 확인해야 한다.

---

# 51. UPDATE 전 SELECT

실무에서 안전하게 작업하려면 먼저:

```sql
SELECT *
FROM users
WHERE id = 1;
```

로 수정 대상을 확인하고:

```sql
UPDATE users
SET age = 31
WHERE id = 1;
```

처럼 실행하는 습관이 좋다.

---

# 52. 여러 Column 수정

```sql
UPDATE users
SET
    name = 'Kim J',
    age = 31
WHERE id = 1;
```

한 번에 여러 Column을 수정할 수도 있다.

---

# 53. DELETE

데이터를 삭제할 때:

```text
DELETE
```

를 사용한다.

```sql
DELETE FROM users
WHERE id = 4;
```

의미:

```text
users Table에서
id가 4인 Row 삭제
```

이다.

---

# 54. DELETE에서도 WHERE가 중요

다음은 매우 주의해야 한다.

```sql
DELETE FROM users;
```

WHERE가 없다.

그러면 Table의 모든 Row가 삭제될 수 있다.

따라서 DELETE에서도:

```text
WHERE
```

조건을 반드시 확인한다.

---

# 55. DELETE 전 SELECT

삭제하기 전에:

```sql
SELECT *
FROM users
WHERE id = 4;
```

로 삭제 대상부터 확인한다.

그다음:

```sql
DELETE FROM users
WHERE id = 4;
```

를 실행한다.

이 습관은 매우 중요하다.

---

# 56. DELETE와 DROP 차이

헷갈리기 쉬운 부분이다.

```sql
DELETE FROM users;
```

는:

```text
users 안의 Row를 삭제
```

한다.

반면:

```sql
DROP TABLE users;
```

는:

```text
users Table 자체를 삭제
```

한다.

즉:

```text
DELETE
→ 데이터 삭제

DROP TABLE
→ Table 자체 삭제
```

이다.

---

# 57. ALTER TABLE

이미 존재하는 Table의 구조를 변경할 때 사용할 수 있다.

예:

```sql
ALTER TABLE users
ADD COLUMN phone VARCHAR(20);
```

그러면:

```text
phone
```

이라는 Column이 추가된다.

세부 문법은 DBMS에 따라 차이가 있을 수 있다.

---

# 58. SQL 명령어 종류

SQL 명령어는 역할에 따라 구분하기도 한다.

대표적으로:

```text
DDL
DML
DQL
DCL
TCL
```

등이 있다.

처음에는 모든 약어를 외우기보다 역할을 이해하는 것이 중요하다.

---

# 59. DDL

DDL:

```text
Data Definition Language
```

Database 구조를 정의한다.

대표적으로:

```text
CREATE
ALTER
DROP
```

등이 있다.

---

# 60. DML

DML:

```text
Data Manipulation Language
```

데이터를 추가, 수정, 삭제한다.

대표적으로:

```text
INSERT
UPDATE
DELETE
```

가 있다.

분류 방식에 따라 SELECT를 DML에 포함해 설명하기도 한다.

---

# 61. DQL

DQL:

```text
Data Query Language
```

데이터 조회에 초점을 둔다.

대표적으로:

```text
SELECT
```

이다.

---

# 62. CRUD와 다시 연결

```text
Create
→ INSERT

Read
→ SELECT

Update
→ UPDATE

Delete
→ DELETE
```

웹 개발에서도 계속 사용하게 된다.

---

# 63. API와 CRUD

REST API를 공부하면 다음과 같은 형태를 자주 볼 수 있다.

```text
GET
POST
PUT / PATCH
DELETE
```

Database의 CRUD와 연결해서 생각하면:

```text
GET
→ Read

POST
→ Create

PUT / PATCH
→ Update

DELETE
→ Delete
```

라는 식으로 이해할 수 있다.

단, HTTP Method와 SQL 명령이 1:1로 완전히 동일한 개념은 아니다.

---

# 64. 회원가입 예시

사용자가 회원가입:

```text
POST /users
```

Backend에서는 입력값을 확인하고 Database에:

```sql
INSERT INTO users (
    name,
    age,
    email
)
VALUES (
    'Kim',
    30,
    'kim@example.com'
);
```

같은 작업을 수행할 수 있다.

---

# 65. 회원 조회 예시

```text
GET /users/1
```

Backend에서는 개념적으로:

```sql
SELECT *
FROM users
WHERE id = 1;
```

같은 조회를 할 수 있다.

---

# 66. 회원 수정 예시

```text
PATCH /users/1
```

Database에서는:

```sql
UPDATE users
SET name = 'New Kim'
WHERE id = 1;
```

같은 작업으로 이어질 수 있다.

---

# 67. 회원 삭제 예시

```text
DELETE /users/1
```

Database에서는:

```sql
DELETE FROM users
WHERE id = 1;
```

같은 작업으로 이어질 수 있다.

---

# 68. Frontend부터 Database까지

전체 흐름:

```text
사용자
   ↓
Frontend
   ↓
HTTP Request
   ↓
Backend API
   ↓
SQL
   ↓
Database
```

응답은 반대로:

```text
Database 결과
   ↓
Backend
   ↓
JSON
   ↓
HTTP Response
   ↓
Frontend
   ↓
사용자 화면
```

으로 전달될 수 있다.

---

# 69. SQL Injection 주의

다음과 같이 사용자의 입력을 SQL 문자열에 직접 합치는 것은 위험할 수 있다.

```python
query = (
    "SELECT * FROM users "
    "WHERE email = '"
    + email
    + "'"
)
```

사용자가 SQL 문법을 포함한 값을 입력하면 의도하지 않은 Query가 실행될 가능성이 있다.

이것을:

```text
SQL Injection
```

이라고 한다.

---

# 70. 안전한 Query

Python DB 라이브러리를 사용할 때는 Parameterized Query를 사용한다.

예를 들어 SQLite에서는:

```python
cursor.execute(
    """
    SELECT *
    FROM users
    WHERE email = ?
    """,
    (email,)
)
```

처럼 값과 SQL 구조를 분리한다.

---

# 71. ORM을 사용하는 경우

SQLAlchemy 같은 ORM을 사용하면 Python 코드로 Database 작업을 표현할 수 있다.

하지만 ORM을 사용한다고 SQL을 몰라도 되는 것은 아니다.

적어도:

```text
SELECT
WHERE
INSERT
UPDATE
DELETE
JOIN
GROUP BY
Primary Key
Foreign Key
```

정도는 이해해야 Database에서 실제로 무슨 일이 일어나는지 파악할 수 있다.

---

# 72. SQL 오류를 읽는 습관

SQL에서도 오류가 발생한다.

예:

```text
Table 이름이 잘못됨

Column 이름이 잘못됨

SQL 문법 오류

자료형 문제

Primary Key 중복

NOT NULL 위반

Foreign Key 문제
```

오류가 발생하면 먼저:

```text
어떤 SQL을 실행했는가?

어떤 Table인가?

어떤 Column인가?

오류 메시지는 무엇인가?
```

를 확인한다.

---

# 73. Pandas와 SQL 비교

| 작업 | Pandas | SQL |
|---|---|---|
| 전체 확인 | `df` | `SELECT *` |
| Column 선택 | `df[["name"]]` | `SELECT name` |
| 조건 | `df.loc[...]` | `WHERE` |
| 정렬 | `sort_values()` | `ORDER BY` |
| 중복 값 확인 | `unique()` | `DISTINCT` |
| 결측 확인 | `isnull()` | `IS NULL` |
| Row 추가 | 새로운 Row 추가 | `INSERT` |
| 값 수정 | `loc` 등 | `UPDATE` |
| Row 삭제 | `drop()` 등 | `DELETE` |

완전히 동일한 기능은 아니지만 개념을 연결하는 데 도움이 된다.

---

# 74. 가장 중요한 SELECT 기본 형태

SQL 조회에서 자주 보는 기본 구조:

```sql
SELECT column
FROM table
WHERE condition
ORDER BY column;
```

예:

```sql
SELECT name, age
FROM users
WHERE age >= 30
ORDER BY age DESC;
```

문장처럼 읽으면:

```text
users에서

age가 30 이상인 사람 중

name과 age를 가져와서

age가 높은 순서대로 보여줘.
```

이다.

---

# 75. SQL을 공부할 때 중요한 습관

SQL을 단순 암기하지 말고 문장으로 해석한다.

예:

```sql
SELECT name
FROM users
WHERE age >= 30;
```

를 보면:

```text
SELECT name
→ name을 보여줘.

FROM users
→ users에서.

WHERE age >= 30
→ age가 30 이상인 것만.
```

으로 해석한다.

---

# 76. CRUD 전체 예제

Table 생성:

```sql
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    name VARCHAR(50),
    age INTEGER,
    email VARCHAR(100)
);
```

데이터 추가:

```sql
INSERT INTO users (
    id,
    name,
    age,
    email
)
VALUES (
    1,
    'Kim',
    30,
    'kim@example.com'
);
```

조회:

```sql
SELECT *
FROM users;
```

조건 조회:

```sql
SELECT *
FROM users
WHERE age >= 30;
```

수정:

```sql
UPDATE users
SET age = 31
WHERE id = 1;
```

삭제:

```sql
DELETE FROM users
WHERE id = 1;
```

이것이 SQL의 가장 기본적인 CRUD 흐름이다.

---

# 77. 이번 단계에서 꼭 기억할 SQL

```sql
CREATE TABLE
```

```text
Table 생성
```

```sql
INSERT INTO
```

```text
데이터 추가
```

```sql
SELECT
```

```text
데이터 조회
```

```sql
WHERE
```

```text
조건 지정
```

```sql
ORDER BY
```

```text
정렬
```

```sql
UPDATE
```

```text
데이터 수정
```

```sql
DELETE
```

```text
데이터 삭제
```

---

# 78. 배운 점

SQL은 관계형 Database에서 데이터를 다루는 언어이다.

가장 기본적인 흐름은:

```text
CREATE TABLE
     ↓
INSERT
     ↓
SELECT
     ↓
WHERE
     ↓
UPDATE
     ↓
DELETE
```

이다.

CRUD와 연결하면:

```text
Create → INSERT
Read   → SELECT
Update → UPDATE
Delete → DELETE
```

이다.

특히 조회에서는:

```sql
SELECT name, age
FROM users
WHERE age >= 30
ORDER BY age DESC;
```

같은 구조를 많이 사용한다.

Pandas와 비교하면:

```text
Pandas
df.loc[df["age"] >= 30]

SQL
SELECT *
FROM users
WHERE age >= 30;
```

처럼 지금까지 배운 데이터 처리 개념과 연결할 수 있다.

또한 UPDATE와 DELETE에서는:

```text
WHERE가 있는지 반드시 확인
```

하는 습관이 중요하다.

SQL을 공부할 때 가장 중요한 것은 문법을 통째로 외우는 것이 아니라:

```text
어느 Table에서?

어떤 Column을?

어떤 조건으로?

어떤 작업을 할 것인가?
```

를 생각하는 것이다.