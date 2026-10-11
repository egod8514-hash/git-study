# Hyperparameter Tuning 하이퍼파라미터 튜닝

## 1. Hyperparameter Tuning이란?

Machine Learning 모델에는 모델의 동작 방식을 조절하는 여러 설정값이 있다.

예를 들어 Random Forest:

```python
RandomForestClassifier(
    n_estimators=200,
    max_depth=10,
    min_samples_split=5,
    random_state=42
)
```

여기서:

```text
n_estimators
max_depth
min_samples_split
```

등이 Hyperparameter이다.

이 값들을 적절하게 찾는 과정을:

```text
Hyperparameter Tuning
```

이라고 한다.

---

# 2. Parameter와 Hyperparameter

두 개를 구분해야 한다.

```text
Parameter
→ 모델이 학습 과정에서 직접 학습

Hyperparameter
→ 사람이 학습 전에 설정
```

---

# 3. Parameter 예

Linear Regression:

```text
Coefficient
Intercept
```

Logistic Regression:

```text
Coefficient
Intercept
```

Neural Network:

```text
Weight
Bias
```

등은 학습 과정에서 모델이 데이터로부터 결정한다.

---

# 4. Hyperparameter 예

Random Forest:

```text
n_estimators
max_depth
min_samples_split
min_samples_leaf
max_features
```

XGBoost:

```text
learning_rate
n_estimators
max_depth
subsample
colsample_bytree
```

등은 우리가 설정한다.

---

# 5. 가장 쉬운 구분

```text
model.fit()
```

을 실행하면서 모델이 배우는 값:

```text
Parameter
```

`fit()` 전에 우리가 지정하는 설정:

```text
Hyperparameter
```

라고 생각하면 이해하기 쉽다.

---

# 6. 왜 Tuning이 필요한가?

같은 모델이라도 Hyperparameter에 따라 성능이 달라질 수 있다.

예:

```text
RandomForest

max_depth=2
→ 너무 단순

max_depth=10
→ 적절할 수 있음

max_depth=None
→ 너무 복잡할 수도 있음
```

따라서 여러 값을 비교한다.

---

# 7. 하지만 무조건 Tuning부터 하면 안 된다

좋은 흐름:

```text
Baseline
   ↓
기본 성능 확인
   ↓
문제점 분석
   ↓
유망한 모델 선택
   ↓
Hyperparameter Tuning
```

이다.

처음부터 수백 개 조합을 돌리는 것은 비효율적일 수 있다.

---

# 8. Manual Tuning

가장 단순한 방법은 사람이 직접 값을 바꾸는 것이다.

```python
model = RandomForestClassifier(
    n_estimators=100,
    max_depth=5,
    random_state=42
)
```

실행 후:

```text
AUC = 0.82
```

다시:

```python
model = RandomForestClassifier(
    n_estimators=300,
    max_depth=10,
    random_state=42
)
```

실행:

```text
AUC = 0.86
```

처럼 비교할 수 있다.

---

# 9. Manual Tuning의 문제

후보가 많아지면 사람이 직접 비교하기 어렵다.

예:

```text
n_estimators
→ 100, 300, 500

max_depth
→ 3, 5, 10, None

min_samples_split
→ 2, 5, 10
```

조합이 빠르게 증가한다.

---

# 10. Grid Search

Grid Search는 미리 지정한 모든 Hyperparameter 조합을 체계적으로 비교하는 방법이다.

예:

```text
max_depth
→ 3, 5

n_estimators
→ 100, 300
```

조합:

```text
3 + 100
3 + 300
5 + 100
5 + 300
```

총:

```text
4개 조합
```

이다.

---

# 11. GridSearchCV

Scikit-learn:

```python
from sklearn.model_selection import (
    GridSearchCV
)
```

`CV`는:

```text
Cross Validation
```

을 의미한다.

즉:

```text
Grid Search
+
Cross Validation
```

이다.

---

# 12. 모델 준비

```python
from sklearn.ensemble import (
    RandomForestClassifier
)


model = RandomForestClassifier(
    random_state=42
)
```

---

# 13. Parameter Grid

```python
param_grid = {
    "n_estimators": [
        100,
        300
    ],

    "max_depth": [
        5,
        10,
        None
    ],

    "min_samples_split": [
        2,
        5
    ]
}
```

---

# 14. 몇 개 조합인가?

```text
n_estimators
→ 2개

max_depth
→ 3개

min_samples_split
→ 2개
```

따라서:

```text
2 × 3 × 2
=
12개 조합
```

이다.

---

# 15. 5-Fold CV까지 하면?

각 조합마다 5번 학습한다.

```text
12개 조합
×
5 Fold

=
60번 학습
```

