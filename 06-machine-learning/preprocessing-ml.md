# Machine Learning 전처리

## 1. 머신러닝에서 전처리란?

Machine Learning 모델에 데이터를 넣기 전에 모델이 사용할 수 있는 형태로 데이터를 변환하는 과정을 전처리라고 한다.

영어:

```text
Preprocessing
```

대표적으로:

```text
결측치 처리
문자형 데이터 변환
Scaling
Encoding
이상값 처리
Feature 선택
```

등이 있다.

---

# 2. 왜 전처리가 필요한가?

실제 데이터는 바로 모델에 넣기 어려운 경우가 많다.

예:

| age | gender | income | BMI |
|---:|---|---:|---:|
| 30 | M | 3500 | 22.5 |
| NaN | F | 4200 | 25.1 |
| 45 | M | NaN | 28.7 |

문제:

```text
결측치 존재

문자 데이터 존재

변수마다 숫자 범위가 다름
```

이런 문제를 모델 학습 전에 처리할 수 있다.

---

# 3. 머신러닝 전처리 전체 흐름

기본적으로:

```text
Raw Data
   ↓
Train / Test Split
   ↓
결측치 처리
   ↓
Categorical Encoding
   ↓
Scaling
   ↓
Model
```

와 같은 흐름을 생각할 수 있다.

중요한 점은:

```text
Train/Test Split을 먼저 한다.
```

는 것이다.

---

# 4. 왜 Split을 먼저 하는가?

잘못된 흐름:

```text
전체 데이터
   ↓
전처리 기준 학습
   ↓
Train/Test Split
```

이렇게 하면 Test Data의 정보가 전처리 과정에 들어갈 수 있다.

이를:

```text
Data Leakage
```

라고 한다.

---

# 5. 올바른 기본 흐름

```text
전체 데이터
   ↓
Train / Test Split
   ↓
Train에서 전처리 기준 학습
   ↓
Train 변환
   ↓
Test는 Train 기준으로 변환
```

이다.

---

# 6. fit과 transform

전처리에서 매우 중요한 두 개념이다.

```text
fit
→ 기준을 학습

transform
→ 학습한 기준으로 변환
```

이다.

---

# 7. StandardScaler 예

예를 들어 Train Data가:

```text
10
20
30
40
50
```

이라면 StandardScaler는 Train Data를 보고:

```text
평균
표준편차
```

를 계산한다.

이 과정이:

```text
fit
```

이다.

---

# 8. transform

학습한 평균과 표준편차를 이용하여 실제 데이터를 변환한다.

```text
원본 데이터
    ↓
Train에서 학습한 평균/표준편차
    ↓
변환된 데이터
```

이 과정이:

```text
transform
```

이다.

---

# 9. fit_transform

다음 두 작업:

```python
scaler.fit(
    X_train
)

X_train_scaled = scaler.transform(
    X_train
)
```

을 한 번에:

```python
X_train_scaled = scaler.fit_transform(
    X_train
)
```

으로 작성할 수 있다.

---

# 10. Train과 Test

일반적으로:

```python
X_train_scaled = scaler.fit_transform(
    X_train
)

X_test_scaled = scaler.transform(
    X_test
)
```

처럼 사용한다.

핵심:

```text
Train
→ fit + transform

Test
→ transform만
```

이다.

---

# 11. Test에 fit하면 안 되는 이유

잘못된 코드:

```python
X_test_scaled = scaler.fit_transform(
    X_test
)
```

이렇게 하면 Test Data 자체의 평균과 표준편차를 새롭게 학습한다.

그러면 Train에서 만든 전처리 기준과 달라진다.

Test는:

```text
처음 보는 새로운 데이터
```

처럼 다뤄야 한다.

---

# 12. 실제 서비스에서도 같다

모델을 학습할 때:

```text
Train Data
↓
Scaler fit
```

을 했다면 실제 서비스의 새로운 사용자 데이터가 들어왔을 때도:

```text
New Data
↓
기존 Scaler transform
↓
Model
```

이어야 한다.

새 사용자 한 명이 들어올 때마다 Scaler를 다시 fit하는 것이 아니다.

---

# 13. Scaling

Feature들의 값 범위를 조정하는 것을:

```text
Scaling
```

이라고 한다.

예:

```text
age
20 ~ 80

income
1,000,000 ~ 100,000,000
```

