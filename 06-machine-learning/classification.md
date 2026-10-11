# Classification 분류

## 1. Classification이란?

Classification은 입력 데이터를 미리 정의된 Class 중 하나로 분류하는 Machine Learning 문제이다.

한국어로:

```text
Classification
→ 분류
```

라고 한다.

예:

```text
정상 / 질병

승인 / 거절

스팸 / 정상 메일

정상 / 폐렴

고양이 / 강아지
```

---

# 2. Classification의 기본 구조

```text
Feature
   ↓
Model
   ↓
Class
```

예:

```text
나이
BMI
혈당
   ↓
분류 모델
   ↓
0 또는 1
```

---

# 3. Feature와 Target

예를 들어:

| age | BMI | glucose | disease |
|---:|---:|---:|---:|
| 25 | 21.5 | 90 | 0 |
| 55 | 30.2 | 170 | 1 |
| 43 | 25.7 | 125 | 0 |
| 67 | 31.8 | 190 | 1 |

여기서:

```text
age
BMI
glucose
```

는 Feature이고:

```text
disease
```

는 Target이다.

---

# 4. X와 y

```python
X = df[
    [
        "age",
        "BMI",
        "glucose"
    ]
]

y = df[
    "disease"
]
```

즉:

```text
X
→ 모델에게 주는 정보

y
→ 모델이 맞혀야 하는 정답
```

이다.

---

# 5. Class

Target이 가질 수 있는 각각의 범주를:

```text
Class
```

라고 한다.

예:

```text
0 = 정상
1 = 질병
```

이라면 Class는 두 개이다.

---

# 6. Binary Classification

Class가 두 개인 문제를:

```text
Binary Classification
```

이라고 한다.

한국어:

```text
이진 분류
```

이다.

예:

```text
0 / 1

정상 / 질병

승인 / 거절

성공 / 실패
```

---

# 7. Multi-class Classification

Class가 3개 이상이면:

```text
Multi-class Classification
```

이라고 한다.

예:

```text
Class 0
Class 1
Class 2
Class 3
```

---

# 8. 이미지 분류 예

예를 들어 여러 장기를 분류한다면:

```text
복부 CT 이미지
      ↓
분류 Model
      ↓
Liver
Kidney
Pancreas
...
```

처럼 여러 Class 중 하나를 예측할 수 있다.

---

# 9. Label

분류 문제에서 실제 Class 값을:

```text
Label
```

이라고 부르기도 한다.

예:

```text
Normal
→ 0

Pneumonia
→ 1
```

---

# 10. Class 숫자는 크기의 의미가 아닐 수 있다

예:

```text
0 = Normal
1 = Pneumonia
```

에서:

```text
1이 0보다 크기 때문에
폐렴이 더 큰 값이다.
```

라는 수학적인 의미가 있는 것은 아니다.

숫자는 Class를 구분하기 위한 Label일 수 있다.

---

# 11. 분류 모델의 학습

기본 구조:

```python
model.fit(
    X_train,
    y_train
)
```

이다.

모델은:

```text
Feature
↓
어떤 Class와 연결되는가?
```

라는 패턴을 학습한다.

---

# 12. 예측

학습 후:

```python
pred = model.predict(
    X_test
)
```

를 사용한다.

예:

```text
pred

0
1
1
0
1
```

처럼 Class 결과를 얻을 수 있다.

---

# 13. 실제 정답과 비교

```text
y_test

0
1
0
0
1
```

모델 예측:

```text
pred

0
1
1
0
1
```

두 값을 비교하여 모델 성능을 평가한다.

---

# 14. 대표적인 분류 알고리즘

대표적으로:

```text
Logistic Regression

Decision Tree

Random Forest

Gradient Boosting

XGBoost

LightGBM

CatBoost

KNN

SVM
```

등이 있다.

---

# 15. Logistic Regression

Logistic Regression은 대표적인 분류 알고리즘이다.

이름에:

```text
Regression
```

이 들어가지만 주로 분류에 사용한다.

특히:

```text
Binary Classification
```

에서 자주 사용된다.

---

# 16. Logistic Regression 기본 코드

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

pred = model.predict(
    X_test
)
```

---

# 17. Logistic Regression은 무엇을 학습할까?

각 Feature에 대한 Weight를 학습한다.

개념적으로:

```text
age       × weight
BMI       × weight
glucose   × weight
        +
