# 결측치 처리

## 1. 결측치란?

결측치(Missing Value)는 데이터에서 값이 존재하지 않는 상태를 의미한다.

예:

| Age | BMI | Gender |
|---:|---:|---|
| 30 | 23.5 | Male |
| 40 | NaN | Female |
| NaN | 27.1 | Male |

여기서:

```text
BMI의 NaN
Age의 NaN
```

이 결측치이다.

데이터 분석에서는 결측치를 어떻게 처리할 것인지 결정하는 것이 매우 중요하다.

---

# 2. NaN

Pandas와 NumPy에서는 결측값을 다음과 같은 형태로 자주 볼 수 있다.

```text
NaN
```

NaN은:

```text
Not a Number
```

의 약자이다.

NumPy에서는:

```python
import numpy as np

value = np.nan
```

처럼 표현할 수 있다.

---

# 3. None과 NaN

Python에서는 값이 없음을 나타낼 때:

```python
None
```

도 사용한다.

예:

```python
data = {
    "Age": [30, None, 50]
}
```

Pandas DataFrame으로 변환하면 상황에 따라 결측값으로 처리된다.

따라서 실제 데이터에서는:

```text
NaN
None
```

등의 형태를 볼 수 있다.

---

# 4. 결측치는 왜 발생할까?

결측치는 여러 이유로 발생할 수 있다.

예:

```text
설문에 응답하지 않음

검사를 시행하지 않음

측정 장비 문제

데이터 입력 누락

해당 항목이 적용되지 않음

다른 시스템과 데이터를 합치는 과정에서 값이 없음
```

따라서:

```text
결측치 = 무조건 잘못된 데이터
```

라고 생각하면 안 된다.

---

# 5. 결측치 확인

가장 기본적인 방법:

```python
df.isnull()
```

각 값이 결측인지 확인한다.

결측이면:

```text
True
```

결측이 아니면:

```text
False
```

가 된다.

---

# 6. Column별 결측 개수

```python
df.isnull().sum()
```

예:

```text
Age        10
BMI        50
Gender      0
Outcome     0
```

의미:

```text
Age
→ 결측 10개

BMI
→ 결측 50개
```

이다.

---

# 7. isna()

다음도 사용할 수 있다.

```python
df.isna()
```

Pandas에서:

```text
isnull()
isna()
```

는 결측값을 확인하는 데 같은 목적으로 사용할 수 있다.

예:

```python
df.isna().sum()
```

---

# 8. 결측이 아닌 값 확인

```python
df.notnull()
```

또는:

```python
df.notna()
```

를 사용할 수 있다.

결측이 아닌 경우:

```text
True
```

가 된다.

---

# 9. 결측치가 있는 Column만 확인

```python
missing = df.isnull().sum()

missing = missing[
    missing > 0
]

print(missing)
```

이렇게 하면 결측값이 없는 Column은 제외할 수 있다.

---

# 10. 결측 개수 기준 정렬

```python
missing = df.isnull().sum()

missing = missing[
    missing > 0
]

missing = missing.sort_values(
    ascending=False
)

print(missing)
```

결측치가 많은 Column부터 확인할 수 있다.

---

# 11. 결측률

결측치 개수만으로 판단하면 안 된다.

예를 들어 결측치가 100개 있다고 하자.

전체 데이터가:

```text
200개
```

라면 결측률은 50%이다.

하지만 전체 데이터가:

```text
100,000개
```

라면 결측률은 0.1%이다.

따라서 전체 데이터 대비 비율도 확인해야 한다.

---

# 12. 결측률 계산

```python
missing_ratio = (
    df.isnull().mean() * 100
)

print(missing_ratio)
```

예:

```text
Age         1.0
BMI        20.0
Gender      0.0
```

의미:

```text
Age
→ 1% 결측

BMI
→ 20% 결측
```

이다.

---

# 13. 결측치 표 만들기

