# KTAS Misclassification Prediction

## 1. 프로젝트 개요

이 프로젝트는 응급실 환자의 KTAS 분류 데이터를 이용하여 초기 중증도 분류가 최종 평가와 다르게 이루어진 사례를 분석하고, 오분류 가능성을 예측하는 Machine Learning 프로젝트이다.

전체 흐름은 다음과 같다.

```text
Patient Data
+
KTAS Data
↓
Data Integration
↓
KTAS 분류 비교
↓
정상분류 / 과소분류 / 과대분류 정의
↓
EDA
↓
Preprocessing
↓
오분류 Target 생성
↓
Machine Learning
↓
Threshold 조정
↓
Evaluation
↓
Feature Importance / SHAP
↓
Failure Analysis
```

---

# 2. KTAS란?

KTAS는:

```text
Korean Triage and Acuity Scale
```

의 약자이다.

응급환자의 중증도와 긴급도를 분류하기 위해 사용하는 체계이다.

일반적으로 여러 단계의 등급을 이용하여 환자의 응급도를 표현한다.

---

# 3. 프로젝트에서 중요한 질문

단순히:

```text
KTAS 등급이 몇 등급인가?
```

만 보는 것이 아니다.

이번 프로젝트의 핵심 질문은:

```text
초기 KTAS 분류가
최종적으로 적절했는가?
```

이다.

---

# 4. 오분류

초기 분류와 최종 평가가 다르다면 다음과 같이 생각할 수 있다.

```text
정상분류

과소분류

과대분류
```

---

# 5. 정상분류

초기 KTAS와 최종 기준이 일치하는 경우이다.

```text
초기 판단
=
최종 판단
```

---

# 6. 과소분류

실제 환자의 상태보다 덜 심각한 것으로 분류한 경우를 의미한다.

개념적으로:

```text
실제로 더 위급한 환자
↓
초기에는 덜 위급하다고 판단
```

한 경우이다.

---

# 7. 과대분류

실제 상태보다 더 위급한 것으로 분류한 경우이다.

```text
실제로는 상대적으로 덜 위급
↓
초기에는 더 위급하다고 판단
```

한 경우이다.

---

# 8. 왜 과소분류가 중요할까?

응급의료에서는 위급한 환자를 낮은 우선순위로 분류하면 적절한 치료가 지연될 가능성이 있다.

따라서 프로젝트 목적에 따라:

```text
과소분류 탐지
```

의 Recall을 중요하게 볼 수 있다.

---

# 9. 하지만 과대분류도 중요하다

모든 환자를 위험하다고 판단하면 과소분류는 줄어들 수 있지만:

```text
불필요한 경고 증가

의료진 부담 증가

응급 자원 사용 증가
```

등의 문제가 발생할 수 있다.

따라서 Recall만 무조건 높이는 것도 정답은 아니다.

---

# 10. Data 구성

프로젝트에서는 크게 두 종류의 Data를 사용했다.

```text
Patient Profile Data

KTAS Data
```

---

# 11. Patient Profile

환자의 기본 정보가 포함된다.

예:

```text
연령

성별

Vital Sign

통증 관련 정보

기타 환자 상태
```

등이다.

---

# 12. KTAS Data

KTAS 평가와 관련된 정보가 포함된다.

예:

```text
KTAS 등급

초기 분류 정보

간호사 평가 정보

Vital Sign

통증 정보
```

등을 포함할 수 있다.

실제 Column 의미는 Data Dictionary를 기준으로 확인한다.

---

# 13. Data 읽기

예:

```python
import pandas as pd
import numpy as np


patient = pd.read_csv(
    "patient_profile_v2.csv"
)


ktas = pd.read_csv(
    "ktas_data.csv"
)
```

실제 파일명과 경로에 맞게 수정한다.

---

# 14. Shape 확인

```python
print(
    "patient:",
    patient.shape
)


print(
    "ktas:",
    ktas.shape
)
```

---

# 15. 당시 Patient Data

전체 Patient Profile은 약:

```text
1,807명
```

이었다.

---

# 16. Cohort

환자를 크게:

```text
성인

소아청소년
```

으로 구분할 수 있었다.

---

# 17. 당시 구성

```text
성인
1,267명

소아청소년
540명
```

이었다.

---

# 18. KTAS Data 연결

KTAS Data는:

```text
1,267명
```

규모였다.

---

# 19. 중요한 확인

통합 후:

```text
성인
→ KTAS Data 있음

소아청소년
→ KTAS Data 없음
```

이라는 구조가 확인되었다.

---

# 20. 이것이 왜 중요할까?

단순히 Missing Value라고 생각해서:

```text
540명의 KTAS가 우연히 누락되었다.
```

라고 해석하면 안 된다.

실제로는 Cohort에 따라 Data 제공 구조가 다른 것일 수 있다.

