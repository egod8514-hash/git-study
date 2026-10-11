# Ensemble Learning 앙상블

## 1. Ensemble이란?

Ensemble Learning은 여러 개의 모델을 결합하여 하나의 최종 예측을 만드는 Machine Learning 방법이다.

한국어:

```text
Ensemble
→ 앙상블
→ 여러 모델을 결합
```

이라고 이해할 수 있다.

기본 구조:

```text
Model A ─┐
Model B ─┤
Model C ─┼→ 결과 결합 → Final Prediction
Model D ─┤
Model E ─┘
```

---

# 2. 왜 여러 모델을 사용할까?

모델 하나는 데이터의 모든 패턴을 완벽하게 학습하기 어렵다.

예:

```text
Model A
→ 특정 데이터에 강함

Model B
→ 다른 데이터에 강함

Model C
→ 또 다른 패턴에 강함
```

이들의 결과를 잘 결합하면 하나의 모델보다 안정적인 결과를 얻을 수 있다.

---

# 3. 쉬운 예

세 명이 문제의 정답을 예측한다고 하자.

```text
사람 A → 1

사람 B → 1

사람 C → 0
```

다수결:

```text
1이 2표

0이 1표
```

최종:

```text
1
```

이런 아이디어가 Ensemble의 가장 단순한 형태와 비슷하다.

---

# 4. Ensemble의 대표적인 방법

대표적으로:

```text
Voting

Bagging

Boosting

Stacking
```

이 있다.

큰 흐름:

```text
Ensemble
   │
   ├── Voting
   │
   ├── Bagging
   │     └── Random Forest
   │
   ├── Boosting
   │     ├── Gradient Boosting
   │     ├── XGBoost
   │     ├── LightGBM
   │     └── CatBoost
   │
   └── Stacking
```

---

# 5. Voting

Voting은 여러 모델의 예측 결과를 투표하여 최종 결과를 결정하는 방법이다.

예:

```text
Logistic Regression → 1

Random Forest       → 1

SVM                 → 0
```

다수결:

```text
Final Prediction
→ 1
```

---

# 6. Voting Classifier

Scikit-learn에서는:

```python
from sklearn.ensemble import (
    VotingClassifier
)
```

를 사용할 수 있다.

---

# 7. Hard Voting

Hard Voting은 각 모델이 예측한 Class를 직접 투표한다.

예:

```text
Model A → 1
Model B → 1
Model C → 0
```

최종:

```text
1
```

이다.

---

# 8. Hard Voting 코드

```python
from sklearn.linear_model import (
    LogisticRegression
)

from sklearn.ensemble import (
    RandomForestClassifier,
    VotingClassifier
)

from sklearn.tree import (
    DecisionTreeClassifier
)


logistic = LogisticRegression(
    max_iter=1000
)

tree = DecisionTreeClassifier(
    max_depth=5,
    random_state=42
)

rf = RandomForestClassifier(
    n_estimators=200,
    random_state=42
)


voting_model = VotingClassifier(
    estimators=[
        (
            "logistic",
            logistic
        ),
        (
            "tree",
            tree
        ),
        (
            "rf",
            rf
        )
    ],
    voting="hard"
)
```

---

# 9. Voting 모델 학습

```python
voting_model.fit(
    X_train,
    y_train
)
```

---

# 10. Voting 예측

```python
pred = voting_model.predict(
    X_test
)
```

---

# 11. Soft Voting

Soft Voting은 Class 자체를 투표하는 대신 각 모델의 예측 확률을 이용한다.

예:

```text
Model A
P(1) = 0.80

Model B
P(1) = 0.70

Model C
P(1) = 0.40
```

평균:

```text
(0.80 + 0.70 + 0.40) / 3

= 0.633...
```

Threshold가 0.5라면:

```text
Final Prediction
→ 1
```

이 될 수 있다.

---

# 12. Soft Voting 코드

```python
voting_model = VotingClassifier(
    estimators=[
        (
            "logistic",
            logistic
        ),
        (
            "rf",
            rf
        )
    ],
    voting="soft"
)
```

---

# 13. Soft Voting 조건

Soft Voting을 사용하려면 일반적으로 포함된 모델들이 Class Probability를 제공할 수 있어야 한다.

예:

```python
model.predict_proba()
```

를 사용할 수 있는 모델들이다.

---

# 14. Hard vs Soft Voting

```text
Hard Voting
→ 최종 Class를 투표

Soft Voting
→ 예측 확률을 결합
```

이다.

---

# 15. Weighted Voting

모든 모델을 똑같이 취급하지 않고 특정 모델에 더 큰 Weight를 줄 수도 있다.

예:

```python
voting_model = VotingClassifier(
    estimators=[
        (
            "logistic",
            logistic
        ),
        (
            "rf",
            rf
        )
    ],
    voting="soft",
    weights=[
        1,
        2
    ]
)
```

의미:

```text
Logistic → Weight 1

Random Forest → Weight 2
```

이다.

하지만 Weight도 Validation 성능을 근거로 결정해야 한다.

---

