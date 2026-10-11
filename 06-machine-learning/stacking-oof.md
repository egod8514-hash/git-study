# Stacking & OOF

## 1. Stacking이란?

Stacking은 여러 Machine Learning 모델의 예측 결과를 다시 하나의 새로운 모델에 입력하여 최종 예측을 만드는 Ensemble 방법이다.

기본 구조:

```text
Original Features
       ↓
 ┌─────┼─────┐
 ↓     ↓     ↓
Model A Model B Model C
 ↓     ↓     ↓
예측 A  예측 B  예측 C
 └─────┼─────┘
       ↓
   Meta Model
       ↓
  Final Prediction
```

즉:

```text
모델의 예측을
다른 모델의 Feature로 사용한다.
```

---

# 2. Base Model

Stacking의 첫 번째 단계에서 사용하는 모델들을:

```text
Base Model
```

이라고 한다.

예:

```text
Random Forest

XGBoost

LightGBM
```

을 Base Model로 사용할 수 있다.

---

# 3. Meta Model

Base Model의 Prediction을 입력으로 받아 최종 Prediction을 만드는 모델을:

```text
Meta Model
```

이라고 한다.

예:

```text
Base Models

Random Forest
XGBoost
LightGBM

       ↓

Prediction 3개

       ↓

Logistic Regression

       ↓

Final Prediction
```

여기서 Logistic Regression이 Meta Model이다.

---

# 4. 왜 Stacking을 사용할까?

모델마다 잘하는 부분이 다를 수 있기 때문이다.

예:

```text
Random Forest
→ Feature Interaction을 잘 잡음

XGBoost
→ Boosting 기반 Pattern 학습

LightGBM
→ 또 다른 Tree Boosting 방식
```

각 모델의 Error Pattern이 다르다면 Prediction을 조합했을 때 더 좋은 성능을 얻을 가능성이 있다.

---

# 5. 단순 평균과 Stacking 차이

단순 Ensemble:

```text
RF Probability = 0.70
XGB Probability = 0.80
LGBM Probability = 0.60

평균
=
0.70
```

이다.

모든 모델을 같은 비중으로 사용한다.

---

# 6. Stacking은 다르다

Stacking에서는 Meta Model이:

```text
RF Prediction

XGB Prediction

LGBM Prediction
```

을 보고 어떤 Prediction을 어떻게 조합할지 학습한다.

예:

```text
RF가 이런 상황에서는 더 믿을 만함

XGB는 저런 상황에서는 더 믿을 만함
```

같은 Pattern을 Meta Model이 학습할 수 있다.

---

# 7. 가장 단순한 Stacking 아이디어

처음에는 다음처럼 생각하기 쉽다.

```python
rf.fit(
    X_train,
    y_train
)

xgb.fit(
    X_train,
    y_train
)

lgb.fit(
    X_train,
    y_train
)
```

그리고:

```python
pred_rf = rf.predict_proba(
    X_train
)[:, 1]

pred_xgb = xgb.predict_proba(
    X_train
)[:, 1]

pred_lgb = lgb.predict_proba(
    X_train
)[:, 1]
```

이 Prediction을 Meta Model에 넣는 것이다.

---

# 8. 그런데 문제가 있다

Base Model은 이미:

```text
X_train
```

으로 학습했다.

그런데 다시:

```text
X_train
```

을 Prediction하고 있다.

즉:

```text
이미 본 데이터를 예측
```

하고 있는 것이다.

---

# 9. 왜 문제가 될까?

예를 들어 Random Forest가 Training Data를 매우 잘 외웠다고 하자.

Training Prediction:

```text
거의 정답
```

이 될 수 있다.

Meta Model은 이 Prediction을 보고:

```text
Random Forest는 엄청 정확하네.
```

라고 잘못 학습할 수 있다.

하지만 새로운 데이터에서는 같은 성능이 나오지 않을 수 있다.

---

# 10. Leakage와 Overfitting

이런 방식은 Meta Model이 Base Model의 지나치게 낙관적인 Training Prediction을 학습하게 만들 수 있다.

따라서 Stacking에서는 Base Model이 해당 Sample을 학습하지 않은 상태에서 만든 Prediction이 필요하다.

여기서 등장하는 것이:

```text
OOF Prediction
```

이다.

---

# 11. OOF란?

OOF:

```text
Out-Of-Fold
```

이다.

Cross Validation에서:

```text
현재 Fold의 Training에 포함되지 않은 Sample을 예측한 결과
```

를 의미한다.

---

# 12. OOF의 핵심

가장 중요한 문장:

```text
각 Sample은
자신을 학습하지 않은 모델에게
예측받는다.
```

이것이 OOF의 핵심이다.

---

# 13. 5-Fold 예

전체 Training Data가:

```text
1000개
```

라고 하자.

5-Fold:

```text
Fold 1 = 200
Fold 2 = 200
Fold 3 = 200
Fold 4 = 200
Fold 5 = 200
```

이다.

---

# 14. 첫 번째 Fold

```text
Train
=
Fold 2 + Fold 3 + Fold 4 + Fold 5

Validation
=
Fold 1
```

Model은 800개로 학습하고 Fold 1의 200개를 Prediction한다.

---

# 15. 두 번째 Fold

```text
Train
=
Fold 1 + Fold 3 + Fold 4 + Fold 5

Validation
=
Fold 2
```

다시 800개로 학습하고 Fold 2의 200개를 Prediction한다.

---

# 16. 반복

```text
Fold 1
→ 나머지 4개로 학습
→ Fold 1 예측

Fold 2
→ 나머지 4개로 학습
→ Fold 2 예측

Fold 3
→ 나머지 4개로 학습
→ Fold 3 예측

Fold 4
→ 나머지 4개로 학습
→ Fold 4 예측

Fold 5
→ 나머지 4개로 학습
→ Fold 5 예측
```

---

# 17. 모든 Prediction을 합친다

각 Fold Validation Prediction을 원래 위치에 넣으면:

```text
전체 1000개에 대한 Prediction
```

을 만들 수 있다.

이것이:

```text
OOF Prediction
```

이다.

---

# 18. 중요한 차이

일반 Training Prediction:

```text
Sample A
↓
Sample A를 포함해 학습한 Model
↓
Sample A Prediction
```

OOF Prediction:

```text
Sample A
↓
Sample A를 제외하고 학습한 Model
↓
Sample A Prediction
```

이다.

---

# 19. 왜 OOF가 더 공정할까?

OOF Prediction은 각 Sample에 대해:

```text
처음 보는 데이터처럼 예측
```

하도록 만들기 때문이다.

따라서 Training Prediction보다 Generalization 성능을 더 현실적으로 반영할 수 있다.

---

# 20. OOF 기본 구조

```text
전체 Train
   ↓
5 Fold
   ↓
Fold마다
   ↓
Train Fold로 Model 학습
   ↓
Validation Fold Prediction
   ↓
원래 위치에 저장
   ↓
모든 Fold 반복
   ↓
OOF Prediction 완성
```

---

# 21. Python OOF 배열

먼저 전체 Training Sample 수만큼 공간을 만든다.

```python
import numpy as np


oof = np.zeros(
    len(
        X
    )
)
```

예:

```text
X = 1000개

oof
=
[0, 0, 0, ..., 0]

길이 = 1000
```

---

# 22. StratifiedKFold

Classification이라면:

```python
from sklearn.model_selection import (
    StratifiedKFold
)


skf = StratifiedKFold(
    n_splits=5,
    shuffle=True,
    random_state=42
)
```

를 사용할 수 있다.

---

# 23. Fold 반복

```python
for fold, (
    train_idx,
    valid_idx
) in enumerate(
    skf.split(
        X,
        y
    ),
    1
):

    print(
        "Fold:",
        fold
    )
```

---

# 24. Index 의미

```text
train_idx
→ 현재 Fold에서 Training에 사용할 Row Index

valid_idx
→ 현재 Fold에서 Validation에 사용할 Row Index
```

이다.

---

# 25. Pandas 데이터 분리

```python
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

# 26. Model 학습

예:

```python
from sklearn.ensemble import (
    RandomForestClassifier
)


model = RandomForestClassifier(
    n_estimators=300,
    random_state=42,
    n_jobs=-1
)


model.fit(
    X_train,
    y_train
)
```

---

# 27. Validation Prediction

```python
valid_prob = model.predict_proba(
    X_valid
)[:, 1]
```

---

# 28. OOF 위치에 저장

가장 중요한 코드:

```python
oof[
    valid_idx
] = valid_prob
```

이다.

---

# 29. 왜 valid_idx에 넣을까?

예를 들어:

```text
valid_idx
=
[2, 7, 10]
```

이고 Prediction이:

```text
[0.2, 0.8, 0.4]
```

라면:

```text
oof[2] = 0.2

oof[7] = 0.8

oof[10] = 0.4
```

가 된다.

즉 Prediction을 원래 Sample 위치에 돌려놓는 것이다.

---

# 30. 모든 Fold가 끝나면

모든 Sample은 정확히 한 번씩 Validation에 포함되므로:

```text
oof 전체가 채워진다.
```

---

# 31. OOF 전체 코드

```python
import numpy as np

from sklearn.model_selection import (
    StratifiedKFold
)

from sklearn.ensemble import (
    RandomForestClassifier
)


skf = StratifiedKFold(
    n_splits=5,
    shuffle=True,
    random_state=42
)


oof = np.zeros(
    len(
        X
    )
)


for fold, (
    train_idx,
    valid_idx
) in enumerate(
    skf.split(
        X,
        y
    ),
    1
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
        n_estimators=300,
        random_state=42,
        n_jobs=-1
    )


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

# 32. OOF 성능 계산

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

# 33. OOF Score의 의미

```text
Training Data 전체를 대상으로 한 Score
```

처럼 보이지만 각 Sample은 자신을 학습하지 않은 Fold Model에게 Prediction을 받았다.

따라서 일반적인 Training Score와 다르다.

---

# 34. OOF는 Test Data인가?

아니다.

OOF는 여전히:

```text
Training Dataset 내부
```

에서 만들어진 Prediction이다.

다만 각 Sample에 대해 Out-of-Fold 방식으로 Prediction한 것이다.

---

# 35. 세 가지를 구분

```text
Training Prediction
→ 자신을 학습한 Model의 Prediction

OOF Prediction
→ 자신을 학습하지 않은 Fold Model의 Prediction

Test Prediction
→ Training Dataset 밖의 Test Data Prediction
```

이다.

---

# 36. 이 차이가 매우 중요하다

```text
OOF
≠
Test
```

이다.

OOF는 Train Data 안에서 생성된다.

---

# 37. Stacking에서는 왜 OOF를 쓸까?

Meta Model을 학습하기 위해:

```text
Base Model Prediction
```

이 필요하다.

그 Prediction을 Training Data 자체에서 그냥 만들면 Overfitting 문제가 발생할 수 있다.

그래서:

```text
OOF Prediction
```

을 Meta Feature로 사용한다.

---

# 38. Base Model이 3개라면

예:

```text
Random Forest

XGBoost

LightGBM
```

각각 OOF Prediction을 만든다.

결과:

```text
RF OOF

XGB OOF

LGBM OOF
```

---

# 39. Meta Dataset

원래 Training Data가 1000개라면:

```text
RF OOF
→ 1000개

XGB OOF
→ 1000개

LGBM OOF
→ 1000개
```

이다.

이를 옆으로 합친다.

---

# 40. Meta X

```python
meta_X = np.column_stack(
    [
        oof_rf,
        oof_xgb,
        oof_lgb
    ]
)
```

Shape:

```text
(1000, 3)
```

이다.

---

# 41. Meta Feature

기존에는 Feature가:

```text
Age

BMI

BloodPressure

...
```

였다.

Meta Model의 Feature는:

```text
RF Prediction

XGB Prediction

LGBM Prediction
```

이 된다.

---

# 42. Meta Target

Target은 원래 Target 그대로 사용한다.

```python
meta_y = y
```

---

# 43. Meta Model 학습

```python
from sklearn.linear_model import (
    LogisticRegression
)


meta_model = LogisticRegression(
    max_iter=1000
)


meta_model.fit(
    meta_X,
    meta_y
)
```

---

# 44. 여기까지가 Meta Model Training

구조:

```text
Original Train
       ↓
 ┌─────┼─────┐
 ↓     ↓     ↓
RF    XGB   LGBM
 ↓     ↓     ↓
OOF   OOF   OOF
 └─────┼─────┘
       ↓
    meta_X
       ↓
Logistic Regression
       ↓
Meta Model
```

---

# 45. 그런데 Test Data는 어떻게 예측할까?

여기서 매우 중요한 질문이 나온다.

Meta Model은 입력으로:

```text
RF Prediction
XGB Prediction
LGBM Prediction
```

3개를 요구한다.

따라서 Test Data도 동일한 3개의 Base Prediction이 필요하다.

---

# 46. Test Prediction 생성

각 Fold에서 Base Model은:

```text
Fold Training Data
```

로 학습한다.

그 Model로:

```text
Validation
```

뿐 아니라:

```text
External Test
```

도 Prediction한다.

---

# 47. 예

Fold 1:

```text
RF Model 1
→ Validation Fold 1 Prediction
→ Test Prediction 1
```

Fold 2:

```text
RF Model 2
→ Validation Fold 2 Prediction
→ Test Prediction 2
```

...

Fold 5:

```text
RF Model 5
→ Validation Fold 5 Prediction
→ Test Prediction 5
```

---

# 48. Test Prediction은 5개가 생긴다

Test Sample 하나에 대해:

```text
Fold 1 Model = 0.72
Fold 2 Model = 0.76
Fold 3 Model = 0.69
Fold 4 Model = 0.74
Fold 5 Model = 0.71
```

처럼 5개의 Prediction이 생긴다.

---

# 49. 평균

보통 이를 평균한다.

```text
(0.72 + 0.76 + 0.69 + 0.74 + 0.71)
/
5
```

---

# 50. 왜 평균할까?

각 Fold Model은 조금씩 다른 Training Data로 학습되었다.

따라서 여러 Fold Model의 Prediction을 평균하면 특정 Split에 대한 의존성을 줄이는 효과를 기대할 수 있다.

---

# 51. Test Prediction 저장 공간