intercept
```

를 계산한다.

---

# 18. 선형 결합

개념적으로:

```text
z =
w1 × age
+
w2 × BMI
+
w3 × glucose
+
b
```

형태를 생각할 수 있다.

여기서:

```text
w
→ coefficient / weight

b
→ intercept
```

이다.

---

# 19. 그런데 분류는 0/1인데?

선형 결합 결과 `z`를 그대로 Class로 사용하는 것이 아니라 Logistic Regression에서는 Logistic Function을 이용하여 확률 형태의 값을 만든다.

대표적으로:

```text
Sigmoid
```

함수를 사용한다.

---

# 20. Sigmoid

Sigmoid 함수는:

```text
어떤 실수 값
↓
0과 1 사이 값
```

으로 변환한다.

수식:

```text
            1
σ(z) = ───────────
        1 + e^(-z)
```

---

# 21. 확률처럼 해석

예:

```text
Model Output
→ 0.82
```

라면 설정과 Class 정의에 따라:

```text
Class 1일 추정 확률 = 82%
```

처럼 해석할 수 있다.

---

# 22. predict()

```python
pred = model.predict(
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

처럼 최종 Class를 반환한다.

---

# 23. predict_proba()

확률을 보고 싶다면:

```python
prob = model.predict_proba(
    X_test
)
```

를 사용할 수 있다.

---

# 24. predict_proba 결과

이진 분류라면 예를 들어:

```text
[[0.90, 0.10],
 [0.20, 0.80],
 [0.45, 0.55]]
```

같은 결과가 나올 수 있다.

각 행은 한 Sample이다.

---

# 25. 두 Column의 의미

모델의 `classes_`가:

```python
print(
    model.classes_
)
```

결과:

```text
[0 1]
```

이라면:

```text
첫 번째 Column
→ Class 0 확률

두 번째 Column
→ Class 1 확률
```

이다.

---

# 26. Class 1 확률만 가져오기

```python
prob_1 = model.predict_proba(
    X_test
)[:, 1]
```

여기서:

```python
[:, 1]
```

은:

```text
모든 Row
+
두 번째 Column
```

을 선택한다.

---

# 27. 결과

```text
0.10
0.80
0.55
```

처럼 Class 1에 대한 확률을 얻는다.

---

# 28. Probability와 Prediction 차이

Probability:

```text
0.82
```

Prediction:

```text
1
```

이다.

즉:

```text
확률
↓
Threshold
↓
Class
```

과정을 거친다.

---

# 29. Threshold

확률을 Class로 바꾸는 기준을:

```text
Threshold
```

라고 한다.

예:

```text
Threshold = 0.5
```

이면:

```text
P(Class 1) >= 0.5
→ 1

P(Class 1) < 0.5
→ 0
```

처럼 결정할 수 있다.

---

# 30. 직접 Threshold 적용

```python
prob_1 = model.predict_proba(
    X_test
)[:, 1]

pred = (
    prob_1 >= 0.5
).astype(int)
```

---

# 31. Threshold 0.3

```python
pred = (
    prob_1 >= 0.3
).astype(int)
```

Threshold를 낮추면 더 많은 Sample을:

```text
Class 1
```

로 예측하게 되는 경향이 있다.

---

# 32. Threshold 0.7

```python
pred = (
    prob_1 >= 0.7
).astype(int)
```

Threshold를 높이면 Class 1로 판단하기 위한 조건이 더 엄격해진다.

---

# 33. Threshold가 중요한 이유

분류 문제마다:

```text
False Positive

False Negative
```

의 비용이 다를 수 있다.

예를 들어 의료 선별 문제에서는 실제 고위험 환자를 놓치는 False Negative가 특히 중요할 수 있다.

하지만 False Positive가 지나치게 많아지는 것도 문제가 될 수 있다.

따라서 목적에 따라 Threshold를 검토한다.

---

# 34. Threshold는 Test로 선택하면 안 된다

Threshold 역시 모델 설정의 일부처럼 생각해야 한다.

따라서:

```text
Validation
→ Threshold 선택

Test
→ 최종 평가
```

가 기본적인 흐름이다.

Test 결과를 계속 보면서 Threshold를 조정하면 Test에 맞추게 될 수 있다.

---

# 35. Decision Tree

Decision Tree는 질문을 반복하면서 데이터를 나누는 모델이다.

한국어:

```text
의사결정나무
```

이다.

---

# 36. Decision Tree 개념

예:

```text
glucose >= 140?
       │
   ┌───┴───┐
  Yes      No
   │        │
BMI >= 30? 정상
   │
 ┌─┴─┐
Yes  No
 │    │
질병  정상
```

실제 Tree는 데이터를 기반으로 분할 기준을 학습한다.

---

# 37. DecisionTreeClassifier

```python
from sklearn.tree import (
    DecisionTreeClassifier
)

model = DecisionTreeClassifier(
    random_state=42
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

# 38. Tree의 장점

Decision Tree는:

```text
구조를 비교적 이해하기 쉬움

비선형 관계 학습 가능

Feature Scaling에 상대적으로 덜 민감
```

등의 특징이 있다.

---

# 39. Tree의 단점

Tree를 지나치게 깊게 만들면 Train Data를 매우 세세하게 나눌 수 있다.

그러면:

```text
Overfitting
```

이 발생할 수 있다.

---

# 40. max_depth

Tree의 최대 깊이를 제한할 수 있다.

```python
model = DecisionTreeClassifier(
    max_depth=5,
    random_state=42
)
```

---

# 41. 너무 깊은 Tree

```text
Train Accuracy
→ 매우 높음

Validation Accuracy
→ 낮아짐
```

이라면 과적합 가능성을 확인한다.

---

# 42. 너무 얕은 Tree

반대로 Tree가 너무 단순하면 중요한 패턴을 충분히 학습하지 못할 수 있다.

```text
Underfitting
```

가능성이 있다.

---

# 43. Random Forest

Random Forest는 여러 Decision Tree를 결합하는 Ensemble 모델이다.

```text
Random
+
Forest
```

즉:

```text
여러 Tree의 숲
```

이라고 생각할 수 있다.

---

# 44. Random Forest 구조

```text
Tree 1 ─┐
Tree 2 ─┤
Tree 3 ─┤
Tree 4 ─┼→ 결과 종합 → Prediction
Tree 5 ─┤
...     │
Tree N ─┘
```

---

# 45. 왜 Tree를 여러 개 사용할까?

Decision Tree 하나는 특정 Training Data에 과도하게 맞을 수 있다.

여러 Tree를 서로 조금씩 다르게 학습시키고 결과를 종합하여 더 안정적인 예측을 얻는 것이 Random Forest의 핵심 아이디어이다.

---

# 46. RandomForestClassifier

```python
from sklearn.ensemble import (
    RandomForestClassifier
)

model = RandomForestClassifier(
    n_estimators=100,
    random_state=42
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

# 47. n_estimators

```python
n_estimators=100
```

은 기본적으로 Forest에 사용할 Tree 개수와 관련된 Hyperparameter이다.

즉:

```text
100개의 Decision Tree
```

를 사용하는 설정이라고 이해할 수 있다.

---

# 48. Random Forest 주요 Hyperparameter

대표적으로:

```text
n_estimators

max_depth

min_samples_split

min_samples_leaf

max_features

class_weight
```

등이 있다.

---

# 49. Random Forest와 Scaling

Random Forest는 Tree 기반 모델이기 때문에 일반적으로 StandardScaler 같은 Feature Scaling의 필요성이 선형 모델보다 낮다.

예:

```text
Logistic Regression
→ Scaling 고려

Random Forest
→ Scaling 영향 상대적으로 적음
```

이다.

---

# 50. Random Forest Feature Importance

학습 후:

```python
model.feature_importances_
```

를 통해 Feature Importance를 확인할 수 있다.

예:

```python
importance = model.feature_importances_

print(
    importance
)
```

---

# 51. Feature 이름과 연결

```python
import pandas as pd

importance_df = pd.DataFrame(
    {
        "feature": X_train.columns,
        "importance": model.feature_importances_
    }
)

importance_df = importance_df.sort_values(
    "importance",
    ascending=False
)

print(
    importance_df
)
```

---

# 52. Feature Importance 해석 주의

중요도가 높다고 해서:

```text
이 Feature가 질병의 원인이다.
```

라고 말할 수는 없다.

정확한 의미는:

```text
이 모델이 예측할 때
이 Feature를 상대적으로 중요하게 사용했다.
```

에 가깝다.

---

# 53. Gradient Boosting

여러 Tree를 순차적으로 만들어 이전 모델의 부족한 부분을 보완하는 Boosting 계열 모델도 있다.

대표적으로:

```text
Gradient Boosting

XGBoost

LightGBM

CatBoost
```

등이 있다.

---

# 54. Random Forest와 Boosting의 큰 차이

아주 단순화하면:

```text
Random Forest
→ 여러 Tree를 만들어 결과를 종합

Boosting
→ 앞 모델의 부족한 부분을 다음 모델이 보완
```

하는 방식이라고 이해할 수 있다.

세부 알고리즘은 더 복잡하다.

---

# 55. XGBoost

XGBoost는 대표적인 Gradient Boosting 계열 Library/Algorithm이다.

Tabular Data 분류에서 자주 사용된다.

예:

```python
from xgboost import (
    XGBClassifier
)
```

---

# 56. XGBoost 예

```python
model = XGBClassifier(
    n_estimators=300,
    learning_rate=0.05,
    max_depth=5,
    random_state=42
)

model.fit(
    X_train,
    y_train
)
```

---

# 57. LightGBM

LightGBM 역시 Gradient Boosting 기반의 강력한 Tree 모델 Library이다.

예:

```python
from lightgbm import (
    LGBMClassifier
)
```

---

# 58. LightGBM 예

```python
model = LGBMClassifier(
    n_estimators=300,
    learning_rate=0.05,
    random_state=42
)

model.fit(
    X_train,
    y_train
)
```

---

# 59. CatBoost

CatBoost 역시 Boosting 계열 모델이다.

범주형 Feature를 다루는 기능으로도 많이 알려져 있다.

```python
from catboost import (
    CatBoostClassifier
)
```

---

# 60. CatBoost 예

```python
model = CatBoostClassifier(
    iterations=300,
    learning_rate=0.05,
    random_seed=42,
    verbose=0
)

model.fit(
    X_train,
    y_train
)
```

---

# 61. 어떤 분류 모델이 가장 좋은가?

항상 하나의 모델이 최고인 것은 아니다.

```text
데이터 크기

Feature 종류

Class 불균형

Noise

결측치

범주형 Feature

평가 Metric
```

등에 따라 결과가 달라진다.

따라서 Validation 또는 Cross Validation을 이용하여 비교한다.

---

# 62. Baseline부터 시작

예:

```text
1. Logistic Regression
       ↓
2. Decision Tree
       ↓
3. Random Forest
       ↓
4. XGBoost
       ↓
5. LightGBM
       ↓
6. CatBoost
```

처럼 기본 모델에서 시작하여 비교할 수 있다.

---

# 63. 왜 Logistic Regression을 Baseline으로 사용할까?

Logistic Regression은:

```text
비교적 단순함

학습이 빠른 편

Coefficient 확인 가능

기준 성능을 만들기 쉬움
```

등의 장점이 있다.

따라서 복잡한 모델이 실제로 개선되는지 비교하기 좋다.

---

# 64. Accuracy

분류 모델의 가장 기본적인 평가 Metric 중 하나이다.

```text
맞게 예측한 Sample 수
────────────────────
전체 Sample 수
```

이다.

Scikit-learn:

```python
from sklearn.metrics import (
    accuracy_score
)

accuracy = accuracy_score(
    y_test,
    pred
)
```

---

# 65. Accuracy 문제

다음 데이터가 있다고 하자.

```text
0 = 950명

1 = 50명
```

모델이 전부:

```text
0
```

이라고 예측하면:

```text
Accuracy = 95%
```

가 될 수 있다.

하지만 Class 1은 한 명도 찾지 못했다.

---

# 66. Confusion Matrix

분류 모델이 어떤 방식으로 맞고 틀렸는지 확인하기 위해:

```text
Confusion Matrix
```

를 사용한다.

한국어:

```text
혼동행렬
```

이다.

---

# 67. Binary Confusion Matrix

```text
                 Predicted
                 0       1

Actual 0         TN      FP

Actual 1         FN      TP
```

이다.

---

# 68. TN

```text
True Negative
```

실제:

```text
0
```

예측:

```text
0
```

즉 Negative를 올바르게 예측했다.

---

# 69. FP

```text
False Positive
```

실제:

```text
0
```

인데 예측:

```text
1
```

이다.

---

# 70. FN

```text
False Negative
```

실제:

```text
1
```

인데 예측:

```text
0
```

이다.

---

# 71. TP

```text
True Positive
```

실제:

```text
1
```

예측:

```text
1
```

이다.

---

# 72. 질병 예측으로 생각

예를 들어:

```text
0 = 정상
1 = 질병
```

이라면:

```text
TN
→ 정상 환자를 정상으로 예측

FP
→ 정상 환자를 질병으로 예측

FN
→ 질병 환자를 정상으로 예측

TP
→ 질병 환자를 질병으로 예측
```

이다.

---

# 73. Confusion Matrix 코드

```python
from sklearn.metrics import (
    confusion_matrix
)

cm = confusion_matrix(
    y_test,
    pred
)

print(
    cm
)
```

---

# 74. 결과 예

```text
[[90 10]
 [ 5 45]]
```

Scikit-learn의 일반적인 이진 분류 Label 순서가 `[0, 1]`이라면:

```text
TN = 90

FP = 10

FN = 5

TP = 45
```

로 읽을 수 있다.

Label 순서는 `labels` 설정 등에 따라 달라질 수 있으므로 실제 Class 순서를 확인한다.

---

# 75. Classification Report

여러 분류 Metric을 한 번에 확인할 수 있다.

```python
from sklearn.metrics import (
    classification_report
)

print(
    classification_report(
        y_test,
        pred
    )
)
```

---

# 76. 출력되는 대표 Metric

```text
precision

recall

f1-score

support
```

등을 확인할 수 있다.

각 Metric의 수식과 의미는 `evaluation-metrics.md`에서 자세히 다룬다.

---

# 77. Precision

간단히:

```text
1이라고 예측한 것 중
실제로 1인 비율
```

이다.

---

# 78. Recall

간단히:

```text
실제 1인 데이터 중
모델이 1이라고 찾아낸 비율
```

이다.

---

# 79. F1 Score

Precision과 Recall을 함께 고려하는 Metric이다.

Class Imbalance가 있는 분류 문제에서 자주 확인한다.

---

# 80. ROC-AUC

Threshold 하나만 보지 않고 여러 Threshold에서 모델의 분류 능력을 평가하는 방법 중 하나이다.

확률 예측:

```python
prob_1 = model.predict_proba(
    X_test
)[:, 1]
```

을 사용할 수 있다.

---

# 81. ROC-AUC 코드

```python
from sklearn.metrics import (
    roc_auc_score
)

auc = roc_auc_score(
    y_test,
    prob_1
)

print(
    auc
)
```

---

# 82. predict와 predict_proba를 구분

```python
model.predict(
    X_test
)
```

→ Class

```text
0
1
1
0
```

반면:

```python
model.predict_proba(
    X_test
)
```

→ Class Probability

```text
0.12
0.91
0.67
0.08
```

등이다.

---

# 83. Metric마다 필요한 값이 다르다

예:

Accuracy:

```python
accuracy_score(
    y_test,
    pred
)
```

Class 예측값을 사용한다.

ROC-AUC:

```python
roc_auc_score(
    y_test,
    prob_1
)
```

에서는 일반적으로 Positive Class에 대한 Score 또는 Probability를 사용한다.

---

# 84. 분류 전체 기본 코드

```python
from sklearn.model_selection import (
    train_test_split
)

from sklearn.preprocessing import (
    StandardScaler
)

from sklearn.linear_model import (
    LogisticRegression
)

from sklearn.metrics import (
    accuracy_score,
    confusion_matrix,
    classification_report,
    roc_auc_score
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
        random_state=42,
        stratify=y
    )
)


scaler = StandardScaler()

X_train_scaled = scaler.fit_transform(
    X_train
)

X_test_scaled = scaler.transform(
    X_test
)


model = LogisticRegression(
    max_iter=1000
)

model.fit(
    X_train_scaled,
    y_train
)


pred = model.predict(
    X_test_scaled
)

prob_1 = model.predict_proba(
    X_test_scaled
)[:, 1]


print(
    "Accuracy:",
    accuracy_score(
        y_test,
        pred
    )
)


print(
    "ROC-AUC:",
    roc_auc_score(
        y_test,
        prob_1
    )
)


print(
    confusion_matrix(
        y_test,
        pred
    )
)


print(
    classification_report(
        y_test,
        pred
    )
)
```

---

# 85. 이 코드의 흐름

```text
Data
 ↓
X / y
 ↓
Train / Test
 ↓
Scaling
 ↓
Logistic Regression
 ↓
fit()
 ↓
predict()
 ↓
predict_proba()
 ↓
Evaluation
```

이다.

---

# 86. Pipeline으로 만들기

이전 파일에서 배운 Pipeline을 사용하면:

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
            "classifier",
            LogisticRegression(
                max_iter=1000
            )
        )
    ]
)
```

처럼 만들 수 있다.

---

# 87. Pipeline 학습

```python
model.fit(
    X_train,
    y_train
)
```

하면:

```text
Scaler fit
↓
Scaler transform
↓
Logistic Regression fit
```

이 연결된다.

---

# 88. Pipeline 예측

```python
pred = model.predict(
    X_test
)