```python
missing_table = pd.DataFrame({
    "결측치 수": df.isnull().sum(),
    "결측률(%)": df.isnull().mean() * 100
})

missing_table = missing_table[
    missing_table["결측치 수"] > 0
]

missing_table = missing_table.sort_values(
    "결측률(%)",
    ascending=False
)

display(missing_table)
```

이렇게 하면 어떤 Column의 결측 문제가 큰지 쉽게 확인할 수 있다.

---

# 14. 특정 Column의 결측 행 확인

```python
df[
    df["BMI"].isnull()
]
```

BMI가 결측인 행만 확인할 수 있다.

`loc`를 사용하면:

```python
df.loc[
    df["BMI"].isnull()
]
```

도 가능하다.

---

# 15. 결측이 아닌 행 확인

```python
df.loc[
    df["BMI"].notnull()
]
```

BMI 값이 존재하는 행만 선택한다.

---

# 16. 필요한 Column과 함께 확인

결측 원인을 분석할 때 다른 변수와 같이 확인하면 도움이 된다.

```python
df.loc[
    df["BMI"].isnull(),
    ["Age", "Gender", "BMI"]
]
```

BMI가 없는 사람들의 나이와 성별 등을 함께 확인할 수 있다.

---

# 17. 실제 분석에서 사용했던 형태

다음과 같은 코드를 사용할 수 있다.

```python
train.loc[
    train["난자 해동 경과일"].notnull(),
    [
        "시술 유형",
        "난자 해동 경과일"
    ]
]
```

이 코드는 단순히 결측치를 제거하는 것이 아니다.

```text
난자 해동 경과일 값이 존재하는 경우
어떤 시술 유형에서 주로 값이 존재하는가?
```

를 확인하기 위한 탐색이다.

---

# 18. 구조적 결측

어떤 결측값은 실제 오류가 아니라 특정 조건에서는 애초에 값이 존재할 필요가 없어서 발생할 수 있다.

이를 구조적 결측으로 생각할 수 있다.

예를 들어 특정 시술을 받지 않은 환자에게:

```text
해당 시술 이후 경과일
```

이라는 값이 없다면 자연스러울 수 있다.

따라서 결측률이 높다고 해서 바로 Column을 삭제해서는 안 된다.

---

# 19. 결측치 처리 방법

대표적인 방법은 다음과 같다.

```text
1. 결측 행 삭제

2. 결측 Column 삭제

3. 평균 대체

4. 중앙값 대체

5. 최빈값 대체

6. 그룹별 대체

7. 특정 값으로 대체

8. 결측 여부 자체를 새로운 Feature로 사용

9. 모델이 결측을 처리하도록 활용
```

어떤 방법이 항상 정답인 것은 아니다.

---

# 20. 결측 행 삭제

결측값이 포함된 행을 삭제할 수 있다.

```python
df_clean = df.dropna()
```

결측치가 하나라도 있는 행이 제거된다.

---

# 21. 특정 Column 기준 삭제

BMI가 결측인 행만 제거:

```python
df_clean = df.dropna(
    subset=["BMI"]
)
```

여러 Column을 기준으로 할 수도 있다.

```python
df_clean = df.dropna(
    subset=[
        "Age",
        "BMI"
    ]
)
```

---

# 22. 행 삭제의 문제점

결측 행을 삭제하면 데이터가 줄어든다.

예:

```text
원본 데이터
10,000행

결측 행 삭제 후
6,000행
```

이라면 40%의 데이터를 잃은 것이다.

따라서 삭제 전:

```python
print(df.shape)

df_clean = df.dropna()

print(df_clean.shape)
```

로 얼마나 많은 데이터가 사라지는지 확인해야 한다.

---

# 23. Column 삭제

특정 Column을 삭제할 수도 있다.

```python
df = df.drop(
    columns=["Column_A"]
)
```

하지만:

```text
결측률이 높다
```

