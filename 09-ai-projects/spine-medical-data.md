# Spine Medical Data Analysis

## 1. 프로젝트 개요

이 프로젝트는 추간판 탈출증(Herniated Disc) 관련 의료 데이터를 이용하여 환자 특성을 분석하고, 수술 결과와 관련된 Pattern을 탐색하며, 수술실패여부를 예측하는 Machine Learning 프로젝트이다.

전체적인 구조는 다음과 같다.

```text
Patient Data
+
Diagnosis Data
+
MRI Image Information
↓
Data Integration
↓
EDA / Statistics
↓
Feature Engineering
↓
Preprocessing
↓
Machine Learning
↓
수술실패여부 예측
```

---

# 2. 프로젝트의 특징

이 프로젝트는 하나의 CSV만 사용하는 문제가 아니었다.

여러 종류의 Data가 존재했다.

```text
환자 기본정보

진단정보

수술정보

MRI Image 정보
```

따라서 가장 먼저 해야 할 일은:

```text
각 Data가 무엇을 의미하는지 이해하고
서로 올바르게 연결하는 것
```

이었다.

---

# 3. 주요 파일

프로젝트에서 사용한 주요 CSV는 다음과 같았다.

```text
02_Patient_Data.csv

02_Patient_Diagnosis.csv

02_Disc_image.csv
```

그리고 MRI Image 파일도 별도로 존재했다.

예:

```text
images/
├── image_001.png
├── image_002.png
├── ...
```

---

# 4. Patient Data

환자의 기본적인 정보가 포함되어 있다.

예:

```text
성별

연령

체중

신장

직업

가족력

고혈압

당뇨
```

등이다.

---

# 5. Diagnosis Data

진단 및 치료와 관련된 정보가 포함될 수 있다.

예:

```text
통증 정도

통증 기간

ODI

헤모글로빈 수치

수술 기법

수술 시간

입원 기간

수술실패여부

재발여부
```

등이다.

---

# 6. Image Data

MRI Image 관련 정보도 존재했다.

이 프로젝트에서는 Tabular Data 분석뿐 아니라 이후 Computer Vision 분석과 연결할 수 있는 구조였다.

```text
Patient
↓
Diagnosis
↓
MRI
```

처럼 생각할 수 있다.

---

# 7. 첫 번째 단계

파일을 읽는다.

```python
import pandas as pd
import numpy as np


patient = pd.read_csv(
    "/content/herniated_data/02_Patient_Data.csv"
)


diagnosis = pd.read_csv(
    "/content/herniated_data/02_Patient_Diagnosis.csv"
)


disc_image = pd.read_csv(
    "/content/herniated_data/02_Disc_image.csv"
)
```

---

# 8. Data 확인

```python
print(
    "patient:",
    patient.shape
)


print(
    "diagnosis:",
    diagnosis.shape
)


print(
    "disc_image:",
    disc_image.shape
)
```

---

# 9. Column 확인

```python
print(
    patient.columns.tolist()
)


print(
    diagnosis.columns.tolist()
)


print(
    disc_image.columns.tolist()
)
```

---

# 10. 왜 먼저 확인할까?

Merge를 하기 전에:

```text
공통 Key가 무엇인가?

한 Patient당 몇 Row인가?

중복 Key가 있는가?

Column 이름이 같은가?
```

를 알아야 하기 때문이다.

---

# 11. Data Integration

여러 DataFrame을 하나의 분석용 DataFrame으로 연결한다.

예:

```python
merged = pd.merge(
    patient,
    diagnosis,
    on="환자ID",
    how="inner"
)
```

실제 Join Key는 Dataset의 Column 이름에 맞춰 사용한다.

---

# 12. Image 정보 연결

필요하다면:

```python
merged = pd.merge(
    merged,
    disc_image,
    on="환자ID",
    how="left"
)
```

처럼 Image 관련 Metadata도 연결할 수 있다.

---

# 13. Merge 후 반드시 확인

```python
print(
    merged.shape
)
```

---

# 14. 왜 중요할까?

Merge 전에는:

```text
216명
```

이었는데 Merge 후:

```text
500 Row
```

가 되었다면 한 Patient가 여러 Image 또는 여러 Record와 연결되면서 Row가 증가했을 가능성이 있다.

