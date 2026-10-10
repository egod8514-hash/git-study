# MongoDB 기초

## 1. MongoDB란?

MongoDB는 대표적인 NoSQL Database 중 하나이다.

관계형 Database에서는 데이터를 주로:

```text
Table
Row
Column
```

구조로 저장했다.

MongoDB에서는:

```text
Collection
Document
Field
```

구조를 사용한다.

예를 들어 사용자 한 명을 다음과 비슷한 형태로 저장할 수 있다.

```json
{
    "name": "Kim",
    "age": 30,
    "email": "kim@example.com"
}
```

JSON과 매우 비슷한 형태이다.

---

# 2. SQL Database 복습

관계형 Database에서는 다음과 같은 Table을 사용할 수 있다.

users:

| user_id | name | age |
|---:|---|---:|
| 1 | Kim | 30 |
| 2 | Lee | 25 |
| 3 | Park | 40 |

구조:

```text
Table
└── Row
    └── Column
```

이다.

---

# 3. MongoDB에서는?

같은 사용자 데이터를 MongoDB에서는 개념적으로:

```json
{
    "_id": 1,
    "name": "Kim",
    "age": 30
}
```

```json
{
    "_id": 2,
    "name": "Lee",
    "age": 25
}
```

처럼 각각 하나의 Document로 저장할 수 있다.

---

# 4. 기본 용어 비교

처음에는 다음처럼 비교하면 이해하기 쉽다.

| 관계형 Database | MongoDB |
|---|---|
| Database | Database |
| Table | Collection |
| Row | Document |
| Column | Field |
| Primary Key | `_id` |

완전히 동일한 개념은 아니지만 처음 구조를 이해하는 데 도움이 된다.

---

# 5. Collection

Collection은 여러 Document를 저장하는 공간이다.

예:

```text
users
```

Collection 안에:

```json
{
    "name": "Kim",
    "age": 30
}
```

```json
{
    "name": "Lee",
    "age": 25
}
```

같은 Document들이 들어갈 수 있다.

관계형 Database의 Table과 비슷한 위치라고 생각할 수 있다.

---

# 6. Document

Document는 하나의 데이터 단위이다.

예:

```json
{
    "name": "Kim",
    "age": 30,
    "email": "kim@example.com"
}
```

하나의 사용자 정보를 하나의 Document로 저장할 수 있다.

관계형 Database의 Row와 비슷한 위치로 이해할 수 있다.

---

# 7. Field

Document 내부의:

```text
name
age
email
```

같은 항목을 Field라고 한다.

예:

```json
{
    "name": "Kim",
    "age": 30
}
```

여기서:

```text
name
→ Field

Kim
→ Value
```

이다.

---

# 8. MongoDB의 전체 구조

```text
MongoDB Server
      ↓
Database
      ↓
Collection
      ↓
Document
      ↓
Field
```

예:

```text
shop
└── users
    ├── Kim Document
    ├── Lee Document
    └── Park Document
```

---

# 9. JSON과 비슷한 구조

MongoDB Document는 JSON과 비슷하게 보인다.

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

문자열, 숫자뿐 아니라 배열과 중첩된 구조도 표현할 수 있다.

---

# 10. BSON

MongoDB는 내부적으로 BSON이라는 형식을 사용한다.

BSON:

```text
Binary JSON
```

이다.

겉으로는 JSON과 비슷하게 보이지만 MongoDB에서는 BSON을 이용하여 JSON보다 다양한 데이터 타입을 지원한다.

처음에는:

```text
MongoDB Document는
JSON과 비슷한 구조로 다룬다.
```

정도로 이해하면 된다.

---

# 11. 중첩된 Document

MongoDB에서는 Document 안에 또 다른 Document를 넣을 수 있다.

```json
{
    "name": "Kim",
    "age": 30,
    "address": {
        "city": "Seoul",
        "zip_code": "12345"
    }
}
```

여기서 `address` 자체가 하나의 중첩 구조이다.

---

# 12. 배열 저장

Document 안에 배열을 넣을 수도 있다.

```json
{
    "name": "Kim",
    "skills": [
        "Python",
        "SQL",
        "FastAPI"
    ]
}
```

관계형 Database와 비교했을 때 구조적으로 다른 특징 중 하나이다.

---

# 13. Document마다 구조가 다를 수 있다

예:

```json
{
    "name": "Kim",
    "age": 30
}
```

다른 Document:

```json
{
    "name": "Lee",
    "age": 25,
    "phone": "010-0000-0000"
}
```

두 번째 Document에만:

```text
phone
```

