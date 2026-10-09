# 데이터 탐색과 기본 점검

## 1. 데이터를 받으면 바로 모델링하지 않는다

머신러닝 프로젝트에서 CSV 파일을 받았다고 바로 다음 코드를 실행하는 것은 좋지 않다.

```python
model.fit(X, y)
```

먼저 데이터가 어떤 상태인지 확인해야 한다.

실제 데이터에는 다음과 같은 문제가 있을 수 있다.

```text
결측값
이상치
잘못 입력된 문자열
중복 데이터
잘못된 자료형
클래스 불균형
불필요한 변수
ID Column
Target 오류
```

따라서 먼저 데이터를 탐색하는 과정이 필요하다.

이러한 과정을 흔히 EDA라고 한다.

```text
EDA
Exploratory Data Analysis
탐색적 데이터 분석
```

---

# 2. 전체 데이터 분석 흐름

데이터를 처음 받으면 다음과 같은 순서로 확인할 수 있다.

```text
1. 파일 불러오기
        ↓
2. 데이터 크기 확인
        ↓
3. 실제 데이터 확인
        ↓
4. Column 확인
        ↓
5. 자료형 확인
        ↓
6. 결측치 확인
        ↓
7. 중복 확인
        ↓
8. 값의 종류 확인
        ↓
9. 수치 분포 확인
        ↓
10. Target 확인
        ↓
11. 이상한 값 탐색
        ↓
12. 전처리 방법 결정
```

이 과정을 거친 뒤 머신러닝에 사용할 데이터를 만든다.

---

# 3. 데이터 불러오기

가장 먼저 데이터를 불러온다.

```python
import pandas as pd

train = pd.read_csv("train.csv")
test = pd.read_csv("test.csv")
```

Colab이라면:

```python
train = pd.read_csv("/content/train.csv")
test = pd.read_csv("/content/test.csv")
```

처럼 경로를 사용할 수도 있다.

---

# 4. 데이터 크기 확인

```python
print("train:", train.shape)
print("test :", test.shape)
```

예:

```text
train: (5216, 2)
test : (624, 1)
```

의미:

```text
train
→ 5216행, 2열

test
→ 624행, 1열
```

이다.

---

# 5. train과 test의 Column 수가 다른 이유

대회 데이터에서는 train과 test의 Column 수가 하나 차이 나는 경우가 많다.

예:

```text
train
image_path
label

test
image_path
```

Train에는 모델이 학습해야 하는 정답:

```text
label
```

이 존재한다.

Test에는 정답이 제공되지 않는다.

모델이 Test의 정답을 예측해야 하기 때문이다.

---

# 6. 데이터 앞부분 확인

```python
train.head()
```

데이터를 직접 눈으로 확인한다.

이 과정에서:

```text
Column 이름
값의 형태
숫자인지 문자열인지
ID가 있는지
Target이 있는지
```

등을 확인할 수 있다.

---

# 7. 데이터 뒷부분 확인

```python
train.tail()
```

앞부분만 보는 것보다 마지막 데이터도 확인하면 데이터 구조가 일정한지 확인하는 데 도움이 된다.

---

# 8. Column 확인

```python
print(train.columns)
```

예:

```text
Index([
    'Age',
    'BMI',
    'Glucose',
    'Outcome'
], dtype='object')
```

Column 이름을 정확하게 확인해야 한다.

예를 들어:

```python
train["age"]
```

와:

```python
train["Age"]
```

는 서로 다른 이름이다.

---

# 9. Column 이름을 직접 확인하는 이유

데이터 분석 중 다음과 같은 오류가 발생할 수 있다.

```text
KeyError
```

예:

```python
train["age"]
```

를 실행했는데 실제 Column 이름이:

```text
Age
```

라면 해당 Column을 찾지 못한다.

따라서:

```python
print(train.columns.tolist())
```

처럼 정확한 이름을 확인하는 습관이 중요하다.

---

# 10. info()

```python
train.info()
```

를 실행하면 데이터의 전체 구조를 확인할 수 있다.

대표적으로:

```text
행 개수
Column 이름
Non-Null 개수
자료형
```

을 확인한다.

예:

```text
Age          1000 non-null    int64
BMI           980 non-null    float64
Gender       1000 non-null    object
Outcome      1000 non-null    int64
```