이다.

그래서 Grid Search는 계산량이 커질 수 있다.

---

# 16. GridSearchCV 생성

```python
grid_search = GridSearchCV(
    estimator=model,
    param_grid=param_grid,
    cv=5,
    scoring="roc_auc",
    n_jobs=-1
)
```

---

# 17. estimator

```python
estimator=model
```

은:

```text
어떤 모델의 Hyperparameter를 찾을 것인가?
```

를 의미한다.

---

# 18. param_grid

```python
param_grid=param_grid
```

은:

```text
어떤 Hyperparameter 후보들을 비교할 것인가?
```

를 의미한다.

---

# 19. cv

```python
cv=5
```

는:

```text
5-Fold Cross Validation
```

을 사용한다는 뜻이다.

---

# 20. scoring

```python
scoring="roc_auc"
```

는 어떤 기준으로 가장 좋은 Hyperparameter를 선택할지 정한다.

예:

```text
accuracy
f1
recall
precision
roc_auc
```

등을 사용할 수 있다.

---

# 21. n_jobs

```python
n_jobs=-1
```

은 가능한 CPU Core들을 병렬 처리에 활용하도록 요청하는 설정이다.

환경에 따라 속도 향상에 도움이 될 수 있다.

---

# 22. Grid Search 실행

```python
grid_search.fit(
    X_train,
    y_train
)
```

이 한 줄을 실행하면 내부적으로 여러 Hyperparameter 조합과 Cross Validation 학습이 반복된다.

---

# 23. 가장 좋은 Hyperparameter

```python
print(
    grid_search.best_params_
)
```

예:

```text
{
    'max_depth': 10,
    'min_samples_split': 5,
    'n_estimators': 300
}
```

---

# 24. 가장 좋은 CV Score

```python
print(
    grid_search.best_score_
)
```

예:

```text
0.8724
```

이다.

---

# 25. 가장 좋은 모델

```python
best_model = (
    grid_search.best_estimator_
)
```

이렇게 가져올 수 있다.

---

# 26. Validation 또는 Test 예측

```python
pred = best_model.predict(
    X_test
)
```

확률:

```python
prob = best_model.predict_proba(
    X_test
)[:, 1]
```

---

# 27. best_score_와 Test Score는 다르다

중요하다.

```text
best_score_
→ Cross Validation에서 얻은 Score

Test Score
→ 마지막 Test Data에서 얻은 Score
```

이다.

둘은 같은 값이 아니다.

---

# 28. Grid Search 결과 전체 보기

```python
import pandas as pd


result = pd.DataFrame(
    grid_search.cv_results_
)


print(
    result.head()
)
```

---

# 29. 중요한 결과 Column

대표적으로:

```text
params

mean_test_score

std_test_score

rank_test_score
```

등이 있다.

---

# 30. 보기 좋게 정렬

```python
result = result[
    [
        "params",
        "mean_test_score",
        "std_test_score",
        "rank_test_score"
    ]
].sort_values(
    "rank_test_score"
)


print(
    result.head(10)
)
```

---

# 31. StratifiedKFold와 같이 사용

Classification이라면 직접:

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

를 만들 수 있다.

---

# 32. GridSearchCV에 전달

```python
grid_search = GridSearchCV(
    estimator=model,
    param_grid=param_grid,
    cv=skf,
    scoring="roc_auc",
    n_jobs=-1
)
```

이렇게 하면 우리가 정의한 Fold 방식을 사용한다.

---

# 33. Pipeline과 GridSearchCV

전처리가 필요한 모델에서는 Pipeline을 사용하는 것이 안전하다.

예:

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

# 34. Pipeline Hyperparameter 이름

Pipeline 내부 모델의 Hyperparameter를 지정할 때:

```text
단계이름__Hyperparameter
```

형식을 사용한다.

예:

```python
param_grid = {
    "model__C": [
        0.01,
        0.1,
        1,
        10
    ]
}
```

중간의:

```text
__
```

는 underscore 두 개이다.

---

# 35. 왜 model__C인가?

Pipeline에서:

```python
(
    "model",
    LogisticRegression()
)
```

라고 이름을 `model`로 지정했기 때문이다.

따라서:

```text
model
+
__
+
C
```

결과:

```text
model__C
```

이다.

---

# 36. Pipeline + GridSearchCV

```python
grid_search = GridSearchCV(
    estimator=pipeline,
    param_grid=param_grid,
    cv=skf,
    scoring="roc_auc",
    n_jobs=-1
)


grid_search.fit(
    X,
    y
)
```

이렇게 하면 각 Fold 안에서 Scaling과 모델 학습이 수행된다.

---