Field가 있을 수도 있다.

---

# 14. Schema가 없다는 뜻?

MongoDB를 흔히:

```text
Schema-less
```

라고 표현하기도 한다.

하지만 이것을:

```text
아무 구조 없이 마음대로 저장해도 된다.
```

라고 이해하면 안 된다.

실제 서비스에서는 데이터 구조를 일정하게 관리하는 것이 중요하다.

MongoDB에서도 Schema Validation 등을 이용할 수 있다.

---

# 15. 관계형 Database와 차이

관계형 Database에서는 Table 구조를 명확하게 정의한다.

예:

```text
users

id      INTEGER
name    VARCHAR
age     INTEGER
```

MongoDB에서는 Document 중심으로 데이터를 저장한다.

```json
{
    "name": "Kim",
    "age": 30
}
```

따라서 데이터 모델링 방식이 다르다.

---

# 16. _id

MongoDB Document에는 특별한 Field가 있다.

```text
_id
```

이다.

각 Document를 고유하게 식별하는 역할을 한다.

예:

```json
{
    "_id": 1,
    "name": "Kim"
}
```

---

# 17. _id를 직접 넣지 않으면?

MongoDB에서 `_id`를 직접 지정하지 않으면 일반적으로 MongoDB Driver가 ObjectId 값을 생성해 사용할 수 있다.

예:

```text
ObjectId(...)
```

형태를 볼 수 있다.

---

# 18. ObjectId

ObjectId는 MongoDB에서 `_id` 값으로 자주 사용하는 식별자이다.

Python에서 조회 결과를 보면:

```python
ObjectId("...")
```

같은 형태를 볼 수 있다.

즉 관계형 Database의:

```text
Primary Key
```

처럼 Document를 구분하는 중요한 역할을 한다.

---

# 19. CRUD

MongoDB에서도 기본 데이터 작업은 CRUD이다.

```text
Create
Read
Update
Delete
```

관계형 Database와 목적은 같다.

하지만 사용하는 명령과 문법이 다르다.

---

# 20. SQL과 MongoDB CRUD 비교

| CRUD | SQL | MongoDB |
|---|---|---|
| Create | INSERT | insert_one / insert_many |
| Read | SELECT | find_one / find |
| Update | UPDATE | update_one / update_many |
| Delete | DELETE | delete_one / delete_many |

Python에서 PyMongo를 사용할 때 위와 같은 메서드를 자주 볼 수 있다.

---

# 21. PyMongo

Python에서 MongoDB를 사용할 때 대표적으로:

```text
PyMongo
```

를 사용할 수 있다.

설치:

```powershell
python -m pip install pymongo
```

확인:

```powershell
pip show pymongo
```

---

# 22. MongoDB Server도 필요하다

PyMongo를 설치했다고 MongoDB Database Server 자체가 설치되는 것은 아니다.

```text
PyMongo
→ Python에서 MongoDB와 통신하기 위한 Driver

MongoDB
→ 실제 Database 시스템
```

이라고 구분해야 한다.

---

# 23. 전체 구조

```text
Python
   ↓
PyMongo
   ↓
MongoDB Server
   ↓
Database
   ↓
Collection
   ↓
Document
```

이다.

SQLAlchemy에서:

```text
Python
↓
SQLAlchemy
↓
RDBMS
```

로 연결했던 것과 비슷한 위치이다.

---

# 24. MongoClient

Python에서 MongoDB에 연결할 때:

```python
from pymongo import MongoClient

client = MongoClient(
    "mongodb://localhost:27017/"
)
```

처럼 MongoClient를 사용할 수 있다.

---

# 25. localhost

```text
localhost
```

는 현재 내 컴퓨터를 의미한다.

즉:

```text
내 컴퓨터에서 실행 중인 MongoDB Server
```

에 연결한다는 의미이다.

---

# 26. 27017

MongoDB가 기본적으로 자주 사용하는 Port 번호이다.

```text
mongodb://localhost:27017/
```

을 분해하면:

```text
mongodb
→ MongoDB 연결

localhost
→ 현재 컴퓨터

27017
→ Port
```

로 생각할 수 있다.

---

# 27. Database 선택

예:

```python
db = client[
    "study_db"
]
```

이제:

```text
study_db
```

Database를 사용한다.

---

# 28. Collection 선택

```python
users = db[
    "users"
]
```

구조:

```text
study_db
└── users
```

이다.

여기서 `users`는 Collection이다.

---

# 29. 전체 연결

```python
from pymongo import MongoClient

client = MongoClient(
    "mongodb://localhost:27017/"
)

db = client[
    "study_db"
]

users = db[
    "users"
]
```

