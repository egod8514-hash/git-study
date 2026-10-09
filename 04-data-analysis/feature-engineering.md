# Feature Engineering

## 1. Feature Engineering이란?

Feature Engineering은 기존 데이터를 이용하여 머신러닝 모델이 더 유용하게 사용할 수 있는 새로운 Feature를 만드는 과정이다.

예를 들어 원본 데이터에:

```text
Height
Weight
```

가 있다면 두 변수를 이용하여:

```text
BMI
```

라는 새로운 Feature를 만들 수 있다.

즉:

```text
기존 Feature
    ↓
변환 / 조합
    ↓
새로운 Feature
```

를 만드는 과정이다.

---

# 2. Feature란?

머신러닝에서:

```text
X
→ Feature

y
→ Target
```

이라고 한다.

예를 들어 당뇨 예측 데이터가:

```text
Age
BMI
Glucose
BloodPressure
Outcome
```

이라면:

```text
Age
BMI
Glucose
BloodPressure
```

는 Feature이고:

```text
Outcome
```

은 Target이다.

---

# 3. Feature Engineering을 왜 할까?

원본 데이터만으로는 중요한 관계가 명확하게 나타나지 않을 수 있다.

예를 들어:

```text
Weight = 80kg
```

라는 정보만 보면 체중이 높은지 낮은지 정확하게 판단하기 어렵다.

키가:

```text
190cm
```

인 사람과:

```text
150cm
```

인 사람에게 80kg의 의미는 다르다.

따라서:

```text
Height
+
Weight
```

를 조합하여:

```text
BMI
```

를 만들면 체격을 더 의미 있게 표현할 수 있다.

---

# 4. Feature Engineering과 전처리 차이

전처리는 기존 데이터를 모델이 사용할 수 있도록 정리하는 과정이다.

예:

```text
결측치 처리
이상치 처리
자료형 변환
Encoding
Scaling
```

Feature Engineering은 기존 데이터를 이용해 새로운 정보를 만드는 과정이다.

예:

```text
Height + Weight
→ BMI

설립연도 + 현재연도
→ 업력

난자 수 + 배아 수
→ 비율

두 검사값
→ 차이
```

둘은 서로 연결되어 있지만 목적이 다르다.

---

# 5. 가장 간단한 Feature Engineering

두 Column을 더할 수 있다.

```python
df["Total"] = (
    df["A"] + df["B"]
)
```

예:

```text
A = 10
B = 20

Total = 30
```

---

# 6. 차이 Feature

```python
df["Difference"] = (
    df["A"] - df["B"]
)
```

예:

```text
수축기 혈압
-
이완기 혈압
```

처럼 두 값 사이의 차이가 의미를 가질 수도 있다.

단, 새로운 변수를 만들기 전에 해당 차이가 실제로 의미 있는지 생각해야 한다.

---

# 7. 곱 Feature

```python
df["Interaction"] = (
    df["A"] * df["B"]
)
```

두 변수의 상호작용을 표현하는 간단한 방법이다.

예:

```text
A가 높으면서
B도 높은 경우
```

를 하나의 Feature로 표현할 수 있다.

---

# 8. 비율 Feature

비율은 Feature Engineering에서 자주 사용한다.

```python
df["Ratio"] = (
    df["A"] / df["B"]
)
```

예:

```text
생성된 배아 수
/
채취한 난자 수
```

처럼 단순 개수보다 과정의 효율을 표현하는 Feature가 될 수 있다.

---

# 9. 비율 Feature의 의미

다음 두 사람이 있다고 하자.

```text
환자 A

난자 = 10
배아 = 5
```

```text
환자 B

난자 = 4
배아 = 2
```

배아 개수만 보면:

```text
5 vs 2
```

로 차이가 있다.

하지만 비율을 계산하면:

```text
A
5 / 10 = 0.5

B
2 / 4 = 0.5
```

둘 다 50%이다.

따라서 비율은 단순한 개수와 다른 정보를 표현할 수 있다.

---

# 10. 분모가 0일 때 주의

다음 코드는:

```python
df["Ratio"] = (
    df["A"] / df["B"]
)
```

`B = 0`이면 문제가 발생할 수 있다.

예:

```text
A = 10
B = 0
```

이 경우:

```text
inf
```

같은 무한대 값이 발생할 수 있다.

---

