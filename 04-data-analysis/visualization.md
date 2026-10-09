# 데이터 시각화

## 1. 데이터 시각화란?

데이터 시각화(Data Visualization)는 숫자로 이루어진 데이터를 그래프로 표현하여 데이터의 특징을 쉽게 파악하는 과정이다.

예를 들어 다음 숫자만 보면:

```text
Age

20
21
22
23
24
25
80
```

80이라는 값이 조금 특이하다는 것을 알 수 있다.

하지만 데이터가 수천 개라면 숫자만 보고 특징을 찾기 어렵다.

그래프로 표현하면:

```text
분포
이상치
그룹 차이
변수 관계
추세
```

등을 쉽게 확인할 수 있다.

---

# 2. 왜 시각화를 할까?

시각화의 목적은 단순히 예쁜 그래프를 만드는 것이 아니다.

데이터를 보면서 다음 질문에 답하기 위해 사용한다.

```text
데이터가 어디에 많이 몰려 있는가?

한쪽으로 치우쳐 있는가?

이상치가 있는가?

그룹별 차이가 있는가?

두 변수 사이에 관계가 있는가?

Target에 따라 Feature 분포가 다른가?
```

즉 시각화도 EDA의 중요한 부분이다.

---

# 3. 대표적인 Python 시각화 Library

대표적으로:

```text
Matplotlib
Seaborn
```

을 많이 사용한다.

설치:

```bash
pip install matplotlib seaborn
```

불러오기:

```python
import matplotlib.pyplot as plt
import seaborn as sns
```

일반적으로:

```text
matplotlib.pyplot
→ plt

seaborn
→ sns
```

라는 별명을 사용한다.

---

# 4. Matplotlib과 Seaborn 차이

간단하게 이해하면:

```text
Matplotlib
→ Python의 기본적인 그래프 도구
→ 세부 설정을 직접 제어하기 좋음

Seaborn
→ Matplotlib 기반
→ 통계 데이터 시각화를 편리하게 작성
```

둘 중 하나만 사용하는 것이 아니라 같이 사용하는 경우가 많다.

---

# 5. 가장 기본적인 그래프

```python
import matplotlib.pyplot as plt

x = [1, 2, 3, 4, 5]
y = [10, 20, 15, 30, 25]

plt.plot(x, y)

plt.show()
```

`plt.show()`는 작성한 그래프를 화면에 표시한다.

---

# 6. 그래프 제목

```python
plt.plot(x, y)

plt.title("Example")

plt.show()
```

---

# 7. X축과 Y축 이름

```python
plt.plot(x, y)

plt.xlabel("X")
plt.ylabel("Y")

plt.show()
```

그래프를 만들 때:

```text
제목
X축
Y축
```

이 무엇을 의미하는지 표시하는 것이 좋다.

---

# 8. Figure 크기

```python
plt.figure(
    figsize=(8, 5)
)

plt.plot(x, y)

plt.show()
```

`figsize`는 그래프의 크기를 지정한다.

---

# 9. Histogram

Histogram은 수치형 변수의 분포를 확인할 때 많이 사용한다.

예:

```python
plt.hist(
    df["Age"].dropna()
)

plt.show()
```

또는 Pandas:

```python
df["Age"].hist()

plt.show()
```

---

# 10. Histogram으로 무엇을 볼까?

Histogram에서는 다음을 확인한다.

```text
어느 구간에 데이터가 많은가?

한쪽으로 치우쳐 있는가?

여러 봉우리가 존재하는가?

극단적인 값이 존재하는가?

전체적인 분포 형태는 어떤가?
```

---

# 11. bins

Histogram에서 `bins`는 구간 개수를 의미한다.

```python
plt.hist(
    df["Age"].dropna(),
    bins=20
)

plt.show()
```

`bins`가 너무 작으면:

```text
분포가 지나치게 단순해짐
```

너무 크면:

```text
그래프가 지나치게 잘게 나뉨
```