흐름:

```text
MongoDB Server
      ↓
study_db
      ↓
users Collection
```

이다.

---

# 30. Create - insert_one

Document 하나를 저장해보자.

```python
user = {
    "name": "Kim",
    "age": 30,
    "email": "kim@example.com"
}

result = users.insert_one(
    user
)
```

---

# 31. Python Dictionary와 Document

Python:

```python
{
    "name": "Kim",
    "age": 30
}
```

MongoDB:

```text
Document
```

PyMongo를 사용할 때 Python Dictionary와 MongoDB Document가 자연스럽게 연결된다.

---

# 32. inserted_id

저장된 Document의 `_id`를 확인할 수 있다.

```python
print(
    result.inserted_id
)
```

---

# 33. insert_many

여러 Document를 한 번에 추가할 수도 있다.

```python
data = [
    {
        "name": "Lee",
        "age": 25
    },
    {
        "name": "Park",
        "age": 40
    },
    {
        "name": "Choi",
        "age": 35
    }
]

result = users.insert_many(
    data
)
```

---

# 34. 여러 ID 확인

```python
print(
    result.inserted_ids
)
```

로 추가된 Document들의 ID를 확인할 수 있다.

---

# 35. Read - find_one

Document 하나를 조회한다.

```python
user = users.find_one(
    {
        "name": "Kim"
    }
)

print(user)
```

---

# 36. SQL과 비교

SQL:

```sql
SELECT *
FROM users
WHERE name = 'Kim';
```

PyMongo:

```python
users.find_one(
    {
        "name": "Kim"
    }
)
```

목적은 비슷하다.

```text
name이 Kim인 데이터 조회
```

이다.

---

# 37. find

여러 Document를 조회할 수 있다.

```python
result = users.find(
    {
        "age": 30
    }
)

for user in result:
    print(user)
```

---

# 38. 모든 Document 조회

```python
result = users.find()

for user in result:
    print(user)
```

SQL로 생각하면:

```sql
SELECT *
FROM users;
```

와 비슷한 목적이다.

---

# 39. 비교 연산자

SQL에서는:

```sql
WHERE age >= 30
```

이라고 작성했다.

MongoDB Query에서는:

```python
{
    "age": {
        "$gte": 30
    }
}
```

처럼 표현할 수 있다.

---

# 40. $gte

```text
$gte
```

는:

```text
greater than or equal
```

즉:

```text
크거나 같다
```

이다.

사용:

```python
users.find(
    {
        "age": {
            "$gte": 30
        }
    }
)
```

---

# 41. 비교 연산자 정리

대표적으로:

```text
$eq
→ 같다

$ne
→ 같지 않다

$gt
→ 크다

$gte
→ 크거나 같다

$lt
→ 작다

$lte
→ 작거나 같다

$in
→ 여러 값 중 하나
```

등을 사용할 수 있다.

---

# 42. SQL과 비교

SQL:

```sql
SELECT *
FROM users
WHERE age >= 30;
```

MongoDB:

```python
users.find(
    {
        "age": {
            "$gte": 30
        }
    }
)
```

Pandas:

```python
df.loc[
    df["age"] >= 30
]
```

세 가지 모두 목적은:

```text
age가 30 이상인 데이터 조회
```

이다.

---

# 43. 여러 조건

예:

```python
users.find(
    {
        "age": {
            "$gte": 30
        },
        "name": "Kim"
    }
)
```

같은 Document 안의 조건들은 기본적으로 AND 관계로 동작하는 경우가 많다.

즉:

```text
age >= 30
AND
name = Kim
```

이다.

---

# 44. $or

OR 조건:

```python
users.find(
    {
        "$or": [
            {
                "age": {
                    "$lt": 30
                }
            },
            {
                "age": {
                    "$gte": 40
                }
            }
        ]
    }
)
```

---

# 45. Projection

특정 Field만 가져올 수도 있다.

```python
users.find(
    {},
    {
        "name": 1,
        "age": 1
    }
)
```

개념적으로 SQL의:

```sql
SELECT name, age
FROM users;
```

와 비슷하다.

---

# 46. _id 제외

Projection을 사용하면 `_id`가 함께 나타날 수 있다.

제외하려면:

```python
users.find(
    {},
    {
        "_id": 0,
        "name": 1,
        "age": 1
    }
)
```

처럼 사용할 수 있다.

---

# 47. Update - update_one

특정 Document 하나를 수정한다.

```python
users.update_one(
    {
        "name": "Kim"
    },
    {
        "$set": {
            "age": 31
        }
    }
)
```

