# 데이터 전처리

## 1. 데이터 전처리란?

데이터 전처리(Data Preprocessing)는 원본 데이터를 분석이나 머신러닝 모델이 사용할 수 있는 형태로 정리하고 변환하는 과정이다.

전체 흐름을 간단하게 보면:

```text
원본 데이터
    ↓
데이터 확인
    ↓
잘못된 값 처리
    ↓
결측치 처리
    ↓
이상치 처리
    ↓
자료형 변환
    ↓
범주형 데이터 변환
    ↓
Train / Validation 분리
    ↓
Scaling
    ↓
머신러닝 모델
```

모든 데이터에 이 과정을 똑같이 적용하는 것은 아니다.

데이터와 모델에 따라 필요한 전처리를 선택한다.

---

# 2. 왜 전처리가 필요한가?

실제 데이터는 처음부터 머신러닝에 적합한 형태로 존재하지 않을 수 있다.

예:

```text
Age
30
40
??
50
```

또는:

```text
Gender
Male
Female
Male
Female
```

처럼 문자열이 포함될 수 있다.

또:

```text
BMI
23.5
NaN
27.1
200
```

처럼 결측값이나 극단값도 존재할 수 있다.

따라서 데이터를 확인하고 모델이 사용할 수 있는 형태로 정리해야 한다.

---

# 3. 원본 데이터 보존

전처리를 시작하기 전에 원본을 보존하는 습관이 좋다.

```python
df_raw = df.copy()
```

이후:

```text
df_raw
→ 원본 데이터

df
→ 전처리 작업용 데이터
```

처럼 사용할 수 있다.

전처리를 잘못했을 때 원본에서 다시 시작할 수 있다.

---

# 4. 자료형 확인

먼저:

```python
df.dtypes
```

또는:

```python
df.info()
```

를 사용한다.

예:

```text
Age          object
BMI         float64
Gender       object
Outcome       int64
```

Age가 원래 숫자여야 하는데 `object`라면 데이터 안에 문자열이 포함되어 있을 가능성을 확인할 수 있다.

---

# 5. 숫자형으로 변환

다음과 같은 데이터가 있다고 하자.

```text
Age
30
40
??
50
```

다음처럼 숫자로 변환할 수 있다.

```python
df["Age"] = pd.to_numeric(
    df["Age"],
    errors="coerce"
)
```

결과:

```text
30
40
NaN
50
```

숫자로 변환할 수 없는 값이 `NaN`으로 바뀐다.

---

# 6. errors="coerce"

```python
pd.to_numeric(
    df["Age"],
    errors="coerce"
)
```

에서:

```text
errors="coerce"
```

는 변환할 수 없는 값을 결측값으로 만든다는 의미이다.

중요한 것은:

```text
??
→ NaN
```

으로 바뀌었다고 해서 문제가 완전히 해결된 것이 아니다.

이후 새로 생긴 결측값을 어떻게 처리할지 결정해야 한다.

---

# 7. 날짜형 변환

날짜가 문자열로 저장될 수도 있다.

```text
2026-09-01
2026-09-02
```

변환:

```python
df["date"] = pd.to_datetime(
    df["date"],
    errors="coerce"
)
```

이후:

```python
df["year"] = df["date"].dt.year
df["month"] = df["date"].dt.month
df["day"] = df["date"].dt.day
```

처럼 날짜 정보를 활용할 수 있다.

---

# 8. 범주형 데이터

다음과 같은 변수는 범주형 데이터이다.

```text
Gender

Male
Female
```

또는:

```text
Job

Office
Student
Self-employed
Unemployed
```

많은 머신러닝 모델에서는 이러한 문자열을 그대로 사용할 수 없기 때문에 숫자로 변환해야 한다.

이 과정을 Encoding이라고 한다.

---

# 9. Encoding

Encoding은 범주형 데이터를 숫자로 변환하는 과정이다.

대표적인 방법:

```text
Label Encoding

One-Hot Encoding
```

이 있다.

---

# 10. Label Encoding

예:

```text
Male   → 0
Female → 1
```

처럼 하나의 숫자로 변환할 수 있다.

Scikit-learn에서는:

```python
from sklearn.preprocessing import LabelEncoder

encoder = LabelEncoder()

df["Gender_encoded"] = (
    encoder.fit_transform(
        df["Gender"]
    )
)
```

처럼 사용할 수 있다.

---

# 11. Label Encoding 주의점

다음과 같은 범주가 있다고 하자.

