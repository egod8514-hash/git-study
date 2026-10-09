# Python 자료구조

## 1. 자료구조란?

지금까지는 하나의 변수에 하나의 값을 저장했다.

```python
age = 30
```

하지만 실제 프로그램에서는 여러 개의 데이터를 함께 관리해야 하는 경우가 많다.

예를 들어 환자 5명의 나이를 저장한다고 생각해보자.

```python
age1 = 30
age2 = 40
age3 = 50
age4 = 60
age5 = 70
```

이렇게 각각 변수를 만드는 것보다 하나로 묶어서 관리하는 것이 편리하다.

```python
ages = [30, 40, 50, 60, 70]
```

Python에서는 여러 데이터를 관리하기 위해 다양한 자료구조를 제공한다.

대표적으로:

```text
List
Tuple
Dictionary
Set
```

이 있다.

---

# 2. List

List는 여러 개의 데이터를 순서대로 저장할 수 있는 자료구조이다.

대괄호 `[]`를 사용한다.

```python
ages = [30, 40, 50, 60]
```

문자열도 저장할 수 있다.

```python
models = [
    "LogisticRegression",
    "RandomForest",
    "XGBoost"
]
```

---

# 3. List에는 서로 다른 자료형도 저장할 수 있다

```python
patient = [
    "Kim",
    30,
    175.5,
    True
]
```

각각:

```text
"Kim" → str
30 → int
175.5 → float
True → bool
```

이다.

Python List에는 서로 다른 자료형의 값을 함께 저장할 수도 있다.

---

# 4. 인덱스

List의 각 데이터에는 위치 번호가 있다.

이를 Index라고 한다.

```python
models = [
    "RandomForest",
    "XGBoost",
    "LightGBM"
]
```

위치를 보면:

```text
Index       값

0        RandomForest
1        XGBoost
2        LightGBM
```

중요한 점은 Python의 Index가 **0부터 시작한다**는 것이다.

---

# 5. List 데이터 가져오기

첫 번째 데이터:

```python
print(models[0])
```

결과:

```text
RandomForest
```

두 번째:

```python
print(models[1])
```

결과:

```text
XGBoost
```

세 번째:

```python
print(models[2])
```

결과:

```text
LightGBM
```

---

# 6. 음수 인덱스

뒤에서부터 데이터를 가져올 수도 있다.

```python
models = [
    "RandomForest",
    "XGBoost",
    "LightGBM"
]
```

마지막 데이터:

```python
models[-1]
```

결과:

```text
LightGBM
```

뒤에서 두 번째:

```python
models[-2]
```

결과:

```text
XGBoost
```

---

# 7. List 값 수정

List는 값을 변경할 수 있다.

```python
models = [
    "RandomForest",
    "XGBoost",
    "LightGBM"
]

models[0] = "LogisticRegression"

print(models)
```

결과:

```text
['LogisticRegression', 'XGBoost', 'LightGBM']
```

---

# 8. append()

List 마지막에 데이터를 추가할 때 사용한다.

```python
models = [
    "RandomForest",
    "XGBoost"
]

models.append("LightGBM")

print(models)
```

결과:

```text
['RandomForest', 'XGBoost', 'LightGBM']
```

---

# 9. insert()

특정 위치에 값을 추가할 수도 있다.

```python
models.insert(1, "CatBoost")
```

1번 Index 위치에 `"CatBoost"`를 추가한다.

---

# 10. remove()

특정 값을 삭제할 수 있다.

```python
models.remove("XGBoost")
```

List에서 `"XGBoost"` 값을 찾아 삭제한다.

---

# 11. pop()

특정 Index의 값을 꺼내면서 삭제할 수 있다.

```python
models = [
    "RandomForest",
    "XGBoost",
    "LightGBM"
]

model = models.pop(1)

print(model)
print(models)
```

결과:

```text
XGBoost
['RandomForest', 'LightGBM']
```

---

# 12. len()

데이터 개수를 확인할 수 있다.

```python
models = [
    "RandomForest",
    "XGBoost",
    "LightGBM"
]

print(len(models))
```

결과:

```text
3
```

`len()`은 이후 데이터 분석에서도 매우 자주 사용한다.

예:

```python
len(train)
len(y)
len(pred)
```

---

# 13. List와 for문

List와 반복문은 매우 자주 함께 사용된다.

```python
models = [
    "RandomForest",
    "XGBoost",
    "LightGBM"
]

for model in models:
    print(model)
```

결과:

```text
RandomForest
XGBoost
LightGBM
```

즉 List 안의 값을 하나씩 꺼내 반복할 수 있다.

---

# 14. 슬라이싱

List의 일정 구간을 가져오는 것을 Slicing이라고 한다.

```python
numbers = [10, 20, 30, 40, 50]
```

다음과 같이 사용할 수 있다.

```python
numbers[1:4]
```

결과:

```text
[20, 30, 40]
```

의미:

```text
1번 Index부터
4번 Index 직전까지
```

이다.

따라서:

```text
start 포함
stop 제외
```

이다.

---

# 15. 다양한 슬라이싱

처음부터 3번 직전까지:

```python
numbers[:3]
```

결과:

```text
[10, 20, 30]
```

2번부터 끝까지:

```python
numbers[2:]
```

결과:

```text
[30, 40, 50]
```

전체:

```python
numbers[:]
```

---

# 16. Tuple

Tuple도 여러 데이터를 순서대로 저장한다.

소괄호 `()`를 사용한다.

```python
patient = ("Kim", 30, 175)
```

List와 비슷하지만 중요한 차이가 있다.

Tuple은 만들어진 후 값을 변경할 수 없다.

---

# 17. List와 Tuple 차이

List:

```python
data = [10, 20, 30]

data[0] = 100
```

변경 가능하다.

Tuple:

```python
data = (10, 20, 30)
```

다음처럼 변경하려고 하면:

```python
data[0] = 100
```

오류가 발생한다.

따라서 간단하게:

```text
List
→ 변경 가능

Tuple
→ 변경 불가능
```

으로 이해할 수 있다.

---

# 18. Tuple Unpacking

Tuple의 값을 여러 변수에 한 번에 저장할 수 있다.

```python
patient = ("Kim", 30, 175)

name, age, height = patient
```

그러면:

```text
name = "Kim"
age = 30
height = 175
```

이 된다.

---

# 19. 머신러닝에서 본 Tuple

우리가 사용했던 코드:

```python
for fold, (tr_idx, va_idx) in enumerate(
    skf.split(X, y), 1
):
```

여기서:

```python
(tr_idx, va_idx)
```

형태를 볼 수 있다.

각 Fold마다 학습 데이터 Index와 검증 데이터 Index를 받아 각각:

```text
tr_idx
va_idx
```

에 나누어 저장하는 것이다.

즉 Python의 Tuple과 Unpacking 개념이 머신러닝 코드에서도 사용된다.

---

# 20. Dictionary

Dictionary는 데이터를:

```text
Key : Value
```

형태로 저장한다.

중괄호 `{}`를 사용한다.

```python
patient = {
    "name": "Kim",
    "age": 30,
    "height": 175
}
```

구조:

```text
Key        Value

name   →   Kim
age    →   30
height →   175
```

---

# 21. Dictionary 값 가져오기

List에서는 Index를 사용했다.

```python
models[0]
```

Dictionary에서는 Key를 사용한다.

```python
patient["name"]
```

결과:

```text
Kim
```

나이:

```python
patient["age"]
```

결과:

```text
30
```

---

# 22. Dictionary 값 수정

```python
patient["age"] = 31
```

이제:

```python
print(patient["age"])
```

결과:

```text
31
```

---

# 23. Dictionary에 새로운 데이터 추가

```python
patient["weight"] = 70
```

결과:

```python
{
    "name": "Kim",
    "age": 31,
    "height": 175,
    "weight": 70
}
```

---

# 24. keys()

Dictionary의 Key를 확인할 수 있다.

```python
print(patient.keys())
```

예:

```text
name
age
height
weight
```

---

# 25. values()

Value를 확인할 수 있다.

```python
print(patient.values())
```

---

# 26. items()

Key와 Value를 함께 가져올 수 있다.

```python
for key, value in patient.items():
    print(key, value)
```

결과:

```text
name Kim
age 31
height 175
weight 70
```

---

# 27. 머신러닝에서 Dictionary

머신러닝에서 여러 모델의 결과를 관리할 때 Dictionary를 사용할 수 있다.

예:

```python
scores = {
    "RandomForest": 0.85,
    "XGBoost": 0.89,
    "LightGBM": 0.88
}
```

특정 모델:

```python
scores["XGBoost"]
```

결과:

```text
0.89
```

---

# 28. 실제 프로젝트에서 사용했던 형태

여러 모델의 이름을 List로 만들 수 있다.