는 이유만으로 무조건 삭제하지 않는다.

그 변수가 중요한 정보를 가지고 있을 수도 있기 때문이다.

---

# 24. 평균 대체

수치형 데이터의 결측치를 평균으로 대체할 수 있다.

```python
mean_value = df["Age"].mean()

df["Age"] = df["Age"].fillna(
    mean_value
)
```

한 줄로:

```python
df["Age"] = df["Age"].fillna(
    df["Age"].mean()
)
```

도 가능하다.

---

# 25. 평균 대체의 특징

예:

```text
20
30
40
NaN
```

평균:

```text
30
```

결측치를 30으로 대체한다.

하지만 극단적인 이상치가 있다면 평균이 크게 영향을 받을 수 있다.

---

# 26. 중앙값 대체

```python
median_value = df["BMI"].median()

df["BMI"] = df["BMI"].fillna(
    median_value
)
```

중앙값은 이상치의 영향을 평균보다 덜 받는다.

따라서 데이터가 한쪽으로 치우쳐 있거나 이상치가 많은 경우 중앙값을 고려할 수 있다.

---

# 27. 평균과 중앙값 비교

예:

```text
20
21
22
23
200
```

평균은 200이라는 극단값의 영향을 크게 받는다.

반면 중앙값은:

```text
22
```

이다.

따라서:

```text
평균
→ 이상치에 영향을 받기 쉬움

중앙값
→ 이상치에 상대적으로 강함
```

이라고 이해할 수 있다.

---

# 28. 최빈값 대체

범주형 변수는 최빈값으로 대체할 수 있다.

```python
mode_value = df["Gender"].mode()[0]

df["Gender"] = df["Gender"].fillna(
    mode_value
)
```

`mode()`는 최빈값을 반환한다.

---

# 29. 특정 문자열로 대체

범주형 데이터에서는:

```python
df["Job"] = df["Job"].fillna(
    "Unknown"
)
```

처럼 결측값을 하나의 새로운 범주로 표현할 수도 있다.

예:

```text
사무직
자영업
무직
Unknown
```

이 된다.

---

# 30. 0으로 대체할 때 주의

다음처럼 할 수도 있다.

```python
df["value"] = df["value"].fillna(0)
```

하지만:

```text
NaN
```

과:

```text
실제 값 0
```

은 의미가 다를 수 있다.

예를 들어 검사 수치에서 0이 실제 측정값이라면 결측값을 0으로 바꾸는 순간 둘을 구분할 수 없게 된다.

따라서 0 대체는 변수의 의미를 확인한 후 사용해야 한다.

---

# 31. 그룹별 대체

전체 데이터의 중앙값 하나를 사용하는 것보다 그룹별 특성을 반영할 수도 있다.

예:

```python
df["BMI"] = df.groupby(
    "Gender"
)["BMI"].transform(
    lambda x: x.fillna(
        x.median()
    )
)
```

의미:

```text
Gender별로 데이터 분리
        ↓
각 그룹의 BMI 중앙값 계산
        ↓
해당 그룹의 결측치를
그룹 중앙값으로 대체
```

이다.

---

# 32. 그룹별 대체가 필요한 이유

예를 들어 두 그룹의 평균이 크게 다르다고 하자.

```text
Group A 평균 = 20

Group B 평균 = 40
```

전체 평균이:

```text
30
```

이라고 해서 모든 결측값을 30으로 대체하면 각 그룹의 특성을 반영하지 못할 수 있다.

따라서 의미 있는 그룹이 있다면 그룹별 대체를 검토할 수 있다.

---

# 33. 결측 여부를 Feature로 만들기

결측 자체가 정보를 가지고 있을 수도 있다.

예:

```python
df["BMI_missing"] = (
    df["BMI"].isnull()
).astype(int)
```

결과:

```text
BMI 값 존재
→ 0

BMI 결측
→ 1
```

이렇게 결측 여부를 새로운 Feature로 만들 수 있다.