---

# 48. $set

```text
$set
```

은 특정 Field 값을 설정하거나 변경할 때 사용한다.

위 코드는:

```text
name이 Kim인 Document를 찾아서
age를 31로 변경
```

한다.

---

# 49. SQL UPDATE와 비교

SQL:

```sql
UPDATE users
SET age = 31
WHERE name = 'Kim';
```

MongoDB:

```python
users.update_one(
    {
        "name": "Kim"
    },
    {
        "$set": {
            "age": 31
        }
    }
)
```

---

# 50. update_many

여러 Document를 수정할 수 있다.

```python
users.update_many(
    {
        "age": {
            "$gte": 30
        }
    },
    {
        "$set": {
            "group": "30_plus"
        }
    }
)
```

age가 30 이상인 여러 Document에:

```text
group = 30_plus
```

Field를 추가하거나 변경할 수 있다.

---

# 51. MongoDB의 유연한 구조

기존 Document가:

```json
{
    "name": "Kim",
    "age": 31
}
```

였는데:

```text
group
```

Field를 새롭게 추가할 수도 있다.

결과:

```json
{
    "name": "Kim",
    "age": 31,
    "group": "30_plus"
}
```

가 될 수 있다.

---

# 52. Delete - delete_one

Document 하나를 삭제한다.

```python
users.delete_one(
    {
        "name": "Kim"
    }
)
```

SQL:

```sql
DELETE FROM users
WHERE name = 'Kim';
```

와 비슷한 목적이다.

---

# 53. delete_many

조건에 맞는 여러 Document를 삭제한다.

```python
users.delete_many(
    {
        "age": {
            "$lt": 20
        }
    }
)
```

---

# 54. 삭제도 주의

SQL에서:

```sql
DELETE FROM users;
```

가 위험했던 것처럼 MongoDB에서도 조건 없이 여러 데이터를 삭제하는 작업은 주의해야 한다.

예:

```python
users.delete_many({})
```

는 Collection 안의 많은 Document를 삭제할 수 있다.

따라서 삭제 전에 먼저 조회한다.

```python
result = users.find(
    {
        "age": {
            "$lt": 20
        }
    }
)
```

대상을 확인한 후 삭제하는 습관이 좋다.

---

# 55. count_documents

조건에 맞는 Document 수를 확인할 수 있다.

```python
count = users.count_documents(
    {}
)

print(count)
```

전체 Document 개수를 확인한다.

---

# 56. 조건에 맞는 개수

```python
count = users.count_documents(
    {
        "age": {
            "$gte": 30
        }
    }
)
```

의미:

```text
30세 이상 사용자가 몇 명인가?
```

이다.

---

# 57. sort

정렬:

```python
result = users.find().sort(
    "age",
    1
)
```

`1`은 오름차순이다.

```python
result = users.find().sort(
    "age",
    -1
)
```

`-1`은 내림차순이다.

---

# 58. SQL ORDER BY와 비교

SQL:

```sql
SELECT *
FROM users
ORDER BY age DESC;
```

MongoDB:

```python
users.find().sort(
    "age",
    -1
)
```

---

# 59. limit

조회 개수를 제한할 수 있다.

```python
result = (
    users
    .find()
    .sort(
        "age",
        -1
    )
    .limit(3)
)
```

의미:

```text
나이가 높은 순으로
3개만 조회
```

이다.

---

# 60. SQL과 비교

SQL:

```sql
SELECT *
FROM users
ORDER BY age DESC
LIMIT 3;
```

MongoDB:

```python
users.find().sort(
    "age",
    -1
).limit(
    3
)
```

---

# 61. Nested Document 조회

Document:

```json
{
    "name": "Kim",
    "address": {
        "city": "Seoul",
        "zip_code": "12345"
    }
}
```

도시로 검색하고 싶다면 Dot Notation을 사용할 수 있다.

```python
users.find(
    {
        "address.city": "Seoul"
    }
)
```

---

# 62. 배열 검색

Document:

```json
{
    "name": "Kim",
    "skills": [
        "Python",
        "SQL"
    ]
}
```

Python을 가진 사용자를 찾을 수 있다.

```python
users.find(
    {
        "skills": "Python"
    }
)
```

MongoDB는 이런 배열 및 중첩 Document 구조를 자연스럽게 다룰 수 있다.

---

# 63. 관계형 Database에서는?

예를 들어 사용자의 기술을 관계형 Database에서 정규화해서 저장한다면:

```text
users

skills

user_skills
```

같이 여러 Table로 나눌 수도 있다.

