# Regression 회귀

## 1. Regression이란?

Regression은 연속적인 숫자 값을 예측하는 Machine Learning 문제이다.

한국어로:

```text
Regression
→ 회귀
```

라고 한다.

예:

```text
집 가격 예측

매출 예측

체중 예측

혈압 예측

입원 기간 예측

온도 예측
```

---

# 2. Classification과 Regression 차이

가장 먼저 이 차이를 이해해야 한다.

```text
Classification
→ 종류를 예측

Regression
→ 숫자 값을 예측
```

예:

```text
환자가 질병인가?
→ Classification

환자의 혈압은 얼마인가?
→ Regression
```

---

# 3. 출력값 비교

Classification:

```text
0
1
```

또는:

```text
Normal
Pneumonia
```

Regression:

```text
123.5

78.2

25000000

15.7
```

처럼 연속적인 숫자를 예측한다.

---

# 4. 기본 구조

```text
Feature
   ↓
Regression Model
   ↓
Numeric Prediction
```

예:

```text
면적
방 개수
지역
건축 연도
   ↓
Regression Model
   ↓
집 가격
```

---

# 5. Feature와 Target

예:

| age | BMI | exercise | blood_pressure |
|---:|---:|---:|---:|
| 25 | 21.5 | 3 | 115 |
| 45 | 26.2 | 1 | 132 |
| 60 | 30.1 | 0 | 148 |
| 35 | 23.7 | 4 | 120 |

여기서:

```text
age
BMI
exercise
```

는 Feature이다.

```text
blood_pressure
```

는 Target이다.

---

# 6. X와 y

```python
X = df[
    [
        "age",
        "BMI",
        "exercise"
    ]
]

y = df[
    "blood_pressure"
]
```

즉:

```text
X
→ 입력 정보

y
→ 예측해야 하는 숫자
```

이다.

---

# 7. Regression Target

회귀의 Target은 일반적으로 숫자형 연속값이다.

예:

```text
120.5

131.2

98.7

145.3
```

이다.

---

# 8. Classification과 다시 비교

Classification:

```python
y = df[
    "disease"
]
```

결과:

```text
0
1
0
1
```

Regression:

```python
y = df[
    "blood_pressure"
]
```

결과:

```text
120
135
128
151
```

이다.

---

# 9. 대표적인 Regression 모델

대표적으로:

```text
Linear Regression

Ridge Regression

Lasso Regression

Decision Tree Regressor

Random Forest Regressor

Gradient Boosting Regressor

XGBoost Regressor

LightGBM Regressor

CatBoost Regressor
```

등이 있다.

---

# 10. Classifier와 Regressor

Scikit-learn 모델 이름을 보면 문제 종류를 파악할 수 있다.

예:

```text
RandomForestClassifier
→ 분류

RandomForestRegressor
→ 회귀
```

또:

```text
XGBClassifier
→ 분류

XGBRegressor
→ 회귀
```

처럼 구분된다.

---

# 11. Linear Regression

가장 기본적인 회귀 모델 중 하나이다.

한국어:

```text
선형 회귀
```

이다.

---

# 12. 아주 간단한 예

공부 시간과 시험 점수가 있다고 하자.

```text
공부시간    점수

1시간       50
2시간       60
3시간       70
4시간       80
5시간       90
```

대략:

```text
공부 시간이 증가하면
점수도 증가한다.
```

라는 관계가 있다.

---

# 13. 선형 관계

이를 직선으로 표현하면:

```text
점수
 ^
 |                 *
 |             *
 |         *
 |     *
 | *
 +----------------------> 공부시간
```

Linear Regression은 이런 관계를 직선 형태로 모델링하는 기본적인 방법이다.

---

# 14. Linear Regression 수식

단순한 형태:

```text
y = wx + b
```

이다.

여기서:

```text
x
→ Feature

y
→ Prediction

w
→ Weight / Coefficient

b
→ Intercept
```

이다.

---

# 15. Feature가 여러 개라면?

예:

```text
집 가격
```

을 예측한다고 하자.

Feature:

```text
면적
방 개수
건축 연도
```

개념적으로:

```text
가격 =
w1 × 면적
+
w2 × 방 개수
+
w3 × 건축 연도
+
b
```

처럼 표현할 수 있다.

---

# 16. LinearRegression

Scikit-learn:

```python
from sklearn.linear_model import (
    LinearRegression
)
```

모델 생성:

```python
model = LinearRegression()
```

---

# 17. 학습

```python
model.fit(
    X_train,
    y_train
)
```

모델은 Train Data를 이용하여:

```text
Coefficient

Intercept
```

를 학습한다.

---

# 18. 예측

```python
pred = model.predict(
    X_test
)
```

결과:

```text
121.4
137.8
145.2
119.7
```