# 37. Pipeline을 사용하는 이유

잘못된 구조:

```text
전체 데이터 Scaling
↓
GridSearchCV
```

는 Leakage를 만들 수 있다.

올바른 구조:

```text
GridSearchCV
   ↓
각 Fold
   ↓
Train Fold에서 Scaler fit
   ↓
Validation Fold transform
   ↓
Model 평가
```

이다.

---

# 38. Randomized Search

Grid Search는 모든 조합을 확인한다.

Randomized Search는 후보 공간에서 일정 수의 조합을 무작위로 선택하여 평가한다.

---

# 39. 왜 Randomized Search?

Hyperparameter가 많아지면 Grid Search 조합 수가 폭발한다.

예:

```text
5 × 5 × 5 × 5 × 5

=
3125개 조합
```

5-Fold라면:

```text
3125 × 5

=
15625번 학습
```

이 될 수 있다.

---

# 40. RandomizedSearchCV

```python
from sklearn.model_selection import (
    RandomizedSearchCV
)
```

---

# 41. Parameter Distribution

```python
param_dist = {
    "n_estimators": [
        100,
        200,
        300,
        500,
        800
    ],

    "max_depth": [
        3,
        5,
        7,
        10,
        None
    ],

    "min_samples_split": [
        2,
        5,
        10
    ]
}
```

---

# 42. RandomizedSearchCV 생성

```python
random_search = RandomizedSearchCV(
    estimator=model,
    param_distributions=param_dist,
    n_iter=20,
    cv=skf,
    scoring="roc_auc",
    random_state=42,
    n_jobs=-1
)
```

---

# 43. n_iter

```python
n_iter=20
```

은:

```text
20개의 Hyperparameter 조합을 뽑아서 평가
```

한다는 뜻이다.

---

# 44. 실행

```python
random_search.fit(
    X_train,
    y_train
)
```

---

# 45. 결과

```python
print(
    random_search.best_params_
)

print(
    random_search.best_score_
)
```

---

# 46. Grid vs Randomized Search

```text
GridSearchCV
→ 지정한 모든 조합 평가

RandomizedSearchCV
→ 일부 조합을 Random하게 평가
```

이다.

---

# 47. 언제 Grid Search?

후보가 적고 탐색 공간이 작다면:

```text
Grid Search
```

가 이해하기 쉽고 체계적이다.

---

# 48. 언제 Randomized Search?

Hyperparameter가 많고 탐색 공간이 크다면:

```text
Randomized Search
```

로 넓게 탐색하는 것이 효율적일 수 있다.

---

# 49. Coarse to Fine

실전에서는:

```text
넓게 탐색
↓
좋은 영역 발견
↓
그 주변을 세밀하게 탐색
```

하는 방법을 사용할 수 있다.

예:

```text
RandomizedSearch
↓
좋은 영역 발견
↓
GridSearch
```

이다.

---

# 50. Bayesian Optimization

Grid Search와 Random Search는 이전 Trial 결과를 적극적으로 활용하지 않고 후보를 탐색한다.

Bayesian Optimization은:

```text
이전에 시도한 결과
```

를 이용하여:

```text
다음에는 어디를 탐색하면 좋을지
```

결정하는 방식이다.

---

# 51. 쉬운 비유

Grid Search:

```text
지도에 표시된 장소를
전부 방문
```

Random Search:

```text
지도에서 무작위로
몇 곳 방문
```

Bayesian Optimization:

```text
지금까지 방문한 결과를 보고
다음에 유망해 보이는 곳을 선택
```

이라고 이해할 수 있다.

---

# 52. Bayesian Optimization 목적

```text
적은 Trial로
좋은 Hyperparameter를 찾는 것
```

을 기대한다.

특히 한 번의 모델 학습 비용이 큰 경우 유용할 수 있다.

---

# 53. BayesSearchCV

Python에서는 `scikit-optimize`의:

```text
BayesSearchCV
```

를 사용할 수 있다.

설치:

```powershell
pip install scikit-optimize
```

Jupyter/Colab:

```python
!pip install scikit-optimize
```

---

# 54. 설치 이름과 Import 이름

여기서 헷갈리기 쉽다.

설치:

```text
scikit-optimize
```

Import:

```python
from skopt import (
    BayesSearchCV
)
```

이다.

즉:

```text
pip install scikit-optimize

from skopt import BayesSearchCV
```

이다.

---

# 55. ModuleNotFoundError

만약:

```text
ModuleNotFoundError:
No module named 'skopt'
```

가 나온다면 현재 Python 환경에 `scikit-optimize`가 설치되어 있는지 확인한다.

```powershell
python -m pip show scikit-optimize
```

