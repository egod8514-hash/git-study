# Infertility Pregnancy Success Prediction

## 1. 프로젝트 개요

이 프로젝트의 목적은 난임 시술 관련 데이터를 이용하여 임신 성공 가능성을 예측하는 것이다.

전체 문제를 단순화하면 다음과 같다.

```text
난임 시술 데이터
↓
전처리
↓
Feature Engineering
↓
Machine Learning
↓
임신 성공 여부 예측
```

---

# 2. 문제 정의

Target:

```text
임신 성공 여부
```

예를 들어:

```text
0
→ 임신 성공하지 않음

1
→ 임신 성공
```

과 같은 Binary Target을 가진다.

따라서 이 프로젝트는:

```text
Binary Classification
```

문제이다.

---

# 3. 프로젝트에서 사용한 파일

Competition 형태의 Dataset은 다음과 같이 구성되었다.

```text
train.csv

test.csv

sample_submission.csv

데이터 명세.xlsx
```

---

# 4. train.csv

`train.csv`에는:

```text
Input Feature
+
Target
```

이 들어 있다.

즉:

```text
환자 및 시술 관련 Feature
+
임신 성공 여부
```

이다.

---

# 5. test.csv

`test.csv`에는 일반적으로 Target이 없다.

```text
Input Feature
```

만 존재한다.

최종 Model이 이 Data에 대한 임신 성공 가능성을 Prediction한다.

---

# 6. sample_submission.csv

제출 결과의 형식을 알려주는 파일이다.

예:

```text
ID

probability
```

같은 구조를 가질 수 있다.

실제 Column 이름은 제공된 Submission 파일을 기준으로 확인한다.

---

# 7. 데이터 명세

Column 이름만 보고 의미를 추측하지 않는다.

먼저:

```text
Column 의미

Category 값 의미

단위

Missing 의미

Target 정의
```

를 확인한다.

---

# 8. 첫 번째 단계

Data를 읽는다.

```python
import pandas as pd
import numpy as np


train = pd.read_csv(
    "/content/train.csv"
)


test = pd.read_csv(
    "/content/test.csv"
)


submission = pd.read_csv(
    "/content/sample_submission.csv"
)
```

---

# 9. Data Shape 확인

```python
print(
    "train:",
    train.shape
)


print(
    "test:",
    test.shape
)


print(
    "submission:",
    submission.shape
)
```

---

# 10. 왜 Shape부터 확인할까?

다음 문제를 빠르게 찾을 수 있다.

```text
파일을 잘못 읽음

다른 Version의 Dataset 사용

Column 누락

Row 수 차이

잘못된 파일 경로
```

---

# 11. Column 확인

```python
print(
    train.columns.tolist()
)
```

---

# 12. Target 확인

```python
target_col = (
    "임신 성공 여부"
)


print(
    train[
        target_col
    ].value_counts()
)
```

---

# 13. 비율도 확인

```python
print(
    train[
        target_col
    ].value_counts(
        normalize=True
    )
)
```

---

# 14. 왜 비율을 확인할까?

Binary Classification에서:

```text
0과 1의 비율
```

이 크게 다르면 Class Imbalance 문제가 있을 수 있기 때문이다.

---

# 15. 기본 EDA

```python
train.info()
```

```python
display(
    train.head()
)
```

```python
display(
    train.describe(
        include="all"
    )
)
```

---

# 16. Data Type 확인

Feature를:

```text
Numeric

Categorical

Date

ID
```

등으로 나누어 생각한다.

---

# 17. ID Column

단순 식별자라면 Model Feature에서 제외하는 것을 검토한다.

예:

```text
ID
```

가 환자를 구분하기 위한 번호일 뿐이라면 실제 임신 성공 원인을 설명하는 Feature라고 보기 어렵다.

---

# 18. Missing Value 확인

난임 Dataset에서 중요한 단계 중 하나였다.

```python
missing = (
    train.isnull().sum()
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

# 19. Missing Ratio

개수뿐 아니라 비율도 확인한다.

```python
missing_ratio = (
    train.isnull().mean()
    *
    100
)


missing_ratio = (
    missing_ratio[
        missing_ratio > 0
    ]
)


