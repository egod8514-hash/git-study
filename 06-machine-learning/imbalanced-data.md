# Imbalanced Data 클래스 불균형

## 1. Imbalanced Data란?

Classification 데이터에서 Class별 Sample 수가 크게 차이나는 경우를:

```text
Imbalanced Data
```

또는:

```text
Class Imbalance
클래스 불균형
```

이라고 한다.

예:

```text
Class 0 = 950
Class 1 = 50
```

전체:

```text
1000개
```

비율:

```text
Class 0 = 95%
Class 1 = 5%
```

이다.

---

# 2. Balanced Data

반대로:

```text
Class 0 = 500
Class 1 = 500
```

처럼 비율이 비슷하면 상대적으로 Balanced한 데이터라고 할 수 있다.

하지만 반드시 정확히 50:50이어야 Balanced라고 부르는 것은 아니다.

---

# 3. 왜 Class Imbalance가 문제인가?

예:

```text
정상 = 950
질병 = 50
```

이라고 하자.

모델이 모든 사람을:

```text
정상
```

이라고 예측해도:

```text
950 / 1000
=
0.95
```

즉:

```text
Accuracy = 95%
```

가 된다.

---

# 4. 그런데 이 모델은 좋은가?

아니다.

질병 환자 50명을:

```text
0명 발견
```

했다.

즉:

```text
질병 Recall = 0%
```

이다.

Accuracy만 보면 좋은 모델처럼 보이지만 실제 목적에는 매우 나쁠 수 있다.

---

# 5. 의료 데이터 예

예:

```text
정상 환자 = 990

위험 환자 = 10
```

모델:

```text
모든 환자 → 정상
```

결과:

```text
Accuracy = 99%
```

하지만:

```text
위험 환자 발견 = 0명
```

이다.

따라서 의료 AI에서는 Accuracy 하나만으로 모델을 평가하면 위험할 수 있다.

---

# 6. 먼저 Class 분포 확인

Pandas:

```python
print(
    y.value_counts()
)
```

예:

```text
0    950
1     50
```

---

# 7. 비율 확인

```python
print(
    y.value_counts(
        normalize=True
    )
)
```

예:

```text
0    0.95
1    0.05
```

---

# 8. 퍼센트로 확인

```python
ratio = (
    y.value_counts(
        normalize=True
    )
    * 100
)

print(
    ratio
)
```

결과:

```text
0    95.0
1     5.0
```

---

# 9. 시각화

```python
import matplotlib.pyplot as plt


y.value_counts().plot(
    kind="bar"
)

plt.xlabel(
    "Class"
)

plt.ylabel(
    "Count"
)

plt.title(
    "Class Distribution"
)

plt.show()
```

---

# 10. Majority Class

Sample이 많은 Class를:

```text
Majority Class
```

라고 한다.

예:

```text
Class 0 = 950
```

이면:

```text
Class 0
→ Majority Class
```

이다.

---

# 11. Minority Class

Sample이 적은 Class:

```text
Minority Class
```

라고 한다.

예:

```text
Class 1 = 50
```

이면:

```text
Class 1
→ Minority Class
```

이다.

---

# 12. Positive Class

Binary Classification에서는 관심 대상 Class를:

```text
Positive Class
```

로 정의하는 경우가 많다.

예:

```text
0 = 정상
1 = 질병
```

이라면 보통:

```text
1
→ Positive Class
```

로 다룬다.

하지만 `1`이 무조건 중요한 Class라는 법칙은 없다.

문제 정의를 확인해야 한다.

---

# 13. Confusion Matrix

불균형 데이터를 이해하려면 Confusion Matrix가 중요하다.

```text
                Prediction
               0         1

Actual 0      TN        FP

Actual 1      FN        TP
```

---

# 14. TP

```text
True Positive
```

실제 Positive이고 모델도 Positive라고 예측했다.

예:

```text
실제 질병
+
질병이라고 예측
```

---

# 15. TN

```text
True Negative
```

실제 Negative이고 모델도 Negative라고 예측했다.

---

# 16. FP

```text
False Positive
```

실제 Negative인데 Positive라고 잘못 예측했다.

예:

```text
실제 정상
+
질병이라고 예측
```

---

# 17. FN

```text
False Negative
```

실제 Positive인데 Negative라고 잘못 예측했다.

예:

```text
실제 질병
+
정상이라고 예측
```

---

# 18. 의료 문제에서 FN

질병 환자를 정상이라고 놓치는 상황:

```text
False Negative
```

이다.

문제에 따라 이런 오류의 비용이 매우 클 수 있다.

그래서 Recall이 중요한 경우가 있다.

---

# 19. Accuracy

```text
Accuracy
=
전체 중 맞게 예측한 비율
```

수식:

```text
Accuracy
=
(TP + TN)
/
(TP + TN + FP + FN)
```

---

# 20. Accuracy의 문제

Class가 매우 불균형하면 Majority Class를 주로 예측하는 것만으로 높은 Accuracy가 나올 수 있다.

그래서:

```text
Accuracy가 높다
=
좋은 모델
```

이라고 무조건 말할 수 없다.

---

# 21. Precision

```text
Precision
```

은:

```text
모델이 Positive라고 예측한 것 중
실제로 Positive인 비율
```

이다.

수식:

```text
Precision
=
TP / (TP + FP)
```

---

# 22. Recall

```text
Recall
```

은:

```text
실제 Positive 중
모델이 찾아낸 비율
```

이다.

수식:

```text
Recall
=
TP / (TP + FN)
```

---

# 23. F1 Score

Precision과 Recall을 함께 고려하는 대표적인 Metric이다.

```text
F1
=
2 ×
(Precision × Recall)
/
(Precision + Recall)
```

---

# 24. 왜 F1을 볼까?

Precision만 높고 Recall이 매우 낮거나 그 반대인 경우를 함께 고려하기 위해 사용할 수 있다.

하지만 모든 문제에서 F1이 최고의 Metric이라는 뜻은 아니다.

---

# 25. ROC-AUC

ROC-AUC는 여러 Threshold에서:

```text
TPR

FPR
```

의 관계를 이용하여 모델의 순위 구분 능력을 평가한다.

Threshold 하나에만 의존하지 않는다는 장점이 있다.

---

# 26. PR-AUC

Class Imbalance가 심한 문제에서는:

```text
Precision-Recall Curve
```

와:

```text
PR-AUC
```

도 중요하게 볼 수 있다.

Positive Class가 매우 희귀할 때 ROC-AUC와 함께 확인하는 것이 유용하다.

---

# 27. Metric을 목적에 맞게 선택

예:

```text
질병 환자를 최대한 놓치지 않는 것이 중요
→ Recall 중요

False Alarm을 줄이는 것이 중요
→ Precision 중요

둘의 균형
→ F1 고려

Probability Ranking 성능
→ ROC-AUC / PR-AUC
```

이다.

---

# 28. 해결 방법

Class Imbalance를 다루는 대표적인 방법:

```text
1. 적절한 Metric 사용

2. Stratified Split

3. Class Weight

4. Undersampling

5. Oversampling

6. SMOTE

7. KMeansSMOTE

8. Threshold 조정

9. Ensemble / Specialized Model

10. 더 많은 Minority Data 확보
```

이다.

---

# 29. Stratified Split

Train/Test를 나눌 때:

```python
from sklearn.model_selection import (
    train_test_split
)


X_train, X_test, y_train, y_test = (
    train_test_split(
        X,
        y,
        test_size=0.2,
        random_state=42,
        stratify=y
    )
)
```

처럼:

```python
stratify=y
```

를 사용할 수 있다.

---

# 30. 왜 stratify?

전체:

```text
0 = 95%
1 = 5%
```

라면 Train과 Test도 가능하면 비슷한 Class 비율을 유지하도록 한다.

---

# 31. Cross Validation에서는?

Classification:

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

# 32. Class Weight

Sampling으로 데이터를 바꾸지 않고 Loss 계산에서 Class별 중요도를 다르게 주는 방법이다.

예:

```text
Class 0
→ Weight 1

Class 1
→ Weight 10
```

처럼 Minority Class 오류에 더 큰 Weight를 줄 수 있다.

---

# 33. Logistic Regression

```python
from sklearn.linear_model import (
    LogisticRegression
)


model = LogisticRegression(
    class_weight="balanced",
    max_iter=1000,
    random_state=42
)
```

---

# 34. class_weight="balanced"

Scikit-learn이 Class 빈도를 고려하여 Weight를 자동으로 계산한다.

대략적으로:

```text
적은 Class
→ 더 큰 Weight

많은 Class
→ 더 작은 Weight
```

를 주는 방향이다.

---

# 35. Random Forest

```python
from sklearn.ensemble import (
    RandomForestClassifier
)


model = RandomForestClassifier(
    n_estimators=300,
    class_weight="balanced",
    random_state=42
)
```

---

# 36. 직접 Weight 지정

```python
model = LogisticRegression(
    class_weight={
        0: 1,
        1: 5
    },
    max_iter=1000
)
```

처럼 직접 설정할 수도 있다.

---

# 37. Weight를 크게 하면 무조건 좋은가?

아니다.

Minority Weight를 지나치게 높이면:

```text
Recall ↑

하지만

False Positive ↑

Precision ↓
```

같은 Trade-off가 발생할 수 있다.

따라서 Validation 결과로 비교한다.

---

# 38. Sampling

Class Imbalance를 해결하기 위해 Training Data의 Class 구성을 바꾸는 방법이다.

대표적으로:

```text
Undersampling

Oversampling
```

이 있다.

---

# 39. Undersampling

Majority Class Sample 일부를 제거한다.

원래:

```text
Class 0 = 1000
Class 1 = 100
```

Undersampling:

```text
Class 0 = 100
Class 1 = 100
```

처럼 만들 수 있다.

---

# 40. Undersampling 장점

```text
Training Data 감소

학습 속도 향상

Class Balance 개선
```

등이 가능하다.

---

# 41. Undersampling 단점

Majority Class의 실제 정보를 버릴 수 있다.

예:

```text
1000개 중 900개 삭제
```

하면 많은 정보가 사라진다.

---

# 42. RandomUnderSampler

설치:

```powershell
pip install imbalanced-learn
```

Import:

```python
from imblearn.under_sampling import (
    RandomUnderSampler
)
```

---

# 43. 사용

