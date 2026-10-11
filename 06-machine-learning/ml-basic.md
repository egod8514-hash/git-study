# Machine Learning 기초

## 1. Machine Learning이란?

Machine Learning은 데이터를 이용해 컴퓨터가 규칙이나 패턴을 학습하고 새로운 데이터에 대한 예측이나 판단을 수행하도록 만드는 방법이다.

한국어로는:

```text
Machine Learning
→ 머신러닝
→ 기계학습
```

이라고 한다.

---

# 2. 일반적인 프로그램과 머신러닝

일반적인 프로그램에서는 사람이 규칙을 직접 작성한다.

예:

```python
if temperature >= 38:
    result = "고열"
else:
    result = "정상"
```

사람이 직접:

```text
38도 이상이면 고열
```

이라는 규칙을 만들었다.

구조:

```text
Data
+
사람이 만든 Rule
        ↓
      Result
```

이다.

---

# 3. Machine Learning에서는?

머신러닝에서는 많은 데이터와 정답을 이용하여 모델이 패턴을 학습하도록 한다.

```text
Data
+
Answer
   ↓
Machine Learning
   ↓
Pattern 학습
   ↓
Model
```

학습된 모델에 새로운 데이터를 넣으면:

```text
New Data
   ↓
Model
   ↓
Prediction
```

이 이루어진다.

---

# 4. 예를 들어 질병 예측

다음과 같은 데이터가 있다고 생각해보자.

| age | bmi | glucose | outcome |
|---:|---:|---:|---:|
| 25 | 21.3 | 90 | 0 |
| 48 | 29.5 | 160 | 1 |
| 36 | 24.1 | 110 | 0 |
| 60 | 31.2 | 180 | 1 |

여기서:

```text
age
bmi
glucose
```

를 이용하여:

```text
outcome
```

을 예측하고 싶다.

이것이 하나의 머신러닝 문제가 될 수 있다.

---

# 5. Feature

모델이 예측하기 위해 사용하는 입력 변수를:

```text
Feature
```

라고 한다.

위 예제에서는:

```text
age
bmi
glucose
```

가 Feature이다.

---

# 6. Target

모델이 예측해야 하는 값을:

```text
Target
```

이라고 한다.

위 예제에서는:

```text
outcome
```

이 Target이다.

---

# 7. X와 y

머신러닝 코드에서는 Feature를:

```text
X
```

Target을:

```text
y
```

라고 표현하는 경우가 많다.

예:

```python
X = df[
    [
        "age",
        "bmi",
        "glucose"
    ]
]

y = df[
    "outcome"
]
```

---

# 8. 왜 X는 대문자이고 y는 소문자일까?

일반적으로 X에는 여러 Feature가 들어가므로 2차원 형태가 되는 경우가 많다.

예:

```text
X

age | bmi | glucose
-------------------
25  | 21  | 90
48  | 29  | 160
36  | 24  | 110
```

반면 y는 하나의 Target Column인 경우가 많다.

```text
y

0
1
0
```

그래서 관습적으로:

```text
X
→ Feature Matrix

y
→ Target Vector
```

라고 표현하는 경우가 많다.

반드시 대문자/소문자를 사용해야 하는 문법 규칙은 아니지만 매우 흔한 표기이다.

---

# 9. X의 Shape

예를 들어:

```python
print(
    X.shape
)
```

결과:

```text
(1000, 10)
```

이라면:

```text
1000
→ 데이터 Row 수

10
→ Feature 수
```

이다.

---

# 10. y의 Shape

```python
print(
    y.shape
)
```

결과:

```text
(1000,)
```

이라면 Target 값이 1000개 있다는 의미이다.

---

# 11. Sample

머신러닝에서 하나의 데이터 Row를:

```text
Sample
```

이라고 부르기도 한다.

예:

```text
age = 48
bmi = 29.5
glucose = 160
outcome = 1
```

한 사람의 데이터 하나가 Sample 하나라고 생각할 수 있다.

---

# 12. Dataset

많은 Sample을 모은 데이터를:

```text
Dataset
```

이라고 한다.

예:

```text
1000명의 환자 데이터
```

가 있다면 전체가 하나의 Dataset이 될 수 있다.

---

# 13. Model

Model은 데이터에서 학습한 패턴을 이용하여 예측하는 함수 또는 시스템이라고 생각할 수 있다.

예:

```text
입력

age
bmi
glucose

   ↓

Machine Learning Model

   ↓

결과

Outcome = 1
```

