# AI Project Retrospective

## 1. 지금까지 진행한 AI 프로젝트

여러 프로젝트를 진행하면서 다양한 형태의 Data와 문제를 경험했다.

```text
Tabular Data
├── 난임 임신 성공 예측
├── 척추 / 추간판 Data 분석
├── 의약품 금기 Data 분석
└── KTAS 오분류 예측

Image Data
├── OrganAMNIST 복부 CT 장기 분류
└── Chest X-ray Classification
```

프로젝트의 주제는 달랐지만 실제 진행 과정에는 공통된 Pattern이 있었다.

---

# 2. AI 프로젝트의 전체 흐름

```text
Problem Definition
↓
Data Understanding
↓
EDA
↓
Data Cleaning
↓
Train / Validation / Test Split
↓
Preprocessing
↓
Baseline
↓
Evaluation
↓
Problem Discovery
↓
Hypothesis
↓
Experiment
↓
Model Comparison
↓
Failure Analysis
↓
Explainability
↓
Final Model
↓
Inference
↓
Deployment / Submission
↓
Retrospective
```

이 흐름은 앞으로 새로운 AI 프로젝트를 시작할 때도 기본 Template으로 사용할 수 있다.

---

# 3. Step 1 - 문제 정의

Model부터 선택하지 않는다.

먼저 질문한다.

```text
무엇을 해결하려는가?

무엇을 Prediction하려는가?

누가 결과를 사용하는가?

언제 Prediction하는가?

잘못 Prediction했을 때 어떤 문제가 생기는가?
```

---

# 4. 좋은 문제 정의

예:

```text
환자의 정보를 이용하여
임신 성공 가능성을 예측한다.
```

또는:

```text
응급실 초기 정보만 이용하여
KTAS 오분류 위험을 탐지한다.
```

또는:

```text
Chest X-ray Image를 이용하여
Normal과 Pneumonia를 분류한다.
```

처럼 Input과 Output을 명확하게 정의한다.

---

# 5. Machine Learning Task 정의

문제를 ML Task로 변환한다.

```text
Binary Classification

Multiclass Classification

Regression

Clustering

Image Classification

Detection

Segmentation
```

등이 있다.

---

# 6. 모든 Data 분석이 Prediction은 아니다

의약품 금기 분석처럼 질문이:

```text
어떤 성분의 제품수 합계가 가장 큰가?

투여경로와 금기등급에 관계가 있는가?
```

라면 반드시 Machine Learning Model이 필요한 것은 아니다.

```text
Pandas

Groupby

Crosstab

Statistical Test
```

만으로도 문제를 해결할 수 있다.

---

# 7. 첫 번째 교훈

```text
AI를 사용해야 하는가?
```

보다 먼저:

```text
이 문제를 해결하는 가장 적절한 방법은 무엇인가?
```

를 생각한다.

---

# 8. Step 2 - Data Understanding

Data를 받자마자 Model에 넣지 않는다.

먼저 구조를 확인한다.

---

# 9. Tabular Data

```python
print(
    df.shape
)

print(
    df.columns.tolist()
)

display(
    df.head()
)

df.info()

display(
    df.describe()
)
```

---

# 10. 확인할 것

```text
Row 수

Column 수

Data Type

Target

Missing Value

Duplicate

Category

Numeric Range

ID Column
```

이다.

---

# 11. Image Data

Image Project에서는 다음을 확인한다.

```text
Image 개수

Class 개수

Class Distribution

Image Size

Channel

Pixel Range

Image Quality

Patient ID

Duplicate Image
```

---

# 12. Data Dictionary

Column 이름만 보고 의미를 추측하지 않는다.

예:

```text
0

1
```

이 무엇을 의미하는지 반드시 Data 명세를 확인한다.

---

# 13. 의료 Data에서는 특히 중요

```text
단위

측정 시점

검사 방법

환자 기준

결측 의미
```

를 확인해야 한다.

---

# 14. Step 3 - EDA

EDA는 단순히 Graph를 그리는 과정이 아니다.

목적은:

```text
Data가 어떤 구조인지 이해하고

Modeling 전에 문제를 발견하는 것
```

이다.

---

# 15. EDA 질문

```text
Target 비율은?

Missing이 많은 Column은?

이상값은?

특정 Group에 Missing이 몰리는가?

Class Imbalance가 있는가?

Train과 Test 분포가 다른가?

Image 밝기 차이가 있는가?
```

등을 확인한다.

---

# 16. 실제 프로젝트에서 발견한 것