```text
사무직
자영업
학생
무직
```

이를:

```text
사무직 → 0
자영업 → 1
학생   → 2
무직   → 3
```

으로 바꾸면 숫자 사이에 순서가 있는 것처럼 보일 수 있다.

하지만 실제로:

```text
무직 > 학생 > 자영업
```

이라는 의미는 아니다.

따라서 단순 명목형 변수에서는 One-Hot Encoding을 많이 사용한다.

---

# 12. One-Hot Encoding

예:

```text
Gender

Male
Female
```

를 다음처럼 변환할 수 있다.

```text
Gender_Male    Gender_Female

1              0
0              1
```

각 범주를 별도의 Column으로 만든다.

---

# 13. Pandas get_dummies()

간단하게:

```python
encoded = pd.get_dummies(
    df,
    columns=["Gender"]
)
```

처럼 사용할 수 있다.

결과:

```text
Age   Gender_Female   Gender_Male
30         0               1
40         1               0
```

---

# 14. OneHotEncoder

Scikit-learn에서는:

```python
from sklearn.preprocessing import OneHotEncoder

encoder = OneHotEncoder(
    handle_unknown="ignore"
)
```

를 사용할 수 있다.

`handle_unknown="ignore"`는 Train에는 없었지만 Validation이나 Test에 새로운 범주가 등장했을 때 오류가 발생하는 문제를 줄이는 데 도움이 된다.

---

# 15. 순서형 데이터

범주형 데이터 중에는 실제 순서가 있는 데이터도 있다.

예:

```text
Low
Medium
High
```

이 경우:

```text
Low    → 0
Medium → 1
High   → 2
```

처럼 순서를 반영할 수 있다.

따라서:

```text
명목형 변수
→ 범주 사이에 순서 없음

순서형 변수
→ 범주 사이에 순서 있음
```

을 구분해야 한다.

---

# 16. Feature와 Target 분리

머신러닝에서는 보통:

```text
X
→ 입력 Feature

y
→ 정답 Target
```

으로 나눈다.

예:

```python
X = df.drop(
    columns=["Outcome"]
)

y = df["Outcome"]
```

여기서 `Outcome`을 예측한다고 가정한다.

---

# 17. 왜 Target을 X에서 제거할까?

만약:

```python
X = df
```

로 사용하면 X 안에 정답인 `Outcome`까지 들어갈 수 있다.

그러면 모델이 학습 과정에서 정답을 직접 보게 된다.

이것은 심각한 Data Leakage가 된다.

따라서:

```python
X = df.drop(
    columns=["Outcome"]
)

y = df["Outcome"]
```

처럼 분리한다.

---

# 18. Train과 Validation

모델을 만든 뒤 같은 데이터로 평가하면 모델이 새로운 데이터에서도 잘 작동하는지 판단하기 어렵다.

따라서 데이터를 나눈다.

```text
전체 학습 데이터
       ↓
 ┌──────────────┐
 │              │
Train       Validation
```

Train:

```text
모델 학습
```

Validation:

```text
학습하지 않은 데이터로 성능 확인
```

에 사용한다.

---

# 19. train_test_split

```python
from sklearn.model_selection import train_test_split

X_train, X_valid, y_train, y_valid = (
    train_test_split(
        X,
        y,
        test_size=0.2,
        random_state=42
    )
)
```

---

# 20. test_size

```python
test_size=0.2
```

는 전체 데이터 중 20%를 Validation 용도로 사용한다는 의미이다.

예:

```text
전체 1000개

Train
→ 약 800개

Validation
→ 약 200개
```

이다.

---

# 21. random_state

```python
random_state=42
```

는 무작위 분할 결과를 재현하기 위해 사용한다.

같은 데이터와 같은 설정이라면 가능한 한 같은 분할을 다시 만들 수 있다.

---

# 22. stratify

분류 문제에서는 Target 비율을 유지하며 나누는 것이 중요할 수 있다.

```python
X_train, X_valid, y_train, y_valid = (
    train_test_split(
        X,
        y,
        test_size=0.2,
        random_state=42,
        stratify=y
    )
)
```

`stratify=y`를 사용하면 Train과 Validation에서 Target 클래스 비율이 비슷하게 유지되도록 분할한다.

---

# 23. 클래스 불균형 데이터에서 stratify

예:

```text
전체

0 → 90%
1 → 10%
```

가능하면:

```text
Train

0 → 약 90%
1 → 약 10%
```

```text
Validation

0 → 약 90%
1 → 약 10%
```