처럼 숫자가 나온다.

---

# 19. Classification과 predict()

Classification:

```python
pred = classifier.predict(
    X_test
)
```

결과:

```text
0
1
1
0
```

Regression:

```python
pred = regressor.predict(
    X_test
)
```

결과:

```text
120.7
138.2
151.9
```

이다.

---

# 20. 회귀에는 일반적으로 predict_proba가 없다

Classification에서는:

```python
predict_proba()
```

를 통해 Class 확률을 확인할 수 있는 모델들이 있다.

Regression에서는 보통:

```python
predict()
```

를 통해 숫자 예측값을 얻는다.

---

# 21. Coefficient 확인

Linear Regression:

```python
print(
    model.coef_
)
```

를 이용하여 학습된 Coefficient를 확인할 수 있다.

---

# 22. Intercept 확인

```python
print(
    model.intercept_
)
```

로 절편을 확인할 수 있다.

---

# 23. Coefficient 해석

예를 들어:

```text
age coefficient = 2.5
```

라고 하자.

다른 Feature들이 같다는 선형 모델의 가정 아래 age가 1 증가할 때 예측값이 약 2.5 증가하는 방향의 관계를 학습했다고 해석할 수 있다.

하지만:

```text
Coefficient가 양수
→ age가 결과의 원인
```

이라고 단정할 수는 없다.

---

# 24. Coefficient 크기 비교 주의

Feature:

```text
age
income
BMI
```

가 서로 단위와 범위가 크게 다르면 Coefficient의 절댓값만 보고 중요도를 단순 비교하기 어렵다.

Scaling 여부도 영향을 줄 수 있다.

---

# 25. Residual

실제값과 예측값의 차이를:

```text
Residual
```

이라고 한다.

한국어:

```text
잔차
```

이다.

개념:

```text
Residual
=
Actual - Prediction
```

---

# 26. 예

실제값:

```text
150
```

예측값:

```text
140
```

이라면:

```text
Residual
=
150 - 140
=
10
```

이다.

---

# 27. 왜 Residual이 중요한가?

회귀 모델은 예측값과 실제값의 차이를 줄이는 방향으로 학습한다.

그리고 잔차를 분석하면:

```text
특정 구간에서 계속 틀리는가?

큰 값에서 오차가 커지는가?

특정 패턴이 남아 있는가?
```

등을 확인할 수 있다.

---

# 28. Regression 평가

분류에서는:

```text
Accuracy
Precision
Recall
F1
ROC-AUC
```

등을 사용했다.

회귀에서는 다른 Metric을 사용한다.

대표적으로:

```text
MAE

MSE

RMSE

R²
```

가 있다.

---

# 29. MAE

```text
Mean Absolute Error
```

한국어:

```text
평균 절대 오차
```

이다.

---

# 30. MAE 개념

실제값과 예측값의 차이에 절댓값을 적용하고 평균을 계산한다.

```text
MAE
=
|실제 - 예측|의 평균
```

---

# 31. MAE 예

실제값:

```text
100
200
300
```

예측값:

```text
110
180
330
```

오차:

```text
-10
20
-30
```

절댓값:

```text
10
20
30
```

평균:

```text
MAE = 20
```

이다.

---

# 32. MAE 코드

```python
from sklearn.metrics import (
    mean_absolute_error
)

mae = mean_absolute_error(
    y_test,
    pred
)

print(
    "MAE:",
    mae
)
```

---

# 33. MAE 해석

예:

```text
MAE = 5.2
```

라면 모델 예측이 실제값과 평균적으로 약 5.2 단위 정도 차이가 난다고 이해할 수 있다.

단위는 Target과 같다.

---

# 34. MSE

```text
Mean Squared Error
```

한국어:

```text
평균 제곱 오차
```

이다.

---

# 35. MSE 개념

오차를 제곱한 후 평균을 계산한다.

```text
MSE
=
(실제 - 예측)²의 평균
```

---

# 36. 왜 제곱하는가?

큰 오차가 발생하면 제곱 때문에 더 큰 영향을 준다.

예:

```text
오차 2
→ 4

오차 10
→ 100
```

이다.

따라서 큰 오류에 더 민감하다.

---

# 37. MSE 코드

```python
from sklearn.metrics import (
    mean_squared_error
)

mse = mean_squared_error(
    y_test,
    pred
)

print(
    "MSE:",
    mse
)
```

---

# 38. MSE 단위

Target이:

```text
kg
```

이라면 MSE는:

```text
kg²
```

처럼 원래 Target의 단위가 제곱된 형태가 된다.

그래서 직관적으로 해석하기 어려울 수 있다.

---

# 39. RMSE

```text
Root Mean Squared Error
```

이다.

MSE에 제곱근을 적용한다.

```text
RMSE
=
√MSE
```

