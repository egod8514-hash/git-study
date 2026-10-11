# Feature Importance & SHAP 모델 해석

## 1. 모델 성능만 보면 충분할까?

Machine Learning 모델을 학습하면 보통 먼저 성능을 확인한다.

예:

```text
Accuracy = 0.91
F1 = 0.84
ROC-AUC = 0.93
```

하지만 여기서 추가적인 질문이 생긴다.

```text
왜 이 모델은 이런 예측을 했을까?

어떤 Feature를 중요하게 사용했을까?

Feature 값이 커지면 예측은 어느 방향으로 변할까?

특정 환자를 왜 고위험이라고 판단했을까?
```

이러한 질문을 다루는 것이:

```text
Model Interpretation
모델 해석
```

이다.

---

# 2. 모델 해석의 두 가지 질문

크게 다음 두 가지를 생각할 수 있다.

```text
Global Interpretation
→ 모델 전체가 어떤 Feature를 중요하게 사용하는가?

Local Interpretation
→ 특정 Sample 하나를 왜 이렇게 예측했는가?
```

---

# 3. Global Interpretation

예:

```text
전체 환자를 예측할 때

나이
혈압
통증점수
BMI

중 어떤 변수를 모델이 많이 사용했는가?
```

를 보는 것이다.

대표적인 방법:

```text
Feature Importance

Coefficient

Permutation Importance

SHAP Summary
```

등이 있다.

---

# 4. Local Interpretation

특정 환자 한 명에 대해:

```text
왜 이 환자를 Class 1로 예측했는가?
```

를 분석하는 것이다.

예:

```text
높은 통증점수
→ 위험 예측 증가

높은 심박수
→ 위험 예측 증가

낮은 혈압
→ 위험 예측 증가
```

처럼 볼 수 있다.

대표적으로:

```text
SHAP
```

을 사용할 수 있다.

---

# 5. Feature Importance란?

Feature Importance는:

```text
모델이 예측 과정에서
각 Feature를 얼마나 중요하게 사용했는가?
```

를 나타내는 값이다.

예:

```text
Age             0.30
BMI             0.25
BloodPressure   0.20
Glucose         0.15
Gender          0.10
```

---

# 6. 중요한 주의점

Feature Importance가 높다고 해서:

```text
이 Feature가 Target의 원인이다.
```

라는 뜻은 아니다.

즉:

```text
Importance
≠
Causality
```

이다.

---

# 7. 예측과 인과관계

예를 들어:

```text
우산 판매량
```

과:

```text
교통사고 수
```

가 함께 증가했다고 하자.

둘 사이에 관계가 있다고 해서:

```text
우산이 교통사고를 일으킨다.
```

라고 결론 내릴 수는 없다.

비가 두 변수 모두에 영향을 주었을 수도 있다.

따라서:

```text
모델이 중요하게 사용했다
```

와:

```text
실제 원인이다
```

는 구분해야 한다.

---

# 8. Tree 기반 Feature Importance

다음과 같은 Tree 기반 모델에서는:

```text
Decision Tree

Random Forest

Extra Trees

Gradient Boosting
```

등에서 Feature Importance를 제공하는 경우가 있다.

Scikit-learn에서는 대표적으로:

```python
model.feature_importances_
```

를 사용할 수 있다.

---

# 9. Random Forest 예

```python
from sklearn.ensemble import (
    RandomForestClassifier
)


model = RandomForestClassifier(
    n_estimators=300,
    random_state=42
)


model.fit(
    X_train,
    y_train
)
```

---

# 10. Feature Importance 확인

```python
print(
    model.feature_importances_
)
```

예:

```text
[0.10 0.35 0.20 0.25 0.10]
```

하지만 숫자만 보면 어떤 Feature인지 알기 어렵다.

---

# 11. Feature 이름과 연결

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
```

---

# 12. 정렬

```python
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

# 13. 결과 예

```text
feature             importance

NRS_pain            0.23
HR                   0.18
SBP                  0.15
BT                   0.11
Age                  0.09
...
```

이런 결과라면 모델이 예측할 때 `NRS_pain`, `HR`, `SBP` 등을 상대적으로 많이 사용했다는 의미로 해석할 수 있다.

---

# 14. Feature Importance 시각화

```python
import matplotlib.pyplot as plt


top_n = 10


plot_df = (
    importance_df
    .head(
        top_n
    )
    .sort_values(
        "importance"
    )
)


plt.figure(
    figsize=(
        8,
        6
    )
)


plt.barh(
    plot_df[
        "feature"
    ],
    plot_df[
        "importance"
    ]
)


plt.xlabel(
    "Importance"
)

plt.ylabel(
    "Feature"
)

plt.title(
    "Top Feature Importance"
)

plt.show()
```

---

# 15. Tree Feature Importance의 의미

Tree 모델은 Feature를 이용하여 Node를 나눈다.

예:

```text
SBP < 90 ?
```

처럼 Feature를 사용해 데이터를 분리한다.

Feature Importance는 이런 Split 과정에서 해당 Feature가 얼마나 유용하게 사용되었는지를 기반으로 계산되는 경우가 많다.

---

# 16. 주의할 점

Tree의 기본 Feature Importance는 상황에 따라 편향될 수 있다.

특히:

```text
연속형 변수

가능한 값이 많은 변수
```

가 더 유리하게 평가되는 경우가 있다.

따라서 Feature Importance 하나만 절대적인 해석으로 사용하지 않는다.

---

# 17. Correlated Features 문제

두 Feature가 매우 비슷한 정보를 가지고 있다고 하자.