따라서 적절한 값을 선택한다.

---

# 12. Seaborn Histogram

```python
sns.histplot(
    data=df,
    x="Age"
)

plt.show()
```

KDE 곡선을 함께 표시할 수도 있다.

```python
sns.histplot(
    data=df,
    x="Age",
    kde=True
)

plt.show()
```

---

# 13. KDE란?

KDE는:

```text
Kernel Density Estimation
```

의 약자이다.

데이터의 분포를 부드러운 곡선으로 표현하는 방법이다.

Histogram과 같이 사용하면 전체적인 분포 형태를 확인하는 데 도움이 된다.

---

# 14. Target에 따른 Histogram

예를 들어:

```text
Outcome = 0
Outcome = 1
```

두 그룹의 BMI 분포를 비교할 수 있다.

```python
sns.histplot(
    data=df,
    x="BMI",
    hue="Outcome",
    kde=True
)

plt.show()
```

이를 통해:

```text
Target에 따라 BMI 분포가 다른가?
```

를 탐색할 수 있다.

---

# 15. Countplot

범주형 변수의 개수를 비교할 때 사용할 수 있다.

```python
sns.countplot(
    data=df,
    x="Gender"
)

plt.show()
```

예:

```text
Male      █████████
Female    ██████
```

처럼 범주별 데이터 수를 확인할 수 있다.

---

# 16. Target 클래스 확인

분류 문제에서는 Target의 분포를 확인하는 데 유용하다.

```python
sns.countplot(
    data=df,
    x="Outcome"
)

plt.show()
```

예:

```text
Outcome 0
→ 900개

Outcome 1
→ 100개
```

라면 클래스 불균형을 시각적으로 쉽게 확인할 수 있다.

---

# 17. Barplot

Barplot은 범주별 통계값을 비교할 때 사용할 수 있다.

예:

```python
sns.barplot(
    data=df,
    x="Gender",
    y="Age"
)

plt.show()
```

Gender 그룹에 따른 Age의 대표적인 통계적 차이를 시각화할 수 있다.

---

# 18. Countplot과 Barplot 차이

헷갈리기 쉬운 부분이다.

```text
Countplot
→ 각 범주가 몇 개인가?

Barplot
→ 각 범주의 수치형 값이 어떻게 다른가?
```

예:

```python
sns.countplot(
    data=df,
    x="Gender"
)
```

→ 남성/여성 수

반면:

```python
sns.barplot(
    data=df,
    x="Gender",
    y="Age"
)
```

→ 성별에 따른 Age 비교

이다.

---

# 19. Boxplot

Boxplot은 데이터의 분포와 이상치를 확인하는 데 유용하다.

```python
sns.boxplot(
    data=df,
    y="BMI"
)

plt.show()
```

확인할 수 있는 것:

```text
중앙값
Q1
Q3
IQR
극단적인 값
```

이다.

---

# 20. 그룹별 Boxplot

```python
sns.boxplot(
    data=df,
    x="Outcome",
    y="BMI"
)

plt.show()
```

이렇게 하면:

```text
Outcome = 0의 BMI 분포

Outcome = 1의 BMI 분포
```

를 비교할 수 있다.

---

# 21. 왜 Boxplot과 통계검정을 같이 사용할까?

Boxplot을 보면 두 그룹이 달라 보일 수 있다.

하지만:

```text
눈으로 달라 보인다
```

와:

```text
통계적으로 유의한 차이가 있다
```

는 같은 의미가 아니다.

따라서:

```text
시각화
+
통계 검정
```

을 함께 사용하는 경우가 많다.

예:

```text
Boxplot
→ 분포 차이를 눈으로 확인

Mann-Whitney U
→ 통계적으로 차이가 있는지 검정
```

---

# 22. Scatterplot

Scatterplot은 두 수치형 변수 사이의 관계를 확인할 때 사용한다.

```python
sns.scatterplot(
    data=df,
    x="Age",
    y="BMI"
)

plt.show()
```