---

# 40. RMSE 장점

RMSE는 다시 Target과 같은 단위로 돌아온다.

예:

```text
Target = kg

RMSE = kg
```

이므로 MSE보다 해석하기 쉬운 경우가 많다.

---

# 41. RMSE 코드

```python
import numpy as np

rmse = np.sqrt(
    mean_squared_error(
        y_test,
        pred
    )
)

print(
    "RMSE:",
    rmse
)
```

Scikit-learn 버전에 따라 RMSE를 직접 계산하는 별도의 함수도 사용할 수 있다.

---

# 42. MAE와 RMSE 차이

MAE:

```text
모든 절대 오차를 비교적 직접적으로 반영
```

RMSE:

```text
큰 오차에 더 큰 패널티
```

를 준다.

예:

```text
모델이 가끔 매우 크게 틀리는 것이
중요한 문제인가?
```

를 생각할 수 있다.

---

# 43. MAE와 RMSE는 낮을수록 좋다

일반적으로:

```text
MAE ↓

MSE ↓

RMSE ↓
```

일수록 실제값과 예측값의 차이가 작다는 의미이다.

---

# 44. R²

```text
R-squared
```

또는:

```text
Coefficient of Determination
```

이라고 한다.

한국어:

```text
결정계수
```

이다.

---

# 45. R² 개념

모델이 Target의 변동을 얼마나 설명하는지를 나타내는 대표적인 회귀 평가 지표이다.

흔히:

```text
1에 가까울수록 좋음
```

이라고 설명한다.

하지만 해석에 주의해야 한다.

---

# 46. R² 코드

```python
from sklearn.metrics import (
    r2_score
)

r2 = r2_score(
    y_test,
    pred
)

print(
    "R2:",
    r2
)
```

---

# 47. R² = 1

```text
R² = 1
```

이면 해당 평가 데이터에서 예측이 실제값과 완전히 일치한다.

---

# 48. R² = 0

```text
R² = 0
```

이면 해당 평가 기준에서 단순히 Target 평균을 예측하는 기준 모델과 비슷한 수준이라고 볼 수 있다.

---

# 49. R²가 음수가 될 수도 있다

중요하다.

```text
R² < 0
```

도 가능하다.

예:

```text
R² = -0.5
```

이는 해당 평가 데이터에서 평균값을 예측하는 단순 기준보다도 좋지 않은 성능일 수 있다는 뜻이다.

---

# 50. R²는 정확도 퍼센트가 아니다

예:

```text
R² = 0.8
```

이라고 해서:

```text
정확도 80%
```

라고 표현하면 안 된다.

Classification Accuracy와 다른 Metric이다.

---

# 51. 평가 Metric 비교

| Metric | 의미 | 방향 |
|---|---|---|
| MAE | 평균 절대 오차 | 낮을수록 좋음 |
| MSE | 평균 제곱 오차 | 낮을수록 좋음 |
| RMSE | MSE의 제곱근 | 낮을수록 좋음 |
| R² | 설명력 관련 지표 | 일반적으로 높을수록 좋음 |

---

# 52. 어떤 Metric을 사용할까?

문제 목적에 따라 결정한다.

예:

```text
평균적인 오차 크기가 중요
→ MAE

큰 오차를 더 강하게 벌점
→ RMSE / MSE

설명력 관점
→ R²
```

여러 Metric을 함께 보는 것도 좋다.

---

# 53. Baseline이 필요하다

회귀에서도 복잡한 모델을 바로 사용하는 것보다 간단한 기준과 비교하는 것이 중요하다.

가장 단순한 방법 중 하나:

```text
항상 Train Target 평균을 예측
```

하는 것이다.

---

# 54. DummyRegressor

Scikit-learn:

```python
from sklearn.dummy import (
    DummyRegressor
)

dummy = DummyRegressor(
    strategy="mean"
)

dummy.fit(
    X_train,
    y_train
)

dummy_pred = dummy.predict(
    X_test
)
```

---

# 55. 왜 Dummy Model을 사용할까?

예를 들어:

```text
복잡한 Model RMSE = 20

Dummy Model RMSE = 19
```

라면 복잡한 모델이 단순 평균 예측보다도 좋지 않을 수 있다.

즉 모델 성능을 판단할 기준점이 생긴다.

---

# 56. Train / Test Split

회귀도 Classification과 마찬가지로 데이터를 나눈다.

```python
from sklearn.model_selection import (
    train_test_split
)

X_train, X_test, y_train, y_test = (
    train_test_split(
        X,
        y,
        test_size=0.2,
        random_state=42
    )
)
```

---

# 57. 왜 stratify=y가 없는가?

Classification에서는:

```python
stratify=y
```

를 자주 사용했다.

Class 비율을 유지하기 위해서였다.

