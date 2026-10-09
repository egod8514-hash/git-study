# 기초 통계 분석

## 1. 통계 분석은 왜 할까?

데이터를 분석하다 보면 다음과 같은 질문이 생긴다.

```text
남성과 여성의 평균 BMI가 다른가?

임신 성공 그룹과 실패 그룹의 나이가 다른가?

통증기간과 통증정도는 관계가 있는가?

투여경로와 금기등급은 관계가 있는가?
```

그래프만으로는:

```text
차이가 있어 보인다.
```

정도는 확인할 수 있다.

하지만 통계 분석을 사용하면 관찰된 차이나 관계가 우연에 의한 것인지 평가할 수 있다.

---

# 2. 기술통계와 추론통계

통계는 크게 두 가지 관점으로 생각할 수 있다.

```text
기술통계
Descriptive Statistics

추론통계
Inferential Statistics
```

기술통계는 현재 데이터를 요약한다.

예:

```text
평균
중앙값
표준편차
최솟값
최댓값
사분위수
```

추론통계는 표본을 이용하여 그룹 간 차이나 변수 간 관계 등을 검정한다.

예:

```text
t-test
Mann-Whitney U
Chi-square
Correlation test
```

---

# 3. 평균

평균(Mean)은 모든 값을 더한 뒤 데이터 개수로 나눈 값이다.

예:

```text
10
20
30
```

평균:

```text
(10 + 20 + 30) / 3
= 20
```

Pandas:

```python
df["Age"].mean()
```

---

# 4. 중앙값

중앙값(Median)은 데이터를 크기순으로 정렬했을 때 가운데 위치한 값이다.

예:

```text
10
20
30
```

중앙값:

```text
20
```

Pandas:

```python
df["Age"].median()
```

---

# 5. 평균과 중앙값 차이

다음 데이터가 있다고 하자.

```text
10
20
30
40
500
```

500이라는 극단값 때문에 평균은 크게 증가한다.

하지만 중앙값은:

```text
30
```

이다.

따라서 이상치가 많거나 한쪽으로 치우친 데이터에서는 중앙값이 데이터의 중심을 설명하는 데 더 적절할 수 있다.

---

# 6. 분산과 표준편차

평균이 같더라도 데이터가 퍼져 있는 정도는 다를 수 있다.

예:

```text
A
49 50 51

B
10 50 90
```

둘 다 평균은 50이지만 B가 훨씬 넓게 퍼져 있다.

이를 나타내는 대표적인 값이:

```text
Variance
분산

Standard Deviation
표준편차
```

이다.

Pandas:

```python
df["Age"].var()
df["Age"].std()
```

---

# 7. 표준편차 해석

표준편차가 작으면:

```text
값들이 평균 주변에 비교적 모여 있음
```

표준편차가 크면:

```text
값들이 평균에서 비교적 넓게 퍼져 있음
```

으로 이해할 수 있다.

---

# 8. 사분위수

데이터를 순서대로 정렬하여 네 부분으로 나눈다.

```text
Q1
→ 25%

Q2
→ 50%
→ Median

Q3
→ 75%
```

Pandas:

```python
df["Age"].quantile(0.25)
df["Age"].quantile(0.50)
df["Age"].quantile(0.75)
```

IQR:

```text
IQR = Q3 - Q1
```

이다.

---

# 9. describe()

여러 기술통계를 한 번에 확인할 수 있다.

```python
df["Age"].describe()
```

결과에는 보통:

```text
count
mean
std
min
25%
50%
75%
max
```

가 포함된다.

---

# 10. 통계적 가설

통계 검정을 할 때 일반적으로 두 가지 가설을 설정한다.

```text
귀무가설
H0

대립가설
H1
```

---

# 11. 귀무가설

귀무가설(Null Hypothesis)은 보통:

```text
차이가 없다.

관계가 없다.
```

라는 가설이다.

예:

```text
H0:
남성과 여성의 BMI 분포에는 차이가 없다.
```

---

# 12. 대립가설

대립가설(Alternative Hypothesis)은:

```text
차이가 있다.

관계가 있다.
```

라는 가설이다.

예:

```text
H1:
남성과 여성의 BMI 분포에는 차이가 있다.
```

