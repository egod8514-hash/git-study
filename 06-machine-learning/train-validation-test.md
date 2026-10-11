# Train / Validation / Test 데이터 분리

## 1. 왜 데이터를 나누는가?

머신러닝의 목적은 단순히 가지고 있는 데이터를 잘 맞히는 것이 아니다.

중요한 것은:

```text
처음 보는 새로운 데이터에서도
잘 예측하는 모델을 만드는 것
```

이다.

이를 확인하기 위해 데이터를 여러 부분으로 나눈다.

대표적으로:

```text
Train
Validation
Test
```

가 있다.

---

# 2. 가장 기본적인 구조

```text
전체 Dataset
      │
      ├── Train
      │
      ├── Validation
      │
      └── Test
```

각 데이터는 역할이 다르다.

```text
Train
→ 모델 학습

Validation
→ 모델 선택 및 조정

Test
→ 마지막 최종 평가
```

---

# 3. 시험 공부로 비유

쉽게 시험으로 생각해보자.

```text
Train
→ 교과서와 연습문제

Validation
→ 모의고사

Test
→ 실제 시험
```

Train으로 공부하고 Validation으로 현재 실력을 확인한다.

그리고 최종적으로 Test를 이용하여 모델 성능을 평가한다.

---

# 4. Train Data

Train Data는 모델을 실제로 학습시키는 데이터이다.

예:

```python
model.fit(
    X_train,
    y_train
)
```

모델은:

```text
X_train
+
y_train
```

을 이용하여 데이터의 패턴을 학습한다.

---

# 5. Validation Data

Validation Data는 모델을 개발하는 과정에서 성능을 확인하기 위한 데이터이다.

예:

```text
Logistic Regression

Random Forest

XGBoost
```

세 모델 중 어떤 모델이 더 좋은지 비교할 수 있다.

```text
Logistic F1 = 0.72

RandomForest F1 = 0.76

XGBoost F1 = 0.81
```

이런 비교에 Validation Data를 사용할 수 있다.

---

# 6. Validation은 모델 선택에 사용된다

Validation 결과를 보고 우리는 여러 결정을 할 수 있다.

예:

```text
어떤 모델을 사용할까?

max_depth를 얼마로 할까?

learning_rate를 얼마로 할까?

Feature를 추가할까?

Feature를 제거할까?

Threshold를 변경할까?

전처리를 변경할까?
```

즉 Validation은:

```text
모델 개발 과정
```

에 사용된다.

---

# 7. Test Data

Test Data는 최종적으로 선택한 모델이 새로운 데이터에서 얼마나 잘 동작하는지 확인하기 위한 데이터이다.

```text
Train
→ 학습

Validation
→ 모델 선택

Test
→ 최종 평가
```

Test는 가능한 한 마지막까지 건드리지 않는 것이 중요하다.

---

# 8. 왜 Test를 마지막에 사용해야 할까?

예를 들어:

```text
Model A
Test Accuracy = 0.82
```

결과를 보고 모델을 수정한다.

다시:

```text
Model B
Test Accuracy = 0.85
```

다시 수정한다.

```text
Model C
Test Accuracy = 0.88
```

이렇게 반복하면 사실상 Test 결과를 보면서 모델을 선택하고 있다.

그러면 Test Data가:

```text
최종 평가용 데이터
```

가 아니라:

```text
모델 개발용 데이터
```

처럼 사용된 것이다.

---

# 9. Test에 과적합될 수 있다

Test Data 자체를 `fit()`에 직접 넣지 않았더라도 Test Score를 반복적으로 보면서 모델을 수정하면 Test 정보가 의사결정에 간접적으로 사용된다.

즉:

```text
Test 결과 확인
     ↓
모델 변경
     ↓
Test 결과 확인
     ↓
모델 변경
```

을 반복하면 Test에 맞춘 선택을 하게 될 수 있다.

---

# 10. 올바른 기본 흐름

```text
전체 Dataset
      ↓
Train / Validation / Test
      ↓
Train으로 학습
      ↓
Validation으로 비교
      ↓
최종 Model 결정
      ↓
Test로 한 번 최종 평가
```

이 흐름을 기억한다.

---

# 11. X와 y

먼저 Feature와 Target을 나눈다.

예:

```python
X = df.drop(
    columns=["target"]
)

y = df[
    "target"
]
```

구조:

```text
df
├── X
│   └── Feature
│
└── y
    └── Target
```

---

# 12. train_test_split

Scikit-learn에서는 데이터를 쉽게 분리할 수 있도록:

```python
train_test_split()
```

을 제공한다.

Import:

```python
from sklearn.model_selection import (
    train_test_split
)
```

---

# 13. 가장 기본적인 분리

```python
X_train, X_test, y_train, y_test = (
    train_test_split(
        X,
        y,
        test_size=0.2,
        random_state=42
    )
)
```