```python
model_names = [
    "LightGBM",
    "XGBoost",
    "CatBoost"
]
```

그리고 각 모델의 예측값을 저장할 Dictionary를 만들 수 있다.

```python
oof = {
    m: np.zeros(len(y))
    for m in model_names
}
```

처음 보면 복잡하지만 구조를 나누면 이해할 수 있다.

먼저:

```python
for m in model_names
```

이므로:

```text
m = LightGBM
m = XGBoost
m = CatBoost
```

순서로 반복한다.

그리고 각각:

```python
np.zeros(len(y))
```

를 Value로 만든다.

결과적으로 개념상:

```python
oof = {
    "LightGBM": 예측값을 저장할 공간,
    "XGBoost": 예측값을 저장할 공간,
    "CatBoost": 예측값을 저장할 공간
}
```

이 된다.

---

# 29. Dictionary Comprehension

앞에서 본:

```python
oof = {
    m: np.zeros(len(y))
    for m in model_names
}
```

같은 방식을 Dictionary Comprehension이라고 한다.

간단한 예:

```python
numbers = [1, 2, 3]

squares = {
    n: n ** 2
    for n in numbers
}
```

결과:

```python
{
    1: 1,
    2: 4,
    3: 9
}
```

반복문을 이용하여 Dictionary를 간단하게 생성한 것이다.

---

# 30. List Comprehension

List도 비슷하게 만들 수 있다.

일반적인 반복문:

```python
numbers = []

for i in range(5):
    numbers.append(i)
```

List Comprehension:

```python
numbers = [
    i
    for i in range(5)
]
```

결과:

```text
[0, 1, 2, 3, 4]
```

---

# 31. 조건이 있는 List Comprehension

짝수만 저장한다고 생각해보자.

```python
numbers = [
    i
    for i in range(10)
    if i % 2 == 0
]
```

결과:

```text
[0, 2, 4, 6, 8]
```

여기에는:

```text
List
+
for
+
if
```

가 결합되어 있다.

---

# 32. Set

Set은 중복되지 않는 값을 관리할 때 사용할 수 있다.

```python
numbers = {1, 2, 3, 3, 3}
```

출력하면 중복이 제거된다.

```text
{1, 2, 3}
```

---

# 33. Set의 특징

Set의 대표적인 특징:

```text
중복 허용하지 않음
순서를 기준으로 Index 접근하지 않음
집합 연산 가능
```

List처럼:

```python
data[0]
```

방식으로 사용하는 자료구조가 아니다.

---

# 34. 중복 제거

List에 중복된 데이터가 있다고 가정한다.

```python
labels = [0, 0, 1, 1, 1, 2]
```

Set으로 변환하면:

```python
unique_labels = set(labels)

print(unique_labels)
```

결과:

```text
{0, 1, 2}
```

고유한 값이 무엇인지 확인할 때 사용할 수 있다.

---

# 35. 자료구조 안에 자료구조

자료구조 안에 다른 자료구조를 넣을 수도 있다.

예:

```python
patients = [
    {
        "name": "Kim",
        "age": 30
    },
    {
        "name": "Lee",
        "age": 40
    }
]
```

여기서는:

```text
List
 ├── Dictionary
 └── Dictionary
```

구조이다.

---

# 36. 첫 번째 환자

```python
patients[0]
```

결과:

```python
{
    "name": "Kim",
    "age": 30
}
```

첫 번째 환자의 이름:

```python
patients[0]["name"]
```

결과:

```text
Kim
```

복잡한 데이터도 이런 방식으로 단계적으로 접근할 수 있다.

---

# 37. Pandas와 대괄호

Pandas에서도 대괄호를 많이 사용했다.

예:

```python
train["Age"]
```

이것은 `train` DataFrame에서 `"Age"`라는 Column을 선택한다.

여러 Column:

```python
train[
    ["Age", "BMI", "Glucose"]
]
```

Python의 List와 대괄호 사용법을 이해하면 Pandas 코드도 읽기 쉬워진다.

---

# 38. Pandas 조건 선택

다음과 같은 코드도 사용한다.

```python
train[
    train["Age"] >= 30
]
```

구조를 나누면:

```python
train["Age"]
```

Age Column 선택

```python
train["Age"] >= 30
```

30 이상인지 비교

그리고:

```python
train[조건]
```

조건을 만족하는 행 선택

이다.

Python의:

```text
대괄호
비교 연산자
조건
```

개념이 Pandas에서 확장된 것이다.

---

# 39. NumPy의 인덱싱