각 점은 하나의 관측값을 나타낸다.

---

# 23. Scatterplot에서 확인할 것

```text
X가 증가하면 Y도 증가하는가?

X가 증가하면 Y는 감소하는가?

특별한 관계가 없어 보이는가?

이상치가 있는가?

특정 그룹이 따로 모여 있는가?
```

등을 확인한다.

---

# 24. 그룹을 색으로 구분

```python
sns.scatterplot(
    data=df,
    x="Age",
    y="BMI",
    hue="Outcome"
)

plt.show()
```

Target에 따라 점을 구분하면:

```text
두 클래스가 어느 정도 분리되는가?
```

를 탐색할 수 있다.

---

# 25. 상관관계

두 수치형 변수가 함께 변화하는 정도를 확인할 수 있다.

```python
corr = df.corr(
    numeric_only=True
)

display(corr)
```

상관계수는 보통:

```text
-1 ~ +1
```

범위에서 표현된다.

---

# 26. 상관계수의 방향

```text
양수
→ X가 증가할 때 Y도 증가하는 경향

음수
→ X가 증가할 때 Y는 감소하는 경향

0 근처
→ 선형적인 관계가 약함
```

이다.

---

# 27. 상관관계가 인과관계는 아니다

두 변수가 높은 상관관계를 가진다고 해서:

```text
X가 Y를 발생시킨다
```

고 바로 결론 내릴 수는 없다.

즉:

```text
Correlation
≠
Causation
```

이다.

---

# 28. Heatmap

상관관계를 시각적으로 확인할 수 있다.

```python
corr = df.corr(
    numeric_only=True
)

sns.heatmap(
    corr
)

plt.show()
```

---

# 29. 상관계수 표시

```python
sns.heatmap(
    corr,
    annot=True
)

plt.show()
```

`annot=True`를 사용하면 각 칸에 상관계수가 표시된다.

---

# 30. Heatmap이 너무 복잡한 경우

Feature가 매우 많다면 모든 변수의 Heatmap을 그리면 보기 어려워질 수 있다.

이 경우:

```text
중요한 Feature만 선택

Target과 상관이 높은 변수 확인

특정 변수 그룹만 선택
```

등의 방법을 사용할 수 있다.

---

# 31. Lineplot

시간이나 순서에 따른 변화는 Lineplot으로 볼 수 있다.

```python
sns.lineplot(
    data=df,
    x="date",
    y="value"
)

plt.show()
```

예:

```text
시간에 따른 환자 수

월별 매출

Epoch에 따른 Loss

Epoch에 따른 Accuracy
```

등을 표현할 수 있다.

---

# 32. 머신러닝 학습 곡선

Deep Learning에서도 Lineplot을 많이 사용한다.

예:

```python
plt.plot(
    train_losses,
    label="Train Loss"
)

plt.plot(
    val_losses,
    label="Validation Loss"
)

plt.legend()
plt.show()
```

이를 통해:

```text
학습이 잘 진행되는가?

Validation Loss가 다시 증가하는가?

Overfitting이 발생하는가?
```

등을 확인할 수 있다.

---

# 33. Overfitting을 그래프로 확인

예:

```text
Epoch 증가

Train Loss
→ 계속 감소

Validation Loss
→ 감소하다 다시 증가
```

한다면 모델이 Train 데이터에 지나치게 맞춰지는 Overfitting 가능성을 생각할 수 있다.

---

# 34. 여러 그래프 한 화면에 표시

Matplotlib에서는 `subplot`을 사용할 수 있다.

```python
plt.figure(
    figsize=(12, 5)
)

plt.subplot(1, 2, 1)

sns.histplot(
    data=df,
    x="Age"
)

plt.subplot(1, 2, 2)

sns.boxplot(
    data=df,
    y="Age"
)

plt.show()
```

한 변수의 Histogram과 Boxplot을 같이 비교할 수 있다.

---

# 35. Histogram + Boxplot