prob_1 = model.predict_proba(
    X_test
)[:, 1]
```

Pipeline이 Test Data에 필요한 전처리를 적용한 후 모델 예측까지 수행한다.

---

# 89. Multi-class에서는?

Target:

```text
0
1
2
```

처럼 Class가 세 개라면:

```python
model.predict_proba(
    X_test
)
```

결과가 예를 들어:

```text
Class 0   Class 1   Class 2

0.10      0.20      0.70
0.80      0.10      0.10
0.25      0.50      0.25
```

처럼 각 Class에 대한 확률을 포함할 수 있다.

---

# 90. 첫 번째 Sample

```text
Class 0 = 0.10
Class 1 = 0.20
Class 2 = 0.70
```

이라면 가장 높은 확률의:

```text
Class 2
```

를 예측할 수 있다.

---

# 91. model.classes_

확률 Column 순서를 추측하지 말고:

```python
print(
    model.classes_
)
```

로 확인할 수 있다.

Pipeline이라면 마지막 estimator 접근 방식이 다를 수 있다.

예:

```python
print(
    model.named_steps[
        "classifier"
    ].classes_
)
```

---

# 92. Class Imbalance

분류에서는 Class 비율을 반드시 확인한다.

```python
print(
    y.value_counts()
)

print(
    y.value_counts(
        normalize=True
    )
)
```

---

# 93. 예

```text
0    950
1     50
```

비율:

```text
0 = 95%