---

# 15. 1:N 관계

예:

```text
Patient 1명
↓
MRI Image 여러 장
```

이면:

```text
1 : N
```

관계이다.

---

# 16. Row 증가가 오류인가?

반드시 오류는 아니다.

하지만 분석 단위가:

```text
Patient
```

인지:

```text
Image
```

인지 반드시 구분해야 한다.

---

# 17. 분석용 통합 Data

당시 Tabular 분석에서는 통합 후 약:

```text
216 Rows

36 Columns
```

규모의 Data를 사용했다.

---

# 18. 기본 확인

```python
merged.info()
```

```python
display(
    merged.head()
)
```

```python
display(
    merged.describe(
        include="all"
    )
)
```

---

# 19. Missing Value

```python
missing = (
    merged.isnull().sum()
)


missing = missing[
    missing > 0
]


display(
    missing.sort_values(
        ascending=False
    )
)
```

---

# 20. 의료 Data의 Missing

Missing이 있다고 무조건 삭제하지 않는다.

먼저:

```text
측정하지 않은 것인가?

해당 검사가 필요하지 않았던 것인가?

기록 누락인가?

특정 환자군에서만 발생하는가?
```

를 확인한다.

---

# 21. 주요 Feature

분석에 사용한 주요 Feature에는 다음과 같은 정보가 있었다.

```text
성별

연령

체중

신장

ODI

헤모글로빈수치

환자통증정도

통증기간(월)

혈액형

직업

가족력

고혈압

당뇨

입원기간

수술기법

수술시간

수술실패여부

재발여부

수술일
```

---

# 22. BMI Feature Engineering

체중과 신장을 이용해 BMI를 만들 수 있다.

공식:

```text
BMI
=
Weight(kg)
/
Height(m)²
```

---

# 23. Python

예:

```python
merged[
    "BMI"
] = (
    merged[
        "체중"
    ]
    /
    (
        merged[
            "신장"
        ]
        /
        100
    ) ** 2
)
```

---

# 24. 왜 BMI를 만들까?

체중과 신장을 각각 사용하는 것과:

```text
체중 대비 신체 크기
```

를 하나의 지표로 표현하는 것은 다른 정보를 제공할 수 있기 때문이다.

---

# 25. Feature Engineering 원칙

새 Feature를 만들었다고 바로 유용하다고 판단하지 않는다.

```text
Domain 의미

Distribution

Target과 관계

Model 성능 변화
```

를 확인한다.

---

# 26. 성별 분석

예를 들어 성별에 따라 특정 Numeric Feature가 차이가 있는지 확인할 수 있다.

```python
male = merged.loc[
    merged[
        "성별"
    ] == 1
]


female = merged.loc[
    merged[
        "성별"
    ] != 1
]
```

실제 성별 Coding은 Data Dictionary를 기준으로 확인한다.

---

# 27. 중요한 주의

```text
1 = 남성
```

이라고 무조건 추측하면 안 된다.

반드시 Dataset 정의를 확인한다.

---

# 28. Distribution 확인

```python
import seaborn as sns
import matplotlib.pyplot as plt


sns.boxplot(
    data=merged,
    x="성별",
    y="통증기간(월)"
)


plt.show()
```

---

# 29. 통계 검정

Group 간 차이를 확인하기 전에 Distribution을 확인한다.

---

# 30. Shapiro-Wilk

```python
from scipy.stats import (
    shapiro
)


stat, p = shapiro(
    male[
        "통증기간(월)"
    ].dropna()
)
```

---

# 31. Normality가 만족되지 않는다면

두 독립 Group의 분포를 비교할 때 상황에 따라:

```text
Mann-Whitney U
```

를 사용할 수 있다.

---

# 32. Mann-Whitney U

```python
from scipy.stats import (
    mannwhitneyu
)


stat, p = mannwhitneyu(
    male[
        "통증기간(월)"
    ].dropna(),

    female[
        "통증기간(월)"
    ].dropna(),

    alternative="two-sided"
)
```

---

# 33. 실제 분석에서

성별에 따라:

```text
통증기간

헤모글로빈수치
```

등에서 통계적으로 유의한 차이가 관찰되었다.

