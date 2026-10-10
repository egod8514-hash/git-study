# SQLAlchemy 기초

## 1. SQLAlchemy란?

SQLAlchemy는 Python에서 관계형 Database를 다루기 위해 많이 사용하는 라이브러리이다.

예를 들어 Python 프로그램에서:

```text
회원가입
상품 등록
주문 저장
AI 예측 결과 저장
```

등을 Database에 저장하고 싶을 수 있다.

전체 구조는:

```text
Python
   ↓
SQLAlchemy
   ↓
Database
```

라고 생각할 수 있다.

---

# 2. 지금까지의 SQL

앞에서는 SQL을 직접 작성했다.

예:

```sql
SELECT *
FROM users
WHERE user_id = 1;
```

이 SQL은:

```text
users Table에서
user_id가 1인 사용자 조회
```

라는 의미이다.

SQLAlchemy를 사용하면 Python에서 이런 Database 작업을 수행할 수 있다.

---

# 3. SQLAlchemy를 사용하는 이유

Python 프로그램에서 Database를 사용하려면 다음 작업이 필요하다.

```text
Database 연결

SQL 실행

결과 가져오기

Transaction 관리

Python 객체와 Table 연결
```

SQLAlchemy는 이러한 작업을 도와준다.

---

# 4. SQLAlchemy와 Database의 위치

전체 구조:

```text
Python Application
       ↓
   SQLAlchemy
       ↓
Database Driver
       ↓
    Database
```

예를 들어:

```text
FastAPI
   ↓
SQLAlchemy
   ↓
PostgreSQL
```

같은 구조를 만들 수 있다.

---

# 5. SQLAlchemy 설치

터미널:

```powershell
pip install sqlalchemy
```

설치 확인:

```powershell
pip show sqlalchemy
```

Python에서 확인:

```python
import sqlalchemy

print(
    sqlalchemy.__version__
)
```

---

# 6. 가상환경 주의

예를 들어:

```text
ml_env
```

환경에서 SQLAlchemy를 사용한다면 해당 환경에 설치되어 있어야 한다.

```powershell
conda activate ml_env
python -m pip install sqlalchemy
```

현재 Python 위치 확인:

```powershell
where python
```

Python 내부:

```python
import sys

print(
    sys.executable
)
```

설치한 환경과 실행 중인 환경이 다르면:

```text
ModuleNotFoundError
```

가 발생할 수 있다.

---

# 7. SQLite로 시작하기

SQLAlchemy를 연습할 때 SQLite를 사용하면 편리하다.

SQLite는 별도의 Database Server를 실행하지 않고 하나의 파일로 Database를 사용할 수 있다.

예:

```text
study.db
```

파일 하나에 Database가 저장될 수 있다.

---

# 8. Engine

SQLAlchemy에서 가장 먼저 자주 만나게 되는 개념이:

```text
Engine
```

이다.

Engine은 Python과 Database가 통신할 수 있도록 연결 설정을 관리하는 핵심 객체이다.

---

# 9. Engine 만들기

```python
from sqlalchemy import create_engine

engine = create_engine(
    "sqlite:///study.db"
)
```

이제:

```text
study.db
```

SQLite Database와 작업할 수 있는 Engine을 만들었다.

---

# 10. create_engine

```python
create_engine()
```

은 Database 연결 정보를 기반으로 Engine을 생성한다.

```python
engine = create_engine(
    "sqlite:///study.db"
)
```

에서:

```text
sqlite
→ 사용할 Database 종류

study.db
→ Database 파일
```

이다.

---

# 11. Database URL

SQLAlchemy에서는 Database 연결 정보를 URL 형태로 표현하는 경우가 많다.

개념적으로:

```text
dialect+driver://username:password@host:port/database
```

형태를 볼 수 있다.

예를 들어 Database 종류에 따라 연결 문자열이 달라진다.

처음에는 SQLite로 구조를 이해하면 된다.

---

# 12. SQLite Memory Database

파일을 만들지 않고 메모리에서만 연습할 수도 있다.

```python
engine = create_engine(
    "sqlite:///:memory:"
)
```

프로그램이 종료되면 데이터도 사라진다.

따라서 간단한 테스트에 사용할 수 있다.

---

# 13. Engine이 Connection 하나인가?

