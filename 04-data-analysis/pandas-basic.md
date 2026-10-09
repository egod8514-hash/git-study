# Pandas 기초

## 1. Pandas란?

Pandas는 Python에서 표 형태의 데이터를 다루기 위한 대표적인 데이터 분석 Library이다.

설치:

```bash
pip install pandas
```

사용:

```python
import pandas as pd
```

일반적으로:

```text
pandas
→ pd
```

라는 별명을 사용한다.

Pandas를 이용하면 CSV, Excel 등의 데이터를 불러와 행과 열 형태로 분석할 수 있다.

---

# 2. DataFrame

Pandas에서 가장 중요한 자료구조가 DataFrame이다.

예를 들어 다음과 같은 표가 있다고 하자.

| 이름 | 나이 | 키 |
|---|---:|---:|
| Kim | 30 | 175 |
| Lee | 40 | 180 |
| Park | 25 | 168 |

Pandas에서는 이런 데이터를 DataFrame으로 표현할 수 있다.

```python
import pandas as pd

data = {
    "이름": ["Kim", "Lee", "Park"],
    "나이": [30, 40, 25],
    "키": [175, 180, 168]
}

df = pd.DataFrame(data)

print(df)
```

결과:

```text
     이름  나이    키
0   Kim  30  175
1   Lee  40  180
2  Park  25  168
```

---

# 3. DataFrame 구조

DataFrame은 기본적으로:

```text
행(Row)
열(Column)
```

로 구성된다.

예:

```text
        이름   나이   키
0       Kim    30   175
1       Lee    40   180
2       Park   25   168
```

여기서:

```text
이름 / 나이 / 키
→ Column

0 / 1 / 2
→ Index

각 사람의 데이터
→ Row
```

이다.

---

# 4. Series

DataFrame에서 하나의 Column을 선택하면 Series라는 자료구조가 나올 수 있다.

```python
df["나이"]
```

결과:

```text
0    30
1    40
2    25
Name: 나이
```

자료형 확인:

```python
print(type(df["나이"]))
```

예:

```text
<class 'pandas.core.series.Series'>
```

따라서 간단하게:

```text
DataFrame
→ 여러 행과 열을 가진 표

Series
→ 하나의 열과 같은 1차원 데이터
```

라고 이해할 수 있다.

---

# 5. CSV 파일

데이터 분석에서는 CSV 파일을 매우 자주 사용한다.

CSV는:

```text
Comma-Separated Values
```

의 약자이다.

예:

```text
name,age,height
Kim,30,175
Lee,40,180
Park,25,168
```

표 형태의 데이터를 텍스트 파일로 저장할 수 있다.

---

# 6. CSV 파일 불러오기

Pandas에서는 `read_csv()`를 사용한다.

```python
import pandas as pd

df = pd.read_csv("data.csv")
```

여기서:

```text
pd
→ pandas

read_csv()
→ CSV 파일을 읽는 기능

"data.csv"
→ 파일 경로

df
→ 불러온 DataFrame을 저장할 변수
```

이다.

---

# 7. train.csv 불러오기

머신러닝 프로젝트에서 자주 사용했던 형태이다.

```python
train = pd.read_csv("train.csv")
test = pd.read_csv("test.csv")
```

이제:

```text
train
→ 학습 데이터

test
→ 테스트 데이터
```

가 된다.

---

# 8. Colab 파일 경로

Google Colab에 파일을 직접 업로드했다면 보통:

```python
train = pd.read_csv("/content/train.csv")
```

처럼 사용할 수 있다.

여기서:

```text
/content/
```

는 Colab Runtime에서 자주 사용하는 작업 경로이다.

---

# 9. FileNotFoundError

다음 코드:

```python
train = pd.read_csv("/content/train.csv")
```

를 실행했는데 해당 위치에 파일이 없다면:

```text
FileNotFoundError
```

가 발생할 수 있다.

즉:

```text
Pandas 문제
```

가 아니라:

```text
파일이 해당 경로에 없음
```

일 가능성이 있다.

따라서 파일을 읽기 전에 경로를 확인하는 것이 중요하다.

---

# 10. 현재 폴더의 파일 확인

Colab에서는:

```python
import os

print(os.listdir("/content"))
```

처럼 파일 목록을 확인할 수 있다.

파일이:

```text
train.csv
```

인지:

```text
Train.csv
```

인지도 확인해야 한다.

파일 이름과 경로가 정확해야 한다.

---

# 11. head()

데이터를 불러온 후 처음 몇 행을 확인한다.

```python
train.head()
```

기본적으로 앞부분 5개 행을 보여준다.

10개를 보고 싶다면:

```python
train.head(10)
```

처럼 사용할 수 있다.

---

# 12. tail()

마지막 데이터를 확인한다.

```python
train.tail()
```

마지막 5개 행을 보여준다.

```python
train.tail(10)
```

처럼 개수를 지정할 수도 있다.

---

# 13. shape

데이터의 행과 열 개수를 확인한다.

```python
train.shape
```

예:

```text
(1000, 20)
```

의미:

```text
1000행
20열
```

이다.

즉:

```text
1000개의 데이터
20개의 변수
```

가 있다는 것을 알 수 있다.

---

# 14. shape는 함수가 아니다

다음처럼 사용한다.

```python
train.shape
```

다음처럼 사용하지 않는다.

```python
train.shape()
```

`shape`는 메서드가 아니라 속성(Attribute)이기 때문이다.

반면:

```python
train.head()
```

는 메서드이므로 `()`를 사용한다.

---

# 15. columns

Column 이름을 확인한다.

```python
train.columns
```

예:

```text
Index(['Age', 'BMI', 'Glucose', 'Outcome'], dtype='object')
```

어떤 변수가 존재하는지 확인할 수 있다.

---

# 16. dtypes

각 Column의 자료형을 확인한다.

```python
train.dtypes
```

예:

```text
Age          int64
BMI        float64
Gender      object
Outcome      int64
```

대표적으로:

```text
int64
→ 정수

float64
→ 실수

object
→ 문자열 등이 포함될 수 있음

bool
→ True / False
```

등을 볼 수 있다.

---

# 17. info()

데이터의 전체적인 구조를 확인할 때 매우 유용하다.

```python
train.info()
```

확인할 수 있는 정보:

```text
행 개수
Column 이름
Non-Null 개수
자료형
메모리 사용량
```

데이터를 처음 받았을 때 자주 사용하는 기능이다.

---

# 18. describe()

수치형 변수의 기본 통계를 확인한다.

```python
train.describe()
```

대표적으로:

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

가 출력된다.

---

# 19. describe() 읽기

예를 들어:

```text
count → 데이터 개수

mean → 평균

std → 표준편차

min → 최솟값

25% → 1사분위수

50% → 중앙값

75% → 3사분위수

max → 최댓값
```

이다.

이 정보는 이상치나 데이터 분포를 처음 확인할 때 도움이 된다.

---

# 20. 특정 Column 선택

```python
train["Age"]
```

`Age` Column 하나를 선택한다.

이 결과는 일반적으로 Series이다.

---

# 21. 여러 Column 선택

여러 열을 선택하려면 List 형태로 전달한다.

```python
train[
    ["Age", "BMI"]
]
```

중요한 차이:

```python
train["Age"]
```

→ 하나의 Column

```python
train[["Age", "BMI"]]
```

→ 여러 Column

이다.

---

# 22. 대괄호가 두 번 보이는 이유

다음 코드:

```python
train[["Age", "BMI"]]
```

를 보면 `[[ ]]`가 보여 처음에는 복잡할 수 있다.

나누어 보면:

```python
["Age", "BMI"]
```

는 Python List이다.

그리고:

```python
train[리스트]
```

형태로 Pandas에게 여러 Column을 선택하라고 전달하는 것이다.

즉:

```text
바깥 []
→ DataFrame에서 선택

안쪽 []
→ 선택할 Column 이름의 List
```

이다.

---

# 23. 새로운 Column 만들기

새로운 Column을 만들 수 있다.

```python
train["Age_plus_10"] = train["Age"] + 10
```

이제 DataFrame에:

```text
Age_plus_10
```

이라는 새로운 Column이 추가된다.

이 방식은 이후 Feature Engineering에서 매우 많이 사용한다.

---

# 24. Column 수정

기존 Column도 변경할 수 있다.

```python
train["Age"] = train["Age"] + 1
```

기존 `Age` 값에 1을 더한 결과로 Column이 변경된다.