```python
test_preds = np.zeros(
    (
        len(
            X_test
        ),
        5
    )
)
```

---

# 52. Fold별 Test Prediction

```python
test_preds[
    :,
    fold - 1
] = model.predict_proba(
    X_test
)[:, 1]
```

---

# 53. 평균

```python
test_pred = test_preds.mean(
    axis=1
)
```

---

# 54. 더 간단한 방식

처음부터 누적해서 평균할 수도 있다.

```python
test_pred = np.zeros(
    len(
        X_test
    )
)
```

Fold마다:

```python
test_pred += (
    model.predict_proba(
        X_test
    )[:, 1]
    /
    skf.n_splits
)
```

---

# 55. OOF + Test Prediction

```python
oof = np.zeros(
    len(
        X
    )
)


test_pred = np.zeros(
    len(
        X_test
    )
)


for fold, (
    train_idx,
    valid_idx
) in enumerate(
    skf.split(
        X,
        y
    ),
    1
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


    oof[
        valid_idx
    ] = model.predict_proba(
        X_valid
    )[:, 1]


    test_pred += (
        model.predict_proba(
            X_test
        )[:, 1]
        /
        skf.n_splits
    )
```

---

# 56. 왜 test.csv를 또 예측하는가?

Competition에서:

```text
test.csv
```

에는 보통 Target이 없다.

예:

```text
train.csv

Feature + Target
```

```text
test.csv

Feature만 존재
```

한다.

따라서 우리가 해야 하는 일은:

```text
Train Data로 Pattern 학습
↓
Test Data의 Target 예측
↓
Submission 생성
```

이다.

---

# 57. Test Data가 이미 있는데 왜 Test Prediction을 만든다는 말인가?

`test.csv`는:

```text
예측해야 할 입력 데이터
```

이다.

`Test Prediction`은:

```text
그 test.csv에 대해
Model이 만들어낸 예측값
```

이다.

둘은 다른 것이다.

---

# 58. 예

test.csv:

```text
ID     Age     BMI

1      35      22
2      51      28
3      64      31
```

Target이 없다.

Model Prediction:

```text
ID     Probability

1      0.12
2      0.63
3      0.88
```

이 Probability가 Test Prediction이다.

---

# 59. Submission

예:

```python
submission[
    "target"
] = test_pred
```

그리고:

```python
submission.to_csv(
    "submission.csv",
    index=False
)
```

한다.

---

# 60. Stacking에서는 Base Model마다 Test Prediction 필요

예:

```text
RF Test Prediction

XGB Test Prediction

LGBM Test Prediction
```

을 만든다.

---

# 61. Meta Test X

```python
meta_test_X = np.column_stack(
    [
        test_rf,
        test_xgb,
        test_lgb
    ]
)
```

Shape:

```text
(Test Sample 수, 3)
```

이다.

---

# 62. Meta Model 최종 Prediction

```python
final_prob = (
    meta_model
    .predict_proba(
        meta_test_X
    )[:, 1]
)
```

이것이 Stacking의 최종 Test Prediction이다.

---

# 63. 전체 구조

```text
                 TRAIN

                  ↓

       ┌──────────┼──────────┐
       ↓          ↓          ↓
      RF         XGB        LGBM
       ↓          ↓          ↓
    RF OOF     XGB OOF    LGBM OOF
       └──────────┼──────────┘
                  ↓
                meta_X
                  ↓
             Meta Model


                 TEST

                  ↓

       ┌──────────┼──────────┐
       ↓          ↓          ↓
      RF         XGB        LGBM
       ↓          ↓          ↓
   RF Test     XGB Test    LGBM Test
       └──────────┼──────────┘
                  ↓
             meta_test_X
                  ↓
             Meta Model
                  ↓
          Final Prediction
```

---

# 64. Base Model별 OOF Dictionary

모델이 여러 개면 Dictionary를 사용할 수 있다.

```python
model_names = [
    "RandomForest",
    "XGBoost",
    "LightGBM"
]


oof = {
    name:
        np.zeros(
            len(
                X
            )
        )

    for name in model_names
}
```

---

# 65. Test Prediction Dictionary

```python
test_pred = {
    name:
        np.zeros(
            len(
                X_test
            )
        )

    for name in model_names
}
```

---

# 66. 이 코드의 의미

예:

```python
oof[
    "RandomForest"
]
```

에는 Random Forest의 전체 OOF Prediction이 들어간다.

```python
oof[
    "XGBoost"
]
```

에는 XGBoost의 OOF Prediction이 들어간다.

---

# 67. 모델 Dictionary

```python
models = {
    "RandomForest":
        rf,

    "XGBoost":
        xgb,

    "LightGBM":
        lgb
}
```

---

# 68. Model 반복

```python
for name, model in models.items():

    print(
        name
    )
```

이렇게 하면 같은 CV 구조를 여러 모델에 반복 적용할 수 있다.

---

# 69. Base Model OOF 생성 함수

```python
def make_oof_predictions(
    model,
    X,
    y,
    X_test,
    cv
):

    oof = np.zeros(
        len(
            X
        )
    )


    test_pred = np.zeros(
        len(
            X_test
        )
    )


    for fold, (
        train_idx,
        valid_idx
    ) in enumerate(
        cv.split(
            X,
            y
        ),
        1
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


        oof[
            valid_idx
        ] = model.predict_proba(
            X_valid
        )[:, 1]


        test_pred += (
            model.predict_proba(
                X_test
            )[:, 1]
            /
            cv.n_splits
        )


    return (
        oof,
        test_pred
    )
```

---

# 70. 함수 사용

```python
oof_rf, test_rf = (
    make_oof_predictions(
        rf,
        X,
        y,
        X_test,
        skf
    )
)
```

---

# 71. 다른 모델도 반복

```python
oof_xgb, test_xgb = (
    make_oof_predictions(
        xgb,
        X,
        y,
        X_test,
        skf
    )
)


oof_lgb, test_lgb = (
    make_oof_predictions(
        lgb,
        X,
        y,
        X_test,
        skf
    )
)
```

---

# 72. Base Model OOF 성능

```python
from sklearn.metrics import (
    roc_auc_score
)


print(
    "RF:",
    roc_auc_score(
        y,
        oof_rf
    )
)


print(
    "XGB:",
    roc_auc_score(
        y,
        oof_xgb
    )
)


print(
    "LGBM:",
    roc_auc_score(
        y,
        oof_lgb
    )
)
```

---

# 73. Meta Dataset 만들기

```python
meta_X = np.column_stack(
    [
        oof_rf,
        oof_xgb,
        oof_lgb
    ]
)


meta_test_X = np.column_stack(
    [
        test_rf,
        test_xgb,
        test_lgb
    ]
)
```

---

# 74. Shape 확인

```python
print(
    meta_X.shape
)


print(
    meta_test_X.shape
)
```

예:

```text
(10000, 3)

(2000, 3)
```

---

# 75. Meta Model

```python
from sklearn.linear_model import (
    LogisticRegression
)


meta_model = LogisticRegression(
    max_iter=1000
)


meta_model.fit(
    meta_X,
    y
)
```

---

# 76. 최종 Test Prediction

```python
final_prob = (
    meta_model
    .predict_proba(
        meta_test_X
    )[:, 1]
)
```