MongoDB에서는 사용 사례에 따라:

```json
{
    "name": "Kim",
    "skills": [
        "Python",
        "SQL"
    ]
}
```

처럼 하나의 Document 안에 포함시키는 모델링을 고려할 수 있다.

---

# 64. Embedded Document

관련 데이터를 하나의 Document 안에 포함하는 방식을 생각할 수 있다.

예:

```json
{
    "name": "Kim",
    "address": {
        "city": "Seoul",
        "zip_code": "12345"
    }
}
```

이를 Embedded Document 구조로 볼 수 있다.

---

# 65. Reference

모든 데이터를 무조건 하나의 Document에 넣는 것은 아니다.

Document 사이의 ID를 이용하여 관계를 표현할 수도 있다.

예:

```json
{
    "order_id": 101,
    "user_id": 1,
    "product": "Keyboard"
}
```

처럼 다른 Document의 식별자를 저장할 수 있다.

---

# 66. Embed vs Reference

MongoDB 모델링에서는 중요한 선택이다.

```text
Embed
→ 관련 데이터를 Document 내부에 포함

Reference
→ 다른 Document의 ID를 저장하여 연결
```

한다.

어느 것이 무조건 좋은 것은 아니다.

데이터를:

```text
어떻게 조회하는가?

함께 변경되는가?

데이터 크기는 얼마나 되는가?

관계가 어떻게 되는가?
```

등을 고려한다.

---

# 67. SQL의 JOIN과 MongoDB

관계형 Database에서는:

```sql
SELECT *
FROM users AS u
INNER JOIN orders AS o
    ON u.user_id = o.user_id;
```

처럼 JOIN을 많이 사용한다.

MongoDB는 Document 중심 모델링을 사용하기 때문에 데이터를 한 Document 안에 포함시키는 설계를 선택할 수도 있다.

MongoDB에도 Collection 간 데이터를 결합할 수 있는 기능이 있지만 관계형 Database의 JOIN 중심 설계와 사고방식이 완전히 같지는 않다.

---

# 68. Aggregation

MongoDB에는:

```text
Aggregation Pipeline
```

이라는 강력한 데이터 처리 기능이 있다.

개념적으로:

```text
데이터 선택
   ↓
필터링
   ↓
그룹화
   ↓
계산
   ↓
정렬
```

같은 작업을 단계적으로 수행할 수 있다.

---

# 69. Aggregation Pipeline

예:

```python
pipeline = [
    {
        "$match": {
            "age": {
                "$gte": 30
            }
        }
    },
    {
        "$sort": {
            "age": -1
        }
    }
]

result = users.aggregate(
    pipeline
)
```

---

# 70. $match

```text
$match
```

는 조건에 맞는 Document를 선택한다.

SQL의:

```sql
WHERE
```

와 비슷한 목적이다.

예:

```python
{
    "$match": {
        "age": {
            "$gte": 30
        }
    }
}
```

---

# 71. $group

그룹화와 집계를 할 수도 있다.

예:

```python
pipeline = [
    {
        "$group": {
            "_id": "$group",
            "count": {
                "$sum": 1
            }
        }
    }
]
```

개념적으로 SQL의:

```sql
GROUP BY
```

와 연결해서 생각할 수 있다.

---

# 72. Aggregation은 단계별 흐름

```text
Document
   ↓
$match
   ↓
$group
   ↓
$sort
   ↓
Result
```

처럼 Pipeline을 통과하면서 데이터가 변환된다.

이 개념은 머신러닝의 Pipeline과 이름은 같지만 동일한 기능은 아니다.

공통적으로:

```text
여러 단계를 순서대로 연결한다.
```

는 느낌으로 이해할 수 있다.

---

# 73. Index

Database에서 검색 속도를 높이기 위해 Index를 사용할 수 있다.

예를 들어:

```text
email
```

로 사용자를 매우 자주 찾는다면 Index를 고려할 수 있다.

```python
users.create_index(
    "email"
)
```

---

# 74. Index를 왜 사용할까?

Index가 없다면 많은 Document를 하나씩 확인해야 하는 상황이 생길 수 있다.

Index를 이용하면 특정 Field를 기준으로 데이터를 더 빠르게 찾을 수 있다.

관계형 Database에도 Index 개념이 존재한다.

---

# 75. Index가 무조건 좋은가?

아니다.

Index는:

```text
저장 공간 사용

INSERT / UPDATE 비용 증가

관리 비용 증가
```

등의 영향을 줄 수 있다.

따라서 모든 Field에 무조건 Index를 만드는 것이 아니다.