# 11. 안전하게 비율 만들기

NumPy를 이용할 수 있다.

```python
import numpy as np

df["Ratio"] = np.where(
    df["B"] != 0,
    df["A"] / df["B"],
    np.nan
)
```

의미:

```text
B가 0이 아니면
→ A / B

B가 0이면
→ NaN
```

이다.

---

# 12. 무한대 확인

```python
np.isinf(
    df["Ratio"]
).sum()
```

무한대 값이 몇 개인지 확인한다.

전체 수치형 데이터:

```python
numeric_df = df.select_dtypes(
    include="number"
)

np.isinf(
    numeric_df
).sum()
```

으로 확인할 수도 있다.

---

# 13. BMI 만들기

대표적인 Feature Engineering 예이다.

BMI 공식:

```text
BMI =
체중(kg) / 신장(m)^2
```

신장이 cm라면 먼저 m로 변환한다.

```python
height_m = (
    df["Height"] / 100
)

df["BMI"] = (
    df["Weight"]
    /
    (height_m ** 2)
)
```

---

# 14. BMI를 만드는 이유

다음 두 Feature:

```text
Height
Weight
```

를 각각 사용하는 것과:

```text
BMI
```

를 추가하는 것은 다르다.

BMI는 키에 비해 체중이 어느 정도인지 표현한다.

즉 단순한 숫자 두 개를 도메인 지식을 이용하여 새로운 의미의 Feature로 만든 것이다.

---

# 15. 날짜에서 새로운 Feature 만들기

날짜 데이터:

```text
설립일
2010-05-01
```

이 있다면:

```python
df["설립일"] = pd.to_datetime(
    df["설립일"],
    errors="coerce"
)
```

연도:

```python
df["설립연도"] = (
    df["설립일"].dt.year
)
```

월:

```python
df["설립월"] = (
    df["설립일"].dt.month
)
```

등을 만들 수 있다.

---

# 16. 업력 만들기

예를 들어 평가가 이루어진 연도와 설립연도가 있다면:

```python
df["업력"] = (
    df["평가연도"]
    -
    df["설립연도"]
)
```

처럼 만들 수 있다.

단순한 설립연도보다:

```text
평가 시점에 회사가 몇 년 되었는가?
```

를 직접 표현한다.

---

# 17. 날짜 차이 Feature

두 날짜가 있다면 경과 기간을 만들 수 있다.

```python
df["start_date"] = pd.to_datetime(
    df["start_date"]
)

df["end_date"] = pd.to_datetime(
    df["end_date"]
)
```

차이:

```python
df["days"] = (
    df["end_date"]
    -
    df["start_date"]
).dt.days
```

결과:

```text
두 사건 사이에 며칠이 지났는가?
```

라는 새로운 Feature가 된다.

---

# 18. 의료 데이터의 경과기간

의료 데이터에서는:

```text
증상 발생일
내원일
수술일
퇴원일
```

등 날짜 정보가 존재할 수 있다.

이들 사이의 차이를 이용하여:

```text
증상 발생 후 내원까지 기간

입원기간

수술 후 퇴원까지 기간
```

등을 만들 수 있다.

단, 예측 시점 이후에 발생한 날짜를 사용하면 Data Leakage가 발생할 수 있으므로 주의해야 한다.

---

# 19. 합계 Feature

여러 변수의 합이 의미가 있을 수도 있다.

```python
df["TotalScore"] = (
    df["Score_A"]
    +
    df["Score_B"]
    +
    df["Score_C"]
)
```

하지만 단순히 더할 수 있다고 해서 무조건 합계를 만들지는 않는다.

각 값의 의미와 단위가 적절한지 확인해야 한다.

---

# 20. 평균 Feature

여러 측정값의 평균:

```python
df["MeanScore"] = df[
    [
        "Score_A",
        "Score_B",
        "Score_C"
    ]
].mean(
    axis=1
)
```

여기서:

```text
axis=1
```

은 각 행을 기준으로 계산한다는 의미이다.

---

# 21. 반복 측정 데이터

예를 들어 한 사람에게:

```text
검사1
검사2
검사3
검사4
```

처럼 반복 측정값이 있다면:

```text
평균

최댓값

최솟값

표준편차

처음과 마지막 차이
```

등을 Feature로 만들 수 있다.

---

# 22. 행별 평균