정확히 말하면 Engine 자체를 단순히:

```text
Database 연결 1개
```

라고 생각하면 부족하다.

Engine은 Database와 통신하기 위한:

```text
연결 설정

Connection 관리

Connection Pool 관리
```

등의 중심 역할을 한다.

실제 작업에서는 Engine을 통해 Connection을 얻을 수 있다.

---

# 14. Connection

Database에 SQL을 실행해보자.

```python
from sqlalchemy import (
    create_engine,
    text
)

engine = create_engine(
    "sqlite:///study.db"
)

with engine.connect() as conn:
    result = conn.execute(
        text(
            "SELECT 1"
        )
    )

    print(
        result.fetchone()
    )
```

---

# 15. with를 사용하는 이유

```python
with engine.connect() as conn:
```

형태를 사용하면 작업이 끝난 뒤 Connection을 적절히 정리하기 편하다.

Python에서 파일을 열 때:

```python
with open(...) as f:
```

를 사용하는 것과 비슷한 흐름으로 생각할 수 있다.

---

# 16. text()

SQLAlchemy에서 문자열 SQL을 실행할 때:

```python
text()
```

를 사용할 수 있다.

```python
from sqlalchemy import text
```

예:

```python
sql = text(
    """
    SELECT *
    FROM users
    """
)
```

---

# 17. SQLAlchemy를 써도 SQL을 직접 사용할 수 있다

SQLAlchemy를 사용한다고 무조건 ORM만 사용해야 하는 것은 아니다.

예:

```python
with engine.connect() as conn:
    result = conn.execute(
        text(
            """
            SELECT *
            FROM users
            """
        )
    )
```

처럼 SQL을 직접 실행할 수도 있다.

---

# 18. Table 만들기

SQL을 직접 실행해 users Table을 만들어보자.

```python
from sqlalchemy import (
    create_engine,
    text
)

engine = create_engine(
    "sqlite:///study.db"
)

with engine.begin() as conn:
    conn.execute(
        text(
            """
            CREATE TABLE IF NOT EXISTS users (
                user_id INTEGER PRIMARY KEY,
                name TEXT NOT NULL,
                age INTEGER,
                email TEXT
            )
            """
        )
    )
```

---

# 19. engine.begin()

```python
with engine.begin() as conn:
```

을 사용하면 Transaction 범위 안에서 작업할 수 있다.

정상적으로 끝나면 Commit되고 오류가 발생하면 Rollback되는 방식으로 사용할 수 있다.

---

# 20. Transaction이란?

Transaction은 여러 Database 작업을 하나의 논리적인 작업 단위로 묶는 개념이다.

예를 들어 계좌이체:

```text
A 계좌 -10000원

B 계좌 +10000원
```

두 작업은 함께 성공해야 한다.

첫 번째만 성공하고 두 번째가 실패하면 문제가 된다.

그래서:

```text
모두 성공
→ COMMIT

중간에 실패
→ ROLLBACK
```

하는 개념이 중요하다.

---

# 21. COMMIT

Transaction에서 변경 내용을 최종적으로 확정한다.

```text
INSERT
UPDATE
DELETE
```

등으로 데이터를 변경한 뒤 해당 변경을 확정하는 것이다.

---

# 22. ROLLBACK

작업 도중 문제가 발생하면 Transaction에서 수행한 변경을 되돌릴 수 있다.

```text
작업 시작
   ↓
INSERT
   ↓
UPDATE
   ↓
오류 발생
   ↓
ROLLBACK
```

처럼 사용할 수 있다.

---

# 23. INSERT 실행

```python
with engine.begin() as conn:
    conn.execute(
        text(
            """
            INSERT INTO users (
                user_id,
                name,
                age,
                email
            )
            VALUES (
                :user_id,
                :name,
                :age,
                :email
            )
            """
        ),
        {
            "user_id": 1,
            "name": "Kim",
            "age": 30,
            "email": "kim@example.com"
        }
    )
```

---

# 24. :user_id는 무엇인가?

다음 부분:

```sql
:user_id
:name
:age
:email
```

은 Parameter를 넣을 자리이다.

Python Dictionary:

```python
{
    "user_id": 1,
    "name": "Kim",
    "age": 30,
    "email": "kim@example.com"
}
```