---

# 77. 이게 Stacking의 기본 구조

```text
Base Models
↓
OOF
↓
Meta Features
↓
Meta Model
↓
Final Prediction
```

이다.

---

# 78. 그런데 여기서 한 단계 더 생각해야 한다

Meta Model을:

```python
meta_model.fit(
    meta_X,
    y
)
```

한 뒤 같은:

```text
meta_X
```

에서 성능을 측정하면 Meta Model 자신의 Training Score가 된다.

즉 이것을 최종 일반화 성능이라고 생각하면 안 된다.

---

# 79. Meta Model도 평가가 필요

Meta Model도:

```text
별도의 Holdout

Cross Validation

Nested CV
```

등으로 평가할 수 있다.

---

# 80. Stacking 성능을 공정하게 평가하려면?

예를 들어 최종 Test Set을 별도로 유지한 상태에서:

```text
Training Set
↓
OOF Base Prediction
↓
Meta Model 학습

Final Test
↓
Base Model Prediction
↓
Meta Model Prediction
↓
최종 성능 평가
```

처럼 구성할 수 있다.

---

# 81. Competition에서는?

Competition의 `test.csv`에는 Label이 없기 때문에 우리가 직접 최종 Accuracy나 AUC를 계산할 수 없다.

Prediction을 제출하면 대회 서버가 숨겨진 정답과 비교하여 Score를 계산한다.

---

# 82. Public Leaderboard

일부 Competition에서는 Test Label의 일부를 이용하여:

```text
Public Leaderboard
```

Score를 보여준다.

---

# 83. Private Leaderboard

최종 순위에서는 다른 숨겨진 부분을 이용하여:

```text
Private Leaderboard
```

를 계산하기도 한다.

따라서 Public Score만 계속 최적화하면 Leaderboard Overfitting이 발생할 수 있다.

---

# 84. OOF의 또 다른 장점

OOF Prediction을 이용하면 Training Data 전체에 대해 비교적 공정한 Probability를 얻을 수 있다.

이를 이용해:

```text
Threshold 선택

Error Analysis

Calibration 분석

Stacking

Blending Weight 탐색
```

등을 할 수 있다.

---

# 85. OOF로 Threshold 선택

예:

```python
thresholds = np.arange(
    0.1,
    0.91,
    0.01
)
```

OOF Probability를 이용해 Validation 관점에서 Threshold를 탐색할 수 있다.

---

# 86. 예

```python
from sklearn.metrics import (
    f1_score
)


best_threshold = 0.5

best_score = -1


for threshold in thresholds:

    pred = (
        oof_xgb
        >=
        threshold
    ).astype(int)


    score = f1_score(
        y,
        pred
    )


    if score > best_score:

        best_score = score

        best_threshold = threshold
```

---

# 87. Test에서 Threshold를 찾지 않는 이유

Test 결과를 계속 확인하며 Threshold를 선택하면 Test에 맞춰지는 문제가 발생한다.

따라서:

```text
Validation

또는

OOF
```

에서 Threshold를 선택하고 Test에는 그 기준을 적용한다.

---

# 88. OOF를 이용한 Error Analysis

```python
oof_pred = (
    oof_xgb
    >=
    0.5
).astype(int)
```

틀린 Sample:

```python
wrong_mask = (
    oof_pred
    !=
    y.to_numpy()
)
```

---

# 89. 틀린 Row 확인

```python
wrong_rows = X.loc[
    wrong_mask
].copy()
```

실제 Label과 Probability도 붙일 수 있다.

```python
wrong_rows[
    "true"
] = y.loc[
    wrong_mask
]


wrong_rows[
    "prob"
] = oof_xgb[
    wrong_mask
]
```

---

# 90. 왜 OOF Error Analysis가 유용할까?

Training Prediction Error보다:

```text
실제 처음 보는 Sample에서
어떤 오류를 낼 가능성이 있는가?
```

에 더 가까운 분석을 할 수 있기 때문이다.

---

# 91. Blending

Stacking과 비슷하게 여러 Prediction을 조합하는 방법으로:

```text
Blending
```

이라는 표현도 사용한다.

가장 간단한 형태:

```python
final_prob = (
    0.3 * test_rf
    +
    0.4 * test_xgb
    +
    0.3 * test_lgb
)
```

---

# 92. Weight는 어디서 정할까?

좋은 기준:

```text
Validation

Cross Validation

OOF
```

이다.

좋지 않은 방식:

```text
Test Score가 가장 잘 나오도록
Weight를 계속 변경
```

이다.

---

# 93. Average Ensemble

가장 단순한 방식:

```python
final_prob = (
    test_rf
    +
    test_xgb
    +
    test_lgb
) / 3
```

이다.

Meta Model이 없다.

---

# 94. Weighted Ensemble

```python
final_prob = (
    0.2 * test_rf
    +
    0.5 * test_xgb
    +
    0.3 * test_lgb
)
```

처럼 Weight를 직접 정한다.

---

# 95. Stacking

```text
Weight 또는 조합 방법 자체를
Meta Model이 학습
```

한다.

---

# 96. 비교

```text
Average
→ 단순 평균

Weighted Average
→ 사람이 Weight 설정

Stacking
→ Meta Model이 조합 학습
```

이다.

---

# 97. 모델 다양성이 중요한 이유

Base Model이 모두 거의 같은 Prediction을 한다면 Stacking 효과가 작을 수 있다.

예:

```text
RF = 0.81

XGB = 0.82

LGBM = 0.81
```

모든 Sample에서 거의 똑같다면 새로운 정보가 적다.

---

# 98. 반대로

모델들의 Error Pattern이 서로 다르면:

```text
Model A가 틀린 것을 Model B가 맞히고

Model B가 틀린 것을 Model C가 맞히는
```

상황이 생길 수 있다.

이런 다양성이 Ensemble에 도움이 될 수 있다.

---

# 99. OOF Prediction 상관관계

```python
import pandas as pd


oof_df = pd.DataFrame(
    {
        "RF":
            oof_rf,

        "XGB":
            oof_xgb,

        "LGBM":
            oof_lgb
    }
)


print(
    oof_df.corr()
)
```

---

# 100. 상관관계 해석

모델 Prediction Correlation이:

```text
0.999
```

처럼 매우 높다면 모델들이 거의 비슷하게 예측하고 있을 수 있다.

하지만 Correlation 하나만으로 Ensemble 가치가 결정되는 것은 아니다.

실제 CV 성능도 확인한다.

---

# 101. Stacking이 항상 성능을 높이나?

아니다.

Stacking을 했는데:

```text
Base Best AUC = 0.91

Stacking AUC = 0.90
```

처럼 오히려 낮아질 수도 있다.

---

# 102. 왜 낮아질 수 있을까?

가능한 이유:

```text
Base Model들이 너무 비슷함

Meta Model Overfitting

데이터가 적음

OOF 설계가 잘못됨

Leakage

Hyperparameter 문제

불안정한 Fold

Meta Feature가 충분한 정보를 제공하지 못함
```

등이 있다.

---

# 103. 복잡한 모델이 무조건 좋은 Meta Model?

아니다.