1 = 5%
```

이라면 심한 Class Imbalance가 존재한다.

---

# 94. Class Imbalance 대응

대표적으로:

```text
class_weight

Over Sampling

Under Sampling

SMOTE

KMeansSMOTE

Threshold 조정

Metric 변경
```

등을 고려할 수 있다.

자세한 내용은 `imbalanced-data.md`에서 다룬다.

---

# 95. class_weight

일부 Scikit-learn 모델에서는:

```python
class_weight="balanced"
```

를 사용할 수 있다.

예:

```python
model = LogisticRegression(
    class_weight="balanced",
    max_iter=1000
)
```

또는:

```python
model = RandomForestClassifier(
    class_weight="balanced",
    random_state=42
)
```

---

# 96. balanced 의미

소수 Class의 오류에 상대적으로 더 큰 Weight를 부여하도록 자동 계산하는 방식이다.

하지만:

```text
class_weight="balanced"
→ 무조건 성능 향상
```

은 아니다.

Validation을 통해 비교해야 한다.

---

# 97. 모델 비교

예:

```python
models = {
    "Logistic": LogisticRegression(
        max_iter=1000
    ),

    "RandomForest": RandomForestClassifier(
        n_estimators=200,
        random_state=42
    )
}
```

---

# 98. 반복 학습

```python
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