```text
체중

BMI
```

또는:

```text
수축기 혈압 A

수축기 혈압 B
```

처럼 강하게 관련된 Feature가 있을 수 있다.

이 경우 모델이 중요도를 둘 사이에 나누어 사용할 수 있다.

따라서:

```text
Importance가 낮다
=
쓸모없는 Feature
```

라고 단순하게 판단하면 안 된다.

---

# 18. Linear Model에서는?

Linear Regression이나 Logistic Regression에서는:

```text
Coefficient
계수
```

를 확인할 수 있다.

```python
model.coef_
```

---

# 19. Logistic Regression 예

```python
from sklearn.linear_model import (
    LogisticRegression
)


model = LogisticRegression(
    max_iter=1000
)


model.fit(
    X_train,
    y_train
)


print(
    model.coef_
)
```

---

# 20. Binary Logistic Regression

Binary Classification에서는 보통:

```python
model.coef_[0]
```

형태로 각 Feature의 Coefficient를 확인할 수 있다.

---

# 21. Feature 이름과 연결

```python
coef_df = pd.DataFrame(
    {
        "feature":
            X_train.columns,

        "coefficient":
            model.coef_[0]
    }
)
```

---

# 22. Coefficient 정렬

```python
coef_df[
    "abs_coefficient"
] = (
    coef_df[
        "coefficient"
    ].abs()
)


coef_df = (
    coef_df
    .sort_values(
        "abs_coefficient",
        ascending=False
    )
)


print(
    coef_df
)
```

---

# 23. Coefficient의 부호

Logistic Regression에서 다른 조건이 같다는 모델 내부의 관점에서:

```text
Coefficient > 0
→ Feature가 증가할수록
  Positive Class 방향과 연결

Coefficient < 0
→ Feature가 증가할수록
  Negative Class 방향과 연결
```

로 해석할 수 있다.

---

# 24. 하지만 Scale을 주의

예:

```text
Age
→ 20 ~ 80

Income
→ 1,000,000 ~ 100,000,000
```

처럼 Feature Scale이 다르면 Coefficient 크기를 단순 비교하기 어렵다.

그래서 Coefficient 크기를 비교할 목적이라면 Scaling을 고려해야 한다.

---

# 25. StandardScaler + Logistic Regression

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


pipeline.fit(
    X_train,
    y_train
)
```

---

# 26. Pipeline 내부 모델

```python
lr_model = (
    pipeline.named_steps[
        "model"
    ]
)


print(
    lr_model.coef_
)
```

---

# 27. Coefficient도 인과관계가 아니다

Coefficient가:

```text
+2.5
```

라고 해서:

```text
이 Feature가 질병을 발생시킨다.
```

는 뜻이 아니다.

Coefficient 역시:

```text
모델이 데이터에서 학습한 관계
```

이다.

---

# 28. Permutation Importance

Permutation Importance는 모델 종류와 비교적 독립적으로 사용할 수 있는 Feature Importance 방법이다.

핵심 아이디어:

```text
특정 Feature의 값을 무작위로 섞는다.
```

---

# 29. 왜 섞을까?

중요한 Feature라면 그 Feature의 값과 Target 사이의 유용한 정보가 깨졌을 때 모델 성능이 크게 떨어질 가능성이 있다.

즉:

```text
Feature 섞기
↓
Model Score 크게 하락
↓
중요한 Feature일 가능성
```

이다.

---

# 30. 반대로

```text
Feature 섞기
↓
Score 거의 변화 없음
```

이라면 현재 모델의 예측에서 해당 Feature 의존도가 낮을 가능성이 있다.

---

# 31. 중요한 장점

Permutation Importance는:

```text
이미 학습된 모델
```

의 예측 성능 변화를 이용한다.

Tree 모델뿐 아니라 다양한 모델에서 사용할 수 있다.

---

# 32. Permutation Importance Import

```python
from sklearn.inspection import (
    permutation_importance
)
```

---

# 33. 계산

가능하면 Training Data보다는 별도의 Validation/Test 성격의 데이터에서 계산하여 일반화 관점의 중요도를 보는 것이 유용하다.

```python
result = permutation_importance(
    model,
    X_valid,
    y_valid,
    n_repeats=10,
    random_state=42,
    scoring="roc_auc"
)
```

---

# 34. n_repeats

한 번만 섞으면 Randomness의 영향을 받을 수 있다.

```python
n_repeats=10
```

은 각 Feature를 여러 번 섞어 결과를 반복 측정한다.

---

# 35. 결과

```python
result.importances_mean
```

은 평균 Importance를 제공한다.

```python
result.importances_std
```

는 반복 결과의 변동성을 보여준다.

---

# 36. DataFrame

```python
perm_df = pd.DataFrame(
    {
        "feature":
            X_valid.columns,

        "importance_mean":
            result.importances_mean,

        "importance_std":
            result.importances_std
    }
)
```

---

# 37. 정렬

```python
perm_df = (
    perm_df
    .sort_values(
        "importance_mean",
        ascending=False
    )
)


print(
    perm_df.head(
        10
    )
)
```

---

# 38. Negative Permutation Importance

가끔 Importance가 음수일 수도 있다.

이는 해당 Feature를 섞었을 때 평가 Score가 오히려 조금 좋아졌다는 뜻이다.

Random Variation이나 Feature 간 관계 등 여러 이유가 있을 수 있다.

따라서 작은 음수 하나만 보고 바로 Feature를 제거하지 않는다.

---

# 39. Feature Importance 방법 비교

```text
Tree Feature Importance
→ Tree 내부 Split 기반