---

# 34. 하지만 주의

```text
p < 0.05
```

라고 해서:

```text
성별이 해당 값 차이의 원인이다.
```

라고 결론 내릴 수는 없다.

---

# 35. 통계적 유의성과 임상적 중요성

둘은 다르다.

```text
Statistical Significance
≠
Clinical Significance
```

이다.

---

# 36. 직업 분석

Categorical Feature의 분포도 확인할 수 있다.

```python
merged[
    "직업"
].value_counts()
```

---

# 37. 당시 분석

직업 Category 중:

```text
무직
```

이 가장 많이 관찰되었고 약:

```text
51명
```

이었다.

---

# 38. 여기서 주의

```text
무직이 많다
```

는 것은 Dataset의 구성에 대한 설명이다.

이것만으로 질환과의 인과관계를 의미하지 않는다.

---

# 39. 재발여부

재발 환자의 특성을 별도로 분석할 수 있다.

```python
recurrence = merged.loc[
    merged[
        "재발여부"
    ] == 1
]
```

---

# 40. 재발 환자 수

당시 분석에서는 재발 환자가 약:

```text
36명
```

이었다.

---

# 41. 재발 환자 평균 연령

약:

```text
59.09세
```

로 계산되었다.

---

# 42. 재발 환자 평균 BMI

약:

```text
29.11
```

로 계산되었다.

---

# 43. 이것의 의미

이 값은:

```text
재발 환자 Group의 기술통계
```

이다.

---

# 44. 잘못된 해석

```text
59세가 되면 재발한다.

BMI 29면 재발한다.
```

라고 해석하면 안 된다.

---

# 45. 필요한 추가 분석

재발과 Feature 관계를 보려면:

```text
재발군 vs 비재발군 비교

Sample Size

Confounder

Statistical Test

Predictive Model
```

등을 추가로 고려해야 한다.

---

# 46. Correlation 분석

통증기간과 환자통증정도 사이의 관계를 확인할 수 있다.

---

# 47. 왜 Pearson만 사용하지 않을까?

Data가 정규성을 만족하지 않거나 순위 관계를 보고 싶다면:

```text
Spearman Correlation
```

을 사용할 수 있다.

---

# 48. Spearman

```python
from scipy.stats import (
    spearmanr
)


corr, p = spearmanr(
    merged[
        "통증기간(월)"
    ],

    merged[
        "환자통증정도"
    ],

    nan_policy="omit"
)
```

---

# 49. 당시 결과

통증기간과 환자통증정도의 관계는:

```text
통계적으로 유의한 관계를 확인하지 못함
```

으로 해석되었다.

---

# 50. 중요한 표현

```text
관계가 없다.
```

라고 단정하는 것보다:

```text
현재 Dataset에서는 통계적으로 유의한 관계를 확인하지 못했다.
```

라고 표현하는 것이 더 정확하다.

---

# 51. 이제 Prediction

EDA와 통계 분석 이후:

```text
수술실패여부
```

를 Target으로 Machine Learning Classification을 진행했다.

---

# 52. 문제 정의

```text
Input
→ 환자 / 진단 / 수술 관련 Feature

Target
→ 수술실패여부

Task
→ Binary Classification
```

이다.

---

# 53. Target Distribution

가장 먼저:

```python
merged[
    "수술실패여부"
].value_counts()
```

를 확인한다.

---

# 54. 당시 Target

대략:

```text
0 = 201

1 = 15
```

이었다.

---

# 55. 비율

전체 216명 중 수술실패는 약:

```text
6.94%
```

였다.

---

# 56. 여기서 가장 중요한 문제

심한:

```text
Class Imbalance
```

이다.

---

# 57. Accuracy 함정

만약 Model이 모든 환자를:

```text
수술 성공
```

이라고 Prediction한다면:

```text
201 / 216
≈
93.1%
```

의 Accuracy가 나온다.

---

# 58. 그런데 이 Model은 좋은가?

아니다.

수술실패 환자:

```text
15명
```

을 단 한 명도 찾지 못한다.

---

# 59. 그래서 Accuracy만 보면 안 된다

특히 다음을 함께 본다.