와 연결된다.

---

# 25. 문자열 연결 방식은 피하기

다음처럼 사용자의 입력을 SQL 문자열에 직접 붙이는 방식은 피한다.

```python
sql = (
    "SELECT * FROM users "
    "WHERE name = '"
    + user_input
    + "'"
)
```

SQL Injection 위험이 생길 수 있다.

---

# 26. Parameterized Query

대신:

```python
stmt = text(
    """
    SELECT *
    FROM users
    WHERE name = :name
    """
)

with engine.connect() as conn:
    result = conn.execute(
        stmt,
        {
            "name": user_input
        }
    )
```

처럼 SQL 구조와 실제 값을 분리한다.

---

# 27. SELECT 실행

```python
with engine.connect() as conn:
    result = conn.execute(
        text(
            """
            SELECT *
            FROM users
            """
        )
    )

    rows = result.fetchall()

    print(rows)
```

---

# 28. fetchall()

```python
result.fetchall()
```

은 Query 결과의 여러 Row를 가져온다.

예:

```text
[(1, 'Kim', 30, 'kim@example.com')]
```

같은 결과를 볼 수 있다.

---

# 29. fetchone()

한 Row만 가져올 수 있다.

```python
row = result.fetchone()
```

예:

```python
with engine.connect() as conn:
    result = conn.execute(
        text(
            """
            SELECT *
            FROM users
            WHERE user_id = :user_id
            """
        ),
        {
            "user_id": 1
        }
    )

    row = result.fetchone()
```

---

# 30. Python + SQL 흐름

지금까지는:

```text
Python
   ↓
Engine
   ↓
Connection
   ↓
SQL
   ↓
Database
   ↓
Result
   ↓
Python
```

이라는 흐름이다.

이 방식에서는 여전히 SQL을 직접 작성하고 있다.

---

# 31. ORM

SQLAlchemy에는 ORM 기능도 있다.

ORM:

```text
Object Relational Mapping
```

이다.

Python Class와 Database Table을 연결해서 Database를 Python 객체 중심으로 다룰 수 있도록 도와준다.

---

# 32. ORM이 없을 때

SQL:

```sql
INSERT INTO users (
    name,
    age
)
VALUES (
    'Kim',
    30
);
```

처럼 직접 SQL을 작성한다.

---

# 33. ORM을 사용하면

개념적으로:

```python
user = User(
    name="Kim",
    age=30
)
```

처럼 Python 객체를 만들고 Database에 저장할 수 있다.

즉:

```text
Python Object
       ↕
Database Row
```

를 연결하는 것이다.

---

# 34. ORM 관계

예를 들어:

```text
Python Class
User

        ↕

Database Table
users
```

그리고:

```text
Python Object

User(
    user_id=1,
    name="Kim",
    age=30
)

        ↕

Database Row

1 | Kim | 30
```

처럼 생각할 수 있다.

---

# 35. DeclarativeBase

SQLAlchemy 2.x 스타일에서는 Base Class를 만들 수 있다.

```python
from sqlalchemy.orm import (
    DeclarativeBase
)

class Base(
    DeclarativeBase
):
    pass
```

앞으로 Database Model들이 이 Base를 상속한다.

---

# 36. Model 만들기

```python
from sqlalchemy import String
from sqlalchemy.orm import (
    DeclarativeBase,
    Mapped,
    mapped_column
)

class Base(
    DeclarativeBase
):
    pass


class User(Base):

    __tablename__ = "users"

    user_id: Mapped[int] = mapped_column(
        primary_key=True
    )

    name: Mapped[str] = mapped_column(
        String(50)
    )

    age: Mapped[int | None]

    email: Mapped[str | None] = mapped_column(
        String(100)
    )
```

---

# 37. Model이란?

여기서:

```python
class User(Base):
```

는 Python Class이면서 Database의:

```text
users Table
```

과 연결되는 Model이다.

---

# 38. __tablename__

```python
__tablename__ = "users"
```

는 이 Model이 연결될 Database Table 이름을 지정한다.

```text
Python
User

Database
users
```

이다.

---

# 39. mapped_column

```python
user_id: Mapped[int] = mapped_column(
    primary_key=True
)
```

는 Column을 정의한다.