가 되도록 나누는 것이다.

의료 데이터처럼 소수 클래스가 중요한 문제에서는 특히 유용하다.

---

# 24. 데이터 분할 후 Shape 확인

```python
print(
    "X_train:",
    X_train.shape
)

print(
    "X_valid:",
    X_valid.shape
)

print(
    "y_train:",
    y_train.shape
)

print(
    "y_valid:",
    y_valid.shape
)
```

데이터가 의도한 대로 나뉘었는지 확인한다.

---

# 25. Scaling

Feature마다 숫자의 범위가 크게 다를 수 있다.

예:

```text
Age
20 ~ 80

Income
1,000,000 ~ 100,000,000
```

이처럼 범위 차이가 큰 Feature를 일정한 기준으로 조정하는 과정을 Scaling이라고 한다.

---

# 26. StandardScaler

대표적인 Scaling 방법이다.

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
```

기본적으로 평균과 표준편차를 이용한다.

개념:

```text
평균을 중심으로 맞추고
표준편차를 기준으로 크기를 조정
```

한다.

---

# 27. StandardScaler 사용

Train에서:

```python
X_train_scaled = (
    scaler.fit_transform(
        X_train
    )
)
```

Validation에서는:

```python
X_valid_scaled = (
    scaler.transform(
        X_valid
    )
)
```

를 사용한다.

---

# 28. 왜 Validation에서는 fit_transform을 하지 않을까?

잘못된 예:

```python
X_valid_scaled = (
    scaler.fit_transform(
        X_valid
    )
)
```

이렇게 하면 Validation 데이터의 평균과 표준편차를 새롭게 학습한다.

Validation은 모델을 평가하기 위한 데이터이므로 전처리 기준도 Train에서 만들어야 한다.

따라서:

```text
Train
→ fit_transform

Validation
→ transform

Test
→ transform
```

이라고 기억하면 된다.

---

# 29. fit의 의미

```python
scaler.fit(X_train)
```

은 Train 데이터에서:

```text
평균
표준편차
```

를 계산하여 기억하는 과정이다.

---

# 30. transform의 의미

```python
scaler.transform(X_valid)
```

은 Train에서 계산한 기준을 Validation 데이터에 적용하는 과정이다.

즉:

```text
fit
→ 기준을 학습

transform
→ 학습한 기준 적용
```

이다.

---

# 31. fit_transform

```python
scaler.fit_transform(X_train)
```

은:

```text
fit
+
transform
```

을 한 번에 실행한다.

따라서 Train 데이터에서 많이 사용한다.

---

# 32. MinMaxScaler

또 다른 Scaling 방법이다.

```python
from sklearn.preprocessing import MinMaxScaler

scaler = MinMaxScaler()
```

일반적으로 데이터를 특정 범위로 조정한다.

기본적으로:

```text
0 ~ 1
```

범위로 변환한다.

---

# 33. MinMaxScaler 개념

공식은 개념적으로:

```text
현재 값 - 최솟값
----------------
최댓값 - 최솟값
```

을 이용한다.

가장 작은 값은 0 근처, 가장 큰 값은 1 근처가 된다.

---

# 34. MinMaxScaler와 이상치

MinMaxScaler는 최솟값과 최댓값을 사용한다.

따라서 극단적인 이상치가 존재하면 대부분의 데이터가 좁은 범위에 몰릴 수 있다.

Scaling 전에 이상치와 데이터 분포를 확인하는 이유 중 하나이다.

---

# 35. RobustScaler

이상치가 많은 경우 고려할 수 있는 Scaling 방법이다.

```python
from sklearn.preprocessing import RobustScaler

scaler = RobustScaler()
```

RobustScaler는 중앙값과 IQR을 사용한다.

따라서 극단값의 영향을 상대적으로 덜 받을 수 있다.

---

# 36. Scaler 비교

```text
StandardScaler
→ 평균 / 표준편차

MinMaxScaler
→ 최솟값 / 최댓값

RobustScaler
→ 중앙값 / IQR
```

이다.

항상 하나가 가장 좋은 것은 아니다.

데이터와 모델에 따라 선택한다.

---

# 37. 모든 모델에 Scaling이 필요한가?

아니다.

예를 들어 다음 모델들은 Scaling의 영향을 크게 받을 수 있다.

```text
Logistic Regression
Linear Regression
Ridge
KNN
SVM
Neural Network
```

반면 Tree 기반 모델:

```text
Decision Tree
Random Forest
XGBoost
LightGBM
CatBoost
```

은 일반적으로 Feature 크기 자체에 덜 민감하다.

따라서 모델에 따라 필요한 전처리가 다르다.

---

# 38. 문자열은 Scaling하지 않는다

다음처럼:

```text
Gender = Male
```

문자열을 `StandardScaler`에 바로 넣을 수는 없다.

먼저 범주형 데이터를 적절히 Encoding해야 한다.

즉:

```text
범주형 데이터
    ↓