회귀 Target은 연속값이므로 일반적인 `train_test_split()`에서 그대로 `stratify=y`를 적용하는 방식은 보통 적절하지 않다.

---

# 58. 회귀에서도 분포 확인은 중요

예:

```text
Train에는 낮은 값만 있음

Test에는 매우 높은 값이 많음
```

이면 평가가 어려워질 수 있다.

따라서:

```python
print(
    y_train.describe()
)

print(
    y_test.describe()
)
```

등으로 Target 분포를 확인한다.

---

# 59. Histogram

```python
import matplotlib.pyplot as plt

plt.hist(
    y_train,
    bins=30
)

plt.xlabel(
    "Target"
)

plt.ylabel(
    "Count"
)

plt.show()
```

Target 분포를 시각적으로 확인할 수 있다.

---

# 60. Scaling

Linear Regression 계열 모델에서는 Feature Scaling을 고려할 수 있다.

특히:

```text
Ridge

Lasso
```

처럼 Regularization을 사용하는 모델에서는 Feature Scale을 맞추는 것이 중요하다.

---

# 61. StandardScaler

```python
from sklearn.preprocessing import (
    StandardScaler
)

scaler = StandardScaler()

X_train_scaled = scaler.fit_transform(
    X_train
)

X_test_scaled = scaler.transform(
    X_test
)
```

다시 한 번:

```text
Train
→ fit_transform

Test
→ transform
```

이다.

---

# 62. Pipeline

더 안전하게:

```python
from sklearn.pipeline import (
    Pipeline
)

model = Pipeline(
    steps=[
        (
            "scaler",
            StandardScaler()
        ),
        (
            "regressor",
            LinearRegression()
        )
    ]
)
```

---

# 63. Pipeline 학습

```python
model.fit(
    X_train,
    y_train
)
```

---

# 64. Pipeline 예측

```python
pred = model.predict(
    X_test
)
```

Pipeline이 Scaling과 Prediction을 순서대로 처리한다.

---

# 65. Linear Regression의 문제

Feature가 많아지거나 서로 강하게 관련되어 있으면 Coefficient가 불안정해질 수 있다.

또한 모델이 Train Data에 지나치게 맞는 문제도 생각해야 한다.

이를 제어하기 위해:

```text
Regularization
```

을 사용할 수 있다.

---

# 66. Regularization

한국어로:

```text
규제
```

또는:

```text
정규화
```

라고 번역되기도 한다.

단, 여기서 Regularization은 데이터 Scaling의 Normalization과 다른 개념이다.

목적은:

```text
Coefficient가 지나치게 커지는 것을 제한
```

하여 모델 복잡도를 제어하는 것이다.

---

# 67. Ridge Regression

Ridge는 Linear Regression에:

```text
L2 Regularization
```

을 추가한 모델이다.

---

# 68. Ridge Import

```python
from sklearn.linear_model import (
    Ridge
)
```

---

# 69. Ridge 사용

```python
model = Ridge(
    alpha=1.0
)

model.fit(
    X_train_scaled,
    y_train
)

pred = model.predict(
    X_test_scaled
)
```

---

# 70. alpha

```python
alpha=1.0
```

은 Regularization 강도를 조절하는 Hyperparameter이다.

개념적으로:

```text
alpha 작음
→ 규제 약함

alpha 큼
→ 규제 강함
```

이다.

---

# 71. alpha가 너무 작으면?

Linear Regression과 비슷해질 수 있다.

모델이 복잡한 패턴에 과하게 맞을 가능성을 충분히 줄이지 못할 수도 있다.

---

# 72. alpha가 너무 크면?

Coefficient를 지나치게 제한하여:

```text
Underfitting
```

이 발생할 수 있다.

따라서 적절한 alpha를 Validation 또는 Cross Validation으로 선택한다.

---

# 73. Lasso Regression

Lasso는:

```text
L1 Regularization
```

을 사용하는 선형 회귀 모델이다.

---

# 74. Lasso Import

```python
from sklearn.linear_model import (
    Lasso
)
```

---

# 75. Lasso 사용

```python
model = Lasso(
    alpha=0.1
)

model.fit(
    X_train_scaled,
    y_train
)

pred = model.predict(
    X_test_scaled
)
```

---

# 76. Ridge와 Lasso 차이

큰 개념으로:

```text
Ridge
→ L2
→ Coefficient를 작게 만드는 방향

Lasso
→ L1
→ 일부 Coefficient가 정확히 0이 될 수 있음
```

이다.

---

# 77. Lasso와 Feature Selection

Lasso는 일부 Coefficient를 0으로 만들 수 있기 때문에 Feature Selection 효과를 가질 수 있다.

하지만:

```text
Lasso가 0으로 만들었다
→ 그 Feature는 현실에서 아무 의미가 없다
```