같은 변수라도 그래프마다 보여주는 정보가 다르다.

```text
Histogram
→ 전체적인 분포 형태

Boxplot
→ 중앙값, 사분위수, 이상치 후보
```

따라서 두 그래프를 같이 보면 데이터의 분포를 더 쉽게 이해할 수 있다.

---

# 36. 데이터 그룹별 시각화

예를 들어:

```python
sns.boxplot(
    data=df,
    x="Gender",
    y="Pain"
)

plt.show()
```

를 이용하면 성별에 따른 통증 분포를 비교할 수 있다.

이후 통계검정을 통해 실제 차이가 유의한지도 확인할 수 있다.

---

# 37. 변수 하나를 볼 때

수치형 변수 하나를 분석한다면:

```text
describe()
    ↓
Histogram
    ↓
Boxplot
```

순서로 볼 수 있다.

예:

```python
df["BMI"].describe()
```

```python
sns.histplot(
    data=df,
    x="BMI",
    kde=True
)

plt.show()
```

```python
sns.boxplot(
    data=df,
    y="BMI"
)

plt.show()
```

---

# 38. 범주형 변수 하나를 볼 때

```python
df["Gender"].value_counts()
```

그리고:

```python
sns.countplot(
    data=df,
    x="Gender"
)

plt.show()
```

처럼 분석할 수 있다.

---

# 39. 수치형 vs 범주형

예:

```text
BMI
vs
Outcome
```

이라면:

```python
sns.boxplot(
    data=df,
    x="Outcome",
    y="BMI"
)

plt.show()
```

또는:

```python
sns.histplot(
    data=df,
    x="BMI",
    hue="Outcome",
    kde=True
)

plt.show()
```

등을 사용할 수 있다.

---

# 40. 수치형 vs 수치형

예:

```text
Age
vs
BMI
```

라면 Scatterplot을 사용할 수 있다.

```python
sns.scatterplot(
    data=df,
    x="Age",
    y="BMI"
)

plt.show()
```

---

# 41. 범주형 vs 범주형

예:

```text
Gender
vs
Outcome
```

이라면 Cross Table을 만들 수 있다.

```python
table = pd.crosstab(
    df["Gender"],
    df["Outcome"]
)

display(table)
```

시각화:

```python
table.plot(
    kind="bar"
)

plt.show()
```

범주형 변수 사이의 통계적 관계는 이후 카이제곱 검정으로 확인할 수도 있다.

---

# 42. 그래프 선택 기본표

| 확인하려는 것 | 그래프 |
|---|---|
| 수치형 변수 분포 | Histogram |
| 이상치 후보 | Boxplot |
| 범주별 개수 | Countplot |
| 범주별 수치 비교 | Barplot / Boxplot |
| 두 수치 변수 관계 | Scatterplot |
| 여러 수치 변수 상관 | Heatmap |
| 시간/순서 변화 | Lineplot |
| Target 클래스 수 | Countplot |

---

# 43. 그래프만 보고 결론 내리지 않는다

예를 들어 Boxplot에서:

```text
Group A
Group B
```

가 달라 보인다고 하자.

그래프는:

```text
차이가 있어 보인다
```

는 탐색 결과를 제공한다.

하지만 통계적으로 유의한 차이인지 확인하려면 상황에 따라:

```text
t-test

Mann-Whitney U

ANOVA

Kruskal-Wallis

Chi-square
```

등의 통계 검정을 고려할 수 있다.

---

# 44. p-value와 연결

예를 들어 두 그룹의 BMI를 비교한다고 하자.

먼저:

```python
sns.boxplot(
    data=df,
    x="Outcome",
    y="BMI"
)

plt.show()
```

시각적으로 확인한다.

그다음 적절한 통계 검정을 통해:

```text
관찰된 차이가 우연으로 설명될 가능성을 어떻게 볼 것인가?
```

를 평가한다.

통계검정은 `statistics.md`에서 자세히 정리한다.

---