---

# 14. Training

모델이 데이터를 이용하여 패턴을 학습하는 과정을:

```text
Training
```

또는:

```text
학습
```

이라고 한다.

예:

```python
model.fit(
    X_train,
    y_train
)
```

`fit()`은 머신러닝에서 매우 자주 등장한다.

---

# 15. fit()

Scikit-learn 모델에서는 일반적으로:

```python
model.fit(
    X,
    y
)
```

형태로 모델을 학습한다.

예:

```python
from sklearn.linear_model import LogisticRegression

model = LogisticRegression()

model.fit(
    X_train,
    y_train
)
```

---

# 16. Prediction

학습된 모델을 이용해 새로운 데이터의 결과를 예측하는 것을:

```text
Prediction
```

이라고 한다.

Scikit-learn에서는:

```python
model.predict(
    X_test
)
```

를 자주 사용한다.

---

# 17. 기본 흐름

```python
model.fit(
    X_train,
    y_train
)

pred = model.predict(
    X_test
)
```

즉:

```text
fit
→ 학습

predict
→ 예측
```

이다.

---

# 18. Training과 Prediction

전체 흐름:

```text
Training Data
      ↓
   model.fit()
      ↓
학습된 Model
      ↓
   model.predict()
      ↓
   Prediction
```

이다.

---

# 19. 정답이 있는 학습

정답을 이용하여 학습하는 방식을:

```text
Supervised Learning
```

즉:

```text
지도학습
```

이라고 한다.

예:

```text
Feature                 Target

나이, BMI, 혈당    →    당뇨 여부

기업 정보          →    대출 승인 여부

환자 정보          →    수술 실패 여부

응급실 정보        →    오분류 여부
```

등이다.

---

# 20. 지도학습

지도학습에서는:

```text
X
+
y
```

가 존재한다.

즉:

```text
입력
+
정답
```

을 이용하여 모델을 학습한다.

---

# 21. 지도학습의 대표 문제

지도학습은 크게:

```text
Classification
Regression
```

으로 나눠 생각할 수 있다.

---

# 22. Classification

Classification은:

```text
분류
```

문제이다.

결과가 특정 Class 중 하나가 된다.

예:

```text
정상 / 질병

승인 / 거절

고양이 / 강아지

정상 / 폐렴

스팸 / 정상메일
```

---

# 23. Binary Classification

Class가 두 개인 분류를:

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
0 = 정상
1 = 질병
```

---

# 24. Multi-class Classification

Class가 3개 이상인 분류를:

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

여러 장기 종류를 분류하는 이미지 문제 등이 여기에 해당할 수 있다.

---

# 25. Regression

Regression은:

```text
회귀
```

문제이다.

연속적인 숫자 값을 예측한다.

예:

```text
집 가격

매출

온도

환자의 특정 수치

자동차 가격
```

등이다.

---

# 26. Classification vs Regression

분류:

```text
입력
↓
Model
↓
Class
```

예:

```text
0 또는 1
```

회귀:

```text
입력
↓
Model
↓
숫자 값
```

예:

```text
325000000원
```

---

# 27. 문제 유형을 먼저 확인해야 한다

머신러닝을 시작할 때 바로 모델부터 선택하면 안 된다.

먼저:

```text
무엇을 예측하는가?
```

를 확인해야 한다.

Target이:

```text
0 / 1
```

이면 이진 분류일 가능성이 높다.

Target이:

```text
A / B / C
```

라면 다중 분류일 수 있다.

Target이:

```text
152.7
327.5
810.2
```

같은 연속적인 숫자라면 회귀 문제일 수 있다.

---

# 28. 정답이 없는 학습

정답 y 없이 데이터의 구조나 패턴을 찾는 방법도 있다.

이를:

```text
Unsupervised Learning
```

즉:

```text
비지도학습
```

이라고 한다.

---

# 29. 비지도학습 예

대표적인 예:

```text
Clustering
차원 축소
이상 탐지의 일부 방법
```

등이 있다.

예를 들어 고객 데이터를 이용하여 비슷한 고객끼리 그룹화할 수 있다.

```text
고객 데이터
   ↓
Clustering
   ↓