---

# 76. Unique Index

중복을 허용하지 않는 Index도 만들 수 있다.

예:

```python
users.create_index(
    "email",
    unique=True
)
```

회원 이메일을 고유하게 유지해야 하는 서비스라면 사용할 수 있다.

---

# 77. SQL Primary Key와 비교

관계형 Database에서는:

```text
PRIMARY KEY

UNIQUE
```

같은 Constraint를 사용했다.

MongoDB에서도:

```text
_id

Unique Index
```

등을 이용하여 고유성을 관리할 수 있다.

---

# 78. MongoDB와 Pandas

MongoDB에서 가져온 데이터를 Pandas DataFrame으로 만들 수도 있다.

```python
import pandas as pd

data = list(
    users.find()
)

df = pd.DataFrame(
    data
)

print(
    df.head()
)
```

흐름:

```text
MongoDB
   ↓
PyMongo
   ↓
Python List
   ↓
Pandas DataFrame
```

이다.

---

# 79. AI 데이터 분석과 MongoDB

AI 서비스에서:

```text
MongoDB
   ↓
PyMongo
   ↓
Pandas
   ↓
전처리
   ↓
Machine Learning
```

같은 흐름을 만들 수도 있다.

---

# 80. AI 예측 결과 저장

예를 들어 AI 예측 결과를:

```python
prediction = {
    "user_id": 1,
    "result": "class_1",
    "probability": 0.91
}

predictions.insert_one(
    prediction
)
```

처럼 Document로 저장할 수 있다.

---

# 81. 이미지 자체를 MongoDB에 넣어야 할까?

AI 서비스에서 이미지가 있다고 해서 반드시 이미지 파일 자체를 일반 Document에 그대로 넣어야 하는 것은 아니다.

예를 들어:

```text
이미지 파일
→ 파일/Object Storage

MongoDB
→ 이미지 경로
→ 사용자 ID
→ 예측 결과
→ 확률
→ 생성 시간
```

처럼 Metadata를 저장하는 구조를 사용할 수도 있다.

서비스 요구사항에 따라 저장 방식을 설계한다.

---

# 82. 웹 서비스와 MongoDB

전체 구조:

```text
Frontend
    ↓
HTTP Request
    ↓
FastAPI
    ↓
PyMongo
    ↓
MongoDB
```

예:

```text
POST /users
```

요청이 들어오면:

```python
users.insert_one(
    user_data
)
```

같은 작업으로 이어질 수 있다.

---

# 83. 사용자 조회

```text
GET /users/1
```

Backend에서:

```python
users.find_one(
    {
        "_id": user_id
    }
)
```

같은 조회를 수행할 수 있다.

실제 `_id`가 ObjectId라면 문자열과 ObjectId 타입 차이를 주의해야 한다.

---

# 84. ObjectId 타입 주의

URL에서 받은 값은 일반적으로 문자열일 수 있다.

하지만 MongoDB의 `_id`가 ObjectId라면:

```text
String
```

과:

```text
ObjectId
```

는 서로 다른 타입이다.

필요한 경우:

```python
from bson import ObjectId

object_id = ObjectId(
    id_string
)
```

처럼 변환할 수 있다.

---

# 85. SQL Database가 잘 맞는 경우

예를 들어:

```text
데이터 관계가 명확함

정형 데이터 중심

복잡한 JOIN 필요

강한 데이터 일관성이 중요

Transaction이 중요
```

한 시스템에서는 관계형 Database가 자연스러운 선택이 될 수 있다.

예:

```text
주문
결제
회계
재고
```

등은 관계와 일관성이 매우 중요할 수 있다.

---

# 86. MongoDB가 잘 맞을 수 있는 경우

예를 들어:

```text
Document 중심 데이터

중첩 구조가 자연스러움

데이터 구조가 비교적 유연함

JSON 형태의 데이터와 자연스럽게 연결
```

되는 서비스에서는 MongoDB를 고려할 수 있다.

하지만 실제 선택은 서비스 요구사항과 데이터 모델에 따라 결정한다.

---

# 87. SQL vs MongoDB

단순하게:

```text
SQL은 옛날 기술

MongoDB는 새로운 기술
```

이라고 생각하면 안 된다.

둘은 데이터 모델링 방식과 장단점이 다르다.

---

# 88. SQL Database

구조:

```text
Table
↓
Row
↓
Column
```

관계:

```text
Primary Key
Foreign Key
JOIN
```

중심으로 데이터를 모델링한다.

---

# 89. MongoDB

구조:

```text
Collection
↓
Document
↓
Field
```