전체 데이터의:

```text
80%
→ Train

20%
→ Test
```

로 나눈다.

---

# 14. 반환되는 데이터

```python
X_train
X_test
y_train
y_test
```

총 네 개가 반환된다.

의미:

```text
X_train
→ 학습용 Feature

X_test
→ 테스트용 Feature

y_train
→ 학습용 정답

y_test
→ 테스트용 정답
```

이다.

---

# 15. X와 y가 함께 나뉘어야 한다

원래 데이터가:

```text
사람 1 → X 데이터 → y = 0

사람 2 → X 데이터 → y = 1

사람 3 → X 데이터 → y = 0
```

라면 데이터를 분리해도 X와 y의 대응 관계가 유지되어야 한다.

`train_test_split()`이 이를 함께 처리한다.

---

# 16. test_size

```python
test_size=0.2
```

의미:

```text
전체 데이터 중
20%를 Test로 사용
```

한다는 뜻이다.

따라서 나머지 약 80%가 Train이 된다.

---

# 17. test_size 예

1000개의 Sample이 있다고 하자.

```python
test_size=0.2
```

라면 개념적으로:

```text
Train ≈ 800

Test ≈ 200
```

이 된다.

실제 개수는 데이터 크기와 분할 방식에 따라 확인하는 것이 좋다.

---

# 18. 직접 확인

```python
print(
    X_train.shape
)

print(
    X_test.shape
)

print(
    y_train.shape
)

print(
    y_test.shape
)
```

데이터를 분리한 후에는 Shape를 확인하는 습관이 좋다.

---

# 19. random_state

```python
random_state=42
```

는 데이터 분할의 난수 상태를 고정하여 같은 조건에서 실행했을 때 동일한 분할을 재현하기 위해 사용한다.

---

# 20. 왜 Random하게 나누는가?

원본 데이터가 특정 순서로 정렬되어 있을 수 있다.

예:

```text
앞부분
→ 대부분 Class 0

뒷부분
→ 대부분 Class 1
```

단순히 앞 80%, 뒤 20%를 자르면 데이터 분포가 크게 달라질 수 있다.

그래서 일반적으로 데이터를 섞어 분할하는 방식을 사용한다.

---

# 21. random_state가 없으면?

실행할 때마다 분할이 달라질 수 있다.

예:

```text
실행 1
Accuracy = 0.81

실행 2
Accuracy = 0.84

실행 3
Accuracy = 0.79
```

분할 자체가 달라져 결과가 변할 수 있다.

---

# 22. random_state를 고정하면?

예:

```python
random_state=42
```

동일한 데이터와 동일한 조건에서 같은 분할을 재현하기 쉬워진다.

따라서 실험 비교가 편해진다.

---

# 23. 왜 42인가?

42라는 숫자 자체에 머신러닝적인 특별한 성능 의미가 있는 것은 아니다.

다른 정수를 사용해도 된다.

예:

```python
random_state=0
```

```python
random_state=123
```

중요한 것은:

```text
실험 재현을 위해 값을 고정한다.
```

는 점이다.

---

# 24. random_state를 바꿔가며 최고 점수를 찾으면?

주의해야 한다.

예:

```text
random_state=1
→ 0.81

random_state=2
→ 0.84

random_state=3
→ 0.89

3이 제일 높으니까 3 사용
```

처럼 특정 분할에서 높은 점수만 선택하면 모델 자체가 좋아진 것이 아니라 우연히 유리한 데이터 분할을 선택한 것일 수 있다.

이런 문제를 줄이기 위해 Cross Validation을 사용할 수 있다.

---

# 25. shuffle

`train_test_split()`은 기본적으로 데이터를 섞어서 나누는 동작을 사용할 수 있다.

명시적으로:

```python
train_test_split(
    X,
    y,
    test_size=0.2,
    shuffle=True,
    random_state=42
)
```

처럼 볼 수도 있다.

---

# 26. 항상 shuffle해야 할까?

아니다.

특히:

```text
시간 순서가 중요한 데이터
```

에서는 함부로 섞으면 안 될 수 있다.

예:

```text
2023년
2024년
2025년
2026년
```

데이터로 미래를 예측한다면 과거 데이터로 학습하고 미래 데이터로 평가하는 구조가 더 자연스러울 수 있다.

---

# 27. 시계열 데이터

예:

```text
Train
2023 ~ 2025

Test
2026
```

처럼 시간 순서를 유지할 수 있다.

즉:

```text
무조건 Random Split
```

이 정답은 아니다.

데이터 특성을 보고 분할 방법을 선택해야 한다.

---

# 28. stratify

분류 문제에서는 매우 중요한 옵션이다.

예:

```python
train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42,
    stratify=y
)
```

---

# 29. stratify=y 의미

전체 Target 비율이:

```text
0 = 75%
1 = 25%
```