Database 개념으로:

```text
user_id
INTEGER
PRIMARY KEY
```

와 연결된다.

---

# 40. Mapped

```python
Mapped[int]
```

처럼 Type Hint를 이용하여 Model의 속성 타입을 표현한다.

예:

```python
name: Mapped[str]
```

```text
name은 문자열 값
```

이라는 의미를 코드에 표현한다.

---

# 41. None이 가능한 Column

```python
age: Mapped[int | None]
```

처럼 작성하면 값이 없을 수 있는 속성을 표현할 수 있다.

즉 Database의:

```text
NULL
```

과 연결해서 이해할 수 있다.

실제 nullability는 Model 정의와 Database Schema를 함께 정확히 확인해야 한다.

---

# 42. Table 생성

Model을 정의한 뒤:

```python
Base.metadata.create_all(
    engine
)
```

을 실행하면 정의된 Model을 기준으로 필요한 Table을 생성할 수 있다.

전체:

```python
engine = create_engine(
    "sqlite:///study.db"
)

Base.metadata.create_all(
    engine
)
```

---

# 43. create_all의 의미

```text
Python Model
    ↓
Metadata
    ↓
Database Table 생성
```

으로 이해할 수 있다.

다만 실제 서비스에서 Schema 변경을 관리할 때는 Migration 도구를 사용하는 경우가 많다.

---

# 44. Migration

서비스 개발 중에는 Table 구조가 바뀔 수 있다.

예:

```text
기존 users

user_id
name
age
```

나중에:

```text
email 추가
```

가 필요할 수 있다.

이런 Database Schema 변경 이력을 관리하는 것을 Migration이라고 한다.

SQLAlchemy 환경에서는 Alembic 같은 Migration 도구를 함께 사용하는 경우가 많다.

지금 단계에서는 개념만 알아둔다.

---

# 45. Session

ORM을 사용할 때 매우 중요한 개념이다.

```text
Session
```

은 ORM 객체를 이용한 Database 작업을 관리하는 작업 단위라고 생각할 수 있다.

예:

```text
Python Object 생성
        ↓
Session에 추가
        ↓
Commit
        ↓
Database 저장
```

---

# 46. Session 만들기

```python
from sqlalchemy.orm import (
    Session
)

with Session(engine) as session:
    pass
```

---

# 47. 사용자 객체 생성

```python
user = User(
    name="Kim",
    age=30,
    email="kim@example.com"
)
```

현재는 Python 객체를 만든 것이다.

아직 Database에 저장되었다고 생각하면 안 된다.

---

# 48. Session에 추가

```python
with Session(engine) as session:

    user = User(
        name="Kim",
        age=30,
        email="kim@example.com"
    )

    session.add(
        user
    )
```

이제 Session이 해당 객체를 관리한다.

---

# 49. Commit

실제 변경을 확정한다.

```python
with Session(engine) as session:

    user = User(
        name="Kim",
        age=30,
        email="kim@example.com"
    )

    session.add(
        user
    )

    session.commit()
```

흐름:

```text
User 객체 생성
      ↓
session.add()
      ↓
session.commit()
      ↓
Database 저장
```

---

# 50. 여러 객체 추가

```python
user1 = User(
    name="Kim",
    age=30
)

user2 = User(
    name="Lee",
    age=25
)

with Session(engine) as session:

    session.add_all(
        [
            user1,
            user2
        ]
    )

    session.commit()
```

---

# 51. ORM으로 SELECT

SQLAlchemy 2.x 스타일에서는:

```python
from sqlalchemy import select
```

를 사용할 수 있다.

```python
stmt = select(User)
```

개념적으로 SQL의:

```sql
SELECT *
FROM users;
```

와 연결해서 생각할 수 있다.

---

# 52. SELECT 실행

```python
from sqlalchemy import select
from sqlalchemy.orm import Session

with Session(engine) as session:

    stmt = select(User)

    users = session.scalars(
        stmt
    ).all()

    for user in users:
        print(
            user.name,
            user.age
        )
```

---

# 53. WHERE 조건

SQL:

```sql
SELECT *
FROM users
WHERE age >= 30;
```

ORM:

```python
stmt = (
    select(User)
    .where(
        User.age >= 30
    )
)
```

---