라고 단정해서는 안 된다.

모델 관점의 결과이다.

---

# 78. Elastic Net

Ridge와 Lasso의 성질을 함께 사용하는:

```text
Elastic Net
```

도 있다.

즉:

```text
L1 + L2
```

Regularization을 함께 고려한다.

---

# 79. Tree Regression

회귀에서도 Decision Tree를 사용할 수 있다.

```python
from sklearn.tree import (
    DecisionTreeRegressor
)

model = DecisionTreeRegressor(
    random_state=42
)
```

---

# 80. Random Forest Regression

```python
from sklearn.ensemble import (
    RandomForestRegressor
)

model = RandomForestRegressor(
    n_estimators=200,
    random_state=42
)
```

---

# 81. Tree Regressor는 무엇을 예측?

Classifier처럼 Class를 예측하는 것이 아니라:

```text
연속적인 숫자
```

를 예측한다.

예:

```text
132.7
145.2
98.5
```

이다.

---

# 82. Tree 계열과 Scaling

Random Forest 같은 Tree 기반 회귀 모델은 일반적으로 Feature Scaling에 상대적으로 덜 민감하다.

따라서:

```text
Linear / Ridge / Lasso
→ Scaling 중요하게 고려

Random Forest
→ Scaling 필요성이 상대적으로 낮음
```

으로 생각할 수 있다.

---

# 83. RandomForestRegressor 예

```python
model = RandomForestRegressor(
    n_estimators=200,
    max_depth=10,
    random_state=42,
    n_jobs=-1
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

# 84. Feature Importance

Random Forest에서는:

```python
model.feature_importances_
```

를 확인할 수 있다.

---

# 85. Feature Importance DataFrame

```python
importance = pd.DataFrame(
    {
        "feature": X_train.columns,
        "importance": model.feature_importances_
    }
)

importance = importance.sort_values(
    "importance",
    ascending=False
)

print(
    importance
)
```

---

# 86. XGBoost Regression

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

# 87. LightGBM Regression

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

# 88. CatBoost Regression

```python
from catboost import (
    CatBoostRegressor
)

model = CatBoostRegressor(
    iterations=500,
    learning_rate=0.05,
    random_seed=42,
    verbose=0
)
```

---

# 89. 실제값과 예측값 비교

```python
result = pd.DataFrame(
    {
        "actual": y_test,
        "prediction": pred
    }
)

print(
    result.head()
)
```

---

# 90. Error 추가

```python
result[
    "error"
] = (
    result["actual"]
    -
    result["prediction"]
)
```

---

# 91. Absolute Error

```python
result[
    "absolute_error"
] = (
    result["error"]
    .abs()
)
```

---

# 92. 가장 크게 틀린 데이터

```python
result = result.sort_values(
    "absolute_error",
    ascending=False
)

print(
    result.head(10)
)
```

이렇게 하면 모델이 가장 크게 틀린 Sample을 찾을 수 있다.

---

# 93. Failure Case 분석

회귀에서도 단순 Metric만 보는 것이 아니라:

```text
어떤 Sample에서 크게 틀리는가?

높은 Target에서 많이 틀리는가?

낮은 Target에서 많이 틀리는가?

특정 그룹에서 오차가 큰가?

Outlier 때문인가?
```

등을 분석한다.

---

# 94. Actual vs Prediction 시각화

```python
import matplotlib.pyplot as plt

plt.scatter(
    y_test,
    pred
)

plt.xlabel(
    "Actual"
)

plt.ylabel(
    "Prediction"
)

plt.show()
```

---

# 95. 좋은 예측이라면

실제값과 예측값이 비슷할수록 점들이:

```text
y = x
```

선 근처에 모이게 된다.

기준선을 추가하면:

```python
min_value = min(
    y_test.min(),
    pred.min()
)

max_value = max(
    y_test.max(),
    pred.max()
)

plt.scatter(
    y_test,
    pred
)

plt.plot(
    [min_value, max_value],
    [min_value, max_value]
)

plt.xlabel(
    "Actual"
)

plt.ylabel(
    "Prediction"
)

plt.show()
```

처럼 볼 수 있다.

---

# 96. Residual Plot

```python
residuals = (
    y_test
    -
    pred
)

plt.scatter(
    pred,
    residuals
)

plt.axhline(
    0
)

plt.xlabel(
    "Prediction"
)

plt.ylabel(
    "Residual"
)

plt.show()
```

---

# 97. Residual Plot을 왜 보는가?

잔차가 특정한 패턴을 보인다면 모델이 데이터 구조를 충분히 설명하지 못하고 있을 가능성이 있다.

예:

```text
예측값이 커질수록 오차도 커짐

곡선 패턴