그리고:

```text
Nested Document
Array
Embed
Reference
```

같은 구조를 활용할 수 있다.

---

# 90. 같은 데이터 비교

SQL:

```text
users

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

---

# 91. 주소 정보 비교

관계형 Database에서는 상황에 따라:

```text
users
addresses
```

처럼 Table을 나눌 수 있다.

MongoDB에서는 사용 사례에 따라:

```json
{
    "name": "Kim",
    "address": {
        "city": "Seoul",
        "zip_code": "12345"
    }
}
```

처럼 Document 내부에 포함할 수 있다.

---

# 92. 어떤 것을 선택해야 할까?

먼저 다음 질문을 한다.

```text
데이터 구조가 어떤가?

데이터 관계가 복잡한가?

Transaction이 얼마나 중요한가?

데이터를 어떤 방식으로 조회하는가?

중첩된 데이터가 많은가?

Schema가 얼마나 자주 변하는가?

일관성 요구 수준은 어떤가?
```

그리고 적절한 Database를 선택한다.

---

# 93. SQL을 먼저 배운 이유

MongoDB를 사용하더라도 관계형 Database를 먼저 배우는 것은 의미가 있다.

다음 개념을 이해했기 때문이다.

```text
Database

데이터 모델링

Primary Key

Foreign Key

Relationship

CRUD

Index

Transaction
```

이제 MongoDB에서는 이 개념들이 어떻게 달라지는지를 비교할 수 있다.

---

# 94. SQLAlchemy와 PyMongo 비교

관계형 Database:

```text
Python
   ↓
SQLAlchemy
   ↓
PostgreSQL / MySQL / SQLite
```

MongoDB:

```text
Python
   ↓
PyMongo
   ↓
MongoDB
```

이다.

---

# 95. ORM 차이

SQLAlchemy ORM에서는:

```text
Python Class
↕
Database Table
```

구조를 사용했다.

PyMongo에서는 Python Dictionary를 MongoDB Document와 직접 연결해서 다루는 방식이 자연스럽다.

```text
Python dict
↕
MongoDB Document
```

---

# 96. MongoDB에서도 데이터 검증이 필요하다

Document 구조가 유연하다고 해서 데이터 검증이 필요 없는 것은 아니다.

예를 들어:

```json
{
    "age": "thirty"
}
```

처럼 숫자여야 할 값이 문자열로 저장되면 문제가 될 수 있다.

따라서 Backend에서:

```text
입력 데이터 검증
```

이 중요하다.

나중에 FastAPI에서는 Pydantic을 이용한 데이터 검증도 배우게 된다.

---

# 97. 비밀번호와 연결 문자열

MongoDB 연결 정보에도 계정 정보가 포함될 수 있다.

예:

```text
MONGODB_URL
```

같은 환경변수로 관리할 수 있다.

코드에:

```python
password = "my-password"
```

처럼 직접 넣고 GitHub에 Commit하지 않는 것이 중요하다.

---

# 98. .env

예:

```text
MONGODB_URL=...
```

그리고 `.gitignore`:

```gitignore
.env
```

로 관리할 수 있다.

---

# 99. MongoDB 오류가 발생하면

먼저 확인한다.

```text
MongoDB Server가 실행 중인가?

PyMongo가 설치되어 있는가?

연결 주소가 맞는가?

Port가 맞는가?

Database 이름이 맞는가?

Collection 이름이 맞는가?

Field 이름이 맞는가?

_id 타입이 맞는가?

Query 조건이 맞는가?
```

---

# 100. 연결 오류

예를 들어 MongoDB Server가 실행되지 않았다면 Python 코드 자체가 맞아도 연결에 실패할 수 있다.

구조를 구분한다.

```text
Python 코드 정상
       ↓
PyMongo 정상
       ↓
MongoDB Server 연결 실패
```

즉 오류가 항상 Python 문법 문제인 것은 아니다.

---

# 101. PyMongo CRUD 전체 예제

```python
from pymongo import MongoClient


client = MongoClient(
    "mongodb://localhost:27017/"
)

db = client[
    "study_db"
]

users = db[
    "users"
]


# Create
users.insert_one(
    {
        "name": "Kim",
        "age": 30
    }
)


# Read
user = users.find_one(
    {
        "name": "Kim"
    }
)

print(user)


# Update
users.update_one(
    {
        "name": "Kim"
    },
    {
        "$set": {
            "age": 31
        }
    }
)


# Delete
users.delete_one(
    {
        "name": "Kim"
    }
)
```

---

# 102. 전체 흐름

```text
MongoClient
     ↓