Coefficient
→ Linear Model의 방향과 크기

Permutation Importance
→ Feature를 섞었을 때 성능 변화

SHAP
→ 각 Feature가 예측값에 기여한 정도
```

이다.

---

# 40. SHAP이란?

SHAP:

```text
SHapley Additive exPlanations
```

이다.

Game Theory의 Shapley Value 아이디어를 Machine Learning 모델 해석에 적용한 방법이다.

---

# 41. SHAP의 핵심 질문

```text
이 Feature가
현재 예측값을
얼마나 올리거나 내렸는가?
```

를 분석한다.

---

# 42. 쉬운 예

어떤 환자의 기본 위험 수준이:

```text
0
```

이라고 단순하게 생각해보자.

Feature 영향:

```text
높은 통증점수
→ +0.8

높은 심박수
→ +0.5

정상 체온
→ -0.2
```

최종적으로 Positive 방향으로 예측이 이동했다고 해석할 수 있다.

실제 SHAP 값의 단위와 출력 공간은 사용하는 모델과 Explainer 설정에 따라 달라질 수 있다.

---

# 43. SHAP Value

각 Sample과 각 Feature에 대해:

```text
SHAP Value
```

가 계산된다.

개념적으로:

```text
SHAP > 0
→ 모델 출력값을 더 높은 방향으로 밀어줌

SHAP < 0
→ 모델 출력값을 더 낮은 방향으로 밀어줌
```

이다.

단, 어떤 Class/출력 공간을 설명하는지 반드시 확인해야 한다.

---

# 44. 중요한 주의

Binary Classification에서:

```text
SHAP 양수
=
무조건 Class 1 확률 증가
```

라고 코드와 모델 구조를 확인하지 않고 단정하면 안 된다.

사용하는:

```text
SHAP 버전

Explainer

Model

Output

Class
```

에 따라 반환 형태가 달라질 수 있다.

---

# 45. SHAP 설치

PowerShell:

```powershell
pip install shap
```

Jupyter / Colab:

```python
!pip install shap
```

---

# 46. Import

```python
import shap
```

---

# 47. 최신 통합 인터페이스

SHAP에서는 모델에 따라 다음과 같이 사용할 수 있다.

```python
explainer = shap.Explainer(
    model,
    X_train
)
```

그리고:

```python
shap_values = explainer(
    X_valid
)
```

---

# 48. 왜 X_train을 넣는가?

Explainer 종류에 따라 Background 또는 Reference Data가 필요할 수 있다.

이 Reference를 기준으로:

```text
현재 Sample의 Feature가
예측을 어느 방향으로 변화시키는가
```

를 설명한다.

---

# 49. Tree 모델

Tree 기반 모델에서는 SHAP이 Tree 구조를 활용해 효율적으로 계산될 수 있다.

예:

```python
model = RandomForestClassifier(
    n_estimators=300,
    random_state=42
)


model.fit(
    X_train,
    y_train
)


explainer = shap.Explainer(
    model,
    X_train
)


shap_values = explainer(
    X_valid
)
```

---

# 50. 반환 Shape 확인

SHAP에서는 먼저:

```python
print(
    shap_values.shape
)
```

등으로 반환 구조를 확인하는 습관이 중요하다.

Classification에서는 버전과 모델에 따라 Class 차원이 포함될 수 있다.

---

# 51. SHAP Plot 전에 확인

무작정:

```python
shap.summary_plot(...)
```

부터 실행하기보다:

```python
print(
    type(
        shap_values
    )
)

print(
    shap_values.shape
)
```

처럼 결과 구조를 먼저 확인한다.

---

# 52. SHAP Global Importance

SHAP에서도 전체적으로 중요한 Feature를 볼 수 있다.

대표적인 방법은:

```text
Mean Absolute SHAP Value
```

이다.

즉:

```text
각 Feature의
|SHAP Value| 평균
```

을 계산한다.

---

# 53. 왜 Absolute Value?

어떤 Feature가:

```text
어떤 Sample에서는 +2

다른 Sample에서는 -2
```

라고 하자.

그냥 평균하면:

```text
0
```

이 되어 중요하지 않아 보일 수 있다.

그래서 전체 중요도에서는:

```text
|SHAP|
```

을 사용하는 경우가 많다.

---

# 54. SHAP Bar Plot

SHAP Explanation 객체가 적절한 형태라면:

```python
shap.plots.bar(
    shap_values
)
```

를 이용해 Global Importance를 볼 수 있다.

Classification에서는 출력 차원이 추가될 수 있으므로 먼저 Shape와 설명 대상 Class를 확인한다.

---

# 55. SHAP Beeswarm Plot

```python
shap.plots.beeswarm(
    shap_values
)
```

은 Feature 중요도뿐 아니라:

```text
Feature 값

SHAP 방향

Sample 분포
```

까지 함께 볼 수 있는 대표적인 Plot이다.

---

# 56. Beeswarm Plot 읽기

일반적인 SHAP Beeswarm Plot에서:

```text
Y축
→ Feature

X축
→ SHAP Value

점 하나
→ Sample 하나

색
→ Feature 값의 상대적 크기
```

를 나타낸다.

---

# 57. 오른쪽에 있는 점

설명 중인 모델 출력에 대해:

```text
SHAP Value > 0
```

이면 해당 Feature가 모델 출력을 증가시키는 방향으로 기여했다는 뜻이다.

---

# 58. 왼쪽에 있는 점

```text
SHAP Value < 0
```

이면 해당 Feature가 모델 출력을 감소시키는 방향으로 기여했다는 뜻이다.

---

# 59. 색까지 같이 본다

예를 들어 어떤 Feature에서:

```text
높은 값의 점들이 주로 오른쪽
```

이라면:

```text
Feature 값이 높은 경우
설명 대상 출력이 증가하는 경향
```

을 모델이 학습했을 가능성을 볼 수 있다.

---

# 60. 반대 패턴

```text
높은 값
→ 왼쪽

