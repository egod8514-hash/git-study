# AI Project Workflow

## 1. AI 프로젝트란?

AI 프로젝트는 단순히 Model을 Training해서 높은 Accuracy를 만드는 과정이 아니다.

전체 흐름은 다음과 같다.

```text
문제 정의
↓
Data 이해
↓
EDA
↓
Data Cleaning
↓
Preprocessing
↓
Feature Engineering
↓
Train / Validation Split
↓
Baseline Model
↓
Evaluation
↓
문제점 분석
↓
가설 설정
↓
Experiment
↓
Model 비교
↓
Failure Analysis
↓
Final Model
↓
Test / Inference
↓
결과 해석
↓
Deployment
```

---

# 2. 가장 먼저 해야 하는 것

Model부터 만드는 것이 아니다.

먼저 질문해야 한다.

```text
무엇을 해결하려는가?

무엇을 Prediction하려는가?

Target은 무엇인가?

어떤 Data를 사용할 수 있는가?

Classification인가?

Regression인가?

Image 문제인가?

Tabular 문제인가?
```

---

# 3. 문제 정의

예를 들어:

```text
환자의 의료정보를 이용하여
임신 성공 여부를 예측한다.
```

라고 정의할 수 있다.

이 경우:

```text
Input
→ 환자 및 시술 관련 Feature

Target
→ 임신 성공 여부

Task
→ Binary Classification
```

이다.

---

# 4. 또 다른 예

```text
Chest X-ray를 이용하여
Normal과 Pneumonia를 분류한다.
```

이면:

```text
Input
→ X-ray Image

Target
→ 0 / 1

Task
→ Image Binary Classification
```

이다.

---

# 5. 문제 정의가 중요한 이유

문제 정의에 따라:

```text
Data

Preprocessing

Model

Loss

Metric

Evaluation
```

이 모두 달라진다.

---

# 6. Data 확인

Data를 받으면 바로 Model에 넣지 않는다.

먼저 구조를 확인한다.

Tabular Data:

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

display(
    df.head()
)
```

---

# 7. Image Data

Image Dataset이라면:

```text
Image 개수

Image Size

Channel

File Format

Label Distribution

파일 경로
```

를 확인한다.

---

# 8. Data Dictionary

가능하면 Data 명세서를 먼저 확인한다.

```text
Column Name

의미

Data Type

단위

Category 의미

Missing 의미
```

를 이해한다.

---

# 9. 의료 Data에서는 특히 중요

예를 들어:

```text
0

1
```

이라는 값만 보고 의미를 추측하면 안 된다.

반드시:

```text
0이 무엇인가?

1이 무엇인가?
```

를 확인한다.

---

# 10. EDA

EDA:

```text
Exploratory Data Analysis
```

즉:

```text
탐색적 데이터 분석
```

이다.

---

# 11. EDA의 목적

```text
Data가 어떤 구조인가?

Missing Value가 있는가?

Outlier가 있는가?

Class Imbalance가 있는가?

Feature Distribution은 어떤가?

Target과 관계가 있는 Feature는 무엇인가?

이상한 값이 있는가?
```

를 찾는다.

---

# 12. 기본 EDA

```python
df.info()
```

```python
df.describe()
```

```python
df.isnull().sum()
```

```python
df[
    "target"
].value_counts()
```

등으로 시작할 수 있다.

---

# 13. EDA는 그래프만 그리는 것이 아니다

EDA는:

```text
Data를 이해하고
다음 의사결정을 하기 위한 과정
```

이다.

---

# 14. 예

EDA 결과:

```text
Class 0 = 90%

Class 1 = 10%
```

을 발견했다.

그러면:

```text
Accuracy만 사용하면 괜찮을까?

Stratified Split이 필요한가?

Class Weight가 필요한가?

Recall / F1도 봐야 하지 않을까?
```

라는 다음 질문으로 이어진다.

---

# 15. Data Cleaning

Data에 문제가 있으면 정리한다.

대표적으로:

```text
Missing Value

Outlier

잘못된 Type

중복 Data

이상한 문자열

잘못된 Label
```

등이다.

---

# 16. Missing Value

먼저 확인한다.

```python
missing = (
    df.isnull().sum()
)


missing = missing[
    missing > 0
]


print(
    missing.sort_values(
        ascending=False
    )
)
```

---

# 17. Missing 처리

방법:

```text
삭제

평균 대체

중앙값 대체

최빈값 대체

Unknown Category