여기서 BMI가 980개라면:

```text
1000 - 980 = 20
```

개의 결측값이 있을 가능성을 생각할 수 있다.

---

# 11. 자료형 확인

```python
train.dtypes
```

대표적인 자료형:

```text
int64
→ 정수

float64
→ 실수

object
→ 문자열 또는 혼합 데이터일 가능성

bool
→ True / False
```

자료형은 머신러닝 전처리 방법을 결정하는 데 중요하다.

---

# 12. object라고 모두 정상 문자열은 아니다

다음과 같은 Column이 있다고 하자.

```text
NRS_pain
```

원래 숫자여야 하는데 데이터 중간에:

```text
#BOŢ!
```

같은 값이 들어 있다면 Pandas가 전체 Column을 문자열 계열로 판단할 수 있다.

따라서:

```python
train["NRS_pain"].dtype
```

가 `object`라면:

```text
원래 범주형 변수인가?

아니면 숫자 Column에 잘못된 문자열이 들어갔는가?
```

를 확인해야 한다.

---

# 13. describe()

수치형 변수의 분포를 확인한다.

```python
train.describe()
```

대표적인 결과:

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

이를 통해 이상한 값이 있는지 빠르게 확인할 수 있다.

---

# 14. min과 max 확인

예를 들어 Age가:

```text
min = -3
max = 250
```

이라고 나온다면 정상적인 나이 데이터라고 보기 어렵다.

이런 값은:

```text
입력 오류인지
특수한 의미를 가진 코드인지
결측값을 특정 숫자로 표현한 것인지
```

확인해야 한다.

무조건 삭제하면 안 된다.

---

# 15. 문자열 Column도 확인

`describe()`는 기본적으로 수치형 변수 중심으로 보여준다.

문자열을 포함한 전체 데이터를 보고 싶다면:

```python
train.describe(include="all")
```

을 사용할 수 있다.

범주형 변수에서는:

```text
count
unique
top
freq
```

등을 확인할 수 있다.

---

# 16. 결측치 확인

가장 기본적인 방법:

```python
train.isnull().sum()
```

Column별 결측값 개수를 확인한다.

결측치가 있는 Column만 보고 싶다면:

```python
missing = train.isnull().sum()

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

# 17. 결측률 확인

결측 개수뿐만 아니라 비율도 중요하다.

```python
missing_count = train.isnull().sum()

missing_ratio = (
    train.isnull().mean() * 100
)

missing_table = pd.DataFrame({
    "missing_count": missing_count,
    "missing_ratio": missing_ratio
})

missing_table = missing_table[
    missing_table["missing_count"] > 0
]

missing_table = missing_table.sort_values(
    "missing_ratio",
    ascending=False
)

display(missing_table)
```

예:

```text
                 missing_count   missing_ratio
Column_A             500             50.0
Column_B             100             10.0
Column_C              10              1.0
```

결측치가 몇 개인지만 보는 것보다 전체 데이터 중 몇 %인지 같이 보는 것이 좋다.

---

# 18. 중복 데이터 확인

```python
train.duplicated().sum()
```

전체 행이 완전히 같은 데이터가 몇 개인지 확인한다.

예:

```text
25
```

라면 25개의 중복 행이 존재한다.

---

# 19. 중복 데이터를 바로 삭제하면 안 되는 이유

중복이라고 해서 항상 오류는 아니다.

예를 들어:

```text
나이 = 30
성별 = 남
BMI = 25
```

인 사람이 여러 명 존재할 수 있다.

따라서 중복을 발견했다면:

```text
정말 동일한 환자가 중복 저장된 것인가?

서로 다른 사람이 우연히 같은 값을 가진 것인가?