Group A
Group B
Group C
```

---

# 30. Clustering

Clustering은:

```text
군집화
```

이다.

정답 Label 없이 비슷한 데이터끼리 그룹을 만든다.

대표적인 알고리즘 중 하나:

```text
K-Means
```

가 있다.

---

# 31. 지도학습 vs 비지도학습

| 구분 | 지도학습 | 비지도학습 |
|---|---|---|
| 정답 y | 있음 | 없음 |
| 목적 | 예측 | 구조/패턴 탐색 |
| 대표 작업 | 분류, 회귀 | 군집화 등 |
| 예 | 질병 예측 | 고객 그룹화 |

---

# 32. Train Data

모델을 학습시키는 데이터를:

```text
Train Data
```

라고 한다.

예:

```text
전체 1000명
      ↓
800명
→ Train
```

모델은 Train Data를 보고 패턴을 학습한다.

---

# 33. Test Data

모델이 처음 보는 데이터에서 얼마나 잘 예측하는지 확인하기 위한 데이터를:

```text
Test Data
```

라고 한다.

예:

```text
전체 1000명

800명
→ Train

200명
→ Test
```

---

# 34. 왜 Train과 Test를 나눌까?

1000명의 데이터를 모두 학습에 사용하고 같은 데이터로 평가하면 문제가 있다.

```text
문제를 외워놓고
같은 문제로 시험 보는 것
```

과 비슷할 수 있다.

우리가 원하는 것은:

```text
처음 보는 데이터도
잘 예측하는 모델
```

이다.

---

# 35. Generalization

처음 보는 데이터에서도 잘 예측하는 능력을:

```text
Generalization
```

즉:

```text
일반화
```

라고 한다.

좋은 머신러닝 모델의 중요한 목표이다.

---

# 36. 단순 암기가 목적이 아니다

머신러닝의 목표를:

```text
Train Accuracy를 최대한 높인다.
```

라고만 생각하면 안 된다.

중요한 것은:

```text
새로운 데이터에서도
성능이 좋은가?
```

이다.

---

# 37. Overfitting

Train Data에는 매우 잘 맞지만 새로운 데이터에서는 성능이 떨어지는 현상을:

```text
Overfitting
```

즉:

```text
과적합
```

이라고 한다.

예:

```text
Train Accuracy = 99%

Validation Accuracy = 80%
```

라면 과적합 가능성을 의심할 수 있다.

단, 수치 하나만으로 확정하는 것은 아니다.

---

# 38. 시험 문제로 비유

과적합은:

```text
문제와 답을 통째로 외움

↓ 시험

조금 다른 문제가 나오자
풀지 못함
```

과 비슷하다.

머신러닝 모델도 Train Data만 지나치게 외우면 새로운 데이터에 약할 수 있다.

---

# 39. Underfitting

반대로 모델이 데이터의 패턴을 충분히 학습하지 못한 상태를:

```text
Underfitting
```

즉:

```text
과소적합
```

이라고 한다.

예:

```text
Train 성능도 낮음

Validation 성능도 낮음
```

같은 패턴을 볼 수 있다.

---

# 40. Overfitting vs Underfitting

```text
Underfitting
→ 너무 단순하거나 충분히 학습하지 못함

적절한 학습
→ 중요한 패턴을 학습

Overfitting
→ Train Data에 지나치게 맞춤
```

---

# 41. Validation Data

모델을 개발하면서 성능을 비교하고 설정을 선택하기 위한 데이터를:

```text
Validation Data
```

라고 한다.

전체:

```text
Dataset
   ↓
Train
Validation
Test
```

로 나눌 수 있다.

---

# 42. 각각의 역할

```text
Train
→ 모델 학습

Validation
→ 모델/설정 비교 및 선택

Test
→ 마지막 최종 평가
```

라고 생각할 수 있다.

다음 파일에서 더 자세히 배운다.

---

# 43. Test를 계속 보면 안 되는 이유

Test 성능을 계속 확인하면서:

```text
모델 변경

Feature 변경

Hyperparameter 변경
```

을 반복하면 Test 결과도 모델 개발에 간접적으로 사용한 셈이 될 수 있다.

그러면 Test가 더 이상 완전히 새로운 최종 평가 데이터 역할을 하기 어려워진다.

---

# 44. 머신러닝 기본 과정

일반적인 흐름:

```text
문제 정의
   ↓
데이터 수집
   ↓
EDA
   ↓
전처리
   ↓
Feature / Target 설정
   ↓
Train / Validation / Test 분리
   ↓
Model 선택
   ↓
Training
   ↓
Prediction
   ↓
Evaluation
   ↓
개선
```

이다.

---

# 45. EDA가 먼저인 이유

모델을 바로 실행하기 전에 데이터를 확인해야 한다.

예:

```text
결측치가 있는가?