---

# 13. p-value

통계 검정을 실행하면 자주 등장하는 값이:

```text
p-value
```

이다.

p-value는 귀무가설이 맞다고 가정했을 때 현재와 같거나 더 극단적인 결과가 관찰될 확률과 관련된 값이다.

초보 단계에서는 다음처럼 판단 기준을 잡을 수 있다.

```text
p < 0.05

→ 일반적인 5% 유의수준에서
귀무가설을 기각
→ 통계적으로 유의한 차이/관계가 있다고 판단
```

반면:

```text
p >= 0.05

→ 귀무가설을 기각할 근거가 충분하지 않음
```

이다.

---

# 14. p >= 0.05 해석 주의

다음처럼 표현하는 것은 조심해야 한다.

```text
두 그룹은 완전히 같다.
```

p-value가 0.05 이상이라는 것은:

```text
현재 데이터와 검정 방법에서
차이가 있다고 결론 내릴 충분한 근거를 확보하지 못했다.
```

에 가깝다.

따라서:

```text
통계적으로 유의한 차이를 확인하지 못했다.
```

라고 표현하는 것이 더 적절하다.

---

# 15. 0.05는 절대적인 법칙인가?

아니다.

```text
0.05
```

는 흔히 사용하는 유의수준 중 하나이다.

연구 목적에 따라:

```text
0.01
0.05
0.10
```

등 다른 기준을 사용할 수도 있다.

따라서 분석 전에 유의수준을 정하는 것이 좋다.

---

# 16. p-value가 작으면 차이가 큰 것인가?

반드시 그렇지는 않다.

```text
p-value
→ 통계적 증거와 관련

Effect Size
→ 차이의 실제 크기와 관련
```

이다.

표본이 매우 크면 작은 차이도 매우 작은 p-value가 나올 수 있다.

따라서:

```text
p-value
+
실제 차이의 크기
+
도메인 의미
```

를 함께 보는 것이 좋다.

---

# 17. 통계 검정을 선택하는 과정

검정 방법을 아무거나 선택하는 것이 아니다.

예를 들어 두 그룹의 수치형 변수를 비교한다면:

```text
수치형 데이터
      ↓
두 그룹 비교
      ↓
분포 확인
      ↓
정규성 확인
      ↓
조건에 맞는 검정 선택
```

처럼 진행할 수 있다.

---

# 18. 정규분포

정규분포(Normal Distribution)는 평균을 중심으로 좌우가 대칭적인 종 모양의 분포이다.

개념적으로:

```text
          /\
        /    \
      /        \
____/            \____
```

와 같은 형태이다.

많은 통계 방법에서 데이터의 분포에 대한 가정이 중요하다.

---

# 19. 정규성 확인

정규성을 확인하는 방법에는:

```text
Histogram
Q-Q Plot
Shapiro-Wilk Test
```

등이 있다.

통계검정 하나만 보고 판단하기보다 시각적인 분포와 함께 확인하는 것이 좋다.

---

# 20. Shapiro-Wilk Test

Python:

```python
from scipy.stats import shapiro

stat, p = shapiro(
    df["Age"].dropna()
)

print("Statistic:", stat)
print("p-value:", p)
```

---

# 21. Shapiro-Wilk 가설

```text
H0
→ 데이터가 정규분포를 따른다고 볼 수 있다.

H1
→ 데이터가 정규분포를 따른다고 보기 어렵다.
```

일반적인 0.05 기준에서:

```text
p >= 0.05
→ 정규성을 기각할 충분한 근거가 없음

p < 0.05
→ 정규성을 기각
```

으로 해석할 수 있다.

---

# 22. Shapiro 결과 예

```text
p-value = 0.00001
```

이라면:

```text
p < 0.05
```

이므로 정규분포 가정을 기각한다.

이후 데이터와 분석 목적에 따라 비모수 검정을 고려할 수 있다.

---

# 23. 표본이 크면 주의

표본 수가 매우 크면 Shapiro-Wilk 같은 정규성 검정은 작은 분포 차이에도 민감하게 반응할 수 있다.

따라서:

```text
p-value
+
Histogram
+
Q-Q Plot
+
표본 크기
```

를 함께 고려하는 것이 좋다.