```python
under = RandomUnderSampler(
    random_state=42
)


X_train_under, y_train_under = (
    under.fit_resample(
        X_train,
        y_train
    )
)
```

---

# 44. 결과 확인

```python
print(
    y_train.value_counts()
)

print(
    y_train_under.value_counts()
)
```

---

# 45. Oversampling

Minority Class Sample 수를 늘리는 방법이다.

원래:

```text
Class 0 = 1000
Class 1 = 100
```

Oversampling 후:

```text
Class 0 = 1000
Class 1 = 1000
```

처럼 만들 수 있다.

---

# 46. Random Oversampling

가장 단순한 방법은 Minority Sample을 복제하는 것이다.

```text
A
B
C
```

가 있다면:

```text
A
B
C
A
B
C
...
```

처럼 Sample 수를 늘릴 수 있다.

---

# 47. RandomOverSampler

```python
from imblearn.over_sampling import (
    RandomOverSampler
)


over = RandomOverSampler(
    random_state=42
)


X_train_over, y_train_over = (
    over.fit_resample(
        X_train,
        y_train
    )
)
```

---

# 48. Oversampling 장점

Majority Sample을 삭제하지 않으면서 Minority Class 비율을 높일 수 있다.

---

# 49. Oversampling 단점

같은 Sample을 반복 복제하면 모델이 해당 Sample에 과하게 맞을 가능성이 있다.

즉:

```text
Overfitting
```

을 주의해야 한다.

---

# 50. SMOTE

대표적인 Synthetic Oversampling 방법이다.

```text
SMOTE
=
Synthetic Minority Over-sampling Technique
```

이다.

---

# 51. SMOTE 핵심

단순히 기존 Minority Sample을 복사하는 것이 아니라 Minority Sample 사이의 Feature 공간을 이용하여 새로운 Synthetic Sample을 만든다.

---

# 52. 쉬운 그림

Minority Sample:

```text
A ●

        ● B
```

SMOTE는 두 Sample 사이와 같은 Feature 공간에:

```text
A ●
   ×
      ×
        ● B
```

처럼 Synthetic Sample을 만들 수 있다.

---

# 53. 중요한 점

SMOTE가 실제 새로운 환자나 실제 새로운 관측값을 만들어낸다는 뜻은 아니다.

```text
Feature Space에서
Synthetic Training Sample을 생성
```

하는 것이다.

---

# 54. SMOTE 설치

`imbalanced-learn`이 없다면:

```powershell
pip install imbalanced-learn
```

Jupyter/Colab:

```python
!pip install imbalanced-learn
```

---

# 55. SMOTE Import

```python
from imblearn.over_sampling import (
    SMOTE
)
```

---

# 56. SMOTE 사용

```python
smote = SMOTE(
    random_state=42
)


X_train_smote, y_train_smote = (
    smote.fit_resample(
        X_train,
        y_train
    )
)
```

---

# 57. 전후 비교

```python
print(
    "Before:"
)

print(
    y_train.value_counts()
)


print(
    "After:"
)

print(
    y_train_smote.value_counts()
)
```

---

# 58. 가장 중요한 규칙

SMOTE는:

```text
Train Data에만 적용
```

한다.

Validation이나 Test에는 적용하지 않는다.

---

# 59. 잘못된 방법

```text
전체 데이터
↓
SMOTE
↓
Train / Test Split
```

이렇게 하면 Synthetic Sample 생성 과정에서 Test 쪽 정보가 간접적으로 Training에 영향을 줄 수 있다.

Data Leakage가 발생할 수 있다.

---

# 60. 올바른 방법

```text
전체 데이터
↓
Train / Test Split
↓
Train에만 SMOTE
↓
Model 학습
↓
원본 Test로 평가
```

이다.

---

# 61. 코드

```python
X_train, X_test, y_train, y_test = (
    train_test_split(
        X,
        y,
        test_size=0.2,
        stratify=y,
        random_state=42
    )
)


smote = SMOTE(
    random_state=42
)


X_train_smote, y_train_smote = (
    smote.fit_resample(
        X_train,
        y_train
    )
)


model.fit(
    X_train_smote,
    y_train_smote
)


pred = model.predict(
    X_test
)
```

---

# 62. Cross Validation에서는 더 주의

5-Fold CV라면 각 Fold의 Training Data에만 SMOTE를 적용해야 한다.

```text
Fold Train
↓
SMOTE
↓
Model fit

Fold Validation
↓
원본 그대로 평가
```

이다.

---

# 63. 잘못된 CV

```text
전체 Train
↓
SMOTE
↓
5-Fold CV
```

이렇게 하면 Fold 간 Leakage가 생길 수 있다.

---

# 64. imblearn Pipeline

이 문제를 안전하게 처리하기 위해:

```python
from imblearn.pipeline import (
    Pipeline
)
```

을 사용할 수 있다.

---

# 65. Pipeline 만들기

```python
from imblearn.pipeline import (
    Pipeline
)

from imblearn.over_sampling import (
    SMOTE
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

# 66. Cross Validation

```python
from sklearn.model_selection import (
    cross_val_score
)