Meta Model로 복잡한 모델을 사용하면 Meta Feature 수가 적은 상황에서 Overfitting할 수도 있다.

그래서:

```text
Logistic Regression
```

같은 단순한 모델이 좋은 시작점이 될 수 있다.

---

# 104. Original Feature도 Meta Model에 넣을 수 있나?

가능하다.

예:

```text
RF Prediction
XGB Prediction
LGBM Prediction
+
Original Features
```

를 Meta Model에 넣는 방식도 존재한다.

하지만 구조가 복잡해지고 Leakage와 Overfitting 관리가 더 중요해진다.

처음에는 Base Prediction만 사용하는 구조를 이해하는 것이 좋다.

---

# 105. OOF에서 전처리도 중요

예:

```text
Scaling

Imputation

Encoding

SMOTE

Feature Selection
```

등을 CV 전에 전체 데이터에 Fit하면 Leakage가 발생할 수 있다.

---

# 106. 잘못된 방식

```python
X_scaled = scaler.fit_transform(
    X
)


for train_idx, valid_idx in skf.split(
    X_scaled,
    y
):
    ...
```

전체 데이터의 평균과 표준편차가 이미 Validation Fold에 반영되었다.

---

# 107. 올바른 방식

Pipeline 안에서 Fold별로 Fit한다.

```python
from sklearn.pipeline import (
    Pipeline
)


pipeline = Pipeline(
    steps=[
        (
            "scaler",
            StandardScaler()
        ),
        (
            "model",
            LogisticRegression()
        )
    ]
)
```

각 Fold의 Training Data에 대해서만 Pipeline이 Fit되도록 한다.

---

# 108. SMOTE도 동일

잘못된 흐름:

```text
전체 Train
↓
SMOTE
↓
Cross Validation
```

이다.

Validation 정보가 Sampling 과정에 간접적으로 섞일 수 있다.

---

# 109. 올바른 흐름

```text
Fold Split
↓
Fold Training Data
↓
SMOTE
↓
Model Fit
↓
Untouched Validation Fold Prediction
```

이다.

`imblearn.pipeline.Pipeline`을 사용할 수 있다.

---

# 110. Group Data도 주의

한 환자에게 여러 Record가 있다면 일반 StratifiedKFold만 사용했을 때:

```text
같은 환자의 Record가

Train Fold와
Validation Fold에

동시에 들어갈 수 있다.
```

---

# 111. 의료 영상 예

한 환자에게 X-ray가 5장 있다고 하자.

```text
Patient A Image 1
Patient A Image 2
Patient A Image 3
Patient A Image 4
Patient A Image 5
```

일반 Random Split을 하면 같은 환자의 영상이 Train과 Validation에 섞일 수 있다.

---

# 112. 해결

환자 ID를 Group으로 사용하여:

```text
GroupKFold

StratifiedGroupKFold
```

등을 고려할 수 있다.

OOF의 핵심은:

```text
실제 Deployment 상황을 반영하는 Fold
```

를 만드는 것이다.

---

# 113. Time Series도 다르다

시간 데이터에서는 Random KFold가 부적절할 수 있다.

예:

```text
2025년 Data로
2024년을 Validation
```

하면 실제 미래 예측 상황과 다르다.

---

# 114. TimeSeriesSplit

시간 순서를 유지하는:

```text
TimeSeriesSplit
```

등을 고려한다.

즉 OOF라고 해서 무조건 StratifiedKFold를 사용하는 것이 아니다.

---

# 115. 문제에 맞는 CV

```text
일반 Regression
→ KFold

Classification
→ StratifiedKFold

같은 환자/사람 반복
→ GroupKFold 계열

Class Balance + Group
→ StratifiedGroupKFold 고려

Time Series
→ TimeSeriesSplit
```

처럼 문제 구조에 맞게 선택한다.

---

# 116. OOF와 Cross Validation 차이

Cross Validation의 주요 목적:

```text
Model 성능 평가
```

OOF:

```text
Cross Validation 과정에서
각 Validation Fold에 대한 Prediction을 저장
```

한 결과이다.

---

# 117. 즉

```text
Cross Validation
→ 평가 방법

OOF Prediction
→ CV 과정에서 얻는 Prediction
```

이라고 이해하면 쉽다.

---

# 118. OOF와 Stacking 차이

```text
OOF
→ Prediction을 만드는 방법

Stacking
→ 여러 Model Prediction을 Meta Model에 넣는 Ensemble 방법
```

이다.

OOF는 Stacking에 자주 사용된다.

---

# 119. Stacking에 OOF가 필수 개념인 이유

Meta Model Training Data를 만들 때 Base Model의 Training Prediction을 그대로 사용하지 않기 위해서이다.

```text
Base Model
↓
OOF Prediction
↓
Meta Model
```

구조가 핵심이다.

---

# 120. Nested CV란?

Nested Cross Validation은:

```text
Cross Validation 안에
또 다른 Cross Validation이 있는 구조
```

이다.

주로 Hyperparameter Tuning과 모델 성능 평가를 더 엄격하게 분리하기 위해 사용한다.

---

# 121. Nested CV 구조

```text
Outer CV
→ 최종 성능 평가

Inner CV
→ Hyperparameter Tuning
```

이다.

---

# 122. 예

```text
Outer Fold 1

    Outer Train
        ↓
      Inner CV
        ↓
Hyperparameter Tuning
        ↓
Best Model
        ↓
Outer Validation 평가
```

이 과정을 Outer Fold마다 반복한다.

---

# 123. 왜 Nested CV를 사용할까?

일반적으로 같은 CV 결과를 계속 보면서 Hyperparameter를 선택하면 그 CV 자체에 맞춰질 수 있다.

Nested CV는:

```text
Model Selection

과

Model Evaluation
```

을 더 명확하게 분리한다.

---

# 124. OOF와 Nested CV는 같은가?

아니다.

```text
OOF
→ Fold 밖 Sample에 대한 Prediction

Nested CV
→ Inner / Outer CV를 이용한 Model Selection과 Evaluation 구조
```

이다.

---

# 125. Stacking과 Nested CV도 같은가?

아니다.

```text
Stacking
→ Ensemble 방법

OOF
→ Prediction 생성 방식

Nested CV
→ 평가 / 튜닝 구조
```

이다.

---

# 126. 세 개를 연결

```text
Cross Validation
      ↓
OOF Prediction 생성 가능
      ↓
Stacking의 Meta Feature로 사용 가능
```

그리고 별도로:

```text
Nested CV
→ Tuning과 Evaluation을 엄격하게 분리
```

한다.

---

# 127. 전체 관계

```text
                Cross Validation
                       │
             ┌─────────┴─────────┐
             ↓                   ↓
       Performance             OOF
       Evaluation           Prediction
                                 ↓
                              Stacking
                                 ↓
                            Meta Model


Nested CV
→ Model Selection과 Evaluation을
  Inner / Outer CV로 분리
```

---

# 128. Scikit-learn StackingClassifier

Scikit-learn에는 직접 Stacking을 수행하는:

```python
from sklearn.ensemble import (
    StackingClassifier
)
```

도 있다.

---

# 129. 기본 예