이상치가 있는가?

자료형이 맞는가?

Target 비율은 어떤가?

중복 데이터가 있는가?

데이터 누수 가능성이 있는가?
```

등을 확인한다.

즉 Chapter 04에서 배운 Data Analysis가 Machine Learning 앞에 위치하는 이유가 있다.

---

# 46. 전처리

모델이 데이터를 사용할 수 있도록 정리한다.

예:

```text
결측치 처리

이상치 처리

문자형 변수 Encoding

Scaling

불필요한 Column 제거
```

등이다.

---

# 47. Feature Engineering

기존 Feature를 이용하여 새로운 Feature를 만들 수도 있다.

예:

```python
df["BMI"] = (
    df["weight"]
    /
    (
        df["height_m"] ** 2
    )
)
```

또는:

```text
합계
비율
차이
기간
상호작용
```

등을 만들 수 있다.

---

# 48. Model 선택

문제에 맞는 Model을 선택한다.

분류 예:

```text
Logistic Regression

Random Forest

XGBoost

LightGBM

CatBoost
```

회귀 예:

```text
Linear Regression

Ridge

Random Forest Regressor

XGBoost Regressor
```

등이 있다.

---

# 49. Logistic Regression

이름에는 Regression이 들어가지만:

```text
Logistic Regression
```

은 대표적인 분류 알고리즘이다.

특히:

```text
0 / 1
```

이진 분류에서 많이 사용한다.

---

# 50. Random Forest

Random Forest는 여러 Decision Tree를 이용하는 Ensemble 모델이다.

개념적으로:

```text
Tree 1 ─┐
Tree 2 ─┤
Tree 3 ─┤
Tree 4 ─┤
        ↓
   결과 종합
        ↓
    Prediction
```

이라고 생각할 수 있다.

---

# 51. Boosting 모델

대표적으로:

```text
XGBoost

LightGBM

CatBoost
```

등이 있다.

Tabular Data 머신러닝에서 자주 사용되는 모델들이다.

각 모델의 자세한 내용은 이후 파일에서 다룬다.

---

# 52. Baseline

처음부터 복잡한 모델을 만들기보다 비교 기준이 되는 기본 모델을 먼저 만드는 것이 좋다.

이를:

```text
Baseline
```

이라고 한다.

예:

```text
Baseline Logistic Regression

↓ 개선

Random Forest

↓ 개선

XGBoost

↓ 튜닝

Final Model
```

처럼 실험할 수 있다.

---

# 53. Baseline이 중요한 이유

Baseline이 없으면:

```text
새로운 방법이 정말 좋아진 것인지
```

비교하기 어렵다.

예:

```text
Baseline F1 = 0.72

새 모델 F1 = 0.78
```

이라면 개선 정도를 확인할 수 있다.

---

# 54. Evaluation

모델이 얼마나 잘 예측하는지 평가하는 과정이다.

분류에서 대표적인 Metric:

```text
Accuracy

Precision

Recall

F1 Score

ROC-AUC

PR-AUC
```

등이 있다.

---

# 55. Accuracy

Accuracy는:

```text
전체 예측 중
얼마나 맞았는가?
```

를 나타낸다.

개념:

```text
맞춘 개수
──────────
전체 개수
```

이다.

---

# 56. Accuracy만 보면 안 되는 경우

예를 들어:

```text
정상 = 950명
질병 = 50명
```

이라고 하자.

모델이 전부:

```text
정상
```

이라고 예측하면:

```text
Accuracy = 95%
```

가 될 수 있다.

하지만 질병 환자는 한 명도 찾지 못한다.

따라서 클래스 불균형 문제에서는 Accuracy만 보면 위험할 수 있다.

---

# 57. Class Imbalance

Target의 Class 비율이 크게 차이나는 것을:

```text
Class Imbalance
```

즉:

```text
클래스 불균형
```

이라고 한다.

예:

```text
0 = 95%

1 = 5%
```

이다.

---

# 58. 불균형 데이터에서는?

다음을 함께 확인할 수 있다.

```text
Precision

Recall

F1 Score

ROC-AUC

PR-AUC