실제 데이터를 수정할 때는 원본 보존 여부를 생각해야 한다.

---

# 25. DataFrame 복사

원본 데이터를 보존하면서 작업하고 싶다면:

```python
df = train.copy()
```

처럼 복사본을 만들 수 있다.

그러면:

```text
train
→ 원본

df
→ 작업용 복사본
```

처럼 관리할 수 있다.

---

# 26. loc

`loc`는 Label이나 조건을 이용하여 데이터를 선택할 때 많이 사용한다.

예:

```python
train.loc[0]
```

Index Label이 0인 행을 선택한다.

---

# 27. loc로 행과 열 선택

기본 형태:

```python
df.loc[행, 열]
```

예:

```python
train.loc[
    0,
    "Age"
]
```

0번 Index의 `Age` 값을 선택한다.

---

# 28. loc로 여러 Column 선택

```python
train.loc[
    :,
    ["Age", "BMI"]
]
```

여기서:

```text
:
→ 모든 행

["Age", "BMI"]
→ Age와 BMI Column
```

이다.

즉:

```text
모든 행에서 Age와 BMI만 선택
```

한다.

---

# 29. 조건과 loc

실제 데이터 분석에서 매우 자주 사용하는 형태이다.

```python
cond = train["Age"] >= 30
```

그리고:

```python
train.loc[cond]
```

30세 이상 데이터만 선택한다.

---

# 30. 조건 + 필요한 Column

```python
train.loc[
    train["Age"] >= 30,
    ["Age", "BMI"]
]
```

의미:

```text
Age가 30 이상인 행에서
Age와 BMI Column만 선택
```

이다.

---

# 31. 실제로 사용했던 형태 이해

다음과 같은 코드를 사용할 수 있다.

```python
train.loc[
    train["난자 해동 경과일"].notnull(),
    ["시술 유형", "난자 해동 경과일"]
]
```

한 번에 보면 복잡하지만 나누어보면 된다.

먼저:

```python
train["난자 해동 경과일"]
```

→ 해당 Column 선택

다음:

```python
.notnull()
```

→ 결측값이 아닌 데이터인지 확인

그리고:

```python
train.loc[
    조건,
    ["시술 유형", "난자 해동 경과일"]
]
```

→ 조건을 만족하는 행에서 필요한 두 Column만 선택

이다.

---

# 32. iloc

`iloc`는 데이터의 위치 번호를 기준으로 선택한다.

예:

```python
train.iloc[0]
```

첫 번째 행을 선택한다.

---

# 33. iloc 행과 열

```python
train.iloc[0, 1]
```

의미:

```text
첫 번째 행
두 번째 열
```

이다.

Python Index는 0부터 시작한다.

---

# 34. iloc 슬라이싱

```python
train.iloc[0:5]
```

처음 5개 행을 선택한다.

또는:

```python
train.iloc[
    0:5,
    0:3
]
```

이면:

```text
처음 5개 행
처음 3개 열
```

을 선택한다.

---

# 35. loc와 iloc 차이

간단하게:

```text
loc
→ 이름(Label), 조건 중심

iloc
→ 위치(Integer Location) 중심
```

이라고 이해하면 된다.

예:

```python
train.loc[:, ["Age", "BMI"]]
```

Column 이름 사용.

```python
train.iloc[:, 0:2]
```

Column 위치 사용.

---

# 36. 조건 필터링

특정 조건을 만족하는 행만 가져올 수 있다.

```python
train[
    train["Age"] >= 30
]
```

구조를 나누면:

```python
train["Age"]
```

→ Age Column

```python
train["Age"] >= 30
```

→ 각 행이 30 이상인지 검사

결과 예:

```text
False
True
True
False
...
```

그리고:

```python
train[조건]
```

으로 조건이 True인 행만 선택한다.

---

# 37. 여러 조건 - and 대신 &

Pandas Series 조건에서는 다음처럼 `&`를 많이 사용한다.

```python
train[
    (train["Age"] >= 30)
    &
    (train["BMI"] >= 25)
]
```

의미:

```text
Age가 30 이상이고
BMI가 25 이상
```

이다.

각 조건을 괄호로 묶는 것이 중요하다.

---

# 38. OR 조건

둘 중 하나를 만족하는 조건:

```python
train[
    (train["Age"] >= 60)
    |
    (train["BMI"] >= 30)
]
```