# 99. 비교 조건을 같게 한다

공정하게 비교하려면 가능하면:

```text
같은 Train

같은 Validation

같은 Feature

같은 Metric
```

을 사용한다.

그래야:

```text
모델 차이
```

를 비교하기 쉽다.

---

# 100. Accuracy가 가장 높은 모델을 무조건 선택?

아니다.

문제 목적에 따라:

```text
Recall

Precision

F1

ROC-AUC

PR-AUC
```

가 더 중요할 수 있다.

예를 들어 실제 Positive를 놓치는 것이 매우 중요한 문제라면 Accuracy만으로 모델을 선택하면 안 될 수 있다.

---

# 101. Threshold도 모델 평가의 일부

같은 모델이어도:

```text
Threshold = 0.5

Threshold = 0.3

Threshold = 0.7
```

에 따라:

```text
Precision

Recall

F1

Confusion Matrix
```

가 달라진다.

---

# 102. Threshold 비교 예

```python
prob_1 = model.predict_proba(
    X_valid
)[:, 1]

for threshold in [
    0.3,
    0.5,
    0.7
]:

    pred = (
        prob_1 >= threshold
    ).astype(int)

    print(
        threshold,
        confusion_matrix(
            y_valid,
            pred
        )
    )
```

---

# 103. Threshold 선택도 Validation에서