```python
from sklearn.ensemble import (
    RandomForestClassifier,
    StackingClassifier
)

from sklearn.linear_model import (
    LogisticRegression
)


estimators = [
    (
        "rf",
        RandomForestClassifier(
            n_estimators=300,
            random_state=42
        )
    ),

    (
        "lr",
        LogisticRegression(
            max_iter=1000
        )
    )
]


stack_model = StackingClassifier(
    estimators=estimators,
    final_estimator=LogisticRegression(
        max_iter=1000
    ),
    cv=5
)
```

---

# 130. 학습

```python
stack_model.fit(
    X_train,
    y_train
)
```

---

# 131. Prediction

```python
pred = stack_model.predict(
    X_valid
)


prob = stack_model.predict_proba(
    X_valid
)[:, 1]
```

---

# 132. 직접 구현을 먼저 배우는 이유

`StackingClassifier`를 사용하면 편리하다.

하지만 직접 OOF를 구현해보면:

```text
왜 Fold가 필요한지

OOF가 어디에 저장되는지

Meta Feature가 어떻게 만들어지는지

Test Prediction을 왜 평균하는지
```

를 정확히 이해할 수 있다.

---

# 133. Classification 전체 예제 준비

예를 들어 세 Base Model을 사용한다고 하자.

```python
from sklearn.ensemble import (
    RandomForestClassifier
)

from xgboost import (
    XGBClassifier
)

from lightgbm import (
    LGBMClassifier
)


rf = RandomForestClassifier(
    n_estimators=300,
    random_state=42,
    n_jobs=-1
)


xgb = XGBClassifier(
    n_estimators=500,
    learning_rate=0.03,
    max_depth=5,
    random_state=42
)


lgb = LGBMClassifier(
    n_estimators=500,
    learning_rate=0.03,
    random_state=42
)
```

---

# 134. CV 준비

```python
from sklearn.model_selection import (
    StratifiedKFold
)


skf = StratifiedKFold(
    n_splits=5,
    shuffle=True,
    random_state=42
)
```

---

# 135. OOF 함수

```python
import numpy as np

from sklearn.base import (
    clone
)


def make_oof(
    model,
    X,
    y,
    X_test,
    cv
):

    oof = np.zeros(
        len(
            X
        )
    )


    test_fold_preds = np.zeros(
        (
            len(
                X_test
            ),
            cv.n_splits
        )
    )


    for fold, (
        train_idx,
        valid_idx
    ) in enumerate(
        cv.split(
            X,
            y
        )
    ):

        fold_model = clone(
            model
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


        fold_model.fit(
            X_train,
            y_train
        )


        oof[
            valid_idx
        ] = (
            fold_model
            .predict_proba(
                X_valid
            )[:, 1]
        )


        test_fold_preds[
            :,
            fold
        ] = (
            fold_model
            .predict_proba(
                X_test
            )[:, 1]
        )


    test_pred = (
        test_fold_preds
        .mean(
            axis=1
        )
    )


    return (
        oof,
        test_pred
    )
```

---

# 136. 왜 clone을 사용할까?

```python
from sklearn.base import clone
```

을 사용하면 Fold마다 같은 설정을 가진 새로운 Model 객체를 만들 수 있다.

```text
Fold 1
→ Model 1

Fold 2
→ Model 2

Fold 3
→ Model 3
```

처럼 생각할 수 있다.

---

# 137. Base Model OOF 생성

```python
oof_rf, test_rf = make_oof(
    rf,
    X,
    y,
    X_test,
    skf
)


oof_xgb, test_xgb = make_oof(
    xgb,
    X,
    y,
    X_test,
    skf
)


oof_lgb, test_lgb = make_oof(
    lgb,
    X,
    y,
    X_test,
    skf
)
```

---

# 138. OOF Score 비교

```python
from sklearn.metrics import (
    roc_auc_score
)


print(
    "RF OOF AUC:",
    roc_auc_score(
        y,
        oof_rf
    )
)


print(
    "XGB OOF AUC:",
    roc_auc_score(
        y,
        oof_xgb
    )
)


print(
    "LGB OOF AUC:",
    roc_auc_score(
        y,
        oof_lgb
    )
)
```

---

# 139. Meta Feature 생성

```python
meta_X = np.column_stack(
    [
        oof_rf,
        oof_xgb,
        oof_lgb
    ]
)


meta_test_X = np.column_stack(
    [
        test_rf,
        test_xgb,
        test_lgb
    ]
)
```

---

# 140. 확인

```python
print(
    "meta_X:",
    meta_X.shape
)


print(
    "meta_test_X:",
    meta_test_X.shape
)
```

---

# 141. Meta Model 학습

```python
from sklearn.linear_model import (
    LogisticRegression
)


meta_model = LogisticRegression(
    max_iter=1000
)


meta_model.fit(
    meta_X,
    y
)
```

---

# 142. 최종 Prediction

```python
final_prob = (
    meta_model
    .predict_proba(
        meta_test_X
    )[:, 1]
)
```

---

# 143. Classification Label이 필요하다면

```python
threshold = 0.5


final_pred = (
    final_prob
    >=
    threshold
).astype(int)
```

대회가 Probability 제출을 요구하는지 Label 제출을 요구하는지 반드시 확인한다.

---

# 144. Submission

```python
submission[
    "target"
] = final_prob


submission.to_csv(
    "submission_stacking.csv",
    index=False
)
```

---

# 145. 여기서 주의

실제 Competition의 Target Column 이름은:

```text
target
```

이 아닐 수도 있다.

반드시:

```python
print(
    submission.columns
)
```

을 확인한다.

---

# 146. Regression Stacking

Regression에서도 구조는 같다.

다만:

```text
predict_proba()
```

가 아니라:

```text
predict()
```

를 사용한다.

---

# 147. Regression OOF

```python
oof[
    valid_idx
] = model.predict(
    X_valid
)
```

Test:

```python
test_fold_preds[
    :,
    fold
] = model.predict(
    X_test
)
```

이다.

---

# 148. Meta Regressor

예:

```python
from sklearn.linear_model import (
    Ridge
)


meta_model = Ridge(
    alpha=1.0
)


meta_model.fit(
    meta_X,
    y
)
```

---

# 149. Regression 최종 예측

```python
final_pred = meta_model.predict(
    meta_test_X
)
```

이다.

---

# 150. OOF 배열이 0으로 남아 있다면?

OOF 생성 후 확인할 수 있다.

```python
print(
    oof.shape
)


print(
    np.isnan(
        oof
    ).sum()
)
```

하지만 실제 Prediction 값이 0일 수도 있기 때문에 단순히:

```python
(oof == 0).sum()
```

만으로 미할당 여부를 판단하는 것은 항상 안전하지 않다.

---

# 151. 더 안전한 초기화

```python
oof = np.full(
    len(
        X
    ),
    np.nan
)
```

으로 만들 수 있다.

Fold Prediction을 채운 뒤:

```python
print(
    np.isnan(
        oof
    ).sum()
)
```

을 확인한다.

결과:

```text
0
```

이면 모든 Sample이 채워졌다는 뜻이다.

---

# 152. 개선된 OOF 초기화

```python
oof = np.full(
    len(
        X
    ),
    np.nan
)
```

이 방식은 디버깅할 때 유용하다.

---

# 153. Index 문제

Pandas DataFrame의 Index가:

```text
0, 1, 2, 3 ...
```

이 아닐 수도 있다.

`skf.split()`에서 반환되는 Index는 위치 Index이므로:

```python
X.iloc[
    train_idx
]
```

처럼 `.iloc`을 사용한다.

---

# 154. loc와 iloc 차이

```text
loc
→ Index Label

iloc
→ 위치
```

이다.

CV에서 반환하는:

```text
train_idx

valid_idx
```

는 위치이므로 일반적으로 `.iloc`이 맞다.

---

# 155. y가 NumPy라면?

```python
y_train = y[
    train_idx
]
```

처럼 사용할 수 있다.

Pandas Series라면:

```python
y_train = y.iloc[
    train_idx
]
```

를 사용할 수 있다.

---

# 156. 같은 Fold를 사용해야 하는 이유

모델 A:

```text
CV Split A
```

모델 B:

```text
CV Split B
```

처럼 서로 다른 Fold를 사용해도 Stacking 구현은 가능하지만 비교와 Meta Feature 정렬을 관리하기 어려워진다.

가능하면 같은 CV Split을 사용하면:

```text
공정한 비교

재현성

구조 관리
```

가 쉬워진다.

---

# 157. random_state

```python
random_state=42
```

는 같은 Fold를 재현하기 위한 것이다.

```text
42가 가장 성능이 좋은 숫자
```

라는 뜻은 아니다.

---

# 158. Seed Hunting 주의

```text
random_state=1
→ 0.89

random_state=2
→ 0.90

random_state=100
→ 0.94
```

라고 해서 가장 높은 Seed를 선택하는 것은 Validation Split에 Overfitting하는 방식이 될 수 있다.

---

# 159. OOF가 높은데 Leaderboard가 낮다면?

가능한 이유:

```text
CV 구조가 실제 Test Distribution을 반영하지 못함

Leakage

Train/Test Distribution Shift

Group Leakage

Public Leaderboard Variance

Overfitting

Feature Engineering 문제
```

등이 있다.

---

# 160. OOF가 낮은데 Leaderboard가 높은 경우

우연히 Public Test와 모델이 잘 맞았을 수도 있다.

따라서 Public Leaderboard 하나만으로 모델 품질을 판단하지 않는다.

---

# 161. 좋은 Validation Strategy가 중요

실제 Machine Learning Competition에서:

```text
모델보다 Validation Strategy가 더 중요한 경우
```

도 많다.

잘못된 CV에서는 모델 개선 여부 자체를 정확히 판단하기 어렵기 때문이다.

---

# 162. OOF와 Feature Engineering

Feature Engineering을 비교할 때:

```text
Baseline OOF AUC = 0.82

Feature A 추가
OOF AUC = 0.84

Feature B 추가
OOF AUC = 0.81
```

처럼 같은 CV 기준으로 비교할 수 있다.

---

# 163. Hyperparameter Tuning과 OOF

Hyperparameter도 같은 Validation Strategy 안에서 비교해야 한다.

하지만 같은 OOF Score를 반복적으로 최적화하면 결국 그 CV에 Overfitting할 수 있다는 점도 기억한다.

---

# 164. 그래서 Nested CV가 등장

모델 Selection과 최종 Evaluation을 엄격히 분리해야 하는 상황에서는 Nested CV를 고려할 수 있다.

```text
Inner CV
→ Tuning

Outer CV
→ Evaluation
```

이다.

---

# 165. 모든 프로젝트에 Nested CV?

아니다.

Nested CV는 계산량이 크다.

따라서:

```text
데이터 크기

프로젝트 목적

계산 자원

평가 엄격성
```

을 고려한다.

---

# 166. Stacking도 무조건 필요한가?

아니다.

단일 LightGBM이:

```text
AUC = 0.92
```

Stacking이:

```text
AUC = 0.921
```

이라면 복잡도 증가에 비해 개선이 작을 수 있다.

실서비스에서는 단순한 모델을 선택할 수도 있다.

---

# 167. 실서비스에서 고려할 것

Stacking은 여러 모델을 실행해야 하므로:

```text
Inference Time

Memory

Model File 관리

Deployment 복잡도

Monitoring

재학습 비용
```

이 증가한다.

---

# 168. Competition과 Production

Competition:

```text
0.001 성능 향상도 중요할 수 있음
```

Production:

```text
성능
+
속도
+
비용
+
안정성
+
설명 가능성
+
유지보수
```

를 함께 고려해야 한다.

---

# 169. Stacking에서 흔한 실수

```text
Base Model의 Train Prediction을 Meta Feature로 사용

OOF와 Test Prediction을 혼동

Fold마다 Test Prediction을 평균하지 않음

전처리를 전체 데이터에 Fit

SMOTE를 CV 전에 수행

Group Leakage 무시

Time Data를 Random Split

Meta Model Training Score를 최종 성능이라고 생각

Test Score를 보면서 Weight 계속 조정

Base Model들이 거의 동일한데 무조건 Stacking
```

등이 있다.

---

# 170. OOF에서 흔한 실수

```text
oof[train_idx]에 Prediction 저장

valid_idx 위치를 잘못 사용

iloc 대신 잘못된 loc 사용

Fold Prediction 길이 불일치

Target Index와 X Index 불일치

Probability가 필요한데 predict() 사용

Positive Class 위치를 확인하지 않음
```

등이다.

---

# 171. predict vs predict_proba

Classification Stacking에서 Class Label:

```text
0
1
```

만 Meta Feature로 사용하는 것보다 Probability:

```text
0.12
0.48
0.91
```

를 사용하면 더 많은 정보를 제공할 수 있다.

그래서 Binary Classification에서는 `predict_proba()`를 자주 사용한다.

---

# 172. 하지만 모든 모델에 predict_proba가 있는 것은 아니다

일부 모델은:

```text
decision_function()
```

을 제공할 수 있다.

따라서 Model API를 확인해야 한다.

---

# 173. Positive Class 확인

```python
print(
    model.classes_
)
```

를 확인한다.

```python
predict_proba(
    X
)[:, 1]
```

에서 두 번째 Column이 우리가 원하는 Positive Class인지 확인해야 한다.

---

# 174. Multi-class Stacking

Multi-class에서는 한 모델이 여러 Class Probability를 출력한다.

예:

```text
Class 0
Class 1
Class 2
```

이면 Base Model 하나가:

```text
3개의 Meta Feature
```

를 만들 수 있다.

---

# 175. 예

RF:

```text
RF_P0
RF_P1
RF_P2
```

XGB:

```text
XGB_P0
XGB_P1
XGB_P2
```

두 모델이면 Meta Feature가 6개가 될 수 있다.

---

# 176. Multi-class Shape

```python
prob = model.predict_proba(
    X_valid
)
```

Shape:

```text
(n_samples, n_classes)
```

이다.

Binary처럼 무조건:

```python
[:, 1]
```

만 사용하는 것이 아니다.

---

# 177. Regression에는 Probability가 없다

Regression은 연속값 자체를 예측한다.

```python
pred = model.predict(
    X_valid
)
```

이 Prediction이 Meta Feature가 된다.

---

# 178. OOF를 DataFrame으로 저장

분석을 쉽게 하려면:

```python
oof_df = pd.DataFrame(
    {
        "rf_oof":
            oof_rf,

        "xgb_oof":
            oof_xgb,

        "lgb_oof":
            oof_lgb,

        "target":
            y.to_numpy()
    }
)
```

처럼 만들 수 있다.

---

# 179. CSV 저장

```python
oof_df.to_csv(
    "oof_predictions.csv",
    index=False
)
```

나중에:

```text
Threshold 분석

Error Analysis

Stacking 실험
```

에 재사용할 수 있다.

---

# 180. Test Prediction도 저장

```python
test_pred_df = pd.DataFrame(
    {
        "rf":
            test_rf,

        "xgb":
            test_xgb,

        "lgb":
            test_lgb
    }
)


test_pred_df.to_csv(
    "base_test_predictions.csv",
    index=False
)
```

---

# 181. Stacking 실험 기록

예:

| Experiment | Base Models | Meta Model | OOF AUC |
|---|---|---|---:|
| S1 | RF + XGB | Logistic | 0.887 |
| S2 | RF + XGB + LGBM | Logistic | 0.901 |
| S3 | XGB + LGBM | Logistic | 0.899 |

이런 식으로 기록하면 어떤 조합이 실제로 도움이 되는지 알 수 있다.

---

# 182. Base Model Score도 같이 기록

| Model | OOF AUC |
|---|---:|
| RF | 0.872 |
| XGB | 0.895 |
| LGBM | 0.891 |
| Stacking | 0.901 |

이렇게 비교해야 Stacking의 실제 개선 정도를 알 수 있다.

---

# 183. 단순 평균도 반드시 비교

Stacking을 만들었다면:

```text
Best Single Model

Simple Average

Weighted Average

Stacking
```

을 같이 비교하는 것이 좋다.

복잡한 Stacking이 단순 평균보다 항상 좋은 것은 아니다.

---

# 184. 핵심 용어 정리

```text
Base Model
→ 첫 단계에서 Prediction을 만드는 Model

Meta Model
→ Base Prediction을 입력으로 받는 Model

Meta Feature
→ Base Model Prediction

OOF
→ Out-Of-Fold Prediction

Fold
→ CV에서 나눈 Data 조각

Stacking
→ Base Prediction을 Meta Model로 조합

Blending
→ 여러 Model Prediction을 조합하는 방식

Nested CV
→ Inner / Outer CV 구조

Test Prediction
→ 학습되지 않은 외부 Test Data의 Prediction
```

---

# 185. 가장 중요한 관계

```text
Cross Validation
       ↓
OOF Prediction
       ↓
Meta Feature
       ↓
Stacking
       ↓
Meta Model
       ↓
Final Prediction
```

---

# 186. 가장 중요한 OOF 문장

```text
OOF Prediction은
각 Sample이 자신을 학습하지 않은 모델에게
받은 Prediction이다.
```

---

# 187. 가장 중요한 Stacking 문장

```text
Stacking은
여러 모델의 Prediction을
새로운 Feature로 사용하여
Meta Model이 최종 Prediction을 만드는 방법이다.
```

---

# 188. 가장 중요한 Test 문장

```text
test.csv
=
예측해야 할 입력 데이터

Test Prediction
=
그 test.csv를 Model이 예측한 결과
```

둘은 같은 것이 아니다.

---

# 189. 전체 흐름 한 번에 보기

```text
             train.csv
                 ↓
              X / y
                 ↓
       StratifiedKFold
                 ↓
      ┌──────────┼──────────┐
      ↓          ↓          ↓
     RF         XGB        LGBM
      ↓          ↓          ↓
   RF OOF     XGB OOF    LGBM OOF
      └──────────┼──────────┘
                 ↓
              meta_X
                 ↓
           Meta Model 학습


              test.csv
                 ↓
      ┌──────────┼──────────┐
      ↓          ↓          ↓
     RF         XGB        LGBM
      ↓          ↓          ↓
 Fold Test   Fold Test   Fold Test
 Prediction Prediction Prediction
      ↓          ↓          ↓
   평균        평균        평균
      ↓          ↓          ↓
   test_rf    test_xgb   test_lgb
      └──────────┼──────────┘
                 ↓
            meta_test_X
                 ↓
             Meta Model
                 ↓
          Final Prediction
                 ↓
         submission.csv
```

---

# 190. Machine Learning 전체 과정과 연결

지금까지 Chapter 06에서 배운 내용을 모두 연결하면:

```text
Machine Learning 기본
        ↓
Train / Validation / Test
        ↓
Preprocessing
        ↓
Classification / Regression
        ↓
Ensemble
        ↓
Cross Validation
        ↓
Hyperparameter Tuning
        ↓
Imbalanced Data 처리
        ↓
Evaluation Metrics
        ↓
Feature Importance / SHAP
        ↓
OOF
        ↓
Stacking
```

이다.

---

# 191. 실전 프로젝트 흐름

```text
1. 문제 정의

2. Train / Test 확인

3. EDA

4. Missing / Outlier 확인

5. Feature Engineering

6. Validation Strategy 설계

7. Baseline Model

8. Cross Validation

9. Model 비교

10. Hyperparameter Tuning

11. Imbalance 처리

12. Evaluation

13. Feature Importance / SHAP

14. OOF Prediction

15. Ensemble / Stacking

16. Test Prediction

17. Submission 또는 Deployment
```

---

# 192. 마지막으로 기억할 것

Stacking을 이해할 때 가장 헷갈리는 부분은:

```text
왜 Train Data를 다시 예측하지?

왜 OOF가 필요하지?

왜 Test Prediction을 Fold마다 만들지?
```

이다.

답은 다음과 같다.

```text
Train Data
→ Meta Model을 학습할 Prediction 필요
→ 그냥 Train Prediction을 쓰면 지나치게 낙관적
→ OOF Prediction 사용

Test Data
→ Meta Model 입력도 Base Prediction이어야 함
→ Fold별 Base Model로 Test Prediction
→ 평균
→ Meta Model 입력
```

---

# 193. 배운 점

Stacking은 단순히 여러 모델을 한꺼번에 사용하는 것이 아니다.

핵심은:

```text
Base Model
↓
OOF Prediction
↓
Meta Feature
↓
Meta Model
```

구조이다.

OOF를 사용하는 이유는:

```text
각 Sample을
그 Sample을 학습하지 않은 Model로
Prediction하기 위해서
```

이다.

그리고:

```text
OOF
≠
Test Data
```

이다.

OOF는 Training Dataset 내부에서 Cross Validation을 통해 만든 Prediction이다.

반면:

```text
test.csv
```

는 Model이 최종적으로 Target을 예측해야 하는 외부 입력 데이터이다.

또한:

```text
Stacking
OOF
Nested CV
```

는 서로 같은 개념이 아니다.

```text
Stacking
→ Ensemble 방법

OOF
→ CV 기반 Prediction 생성 방식

Nested CV
→ Model Selection과 Evaluation을 분리하는 CV 구조
```

이다.

최종적으로 가장 중요한 구조는:

```text
Train
↓
CV
↓
OOF
↓
Meta Model 학습

Test
↓
Fold Model Prediction 평균
↓
Meta Model
↓
Final Prediction
```

이다.