# 45. 클래스 불균형 시각화

```python
sns.countplot(
    data=df,
    x="Outcome"
)

plt.show()
```

만으로도 클래스 불균형을 쉽게 확인할 수 있다.

하지만 숫자도 함께 확인한다.

```python
print(
    df["Outcome"].value_counts()
)

print(
    df["Outcome"].value_counts(
        normalize=True
    ) * 100
)
```

그래프와 실제 숫자를 함께 보는 것이 좋다.

---

# 46. Feature Importance 시각화

머신러닝 모델을 학습한 후 Feature Importance를 시각화할 수도 있다.

예:

```python
importance = model.feature_importances_
```

이후 중요도가 높은 Feature를 Barplot 등으로 표현할 수 있다.

이 부분은 머신러닝 단원에서 자세히 다룬다.

---

# 47. SHAP 시각화

모델이 어떤 Feature를 이용하여 예측했는지 해석하기 위해 SHAP을 사용할 수도 있다.

대표적으로:

```text
Summary Plot
Bar Plot
Waterfall Plot
```

등이 있다.

이것은 단순한 EDA 시각화라기보다 모델 해석에 가깝기 때문에 머신러닝 단원에서 자세히 다룬다.

---

# 48. 이미지 데이터 시각화

Computer Vision에서는 실제 이미지를 확인하는 것도 시각화이다.

```python
from PIL import Image
import matplotlib.pyplot as plt

img = Image.open(
    "image.png"
)

plt.imshow(
    img,
    cmap="gray"
)

plt.axis("off")

plt.show()
```

---

# 49. 왜 이미지를 직접 확인할까?

이미지 데이터에서는 다음 문제를 찾을 수 있다.

```text
이미지가 뒤집혀 있음

크기가 서로 다름

흑백/컬러가 예상과 다름

잘못된 Label

손상된 이미지

지나치게 어둡거나 밝음

불필요한 여백
```

CSV만 확인해서는 발견하기 어려운 문제이다.

---

# 50. 흉부 X-ray 같은 이미지 데이터

예를 들어 흉부 X-ray 분류라면 클래스별 이미지를 직접 확인할 수 있다.

```text
Normal 이미지

Pneumonia 이미지
```

그리고 클래스별:

```text
밝기
패턴
크기
촬영 방향
```

등에 차이가 있는지도 탐색할 수 있다.

모델이 실제 질환 특징이 아니라 촬영 방식이나 이미지 밝기 같은 차이를 학습하지 않는지도 생각해야 한다.

---

# 51. 이미지 밝기 분포

이미지를 NumPy 배열로 바꿔 평균 밝기를 계산할 수도 있다.

```python
import numpy as np

image_array = np.array(img)

mean_brightness = (
    image_array.mean()
)

print(mean_brightness)
```

클래스별 평균 밝기가 크게 다르다면 그 이유를 확인할 필요가 있다.

---

# 52. 좋은 시각화의 기본

좋은 그래프는:

```text
무엇을 보여주는지 명확하고

축의 의미가 명확하고

필요한 정보가 잘 보이며

불필요하게 복잡하지 않아야 한다.
```

그래프가 화려하다고 좋은 분석은 아니다.

---

# 53. 제목을 구체적으로 작성

좋지 않은 예:

```python
plt.title("Graph")
```

더 나은 예:

```python
plt.title(
    "BMI Distribution by Outcome"
)
```

그래프만 보더라도 무엇을 비교하는지 알 수 있게 작성한다.

---

# 54. 그래프를 만든 뒤 설명하기

분석에서는 그래프를 보여주고 끝내지 않는다.

예:

```text
Outcome 1 그룹에서 BMI 중앙값이
조금 더 높아 보인다.
```

처럼 관찰 내용을 설명한다.

그다음:

```text
이 차이가 통계적으로 유의한지는
추가 검정이 필요하다.
```

처럼 과도한 결론을 피한다.

---

# 55. 시각화 분석 흐름