라면 Train과 Test도 가능한 한 비슷한 Class 비율을 유지하도록 분리한다.

```text
Train

0 ≈ 75%
1 ≈ 25%


Test

0 ≈ 75%
1 ≈ 25%
```

---

# 30. stratify가 중요한 이유

불균형 데이터가 있다고 하자.

```text
0 = 950

1 = 50
```

Random하게 나누는 과정에서 특정 Split에 Class 1이 지나치게 적게 들어갈 수 있다.

그러면:

```text
Train과 Test의 Class 분포가 크게 달라짐
```

문제가 생길 수 있다.

---

# 31. 분류에서는 Class 비율 확인

분할 전:

```python
print(
    y.value_counts(
        normalize=True
    )
)
```

분할 후:

```python
print(
    y_train.value_counts(
        normalize=True
    )
)

print(
    y_test.value_counts(
        normalize=True
    )
)
```

비율을 비교한다.

---

# 32. Train/Test 분리 추천 형태

일반적인 분류 문제에서는 다음 형태를 자주 볼 수 있다.

```python
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

하지만 모든 문제에 무조건 적용하는 공식은 아니다.

---

# 33. Train / Validation / Test 세 개로 나누기

`train_test_split()`을 두 번 사용할 수 있다.

먼저:

```text
전체
↓
Train + Temp
```

로 나눈다.

그리고:

```text
Temp
↓
Validation + Test
```

로 다시 나눈다.

---

# 34. 예: 60 / 20 / 20

전체 데이터를:

```text
Train = 60%

Validation = 20%

Test = 20%
```

로 나눠보자.

```python
X_train, X_temp, y_train, y_temp = (
    train_test_split(
        X,
        y,
        test_size=0.4,
        random_state=42,
        stratify=y
    )
)
```

첫 번째 분리:

```text
Train = 60%

Temp = 40%
```

이다.

---

# 35. Temp를 반으로 나누기

```python
X_valid, X_test, y_valid, y_test = (
    train_test_split(
        X_temp,
        y_temp,
        test_size=0.5,
        random_state=42,
        stratify=y_temp
    )
)
```

Temp 40%의 절반씩이므로:

```text
Validation = 20%

Test = 20%
```

가 된다.

최종:

```text
Train = 60%

Validation = 20%

Test = 20%
```

이다.

---

# 36. Shape 확인

```python
print(
    "Train:",
    X_train.shape
)

print(
    "Validation:",
    X_valid.shape
)

print(
    "Test:",
    X_test.shape
)
```

반드시 실제 결과를 확인한다.

---

# 37. 비율 확인

```python
total = len(X)

print(
    len(X_train) / total
)

print(
    len(X_valid) / total
)

print(
    len(X_test) / total
)
```

대략:

```text
0.6
0.2
0.2
```

가 나온다.

---

# 38. 꼭 60/20/20이어야 하나?

아니다.

예:

```text
70 / 15 / 15

80 / 10 / 10

80 / 20
```

등 여러 방식이 가능하다.

데이터 크기와 문제에 따라 결정한다.

---

# 39. 데이터가 적다면?

데이터가 매우 적은데 Validation과 Test를 크게 떼어놓으면 실제 학습에 사용할 데이터가 너무 줄어든다.

이런 경우:

```text
Cross Validation
```

을 적극적으로 고려할 수 있다.

---

# 40. Hold-out

Train/Test 또는 Train/Validation/Test처럼 데이터를 한 번 나누어 평가하는 방법을:

```text
Hold-out
```

방식이라고 부르기도 한다.

장점:

```text
간단함

빠름

이해하기 쉬움
```

단점:

```text
어떻게 나눴는지에 따라
평가 결과가 달라질 수 있음
```

이다.

---

# 41. Cross Validation과 차이

Hold-out:

```text
한 번 분리
```

Cross Validation:

```text
여러 Fold로 나누어
반복적으로 학습/평가
```

한다.

예:

```text
Fold 1
Fold 2
Fold 3
Fold 4
Fold 5
```

---

# 42. Validation 없이 Cross Validation 사용

Tabular Machine Learning에서는 별도의 고정 Validation Set 대신 Train Data 내부에서 Cross Validation을 사용하는 경우도 많다.

예:

```text
전체 Dataset
     ↓
Train / Test
     ↓
Train 내부에서
5-Fold Cross Validation
     ↓
Model 선택
     ↓
마지막 Test 평가
```

---

# 43. 구조

```text
전체 Dataset
      │
      ├── Test
      │
      └── Train
            │
            ├── Fold 1
            ├── Fold 2
            ├── Fold 3
            ├── Fold 4
            └── Fold 5