---

# 21. 구조적 Missing

이런 경우를:

```text
Structural Missing
```

관점에서 생각할 수 있다.

즉:

```text
Data가 원래 존재하지 않는 구조
```

일 수 있다.

---

# 22. Data Merge

공통 Key를 기준으로 연결한다.

예:

```python
merged = pd.merge(
    patient,
    ktas,
    on="patient_id",
    how="left"
)
```

실제 Key 이름에 맞게 수정한다.

---

# 23. Merge 후 확인

```python
print(
    merged.shape
)
```

---

# 24. 당시 최종 통합 Data

약:

```text
1,807 Rows

54 Columns
```

구조였다.

---

# 25. Cohort 확인

```python
print(
    merged[
        "cohort"
    ].value_counts(
        dropna=False
    )
)
```

---

# 26. KTAS 연결 여부

예:

```python
merged[
    "has_ktas_data"
] = (
    merged[
        "KTAS_RN"
    ].notna()
)
```

실제 기준 Column은 Data에 맞게 선택한다.

---

# 27. 확인

```python
print(
    merged[
        "has_ktas_data"
    ].value_counts()
)
```

---

# 28. Cross Tabulation

```python
print(
    pd.crosstab(
        merged[
            "cohort"
        ],
        merged[
            "has_ktas_data"
        ]
    )
)
```

---

# 29. 왜 Crosstab?

단순 Missing Count보다:

```text
어떤 Group에서 Missing이 발생하는가?
```

를 확인할 수 있기 때문이다.

---

# 30. 중요한 교훈

Missing을 발견하면:

```text
몇 개인가?
```

만 보는 것이 아니라:

```text
누구에게 발생했는가?
```

도 확인한다.

---

# 31. KTAS 일치 확인

통합 과정에서 같은 의미의 KTAS 정보가 여러 Data에 존재한다면 서로 일치하는지 확인한다.

---

# 32. 예

```python
match = (
    merged[
        "KTAS_RN_x"
    ]
    ==
    merged[
        "KTAS_RN_y"
    ]
)
```

---

# 33. 일치율

```python
print(
    match.mean()
    *
    100
)
```

---

# 34. 당시 확인

KTAS_RN의 일치율은:

```text
100%
```

였다.

---

# 35. 왜 중요한가?

Merge를 했다고 바로 분석하지 않는다.

```text
정말 같은 Patient끼리 연결되었는가?

중요한 값이 일치하는가?
```

를 확인해야 한다.

---

# 36. Data Quality

의료 Data에서는 Model 성능보다 먼저:

```text
Data 연결의 정확성
```

을 확인하는 것이 중요하다.

---

# 37. 분석 대상

KTAS Data가 실제 존재하는 성인 Cohort를 중심으로 오분류 분석을 진행할 수 있다.

---

# 38. 전체와 분석 대상 구분

```text
전체 Patient
1,807명

KTAS 분석 가능
1,267명
```

처럼 구분한다.

---

# 39. 왜 구분해야 할까?

전체 1,807명을 그대로 Model에 넣으면 KTAS가 없는 540명이 Target을 정의할 수 없는 상태로 포함될 수 있기 때문이다.

---

# 40. KTAS Data Cleaning

의료 Data에는 이상한 문자열이 들어 있을 수 있다.

실제로:

```text
NRS_pain

SBP
```

등에서 문제가 있었다.

---

# 41. NRS_pain

통증 점수 Column에 정상적인 숫자 대신:

```text
#BOŢ!
```

와 같은 문자열이 포함되어 있었다.

---

# 42. 당시 개수

약:

```text
556건
```

에서 해당 값이 관찰되었다.

---

# 43. 이것의 문제

Model은 Numeric Feature를 기대하는데 문자열이 섞여 있으면:

```text
Scaling 실패

Model Training 실패

잘못된 Data Type
```

문제가 발생할 수 있다.

---

# 44. Numeric 변환

```python
merged[
    "NRS_pain"
] = pd.to_numeric(
    merged[
        "NRS_pain"
    ],
    errors="coerce"
)
```

---

# 45. errors="coerce"

숫자로 변환할 수 없는 값은:

```text
NaN
```

으로 변환한다.

---

# 46. 중요한 점

이것으로 분석이 끝난 것이 아니다.

이제:

```text
왜 이 값이 발생했는가?

NaN을 어떻게 처리할 것인가?

Missing 자체가 의미가 있는가?
```

를 생각해야 한다.

---

# 47. SBP

수축기 혈압을 의미하는 SBP에서도:

```text
??
```

같은 비정상 문자열이 존재했다.

---

# 48. 당시 개수

약:

```text
25건
```

이었다.

---

# 49. 변환

```python
merged[
    "SBP"
] = pd.to_numeric(
    merged[
        "SBP"
    ],
    errors="coerce"
)
```