---

# 24. 두 그룹의 평균 비교

예를 들어:

```text
남성 BMI

여성 BMI
```

두 독립 그룹의 평균 차이를 검정하고 싶다고 하자.

조건이 적절하다면:

```text
Independent t-test
독립표본 t 검정
```

을 사용할 수 있다.

---

# 25. Independent t-test

```python
from scipy.stats import ttest_ind

group1 = df.loc[
    df["Gender"] == "Male",
    "BMI"
].dropna()

group2 = df.loc[
    df["Gender"] == "Female",
    "BMI"
].dropna()

stat, p = ttest_ind(
    group1,
    group2
)

print("Statistic:", stat)
print("p-value:", p)
```

---

# 26. t-test의 기본 가설

```text
H0
→ 두 그룹의 평균에 차이가 없다.

H1
→ 두 그룹의 평균에 차이가 있다.
```

이다.

---

# 27. Welch's t-test

두 그룹의 분산이 같다고 가정하기 어려운 경우:

```python
stat, p = ttest_ind(
    group1,
    group2,
    equal_var=False
)
```

처럼 Welch's t-test를 사용할 수 있다.

실제 분석에서는 등분산성 여부와 데이터 구조를 함께 고려한다.

---

# 28. Mann-Whitney U Test

두 독립 그룹의 수치 데이터를 비교할 때 정규성 가정이 적절하지 않은 경우 자주 고려하는 비모수 검정이다.

```python
from scipy.stats import mannwhitneyu

stat, p = mannwhitneyu(
    group1,
    group2,
    alternative="two-sided"
)

print("U:", stat)
print("p-value:", p)
```

---

# 29. Mann-Whitney U의 가설

단순하게는:

```text
H0
→ 두 그룹의 분포에 차이가 없다.

H1
→ 두 그룹의 분포에 차이가 있다.
```

라고 이해할 수 있다.

Mann-Whitney U를 단순히:

```text
두 그룹의 평균 검정
```

이라고 생각하면 안 된다.

---

# 30. t-test와 Mann-Whitney U

초보 단계에서는 다음처럼 구분할 수 있다.

```text
독립된 두 그룹의 수치형 데이터 비교

정규성 등의 모수 검정 가정이 적절
→ t-test 고려

정규성 가정이 어렵거나
순위 기반 비교가 적절
→ Mann-Whitney U 고려
```

단순히 Shapiro 결과 하나만으로 자동 선택하는 것은 피한다.

---

# 31. 실제 분석 흐름

예:

```python
male = df.loc[
    df["Gender"] == "Male",
    "PainDuration"
].dropna()

female = df.loc[
    df["Gender"] == "Female",
    "PainDuration"
].dropna()
```

정규성 확인:

```python
print(
    shapiro(male)
)

print(
    shapiro(female)
)
```

분포 확인:

```python
import seaborn as sns
import matplotlib.pyplot as plt

sns.boxplot(
    data=df,
    x="Gender",
    y="PainDuration"
)

plt.show()
```

필요하면 Mann-Whitney U:

```python
stat, p = mannwhitneyu(
    male,
    female
)
```

이런 흐름으로 접근할 수 있다.

---

# 32. 두 수치형 변수의 관계

예:

```text
Age
BMI
```

또는:

```text
통증기간
통증정도
```

처럼 두 수치 변수의 관계를 확인하고 싶을 수 있다.

이때 대표적으로:

```text
Pearson Correlation

Spearman Correlation
```

을 사용할 수 있다.

---

# 33. Pearson 상관계수

Pearson correlation은 두 수치 변수 사이의 선형적인 관계를 측정한다.

```python
from scipy.stats import pearsonr

r, p = pearsonr(
    df["Age"],
    df["BMI"]
)

print("r:", r)
print("p-value:", p)
```

실제로는 결측값을 먼저 맞춰 제거해야 한다.

---

# 34. 결측값 처리 후 Pearson

```python
temp = df[
    ["Age", "BMI"]
].dropna()

r, p = pearsonr(
    temp["Age"],
    temp["BMI"]
)
```

두 변수에서 같은 행을 기준으로 결측값을 제거한다.

---

# 35. 상관계수 범위

