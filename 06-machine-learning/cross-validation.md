# Cross Validation 교차 검증

## 1. Cross Validation이란?

Cross Validation은 데이터를 여러 부분으로 나누어 학습과 검증을 반복하는 방법이다.

한국어로:

```text
Cross Validation
→ 교차 검증
```

이라고 한다.

가장 중요한 목적은:

```text
모델의 성능을
한 번의 데이터 분할만으로 판단하지 않는 것
```

이다.

---

# 2. 기존 Train / Validation 방식

우리는 앞에서 데이터를:

```text
전체 데이터
   ↓
Train
Validation
Test
```

로 나누었다.

예:

```text
Train      60%
Validation 20%
Test       20%
```

Train으로 학습하고:

```text
Validation
→ 모델 비교

Test
→ 최종 평가
```

를 수행한다.

---

# 3. 그런데 한 번만 나누면?

예를 들어 데이터 100개가 있다고 하자.

```text
80개 → Train
20개 → Validation
```

이라고 나눴다.

그런데 어떤 20개가 Validation에 들어갔느냐에 따라 성능이 달라질 수 있다.

---

# 4. 예

첫 번째 Split:

```text
Accuracy = 0.92
```

다른 Random Split:

```text
Accuracy = 0.84
```

또 다른 Split:

```text
Accuracy = 0.88
```

이런 일이 발생할 수 있다.

---

# 5. 왜 결과가 달라질까?

각 Split에 포함되는 Sample이 다르기 때문이다.

특히 데이터가 적으면:

```text
어떤 Sample이 Train에 들어갔는가?

어떤 Sample이 Validation에 들어갔는가?
```

에 따라 결과 차이가 커질 수 있다.

---

# 6. Cross Validation의 아이디어

한 번만 Validation을 만드는 대신:

```text
여러 번 Train / Validation 역할을 바꿔가며 평가
```

한다.

대표적인 방법이:

```text
K-Fold Cross Validation
```

이다.

---

# 7. K-Fold란?

전체 데이터를 K개의 부분으로 나눈다.

각 부분을:

```text
Fold
```

라고 한다.

예:

```text
K = 5
```

라면:

```text
Fold 1
Fold 2
Fold 3
Fold 4
Fold 5
```

로 나눈다.

---

# 8. 5-Fold Cross Validation

첫 번째 학습:

```text
Validation
   ↓
Fold 1

Train
   ↓
Fold 2
Fold 3
Fold 4
Fold 5
```

두 번째:

```text
Validation
   ↓
Fold 2

Train
   ↓
Fold 1
Fold 3
Fold 4
Fold 5
```

이런 식으로 반복한다.

---

# 9. 전체 구조

```text
Fold 1 → Validation
Fold 2~5 → Train

Fold 2 → Validation
Fold 1,3,4,5 → Train

Fold 3 → Validation
Fold 1,2,4,5 → Train

Fold 4 → Validation
Fold 1,2,3,5 → Train

Fold 5 → Validation
Fold 1~4 → Train
```

결과적으로:

```text
5번 학습
+
5번 검증
```

한다.

---

# 10. 각 Sample의 역할

5-Fold Cross Validation에서는 일반적으로 각 Sample이:

```text
4번 → Train

1번 → Validation
```

에 사용된다.

즉 모든 Sample이 한 번씩 Validation 역할을 하게 된다.

---

# 11. Cross Validation 결과

예:

```text
Fold 1 Accuracy = 0.90
Fold 2 Accuracy = 0.87
Fold 3 Accuracy = 0.91
Fold 4 Accuracy = 0.88
Fold 5 Accuracy = 0.89
```

이 결과를 이용하여:

```text
평균 성능

성능 변동성
```

을 확인한다.

---

# 12. 평균

```text
Mean Accuracy
=
(0.90 + 0.87 + 0.91 + 0.88 + 0.89) / 5
```

결과:

```text
0.89
```

이다.

---

# 13. 평균만 보면 충분할까?

아니다.

예를 들어:

모델 A:

```text
0.89
0.90
0.88
0.90
0.89
```

모델 B:

```text
0.70
0.99
0.72
0.98
0.96
```

평균만 비슷하더라도 안정성은 크게 다를 수 있다.

---

# 14. 표준편차

그래서 Fold Score의:

```text
Standard Deviation
```

도 확인한다.

예:

```python
print(
    scores.mean()
)

print(
    scores.std()
)
```

---

# 15. 해석

예:

```text
CV Accuracy
= 0.89 ± 0.01
```

이면 Fold마다 비교적 비슷한 성능을 냈다는 뜻이다.

반면:

```text
0.89 ± 0.15
```

라면 Fold에 따라 성능 차이가 크다는 뜻이다.

---

# 16. KFold

Scikit-learn:

```python
from sklearn.model_selection import (
    KFold
)
```

---

# 17. KFold 생성

```python
kf = KFold(
    n_splits=5,
    shuffle=True,
    random_state=42
)
```

---

# 18. n_splits

```python
n_splits=5
```

는:

```text
데이터를 5개의 Fold로 나눈다.
```

라는 뜻이다.

---

# 19. shuffle