---

# 50. Data Cleaning의 핵심

```text
문자열을 숫자로 바꾼다.
```

에서 끝나지 않는다.

---

# 51. 순서

```text
이상값 발견
↓
원본 값 확인
↓
의미 확인
↓
Numeric 변환
↓
Missing 발생 확인
↓
처리 방법 결정
```

이다.

---

# 52. 주요 Feature

프로젝트에서 중요하게 관찰된 Feature에는 다음과 같은 것들이 있었다.

```text
NRS_pain

HR

SBP

BT

KTAS 관련 Feature
```

---

# 53. NRS

```text
Numeric Rating Scale
```

로 통증 정도를 수치화하는 데 사용되는 척도이다.

---

# 54. HR

```text
Heart Rate
```

즉 심박수이다.

---

# 55. SBP

```text
Systolic Blood Pressure
```

즉 수축기 혈압이다.

---

# 56. BT

```text
Body Temperature
```

즉 체온이다.

---

# 57. 오분류 Target

초기 KTAS와 비교 기준을 이용해:

```text
정상분류

과소분류

과대분류
```

를 정의한다.

---

# 58. 당시 통합 결과

전체 1,807명 기준으로:

```text
정상분류
1,081

과소분류
131

과대분류
55

KTAS Data 없음
540
```

구조로 볼 수 있었다.

---

# 59. KTAS 분석 대상만 보면

```text
1,081
+
131
+
55
=
1,267
```

이다.

---

# 60. 비율 확인

```python
triage_counts = (
    merged[
        "triage_type"
    ].value_counts(
        dropna=False
    )
)


print(
    triage_counts
)
```

---

# 61. 오분류

과소분류와 과대분류를 합쳐:

```text
오분류
```

라는 Binary Target을 만들 수도 있다.

---

# 62. 예

```python
analysis_df[
    "misclassified"
] = (
    analysis_df[
        "triage_type"
    ]
    !=
    "정상분류"
).astype(
    int
)
```

---

# 63. Binary Target

```text
0
→ 정상분류

1
→ 오분류
```

이다.

---

# 64. 하지만 주의

과소분류와 과대분류는 의료적으로 의미가 다르다.

따라서 단순히 둘을 합치는 것이 항상 최선은 아니다.

---

# 65. 분석 목적에 따라

다음처럼 별도로 만들 수도 있다.

```text
과소분류 예측

과대분류 예측

전체 오분류 예측
```

---

# 66. 이번 Model

프로젝트에서는 오분류 가능성을 탐지하는 Binary Classification 관점으로 Model을 만들었다.

---

# 67. Train / Test Split

정제 후 Model에 사용한 Data는 약:

```text
1,212명
```

이었다.

---

# 68. Split

```text
Train
969

Test
243
```

으로 나누었다.

---

# 69. 왜 1,267 → 1,212?

전처리와 Model에 사용할 수 있는 Record 조건에 따라 일부 Sample이 제외될 수 있다.

따라서 항상:

```text
전처리 전 Row 수

전처리 후 Row 수
```

를 기록해야 한다.

---

# 70. Test 오분류

Test 243명 중 실제 오분류는:

```text
26명
```

이었다.

---

# 71. 비율

약:

```text
10.7%
```

이다.

---

# 72. Class Imbalance

즉 Test Data에서:

```text
정상분류
약 89.3%

오분류
약 10.7%
```

이다.

---

# 73. Accuracy 함정

모든 환자를:

```text
정상분류
```

라고 Prediction해도 약:

```text
89%
```

Accuracy가 나올 수 있다.

---

# 74. 따라서

Accuracy만 사용하면 Model이 오분류 환자를 실제로 찾아내는지 판단하기 어렵다.

---

# 75. 중요한 Metric

```text
Precision

Recall

F1

Confusion Matrix

ROC-AUC

PR-AUC
```

등을 함께 고려한다.

---

# 76. 특히 Recall

오분류 환자를 놓치지 않는 것이 중요하다면:

```text
Recall
```

을 중요하게 볼 수 있다.

---

# 77. Recall

```text
TP
─────────
TP + FN
```

이다.

---

# 78. 의미

실제 오분류 환자 중:

```text
Model이 몇 %를 찾아냈는가?
```

이다.

---

# 79. Precision

```text
TP
─────────
TP + FP
```

이다.

---

# 80. 의미

Model이:

```text
오분류 위험
```

이라고 경고한 환자 중 실제 오분류가 몇 %였는가를 의미한다.

---

# 81. Recall만 높이면?

Threshold를 매우 낮추면 거의 모든 환자를 위험하다고 예측할 수 있다.

---

# 82. 그러면

```text
Recall ↑

Precision ↓

False Positive ↑
```

가 될 수 있다.

---