Confusion Matrix
```

문제 목적에 따라 중요한 Metric이 달라진다.

---

# 59. 의료 데이터 예

질병 탐지에서:

```text
실제 질병 환자를
얼마나 놓치지 않았는가?
```

가 중요하다면 Recall을 중요하게 볼 수 있다.

하지만 무조건 Recall만 최대화하는 것도 좋은 것은 아니다.

False Positive가 지나치게 증가할 수 있기 때문이다.

따라서 문제 목적과 비용을 함께 고려해야 한다.

---

# 60. Confusion Matrix

이진 분류에서는 다음 네 가지 결과를 생각할 수 있다.

```text
TP
TN
FP
FN
```

예:

```text
실제 질병 + 예측 질병
→ TP

실제 정상 + 예측 정상
→ TN

실제 정상 + 예측 질병
→ FP

실제 질병 + 예측 정상
→ FN
```

이다.

---

# 61. Probability

분류 모델은 Class뿐 아니라 확률 또는 Score를 제공하는 경우가 있다.

예:

```python
prob = model.predict_proba(
    X_test
)
```

결과 예:

```text
정상 확률 = 0.15
질병 확률 = 0.85
```

---

# 62. Threshold

확률을 Class로 바꾸기 위한 기준을:

```text
Threshold
```

라고 한다.

예:

```text
질병 확률 >= 0.5
→ 1

질병 확률 < 0.5
→ 0
```

이다.

---

# 63. Threshold는 항상 0.5여야 할까?

아니다.

문제에 따라:

```text
0.5
0.4
0.3
0.2
```

등을 비교할 수 있다.

Threshold를 낮추면 일반적으로 Positive를 더 많이 예측하게 되며 Recall과 Precision 사이의 균형이 달라질 수 있다.

---

# 64. 모델 학습과 평가를 구분

다음은 서로 다른 작업이다.

```python
model.fit(
    X_train,
    y_train
)
```

```text
모델 학습
```

그리고:

```python
pred = model.predict(
    X_test
)
```

```text
예측
```

그리고:

```python
accuracy_score(
    y_test,
    pred
)
```

```text
평가
```

이다.

---

# 65. Scikit-learn

Python에서 머신러닝을 할 때 많이 사용하는 라이브러리이다.

설치 패키지 이름:

```text
scikit-learn
```

Python Import에서는:

```python
sklearn
```

을 사용한다.

예:

```python
from sklearn.linear_model import (
    LogisticRegression
)
```

---

# 66. 기본 코드 구조

```python
from sklearn.model_selection import (
    train_test_split
)

from sklearn.linear_model import (
    LogisticRegression
)

from sklearn.metrics import (
    accuracy_score
)


X = df.drop(
    columns=["target"]
)

y = df["target"]


X_train, X_test, y_train, y_test = (
    train_test_split(
        X,
        y,
        test_size=0.2,
        random_state=42
    )
)


model = LogisticRegression()


model.fit(
    X_train,
    y_train
)


pred = model.predict(
    X_test
)


score = accuracy_score(
    y_test,
    pred
)


print(score)
```

---

# 67. 위 코드의 흐름

```text
df
 ↓
X / y 분리
 ↓
Train / Test 분리
 ↓
Model 생성
 ↓
fit()
 ↓
predict()
 ↓
Evaluation
```

이다.

---

# 68. model = LogisticRegression()

```python
model = LogisticRegression()
```

은 아직 학습된 모델이 아니다.

단순히:

```text
Logistic Regression 모델 객체 생성
```

단계이다.

---

# 69. model.fit()

```python
model.fit(
    X_train,
    y_train
)
```

을 실행하면서 Train Data를 이용한 학습이 이루어진다.

---

# 70. model.predict()

```python
pred = model.predict(
    X_test
)
```

는 학습된 모델에게:

```text
이 데이터의 정답을 예측해봐.
```

라고 요청하는 것이다.

---

# 71. y_test와 pred

평가할 때:

```text
y_test
→ 실제 정답

pred
→ 모델 예측
```

을 비교한다.

예:

```text
y_test

0
1
1
0
1
```

```text
pred

0
1
0
0
1
```

이 둘을 비교하여 성능을 계산한다.

---

# 72. Parameter

모델이 데이터에서 학습하는 내부 값을:

```text
Parameter
```

라고 한다.

예를 들어 선형 모델의:

```text
Coefficient

Intercept
```

등이 Parameter가 될 수 있다.

---

# 73. Hyperparameter

학습 전에 사람이 설정하는 값을:

```text
Hyperparameter
```

라고 한다.

예:

```text
RandomForest

n_estimators
max_depth
min_samples_split
```

등이다.

---

# 74. Parameter vs Hyperparameter

```text
Parameter
→ 모델이 학습을 통해 결정