---

# 34. 왜 결측 여부가 정보가 될 수 있을까?

예를 들어 특정 검사는:

```text
의사가 필요하다고 판단한 환자에게만 시행
```

되었다고 하자.

그러면:

```text
검사 결과가 존재하는가?
```

자체가 환자의 상태와 관련된 정보를 포함할 가능성이 있다.

따라서 단순히 결측값을 채우면서 결측 여부 정보를 없애는 것보다:

```text
원래 결측이었는가?
```

를 별도의 Feature로 남길 수도 있다.

---

# 35. 문자열 오류를 결측값으로 바꾸기

숫자 Column에:

```text
??
ERROR
#BOŢ!
```

같은 문자열이 들어 있을 수 있다.

다음처럼 숫자로 변환할 수 있다.

```python
df["SBP"] = pd.to_numeric(
    df["SBP"],
    errors="coerce"
)
```

숫자로 변환할 수 없는 값은:

```text
NaN
```

으로 변환된다.

---

# 36. 변환 후 반드시 다시 확인

```python
df["SBP"] = pd.to_numeric(
    df["SBP"],
    errors="coerce"
)
```

후:

```python
print(
    df["SBP"].isnull().sum()
)
```

으로 결측치가 얼마나 생겼는지 확인한다.

중요한 것은 `errors="coerce"`가 문제를 해결한 것이 아니라:

```text
잘못된 문자열
→ NaN으로 표현
```

한 것이다.

이후 NaN을 어떻게 처리할지 다시 결정해야 한다.

---

# 37. 원본을 보존하는 습관

전처리 전:

```python
df_raw = df.copy()
```

처럼 원본을 보관해두는 것이 좋다.

예:

```text
df_raw
→ 원본

df
→ 전처리용 데이터
```

그러면 잘못 처리했을 때 다시 원본으로 돌아갈 수 있다.

---

# 38. Target 결측치는 별도로 생각한다

Feature 결측과 Target 결측은 의미가 다르다.

지도학습에서:

```text
X
→ 입력 Feature

y
→ 정답 Target
```

이다.

Train 데이터의 Target이 결측이라면 해당 데이터로는 정답을 이용한 일반적인 지도학습을 수행하기 어렵다.

따라서:

```python
df["Outcome"].isnull().sum()
```

처럼 Target 결측 여부를 반드시 확인한다.

---

# 39. Train과 Test를 같이 볼 때 주의

머신러닝 대회에서 Train에는 Target이 있지만 Test에는 Target이 없다.

따라서 Train과 Test를 합친 뒤:

```python
df.isnull().sum()
```

을 확인하면 Test의 Target 전체가 결측처럼 나타날 수 있다.

이는 실제 데이터 오류가 아니다.

따라서:

```text
Train의 Target 결측

Test에 Target이 없는 것
```

을 구분해야 한다.

---

# 40. 데이터 누수와 결측치 대체

머신러닝에서 매우 중요한 문제이다.

다음처럼 전체 데이터의 평균을 먼저 계산하고:

```python
mean_value = X["Age"].mean()
```

그 후 Train/Validation을 나누면 Validation 데이터의 정보가 전처리에 사용될 수 있다.

더 안전한 흐름은:

```text
Train / Validation 분리
        ↓
Train으로 대체값 계산
        ↓
Train에 적용
        ↓
같은 대체값을 Validation에 적용
```

이다.

---

# 41. 잘못된 예

```python
X["Age"] = X["Age"].fillna(
    X["Age"].mean()
)

X_train, X_valid = train_test_split(
    X,
    y
)
```

전체 데이터에서 평균을 먼저 계산했다.

검증 데이터의 정보가 평균 계산에 포함된다.

---

# 42. 더 적절한 기본 흐름

```python
X_train, X_valid, y_train, y_valid = (
    train_test_split(
        X,
        y,
        test_size=0.2,
        random_state=42
    )
)
```