Database 선택
     ↓
Collection 선택
     ↓
Document CRUD
```

즉:

```python
client
```

↓

```python
db
```

↓

```python
users
```

↓

```python
insert_one()
find_one()
update_one()
delete_one()
```

이다.

---

# 103. SQL과 MongoDB CRUD 최종 비교

Create:

SQL:

```sql
INSERT INTO users ...
```

MongoDB:

```python
users.insert_one(...)
```

Read:

SQL:

```sql
SELECT *
FROM users;
```

MongoDB:

```python
users.find()
```

Update:

SQL:

```sql
UPDATE users
SET ...
WHERE ...;
```

MongoDB:

```python
users.update_one(
    조건,
    {
        "$set": 변경값
    }
)
```

Delete:

SQL:

```sql
DELETE FROM users
WHERE ...;
```

MongoDB:

```python
users.delete_one(
    조건
)
```

---

# 104. 핵심 용어

```text
MongoDB
→ Document 기반 NoSQL Database

Collection
→ Document들을 저장하는 공간

Document
→ 하나의 데이터 단위

Field
→ Document 내부의 항목

BSON
→ MongoDB가 사용하는 Binary JSON 형식

_id
→ Document의 고유 식별자

ObjectId
→ _id에 자주 사용되는 식별자 타입

PyMongo
→ Python용 MongoDB Driver

MongoClient
→ MongoDB 연결 Client

Embedded Document
→ Document 내부에 포함된 Document

Reference
→ 다른 Document의 식별자를 이용한 연결

Aggregation Pipeline
→ 여러 데이터 처리 단계를 연결

Index
→ 검색 성능을 높이기 위한 자료구조
```

---

# 105. 이번 Chapter와 연결

지금까지 Database를:

```text
Database 기초
      ↓
SQL 기초
      ↓
SQL Query
      ↓
SQLAlchemy
      ↓
MongoDB
```

순서로 배웠다.

관계형 Database:

```text
Python
   ↓
SQLAlchemy
   ↓
RDBMS
```

MongoDB:

```text
Python
   ↓
PyMongo
   ↓
MongoDB
```

로 연결할 수 있다.

---

# 106. 앞으로의 연결

나중에 FastAPI를 배우면:

```text
Frontend
   ↓
HTTP Request
   ↓
FastAPI
   ↓
Database
```

구조를 만들 수 있다.

관계형 Database를 선택하면:

```text
FastAPI
   ↓
SQLAlchemy
   ↓
PostgreSQL
```

같은 구조를 사용할 수 있다.

MongoDB를 선택하면:

```text
FastAPI
   ↓
PyMongo
   ↓
MongoDB
```

같은 구조를 생각할 수 있다.

---

# 107. AI 서비스까지 연결

AI 모델까지 추가하면:

```text
사용자
   ↓
Frontend
   ↓
FastAPI
   ↓
AI Model
   ↓
Prediction
   ↓
Database
```

가 된다.

Database에는 예를 들어:

```text
사용자 정보

입력 정보

예측 결과

예측 확률

생성 시간
```

등을 저장할 수 있다.

---

# 108. 배운 점

MongoDB는 Document 기반 NoSQL Database이다.

관계형 Database가:

```text
Database
→ Table
→ Row
→ Column
```

구조를 사용한다면 MongoDB는:

```text
Database
→ Collection
→ Document
→ Field
```

구조를 사용한다.

Python에서는 PyMongo를 이용하여 MongoDB와 통신할 수 있다.

기본 CRUD:

```text
Create
→ insert_one()

Read
→ find_one()
→ find()

Update
→ update_one()

Delete
→ delete_one()
```

이다.

MongoDB의 중요한 특징은:

```text
Document

Nested Document

Array

Flexible Structure

Aggregation Pipeline
```

등이다.

하지만 MongoDB가 관계형 Database보다 무조건 좋은 것은 아니다.

관계형 Database는:

```text
명확한 관계
JOIN
Transaction
정형 데이터
강한 일관성
```

이 중요한 서비스에서 매우 유용하다.

MongoDB는:

```text
Document 중심 구조
중첩 데이터
JSON과 유사한 데이터
비교적 유연한 구조
```

가 자연스러운 서비스에서 고려할 수 있다.

가장 중요한 것은:

```text
어떤 Database가 더 좋을까?
```

가 아니라:

```text
내 데이터는 어떤 구조인가?

데이터끼리 어떤 관계가 있는가?

어떻게 저장하고 조회할 것인가?

어떤 일관성이 필요한가?
```

를 먼저 생각하는 것이다.