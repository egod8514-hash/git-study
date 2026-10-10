# SQL Query 심화

## 1. 이번에 배울 내용

지금까지 기본적인 SQL을 배웠다.

```sql
SELECT *
FROM users
WHERE age >= 30;
```

이번에는 여기서 한 단계 더 나아간다.

```text
집계 함수
    ↓
COUNT / SUM / AVG / MIN / MAX
    ↓
GROUP BY
    ↓
HAVING
    ↓
JOIN
    ↓
여러 Table 연결
    ↓
Subquery
```

특히 Pandas에서 사용했던:

```text
value_counts()
groupby()
mean()
sum()
merge()
```

와 연결해서 이해하면 쉽다.

---

# 2. 예제 Table

이번에는 두 개의 Table을 사용한다.

users:

| user_id | name | age |
|---:|---|---:|
| 1 | Kim | 30 |
| 2 | Lee | 25 |
| 3 | Park | 40 |
| 4 | Choi | 35 |

orders:

| order_id | user_id | product | price |
|---:|---:|---|---:|
| 101 | 1 | Keyboard | 50000 |
| 102 | 1 | Mouse | 30000 |
| 103 | 2 | Monitor | 200000 |
| 104 | 3 | Laptop | 1500000 |
| 105 | 3 | Mouse | 30000 |

관계는:

```text
users
user_id PK
    │
    │
    ↓
orders
user_id FK
```

이다.

한 명의 사용자가 여러 주문을 할 수 있으므로:

```text
users 1 : N orders
```

관계이다.

---

# 3. 집계 함수

여러 Row를 이용하여 하나의 통계값을 계산하는 함수를 집계 함수라고 한다.

대표적으로:

```text
COUNT
SUM
AVG
MIN
MAX
```

가 있다.

---

# 4. COUNT

Row의 개수를 계산한다.

```sql
SELECT COUNT(*)
FROM users;
```

결과:

```text
4
```

즉:

```text
users에는 몇 명이 있는가?
```

를 확인할 수 있다.

---

# 5. COUNT와 Pandas

Pandas에서는:

```python
len(df)
```

또는 상황에 따라:

```python
df.shape[0]
```

등으로 Row 개수를 확인할 수 있었다.

SQL에서는:

```sql
SELECT COUNT(*)
FROM users;
```

처럼 사용할 수 있다.

---

# 6. COUNT(Column)

```sql
SELECT COUNT(age)
FROM users;
```

처럼 특정 Column을 지정할 수도 있다.

중요한 차이가 있다.

```text
COUNT(*)
→ Row 개수

COUNT(age)
→ age가 NULL이 아닌 Row 개수
```

이다.

---

# 7. COUNT DISTINCT

서로 다른 값의 개수를 확인할 수 있다.

```sql
SELECT COUNT(
    DISTINCT user_id
)
FROM orders;
```

의미:

```text
주문한 사용자가 총 몇 명인가?
```

이다.

Pandas의:

```python
df["user_id"].nunique()
```

와 비슷한 목적이다.

---

# 8. SUM

합계를 계산한다.

```sql
SELECT SUM(price)
FROM orders;
```

예제에서는:

```text
50000
+ 30000
+ 200000
+ 1500000
+ 30000
```

의 합계를 계산한다.

---

# 9. Pandas SUM

Pandas:

```python
df["price"].sum()
```

SQL:

```sql
SELECT SUM(price)
FROM orders;
```

이다.

---

# 10. AVG

평균을 계산한다.

```sql
SELECT AVG(price)
FROM orders;
```

Pandas:

```python
df["price"].mean()
```

와 비슷하다.

---

# 11. MIN

최솟값:

```sql
SELECT MIN(price)
FROM orders;
```

Pandas:

```python
df["price"].min()
```

---

# 12. MAX

최댓값:

```sql
SELECT MAX(price)
FROM orders;
```

Pandas:

```python
df["price"].max()
```

---

# 13. 여러 집계 함수 사용

한 번에 여러 값을 계산할 수 있다.