Encoding

수치형 데이터
    ↓
필요하면 Scaling
```

이다.

---

# 39. ID Column

다음과 같은 Column이 있다고 하자.

```text
Patient_ID

P001
P002
P003
```

단순히 환자를 구분하기 위한 식별자라면 모델의 Feature에서 제외하는 것을 고려한다.

예:

```python
X = df.drop(
    columns=[
        "Outcome",
        "Patient_ID"
    ]
)
```

하지만 ID처럼 보여도 의미가 있는 경우가 있을 수 있으므로 데이터 명세를 먼저 확인한다.

---

# 40. Data Leakage

Data Leakage는 모델이 실제 예측 시점에는 알 수 없는 정보를 학습 과정에서 사용하는 문제이다.

예를 들어:

```text
수술 실패 여부 예측
```

을 하는데 Feature에:

```text
수술 실패 이후 시행한 치료
```

가 들어가 있다면 실제 예측 시점에는 알 수 없는 미래 정보가 들어간 것이다.

---

# 41. 전처리에서도 Leakage가 발생한다

Leakage는 Feature 선택에서만 발생하는 것이 아니다.

예:

```text
전체 데이터 평균 계산
        ↓
결측치 대체
        ↓
Train / Validation 분리
```

도 Validation의 정보가 Train 전처리에 사용될 수 있다.

---

# 42. 잘못된 전처리 순서

```python
X["Age"] = X["Age"].fillna(
    X["Age"].mean()
)

scaler = StandardScaler()

X_scaled = scaler.fit_transform(X)

X_train, X_valid, y_train, y_valid = (
    train_test_split(
        X_scaled,
        y
    )
)
```

이 코드는:

```text
결측치 기준
Scaling 기준
```

을 전체 데이터에서 먼저 계산한다.

Validation 데이터의 정보가 전처리 과정에 들어갈 수 있다.

---

# 43. 더 적절한 순서

```text
X / y 분리
      ↓
Train / Validation 분리
      ↓
Train에서 전처리 기준 학습
      ↓
Train에 적용
      ↓
같은 기준을 Validation에 적용
      ↓
모델 학습
      ↓
Validation 평가
```

이 흐름이 중요하다.

---

# 44. 결측치 + Scaling 예제

```python
from sklearn.model_selection import train_test_split
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import StandardScaler

X_train, X_valid, y_train, y_valid = (
    train_test_split(
        X,
        y,
        test_size=0.2,
        random_state=42,
        stratify=y
    )
)
```

결측치 처리:

```python
imputer = SimpleImputer(
    strategy="median"
)

X_train = imputer.fit_transform(
    X_train
)

X_valid = imputer.transform(
    X_valid
)
```

Scaling:

```python
scaler = StandardScaler()

X_train = scaler.fit_transform(
    X_train
)

X_valid = scaler.transform(
    X_valid
)
```

핵심:

```text
Imputer
→ Train에서 fit

Scaler
→ Train에서 fit
```

이다.

---

# 45. Pipeline

여러 전처리를 하나로 묶을 수 있다.

```python
from sklearn.pipeline import Pipeline
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression

pipeline = Pipeline([
    (
        "imputer",
        SimpleImputer(
            strategy="median"
        )
    ),
    (
        "scaler",
        StandardScaler()
    ),
    (
        "model",
        LogisticRegression(
            max_iter=1000
        )
    )
])
```

---

# 46. Pipeline 학습

```python
pipeline.fit(
    X_train,
    y_train
)
```

내부적으로:

```text
결측치 처리
    ↓
Scaling
    ↓
Logistic Regression 학습
```

이 순서가 실행된다.

---

# 47. Pipeline 예측

```python
pred = pipeline.predict(
    X_valid
)
```

Validation 데이터에도 Train에서 학습한 전처리 기준이 적용된 뒤 모델 예측이 수행된다.

---

# 48. Pipeline의 장점

Pipeline을 사용하면:

```text
전처리 순서 관리

Data Leakage 방지에 도움