# 54. 조건 조회 전체

```python
with Session(engine) as session:

    stmt = (
        select(User)
        .where(
            User.age >= 30
        )
    )

    users = session.scalars(
        stmt
    ).all()

    for user in users:
        print(
            user.name,
            user.age
        )
```

---

# 55. 특정 사용자 조회

Primary Key로 하나의 객체를 가져올 때:

```python
with Session(engine) as session:

    user = session.get(
        User,
        1
    )

    print(user)
```

여기서:

```text
User
→ Model

1
→ Primary Key 값
```

이다.

---

# 56. UPDATE

ORM에서는 객체의 값을 변경하고 Commit할 수 있다.

```python
with Session(engine) as session:

    user = session.get(
        User,
        1
    )

    if user is not None:

        user.age = 31

        session.commit()
```

SQL 개념으로는:

```sql
UPDATE users
SET age = 31
WHERE user_id = 1;
```

와 연결된다.

---

# 57. DELETE

```python
with Session(engine) as session:

    user = session.get(
        User,
        1
    )

    if user is not None:

        session.delete(
            user
        )

        session.commit()
```

SQL:

```sql
DELETE FROM users
WHERE user_id = 1;
```

와 연결된다.

---

# 58. ORM CRUD

정리하면:

```text
Create
→ 객체 생성
→ session.add()
→ commit()

Read
→ select()
→ session.scalars()
또는 session.get()

Update
→ 객체 조회
→ 속성 변경
→ commit()

Delete
→ 객체 조회
→ session.delete()
→ commit()
```

이다.

---

# 59. SQL과 ORM 비교

SQL:

```sql
SELECT *
FROM users
WHERE age >= 30;
```

ORM:

```python
stmt = (
    select(User)
    .where(
        User.age >= 30
    )
)
```

SQL:

```sql
INSERT INTO users ...
```

ORM:

```python
user = User(...)
session.add(user)
session.commit()
```

SQL:

```sql
UPDATE users ...
```

ORM:

```python
user.age = 31
session.commit()
```

SQL:

```sql
DELETE FROM users ...
```

ORM:

```python
session.delete(user)
session.commit()
```

---

# 60. SQL을 배우고 ORM을 배워야 하는 이유

ORM을 사용하면 SQL을 직접 작성하는 양이 줄어들 수 있다.

하지만 Database에서는 결국:

```text
SELECT
INSERT
UPDATE
DELETE
JOIN
Transaction
Primary Key
Foreign Key
```

같은 작업이 이루어진다.

따라서 SQL을 이해해야 ORM 코드가 실제 Database에서 어떤 작업을 하는지 이해할 수 있다.

---

# 61. Foreign Key ORM Model

두 Table을 연결해보자.

```text
users
orders
```

Order Model:

```python
from sqlalchemy import ForeignKey

class Order(Base):

    __tablename__ = "orders"

    order_id: Mapped[int] = mapped_column(
        primary_key=True
    )

    user_id: Mapped[int] = mapped_column(
        ForeignKey(
            "users.user_id"
        )
    )

    product: Mapped[str] = mapped_column(
        String(100)
    )
```

---

# 62. ForeignKey

```python
ForeignKey(
    "users.user_id"
)
```

는:

```text
orders.user_id
        ↓
users.user_id
```

를 연결한다.

앞에서 SQL에서 배운 Foreign Key와 같은 Database 개념이다.

---

# 63. Relationship

ORM에서는 객체 사이의 관계를 표현하기 위해:

```python
relationship()
```

을 사용할 수 있다.

개념적으로:

```text
User 객체
   ↓
해당 사용자의 Orders
```

처럼 접근할 수 있도록 관계를 정의할 수 있다.

---

# 64. 관계 개념

Database:

```text
users
1
│
│
N
orders
```

ORM:

```text
User
1
│
│
N
Order
```

이다.

즉 관계형 Database의 관계를 Python 객체 관계로 표현한다.

---

# 65. FastAPI와 SQLAlchemy

나중에 FastAPI에서는 다음과 같은 구조를 만들 수 있다.

```text
Client
   ↓
HTTP Request
   ↓
FastAPI
   ↓
SQLAlchemy
   ↓
Database
```

예:

```text
POST /users
```

요청이 들어오면:

```text
FastAPI
   ↓
User 객체 생성
   ↓
session.add()
   ↓
session.commit()
   ↓
Database 저장
```

으로 이어질 수 있다.

---

# 66. GET 요청

예:

```text
GET /users/1
```

흐름:

```text
Client
   ↓
FastAPI
   ↓
SQLAlchemy
   ↓
session.get(User, 1)
   ↓
Database
   ↓
User
   ↓
JSON Response
```

이다.

---

# 67. AI 서비스와 SQLAlchemy

AI 서비스를 만든다고 생각해보자.

사용자가 데이터를 입력한다.

```text
사용자 입력
    ↓
FastAPI
    ↓
AI Model
    ↓
Prediction
```

예측 결과를 Database에 저장할 수 있다.

```text
Prediction
    ↓
SQLAlchemy
    ↓
Database
```

---

# 68. 예측 기록 Model 예

개념적으로:

```python
class Prediction(Base):

    __tablename__ = "predictions"

    prediction_id: Mapped[int] = mapped_column(
        primary_key=True
    )

    result: Mapped[str]

    probability: Mapped[float]
```

같은 Model을 만들 수 있다.

---

# 69. AI 예측 저장

개념적으로:

```python
prediction = Prediction(
    result="Pneumonia",
    probability=0.91
)

session.add(
    prediction
)

session.commit()
```

처럼 예측 결과를 저장할 수 있다.

---

# 70. 나중에 조회

사용자의 과거 예측 기록을:

```python
stmt = select(
    Prediction
)
```

같은 방식으로 조회할 수 있다.

이를 API와 연결하면:

```text
GET /predictions
```

같은 기능을 만들 수 있다.

---

# 71. Connection과 Session 차이

처음에는 이 부분이 헷갈릴 수 있다.

Connection:

```text
SQL을 직접 실행하는
Database 연결 작업
```

에 가깝게 생각할 수 있다.

Session:

```text
ORM 객체를 중심으로
Database 작업을 관리
```

한다고 생각하면 이해하기 쉽다.

---

# 72. Engine / Connection / Session

정리:

```text
Engine
→ Database 연결 설정과 연결 관리의 중심

Connection
→ SQL 실행을 위한 연결

Session
→ ORM 객체와 Transaction 작업 관리
```

이다.

---

# 73. SQLAlchemy 구조

전체적으로:

```text
Python
   ↓
Engine
   ↓
Connection
   ↓
SQL
   ↓
Database
```

또는 ORM에서는:

```text
Python Object
     ↓
Session
     ↓
SQLAlchemy ORM
     ↓
Engine
     ↓
Database
```

라고 생각할 수 있다.

---

# 74. Pandas와 SQLAlchemy

SQLAlchemy를 이용해 Database의 데이터를 Pandas로 가져오는 경우도 있다.

개념적으로:

```python
import pandas as pd

df = pd.read_sql(
    "SELECT * FROM users",
    engine
)
```

이렇게:

```text
Database
    ↓
SQLAlchemy
    ↓
Pandas DataFrame
```

흐름을 만들 수 있다.

---

# 75. 데이터 분석 전체 흐름

```text
Database
    ↓
SQL
    ↓
SQLAlchemy
    ↓
Pandas
    ↓
EDA
    ↓
Preprocessing
    ↓
Feature Engineering
    ↓
Machine Learning
```

지금까지 배운 Chapter들이 서로 연결된다.

---

# 76. Database 비밀번호 주의

PostgreSQL이나 MySQL 등에 연결하면 Database 계정과 비밀번호가 필요할 수 있다.

다음처럼 코드에 직접 비밀번호를 적고 GitHub에 올리는 것은 피해야 한다.

```python
password = "my-secret-password"
```

특히 공개 GitHub Repository에:

```text
Database Password

API Key

Secret Key
```

등을 Commit하면 안 된다.

---

# 77. 환경변수

실제 프로젝트에서는 중요한 설정을 환경변수로 관리할 수 있다.

예:

```text
DATABASE_URL
```

그리고 Python에서:

```python
import os

database_url = os.getenv(
    "DATABASE_URL"
)
```

처럼 가져올 수 있다.

---

# 78. .env

개발 환경에서는 `.env` 파일을 이용하기도 한다.