낮은 값
→ 오른쪽
```

이라면 Feature 값이 높을수록 모델 출력을 낮추는 방향의 관계를 학습했을 가능성이 있다.

---

# 61. 비선형 관계

SHAP의 장점 중 하나는 단순한:

```text
Feature 값 증가
→ 항상 위험 증가
```

같은 선형 관계뿐 아니라 복잡한 모델의 비선형 관계도 분석할 수 있다는 것이다.

---

# 62. Dependence / Scatter Plot

Feature 하나의 값과 SHAP Value 관계를 볼 수 있다.

Explanation 객체에서는 예를 들어:

```python
shap.plots.scatter(
    shap_values[
        :,
        "Age"
    ]
)
```

와 같은 형태를 사용할 수 있다.

단, 실제 사용 가능한 indexing 형태는 SHAP 결과 구조를 확인해야 한다.

---

# 63. 무엇을 볼 수 있을까?

예:

```text
Age < 40
→ SHAP 영향 작음

Age 40~60
→ 점차 증가

Age > 70
→ 크게 증가
```

처럼 모델이 학습한 비선형 패턴을 확인할 수 있다.

---

# 64. SHAP Interaction

Tree 모델 등에서는 Feature 사이의 Interaction을 분석하는 방법도 있다.

예:

```text
Age의 영향이
BMI에 따라 달라진다.
```

같은 관계이다.

하지만 계산량과 해석 난도가 올라가므로 기본 SHAP 해석을 먼저 익힌다.

---

# 65. Local Explanation

이제 특정 Sample 한 개를 보자.

예:

```python
sample = X_valid.iloc[
    [
        0
    ]
]
```

---

# 66. 한 Sample 설명

```python
sample_shap = explainer(
    sample
)
```

---

# 67. Waterfall Plot

```python
shap.plots.waterfall(
    sample_shap[
        0
    ]
)
```

을 사용할 수 있다.

단, Classification 결과가 Class 차원을 포함한다면 설명할 Class를 먼저 선택해야 할 수 있다.

---

# 68. Waterfall Plot의 의미

Waterfall Plot은:

```text
Base Value
↓
Feature A 영향
↓
Feature B 영향
↓
Feature C 영향
↓
최종 Model Output
```

흐름을 보여준다.

---

# 69. Base Value

SHAP에서:

```text
Base Value
```

는 Background Data를 기준으로 한 모델의 기준 출력값이다.

여기에서 각 Feature의 SHAP Value가 더해져 현재 Sample의 Model Output으로 이동한다.

---

# 70. Additive 구조

개념적으로:

```text
Model Output
≈
Base Value
+
SHAP Feature 1
+
SHAP Feature 2
+
...
```

형태로 설명한다.

---

# 71. 한 환자 예

가상의 결과:

```text
Base
= -1.0

NRS_pain
= +0.8

HR
= +0.6

SBP
= +0.3

BT
= -0.2
```

라면:

```text
NRS_pain, HR, SBP
→ 예측을 높은 방향으로 이동

BT
→ 반대 방향으로 이동
```

했다고 해석할 수 있다.

---

# 72. 중요한 표현

발표에서:

```text
NRS_pain이 환자의 상태를 악화시켰다.
```

라고 말하면 인과관계처럼 들린다.

더 안전한 표현:

```text
이 모델에서는 NRS_pain이
Positive 예측 방향에 큰 영향을 준 Feature로 나타났다.
```

이다.

---

# 73. Global과 Local의 차이

```text
Global
→ 전체 데이터에서 모델이 무엇을 중요하게 보는가?

Local
→ 이 Sample 하나를 왜 이렇게 예측했는가?
```

이다.

---

# 74. Feature Importance는 Global 중심

```python
model.feature_importances_
```

는 일반적으로:

```text
모델 전체에서
어떤 Feature가 중요한가?
```

를 보는 데 사용한다.

---

# 75. SHAP은 둘 다 가능

SHAP:

```text
Global Interpretation
+
Local Interpretation
```

둘 다 가능하다.

---

# 76. SHAP Summary에서 중요한 Feature

예:

```text
NRS_pain
HR
SBP
Age
BT
```

순서로 Mean Absolute SHAP Value가 크다면:

```text
전체 예측에서 이 Feature들의 영향 크기가 상대적으로 컸다.
```

라고 해석할 수 있다.

---

# 77. Feature Importance와 SHAP 순위가 다를 수 있다

예:

Random Forest Importance:

```text
1. Age
2. BMI
3. HR
```

SHAP:

```text
1. HR
2. Age
3. BMI
```

일 수 있다.

계산 방법 자체가 다르기 때문에 이상한 것이 아니다.

---

# 78. 어떤 것이 정답?

하나를 절대적인 정답이라고 생각하지 않는다.

각 방법은:

```text
서로 다른 관점
```

에서 모델을 설명한다.

가능하면 여러 방법을 비교한다.

---

# 79. Permutation과 SHAP도 다르다

Permutation Importance:

```text
이 Feature 정보를 깨뜨렸을 때
성능이 얼마나 떨어지는가?
```

SHAP:

```text
이 Feature가 각 Prediction에
얼마나 기여했는가?
```

를 본다.

---

# 80. 해석 전에 성능부터 확인

매우 중요한 원칙:

```text
성능이 형편없는 모델
↓
SHAP 분석
```

을 해도:

```text
나쁜 모델이 왜 나쁜 예측을 했는지
설명하는 것
```

일 수 있다.

먼저 모델이 충분히 검증되었는지 확인한다.

---

# 81. 올바른 흐름

```text
Data
↓
Train / Validation / Test
↓
Model
↓
Evaluation
↓
성능 확인
↓
Feature Importance / SHAP
↓
Error Analysis
```

이다.

---

# 82. Data Leakage가 있으면?

Leakage가 있는 모델에서:

```text
Target과 거의 같은 Feature
```

가 가장 중요한 Feature로 나올 수 있다.

이것을 보고:

```text
엄청 중요한 Feature를 발견했다.
```

라고 생각하면 안 된다.

먼저 Leakage 여부를 확인해야 한다.

---

# 83. ID가 중요하게 나온다면?

예:

```text
patient_id
```

같은 ID Column이 높은 Importance를 보인다면:

```text
ID가 실제 의미를 가진 것인가?