두 Feature의 숫자 크기가 매우 다르다.

---

# 14. Scaling이 필요한 이유

일부 알고리즘은 Feature의 크기에 영향을 많이 받을 수 있다.

예:

```text
Logistic Regression

Linear Regression

Ridge

KNN

SVM

Neural Network
```

등에서는 Scaling이 중요할 수 있다.

---

# 15. 모든 모델에 Scaling이 필수인가?

아니다.

예를 들어 Tree 기반 모델:

```text
Decision Tree

Random Forest

XGBoost

LightGBM

CatBoost
```

은 일반적으로 Feature Scaling의 영향을 상대적으로 덜 받는다.

따라서:

```text
모든 머신러닝 = 무조건 Scaling
```

이라고 외우면 안 된다.

---

# 16. StandardScaler

대표적인 Scaling 방법이다.

```python
from sklearn.preprocessing import (
    StandardScaler
)
```

사용:

```python
scaler = StandardScaler()
```

---

# 17. Standardization

StandardScaler는 데이터를 평균과 표준편차를 기준으로 변환한다.

개념적으로:

```text
z = (x - 평균) / 표준편차
```

이다.

변환 후 Train Data는 일반적으로:

```text
평균 ≈ 0
표준편차 ≈ 1
```

에 가까워진다.

---

# 18. StandardScaler 코드

```python
scaler = StandardScaler()

X_train_scaled = scaler.fit_transform(
    X_train
)

X_test_scaled = scaler.transform(
    X_test
)
```

---

# 19. 평균 확인

NumPy Array라면:

```python
print(
    X_train_scaled.mean(
        axis=0
    )
)
```

평균이 0에 가까운 값을 볼 수 있다.

---

# 20. 표준편차 확인

```python
print(
    X_train_scaled.std(
        axis=0
    )
)
```

대략 1에 가까운 값을 확인할 수 있다.

---

# 21. MinMaxScaler

다른 Scaling 방법:

```python
from sklearn.preprocessing import (
    MinMaxScaler
)
```

사용:

```python
scaler = MinMaxScaler()
```

---

# 22. Min-Max Scaling

일반적인 기본 설정에서는 Train Data의 최소값과 최대값을 이용하여 값을 대략:

```text
0 ~ 1
```

범위로 변환한다.

개념:

```text
x - min
─────────
max - min
```

이다.

---

# 23. MinMaxScaler 코드

```python
scaler = MinMaxScaler()

X_train_scaled = scaler.fit_transform(
    X_train
)

X_test_scaled = scaler.transform(
    X_test
)
```

여기서도:

```text
Train에서 min/max 학습

Test에는 같은 min/max 적용
```

이 중요하다.

Test 값이 Train 범위를 벗어나면 변환값이 0보다 작거나 1보다 커질 수도 있다.

---

# 24. RobustScaler

이상치의 영향을 상대적으로 줄이고 싶을 때 고려할 수 있는 Scaling 방법이다.

```python
from sklearn.preprocessing import (
    RobustScaler
)
```

사용:

```python
scaler = RobustScaler()
```

---

# 25. RobustScaler 기준

RobustScaler는 Median과 IQR 등을 이용한다.

따라서 극단적인 이상치에 StandardScaler보다 덜 민감할 수 있다.

하지만:

```text
이상치가 있으면 무조건 RobustScaler
```

라는 뜻은 아니다.

데이터와 모델을 보고 비교한다.

---

# 26. Scaler 비교

| Scaler | 주요 기준 | 특징 |
|---|---|---|
| StandardScaler | 평균, 표준편차 | 자주 사용 |
| MinMaxScaler | 최소, 최대 | 기본적으로 0~1 범위 |
| RobustScaler | 중앙값, IQR | 이상치 영향에 비교적 강함 |

---

# 27. 어떤 Scaler를 선택할까?

정답 하나가 있는 것은 아니다.

다음을 고려한다.

```text
Model 종류

데이터 분포

이상치

Feature 특성

Validation 성능
```

그리고 실험으로 비교할 수 있다.

---

# 28. 결측치

머신러닝 모델에 따라 NaN을 직접 처리하지 못할 수 있다.

예:

```python
df.isnull().sum()
```

으로 먼저 결측치를 확인한다.

---

# 29. SimpleImputer

Scikit-learn에서 결측치를 처리할 때 사용할 수 있다.