```python
shuffle=True
```

는 Fold를 만들기 전에 데이터를 섞는다는 뜻이다.

---

# 20. random_state

```python
random_state=42
```

는 Random Split 결과를 재현할 수 있도록 한다.

즉 같은 조건에서 다시 실행했을 때 동일한 Fold를 얻기 쉽게 한다.

---

# 21. random_state=42의 의미

42 자체가 특별히 성능이 좋은 숫자는 아니다.

```text
재현 가능한 Randomness
```

를 만들기 위한 Seed 값이다.

예:

```python
random_state=1
```

도 가능하고:

```python
random_state=100
```

도 가능하다.

---

# 22. KFold split()

```python
for train_idx, valid_idx in kf.split(X):

    print(
        train_idx
    )

    print(
        valid_idx
    )
```

---

# 23. train_idx와 valid_idx

`train_idx`:

```text
이번 Fold에서
Training에 사용할 Row 번호
```

`valid_idx`:

```text
이번 Fold에서
Validation에 사용할 Row 번호
```

이다.

---

# 24. Pandas에서 사용

DataFrame이라면:

```python
X_train = X.iloc[
    train_idx
]

X_valid = X.iloc[
    valid_idx
]
```

처럼 사용한다.

---

# 25. y도 동일한 Index 사용

```python
y_train = y.iloc[
    train_idx
]

y_valid = y.iloc[
    valid_idx
]
```

이다.

X와 y는 반드시 같은 Sample 기준으로 나뉘어야 한다.

---

# 26. 기본 KFold 코드

```python
from sklearn.model_selection import (
    KFold
)

from sklearn.ensemble import (
    RandomForestRegressor
)

from sklearn.metrics import (
    mean_absolute_error
)


kf = KFold(
    n_splits=5,
    shuffle=True,
    random_state=42
)


for fold, (
    train_idx,
    valid_idx
) in enumerate(
    kf.split(X),
    start=1
):

    X_train = X.iloc[
        train_idx
    ]

    X_valid = X.iloc[
        valid_idx
    ]

    y_train = y.iloc[
        train_idx
    ]

    y_valid = y.iloc[
        valid_idx
    ]


    model = RandomForestRegressor(
        n_estimators=200,
        random_state=42
    )


    model.fit(
        X_train,
        y_train
    )


    pred = model.predict(
        X_valid
    )


    mae = mean_absolute_error(
        y_valid,
        pred
    )


    print(
        f"Fold {fold} MAE:",
        mae
    )
```

---

# 27. 왜 Fold마다 새 모델을 만드는가?

각 Fold는 서로 다른 Training Data를 사용한다.

따라서:

```text
Fold마다 새로운 모델을 생성
→ 해당 Fold의 Train Data로 학습
```

하는 것이 명확하고 안전하다.

---

# 28. Classification에서는?

Classification에서는 Class 비율이 중요하다.

예:

```text
0 = 90%
1 = 10%
```

인 데이터가 있다고 하자.

일반 KFold에서는 Fold마다 Class 비율이 크게 달라질 수 있다.

---

# 29. 문제 예

전체:

```text
Class 0 = 90
Class 1 = 10
```

그런데 어떤 Validation Fold가:

```text
Class 0 = 20
Class 1 = 0
```

처럼 만들어진다면 제대로 평가하기 어렵다.

---

# 30. StratifiedKFold

이런 문제를 줄이기 위해 Classification에서 자주 사용하는 것이:

```text
StratifiedKFold
```

이다.

---

# 31. Stratified 의미

각 Fold의 Class 비율이 전체 데이터의 Class 비율과 비슷하도록 나눈다.

전체:

```text
0 = 90%
1 = 10%
```

각 Fold도 가능하면:

```text
0 ≈ 90%
1 ≈ 10%
```

이 되도록 한다.

---

# 32. StratifiedKFold Import

```python
from sklearn.model_selection import (
    StratifiedKFold
)
```

---

# 33. 생성

```python
skf = StratifiedKFold(
    n_splits=5,
    shuffle=True,
    random_state=42
)
```

---

# 34. 중요한 차이

KFold:

```python
kf.split(
    X
)
```

StratifiedKFold:

```python
skf.split(
    X,
    y
)
```

이다.

왜냐하면 StratifiedKFold는:

```text
y의 Class 비율
```

을 확인해야 하기 때문이다.

---

# 35. Classification 기본 코드

```python
from sklearn.model_selection import (
    StratifiedKFold
)

from sklearn.ensemble import (
    RandomForestClassifier
)

from sklearn.metrics import (
    accuracy_score
)


skf = StratifiedKFold(
    n_splits=5,
    shuffle=True,
    random_state=42
)


for fold, (
    train_idx,
    valid_idx
) in enumerate(
    skf.split(
        X,
        y
    ),
    start=1
):

    X_train = X.iloc[
        train_idx
    ]

    X_valid = X.iloc[
        valid_idx
    ]

    y_train = y.iloc[
        train_idx
    ]

    y_valid = y.iloc[
        valid_idx
    ]


    model = RandomForestClassifier(
        n_estimators=200,
        random_state=42
    )


    model.fit(
        X_train,
        y_train
    )


    pred = model.predict(
        X_valid
    )


    accuracy = accuracy_score(
        y_valid,
        pred
    )


    print(
        f"Fold {fold} Accuracy:",
        accuracy
    )
```