Missing Indicator

Model 자체 처리
```

등이 있다.

---

# 18. 무조건 하나의 방법이 정답은 아니다

예:

```text
Missing = 0
```

으로 무조건 바꾸면 실제 의미가 달라질 수 있다.

Missing이 왜 발생했는지 먼저 생각한다.

---

# 19. Outlier

대표적인 방법:

```text
IQR

Z-score

Boxplot

Domain Knowledge
```

등으로 확인한다.

---

# 20. 중요한 점

Outlier라고 해서 무조건 삭제하지 않는다.

의료 Data에서는 극단적인 값이 실제 중요한 환자 정보일 수도 있다.

---

# 21. Data Type

예:

```text
"120"

"135"

"??"
```

가 하나의 Column에 섞여 있으면 숫자 분석이 제대로 되지 않을 수 있다.

---

# 22. Numeric 변환

```python
df[
    "SBP"
] = pd.to_numeric(
    df[
        "SBP"
    ],
    errors="coerce"
)
```

처럼 잘못된 문자열을 NaN으로 바꾼 후 처리할 수 있다.

---

# 23. Preprocessing

Model이 Data를 사용할 수 있는 형태로 변환한다.

Tabular:

```text
Missing 처리

Encoding

Scaling
```

Image:

```text
Resize

Tensor 변환

Normalize
```

등이다.

---

# 24. Categorical Feature

문자 Category는 Model에 따라 Encoding이 필요할 수 있다.

예:

```text
Male

Female
```

을:

```text
One-Hot Encoding
```

등으로 변환할 수 있다.

---

# 25. Scaling

예:

```text
Age
20 ~ 90

Income
1,000,000 ~ 100,000,000
```

처럼 Feature Scale이 크게 다를 수 있다.

---

# 26. StandardScaler

```python
from sklearn.preprocessing import (
    StandardScaler
)


scaler = StandardScaler()
```

---

# 27. 중요한 원칙

```text
Train Data로 fit

Validation / Test는 transform만
```

한다.

---

# 28. 잘못된 방법

```text
전체 Data
↓
Scaler.fit()
↓
Train / Validation Split
```

은 Validation 정보가 Training 과정에 들어갈 수 있다.

---

# 29. Data Leakage

AI 프로젝트에서 매우 중요한 개념이다.

```text
Prediction 시점에는 알 수 없는 정보가
Training 과정에 들어가는 것
```

을 주의해야 한다.

---

# 30. Leakage 예

```text
Target 정보를 직접 포함한 Feature

Test Data를 이용한 Scaling

전체 Data를 이용한 Missing 처리 기준

같은 Patient가 Train / Validation에 존재

중복 Image가 Train / Validation에 존재
```

등이다.

---

# 31. Feature Engineering

기존 Feature를 이용해 새로운 의미 있는 Feature를 만든다.

예:

```text
Weight + Height
↓
BMI
```

---

# 32. BMI

```text
BMI
=
Weight
/
Height²
```

이다.

---

# 33. 다른 예

```text
배아 수

난자 수
```

가 있다면:

```text
배아 / 난자 비율
```

같은 Feature를 만들 수 있다.

---

# 34. Feature Engineering의 목적

단순히 Column 수를 늘리는 것이 아니다.

```text
Model이 Pattern을 더 잘 학습할 수 있도록
의미 있는 정보를 표현
```

하는 것이 목적이다.

---

# 35. Domain Knowledge

Feature Engineering에서는 Domain Knowledge가 중요하다.

의료 Data라면:

```text
의학적 의미

측정 단위

시간 관계

환자 상태
```

를 고려한다.

---

# 36. Feature를 많이 만들면 좋은가?

아니다.

의미 없는 Feature를 많이 만들면:

```text
Noise 증가

Overfitting

복잡도 증가
```

가 발생할 수 있다.

---

# 37. Train / Validation Split

Model을 Training하기 전에 Data를 분리한다.

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

# 38. Train

```text
Model이 학습하는 Data
```

이다.

---

# 39. Validation

```text
Experiment 비교

Hyperparameter 선택

Threshold 선택