scores = cross_val_score(
    pipeline,
    X,
    y,
    cv=skf,
    scoring="f1"
)
```

이렇게 하면 각 Fold의 Training 부분에서만 SMOTE가 수행된다.

---

# 67. 왜 sklearn Pipeline이 아닌가?

일반적인 Scikit-learn Transformer는 보통:

```text
Sample 수를 유지
```

한다.

하지만 SMOTE는:

```text
Sample 수 자체를 변경
```

한다.

그래서 Sampling을 포함한 Pipeline에서는:

```text
imblearn.pipeline.Pipeline
```

을 사용하는 것이 일반적이다.

---

# 68. SMOTE도 무조건 좋은가?

아니다.

SMOTE가 만든 Synthetic Sample이 실제 데이터 구조를 잘 반영하지 못할 수 있다.

특히:

```text
Noise

Outlier

Class Overlap
```

이 심한 데이터에서는 주의해야 한다.

---

# 69. 범주형 Feature 주의

일반 SMOTE는 숫자 Feature 공간에서 보간을 수행한다.

범주형 변수가 단순 숫자로 Encoding되어 있을 때 일반 SMOTE를 무작정 적용하면 의미 없는 중간값이 만들어질 수 있다.

예:

```text
혈액형

A = 0
B = 1
O = 2
```

에서:

```text
0.5
```

같은 값은 실제 혈액형을 의미하지 않는다.

---

# 70. SMOTENC

Numeric Feature와 Categorical Feature가 함께 있는 경우 상황에 따라:

```text
SMOTENC
```

를 고려할 수 있다.

```python
from imblearn.over_sampling import (
    SMOTENC
)
```

사용 시 어떤 Feature가 Categorical인지 정확히 지정해야 한다.

---

# 71. KMeansSMOTE

SMOTE의 변형 중 하나이다.

```text
KMeans
+
SMOTE
```

아이디어를 결합한다.

---

# 72. KMeansSMOTE 개념

단순하게 모든 Minority Sample을 같은 방식으로 처리하기보다 데이터 공간을 Cluster 관점에서 나눈 뒤 Synthetic Sample을 생성한다.

개념:

```text
Training Data
↓
Clustering
↓
Minority 분포 확인
↓
적절한 Cluster에서
Synthetic Sample 생성
```

이다.

---

# 73. KMeansSMOTE Import

```python
from imblearn.over_sampling import (
    KMeansSMOTE
)
```

---

# 74. 사용

```python
kmeans_smote = KMeansSMOTE(
    random_state=42
)


X_train_resampled, y_train_resampled = (
    kmeans_smote.fit_resample(
        X_train,
        y_train
    )
)
```

---

# 75. KMeansSMOTE도 Train에만 적용

SMOTE와 똑같다.

```text
Train
→ KMeansSMOTE

Validation
→ 원본

Test
→ 원본
```

이다.

---

# 76. Sampling Strategy

Sampling 정도를 조절할 수도 있다.

예:

```python
smote = SMOTE(
    sampling_strategy=0.5,
    random_state=42
)
```

Binary Classification에서 설정 의미는 sampler와 상황에 따라 정확히 확인해야 하지만, 핵심은 반드시 완벽한 1:1 비율로 만들 필요는 없다는 것이다.

---

# 77. 1:1이 무조건 최선인가?

아니다.

원본:

```text
0 = 1000
1 = 100
```

을 반드시:

```text
0 = 1000
1 = 1000
```

으로 만들 필요는 없다.

예:

```text
0 = 1000
1 = 500
```

같은 비율이 더 좋은 결과를 낼 수도 있다.

Validation으로 비교해야 한다.

---

# 78. Threshold

Binary Classification 모델은 흔히 Probability를 출력한다.

```python
prob = model.predict_proba(
    X_valid
)[:, 1]
```

예:

```text
0.10
0.23
0.47
0.62
0.91
```

---

# 79. 기본 Threshold

많은 Binary Classifier에서는 보통:

```text
0.5
```

를 기준으로 Class를 결정한다.

```python
pred = (
    prob >= 0.5
).astype(int)
```

---

# 80. Threshold를 낮추면?

예:

```text
0.5
→
0.3
```

으로 낮추면 더 많은 Sample을 Positive로 예측하게 된다.

일반적으로:

```text
Recall ↑

False Positive ↑

Precision ↓ 가능
```

같은 변화가 발생할 수 있다.

---

# 81. Threshold를 높이면?

예:

```text
0.5
→
0.8
```

이면 Positive라고 판단하기 더 어려워진다.

일반적으로:

```text
Precision ↑ 가능

Recall ↓
```

같은 Trade-off가 나타날 수 있다.

---

# 82. Threshold도 Hyperparameter처럼 선택해야 한다

잘못된 방식:

```text
Test 결과를 보고
Threshold 0.5 → 0.4 → 0.3
```

로 바꾸는 것이다.

Threshold는:

```text
Validation

Cross Validation

OOF Prediction
```

을 이용하여 선택하는 것이 적절하다.

---

# 83. Threshold 실험

```python
from sklearn.metrics import (
    precision_score,
    recall_score,
    f1_score
)


thresholds = [
    0.1,
    0.2,
    0.3,
    0.4,
    0.5,
    0.6,
    0.7,
    0.8,
    0.9
]