---

# 36. KFold vs StratifiedKFold

```text
KFold
→ 데이터를 K개 Fold로 나눔

StratifiedKFold
→ Class 비율을 고려하여
  K개 Fold로 나눔
```

이다.

---

# 37. 언제 무엇을 사용할까?

일반적인 기준:

```text
Regression
→ KFold

Classification
→ StratifiedKFold
```

로 시작할 수 있다.

하지만 데이터 구조에 따라 다른 Split 방법이 필요할 수 있다.

---

# 38. cross_val_score()

Scikit-learn에서는 직접 Loop를 만들지 않고 간단하게 Cross Validation을 실행할 수도 있다.

```python
from sklearn.model_selection import (
    cross_val_score
)
```

---

# 39. Classification 예

```python
scores = cross_val_score(
    model,
    X,
    y,
    cv=5,
    scoring="accuracy"
)
```

---

# 40. 결과

예:

```text
[0.90 0.87 0.91 0.88 0.89]
```

각 숫자가 Fold별 Score이다.

---

# 41. 평균

```python
print(
    "Mean:",
    scores.mean()
)
```

---

# 42. 표준편차

```python
print(
    "Std:",
    scores.std()
)
```

---

# 43. 한 번에 출력

```python
print(
    "CV Scores:",
    scores
)

print(
    "CV Mean:",
    scores.mean()
)

print(
    "CV Std:",
    scores.std()
)
```

---

# 44. cv=5의 의미

```python
cv=5
```

처럼 정수만 넣으면 Scikit-learn이 estimator와 target 유형에 따라 기본 splitter를 선택한다.

하지만 실제 프로젝트에서는 분할 방식을 명확하게 제어하기 위해:

```python
cv=skf
```

처럼 직접 만든 Splitter를 전달할 수 있다.

---

# 45. 직접 StratifiedKFold 전달

```python
skf = StratifiedKFold(
    n_splits=5,
    shuffle=True,
    random_state=42
)


scores = cross_val_score(
    model,
    X,
    y,
    cv=skf,
    scoring="accuracy"
)
```

---

# 46. ROC-AUC 평가

```python
scores = cross_val_score(
    model,
    X,
    y,
    cv=skf,
    scoring="roc_auc"
)
```

---

# 47. F1 평가

```python
scores = cross_val_score(
    model,
    X,
    y,
    cv=skf,
    scoring="f1"
)
```

---

# 48. Metric에 따라 결과가 다르다

같은 모델이라도:

```text
Accuracy

F1

ROC-AUC

Recall

Precision
```

값은 서로 다르다.

따라서 문제 목적에 맞는 Metric을 사용해야 한다.

---

# 49. Regression Cross Validation

예:

```python
scores = cross_val_score(
    model,
    X,
    y,
    cv=5,
    scoring="neg_mean_absolute_error"
)
```

---

# 50. 왜 Negative?

Scikit-learn의 Scoring 시스템은 기본적으로:

```text
값이 클수록 좋은 Score
```

방향으로 통일되어 있다.

하지만 MAE는:

```text
작을수록 좋음
```

이다.

그래서:

```text
Negative MAE
```

형태로 반환한다.

---

# 51. 실제 MAE로 바꾸기

```python
mae_scores = -scores
```

이제:

```python
print(
    mae_scores
)
```

하면 실제 MAE 형태로 볼 수 있다.

---

# 52. 평균 MAE

```python
print(
    "Mean MAE:",
    mae_scores.mean()
)
```

---

# 53. RMSE Cross Validation

환경의 Scikit-learn 버전이 지원한다면:

```python
scores = cross_val_score(
    model,
    X,
    y,
    cv=5,
    scoring="neg_root_mean_squared_error"
)
```

를 사용할 수 있다.

실제 RMSE:

```python
rmse_scores = -scores
```

이다.

---

# 54. Cross Validation과 Test는 다르다

중요하다.

```text
Cross Validation
→ 모델 개발 / 비교 / 선택

Test
→ 마지막 최종 평가
```

이다.

Cross Validation을 한다고 Test가 필요 없어지는 것은 아니다.

---

# 55. 올바른 큰 구조

```text
전체 데이터
      ↓
 ┌────┴────┐
 ↓         ↓
Train      Test
 ↓
Cross Validation
 ↓
모델 선택
Hyperparameter 선택
 ↓
최종 모델
 ↓
Test 평가
```

이다.

---

# 56. Test를 먼저 분리

매우 중요한 방법 중 하나:

```text
전체 데이터
      ↓
Train/Test 분리
      ↓
Train에서만 Cross Validation
      ↓
모델 선택
      ↓
Test 최종 평가
```

이다.

---

# 57. 왜 Test까지 CV에 넣으면 안 될까?

Test는:

```text
마지막까지 보지 않은 데이터
```

역할을 해야 한다.

Test까지 모델 선택 과정에 사용하면:

```text
Test Leakage
```

가 발생할 수 있다.

---

# 58. Cross Validation과 Preprocessing

매우 중요하다.

잘못된 예:

```text
전체 데이터
↓
StandardScaler fit
↓
Cross Validation
```

이렇게 하면 Validation Fold의 정보가 Scaling에 사용될 수 있다.

---

# 59. 올바른 방식

각 Fold에서:

```text
Fold Train
↓
Scaler fit

Fold Validation
↓
Scaler transform
```

이 되어야 한다.

---

# 60. 직접 구현한다면

```python
for train_idx, valid_idx in skf.split(
    X,
    y
):

    X_train = X.iloc[
        train_idx
    ]

    X_valid = X.iloc[
        valid_idx
    ]


    scaler = StandardScaler()


    X_train_scaled = (
        scaler.fit_transform(
            X_train
        )
    )


    X_valid_scaled = (
        scaler.transform(
            X_valid
        )
    )
```

처럼 각 Fold 내부에서 처리해야 한다.

---

# 61. Pipeline이 중요한 이유

더 안전하게:

```python
from sklearn.pipeline import (
    Pipeline
)

from sklearn.preprocessing import (
    StandardScaler
)

from sklearn.linear_model import (
    LogisticRegression
)


pipeline = Pipeline(
    steps=[
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
    ]
)
```

---

# 62. Pipeline + Cross Validation

```python
scores = cross_val_score(
    pipeline,
    X,
    y,
    cv=skf,
    scoring="roc_auc"
)
```

이렇게 하면 각 Fold에서:

```text
Scaler fit
↓
Train transform
↓
Validation transform
↓
Model fit
↓
Validation 평가
```

가 독립적으로 수행된다.

---

# 63. Missing Value 처리도 같다

잘못된 예:

```text
전체 데이터 Median 계산
↓
결측치 채움
↓
CV
```

Validation 정보가 Median 계산에 포함된다.

---

# 64. 올바른 방식

각 Fold에서:

```text
Train Fold
↓
Median 학습

Validation Fold
↓
Train Median으로 변환
```

해야 한다.

---

# 65. Imputer Pipeline

```python
from sklearn.impute import (
    SimpleImputer
)

from sklearn.pipeline import (
    Pipeline
)


pipeline = Pipeline(
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
        ),
        (
            "model",
            LogisticRegression(
                max_iter=1000
            )
        )
    ]
)
```

---

# 66. Cross Validation Leakage

다음은 모두 주의해야 한다.

```text
전체 데이터에서 결측치 통계 계산

전체 데이터 Scaling

전체 데이터 Feature Selection

전체 데이터에서 Target Encoding

전체 데이터 SMOTE

전체 데이터에서 Outlier 기준 계산
```

후에 Cross Validation을 하면 Validation Fold 정보가 학습 과정에 들어갈 수 있다.

---

# 67. SMOTE는 특히 주의

잘못된 구조:

```text
전체 데이터
↓
SMOTE
↓
Cross Validation
```

이렇게 하면 Fold 간 정보가 섞일 수 있다.

---

# 68. 올바른 SMOTE 구조

```text
Fold Train
↓
SMOTE
↓
Model 학습

Fold Validation
↓
원본 그대로 평가
```

이다.

SMOTE는:

```text
Training Fold에만
```

적용해야 한다.

---

# 69. Imbalanced-learn Pipeline

SMOTE 같은 Sampling까지 Pipeline에 넣을 때는 일반 `sklearn.pipeline.Pipeline` 대신 `imblearn.pipeline.Pipeline`을 사용하는 경우가 많다.

예:

```python
from imblearn.pipeline import (
    Pipeline
)

from imblearn.over_sampling import (
    SMOTE
)


pipeline = Pipeline(
    steps=[
        (
            "scaler",
            StandardScaler()
        ),
        (
            "smote",
            SMOTE(
                random_state=42
            )
        ),
        (
            "model",
            LogisticRegression(
                max_iter=1000
            )
        )
    ]
)
```

---

# 70. Pipeline + SMOTE + CV

```python
scores = cross_val_score(
    pipeline,
    X,
    y,
    cv=skf,
    scoring="f1"
)
```

이렇게 하면 SMOTE가 각 Training Fold 안에서 수행된다.

---

# 71. StratifiedKFold도 완벽하지 않다

Class 비율을 맞춰준다고 해서 모든 문제가 해결되는 것은 아니다.

예를 들어 한 환자에게 여러 Row가 있는 의료 데이터라면:

```text
같은 환자의 Row
→ Train에도 존재

같은 환자의 다른 Row
→ Validation에도 존재
```

할 수 있다.

---

# 72. Patient Leakage

예:

```text
Patient A
→ 검사 1
→ 검사 2
→ 검사 3
```

Random Split 후:

```text
검사 1, 2 → Train

검사 3 → Validation
```

이 되면 모델이 같은 환자의 특성을 이미 학습했을 수 있다.

---

# 73. GroupKFold

이런 경우:

```text
GroupKFold
```

를 고려한다.

Scikit-learn:

```python
from sklearn.model_selection import (
    GroupKFold
)
```

