# 이상치 처리

## 1. 이상치란?

이상치(Outlier)는 다른 데이터들과 비교했을 때 지나치게 크거나 작은 값을 의미한다.

예:

```text
20
21
22
23
24
200
```

여기서:

```text
200
```

은 다른 값들과 크게 떨어져 있다.

이런 값을 이상치라고 볼 수 있다.

하지만 값이 크거나 작다는 이유만으로 무조건 잘못된 데이터는 아니다.

---

# 2. 이상치가 발생하는 이유

이상치는 여러 이유로 발생할 수 있다.

```text
입력 오류

측정 장비 오류

단위 오류

실제로 매우 특이한 관측값

데이터 변환 오류

특정 집단의 특성
```

예를 들어 나이가:

```text
250세
```

라면 입력 오류일 가능성이 높다.

하지만 어떤 검사 수치가 다른 사람보다 매우 높다고 해서 반드시 오류라고 할 수는 없다.

실제 환자의 상태를 나타내는 중요한 정보일 수도 있다.

---

# 3. 이상치와 오류값은 다르다

다음 두 개념을 구분해야 한다.

```text
오류값
→ 현실적으로 잘못 입력된 값

이상치
→ 다른 데이터와 크게 차이나지만 실제로 존재할 수도 있는 값
```

예:

```text
Age = -20
```

은 일반적인 사람의 나이 데이터라면 오류일 가능성이 매우 높다.

반면:

```text
Age = 100
```

은 드물지만 실제로 존재할 수 있다.

따라서 이상치를 발견했다고 바로 삭제해서는 안 된다.

---

# 4. 기본 통계로 이상치 확인

먼저:

```python
df["Age"].describe()
```

를 확인할 수 있다.

예:

```text
count    1000
mean       42
std        18
min         1
25%        29
50%        40
75%        55
max       150
```

여기서:

```text
max = 150
```

이 눈에 띈다면 실제 데이터인지 확인해볼 수 있다.

---

# 5. 최솟값과 최댓값

간단하게:

```python
print(df["Age"].min())
print(df["Age"].max())
```

로 범위를 확인할 수 있다.

여러 수치형 Column을 확인하려면:

```python
numeric_cols = df.select_dtypes(
    include="number"
).columns

for col in numeric_cols:

    print("=" * 50)
    print(col)

    print("min:", df[col].min())
    print("max:", df[col].max())
```

---

# 6. 정렬해서 직접 확인

큰 값부터 확인:

```python
df["Age"].sort_values(
    ascending=False
).head(10)
```

작은 값부터 확인:

```python
df["Age"].sort_values().head(10)
```

이렇게 하면 극단적인 값을 직접 볼 수 있다.

---

# 7. Boxplot

이상치를 시각적으로 확인할 때 Boxplot을 많이 사용한다.

```python
import matplotlib.pyplot as plt

plt.boxplot(
    df["Age"].dropna()
)

plt.show()
```

Boxplot은 데이터의 분포와 극단적인 값을 확인하는 데 도움이 된다.

---

# 8. Boxplot의 기본 구조

Boxplot에서는 다음과 같은 값이 중요하다.

```text
Q1
→ 1사분위수
→ 25%

Median
→ 중앙값
→ 50%

Q3
→ 3사분위수
→ 75%
```

그리고:

```text
IQR = Q3 - Q1
```

을 계산한다.

---

# 9. 사분위수

예를 들어 데이터가:

```text
10
20
30
40
50
60
70
80
```

처럼 존재한다고 하자.

전체 데이터를 순서대로 나눈 뒤:

```text
Q1
→ 하위 25% 지점

Q2
→ 50% 지점
→ Median

Q3
→ 75% 지점
```

을 이용해 데이터의 분포를 파악할 수 있다.

---

# 10. IQR

IQR은:

```text
Interquartile Range
```

의 약자이다.

계산:

```text
IQR = Q3 - Q1
```

이다.