Best Model 선택
```

등에 사용한다.

---

# 40. Test

최종 Model의 Generalization을 확인한다.

Competition에서는 Test Label을 제공하지 않는 경우가 많다.

---

# 41. Stratify

Classification에서 Class 비율을 유지하며 Split하는 데 사용할 수 있다.

```python
stratify=y
```

---

# 42. 하지만 의료 Data

같은 Patient에게 여러 Record가 있다면 단순 Stratified Split보다 Patient-level Split이 더 중요할 수 있다.

---

# 43. Baseline

처음부터 가장 복잡한 Model을 만들지 않는다.

먼저 간단한 기준 Model을 만든다.

이를:

```text
Baseline
```

이라고 한다.

---

# 44. 왜 Baseline이 필요한가?

나중에:

```text
Feature Engineering

Model 변경

Hyperparameter Tuning

Augmentation

Fine-tuning
```

을 했을 때 실제로 개선되었는지 비교할 기준이 필요하기 때문이다.

---

# 45. Tabular Baseline

예:

```text
Logistic Regression

Random Forest
```

등으로 시작할 수 있다.

---

# 46. Image Baseline

예:

```text
Simple CNN

또는

Pretrained ResNet18
```

등으로 시작할 수 있다.

---

# 47. Baseline 결과 기록

예:

```text
Baseline

Accuracy = 0.85

Recall = 0.62

F1 = 0.68

ROC-AUC = 0.81
```

처럼 기록한다.

---

# 48. Model Evaluation

Classification에서는:

```text
Accuracy

Precision

Recall

F1

ROC-AUC

PR-AUC

Confusion Matrix
```

등을 사용할 수 있다.

---

# 49. Regression에서는

```text
MAE

MSE

RMSE

R²
```

등을 사용할 수 있다.

---

# 50. Metric 선택

Metric은:

```text
어떤 Model이 좋아 보이는가?
```

가 아니라:

```text
Project 목적에 어떤 Error가 중요한가?
```

를 기준으로 선택한다.

---

# 51. 의료 Classification

예:

```text
질환 환자를 놓치는 Error
```

가 중요하다면 Recall / Sensitivity를 중요하게 볼 수 있다.

하지만 Specificity와 False Positive도 함께 확인해야 한다.

---

# 52. Baseline 이후

이제 문제점을 찾는다.

예:

```text
Accuracy는 높은데
Minority Class Recall이 낮다.
```

---

# 53. 문제 정의

```text
Class Imbalance 때문에
Minority Class를 잘 찾지 못하는 것 같다.
```

라고 생각할 수 있다.

---

# 54. 이것은 아직 결론이 아니다

현재는:

```text
Hypothesis
```

즉 가설이다.

---

# 55. Experiment

가설을 검증한다.

예:

```text
Experiment A
Class Weight 적용
```

---

# 56. 결과 비교

```text
Baseline

Recall = 0.62

Weighted Loss

Recall = 0.75
```

처럼 같은 Validation Data에서 비교한다.

---

# 57. 중요한 원칙

가능하면 한 번에 하나의 핵심 요소를 변경한다.

---

# 58. 나쁜 실험

한 번에:

```text
Model 변경

Feature 변경

Scaling 변경

Sampling 변경

Hyperparameter 변경
```

을 모두 한다.

---

# 59. 왜 문제?

성능이 올라가도:

```text
무엇 때문에 올라갔는가?
```

를 알기 어렵다.

---

# 60. 좋은 실험

```text
Baseline

↓

Experiment A
Feature Engineering만 추가

↓

Experiment B
Class Weight만 추가

↓

Experiment C
Model만 변경
```

처럼 비교한다.

---

# 61. Experiment Table

```text
Experiment | Change | Accuracy | Recall | F1 | ROC-AUC

Baseline   | -      | 0.85     | 0.62   | 0.68 | 0.81

Exp A      | Weight | 0.83     | 0.75   | 0.72 | 0.84

Exp B      | XGB    | 0.87     | 0.70   | 0.74 | 0.86
```

처럼 기록할 수 있다.

---

# 62. Cross Validation

한 번의 Train / Validation Split 결과만으로 Model을 평가하면 Split에 따라 결과가 달라질 수 있다.

그래서:

```text
K-Fold Cross Validation
```

을 사용할 수 있다.

---

# 63. Classification

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

# 64. Fold

```text
Fold 1

Fold 2

Fold 3

Fold 4

Fold 5
```

의 성능을 구한다.

---

# 65. 최종

```text
Mean Score

Standard Deviation
```

을 함께 본다.

---

# 66. Hyperparameter Tuning

Model 구조는 같지만 설정값을 바꾸며 좋은 조합을 찾는다.

예:

```text
Learning Rate

Tree Depth

Number of Trees

Regularization

C