특정 구간에 오차 집중
```

등이다.

---

# 98. Outlier와 Regression

회귀에서는 극단적인 Target 값이나 Feature 값이 모델과 평가 Metric에 큰 영향을 줄 수 있다.

특히:

```text
MSE

RMSE
```

는 큰 오차를 제곱하기 때문에 Outlier의 영향을 크게 받을 수 있다.

---

# 99. Outlier를 무조건 삭제하면 안 된다

극단적인 값이:

```text
입력 오류
```

인지:

```text
실제로 중요한 희귀 사례
```

인지 먼저 확인해야 한다.

특히 의료/금융 데이터에서는 극단값 자체가 중요한 정보일 수 있다.

---

# 100. Target Transformation

Target 분포가 매우 치우쳐 있다면 상황에 따라:

```text
log transformation
```

등을 검토할 수 있다.

예:

```python
y_log = np.log1p(
    y
)
```

---

# 101. 원래 단위로 복원

Log Target으로 모델을 학습했다면 예측 후:

```python
pred_original = np.expm1(
    pred_log
)
```

처럼 원래 Scale로 되돌릴 수 있다.

단, 변환된 공간의 오차와 원래 공간의 오차는 해석이 다를 수 있으므로 평가 방식을 명확히 해야 한다.

---

# 102. Regression Overfitting

예:

```text
Train RMSE = 2

Validation RMSE = 20
```

이라면 Train에서는 매우 잘 맞지만 새로운 데이터에서는 성능이 크게 떨어진다.

```text
Overfitting
```

가능성을 의심한다.

---

# 103. Underfitting

예:

```text
Train RMSE = 25

Validation RMSE = 27
```

이고 둘 다 목표 수준보다 매우 나쁘다면 모델이 데이터 패턴을 충분히 학습하지 못했을 수 있다.

이를:

```text
Underfitting
```

관점에서 볼 수 있다.

---

# 104. Overfitting 해결 방향

예:

```text
모델 복잡도 낮추기

Regularization

Feature 정리

Cross Validation

더 많은 데이터

Tree Depth 제한

Hyperparameter 조정
```

등을 고려할 수 있다.

---

# 105. Linear Regression vs Ridge

Linear Regression:

```text
기본 선형 회귀
```

Ridge:

```text
Linear Regression
+
L2 Regularization
```

이다.

---

# 106. Linear Regression vs Lasso

Linear Regression:

```text
Coefficient 크기에 별도의 L1 규제 없음
```

Lasso:

```text
L1 Regularization
+
일부 Coefficient를 0으로 만들 수 있음
```

이다.

---

# 107. Ridge는 왜 Scaling이 중요한가?

Ridge는 Coefficient 크기에 패널티를 준다.

Feature들의 Scale이 크게 다르면 Coefficient 크기의 의미도 달라질 수 있다.

그래서 일반적으로:

```text
Scaling
↓
Ridge
```

구조를 사용한다.

---

# 108. Ridge Pipeline

```python
from sklearn.pipeline import (
    Pipeline
)

from sklearn.preprocessing import (
    StandardScaler
)

from sklearn.linear_model import (
    Ridge
)


ridge_model = Pipeline(
    steps=[
        (
            "scaler",
            StandardScaler()
        ),
        (
            "ridge",
            Ridge(
                alpha=1.0
            )
        )
    ]
)
```

---

# 109. 학습

```python
ridge_model.fit(
    X_train,
    y_train
)
```

---

# 110. 예측

```python
pred = ridge_model.predict(
    X_test
)
```

---

# 111. 평가

```python
mae = mean_absolute_error(
    y_test,
    pred
)

mse = mean_squared_error(
    y_test,
    pred
)

rmse = np.sqrt(
    mse
)

r2 = r2_score(
    y_test,
    pred
)

print(
    "MAE:",
    mae
)

print(
    "MSE:",
    mse
)

print(
    "RMSE:",
    rmse
)

print(
    "R2:",
    r2
)
```

---

# 112. 전체 기본 코드

```python
import numpy as np

from sklearn.model_selection import (
    train_test_split
)

from sklearn.pipeline import (
    Pipeline
)

from sklearn.preprocessing import (
    StandardScaler
)

from sklearn.linear_model import (
    Ridge
)

from sklearn.metrics import (
    mean_absolute_error,
    mean_squared_error,
    r2_score
)


X = df.drop(
    columns=["target"]
)

y = df[
    "target"
]


X_train, X_test, y_train, y_test = (
    train_test_split(
        X,
        y,
        test_size=0.2,
        random_state=42
    )
)