# 16. Bagging

Bagging은:

```text
Bootstrap Aggregating
```

에서 나온 말이다.

핵심 아이디어:

```text
Training Data에서
여러 개의 Sample Dataset 생성
        ↓
각 Dataset으로 모델 학습
        ↓
결과 결합
```

이다.

---

# 17. Bootstrap Sampling

원본 Training Data:

```text
A
B
C
D
E
```

가 있다고 하자.

Bootstrap Sample 1:

```text
A
A
C
D
E
```

Bootstrap Sample 2:

```text
B
C
C
D
D
```

Bootstrap Sample 3:

```text
A
B
B
E
E
```

처럼 복원 추출을 통해 여러 Training Sample을 만들 수 있다.

---

# 18. 복원 추출

Bootstrap Sampling에서는 한 번 선택된 Sample이 다시 선택될 수 있다.

그래서:

```text
A
A
C
D
E
```

처럼 같은 Sample이 여러 번 등장할 수 있다.

---

# 19. Bagging 구조

```text
Original Train Data
        ↓
 ┌──────┼──────┐
 ↓      ↓      ↓
Sample1 Sample2 Sample3
 ↓      ↓      ↓
Model1  Model2  Model3
 └──────┼──────┘
        ↓
     Aggregate
        ↓
 Final Prediction
```

---

# 20. Bagging의 목적

Decision Tree 하나는 Training Data 변화에 민감할 수 있다.

Training Data가 조금만 달라져도 Tree 구조가 크게 달라질 수 있다.

여러 Tree를 서로 다른 Sample에서 학습시키고 결과를 평균 또는 투표하면 예측의 변동성을 줄이는 데 도움이 될 수 있다.

---

# 21. BaggingClassifier

Scikit-learn:

```python
from sklearn.ensemble import (
    BaggingClassifier
)
```

---

# 22. Bagging 예

```python
from sklearn.tree import (
    DecisionTreeClassifier
)

from sklearn.ensemble import (
    BaggingClassifier
)


tree = DecisionTreeClassifier(
    random_state=42
)


model = BaggingClassifier(
    estimator=tree,
    n_estimators=100,
    random_state=42
)
```

Scikit-learn 버전에 따라 오래된 코드에서는 `base_estimator`가 보일 수도 있다.

현재 코드를 작성할 때는 설치된 버전의 API를 확인한다.

---

# 23. Bagging 학습

```python
model.fit(
    X_train,
    y_train
)
```

---

# 24. Bagging 예측

```python
pred = model.predict(
    X_test
)
```

---

# 25. Random Forest

Random Forest는 대표적인 Bagging 계열 Ensemble 모델이다.

기본 아이디어:

```text
Decision Tree 여러 개
+
Bootstrap Sampling
+
Feature Randomness
```

이다.

---

# 26. Random Forest 구조

```text
Training Data
     ↓
┌────┼────┐
↓    ↓    ↓
Tree Tree Tree
 1    2    3
↓    ↓    ↓
└────┼────┘
     ↓
Voting / Average
     ↓
Prediction
```

---

# 27. Random이라는 이름의 이유

Random Forest에서는 Tree마다:

```text
서로 다른 Sample

서로 다른 Feature 후보
```

를 사용하도록 Randomness를 넣는다.

이 때문에 Tree들이 모두 똑같은 구조가 되는 것을 줄일 수 있다.

---

# 28. 왜 Tree를 다르게 만들까?

Tree 100개가 모두 완전히 똑같다면:

```text
같은 실수
×
100
```

이 될 뿐이다.

Ensemble의 효과를 얻으려면 모델들 사이에 어느 정도 차이가 있는 것이 중요하다.

---

# 29. RandomForestClassifier

```python
from sklearn.ensemble import (
    RandomForestClassifier
)


model = RandomForestClassifier(
    n_estimators=200,
    random_state=42
)
```

---

# 30. RandomForestRegressor

회귀에서는:

```python
from sklearn.ensemble import (
    RandomForestRegressor
)


model = RandomForestRegressor(
    n_estimators=200,
    random_state=42
)
```

를 사용할 수 있다.

---

# 31. Classification에서 결과 결합

분류에서는 여러 Tree의 결과를 종합하여 Class를 결정한다.

개념적으로:

```text
Tree 1 → 1
Tree 2 → 0
Tree 3 → 1
Tree 4 → 1
Tree 5 → 0

Final
→ 1
```

처럼 생각할 수 있다.

실제 `RandomForestClassifier`의 확률 계산과 예측은 각 Tree의 확률 출력 등을 종합하는 방식으로 구현된다.

---

# 32. Regression에서 결과 결합

회귀:

```text
Tree 1 → 120

Tree 2 → 130

Tree 3 → 125
```

평균:

```text
125
```

처럼 여러 Tree의 예측을 평균하여 최종 예측을 만들 수 있다.

---

# 33. Random Forest 주요 Hyperparameter

대표적으로:

```text
n_estimators

max_depth

min_samples_split

min_samples_leaf

max_features

bootstrap

class_weight
```

등이 있다.