---

# 56. 현재 환경에 설치

환경이 여러 개라면:

```powershell
python -m pip install scikit-optimize
```

처럼 현재 실행 중인 Python에 설치하는 방법이 안전할 수 있다.

---

# 57. BayesSearchCV 기본 Import

```python
from skopt import (
    BayesSearchCV
)

from skopt.space import (
    Integer,
    Real
)
```

---

# 58. Search Space

예:

```python
search_space = {
    "n_estimators":
        Integer(
            100,
            1000
        ),

    "max_depth":
        Integer(
            3,
            15
        ),

    "max_features":
        Real(
            0.3,
            1.0
        )
}
```

---

# 59. Integer

```python
Integer(
    3,
    15
)
```

는:

```text
3부터 15 사이의 정수
```

를 탐색한다.

---

# 60. Real

```python
Real(
    0.3,
    1.0
)
```

는:

```text
0.3부터 1.0 사이의 실수
```

를 탐색한다.

---

# 61. Log Scale

Learning Rate처럼 여러 자릿수 범위를 탐색할 때:

```python
Real(
    1e-4,
    1e-1,
    prior="log-uniform"
)
```

처럼 로그 스케일 탐색을 사용할 수 있다.

---

# 62. BayesSearchCV 생성

```python
bayes_search = BayesSearchCV(
    estimator=model,
    search_spaces=search_space,
    n_iter=30,
    cv=skf,
    scoring="roc_auc",
    random_state=42,
    n_jobs=-1
)
```

---

# 63. 실행

```python
bayes_search.fit(
    X_train,
    y_train
)
```

---

# 64. 결과

```python
print(
    bayes_search.best_params_
)

print(
    bayes_search.best_score_
)
```

---

# 65. BayesSearchCV도 Cross Validation을 사용한다

중요:

```text
Bayesian Optimization
```

과:

```text
Cross Validation
```

은 서로 다른 개념이다.

BayesSearchCV는:

```text
Bayesian 방식으로 Hyperparameter 후보 선택
+
Cross Validation으로 후보 평가
```

를 결합한 것이다.

---

# 66. Optuna

Optuna는 Hyperparameter Optimization Library이다.

설치:

```powershell
pip install optuna
```

Jupyter/Colab:

```python
!pip install optuna
```

---

# 67. Optuna 기본 구조

Optuna에서는 우리가:

```text
Objective Function
```

을 만든다.

이 함수는:

```text
Hyperparameter 선택
↓
Model 생성
↓
Model 평가
↓
Score 반환
```

을 수행한다.

---

# 68. Objective Function

예:

```python
def objective(
    trial
):

    ...
```

형태로 만든다.

---

# 69. trial

`trial`은 현재 한 번의 Hyperparameter 실험을 나타낸다.

예:

```text
Trial 0

Trial 1

Trial 2

Trial 3
```

처럼 여러 실험이 수행된다.

---

# 70. Integer Hyperparameter

```python
max_depth = trial.suggest_int(
    "max_depth",
    3,
    12
)
```

---

# 71. Float Hyperparameter

```python
learning_rate = trial.suggest_float(
    "learning_rate",
    0.001,
    0.1,
    log=True
)
```

---

# 72. Categorical Hyperparameter

```python
criterion = trial.suggest_categorical(
    "criterion",
    [
        "gini",
        "entropy"
    ]
)
```

---

# 73. Random Forest Objective 예

```python
import optuna

from sklearn.ensemble import (
    RandomForestClassifier
)

from sklearn.model_selection import (
    cross_val_score
)


def objective(
    trial
):

    n_estimators = (
        trial.suggest_int(
            "n_estimators",
            100,
            500
        )
    )

    max_depth = (
        trial.suggest_int(
            "max_depth",
            3,
            15
        )
    )

    min_samples_split = (
        trial.suggest_int(
            "min_samples_split",
            2,
            10
        )
    )


    model = RandomForestClassifier(
        n_estimators=n_estimators,
        max_depth=max_depth,
        min_samples_split=min_samples_split,
        random_state=42,
        n_jobs=-1
    )


    scores = cross_val_score(
        model,
        X_train,
        y_train,
        cv=skf,
        scoring="roc_auc",
        n_jobs=-1
    )


    return scores.mean()
```

---

# 74. Study 생성

```python
study = optuna.create_study(
    direction="maximize"
)
```

ROC-AUC처럼:

```text
높을수록 좋은 Metric
```

이면:

```text
maximize
```

이다.

---

# 75. 최소화 Metric

MAE나 RMSE처럼:

```text
낮을수록 좋은 Metric
```

이라면:

```python
study = optuna.create_study(
    direction="minimize"
)
```