for threshold in thresholds:

    pred = (
        prob >= threshold
    ).astype(int)

    precision = precision_score(
        y_valid,
        pred,
        zero_division=0
    )

    recall = recall_score(
        y_valid,
        pred,
        zero_division=0
    )

    f1 = f1_score(
        y_valid,
        pred,
        zero_division=0
    )

    print(
        threshold,
        precision,
        recall,
        f1
    )
```

---

# 84. F1이 가장 높은 Threshold

예:

```text
Threshold 0.2
F1 = 0.61

Threshold 0.3
F1 = 0.67

Threshold 0.4
F1 = 0.64

Threshold 0.5
F1 = 0.58
```

이라면 Validation 기준으로:

```text
0.3
```

이 후보가 될 수 있다.

하지만 실제 문제에서 Recall 등 다른 목표가 더 중요하다면 F1 최고점만으로 결정하지 않는다.

---

# 85. 의료 AI Threshold

예를 들어:

```text
위험 환자를 놓치지 않는 것
```

이 매우 중요하다면 높은 Recall을 요구할 수 있다.

하지만 Threshold를 너무 낮추면:

```text
False Alarm
```

이 너무 많이 발생할 수 있다.

따라서 임상적 목적과 운영 비용을 함께 고려해야 한다.

---

# 86. Cost-Sensitive Learning

모든 오류의 비용이 같지 않을 수 있다.

예:

```text
False Negative 비용
>
False Positive 비용
```

인 문제에서는 이러한 차이를 학습과 Threshold 선택에 반영할 수 있다.

Class Weight도 Cost-Sensitive 접근의 한 방법으로 볼 수 있다.

---

# 87. XGBoost의 불균형 처리

XGBoost에서는 Binary Classification에서:

```text
scale_pos_weight
```

를 사용할 수 있다.

대표적인 시작값:

```text
negative samples
/
positive samples
```

이다.

---

# 88. 계산

```python
negative = (
    y_train == 0
).sum()

positive = (
    y_train == 1
).sum()


scale_pos_weight = (
    negative
    /
    positive
)


print(
    scale_pos_weight
)
```

---

# 89. XGBoost 적용

```python
from xgboost import (
    XGBClassifier
)


model = XGBClassifier(
    n_estimators=500,
    learning_rate=0.05,
    max_depth=5,
    scale_pos_weight=scale_pos_weight,
    random_state=42
)
```

---

# 90. 계산값이 무조건 최적인가?

아니다.

```text
negative / positive
```

는 합리적인 시작점으로 사용할 수 있지만 최적값은 Validation 또는 Cross Validation으로 확인한다.

---

# 91. LightGBM

LightGBM Classification에서도 상황에 따라:

```text
class_weight

is_unbalance

scale_pos_weight
```

등의 불균형 관련 옵션을 사용할 수 있다.

동시에 여러 옵션을 무작정 조합하기보다 사용 중인 API와 목적을 확인한다.

---

# 92. CatBoost

CatBoost에서도:

```text
class_weights
```

등을 사용할 수 있다.

예:

```python
from catboost import (
    CatBoostClassifier
)


model = CatBoostClassifier(
    class_weights=[
        1,
        5
    ],
    verbose=0,
    random_seed=42
)
```

---

# 93. Class Weight vs SMOTE

Class Weight:

```text
원본 Sample 수 유지

Loss에서 Minority 중요도 증가
```

SMOTE:

```text
Training Sample 자체 증가

Synthetic Minority Sample 생성
```

이다.

---

# 94. 어느 것이 더 좋은가?

정답은 없다.

비교 실험한다.

예:

```text
Baseline

Class Weight

Random Oversampling

SMOTE

KMeansSMOTE

Threshold Tuning

Class Weight + Threshold
```

등을 같은 Validation 방식으로 비교한다.

---

# 95. 한꺼번에 전부 적용하면?

예:

```text
SMOTE
+
class_weight
+
Threshold 0.1
```

을 처음부터 동시에 적용하면 어떤 방법이 성능 변화의 원인인지 알기 어렵다.

따라서 한 단계씩 실험하는 것이 좋다.

---

# 96. 실험 구조

```text
Experiment 0
→ Baseline

Experiment 1
→ class_weight

Experiment 2
→ SMOTE

Experiment 3
→ KMeansSMOTE

Experiment 4
→ Threshold Tuning
```

처럼 기록한다.

---

# 97. 동일 조건 유지

비교할 때:

```text
같은 Split

같은 Fold

같은 Feature

같은 Metric

같은 Random Seed
```

등을 유지하면 비교하기 쉽다.

---

# 98. Baseline

먼저 아무 처리 없이 모델을 학습한다.

```python
baseline = LogisticRegression(
    max_iter=1000
)
```

결과를 기록한다.

---

# 99. Class Weight Experiment

```python
weighted_model = LogisticRegression(
    class_weight="balanced",
    max_iter=1000
)
```

결과를 Baseline과 비교한다.

---

# 100. SMOTE Experiment

```text
Train
↓
SMOTE
↓
Model
↓
Validation
```

으로 평가한다.

---

# 101. 비교표 예

| Method | Accuracy | Precision | Recall | F1 | ROC-AUC |
|---|---:|---:|---:|---:|---:|
| Baseline | 0.94 | 0.70 | 0.30 | 0.42 | 0.82 |
| Weight | 0.88 | 0.45 | 0.72 | 0.55 | 0.84 |
| SMOTE | 0.87 | 0.43 | 0.75 | 0.55 | 0.83 |

이런 결과라면 단순 Accuracy만 보고 Baseline을 선택해서는 안 될 수 있다.

---

# 102. Precision과 Recall Trade-off

보통:

```text
Recall을 높이려고 하면
False Positive가 증가할 수 있음