IQR을 이용하여 이상치 후보 범위를 계산할 수 있다.

---

# 11. IQR 이상치 기준

일반적으로 다음 기준을 많이 사용한다.

```text
Lower Bound
= Q1 - 1.5 × IQR

Upper Bound
= Q3 + 1.5 × IQR
```

이 범위를 벗어난 값을 이상치 후보로 본다.

---

# 12. Python으로 IQR 계산

```python
Q1 = df["Age"].quantile(0.25)

Q3 = df["Age"].quantile(0.75)

IQR = Q3 - Q1

lower = Q1 - 1.5 * IQR
upper = Q3 + 1.5 * IQR
```

확인:

```python
print("Q1:", Q1)
print("Q3:", Q3)
print("IQR:", IQR)
print("Lower:", lower)
print("Upper:", upper)
```

---

# 13. IQR 이상치 찾기

```python
outlier_condition = (
    (df["Age"] < lower)
    |
    (df["Age"] > upper)
)

outliers = df.loc[
    outlier_condition
]

display(outliers)
```

의미:

```text
Lower보다 작거나
Upper보다 크면
→ 이상치 후보
```

이다.

---

# 14. 이상치 개수 확인

```python
print(
    outlier_condition.sum()
)
```

전체 데이터 대비 비율:

```python
outlier_ratio = (
    outlier_condition.mean() * 100
)

print(outlier_ratio)
```

예:

```text
이상치 20개
이상치 비율 2%
```

처럼 확인할 수 있다.

---

# 15. 이상치 삭제

IQR 기준으로 이상치를 제거하려면:

```python
df_removed = df.loc[
    ~outlier_condition
].copy()
```

여기서:

```text
outlier_condition
→ 이상치 = True

~
→ True와 False 반전
```

이므로 정상 범위의 데이터만 선택한다.

---

# 16. 삭제 전후 데이터 수 확인

이상치를 삭제하기 전에 얼마나 많은 데이터가 사라지는지 확인한다.

```python
print(
    "처리 전:",
    df.shape
)

print(
    "처리 후:",
    df_removed.shape
)
```

이상치 삭제로 데이터가 지나치게 많이 줄어든다면 다른 방법을 고려해야 한다.

---

# 17. 중앙값으로 대체

이상치를 삭제하지 않고 중앙값으로 대체할 수도 있다.

```python
median_value = df["Age"].median()

df_replaced = df.copy()

df_replaced.loc[
    outlier_condition,
    "Age"
] = median_value
```

즉:

```text
이상치 발견
    ↓
행을 삭제하지 않음
    ↓
해당 값만 중앙값으로 변경
```

한다.

---

# 18. 왜 중앙값을 사용할까?

이상치가 존재하면 평균 자체가 이상치의 영향을 받을 수 있다.

예:

```text
20
21
22
23
200
```

이 데이터에서는 200 때문에 평균이 크게 올라간다.

중앙값은:

```text
22
```

이므로 극단값의 영향을 상대적으로 덜 받는다.

---

# 19. Clip 방법

값을 삭제하거나 중앙값으로 바꾸는 대신 일정 범위를 넘지 못하도록 제한할 수도 있다.

```python
df_clipped = df.copy()

df_clipped["Age"] = (
    df_clipped["Age"].clip(
        lower=lower,
        upper=upper
    )
)
```

예를 들어 Upper Bound가 80이라면:

```text
95
→ 80

100
→ 80
```

처럼 제한된다.

이런 방식을 Capping 또는 Winsorization과 비슷한 목적으로 활용할 수 있다.

---

# 20. IQR 함수 만들기

여러 Column에서 반복한다면 함수로 만들 수 있다.

```python
def get_iqr_bounds(series):

    Q1 = series.quantile(0.25)
    Q3 = series.quantile(0.75)

    IQR = Q3 - Q1

    lower = Q1 - 1.5 * IQR
    upper = Q3 + 1.5 * IQR

    return lower, upper
```

사용:

```python
lower, upper = get_iqr_bounds(
    df["Age"]
)
```

---

# 21. 여러 Column의 IQR 확인

```python
numeric_cols = [
    "Age",
    "BMI",
    "Glucose"
]

for col in numeric_cols:

    Q1 = df[col].quantile(0.25)
    Q3 = df[col].quantile(0.75)

    IQR = Q3 - Q1

    lower = Q1 - 1.5 * IQR
    upper = Q3 + 1.5 * IQR

    condition = (
        (df[col] < lower)
        |
        (df[col] > upper)
    )

    print(
        col,
        "이상치:",
        condition.sum()
    )
```

각 Column의 이상치 후보 개수를 확인할 수 있다.

---

# 22. Z-score

이상치를 찾는 또 다른 방법으로 Z-score가 있다.

Z-score는 어떤 값이 평균에서 표준편차 기준으로 얼마나 떨어져 있는지를 나타낸다.

공식:

```text
Z = (값 - 평균) / 표준편차
```

---

# 23. Z-score 해석

예:

```text
Z = 0
→ 평균 근처

Z = 1
→ 평균보다 표준편차 1개만큼 높음

Z = -1
→ 평균보다 표준편차 1개만큼 낮음

Z = 3
→ 평균보다 표준편차 3개만큼 높음
```

이다.

---

# 24. SciPy로 Z-score 계산

```python
from scipy.stats import zscore

z_scores = zscore(
    df["Age"],
    nan_policy="omit"
)
```

절댓값으로 확인:

```python
import numpy as np

abs_z = np.abs(z_scores)
```

---

# 25. Z-score 이상치 기준

흔히:

```text
|Z| > 3
```

인 데이터를 이상치 후보로 보는 방법을 사용할 수 있다.

```python
condition = abs_z > 3
```

다만 `3`이라는 값도 모든 데이터에 절대적으로 적용해야 하는 법칙은 아니다.

---

# 26. Z-score 이상치 삭제

예:

```python
from scipy.stats import zscore
import numpy as np

z = np.abs(
    zscore(
        df["Age"],
        nan_policy="omit"
    )
)

df_removed = df.loc[
    z <= 3
].copy()
```

단, 결측값과 Index 정렬 문제 등을 함께 고려해야 한다.

---

# 27. IQR과 Z-score 차이

간단하게 비교하면:

```text
IQR
→ 사분위수 사용
→ 중앙 부분의 범위를 이용
→ 극단값의 영향을 상대적으로 덜 받음

Z-score
→ 평균과 표준편차 사용
→ 평균에서 얼마나 떨어졌는지 확인
→ 분포 형태와 극단값의 영향을 고려해야 함
```

---

# 28. 정규분포와 Z-score

Z-score는 평균과 표준편차를 이용한다.

따라서 데이터가 심하게 비대칭이거나 극단적인 값이 많다면 단순한 Z-score 기준이 적절하지 않을 수 있다.

이런 경우:

```text
Histogram
Boxplot
IQR
도메인 기준
```

등을 함께 확인하는 것이 좋다.

---

# 29. 이상치와 정규성

통계 분석에서는 데이터가 정규분포를 따르는지 확인하기도 한다.

예:

```python
from scipy.stats import shapiro

stat, p = shapiro(
    df["Age"].dropna()
)
```

하지만:

```text
정규분포가 아니다
```

라는 이유만으로 이상치를 삭제하는 것은 아니다.

정규성 검정과 이상치 탐지는 서로 다른 목적을 가진다.

---

# 30. 도메인 기준

통계적 기준보다 실제 변수의 의미가 더 중요할 때도 있다.

예를 들어:

```text
나이 < 0
```

은 통계 계산을 하지 않아도 잘못된 값으로 판단할 수 있다.

반면 의료 검사 수치가 IQR 범위를 벗어났더라도 실제 환자의 비정상 상태를 나타내는 값일 수 있다.

따라서:

```text
통계적 이상치
≠
의학적으로 잘못된 값
```