여러 프로젝트에서 EDA를 통해:

```text
Class Imbalance

이상 문자열

구조적 Missing

Brightness 차이

특정 Feature 분포 차이
```

등을 발견할 수 있었다.

---

# 17. EDA의 역할

```text
EDA
↓
문제 발견
↓
전처리 방법 결정
↓
실험 가설 생성
```

으로 이어진다.

---

# 18. Step 4 - Data Cleaning

현실 Data는 바로 Model에 사용할 수 없는 경우가 많다.

---

# 19. 대표 문제

```text
Missing Value

Outlier

잘못된 문자열

잘못된 Data Type

Duplicate

Invalid Category

File Path Error

Corrupted Image
```

등이다.

---

# 20. 숫자 Column의 이상 문자열

예:

```text
NRS_pain
"#BOŢ!"

SBP
"??"
```

같은 값이 들어 있을 수 있다.

---

# 21. 처리 예

```python
df[
    "NRS_pain"
] = pd.to_numeric(
    df[
        "NRS_pain"
    ],
    errors="coerce"
)
```

---

# 22. 하지만

```text
문자열 → NaN
```

으로 바꾸었다고 전처리가 끝난 것은 아니다.

---

# 23. 다음 질문

```text
왜 이런 값이 발생했는가?

Missing으로 처리해도 되는가?

삭제할 것인가?

대체할 것인가?

Missing Indicator를 만들 것인가?
```

를 생각한다.

---

# 24. Missing은 모두 같은 Missing이 아니다

KTAS 프로젝트처럼 특정 Cohort에는 Data 자체가 존재하지 않을 수 있다.

```text
우연한 Missing

구조적인 Missing
```

을 구분해야 한다.

---

# 25. Step 5 - Split

Modeling에서 매우 중요한 단계이다.

```text
Train

Validation

Test
```

를 구분한다.

---

# 26. 역할

```text
Train
→ Model 학습

Validation
→ Model / Hyperparameter / Threshold 선택

Test
→ 마지막 성능 평가
```

이다.

---

# 27. Test를 계속 보면 안 되는 이유

Test를 반복해서 보면서 Model을 수정하면 결국:

```text
Test Data에 맞춘 Model
```

이 될 수 있다.

---

# 28. Tabular Classification

Class 비율을 유지하려면:

```python
train_test_split(
    X,
    y,
    stratify=y,
    random_state=42
)
```

처럼 Stratified Split을 사용할 수 있다.

---

# 29. 의료 Data

같은 Patient의 여러 Record가 있다면:

```text
Patient-level Split
```

이 중요하다.

---

# 30. 잘못된 예

```text
Patient A Record 1
→ Train

Patient A Record 2
→ Validation
```

---

# 31. 문제

Model이 Validation에서 사실상 이미 본 Patient의 Pattern을 볼 수 있다.

이를 Data Leakage의 한 형태로 생각할 수 있다.

---

# 32. Image Data도 동일

같은 Patient의 여러 X-ray나 CT Slice가 있다면 Patient 단위로 나누는 것이 중요하다.

---

# 33. Time Data

미래를 Prediction하는 문제라면 Random Split보다:

```text
과거
→ Train

미래
→ Validation / Test
```

형태의 Time Split이 더 현실적일 수 있다.

---

# 34. Step 6 - Data Leakage

AI Project에서 가장 위험한 문제 중 하나이다.

---

# 35. Data Leakage란?

Model이 실제 Prediction 시점에는 알 수 없는 정보를 학습 과정에서 사용한 경우이다.

---

# 36. 예

```text
최종 진단 결과를 이용해
초기 진단을 Prediction
```

한다면 Leakage가 발생할 수 있다.

---

# 37. 또 다른 Leakage

Preprocessing을 전체 Data에 먼저 Fit하는 것도 문제가 될 수 있다.

잘못된 예:

```python
scaler.fit(
    X
)
```

후 Split.

---

# 38. 올바른 방향

```text
Split 먼저

↓

Train에서 Fit

↓

Validation / Test에는 Transform만
```

이다.

---

# 39. Cross Validation에서도 동일

각 Fold의 Training Data만 이용해:

```text
Imputation

Scaling

Encoding

SMOTE
```

등을 Fit해야 한다.

---

# 40. 그래서 Pipeline

```python
from sklearn.pipeline import (
    Pipeline
)
```

같은 구조를 사용하면 Leakage를 줄이는 데 도움이 된다.

---