```text
Train
→ 모델 학습

Validation
→ Threshold 선택

Test
→ 최종 성능 확인
```

한다.

---

# 104. 의료 분류에서 생각할 것

예를 들어:

```text
0 = 정상
1 = 고위험
```

이라면 단순 Accuracy뿐 아니라:

```text
실제 고위험 환자를 얼마나 찾았는가?

정상 환자를 고위험으로 얼마나 잘못 분류했는가?

어떤 Threshold를 사용할 것인가?
```

등을 함께 고려한다.

---

# 105. 분류 모델은 진단 자체가 아니다

의료 AI 모델이:

```text
Class 1 Probability = 0.87
```

을 출력했다고 해서 그 자체로 환자의 질환이 확정되는 것은 아니다.

모델은 학습 데이터와 정의된 Target에 기반하여 예측 결과를 제공하는 것이다.

---

# 106. 오류 분석

성능 숫자만 보고 끝내지 않는다.

잘못 예측한 Sample을 확인한다.

```python
result = X_test.copy()

result[
    "actual"
] = y_test

result[
    "pred"
] = pred
```

---

# 107. 오분류만 추출

```python
errors = result[
    result["actual"]
    !=
    result["pred"]
]
```

이제:

```text
어떤 데이터에서
모델이 자주 틀리는가?
```