```python
from sklearn.impute import (
    SimpleImputer
)
```

---

# 30. Median Imputation

```python
imputer = SimpleImputer(
    strategy="median"
)
```

사용:

```python
X_train_imputed = imputer.fit_transform(
    X_train
)

X_test_imputed = imputer.transform(
    X_test
)
```

---

# 31. 여기서도 fit이 중요

Train:

```text
Train Median 계산
↓
Train 결측치 처리
```

Test:

```text
Train에서 계산한 Median
↓
Test 결측치 처리
```

이다.

---

# 32. Test Median을 사용하면?

잘못된 예:

```python
X_test_imputed = imputer.fit_transform(
    X_test
)
```

Test Data의 정보를 이용하여 결측치 처리 기준을 새로 학습하게 된다.

따라서 피해야 한다.

---

# 33. SimpleImputer strategy

대표적으로:

```text
mean
median
most_frequent
constant
```

등을 사용할 수 있다.

---

# 34. mean

```python
SimpleImputer(
    strategy="mean"
)
```

평균으로 결측치를 채운다.

숫자형 데이터에서 사용할 수 있다.

---

# 35. median

```python
SimpleImputer(
    strategy="median"
)
```

중앙값으로 채운다.

이상치가 있는 숫자형 변수에서 평균보다 안정적일 수 있다.

---

# 36. most_frequent

```python
SimpleImputer(
    strategy="most_frequent"
)
```

가장 자주 등장하는 값으로 채운다.

범주형 변수 등에서 고려할 수 있다.

---

# 37. constant

```python
SimpleImputer(
    strategy="constant",
    fill_value="Unknown"
)
```

특정 값으로 결측치를 채운다.

예:

```text
Unknown
Missing
```

등이다.

---

# 38. 결측치 자체가 정보일 수도 있다

어떤 Feature가 비어 있다는 사실 자체가 의미를 가질 수 있다.

이 경우:

```text
Missing Indicator
```

를 Feature로 만들 수도 있다.

예:

```python
df[
    "income_missing"
] = (
    df["income"]
    .isnull()
    .astype(int)
)
```

---

# 39. Categorical Data

다음과 같은 데이터가 있다고 하자.

```text
gender

M
F
M
F
```

문자열을 그대로 사용할 수 없는 모델도 많다.

따라서 숫자 형태로 변환해야 할 수 있다.

이를:

```text
Encoding
```

이라고 한다.

---

# 40. Encoding

대표적인 방법:

```text
One-Hot Encoding

Ordinal Encoding
```

등이 있다.

LabelEncoder도 있지만 Feature Encoding에서는 목적과 데이터 의미를 구분해서 사용해야 한다.

---

# 41. One-Hot Encoding

예:

```text
city

Seoul
Busan
Daegu
```

를:

```text
city_Seoul
city_Busan
city_Daegu
```

같은 새로운 Feature로 만든다.

---

# 42. One-Hot 결과

원본:

| city |
|---|
| Seoul |
| Busan |
| Seoul |

변환:

| city_Seoul | city_Busan |
|---:|---:|
| 1 | 0 |
| 0 | 1 |
| 1 | 0 |

---

# 43. OneHotEncoder

Scikit-learn:

```python
from sklearn.preprocessing import (
    OneHotEncoder
)
```

예:

```python
encoder = OneHotEncoder(
    handle_unknown="ignore"
)
```

---

# 44. handle_unknown="ignore"

Train에는:

```text
Seoul
Busan
```

만 있었는데 Test에:

```text
Daegu
```

가 처음 등장할 수 있다.

`handle_unknown="ignore"`를 사용하면 학습할 때 없던 Category가 나타났을 때 오류가 발생하는 문제를 줄일 수 있다.

---

# 45. Train/Test에서 따로 get_dummies하면?

예:

Train:

```text
Seoul
Busan
```

Test:

```text
Seoul
Daegu
```

각각 따로:

```python
pd.get_dummies()
```

를 실행하면 생성되는 Column이 달라질 수 있다.

Train:

```text
city_Busan
city_Seoul
```

Test:

```text
city_Daegu
city_Seoul
```

가 될 수 있다.

---

# 46. Feature 구조가 달라지는 문제

머신러닝 모델은 학습할 때 사용한 Feature 구조와 예측할 때 Feature 구조가 맞아야 한다.