# 41. Image Augmentation Leakage

Augmented Image를 먼저 생성한 뒤 Random Split하면:

```text
원본
→ Train

거의 동일한 Augmented Image
→ Validation
```

이 될 수 있다.

---

# 42. 올바른 순서

```text
Original Data

↓

Split

↓

Train에만 Random Augmentation
```

이다.

---

# 43. Leakage Checklist

```text
Target을 직접 포함하는 Feature가 있는가?

Prediction 이후에만 알 수 있는 정보인가?

같은 Patient가 양쪽에 있는가?

Duplicate가 양쪽에 있는가?

Preprocessing을 전체 Data에 Fit했는가?

Augmentation 후 Split했는가?
```

를 확인한다.

---

# 44. Step 7 - Baseline

처음부터 XGBoost, Ensemble, Fine-tuning을 모두 넣지 않는다.

---

# 45. Baseline이란?

비교 기준이 되는 첫 Model이다.

---

# 46. 예

Tabular:

```text
Logistic Regression

Random Forest
```

Image:

```text
Simple CNN

Pretrained Model 기본 Fine-tuning
```

등을 사용할 수 있다.

---

# 47. Baseline이 중요한 이유

개선 Model이:

```text
정말 좋아졌는가?
```

를 판단할 기준이 생긴다.

---

# 48. Baseline 없이

```text
Accuracy = 0.90
```

이라고 해도 좋은 결과인지 판단하기 어렵다.

---

# 49. Dummy Baseline

Class Imbalance가 심하다면:

```python
from sklearn.dummy import (
    DummyClassifier
)
```

같은 단순 Model과 비교하는 것도 중요하다.

---

# 50. Step 8 - Metric

Model 성능은 문제에 맞는 Metric으로 평가한다.

---

# 51. Classification

```text
Accuracy

Precision

Recall

Specificity

F1

ROC-AUC

PR-AUC
```

등이 있다.

---

# 52. Regression

```text
MAE

MSE

RMSE

R²
```

등이 있다.

---

# 53. Accuracy 함정

Class Imbalance가 심하면 Accuracy가 높아도 Minority Class를 하나도 찾지 못할 수 있다.

---

# 54. 예

```text
Normal
90%

Risk
10%
```

일 때 모두 Normal이라고 Prediction하면:

```text
Accuracy
90%
```

이다.

---

# 55. 하지만

Risk Patient:

```text
Recall
0%
```

이다.

---

# 56. 그래서 Metric은 목적에 따라 선택한다

```text
위험 환자를 놓치면 안 된다.
→ Recall 중요

False Alarm 비용이 크다.
→ Precision 중요

두 가지 균형
→ F1

Class Imbalance가 크다.
→ PR-AUC / Macro F1 고려
```

처럼 생각한다.

---

# 57. 의료 AI

의료 문제에서는:

```text
False Negative

False Positive
```

가 서로 다른 의미와 비용을 가진다.

---

# 58. 따라서

```text
Accuracy가 가장 높은 Model
```

이 항상 가장 좋은 Model은 아니다.

---

# 59. Step 9 - Threshold

Binary Classification Model은 Probability를 출력할 수 있다.

예:

```text
P(Risk)
=
0.72
```

---

# 60. Threshold

```text
0.5 이상
→ Risk

0.5 미만
→ Normal
```

처럼 Class를 결정한다.

---

# 61. Threshold를 낮추면

일반적으로:

```text
Recall ↑

False Positive ↑
```

가 발생할 수 있다.

---

# 62. Threshold를 높이면

일반적으로:

```text
Precision ↑ 가능

Recall ↓ 가능
```

하다.

---

# 63. KTAS 프로젝트에서 배운 것

Threshold를 크게 낮추면서 Recall을 높일 수 있었지만 Accuracy와 Precision 측면에서 Trade-off가 발생했다.

---

# 64. 중요한 교훈

```text
Threshold 0.5
```

는 자연법칙이 아니다.

---

# 65. 하지만

Test Set에서 가장 좋은 Threshold를 찾으면 안 된다.

---

# 66. Threshold 선택

```text
Validation

또는

OOF Prediction
```

에서 결정하고 Test는 마지막 평가에 사용한다.

---

# 67. Step 10 - Class Imbalance

여러 의료 Data에서 자주 발생한다.

---

# 68. 해결 방법

```text
Class Weight

Oversampling

Undersampling

SMOTE

KMeansSMOTE

Threshold Adjustment
```

등이 있다.

---