예:

```text
DATABASE_URL=...
```

그리고:

```text
.env
```

파일을 `.gitignore`에 추가하여 Git에 올라가지 않도록 관리한다.

---

# 79. .gitignore

예:

```gitignore
.env
*.db
```

상황에 따라 로컬 Database 파일이나 비밀정보 파일을 Git에서 제외할 수 있다.

단, 프로젝트에서 DB 파일 자체를 예제 데이터로 공유해야 하는 특별한 목적이 있다면 별도로 판단한다.

---

# 80. SQLAlchemy 오류가 발생하면

먼저 다음을 확인한다.

```text
SQLAlchemy가 설치되었는가?

현재 Python 환경이 맞는가?

Database URL이 맞는가?

Database Server가 실행 중인가?

Table이 존재하는가?

Column 이름이 맞는가?

Transaction이 Commit되었는가?

Model과 실제 Table 구조가 일치하는가?
```

---

# 81. ModuleNotFoundError

예:

```text
ModuleNotFoundError:
No module named 'sqlalchemy'
```

라면 먼저:

```powershell
python -m pip install sqlalchemy
```

을 확인한다.

그리고:

```powershell
where python
```

으로 현재 Python 환경도 확인한다.

---

# 82. Table이 없다는 오류

예:

```text
no such table: users
```

같은 오류가 나온다면:

```text
Table 생성 코드를 실행했는가?

올바른 Database 파일에 연결했는가?

Model create_all을 실행했는가?
```

등을 확인한다.

---

# 83. 데이터가 저장되지 않는다면

INSERT나 UPDATE를 했는데 데이터가 보이지 않는다면:

```text
Commit했는가?
```

를 확인한다.

ORM:

```python
session.commit()
```

Transaction:

```python
with engine.begin() as conn:
```

등의 흐름을 확인한다.

---

# 84. ORM을 사용해도 Query를 확인해야 한다

ORM 코드는 편리하지만 성능 문제를 만들 수도 있다.

예를 들어 관계 데이터를 반복해서 조회하면서 Query가 지나치게 많이 실행되는 경우가 있다.

따라서 실제 개발에서는:

```text
어떤 SQL이 실행되는가?

Query가 몇 번 실행되는가?

JOIN이 필요한가?

Index가 필요한가?
```

등을 확인하는 것이 중요하다.

지금 단계에서는 ORM도 결국 SQL로 Database와 통신한다는 점을 기억한다.

---

# 85. SQLAlchemy에서 가장 중요한 흐름

처음에는 다음만 확실히 이해해도 된다.

```text
1. create_engine()

Database와 작업하기 위한 Engine 생성


2. Model 정의

Python Class
↔
Database Table


3. Session 생성

ORM 작업 관리


4. 객체 생성

User(...)


5. session.add()

Session에 추가


6. session.commit()

Database에 변경 확정


7. select()

데이터 조회
```

---

# 86. 간단한 전체 예제

```python
from sqlalchemy import (
    create_engine,
    String,
    select
)

from sqlalchemy.orm import (
    DeclarativeBase,
    Mapped,
    mapped_column,
    Session
)


class Base(
    DeclarativeBase
):
    pass


class User(Base):

    __tablename__ = "users"

    user_id: Mapped[int] = mapped_column(
        primary_key=True
    )

    name: Mapped[str] = mapped_column(
        String(50)
    )

    age: Mapped[int | None]


engine = create_engine(
    "sqlite:///study.db"
)


Base.metadata.create_all(
    engine
)


with Session(engine) as session:

    user = User(
        name="Kim",
        age=30
    )

    session.add(
        user
    )

    session.commit()


with Session(engine) as session:

    stmt = select(
        User
    )

    users = session.scalars(
        stmt
    ).all()

    for user in users:

        print(
            user.user_id,
            user.name,
            user.age
        )
```

---

# 87. 위 코드의 흐름

```text
DeclarativeBase
      ↓
Base 생성
      ↓
User Model 정의
      ↓
Engine 생성
      ↓
Table 생성
      ↓
Session 생성
      ↓
User 객체 생성
      ↓
session.add()
      ↓
session.commit()
      ↓
Database 저장
      ↓
select(User)
      ↓
Database 조회
```

이다.

---