상관계수는:

```text
-1 ~ +1
```

범위를 가진다.

```text
+1에 가까움
→ 강한 양의 관계

-1에 가까움
→ 강한 음의 관계

0에 가까움
→ 선형 관계가 약함
```

이다.

---

# 36. 상관계수 크기 해석 주의

예를 들어:

```text
r = 0.1
```

이면 일반적으로 강한 관계라고 보기 어렵다.

하지만:

```text
0.1 이하면 무조건 의미 없음
0.5 이상이면 무조건 강함
```

처럼 기계적으로 판단하면 안 된다.

분야와 데이터 특성을 함께 고려한다.

---

# 37. Spearman 상관계수

Spearman correlation은 값 자체보다 순위를 기반으로 관계를 측정한다.

```python
from scipy.stats import spearmanr

rho, p = spearmanr(
    temp["Age"],
    temp["BMI"]
)

print("rho:", rho)
print("p-value:", p)
```

---

# 38. Pearson과 Spearman

간단하게:

```text
Pearson
→ 선형 관계
→ 실제 수치값 기반

Spearman
→ 단조 관계
→ 순위 기반
→ 이상치나 비정규 데이터 상황에서 고려 가능
```

이다.

---

# 39. 단조 관계란?

X가 증가할수록 Y가 계속 증가하거나 계속 감소하는 경향을 의미한다.

꼭 직선일 필요는 없다.

따라서:

```text
Pearson
→ 직선 형태의 관계에 초점

Spearman
→ 순서가 함께 움직이는 관계에 초점
```

이라고 이해할 수 있다.

---

# 40. 상관관계의 가설

일반적으로:

```text
H0
→ 두 변수 사이에 해당 상관관계가 없다.

H1
→ 두 변수 사이에 해당 상관관계가 있다.
```

로 볼 수 있다.

따라서:

```text
상관계수
+
p-value
```

를 함께 확인한다.

---

# 41. 상관계수와 p-value는 다르다

예:

```text
rho = 0.10
p = 0.001
```

이라면 통계적으로는 관계가 있다고 볼 근거가 있을 수 있지만 관계의 크기는 약할 수 있다.

반대로 표본이 매우 적으면 비교적 큰 상관계수가 나와도 통계적 불확실성이 클 수 있다.

따라서:

```text
상관계수
→ 관계의 방향과 크기

p-value
→ 통계적 증거
```

로 구분한다.

---

# 42. Point-Biserial Correlation

한 변수는 연속형이고 다른 변수는 0/1 이진 변수일 때 사용할 수 있는 상관 분석 방법이다.

예:

```text
Age
→ 연속형

Outcome
→ 0 / 1
```

Python:

```python
from scipy.stats import pointbiserialr

temp = df[
    ["Age", "Outcome"]
].dropna()

r, p = pointbiserialr(
    temp["Outcome"],
    temp["Age"]
)

print("r:", r)
print("p-value:", p)
```

---

# 43. Point-Biserial을 왜 사용할까?

예를 들어:

```text
임신 성공 여부
0 / 1
```

와:

```text
Age
BMI
특정 검사 수치
```

같은 연속형 변수의 관계를 탐색할 때 사용할 수 있다.

하지만 이것만으로 Feature의 머신러닝 예측력을 완전히 판단할 수 있는 것은 아니다.

---

# 44. 범주형 vs 범주형

다음과 같은 두 범주형 변수의 관계를 보고 싶다고 하자.

```text
투여경로
→ 내복 / 외용 / 주사

금기등급
→ 1 / 2
```

이때 사용할 수 있는 대표적인 검정이:

```text
Chi-square Test
카이제곱 검정
```

이다.

---

# 45. 교차표

먼저 두 범주형 변수의 조합을 확인한다.

```python
table = pd.crosstab(
    df["Route"],
    df["Grade"]
)

display(table)
```

예:

```text
Grade       1      2

내복       100    500
외용        10     80
주사        20    100
```

---

# 46. Chi-square Test

```python
from scipy.stats import chi2_contingency

chi2, p, dof, expected = (
    chi2_contingency(
        table
    )
)

print("Chi-square:", chi2)
print("p-value:", p)
print("df:", dof)
```

---