```python
cols = [
    "Test1",
    "Test2",
    "Test3",
    "Test4"
]

df["Test_mean"] = (
    df[cols].mean(
        axis=1
    )
)
```

---

# 23. 행별 최댓값

```python
df["Test_max"] = (
    df[cols].max(
        axis=1
    )
)
```

---

# 24. 행별 최솟값

```python
df["Test_min"] = (
    df[cols].min(
        axis=1
    )
)
```

---

# 25. 행별 표준편차

```python
df["Test_std"] = (
    df[cols].std(
        axis=1
    )
)
```

이 값은 반복 측정 결과가 얼마나 변동하는지를 나타낼 수 있다.

---

# 26. 처음과 마지막 차이

```python
df["Test_change"] = (
    df["Test4"]
    -
    df["Test1"]
)
```

단순 평균뿐 아니라 시간에 따른 변화량을 Feature로 사용할 수 있다.

---

# 27. 조건 기반 Feature

특정 조건을 만족하는지 새로운 Feature로 만들 수 있다.

예:

```python
df["High_BMI"] = (
    df["BMI"] >= 25
).astype(int)
```

결과:

```text
BMI < 25
→ 0

BMI >= 25
→ 1
```

이다.

---

# 28. Boolean을 숫자로 변환

```python
df["BMI"] >= 25
```

결과는:

```text
True
False
```

이다.

여기에:

```python
.astype(int)
```

를 사용하면:

```text
True  → 1
False → 0
```

으로 변환된다.

---

# 29. 결측 여부 Feature

결측값 자체가 정보를 가질 수도 있다.

```python
df["BMI_missing"] = (
    df["BMI"].isnull()
).astype(int)
```

결과:

```text
BMI 존재
→ 0

BMI 결측
→ 1
```

이다.

---

# 30. 왜 결측 여부를 Feature로 만들까?

특정 검사가 일부 환자에게만 시행되었다면:

```text
검사를 받았는가?
```

자체가 환자의 상태와 관련된 정보를 가질 수 있다.

따라서:

```text
검사값 결측
```

을 단순히 채우는 것보다:

```text
원래 결측이었는지
```

를 별도로 저장할 수도 있다.

---

# 31. 범주 결합

두 범주형 변수를 하나의 Feature로 결합할 수도 있다.

```python
df["Gender_Job"] = (
    df["Gender"].astype(str)
    +
    "_"
    +
    df["Job"].astype(str)
)
```

예:

```text
Male_Office
Female_Office
Male_Student
```

처럼 두 변수의 조합을 표현한다.

---

# 32. Interaction Feature

두 Feature의 상호작용을 직접 만들 수도 있다.

```python
df["Age_BMI"] = (
    df["Age"]
    *
    df["BMI"]
)
```

의미:

```text
Age의 영향이 BMI에 따라 달라질 가능성
```

을 모델이 쉽게 표현하도록 도울 수 있다.

---

# 33. 모든 Feature를 무작정 곱하면 안 된다

예를 들어 Feature가 100개인데 모든 조합을 만들면 Feature 수가 급격하게 증가한다.

이로 인해:

```text
Noise 증가

Overfitting

학습시간 증가

해석 어려움
```

등의 문제가 생길 수 있다.

따라서 Feature Engineering에는 가설이 필요하다.

---

# 34. 좋은 Feature Engineering의 질문

새로운 Feature를 만들기 전에:

```text
왜 이 두 변수를 합치는가?

이 비율은 어떤 의미인가?

도메인적으로 설명할 수 있는가?

원래 Feature와 다른 정보를 제공하는가?

실제 예측 시점에 계산 가능한가?
```

를 생각한다.

---

# 35. 도메인 지식

Feature Engineering에서는 도메인 지식이 매우 중요하다.

도메인 지식은:

```text
해당 분야에 대한 전문적인 이해
```

를 의미한다.

예:

```text
의료
금융
제조
스포츠
```

분야마다 의미 있는 변수 조합이 다르다.

---

# 36. 의료 데이터 Feature Engineering

예를 들어:

```text
Height
Weight
```

가 있다면:

```text
BMI
```

를 만들 수 있다.

또:

```text
수축기 혈압
이완기 혈압
```

에서 두 값의 차이를 이용한 Feature를 생각해볼 수도 있다.