이다.

---

# 31. 의료 데이터에서 특히 주의

의료 데이터에서는 이상치가 오히려 중요한 환자를 나타낼 수 있다.

예:

```text
매우 높은 혈압

매우 높은 심박수

매우 낮은 산소포화도

매우 높은 통증 점수
```

등은 모델이 찾아야 하는 중요한 패턴일 수도 있다.

따라서 IQR 범위를 벗어났다는 이유만으로 자동 삭제하면 중요한 정보를 잃을 수 있다.

---

# 32. 먼저 오류값인지 확인

이상치를 발견하면 다음 순서로 생각할 수 있다.

```text
1. 현실적으로 가능한 값인가?

2. 단위가 맞는가?

3. 입력 오류인가?

4. 특정 코드를 의미하는가?

5. 실제 극단적인 관측값인가?

6. 전체 데이터에서 몇 개인가?

7. Target과 관련이 있는가?
```

그 후 처리 방법을 결정한다.

---

# 33. 이상치 유지

이상치를 반드시 처리해야 하는 것은 아니다.

예:

```text
실제 존재할 수 있는 값

Target 예측에 중요한 정보

Tree 기반 모델 사용

데이터가 충분하지 않음
```

등의 경우 그대로 유지하고 모델 성능을 비교할 수도 있다.

---

# 34. 모델에 따라 영향이 다르다

일부 모델은 이상치에 더 민감할 수 있다.

예:

```text
선형 모델
거리 기반 모델
평균/분산을 사용하는 전처리
```

에서는 극단적인 값의 영향을 받을 수 있다.

반면:

```text
Decision Tree
Random Forest
Gradient Boosting 계열
```

은 데이터 분할을 기반으로 하기 때문에 선형 모델이나 거리 기반 방법과 이상치의 영향 방식이 다르다.

하지만 Tree 모델이라고 해서 이상치를 무조건 무시해도 된다는 뜻은 아니다.

---

# 35. StandardScaler와 이상치

`StandardScaler`는 평균과 표준편차를 사용한다.

```python
from sklearn.preprocessing import StandardScaler
```

이상치가 매우 크면 평균과 표준편차에도 영향을 줄 수 있다.

따라서 Scaling 전에 데이터 분포와 이상치를 확인하는 것이 중요하다.

---

# 36. RobustScaler

이상치가 많은 데이터에서는 `RobustScaler`를 고려할 수도 있다.

```python
from sklearn.preprocessing import RobustScaler

scaler = RobustScaler()
```

RobustScaler는 중앙값과 IQR을 기반으로 Scaling한다.

따라서 극단값의 영향을 상대적으로 덜 받을 수 있다.

---

# 37. 이상치 처리와 Data Leakage

결측치와 마찬가지로 이상치 기준도 Train 데이터에서 계산하는 것이 중요하다.

잘못된 흐름:

```text
전체 데이터로 Q1/Q3 계산
        ↓
Train / Validation 분리
```

이렇게 하면 Validation 데이터의 분포 정보가 전처리에 들어갈 수 있다.

---

# 38. 더 적절한 흐름

```text
Train / Validation 분리
        ↓
X_train에서 Q1 계산
        ↓
X_train에서 Q3 계산
        ↓
IQR 기준 결정
        ↓
Train에 적용
        ↓
같은 기준을 Validation/Test에 적용
```

즉 Validation의 정보를 이용해 전처리 기준을 만들지 않는다.

---

# 39. 처리 방법을 비교한다

이상치를 발견했다고 하나의 방법만 사용하지 않아도 된다.

예:

```text
실험 A
이상치 유지

실험 B
IQR 이상치 삭제

실험 C
IQR 이상치 중앙값 대체

실험 D
IQR 범위로 Clip

실험 E
Z-score 기준 처리
```

각 방법으로 동일한 검증을 수행한 뒤 성능을 비교할 수 있다.

---

# 40. 왜 삭제와 중앙값 대체를 비교할까?