```sql
SELECT
    COUNT(*) AS order_count,
    SUM(price) AS total_price,
    AVG(price) AS avg_price,
    MIN(price) AS min_price,
    MAX(price) AS max_price
FROM orders;
```

---

# 14. AS

`AS`를 사용하면 결과 Column에 별명을 붙일 수 있다.

```sql
SELECT COUNT(*) AS user_count
FROM users;
```

결과 Column 이름:

```text
user_count
```

이 된다.

---

# 15. Alias

이렇게 붙인 별명을:

```text
Alias
```

라고 한다.

예:

```sql
SELECT
    AVG(price) AS average_price
FROM orders;
```

`AVG(price)`라는 표현보다 `average_price`가 결과를 이해하기 쉽다.

---

# 16. Column Alias

일반 Column에도 사용할 수 있다.

```sql
SELECT
    name AS user_name,
    age AS user_age
FROM users;
```

---

# 17. Table Alias

Table에도 별명을 붙일 수 있다.

```sql
SELECT
    u.name,
    u.age
FROM users AS u;
```

여기서:

```text
users
→ u
```

라는 별명을 사용했다.

JOIN에서 특히 많이 사용한다.

---

# 18. GROUP BY

GROUP BY는 같은 값을 가진 Row들을 그룹으로 묶어서 집계할 때 사용한다.

예를 들어:

```text
사용자별 주문 금액
```

을 계산하고 싶다고 하자.

```sql
SELECT
    user_id,
    SUM(price)
FROM orders
GROUP BY user_id;
```

---

# 19. GROUP BY 결과

orders가:

| order_id | user_id | price |
|---:|---:|---:|
| 101 | 1 | 50000 |
| 102 | 1 | 30000 |
| 103 | 2 | 200000 |
| 104 | 3 | 1500000 |
| 105 | 3 | 30000 |

이라면:

```text
user_id = 1
→ 50000 + 30000

user_id = 2
→ 200000

user_id = 3
→ 1500000 + 30000
```

처럼 그룹별로 계산한다.

---

# 20. GROUP BY + SUM

```sql
SELECT
    user_id,
    SUM(price) AS total_price
FROM orders
GROUP BY user_id;
```

결과:

| user_id | total_price |
|---:|---:|
| 1 | 80000 |
| 2 | 200000 |
| 3 | 1530000 |

---

# 21. Pandas groupby와 비교

Pandas:

```python
df.groupby(
    "user_id"
)["price"].sum()
```

SQL:

```sql
SELECT
    user_id,
    SUM(price)
FROM orders
GROUP BY user_id;
```

같은 목적이다.

---

# 22. 사용자별 주문 횟수

```sql
SELECT
    user_id,
    COUNT(*) AS order_count
FROM orders
GROUP BY user_id;
```

결과:

```text
user 1
→ 2번

user 2
→ 1번

user 3
→ 2번
```

이다.

---

# 23. 사용자별 평균 주문금액

```sql
SELECT
    user_id,
    AVG(price) AS avg_price
FROM orders
GROUP BY user_id;
```

---

# 24. 여러 집계 같이 사용

```sql
SELECT
    user_id,
    COUNT(*) AS order_count,
    SUM(price) AS total_price,
    AVG(price) AS avg_price
FROM orders
GROUP BY user_id;
```

사용자별로:

```text
주문 횟수
총 주문 금액
평균 주문 금액
```

을 한 번에 계산한다.

---

# 25. GROUP BY를 왜 사용할까?

단순히 전체 평균:

```sql
SELECT AVG(price)
FROM orders;
```

을 계산하는 것과:

```sql
SELECT
    user_id,
    AVG(price)
FROM orders
GROUP BY user_id;
```

는 다르다.

첫 번째:

```text
전체 주문의 평균
```

두 번째:

```text
사용자별 주문 평균
```

이다.

---

# 26. WHERE + GROUP BY

특정 데이터만 선택한 후 그룹화할 수 있다.

```sql
SELECT
    user_id,
    SUM(price) AS total_price
FROM orders
WHERE price >= 50000
GROUP BY user_id;
```

흐름:

```text
orders
   ↓
price >= 50000만 선택
   ↓
user_id별 그룹
   ↓
SUM 계산
```

이다.

---

# 27. HAVING

GROUP BY 결과에 조건을 걸고 싶다면:

```text
HAVING
```

을 사용한다.

예:

```sql
SELECT
    user_id,
    SUM(price) AS total_price
FROM orders
GROUP BY user_id
HAVING SUM(price) >= 100000;
```

의미:

```text
사용자별 주문금액을 합산한 후
총 주문금액이 100000 이상인 사용자만 조회
```

이다.

---

# 28. WHERE와 HAVING 차이

매우 중요하다.

```text
WHERE
→ GROUP BY 전에 Row를 필터링

HAVING
→ GROUP BY 후 그룹 결과를 필터링
```

이다.

---

# 29. WHERE 예제

```sql
SELECT *
FROM orders
WHERE price >= 100000;
```

각 주문 Row를 기준으로:

```text
price >= 100000
```

인지 확인한다.

---

# 30. HAVING 예제

```sql
SELECT
    user_id,
    SUM(price)
FROM orders
GROUP BY user_id
HAVING SUM(price) >= 100000;
```

개별 주문이 아니라:

```text
사용자별 합계
```

를 기준으로 조건을 적용한다.

---

# 31. Pandas와 비교

Pandas에서는 개념적으로:

```python
result = (
    df.groupby("user_id")["price"]
      .sum()
)

result[
    result >= 100000
]
```

와 비슷한 작업이다.

SQL에서는:

```sql
SELECT
    user_id,
    SUM(price)
FROM orders
GROUP BY user_id
HAVING SUM(price) >= 100000;
```

이다.

---

# 32. SQL 작성 순서

대표적인 SELECT Query는:

```sql
SELECT
FROM
WHERE
GROUP BY
HAVING
ORDER BY
LIMIT
```

순서로 작성한다.

예:

```sql
SELECT
    user_id,
    SUM(price) AS total_price
FROM orders
WHERE price >= 30000
GROUP BY user_id
HAVING SUM(price) >= 100000
ORDER BY total_price DESC
LIMIT 5;
```

---

# 33. Query를 문장으로 읽기

위 SQL을 읽어보면:

```text
orders에서

price가 30000 이상인 주문을 선택하고

user_id별로 묶어서

주문금액 합계를 계산하고

합계가 100000 이상인 사용자만 남긴 후

총액이 높은 순으로 정렬하고

5개만 보여줘.
```

이다.

---

# 34. JOIN

이제 SQL에서 매우 중요한 JOIN이다.

JOIN은:

```text
서로 다른 Table을 연결
```

할 때 사용한다.

Pandas에서 사용했던:

```python
pd.merge()
```

와 비슷한 역할이다.

---

# 35. 왜 JOIN이 필요할까?

orders Table을 보자.

| order_id | user_id | product |
|---:|---:|---|
| 101 | 1 | Keyboard |
| 102 | 1 | Mouse |
| 103 | 2 | Monitor |

여기에는:

```text
user_id
```

만 있고 사용자의 이름은 없다.

사용자 이름은 users Table에 있다.

---

# 36. users Table

| user_id | name |
|---:|---|
| 1 | Kim |
| 2 | Lee |
| 3 | Park |

우리가 원하는 결과가:

| order_id | name | product |
|---:|---|---|
| 101 | Kim | Keyboard |
| 102 | Kim | Mouse |
| 103 | Lee | Monitor |

라면 두 Table을 연결해야 한다.

그때 JOIN을 사용한다.

---

# 37. INNER JOIN

가장 기본적인 JOIN 중 하나이다.

```sql
SELECT
    users.name,
    orders.product
FROM users
INNER JOIN orders
    ON users.user_id = orders.user_id;
```

---

# 38. JOIN을 읽어보기

```sql
FROM users
```

```text
users Table을 기준으로
```

```sql
INNER JOIN orders
```

```text
orders Table을 연결
```

```sql
ON users.user_id = orders.user_id
```