# 47. Chi-square 가설

```text
H0
→ 두 범주형 변수는 독립이다.
→ 관계가 없다.

H1
→ 두 범주형 변수는 독립이 아니다.
→ 관계가 있다.
```

일반적인 기준에서:

```text
p < 0.05
```

이면 두 범주형 변수 사이에 통계적 연관성이 있다고 판단할 근거가 생긴다.

---

# 48. 기대빈도

`chi2_contingency()`는:

```python
expected
```

도 반환한다.

이는 두 변수가 독립이라고 가정했을 때 각 칸에서 기대되는 빈도이다.

실제 관측 빈도와 기대 빈도의 차이를 이용해 카이제곱 통계량을 계산한다.

---

# 49. 카이제곱도 p-value만 보면 안 된다

표본이 매우 크면 작은 차이도 통계적으로 유의하게 나타날 수 있다.

따라서:

```text
실제 비율

교차표

효과 크기

도메인 의미
```

도 함께 확인하는 것이 좋다.

---

# 50. VIF

VIF는:

```text
Variance Inflation Factor
```

의 약자이다.

여러 독립변수 사이의 다중공선성을 확인할 때 사용할 수 있다.

---

# 51. 다중공선성이란?

예를 들어 Feature가:

```text
키(cm)

키(m)

키를 이용해 만든 다른 변수
```

처럼 서로 매우 강하게 관련되어 있을 수 있다.

이렇게 독립변수끼리 강하게 연관되어 있으면 특히 회귀계수 해석 등에 문제가 생길 수 있다.

이를 다중공선성이라고 한다.

---

# 52. VIF 계산

```python
from statsmodels.stats.outliers_influence import (
    variance_inflation_factor
)

X = df[
    [
        "Age",
        "BMI",
        "Glucose"
    ]
].dropna()

vif_data = pd.DataFrame()

vif_data["feature"] = X.columns

vif_data["VIF"] = [
    variance_inflation_factor(
        X.values,
        i
    )
    for i in range(
        X.shape[1]
    )
]

display(vif_data)
```

---

# 53. VIF 해석

흔히 참고하는 기준으로:

```text
VIF ≈ 1
→ 다른 변수와 선형적인 중복 관계가 적음

VIF > 5
→ 다중공선성 가능성을 살펴볼 수 있음

VIF > 10
→ 높은 다중공선성을 의심하는 기준으로 자주 사용
```

등이 있다.

하지만 5나 10 역시 절대적인 법칙은 아니다.

---

# 54. VIF가 높으면 무조건 삭제?

아니다.

VIF가 높다고:

```text
이 Feature 삭제
```

를 자동으로 실행하면 안 된다.

다음도 확인해야 한다.

```text
변수의 의미

다른 변수와 왜 겹치는가?

모델의 목적

예측이 목적인가?

계수 해석이 중요한가?

Feature Engineering으로 만들어진 변수인가?
```

특히 Tree 기반 모델에서는 선형 회귀계수 해석 문제와 상황이 다를 수 있다.

---

# 55. 어떤 통계 검정을 사용할까?

간단하게 정리하면:

| 분석 질문 | 대표적인 방법 |
|---|---|
| 한 수치 변수의 정규성 | Shapiro-Wilk |
| 독립된 두 그룹 수치 비교 | t-test |
| 독립된 두 그룹 비모수 비교 | Mann-Whitney U |
| 두 수치 변수 선형 관계 | Pearson |
| 두 수치 변수 순위/단조 관계 | Spearman |
| 연속형 + 0/1 변수 관계 | Point-Biserial |
| 범주형 + 범주형 관계 | Chi-square |
| 독립변수 간 다중공선성 | VIF |

이 표는 시작점을 잡기 위한 것이다.

실제 검정 선택에는 데이터 구조와 가정이 추가로 필요하다.

---

# 56. 통계 분석과 머신러닝은 목적이 다르다

통계 분석에서는:

```text
이 변수와 Target 사이에 관계가 있는가?

두 그룹에 차이가 있는가?
```

같은 질문을 중요하게 볼 수 있다.

머신러닝에서는:

```text
이 Feature들을 함께 사용했을 때
새로운 데이터를 얼마나 잘 예측하는가?
```