---

# 74. Group의 의미

예:

```text
patient_id
```

를 Group으로 사용한다.

같은 Group은:

```text
Train과 Validation에 동시에 들어가지 않도록
```

분리한다.

---

# 75. GroupKFold 예

```python
groups = df[
    "patient_id"
]


gkf = GroupKFold(
    n_splits=5
)


for train_idx, valid_idx in gkf.split(
    X,
    y,
    groups=groups
):

    X_train = X.iloc[
        train_idx
    ]

    X_valid = X.iloc[
        valid_idx
    ]
```

---

# 76. GroupKFold 핵심

```text
Patient A
→ 한 Fold에만 존재

Patient B
→ 다른 Fold에 존재
```

하도록 만들어 같은 환자가 Train과 Validation에 동시에 들어가는 문제를 방지한다.

---

# 77. StratifiedGroupKFold

Classification이면서:

```text
Class 비율도 중요하고

Group도 분리해야 한다.
```

면 Scikit-learn 버전에 따라:

```text
StratifiedGroupKFold
```

를 사용할 수 있다.

---

# 78. 예

```python
from sklearn.model_selection import (
    StratifiedGroupKFold
)


sgkf = StratifiedGroupKFold(
    n_splits=5,
    shuffle=True,
    random_state=42
)
```

---

# 79. 사용

```python
for train_idx, valid_idx in sgkf.split(
    X,
    y,
    groups=groups
):

    ...
```

이 방식은:

```text
Class 비율
+
Group 분리
```

를 함께 고려한다.

---

# 80. Time Series는?

시간 순서가 있는 데이터에서는 Random KFold를 사용하면 미래 데이터가 과거 예측에 사용될 수 있다.

예:

```text
2023
2024
2025
2026
```

데이터가 있다고 하자.

---

# 81. 잘못된 경우

Random KFold 때문에:

```text
2026 → Train

2024 → Validation
```

이 될 수 있다.

미래 정보를 이용해 과거를 평가하게 되는 셈이다.

---

# 82. TimeSeriesSplit

시간 데이터에는:

```python
from sklearn.model_selection import (
    TimeSeriesSplit
)
```

을 고려할 수 있다.

---

# 83. TimeSeriesSplit 구조

개념적으로:

```text
Train: 1
Valid: 2

Train: 1,2
Valid: 3

Train: 1,2,3
Valid: 4
```

처럼 과거로 학습하고 이후 시점을 검증한다.

---

# 84. TimeSeriesSplit 예

```python
tscv = TimeSeriesSplit(
    n_splits=5
)


for train_idx, valid_idx in tscv.split(
    X
):

    X_train = X.iloc[
        train_idx
    ]

    X_valid = X.iloc[
        valid_idx
    ]
```

---

# 85. Split 방법 선택

```text
일반 Regression
→ KFold

일반 Classification
→ StratifiedKFold

같은 사람/고객/장비의 반복 데이터
→ GroupKFold

Class 비율 + Group
→ StratifiedGroupKFold

시간 순서 데이터
→ TimeSeriesSplit
```

처럼 생각할 수 있다.

---

# 86. 데이터 구조가 CV 방법을 결정한다

무조건:

```text
5-Fold StratifiedKFold
```

가 정답인 것은 아니다.

먼저 데이터가:

```text
독립적인 Row인가?

같은 환자의 반복 측정인가?

시간 순서가 있는가?

Class 불균형이 있는가?
```

를 확인한다.

---

# 87. Cross Validation과 Hyperparameter Tuning

예를 들어 Random Forest의:

```text
max_depth
```

를 선택한다고 하자.

후보:

```text
3
5
10
None
```

각 후보를 Cross Validation으로 평가할 수 있다.

---

# 88. 개념

```text
max_depth=3
→ 5-Fold CV
→ 평균 Score

max_depth=5
→ 5-Fold CV
→ 평균 Score

max_depth=10
→ 5-Fold CV
→ 평균 Score
```

비교 후 가장 적절한 값을 선택한다.

---

# 89. 이것이 Tuning과 연결된다

이 과정을 자동화하는 대표적인 방법:

```text
GridSearchCV

RandomizedSearchCV

Bayesian Optimization

Optuna
```

등이 있다.

다음 파일:

```text
hyperparameter-tuning.md
```

에서 자세히 다룬다.

---

# 90. Cross Validation과 OOF

Cross Validation은 단순히 Score를 계산하는 데만 사용하는 것이 아니다.

각 Validation Fold의 예측값을 저장하면:

```text
OOF Prediction
```

을 만들 수 있다.

---

# 91. OOF

OOF:

```text
Out-Of-Fold
```

이다.

각 Sample이:

```text
자기 자신을 학습하지 않은 모델
```

에게서 예측을 받는다.

---

# 92. OOF 배열 만들기

```python
import numpy as np


oof = np.zeros(
    len(X)
)
```

---

# 93. Fold별 OOF 저장