우연한 Pattern인가?

Data Leakage인가?

Train/Test 구조의 Artifact인가?
```

를 의심해야 한다.

---

# 84. Feature Importance는 EDA 도구와 다르다

EDA에서:

```text
Correlation

Group Mean

Statistical Test
```

등을 본다.

Feature Importance는:

```text
학습된 모델이
Feature를 어떻게 활용했는가
```

를 본다.

둘은 목적이 다르다.

---

# 85. Correlation이 낮아도 중요할 수 있다

Pearson Correlation은 주로 선형 관계를 본다.

하지만 Tree Model은:

```text
비선형 관계

Feature Interaction
```

을 사용할 수 있다.

따라서:

```text
Correlation 낮음
=
모델에서 중요하지 않음
```

이라고 단정할 수 없다.

---

# 86. p-value가 높아도 중요할 수 있다

통계적 가설검정과 Machine Learning Feature Importance도 목적이 다르다.

```text
p-value
→ 통계적 관계에 대한 검정

Feature Importance
→ 모델 예측에서의 활용 정도
```

이다.

따라서 p-value만으로 Feature를 자동 삭제하면 안 된다.

---

# 87. Importance가 낮으면 제거?

바로 제거하지 않는다.

먼저:

```text
Validation / CV 성능

Feature 간 상관

Model 종류

Permutation Importance

SHAP

Domain Knowledge
```

등을 함께 확인한다.

---

# 88. Feature 제거 실험

가장 확실한 방법 중 하나:

```text
Feature 포함 모델
vs
Feature 제거 모델
```

을 같은 Cross Validation 조건에서 비교하는 것이다.

---

# 89. Ablation Study

이런 식으로 일부 Feature나 구성요소를 제거하고 성능 변화를 비교하는 실험을:

```text
Ablation Study
```

라고 부르기도 한다.

예:

```text
Baseline
→ AUC 0.85

BMI 제거
→ AUC 0.84

Age 제거
→ AUC 0.78
```

이라면 Age 제거가 성능에 더 큰 영향을 준 것으로 볼 수 있다.

---

# 90. Feature Selection과 연결

Feature Importance를 Feature Selection 참고 자료로 사용할 수 있다.

하지만:

```text
Importance가 낮은 Feature 전부 삭제
```

처럼 기계적으로 사용하지 않는다.

삭제 후 반드시 CV 성능을 다시 확인한다.

---

# 91. Multi-class SHAP

Multi-class Classification에서는:

```text
Class 0에 대한 설명

Class 1에 대한 설명

Class 2에 대한 설명
```

처럼 Class별 SHAP 결과가 존재할 수 있다.

따라서:

```text
어느 Class를 설명하고 있는가?
```

를 반드시 확인한다.

---

# 92. One-Hot Encoding 후 Feature 이름

Categorical Feature를 One-Hot Encoding하면:

```text
job
```

하나가:

```text
job_office
job_student
job_unemployed
```

처럼 여러 Feature로 바뀔 수 있다.

따라서 Importance도 변환된 Feature 단위로 나온다.

---

# 93. Pipeline에서 Feature 이름 문제

`ColumnTransformer`와 `OneHotEncoder`를 사용했다면 원본:

```text
Age
Job
Gender
```

와 실제 모델 입력 Feature 이름이 달라질 수 있다.

---

# 94. 변환된 Feature 이름 가져오기

예:

```python
feature_names = (
    pipeline
    .named_steps[
        "preprocessor"
    ]
    .get_feature_names_out()
)
```

처럼 가져올 수 있다.

Pipeline 구조에 따라 실제 단계 이름은 달라진다.

---

# 95. 모델 Importance와 연결

```python
model = (
    pipeline
    .named_steps[
        "model"
    ]
)


importance_df = pd.DataFrame(
    {
        "feature":
            feature_names,

        "importance":
            model.feature_importances_
    }
)
```

---

# 96. Shape 확인

항상:

```python
print(
    len(
        feature_names
    )
)