# 69. 중요한 점

이 방법들은 모두 같은 역할이 아니다.

---

# 70. Class Weight

Loss 계산에서 Minority Class Error를 더 크게 반영한다.

---

# 71. SMOTE

Minority Class의 Synthetic Sample을 생성한다.

---

# 72. Threshold

Model Training 이후 Decision 기준을 변경한다.

---

# 73. 따라서

```text
Class Weight

Sampling

Threshold
```

은 서로 다른 단계의 방법이다.

---

# 74. Sampling Leakage

SMOTE는 Split 전에 적용하지 않는다.

---

# 75. 올바른 방향

```text
Train Fold

↓

SMOTE

↓

Model
```

이다.

---

# 76. Step 11 - Cross Validation

한 번의 Split 결과만 믿으면 우연의 영향을 받을 수 있다.

---

# 77. KFold

Data를 여러 Fold로 나누어 반복 평가한다.

---

# 78. Classification

Class 비율을 유지하려면:

```text
StratifiedKFold
```

를 사용할 수 있다.

---

# 79. Patient Data

같은 Patient가 여러 Record를 가진다면:

```text
GroupKFold

StratifiedGroupKFold
```

등을 고려한다.

---

# 80. Cross Validation 결과

평균만 보지 않는다.

```text
Mean

Standard Deviation

각 Fold Score
```

를 같이 본다.

---

# 81. 왜?

```text
0.90 ± 0.01
```

과:

```text
0.90 ± 0.15
```

는 같은 Model 안정성이라고 보기 어렵기 때문이다.

---

# 82. Step 12 - Hyperparameter Tuning

Baseline을 만든 후 필요한 경우 Tuning을 한다.

---

# 83. 방법

```text
GridSearchCV

RandomizedSearchCV

BayesSearchCV

Optuna
```

등을 사용할 수 있다.

---

# 84. 하지만

Hyperparameter Tuning이 Data 문제를 해결해주지는 않는다.

---

# 85. 예

```text
Leakage가 있다.

↓

Optuna 1,000회
```

를 해도 올바른 Model이 되는 것은 아니다.

---

# 86. 우선순위

```text
Data Quality

Split

Leakage

Baseline

Metric

↓

그 다음 Tuning
```

이다.

---

# 87. Step 13 - Feature Engineering

Tabular Project에서는 Domain Knowledge를 이용해 새로운 Feature를 만들 수 있다.

---

# 88. 예

```text
BMI

Ratio

Sum

Difference

Interaction

Missing Indicator
```

등이다.

---

# 89. 중요한 질문

```text
이 Feature를 실제 Prediction 시점에 계산할 수 있는가?
```

를 확인한다.

---

# 90. Feature가 많다고 좋은 것은 아니다

불필요한 Feature는:

```text
Noise

Overfitting

Complexity
```

를 증가시킬 수 있다.

---

# 91. Step 14 - Experiment Design

AI Project는 단순히 Code를 여러 번 실행하는 것이 아니다.

---

# 92. 좋은 Experiment

```text
Problem
↓
Hypothesis
↓
One Change
↓
Result
↓
Interpretation
```

이다.

---

# 93. 예

문제:

```text
Minority Class Recall이 낮다.
```

가설:

```text
Class Weight를 적용하면
Minority Class Recall이 개선될 것이다.
```

실험:

```text
Baseline

vs

Weighted Loss
```

비교.

---

# 94. 나쁜 Experiment

한 번에:

```text
Model 변경

Learning Rate 변경

Augmentation 변경

Loss 변경

Batch Size 변경
```

을 모두 바꾼다.

---

# 95. 왜 문제인가?

성능이 바뀌어도:

```text
무엇 때문에 바뀌었는가?
```

를 알 수 없기 때문이다.

---

# 96. Experiment Table

```text
Experiment

Model

Preprocessing

Loss

Learning Rate

Augmentation

Threshold

Validation Metric

Result
```

을 기록한다.

---

# 97. Step 15 - Validation Score를 의심하기

높은 Score가 나오면 기뻐하기 전에 확인한다.

```text
너무 높은 것은 아닌가?
```

---

# 98. 특히

```text
Accuracy = 0.99
```

같은 결과가 나오면:

```text
Leakage?

Duplicate?

Easy Shortcut?

Split 문제?

Target Proxy?
```

를 확인한다.

---

# 99. Chest X-ray 프로젝트

Local Validation에서는 약:

```text
0.98 ~ 0.99
```