`|`는 Pandas Series 조건에서 OR 역할을 한다.

---

# 39. NOT 조건

조건을 반대로 만들 때:

```python
~condition
```

을 사용할 수 있다.

예:

```python
cond = train["Age"] >= 30

train.loc[~cond]
```

30세 이상이 아닌 데이터를 선택한다.

---

# 40. 정렬

특정 Column 기준으로 정렬할 수 있다.

```python
train.sort_values(
    "Age"
)
```

기본적으로 오름차순이다.

내림차순:

```python
train.sort_values(
    "Age",
    ascending=False
)
```

---

# 41. 결측치 개수와 정렬

이후 결측치 분석에서 자주 사용하는 코드이다.

```python
missing = train.isnull().sum()

missing = missing[
    missing > 0
]

missing.sort_values(
    ascending=False
)
```

흐름:

```text
isnull()
→ 결측 여부 확인

sum()
→ Column별 결측 개수

missing > 0
→ 결측치가 있는 Column만 선택

sort_values()
→ 결측 개수 기준 정렬
```

이다.

---

# 42. value_counts()

범주별 데이터 개수를 확인한다.

```python
train["Gender"].value_counts()
```

예:

```text
Male      600
Female    400
```

머신러닝에서는 Target의 클래스 비율을 확인할 때도 많이 사용한다.

```python
train["Outcome"].value_counts()
```

---

# 43. 비율 확인

```python
train["Outcome"].value_counts(
    normalize=True
)
```

처럼 `normalize=True`를 사용하면 비율을 확인할 수 있다.

예:

```text
0    0.75
1    0.25
```

즉:

```text
0 → 75%
1 → 25%
```

이다.

클래스 불균형을 확인할 때 유용하다.

---

# 44. unique()

Column에 어떤 값들이 있는지 확인한다.

```python
train["Gender"].unique()
```

예:

```text
['Male', 'Female']
```

범주형 데이터의 종류를 확인할 때 유용하다.

---

# 45. nunique()

고유한 값의 개수를 확인한다.

```python
train["Gender"].nunique()
```

결과:

```text
2
```

---

# 46. 데이터 삭제

Column을 삭제할 수 있다.

```python
df = train.drop(
    columns=["ID"]
)
```

여러 Column:

```python
df = train.drop(
    columns=["ID", "Name"]
)
```

---

# 47. 특정 행 삭제

Index를 기준으로 행을 삭제할 수도 있다.

```python
df = train.drop(
    index=[0, 1]
)
```

0번과 1번 Index 행을 제거한다.

---

# 48. drop의 중요한 특징

다음 코드:

```python
train.drop(
    columns=["ID"]
)
```

만 실행했다고 해서 원본 `train`이 반드시 변경되는 것은 아니다.

결과를 저장할 수 있다.

```python
train = train.drop(
    columns=["ID"]
)
```

또는 작업용 데이터에 저장한다.

```python
df = train.drop(
    columns=["ID"]
)
```

---

# 49. rename()

Column 이름을 변경할 수도 있다.

```python
df = df.rename(
    columns={
        "Age": "age",
        "Gender": "gender"
    }
)
```

Dictionary의:

```text
기존 이름 : 새로운 이름
```

형태를 이용한다.

---

# 50. 데이터 처음 받았을 때 기본 확인

CSV 파일을 처음 받았다면 다음 정도부터 확인할 수 있다.

```python
import pandas as pd

df = pd.read_csv("data.csv")

print(df.shape)

display(df.head())

print(df.columns)

print(df.dtypes)

df.info()

display(df.describe())
```

이렇게 하면 데이터의 전체적인 구조를 빠르게 파악할 수 있다.

---

# 51. 왜 바로 머신러닝을 하지 않을까?

데이터를 불러왔다고 바로:

```python
model.fit(X, y)
```

을 하는 것이 아니라 먼저 데이터를 확인해야 한다.

왜냐하면 데이터에는:

```text
결측치
이상치
문자열
잘못된 값
중복
클래스 불균형
불필요한 Column
```

등이 존재할 수 있기 때문이다.

따라서:

```text
데이터 로드
    ↓
데이터 확인
    ↓
문제 발견
    ↓
전처리
    ↓
머신러닝
```