Batch Size
```

등이다.

---

# 67. 방법

```text
GridSearch

RandomizedSearch

Bayesian Optimization

Optuna
```

등을 사용할 수 있다.

---

# 68. 주의

Test Data를 보고 Hyperparameter를 선택하면 안 된다.

---

# 69. Ensemble

여러 Model의 Prediction을 결합할 수 있다.

예:

```text
LightGBM

XGBoost

CatBoost
↓
Average
```

---

# 70. Stacking

```text
Base Models
↓
OOF Prediction
↓
Meta Model
↓
Final Prediction
```

구조도 사용할 수 있다.

---

# 71. OOF

Out-of-Fold Prediction은 각 Training Sample을 그 Sample을 학습하지 않은 Fold Model로 Prediction한 결과이다.

---

# 72. 왜 필요한가?

Stacking에서 Training Data를 그대로 학습한 Model의 Prediction을 Meta Model에 넣으면 지나치게 낙관적인 Prediction이 될 수 있기 때문이다.

---

# 73. Feature Importance

Tree Model에서는:

```text
Feature Importance
```

를 확인할 수 있다.

---

# 74. SHAP

```text
어떤 Feature가
Prediction을 어느 방향으로 얼마나 밀었는가?
```

를 분석하는 데 사용할 수 있다.

---

# 75. 주의

```text
Feature Importance가 높다
=
원인이다
```

가 아니다.

---

# 76. Image Model

Image Classification에서는:

```text
Grad-CAM
```

같은 방법으로 Model Activation을 분석할 수 있다.

---

# 77. Failure Analysis

Model 성능 숫자만 보지 않는다.

```text
어떤 Sample을 틀렸는가?
```

를 확인한다.

---

# 78. Classification Error

```text
False Positive

False Negative
```

를 나누어 확인한다.

---

# 79. Confidence

```text
High-confidence Error

Low-confidence Error
```

도 확인할 수 있다.

---

# 80. Error에서 질문

```text
특정 Feature 값에서 많이 틀리는가?

특정 Patient Group에서 많이 틀리는가?

Missing이 많은 Sample인가?

Image가 너무 어두운가?

특정 촬영 조건인가?

Label 문제가 있는가?
```

를 확인한다.

---

# 81. Failure Analysis의 목적

```text
틀렸다
```

로 끝나는 것이 아니다.

다음 Experiment를 만드는 것이다.

---

# 82. 흐름

```text
Failure 발견
↓
원인 가설
↓
Experiment
↓
재평가
```

이다.

---

# 83. Best Model

Validation 결과를 기반으로 가장 적합한 Model을 선택한다.

---

# 84. Best Model은 Accuracy 최고 Model?

항상 그렇지는 않다.

Project 목적에 따라:

```text
Recall

F1

ROC-AUC

PR-AUC

RMSE
```

등 다른 기준을 사용할 수 있다.

---

# 85. Test Inference

최종 Model을 선택한 후 Test Data를 Prediction한다.

---

# 86. Competition

```text
train.csv
→ Label 있음

test.csv
→ Label 없음

sample_submission.csv
→ 제출 형식
```

구조가 자주 사용된다.

---

# 87. Test.csv의 의미

Test Data가 이미 존재한다고 해서 Prediction 결과가 들어 있는 것은 아니다.

```text
test.csv
=
Model이 Prediction해야 할 Input Data
```

이다.

---

# 88. Test Prediction

```python
test_pred = model.predict(
    X_test
)
```

처럼 Model이 결과를 생성한다.

---

# 89. Submission

```text
Test ID

Prediction
```

형태로 저장한다.

---

# 90. Leaderboard

Competition Server가 숨겨진 Test Label과 제출 Prediction을 비교한다.

---

# 91. Local Validation과 Leaderboard 차이

예:

```text
Local Validation
0.98

Leaderboard
0.91
```

처럼 차이가 날 수 있다.

---

# 92. 이때 질문

```text
Validation Split이 적절했는가?

Overfitting인가?

Leakage가 있는가?

Train/Test Distribution이 다른가?

Leaderboard에 맞춰 과도하게 튜닝했는가?
```

를 확인한다.

---

# 93. Model 저장

Machine Learning:

```python
import joblib


joblib.dump(
    model,
    "model.pkl"
)
```

---

# 94. Deep Learning

```python
torch.save(
    model.state_dict(),
    "model.pt"
)
```

---

# 95. Model만 저장하면 끝?

아니다.

함께 기록해야 한다.

```text
Preprocessing