이상치를 삭제하면:

```text
장점
→ 극단값의 영향 제거

단점
→ 데이터 자체가 줄어듦
```

중앙값 대체는:

```text
장점
→ 행 개수를 유지

단점
→ 실제 값의 정보가 사라질 수 있음
```

따라서 어느 방법이 항상 더 좋다고 할 수 없다.

실제 Validation 결과를 비교해야 한다.

---

# 41. 이상치 처리 전후 통계 비교

```python
print(
    df["Age"].describe()
)

print(
    df_removed["Age"].describe()
)
```

처리 전후:

```text
평균
표준편차
최솟값
최댓값
사분위수
```

가 어떻게 변했는지 확인할 수 있다.

---

# 42. 이상치 처리 전후 시각화

처리 전:

```python
plt.boxplot(
    df["Age"].dropna()
)

plt.show()
```

처리 후:

```python
plt.boxplot(
    df_removed["Age"].dropna()
)

plt.show()
```

두 결과를 비교하면 처리 효과를 시각적으로 확인할 수 있다.

---

# 43. 여러 Column의 이상치를 동시에 삭제할 때 주의

예를 들어:

```text
Age 이상치 삭제
BMI 이상치 삭제
Glucose 이상치 삭제
BloodPressure 이상치 삭제
...
```

를 반복하면 처음에는 적은 이상치처럼 보여도 최종적으로 많은 행이 제거될 수 있다.

따라서:

```python
print(
    "원본:",
    df.shape
)

print(
    "처리 후:",
    df_clean.shape
)
```

를 반드시 확인한다.

---

# 44. 이상치 개수 표 만들기

```python
numeric_cols = df.select_dtypes(
    include="number"
).columns

results = []

for col in numeric_cols:

    series = df[col].dropna()

    Q1 = series.quantile(0.25)
    Q3 = series.quantile(0.75)

    IQR = Q3 - Q1

    lower = Q1 - 1.5 * IQR
    upper = Q3 + 1.5 * IQR

    count = (
        (series < lower)
        |
        (series > upper)
    ).sum()

    ratio = (
        count / len(series) * 100
    )

    results.append({
        "column": col,
        "outlier_count": count,
        "outlier_ratio": ratio
    })

outlier_table = pd.DataFrame(results)

outlier_table = outlier_table.sort_values(
    "outlier_ratio",
    ascending=False
)

display(outlier_table)
```

이렇게 하면 어느 Column에 이상치 후보가 많은지 한 번에 확인할 수 있다.

---

# 45. IQR 함수로 정리

```python
def detect_iqr_outliers(
    df,
    column
):

    Q1 = df[column].quantile(0.25)
    Q3 = df[column].quantile(0.75)

    IQR = Q3 - Q1

    lower = Q1 - 1.5 * IQR
    upper = Q3 + 1.5 * IQR

    condition = (
        (df[column] < lower)
        |
        (df[column] > upper)
    )

    return condition, lower, upper
```

사용:

```python
condition, lower, upper = (
    detect_iqr_outliers(
        df,
        "BMI"
    )
)
```

이후:

```python
df.loc[condition]
```

으로 이상치 후보를 확인할 수 있다.

---

# 46. 이상치 처리 후 바로 모델링하지 않는다

처리 후 다시 확인한다.

```text
데이터 개수

결측치

분포

평균/중앙값

Target 비율

Train/Validation 구조
```

특히 행을 삭제하면 Target의 클래스 비율까지 달라질 수 있다.

---

# 47. Target의 이상치?

회귀 문제에서는 Target 자체의 극단값도 확인할 수 있다.

하지만:

```text
Target이 극단적이다
→ 삭제
```

라고 단순하게 판단하면 안 된다.

모델이 예측해야 할 중요한 사례일 수도 있기 때문이다.

분류 문제에서 Target이:

```text
0
1
```

이라면 이것은 이상치 문제가 아니라 클래스 분포 문제로 보는 것이 적절하다.