고유 ID가 존재하는가?
```

등을 확인해야 한다.

---

# 20. 고유값 확인

범주형 변수에서는:

```python
train["Gender"].unique()
```

를 사용할 수 있다.

예:

```text
['Male', 'Female']
```

하지만 다음과 같이 나온다면:

```text
['Male', 'Female', 'male', 'F', '?']
```

데이터 정리가 필요할 가능성이 있다.

---

# 21. 고유값 개수

```python
train["Gender"].nunique()
```

고유한 값의 개수를 확인한다.

많은 Column을 한 번에 확인하려면:

```python
train.nunique()
```

도 사용할 수 있다.

---

# 22. value_counts()

범주별 데이터 개수를 확인한다.

```python
train["Gender"].value_counts()
```

예:

```text
Male      600
Female    400
```

이렇게 실제 데이터 분포를 확인할 수 있다.

---

# 23. 결측값까지 포함하여 확인

```python
train["Gender"].value_counts(
    dropna=False
)
```

를 사용하면 `NaN`도 함께 확인할 수 있다.

범주형 변수를 점검할 때 유용하다.

---

# 24. 모든 범주형 Column 확인

다음처럼 반복문을 사용할 수도 있다.

```python
categorical_cols = train.select_dtypes(
    include="object"
).columns

for col in categorical_cols:

    print("=" * 50)
    print(col)

    print(
        train[col].value_counts(
            dropna=False
        ).head(20)
    )
```

이 코드는 문자열 형태의 Column을 하나씩 확인한다.

---

# 25. 숫자로 변환 가능한지 확인

원래 숫자여야 하는 Column에 이상한 문자열이 들어 있을 수 있다.

예:

```python
train["SBP"]
```

에:

```text
120
130
??
140
```

이 있다고 하자.

다음처럼 숫자로 변환해볼 수 있다.

```python
sbp_numeric = pd.to_numeric(
    train["SBP"],
    errors="coerce"
)
```

`errors="coerce"`는 숫자로 바꿀 수 없는 값을 `NaN`으로 만든다.

---

# 26. 잘못된 값을 찾는 방법

원래는 값이 있었지만 숫자로 변환한 후 `NaN`이 된 행을 찾을 수 있다.

```python
converted = pd.to_numeric(
    train["SBP"],
    errors="coerce"
)

invalid_mask = (
    train["SBP"].notna()
    &
    converted.isna()
)

train.loc[
    invalid_mask,
    ["SBP"]
]
```

이를 통해 실제 어떤 문자열 때문에 숫자 변환이 안 되는지 확인할 수 있다.

---

# 27. 실제 데이터에서 이상한 값

실제 의료 데이터에서도 숫자 Column에 다음과 같은 값이 포함될 수 있다.

```text
??
#BOŢ!
```

이런 값을 발견하면 바로 모델에 넣지 않고:

```text
왜 이런 값이 있는지
몇 개인지
어떤 Column에 있는지
어떻게 처리할지
```

를 결정해야 한다.

---

# 28. Target 확인

지도학습에서는 Target 확인이 매우 중요하다.

예:

```python
train["Outcome"].value_counts()
```

결과:

```text
0    750
1    250
```

이라면:

```text
0 → 75%
1 → 25%
```

이다.

---

# 29. Target 비율

```python
train["Outcome"].value_counts(
    normalize=True
) * 100
```

결과 예:

```text
0    75.0
1    25.0
```

클래스 불균형이 존재하는지 판단하는 데 도움이 된다.

---

# 30. 클래스 불균형

예를 들어:

```text
0 = 950
1 = 50
```

이라면 상당한 불균형이 존재한다.

이 경우 단순 Accuracy만 사용하면 모델 성능을 잘못 판단할 수 있다.

예를 들어 모델이 모든 데이터를 0이라고 예측해도:

```text
Accuracy = 95%
```

가 될 수 있다.

하지만 실제로 중요한 1을 하나도 찾지 못한다.

따라서 이후 머신러닝에서는:

```text
Precision
Recall
F1-score
ROC-AUC
PR-AUC
```

등도 함께 확인할 필요가 있다.

---

# 31. ID Column 확인

데이터에는 다음과 같은 Column이 있을 수 있다.

```text
Patient_ID
ID
Number
Index
```

이런 값은 각 데이터를 구분하기 위한 식별자일 수 있다.

예:

```text
P0001
P0002
P0003
```

이 값 자체가 예측에 의미가 없는 단순 식별자라면 Feature에서 제외하는 것을 고려할 수 있다.

하지만 무조건 제거하기 전에 데이터 명세와 역할을 확인해야 한다.

---

# 32. 데이터 명세서 확인

데이터를 분석할 때 CSV만 보는 것보다 데이터 명세서가 있다면 함께 확인하는 것이 중요하다.

명세서에는 다음과 같은 정보가 있을 수 있다.

```text
Column 이름
Column 설명
단위
범주 코드
결측값 의미
Target 정의
수집 방법
```

예를 들어:

```text
성별
0 = 여성
1 = 남성
```

인지:

```text
1 = 여성
2 = 남성
```

인지 데이터만 보고 추측하면 위험하다.

---

# 33. 단위 확인

의료 데이터에서는 단위도 매우 중요하다.

예:

```text
Height
→ cm인가?
→ m인가?