```python
for train_idx, valid_idx in skf.split(
    X,
    y
):

    X_train = X.iloc[
        train_idx
    ]

    X_valid = X.iloc[
        valid_idx
    ]

    y_train = y.iloc[
        train_idx
    ]


    model.fit(
        X_train,
        y_train
    )


    valid_prob = model.predict_proba(
        X_valid
    )[:, 1]


    oof[
        valid_idx
    ] = valid_prob
```

---

# 94. 반복이 끝나면

```text
oof[0]
oof[1]
oof[2]
...
```

모든 Training Sample에 대해 예측값이 채워진다.

각 값은 해당 Sample을 학습에 사용하지 않은 Fold Model이 만든 예측이다.

---

# 95. OOF 성능

```python
from sklearn.metrics import (
    roc_auc_score
)


oof_auc = roc_auc_score(
    y,
    oof
)


print(
    "OOF ROC-AUC:",
    oof_auc
)
```

---

# 96. OOF가 중요한 이유

OOF Prediction은:

```text
전체 Training Data에 대한
비교적 공정한 예측값
```

을 만들 수 있다.

그리고:

```text
Stacking

Blending

Error Analysis
```

등에 활용할 수 있다.

---

# 97. Test Prediction은 어떻게 만들까?

5-Fold CV에서 각 Fold마다 Test Data를 예측할 수 있다.

```text
Fold 1 Model → Test Prediction

Fold 2 Model → Test Prediction

Fold 3 Model → Test Prediction

Fold 4 Model → Test Prediction

Fold 5 Model → Test Prediction
```

---

# 98. Test Prediction 평균

각 Fold Model의 Test Prediction을 평균한다.

```text
Final Test Prediction
=
5개 Fold Prediction 평균
```

이다.

---

# 99. 코드

```python
test_pred = np.zeros(
    len(X_test)
)


for train_idx, valid_idx in skf.split(
    X,
    y
):

    X_train = X.iloc[
        train_idx
    ]

    y_train = y.iloc[
        train_idx
    ]


    model.fit(
        X_train,
        y_train
    )


    fold_test_prob = (
        model.predict_proba(
            X_test
        )[:, 1]
    )


    test_pred += (
        fold_test_prob
        /
        skf.n_splits
    )
```

---

# 100. 왜 평균을 내는가?

Fold마다 학습한 데이터가 조금씩 다르다.

따라서 Test Prediction도 조금씩 다를 수 있다.

이를 평균하여:

```text
여러 Fold Model의 예측을 결합
```

한다.

일종의 Ensemble 효과도 얻을 수 있다.

---

# 101. Competition에서 자주 보이는 구조

```text
Train Data
    ↓
5-Fold CV
    ↓
┌───────────────┐
│ OOF Prediction│
└───────────────┘
    +
┌───────────────────┐
│ Test Fold Predictions│
└───────────────────┘
    ↓
Test Prediction 평균
    ↓
submission.csv
```

---

# 102. Fold Loop 읽는 방법

다음 코드:

```python
for fold, (
    tr_idx,
    va_idx
) in enumerate(
    skf.split(
        X,
        y
    ),
    1
):
```

처음 보면 복잡하다.

부분별로 보면 쉽다.

---

# 103. skf.split(X, y)

```python
skf.split(
    X,
    y
)
```

가 매 Fold마다:

```text
Train Index

Validation Index
```

를 만들어 준다.

---

# 104. tr_idx

```text
tr
→ train
```

즉:

```text
tr_idx
→ Training Index
```

이다.

---

# 105. va_idx

```text
va
→ validation
```

즉:

```text
va_idx
→ Validation Index
```

이다.

---

# 106. enumerate(..., 1)

```python
enumerate(
    ...,
    1
)
```

을 사용하면 Fold 번호를:

```text
1
2
3
4
5
```

부터 받을 수 있다.

---

# 107. 그래서 전체 의미

```python
for fold, (
    tr_idx,
    va_idx
) in enumerate(
    skf.split(
        X,
        y
    ),
    1
):
```

은:

```text
각 Fold마다

Fold 번호와

Train Index

Validation Index

를 가져와서 반복한다.
```

라는 뜻이다.

---

# 108. 흔히 보는 출력

```python
print(
    f"\n===== Fold {fold}/5 ====="
)
```

결과:

```text
===== Fold 1/5 =====

===== Fold 2/5 =====

===== Fold 3/5 =====
...
```

이다.

---

# 109. Cross Validation 전체 분류 예제