---

# 48. 이상치와 클래스 불균형은 다르다

예:

```text
Class 0 = 950명
Class 1 = 50명
```

여기서 Class 1이 적다고 해서 이상치라고 부르지 않는다.

이것은:

```text
Class Imbalance
클래스 불균형
```

문제이다.

이상치는 Feature 값의 극단적인 관측값과 관련된 개념이다.

---

# 49. 실제 프로젝트에서의 접근

실제 프로젝트에서는 다음처럼 실험할 수 있다.

```text
Baseline
→ 이상치 처리 없음

Experiment 1
→ IQR 이상치 삭제

Experiment 2
→ IQR 이상치 중앙값 대체

Experiment 3
→ Z-score 기반 제거
```

그리고 같은 Validation 방식으로:

```text
Accuracy
Precision
Recall
F1
ROC-AUC
```

등을 비교한다.

이렇게 해야:

```text
이상치 처리가 실제 모델에 도움이 되었는가?
```

를 판단할 수 있다.

---

# 50. 처리 결과 기록

예:

```text
BMI

IQR Lower = 14.2
IQR Upper = 39.8

이상치 후보 = 25개
전체 대비 = 2.5%

처리 방법
→ 중앙값 대체

처리 이유
→ 행 삭제로 데이터 손실을 피하고
극단값 영향을 줄이기 위해 실험
```

처럼 기록해두면 발표나 보고서에서도 처리 이유를 설명하기 쉽다.

---

# 51. 이상치 처리 체크리스트

이상치를 발견하면 다음을 확인한다.

```text
[ ] 현실적으로 가능한 값인가?

[ ] 입력 오류인가?

[ ] 단위가 맞는가?

[ ] 결측값을 특수 숫자로 표현한 것은 아닌가?

[ ] 몇 개 존재하는가?

[ ] 전체 데이터의 몇 %인가?

[ ] 특정 그룹에 몰려 있는가?

[ ] Target과 관련된 중요한 값인가?

[ ] 삭제하면 데이터가 얼마나 줄어드는가?

[ ] 대체 방법이 합리적인가?

[ ] Validation 성능이 실제로 좋아지는가?
```

---

# 52. 전체 흐름

```text
데이터 확인
     ↓
이상치 후보 발견
     ↓
Boxplot / 통계 확인
     ↓
IQR / Z-score 계산
     ↓
도메인 기준 확인
     ↓
오류인지 실제 값인지 판단
     ↓
유지 / 삭제 / 대체 / Clip
     ↓
처리 전후 비교
     ↓
모델 Validation
     ↓
최종 처리 방법 선택
```

---

# 53. 배운 점

이상치는 다른 데이터와 비교했을 때 지나치게 크거나 작은 값을 의미한다.

대표적인 탐지 방법으로:

```text
Boxplot
IQR
Z-score
```

등을 사용할 수 있다.

IQR은:

```text
IQR = Q3 - Q1

Lower = Q1 - 1.5 × IQR

Upper = Q3 + 1.5 × IQR
```

로 이상치 후보 범위를 계산한다.

Z-score는:

```text
Z = (값 - 평균) / 표준편차
```

를 이용해 평균에서 얼마나 떨어져 있는지 확인한다.

하지만 통계적으로 이상치라고 해서 무조건 잘못된 데이터는 아니다.

특히 의료 데이터에서는 극단적인 수치가 실제로 중요한 환자의 상태를 나타낼 수도 있다.

따라서:

```text
이상치 발견
→ 바로 삭제
```

가 아니라:

```text
이상치 발견
→ 의미 확인
→ 오류 여부 확인
→ 처리 방법 비교
→ Validation 결과 확인
```

순서로 접근하는 것이 중요하다.

또한 머신러닝에서는 IQR이나 대체 기준을 전체 데이터에서 계산하지 않고 Train 데이터에서 계산한 뒤 같은 기준을 Validation과 Test에 적용하여 Data Leakage를 방지해야 한다.