Weight
→ kg인가?

Glucose
→ mg/dL인가?

통증기간
→ 일인가?
→ 월인가?
```

단위를 잘못 이해하면 Feature Engineering이나 통계 분석 결과가 완전히 달라질 수 있다.

---

# 34. 날짜 Column 확인

날짜가 문자열로 저장될 수 있다.

예:

```text
2026-09-01
2026-09-02
```

자료형을 확인하면:

```text
object
```

일 수 있다.

날짜로 변환:

```python
train["date"] = pd.to_datetime(
    train["date"],
    errors="coerce"
)
```

이후:

```python
train["date"].dt.year
train["date"].dt.month
train["date"].dt.day
```

처럼 활용할 수 있다.

---

# 35. 데이터 범위 확인

수치형 변수는 최소/최대 범위를 확인한다.

```python
numeric_cols = train.select_dtypes(
    include="number"
).columns

for col in numeric_cols:

    print(
        col,
        train[col].min(),
        train[col].max()
    )
```

이를 통해 비정상적으로 큰 값이나 작은 값을 찾을 수 있다.

---

# 36. 평균만 보면 안 되는 이유

다음 두 데이터가 있다고 하자.

```text
A
50 50 50 50 50

B
10 30 50 70 90
```

둘의 평균은 모두 50이다.

하지만 분포는 완전히 다르다.

따라서:

```text
평균
중앙값
표준편차
최솟값
최댓값
사분위수
```

등을 함께 확인해야 한다.

---

# 37. 분포를 시각적으로 확인

숫자만으로 모든 것을 파악하기 어려울 수 있다.

따라서 이후에는:

```text
Histogram
Boxplot
Barplot
Scatterplot
```

등을 이용해 데이터를 시각적으로 확인한다.

예:

```python
train["Age"].hist()
```

이렇게 하면 나이 분포를 확인할 수 있다.

시각화는 이후 `visualization.md`에서 자세히 정리한다.

---

# 38. Target에 따른 차이 확인

예를 들어 임신 성공 여부가 Target이라면:

```python
train.groupby(
    "임신 성공 여부"
)["Age"].mean()
```

처럼 Target별 평균 차이를 확인할 수 있다.

또는:

```python
train.groupby(
    "Outcome"
)["BMI"].median()
```

처럼 중앙값을 비교할 수도 있다.

이것은:

```text
이 변수가 Target과 관련이 있을 가능성이 있는가?
```

를 탐색하는 과정이다.

---

# 39. groupby()

`groupby()`는 특정 그룹별로 데이터를 분석할 때 사용한다.

예:

```python
train.groupby(
    "Gender"
)["Age"].mean()
```

의미:

```text
Gender별로 데이터를 나눈다.
        ↓
각 그룹의 Age 평균을 계산한다.
```

이다.

---

# 40. 여러 통계량 확인

```python
train.groupby(
    "Outcome"
)["Age"].agg(
    ["count", "mean", "median", "std"]
)
```

Target 그룹별:

```text
데이터 개수
평균
중앙값
표준편차
```

를 한 번에 확인할 수 있다.

---

# 41. 상관관계

수치형 변수끼리 관계가 있는지 간단하게 확인할 수도 있다.

```python
corr = train.corr(
    numeric_only=True
)

display(corr)
```

상관계수는 일반적으로:

```text
+ 방향
→ 한 변수가 증가할 때 다른 변수도 증가하는 경향

- 방향
→ 한 변수가 증가할 때 다른 변수는 감소하는 경향