```python
import numpy as np

from sklearn.model_selection import (
    StratifiedKFold
)

from sklearn.ensemble import (
    RandomForestClassifier
)

from sklearn.metrics import (
    accuracy_score,
    roc_auc_score
)


skf = StratifiedKFold(
    n_splits=5,
    shuffle=True,
    random_state=42
)


oof = np.zeros(
    len(X)
)


fold_scores = []


for fold, (
    train_idx,
    valid_idx
) in enumerate(
    skf.split(
        X,
        y
    ),
    start=1
):

    print(
        f"===== Fold {fold} ====="
    )


    X_train = X.iloc[
        train_idx
    ]

    X_valid = X.iloc[
        valid_idx
    ]

    y_train = y.iloc[
        train_idx
    ]

    y_valid = y.iloc[
        valid_idx
    ]


    model = RandomForestClassifier(
        n_estimators=200,
        random_state=42
    )


    model.fit(
        X_train,
        y_train
    )


    valid_pred = model.predict(
        X_valid
    )

    valid_prob = model.predict_proba(
        X_valid
    )[:, 1]


    oof[
        valid_idx
    ] = valid_prob


    accuracy = accuracy_score(
        y_valid,
        valid_pred
    )

    auc = roc_auc_score(
        y_valid,
        valid_prob
    )


    fold_scores.append(
        auc
    )


    print(
        "Accuracy:",
        accuracy
    )

    print(
        "ROC-AUC:",
        auc
    )


print(
    "CV Mean AUC:",
    np.mean(
        fold_scores
    )
)

print(
    "CV Std AUC:",
    np.std(
        fold_scores
    )
)


print(
    "OOF AUC:",
    roc_auc_score(
        y,
        oof
    )
)
```

---

# 110. Mean CV와 OOF Score는 같은가?

반드시 정확히 같지는 않다.

```text
Mean CV Score
```

는 각 Fold Score를 계산한 뒤 평균낸 것이다.

```text
OOF Score
```

는 모든 OOF Prediction을 합친 뒤 전체 Target과 한 번에 비교한 것이다.

특히 Fold 크기나 Metric 특성에 따라 차이가 날 수 있다.

---

# 111. Cross Validation의 장점

```text
한 번의 Split 의존성 감소

모든 Sample을 Validation에 활용

모델 안정성 확인

Hyperparameter 비교

OOF 생성

Stacking 활용
```

등이 있다.

---

# 112. Cross Validation의 단점

K-Fold 5개라면 모델을 최소:

```text
5번
```

학습해야 한다.

Hyperparameter 후보가 20개라면:

```text
20 × 5
=
100번
```

학습할 수 있다.

따라서 계산 비용이 증가한다.

---

# 113. Deep Learning에서는?

Deep Learning 모델은 학습 비용이 매우 크기 때문에 5-Fold CV가 부담스러울 수 있다.

따라서 상황에 따라:

```text
고정 Validation Set

적은 수의 Fold

Cross Validation 없이 여러 Seed 비교
```

등을 사용할 수도 있다.

---

# 114. 작은 데이터에서는?

데이터가 적으면 Cross Validation이 특히 유용할 수 있다.

한 번의 Validation Split으로 많은 데이터를 따로 빼두는 부담을 줄이면서 여러 번 평가할 수 있기 때문이다.

---

# 115. n_splits는 무조건 5?

아니다.

대표적으로:

```text
5-Fold

10-Fold
```

등을 사용할 수 있다.

하지만:

```text
데이터 크기

Class 개수

소수 Class Sample 수

학습 비용
```

을 고려해야 한다.

---

# 116. 소수 Class가 너무 적다면?

예:

```text
Class 1 = 3개
```

인데:

```text
n_splits=5
```

인 StratifiedKFold를 사용하려 하면 문제가 발생할 수 있다.

모든 Fold에 Class 1을 배치할 수 없기 때문이다.

---

# 117. 따라서 먼저 확인

```python
print(
    y.value_counts()
)
```

으로 각 Class의 Sample 수를 확인한다.

---

# 118. Cross Validation 결과 기록

예:

| Model | CV Mean AUC | CV Std |
|---|---:|---:|
| Logistic | 0.81 | 0.02 |
| Random Forest | 0.85 | 0.03 |
| XGBoost | 0.87 | 0.01 |

이렇게 기록하면 모델을 비교하기 쉽다.

---

# 119. 평균이 높은 모델만 선택?

항상 그렇지는 않다.

예:

```text
Model A
Mean = 0.88
Std = 0.01

Model B
Mean = 0.89
Std = 0.10
```

이라면 Model B가 평균은 조금 높지만 Fold에 따른 변동성이 매우 크다.

따라서 안정성도 함께 본다.

---

# 120. Cross Validation과 Ensemble

예:

```text
Random Forest CV

XGBoost CV

LightGBM CV

CatBoost CV
```

를 수행하고 각 모델의 OOF Prediction을 만들 수 있다.

---

# 121. 모델별 OOF

```text
RF OOF

XGB OOF

LGBM OOF

CatBoost OOF
```

이렇게 만들어진 OOF Prediction을:

```text
Stacking Meta Features
```

로 사용할 수 있다.

---

# 122. OOF Stacking 구조

```text
Training Data
     ↓
┌────┼────┐
↓    ↓    ↓
RF   XGB  LGBM
↓    ↓    ↓
OOF  OOF  OOF
└────┼────┘
     ↓
Meta Data
     ↓
Meta Model
     ↓
Final Prediction
```

---

# 123. Nested Cross Validation

더 엄격한 평가가 필요한 경우:

```text
Nested Cross Validation
```

을 사용할 수도 있다.

구조:

```text
Outer CV
→ 최종 성능 평가

Inner CV
→ Hyperparameter Tuning
```

이다.

---

# 124. 왜 Nested CV가 필요한가?

같은 CV 결과를 계속 보면서 Hyperparameter를 선택하면 그 Validation 과정 자체에 점점 맞춰질 수 있다.