---

# 34. n_estimators

```python
n_estimators=200
```

은 Forest에 사용할 Tree의 개수와 관련된다.

Tree 수를 늘리면 일반적으로 계산량과 메모리 사용도 증가한다.

무조건 많이 만든다고 성능이 계속 향상되는 것은 아니다.

---

# 35. max_depth

```python
max_depth=10
```

은 각 Tree의 최대 깊이를 제한한다.

너무 깊으면:

```text
Overfitting
```

가능성이 커질 수 있다.

너무 얕으면:

```text
Underfitting
```

가능성이 있다.

---

# 36. max_features

Tree의 분할을 만들 때 고려할 Feature 수를 조절한다.

이를 통해 Tree들 사이에 다양성을 만들 수 있다.

---

# 37. Random Forest와 Scaling

Random Forest는 Tree 기반 모델이기 때문에 일반적으로 StandardScaler 같은 Scaling에 상대적으로 덜 민감하다.

```text
Logistic Regression
→ Scaling 고려

Ridge
→ Scaling 중요

Random Forest
→ Scaling 영향 상대적으로 적음
```

이다.

---

# 38. Bagging 핵심

한 줄로:

```text
여러 모델을 비교적 독립적으로 학습하고
결과를 합친다.
```

라고 기억할 수 있다.

---

# 39. Boosting

Boosting 역시 여러 모델을 결합하지만 Bagging과 접근 방식이 다르다.

핵심:

```text
Model 1 학습
    ↓
부족한 부분 확인
    ↓
Model 2가 보완
    ↓
부족한 부분 확인
    ↓
Model 3가 보완
    ↓
...
```

이다.

---

# 40. Bagging과 Boosting 비교

Bagging:

```text
Model 1
Model 2
Model 3

비교적 독립적으로 학습
↓
결과 결합
```

Boosting:

```text
Model 1
  ↓
Model 2가 보완
  ↓
Model 3가 보완
  ↓
결과 결합
```

이다.

---

# 41. 쉬운 비유

Bagging:

```text
여러 사람이
각자 문제를 풀고
마지막에 의견을 합침
```

Boosting:

```text
첫 사람이 문제를 풀고
틀린 부분을 다음 사람이 보고
그 부분을 보완
```

하는 느낌이다.

---

# 42. Gradient Boosting

Gradient Boosting은 대표적인 Boosting 방법이다.

여러 개의 약한 모델을 순차적으로 추가하면서 이전 모델의 오차를 줄이는 방향으로 학습한다.

---

# 43. Weak Learner

Boosting에서는 각각의 개별 모델을:

```text
Weak Learner
```

라고 부르기도 한다.

보통 얕은 Decision Tree 등이 사용될 수 있다.

---

# 44. GradientBoostingClassifier

```python
from sklearn.ensemble import (
    GradientBoostingClassifier
)


model = GradientBoostingClassifier(
    n_estimators=100,
    learning_rate=0.05,
    max_depth=3,
    random_state=42
)
```

---

# 45. GradientBoostingRegressor

```python
from sklearn.ensemble import (
    GradientBoostingRegressor
)


model = GradientBoostingRegressor(
    n_estimators=100,
    learning_rate=0.05,
    max_depth=3,
    random_state=42
)
```

---

# 46. learning_rate

Boosting에서 매우 중요한 Hyperparameter이다.

```python
learning_rate=0.05
```

는 각 단계에서 새 모델의 기여 정도를 조절한다.

개념적으로:

```text
작은 learning_rate
→ 한 번에 조금씩 보완

큰 learning_rate
→ 한 번에 더 크게 보완
```

이라고 볼 수 있다.

---

# 47. learning_rate와 n_estimators

두 값은 서로 연결해서 생각하는 경우가 많다.

예:

```text
learning_rate ↓
n_estimators ↑
```

처럼 작은 Learning Rate로 더 많은 Tree를 사용하는 조합을 실험할 수 있다.

하지만 최적값은 데이터에 따라 다르다.

---

# 48. Boosting의 장점

Tabular Data에서 강력한 성능을 보이는 경우가 많다.

특히:

```text
XGBoost

LightGBM

CatBoost
```

같은 모델들이 많이 사용된다.

---

# 49. Boosting의 주의점

모델을 계속 추가하면 Training Data에 지나치게 맞을 수 있다.

따라서:

```text
n_estimators

learning_rate

max_depth

regularization
```

등을 조절해야 한다.

---

# 50. XGBoost

XGBoost는:

```text
Extreme Gradient Boosting
```

의 약자이다.

Gradient Boosting 계열의 대표적인 Library/Algorithm이다.

---

# 51. XGBoost 설치

환경에 없다면:

```powershell
pip install xgboost
```

Jupyter/Colab에서는 환경에 따라:

```python
!pip install xgboost
```

를 사용할 수 있다.

---

# 52. XGBClassifier

```python
from xgboost import (
    XGBClassifier
)


model = XGBClassifier(
    n_estimators=500,
    learning_rate=0.05,
    max_depth=5,
    random_state=42
)
```