print(
    len(
        model.feature_importances_
    )
)
```

를 확인한다.

두 길이가 같아야 정확히 연결할 수 있다.

---

# 97. SHAP도 Shape가 중요

예:

```text
X_valid shape
=
(200, 20)
```

이면 일반적인 Regression 설명에서는 SHAP 값도 Sample × Feature 구조를 기대할 수 있다.

하지만 Classification에서는 Class 차원이 추가될 수 있다.

따라서:

```text
무조건 Shape를 먼저 확인
```

한다.

---

# 98. SHAP 계산량

SHAP은 모델과 Explainer에 따라 계산량이 커질 수 있다.

데이터가 매우 많다면 처음에는 일부 Sample로 분석할 수 있다.

예:

```python
X_explain = X_valid.sample(
    n=200,
    random_state=42
)
```

---

# 99. Background Data도 줄일 수 있다

Explainer 종류에 따라 Background Data가 너무 크면 계산량이 증가할 수 있다.

예:

```python
X_background = X_train.sample(
    n=200,
    random_state=42
)
```

처럼 대표 Sample을 사용할 수 있다.

단, Background 선택이 설명 결과에 영향을 줄 수 있다는 점을 이해해야 한다.

---

# 100. Random Forest 전체 예제

```python
import pandas as pd

from sklearn.ensemble import (
    RandomForestClassifier
)

from sklearn.metrics import (
    roc_auc_score
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


prob = model.predict_proba(
    X_valid
)[:, 1]


auc = roc_auc_score(
    y_valid,
    prob
)


print(
    "Validation ROC-AUC:",
    auc
)


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
    importance_df.head(
        10
    )
)
```

---

# 101. 여기서 중요한 순서

```text
Model 학습

↓

Validation 성능 확인

↓

Feature Importance 확인
```

이다.

Importance부터 보는 것이 아니다.

---

# 102. Permutation Importance 전체 예

```python
from sklearn.inspection import (
    permutation_importance
)


result = permutation_importance(
    model,
    X_valid,
    y_valid,
    scoring="roc_auc",
    n_repeats=10,
    random_state=42,
    n_jobs=-1
)


perm_df = pd.DataFrame(
    {
        "feature":
            X_valid.columns,

        "importance_mean":
            result.importances_mean,

        "importance_std":
            result.importances_std
    }
)


perm_df = (
    perm_df
    .sort_values(
        "importance_mean",
        ascending=False
    )
)


print(
    perm_df.head(
        10
    )
)
```

---

# 103. 두 Importance 비교

```python
print(
    importance_df.head(
        10
    )
)


print(
    perm_df.head(
        10
    )
)
```

두 결과가 비슷한지, 다른지 확인할 수 있다.

---

# 104. SHAP 기본 흐름

```text
Model 학습
↓
Explainer 생성
↓
SHAP Value 계산
↓
Global Plot
↓
Local Plot
↓
모델 예측 해석
```

이다.

---

# 105. SHAP 예제

SHAP API는 모델과 버전에 따라 반환 구조가 다를 수 있으므로 먼저 구조를 확인한다.

```python
import shap


X_background = X_train.sample(
    n=min(
        200,
        len(
            X_train
        )
    ),
    random_state=42
)


X_explain = X_valid.sample(
    n=min(
        200,
        len(
            X_valid
        )
    ),
    random_state=42
)


explainer = shap.Explainer(
    model,
    X_background
)


shap_values = explainer(
    X_explain
)


print(
    type(
        shap_values
    )
)


print(
    shap_values.shape
)
```

---

# 106. 왜 바로 Plot을 안 그릴까?

먼저:

```text
반환 Shape

Class Dimension

Feature Dimension
```

을 확인해야 잘못된 Class를 해석하는 실수를 줄일 수 있기 때문이다.

---

# 107. Global Plot

결과 구조가:

```text
Sample × Feature
```

형태라면:

```python
shap.plots.bar(
    shap_values
)
```

또는:

```python
shap.plots.beeswarm(
    shap_values
)
```

을 사용할 수 있다.

---

# 108. Class Dimension이 있다면?

예를 들어 결과가 개념적으로:

```text
Sample
×
Feature
×
Class
```

라면 먼저 원하는 Class에 해당하는 Explanation을 선택해야 한다.

실제 선택 코드는 SHAP 버전과 반환 Shape에 맞게 작성한다.

중요한 것은:

```text
Shape 확인 없이
Class 1이라고 가정하지 않는다.
```

이다.

---

# 109. Local SHAP

원하는 Sample을 선택한다.

```python
sample = X_valid.iloc[
    [
        0
    ]
]


sample_shap = explainer(
    sample
)
```

그리고 반환 구조를 확인한다.

```python
print(
    sample_shap.shape
)
```

---

# 110. Waterfall

설명 대상 Class까지 올바르게 선택한 뒤:

```python
shap.plots.waterfall(
    explanation_for_one_sample
)
```

을 사용한다.

Waterfall은:

```text
어떤 Feature가
예측을 어느 방향으로
얼마나 이동시켰는가?
```

를 보는 데 유용하다.

---

# 111. 예측이 틀린 Sample 분석

SHAP의 유용한 활용 중 하나이다.

먼저:

```python
pred = model.predict(
    X_valid
)
```

오분류 Index:

```python
wrong_mask = (
    pred
    !=
    y_valid.to_numpy()
)
```

---

# 112. 오분류 Sample

```python
wrong_indices = (
    X_valid.index[
        wrong_mask
    ]
)


print(
    wrong_indices[
        :10
    ]
)
```

---

# 113. 왜 틀렸는지 분석

오분류 Sample 하나를 골라:

```text
실제 Label

예측 Label

예측 Probability

Feature 값

SHAP Value
```

를 같이 본다.

---

# 114. Failure Case Analysis

이 과정을:

```text
Failure Case Analysis
```

라고 볼 수 있다.

예:

```text
True = 0
Pred = 1
P(Class 1) = 0.96
```

이라면 모델이 강하게 틀린 사례이다.

---

# 115. High-confidence Error

모델이 높은 Confidence로 틀린 Sample은 특히 분석 가치가 있다.

예:

```text
True = 0