를 분석할 수 있다.

---

# 108. False Positive

```python
false_positive = result[
    (
        result["actual"] == 0
    )
    &
    (
        result["pred"] == 1
    )
]
```

---

# 109. False Negative

```python
false_negative = result[
    (
        result["actual"] == 1
    )
    &
    (
        result["pred"] == 0
    )
]
```

---

# 110. 왜 Failure Case를 보는가?

예:

```text
특정 연령대에서 많이 틀리는가?

특정 범주에서 많이 틀리는가?

결측치가 많은 Sample인가?

경계 확률의 Sample인가?

Label 오류 가능성이 있는가?
```

등을 확인할 수 있다.

이것이 다음 실험의 아이디어가 될 수 있다.

---

# 111. Confidence

이진 분류에서 Class 1 확률이:

```text
0.99
```

라면 Class 1 쪽으로 매우 강한 출력이다.

반대로:

```text
0.51
```

이라면 Threshold 0.5 기준으로 Class 1이지만 결정 경계에 가깝다.

---

# 112. 확률이 높다고 항상 맞는 것은 아니다

예:

```text
True = 0

Pred = 1

P(Class 1) = 0.99
```

처럼 모델이 매우 자신 있게 틀릴 수도 있다.

따라서 Confidence와 Correctness는 같은 개념이 아니다.

---

# 113. Calibration

모델이 출력한 확률이 실제 발생 비율과 얼마나 잘 대응하는지를:

```text
Calibration
```

관점에서 분석할 수도 있다.

예를 들어 모델이:

```text
0.8
```

이라고 예측한 Sample들이 실제로도 약 80% 정도 Positive인지 확인하는 식이다.

이는 단순 Accuracy와 다른 문제이다.

---

# 114. 분류 프로젝트 기본 흐름

```text
1. 문제 정의

2. Target 확인

3. Class 분포 확인

4. EDA

5. Train / Validation / Test

6. 전처리

7. Baseline Model

8. fit()

9. predict()

10. predict_proba()

11. Metric 평가

12. Confusion Matrix

13. Threshold 검토

14. Failure Case 분석

15. 모델 개선
```