---

# 53. XGBRegressor

```python
from xgboost import (
    XGBRegressor
)


model = XGBRegressor(
    n_estimators=500,
    learning_rate=0.05,
    max_depth=5,
    random_state=42
)
```

---

# 54. XGBoost 주요 Hyperparameter

대표적으로:

```text
n_estimators

learning_rate

max_depth

min_child_weight

subsample

colsample_bytree

reg_alpha

reg_lambda
```

등이 있다.

---

# 55. subsample

```python
subsample=0.8
```

처럼 Training Sample 일부를 각 단계에서 사용하도록 설정할 수 있다.

이는 Randomness를 추가하고 Overfitting을 줄이는 데 도움이 될 수 있다.

---

# 56. colsample_bytree

```python
colsample_bytree=0.8
```

은 Tree를 만들 때 사용할 Feature의 비율과 관련된다.

예:

```text
전체 Feature의 약 80%
```

를 사용하도록 설정할 수 있다.

---

# 57. reg_alpha

```text
L1 Regularization
```

과 관련된 Hyperparameter이다.

---

# 58. reg_lambda

```text
L2 Regularization
```

과 관련된 Hyperparameter이다.

---

# 59. LightGBM

LightGBM은:

```text
Light Gradient Boosting Machine
```

의 약자이다.

대규모 Tabular Data에서 빠른 학습과 좋은 성능을 보이는 경우가 많다.

---

# 60. LightGBM 설치

```powershell
pip install lightgbm
```

Jupyter/Colab:

```python
!pip install lightgbm
```

---

# 61. LGBMClassifier

```python
from lightgbm import (
    LGBMClassifier
)


model = LGBMClassifier(
    n_estimators=500,
    learning_rate=0.05,
    random_state=42
)
```

---

# 62. LGBMRegressor

```python
from lightgbm import (
    LGBMRegressor
)


model = LGBMRegressor(
    n_estimators=500,
    learning_rate=0.05,
    random_state=42
)
```

---

# 63. LightGBM 주요 Hyperparameter

대표적으로:

```text
n_estimators

learning_rate

num_leaves

max_depth

min_child_samples

subsample

colsample_bytree

reg_alpha

reg_lambda
```

등이 있다.

---

# 64. num_leaves

LightGBM에서 Tree의 복잡도와 관련된 중요한 Hyperparameter 중 하나이다.

너무 크게 설정하면 복잡한 패턴을 학습할 수 있지만 Overfitting 가능성도 커질 수 있다.

---

# 65. CatBoost

CatBoost는 Gradient Boosting 계열의 모델이다.

특히:

```text
Categorical Feature
```

를 다루는 기능으로 많이 알려져 있다.

---

# 66. CatBoost 설치

```powershell
pip install catboost
```

Jupyter/Colab:

```python
!pip install catboost
```

---

# 67. CatBoostClassifier

```python
from catboost import (
    CatBoostClassifier
)


model = CatBoostClassifier(
    iterations=500,
    learning_rate=0.05,
    depth=6,
    random_seed=42,
    verbose=0
)
```

---

# 68. CatBoostRegressor

```python
from catboost import (
    CatBoostRegressor
)


model = CatBoostRegressor(
    iterations=500,
    learning_rate=0.05,
    depth=6,
    random_seed=42,
    verbose=0
)
```

---

# 69. CatBoost의 iterations

CatBoost에서는:

```python
iterations=500
```

처럼 Boosting 반복 횟수를 설정한다.

다른 Library의:

```text
n_estimators
```

와 비슷한 역할로 이해할 수 있다.

---

# 70. CatBoost depth

```python
depth=6
```

은 Tree 깊이와 관련된다.

너무 깊으면 모델 복잡도가 증가하고 Overfitting 가능성이 높아질 수 있다.

---

# 71. 세 Boosting 모델

```text
XGBoost

LightGBM

CatBoost
```

는 모두 Boosting 계열이지만 내부 구현과 Tree 성장 방식, 범주형 데이터 처리, 속도, Hyperparameter 등이 서로 다르다.

따라서 이름만 다른 동일 모델은 아니다.

---

# 72. 어떤 모델이 가장 좋은가?

항상:

```text
XGBoost가 최고

LightGBM이 최고

CatBoost가 최고
```

라고 정할 수 없다.

데이터에 따라 달라진다.

따라서:

```text
같은 데이터

같은 Split

같은 Metric
```

조건에서 비교한다.

---

# 73. 모델 비교 예

```python
models = {
    "RandomForest":
        RandomForestClassifier(
            n_estimators=200,
            random_state=42
        ),

    "XGBoost":
        XGBClassifier(
            n_estimators=300,
            learning_rate=0.05,
            random_state=42
        ),

    "LightGBM":
        LGBMClassifier(
            n_estimators=300,
            learning_rate=0.05,
            random_state=42
        ),

    "CatBoost":
        CatBoostClassifier(
            iterations=300,
            learning_rate=0.05,
            random_seed=42,
            verbose=0
        )
}
```

---

# 74. 반복 학습