```text
두 Table의 user_id가 같은 Row끼리 연결
```

한다.

---

# 39. Table Alias 사용

JOIN에서는 Table 이름이 길어질 수 있다.

그래서:

```sql
SELECT
    u.name,
    o.product
FROM users AS u
INNER JOIN orders AS o
    ON u.user_id = o.user_id;
```

처럼 Alias를 많이 사용한다.

여기서:

```text
users  → u
orders → o
```

이다.

---

# 40. JOIN 결과

예:

```text
Kim   Keyboard
Kim   Mouse
Lee   Monitor
Park  Laptop
Park  Mouse
```

처럼 사용자의 이름과 주문 정보를 같이 볼 수 있다.

---

# 41. Primary Key와 Foreign Key

JOIN을 이해할 때 이전에 배운 Key가 중요하다.

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

관계:

```text
users.user_id
        │
        │
        ↓
orders.user_id
```

이 연결을 JOIN에서 사용한다.

---

# 42. INNER JOIN의 의미

INNER JOIN은:

```text
양쪽 Table에서 JOIN 조건이 일치하는 데이터
```

를 가져온다.

예를 들어 users에는 Choi가 있지만 Choi가 주문한 적이 없다면 INNER JOIN 결과에는 Choi가 나타나지 않을 수 있다.

---

# 43. LEFT JOIN

users의 모든 사용자를 보고 싶다고 하자.

주문하지 않은 사용자도 포함해야 한다.

이때:

```sql
SELECT
    u.name,
    o.product
FROM users AS u
LEFT JOIN orders AS o
    ON u.user_id = o.user_id;
```

를 사용할 수 있다.

---

# 44. LEFT JOIN 결과

예:

```text
Kim    Keyboard
Kim    Mouse
Lee    Monitor
Park   Laptop
Park   Mouse
Choi   NULL
```

Choi는 주문이 없지만:

```text
LEFT
```

에 있는 users의 데이터는 유지된다.

---

# 45. INNER JOIN vs LEFT JOIN

```text
INNER JOIN
→ 양쪽에 일치하는 데이터만

LEFT JOIN
→ 왼쪽 Table은 모두 유지
```

이다.

---

# 46. 그림으로 이해하기

INNER JOIN:

```text
users       orders

  A  ────── A
  B  ────── B
  C

결과
A
B
```

LEFT JOIN:

```text
users       orders

  A  ────── A
  B  ────── B
  C

결과
A
B
C → NULL
```

이다.

---

# 47. Pandas merge와 비교

Pandas INNER JOIN:

```python
pd.merge(
    users,
    orders,
    on="user_id",
    how="inner"
)
```

SQL:

```sql
SELECT *
FROM users AS u
INNER JOIN orders AS o
    ON u.user_id = o.user_id;
```

---

# 48. Pandas LEFT JOIN

Pandas:

```python
pd.merge(
    users,
    orders,
    on="user_id",
    how="left"
)
```

SQL:

```sql
SELECT *
FROM users AS u
LEFT JOIN orders AS o
    ON u.user_id = o.user_id;
```

이렇게 연결해서 기억하면 쉽다.

---

# 49. JOIN + WHERE

JOIN한 결과에 조건을 적용할 수도 있다.

```sql
SELECT
    u.name,
    o.product,
    o.price
FROM users AS u
INNER JOIN orders AS o
    ON u.user_id = o.user_id
WHERE o.price >= 100000;
```

의미:

```text
사용자와 주문을 연결한 후
100000원 이상 주문만 조회
```

이다.

---

# 50. JOIN + GROUP BY

사용자 이름별 총 주문금액을 계산해보자.

```sql
SELECT
    u.name,
    SUM(o.price) AS total_price
FROM users AS u
INNER JOIN orders AS o
    ON u.user_id = o.user_id
GROUP BY
    u.user_id,
    u.name;
```

---

# 51. JOIN + GROUP BY의 흐름

```text
users
+
orders
    ↓
user_id로 JOIN
    ↓
사용자별 GROUP BY
    ↓
price SUM
```

결과:

```text
Kim
→ 80000

Lee
→ 200000

Park
→ 1530000
```

처럼 계산할 수 있다.

---

# 52. 주문하지 않은 사용자도 포함

LEFT JOIN을 사용할 수 있다.

```sql
SELECT
    u.name,
    SUM(o.price) AS total_price
FROM users AS u
LEFT JOIN orders AS o
    ON u.user_id = o.user_id
GROUP BY
    u.user_id,
    u.name;
```

주문하지 않은 Choi는:

```text
total_price = NULL
```

이 될 수 있다.

---

# 53. COALESCE

NULL 대신 다른 값을 표시하고 싶다면 `COALESCE`를 사용할 수 있다.

```sql
SELECT
    u.name,
    COALESCE(
        SUM(o.price),
        0
    ) AS total_price
FROM users AS u
LEFT JOIN orders AS o
    ON u.user_id = o.user_id
GROUP BY
    u.user_id,
    u.name;
```

주문이 없는 사용자는:

```text
0
```

으로 표시할 수 있다.

---

# 54. COALESCE 의미

```sql
COALESCE(value, 0)
```

는 개념적으로:

```text
value가 NULL이 아니면
→ value

value가 NULL이면
→ 0
```

으로 이해할 수 있다.

Pandas의:

```python
fillna(0)
```

와 비슷한 목적이다.

---

# 55. 여러 Table JOIN

실제 서비스에서는 Table이 두 개만 있는 것이 아니다.

예:

```text
users
orders
products
```

구조:

```text
users
  │
  │ user_id
  ↓
orders
  │
  │ product_id
  ↓
products
```

처럼 여러 Table이 연결될 수 있다.

---

# 56. 여러 JOIN 예

```sql
SELECT
    u.name,
    p.product_name,
    o.quantity
FROM orders AS o
INNER JOIN users AS u
    ON o.user_id = u.user_id
INNER JOIN products AS p
    ON o.product_id = p.product_id;
```

이렇게 여러 Table을 연결할 수도 있다.

---

# 57. Cartesian Product 주의

JOIN 조건을 잘못 작성하면 데이터가 예상보다 엄청나게 많아질 수 있다.

예를 들어:

```text
users 100명

orders 1000건
```

인데 연결 조건이 없으면 경우에 따라:

```text
100 × 1000
= 100000 Row
```

같은 조합이 생길 수 있다.

따라서 JOIN에서는:

```text
어떤 Key로 연결하는가?
```

를 반드시 확인한다.

---

# 58. JOIN 후 Row 수 확인

Pandas에서 merge 후:

```python
merged.shape
```

를 확인했던 것처럼 SQL에서도 JOIN 결과의 Row 수가 예상과 맞는지 확인하는 습관이 중요하다.

예:

```sql
SELECT COUNT(*)
FROM users AS u
INNER JOIN orders AS o
    ON u.user_id = o.user_id;
```

---

# 59. 중복처럼 보이는 결과

JOIN 후:

```text
Kim
Kim
Kim
```

처럼 같은 사용자가 여러 번 나올 수 있다.

이것이 반드시 오류는 아니다.

한 사용자가 주문을 3번 했다면:

```text
users 1 Row
     ↓
orders 3 Row
```

이므로 JOIN 후 사용자 정보가 3번 나타날 수 있다.

---

# 60. DISTINCT로 무조건 제거하면 안 된다

JOIN 결과가 많다고:

```sql
SELECT DISTINCT ...
```

를 바로 사용하는 것은 좋지 않다.

먼저:

```text
왜 Row가 증가했는가?
```

를 확인해야 한다.

1:N 관계 때문에 증가한 것이라면 정상일 수 있다.

---

# 61. Subquery

Subquery는 SQL 안에 또 다른 SQL Query를 넣는 것이다.

예:

```sql
SELECT *
FROM users
WHERE user_id IN (
    SELECT user_id
    FROM orders
);
```

---

# 62. Subquery 해석

안쪽 Query:

```sql
SELECT user_id
FROM orders;
```

먼저 주문한 사용자의 ID를 가져온다.