따라서 Train에서 Encoding 규칙을 학습하고 Test에 동일하게 적용하는 방식이 안전하다.

---

# 47. Ordinal Encoding

순서가 있는 범주형 데이터라면 순서를 반영할 수 있다.

예:

```text
Low
Medium
High
```

개념적으로:

```text
Low = 0
Medium = 1
High = 2
```

처럼 표현할 수 있다.

---

# 48. 순서가 없는 데이터에 숫자를 부여하면?

예:

```text
Seoul = 0
Busan = 1
Daegu = 2
```

라고 하면 일부 모델은 숫자의 크기 관계를 의미 있는 순서로 받아들일 수 있다.

하지만:

```text
Daegu > Busan > Seoul
```

이라는 의미는 없다.

따라서 범주형 변수의 의미를 보고 Encoding 방법을 선택한다.

---

# 49. LabelEncoder

```python
from sklearn.preprocessing import (
    LabelEncoder
)
```

LabelEncoder는 Target Label을 숫자로 변환할 때 자주 사용할 수 있다.

예:

```text
Normal
Disease
```

↓

```text
0
1
```

---

# 50. LabelEncoder를 모든 Feature에 무조건 쓰지 않는다

Feature가:

```text
Seoul
Busan
Daegu
```

인데:

```text
0
1
2
```

로 변환하면 존재하지 않는 순서 관계가 생길 수 있다.

따라서:

```text
Feature인가?

Target인가?

순서가 있는가?
```

를 확인한다.

---

# 51. 숫자형과 범주형을 다르게 처리

실제 데이터에는:

```text
age
BMI
gender
job
blood_type
```

같이 여러 자료형이 섞여 있을 수 있다.

예:

```text
숫자형

age
BMI
```

```text
범주형

gender
job
blood_type
```

각각 다른 전처리가 필요할 수 있다.

---

# 52. 숫자형 Pipeline

예:

```text
Numeric Data
    ↓
Median Imputation
    ↓
StandardScaler
```

이다.

---

# 53. 범주형 Pipeline

예:

```text
Categorical Data
      ↓
Most Frequent Imputation
      ↓
One-Hot Encoding
```

이다.

---

# 54. Pipeline

Scikit-learn에서는 여러 전처리 단계를 하나로 연결할 수 있다.

```python
from sklearn.pipeline import (
    Pipeline
)
```

---

# 55. 숫자형 Pipeline

```python
numeric_pipeline = Pipeline(
    steps=[
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
    ]
)
```

흐름:

```text
Numeric Feature
      ↓
SimpleImputer
      ↓
StandardScaler
```

이다.

---

# 56. 범주형 Pipeline

```python
categorical_pipeline = Pipeline(
    steps=[
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
    ]
)
```

흐름:

```text
Categorical Feature
        ↓
SimpleImputer
        ↓
OneHotEncoder
```

이다.

---

# 57. ColumnTransformer

숫자형 Column과 범주형 Column에 서로 다른 전처리를 적용할 수 있다.

```python
from sklearn.compose import (
    ColumnTransformer
)
```

---

# 58. Column 목록

```python
numeric_features = [
    "age",
    "BMI",
    "glucose"
]

categorical_features = [
    "gender",
    "job"
]
```

---

# 59. ColumnTransformer 생성

```python
preprocessor = ColumnTransformer(
    transformers=[
        (
            "num",
            numeric_pipeline,
            numeric_features
        ),
        (
            "cat",
            categorical_pipeline,
            categorical_features
        )
    ]
)
```

---

# 60. 구조

```text
                    ┌→ Numeric Columns
                    │      ↓
Raw Data ───────────┤   Imputer
                    │      ↓
                    │   Scaler
                    │
                    └→ Categorical Columns
                           ↓
                        Imputer
                           ↓
                     OneHotEncoder
```

그리고 결과를 다시 하나의 Feature Matrix로 결합한다.

---

# 61. Model까지 Pipeline에 넣기

전처리뿐 아니라 Model까지 하나로 연결할 수 있다.

```python
from sklearn.linear_model import (
    LogisticRegression
)

model = Pipeline(
    steps=[
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
    ]
)
```

---

# 62. 전체 구조