display(
    missing_ratio.sort_values(
        ascending=False
    )
)
```

---

# 20. Missing은 모두 같은 의미가 아니다

예를 들어 특정 시술을 하지 않은 환자에게 해당 시술 관련 값이 없는 경우:

```text
측정 실패
```

가 아니라:

```text
해당 없음
```

일 가능성도 있다.

---

# 21. 대표적으로 확인했던 Feature

예:

```text
난자 해동 경과일
```

같은 Column은 Missing이 매우 많을 수 있다.

---

# 22. 여기서 중요한 질문

```text
왜 Missing인가?

실제 정보가 누락된 것인가?

해당 시술을 하지 않았기 때문인가?
```

를 구분해야 한다.

---

# 23. 특정 Feature 값 확인

예:

```python
train.loc[
    train[
        "난자 해동 경과일"
    ].notnull(),
    [
        "시술 유형",
        "난자 해동 경과일"
    ]
].head(
    20
)
```

---

# 24. Missing 개수가 달랐던 문제

같은 Column인데 실행할 때 Missing Count가 크게 다르면 먼저:

```text
같은 train.csv인가?

같은 Dataset Version인가?

현재 train 변수가 무엇을 가리키는가?

중간에 DataFrame을 수정했는가?

Notebook Cell 실행 순서는 올바른가?
```

를 확인해야 한다.

---

# 25. 중요한 교훈

분석 결과가 이상하면 바로 통계 해석으로 넘어가지 않는다.

먼저:

```python
print(
    train.shape
)
```

```python
print(
    train[
        "난자 해동 경과일"
    ].isnull().sum()
)
```

처럼 현재 Data 자체를 확인한다.

---

# 26. Target Missing

Target에 Missing이 있는지도 확인한다.

```python
print(
    train[
        target_col
    ].isnull().sum()
)
```

---

# 27. Duplicate 확인

```python
print(
    train.duplicated().sum()
)
```

---

# 28. Categorical Feature

문자 또는 Category Feature의 값을 확인한다.

```python
categorical_cols = (
    train.select_dtypes(
        include=[
            "object",
            "category"
        ]
    ).columns
)


for col in categorical_cols:

    print(
        "\n",
        col
    )

    print(
        train[
            col
        ].value_counts(
            dropna=False
        ).head(
            20
        )
    )
```

---

# 29. Numeric Feature

```python
numeric_cols = (
    train.select_dtypes(
        include=np.number
    ).columns
)
```

---

# 30. Distribution 확인

예:

```python
import matplotlib.pyplot as plt


train[
    "특정수치형컬럼"
].hist(
    bins=30
)


plt.show()
```

실제 Column 이름은 Dataset에 존재하는 Feature로 바꿔 사용한다.

---

# 31. Target별 Distribution

```python
import seaborn as sns


sns.histplot(
    data=train,
    x="특정수치형컬럼",
    hue=target_col,
    kde=True
)
```

---

# 32. 통계 분석

EDA에서 눈으로 차이가 보인다고 바로:

```text
유의한 차이가 있다.
```

고 판단하지 않는다.

필요한 경우 통계 검정을 사용할 수 있다.

---

# 33. Normality

예:

```python
from scipy.stats import (
    shapiro
)


stat, p = shapiro(
    data
)
```

---

# 34. Shapiro-Wilk

귀무가설:

```text
정규분포를 따른다고 볼 수 있다.
```

대립가설:

```text
정규분포를 따른다고 보기 어렵다.
```

---

# 35. 비정규 분포

두 독립 Group의 분포 차이를 확인할 때 상황에 따라:

```text
Mann-Whitney U
```

를 사용할 수 있다.

---

# 36. Mann-Whitney U

```python
from scipy.stats import (
    mannwhitneyu
)