# 83. 그래서 F1

Precision과 Recall의 균형을 보기 위해:

```text
F1 Score
```

를 사용할 수 있다.

---

# 84. 첫 번째 Model

Random Forest를 사용하여 오분류 가능성을 예측했다.

---

# 85. Random Forest

```python
from sklearn.ensemble import (
    RandomForestClassifier
)


rf = RandomForestClassifier(
    n_estimators=300,
    random_state=42,
    n_jobs=-1
)
```

실제 Hyperparameter는 실험 설정에 따라 달라질 수 있다.

---

# 86. Probability

```python
rf.fit(
    X_train,
    y_train
)


rf_prob = rf.predict_proba(
    X_test
)[
    :,
    1
]
```

---

# 87. 기본 Threshold

보통:

```text
0.5
```

를 사용한다.

하지만 Imbalanced Classification에서는 0.5가 목적에 맞는 최적 Threshold라는 보장은 없다.

---

# 88. Threshold 조정

Random Forest에서는:

```text
Threshold = 0.19
```

수준의 실험을 진행했다.

---

# 89. Prediction

```python
threshold = 0.19


rf_pred = (
    rf_prob
    >=
    threshold
).astype(
    int
)
```

---

# 90. 당시 RF 결과

Threshold 0.19에서 약:

```text
Precision
0.3841

Recall
0.5524

F1
0.4531
```

의 결과를 얻었다.

---

# 91. Recall 0.5524

실제 오분류 환자의 약:

```text
55%
```

를 탐지했다는 의미로 해석할 수 있다.

---

# 92. Precision 0.3841

오분류라고 경고한 Sample 중 실제 오분류의 비율이 약:

```text
38%
```

였다는 의미이다.

---

# 93. 이것은 완벽한 Model인가?

아니다.

오분류 환자의 상당수를 여전히 놓치며 False Positive도 존재한다.

---

# 94. 하지만 Baseline보다

단순히 모든 환자를 정상이라고 판단하는 Model보다 오분류 위험 환자를 실제로 탐지하는 방향으로 발전한 것이다.

---

# 95. 두 번째 Model

XGBoost도 실험했다.

---

# 96. XGBoost

```python
from xgboost import (
    XGBClassifier
)


xgb = XGBClassifier(
    random_state=42
)
```

---

# 97. Probability

```python
xgb.fit(
    X_train,
    y_train
)


xgb_prob = xgb.predict_proba(
    X_test
)[
    :,
    1
]
```

---

# 98. XGBoost Threshold

Recall을 높이기 위해:

```text
Threshold = 0.06
```

수준까지 낮추는 실험을 진행했다.

---

# 99. 당시 XGBoost 결과

약:

```text
Accuracy
0.4691

Recall
0.9231
```

이었다.

---

# 100. Recall 0.9231

실제 오분류 환자의 약:

```text
92.31%
```

를 탐지했다는 의미이다.

---

# 101. 숫자만 보면 매우 좋아 보인다

하지만 Accuracy는:

```text
46.91%
```

까지 감소했다.

---

# 102. 왜?

Threshold를 낮추면서 많은 정상 환자까지:

```text
오분류 위험
```

이라고 Prediction했을 가능성이 크다.

---

# 103. Trade-off

```text
Threshold ↓

Recall ↑

False Positive ↑

Precision / Accuracy ↓ 가능
```

이다.

---

# 104. 이 프로젝트의 핵심 실험

```text
오분류 환자를 더 많이 찾기 위해
얼마나 많은 False Positive를 허용할 것인가?
```

라는 문제이다.

---

# 105. Threshold는 Model Hyperparameter와 다르다

Model Training이 끝난 후 Probability를 Class로 바꾸는 기준이다.

---

# 106. 예

```text
P(misclassification)
=
0.18
```

인 Patient가 있다고 하자.

---

# 107. Threshold 0.5

```text
0.18 < 0.5
→ 정상
```

---

# 108. Threshold 0.19

```text
0.18 < 0.19
→ 정상
```

---

# 109. Threshold 0.10

```text
0.18 >= 0.10
→ 오분류 위험
```

이 된다.

---

# 110. 같은 Model이다

Model이 바뀐 것이 아니라:

```text
의사결정 기준
```

이 바뀐 것이다.

---

# 111. Threshold 선택

Test Data를 보고 가장 좋은 Threshold를 선택하면 안 된다.

---

# 112. 올바른 방법

```text
Train
→ Model Training

Validation
→ Threshold 선택

Test
→ 최종 평가
```

이다.

---

# 113. Cross Validation

더 안정적으로 하려면 Validation Fold의 OOF Probability를 이용해 Threshold를 선택하는 방법도 있다.

---

# 114. PR Curve

Imbalanced Classification에서는 Precision-Recall Curve를 이용해 Threshold를 분석할 수 있다.