수준이었지만 실제 Competition Leaderboard에서는 약:

```text
0.915
```

수준이었다.

---

# 100. 여기서 배운 것

```text
Validation Score
≠
실제 Generalization 성능
```

이다.

---

# 101. Step 16 - Failure Analysis

평균 Metric만 보면 Model이 왜 실패하는지 알 수 없다.

---

# 102. Error Sample을 직접 본다

```text
False Positive

False Negative

High-confidence Error

Low-confidence Error
```

를 확인한다.

---

# 103. High-confidence Error

Model이:

```text
99%
```

이상 확신했는데 틀린 경우이다.

---

# 104. 중요한 이유

```text
Model이 무엇을 잘못 학습했는가?
```

를 찾는 데 도움이 된다.

---

# 105. 질문

```text
Label이 잘못되었는가?

Outlier인가?

Data Quality 문제인가?

Shortcut Learning인가?

Training Data가 부족한 영역인가?
```

를 확인한다.

---

# 106. Low-confidence Error

Decision Boundary 근처에서 발생한 Error이다.

---

# 107. Probability도 Data다

Prediction 결과를:

```text
0

1
```

만 보지 않는다.

---

# 108. 함께 저장

```text
True Label

Predicted Label

Probability

Confidence

Error Type
```

을 저장하면 분석하기 좋다.

---

# 109. Step 17 - Explainability

Tabular Model에서는:

```text
Feature Importance

Permutation Importance

SHAP
```

등을 사용할 수 있다.

---

# 110. Image Model에서는

```text
Grad-CAM
```

같은 방법을 사용할 수 있다.

---

# 111. Explainability의 질문

```text
Model이 무엇을 이용해서
Prediction했는가?
```

이다.

---

# 112. Feature Importance

```text
어떤 Feature가 Model에서
상대적으로 많이 사용되었는가?
```

를 확인한다.

---

# 113. SHAP

```text
특정 Feature가 Prediction을
어느 방향으로 얼마나 변화시켰는가?
```

를 분석한다.

---

# 114. Grad-CAM

```text
CNN이 Image의 어느 영역을
Prediction에 활용했는가?
```

를 탐색한다.

---

# 115. 중요한 한계

```text
SHAP
≠
Causality

Grad-CAM
≠
병변의 정답
```

이다.

---

# 116. Explainability는 증명 도구가 아니다

Model의 동작을 이해하고 이상 Pattern을 찾는 데 도움을 주는 분석 도구이다.

---

# 117. Step 18 - Shortcut Learning

Image AI에서 매우 중요한 문제이다.

---

# 118. Model이 학습할 수 있는 쉬운 단서

```text
Brightness

Border

Text Marker

Position

Scanner

Hospital

Crop Style
```

등이다.

---

# 119. Chest X-ray 프로젝트

Class별 평균 Brightness 차이가 관찰되었다.

---

# 120. 질문

```text
Model이 폐렴 Pattern이 아니라
Brightness를 이용하고 있는 것은 아닐까?
```

를 생각해야 한다.

---

# 121. 이를 확인하는 방법

```text
Brightness Distribution

Failure Analysis

Grad-CAM

Source별 Performance

External Validation
```

등을 사용할 수 있다.

---

# 122. Step 19 - Overfitting

Training 성능은 좋아지는데 Validation 성능이 나빠지면 Overfitting 가능성이 있다.

---

# 123. 대응

```text
Early Stopping

Weight Decay

Dropout

Data Augmentation

Simpler Model

More Data

Transfer Learning
```

등을 고려할 수 있다.

---

# 124. Image Project

Pretrained Model에서는:

```text
Freeze

↓

Classifier Training

↓

Partial Unfreeze

↓

Fine-tuning
```

전략을 사용할 수 있다.

---

# 125. Fine-tuning

Pretrained Feature를 활용하면서 새로운 Dataset에 Model을 적응시키는 과정이다.

---

# 126. 낮은 Learning Rate

Fine-tuning에서는 Pretrained Weight를 지나치게 크게 변경하지 않도록 낮은 Learning Rate를 사용하는 경우가 많다.

---

# 127. Step 20 - Data Augmentation

Image Dataset을 다양하게 보이도록 변형한다.

---

# 128. 목적

```text
Overfitting 감소

Generalization 개선

작은 Image 변화에 대한 강건성
```

이다.

---

# 129. 하지만 의료영상

Augmentation이 해부학적 또는 병리학적 의미를 훼손하면 안 된다.

---

# 130. 따라서