```text
Precision

Recall

F1

ROC-AUC

PR-AUC

Confusion Matrix
```

---

# 60. Train / Test Split

당시:

```text
80 : 20
```

정도로 분리하여 Model을 평가했다.

---

# 61. Stratify

불균형 Classification에서는 가능하면:

```python
stratify=y
```

를 사용해 Class 비율을 유지한다.

---

# 62. 예

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

---

# 63. 하지만 Sample 수가 매우 적다

수술실패 환자가 15명뿐이라면 20% Test에는 Positive Sample이 매우 적게 들어간다.

---

# 64. 예

대략:

```text
Test Positive
≈ 3명
```

정도일 수 있다.

---

# 65. 왜 문제?

Positive 1명을 더 맞히거나 틀리는 것만으로 Recall이 크게 바뀔 수 있다.

---

# 66. 따라서

한 번의 Train/Test Split 결과만으로 Model을 강하게 평가하기 어렵다.

---

# 67. Cross Validation

가능하면:

```text
Stratified Cross Validation
```

등을 함께 고려한다.

---

# 68. Logistic Regression

Baseline Model 중 하나로 Logistic Regression을 사용할 수 있다.

```python
from sklearn.linear_model import (
    LogisticRegression
)


model = LogisticRegression(
    max_iter=1000,
    random_state=42
)
```

---

# 69. Scaling

Logistic Regression에서는 Numeric Feature의 Scale을 맞추는 것이 도움이 될 수 있다.

```python
from sklearn.preprocessing import (
    StandardScaler
)
```

---

# 70. Pipeline

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
            LogisticRegression(
                max_iter=1000,
                random_state=42
            )
        )
    ]
)
```

실제로는 Missing/Categorical 처리도 함께 구성해야 한다.

---

# 71. 당시 Logistic Regression 결과

대략:

```text
Accuracy
0.9318
```

이었다.

---

# 72. 숫자만 보면

```text
93.18%
```

이므로 상당히 좋아 보인다.

---

# 73. 하지만 Confusion Matrix

Positive Class에 대해:

```text
TP = 0
```

인 결과가 발생했다.

---

# 74. 이것의 의미

Model이 실제 수술실패 환자를:

```text
한 명도 제대로 찾지 못했다.
```

는 뜻이다.

---

# 75. Accuracy가 높은 이유

Majority Class인:

```text
수술 성공
```

을 대부분 맞혔기 때문이다.

---

# 76. 이것이 Class Imbalance의 핵심 문제

```text
Accuracy 높음
≠
Minority Class를 잘 찾음
```

이다.

---

# 77. ROC-AUC

당시 Logistic Regression의 ROC-AUC는 약:

```text
0.7927
```

수준이었다.

---

# 78. Accuracy와 ROC-AUC가 다른 이유

Accuracy는 특정 Threshold에서 만들어진 최종 Class를 평가한다.

ROC-AUC는 여러 Threshold에 걸친 Ranking 능력을 평가한다.

---

# 79. 그래서 가능한 상황

```text
ROC-AUC는 어느 정도 높음

하지만 Threshold 0.5에서는
Positive Prediction이 없음
```

같은 상황이 가능하다.

---

# 80. Threshold

기본적으로:

```text
Probability >= 0.5
→ Class 1
```

을 많이 사용한다.

---

# 81. 하지만 0.5가 항상 최적인가?

아니다.

특히 Imbalanced Dataset에서는 Validation Data를 이용해 Threshold를 조정할 수 있다.

---

# 82. 예

```python
prob = model.predict_proba(
    X_test
)[
    :,
    1
]


threshold = 0.3


pred = (
    prob >= threshold
).astype(
    int
)
```

---

# 83. Threshold를 낮추면

일반적으로:

```text
Recall ↑ 가능

TP ↑ 가능

FP ↑ 가능
```

하다.

---

# 84. Trade-off

수술실패 환자를 더 많이 찾는 대신 성공 환자를 실패로 잘못 분류하는 경우가 증가할 수 있다.

---

# 85. Threshold는 Test에서 맞추지 않는다

Test 결과를 보고 가장 좋은 Threshold를 찾으면 Test에 Overfitting한다.

Validation 또는 Cross Validation을 사용한다.

---

# 86. 다른 Model

Class Imbalance 문제를 개선하기 위해 다른 Model 또는 전략을 실험할 수 있다.

예:

```text
Random Forest