Train 평균:

```python
age_mean = X_train["Age"].mean()
```

Train 적용:

```python
X_train["Age"] = (
    X_train["Age"].fillna(
        age_mean
    )
)
```

Validation에도 같은 값을 적용:

```python
X_valid["Age"] = (
    X_valid["Age"].fillna(
        age_mean
    )
)
```

핵심은:

```text
Validation에서 대체값을 학습하지 않는다.
```

는 것이다.

---

# 43. Scikit-learn SimpleImputer

Scikit-learn에서는 결측치 대체를 위한 기능도 제공한다.

```python
from sklearn.impute import SimpleImputer
```

중앙값 대체:

```python
imputer = SimpleImputer(
    strategy="median"
)
```

Train으로 학습:

```python
imputer.fit(X_train)
```

Train 변환:

```python
X_train_imputed = imputer.transform(
    X_train
)
```

Validation 변환:

```python
X_valid_imputed = imputer.transform(
    X_valid
)
```

---

# 44. fit과 transform

결측치 처리에서도 머신러닝에서 자주 보는:

```text
fit
transform
```

이 등장한다.

```text
fit
→ Train 데이터에서 필요한 값 학습

transform
→ 학습한 기준을 실제 데이터에 적용
```

예를 들어 중앙값 대체라면:

```text
fit
→ Train 데이터의 중앙값 계산

transform
→ 그 중앙값으로 결측값 대체
```

라고 이해할 수 있다.

---

# 45. fit_transform

Train에서는:

```python
X_train_imputed = (
    imputer.fit_transform(
        X_train
    )
)
```

처럼 `fit`과 `transform`을 한 번에 실행할 수도 있다.

Validation에서는:

```python
X_valid_imputed = (
    imputer.transform(
        X_valid
    )
)
```

만 사용한다.

Validation에 다시 `fit`하면 안 된다.

---

# 46. Pipeline과 결측치 처리

나중에는 다음처럼 Pipeline에 전처리를 넣을 수도 있다.

```python
from sklearn.pipeline import Pipeline
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression

pipeline = Pipeline([
    (
        "imputer",
        SimpleImputer(
            strategy="median"
        )
    ),
    (
        "scaler",
        StandardScaler()
    ),
    (
        "model",
        LogisticRegression()
    )
])
```

흐름:

```text
결측치 처리
    ↓
Scaling
    ↓
모델 학습
```

을 하나로 묶을 수 있다.

Pipeline은 머신러닝 단원에서 더 자세히 다룬다.

---

# 47. 교차검증에서도 주의

Cross Validation에서도 전체 데이터의 중앙값을 먼저 계산해버리면 각 Validation Fold의 정보가 전처리에 섞일 수 있다.

따라서:

```text
각 Fold의 Train 데이터로
전처리 기준 학습
        ↓
해당 Fold의 Validation에 적용
```

하는 것이 중요하다.

Pipeline을 사용하면 이러한 과정을 관리하기 편리하다.

---

# 48. 결측치 처리 방법 비교

예를 들어 다음 방법들을 비교할 수 있다.

```text
방법 A
결측 행 삭제

방법 B
평균 대체

방법 C
중앙값 대체

방법 D
그룹별 중앙값 대체

방법 E
결측 여부 Feature + 중앙값 대체
```

그리고 동일한 Validation 방식으로 모델 성능을 비교할 수 있다.

---

# 49. 어떤 방법이 가장 좋은가?

항상:

```text
중앙값 대체가 최고
```

또는:

```text
결측치는 무조건 삭제
```

같은 정답은 없다.

판단할 때 고려할 것:

```text
결측률

변수의 의미

수치형 / 범주형 여부

데이터 분포

이상치

결측 발생 원인

전체 데이터 크기

모델 종류

검증 성능
```

등이 있다.

---

# 50. 결측치 처리 전후 확인

처리가 끝난 후 다시 확인한다.