model = Pipeline(
    steps=[
        (
            "scaler",
            StandardScaler()
        ),
        (
            "regressor",
            Ridge(
                alpha=1.0
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


mae = mean_absolute_error(
    y_test,
    pred
)

mse = mean_squared_error(
    y_test,
    pred
)

rmse = np.sqrt(
    mse
)

r2 = r2_score(
    y_test,
    pred
)


print(
    "MAE:",
    mae
)

print(
    "MSE:",
    mse
)

print(
    "RMSE:",
    rmse
)

print(
    "R2:",
    r2
)
```

---

# 113. 코드 전체 흐름

```text
DataFrame
   ↓
X / y
   ↓
Train / Test Split
   ↓
StandardScaler
   ↓
Ridge Regression
   ↓
fit()
   ↓
predict()
   ↓
MAE
MSE
RMSE
R²
```

이다.

---

# 114. 여러 모델 비교

예:

```python
from sklearn.linear_model import (
    LinearRegression,
    Ridge,
    Lasso
)

from sklearn.ensemble import (
    RandomForestRegressor
)


models = {
    "Linear": LinearRegression(),

    "Ridge": Ridge(
        alpha=1.0
    ),

    "Lasso": Lasso(
        alpha=0.1
    ),

    "RandomForest": RandomForestRegressor(
        n_estimators=200,
        random_state=42
    )
}
```

---

# 115. 주의할 점

위 모델들은 전처리 요구가 서로 다를 수 있다.

특히:

```text
Linear / Ridge / Lasso
→ Scaling 고려

RandomForest
→ Scaling 필요성이 상대적으로 낮음
```

이다.

따라서 실제 공정한 비교에서는 모델별 적절한 Pipeline을 구성하는 것이 좋다.

---

# 116. Regression과 Cross Validation

한 번의 Train/Test Split 결과만으로 모델을 판단하면 특정 Split에 따라 결과가 달라질 수 있다.

따라서:

```text
Cross Validation
```

을 사용할 수 있다.

예:

```text
Fold 1
Fold 2
Fold 3
Fold 4
Fold 5
```

에서 반복 평가한다.

---

# 117. Cross Validation Metric

Scikit-learn에서 회귀 Cross Validation을 할 때:

```text
neg_mean_absolute_error

neg_mean_squared_error

neg_root_mean_squared_error

r2
```

등의 Scoring을 볼 수 있다.

`neg_`가 붙는 이유는 Scikit-learn의 scoring 체계에서 높은 점수를 더 좋은 값으로 다루기 위한 것이다.

---

# 118. Negative MAE 주의

예:

```text
Cross Validation Score
= -5.2
```

라고 해서 실제 MAE가 음수라는 뜻은 아니다.

원래 MAE는:

```text
5.2
```

인데 Scikit-learn scoring 규칙 때문에 음수 형태로 반환될 수 있다.

---

# 119. Regression에서 Data Leakage

회귀에서도 똑같이 중요하다.

잘못된 예:

```text
전체 데이터에서 Scaling
↓
Train/Test Split
```

또는:

```text
전체 Target 정보를 이용해 Feature 생성
↓
Split
```

등은 Leakage를 만들 수 있다.

---

# 120. Target Leakage

예를 들어 입원 기간을 예측한다고 하자.

Target:

```text
입원기간
```

인데 Feature에:

```text
퇴원일
```

을 넣는다면 예측 시점에 알 수 없는 미래 정보일 수 있다.

이런 경우 매우 높은 성능이 나와도 실제 서비스에서는 사용할 수 없다.

---

# 121. 예측 시점을 생각한다

항상 질문한다.

```text
이 모델이 실제 예측을 하는 순간
이 Feature를 알 수 있는가?
```

이 질문은 Classification과 Regression 모두 중요하다.

---

# 122. Time Series Regression

시간에 따라 변하는 값을 예측한다면 일반적인 Random Split이 적절하지 않을 수 있다.

예:

```text
과거 매출
↓
미래 매출 예측
```

이라면:

```text
과거
→ Train

미래
→ Validation / Test
```

처럼 시간 순서를 유지해야 한다.

---

# 123. Group 데이터

한 환자에게 여러 측정값이 있는 경우:

```text
Patient A
→ Train

Patient A
→ Test
```

가 되면 Leakage가 생길 수 있다.

따라서 Group 단위 Split을 고려한다.

---

# 124. 회귀 성능이 너무 좋다면?

예:

```text
R² = 0.9999
```

라고 나왔다고 무조건 좋은 모델이라고 판단하지 않는다.

확인:

```text
Data Leakage?

Target Leakage?

중복 데이터?

Train/Test 중복?

시간 정보 누수?

같은 환자 중복?

Target에서 직접 만들어진 Feature?
```

등을 점검한다.

---

# 125. 회귀 성능이 낮다면?

모델만 바꾸기 전에:

```text
Target 자체가 예측 가능한가?

Feature가 충분한가?

결측치 처리가 적절한가?

Outlier가 심한가?

Target 분포가 심하게 치우쳤는가?

Feature Engineering이 필요한가?

Split 방식이 적절한가?
```

를 확인한다.

---

# 126. Classification과 Regression 비교표

| 구분 | Classification | Regression |
|---|---|---|
| 목적 | Class 예측 | 숫자 예측 |
| 예시 | 정상/질병 | 혈압 |
| 출력 | 0, 1 | 132.5 |
| 대표 기본 모델 | LogisticRegression | LinearRegression |
| Tree | DecisionTreeClassifier | DecisionTreeRegressor |
| Random Forest | RandomForestClassifier | RandomForestRegressor |
| XGBoost | XGBClassifier | XGBRegressor |
| 대표 Metric | Accuracy, F1, AUC | MAE, RMSE, R² |
| Threshold | 중요할 수 있음 | 일반적으로 사용하지 않음 |

---

# 127. Logistic Regression과 Linear Regression

이름 때문에 혼동하기 쉽다.

```text
Linear Regression
→ Regression

Logistic Regression
→ Classification
```

이다.

이 부분을 확실히 기억한다.

---

# 128. 가장 쉬운 구분 방법

Target을 본다.

Target:

```text
0 / 1
```

이고 두 Class를 의미한다면:

```text
Classification
```

Target:

```text
125.7
138.2
99.4
```

같은 연속값이라면:

```text
Regression
```

이다.

---

# 129. 하지만 숫자라고 무조건 Regression은 아니다

예:

```text
0 = 고양이
1 = 강아지
2 = 새
```

Target이 숫자로 되어 있지만 이것은:

```text
Multi-class Classification
```

이다.

숫자의 모양이 아니라:

```text
Target이 무엇을 의미하는가?
```

를 봐야 한다.

---

# 130. 반대로 나이도 상황에 따라 다르다

예:

```text
정확한 나이 예측
→ Regression
```

하지만:

```text
청년 / 중년 / 노년
→ Classification
```

이 될 수 있다.

문제 정의에 따라 달라진다.

---

# 131. 회귀 프로젝트 기본 흐름

```text
문제 정의
   ↓
Target 확인
   ↓
EDA
   ↓
Feature / Target 분리
   ↓
Train / Validation / Test
   ↓
Preprocessing
   ↓
Baseline
   ↓
Regression Model
   ↓
Prediction
   ↓
MAE / RMSE / R²
   ↓
Residual 분석
   ↓
Failure Case 분석
   ↓
Model 개선
```

---

# 132. 핵심 용어 정리

```text
Regression
→ 연속적인 숫자를 예측하는 문제

Linear Regression
→ 선형 관계를 학습하는 기본 회귀 모델

Coefficient
→ Feature에 학습된 계수

Intercept
→ 절편

Residual
→ 실제값과 예측값의 차이

MAE
→ 평균 절대 오차

MSE
→ 평균 제곱 오차

RMSE
→ MSE의 제곱근

R²
→ 결정계수

Regularization
→ 모델 복잡도를 제어하는 방법

Ridge
→ L2 Regularization

Lasso
→ L1 Regularization

Overfitting
→ Train에 지나치게 맞은 상태

Underfitting
→ 데이터 패턴을 충분히 학습하지 못한 상태
```

---

# 133. 가장 중요한 구조

```text
Feature
   ↓
Regression Model
   ↓
Numeric Prediction
   ↓
Actual과 비교
   ↓
Error
   ↓
MAE / MSE / RMSE / R²
```

---

# 134. Classification과 함께 기억하기

```text
Classification
→ 어떤 것인가?

Regression
→ 얼마인가?
```

예:

```text
폐렴인가?
→ Classification

혈압이 얼마인가?
→ Regression
```

---

# 135. 배운 점

Regression은:

```text
연속적인 숫자 값을 예측하는
Machine Learning 문제
```

이다.

가장 기본적인 모델:

```text
Linear Regression
```

이 있고 Regularization을 추가한:

```text
Ridge
Lasso
```

등이 있다.

Tree 계열에서는:

```text
DecisionTreeRegressor
RandomForestRegressor
XGBRegressor
LGBMRegressor
CatBoostRegressor
```

등을 사용할 수 있다.

회귀 모델의 예측:

```python
pred = model.predict(
    X_test
)
```

은 Class가 아니라 숫자이다.

성능은 대표적으로:

```text
MAE
MSE
RMSE
R²
```

등으로 평가한다.

그리고 단순히 Metric만 보는 것이 아니라:

```text
Residual

Actual vs Prediction

Outlier

Train / Validation 성능 차이

Data Leakage

Target Leakage
```

도 함께 확인해야 한다.

가장 중요한 것은:

```text
Classification
→ 종류를 예측

Regression
→ 값을 예측
```

이라는 차이를 이해하는 것이다.