Hyperparameter
→ 학습 전에 사람이 설정
```

이라고 이해하면 된다.

---

# 75. Hyperparameter Tuning

여러 Hyperparameter를 비교하여 더 적절한 설정을 찾는 것을:

```text
Hyperparameter Tuning
```

이라고 한다.

예:

```text
max_depth = 3

max_depth = 5

max_depth = 10
```

을 비교할 수 있다.

---

# 76. Tuning 방법

대표적으로:

```text
Grid Search

Random Search

Bayesian Optimization

Optuna
```

등을 사용할 수 있다.

나중에 별도 파일에서 자세히 다룬다.

---

# 77. Cross Validation

한 번의 Train/Validation 분리 결과만 보는 대신 여러 번 데이터를 나누어 모델을 평가할 수 있다.

이를:

```text
Cross Validation
```

즉:

```text
교차검증
```

이라고 한다.

---

# 78. KFold

예를 들어 5-Fold라면:

```text
전체 데이터

Fold 1
Fold 2
Fold 3
Fold 4
Fold 5
```

로 나누어 번갈아 Validation으로 사용한다.

---

# 79. Cross Validation 목적

한 번의 분할 결과에만 지나치게 의존하지 않고 모델의 성능을 더 안정적으로 평가하기 위해 사용할 수 있다.

예:

```text
Fold 1 Accuracy = 0.81
Fold 2 Accuracy = 0.79
Fold 3 Accuracy = 0.83
Fold 4 Accuracy = 0.80
Fold 5 Accuracy = 0.82

평균 = 0.81
```

처럼 볼 수 있다.

---

# 80. StratifiedKFold

분류 문제에서 각 Fold의 Class 비율을 비슷하게 유지하도록 나누는 방법이다.

예:

```text
전체

0 = 75%
1 = 25%
```

각 Fold에서도 가능한 한 비슷한 비율을 유지한다.

불균형 분류에서 자주 사용된다.

---

# 81. Data Leakage

머신러닝에서 매우 중요한 개념이다.

```text
Data Leakage
```

는 모델이 실제 예측 시점에는 알 수 없는 정보나 평가 데이터의 정보를 학습 과정에서 미리 사용해버리는 문제를 말한다.

---

# 82. Leakage 예

전체 데이터에 Scaling을 먼저 적용한 후 Train/Test를 나누는 상황을 생각해볼 수 있다.

잘못된 흐름:

```text
전체 데이터
   ↓
Scaler fit
   ↓
Train/Test Split
```

이 경우 Test Data의 통계 정보가 Scaling 과정에 영향을 줄 수 있다.

---

# 83. 더 안전한 흐름

```text
전체 데이터
   ↓
Train/Test Split
   ↓
Train으로 Scaler fit
   ↓
Train transform
Test transform
```

처럼 Train 기준으로 전처리 정보를 학습해야 한다.

---

# 84. fit과 transform

전처리에서도:

```text
fit
→ 필요한 기준을 학습

transform
→ 학습한 기준으로 데이터 변환
```

이다.

StandardScaler를 예로 들면:

```python
scaler.fit(
    X_train
)
```

에서 Train의 평균과 표준편차 등을 학습하고:

```python
X_train_scaled = scaler.transform(
    X_train
)

X_test_scaled = scaler.transform(
    X_test
)
```

한다.

---

# 85. Pipeline

전처리와 모델을 하나의 흐름으로 묶을 수 있다.

```text
Missing Value 처리
       ↓
Scaling
       ↓
Model
```

Scikit-learn의 Pipeline을 이용하면 Cross Validation에서도 Leakage를 줄이는 데 도움이 된다.

---

# 86. Feature Importance

일부 모델에서는 어떤 Feature가 예측에 많이 사용되었는지 확인할 수 있다.

예:

```text
age        0.12
BMI        0.18
glucose    0.42
blood      0.08
```

같은 결과를 볼 수 있다.

---

# 87. Feature Importance 주의

Feature Importance가 높다고 해서:

```text
이 변수가 질병의 원인이다.
```

라고 바로 결론 내릴 수는 없다.

의미는:

```text
이 모델의 예측 과정에서
이 Feature가 중요하게 사용되었다.
```

에 가깝다.

예측 중요도와 인과관계는 다르다.

---

# 88. SHAP

SHAP은 모델의 예측을 해석하는 데 많이 사용하는 방법 중 하나이다.

예:

```text
어떤 Feature가
예측을 1 방향으로 얼마나 밀었는가?