```python
from sklearn.metrics import (
    accuracy_score
)


for name, model in models.items():

    model.fit(
        X_train,
        y_train
    )

    pred = model.predict(
        X_valid
    )

    score = accuracy_score(
        y_valid,
        pred
    )

    print(
        name,
        score
    )
```

---

# 75. 하지만 Accuracy만 보면 안 된다

Class Imbalance가 있다면:

```text
Accuracy

Precision

Recall

F1

ROC-AUC

PR-AUC
```

등을 함께 확인해야 한다.

---

# 76. Boosting에서도 predict_proba

분류 모델:

```python
prob = model.predict_proba(
    X_valid
)[:, 1]
```

처럼 Positive Class 확률을 얻을 수 있다.

---

# 77. ROC-AUC 비교

```python
from sklearn.metrics import (
    roc_auc_score
)


prob = model.predict_proba(
    X_valid
)[:, 1]

auc = roc_auc_score(
    y_valid,
    prob
)

print(
    auc
)
```

---

# 78. Feature Importance

Tree Ensemble 모델은 Feature Importance를 제공하는 경우가 많다.

예:

```python
model.feature_importances_
```

---

# 79. Feature Importance 정리

```python
import pandas as pd


importance_df = pd.DataFrame(
    {
        "feature":
            X_train.columns,

        "importance":
            model.feature_importances_
    }
)


importance_df = (
    importance_df
    .sort_values(
        "importance",
        ascending=False
    )
)


print(
    importance_df
)
```

---

# 80. Feature Importance 해석

중요도가 높은 Feature는:

```text
모델이 예측 과정에서
상대적으로 많이 활용한 Feature
```

라고 볼 수 있다.

하지만:

```text
중요도 높음
=
원인
```

은 아니다.

---

# 81. Stacking

Stacking도 Ensemble 방법이다.

하지만 Voting과는 조금 다르다.

Voting:

```text
여러 모델의 결과
↓
투표 / 평균
```

Stacking:

```text
여러 모델의 예측
↓
새로운 Feature
↓
다른 모델이 다시 학습
```

이다.

---

# 82. Stacking 기본 구조

```text
Original Features
      ↓
 ┌────┼────┐
 ↓    ↓    ↓
RF   XGB   LGBM
 ↓    ↓    ↓
P1   P2    P3
 └────┼────┘
      ↓
Meta Model
      ↓
Final Prediction
```

---

# 83. Meta Model

첫 번째 단계 모델들의 예측값을 입력받아 최종 예측을 만드는 모델을:

```text
Meta Model
```

또는:

```text
Meta Learner
```

라고 부른다.

---

# 84. 예

첫 번째 모델:

```text
Random Forest
```

두 번째:

```text
XGBoost
```

세 번째:

```text
LightGBM
```

이 각각:

```text
0.72

0.81

0.76
```

이라는 확률을 만들었다고 하자.

Meta Model은:

```text
RF_prob
XGB_prob
LGBM_prob
```

를 새로운 Feature처럼 사용할 수 있다.

---

# 85. Stacking에서 중요한 문제

Training Data를 학습한 모델이 같은 Training Data를 다시 예측하면 지나치게 좋은 예측값이 나올 수 있다.

이를 그대로 Meta Model에 넣으면:

```text
Data Leakage

Overfitting
```

문제가 발생할 수 있다.

---

# 86. 그래서 OOF가 중요

Stacking에서는:

```text
OOF
```

예측을 자주 사용한다.

OOF:

```text
Out-Of-Fold
```

이다.

---

# 87. OOF 기본 아이디어

예를 들어 5-Fold라면:

```text
Fold 1
→ 나머지 4개 Fold로 학습
→ Fold 1 예측

Fold 2
→ 나머지 4개 Fold로 학습
→ Fold 2 예측

...

Fold 5
→ 나머지 4개 Fold로 학습
→ Fold 5 예측
```

한다.

---

# 88. 이렇게 만든 예측

각 Sample은:

```text
자기 자신을 학습에 사용하지 않은 모델
```

에게서 예측을 받는다.

이 예측을 모으면:

```text
OOF Prediction
```

이 된다.

---

# 89. OOF와 Stacking

```text
Training Data
      ↓
Cross Validation
      ↓
OOF Predictions
      ↓
Meta Features
      ↓
Meta Model
```

구조가 된다.

이 내용은 뒤의:

```text
stacking-oof.md
```

에서 자세히 다룬다.

---

# 90. Ensemble이라고 무조건 좋아질까?

아니다.

예를 들어 세 모델이 모두 거의 같은 방식으로 틀린다면:

```text
같은 오류
+
같은 오류
+
같은 오류
```

를 결합하는 것에 불과할 수 있다.

---

# 91. Diversity

Ensemble에서는 모델 간:

```text
Diversity
```

즉 다양성이 중요할 수 있다.

예:

```text
Logistic Regression

Random Forest

XGBoost
```

처럼 서로 다른 특성을 가진 모델을 결합할 수 있다.

---

# 92. 하지만 다양성만 보면 안 된다