Nested CV에서는:

```text
모델 선택 과정

최종 평가 과정
```

을 더 엄격하게 분리한다.

---

# 125. Nested CV 구조

```text
Outer Fold
    ↓
Outer Train
    ↓
Inner Cross Validation
    ↓
Hyperparameter 선택
    ↓
Outer Validation 평가
```

이 과정을 Outer Fold마다 반복한다.

---

# 126. 일반 프로젝트에서 항상 필요한가?

아니다.

Nested CV는 계산 비용이 크다.

따라서:

```text
데이터가 작음

엄격한 성능 추정이 중요함

연구 목적

Hyperparameter Tuning의 Bias를 줄이고 싶음
```

같은 상황에서 고려할 수 있다.

---

# 127. 의료 데이터에서 특히 중요한 점

의료 데이터에서는 단순 Random Split보다:

```text
환자 단위

병원 단위

시간 단위

촬영 단위
```

등 실제 서비스 상황을 고려한 Split이 중요할 수 있다.

---

# 128. 같은 환자의 이미지 여러 장

예:

```text
Patient A

image_01
image_02
image_03
```

이 있다고 하자.

잘못 나누면:

```text
image_01 → Train

image_02 → Validation
```

이 될 수 있다.

모델이 같은 환자의 특징을 이미 봤을 가능성이 있다.

---

# 129. 이 경우

이미지 단위 Random Split보다:

```text
Patient-level Split
```

이 더 적절할 수 있다.

즉:

```text
Patient A 전체 → Train

Patient B 전체 → Validation
```

처럼 나눈다.

---

# 130. 외부 검증

의료 AI에서는 가능하다면:

```text
다른 병원

다른 장비

다른 기간

다른 환자 집단
```

의 데이터로 평가하는 External Validation도 중요하다.

Cross Validation 성능이 높다고 실제 모든 환경에서 같은 성능이 보장되는 것은 아니다.

---

# 131. Cross Validation 체크리스트

Cross Validation을 하기 전에:

```text
1. Classification인가 Regression인가?

2. Class Imbalance가 있는가?

3. 같은 환자/고객의 반복 Row가 있는가?

4. 시간 순서가 있는가?

5. Preprocessing이 Fold 내부에서 수행되는가?

6. SMOTE가 Train Fold에만 적용되는가?

7. Test Data를 모델 선택에 사용하지 않았는가?

8. 평가 Metric이 문제 목적에 맞는가?

9. Mean뿐 아니라 Std도 확인했는가?

10. Leakage 가능성을 확인했는가?
```

를 점검한다.

---

# 132. Split 방법 정리

```text
KFold
→ 일반적인 K-Fold

StratifiedKFold
→ Classification Class 비율 유지

GroupKFold
→ 같은 Group 분리

StratifiedGroupKFold
→ Class 비율 + Group 분리

TimeSeriesSplit
→ 시간 순서 유지
```

---

# 133. Cross Validation 핵심 코드

Classification:

```python
skf = StratifiedKFold(
    n_splits=5,
    shuffle=True,
    random_state=42
)


for train_idx, valid_idx in skf.split(
    X,
    y
):

    X_train = X.iloc[
        train_idx
    ]

    X_valid = X.iloc[
        valid_idx
    ]

    y_train = y.iloc[
        train_idx
    ]

    y_valid = y.iloc[
        valid_idx
    ]
```

---

# 134. 가장 중요한 그림

```text
전체 Train Data
       ↓
┌──────┬──────┬──────┬──────┬──────┐
│ F1   │ F2   │ F3   │ F4   │ F5   │
└──────┴──────┴──────┴──────┴──────┘

1회차
Valid  Train  Train  Train  Train

2회차
Train  Valid  Train  Train  Train

3회차
Train  Train  Valid  Train  Train

4회차
Train  Train  Train  Valid  Train

5회차
Train  Train  Train  Train  Valid
```

---

# 135. 배운 점

Cross Validation은:

```text
한 번의 Train / Validation Split에만
의존하지 않고

여러 Fold에서 반복해서
모델을 평가하는 방법
```

이다.

대표적으로:

```text
KFold
StratifiedKFold
GroupKFold
StratifiedGroupKFold
TimeSeriesSplit
```

등이 있다.

Classification에서는:

```text
StratifiedKFold
```

를 자주 사용하고 Regression에서는:

```text
KFold
```

를 기본적으로 고려할 수 있다.

하지만 더 중요한 것은:

```text
데이터 구조에 맞는 Split 방법을 선택하는 것
```

이다.

또한 Cross Validation을 할 때:

```text
Scaling

Imputation

Feature Selection

Sampling
```

등의 학습되는 전처리는 각 Training Fold 내부에서 수행해야 한다.

그리고:

```text
OOF Prediction
```

을 만들면 각 Sample에 대해 자신을 학습하지 않은 모델의 예측값을 얻을 수 있다.

이 OOF는 나중에:

```text
Stacking
```

에서 매우 중요하게 사용된다.

가장 중요한 원칙은:

```text
Validation Data의 정보가
Training 과정으로 들어가지 않도록 하는 것
```

이다.