```python
print(
    df.isnull().sum()
)
```

전체 결측 개수:

```python
print(
    df.isnull().sum().sum()
)
```

결과가:

```text
0
```

이라면 현재 DataFrame에는 Pandas가 인식하는 결측값이 없다는 의미이다.

다만 이것이 데이터가 완전히 정상이라는 뜻은 아니다.

예:

```text
"?"
"Unknown"
-999
```

처럼 결측을 다른 값으로 표현했을 수도 있기 때문이다.

---

# 51. 결측치 처리 기록

프로젝트에서는 어떤 처리를 했는지 기록하는 것도 중요하다.

예:

```text
Age
→ 결측률 1.2%
→ 중앙값 대체

Gender
→ 결측률 0.5%
→ 최빈값 대체

Job
→ 결측률 12%
→ Unknown 범주 생성

Test_Result
→ 특정 검사를 시행하지 않은 경우 구조적 결측
→ 결측 여부 Feature 추가
```

이렇게 기록하면 나중에:

```text
왜 이렇게 전처리했는가?
```

를 설명할 수 있다.

---

# 52. 결측치 처리 전체 흐름

```text
결측치 발견
      ↓
개수 확인
      ↓
비율 확인
      ↓
변수 의미 확인
      ↓
결측 발생 이유 추정
      ↓
구조적 결측인지 확인
      ↓
처리 방법 결정
      ↓
삭제 / 대체 / Feature화
      ↓
처리 결과 확인
      ↓
모델 검증
```

---

# 53. 초보자가 피해야 할 방식

다음처럼 무조건:

```python
df = df.dropna()
```

부터 실행하지 않는다.

또는 모든 숫자 Column을 무조건:

```python
df = df.fillna(0)
```

으로 처리하지 않는다.

결측값마다 의미가 다를 수 있기 때문이다.

먼저:

```text
무엇이 결측인가?
왜 결측인가?
얼마나 결측인가?
```

를 확인한다.

---

# 54. 실제 프로젝트에서 생각하는 방법

예를 들어 어떤 Column의 결측률이 매우 높다고 하자.

바로 삭제하지 않고:

```text
1. 변수의 의미는 무엇인가?

2. 특정 그룹에서만 측정되는가?

3. 결측 자체가 의미가 있는가?

4. Target과 관련이 있는가?

5. 삭제했을 때 중요한 정보를 잃는가?

6. 대체할 수 있는 합리적인 값이 있는가?

7. 여러 처리 방법의 Validation 성능은 어떤가?
```

를 확인할 수 있다.

---

# 55. 배운 점

결측치는 단순히 값이 비어 있는 문제가 아니다.

먼저:

```python
df.isnull().sum()
```

으로 개수를 확인하고:

```python
df.isnull().mean() * 100
```

으로 결측률을 확인할 수 있다.

대표적인 처리 방법은:

```text
행 삭제
Column 삭제
평균 대체
중앙값 대체
최빈값 대체
그룹별 대체
특정 범주로 대체
결측 여부 Feature 생성
```

등이 있다.

하지만 결측률이 높다는 이유만으로 무조건 삭제하거나 모든 결측값을 0으로 바꾸면 안 된다.

특히 실제 데이터에서는:

```text
측정하지 않아서 없는 값
적용 대상이 아니라서 없는 값
입력 오류 때문에 없는 값
```

의 의미가 서로 다를 수 있다.

머신러닝에서는 결측치 대체 기준을 전체 데이터에서 계산하면 Data Leakage가 발생할 수 있으므로:

```text
Train으로 기준 학습
        ↓
Validation / Test에 같은 기준 적용
```

하는 것이 중요하다.

결국 결측치 처리에서 가장 중요한 것은:

```text
결측값을 어떻게 채울까?
```

만 생각하는 것이 아니라:

```text
왜 이 값이 결측인가?
```

부터 생각하는 것이다.