성능이 매우 나쁜 모델을 단순히 다르다는 이유만으로 추가하면 Ensemble 성능이 떨어질 수도 있다.

따라서:

```text
개별 모델 성능

모델 간 오류 패턴

예측 상관관계

Validation 결과
```

등을 함께 확인한다.

---

# 93. 예측 확률 상관관계

모델 A와 B의 예측 확률이 거의 똑같다면 두 모델이 매우 비슷하게 행동하고 있을 수 있다.

예:

```python
prediction_df = pd.DataFrame(
    {
        "rf": rf_prob,
        "xgb": xgb_prob,
        "lgbm": lgbm_prob
    }
)


print(
    prediction_df.corr()
)
```

---

# 94. 상관관계가 높다고 무조건 나쁜가?

아니다.

하지만 Ensemble을 만들 때:

```text
모델들이 얼마나 다른 판단을 하는가?
```

를 확인하는 하나의 방법으로 사용할 수 있다.

---

# 95. Bagging vs Boosting vs Stacking

```text
Bagging
→ 여러 모델을 병렬적으로 학습
→ 결과 평균/투표

Boosting
→ 모델을 순차적으로 학습
→ 앞 모델의 부족한 부분 보완

Stacking
→ 여러 모델의 예측을
→ 다른 모델의 Feature로 사용
```

이다.

---

# 96. Voting vs Stacking

Voting:

```text
Model A ─┐
Model B ─┼→ 투표/평균
Model C ─┘
```

Stacking:

```text
Model A ─┐
Model B ─┼→ 예측값 → Meta Model → Final
Model C ─┘
```

이다.

---

# 97. Random Forest는 어디에 속할까?

```text
Ensemble
   ↓
Bagging 계열
   ↓
Random Forest
```

이다.

---

# 98. XGBoost는 어디에 속할까?

```text
Ensemble
   ↓
Boosting
   ↓
Gradient Boosting 계열
   ↓
XGBoost
```

이다.

---

# 99. LightGBM은?

```text
Ensemble
   ↓
Boosting
   ↓
Gradient Boosting 계열
   ↓
LightGBM
```

이다.

---

# 100. CatBoost는?

```text
Ensemble
   ↓
Boosting
   ↓
Gradient Boosting 계열
   ↓
CatBoost
```

이다.

---

# 101. Ensemble 전체 그림

```text
Machine Learning
      ↓
Ensemble
      │
      ├── Voting
      │
      ├── Bagging
      │      ↓
      │   Random Forest
      │
      ├── Boosting
      │      ↓
      │   Gradient Boosting
      │      ↓
      │   XGBoost
      │   LightGBM
      │   CatBoost
      │
      └── Stacking
             ↓
             OOF
             ↓
          Meta Model
```

---

# 102. 모델 비교 시 중요한 원칙

예:

```text
Random Forest
XGBoost
LightGBM
CatBoost
```

를 비교한다면 가능하면:

```text
같은 Train

같은 Validation

같은 Feature

같은 Metric
```

을 사용한다.

---

# 103. 한 번의 Split만 믿지 않는다

한 번의 Train/Validation Split에서:

```text
XGBoost가 최고
```

였다고 해서 항상 최고라고 할 수 없다.

Split이 바뀌면 결과도 달라질 수 있다.

그래서:

```text
Cross Validation
```

이 중요해진다.

---

# 104. Ensemble과 Cross Validation

예:

```text
Random Forest
→ 5-Fold CV

XGBoost
→ 5-Fold CV

LightGBM
→ 5-Fold CV

CatBoost
→ 5-Fold CV
```

처럼 같은 Fold에서 모델들을 비교할 수 있다.

---

# 105. Ensemble과 Hyperparameter Tuning

모델마다 Hyperparameter가 있다.

Random Forest:

```text
n_estimators
max_depth
max_features
```

XGBoost:

```text
n_estimators
learning_rate
max_depth
subsample
colsample_bytree
```

LightGBM:

```text
num_leaves
learning_rate
n_estimators
```

CatBoost:

```text
depth
learning_rate
iterations
```

등이다.

---

# 106. 기본 모델부터 시작

처음부터 수백 가지 Hyperparameter 조합을 탐색하기보다:

```text
Baseline
   ↓
모델 비교
   ↓
가능성 있는 모델 선택
   ↓
Hyperparameter Tuning
```

순서가 효율적일 수 있다.

---

# 107. Ensemble과 Overfitting

Ensemble이라고 해서 Overfitting이 사라지는 것은 아니다.

Random Forest:

```text
Tree가 너무 복잡
```

Boosting:

```text
너무 많은 Tree
너무 큰 Depth
```

Stacking:

```text
OOF 없이 Training Prediction 사용
```

등의 문제가 있을 수 있다.

---

# 108. Train과 Validation 비교

예:

```text
Train AUC = 0.999

Validation AUC = 0.78
```

이라면 과적합 가능성을 확인한다.

---

# 109. Ensemble에서 Leakage 확인

특히 다음을 조심한다.