stat, p = mannwhitneyu(
    group_0,
    group_1,
    alternative="two-sided"
)
```

---

# 37. Binary Target과 Numeric Feature

Point-biserial Correlation을 사용할 수도 있다.

```python
from scipy.stats import (
    pointbiserialr
)
```

---

# 38. 중요한 주의

```text
p-value가 작다
=
좋은 Prediction Feature이다
```

가 아니다.

통계적 연관성과 Model의 Prediction 기여도는 다른 개념이다.

---

# 39. Multicollinearity

Numeric Feature 사이의 강한 선형 관계를 점검하기 위해 VIF를 사용할 수도 있다.

---

# 40. VIF

```python
from statsmodels.stats.outliers_influence import (
    variance_inflation_factor
)
```

---

# 41. 하지만

VIF가 높다고 Tree Model에서도 반드시 Feature를 삭제해야 하는 것은 아니다.

사용할 Model과 분석 목적을 함께 고려한다.

---

# 42. Preprocessing

EDA가 끝나면 Model Training을 위한 Data를 준비한다.

전체적인 흐름:

```text
Missing 처리
↓
Categorical Encoding
↓
Numeric Scaling
↓
X / y 분리
↓
Train / Validation
```

---

# 43. X와 y

```python
X = train.drop(
    columns=[
        target_col
    ]
)


y = train[
    target_col
]
```

---

# 44. Train / Validation

```python
from sklearn.model_selection import (
    train_test_split
)