Gradient Boosting

XGBoost

Class Weight

Sampling

Threshold Tuning
```

등이다.

---

# 87. 다른 실험 결과

한 대안 Model에서는 대략:

```text
Accuracy
0.7501

Recall
0.3333
```

수준의 결과가 관찰되었다.

---

# 88. Accuracy는 낮아졌다

```text
0.9318
→
0.7501
```

이다.

---

# 89. 하지만 Recall

기존 Model에서 수술실패를 하나도 찾지 못했다면 대안 Model은 일부 Positive 환자를 찾을 수 있게 된 것이다.

---

# 90. 어떤 Model이 더 좋은가?

Accuracy만 보면 첫 번째 Model이 좋아 보인다.

하지만 프로젝트 목적이:

```text
수술실패 가능성이 있는 환자를 찾는 것
```

이라면 Recall 개선도 중요한 의미를 가진다.

---

# 91. 여기서 핵심

```text
Metric은 프로젝트 목적에 맞게 선택한다.
```

이다.

---

# 92. Confusion Matrix

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

# 93. 구조

```text
TN  FP

FN  TP
```

이다.

---

# 94. 의료 AI에서 질문

단순히:

```text
Accuracy가 몇 %인가?
```

보다:

```text
실패 환자 몇 명을 놓쳤는가?

실패 환자 몇 명을 찾았는가?

성공 환자를 몇 명 잘못 경고했는가?
```

를 보는 것이 중요하다.

---

# 95. Classification Report

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

# 96. UndefinedMetricWarning

Positive Prediction이 하나도 없으면 Precision 계산 과정에서:

```text
UndefinedMetricWarning
```

이 발생할 수 있다.

---

# 97. 왜?

Precision:

```text
TP
/
(TP + FP)
```

인데:

```text
TP + FP = 0
```

이면 분모가 0이 되기 때문이다.

---

# 98. 중요한 점

Warning을 없애는 것보다:

```text
왜 Positive Prediction이 하나도 없는가?
```

를 분석하는 것이 먼저이다.

---

# 99. zero_division

출력 Warning을 제어하기 위해:

```python
classification_report(
    y_test,
    pred,
    zero_division=0
)
```

을 사용할 수 있다.

---

# 100. 하지만

이것은 Model 성능 문제를 해결한 것이 아니다.

단지 Metric 계산 시 처리 방식을 지정한 것이다.

---

# 101. Class Weight

Logistic Regression에서는:

```python
LogisticRegression(
    class_weight="balanced"
)
```

를 실험할 수 있다.

---

# 102. 의미

Minority Class의 Error에 더 큰 영향을 주도록 Model 학습을 조정한다.

---

# 103. Random Forest

```python
from sklearn.ensemble import (
    RandomForestClassifier
)


rf = RandomForestClassifier(
    n_estimators=300,
    class_weight="balanced",
    random_state=42
)
```

처럼 실험할 수도 있다.

---

# 104. Sampling

Training Data에:

```text
Oversampling

SMOTE
```

등을 적용하는 방법도 생각할 수 있다.

---

# 105. 하지만

Positive Sample이 매우 적기 때문에 Synthetic Sample을 만드는 것이 항상 안전하거나 효과적인 것은 아니다.

---

# 106. 의료 Data에서 특히

Synthetic Data가 실제 임상적으로 가능한 조합인지도 생각해야 한다.

---

# 107. Sampling 원칙

```text
Train에만 적용

Validation / Test에는 적용하지 않음
```

이다.

---

# 108. Cross Validation에서도

```text
Training Fold
→ Sampling

Validation Fold
→ 원본 유지
```

해야 한다.

---

# 109. PR-AUC

심한 Class Imbalance에서는 ROC-AUC와 함께:

```text
Precision-Recall Curve

PR-AUC
```

도 유용할 수 있다.

---

# 110. 왜?

Minority Positive Class의:

```text
Precision

Recall
```

Trade-off를 직접 보여주기 때문이다.

---

# 111. Baseline 비교

좋은 실험 구조:

```text
Baseline
Logistic Regression