```text
전체 데이터 Scaling 후 CV

전체 데이터 SMOTE 후 CV

Test Data를 이용한 Hyperparameter 선택

Test Data를 이용한 Ensemble Weight 결정

OOF 없이 Stacking

Target을 이용한 Feature 생성
```

---

# 110. Test는 마지막

기본 흐름:

```text
Train
→ Model 학습

Validation / CV
→ Model 비교
→ Hyperparameter 선택
→ Ensemble 방법 선택

Test
→ 마지막 최종 평가
```

이다.

---

# 111. Competition의 Test Data

대회에서는:

```text
train.csv
```

에 Target이 있고:

```text
test.csv
```

에는 Target이 없는 경우가 많다.

이때:

```text
test.csv
→ 모델이 예측해야 하는 데이터
```

이다.

---

# 112. 여러 모델의 Test Prediction

예:

```python
rf_test_prob = rf.predict_proba(
    X_test
)[:, 1]

xgb_test_prob = xgb.predict_proba(
    X_test
)[:, 1]

lgbm_test_prob = lgbm.predict_proba(
    X_test
)[:, 1]
```

---

# 113. Probability Averaging

간단한 Ensemble:

```python
ensemble_prob = (
    rf_test_prob
    +
    xgb_test_prob
    +
    lgbm_test_prob
) / 3
```

이다.

---

# 114. 최종 Class

```python
ensemble_pred = (
    ensemble_prob >= 0.5
).astype(int)
```

처럼 Threshold를 적용할 수 있다.

---

# 115. Weighted Average

모델 성능을 고려하여:

```python
ensemble_prob = (
    0.2 * rf_test_prob
    +
    0.4 * xgb_test_prob
    +
    0.4 * lgbm_test_prob
)
```

처럼 가중 평균을 만들 수도 있다.

---

# 116. Weight는 어디서 결정?

중요:

```text
Test 점수를 보고 Weight 조정
```

을 반복하면 Test에 맞추게 된다.

따라서 Weight는:

```text
Validation

Cross Validation

OOF
```

결과를 이용하여 결정하는 것이 더 적절하다.

---

# 117. Ensemble이 효과적인 경우

예:

```text
Model A는 Sample 1에서 맞음

Model B는 Sample 2에서 맞음

Model C는 Sample 3에서 맞음
```

처럼 모델들의 오류 패턴이 서로 다르면 결합했을 때 도움이 될 수 있다.

---

# 118. Ensemble이 별 효과가 없을 수 있는 경우

```text
모델들의 예측이 거의 동일

같은 Feature

비슷한 Algorithm

같은 오류 반복
```

이라면 Ensemble 효과가 작을 수 있다.

---

# 119. 계산 비용

모델 하나:

```text
학습 1번
예측 1번
```

Ensemble:

```text
여러 모델 학습
여러 모델 저장
여러 모델 예측
결과 결합
```

이 필요하다.

따라서 성능뿐 아니라:

```text
학습 시간

메모리

Inference 속도

서비스 복잡도
```

도 고려해야 한다.

---

# 120. 실제 서비스에서는?

예:

```text
Random Forest
+
XGBoost
+
LightGBM
+
CatBoost
+
Meta Model
```

이 성능은 조금 더 좋지만 매우 무거울 수 있다.

반면:

```text
LightGBM 하나
```

가 거의 같은 성능을 내면서 훨씬 빠르다면 단일 모델이 더 적절할 수도 있다.

---

# 121. 모델 선택은 점수만의 문제가 아니다

다음을 함께 고려한다.

```text
Validation Performance

Generalization

Inference Speed

Memory

Interpretability

Maintenance

Deployment Complexity
```

---

# 122. 전체 예제

```python
from sklearn.ensemble import (
    RandomForestClassifier
)

from sklearn.metrics import (
    accuracy_score,
    roc_auc_score
)

from xgboost import (
    XGBClassifier
)

from lightgbm import (
    LGBMClassifier
)


models = {
    "RandomForest":
        RandomForestClassifier(
            n_estimators=200,
            random_state=42
        ),

    "XGBoost":
        XGBClassifier(
            n_estimators=300,
            learning_rate=0.05,
            max_depth=5,
            random_state=42
        ),

    "LightGBM":
        LGBMClassifier(
            n_estimators=300,
            learning_rate=0.05,
            random_state=42
        )
}


valid_predictions = {}


for name, model in models.items():

    model.fit(
        X_train,
        y_train
    )

    pred = model.predict(
        X_valid
    )

    prob = model.predict_proba(
        X_valid
    )[:, 1]

    accuracy = accuracy_score(
        y_valid,
        pred
    )

    auc = roc_auc_score(
        y_valid,
        prob
    )

    valid_predictions[
        name
    ] = prob

    print(
        name,
        "Accuracy:",
        accuracy,
        "ROC-AUC:",
        auc
    )
```

---

# 123. 간단한 평균 Ensemble

```python
ensemble_prob = (
    valid_predictions[
        "RandomForest"
    ]
    +
    valid_predictions[
        "XGBoost"
    ]
    +
    valid_predictions[
        "LightGBM"
    ]
) / 3
```