```text
이 변환이 실제로 가능한가?

Label이 유지되는가?
```

를 확인한다.

---

# 131. Step 21 - Model Selection

최종 Model은:

```text
Validation Accuracy가 가장 높은 Model
```

만으로 선택하지 않는다.

---

# 132. 함께 고려

```text
Validation Loss

Precision

Recall

F1

Macro F1

ROC-AUC

PR-AUC

Stability

Failure Cases

Inference Cost
```

등이다.

---

# 133. Best Model과 Last Model

마지막 Epoch의 Model이 항상 가장 좋은 Model은 아니다.

---

# 134. Best Checkpoint

Validation Metric을 기준으로 가장 좋은 시점의 Weight를 저장한다.

---

# 135. Step 22 - Test Inference

최종 Model이 정해진 후 Test Data를 Prediction한다.

---

# 136. Image Test Loader

```text
shuffle=False
```

를 사용하여 ID 순서를 유지한다.

---

# 137. Prediction 확인

```text
Prediction Distribution

Probability Distribution

Confidence

NaN 여부

Row 수
```

를 확인한다.

---

# 138. Competition Submission

```text
Test Data
↓
Final Model
↓
Prediction
↓
sample_submission 형식
↓
submission.csv
```

이다.

---

# 139. 제출 전 Checklist

```text
Test와 Row 수가 같은가?

ID 순서가 같은가?

Column 이름이 맞는가?

Prediction Type이 맞는가?

index=False인가?
```

를 확인한다.

---

# 140. Step 23 - Reproducibility

같은 Experiment를 다시 실행할 수 있어야 한다.

---

# 141. 기록

```text
Random Seed

Train / Validation Split

Library Version

Model

Hyperparameter

Feature

Preprocessing

Metric

Threshold
```

을 기록한다.

---

# 142. Random Seed

예:

```python
import random
import numpy as np
import torch


random.seed(
    42
)

np.random.seed(
    42
)

torch.manual_seed(
    42
)
```

---

# 143. GPU

필요하다면:

```python
torch.cuda.manual_seed_all(
    42
)
```

도 사용할 수 있다.

---

# 144. 완전한 재현성

GPU 연산이나 Library 구현에 따라 Seed만으로 완전히 동일한 결과가 항상 보장되는 것은 아니다.

---

# 145. Environment 기록

예:

```text
Python Version

PyTorch Version

CUDA Version

GPU

scikit-learn Version
```

등을 기록한다.

---

# 146. Step 24 - Model 저장

Model Weight만 저장한다고 프로젝트가 완성되는 것은 아니다.

---

# 147. 함께 저장할 정보

```text
Model Architecture

Preprocessing

Feature List

Class Mapping

Threshold

Image Size

Normalization

Library Version
```

등이다.

---

# 148. 왜?

Training과 실제 Inference 환경이 달라지면 Prediction 결과가 달라질 수 있기 때문이다.

---

# 149. Step 25 - GitHub

AI 프로젝트도 Code Version 관리가 필요하다.

---

# 150. Git에 올릴 것

```text
Source Code

README

requirements.txt

Small Example Data

Experiment Documentation

Configuration
```

등이다.

---

# 151. 일반적으로 주의할 것

```text
Large Raw Dataset

Patient Data

API Key

Password

Secret

Large Model Weight
```

등을 일반 Git Repository에 그대로 올리지 않는다.

---

# 152. `.gitignore`

예:

```gitignore
.env

__pycache__/

.ipynb_checkpoints/

data/

models/

*.pt

*.pth
```

프로젝트 목적에 따라 조정한다.

---

# 153. 의료 Data

개인정보나 민감한 환자 Data는 공개 Repository에 올리지 않는다.

---

# 154. Step 26 - README

Project README에는 최소한 다음을 설명한다.

```text
Project 목적

Dataset

Task

Workflow

Model

Evaluation Metric

Experiments

Result

Limitations

How to Run
```

---

# 155. README 예시 구조

```text
# Project Name

## Problem

## Dataset

## EDA

## Preprocessing

## Model

## Experiments

## Evaluation

## Failure Analysis

## Results

## Limitations

## Future Work
```

---

# 156. Step 27 - 결과를 과장하지 않는다

예:

```text
Accuracy 99%
```

가 나왔다고:

```text
실제 의료현장에서 99% 정확하다.
```

라고 말하면 안 된다.

---

# 157. 정확한 표현

```text
해당 Validation Split에서
Accuracy 99%가 관찰되었다.
```