```python
from sklearn.metrics import (
    precision_recall_curve
)


precision, recall, thresholds = (
    precision_recall_curve(
        y_val,
        val_prob
    )
)
```

---

# 115. Threshold별 F1

```python
f1 = (
    2
    *
    precision
    *
    recall
    /
    (
        precision
        +
        recall
        +
        1e-8
    )
)
```

---

# 116. 하지만

단순히 F1이 최대인 Threshold가 의료적으로 가장 좋은 Threshold라는 보장은 없다.

---

# 117. 실제 목적

예를 들어:

```text
Recall 최소 0.80을 유지하면서
Precision을 최대화
```

같은 임상적 운영 기준을 먼저 정의할 수도 있다.

---

# 118. Feature Importance

Random Forest에서는:

```python
importance = pd.Series(
    rf.feature_importances_,
    index=X_train.columns
)


importance = importance.sort_values(
    ascending=False
)


display(
    importance.head(
        10
    )
)
```

---

# 119. 당시 RF 중요 Feature

상위 Feature에는:

```text
NRS_pain

HR

SBP

KTAS_RN_4

BT
```

등이 포함되었다.

---

# 120. 의미

Model이 오분류 여부를 구분할 때 이러한 Feature들을 상대적으로 많이 활용했다는 의미이다.

---

# 121. 하지만

```text
Feature Importance가 높다
=
오분류의 원인이다
```

라고 해석하면 안 된다.

---

# 122. SHAP

Model Prediction을 더 자세히 해석하기 위해 SHAP을 사용할 수 있다.

---

# 123. SHAP 질문

```text
이 Patient를
왜 오분류 위험이라고 판단했는가?
```

를 Model 관점에서 분석할 수 있다.

---

# 124. Global SHAP

전체 Dataset에서 어떤 Feature가 Prediction에 많이 기여하는지 확인한다.

---

# 125. 당시 SHAP

상위 Feature에:

```text
NRS_pain

HR

SBP
```

등이 나타났다.

---

# 126. RF Importance와 SHAP

두 방법 모두 비슷한 Feature를 중요하게 볼 수 있지만 의미는 완전히 동일하지 않다.

---

# 127. Feature Importance

Tree가 Feature를 얼마나 활용했는지에 대한 Model 기반 중요도이다.

---

# 128. SHAP

각 Feature가 Prediction을:

```text
Positive 방향

Negative 방향
```

으로 얼마나 변화시키는지를 분석한다.

---

# 129. Global과 Local

SHAP은:

```text
전체 Pattern
```

뿐 아니라:

```text
한 Patient Prediction
```

도 분석할 수 있다.

---

# 130. Medical Black Box

Machine Learning Model이 높은 성능을 내더라도:

```text
왜 이런 Prediction을 했는지
이해하기 어려운 문제
```

가 있을 수 있다.

이를 흔히 Black Box 문제라고 표현한다.

---

# 131. 이 프로젝트에서 질문

```text
AI가 이 환자를
오분류 위험이라고 판단했다.

왜?
```

라는 질문에 답할 필요가 있다.

---

# 132. 설명 가능성

이를 위해:

```text
Feature Importance

SHAP

Failure Analysis

Patient-level Review
```

등을 사용할 수 있다.

---

# 133. 하지만 Explainability에도 한계가 있다

SHAP이 보여주는 것은:

```text
Model이 어떻게 Prediction했는가?
```

에 가깝다.

---

# 134. SHAP이 증명하지 않는 것

```text
의학적 원인

인과관계

치료 효과
```

를 증명하지 않는다.

---

# 135. Leakage 확인

KTAS 오분류 Prediction에서는 특히 Leakage를 조심해야 한다.

---

# 136. 예

Prediction 시점 이후에만 알 수 있는 정보를 Feature로 넣으면 안 된다.

---

# 137. 잘못된 예

```text
최종 KTAS 결과를 이용하여
초기 KTAS 오분류를 예측
```

하는 식의 구조가 되면 Target 정보를 사실상 Model에 알려주는 문제가 발생할 수 있다.

---

# 138. Prediction 시점

먼저 질문한다.

```text
AI를 언제 사용할 것인가?
```

---

# 139. 예

```text
응급실 초기 분류 직후
오분류 위험을 경고한다.
```

가 목적이라면 그 시점에 실제로 알 수 있는 Feature만 사용해야 한다.

---

# 140. 이것이 중요한 이유

Offline Accuracy가 아무리 높아도 Prediction 시점에 존재하지 않는 Feature를 사용하면 실제 서비스에서는 사용할 수 없다.

---

# 141. Feature Availability

각 Feature마다:

```text
초기 분류 전에 존재?

초기 분류 중에 존재?

최종 진료 후에 존재?
```

를 확인해야 한다.

---