```text
Raw Data
   ↓
ColumnTransformer
   │
   ├── Numeric
   │     ↓
   │   Imputer
   │     ↓
   │   Scaler
   │
   └── Categorical
         ↓
       Imputer
         ↓
       Encoder
   ↓
Logistic Regression
   ↓
Prediction
```

---

# 63. Pipeline 학습

이제:

```python
model.fit(
    X_train,
    y_train
)
```

만 실행해도 Pipeline 내부에서:

```text
전처리 fit
      ↓
전처리 transform
      ↓
Model fit
```

이 순서대로 이루어진다.

---

# 64. Pipeline 예측

```python
pred = model.predict(
    X_test
)
```

하면:

```text
X_test
   ↓
Train에서 학습된 전처리 기준으로 transform
   ↓
학습된 Model
   ↓
Prediction
```

이 이루어진다.

---

# 65. Pipeline의 큰 장점

Pipeline을 사용하면:

```text
전처리와 Model을 하나의 작업 흐름으로 관리

Train/Test 전처리 규칙 일치

코드 정리

Cross Validation과 결합

Data Leakage 방지에 도움
```

등의 장점이 있다.

---

# 66. Pipeline이 Leakage를 자동으로 모두 막아줄까?

아니다.

Pipeline 밖에서 이미:

```text
전체 데이터로 Feature Selection

전체 데이터로 평균 계산

전체 데이터로 SMOTE

Target 정보를 이용한 Feature 생성
```

등을 했다면 Leakage가 발생할 수 있다.

Pipeline은 올바르게 구성해야 한다.

---

# 67. Pipeline과 Cross Validation

예:

```python
from sklearn.model_selection import (
    cross_val_score
)

scores = cross_val_score(
    model,
    X,
    y,
    cv=5,
    scoring="accuracy"
)
```

Pipeline을 사용하면 각 Fold의 Training Data에서 전처리 기준을 학습하고 해당 Validation Fold에 적용하도록 구성할 수 있다.

---

# 68. Pipeline 없이 CV를 잘못하면?

잘못된 예:

```python
X_scaled = scaler.fit_transform(
    X
)

scores = cross_val_score(
    model,
    X_scaled,
    y,
    cv=5
)
```

Scaling이 Cross Validation보다 먼저 전체 데이터에 fit되었다.

즉 각 Validation Fold의 정보도 Scaling 기준에 들어갔다.

---

# 69. 더 적절한 방식

```text
Pipeline
   ↓
각 Fold마다
   ↓
Training Fold에서 Scaler fit
   ↓
Validation Fold transform
   ↓
Model 평가
```

이다.

---

# 70. SMOTE와 Pipeline

Class Imbalance를 처리하기 위해 SMOTE를 사용할 수 있다.

중요한 것은:

```text
SMOTE는 Training Data에만 적용
```

하는 것이다.

---

# 71. 잘못된 SMOTE

```text
전체 데이터
   ↓
SMOTE
   ↓
Cross Validation
```

은 Leakage 위험이 있다.

---

# 72. CV에서 SMOTE

각 Fold에서:

```text
Training Fold
   ↓
SMOTE
   ↓
Model fit

Validation Fold
→ 원래 데이터로 평가
```

하는 구조가 필요하다.

이때 `imbalanced-learn`의 Pipeline을 사용할 수 있다.

---

# 73. imblearn Pipeline

예:

```python
from imblearn.pipeline import (
    Pipeline
)
```

Scikit-learn Pipeline과 이름은 같지만 Sampling 단계까지 Pipeline 안에 넣을 수 있다는 점에서 활용할 수 있다.

자세한 내용은 불균형 데이터 파일에서 다룬다.

---

# 74. Feature Selection

모든 Feature를 무조건 모델에 넣을 필요는 없다.

예:

```text
ID

중복 Column

Target Leakage Column

예측 시점에 알 수 없는 정보

의미 없는 상수 Column
```

등은 제거를 고려할 수 있다.

---

# 75. ID Column

예:

```text
patient_id
```

가 단순 식별 번호라면 모델 Feature에서 제외할 수 있다.

하지만 Patient 단위 Split을 위해서는 ID가 필요할 수 있다.

즉:

```text
모델 Feature로 사용하지 않음

하지만 데이터 관리/Split에는 사용
```

할 수 있다.

---

# 76. Target Leakage Feature

예를 들어 수술 실패를 예측하는데 Feature에:

```text
수술 실패 후 처치 여부
```

가 들어있다면 문제가 될 수 있다.