처럼 평가 조건을 함께 말한다.

---

# 158. Statistical Association

```text
통계적으로 유의한 관계
```

가 있다고 해서:

```text
원인
```

이라는 의미는 아니다.

---

# 159. Model Importance

Feature Importance가 높다고:

```text
질병의 원인
```

이라고 말할 수 없다.

---

# 160. Explainability

Grad-CAM이 특정 부위를 강조한다고:

```text
실제 병변 위치
```

라고 단정할 수 없다.

---

# 161. Step 28 - External Validation

실제 AI System에서는 새로운 환경에서도 성능이 유지되는지 중요하다.

---

# 162. 예

```text
다른 병원

다른 Scanner

다른 Patient Group

다른 기간
```

의 Data에서 평가한다.

---

# 163. Domain Shift

Training과 실제 사용 환경의 Data Distribution이 다른 현상이다.

---

# 164. Chest X-ray 경험

Local Validation과 Leaderboard 사이의 성능 차이는 Generalization과 Domain Shift를 생각하게 만드는 중요한 경험이었다.

---

# 165. Step 29 - AI를 실제 서비스로 연결

Model이 완성되면 다음 단계는:

```text
Model
↓
API
↓
Web / App
↓
User
```

이다.

---

# 166. 예

```text
PyTorch Model
↓
FastAPI
↓
Prediction API
↓
Web Frontend
```

구조로 연결할 수 있다.

---

# 167. Tabular AI Service

```text
사용자 Data 입력
↓
FastAPI
↓
Preprocessing
↓
ML Model
↓
Probability
↓
Result
```

---

# 168. Image AI Service

```text
Image Upload
↓
FastAPI
↓
Image Preprocessing
↓
CNN
↓
Prediction
↓
Probability
↓
Result
```

---

# 169. Docker

API와 Model을 일정한 환경에서 실행하기 위해 Docker를 사용할 수 있다.

```text
AI Model
+
FastAPI
+
Python Environment
↓
Docker Container
```

---

# 170. 앞으로의 연결

지금까지 공부한 내용을 연결하면:

```text
Python
↓
Data Analysis
↓
Machine Learning
↓
Deep Learning
↓
Computer Vision
↓
AI Project
↓
Web
↓
HTTP / API
↓
FastAPI
↓
Docker
↓
AI Web Service
```

로 이어진다.

---

# 171. 프로젝트마다 반복된 문제

여러 프로젝트에서 계속 등장한 문제는:

```text
File Path

Missing Value

Data Type

Class Imbalance

Data Leakage

Overfitting

Metric 선택

Shape Error

Environment

Package Installation
```

등이었다.

---

# 172. Error는 학습 과정

Error가 발생했다는 것은 프로젝트가 실패했다는 의미가 아니다.

---

# 173. 중요한 것은

```text
Error Message를 읽고

어느 단계에서 발생했는지 찾고

Input과 Output을 확인하고

한 단계씩 검증하는 것
```

이다.

---

# 174. Debugging 기본 원칙

```text
전체 Code 실행
↓
Error
↓
무작정 수정
```

보다:

```text
작은 단위 확인
↓
Shape
↓
Type
↓
Value
↓
Path
↓
한 단계 실행
```

방식이 좋다.

---

# 175. Tabular Debugging

```python
print(
    df.shape
)

print(
    df.columns
)

print(
    df.dtypes
)

print(
    df.isnull().sum()
)
```

---

# 176. Deep Learning Debugging

```python
print(
    images.shape
)

print(
    labels.shape
)

print(
    images.dtype
)

print(
    images.device
)
```

---

# 177. One Batch Test

```text
Dataset 한 Sample

↓

DataLoader 한 Batch

↓

Model Forward

↓

Loss

↓

Backward
```

까지 확인한 뒤 전체 Training을 시작한다.

---

# 178. 프로젝트에서 가장 중요한 습관

결과만 저장하지 않는다.

---

# 179. 기록할 것

```text
무엇을 바꿨는가?

왜 바꿨는가?

결과는 무엇인가?

예상과 같았는가?

다음 Experiment는 무엇인가?
```

를 기록한다.

---

# 180. Experiment Note 예

```text
Experiment
Weighted Loss

Reason
Class 0 Sample이 적음

Hypothesis
Class 0 Recall이 개선될 것이다.

Result
Macro F1과 Class 0 Recall 확인

Conclusion
Baseline과 비교 후 유지 여부 결정
```

---

# 181. 좋은 AI 프로젝트