```

Test는 여전히 마지막까지 분리해 둔다.

---

# 44. Cross Validation이 Test를 대체하는 것은 아니다

Cross Validation을 했다고 해서 반드시 Test가 필요 없다는 뜻은 아니다.

Cross Validation도 모델 개발과 선택에 사용될 수 있기 때문이다.

최종적인 독립 평가가 필요하다면 별도의 Test Set을 유지하는 것이 중요하다.

---

# 45. Kaggle / DACON 같은 대회

머신러닝 대회에서는 구조가 조금 다르게 보일 수 있다.

파일:

```text
train.csv

test.csv

sample_submission.csv
```

이 제공되는 경우가 많다.

---

# 46. 대회의 train.csv

대회 `train.csv`에는 일반적으로:

```text
Feature
+
Target
```

이 포함되어 있다.

예:

```text
age
BMI
glucose
target
```

---

# 47. 대회의 test.csv

대회 `test.csv`에는 일반적으로 Target이 공개되지 않는다.

예:

```text
age
BMI
glucose
```

만 존재한다.

우리가 해야 하는 것은:

```text
test.csv
     ↓
학습된 Model
     ↓
Target Prediction
     ↓
submission
```

이다.

---

# 48. 대회 test.csv와 우리가 말하는 Test Set

여기서 매우 중요하다.

대회에서 제공하는:

```text
test.csv
```

와 머신러닝 개발 과정에서 직접 만드는:

```text
X_test
y_test
```

는 역할이 다를 수 있다.

---

# 49. 대회 train.csv를 다시 나누는 이유

대회 `test.csv`에는 정답이 없으므로 로컬에서 모델 성능을 계산할 수 없다.

그래서 대회의 `train.csv`를 다시:

```text
train.csv
   ↓
Train
Validation
```

또는:

```text
Cross Validation
```

으로 나눈다.

---

# 50. 대회 흐름

```text
대회 train.csv
       ↓
X / y
       ↓
Train / Validation
       ↓
모델 실험
       ↓
최종 모델 선택
       ↓
대회 test.csv 예측
       ↓
submission.csv
       ↓
Leaderboard
```

이다.

---

# 51. sample_submission.csv

`sample_submission.csv`은 제출 형식을 알려주는 예제 파일이다.

예:

```text
ID,target
1,0
2,0
3,0
```

우리는 Target 부분을 모델의 예측값으로 채운다.

---

# 52. Submission

예:

```python
test_pred = model.predict(
    test_X
)

submission[
    "target"
] = test_pred

submission.to_csv(
    "submission.csv",
    index=False
)
```

그리고 생성한 파일을 대회에 제출한다.

---

# 53. Leaderboard Score

대회 서버는 참가자에게 공개되지 않은 실제 정답과 제출한 예측을 비교하여 점수를 계산한다.

즉:

```text
test.csv
→ 우리는 Feature만 봄

실제 Target
→ 대회 운영 측이 보유

우리 Prediction
→ 제출

대회 서버
→ 실제 Target과 비교

Leaderboard Score
```

이다.

---

# 54. Validation 점수와 Leaderboard 점수가 다를 수 있다

예:

```text
Validation Accuracy = 0.98

Leaderboard Accuracy = 0.91
```

처럼 차이가 날 수 있다.

가능한 이유:

```text
Validation Split이 실제 Test 분포를 잘 대표하지 못함

Overfitting

Data Leakage

Train/Test Distribution 차이

Validation Data가 너무 적음

특정 Split에 우연히 잘 맞음
```

등이 있다.

---

# 55. Validation 성능이 높다고 제출 성능도 같은 것은 아니다

Validation:

```text
우리가 가지고 있는 Train Data 내부 평가
```

Leaderboard:

```text
공개되지 않은 실제 Test Target 기준 평가
```

이다.

따라서 두 점수가 정확히 같을 이유는 없다.

---

# 56. Public / Private Leaderboard

일부 대회에서는 Test Data의 일부로:

```text
Public Leaderboard
```

점수를 계산하고, 나머지 데이터로:

```text
Private Leaderboard
```

최종 순위를 계산하기도 한다.

따라서 Public Score만 지나치게 최적화하면 최종 Private Score가 떨어질 수도 있다.

---

# 57. Leaderboard도 계속 보면?

Leaderboard Score를 보고:

```text
제출

수정

제출

수정

제출
```

을 반복하면 사실상 Leaderboard 결과에 맞춰 모델을 조정하게 될 수 있다.

이를:

```text
Leaderboard Overfitting
```

문제로 생각할 수 있다.

---

# 58. 좋은 대회 실험 방식

```text
Local Validation 설계
       ↓
실험
       ↓
Cross Validation 확인
       ↓
좋은 실험만 선택
       ↓
Submission
       ↓
Leaderboard는 추가 확인
```

하는 방식이 더 안정적이다.

---

# 59. 데이터 누수와 분할

Train/Test를 나누기 전에 전체 데이터에서 평균을 계산했다고 하자.

```python
mean_value = X[
    "age"
].mean()
```

그리고 이 값을 Train과 Test 모두에 사용한다면 Test의 정보가 전처리 기준에 들어갈 수 있다.

---

# 60. 잘못된 Scaling 예

```python
scaler = StandardScaler()