예측 시점에는 아직 알 수 없는 정보이기 때문이다.

---

# 77. 예측 시점이 중요

Feature를 선택할 때 항상 질문한다.

```text
실제로 예측하는 순간
이 정보를 알 수 있는가?
```

알 수 없다면 모델 Feature로 사용하면 안 될 가능성이 높다.

---

# 78. 문자열을 숫자로 바꾸기

데이터에:

```text
"120"
"135"
"??"
```

같은 값이 있을 수 있다.

이때:

```python
df[
    "SBP"
] = pd.to_numeric(
    df["SBP"],
    errors="coerce"
)
```

처럼 처리할 수 있다.

---

# 79. errors="coerce"

숫자로 변환할 수 없는 값:

```text
??
error
unknown
```

등을:

```text
NaN
```

으로 바꾼다.

그 후 Imputer를 사용할 수 있다.

---

# 80. 잘못된 값을 바로 0으로 바꾸면?

예:

```text
혈압 = ??
```

를:

```text
혈압 = 0
```

으로 바꾸면 모델은 실제 혈압 0이라는 의미로 받아들일 수 있다.

따라서:

```text
잘못된 값
→ NaN
→ 적절한 결측치 처리
```

를 고려하는 것이 더 자연스러울 수 있다.

---

# 81. Feature 이름 유지

전처리를 거치면 NumPy Array나 Sparse Matrix로 변환되어 원래 Column 이름을 확인하기 어려워질 수 있다.

필요하면:

```python
preprocessor.get_feature_names_out()
```

등을 이용하여 변환된 Feature 이름을 확인할 수 있다.

---

# 82. One-Hot 후 Feature 수

원본 Feature:

```text
gender
job
```

두 개뿐이어도 Category가 많으면 One-Hot Encoding 후 Feature 수가 크게 증가할 수 있다.

예:

```text
job

student
developer
doctor
teacher
nurse
...
```

↓

```text
job_student
job_developer
job_doctor
job_teacher
job_nurse
...
```

---

# 83. High Cardinality

범주 종류가 매우 많은 변수를:

```text
High Cardinality
```

범주형 변수라고 부르기도 한다.

예:

```text
수천 개의 상품 코드

수많은 주소

고유 ID
```

One-Hot Encoding을 무조건 적용하면 Feature 수가 지나치게 증가할 수 있다.

---

# 84. CatBoost와 범주형 변수

CatBoost 같은 모델은 범주형 변수를 다루는 기능을 제공한다.

따라서:

```text
모든 모델에 동일한 Encoding 방법
```

을 반드시 사용할 필요는 없다.

모델 특성을 확인해야 한다.

---

# 85. Tree 모델과 결측치

일부 Boosting Library는 결측치를 자체적으로 다룰 수 있다.

예:

```text
XGBoost
LightGBM
CatBoost
```

등은 설정과 데이터 타입에 따라 NaN을 처리할 수 있다.

따라서 무조건 모든 결측치를 먼저 채워야 한다고 단정하지 않는다.

---

# 86. 전처리는 모델과 함께 결정한다

예:

```text
Logistic Regression
→ Scaling + Encoding 고려

Random Forest
→ Encoding 필요 여부 확인
→ Scaling 영향 상대적으로 적음

CatBoost
→ 범주형 처리 기능 활용 가능

Neural Network
→ Scaling 중요할 수 있음
```

이다.

---

# 87. Preprocessing vs Feature Engineering

Preprocessing:

```text
모델이 데이터를 안정적으로 사용할 수 있도록 변환
```

예:

```text
결측치 처리
Scaling
Encoding
```

Feature Engineering:

```text
예측에 도움이 될 새로운 정보를 생성
```

예:

```text
BMI

비율

합계

기간

상호작용
```

이다.

---

# 88. 둘은 연결된다

실제 프로젝트:

```text
Raw Data
   ↓
Cleaning
   ↓
Feature Engineering
   ↓
Train/Test Split
   ↓
Train 기준 Preprocessing
   ↓
Model
```

같은 흐름을 만들 수 있다.

단, Feature Engineering도 데이터 전체의 통계나 Target을 이용한다면 Leakage를 주의해야 한다.

---

# 89. 예: BMI

```python
df[
    "BMI"
] = (
    df["weight"]
    /
    (
        df["height_m"] ** 2
    )
)
```