Precision을 높이려고 하면
Positive 예측을 더 보수적으로 하여
Recall이 낮아질 수 있음
```

이라는 Trade-off가 있다.

---

# 103. 어떤 오류가 더 중요한가?

이 질문이 먼저다.

```text
FP가 더 위험한가?

FN이 더 위험한가?
```

에 따라 모델 선택 기준이 달라진다.

---

# 104. 예: 암 선별 검사

단순화한 예로:

```text
질병 환자를 놓치는 것
→ FN
```

의 비용이 매우 크다고 판단하면 Recall을 중요하게 볼 수 있다.

하지만 실제 의료 시스템에서는 후속 검사 비용, False Alarm, 환자 부담 등도 함께 고려해야 한다.

---

# 105. 예: 스팸 분류

정상 이메일을 스팸으로 잘못 분류:

```text
False Positive
```

가 사용자에게 큰 불편을 줄 수 있다.

문제마다 중요한 오류가 다르다.

---

# 106. ROC Curve

```python
from sklearn.metrics import (
    roc_curve
)


fpr, tpr, thresholds = (
    roc_curve(
        y_valid,
        prob
    )
)
```

---

# 107. Precision-Recall Curve

```python
from sklearn.metrics import (
    precision_recall_curve
)


precision, recall, thresholds = (
    precision_recall_curve(
        y_valid,
        prob
    )
)
```

이 결과를 이용하여 Threshold에 따른 Precision/Recall 변화를 분석할 수 있다.

---

# 108. Average Precision

Scikit-learn:

```python
from sklearn.metrics import (
    average_precision_score
)


ap = average_precision_score(
    y_valid,
    prob
)
```

Average Precision은 Precision-Recall 성능을 요약하는 지표 중 하나이다.

---

# 109. ROC-AUC가 높으면 끝?

아니다.

예:

```text
ROC-AUC = 0.90
```

이어도 실제 Threshold에서:

```text
Recall = 0.20
```

일 수 있다.

따라서:

```text
Ranking Metric

실제 Threshold Metric
```

을 함께 확인하는 것이 좋다.

---

# 110. PR-AUC가 특히 유용한 경우

Positive Class가 매우 희귀하면:

```text
Precision

Recall
```

관계가 중요한 경우가 많다.

따라서:

```text
ROC-AUC
+
PR Curve / Average Precision
```

을 함께 확인할 수 있다.

---

# 111. Data Leakage 다시 확인

절대 주의:

```text
전체 데이터 SMOTE
↓
Split
```

하면 안 된다.

또:

```text
전체 데이터에서 Threshold 선택
```

도 최종 Test가 포함되어 있다면 적절하지 않다.

---

# 112. 올바른 전체 흐름

```text
전체 데이터
       ↓
Train / Test
       ↓
Train에서 CV
       ↓
각 Training Fold에만 Sampling
       ↓
Validation Fold 평가
       ↓
방법 선택
       ↓
Threshold 선택
       ↓
최종 Test
```

이다.

---

# 113. OOF와 Threshold

Cross Validation의:

```text
OOF Probability
```

를 모아 Threshold를 선택할 수도 있다.

```text
Fold 1 Validation Probability
Fold 2 Validation Probability
Fold 3 Validation Probability
Fold 4 Validation Probability
Fold 5 Validation Probability
        ↓
전체 OOF Probability
        ↓
Threshold 분석
```

이다.

---

# 114. 왜 OOF Threshold?

한 Validation Split에만 Threshold를 맞추는 것보다 전체 Training Sample의 OOF Prediction을 활용할 수 있다는 장점이 있다.

단, 최종 Test는 여전히 별도로 유지한다.

---

# 115. 이미지 Classification에서는?

Image Dataset도 Class Imbalance가 있을 수 있다.

예:

```text
Normal = 1000

Disease = 4000
```

같은 상황이다.

이 경우:

```text
Weighted Loss

Weighted Sampler

Data Augmentation

Threshold Tuning
```

등을 고려할 수 있다.

---

# 116. Weighted CrossEntropyLoss

PyTorch에서는:

```python
import torch

import torch.nn as nn


class_weights = torch.tensor(
    [
        1.9,
        0.7
    ],
    dtype=torch.float32
).to(
    device
)


loss_fn = nn.CrossEntropyLoss(
    weight=class_weights
)
```

처럼 Class Weight를 Loss에 넣을 수 있다.

---

# 117. Weighted Loss 의미

Sample이 적은 Class의 오분류에 더 큰 Loss를 부여하여 모델이 해당 Class를 더 중요하게 학습하도록 유도한다.

---

# 118. Weighted Loss도 검증 필요

Weight를 적용했다고 무조건 좋아지는 것은 아니다.

비교:

```text
Baseline Loss