0 근처
→ 선형 관계가 약함
```

정도로 해석할 수 있다.

하지만 상관관계가 있다고 해서 인과관계가 있다는 뜻은 아니다.

---

# 42. 데이터 누수 확인

머신러닝에서는 Data Leakage도 확인해야 한다.

예를 들어 예측하려는 Target이:

```text
수술실패여부
```

인데 Feature에:

```text
수술실패 후 처치
```

같이 결과가 발생한 뒤에 알 수 있는 정보가 들어 있다면 문제가 될 수 있다.

모델은 실제 예측 시점에는 알 수 없는 정보를 사용해서는 안 된다.

---

# 43. 예측 시점을 생각한다

Feature를 선택할 때 중요한 질문:

```text
이 정보를 실제 예측하는 순간에 알 수 있는가?
```

예를 들어 응급실 도착 시 오분류 위험을 예측한다면:

```text
도착 당시 나이
혈압
심박수
체온
통증 정도
```

는 사용할 수 있을 가능성이 있다.

하지만 이후에 확정된 결과를 Feature로 사용하면 데이터 누수가 발생할 수 있다.

---

# 44. Train과 Test 구조 비교

대회 데이터에서는 Train과 Test의 Feature가 맞는지도 확인한다.

```python
train_cols = set(train.columns)
test_cols = set(test.columns)

print(
    "train에만 존재:",
    train_cols - test_cols
)

print(
    "test에만 존재:",
    test_cols - train_cols
)
```

Train에만 Target이 존재한다면 정상적인 구조일 수 있다.

---

# 45. Train과 Test 분포 차이

Train과 Test의 분포가 크게 다르면 모델 성능에 영향을 줄 수 있다.

예를 들어:

```python
print(
    train["Age"].mean()
)

print(
    test["Age"].mean()
)
```

처럼 간단하게 비교할 수 있다.

하지만 평균 하나만으로 판단하지 않고 전체 분포도 확인해야 한다.

---

# 46. Sample Submission 확인

대회에서는:

```python
submission = pd.read_csv(
    "sample_submission.csv"
)
```

도 확인한다.

```python
submission.head()
```

```python
submission.shape
```

```python
submission.columns
```

을 확인한다.

이 파일은 최종적으로:

```text
어떤 Column 이름으로
몇 개의 예측값을
어떤 구조로 제출해야 하는지
```

알려주는 예시 파일이다.

---

# 47. 이미지 데이터도 CSV만 보면 안 된다

Computer Vision 프로젝트에서는 CSV뿐 아니라 실제 이미지도 확인해야 한다.

예:

```text
이미지 개수
이미지 크기
Channel
파일 경로
Label
손상된 이미지 여부
```

등을 확인해야 한다.

예를 들어:

```text
train.csv = 5216행
train 이미지 = 5216개
```

라면 개수가 맞는지 확인할 수 있다.

---

# 48. 의료 데이터에서 특히 확인할 것

의료 데이터에서는 다음을 특히 확인한다.

```text
환자 단위 데이터인가?

한 환자에게 여러 행이 존재하는가?

단위는 무엇인가?

0이 실제 0인가?

0이 결측을 의미하는가?

진단 전 정보인가?

진단 후 정보인가?

같은 환자가 Train과 Validation에 동시에 들어갈 가능성이 있는가?
```

이런 부분은 모델 성능보다 먼저 확인해야 할 수도 있다.

---

# 49. EDA와 전처리는 다르다

EDA:

```text
데이터가 어떤 상태인지 알아보는 과정
```

전처리:

```text
발견한 문제를 실제로 수정하는 과정
```

예:

```text
EDA
→ BMI에 결측값이 100개 있음을 발견

전처리
→ 중앙값으로 대체
```

또는:

```text
EDA
→ SBP에서 "??" 발견

전처리
→ 숫자로 변환하고 처리 방법 결정
```

즉 먼저 발견하고 그다음 처리한다.

---

# 50. 처음부터 데이터를 삭제하지 않는다

다음과 같은 데이터를 발견했다고 하자.

```text
결측치
이상치
중복값
```

바로:

```python
drop()
dropna()
```

로 제거하지 않는다.

먼저:

```text
왜 이런 값이 존재하는가?

몇 개인가?

전체 데이터에서 몇 %인가?

Target과 관계가 있는가?

제거하면 데이터가 얼마나 줄어드는가?
```

를 확인해야 한다.

---

# 51. 데이터 점검 기본 코드

새로운 CSV 데이터를 받았을 때 다음 코드를 기본적으로 사용할 수 있다.

```python
import pandas as pd

# =========================
# 1. 데이터 로드
# =========================

df = pd.read_csv("data.csv")