---

# 124. Ensemble Class

```python
ensemble_pred = (
    ensemble_prob >= 0.5
).astype(int)
```

---

# 125. Ensemble 평가

```python
ensemble_accuracy = (
    accuracy_score(
        y_valid,
        ensemble_pred
    )
)

ensemble_auc = (
    roc_auc_score(
        y_valid,
        ensemble_prob
    )
)


print(
    "Ensemble Accuracy:",
    ensemble_accuracy
)

print(
    "Ensemble ROC-AUC:",
    ensemble_auc
)
```

---

# 126. 비교

이제:

```text
RandomForest

XGBoost

LightGBM

Ensemble
```

의 결과를 비교한다.

중요한 것은:

```text
Ensemble을 했다는 이유로
무조건 Ensemble을 선택하지 않는다.
```

이다.

Validation 결과가 실제로 개선되었는지 확인한다.

---

# 127. Regression Ensemble

회귀에서도 같은 아이디어를 사용할 수 있다.

예:

```python
rf_pred = rf.predict(
    X_valid
)

xgb_pred = xgb.predict(
    X_valid
)

lgbm_pred = lgbm.predict(
    X_valid
)
```

평균:

```python
ensemble_pred = (
    rf_pred
    +
    xgb_pred
    +
    lgbm_pred
) / 3
```

---

# 128. Regression Ensemble 평가

```python
from sklearn.metrics import (
    mean_absolute_error
)


mae = mean_absolute_error(
    y_valid,
    ensemble_pred
)

print(
    "Ensemble MAE:",
    mae
)
```

---

# 129. Classification과 Regression

Classification Ensemble:

```text
Class Vote

Probability Average

Weighted Probability
```

등을 사용할 수 있다.

Regression Ensemble:

```text
Prediction Average

Weighted Prediction
```

등을 사용할 수 있다.

---

# 130. 핵심 용어

```text
Ensemble
→ 여러 모델을 결합

Voting
→ 여러 모델의 결과를 투표

Hard Voting
→ Class를 투표

Soft Voting
→ Probability를 결합

Bagging
→ 서로 다른 Sample로 여러 모델 학습 후 결합

Bootstrap
→ 복원 추출 방식

Random Forest
→ 대표적인 Bagging 계열 Tree Ensemble

Boosting
→ 이전 모델의 부족한 부분을 다음 모델이 보완

Gradient Boosting
→ Gradient 기반으로 오차를 줄여가는 Boosting

XGBoost
→ Gradient Boosting 계열 모델

LightGBM
→ Gradient Boosting 계열 모델

CatBoost
→ Gradient Boosting 계열 모델

Stacking
→ 여러 모델의 예측을 Meta Model의 Feature로 사용

Meta Model
→ Base Model들의 예측을 이용해 최종 예측

OOF
→ Out-Of-Fold Prediction
```

---

# 131. 가장 중요한 차이

```text
Voting
→ 결과를 투표

Bagging
→ 여러 모델을 비교적 독립적으로 학습

Boosting
→ 순차적으로 오류를 보완

Stacking
→ 다른 모델의 예측을 다시 학습
```

---

# 132. Random Forest 기억하기

```text
Decision Tree 하나
        ↓
불안정할 수 있음

Decision Tree 여러 개
        ↓
서로 다른 Sample / Feature
        ↓
결과 결합
        ↓
Random Forest
```

---

# 133. Boosting 기억하기

```text
Model 1
   ↓
틀린 부분
   ↓
Model 2
   ↓
틀린 부분
   ↓
Model 3
   ↓
...
   ↓
Final Prediction
```

---

# 134. Stacking 기억하기

```text
Random Forest ─┐
XGBoost ───────┼→ OOF Prediction
LightGBM ──────┘
                      ↓
                 Meta Features
                      ↓
                  Meta Model
                      ↓
               Final Prediction
```

---

# 135. 배운 점

Ensemble Learning은:

```text
여러 모델을 결합하여
더 좋은 예측을 만드는 방법
```

이다.

대표적인 방법은:

```text
Voting

Bagging

Boosting

Stacking
```

이다.

Random Forest는:

```text
Bagging 계열
```

로 볼 수 있고:

```text
XGBoost
LightGBM
CatBoost
```

는:

```text
Boosting 계열
```

이다.

Stacking에서는:

```text
Base Model Prediction
↓
Meta Feature
↓
Meta Model
```

구조를 사용한다.

특히 Training Data에서 Stacking Feature를 만들 때는:

```text
OOF Prediction
```

이 중요하다.

하지만 Ensemble이라고 무조건 성능이 좋아지는 것은 아니다.

반드시:

```text
같은 Validation

같은 Metric

Cross Validation

Overfitting 확인

Data Leakage 확인
```

을 통해 실제 개선 여부를 검증해야 한다.

가장 중요한 것은:

```text
모델을 많이 사용하는 것
```

이 아니라:

```text
서로 다른 장점을 가진 모델들을
올바른 검증 방법으로 결합하는 것
```

이다.