X_scaled = scaler.fit_transform(
    X
)

X_train, X_test = train_test_split(
    X_scaled,
    test_size=0.2
)
```

문제:

```text
Scaler가 전체 X를 보고
평균과 표준편차를 계산함

↓
Test 정보도 포함됨
```

이다.

---

# 61. 올바른 Scaling 흐름

먼저 분리한다.

```python
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

그다음:

```python
scaler = StandardScaler()

X_train_scaled = scaler.fit_transform(
    X_train
)

X_test_scaled = scaler.transform(
    X_test
)
```

한다.

---

# 62. 왜 Test에는 fit_transform을 안 쓰는가?

잘못된 예:

```python
X_test_scaled = scaler.fit_transform(
    X_test
)
```

이렇게 하면 Test Data를 이용하여 새로운 평균과 표준편차를 학습한다.

우리가 원하는 것은 Train에서 배운 기준을 Test에도 그대로 적용하는 것이다.

따라서:

```python
X_train
→ fit_transform()

X_test
→ transform()
```

이다.

---

# 63. 결측치 처리도 마찬가지

예를 들어 중앙값으로 결측치를 채운다고 하자.

Train:

```text
Train의 중앙값 계산
        ↓
Train 결측치 처리
```

Test:

```text
Train에서 계산한 중앙값
        ↓
Test 결측치 처리
```

해야 한다.

Test의 중앙값을 따로 학습 기준으로 사용하는 것을 피한다.

---

# 64. Feature Selection도 Leakage 가능

전체 Dataset을 보고 Target과 관계가 높은 Feature를 먼저 선택한 다음 Train/Test를 나누면 Test의 정보가 Feature 선택 과정에 들어갈 수 있다.

즉:

```text
Feature Selection

Scaling

Imputation

Oversampling
```

등도 분할 위치를 주의해야 한다.

---

# 65. SMOTE도 주의

잘못된 흐름:

```text
전체 데이터
   ↓
SMOTE
   ↓
Train/Test Split
```

이렇게 하면 생성된 Sample의 정보가 Train/Test 사이에 부적절하게 섞일 위험이 있다.

더 적절한 기본 흐름:

```text
전체 데이터
   ↓
Train/Test Split
   ↓
Train에만 SMOTE
```

이다.

Cross Validation에서는 각 Training Fold 안에서만 Oversampling하도록 Pipeline을 사용하는 방법을 고려한다.

---

# 66. 같은 환자의 데이터가 여러 개라면?

의료 데이터에서는 한 환자에게 여러 검사나 이미지가 있을 수 있다.

예:

```text
Patient A
├── Image 1
├── Image 2
└── Image 3
```

Random하게 이미지 단위로 나누면:

```text
Train
→ Patient A Image 1

Validation
→ Patient A Image 2
```

처럼 같은 환자의 데이터가 양쪽에 들어갈 수 있다.

---

# 67. Patient Leakage

같은 환자의 매우 유사한 데이터가 Train과 Validation에 동시에 들어가면 Validation 성능이 실제보다 높게 나타날 수 있다.

따라서 데이터 구조에 따라:

```text
Patient 단위 Split
```

이 필요할 수 있다.

---

# 68. Group Split

그룹 단위로 데이터가 묶여 있는 경우:

```text
환자

병원

사용자

회사

장비
```

등을 기준으로 Train과 Validation이 겹치지 않도록 나누는 방법을 고려한다.

Scikit-learn에는 Group 기반 분할 방법들도 존재한다.

---

# 69. 이미지 데이터에서도 중요

예:

```text
원본 이미지 A
```

에서 비슷한 이미지 여러 장을 만든 뒤:

```text
Train
Validation
```

양쪽에 매우 유사한 이미지가 들어가면 평가가 과도하게 좋아질 수 있다.

따라서:

```text
Split 먼저

Augmentation은 Train에 적용
```

하는 흐름이 중요하다.

---

# 70. Augmentation

이미지 Augmentation:

```text
회전

이동

밝기 변화

Crop

Flip
```

등을 Train에 적용하여 데이터 다양성을 늘릴 수 있다.

Validation/Test는 일반적으로 모델의 실제 평가를 위해 학습용 Random Augmentation과 구분한다.

---

# 71. Train과 Validation의 분포

Train과 Validation이 너무 다르면 평가가 불안정해질 수 있다.

하지만 실제 서비스 환경 자체가 Train과 다르다면 그 차이를 숨겨서는 안 된다.

중요한 것은:

```text
실제 사용할 환경을
평가 데이터가 얼마나 잘 대표하는가?
```

이다.

---

# 72. Distribution Shift

Train과 실제 예측 데이터의 분포가 달라지는 현상을 넓게:

```text
Distribution Shift
```

라고 부를 수 있다.

예:

```text
Train
→ 특정 병원 데이터

실제 서비스
→ 여러 병원 데이터
```

라면 성능이 달라질 수 있다.

---

# 73. 모델 평가에서 데이터 분할이 중요한 이유

같은 모델이어도 데이터 분할 방법에 따라 성능이 달라질 수 있다.

즉:

```text
Model 성능
```

만 보는 것이 아니라:

```text
어떤 데이터로
어떻게 평가했는가?
```

도 함께 확인해야 한다.

---

# 74. 비교 실험에서는 Split을 동일하게

Model A와 Model B를 비교한다고 하자.

Model A:

```text
random_state = 42
```

Model B:

```text
random_state = 123
```

로 각각 다른 Validation Set을 사용하면 공정한 비교가 어려워진다.

가능하면 같은 Split 또는 같은 Cross Validation 전략으로 비교한다.

---

# 75. Baseline과 실험 모델

좋은 비교:

```text
동일 Train

동일 Validation

동일 Metric

Baseline
vs
Experiment
```

이다.

그러면:

```text
모델 변경으로 성능이 바뀐 것인지
```

비교하기 쉬워진다.

---

# 76. Reproducibility

같은 실험을 다시 실행했을 때 비슷한 결과를 재현할 수 있는 성질을:

```text
Reproducibility
```

즉:

```text
재현성
```

이라고 한다.

이를 위해:

```text
random_state

데이터 버전

Feature 목록

전처리 방법

모델 설정

Library 버전
```

등을 기록하는 것이 도움이 된다.

---

# 77. Random Seed

NumPy:

```python
import numpy as np

np.random.seed(
    42
)
```

PyTorch:

```python
import torch

torch.manual_seed(
    42
)
```

Scikit-learn 모델에서는:

```python
random_state=42
```

같은 설정을 볼 수 있다.

단, 모든 환경에서 모든 연산이 완전히 동일하게 재현되는 것은 별개의 문제일 수 있다.

---

# 78. 데이터 분할 후 확인해야 할 것

최소한 다음을 확인한다.

```text
Train 크기

Validation 크기

Test 크기

Target 비율

Feature 개수

중복 여부

같은 Group이 겹치는지

시간 순서 문제는 없는지
```

---

# 79. 기본 확인 코드

```python
print(
    "X_train:",
    X_train.shape
)

print(
    "X_valid:",
    X_valid.shape
)

print(
    "X_test:",
    X_test.shape
)


print(
    "\nTrain Target Ratio"
)

print(
    y_train.value_counts(
        normalize=True
    )
)


print(
    "\nValidation Target Ratio"
)

print(
    y_valid.value_counts(
        normalize=True
    )
)


print(
    "\nTest Target Ratio"
)

print(
    y_test.value_counts(
        normalize=True
    )
)
```

---

# 80. Index도 확인

Pandas 데이터를 나눈 후 필요하면:

```python
print(
    X_train.index[:10]
)

print(
    X_valid.index[:10]
)
```

처럼 Index를 확인할 수 있다.

Group이나 중복 문제를 점검할 때 도움이 될 수 있다.

---

# 81. Train과 Validation이 겹치는지 간단 확인

일반적인 Index가 Sample을 고유하게 나타낸다는 전제에서는:

```python
overlap = set(
    X_train.index
) & set(
    X_valid.index
)

print(
    len(overlap)
)
```

결과:

```text
0
```

이면 동일 Index가 겹치지 않는다.

단, 이것만으로 모든 종류의 데이터 누수를 검증할 수 있는 것은 아니다.

---

# 82. ID Column 주의

예:

```text
patient_id

user_id

image_id
```

같은 Column은 데이터 식별에 필요할 수 있다.

하지만 모델 Feature로 사용할지는 별도로 판단해야 한다.

ID 자체가 의미 없는 식별자인데 모델에 넣으면 불필요한 패턴을 학습할 수 있다.

반대로 Split을 그룹 단위로 해야 할 때는 ID가 매우 중요할 수 있다.

---

# 83. Train/Validation/Test 역할 다시 정리

Train:

```text
모델이 공부하는 데이터
```

Validation:

```text
우리가 모델을 선택하는 데이터
```

Test:

```text
선택이 끝난 모델을
마지막으로 평가하는 데이터
```

이다.

---

# 84. 코드로 보면

Train:

```python
model.fit(
    X_train,
    y_train
)
```

Validation:

```python
valid_pred = model.predict(
    X_valid
)
```

```python
valid_score = accuracy_score(
    y_valid,
    valid_pred
)
```

Test:

```python
test_pred = model.predict(
    X_test
)
```

```python
test_score = accuracy_score(
    y_test,
    test_pred
)
```

---

# 85. 모델을 Validation으로 선택

예:

```text
Logistic
Validation F1 = 0.72

RandomForest
Validation F1 = 0.77

XGBoost
Validation F1 = 0.81
```

Validation 결과를 바탕으로 최종 후보를 결정할 수 있다.

그 후 Test에서 최종 성능을 확인한다.

---

# 86. 최종 모델을 다시 학습하는 경우

모델과 Hyperparameter 선택이 끝난 후 목적에 따라 Train과 Validation을 합쳐 최종 모델을 다시 학습하는 경우도 있다.

개념:

```text
Train + Validation
        ↓
Final Training
        ↓
Test 평가
```

하지만 Test는 여전히 학습에 사용하지 않는다.

---

# 87. 대회에서는 최종적으로 전체 train.csv를 사용할 수도 있다

대회에서 Local Validation으로 모델과 설정을 결정한 후:

```text
대회 train.csv 전체
```

를 이용해 최종 모델을 다시 학습하고:

```text
대회 test.csv
```

를 예측할 수 있다.

왜냐하면 대회의 test.csv 정답은 우리가 가지고 있지 않기 때문이다.

---

# 88. Cross Validation Test Prediction

5-Fold 모델을 사용한다면 각 Fold 모델이 대회 test.csv를 예측하고 평균을 낼 수도 있다.

예:

```text
Fold 1 Test Prediction ─┐
Fold 2 Test Prediction ─┤
Fold 3 Test Prediction ─┼→ Average → Submission
Fold 4 Test Prediction ─┤
Fold 5 Test Prediction ─┘
```

이 방식은 대회에서 자주 사용되는 방법 중 하나이다.

---

# 89. OOF와 연결

Cross Validation에서 각 Sample이 Validation Fold에 있을 때의 예측을 모으면:

```text
OOF Prediction
```

을 만들 수 있다.

```text
Fold 1 Validation Prediction
Fold 2 Validation Prediction
Fold 3 Validation Prediction
...
       ↓
전체 Train 크기의 OOF Prediction
```

이것은 나중에 Stacking에서도 중요하게 사용된다.

---

# 90. 자주 하는 실수 1

잘못된 흐름:

```text
Test 성능 확인
↓
모델 변경
↓
Test 성능 확인
↓
Feature 변경
↓
Test 성능 확인
```

Test를 Validation처럼 사용하는 문제이다.

---

# 91. 자주 하는 실수 2

```text
전체 데이터로 Scaler fit
↓
Train/Test Split
```

Data Leakage가 발생할 수 있다.

---

# 92. 자주 하는 실수 3

```text
전체 데이터에 SMOTE
↓
Train/Test Split
```

평가 데이터가 독립적이지 않게 될 위험이 있다.

---

# 93. 자주 하는 실수 4

Class 비율이 심하게 불균형한데:

```python
stratify=None
```

으로 한 번만 Random Split하고 그 결과만 믿는 것이다.

데이터 특성과 Sample 수를 확인해야 한다.

---

# 94. 자주 하는 실수 5

같은 환자의 여러 데이터가 있는데 Sample 단위로 Random Split하는 것이다.

```text
Patient A
→ Train에도 있음

Patient A
→ Validation에도 있음
```

이면 실제보다 좋은 성능이 나올 수 있다.

---

# 95. 자주 하는 실수 6

시계열 데이터를 무작위로 섞는 것이다.

```text
미래 데이터
→ Train

과거 데이터
→ Test
```

가 되어 실제 예측 상황과 맞지 않을 수 있다.

---

# 96. 어떤 Split을 사용할지 결정

먼저 데이터 구조를 확인한다.

일반적인 독립 Sample:

```text
Random Split
+
필요하면 stratify
```

Class 불균형 분류:

```text
Stratified Split
```

환자별 반복 데이터:

```text
Group Split
```

시간 데이터:

```text
Time-based Split
```

데이터가 적음:

```text
Cross Validation 고려
```

---

# 97. 분할도 모델링의 일부다

데이터 분할은 단순히:

```python
train_test_split()
```

한 줄을 실행하는 작업이 아니다.

실제로는:

```text
이 평가 방식이
실제 서비스 상황을 제대로 재현하는가?
```

를 결정하는 과정이다.

---

# 98. 의료 데이터 예

실제 목표가:

```text
새로운 환자의 결과 예측
```

이라면 Validation에도 새로운 환자가 들어가는 구조가 자연스럽다.

같은 환자의 다른 검사 결과를 Train과 Validation에 섞으면:

```text
새로운 환자에 대한 일반화
```

를 제대로 측정하지 못할 수 있다.

---

# 99. 이미지 분류 예

Chest X-ray 데이터가:

```text
5216장
```

있다고 하자.

예를 들어:

```text
Train
80%

Validation
20%
```

로 분리할 수 있다.

하지만 먼저 확인해야 할 것은:

```text
한 환자당 이미지가 하나인가?

한 환자의 여러 이미지가 존재하는가?

중복 이미지가 있는가?

같은 이미지에서 파생된 데이터가 있는가?
```

등이다.

단순히 이미지 수만 보고 Random Split하면 안 될 수 있다.

---

# 100. 데이터 분할과 성능 해석

예:

```text
Validation Accuracy = 98%
```

라고 해도 먼저:

```text
Split 방식은?

중복은?

Patient Leakage는?

Class 비율은?

Validation Sample 수는?

Data Leakage는?
```

를 확인해야 한다.

점수만 보면 안 된다.

---

# 101. 가장 중요한 코드

일반적인 이진 분류라면 우선 다음 구조를 이해한다.

```python
from sklearn.model_selection import (
    train_test_split
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


print(
    X_train.shape,
    X_test.shape
)


print(
    y_train.value_counts(
        normalize=True
    )
)


print(
    y_test.value_counts(
        normalize=True
    )
)
```

---

# 102. 이 코드를 읽는 방법

```python
X = df.drop(
    columns=["target"]
)
```

```text
Target을 제외한 나머지를 Feature로 사용
```

```python
y = df["target"]
```

```text
target을 정답으로 사용
```

```python
test_size=0.2
```

```text
20%를 Test로 사용
```

```python
random_state=42
```

```text
분할을 재현할 수 있도록 난수 상태 고정
```

```python
stratify=y
```

```text
Target Class 비율을 가능한 한 유지
```

이다.

---

# 103. 핵심 용어 정리

```text
Train
→ 모델 학습용 데이터

Validation
→ 모델 선택과 조정용 데이터

Test
→ 최종 평가용 데이터

Hold-out
→ 데이터를 한 번 나누어 평가하는 방식

train_test_split
→ Scikit-learn 데이터 분할 함수

test_size
→ Test로 사용할 데이터 비율 또는 개수

random_state
→ 난수 상태를 고정하여 분할 재현

stratify
→ Class 비율을 유지하도록 분할

Cross Validation
→ 여러 Fold로 반복 평가

Group Split
→ 동일 그룹이 Train/Validation에 겹치지 않도록 분리

Time-based Split
→ 시간 순서를 고려한 분리

Data Leakage
→ 평가에 사용하면 안 되는 정보가 학습 과정에 들어가는 문제

Distribution Shift
→ Train과 실제 예측 데이터의 분포 차이

Leaderboard
→ 대회 제출 예측을 평가한 점수표

Submission
→ 대회 Test Data에 대한 예측 결과 파일

OOF
→ 각 Sample을 학습하지 않은 Fold 모델에서 얻은 예측
```

---

# 104. 전체 흐름

일반 프로젝트:

```text
Dataset
   ↓
X / y
   ↓
Train / Validation / Test
   ↓
Train
→ 모델 학습
   ↓
Validation
→ 모델 선택
   ↓
Test
→ 최종 평가
```

대회:

```text
train.csv
   ↓
Local Train / Validation
   ↓
모델 개발
   ↓
최종 모델
   ↓
test.csv
   ↓
Prediction
   ↓
submission.csv
   ↓
Leaderboard
```

이다.

---

# 105. 가장 중요한 원칙

데이터를 나눌 때 가장 중요한 질문은:

```text
실제 모델이 사용될 상황을
이 평가 방식이 제대로 재현하고 있는가?
```

이다.

따라서:

```text
무조건 8:2

무조건 random_state=42

무조건 stratify

무조건 Random Split
```

처럼 외우면 안 된다.

데이터에 따라:

```text
Random

Stratified

Group

Time-based

Cross Validation
```

중 적절한 방법을 선택한다.

---

# 106. 배운 점

Train / Validation / Test는 서로 역할이 다르다.

```text
Train
→ 학습

Validation
→ 선택

Test
→ 최종 평가
```

이다.

`train_test_split()`에서는:

```python
test_size=0.2
```

로 Test 크기를 정하고:

```python
random_state=42
```

로 동일한 분할을 재현할 수 있으며:

```python
stratify=y
```

를 사용하여 분류 문제에서 Class 비율을 가능한 한 유지할 수 있다.

하지만 데이터 분할은 단순한 코드 문제가 아니다.

환자 데이터라면:

```text
Patient Leakage
```

를 확인해야 하고,

시간 데이터라면:

```text
Time-based Split
```

을 고려해야 하며,

데이터가 적다면:

```text
Cross Validation
```

을 고려할 수 있다.

또한:

```text
Scaling
Imputation
Feature Selection
SMOTE
```

등의 전처리를 전체 데이터에 먼저 학습시키면 Data Leakage가 발생할 수 있다.

따라서 가장 중요한 흐름은:

```text
먼저 올바르게 데이터를 분리하고

Train에서 학습하고

Validation으로 선택하고

Test는 마지막 평가에 사용한다.
```

이다.