Pred = 1

P1 = 0.99
```

이 경우:

```text
어떤 Feature 때문에
모델이 강하게 1이라고 판단했는가?
```

를 SHAP으로 분석할 수 있다.

---

# 116. Low-confidence Error

예:

```text
True = 0

Pred = 1

P1 = 0.51
```

이면 Decision Boundary 근처에서 애매하게 틀린 Sample일 수 있다.

High-confidence Error와 의미가 다르다.

---

# 117. Correct Case도 비교

틀린 Sample만 보지 않고:

```text
Correct Positive

Correct Negative

High-confidence Error

Low-confidence Error
```

를 비교하면 모델의 판단 패턴을 이해하기 쉽다.

---

# 118. Error Analysis의 목적

단순히:

```text
왜 틀렸지?
```

로 끝나는 것이 아니다.

다음 실험 아이디어를 얻을 수 있다.

예:

```text
Feature Engineering 필요?

Label 오류?

Outlier?

Data Quality 문제?

Threshold 문제?

새 Feature 필요?

Class Imbalance 문제?
```

등이다.

---

# 119. SHAP과 Threshold

SHAP은 기본적으로 모델의 출력 설명과 관련된 도구이다.

Threshold:

```text
0.3

0.5

0.7
```

는 그 출력으로 최종 Class를 결정하는 별도의 단계이다.

즉:

```text
Model Score
↓
SHAP으로 Score 해석

Model Score
↓
Threshold
↓
Class Prediction
```

으로 구분할 수 있다.

---

# 120. SHAP과 Feature Importance는 성능 Metric이 아니다

다음은 성능 Metric:

```text
Accuracy
F1
ROC-AUC
```

다음은 해석 방법:

```text
Feature Importance
Permutation Importance
SHAP
```

이다.

혼동하지 않는다.

---

# 121. 높은 Importance = 좋은 Feature?

반드시 그렇지는 않다.

높은 Importance가 나온 Feature가:

```text
Leakage Feature

잘못 수집된 Feature

ID Artifact
```

일 수도 있다.

따라서 Data 의미를 함께 확인해야 한다.

---

# 122. 의료 AI에서 특히 주의

예를 들어:

```text
수술 여부
```

를 수술 전 예측 모델에 넣었는데 이 정보가 실제로는 결과 이후 결정되는 변수라면:

```text
Target Leakage
```

가 될 수 있다.

Importance가 매우 높게 나와도 좋은 Feature가 아니다.

---

# 123. Prediction Time Availability

항상 질문한다.

```text
실제 예측 시점에
이 Feature를 알고 있는가?
```

알 수 없다면 모델 입력으로 사용하면 안 될 가능성이 높다.

---

# 124. SHAP도 모델의 편향을 설명할 수 있다

모델이 데이터의 Bias를 학습했다면 SHAP도 그 Bias를 설명할 수 있다.

즉:

```text
SHAP 결과가 그럴듯하다
=
모델이 공정하고 올바르다
```

는 뜻이 아니다.

---

# 125. 설명 가능성과 신뢰성

Explainability는 중요하지만:

```text
설명이 가능하다
=
모델이 정확하다

설명이 가능하다
=
인과관계가 맞다

설명이 가능하다
=
안전하다
```

라는 뜻은 아니다.

---

# 126. Model Interpretation의 올바른 위치

```text
Data Quality
↓
Leakage Check
↓
Validation
↓
Performance
↓
Interpretation
↓
Error Analysis
↓
추가 실험
```

이다.

---

# 127. 발표에서 Feature Importance 설명

예:

```text
Random Forest Feature Importance 분석 결과,
NRS_pain, HR, SBP 순으로
모델 예측에서 높은 중요도를 보였습니다.
```

정도로 표현할 수 있다.

---

# 128. 피해야 할 표현

```text
NRS_pain이 가장 중요한 원인입니다.
```

처럼 인과관계로 단정하지 않는다.

---

# 129. SHAP 발표 표현

예:

```text
SHAP 분석에서도 NRS_pain과 HR 등이
모델 출력에 상대적으로 큰 영향을 주는
Feature로 나타났습니다.
```

처럼 표현할 수 있다.

---

# 130. 특정 Sample 설명

예:

```text
해당 Sample에서는
높은 NRS_pain과 HR이
Positive 예측 방향으로 크게 기여했고,
BT는 반대 방향으로 기여했습니다.
```

처럼 설명할 수 있다.

---

# 131. 모델 전체와 개인 예측을 연결

```text
Feature Importance
→ 전체 모델

SHAP Summary
→ 전체 모델 + 방향

SHAP Waterfall
→ Sample 하나
```

라고 기억하면 쉽다.

---

# 132. Feature Importance vs SHAP

| 방법 | 무엇을 보는가? |
|---|---|
| Tree Importance | Tree가 Feature를 얼마나 사용했는가 |
| Coefficient | Linear Model에서 Feature 방향과 크기 |
| Permutation Importance | Feature를 섞었을 때 성능이 얼마나 감소하는가 |
| SHAP Global | 전체 예측에서 Feature 영향 크기 |
| SHAP Local | 특정 Sample 예측에 Feature가 어떻게 기여했는가 |

---

# 133. 해석 순서 추천

```text
1. Validation / CV 성능 확인

2. Feature Importance 확인

3. Permutation Importance 확인

4. SHAP Global 확인

5. SHAP Local 확인