```text
분석 질문 설정
      ↓
변수 종류 확인
      ↓
적절한 그래프 선택
      ↓
분포/관계 확인
      ↓
이상한 패턴 확인
      ↓
숫자로 다시 확인
      ↓
필요하면 통계 검정
      ↓
전처리 / Feature Engineering 결정
```

---

# 56. 그래프 선택 예시

질문:

```text
BMI는 어떤 분포인가?
```

사용:

```text
Histogram
Boxplot
```

질문:

```text
Outcome별 BMI가 다른가?
```

사용:

```text
Boxplot
Histogram
```

질문:

```text
Age와 BMI 관계가 있는가?
```

사용:

```text
Scatterplot
Correlation
```

질문:

```text
Outcome 클래스가 불균형한가?
```

사용:

```text
Countplot
value_counts()
```

질문:

```text
여러 수치 변수끼리 어떤 관계가 있는가?
```

사용:

```text
Correlation Heatmap
```

---

# 57. 실전 EDA 기본 코드

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv(
    "data.csv"
)

print(df.shape)

display(
    df.head()
)

df.info()

display(
    df.describe(
        include="all"
    )
)
```

수치형 변수:

```python
numeric_cols = df.select_dtypes(
    include="number"
).columns
```

Histogram:

```python
for col in numeric_cols:

    plt.figure(
        figsize=(7, 4)
    )

    sns.histplot(
        data=df,
        x=col,
        kde=True
    )

    plt.title(
        f"{col} Distribution"
    )

    plt.show()
```

Boxplot:

```python
for col in numeric_cols:

    plt.figure(
        figsize=(7, 3)
    )

    sns.boxplot(
        data=df,
        x=col
    )

    plt.title(
        f"{col} Boxplot"
    )

    plt.show()
```

---

# 58. 시각화 결과를 기록한다

예:

```text
Age
→ 20~60대에 주로 분포
→ 일부 고령 이상치 후보 존재

BMI
→ 오른쪽으로 약간 치우친 분포
→ 상단에 극단값 존재

Outcome
→ Class 0이 Class 1보다 많음

Age vs BMI
→ 강한 선형 관계는 보이지 않음
```

처럼 기록한다.

이 기록이 이후:

```text
전처리
통계검정
Feature Engineering
모델링
```

의 근거가 된다.

---

# 59. 시각화에서 가장 중요한 질문

그래프를 그리기 전에:

```text
내가 무엇을 알고 싶은가?
```

를 먼저 생각한다.

그다음:

```text
어떤 변수인가?

수치형인가?

범주형인가?

두 그룹을 비교하는가?

두 숫자의 관계를 보는가?

시간에 따른 변화를 보는가?
```

를 생각하면 적절한 그래프를 선택하기 쉬워진다.

---

# 60. 배운 점

데이터 시각화는 단순히 예쁜 그래프를 만드는 작업이 아니다.

데이터를 이해하기 위한 EDA 과정이다.

대표적으로:

```text
Histogram
→ 수치형 데이터 분포

Countplot
→ 범주별 개수

Barplot
→ 범주별 수치 비교

Boxplot
→ 분포와 이상치 후보

Scatterplot
→ 두 수치 변수 관계

Heatmap
→ 여러 변수의 상관관계

Lineplot
→ 시간 또는 순서에 따른 변화
```

를 확인할 수 있다.

하지만 그래프에서 차이가 보인다고 해서 바로:

```text
두 그룹은 통계적으로 다르다.
```

라고 결론 내리면 안 된다.

좋은 분석 흐름은:

```text
질문
 ↓
시각화
 ↓
패턴 발견
 ↓
숫자로 확인
 ↓
필요하면 통계 검정
 ↓
해석
```

이다.

결국 시각화에서 가장 중요한 것은:

```text
어떤 그래프를 그릴까?
```

보다:

```text
이 그래프를 통해 무엇을 확인하려고 하는가?
```

를 먼저 생각하는 것이다.