가 중요하다.

따라서:

```text
p-value가 크다
→ Feature 무조건 삭제
```

라고 하면 안 된다.

---

# 57. 개별적으로 약해도 모델에서 유용할 수 있다

어떤 Feature 하나만 보면 Target과 강한 관계가 없을 수 있다.

하지만:

```text
Feature A
+
Feature B
+
Feature C
```

의 조합에서는 예측에 도움이 될 수 있다.

특히 비선형 모델에서는 Feature 간 상호작용을 학습할 수도 있다.

따라서 통계검정은 Feature 선택의 참고자료이지 절대적인 삭제 규칙은 아니다.

---

# 58. 통계적 유의성과 예측 성능

다음은 서로 다른 개념이다.

```text
통계적으로 유의하다.

머신러닝 예측력이 높다.
```

어떤 변수가 통계적으로 유의하더라도 모델 성능 향상이 작을 수 있다.

반대로 단독 통계검정에서는 약해 보여도 다른 Feature와 함께 모델 성능에 기여할 수 있다.

---

# 59. 다중검정 문제

많은 Feature에 대해 반복적으로 통계검정을 하면 우연히 p < 0.05가 나오는 변수도 증가할 수 있다.

예를 들어 수십 개의 Feature에 대해 계속 검정을 반복한다면:

```text
Multiple Testing
다중검정
```

문제를 생각해야 한다.

필요한 경우:

```text
Bonferroni correction

FDR
```

같은 보정 방법을 사용할 수 있다.

---

# 60. 표본 크기도 중요하다

같은 차이가 있어도:

```text
표본 10명

표본 10,000명
```

에서는 통계검정 결과가 달라질 수 있다.

표본이 매우 크면 작은 차이도 유의하게 나타날 수 있고, 표본이 너무 작으면 실제 차이가 있어도 충분한 통계적 증거를 얻지 못할 수 있다.

---

# 61. 결측치 처리

통계검정 전에 결측값을 확인한다.

예:

```python
group1 = df.loc[
    df["Outcome"] == 0,
    "BMI"
].dropna()

group2 = df.loc[
    df["Outcome"] == 1,
    "BMI"
].dropna()
```

두 변수를 함께 분석한다면:

```python
temp = df[
    ["Age", "BMI"]
].dropna()
```

처럼 동일한 행을 기준으로 결측값을 제거한다.

---

# 62. 시각화와 함께 사용

예를 들어 두 그룹을 비교한다면:

```python
sns.boxplot(
    data=df,
    x="Outcome",
    y="BMI"
)

plt.show()
```

그리고:

```python
stat, p = mannwhitneyu(
    group1,
    group2
)
```

를 사용할 수 있다.

분석 흐름:

```text
숫자 확인
   ↓
분포 시각화
   ↓
가정 확인
   ↓
적절한 검정
   ↓
p-value
   ↓
효과의 크기와 실제 의미 확인
```

이다.

---

# 63. 결과를 표현하는 방법

예:

```text
Mann-Whitney U = 361598.0
p = 0.8405
```

이라면:

```text
p > 0.05이므로
두 그룹의 분포에 통계적으로 유의한 차이를
확인하지 못했다.
```

처럼 표현할 수 있다.

---

# 64. 매우 작은 p-value

예:

```text
p = 1.52e-77
```

과 같은 값이 나올 수 있다.

이는 과학적 표기법이다.

```text
1.52 × 10^-77
```

을 의미한다.

매우 작은 숫자이다.

일반적인 0.05 기준보다 훨씬 작다.

---

# 65. e 표기법 읽기

예:

```text
4.41e-4
```

는:

```text
4.41 × 10^-4
= 0.000441
```

이다.

```text
1.67e-7
```

은:

```text
1.67 × 10^-7
= 0.000000167
```

이다.

Python과 데이터 분석 결과에서 자주 볼 수 있다.

---

# 66. p-value 출력

```python
print(
    f"p-value: {p:.6f}"
)
```

처럼 출력할 수 있다.

과학적 표기법:

```python
print(
    f"p-value: {p:.3e}"
)
```

예:

```text
p-value: 1.520e-77
```

---

# 67. 통계 분석 기본 코드 흐름

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