6. 오분류 Sample 확인

7. 새로운 가설 수립

8. 다시 실험
```

---

# 134. 예시 전체 흐름

```text
Random Forest 학습
       ↓
ROC-AUC = 0.87
       ↓
Feature Importance
       ↓
NRS_pain / HR / SBP
       ↓
Permutation Importance
       ↓
비슷한 Feature 확인
       ↓
SHAP Summary
       ↓
Feature 영향 방향 확인
       ↓
High-confidence Error 선택
       ↓
SHAP Waterfall
       ↓
왜 틀렸는지 분석
       ↓
새로운 Feature / Threshold / 모델 실험
```

---

# 135. Feature Importance 체크리스트

```text
□ 모델 성능을 먼저 확인했는가?

□ Feature 이름과 Importance 길이가 같은가?

□ Leakage Feature가 없는가?

□ ID가 포함되어 있지 않은가?

□ Correlated Feature를 확인했는가?

□ Importance를 인과관계로 해석하지 않았는가?

□ Validation/CV 기준으로 모델을 검증했는가?

□ 다른 Importance 방법과 비교했는가?
```

---

# 136. SHAP 체크리스트

```text
□ SHAP Package가 설치되어 있는가?

□ 어떤 Model을 설명하는가?

□ 어떤 Data를 Background로 사용하는가?

□ SHAP 결과 Shape를 확인했는가?

□ 어떤 Class를 설명하는지 확인했는가?

□ SHAP Value 방향을 무조건 Probability로 해석하지 않았는가?

□ Global과 Local을 구분했는가?

□ 인과관계로 단정하지 않았는가?

□ Error Case도 분석했는가?
```

---

# 137. 흔한 실수

```text
Feature Importance가 높으면 원인이라고 생각

Importance가 낮으면 바로 Feature 삭제

SHAP Plot만 보고 모델 성능은 확인하지 않음

Class Dimension을 확인하지 않고 SHAP 해석

Test Data를 계속 보면서 Feature Engineering

ID Leakage를 중요 Feature라고 생각

Coefficient Scale을 무시하고 크기 비교

Correlation과 Feature Importance를 같은 개념으로 생각
```

---

# 138. 가장 중요한 차이

```text
Correlation
→ 데이터 변수 간 관계

Statistical Test
→ 통계적 차이/관계 검정

Feature Importance
→ 모델이 Feature를 활용한 정도

SHAP
→ Feature가 모델 Prediction에 기여한 정도
```

이다.

---

# 139. 머신러닝 분석의 흐름

```text
EDA
↓
Preprocessing
↓
Feature Engineering
↓
Train / Validation / Test
↓
Model
↓
Evaluation Metrics
↓
Feature Importance
↓
SHAP
↓
Failure Case Analysis
↓
다음 실험
```

이렇게 지금까지 배운 내용이 연결된다.

---

# 140. 핵심 용어

```text
Feature Importance
→ 모델에서 Feature의 상대적인 중요도

Coefficient
→ Linear Model이 학습한 Feature 계수

Permutation Importance
→ Feature를 섞었을 때 발생하는 성능 변화

SHAP
→ Feature별 Prediction 기여도

Global Interpretation
→ 모델 전체 설명

Local Interpretation
→ 특정 Sample 설명

Base Value
→ SHAP 설명의 기준 모델 출력

SHAP Value
→ Feature가 모델 출력에 기여한 값

Failure Case
→ 모델이 잘못 예측한 사례

High-confidence Error
→ 높은 확신으로 틀린 Prediction
```

---

# 141. 가장 중요한 그림

```text
              Model
                ↓
          좋은 성능인가?
                ↓
        Evaluation Metrics
                ↓
          왜 예측했는가?
                ↓
    ┌───────────┴───────────┐
    ↓                       ↓
Global                    Local
    ↓                       ↓
Feature Importance       SHAP
Permutation              Waterfall
SHAP Summary                 ↓
    ↓                 Sample 하나 분석
전체 모델 분석                ↓
    └───────────┬───────────┘
                ↓
        Failure Case Analysis
                ↓
          다음 실험 설계
```

---

# 142. 한 문장으로 기억하기

```text
Feature Importance
→ "모델 전체에서 무엇이 중요했나?"

SHAP
→ "무엇이 예측을 어느 방향으로 밀었나?"

Local SHAP
→ "이 Sample은 왜 이렇게 예측됐나?"
```

---

# 143. 배운 점

Machine Learning에서는 모델의 성능만 확인하는 것으로 끝나지 않는다.

예:

```text
ROC-AUC = 0.92
```

라고 하더라도:

```text
모델이 어떤 Feature를 사용했는가?

Leakage는 없는가?

특정 Sample을 왜 잘못 예측했는가?
```

를 분석할 필요가 있다.

대표적인 방법:

```text
Feature Importance

Coefficient

Permutation Importance

SHAP
```

이 있다.

하지만:

```text
Feature Importance
≠
Causality
```

라는 점을 반드시 기억해야 한다.

그리고 SHAP은:

```text
Global Interpretation
+
Local Interpretation
```

에 활용할 수 있다.

최종적으로 중요한 흐름은:

```text
모델 학습
↓
성능 평가
↓
Feature Importance
↓
SHAP
↓
Failure Case Analysis
↓
문제 발견
↓
새로운 실험
```

이다.

즉 모델 해석의 목적은 단순히 예쁜 그래프를 만드는 것이 아니라:

```text
모델이 무엇을 학습했는지 이해하고
문제를 발견하여
다음 모델을 더 잘 만드는 것
```

이다.