↓

Experiment A
class_weight="balanced"

↓

Experiment B
Random Forest

↓

Experiment C
Sampling

↓

Experiment D
Threshold Tuning
```

이다.

---

# 112. 같은 조건 유지

실험 비교에서는 가능하면:

```text
같은 Split

같은 Feature

같은 Metric
```

을 사용한다.

---

# 113. 그래야

```text
무엇 때문에 성능이 바뀌었는가?
```

를 설명하기 쉽다.

---

# 114. Feature Importance

Tree Model을 사용하면:

```python
rf.feature_importances_
```

를 통해 Model이 많이 사용한 Feature를 탐색할 수 있다.

---

# 115. SHAP

더 자세한 분석을 위해 SHAP을 사용할 수도 있다.

```text
전체적으로 중요한 Feature

개별 환자 Prediction에 기여한 Feature
```

를 탐색할 수 있다.

---

# 116. 하지만

```text
Feature Importance가 높다
=
수술실패의 원인이다
```

라고 해석하지 않는다.

---

# 117. MRI Image와 연결

이 Dataset에는 MRI Image도 존재했기 때문에 향후:

```text
Tabular Model
+
Image Model
```

을 결합하는 방향도 생각할 수 있다.

---

# 118. Image Classification

예:

```text
MRI Middle Slice
↓
CNN
↓
Disc Grade Prediction
```

과 같은 Computer Vision Task를 만들 수 있다.

---

# 119. Tabular Input

```text
Age

BMI

Pain

Diagnosis

Surgery Information
```

등을 사용할 수 있다.

---

# 120. Image Input

```text
MRI
```

를 CNN에 입력한다.

---

# 121. Multimodal AI

두 종류의 Data를 함께 사용하는 것을:

```text
Multimodal
```

관점에서 생각할 수 있다.

---

# 122. 예

```text
MRI
↓
CNN
↓
Image Feature
             ┐
             ├→ Combined Model
             │
Patient Data ┘
↓
Tabular Feature
```

---

# 123. 단순한 방법

Image Model Prediction과 Tabular Model Prediction을 결합하는 Late Fusion도 생각할 수 있다.

예:

```text
MRI Probability
+
Tabular Probability
↓
Final Probability
```

---

# 124. 더 복잡한 방법

CNN에서 추출한 Feature Vector와 Tabular Feature를 연결한 후 Neural Network에 넣을 수도 있다.

---

# 125. 하지만 먼저

복잡한 Multimodal Model보다:

```text
Tabular Baseline

Image Baseline
```

을 각각 안정적으로 만드는 것이 좋다.

---

# 126. MRI Image 경로

Image Project에서는 경로 문제도 자주 발생한다.

예:

```text
/content/data/images/

vs

/content/herniated_data/images/
```

---

# 127. FileNotFoundError

Image를 찾지 못하면 먼저:

```python
import os


print(
    os.getcwd()
)


print(
    os.listdir(
        "/content"
    )
)
```

을 확인한다.

---

# 128. Folder 확인

```python
print(
    os.listdir(
        "/content/herniated_data"
    )
)
```

---

# 129. Image 개수

```python
image_dir = (
    "/content/herniated_data/images"
)


image_files = os.listdir(
    image_dir
)


print(
    len(
        image_files
    )
)
```

---

# 130. 중요한 교훈

경로 Error가 발생하면 Model Code를 수정하는 것이 아니라:

```text
실제 파일 위치
```

부터 확인한다.

---

# 131. ModuleNotFoundError

프로젝트 중:

```text
skopt
```

가 설치되어 있지 않은 문제도 발생할 수 있다.

---

# 132. 설치

```python
!pip install scikit-optimize
```

---

# 133. Import

```python
from skopt import (
    BayesSearchCV
)
```

---

# 134. Package 이름과 Import 이름

```text
설치 이름
scikit-optimize

Import 이름
skopt
```

처럼 다를 수 있다.

---

# 135. BayesSearchCV

Hyperparameter Tuning을 위한 Bayesian Search 방법 중 하나이다.

---

# 136. 하지만

Data가 매우 작고 Positive Sample도 매우 적다면 Hyperparameter를 정교하게 Tuning하는 것보다:

```text
Validation Strategy