하지만 의료적 의미가 있는지는 도메인 지식과 데이터 정의를 확인해야 한다.

---

# 37. Feature Engineering과 Data Leakage

새로운 Feature를 만들 때 가장 조심해야 할 문제 중 하나이다.

예를 들어:

```text
수술 실패 여부
```

를 예측하는데:

```text
수술 실패 후 재수술 여부
```

를 Feature로 만들면 실제 예측 시점 이후의 정보를 사용하게 된다.

이것은 Data Leakage이다.

---

# 38. Feature 생성 전 질문

반드시:

```text
이 Feature를 실제 예측하는 순간에
계산할 수 있는가?
```

를 생각한다.

답이:

```text
아니다.
```

라면 모델 Feature로 사용하는 것이 적절하지 않을 수 있다.

---

# 39. Target을 이용한 Feature 생성 주의

다음처럼 Target 정보를 Feature에 직접 포함하면 안 된다.

```python
df["new_feature"] = (
    df["Age"]
    *
    df["Outcome"]
)
```

`Outcome`이 예측해야 하는 정답이라면 모델이 정답 정보를 보게 된다.

심각한 Data Leakage가 발생한다.

---

# 40. 전체 데이터 통계를 사용한 Feature

예를 들어:

```python
df["Age_diff"] = (
    df["Age"]
    -
    df["Age"].mean()
)
```

처럼 전체 데이터 평균을 사용한다면 Validation/Test 정보가 포함되는 상황을 주의해야 한다.

머신러닝 검증에서는 이런 통계 기반 변환도 Train에서 기준을 학습하고 Validation/Test에 적용하는 원칙을 생각해야 한다.

---

# 41. 로그 변환

한쪽으로 크게 치우친 수치 데이터는 로그 변환을 고려할 수 있다.

```python
df["Amount_log"] = (
    np.log1p(
        df["Amount"]
    )
)
```

`log1p(x)`는:

```text
log(1 + x)
```

를 계산한다.

---

# 42. 왜 log1p를 사용할까?

일반적인 로그에서:

```text
log(0)
```

은 문제가 된다.

`log1p()`는 1을 더한 후 로그를 계산하기 때문에 0을 포함하는 데이터에서 편리할 수 있다.

단, 음수 값의 범위 등에 대해서는 별도의 확인이 필요하다.

---

# 43. 로그 변환의 목적

예:

```text
10
20
30
100
10000
```

처럼 오른쪽으로 크게 치우친 데이터가 있을 때 로그 변환을 하면 큰 값의 간격을 줄일 수 있다.

하지만 모든 변수에 로그 변환을 적용하는 것은 아니다.

---

# 44. 구간화 Binning

연속형 변수를 구간으로 나눌 수도 있다.

예:

```python
df["Age_group"] = pd.cut(
    df["Age"],
    bins=[
        0,
        20,
        40,
        60,
        100
    ],
    labels=[
        "0-19",
        "20-39",
        "40-59",
        "60+"
    ]
)
```

---

# 45. Binning의 장단점

장점:

```text
해석하기 쉬움

비선형적인 패턴 표현 가능
```

단점:

```text
원래 수치의 세밀한 정보 손실

구간 기준에 따라 결과가 달라짐
```

따라서 원본 Age를 삭제하지 않고 Age와 Age_group을 함께 비교해볼 수도 있다.

---

# 46. Polynomial Feature

수치 변수의 제곱이나 상호작용을 자동으로 만들 수도 있다.

```python
from sklearn.preprocessing import (
    PolynomialFeatures
)

poly = PolynomialFeatures(
    degree=2,
    include_bias=False
)

X_poly = poly.fit_transform(
    X
)
```

예를 들어:

```text
A
B
```

가 있다면:

```text
A
B
A²
A×B
B²
```

같은 Feature가 만들어질 수 있다.

---

# 47. Polynomial Feature 주의

Feature 수가 많으면 생성되는 Feature가 급격하게 증가한다.

따라서:

```text
Overfitting

계산량 증가

해석 어려움
```

등의 문제가 발생할 수 있다.

필요한 경우에 제한적으로 사용한다.

---

# 48. Feature Engineering 전 Baseline

새로운 Feature를 만들기 전에 먼저 기본 모델을 만든다.

예:

```text
Baseline

원본 Feature만 사용
ROC-AUC = 0.75
```