를 사용할 수 있다.

---

# 76. Optimization 실행

```python
study.optimize(
    objective,
    n_trials=50
)
```

의미:

```text
50번의 Trial을 실행
```

한다.

---

# 77. Best Parameters

```python
print(
    study.best_params
)
```

---

# 78. Best Value

```python
print(
    study.best_value
)
```

---

# 79. Best Model은 자동으로 만들어질까?

`study.best_params`는 가장 좋은 Hyperparameter를 알려준다.

이 값을 이용해 최종 모델을 다시 생성할 수 있다.

```python
best_model = RandomForestClassifier(
    **study.best_params,
    random_state=42,
    n_jobs=-1
)
```

---

# 80. **의 의미

```python
**study.best_params
```

는 Dictionary를 Keyword Argument로 풀어 넣는 Python 문법이다.

예:

```python
params = {
    "max_depth": 10,
    "n_estimators": 300
}
```

이라면:

```python
RandomForestClassifier(
    **params
)
```

는 개념적으로:

```python
RandomForestClassifier(
    max_depth=10,
    n_estimators=300
)
```

와 같다.

---

# 81. 최종 학습

```python
best_model.fit(
    X_train,
    y_train
)
```

---

# 82. Test 평가

```python
test_prob = (
    best_model.predict_proba(
        X_test
    )[:, 1]
)
```

그리고:

```python
from sklearn.metrics import (
    roc_auc_score
)


test_auc = roc_auc_score(
    y_test,
    test_prob
)


print(
    "Test ROC-AUC:",
    test_auc
)
```

---

# 83. 중요한 원칙

Hyperparameter Tuning은:

```text
Train / Validation / CV
```

를 이용하여 수행한다.

최종 Test Data를 보면서:

```text
max_depth 변경

learning_rate 변경

Threshold 변경

Feature 변경
```

을 반복하면 Test Data에 맞춰지기 시작한다.

---

# 84. Test Leakage

잘못된 흐름:

```text
모델 A
→ Test = 0.82

설정 변경

모델 B
→ Test = 0.84

설정 변경

모델 C
→ Test = 0.86
```

이렇게 Test 결과를 계속 보고 모델을 선택하면 사실상 Test를 Validation처럼 사용한 것이다.

---

# 85. 올바른 흐름

```text
Train
   ↓
Cross Validation
   ↓
Hyperparameter Tuning
   ↓
Best Hyperparameter
   ↓
최종 모델
   ↓
Test
   ↓
최종 성능
```

이다.

---

# 86. Hyperparameter Tuning과 Overfitting

Hyperparameter도 너무 많이 탐색하면 Validation/CV 결과에 과하게 맞춰질 수 있다.

즉:

```text
모델 Parameter만 Overfitting되는 것이 아니라

Hyperparameter 선택 과정도
Validation에 Overfitting될 수 있다.
```

---

# 87. Nested Cross Validation

이를 더 엄격하게 평가하고 싶다면:

```text
Nested Cross Validation
```

을 사용할 수 있다.

```text
Outer CV
→ 최종 평가

Inner CV
→ Hyperparameter Tuning
```

이다.

---

# 88. Random Forest 주요 Hyperparameter

```text
n_estimators
→ Tree 개수

max_depth
→ Tree 최대 깊이

min_samples_split
→ Node를 나누기 위한 최소 Sample 수

min_samples_leaf
→ Leaf가 가져야 하는 최소 Sample 수

max_features
→ Split에서 고려할 Feature 수

class_weight
→ Class Weight
```

---

# 89. Logistic Regression 주요 Hyperparameter

대표적으로:

```text
C
penalty
solver
class_weight
```

등이 있다.

---

# 90. Logistic Regression의 C

`C`는 Regularization 강도와 반대 방향으로 생각한다.

```text
C 작음
→ Regularization 강함

C 큼
→ Regularization 약함
```

이다.

---

# 91. C 후보

예:

```python
C_values = [
    1e-4,
    1e-3,
    1e-2,
    1e-1,
    1,
    10,
    100
]
```

처럼 로그 단위로 비교하는 경우가 많다.

---

# 92. Ridge의 alpha

Ridge:

```text
alpha 작음
→ Regularization 약함

alpha 큼
→ Regularization 강함
```

이다.

Logistic Regression의 `C`와 방향이 반대라는 점을 주의한다.

---

# 93. XGBoost 주요 Hyperparameter

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

# 94. LightGBM 주요 Hyperparameter

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

# 95. CatBoost 주요 Hyperparameter

```text
iterations
learning_rate
depth
l2_leaf_reg
```

등이 있다.

---

# 96. 모든 Hyperparameter를 한꺼번에 튜닝?