Cross Validation 적용 편리

코드 관리 편리

Train/Test 동일한 처리
```

등의 장점이 있다.

---

# 49. 수치형과 범주형 전처리 분리

실제 데이터에는:

```text
Age
BMI
Gender
Job
```

처럼 수치형과 범주형이 섞여 있다.

이 경우:

```text
수치형
→ 결측치 중앙값
→ Scaling

범주형
→ 결측치 최빈값
→ One-Hot Encoding
```

처럼 서로 다른 전처리를 적용할 수 있다.

---

# 50. ColumnTransformer

Scikit-learn에서는 Column별로 서로 다른 전처리를 적용할 수 있다.

```python
from sklearn.compose import ColumnTransformer
from sklearn.pipeline import Pipeline
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import (
    StandardScaler,
    OneHotEncoder
)
```

수치형 Column:

```python
numeric_cols = [
    "Age",
    "BMI"
]
```

범주형 Column:

```python
categorical_cols = [
    "Gender",
    "Job"
]
```

---

# 51. 수치형 Pipeline

```python
numeric_pipeline = Pipeline([
    (
        "imputer",
        SimpleImputer(
            strategy="median"
        )
    ),
    (
        "scaler",
        StandardScaler()
    )
])
```

---

# 52. 범주형 Pipeline

```python
categorical_pipeline = Pipeline([
    (
        "imputer",
        SimpleImputer(
            strategy="most_frequent"
        )
    ),
    (
        "encoder",
        OneHotEncoder(
            handle_unknown="ignore"
        )
    )
])
```

---

# 53. ColumnTransformer 만들기

```python
preprocessor = ColumnTransformer([
    (
        "numeric",
        numeric_pipeline,
        numeric_cols
    ),
    (
        "categorical",
        categorical_pipeline,
        categorical_cols
    )
])
```

의미:

```text
Age / BMI
→ 수치형 전처리

Gender / Job
→ 범주형 전처리
```

이다.

---

# 54. 모델까지 연결

```python
from sklearn.linear_model import LogisticRegression

model = Pipeline([
    (
        "preprocessor",
        preprocessor
    ),
    (
        "classifier",
        LogisticRegression(
            max_iter=1000
        )
    )
])
```

학습:

```python
model.fit(
    X_train,
    y_train
)
```

예측:

```python
pred = model.predict(
    X_valid
)
```

---

# 55. 전체 구조 이해

방금 코드는 길어 보이지만 구조는 단순하다.

```text
원본 X
   ↓
ColumnTransformer
   │
   ├─ 수치형
   │    ↓
   │  결측치 처리
   │    ↓
   │  Scaling
   │
   └─ 범주형
        ↓
      결측치 처리
        ↓
      One-Hot Encoding

          ↓

Logistic Regression
```

이다.

---

# 56. Cross Validation과 Pipeline

Pipeline은 교차검증에서도 중요하다.

```python
from sklearn.model_selection import cross_val_score

scores = cross_val_score(
    model,
    X,
    y,
    cv=5,
    scoring="roc_auc"
)
```

각 Fold에서 Train 부분을 이용하여 전처리 기준을 학습하고 Validation 부분에 적용하는 구조를 만들기 쉬워진다.

---

# 57. SMOTE는 어디에 적용해야 할까?

클래스 불균형을 처리하기 위해 SMOTE를 사용할 수도 있다.

중요한 원칙:

```text
전체 데이터에 SMOTE
        ↓
Train / Validation 분리
```

처럼 하면 안 된다.

Validation 데이터의 정보가 합성 데이터 생성 과정에 영향을 줄 수 있기 때문이다.

---

# 58. SMOTE의 기본 원칙

```text
Train / Validation 분리
        ↓
Train에만 SMOTE
        ↓
모델 학습
        ↓
원본 Validation으로 평가
```

한다.

Validation 데이터는 인위적으로 증강하지 않는다.

---

# 59. 교차검증과 SMOTE

Cross Validation에서도:

```text
각 Fold의 Train
        ↓
SMOTE
        ↓
모델 학습

각 Fold의 Validation
        ↓
원본 그대로 평가
```

해야 한다.

불균형 데이터 처리는 머신러닝 단원에서 자세히 다룬다.

---

# 60. 전처리와 Feature Engineering 차이

전처리:

```text
기존 데이터를 모델이 사용할 수 있도록 정리
```

예:

```text
결측치 처리
문자열 변환
Scaling
Encoding
```

Feature Engineering:

```text
기존 데이터를 이용해 더 의미 있는 변수를 생성하거나 변환
```

예:

```text
체중 + 신장
→ BMI