# 88. SQL과 비교

Model:

```python
class User(Base):
    ...
```

SQL 개념:

```sql
CREATE TABLE users (...);
```

객체 생성:

```python
User(
    name="Kim",
    age=30
)
```

SQL:

```sql
INSERT INTO users (...);
```

조회:

```python
select(User)
```

SQL:

```sql
SELECT *
FROM users;
```

수정:

```python
user.age = 31
session.commit()
```

SQL:

```sql
UPDATE users
SET age = 31
WHERE ...;
```

삭제:

```python
session.delete(user)
session.commit()
```

SQL:

```sql
DELETE FROM users
WHERE ...;
```

---

# 89. SQLAlchemy를 공부하면서 가장 중요한 것

SQLAlchemy 문법만 외우지 않는다.

항상:

```text
이 Python 코드가
Database에서는 어떤 작업인가?
```

를 생각한다.

예:

```python
session.add(user)
```

를 보면:

```text
새로운 Row를 추가하려고 하는구나.
```

라고 이해한다.

```python
select(User)
```

를 보면:

```text
users Table을 조회하려는 것이구나.
```

라고 이해한다.

---

# 90. 전체 서비스 연결

최종적으로 우리가 만들 수 있는 구조는:

```text
Frontend
    ↓
HTTP Request
    ↓
FastAPI
    ↓
SQLAlchemy
    ↓
Database
```

AI 모델이 있다면:

```text
Frontend
    ↓
FastAPI
    ↓
AI Model
    ↓
Prediction
    ↓
SQLAlchemy
    ↓
Database
```

가 될 수 있다.

그리고 과거 결과 조회:

```text
Database
    ↓
SQLAlchemy
    ↓
FastAPI
    ↓
JSON
    ↓
Frontend
```

로 이어진다.

---

# 91. 핵심 용어 정리

```text
SQLAlchemy
→ Python에서 관계형 Database를 다루기 위한 라이브러리

Engine
→ Database 연결 설정과 Connection 관리의 중심

Connection
→ SQL을 실행하는 Database 연결

Transaction
→ 여러 Database 작업을 하나의 작업 단위로 관리

COMMIT
→ 변경사항 확정

ROLLBACK
→ 변경사항 되돌리기

ORM
→ Python 객체와 Database Table을 연결

Model
→ Database Table을 표현하는 Python Class

Session
→ ORM 객체와 Database 작업을 관리

DeclarativeBase
→ ORM Model들의 Base Class

Mapped
→ ORM 속성의 Mapping Type 표현

mapped_column
→ ORM Column 정의

select
→ ORM 방식의 조회문 구성

ForeignKey
→ 다른 Table과 관계 연결

Migration
→ Database Schema 변경 이력 관리
```

---

# 92. 배운 점

SQLAlchemy는:

```text
Python
↕
Database
```

를 연결하는 역할을 한다.

SQL을 직접 실행하는 방식에서는:

```text
Engine
   ↓
Connection
   ↓
SQL
   ↓
Database
```

구조를 사용할 수 있다.

ORM을 사용하면:

```text
Python Object
   ↓
Session
   ↓
SQLAlchemy ORM
   ↓
Engine
   ↓
Database
```

구조로 생각할 수 있다.

예를 들어:

```python
user = User(
    name="Kim",
    age=30
)

session.add(user)
session.commit()
```

은 Python 객체를 Database Row로 저장하는 작업으로 이해할 수 있다.

중요한 것은 ORM이 SQL을 없애는 것이 아니라는 점이다.

Database에서는 여전히:

```text
SELECT
INSERT
UPDATE
DELETE
JOIN
Transaction
```

같은 개념이 존재한다.

따라서:

```text
SQL 기초
     ↓
SQL Query
     ↓
SQLAlchemy
     ↓
FastAPI + Database
```

순서로 이해하면 전체 구조가 자연스럽게 연결된다.

최종적으로 웹/AI 서비스에서는:

```text
Frontend
   ↓
HTTP
   ↓
FastAPI
   ↓
SQLAlchemy
   ↓
Database
```

그리고:

```text
FastAPI
   ↓
AI Model
   ↓
Prediction
   ↓
SQLAlchemy
   ↓
Database
```

같은 구조를 만들 수 있다.