그다음 새로운 Feature를 추가한다.

```text
Experiment

원본 Feature
+
BMI
+
Ratio

ROC-AUC = 0.78
```

이렇게 해야 Feature Engineering이 실제로 도움이 되었는지 알 수 있다.

---

# 49. 한 번에 너무 많이 바꾸지 않는다

다음처럼:

```text
Feature 20개 추가
모델 변경
결측치 처리 변경
하이퍼파라미터 변경
```

을 동시에 하면 성능이 좋아졌을 때 무엇 때문인지 알기 어렵다.

가능하면 실험을 분리한다.

---

# 50. 실험 예

```text
Baseline
→ 원본 Feature

Experiment 1
→ + BMI

Experiment 2
→ + BMI + Age_BMI

Experiment 3
→ + BMI + Ratio

Experiment 4
→ + BMI + Ratio + Missing Flag
```

각 실험 결과를 비교한다.

---

# 51. 실험 결과 기록

예:

| Experiment | Feature | ROC-AUC |
|---|---|---:|
| Baseline | Original | 0.750 |
| Exp 1 | + BMI | 0.758 |
| Exp 2 | + Age × BMI | 0.756 |
| Exp 3 | + Ratio | 0.770 |

이렇게 기록하면 어떤 Feature가 실제로 도움이 되었는지 확인하기 쉽다.

---

# 52. 같은 Validation 조건 사용

Feature Engineering 실험을 비교할 때는 Validation 조건을 같게 유지하는 것이 중요하다.

예:

```python
random_state=42
```

그리고 같은 Train/Validation Split을 사용한다.

그렇지 않으면:

```text
Feature 때문에 성능이 변한 것인지

데이터 분할이 달라져서 변한 것인지
```

구분하기 어렵다.

---

# 53. Cross Validation으로 비교

한 번의 Train/Validation Split 결과가 우연일 수도 있다.

따라서 Cross Validation을 사용할 수 있다.

```python
from sklearn.model_selection import (
    cross_val_score
)

scores = cross_val_score(
    model,
    X,
    y,
    cv=5,
    scoring="roc_auc"
)

print(scores)

print(
    scores.mean()
)
```

---

# 54. Feature 추가 전후 비교

```text
Original Features

Fold 1 = 0.74
Fold 2 = 0.76
Fold 3 = 0.75
Fold 4 = 0.73
Fold 5 = 0.77

Mean = 0.75
```

새 Feature:

```text
+ BMI

Fold 1 = 0.76
Fold 2 = 0.78
Fold 3 = 0.77
Fold 4 = 0.75
Fold 5 = 0.79

Mean = 0.77
```

이런 식으로 비교하면 Feature가 여러 Fold에서 안정적으로 도움이 되는지도 볼 수 있다.

---

# 55. Train 성능만 좋아지는 경우

예:

```text
Baseline

Train = 0.85
Validation = 0.80
```

Feature를 많이 추가한 후:

```text
Train = 0.99
Validation = 0.76
```

이라면 좋은 개선이라고 보기 어렵다.

모델이 Train 데이터에 과하게 맞춰진:

```text
Overfitting
```

가능성을 생각해야 한다.

---

# 56. Feature Importance

모델 학습 후 Feature Importance를 확인할 수도 있다.

예:

```python
model.feature_importances_
```

새로 만든 Feature가 모델에서 어느 정도 사용되는지 확인하는 데 도움이 된다.

하지만 Feature Importance가 높다는 이유만으로 그 Feature가 반드시 좋은 Feature라고 단정하지 않는다.

---

# 57. SHAP

SHAP을 사용하면 Feature가 모델 예측에 어떤 방향으로 영향을 주었는지 더 자세하게 분석할 수 있다.

예:

```text
BMI가 높을수록
예측 확률이 증가하는가?

Age가 특정 범위에서
어떤 방향으로 작용하는가?

새로 만든 Ratio Feature가
실제로 예측에 영향을 주는가?
```

등을 확인할 수 있다.

SHAP은 머신러닝 모델 해석 단원에서 자세히 다룬다.

---

# 58. Feature Importance와 상관관계 차이

상관관계:

```text
두 변수 사이의 관계를 분석
```

Feature Importance:

```text
학습된 모델이 해당 Feature를
얼마나 활용했는지 확인
```

SHAP:

```text
Feature가 모델 예측값에
어떤 방향과 크기로 기여했는지 분석
```

서로 같은 개념이 아니다.

---

# 59. Feature를 삭제하는 것도 Feature Engineering

Feature Engineering은 새로운 Feature를 추가하는 것만 의미하지 않는다.

불필요한 Feature를 제거하는 것도 중요하다.

예:

```text
단순 ID

거의 모든 값이 같은 Column

Target Leakage Column

중복 정보

의미 없는 Column
```

등을 제거할 수 있다.

---

# 60. 거의 값이 변하지 않는 Feature

확인:

```python
df.nunique().sort_values()
```

예:

```text
Column_A    1
```

이라면 모든 데이터가 같은 값을 가지고 있을 수 있다.

이런 Feature는 데이터를 구분하는 정보가 거의 없을 수 있다.

---

# 61. 중복 Feature

예:

```text
Height_cm

Height_m
```

처럼 사실상 같은 정보를 다른 단위로 표현한 Feature가 동시에 존재할 수 있다.

이런 경우 모델 종류와 목적에 따라 중복 정보를 어떻게 처리할지 검토한다.

---

# 62. 통계적으로 유의하지 않으면 삭제?

아니다.

예를 들어 어떤 Feature의 통계검정 결과:

```text
p = 0.20
```

이라고 해서 바로 삭제하면 안 된다.

머신러닝 모델에서는 다른 Feature와의 상호작용을 통해 예측에 도움이 될 수도 있다.

따라서:

```text
통계 분석
+
Cross Validation
+
Feature Importance
+
SHAP
+
도메인 지식
```

등을 함께 고려한다.

---

# 63. Tree 모델과 Feature Engineering

다음과 같은 Tree 기반 모델은:

```text
Random Forest
XGBoost
LightGBM
CatBoost
```

비선형 관계와 Feature 상호작용을 어느 정도 직접 학습할 수 있다.

따라서 모든 곱셈 Feature를 직접 만들어야 하는 것은 아니다.

하지만 도메인적으로 의미 있는:

```text
BMI
비율
경과기간
효율
```

같은 Feature는 여전히 도움이 될 수 있다.

---

# 64. 선형 모델과 Feature Engineering

Logistic Regression 같은 선형 모델은 기본적으로 Feature와 결과 사이의 비교적 단순한 관계를 학습한다.

따라서:

```text
A × B

A²

비율
```

등을 직접 만들어주면 원본 Feature만 사용했을 때 표현하기 어려운 관계를 모델이 학습하는 데 도움이 될 수 있다.

---

# 65. Feature Engineering 실전 흐름

```text
데이터 이해
     ↓
Baseline 모델 생성
     ↓
도메인 지식 확인
     ↓
Feature 아이디어 생성
     ↓
새로운 Feature 생성
     ↓
결측 / inf 확인
     ↓
Train / Validation
     ↓
동일 조건에서 모델 학습
     ↓
성능 비교
     ↓
Feature Importance / SHAP 확인
     ↓
유지 또는 제거 결정
```

---

# 66. 간단한 실전 예제

원본:

```python
features = [
    "Age",
    "Height",
    "Weight"
]
```

BMI 생성:

```python
df["BMI"] = (
    df["Weight"]
    /
    (
        (df["Height"] / 100) ** 2
    )
)
```

상호작용:

```python
df["Age_BMI"] = (
    df["Age"]
    *
    df["BMI"]
)
```

결측 여부:

```python
df["BMI_missing"] = (
    df["BMI"].isnull()
).astype(int)
```

이후:

```python
features = [
    "Age",
    "Height",
    "Weight",
    "BMI",
    "Age_BMI",
    "BMI_missing"
]
```

로 모델을 다시 학습하여 Baseline과 비교할 수 있다.

---

# 67. Feature 생성 후 반드시 확인

```python
df[
    [
        "Age",
        "Height",
        "Weight",
        "BMI",
        "Age_BMI"
    ]
].head()
```

그리고:

```python
df[
    [
        "BMI",
        "Age_BMI"
    ]
].describe()
```

결측:

```python
df[
    [
        "BMI",
        "Age_BMI"
    ]
].isnull().sum()
```

무한대:

```python
np.isinf(
    df[
        [
            "BMI",
            "Age_BMI"
        ]
    ]
).sum()
```

을 확인한다.