난자 수 + 배아 수
→ 비율 Feature

날짜
→ 업력
```

둘은 연결되어 있지만 목적이 조금 다르다.

---

# 61. 전처리 전후 Shape 확인

전처리 후 데이터 구조가 달라질 수 있다.

특히 One-Hot Encoding을 하면 Column 수가 증가할 수 있다.

따라서:

```python
print(
    X_train.shape
)

print(
    X_train_processed.shape
)
```

처럼 Shape를 확인하는 습관이 중요하다.

---

# 62. 전처리 후 확인할 것

```text
결측치가 남아 있는가?

문자열이 남아 있는가?

무한대 값이 있는가?

Train과 Validation Column 구조가 같은가?

Target이 X에 들어가 있지 않은가?

Shape가 예상한 것과 같은가?

클래스 비율은 어떤가?
```

를 확인한다.

---

# 63. 무한대 값 확인

Feature Engineering 과정에서 나누기를 사용하면:

```text
inf
-inf
```

가 발생할 수 있다.

예:

```python
df["ratio"] = (
    df["A"] / df["B"]
)
```

`B = 0`이면 문제가 발생할 수 있다.

확인:

```python
import numpy as np

np.isinf(
    df.select_dtypes(
        include="number"
    )
).sum()
```

---

# 64. 무한대 처리

상황에 따라:

```python
df = df.replace(
    [np.inf, -np.inf],
    np.nan
)
```

으로 바꾼 뒤 결측치 처리 방법을 적용할 수도 있다.

하지만 왜 무한대가 발생했는지 먼저 확인하는 것이 중요하다.

---

# 65. 전처리에서 가장 중요한 질문

어떤 코드를 사용할지보다 먼저 다음을 생각한다.

```text
이 변수는 무엇을 의미하는가?

숫자인가 범주인가?

결측치는 왜 발생했는가?

이상치는 실제 값인가?

Encoding이 필요한가?

Scaling이 필요한 모델인가?

실제 예측 시점에 사용할 수 있는 정보인가?

전처리 기준을 Train에서만 만들었는가?
```

---

# 66. 전처리 전체 흐름

```text
원본 데이터 보존
        ↓
자료형 확인
        ↓
잘못된 값 확인
        ↓
결측치 처리
        ↓
이상치 검토
        ↓
Feature / Target 분리
        ↓
Train / Validation 분리
        ↓
Train 기준으로
전처리 학습
        ↓
Validation/Test에
같은 기준 적용
        ↓
모델 학습
        ↓
성능 평가
```

---

# 67. 가장 중요한 원칙

머신러닝 전처리에서 매우 중요한 원칙은:

```text
Validation과 Test는
미래에 처음 만나는 데이터처럼 다룬다.
```

는 것이다.

따라서 Validation의:

```text
평균
중앙값
표준편차
최솟값
최댓값
범주 정보
```

를 이용해서 Train 전처리 기준을 만들면 안 된다.

Train에서 기준을 학습하고:

```text
Train → fit
Validation → transform
Test → transform
```

으로 적용한다.

---

# 68. 배운 점

데이터 전처리는 단순히 데이터를 깨끗하게 만드는 작업이 아니다.

머신러닝 모델이 데이터를 올바르게 학습할 수 있도록 준비하는 과정이다.

대표적으로:

```text
자료형 변환
결측치 처리
이상치 처리
Encoding
Scaling
Feature / Target 분리
Train / Validation 분리
```

등이 포함된다.

범주형 데이터에는:

```text
Label Encoding
One-Hot Encoding
```

등을 사용할 수 있다.

수치형 데이터에는 필요에 따라:

```text
StandardScaler
MinMaxScaler
RobustScaler
```

등을 사용할 수 있다.

그리고 가장 중요한 것은 전처리 기준을 전체 데이터에서 만들지 않는 것이다.

```text
Train에서 fit
        ↓
Validation/Test에는 transform
```

원칙을 지켜야 Data Leakage를 줄일 수 있다.

전처리 과정이 많아지면:

```text
Pipeline
ColumnTransformer
```

를 사용하여 전처리와 모델을 하나의 흐름으로 관리할 수 있다.

결국 좋은 전처리는:

```text
데이터의 의미 이해
+
올바른 처리 방법
+
Data Leakage 방지
+
Validation을 통한 확인
```

의 조합이라고 볼 수 있다.