Weighted Loss
```

그리고:

```text
Accuracy
Macro F1
Class별 Recall
Confusion Matrix
```

등을 함께 확인한다.

---

# 119. Macro F1

각 Class의 F1을 계산한 뒤 동일한 Weight로 평균하는 방식이다.

따라서 Majority Class가 전체 결과를 지나치게 지배하는 문제를 줄여서 볼 수 있다.

---

# 120. Weighted F1

각 Class의 Sample 수를 고려하여 F1을 평균한다.

따라서 Majority Class의 영향이 더 크게 반영될 수 있다.

---

# 121. Macro와 Weighted 차이

```text
Macro
→ 각 Class를 동등하게 취급

Weighted
→ 각 Class Sample 수를 고려
```

이다.

불균형 데이터에서는 이 차이를 이해해야 한다.

---

# 122. Minority Sample이 너무 적으면?

예:

```text
Class 1 = 5개
```

밖에 없다면 단순히 SMOTE를 적용한다고 문제가 해결되지는 않는다.

근본적으로:

```text
실제 Minority Data가 너무 부족
```

하기 때문이다.

---

# 123. Synthetic Data의 한계

Synthetic Sampling은 원본 Minority Data에서 정보를 얻는다.

원본 자체가 매우 적거나 편향되어 있다면 Synthetic Sample도 그 한계를 가진다.

---

# 124. 가장 좋은 해결책 중 하나

가능하다면:

```text
실제 Minority Class 데이터를 더 확보
```

하는 것이 중요하다.

하지만 실제 의료 데이터에서는 비용, 개인정보, 희귀질환 등의 이유로 어려울 수 있다.

---

# 125. Label 품질도 확인

Minority Class가 적을수록 잘못된 Label 몇 개가 큰 영향을 줄 수 있다.

따라서:

```text
Label 오류

중복

이상치

잘못된 기록
```

등을 점검하는 것도 중요하다.

---

# 126. Class Imbalance와 Overfitting

Minority Data가 매우 적으면 모델이 소수 Sample 자체를 외워버릴 수 있다.

따라서:

```text
Train Recall = 매우 높음

Validation Recall = 매우 낮음
```

같은 현상이 발생할 수 있다.

---

# 127. Sampling 전후 분포 기록

```python
before = (
    y_train.value_counts()
)

after = (
    y_train_smote.value_counts()
)


print(
    "Before:"
)

print(
    before
)


print(
    "After:"
)

print(
    after
)
```

실험 기록에 남긴다.

---

# 128. Sampling은 Validation/Test를 현실과 다르게 만들면 안 된다

실제 서비스 환경:

```text
질병 = 5%
```

인데 Test를 인위적으로:

```text
질병 = 50%
```

로 바꾸면 실제 환경에서의 성능을 제대로 평가하기 어려울 수 있다.

Training Balance와 Evaluation Distribution은 구분해야 한다.

---

# 129. 확률 Calibration도 생각할 수 있다

Sampling이나 Class Weight를 적용하면 모델의 출력 Probability가 실제 발생 확률과 다르게 Calibration될 수 있다.

예:

```text
P = 0.8
```

이 실제로 정확히 80% 위험을 뜻하는지는 별도의 Calibration 평가가 필요하다.

---

# 130. Probability와 Class는 다르다

```python
prob = model.predict_proba(
    X_test
)[:, 1]
```

는:

```text
Positive Class에 대한 모델의 점수/확률 출력
```

이다.

Threshold를 적용하면:

```python
pred = (
    prob >= 0.5
).astype(int)
```

Class가 된다.

---

# 131. 불균형 문제 해결 순서

처음부터 SMOTE부터 적용하지 않는다.

추천 흐름:

```text
1. Class 분포 확인

2. Baseline 학습

3. Confusion Matrix 확인

4. Precision / Recall / F1 확인

5. ROC-AUC / PR 관련 Metric 확인

6. Stratified CV

7. Class Weight 실험

8. Sampling 실험

9. Threshold 실험

10. 결과 비교
```

---

# 132. 전체 예제

```python
from sklearn.model_selection import (
    train_test_split
)

from sklearn.linear_model import (
    LogisticRegression
)

from sklearn.metrics import (
    classification_report,
    confusion_matrix,
    roc_auc_score,
    average_precision_score
)


X_train, X_test, y_train, y_test = (
    train_test_split(
        X,
        y,
        test_size=0.2,
        stratify=y,
        random_state=42
    )
)


model = LogisticRegression(
    class_weight="balanced",
    max_iter=1000,
    random_state=42
)


model.fit(
    X_train,
    y_train
)


pred = model.predict(
    X_test
)


prob = model.predict_proba(
    X_test
)[:, 1]


print(
    confusion_matrix(
        y_test,
        pred
    )
)


print(
    classification_report(
        y_test,
        pred,
        zero_division=0
    )
)


print(
    "ROC-AUC:",
    roc_auc_score(
        y_test,
        prob
    )
)


print(
    "Average Precision:",
    average_precision_score(
        y_test,
        prob
    )
)
```

---

# 133. SMOTE + CV 전체 예

```python
from sklearn.model_selection import (
    StratifiedKFold,
    cross_val_score
)

from sklearn.preprocessing import (
    StandardScaler
)

from sklearn.linear_model import (
    LogisticRegression
)

from imblearn.over_sampling import (
    SMOTE
)

from imblearn.pipeline import (
    Pipeline
)