from scipy.stats import (
    shapiro,
    ttest_ind,
    mannwhitneyu,
    pearsonr,
    spearmanr,
    pointbiserialr,
    chi2_contingency
)
```

데이터 확인:

```python
print(df.shape)

df.info()

display(
    df.describe(
        include="all"
    )
)
```

시각화:

```python
sns.boxplot(
    data=df,
    x="Outcome",
    y="BMI"
)

plt.show()
```

그룹 생성:

```python
group0 = df.loc[
    df["Outcome"] == 0,
    "BMI"
].dropna()

group1 = df.loc[
    df["Outcome"] == 1,
    "BMI"
].dropna()
```

정규성 확인:

```python
print(
    shapiro(group0)
)

print(
    shapiro(group1)
)
```

필요한 검정:

```python
stat, p = mannwhitneyu(
    group0,
    group1,
    alternative="two-sided"
)

print("Statistic:", stat)
print("p-value:", p)
```

---

# 68. 통계 분석에서 피해야 할 방식

다음처럼 하면 안 된다.

```text
p < 0.05
→ 중요한 Feature 확정

p >= 0.05
→ 필요 없는 Feature 확정
```

통계검정 하나만으로 머신러닝 Feature를 결정하지 않는다.

또:

```text
p = 0.0001
→ 두 그룹 차이가 엄청 크다.
```

라고 단정하지 않는다.

p-value와 차이의 크기는 다른 개념이다.

---

# 69. 의료 데이터에서 특히 주의할 점

의료 데이터에서는:

```text
통계적으로 유의한가?
```

뿐 아니라:

```text
임상적으로 의미 있는 차이인가?
```

도 중요하다.

예를 들어 두 그룹의 평균 차이가 매우 작지만 표본이 많아 p-value가 매우 작게 나올 수도 있다.

따라서:

```text
통계적 유의성
+
효과 크기
+
임상적 의미
```

를 함께 고려해야 한다.

---

# 70. 전체 통계 분석 흐름

```text
분석 질문 설정
       ↓
변수 종류 확인
       ↓
기술통계 확인
       ↓
시각화
       ↓
분포 / 가정 확인
       ↓
적절한 통계검정 선택
       ↓
검정통계량 확인
       ↓
p-value 확인
       ↓
효과의 크기 확인
       ↓
도메인 관점에서 해석
```

---

# 71. 핵심 정리

### Shapiro-Wilk

```text
정규성 확인
```

### t-test

```text
두 독립 그룹의 평균 비교
```

### Mann-Whitney U

```text
두 독립 그룹의 분포를
순위 기반으로 비교
```

### Pearson

```text
두 수치 변수의 선형 관계
```

### Spearman

```text
두 변수의 순위/단조 관계
```

### Point-Biserial

```text
연속형 변수
+
0/1 이진 변수
```

### Chi-square

```text
범주형
+
범주형
```

### VIF

```text
독립변수들 사이의
다중공선성 확인
```

---

# 72. 배운 점

통계 분석은 단순히 p-value를 계산하는 작업이 아니다.

먼저:

```text
무엇을 비교하려는가?
```

를 명확하게 해야 한다.

그다음:

```text
변수의 종류
데이터 분포
그룹 구조
통계검정의 가정
```

을 확인하고 적절한 방법을 선택한다.

그리고:

```text
p < 0.05
```

라고 해서 무조건 중요한 Feature라고 할 수 없으며:

```text
p >= 0.05
```

라고 해서 두 그룹이 완전히 같다고 말할 수도 없다.

좋은 분석 흐름은:

```text
질문
 ↓
기술통계
 ↓
시각화
 ↓
가정 확인
 ↓
통계검정
 ↓
p-value
 ↓
효과의 크기
 ↓
도메인 해석
```

이다.

통계 분석과 머신러닝도 구분해야 한다.

```text
통계 분석
→ 차이와 관계를 설명

머신러닝
→ 새로운 데이터의 결과를 예측
```

따라서 통계 분석 결과를 Feature 선택의 참고자료로 활용할 수 있지만, 최종 머신러닝 Feature는 Validation 성능과 모델 해석 등을 함께 확인하여 결정하는 것이 좋다.