처음부터 모든 값을 탐색할 필요는 없다.

예:

```text
1단계
→ learning_rate
→ depth
→ n_estimators

2단계
→ subsample
→ colsample

3단계
→ regularization
```

처럼 단계적으로 접근할 수도 있다.

---

# 97. Search Space가 중요하다

Tuning Algorithm이 좋아도 Search Space가 이상하면 좋은 결과를 얻기 어렵다.

예:

실제로 적절한 `max_depth`가 5인데:

```text
50 ~ 100
```

만 탐색하면 좋은 값을 찾을 수 없다.

---

# 98. 너무 넓은 Search Space

```text
1 ~ 100000
```

처럼 지나치게 넓게 잡으면 탐색 효율이 떨어질 수 있다.

도메인 지식과 모델 특성을 이용해 합리적인 범위를 설정한다.

---

# 99. 너무 좁은 Search Space

반대로:

```text
max_depth
→ 9, 10, 11
```

만 탐색하면 실제 좋은 영역이:

```text
3 ~ 5
```

일 경우 찾을 수 없다.

---

# 100. Tuning 결과 기록

예:

| Trial | max_depth | n_estimators | learning_rate | CV AUC |
|---:|---:|---:|---:|---:|
| 1 | 3 | 200 | 0.10 | 0.842 |
| 2 | 5 | 300 | 0.05 | 0.861 |
| 3 | 7 | 500 | 0.03 | 0.858 |

이런 식으로 기록하면 어떤 설정이 효과적이었는지 확인하기 쉽다.

---

# 101. Best Score만 보지 않는다

예:

```text
Trial A
CV Mean = 0.870
CV Std = 0.010

Trial B
CV Mean = 0.872
CV Std = 0.080
```

B가 평균은 조금 높지만 Fold마다 매우 불안정할 수 있다.

따라서:

```text
Mean
+
Std
```

를 함께 보는 것이 좋다.

---

# 102. 성능 차이가 아주 작다면?

예:

```text
Model A AUC = 0.871

Model B AUC = 0.872
```

차이가 매우 작다.

이때는 무조건 B가 실제로 더 좋은 모델이라고 단정하기보다:

```text
CV 변동성

Seed 변화

계산 비용

Inference 속도

모델 복잡도
```

도 함께 고려한다.

---

# 103. Early Stopping

Boosting 모델에서는:

```text
Early Stopping
```

도 중요한 개념이다.

모델 성능이 더 이상 개선되지 않으면 학습을 일찍 중단한다.

---

# 104. 왜 Early Stopping?

예:

```text
100 Tree → 개선

200 Tree → 개선

300 Tree → 개선 거의 없음

400 Tree → Validation 악화
```

라면 계속 학습하는 것이 도움이 되지 않을 수 있다.

---

# 105. Tuning과 Early Stopping

Boosting 모델에서는:

```text
learning_rate

tree depth

iteration 수

Early Stopping
```

을 함께 고려할 수 있다.

Library와 버전에 따라 Early Stopping 설정 방식이 다를 수 있으므로 사용하는 버전의 API를 확인한다.

---

# 106. GPU Tuning

XGBoost, LightGBM, CatBoost 등의 일부 설정에서는 GPU 학습을 지원한다.

하지만:

```text
GPU를 사용한다고
모델 자체의 성능이 자동으로 좋아지는 것은 아니다.
```

주로 학습 속도를 높이는 목적이다.

---

# 107. GPU가 항상 빠른가?

아니다.

데이터가 작거나 모델이 단순하면:

```text
CPU
```

가 충분히 빠를 수 있다.

GPU 초기화 및 데이터 이동 비용 때문에 차이가 작을 수도 있다.

---

# 108. Parallel Tuning 주의

예:

```text
GridSearchCV n_jobs=-1

그리고

RandomForest n_jobs=-1
```

처럼 내부 모델과 외부 Search가 모두 최대 병렬화를 사용하면 CPU/메모리 사용량이 매우 커질 수 있다.

환경에 따라 병렬화 수준을 조절한다.

---

# 109. Random Seed

Tuning에서도:

```python
random_state=42
```

등을 지정하면 실험 재현에 도움이 된다.

하지만 Seed 하나의 결과만 절대적인 진실이라고 생각해서는 안 된다.

---

# 110. Seed Hunting 주의

예:

```text
random_state=1
→ 0.84

random_state=2
→ 0.86

random_state=3
→ 0.90
```

를 보고 단순히 가장 높은 Seed를 선택하는 것은 모델 개선이라기보다 특정 Split에 맞춘 선택이 될 수 있다.

---

# 111. Feature Engineering과 Tuning 순서