Class Imbalance

Feature Quality
```

를 먼저 해결하는 것이 중요할 수 있다.

---

# 137. 작은 Dataset 문제

전체:

```text
216명
```

수준이면 복잡한 Model이 쉽게 Overfitting할 수 있다.

---

# 138. 특히 Positive

```text
15명
```

밖에 없다면 Model이 학습할 수 있는 실패 Pattern 자체가 매우 제한적이다.

---

# 139. 따라서 결과 해석

```text
이 Model은 수술실패를 정확하게 예측한다.
```

라고 강하게 주장하면 안 된다.

---

# 140. 더 적절한 표현

```text
현재 Dataset에서 수술실패여부 예측 가능성을 탐색하였다.
```

정도로 표현하는 것이 더 적절하다.

---

# 141. Data 규모와 Model 복잡도

Data가 작다고 항상 단순 Model만 사용해야 하는 것은 아니지만:

```text
복잡한 Model
+
작은 Dataset
```

에서는 Overfitting 위험을 더 주의해야 한다.

---

# 142. Repeated Validation

가능하다면:

```text
Repeated Stratified K-Fold
```

등을 이용하여 Split 변화에 따른 성능 변동을 확인하는 방법도 생각할 수 있다.

---

# 143. Confidence Interval

Sample이 적은 의료 Data에서는 Metric 하나만 제시하기보다 가능하다면 불확실성도 함께 평가하는 것이 좋다.

---

# 144. Project Presentation

발표에서는 다음 순서로 정리할 수 있다.

```text
1. 문제 정의

2. Dataset 구조

3. Data Merge

4. EDA

5. BMI Feature Engineering

6. Statistical Analysis

7. Target Imbalance

8. Baseline Model

9. Accuracy 문제 발견

10. Recall / ROC-AUC 분석

11. Imbalance 대응 실험

12. MRI 연계 가능성

13. 한계

14. 개선 방향
```

---

# 145. 발표에서 중요한 부분

단순히:

```text
Accuracy = 93.18%
```

이라고 말하면 Model이 매우 좋아 보인다.

---

# 146. 더 정확한 설명

```text
Logistic Regression의 Accuracy는
약 93.18%였지만,

전체 216명 중 수술실패 환자가
15명뿐인 심한 Class Imbalance가 존재했고,

Confusion Matrix 확인 결과
실패 환자를 한 명도 탐지하지 못했다.

따라서 Accuracy만으로
Model을 평가할 수 없다고 판단했다.
```

라고 설명한다.

---

# 147. 이것이 프로젝트의 중요한 발견

```text
높은 Accuracy
```

가 반드시:

```text
좋은 Model
```

을 의미하지 않는다는 것을 실제 Data를 통해 확인했다.

---

# 148. 다음 실험 설명

```text
Minority Class 탐지 성능을 개선하기 위해
다른 Model과 Imbalance 대응 전략을 비교하였다.
```

라고 이어갈 수 있다.

---

# 149. Recall 개선

대안 Model에서:

```text
Recall ≈ 0.3333
```

이 나왔다면 실패 환자 일부를 탐지하게 된 것이다.

---

# 150. Trade-off

대신 Accuracy가:

```text
약 0.75
```

수준으로 감소했다.

---

# 151. 여기서 얻은 교훈

```text
Accuracy를 높이는 것

vs

중요한 Positive 환자를 찾는 것
```

사이에 Trade-off가 존재할 수 있다.

---

# 152. 의료 AI에서 Metric 선택

Model 목적에 따라:

```text
Sensitivity

Specificity

Precision

F1

ROC-AUC

PR-AUC
```

를 함께 고려한다.

---

# 153. 프로젝트 한계

이 프로젝트의 중요한 한계는 다음과 같다.

```text
전체 Sample 수가 작음

수술실패 Sample이 매우 적음

Single Dataset 분석

External Validation 없음

Tabular와 MRI를 완전히 통합하지 않음

일부 Feature의 Missing 가능성

의료적 인과관계를 증명할 수 없음
```

---

# 154. 개선 방향

```text
더 많은 Patient Data 확보

External Validation

Repeated Cross Validation

Class Weight 비교