Feature List

Class Mapping

Threshold

Model Architecture

Hyperparameter

Library Version
```

---

# 96. Reproducibility

같은 Experiment를 다시 실행할 수 있어야 한다.

---

# 97. 기록

```text
Random Seed

Data Version

Split Method

Model

Hyperparameter

Metric

Code Version
```

등을 남긴다.

---

# 98. Git과 연결

AI Project Code도 Git으로 관리할 수 있다.

```text
Experiment Code
↓
Git Commit
↓
변경 기록
```

이다.

---

# 99. 하지만 큰 Dataset은?

대규모 Dataset이나 Model Weight를 일반 Git Repository에 그대로 넣는 것은 적절하지 않을 수 있다.

---

# 100. .gitignore

예:

```gitignore
data/
models/
*.pt
*.pth
*.pkl
```

처럼 관리할 수 있다.

실제 Project 목적에 따라 조정한다.

---

# 101. README

AI Project에는 README를 작성한다.

예:

```text
Project 목적

Dataset

Environment

Folder Structure

Preprocessing

Model

Training 방법

Evaluation

Result

실행 방법
```

을 기록한다.

---

# 102. 발표 흐름

AI Project 발표는 다음처럼 구성할 수 있다.

```text
1. 문제 정의

2. Dataset

3. EDA

4. Data Cleaning

5. Preprocessing

6. Feature Engineering

7. Baseline

8. Experiment

9. Evaluation

10. Failure Analysis

11. Final Model

12. 결과 및 한계
```

---

# 103. 좋은 발표

단순히:

```text
Accuracy 95%가 나왔습니다.
```

로 끝나지 않는다.

---

# 104. 더 좋은 설명

```text
Baseline에서 Minority Class Recall이 낮았습니다.

Class Imbalance를 원인 중 하나로 가정하고
Weighted Loss를 적용했습니다.

동일한 Validation Set에서 비교한 결과
Recall이 개선되었습니다.

다만 False Positive가 증가하는 Trade-off가 있었습니다.
```

처럼 설명한다.

---

# 105. Project에서 중요한 것

```text
최고 점수
```

만이 아니다.

---

# 106. 더 중요한 것

```text
왜 이 방법을 사용했는가?

어떤 문제가 있었는가?

어떤 가설을 세웠는가?

어떻게 검증했는가?

무엇이 개선되었는가?

어떤 한계가 있는가?
```

를 설명할 수 있어야 한다.

---

# 107. 실패한 Experiment도 기록

성능이 떨어졌다고 쓸모없는 실험은 아니다.

예:

```text
Experiment B

Augmentation 강화

↓

Validation F1 감소
```

---

# 108. 여기서 배울 수 있는 것

```text
Augmentation이 너무 강했을 가능성

Task에 적합하지 않은 변환

Training 난이도 증가
```

등의 새로운 가설을 만들 수 있다.

---

# 109. 실패도 결과

AI 프로젝트에서는:

```text
가설
↓
실험
↓
실패
↓
원인 분석
↓
다음 실험
```

도 정상적인 과정이다.

---

# 110. Project의 핵심 구조

```text
문제
↓
Data
↓
Baseline
↓
문제 발견
↓
가설
↓
Experiment
↓
Evaluation
↓
Failure Analysis
↓
개선
```

이다.

---

# 111. Tabular AI Workflow

```text
CSV
↓
EDA
↓
Missing / Outlier
↓
Feature Engineering
↓
Encoding / Scaling
↓
Train / Validation
↓
ML Model
↓
Cross Validation
↓
Evaluation
↓
SHAP
↓
Inference
```

---

# 112. Image AI Workflow

```text
Images
↓
Image EDA
↓
Patient-level Split
↓
Resize / Normalize
↓
DataLoader
↓
CNN
↓
Transfer Learning
↓
Fine-tuning
↓
Evaluation
↓
Failure Case
↓
Grad-CAM
↓
Inference
```

---

# 113. 의료 AI Workflow

```text
Clinical Problem
↓
Data / Label Definition
↓
Patient Independence
↓
EDA
↓
Leakage Check
↓
Baseline
↓
Model
↓
Sensitivity / Specificity
↓
Failure Analysis
↓
Interpretability
↓
Generalization
```

---

# 114. 내가 프로젝트를 볼 때의 질문

새 Project를 시작하면 다음 순서로 생각한다.

```text
1. Target이 무엇인가?