어떤 Feature가
예측을 0 방향으로 얼마나 밀었는가?
```

등을 분석할 수 있다.

---

# 89. Feature Importance와 SHAP

Feature Importance:

```text
전체적으로
어떤 Feature가 중요했는가?
```

SHAP:

```text
전체 중요도뿐 아니라
개별 예측에서
각 Feature가 어떤 방향으로 영향을 주었는가?
```

등을 분석하는 데 사용할 수 있다.

모델과 SHAP 방식에 따라 해석 방법은 달라질 수 있다.

---

# 90. Ensemble

여러 모델의 예측을 결합하는 방법을:

```text
Ensemble
```

이라고 한다.

예:

```text
Random Forest

Gradient Boosting

XGBoost
```

자체도 Ensemble 계열 모델이다.

여러 서로 다른 모델을 추가로 결합할 수도 있다.

---

# 91. Stacking

여러 모델의 예측값을 다시 새로운 모델의 입력으로 사용하는 Ensemble 방법 중 하나이다.

개념:

```text
Model A ─┐
Model B ─┼→ 예측값 → Meta Model → Final Prediction
Model C ─┘
```

이다.

---

# 92. OOF

Stacking에서는:

```text
OOF
```

라는 개념이 중요하다.

OOF:

```text
Out Of Fold
```

이다.

각 Fold에서 해당 Sample을 학습에 사용하지 않은 모델의 예측값을 모아 Meta Model 학습 등에 활용할 수 있다.

---

# 93. 머신러닝 실험

머신러닝은 한 번 모델을 실행하고 끝나는 작업이 아니다.

예:

```text
Baseline
   ↓
전처리 변경
   ↓
Feature Engineering
   ↓
Model 변경
   ↓
Hyperparameter Tuning
   ↓
Threshold 변경
   ↓
성능 비교
```

를 반복한다.

---

# 94. 실험 기록

실험할 때는 다음을 기록하는 것이 좋다.

```text
Model

Features

Preprocessing

Hyperparameters

Validation 방식

Metric

Score
```

예:

| Experiment | Model | F1 | ROC-AUC |
|---|---|---:|---:|
| Baseline | Logistic | 0.72 | 0.81 |
| Exp 1 | RandomForest | 0.76 | 0.84 |
| Exp 2 | XGBoost | 0.79 | 0.87 |

---

# 95. 성능이 높다고 무조건 좋은 모델인가?

아니다.

다음도 고려해야 한다.

```text
Data Leakage가 없는가?

Overfitting이 없는가?

Test Data를 반복적으로 사용하지 않았는가?

불균형 데이터에서 Metric이 적절한가?

실제 서비스에서 사용할 수 있는 Feature인가?

해석 가능성이 필요한가?

예측 시간이 적절한가?
```

---

# 96. 의료 AI에서 특히 주의

의료 데이터에서는 높은 Accuracy 하나만으로 실제 임상적 유용성을 판단할 수 없다.

예:

```text
Recall

Specificity

Precision

False Negative

False Positive

데이터 대표성

외부 검증
```

등을 함께 고려해야 할 수 있다.

모델의 예측은 의료진의 진단과 동일한 개념이 아니다.

---

# 97. 데이터 분석과 머신러닝 차이

Data Analysis:

```text
데이터에 어떤 특징이 있는가?

어떤 관계가 있는가?

어떤 차이가 있는가?
```

를 분석한다.

Machine Learning:

```text
이 데이터를 이용해
새로운 데이터의 결과를
예측할 수 있는가?
```

에 초점을 둘 수 있다.

둘은 서로 연결되어 있다.

---

# 98. Deep Learning과 Machine Learning

Deep Learning은 Machine Learning의 한 분야이다.

개념적으로:

```text
Artificial Intelligence
        ↓
Machine Learning
        ↓
Deep Learning
```

으로 생각할 수 있다.

---

# 99. 언제 Deep Learning을 사용할까?

예를 들어:

```text
Image

Text

Audio

Video
```

같은 복잡한 비정형 데이터에서는 Deep Learning이 강력하게 활용된다.

하지만 Tabular Data에서는 전통적인 Machine Learning 모델이 매우 좋은 성능을 보이는 경우도 많다.

문제에 맞는 방법을 선택해야 한다.

---

# 100. 전체 학습 흐름

지금까지 공부한 내용을 연결하면:

```text
Python
   ↓
NumPy / Pandas
   ↓
EDA
   ↓
전처리
   ↓
Feature Engineering
   ↓