그 결과를 바깥 Query가 사용한다.

```sql
SELECT *
FROM users
WHERE user_id IN (...);
```

즉:

```text
주문한 적이 있는 사용자 조회
```

이다.

---

# 63. 평균보다 비싼 주문

Subquery를 이용하면:

```sql
SELECT *
FROM orders
WHERE price > (
    SELECT AVG(price)
    FROM orders
);
```

처럼 작성할 수 있다.

---

# 64. Subquery 실행 흐름

안쪽:

```sql
SELECT AVG(price)
FROM orders;
```

먼저 전체 평균 가격을 계산한다.

그다음 바깥:

```sql
SELECT *
FROM orders
WHERE price > 평균값;
```

이 실행된다.

결과적으로:

```text
평균보다 가격이 높은 주문
```

을 찾는다.

---

# 65. JOIN과 Subquery

같은 문제를 JOIN으로 해결할 수도 있고 Subquery로 해결할 수도 있는 경우가 있다.

어떤 방식이 더 좋은지는:

```text
Query 목적
가독성
Database Optimizer
데이터 구조
성능
```

등을 고려해야 한다.

초보 단계에서는 먼저:

```text
내가 원하는 데이터를 정확하게 가져오는가?
```

를 이해하는 것이 중요하다.

---

# 66. GROUP BY와 Subquery

사용자별 총 주문금액 중 특정 조건을 찾는 등 더 복잡한 Query에서도 Subquery를 사용할 수 있다.

하지만 Query가 너무 복잡해지면 읽기 어려워진다.

따라서 SQL을 작은 단계로 나누어 생각하는 습관이 중요하다.

---

# 67. SQL Query를 단계별로 만들기

처음부터 복잡하게 작성하지 않는다.

예를 들어:

```text
30세 이상 사용자의
총 주문금액을 계산하고
높은 순으로 보고 싶다.
```

라고 하자.

먼저 사용자:

```sql
SELECT *
FROM users
WHERE age >= 30;
```

확인한다.

그다음 JOIN:

```sql
SELECT *
FROM users AS u
INNER JOIN orders AS o
    ON u.user_id = o.user_id
WHERE u.age >= 30;
```

그다음 GROUP BY:

```sql
SELECT
    u.name,
    SUM(o.price) AS total_price
FROM users AS u
INNER JOIN orders AS o
    ON u.user_id = o.user_id
WHERE u.age >= 30
GROUP BY
    u.user_id,
    u.name;
```

마지막으로 정렬:

```sql
SELECT
    u.name,
    SUM(o.price) AS total_price
FROM users AS u
INNER JOIN orders AS o
    ON u.user_id = o.user_id
WHERE u.age >= 30
GROUP BY
    u.user_id,
    u.name
ORDER BY total_price DESC;
```

---

# 68. 복잡한 Query 읽는 순서

다음 SQL을 보자.

```sql
SELECT
    u.name,
    COUNT(o.order_id) AS order_count,
    SUM(o.price) AS total_price
FROM users AS u
INNER JOIN orders AS o
    ON u.user_id = o.user_id
WHERE u.age >= 30
GROUP BY
    u.user_id,
    u.name
HAVING COUNT(o.order_id) >= 2
ORDER BY total_price DESC;
```

처음 보면 복잡해 보인다.

하지만 단계별로 읽으면 된다.

---

# 69. FROM

```sql
FROM users AS u
```

```text
users에서 시작
```

---

# 70. JOIN

```sql
INNER JOIN orders AS o
    ON u.user_id = o.user_id
```

```text
users와 orders를
user_id로 연결
```

---

# 71. WHERE

```sql
WHERE u.age >= 30
```

```text
30세 이상 사용자만
```

---

# 72. GROUP BY

```sql
GROUP BY
    u.user_id,
    u.name
```

```text
사용자별로 그룹
```

---

# 73. HAVING

```sql
HAVING COUNT(o.order_id) >= 2
```

```text
주문을 2번 이상 한 사용자만
```

---

# 74. SELECT