Threshold Optimization

PR-AUC 평가

Calibration

Feature Selection

SHAP Error Analysis

MRI CNN Model 개발

Tabular + MRI Multimodal Model
```

등을 생각할 수 있다.

---

# 155. 전체 Workflow

```text
Patient CSV
+
Diagnosis CSV
+
Image Metadata
↓
Merge
↓
Data Structure 확인
↓
Missing 확인
↓
EDA
↓
BMI Feature Engineering
↓
Statistical Analysis
↓
Target 설정
수술실패여부
↓
Class Distribution
201 : 15
↓
Train / Test Split
↓
Logistic Regression
↓
Accuracy 0.9318
↓
Confusion Matrix
↓
TP = 0 발견
↓
Accuracy 함정 확인
↓
다른 Model / Imbalance 대응
↓
Recall 개선 가능성 확인
↓
ROC-AUC / F1 / PR-AUC 고려
↓
MRI 연계 가능성
```

---

# 156. 프로젝트 핵심 숫자

```text
통합 Data
216 × 36

수술 성공
201

수술 실패
15

수술 실패 비율
약 6.94%

Logistic Regression Accuracy
약 0.9318

Logistic Regression ROC-AUC
약 0.7927

Logistic Regression TP
0

대안 Model Accuracy
약 0.7501

대안 Model Recall
약 0.3333

재발 환자
36

재발 환자 평균 연령
약 59.09

재발 환자 평균 BMI
약 29.11
```

---

# 157. 이 숫자에서 가장 중요한 것

```text
Accuracy 0.9318
```

보다:

```text
TP = 0
```

가 더 중요한 발견이었다.

---

# 158. 왜?

Model의 목적이 수술실패 위험을 찾는 것이라면 실패 환자를 전혀 찾지 못하는 Model은 높은 Accuracy에도 불구하고 목적을 제대로 달성하지 못했기 때문이다.

---

# 159. 프로젝트에서 배운 점 1

여러 CSV를 사용할 때는:

```text
Merge부터 정확하게
```

해야 한다.

---

# 160. 배운 점 2

Merge 후:

```text
Row 수

중복 Key

1:N 관계
```

를 반드시 확인한다.

---

# 161. 배운 점 3

의료 Data의 통계 결과는:

```text
Association
```

과:

```text
Causality
```

를 구분해서 해석한다.

---

# 162. 배운 점 4

Feature Engineering은:

```text
체중 + 신장
→ BMI
```

처럼 Domain 의미를 반영할 수 있다.

---

# 163. 배운 점 5

Imbalanced Classification에서:

```text
Accuracy만 보면 위험하다.
```

---

# 164. 배운 점 6

반드시:

```text
Confusion Matrix
```

를 확인한다.

---

# 165. 배운 점 7

특히 Positive Class가 중요하다면:

```text
Recall

PR-AUC

Threshold
```

등을 함께 본다.

---

# 166. 배운 점 8

Sample 수가 작으면 Metric이 Split에 따라 크게 달라질 수 있다.

---

# 167. 배운 점 9

Hyperparameter Tuning보다 먼저:

```text
Data Quality

Validation

Class Imbalance
```

를 확인한다.

---

# 168. 배운 점 10

의료영상이 있다고 처음부터 복잡한 Multimodal Model을 만들 필요는 없다.

먼저:

```text
Tabular Baseline

Image Baseline
```

을 각각 만든다.

---

# 169. 한 문장으로 설명

이 프로젝트는:

```text
추간판 탈출증 환자의
기본정보·진단정보·수술정보를 통합하여
환자 특성과 재발 관련 Pattern을 분석하고,

수술실패여부를 예측하는
Binary Classification Model을 구축하면서

심한 Class Imbalance 상황에서는
높은 Accuracy만으로 Model을 평가할 수 없다는 점을
확인한 프로젝트
```

이다.

---

# 170. 가장 중요한 교훈

```text
Accuracy가 높다.
```

에서 멈추지 않는다.

반드시:

```text
누구를 맞혔는가?

누구를 놓쳤는가?

Minority Class를 찾았는가?

Confusion Matrix는 어떤가?

이 Metric이 Project 목적에 맞는가?
```

까지 확인한다.