머신러닝에서는 NumPy 배열도 많이 사용한다.

예:

```python
X[0]
```

첫 번째 데이터를 가져온다.

다음과 같은 형태도 볼 수 있다.

```python
X[:, 0]
```

여기서:

```text
:
→ 모든 행

0
→ 첫 번째 열
```

이라는 의미이다.

즉:

```python
X[:, 0]
```

은:

```text
모든 행의 첫 번째 열
```

을 선택한다.

---

# 40. 머신러닝의 train_idx

Cross Validation에서 다음과 같은 코드를 볼 수 있다.

```python
X_train = X.iloc[tr_idx]
X_valid = X.iloc[va_idx]
```

여기서:

```text
tr_idx
→ 학습에 사용할 행 번호

va_idx
→ 검증에 사용할 행 번호
```

이다.

결국 머신러닝의 데이터 분리도 Index를 이용하여 필요한 데이터를 선택하는 작업이라고 이해할 수 있다.

---

# 41. 자료구조 비교

| 자료구조 | 기호 | 특징 |
|---|---|---|
| List | `[]` | 순서 있음, 수정 가능 |
| Tuple | `()` | 순서 있음, 수정 불가 |
| Dictionary | `{key: value}` | Key와 Value |
| Set | `{}` | 중복 제거, 집합 |

각 자료구조는 목적에 따라 선택한다.

---

# 42. 언제 무엇을 사용할까?

여러 모델 이름을 순서대로 저장:

```python
models = [
    "RF",
    "XGB",
    "LGBM"
]
```

→ List

변경하면 안 되는 고정된 값 묶음:

```python
shape = (224, 224)
```

→ Tuple

모델별 점수 저장:

```python
scores = {
    "RF": 0.85,
    "XGB": 0.89
}
```

→ Dictionary

중복되지 않는 Label 확인:

```python
labels = {0, 1}
```

→ Set

---

# 43. AI 프로젝트와 자료구조

머신러닝이나 딥러닝 프로젝트에서는 자료구조가 계속 등장한다.

예:

```python
model_names = [
    "LightGBM",
    "XGBoost",
    "CatBoost"
]
```

List

```python
scores = {
    "LightGBM": 0.85,
    "XGBoost": 0.87
}
```

Dictionary

```python
image_size = (224, 224)
```

Tuple

```python
unique_labels = {0, 1}
```

Set

즉 자료구조를 이해하면 AI 코드에서 데이터를 어떤 형태로 관리하고 있는지 파악하기 쉬워진다.

---

# 44. 복잡한 코드를 읽는 방법

다음 코드가 있다고 하자.

```python
oof = {
    m: np.zeros(len(y))
    for m in model_names
}
```

한 번에 이해하려 하지 않고 다음 순서로 나눈다.

```text
1. {}가 있다.
→ Dictionary

2. for가 있다.
→ 반복

3. model_names를 반복한다.

4. m이 Dictionary의 Key가 된다.

5. np.zeros(len(y))가 Value가 된다.
```

이런 식으로 Python 기본 문법 단위로 분리해서 보면 복잡한 머신러닝 코드도 읽을 수 있다.

---

# 45. 배운 점

Python에서는 여러 개의 데이터를 관리하기 위해 다양한 자료구조를 사용한다.

```text
List
Tuple
Dictionary
Set
```

List는 순서가 있고 수정할 수 있으며 Index를 이용하여 데이터를 선택한다.

Tuple은 List와 비슷하지만 값을 변경할 수 없다.

Dictionary는:

```text
Key : Value
```

형태로 데이터를 관리한다.

Set은 중복되지 않는 값을 관리할 때 사용할 수 있다.

또한 Index와 Slicing은 이후:

```text
NumPy
Pandas
머신러닝
딥러닝
```

에서도 계속 사용되는 중요한 개념이다.

특히 복잡해 보였던:

```python
model_names = [
    "LightGBM",
    "XGBoost",
    "CatBoost"
]

oof = {
    m: np.zeros(len(y))
    for m in model_names
}
```

같은 머신러닝 코드도 결국:

```text
List
+
Dictionary
+
for 반복문
+
함수 호출
```

을 조합한 Python 코드라는 것을 이해했다.

따라서 복잡한 코드를 만났을 때 전체를 한 번에 이해하려고 하기보다:

```text
이건 List인가?
Dictionary인가?
반복문인가?
함수인가?
```

처럼 Python의 기본 구성요소로 나누어 읽는 것이 중요하다.