X_train, X_val, y_train, y_val = (
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

# 45. 왜 Stratify?

Class 비율을 Train과 Validation에서 비슷하게 유지하기 위해 사용한다.

---

# 46. Leakage 방지

Missing 처리나 Scaling 기준을 전체 Dataset에서 먼저 학습하지 않도록 주의한다.

좋은 흐름:

```text
Split
↓
Train에서 Preprocessing fit
↓
Validation transform
```

이다.

---

# 47. Pipeline

scikit-learn의 Pipeline을 사용하면 Preprocessing Leakage를 줄이는 데 도움이 된다.

---

# 48. Numeric Pipeline

예:

```python
from sklearn.pipeline import (
    Pipeline
)

from sklearn.impute import (
    SimpleImputer
)

from sklearn.preprocessing import (
    StandardScaler
)


numeric_pipeline = Pipeline(
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
        )
    ]
)
```

---

# 49. Categorical Pipeline

```python
from sklearn.preprocessing import (
    OneHotEncoder
)


categorical_pipeline = Pipeline(
    steps=[
        (
            "imputer",
            SimpleImputer(
                strategy="most_frequent"
            )
        ),

        (
            "encoder",
            OneHotEncoder(
                handle_unknown="ignore"
            )
        )
    ]
)
```

---

# 50. ColumnTransformer

```python
from sklearn.compose import (
    ColumnTransformer
)


preprocessor = ColumnTransformer(
    transformers=[
        (
            "num",
            numeric_pipeline,
            numeric_cols
        ),

        (
            "cat",
            categorical_pipeline,
            categorical_cols
        )
    ]
)
```

---

# 51. Baseline

처음부터 Ensemble이나 Stacking을 만들지 않는다.

먼저 Baseline Model을 만든다.

예:

```text
Logistic Regression
```

---

# 52. Logistic Regression

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

# 53. Pipeline과 연결

```python
pipeline = Pipeline(
    steps=[
        (
            "preprocessor",
            preprocessor
        ),

        (
            "model",
            model
        )
    ]
)
```

---

# 54. Training

```python
pipeline.fit(
    X_train,
    y_train
)
```

---

# 55. Prediction

```python
pred = pipeline.predict(
    X_val
)
```

---

# 56. Probability

```python
prob = (
    pipeline.predict_proba(
        X_val
    )[
        :,
        1
    ]
)
```

---

# 57. Evaluation

```python
from sklearn.metrics import (
    accuracy_score,
    f1_score,
    roc_auc_score
)


print(
    "Accuracy:",
    accuracy_score(
        y_val,
        pred
    )
)


print(
    "F1:",
    f1_score(
        y_val,
        pred
    )
)


print(
    "ROC-AUC:",
    roc_auc_score(
        y_val,
        prob
    )
)
```

---

# 58. Logistic Regression을 사용하는 이유

복잡한 Model보다 먼저:

```text
비교 기준

Feature와 Target 관계의 기본 Pattern

Preprocessing Pipeline 정상 작동 여부
```

를 확인할 수 있다.

---

# 59. Logistic Regression의 C

`C`는 Regularization 강도와 관련된 Hyperparameter이다.

작은 C:

```text
Regularization 강함
```

큰 C:

```text
Regularization 약함
```

으로 이해할 수 있다.

---

# 60. C 후보 비교

예:

```python
C_values = [
    1e-7,
    1e-6,
    1e-5,
    1e-4,
    1e-3,
    1e-2,
    1e-1,
    1,
    10,
    100
]
```

---

# 61. Cross Validation

한 번의 Split만 보는 대신 K-Fold를 사용할 수 있다.

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

# 62. 왜 StratifiedKFold?

Binary Target의 Class 비율을 각 Fold에서 최대한 비슷하게 유지하기 위해서이다.

---

# 63. Cross Validation의 목적

```text
한 번의 Validation 결과에
지나치게 의존하지 않기 위해
```

사용한다.

---

# 64. Random Forest

다음 Model로 Random Forest를 비교할 수 있다.

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

---

# 65. Random Forest의 장점

```text
비선형 관계

Feature Interaction

Scaling에 비교적 덜 민감
```

한 특성을 가진다.

---

# 66. Tree Boosting Model

Tabular Data에서는 다음 Model도 많이 사용된다.

```text
XGBoost

LightGBM

CatBoost
```

---

# 67. XGBoost

Boosting 방식으로 여러 Tree를 순차적으로 학습한다.

앞선 Tree의 Error를 다음 Tree가 보완하는 방향으로 학습한다.

---

# 68. LightGBM

대규모 Tabular Data에서 효율적인 Gradient Boosting Model로 많이 사용된다.

---

# 69. CatBoost

Categorical Feature를 다루는 기능이 강점 중 하나이다.

하지만 Category 처리 방식을 정확히 구성해야 한다.

---

# 70. Model 비교

예:

```text
Logistic Regression

Random Forest

XGBoost

LightGBM

CatBoost
```

를 같은 Validation 기준에서 비교한다.

---

# 71. Feature Engineering

이 프로젝트에서 중요한 부분 중 하나였다.

단순히 원본 Feature만 사용하는 것이 아니라 난임 시술 과정에서 의미가 있을 수 있는 Feature 조합을 생각한다.

---

# 72. 난자와 배아

예를 들어 Dataset에 다음과 같은 정보가 있다고 가정한다.

```text
난자 관련 수

배아 관련 수
```

---

# 73. Ratio Feature

단순 개수 외에:

```text
배아 수
/
난자 수
```

와 같은 Ratio를 만들 수 있다.

---

# 74. 왜 Ratio?

예를 들어:

```text
난자 20개 → 배아 5개
```

와:

```text
난자 5개 → 배아 5개
```

는 최종 배아 수는 같지만 과정의 비율은 다르다.

---

# 75. 안전한 나눗셈

0으로 나누는 문제를 방지해야 한다.

```python
denominator = (
    train[
        "분모컬럼"
    ].replace(
        0,
        np.nan
    )
)


train[
    "새로운비율"
] = (
    train[
        "분자컬럼"
    ]
    /
    denominator
)
```

실제 Dataset Column 이름에 맞게 사용한다.

---

# 76. Feature Engineering 예

```text
합

차이

비율

상호작용

시술 단계별 전환율

시간 관련 Feature
```

등을 생각할 수 있다.

---

# 77. Feature Engineering의 원칙

새 Feature를 만들기 전에 질문한다.

```text
이 Feature는 어떤 의미가 있는가?

Prediction 시점에 알 수 있는 정보인가?

Target Leakage는 없는가?
```

---

# 78. 시간 Feature

예를 들어:

```text
난자 해동 경과일

배아 이식 경과일
```

같은 시간 Feature가 존재할 수 있다.

---

# 79. 단순 숫자로만 보면 안 된다

예를 들어:

```text
0일

1일

2일

3일

4일

5일

6일

7일
```

에 따른 성공률 Pattern을 확인할 수 있다.

---

# 80. Group별 성공률

예:

```python
result = (
    train.groupby(
        "배아 이식 경과일"
    )[
        target_col
    ]
    .agg(
        [
            "count",
            "mean"
        ]
    )
)


display(
    result
)
```

---

# 81. mean의 의미

Target이:

```text
0 / 1
```

이면 Group별 Target Mean은 해당 Group의 성공 비율로 해석할 수 있다.

---

# 82. Count도 함께 봐야 한다

예:

```text
7일 성공률 = 40%
```

만 보면 안 된다.

Sample이:

```text
5개
```

인지:

```text
5,000개
```

인지에 따라 신뢰도가 크게 달라질 수 있다.

---

# 83. Group Interaction

예를 들어:

```text
신선 배아

동결 배아
```

등 Group에 따라 같은 `배아 이식 경과일`의 Pattern이 다를 가능성을 확인할 수 있다.

---

# 84. Groupby

```python
result = (
    train.groupby(
        [
            "동결 배아 사용 여부",
            "배아 이식 경과일"
        ]
    )[
        target_col
    ]
    .agg(
        [
            "count",
            "mean"
        ]
    )
)


display(
    result
)
```

---

# 85. Pattern이 비슷해 보이면?

눈으로 비슷해 보이는 것과 실제로 같은 효과를 가진다는 것은 다르다.

```text
Sample 수

분산

다른 Feature 영향

Interaction
```

을 함께 생각해야 한다.

---

# 86. Feature Interaction

두 Feature가 결합되었을 때 의미가 달라지는 관계를 생각할 수 있다.

예:

```text
동결 여부
×
이식 경과일
```

이다.

---

# 87. Class Imbalance

Target Distribution이 불균형하다면 Accuracy만으로 평가하지 않는다.

---

# 88. 추가 Metric

```text
Precision

Recall

F1

ROC-AUC

PR-AUC
```

등을 확인한다.

---

# 89. Sampling

필요한 경우:

```text
SMOTE

KMeansSMOTE
```

같은 방법을 실험할 수 있다.

---

# 90. KMeansSMOTE

Minority Sample의 구조를 고려하여 Oversampling을 수행하는 방법 중 하나이다.

---

# 91. 중요한 원칙

Sampling은:

```text
Training Data에만
```

적용해야 한다.

---

# 92. 잘못된 방법

```text
전체 Data
↓
SMOTE
↓
Train / Validation Split
```

은 Validation Leakage를 일으킬 수 있다.

---

# 93. Cross Validation에서도

각 Fold에서:

```text
Training Fold
↓
Sampling
↓
Model Training

Validation Fold
↓
Sampling하지 않음
```

이어야 한다.

---

# 94. imblearn Pipeline

Sampling을 포함하는 경우:

```python
from imblearn.pipeline import (
    Pipeline as ImbPipeline
)
```

을 사용할 수 있다.

---

# 95. Outlier Experiment

Numeric Feature에서 IQR 또는 Z-score를 이용한 Outlier 처리도 실험할 수 있다.

---

# 96. IQR

```text
IQR
=
Q3 - Q1
```

---

# 97. Boundary

```text
Lower
=
Q1 - 1.5 × IQR

Upper
=
Q3 + 1.5 × IQR
```

이다.

---

# 98. Outlier 처리 방법

```text
삭제

중앙값 대체

Clipping

그대로 유지
```

등을 비교할 수 있다.

---

# 99. 중요한 점

Outlier 처리도:

```text
무조건 해야 하는 전처리
```

가 아니다.

Model 성능과 Domain 의미를 함께 확인한다.

---

# 100. Scaling

Logistic Regression 같은 Model에서는 StandardScaler를 사용할 수 있다.

```python
from sklearn.preprocessing import (
    StandardScaler
)
```

---

# 101. Coefficient

Scaling 후 Logistic Regression의 `coef_`를 확인하면 Feature가 Prediction Score에 어떤 방향으로 연결되는지 탐색할 수 있다.

---

# 102. 주의

```text
Coefficient가 양수
=
그 Feature가 임신 성공의 원인
```

은 아니다.

Model 내부 관계를 해석하는 것이다.

---

# 103. Tree Feature Importance

Random Forest 등의 Model에서는:

```python
model.feature_importances_
```

를 확인할 수 있다.

---

# 104. SHAP

더 자세한 Model Interpretation을 위해 SHAP을 사용할 수 있다.

---

# 105. SHAP의 질문

```text
이 Prediction에서
어떤 Feature가
Prediction을 어느 방향으로 밀었는가?
```

를 분석한다.

---

# 106. Global SHAP

Dataset 전체에서 중요한 Feature Pattern을 확인한다.

예:

```text
SHAP Bar Plot

SHAP Beeswarm
```

---

# 107. Local SHAP

한 Patient Prediction을 설명하는 데 사용할 수 있다.

---

# 108. 주의

SHAP도:

```text
인과관계
```

를 증명하는 방법은 아니다.

---

# 109. Ensemble

개별 Model의 장점을 결합하기 위해:

```text
LightGBM

XGBoost

CatBoost
```

등의 Prediction을 결합할 수 있다.

---

# 110. 단순 Average

예:

```python
final_prob = (
    lgb_prob
    +
    xgb_prob
    +
    cat_prob
) / 3
```

---

# 111. Weighted Average

Model 성능에 따라 Weight를 다르게 줄 수도 있다.

예:

```python
final_prob = (
    0.4 * lgb_prob
    +
    0.3 * xgb_prob
    +
    0.3 * cat_prob
)
```

Weight 역시 Validation을 통해 결정해야 한다.

---

# 112. Stacking

더 나아가 Base Model Prediction을 새로운 Feature로 만들어 Meta Model을 학습할 수 있다.

```text
LightGBM ─┐
          │
XGBoost ──┼→ OOF Prediction
          │
CatBoost ─┘
             ↓
          Meta Model
             ↓
        Final Prediction
```

---

# 113. 왜 OOF?

Training Data를 그대로 학습한 Model의 Prediction을 Meta Model에 사용하면 Leakage와 Overfitting 위험이 커진다.

---

# 114. OOF

각 Sample은:

```text
자신을 Training하지 않은 Model
```

에게 Prediction을 받아야 한다.

---

# 115. 기본 구조

```python
oof = np.zeros(
    len(y)
)
```

각 Fold에서:

```python
oof[
    valid_idx
] = model.predict_proba(
    X_valid
)[
    :,
    1
]
```

처럼 저장한다.

---

# 116. Test Prediction

각 Fold Model이 `test.csv`를 Prediction하고 평균을 낸다.

```text
Fold 1 Test Prediction
+
Fold 2 Test Prediction
+
...
+
Fold 5 Test Prediction

↓

Average
```

---

# 117. Test.csv와 Test Prediction

둘은 다른 개념이다.

```text
test.csv
=
Prediction해야 하는 Input

Test Prediction
=
Model이 test.csv에 대해 만든 결과
```

이다.

---

# 118. Nested CV

Model Selection과 Performance Estimation을 더 엄격하게 분리하기 위해 Nested Cross Validation을 사용할 수도 있다.

---

# 119. 하지만

Nested CV는 계산량과 구현 복잡도가 증가한다.

프로젝트 목적에 따라 필요한 수준을 선택한다.

---

# 120. Hyperparameter Tuning

XGBoost 등의 Model은 Hyperparameter를 조정할 수 있다.

예:

```text
n_estimators

max_depth

learning_rate

subsample

colsample_bytree

reg_alpha

reg_lambda
```

---

# 121. Tuning 방법

```text
GridSearch

RandomizedSearch

Bayesian Optimization

Optuna
```

등이 있다.

---

# 122. Tuning 전에

먼저:

```text
Baseline

Feature Engineering

Validation Strategy
```

를 안정적으로 만든다.

---

# 123. 왜?

Validation 자체가 잘못되어 있으면 정교하게 Hyperparameter를 찾아도 잘못된 기준을 최적화하게 된다.

---

# 124. Competition Prediction

최종 Model을 이용해:

```python
test_probability = (
    model.predict_proba(
        test
    )[
        :,
        1
    ]
)
```

처럼 Probability를 생성할 수 있다.

실제 코드는 사용한 Preprocessing 구조에 맞춰야 한다.

---

# 125. 왜 Probability?

Competition Evaluation Metric이 Probability 기반이라면 0/1 Class가 아니라 Probability를 제출해야 할 수 있다.

반드시 대회의 평가 방식과 Submission 형식을 확인한다.

---

# 126. Submission 생성

```python
submission[
    "probability"
] = test_probability
```

실제 Target Column 이름에 맞게 수정한다.

---

# 127. 저장

```python
submission.to_csv(
    "submission.csv",
    index=False
)
```

---

# 128. 저장 확인

```python
display(
    submission.head()
)


print(
    submission.shape
)
```

---

# 129. Competition Score

Leaderboard Score는 제출 Prediction과 숨겨진 Test Label을 Competition Server가 비교한 결과이다.

---

# 130. Local Score와 다른 이유

```text
Data Distribution 차이

Validation Split 차이

Overfitting

Feature Leakage

Random Variation

Public / Private Test 구성
```

등 여러 이유가 있을 수 있다.

---

# 131. 따라서

Leaderboard Score만 계속 보고 Model을 바꾸면 Leaderboard 자체에 Overfitting할 수 있다.

---

# 132. 좋은 Experiment 관리

예:

```text
Baseline

Exp 1
Feature Engineering

Exp 2
Outlier Processing

Exp 3
KMeansSMOTE

Exp 4
LightGBM

Exp 5
CatBoost

Exp 6
Ensemble

Exp 7
Stacking
```

처럼 기록한다.

---

# 133. 결과표

```text
Experiment | Model | Features | Sampling | CV Score

Baseline   | LR    | Original | None     | ...

Exp 1      | LR    | +Ratio   | None     | ...

Exp 2      | RF    | +Ratio   | None     | ...

Exp 3      | XGB   | +Ratio   | KMeansSMOTE | ...

Exp 4      | LGBM  | Engineered | None   | ...
```

처럼 비교할 수 있다.

---

# 134. 프로젝트 발표

좋은 발표 흐름:

```text
1. 문제 정의

2. Dataset 설명

3. Target Distribution

4. Missing Value 분석

5. EDA / Statistics

6. Preprocessing

7. Feature Engineering

8. Baseline

9. Model 비교

10. Class Imbalance 대응

11. Hyperparameter Tuning

12. Ensemble / Stacking

13. SHAP

14. 최종 결과

15. 한계 및 개선 방향
```

---

# 135. 발표에서 중요한 질문

```text
왜 이 Feature를 만들었는가?

왜 이 Model을 사용했는가?

왜 이 Metric을 사용했는가?

왜 Sampling을 적용했는가?

실제로 성능이 개선되었는가?
```

에 답할 수 있어야 한다.

---

# 136. Feature Engineering 발표 예

단순히:

```text
배아/난자 비율을 만들었습니다.
```

보다:

```text
절대적인 난자 또는 배아 수만으로는
시술 과정의 전환 정도를 충분히 표현하기 어렵다고 판단하여
배아/난자 비율 Feature를 추가하고
Validation 성능 변화를 비교하였다.
```

라고 설명하는 것이 좋다.

---

# 137. Sampling 발표 예

```text
Target Class 불균형으로 인해
Minority Class Prediction 성능이 낮을 가능성을 가정하였다.

Training Fold에만 KMeansSMOTE를 적용하고
기존 Model과 Recall/F1/ROC-AUC를 비교하였다.
```

처럼 설명한다.

---

# 138. SHAP 발표 예

```text
최종 Model의 Prediction Pattern을 확인하기 위해
SHAP을 이용하여 Feature별 기여도를 분석하였다.

다만 SHAP Importance를
의학적 인과관계로 해석하지 않았다.
```

라고 설명할 수 있다.

---

# 139. 프로젝트에서 배운 점 1

Data 분석은:

```text
Model부터 돌리는 것
```

이 아니라:

```text
Data 구조를 이해하는 것
```

부터 시작한다.

---

# 140. 배운 점 2

Missing Value의:

```text
개수
```

만 보는 것이 아니라:

```text
왜 Missing인지
```

를 확인해야 한다.

---

# 141. 배운 점 3

Feature Engineering은 단순히 Column을 많이 만드는 것이 아니다.

```text
Domain 의미가 있는 Feature를 만들고
실제로 Validation에서 검증
```

해야 한다.

---

# 142. 배운 점 4

한 번의 Score보다:

```text
Cross Validation

OOF

같은 Validation 기준
```

으로 Model을 비교하는 것이 중요하다.

---

# 143. 배운 점 5

Class Imbalance가 있다고 해서 무조건:

```text
SMOTE
```

가 정답은 아니다.

```text
Class Weight

Sampling

Threshold

Model

Metric
```

을 함께 비교한다.

---

# 144. 배운 점 6

통계적으로 유의한 Feature가 반드시 최고의 Machine Learning Feature인 것은 아니다.

---

# 145. 배운 점 7

Model Interpretation은:

```text
Feature Importance

Coefficient

SHAP
```

등 여러 관점에서 볼 수 있다.

하지만:

```text
Importance
≠
Causality
```

이다.

---

# 146. 배운 점 8

Competition에서는:

```text
Local Validation

Leaderboard
```

가 다를 수 있다.

따라서 Validation Strategy가 매우 중요하다.

---

# 147. 배운 점 9

복잡한 Model보다 먼저:

```text
Data

Split

Preprocessing

Leakage
```

를 확인한다.

---

# 148. 배운 점 10

최종 결과보다 중요한 것은:

```text
문제
↓
가설
↓
실험
↓
비교
↓
해석
```

의 흐름을 설명할 수 있는 것이다.

---

# 149. 프로젝트 한계

가능한 한계:

```text
Missing이 많은 Feature

Category 해석의 어려움

Class Imbalance

Feature 간 복잡한 Interaction

Competition Data의 제한된 정보

외부 Dataset 검증 부족
```

등을 생각할 수 있다.

---

# 150. 추가 개선 방향

```text
Domain Knowledge 기반 Feature Engineering

Feature Selection

더 체계적인 Cross Validation

Optuna Hyperparameter Tuning

Ensemble Weight Optimization

Probability Calibration

SHAP 기반 Error Analysis

External Validation
```

등을 생각할 수 있다.

---

# 151. 전체 Workflow

```text
train.csv
↓
Data Structure 확인
↓
Target 확인
↓
Missing Value
↓
EDA
↓
Statistics
↓
Preprocessing
↓
Feature Engineering
↓
Train / Validation
↓
Logistic Regression Baseline
↓
Random Forest
↓
XGBoost
↓
LightGBM
↓
CatBoost
↓
Class Imbalance Experiment
↓
Cross Validation
↓
Hyperparameter Tuning
↓
Ensemble / Stacking
↓
SHAP
↓
test.csv Prediction
↓
submission.csv
```

---

# 152. 가장 중요한 구조

이 프로젝트를 한 문장으로 설명하면:

```text
난임 시술 관련 Tabular Data를 분석하고,
결측치와 Feature 특성을 검토한 뒤,
Feature Engineering과 여러 Classification Model을 비교하여
임신 성공 가능성을 예측하는 프로젝트
```

이다.

---

# 153. 핵심 복습

```text
Problem
→ 임신 성공 여부 예측

Task
→ Binary Classification

Data
→ Tabular Data

EDA
→ Missing / Distribution / Class Balance

Preprocessing
→ Missing / Encoding / Scaling

Feature Engineering
→ 난자·배아·시술 관련 조합 및 비율

Models
→ LR / RF / XGBoost / LightGBM / CatBoost

Validation
→ Stratified Split / Cross Validation

Imbalance
→ Weight / Sampling / KMeansSMOTE

Advanced
→ Hyperparameter Tuning / Ensemble / OOF / Stacking

Interpretation
→ Coefficient / Feature Importance / SHAP

Final
→ test.csv Probability Prediction
→ submission.csv
```

---

# 154. 마지막 교훈

좋은 난임 Prediction Model을 만드는 과정은:

```text
복잡한 Model 하나를 찾는 것
```

이 아니라:

```text
Data를 이해하고

의미 있는 Feature를 만들고

Leakage 없는 Validation을 구성하고

가설을 하나씩 실험하고

Model의 성공과 실패를 해석하는 것
```

이다.