```sql
SELECT
    u.name,
    COUNT(o.order_id),
    SUM(o.price)
```

```text
사용자 이름
주문 횟수
총 주문금액
```

을 결과로 만든다.

---

# 75. ORDER BY

```sql
ORDER BY total_price DESC
```

```text
총 주문금액이 높은 순
```

으로 정렬한다.

---

# 76. 전체 의미

결국 이 Query는:

```text
30세 이상 사용자 중

주문을 2번 이상 한 사람의

이름
주문 횟수
총 주문금액을 계산하여

총 주문금액이 높은 순서로 보여준다.
```

라는 의미이다.

---

# 77. SQL과 Pandas 전체 비교

| 작업 | Pandas | SQL |
|---|---|---|
| Row 수 | `len(df)` | `COUNT(*)` |
| 합계 | `sum()` | `SUM()` |
| 평균 | `mean()` | `AVG()` |
| 최솟값 | `min()` | `MIN()` |
| 최댓값 | `max()` | `MAX()` |
| 그룹화 | `groupby()` | `GROUP BY` |
| Row 조건 | Boolean Filtering | `WHERE` |
| 그룹 조건 | 그룹 결과 Filtering | `HAVING` |
| Table 연결 | `pd.merge()` | `JOIN` |
| 결측 대체 | `fillna()` | `COALESCE()` |
| 중복 값 제거 | `drop_duplicates()` | `DISTINCT` |

완전히 동일한 동작이라고 생각하기보다 개념적인 대응 관계로 이해한다.

---

# 78. 데이터 분석에서 SQL

실제 데이터 분석에서는:

```text
Database
    ↓
SQL
    ↓
필요한 데이터 추출
    ↓
Pandas DataFrame
    ↓
EDA
    ↓
전처리
    ↓
통계 분석
    ↓
Machine Learning
```

처럼 사용할 수 있다.

SQL에서 모든 데이터를 가져오는 것이 아니라 필요한 데이터만 먼저 추출할 수도 있다.

---

# 79. 웹 서비스에서 SQL

웹 서비스에서는:

```text
Frontend
    ↓
HTTP Request
    ↓
Backend
    ↓
SQL Query
    ↓
Database
```

흐름으로 사용할 수 있다.

예:

```text
GET /users/1/orders
```

Backend가:

```sql
SELECT *
FROM orders
WHERE user_id = 1;
```

같은 Query를 실행할 수 있다.

---

# 80. AI 서비스에서 SQL

AI 서비스에서도:

```text
사용자
   ↓
Backend
   ↓
Database
   ↓
사용자 데이터 조회
   ↓
AI Model
   ↓
Prediction
   ↓
Database 저장
```

같은 구조를 사용할 수 있다.

예측 기록을 다시 조회하면:

```text
Database
→ SQL
→ Backend
→ Frontend
```

로 전달할 수 있다.

---

# 81. SQL 분석에서 주의할 점

SQL 결과가 나왔다고 바로 믿지 않는다.

다음을 확인한다.

```text
JOIN Key가 맞는가?

JOIN 후 Row가 예상보다 증가하지 않았는가?

NULL은 어떻게 처리되었는가?

중복 데이터가 있는가?

WHERE 조건이 정확한가?

GROUP BY 기준이 맞는가?

단위가 같은가?
```

---

# 82. JOIN이 특히 위험한 이유

잘못된 JOIN은 Query 오류가 발생하지 않고도 잘못된 결과를 만들 수 있다.

예:

```text
원본 1,000 Row
       ↓
잘못된 JOIN
       ↓
50,000 Row
```

가 되었는데 SQL 자체는 정상 실행될 수도 있다.

따라서:

```text
실행 성공
=
분석 결과가 정확함
```

은 아니다.

---

# 83. GROUP BY도 검증한다

예:

```sql
SELECT
    user_id,
    COUNT(*)
FROM orders
GROUP BY user_id;
```

결과가 나왔다면 몇 개 사용자를 그룹화했는지 확인할 수 있다.

```sql
SELECT COUNT(
    DISTINCT user_id
)
FROM orders;
```