좋은 AI 프로젝트는:

```text
가장 복잡한 Model
```

을 사용한 프로젝트가 아니다.

---

# 182. 더 중요한 것

```text
문제를 명확하게 정의하고

Data를 이해하고

Leakage를 방지하고

Baseline을 만들고

공정하게 실험하고

적절한 Metric으로 평가하고

실패 사례를 분석하고

결과의 한계를 설명할 수 있는 것
```

이다.

---

# 183. 프로젝트 발전 단계

처음에는:

```text
Code가 실행되는가?
```

가 중요했다.

---

# 184. 다음 단계

```text
Model 성능이 얼마나 나오는가?
```

를 보기 시작했다.

---

# 185. 그 다음

```text
왜 이 성능이 나왔는가?
```

를 보기 시작했다.

---

# 186. 더 나아가

```text
이 Validation 결과를 믿을 수 있는가?
```

를 생각하게 된다.

---

# 187. 마지막으로

```text
실제 환경에서도 사용할 수 있는가?
```

를 생각해야 한다.

---

# 188. AI 학습 단계

```text
1단계
Code 실행

↓

2단계
Model Training

↓

3단계
Metric 이해

↓

4단계
Experiment

↓

5단계
Failure Analysis

↓

6단계
Leakage / Generalization

↓

7단계
Explainability

↓

8단계
Deployment
```

으로 발전할 수 있다.

---

# 189. 프로젝트 시작 Template

새로운 프로젝트를 시작할 때 다음 질문을 사용한다.

```text
1. 문제는 무엇인가?

2. Target은 무엇인가?

3. Task는 Classification인가 Regression인가?

4. Prediction 시점은 언제인가?

5. 사용할 수 있는 Feature는 무엇인가?

6. Leakage 가능성은 없는가?

7. Data 크기는?

8. Missing은?

9. Class Imbalance는?

10. Split 방법은?

11. Baseline은?

12. Metric은?

13. 첫 번째 가설은?

14. 어떤 Experiment를 할 것인가?

15. Failure Case는?

16. Explainability가 필요한가?

17. Test는 언제 사용할 것인가?

18. Model과 Preprocessing은 어떻게 저장할 것인가?

19. 실제 서비스에서는 어떻게 사용할 것인가?

20. 한계는 무엇인가?
```

---

# 190. 프로젝트 완료 Checklist

```text
[ ] 문제 정의

[ ] Data 구조 확인

[ ] Target 확인

[ ] Missing 확인

[ ] Duplicate 확인

[ ] Leakage 확인

[ ] Split 결정

[ ] Baseline 생성

[ ] Metric 결정

[ ] Experiment 기록

[ ] Model 비교

[ ] Failure Analysis

[ ] Explainability

[ ] Best Model 저장

[ ] Test Evaluation

[ ] Result 해석

[ ] Limitations 작성

[ ] README 작성

[ ] requirements 기록
```

---

# 191. 최종 교훈

AI Project는:

```text
Data
→ Model
→ Accuracy
```

세 단계로 끝나는 것이 아니다.

실제 과정은:

```text
Problem
↓
Data
↓
EDA
↓
Cleaning
↓
Split
↓
Leakage Check
↓
Baseline
↓
Metric
↓
Experiment
↓
Validation
↓
Failure Analysis
↓
Explainability
↓
Generalization
↓
Inference
↓
Deployment
↓
Monitoring
```

처럼 이어진다.

---

# 192. 가장 중요한 질문

Model 성능이 높게 나왔을 때:

```text
점수가 높다!
```

에서 끝내지 않는다.

다음 질문을 한다.

```text
왜 높게 나왔지?

Data Leakage는 없나?

Validation Split은 적절한가?

새로운 Data에서도 유지될까?

어떤 Sample에서 틀리는가?

왜 틀리는가?

Probability는 믿을 수 있는가?

실제 사용자는 이 결과를 어떻게 사용할까?
```

---

# 193. 이 Chapter의 결론

여러 AI 프로젝트를 진행하면서 가장 중요한 것은 특정 Library나 특정 Model 하나를 외우는 것이 아니었다.

핵심은:

```text
문제를 정의하고

Data를 이해하고

검증 가능한 실험을 만들고

결과를 올바르게 해석하고

실패 원인을 찾아

다음 Experiment로 연결하는 능력
```

이다.

이 Workflow를 이해하면 새로운 Dataset이나 새로운 Model을 만나더라도 같은 원칙을 적용할 수 있다.