---

# 115. 모델 성능이 낮으면?

무조건 더 복잡한 모델부터 사용하는 것이 아니다.

먼저:

```text
데이터가 올바른가?

Target 정의가 맞는가?

Feature가 충분한가?

Leakage는 없는가?

Class Imbalance가 심한가?

Split 방식이 맞는가?

전처리가 맞는가?

Metric이 적절한가?
```

를 확인한다.

---

# 116. 모델 성능이 너무 높아도 확인

예:

```text
Accuracy = 0.999
```

라고 해서 무조건 좋아하면 안 된다.

다음을 확인한다.

```text
Target Leakage?

중복 데이터?

같은 환자가 Train/Validation에 동시에 존재?

전처리 Leakage?

Label과 거의 동일한 Feature?

잘못된 Split?
```

이다.

---

# 117. 핵심 용어

```text
Classification
→ Class를 예측하는 문제

Binary Classification
→ 두 개 Class 분류

Multi-class Classification
→ 세 개 이상 Class 분류

Label
→ 실제 Class 값

Logistic Regression
→ 대표적인 분류 모델

Decision Tree
→ 조건에 따라 데이터를 분할하는 Tree 모델

Random Forest
→ 여러 Decision Tree를 결합한 Ensemble

Boosting
→ 앞 모델의 부족한 부분을 순차적으로 보완하는 방식

predict()
→ 최종 Class 예측

predict_proba()
→ 각 Class에 대한 확률 예측

Threshold
→ 확률을 Class로 변환하는 기준

Confusion Matrix
→ 실제 Class와 예측 Class의 조합을 나타내는 표

TP
→ 실제 Positive, 예측 Positive

TN
→ 실제 Negative, 예측 Negative

FP
→ 실제 Negative, 예측 Positive

FN
→ 실제 Positive, 예측 Negative
```

---

# 118. 가장 중요한 구조

```text
X_train + y_train
        ↓
      fit()
        ↓
  Classification Model
        ↓
      X_test
        ↓
  predict_proba()
        ↓
    Probability
        ↓
    Threshold
        ↓
    Prediction
        ↓
 y_test와 비교
        ↓
    Evaluation
```

---

# 119. Logistic Regression 구조

```text
Feature
   ↓
Linear Combination
   ↓
Sigmoid
   ↓
Probability
   ↓
Threshold
   ↓
Class
```

---

# 120. Tree 계열 구조

```text
Feature
   ↓
조건에 따라 데이터 분할
   ↓
Decision Tree
```

여러 Tree를 결합하면:

```text
Decision Trees
      ↓
Random Forest
      ↓
Prediction
```

순차적으로 보완하면:

```text
Boosting
      ↓
XGBoost / LightGBM / CatBoost
```

등으로 연결된다.

---

# 121. 배운 점

Classification은:

```text
입력 Feature를 이용하여
미리 정의된 Class를 예측하는 문제
```

이다.

Class가 두 개이면:

```text
Binary Classification
```

Class가 세 개 이상이면:

```text
Multi-class Classification
```

이다.

모델은:

```python
model.fit(
    X_train,
    y_train
)
```

으로 학습하고:

```python
model.predict(
    X_test
)
```

로 Class를 예측한다.

확률이 필요한 경우:

```python
model.predict_proba(
    X_test
)
```

를 사용할 수 있다.

이진 분류에서는:

```text
Probability
↓
Threshold
↓
0 또는 1
```

로 최종 Class가 결정될 수 있다.

그리고 분류 모델은 단순히 Accuracy만 확인해서는 안 된다.

```text
Confusion Matrix

Precision

Recall

F1

ROC-AUC

PR-AUC
```

등을 문제 목적에 맞게 확인해야 한다.

또한:

```text
Threshold

Class Imbalance

Data Leakage

Overfitting

Failure Case
```

도 함께 분석해야 한다.

가장 중요한 것은:

```text
점수가 높은 모델
```

을 무조건 찾는 것이 아니라:

```text
우리 문제에서 어떤 오류가 중요한지 정의하고

그 목적에 맞는 평가 기준으로

새로운 데이터에서도 잘 작동하는 모델을 선택하는 것
```

이다.