같은 Query와 비교하면서 검증할 수 있다.

---

# 84. SQL 분석 습관

복잡한 Query를 작성할 때는:

```text
1. 원본 Table 확인

2. WHERE만 실행

3. JOIN 실행

4. Row 수 확인

5. GROUP BY 추가

6. 집계값 확인

7. HAVING 추가

8. ORDER BY 추가

9. 최종 결과 검증
```

순서로 진행하면 오류를 찾기 쉽다.

---

# 85. 핵심 Query 구조

이번 장에서 가장 중요한 형태이다.

```sql
SELECT
    group_column,
    COUNT(*) AS count,
    SUM(value_column) AS total
FROM table
WHERE condition
GROUP BY group_column
HAVING COUNT(*) >= 2
ORDER BY total DESC;
```

그리고 여러 Table이라면:

```sql
SELECT
    a.column,
    SUM(b.value)
FROM table_a AS a
INNER JOIN table_b AS b
    ON a.id = b.a_id
GROUP BY a.column;
```

형태가 자주 등장한다.

---

# 86. 핵심 용어 정리

```text
COUNT
→ 개수

SUM
→ 합계

AVG
→ 평균

MIN
→ 최솟값

MAX
→ 최댓값

GROUP BY
→ 같은 값끼리 그룹화

WHERE
→ Row에 조건

HAVING
→ 그룹 결과에 조건

JOIN
→ Table 연결

INNER JOIN
→ 양쪽에 일치하는 데이터

LEFT JOIN
→ 왼쪽 Table을 모두 유지

AS
→ 별명

COALESCE
→ NULL일 때 대체값 사용

Subquery
→ Query 안의 Query
```

---

# 87. Pandas와 연결해서 기억하기

지금까지 배운 Pandas가:

```python
result = (
    df.groupby("user_id")["price"]
      .sum()
      .sort_values(
          ascending=False
      )
)
```

였다면 SQL에서는:

```sql
SELECT
    user_id,
    SUM(price) AS total_price
FROM orders
GROUP BY user_id
ORDER BY total_price DESC;
```

처럼 표현할 수 있다.

Pandas와 SQL은 문법은 다르지만:

```text
데이터를 선택하고

조건을 적용하고

그룹을 만들고

집계하고

정렬한다.
```

는 사고방식은 연결되어 있다.

---

# 88. 배운 점

이번에는 단순한 SELECT에서 한 단계 더 나아가:

```text
COUNT
SUM
AVG
MIN
MAX
```

를 이용한 집계를 배웠다.

그리고:

```text
GROUP BY
```

를 이용하면:

```text
사용자별
상품별
지역별
날짜별
```

등으로 데이터를 묶어서 분석할 수 있다.

`WHERE`와 `HAVING`의 차이는:

```text
WHERE
→ 그룹화 전 Row 조건

HAVING
→ 그룹화 후 그룹 조건
```

이다.

또한 관계형 Database의 핵심 중 하나인:

```text
JOIN
```

을 이용하면 서로 다른 Table을 연결할 수 있다.

특히:

```text
Primary Key
↕
Foreign Key
```

관계를 이용하여:

```text
users
+
orders
```

같은 데이터를 연결한다.

Pandas와 연결하면:

```text
groupby()
↔
GROUP BY

merge()
↔
JOIN
```

으로 생각할 수 있다.

SQL에서 중요한 것은 복잡한 Query를 한 번에 외우는 것이 아니다.

```text
어느 Table에서 시작하는가?

어떤 Row가 필요한가?

어떤 Table과 연결하는가?

어떤 기준으로 그룹화하는가?

무엇을 계산하는가?

어떤 조건으로 결과를 제한하는가?
```

를 단계별로 생각하는 것이 중요하다.

결국:

```text
FROM
  ↓
JOIN
  ↓
WHERE
  ↓
GROUP BY
  ↓
HAVING
  ↓
SELECT
  ↓
ORDER BY
```

처럼 데이터가 어떻게 변하는지를 머릿속으로 따라가면 복잡한 SQL도 읽을 수 있게 된다.