둘 중 하나만 중요한 것이 아니다.

예:

```text
좋지 않은 Feature
+
강력한 Tuning
```

보다:

```text
좋은 Feature
+
적절한 Model
+
적당한 Tuning
```

이 더 중요할 수 있다.

---

# 112. Tuning이 마법은 아니다

성능이 낮은 이유가:

```text
잘못된 Label

Data Leakage

Feature 부족

잘못된 Split

심한 Distribution Shift

데이터 품질 문제
```

라면 Hyperparameter Tuning만으로 해결하기 어렵다.

---

# 113. Tuning 전 체크

```text
1. 데이터가 제대로 로드되었는가?

2. Target이 올바른가?

3. Leakage가 없는가?

4. Split 방법이 적절한가?

5. Metric이 문제 목적과 맞는가?

6. Baseline이 있는가?

7. 모델이 정상적으로 학습되는가?

8. Class Imbalance를 확인했는가?
```

를 먼저 확인한다.

---

# 114. Classification Tuning 전체 흐름

```text
Data
 ↓
Train / Test
 ↓
Train에서 CV
 ↓
Baseline
 ↓
Search Space
 ↓
Grid / Random / Bayesian / Optuna
 ↓
Best Hyperparameter
 ↓
Best Model
 ↓
Test
 ↓
Final Score
```

---

# 115. Regression Tuning도 동일

차이는 주로 Metric이다.

Classification:

```text
Accuracy
F1
ROC-AUC
PR-AUC
```

Regression:

```text
MAE
MSE
RMSE
R²
```

를 사용할 수 있다.

---

# 116. GridSearchCV Regression 예

```python
from sklearn.ensemble import (
    RandomForestRegressor
)

from sklearn.model_selection import (
    GridSearchCV
)


model = RandomForestRegressor(
    random_state=42
)


param_grid = {
    "n_estimators": [
        100,
        300
    ],

    "max_depth": [
        5,
        10,
        None
    ]
}


grid_search = GridSearchCV(
    estimator=model,
    param_grid=param_grid,
    cv=5,
    scoring="neg_mean_absolute_error",
    n_jobs=-1
)


grid_search.fit(
    X_train,
    y_train
)
```

---

# 117. Regression best_score_

`neg_mean_absolute_error`를 사용했다면:

```python
best_mae = (
    -grid_search.best_score_
)
```

로 실제 MAE 방향으로 바꿀 수 있다.

---

# 118. Grid / Random / Bayesian / Optuna 비교

| 방법 | 핵심 |
|---|---|
| Manual | 사람이 직접 변경 |
| GridSearch | 모든 지정 조합 탐색 |
| RandomizedSearch | 일부 조합 무작위 탐색 |
| Bayesian Optimization | 이전 결과를 이용해 다음 후보 선택 |
| Optuna | 효율적인 자동 Hyperparameter 탐색 Framework |

---

# 119. 가장 단순한 선택 기준

탐색 공간이 작다:

```text
GridSearchCV
```

탐색 공간이 크다:

```text
RandomizedSearchCV
```

학습 비용이 크고 효율적인 탐색이 필요하다:

```text
Bayesian Optimization
Optuna
```

를 고려할 수 있다.

---

# 120. 하지만 가장 중요한 것은 CV

어떤 Search Algorithm을 사용하든 후보 모델을 잘못 평가하면 의미가 없다.

따라서:

```text
좋은 Tuning
=
적절한 Search
+
적절한 Cross Validation
+
적절한 Metric
+
Leakage 방지
```

라고 생각하는 것이 좋다.

---

# 121. 흔한 실수

```text
Test Score를 보고 계속 Hyperparameter 변경

전체 데이터 Scaling 후 GridSearchCV

전체 데이터 SMOTE 후 Tuning

한 Metric만 맹신

너무 큰 Search Space

너무 작은 Search Space

Baseline 없이 바로 Tuning

Best CV Score만 보고 Test 성능 보장이라고 생각

Random Seed를 성능 향상 Hyperparameter처럼 선택
```

등을 주의한다.

---

# 122. 전체 GridSearchCV 예제

```python
from sklearn.model_selection import (
    StratifiedKFold,
    GridSearchCV
)

from sklearn.ensemble import (
    RandomForestClassifier
)


X = df.drop(
    columns=["target"]
)

y = df[
    "target"
]


skf = StratifiedKFold(
    n_splits=5,
    shuffle=True,
    random_state=42
)


model = RandomForestClassifier(
    random_state=42,
    n_jobs=-1
)


param_grid = {
    "n_estimators": [
        100,
        300,
        500
    ],

    "max_depth": [
        5,
        10,
        None
    ],

    "min_samples_split": [
        2,
        5
    ]
}


grid_search = GridSearchCV(
    estimator=model,
    param_grid=param_grid,
    cv=skf,
    scoring="roc_auc",
    n_jobs=-1
)


grid_search.fit(
    X,
    y
)


print(
    "Best Parameters:"
)

print(
    grid_search.best_params_
)


print(
    "Best CV Score:"
)

print(
    grid_search.best_score_
)
```