# =========================
# 2. 기본 구조
# =========================

print("Shape:", df.shape)

display(df.head())

print("\nColumns:")
print(df.columns.tolist())


# =========================
# 3. 자료형 / 결측
# =========================

print("\nInfo:")
df.info()

print("\nDtypes:")
print(df.dtypes)


# =========================
# 4. 기본 통계
# =========================

display(
    df.describe(
        include="all"
    )
)


# =========================
# 5. 결측치
# =========================

missing = pd.DataFrame({
    "count": df.isnull().sum(),
    "ratio": df.isnull().mean() * 100
})

missing = missing[
    missing["count"] > 0
].sort_values(
    "ratio",
    ascending=False
)

display(missing)


# =========================
# 6. 중복
# =========================

print(
    "중복 행:",
    df.duplicated().sum()
)


# =========================
# 7. 고유값 개수
# =========================

print("\nUnique values:")

print(
    df.nunique().sort_values()
)
```

이 코드를 실행하면 데이터의 기본적인 상태를 빠르게 파악할 수 있다.

---

# 52. 범주형 데이터 추가 점검

```python
categorical_cols = df.select_dtypes(
    include=["object", "category"]
).columns

for col in categorical_cols:

    print("\n" + "=" * 60)
    print("Column:", col)

    print(
        df[col].value_counts(
            dropna=False
        ).head(20)
    )
```

이를 통해:

```text
오타
이상한 문자열
범주의 종류
결측값
```

등을 확인할 수 있다.

---

# 53. 수치형 데이터 추가 점검

```python
numeric_cols = df.select_dtypes(
    include="number"
).columns

for col in numeric_cols:

    print("\n" + "=" * 60)
    print("Column:", col)

    print(
        "min:",
        df[col].min()
    )

    print(
        "max:",
        df[col].max()
    )

    print(
        "mean:",
        df[col].mean()
    )

    print(
        "median:",
        df[col].median()
    )
```

수치형 데이터의 범위를 빠르게 확인할 수 있다.

---

# 54. 분석 질문을 만드는 것도 EDA이다

EDA는 단순히 명령어를 실행하는 과정이 아니다.

데이터를 보면서 질문을 만드는 과정도 중요하다.

예:

```text
왜 이 Column에 결측치가 많을까?

왜 특정 그룹의 값이 더 높을까?

왜 Target이 불균형할까?

왜 숫자 Column에 문자열이 들어 있을까?

왜 특정 환자의 값이 극단적으로 클까?

이 변수는 예측 시점에 실제로 알 수 있는 정보인가?
```

이런 질문을 하나씩 확인하면서 데이터에 대한 이해를 높인다.

---

# 55. 좋은 데이터 분석의 흐름

좋지 않은 흐름:

```text
CSV 로드
↓
바로 모델 학습
↓
Accuracy 확인
```

더 좋은 흐름:

```text
데이터 로드
↓
구조 확인
↓
데이터 의미 확인
↓
문제 발견
↓
시각화 / 통계 분석
↓
전처리
↓
Feature Engineering
↓
모델 학습
↓
평가
↓
결과 해석
```

머신러닝 모델을 만드는 것만큼 데이터를 이해하는 과정도 중요하다.

---

# 56. 배운 점

새로운 데이터를 받으면 바로 머신러닝 모델부터 만드는 것이 아니라 먼저 데이터의 상태를 확인해야 한다.

기본적으로:

```text
shape
head()
columns
dtypes
info()
describe()
isnull()
duplicated()
unique()
value_counts()
```

등을 이용하여 데이터를 탐색할 수 있다.

특히 단순히 결측치와 이상치를 찾는 것에서 끝나는 것이 아니라:

```text
왜 이런 값이 존재하는가?
실제로 잘못된 값인가?
데이터 명세에서는 어떻게 정의했는가?
예측 시점에 사용할 수 있는 정보인가?
```

를 함께 생각해야 한다.

EDA는 단순히 그래프를 그리는 작업이 아니라 데이터를 이해하고 이후 어떤 전처리와 분석을 해야 할지 결정하기 위한 과정이다.

전체 흐름은:

```text
데이터 확인
    ↓
문제 발견
    ↓
원인 확인
    ↓
처리 방법 결정
    ↓
전처리
    ↓
모델링
```

으로 이해할 수 있다.