# 142. Target Leakage Checklist

```text
최종 진단 정보가 포함되어 있는가?

최종 KTAS 정보가 포함되어 있는가?

치료 이후 정보가 포함되어 있는가?

Target을 계산하는 데 사용한 Feature가
Input에도 포함되어 있는가?
```

를 확인한다.

---

# 143. 의료 AI에서는 특히 중요

Leakage Model은 실제보다 매우 높은 성능을 보여줄 수 있기 때문이다.

---

# 144. Failure Analysis

Model이 틀린 Patient를 직접 확인한다.

---

# 145. False Negative

실제 오분류인데 Model은:

```text
정상
```

이라고 Prediction한 Patient이다.

---

# 146. 이번 문제에서 중요

오분류 위험 탐지가 목적이라면 False Negative를 특히 자세히 분석한다.

---

# 147. 질문

```text
NRS가 Missing인가?

Vital Sign이 정상 범위인가?

KTAS Feature가 특이한가?

특정 연령대인가?

Probability가 Threshold 바로 아래였는가?
```

등을 확인한다.

---

# 148. False Positive

실제로 정상분류인데 Model은:

```text
오분류 위험
```

이라고 Prediction한 경우이다.

---

# 149. 질문

```text
어떤 Feature 때문에 위험하게 보였는가?

실제로 경계 사례인가?

Threshold가 너무 낮은가?

특정 Feature가 Model을 과도하게 자극하는가?
```

를 확인한다.

---

# 150. Probability Distribution

```python
import matplotlib.pyplot as plt


plt.hist(
    test_prob,
    bins=30
)


plt.xlabel(
    "Misclassification Probability"
)


plt.ylabel(
    "Count"
)


plt.show()
```

---

# 151. Class별 Probability

실제 정상과 오분류의 Probability 분포를 비교하면 Model이 두 Group을 얼마나 구분하는지 확인할 수 있다.

---

# 152. Threshold Line

그래프에 Threshold를 표시할 수도 있다.

```python
plt.axvline(
    threshold,
    linestyle="--"
)
```

---

# 153. Confusion Matrix

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

# 154. 반드시 숫자로 확인

```text
TN

FP

FN

TP
```

를 직접 확인한다.

---

# 155. Classification Report

```python
from sklearn.metrics import (
    classification_report
)


print(
    classification_report(
        y_test,
        pred,
        zero_division=0
    )
)
```

---

# 156. 모델 비교표

예:

```text
Model        Threshold   Precision   Recall   F1

RF           0.19        0.3841      0.5524   0.4531

XGBoost      0.06        낮아질 수 있음  0.9231   ...
```

처럼 비교할 수 있다.

---

# 157. Accuracy도 같이 기록

XGBoost의 경우:

```text
Accuracy
0.4691
```

처럼 Recall 개선의 대가도 같이 보여준다.

---

# 158. 좋은 발표

```text
XGBoost Recall이 92%였습니다.
```

만 말하지 않는다.

---

# 159. 더 좋은 설명

```text
Threshold를 0.06까지 낮추면서
오분류 환자의 Recall은 약 92.31%까지 증가했지만,

정상 환자를 오분류 위험으로 판단하는
False Positive가 크게 증가하면서
Accuracy는 약 46.91%까지 감소하였다.

따라서 실제 적용에서는
Recall과 False Positive 사이의
운영 기준을 추가로 정의해야 한다.
```

라고 설명한다.

---

# 160. 이것이 Threshold Experiment의 의미

단순히:

```text
가장 높은 점수 찾기
```

가 아니다.

---

# 161. 실제 질문

```text
어떤 종류의 Error를
얼마나 허용할 것인가?
```

를 결정하는 과정이다.

---

# 162. 모델이 의사를 대체하는가?

이 프로젝트의 목적은 그렇게 설정하지 않는 것이 적절하다.

---

# 163. 더 현실적인 역할

```text
오분류 위험이 높은 환자를
의료진에게 추가 확인 대상으로 제시
```

하는 보조 시스템으로 생각할 수 있다.

---

# 164. CDSS 관점

```text
Clinical Decision Support System
```

즉 임상 의사결정 지원 시스템 관점이다.

---

# 165. 예

```text
KTAS 평가
↓
AI Risk Score
↓
위험 높음
↓
"재확인 필요"
↓
의료진 최종 판단
```

과 같은 구조이다.

---

# 166. AI Output

단순히:

```text
0 / 1
```

만 보여주기보다:

```text
오분류 위험도

주요 영향 Feature

현재 Vital Sign

재확인 권고
```

등을 보여주는 형태를 생각할 수 있다.

---

# 167. 예시 화면

```text
오분류 위험도
72%

주요 영향 정보
- NRS_pain
- HR
- SBP

결과
재확인 권고
```

---

# 168. 하지만