skf = StratifiedKFold(
    n_splits=5,
    shuffle=True,
    random_state=42
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
                max_iter=1000,
                random_state=42
            )
        )
    ]
)


scores = cross_val_score(
    pipeline,
    X,
    y,
    cv=skf,
    scoring="f1"
)


print(
    "Fold F1:",
    scores
)


print(
    "Mean F1:",
    scores.mean()
)


print(
    "Std F1:",
    scores.std()
)
```

---

# 134. 이 Pipeline에서 일어나는 일

각 Fold마다:

```text
Training Fold
      ↓
StandardScaler fit
      ↓
Training Fold Scaling
      ↓
SMOTE
      ↓
Logistic Regression
      ↓
Model fit
```

그리고:

```text
Validation Fold
      ↓
Train에서 학습한 Scaler로 transform
      ↓
SMOTE 하지 않음
      ↓
Prediction
      ↓
F1 평가
```

이다.

---

# 135. 가장 중요한 실수

```text
전체 데이터에 SMOTE
```

를 먼저 해버리는 것이다.

반드시 기억:

```text
Split 먼저

Sampling 나중
```

이다.

Cross Validation에서는 더 정확히:

```text
Fold 먼저

각 Train Fold에 Sampling
```

이다.

---

# 136. 해결 방법 비교

```text
Class Weight
→ 데이터 개수는 그대로
→ Loss 중요도 변경

Undersampling
→ Majority Sample 감소

Oversampling
→ Minority Sample 증가

SMOTE
→ Synthetic Minority Sample 생성

KMeansSMOTE
→ Cluster를 고려한 Synthetic Sampling

Threshold Tuning
→ Class를 결정하는 기준 변경
```

---

# 137. 해결 방법과 평가 방법은 다르다

예:

```text
SMOTE
Class Weight
Threshold
```

는 불균형 문제에 대응하는 방법이다.

반면:

```text
Recall
F1
ROC-AUC
PR-AUC
```

는 모델을 평가하는 Metric이다.

두 개를 혼동하지 않는다.

---

# 138. Accuracy가 나쁜 Metric이라는 뜻?

아니다.

Accuracy 자체가 잘못된 Metric은 아니다.

다만 심한 Class Imbalance에서:

```text
Accuracy 하나만 보는 것
```

이 위험하다는 뜻이다.

---

# 139. 가장 중요한 질문

불균형 데이터에서는 먼저:

```text
어떤 Class가 중요한가?

어떤 오류가 더 위험한가?

FP와 FN 중 무엇을 더 줄여야 하는가?
```

를 생각한다.

그다음 Metric과 Threshold를 선택한다.

---

# 140. 핵심 용어

```text
Class Imbalance
→ Class별 Sample 수 차이가 큰 상태

Majority Class
→ Sample이 많은 Class

Minority Class
→ Sample이 적은 Class

Class Weight
→ Class별 학습 중요도 조정

Undersampling
→ Majority Sample 감소

Oversampling
→ Minority Sample 증가

SMOTE
→ Synthetic Minority Sample 생성

KMeansSMOTE
→ Clustering과 SMOTE 결합

Threshold
→ Probability를 Class로 바꾸는 기준

Precision
→ Positive 예측 중 실제 Positive 비율

Recall
→ 실제 Positive 중 찾아낸 비율

F1
→ Precision과 Recall을 함께 고려

ROC-AUC
→ 여러 Threshold에서 구분 능력 평가

PR-AUC
→ Precision-Recall 관계를 중심으로 평가
```

---

# 141. 가장 중요한 그림

```text
Imbalanced Data
       ↓
Class 분포 확인
       ↓
Baseline
       ↓
Confusion Matrix
       ↓
Precision / Recall / F1
       ↓
ROC-AUC / PR 관련 Metric
       ↓
┌───────────────┬──────────────┐
↓               ↓              ↓
Class Weight   Sampling      Threshold
                ↓
        ┌───────┼────────┐
        ↓       ↓        ↓
      Under    Over     SMOTE
                         ↓
                    KMeansSMOTE
        └───────┬────────┘
                ↓
        Cross Validation
                ↓
          방법 비교
                ↓
          Final Test
```

---

# 142. 배운 점

Class Imbalance는:

```text
Class별 데이터 수가 크게 다른 문제
```

이다.

예:

```text
0 = 95%
1 = 5%
```

일 때 모델이 모든 데이터를 `0`으로 예측해도:

```text
Accuracy = 95%
```

가 나올 수 있다.

따라서 Accuracy만 보지 않고:

```text
Confusion Matrix

Precision

Recall

F1

ROC-AUC

PR 관련 Metric
```

등을 함께 확인해야 한다.

불균형 문제를 다루는 대표적인 방법:

```text
Class Weight

Undersampling

Oversampling

SMOTE

KMeansSMOTE

Threshold Tuning
```

등이 있다.

하지만 가장 중요한 것은:

```text
Sampling은 Train에만 적용
```

하는 것이다.

Cross Validation에서는:

```text
각 Training Fold에만 Sampling
```

해야 한다.

그리고:

```text
불균형 데이터를 무조건 1:1로 만드는 것
```

이 목적이 아니다.

진짜 목적은:

```text
실제 문제에서 중요한 Class를
더 잘 구분하도록 모델을 만드는 것
```

이다.