2. Input Feature는 무엇인가?

3. Classification / Regression인가?

4. Data 크기는 얼마인가?

5. Missing이 있는가?

6. Class Imbalance가 있는가?

7. Leakage 가능성이 있는가?

8. 어떤 Split을 사용할 것인가?

9. Baseline은 무엇인가?

10. 어떤 Metric을 사용할 것인가?

11. Baseline의 문제는 무엇인가?

12. 어떤 가설을 세울 것인가?

13. 어떤 Experiment를 할 것인가?

14. Error는 어디에서 발생하는가?

15. 최종 Model을 어떻게 검증할 것인가?
```

---

# 115. 프로젝트가 꼬였을 때

처음부터 복잡한 Model Code를 계속 수정하지 않는다.

다시 다음 순서로 확인한다.

```text
Data
↓
Shape
↓
Target
↓
Split
↓
Preprocessing
↓
One Batch / Small Sample
↓
Baseline
↓
Full Training
```

---

# 116. Error가 발생했을 때

예:

```text
NameError
```

이면:

```text
변수가 생성되었는가?

이전 Cell을 실행했는가?
```

를 확인한다.

---

# 117. FileNotFoundError

```text
현재 경로

파일명

압축 해제 위치

os.listdir()
```

을 확인한다.

---

# 118. ModuleNotFoundError

```text
Package 설치 여부

현재 Python Environment

현재 Jupyter Kernel
```

을 확인한다.

---

# 119. Shape Error

Deep Learning:

```text
Input Shape

Batch Shape

Channel

Flatten 결과

Linear in_features

Model Output Shape
```

를 확인한다.

---

# 120. AI Project Debugging 원칙

```text
한꺼번에 전체 Code를 의심하지 않는다.

Data가 제대로 들어오는지부터
한 단계씩 확인한다.
```

---

# 121. 좋은 Debugging

```text
파일을 읽을 수 있는가?

↓

DataFrame Shape이 맞는가?

↓

Target이 맞는가?

↓

Split이 되는가?

↓

한 Batch가 나오는가?

↓

Model Forward가 되는가?

↓

Loss 계산이 되는가?

↓

Backward가 되는가?

↓

1 Epoch가 되는가?

↓

전체 Training
```

이다.

---

# 122. Project 결과 정리

최종적으로 다음을 남긴다.

```text
Problem

Dataset

EDA

Preprocessing

Feature Engineering

Model

Experiment

Metric

Result

Failure Analysis

Limitation

Next Step
```

---

# 123. Limitation

한계를 숨기지 않는다.

예:

```text
Dataset 크기가 작음

External Validation 없음

특정 기관 Data만 사용

Class Imbalance

Label Quality 불확실

일부 Feature Missing이 많음
```

등이다.

---

# 124. Next Step

예:

```text
더 많은 Data 확보

External Dataset 검증

Feature Engineering 개선

Hyperparameter Tuning

Ensemble

Calibration

Model Deployment
```

등이다.

---

# 125. AI Project의 진짜 결과

Project의 결과는:

```text
Model File 하나
```

만이 아니다.

---

# 126. 함께 남아야 할 것

```text
Code

Data 이해

Experiment 기록

Model

Metric

Failure Analysis

문서

발표자료

Git History
```

이다.

---

# 127. 전체 흐름 복습

```text
Problem Definition
↓
Data Understanding
↓
EDA
↓
Cleaning
↓
Preprocessing
↓
Feature Engineering
↓
Split
↓
Baseline
↓
Evaluation
↓
Hypothesis
↓
Experiment
↓
Cross Validation
↓
Tuning
↓
Model Comparison
↓
Failure Analysis
↓
Final Model
↓
Test Inference
↓
Interpretation
↓
Documentation
↓
Deployment
```

---

# 128. 가장 중요한 원칙

```text
Model부터 만들지 않는다.

Data부터 이해한다.
```

그리고:

```text
점수만 보지 않는다.

왜 그런 결과가 나왔는지 분석한다.
```

그리고:

```text
무작정 개선하지 않는다.

문제 → 가설 → 실험 → 비교
순서로 진행한다.
```

---

# 129. 한 줄 정리

AI 프로젝트는:

```text
Data를 이해하고
문제를 정의한 뒤
Baseline을 만들고

문제점을 발견하여
가설과 실험을 반복하고

최종적으로 새로운 Data에서도
일반화할 수 있는 Model을 만드는 과정
```

이다.