이러한 화면도 임상 검증과 실제 Workflow 검토 없이 바로 의료 현장에 적용할 수 있는 것은 아니다.

---

# 169. 프로젝트 한계

```text
Sample Size 제한

오분류 Sample 부족

Single Dataset

External Validation 없음

Missing / 이상 문자열

Threshold 의존성

Potential Leakage

Calibration 미검증

실제 임상 Workflow 검증 없음
```

등을 고려할 수 있다.

---

# 170. Probability Calibration

Model이:

```text
0.8
```

이라고 출력했다고 해서 실제로 반드시:

```text
80% 위험
```

이라는 의미는 아니다.

---

# 171. Calibration

Probability 자체가 실제 발생률과 얼마나 잘 일치하는지 평가하는 과정이다.

---

# 172. 향후

```text
Calibration Curve

Brier Score

Platt Scaling

Isotonic Regression
```

등을 검토할 수 있다.

---

# 173. External Validation

다른 병원이나 다른 기간의 Data에서도 성능이 유지되는지 확인하는 것이 중요하다.

---

# 174. 왜?

한 기관 Data에서 학습한 Model은:

```text
환자 구성

장비

업무 방식

KTAS 평가 방식

Data 기록 방식
```

에 의존할 수 있기 때문이다.

---

# 175. 추가 개선 방향

```text
더 많은 오분류 Sample 확보

Class Weight

Sampling 비교

Cross Validation

OOF Threshold Selection

PR-AUC 평가

Probability Calibration

Feature Leakage 재검토

SHAP Error Analysis

External Validation
```

등을 생각할 수 있다.

---

# 176. Multiclass 확장

Binary:

```text
정상
vs
오분류
```

에서 더 나아가:

```text
정상

과소분류

과대분류
```

3-Class Classification으로 만들 수도 있다.

---

# 177. 하지만

과소분류와 과대분류 Sample이 적기 때문에 Class별 Sample Size와 평가 안정성을 먼저 확인해야 한다.

---

# 178. Module 구조

프로젝트를 코드로 분리한다면:

```text
Module 1
Data Integration / Cleaning

Module 2
Modeling / Evaluation
```

처럼 나눌 수 있다.

---

# 179. Module 1

```text
CSV Load

Adult / Pediatric 구분

KTAS Merge

Data Quality Check

Invalid Value Cleaning

Triage Type 생성
```

등을 담당한다.

---

# 180. Module 2

```text
Feature Selection

Train / Test Split

Preprocessing

Random Forest

XGBoost

Threshold

Evaluation

Feature Importance

SHAP
```

등을 담당한다.

---

# 181. 왜 Module을 나눌까?

하나의 Notebook에 모든 코드를 넣으면:

```text
어디서 Data가 바뀌었는지

어디서 Model이 시작되는지

어떤 결과가 어느 단계인지
```

파악하기 어려워질 수 있다.

---

# 182. 좋은 프로젝트 구조

```text
01_data_preparation.ipynb

02_eda.ipynb

03_modeling.ipynb

04_explainability.ipynb
```

처럼 분리하는 것도 방법이다.

---

# 183. Reproducibility

최종 Data를 만들 때:

```text
원본 Row 수

제외된 Row 수

제외 이유

최종 Feature

Target Distribution
```

을 기록한다.

---

# 184. Experiment 기록

예:

```text
Experiment 1
RF / Threshold 0.5

Experiment 2
RF / Threshold 0.19

Experiment 3
XGB / Threshold 0.5

Experiment 4
XGB / Threshold 0.06
```

처럼 기록한다.

---

# 185. 중요한 점

Model 이름만 기록하지 않는다.

Threshold가 다르면 최종 Classification 결과도 크게 달라지기 때문이다.

---

# 186. 프로젝트 발표 흐름

```text
1. 문제 정의

2. KTAS 설명

3. Dataset 구성

4. Patient + KTAS Merge

5. Adult / Pediatric 구조 확인

6. Data Quality

7. 오분류 정의

8. Class Distribution

9. Baseline

10. Random Forest

11. Threshold 조정

12. XGBoost

13. Recall / Precision Trade-off

14. Feature Importance

15. SHAP

16. Failure Analysis

17. Black Box / CDSS

18. 한계 및 개선 방향
```

---

# 187. 핵심 숫자

```text
전체 Patient
1,807

성인
1,267

소아청소년
540

KTAS Data
1,267

최종 Modeling Data
1,212

Train
969

Test
243

Test 오분류
26

Test 오분류 비율
약 10.7%
```

---

# 188. RF 결과

```text
Threshold
0.19

Precision
0.3841

Recall
0.5524

F1
0.4531
```

---

# 189. XGBoost 결과

```text
Threshold
0.06

Accuracy
0.4691

Recall
0.9231
```

---