---

# 123. 이 코드에서 실제로 일어나는 일

```text
1. Hyperparameter 조합 생성

2. 첫 번째 조합 선택

3. 5-Fold Cross Validation

4. 평균 ROC-AUC 계산

5. 다음 조합 선택

6. 다시 5-Fold CV

7. 모든 조합 반복

8. 가장 높은 평균 ROC-AUC 조합 선택
```

이다.

---

# 124. 최종 Test가 있다면

개발 과정:

```text
Training Data
↓
GridSearchCV
↓
best_params_
```

그다음 마지막:

```text
Test Data
↓
best_estimator_ 평가
```

를 한다.

---

# 125. Competition이라면

대회에서 제공된:

```text
train.csv
```

에는 Target이 있고:

```text
test.csv
```

에는 Target이 없을 수 있다.

이 경우:

```text
train.csv 내부
→ Cross Validation
→ Hyperparameter 선택

전체 train.csv
→ 최종 모델 학습

test.csv
→ Prediction

sample_submission
→ 제출
```

흐름을 사용할 수 있다.

---

# 126. Leaderboard 주의

Submission 결과를 계속 보고:

```text
Feature 변경

Hyperparameter 변경

Ensemble Weight 변경
```

을 반복하면 Public Leaderboard에 Overfitting될 수 있다.

따라서 로컬 Cross Validation을 신뢰할 수 있게 만드는 것이 중요하다.

---

# 127. Hyperparameter Tuning 체크리스트

```text
□ Baseline을 만들었는가?

□ 올바른 Metric을 선택했는가?

□ 적절한 CV를 선택했는가?

□ Leakage를 막았는가?

□ Search Space가 합리적인가?

□ Trial 수가 너무 많거나 적지 않은가?

□ Mean CV Score를 확인했는가?

□ CV 변동성도 확인했는가?

□ Test Data를 Tuning에 사용하지 않았는가?

□ 최종 설정을 기록했는가?
```

---

# 128. 가장 중요한 구조

```text
Hyperparameter 후보
        ↓
Cross Validation
        ↓
Score 비교
        ↓
Best Hyperparameter
        ↓
Best Model
```

이다.

---

# 129. Parameter vs Hyperparameter 다시 정리

```text
Parameter
→ 모델이 데이터에서 학습

Hyperparameter
→ 학습 방법을 사람이 설정
```

예:

```text
Linear Regression Coefficient
→ Parameter

Random Forest max_depth
→ Hyperparameter
```

---

# 130. Search 방법 다시 정리

```text
Grid
→ 전부 탐색

Random
→ 무작위 일부 탐색

Bayesian
→ 이전 결과를 이용해 다음 탐색

Optuna
→ Trial 기반 자동 최적화
```

---

# 131. Cross Validation과 연결

Hyperparameter Tuning은 결국:

```text
이 Hyperparameter가 좋은가?
```

를 판단해야 한다.

그 판단을 한 번의 Validation Split보다 안정적으로 하기 위해:

```text
Cross Validation
```

을 사용한다.

따라서:

```text
Cross Validation
↓
Hyperparameter Tuning
```

은 매우 밀접하게 연결되어 있다.

---

# 132. 배운 점

Hyperparameter Tuning은:

```text
모델의 설정값을 여러 방식으로 탐색하여
더 적절한 설정을 찾는 과정
```

이다.

대표적인 방법:

```text
Manual Search

GridSearchCV

RandomizedSearchCV

Bayesian Optimization

BayesSearchCV

Optuna
```

가 있다.

하지만 가장 중요한 것은 단순히 많은 조합을 돌리는 것이 아니다.

```text
적절한 Cross Validation

적절한 Metric

Data Leakage 방지

합리적인 Search Space

최종 Test 분리
```

가 함께 필요하다.

좋은 Machine Learning 실험은:

```text
Baseline
   ↓
Cross Validation
   ↓
Model 비교
   ↓
Hyperparameter Tuning
   ↓
최종 모델 선택
   ↓
Test 평가
```

순서로 진행한다.

그리고:

```text
Tuning은 모델의 가능성을 끌어내는 과정이지
잘못된 데이터나 잘못된 검증 방법을
고쳐주는 마법이 아니다.
```