순서가 중요하다.

---

# 52. 실제 데이터 분석 흐름

예를 들어:

```python
train = pd.read_csv("train.csv")
```

다음:

```python
train.shape
```

데이터 크기 확인.

다음:

```python
train.head()
```

실제 데이터 확인.

다음:

```python
train.info()
```

자료형과 결측치 확인.

다음:

```python
train.describe()
```

수치형 데이터 분포 확인.

다음:

```python
train["Outcome"].value_counts()
```

Target 분포 확인.

이렇게 조금씩 데이터의 상태를 파악한다.

---

# 53. Pandas와 머신러닝 연결

Pandas에서 데이터를 정리한 후:

```python
X = train.drop(
    columns=["Outcome"]
)

y = train["Outcome"]
```

처럼 입력 데이터와 정답을 분리할 수 있다.

여기서:

```text
X
→ Feature

y
→ Target
```

이다.

그리고 머신러닝에서:

```python
model.fit(X, y)
```

로 사용한다.

즉 Pandas는 머신러닝에 넣기 전 데이터를 준비하는 과정에서 매우 중요한 역할을 한다.

---

# 54. 코드가 길어 보일 때 읽는 방법

다음 코드를 보자.

```python
train.loc[
    train["난자 해동 경과일"].notnull(),
    ["시술 유형", "난자 해동 경과일"]
].head(20)
```

한 번에 이해하지 않고 안쪽부터 본다.

```text
1.

train["난자 해동 경과일"]

→ Column 선택
```

```text
2.

.notnull()

→ 결측값이 아닌지 확인
```

```text
3.

["시술 유형", "난자 해동 경과일"]

→ 보고 싶은 Column 목록
```

```text
4.

train.loc[조건, 열]

→ 필요한 행과 열 선택
```

```text
5.

.head(20)

→ 결과 중 앞 20개 확인
```

복잡한 Pandas 코드도 작은 기능들의 조합이다.

---

# 55. 자주 사용하는 Pandas 기능

```text
pd.read_csv()
→ CSV 불러오기

head()
→ 앞부분 확인

tail()
→ 뒷부분 확인

shape
→ 행/열 개수

columns
→ Column 이름

dtypes
→ 자료형

info()
→ 데이터 구조

describe()
→ 기본 통계

loc[]
→ Label/조건 기반 선택

iloc[]
→ 위치 기반 선택

value_counts()
→ 값별 개수

unique()
→ 고유값

nunique()
→ 고유값 개수

sort_values()
→ 정렬

drop()
→ 삭제

rename()
→ 이름 변경

copy()
→ 복사
```

---

# 56. NumPy와 Pandas 연결

NumPy:

```python
data = np.array([
    [10, 20],
    [30, 40]
])
```

Pandas:

```python
df = pd.DataFrame(
    data,
    columns=["A", "B"]
)
```

NumPy는 숫자 배열 처리에 강하고 Pandas는 행과 Column 이름을 가진 표 형태의 데이터를 분석하기 편리하게 만들어준다.

둘은 데이터 분석에서 함께 사용되는 경우가 많다.

---

# 57. 배운 점

Pandas는 표 형태의 데이터를 다루기 위한 Python의 핵심 데이터 분석 Library이다.

가장 중요한 자료구조는:

```text
DataFrame
Series
```

이다.

CSV 데이터를 불러온 뒤 바로 머신러닝을 시작하는 것이 아니라:

```text
shape
head()
columns
dtypes
info()
describe()
```

등을 이용하여 데이터의 구조를 먼저 확인해야 한다.

또한:

```python
df["column"]
```

으로 Column을 선택하고:

```python
df.loc[조건]
```

으로 원하는 데이터만 선택할 수 있다.

`loc`와 `iloc`의 기본적인 차이는:

```text
loc
→ 이름과 조건 중심

iloc
→ 위치 번호 중심
```

이다.

우리가 실제 데이터 분석에서 사용했던 긴 Pandas 코드도 결국:

```text
Column 선택
+
조건 생성
+
행 선택
+
결과 확인
```

같은 기본 기능을 조합한 것이다.

Pandas를 이해하는 것은 이후 학습할:

```text
결측치 처리
이상치 처리
데이터 전처리
시각화
통계 분석
Feature Engineering
머신러닝
```

의 기반이 된다.