# 190. Data Quality 이슈

```text
NRS_pain
"#BOŢ!"
약 556건

SBP
"??"
약 25건
```

---

# 191. RF Feature Importance 상위

```text
NRS_pain

HR

SBP

KTAS_RN_4

BT
```

---

# 192. SHAP 상위 Feature

```text
NRS_pain

HR

SBP
```

등이 중요하게 나타났다.

---

# 193. 이 프로젝트의 가장 중요한 발견

```text
Recall을 높이는 것은 가능했지만
그 과정에서 False Positive가 크게 증가했다.
```

---

# 194. 따라서

```text
Recall이 가장 높은 Model
=
무조건 가장 좋은 Model
```

이 아니다.

---

# 195. 프로젝트 목적에 따라

```text
Recall

Precision

F1

False Positive Rate

PR-AUC

운영 가능한 Alert 수
```

를 함께 고려해야 한다.

---

# 196. 프로젝트에서 배운 점 1

Data Merge 후 반드시:

```text
Row 수

Cohort

연결 여부

핵심 Column 일치율
```

을 검증한다.

---

# 197. 배운 점 2

Missing은 모두 같은 Missing이 아니다.

```text
구조적으로 존재하지 않는 Data
```

가 있을 수 있다.

---

# 198. 배운 점 3

이상한 문자열을 발견하면:

```text
pd.to_numeric(
    errors="coerce"
)
```

를 사용할 수 있다.

하지만 변환 후 Missing 처리까지 생각해야 한다.

---

# 199. 배운 점 4

Imbalanced Classification에서는 Accuracy만 보면 안 된다.

---

# 200. 배운 점 5

Threshold를 바꾸면 같은 Model에서도:

```text
Precision

Recall

F1

Accuracy
```

가 크게 달라질 수 있다.

---

# 201. 배운 점 6

Threshold는 Test Set에 맞추지 않는다.

Validation 또는 OOF Prediction을 사용한다.

---

# 202. 배운 점 7

Feature Importance와 SHAP은 Model을 이해하는 데 도움을 주지만 인과관계를 증명하지 않는다.

---

# 203. 배운 점 8

의료 AI에서는:

```text
Model Performance
```

뿐 아니라:

```text
왜 그런 Prediction을 했는가?
```

도 중요하다.

---

# 204. 배운 점 9

더 높은 Recall을 얻었다고 끝이 아니다.

```text
False Positive가 얼마나 늘어났는가?
```

를 확인한다.

---

# 205. 배운 점 10

실제 서비스에서는 AI를:

```text
최종 판단자
```

보다:

```text
위험 사례 재확인을 돕는 보조 도구
```

로 설계하는 관점이 중요하다.

---

# 206. 전체 Workflow

```text
Patient Profile
1,807
+
KTAS Data
1,267
↓
Merge
↓
Adult 1,267
Pediatric 540
↓
KTAS 연결 검증
↓
Data Cleaning
↓
NRS_pain / SBP 이상값 처리
↓
정상 / 과소 / 과대 분류
↓
Binary Misclassification Target
↓
Modeling Data 1,212
↓
Train 969
Test 243
↓
Random Forest
↓
Threshold 0.19
↓
Recall 0.5524
F1 0.4531
↓
XGBoost
↓
Threshold 0.06
↓
Recall 0.9231
Accuracy 0.4691
↓
Precision / Recall Trade-off
↓
Feature Importance
↓
SHAP
↓
Failure Analysis
↓
CDSS 적용 가능성 검토
```

---

# 207. 한 문장으로 설명

이 프로젝트는:

```text
응급환자의 환자정보와 KTAS Data를 통합하여
초기 중증도 분류의 오분류 Pattern을 분석하고,

Random Forest와 XGBoost를 이용해
오분류 위험을 예측하면서

Threshold 조정을 통해
Recall과 False Positive 사이의 Trade-off를 확인하고,

Feature Importance와 SHAP을 이용하여
Model Prediction을 해석한
응급의료 Classification 프로젝트
```

이다.

---

# 208. 가장 중요한 교훈

이 프로젝트에서 중요한 것은:

```text
Recall 92%가 나왔다.
```

라는 숫자 하나가 아니다.

핵심은:

```text
오분류를 더 많이 찾고 싶다.
↓
Threshold를 낮춘다.
↓
Recall이 올라간다.
↓
하지만 False Positive도 증가한다.
↓
Accuracy와 Precision이 떨어질 수 있다.
↓
그렇다면 실제 의료 현장에서
어느 수준의 경고를 허용할 것인가?
```

라는 질문까지 연결하는 것이다.

즉 Machine Learning의 목표는 단순히:

```text
가장 높은 점수
```

를 만드는 것이 아니라:

```text
프로젝트 목적에 맞는 Error의 균형을 찾는 것
```

이다.