X / y
   ↓
Train / Validation / Test
   ↓
Model
   ↓
Training
   ↓
Prediction
   ↓
Evaluation
   ↓
Interpretation
   ↓
Improvement
```

이다.

---

# 101. Scikit-learn 기본 패턴

앞으로 머신러닝 코드를 보면 다음 구조를 먼저 찾는다.

```python
# 1. 모델 생성
model = SomeModel()


# 2. 학습
model.fit(
    X_train,
    y_train
)


# 3. 예측
pred = model.predict(
    X_valid
)


# 4. 평가
score = some_metric(
    y_valid,
    pred
)
```

모델이 달라져도 이 기본 흐름은 반복된다.

---

# 102. 머신러닝 코드를 읽는 순서

긴 코드가 나오면 처음부터 한 줄씩 외우려고 하지 않는다.

먼저 다음을 찾는다.

```text
1. Target은 무엇인가?

2. X는 무엇인가?

3. 데이터를 어떻게 나눴는가?

4. 전처리는 무엇을 했는가?

5. 어떤 Model인가?

6. fit은 어디인가?

7. predict는 어디인가?

8. Metric은 무엇인가?

9. Validation 방식은 무엇인가?

10. 결과는 무엇인가?
```

이 순서로 보면 긴 머신러닝 코드도 구조가 보이기 시작한다.

---

# 103. 핵심 용어 정리

```text
Feature
→ 모델의 입력 변수

Target
→ 모델이 예측해야 하는 값

X
→ Feature 데이터

y
→ Target 데이터

Sample
→ 데이터 한 건

Dataset
→ Sample들의 집합

Model
→ 데이터 패턴을 학습하여 예측하는 시스템

Training
→ 모델 학습

Prediction
→ 학습된 모델로 결과 예측

Supervised Learning
→ 정답을 이용한 지도학습

Unsupervised Learning
→ 정답 없이 패턴을 찾는 비지도학습

Classification
→ Class를 예측하는 분류

Regression
→ 연속적인 숫자를 예측하는 회귀

Generalization
→ 새로운 데이터에서도 잘 예측하는 능력

Overfitting
→ Train Data에 지나치게 맞는 현상

Underfitting
→ 패턴을 충분히 학습하지 못한 현상

Baseline
→ 비교 기준이 되는 기본 모델

Metric
→ 모델 성능 평가 기준

Parameter
→ 모델이 학습하는 값

Hyperparameter
→ 사람이 학습 전에 설정하는 값

Cross Validation
→ 데이터를 여러 방식으로 나누어 평가

Data Leakage
→ 학습 과정에 사용하면 안 되는 정보가 들어가는 문제

Ensemble
→ 여러 모델이나 예측을 결합하는 방법
```

---

# 104. 가장 중요한 흐름

Machine Learning을 한 문장으로 생각하면:

```text
과거 데이터에서 패턴을 학습해서
새로운 데이터의 결과를 예측한다.
```

이다.

코드로 보면:

```text
X_train + y_train
        ↓
     fit()
        ↓
     Model
        ↓
     X_test
        ↓
    predict()
        ↓
      pred
        ↓
     y_test와 비교
        ↓
    Evaluation
```

이다.

---

# 105. 배운 점

Machine Learning은 단순히 모델 코드를 실행하는 것이 아니다.

먼저:

```text
무엇을 예측할 것인가?
```

를 정의해야 한다.

그다음:

```text
Feature
Target
```

을 정하고:

```text
X
y
```

로 나눈다.

그리고:

```text
Train
Validation
Test
```

의 역할을 구분해야 한다.

모델은:

```python
model.fit()
```

으로 학습하고:

```python
model.predict()
```

로 새로운 데이터의 결과를 예측한다.

하지만 Train Data에서 높은 성능이 나온다고 끝나는 것이 아니다.

중요한 것은:

```text
처음 보는 데이터에서도
잘 예측할 수 있는가?
```

이며 이를 일반화라고 한다.

따라서 머신러닝에서는:

```text
Overfitting

Underfitting

Data Leakage

Class Imbalance

Evaluation Metric
```

같은 문제도 함께 고려해야 한다.

전체적으로:

```text
문제 정의
   ↓
EDA
   ↓
전처리
   ↓
Feature / Target
   ↓
데이터 분할
   ↓
Baseline
   ↓
Model Training
   ↓
Prediction
   ↓
Evaluation
   ↓
개선
```

이라는 흐름을 이해하는 것이 가장 중요하다.