각 행 자체의 체중과 신장만 이용한다면 일반적으로 다른 Sample의 정보를 사용하지 않는다.

---

# 90. 반면 전체 평균을 이용한다면?

예:

```python
global_mean = df[
    "income"
].mean()
```

전체 Dataset의 평균을 이용하여 Feature를 만들면 Test 정보가 포함될 수 있다.

따라서 Train 기준으로 계산해야 하는 통계인지 확인한다.

---

# 91. 모델 학습 전 확인

```python
print(
    X_train.shape
)

print(
    X_test.shape
)

print(
    X_train.dtypes
)

print(
    X_train.isnull().sum()
)
```

등으로 상태를 확인한다.

---

# 92. 무한대 값 확인

비율 Feature를 만들면:

```text
inf
-inf
```

가 생길 수 있다.

확인:

```python
import numpy as np

print(
    np.isinf(
        X.select_dtypes(
            include=np.number
        )
    ).sum()
)
```

---

# 93. inf 처리

상황에 따라:

```python
df = df.replace(
    [np.inf, -np.inf],
    np.nan
)
```

으로 바꾼 뒤 결측치 처리 전략을 적용할 수 있다.

---

# 94. 전체 Pipeline 예제

```python
from sklearn.model_selection import (
    train_test_split
)

from sklearn.impute import (
    SimpleImputer
)

from sklearn.preprocessing import (
    StandardScaler,
    OneHotEncoder
)

from sklearn.compose import (
    ColumnTransformer
)

from sklearn.pipeline import (
    Pipeline
)

from sklearn.linear_model import (
    LogisticRegression
)


numeric_features = [
    "age",
    "BMI",
    "glucose"
]

categorical_features = [
    "gender",
    "job"
]


X = df[
    numeric_features
    +
    categorical_features
]

y = df[
    "target"
]


X_train, X_test, y_train, y_test = (
    train_test_split(
        X,
        y,
        test_size=0.2,
        random_state=42,
        stratify=y
    )
)


numeric_pipeline = Pipeline(
    steps=[
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
    ]
)


categorical_pipeline = Pipeline(
    steps=[
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
    ]
)


preprocessor = ColumnTransformer(
    transformers=[
        (
            "num",
            numeric_pipeline,
            numeric_features
        ),
        (
            "cat",
            categorical_pipeline,
            categorical_features
        )
    ]
)


model = Pipeline(
    steps=[
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
    ]
)


model.fit(
    X_train,
    y_train
)


pred = model.predict(
    X_test
)
```

---

# 95. 위 코드에서 중요한 점

우리가 직접:

```python
scaler.fit(
    X_train
)
```

을 따로 호출하지 않았다.

Pipeline 안에서:

```python
model.fit(
    X_train,
    y_train
)
```

을 실행하면 필요한 전처리 단계들이 Train Data를 기준으로 학습된다.

---

# 96. predict할 때

```python
model.predict(
    X_test
)
```

하면 Pipeline이:

```text
X_test
↓
Imputer transform
↓
Scaler transform
↓
Encoder transform
↓
Logistic Regression
↓
Prediction
```

을 처리한다.

---

# 97. 새로운 사용자 데이터

서비스에서 새로운 데이터가 들어왔다고 하자.

```python
new_data = pd.DataFrame(
    [
        {
            "age": 45,
            "BMI": 27.3,
            "glucose": 135,
            "gender": "M",
            "job": "developer"
        }
    ]
)
```

예측:

```python
prediction = model.predict(
    new_data
)
```

Pipeline을 저장해두었다면 동일한 전처리 규칙과 모델을 함께 적용할 수 있다.

---

# 98. 전처리 객체도 모델의 일부다

서비스에 Model만 저장하고 Scaler를 버리면 문제가 생길 수 있다.

학습할 때:

```text
Raw Data
↓
Scaler
↓
Model
```

이었다면 실제 예측에서도:

```text
Raw New Data
↓
같은 Scaler
↓
같은 Model
```

이 필요하다.

그래서 Pipeline으로 함께 관리하는 것이 편리하다.

---

# 99. 모델 저장과 연결

나중에:

```text
joblib

pickle
```

등을 이용하여 학습된 Pipeline을 저장할 수 있다.

예:

```python
import joblib

joblib.dump(
    model,
    "model.pkl"
)
```

불러오기:

```python
model = joblib.load(
    "model.pkl"
)
```

신뢰할 수 없는 pickle/joblib 파일을 임의로 로드하는 것은 보안상 위험할 수 있다.

---

# 100. FastAPI와 연결

나중에 AI 서비스를 만들면:

```text
사용자 입력
    ↓
FastAPI
    ↓
저장된 Pipeline
    ↓
Preprocessing
    ↓
Model
    ↓
Prediction
    ↓
Response
```

구조를 만들 수 있다.

---

# 101. 자주 하는 실수

### 실수 1

```python
scaler.fit_transform(
    X
)
```

을 전체 데이터에 먼저 수행한 뒤 Split.

```text
→ Leakage 가능
```

### 실수 2

Train과 Test에 각각:

```python
fit_transform()
```

사용.

```text
→ 서로 다른 전처리 기준
```

### 실수 3

Train/Test에서 각각 독립적으로 One-Hot Encoding.

```text
→ Feature Column 불일치 가능
```

### 실수 4

전체 데이터에 SMOTE 후 Split.

```text
→ Leakage 위험
```

### 실수 5

예측 시점에 알 수 없는 Feature 사용.

```text
→ Target Leakage 가능
```

---

# 102. 전처리할 때 질문

항상 다음을 확인한다.

```text
이 Feature는 숫자인가?

범주형인가?

결측치는 있는가?

이상한 문자열은 있는가?

Scaling이 필요한 모델인가?

Encoding이 필요한가?

이 전처리 기준을 Train에서만 학습했는가?

Test 정보를 미리 사용하지 않았는가?

실제 서비스에서도 동일하게 처리할 수 있는가?
```

---

# 103. 핵심 용어

```text
Preprocessing
→ 모델 입력 전 데이터 변환

Scaling
→ Feature의 값 범위 조정

StandardScaler
→ 평균/표준편차 기반 Scaling

MinMaxScaler
→ 최소/최대 기반 Scaling

RobustScaler
→ 중앙값/IQR 기반 Scaling

Imputation
→ 결측치 처리

SimpleImputer
→ Scikit-learn 결측치 처리 도구

Encoding
→ 범주형 값을 모델이 사용할 수 있는 형태로 변환

OneHotEncoder
→ 범주를 여러 0/1 Feature로 변환

fit
→ 전처리 기준 학습

transform
→ 학습된 기준으로 데이터 변환

fit_transform
→ fit + transform

Pipeline
→ 여러 전처리/모델 단계를 연결

ColumnTransformer
→ Column 종류별로 다른 전처리 적용

Data Leakage
→ 평가 데이터 정보가 학습 과정에 들어가는 문제
```

---

# 104. 가장 중요한 구조

```text
전체 데이터
      ↓
Train / Test Split
      ↓
Train
      ↓
Preprocessor fit
      ↓
Train transform
      ↓
Model fit
```

그리고 Test:

```text
Test
 ↓
Train에서 학습한
Preprocessor transform
 ↓
학습된 Model
 ↓
Prediction
```

이다.

---

# 105. 한 줄로 기억하기

```text
fit은 Train에서만,
Test는 transform만.
```

이 원칙은:

```text
Scaler

Imputer

Encoder

Feature Selection
```

등 Train Data에서 기준을 학습하는 전처리에 매우 중요하다.

---

# 106. 배운 점

Machine Learning 전처리는 단순히 데이터를 깨끗하게 만드는 작업만을 의미하지 않는다.

중요한 것은:

```text
모델이 학습할 수 있는 형태로 만들고

평가 데이터의 정보를 미리 보지 않도록 하는 것
```

이다.

특히:

```text
fit
→ 기준 학습

transform
→ 기준 적용
```

을 구분해야 한다.

Train Data에서는:

```python
fit_transform()
```

을 사용할 수 있고 Test Data에는:

```python
transform()
```

을 사용한다.

숫자형 데이터에는:

```text
Imputation
Scaling
```

범주형 데이터에는:

```text
Imputation
Encoding
```

등을 적용할 수 있다.

그리고:

```text
Pipeline
+
ColumnTransformer
```

를 이용하면 전처리와 모델을 하나의 일관된 흐름으로 관리할 수 있다.

가장 중요한 원칙은:

```text
Test Data는
학습의 기준을 만드는 데 사용하지 않는다.
```

이다.