---

# 68. Feature Engineering 체크리스트

새로운 Feature를 만들 때:

```text
[ ] 이 Feature는 어떤 의미인가?

[ ] 왜 만들었는가?

[ ] 실제 예측 시점에 계산 가능한가?

[ ] Target 정보를 사용하지 않았는가?

[ ] 분모가 0이 될 가능성이 있는가?

[ ] NaN이 새롭게 생겼는가?

[ ] inf가 발생했는가?

[ ] 기존 Feature와 지나치게 중복되는가?

[ ] Train 성능만 좋아진 것은 아닌가?

[ ] Validation 성능도 좋아졌는가?

[ ] 여러 Fold에서도 개선되는가?
```

를 확인한다.

---

# 69. 좋은 Feature Engineering

좋은 Feature Engineering은:

```text
Feature를 많이 만드는 것
```

이 아니다.

좋은 Feature는:

```text
의미를 설명할 수 있고

실제 예측 시점에 사용할 수 있으며

데이터 누수가 없고

모델의 일반화 성능 향상에 도움이 되는 Feature
```

이다.

---

# 70. 프로젝트에서 Feature Engineering을 설명하는 방법

발표에서는:

```text
BMI를 추가했습니다.
```

로 끝내기보다:

```text
신장과 체중을 개별 변수로 사용하는 것보다
체격을 상대적으로 표현하기 위해
BMI를 파생변수로 생성했습니다.
```

처럼:

```text
무엇을 만들었는가?
+
왜 만들었는가?
```

를 함께 설명하는 것이 좋다.

---

# 71. 모델 성능이 안 좋아졌다면?

새로운 Feature를 만들었다고 반드시 성능이 좋아지는 것은 아니다.

예:

```text
Baseline ROC-AUC
0.78

Feature 추가 후
0.76
```

이라면 해당 Feature가 Noise를 추가했을 가능성이 있다.

이 경우 제거하거나 다른 방식으로 수정할 수 있다.

실패한 Feature Engineering도 중요한 실험 결과이다.

---

# 72. 실험 기록의 중요성

예:

```text
Experiment 1
Feature: BMI
Reason: 신장 대비 체중 표현
CV ROC-AUC: +0.012
Result: 유지

Experiment 2
Feature: Age × BMI
Reason: 상호작용 확인
CV ROC-AUC: -0.004
Result: 제거
```

처럼 기록하면 나중에 어떤 실험을 했는지 쉽게 확인할 수 있다.

---

# 73. Feature Engineering 전체 정리

대표적인 Feature Engineering 방법:

```text
합계
차이
곱
비율
평균
최댓값
최솟값
표준편차
변화량
날짜 차이
업력
BMI
조건 Feature
결측 여부
범주 조합
로그 변환
Binning
Polynomial Feature
```

등이 있다.

하지만 중요한 것은 방법의 개수가 아니다.

```text
왜 이 Feature가 필요한가?
```

를 설명할 수 있어야 한다.

---

# 74. 배운 점

Feature Engineering은 기존 데이터를 조합하거나 변환하여 모델이 더 유용하게 사용할 수 있는 새로운 정보를 만드는 과정이다.

예:

```text
Height + Weight
→ BMI

A + B
→ 합계

A - B
→ 차이

A / B
→ 비율

A × B
→ 상호작용

날짜 - 날짜
→ 경과기간

여러 반복 측정값
→ 평균 / 표준편차 / 변화량
```

등을 만들 수 있다.

하지만 Feature를 많이 만든다고 모델이 좋아지는 것은 아니다.

Feature Engineering에서는:

```text
도메인 지식
Data Leakage
결측치
무한대
중복 정보
Overfitting
```

을 함께 확인해야 한다.

가장 중요한 것은 새로운 Feature를 만든 뒤:

```text
Baseline
vs
Feature Engineering
```

을 동일한 Validation 조건에서 비교하는 것이다.

결국 Feature Engineering의 핵심은:

```text
아이디어
   ↓
Feature 생성
   ↓
검증
   ↓
성능 비교
   ↓
해석
   ↓
유지 / 제거
```

이다.

좋은 Feature는 복잡한 Feature가 아니라:

```text
의미를 설명할 수 있고
실제 예측 시점에 사용할 수 있고
Validation에서 실제 도움이 되는 Feature
```

이다.