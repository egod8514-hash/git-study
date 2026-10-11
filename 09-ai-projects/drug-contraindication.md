# Drug Contraindication Data Analysis

## 1. 프로젝트 개요

이 프로젝트는 의약품 금기 데이터를 분석하여 다음과 같은 질문에 답하는 것을 목적으로 한다.

```text
금기등급은 어떻게 분포하는가?

투여경로에 따라 금기등급 분포가 다른가?

어떤 성분의 제품수가 가장 많은가?

그 성분은 어떤 병용성분과의 금기 조합에서
제품수가 가장 많은가?
```

전체적인 흐름은 다음과 같다.

```text
의약품 금기 Data
↓
Data 구조 확인
↓
Cleaning
↓
Category Distribution
↓
Group Aggregation
↓
Cross Tabulation
↓
Chi-square Test
↓
성분별 제품수 집계
↓
병용금기 조합 분석
↓
결과 해석
```

---

# 2. 이 프로젝트의 특징

앞에서 진행했던:

```text
임신 성공 여부 예측

수술실패여부 예측
```

과는 목적이 다르다.

이번 프로젝트의 중심은:

```text
Prediction
```

보다는:

```text
Data Analysis

Aggregation

Statistical Test
```

이다.

---

# 3. 중요한 구분

모든 Data Project가 Machine Learning을 필요로 하는 것은 아니다.

질문이:

```text
어떤 Category가 가장 많은가?

두 Category 사이에 관계가 있는가?

어떤 조합의 제품수가 가장 많은가?
```

라면 Pandas와 통계 분석만으로도 답할 수 있다.

---

# 4. 의약품 금기

특정 상황에서 의약품 사용을 피해야 하거나 제한해야 하는 정보를 금기 정보로 다룰 수 있다.

Dataset의 정확한 의미와 등급 정의는 반드시 해당 Data 명세를 기준으로 확인한다.

---

# 5. 임부금기

임신 중 특정 의약품 성분 사용에 대한 금기 정보를 분석할 수 있다.

예:

```text
성분

금기등급

투여경로

제품수
```

등의 정보를 생각할 수 있다.

---

# 6. 병용금기

두 의약품 성분을 함께 사용하는 것과 관련된 금기 정보를 다룰 수 있다.

예:

```text
성분 A
+
성분 B
↓
병용금기
```

형태이다.

---

# 7. 첫 단계

Data를 읽는다.

```python
import pandas as pd
import numpy as np


df = pd.read_csv(
    "data.csv"
)
```

실제 파일명과 경로는 Dataset에 맞게 수정한다.

---

# 8. Data Shape

```python
print(
    df.shape
)
```

---

# 9. Column 확인

```python
print(
    df.columns.tolist()
)
```

---

# 10. 첫 행 확인

```python
display(
    df.head()
)
```

---

# 11. Data Type

```python
df.info()
```

---

# 12. Missing Value

```python
missing = (
    df.isnull().sum()
)


missing = missing[
    missing > 0
]


display(
    missing.sort_values(
        ascending=False
    )
)
```

---

# 13. 왜 기본 확인부터 할까?

의약품 Data에서는 같은 의미처럼 보이는 값도 실제로는:

```text
공백

다른 표기

Missing

문자 Type

숫자 Type
```

등으로 다르게 저장되어 있을 수 있기 때문이다.

---

# 14. 문자열 정리

예:

```python
df[
    "투여경로"
] = (
    df[
        "투여경로"
    ]
    .astype(
        "string"
    )
    .str.strip()
)
```

---

# 15. Category 값 확인

```python
print(
    df[
        "투여경로"
    ].value_counts(
        dropna=False
    )
)
```

---

# 16. Unique 확인

```python
print(
    df[
        "투여경로"
    ].unique()
)
```

---

# 17. 왜 필요한가?

예를 들어:

```text
내복

내복 

 내복
```

은 사람이 보기에는 같지만 문자열로는 다르게 처리될 수 있다.

---

# 18. 금기등급 분석

먼저 금기등급별 Record 수를 확인한다.

```python
grade_count = (
    df[
        "금기등급"
    ].value_counts()
)


print(
    grade_count
)
```

---

# 19. 비율

```python
grade_ratio = (
    df[
        "금기등급"
    ]
    .value_counts(
        normalize=True
    )
    *
    100
)


print(
    grade_ratio
)
```

---

# 20. 당시 임부금기 원자료

분석에서 확인한 금기등급별 Record 수는:

```text
1등급
3,132

2등급
15,480
```

이었다.

---

# 21. 전체

```text
3,132 + 15,480
=
18,612
```

Record이다.

---

# 22. 1등급 비율

대략:

```text
3,132
/
18,612
≈
16.8%
```

정도이다.

---

# 23. 2등급 비율

대략:

```text
83.2%
```

이다.

---

# 24. 여기서 알 수 있는 것

전체 Dataset에서는:

```text
2등급 Record가
1등급보다 훨씬 많다.
```

는 것을 알 수 있다.

---

# 25. 하지만

이것만으로:

```text
모든 투여경로에서도
같은 비율일 것이다.
```

라고 판단할 수 없다.

---

# 26. 다음 질문

```text
투여경로에 따라
금기등급의 분포가 다른가?
```

를 확인한다.

---

# 27. Cross Tabulation

두 Category의 관계를 표로 확인할 수 있다.

```python
cross_table = pd.crosstab(
    df[
        "투여경로"
    ],
    df[
        "금기등급"
    ]
)


display(
    cross_table
)
```

---

# 28. Crosstab이란?

예:

```text
              1등급    2등급

내복            ?        ?

외용            ?        ?

주사            ?        ?
```

처럼 두 범주형 변수의 조합별 빈도를 보여준다.

---

# 29. Row Ratio

각 투여경로 안에서 금기등급 비율을 보고 싶다면:

```python
cross_ratio = pd.crosstab(
    df[
        "투여경로"
    ],
    df[
        "금기등급"
    ],
    normalize="index"
) * 100


display(
    cross_ratio
)
```

---

# 30. normalize="index"

각 Row의 합을:

```text
100%
```

로 만든다.

따라서 질문은:

```text
이 투여경로 안에서
각 금기등급이 몇 %인가?
```

가 된다.

---

# 31. 당시 결과

1등급 비율은 투여경로별로 다음과 같은 차이를 보였다.

```text
내복
약 19.08%

외용
약 3.15%

주사
약 5.94%
```

---

# 32. 단순 비율 비교

이 결과만 보면:

```text
내복에서 1등급 비율이
상대적으로 높다.
```

는 Pattern을 확인할 수 있다.

---

# 33. 하지만 다음 질문

```text
이 차이가 단순한 우연인가?

아니면 통계적으로 유의한 관계가 있는가?
```

를 확인해야 한다.

---

# 34. Chi-square Test

두 범주형 변수의 관계를 검정할 때:

```text
Chi-square Test
```

를 사용할 수 있다.

한국어로:

```text
카이제곱 검정
```

이라고 한다.

---

# 35. 이번 문제

두 변수:

```text
투여경로

금기등급
```

은 모두 범주형 변수이다.

따라서 카이제곱 독립성 검정을 사용할 수 있다.

---

# 36. 가설

귀무가설:

```text
투여경로와 금기등급은 독립이다.
```

즉:

```text
두 변수 사이에 통계적인 관련성을
확인하기 어렵다.
```

이다.

---

# 37. 대립가설

```text
투여경로와 금기등급은 독립이 아니다.
```

즉:

```text
두 변수 사이에 통계적인 관련성이 있다.
```

이다.

---

# 38. Python

```python
from scipy.stats import (
    chi2_contingency
)


chi2, p, dof, expected = (
    chi2_contingency(
        cross_table
    )
)
```

---

# 39. 결과 출력

```python
print(
    "Chi-square:",
    chi2
)


print(
    "p-value:",
    p
)


print(
    "Degrees of freedom:",
    dof
)
```

---

# 40. 당시 결과

```text
Chi-square
≈ 353.766

p-value
≈ 1.52 × 10^-77
```

이었다.

---

# 41. p-value

매우 작은 값이다.

```text
0.05
```

보다 훨씬 작다.

---

# 42. 통계적 해석

따라서 일반적인 유의수준 0.05를 기준으로:

```text
귀무가설을 기각한다.
```

고 해석할 수 있다.

---

# 43. 결과

```text
투여경로와 금기등급 사이에
통계적으로 유의한 관련성이 관찰되었다.
```

라고 표현할 수 있다.

---

# 44. 중요한 주의

이 결과가:

```text
투여경로가 금기등급의 원인이다.
```

라는 의미는 아니다.

---

# 45. Association과 Causality

```text
Statistical Association
≠
Causality
```

이다.

---

# 46. Sample Size

Sample이 많으면 작은 차이도 매우 작은 p-value를 만들 수 있다.

따라서:

```text
p-value
```

만 보지 않는다.

---

# 47. 실제 비율도 같이 본다

이번 분석에서는:

```text
내복
19.08%

외용
3.15%

주사
5.94%
```

처럼 실제 비율 차이도 같이 제시하는 것이 좋다.

---

# 48. Expected Frequency

카이제곱 검정에서는 Expected Frequency도 확인할 수 있다.

```python
expected_df = pd.DataFrame(
    expected,
    index=cross_table.index,
    columns=cross_table.columns
)


display(
    expected_df
)
```

---

# 49. 왜 확인할까?

카이제곱 검정의 적용 조건을 점검하기 위해서이다.

Expected Frequency가 지나치게 작은 Cell이 많은 경우 검정 방법을 다시 검토해야 할 수 있다.

---

# 50. Visualization

투여경로별 금기등급 비율을 그래프로 나타낼 수 있다.

```python
import matplotlib.pyplot as plt


cross_ratio.plot(
    kind="bar"
)


plt.ylabel(
    "Ratio (%)"
)


plt.title(
    "Contraindication Grade by Administration Route"
)


plt.show()
```

---

# 51. 그래프의 역할

통계 검정 결과만 보여주는 것보다:

```text
어느 투여경로에서
어떤 차이가 나타나는가?
```

를 시각적으로 이해하기 쉽다.

---

# 52. 통계 분석 흐름

```text
Category Count
↓
Crosstab
↓
Ratio
↓
Visualization
↓
Chi-square Test
↓
Interpretation
```

이다.

---

# 53. 다음 분석

이번에는:

```text
제품수 합계가 가장 많은 성분은 무엇인가?
```

를 찾는다.

---

# 54. 필요한 Column

예를 들어 Dataset에:

```text
성분명

제품수
```

가 있다고 생각한다.

실제 Column 이름은 Dataset에서 확인한다.

---

# 55. 제품수 Type

먼저 제품수가 Numeric인지 확인한다.

```python
print(
    df[
        "제품수"
    ].dtype
)
```

---

# 56. 문자열이라면

```python
df[
    "제품수"
] = pd.to_numeric(
    df[
        "제품수"
    ],
    errors="coerce"
)
```

처럼 변환할 수 있다.

---

# 57. NaN 확인

```python
print(
    df[
        "제품수"
    ].isnull().sum()
)
```

---

# 58. 성분별 제품수 합계

```python
ingredient_product_sum = (
    df.groupby(
        "성분명"
    )[
        "제품수"
    ]
    .sum()
    .sort_values(
        ascending=False
    )
)
```

---

# 59. 결과 확인

```python
display(
    ingredient_product_sum.head(
        10
    )
)
```

---

# 60. 가장 많은 성분

```python
top_ingredient = (
    ingredient_product_sum.index[
        0
    ]
)


top_product_count = (
    ingredient_product_sum.iloc[
        0
    ]
)
```

---

# 61. 출력

```python
print(
    "제품수 합계가 가장 많은 성분:",
    top_ingredient
)


print(
    "제품수 합계:",
    top_product_count
)
```

---

# 62. 여기서 중요한 것

문제는:

```text
Row가 가장 많은 성분
```

을 묻는 것이 아니다.

---

# 63. 문제는

```text
제품수의 합계가
가장 많은 성분
```

을 묻는다.

---

# 64. 따라서

이 코드는 문제에 맞지 않을 수 있다.

```python
df[
    "성분명"
].value_counts()
```

---

# 65. 왜?

`value_counts()`는:

```text
Record 개수
```

를 계산한다.

---

# 66. 우리가 원하는 것

```text
제품수 Column의 합
```

이다.

따라서:

```python
groupby(
    "성분명"
)[
    "제품수"
].sum()
```

을 사용한다.

---

# 67. 이것은 매우 중요한 차이

예:

```text
성분 A

Row 1 제품수 = 100
Row 2 제품수 = 200
```

이면:

```text
Record 수 = 2

제품수 합계 = 300
```

이다.

---

# 68. 문제 읽기

```text
가장 많이 등장한 성분
```

과:

```text
제품수 합계가 가장 많은 성분
```

은 완전히 다른 질문이다.

---

# 69. 다음 문제

가장 많은 제품수를 가진 성분을 찾았다면:

```text
그 성분은 어떤 병용성분과의
병용금기에서 제품수가 가장 많은가?
```

를 확인한다.

---

# 70. 먼저 Top Ingredient Data만 추출

```python
top_df = df.loc[
    df[
        "성분명"
    ] == top_ingredient
].copy()
```

---

# 71. 확인

```python
print(
    top_df.shape
)


display(
    top_df.head()
)
```

---

# 72. 병용성분별 제품수 합계

Dataset에:

```text
병용성분명
```

Column이 있다고 가정한다.

```python
combination_sum = (
    top_df.groupby(
        "병용성분명"
    )[
        "제품수"
    ]
    .sum()
    .sort_values(
        ascending=False
    )
)
```

---

# 73. 결과

```python
display(
    combination_sum.head(
        10
    )
)
```

---

# 74. 가장 큰 조합

```python
top_combination = (
    combination_sum.index[
        0
    ]
)


top_combination_count = (
    combination_sum.iloc[
        0
    ]
)
```

---

# 75. 출력

```python
print(
    "기준 성분:",
    top_ingredient
)


print(
    "제품수가 가장 많은 병용성분:",
    top_combination
)


print(
    "제품수 합계:",
    top_combination_count
)
```

---

# 76. 전체 문제의 논리

```text
Step 1
성분별 제품수 합계 계산

↓

Step 2
가장 큰 성분 선택

↓

Step 3
해당 성분의 Data만 Filtering

↓

Step 4
병용성분별 제품수 합계 계산

↓

Step 5
가장 큰 병용성분 선택
```

이다.

---

# 77. 코드로 연결

```python
ingredient_sum = (
    df.groupby(
        "성분명"
    )[
        "제품수"
    ]
    .sum()
)


top_ingredient = (
    ingredient_sum.idxmax()
)


top_df = df.loc[
    df[
        "성분명"
    ] == top_ingredient
]


combination_sum = (
    top_df.groupby(
        "병용성분명"
    )[
        "제품수"
    ]
    .sum()
)


top_combination = (
    combination_sum.idxmax()
)
```

---

# 78. idxmax()

```python
series.idxmax()
```

는 Series에서:

```text
가장 큰 Value를 가진 Index
```

를 반환한다.

---

# 79. max()

반면:

```python
series.max()
```

는:

```text
가장 큰 Value 자체
```

를 반환한다.

---

# 80. 예

```python
s = pd.Series(
    {
        "A": 100,
        "B": 300,
        "C": 200
    }
)
```

---

# 81. idxmax

```python
print(
    s.idxmax()
)
```

결과:

```text
B
```

---

# 82. max

```python
print(
    s.max()
)
```

결과:

```text
300
```

---

# 83. 둘을 같이 사용

```python
print(
    "성분:",
    ingredient_sum.idxmax()
)


print(
    "제품수:",
    ingredient_sum.max()
)
```

처럼 사용할 수 있다.

---

# 84. 두 성분의 방향

병용금기 Dataset에서는:

```text
성분 A

성분 B
```

의 방향이 실제 분석에서 중요한지 확인해야 한다.

---

# 85. 예

Dataset에:

```text
A + B
```

와:

```text
B + A
```

가 별도 Row로 존재할 수 있다.

---

# 86. 같은 조합으로 볼 것인가?

이것은 Dataset 정의와 분석 목적을 확인해야 한다.

---

# 87. 순서 없는 Pair라면

예를 들어:

```text
A + B
```

와:

```text
B + A
```

를 같은 Pair로 분석해야 할 수도 있다.

---

# 88. Pair 정규화 예

```python
def normalize_pair(
    row
):

    pair = sorted(
        [
            str(
                row[
                    "성분A"
                ]
            ),

            str(
                row[
                    "성분B"
                ]
            )
        ]
    )

    return (
        pair[
            0
        ],
        pair[
            1
        ]
    )
```

---

# 89. Pair Column

```python
df[
    "pair"
] = df.apply(
    normalize_pair,
    axis=1
)
```

---

# 90. Pair별 집계

```python
pair_sum = (
    df.groupby(
        "pair"
    )[
        "제품수"
    ]
    .sum()
    .sort_values(
        ascending=False
    )
)
```

---

# 91. 하지만 주의

무조건 이렇게 바꾸면 안 된다.

Dataset에서 A와 B가 서로 다른 역할을 가진다면 순서를 유지해야 한다.

---

# 92. Data Dictionary가 중요한 이유

Column 이름만 보고:

```text
두 성분은 완전히 대칭이다.
```

라고 가정하면 안 된다.

---

# 93. 제품수 중복 문제

같은 제품이 여러 Row에 반복되어 있다면 단순 `sum()`이 실제 제품 개수를 중복 계산할 수도 있다.

---

# 94. 따라서 확인

```text
제품수 Column은 이미 집계된 값인가?

제품 ID가 따로 있는가?

한 제품이 여러 Row에 중복되는가?
```

를 확인한다.

---

# 95. Count와 Sum과 nunique

서로 다른 연산이다.

```text
count()
→ Row / Non-null 개수

sum()
→ 숫자 값의 합

nunique()
→ 서로 다른 값의 개수
```

이다.

---

# 96. 예

```python
df.groupby(
    "성분명"
).agg(
    row_count=(
        "제품수",
        "count"
    ),

    product_sum=(
        "제품수",
        "sum"
    )
)
```

---

# 97. 제품 ID가 있다면

```python
df.groupby(
    "성분명"
)[
    "제품ID"
].nunique()
```

처럼 Unique 제품 수를 구할 수도 있다.

---

# 98. 문제에서 요구하는 기준

항상:

```text
개수인가?

합계인가?

평균인가?

비율인가?

Unique 개수인가?
```

를 먼저 확인한다.

---

# 99. Visualization

제품수 합계 상위 성분을 그래프로 볼 수 있다.

```python
top10 = (
    ingredient_product_sum.head(
        10
    )
)


top10.plot(
    kind="bar"
)
```

---

# 100. 제목

```python
import matplotlib.pyplot as plt


plt.title(
    "Top 10 Ingredients by Product Count"
)


plt.ylabel(
    "Total Product Count"
)


plt.show()
```

---

# 101. Groupby의 핵심

Pandas의:

```python
groupby()
```

는 실제 Data Analysis에서 매우 중요하다.

---

# 102. 기본 구조

```text
어떤 기준으로 묶을 것인가?

↓

각 Group에서 무엇을 계산할 것인가?
```

이다.

---

# 103. 예

```python
df.groupby(
    "성분명"
)[
    "제품수"
].sum()
```

을 말로 읽으면:

```text
성분명별로 묶고

각 성분의 제품수를 합한다.
```

이다.

---

# 104. 여러 기준

```python
df.groupby(
    [
        "성분명",
        "병용성분명"
    ]
)[
    "제품수"
].sum()
```

은:

```text
성분 + 병용성분 조합별
제품수 합계
```

이다.

---

# 105. MultiIndex 결과

여러 Column으로 Groupby하면 결과 Index가 여러 단계가 될 수 있다.

---

# 106. reset_index()

일반 DataFrame 형태로 만들고 싶다면:

```python
result = (
    df.groupby(
        [
            "성분명",
            "병용성분명"
        ],
        as_index=False
    )[
        "제품수"
    ]
    .sum()
)
```

또는:

```python
result = (
    df.groupby(
        [
            "성분명",
            "병용성분명"
        ]
    )[
        "제품수"
    ]
    .sum()
    .reset_index()
)
```

을 사용할 수 있다.

---

# 107. Sort

```python
result = result.sort_values(
    "제품수",
    ascending=False
)
```

---

# 108. Top 조합

```python
display(
    result.head(
        10
    )
)
```

---

# 109. 한 번에 전체 Pair 찾기

질문에 따라 전체 성분 Pair 중 제품수 합계가 가장 큰 조합을 찾는 문제라면:

```python
pair_result = (
    df.groupby(
        [
            "성분명",
            "병용성분명"
        ],
        as_index=False
    )[
        "제품수"
    ]
    .sum()
    .sort_values(
        "제품수",
        ascending=False
    )
)


display(
    pair_result.head(
        1
    )
)
```

처럼 할 수 있다.

---

# 110. 하지만 문제를 정확히 읽는다

다음 두 문제는 다르다.

```text
전체 병용 조합 중
제품수가 가장 많은 조합
```

과:

```text
제품수 합계가 가장 많은 성분을 먼저 찾고,
그 성분의 병용 조합 중
제품수가 가장 많은 조합
```

은 계산 순서가 다르다.

---

# 111. 이번 문제 구조

후자라면:

```text
Top Ingredient 먼저

↓

그 Ingredient만 Filtering

↓

Top Combination
```

순서로 풀어야 한다.

---

# 112. SQL로 생각하면

같은 분석을 SQL 관점으로 생각할 수도 있다.

```sql
SELECT
    ingredient,
    SUM(product_count) AS total_products
FROM contraindication
GROUP BY ingredient
ORDER BY total_products DESC;
```

---

# 113. Pandas와 SQL

```text
Pandas
groupby()

SQL
GROUP BY
```

는 비슷한 역할을 한다.

---

# 114. Filtering

Pandas:

```python
df.loc[
    df[
        "성분명"
    ] == top_ingredient
]
```

SQL:

```sql
WHERE ingredient = ...
```

와 비슷하다.

---

# 115. Sort

Pandas:

```python
sort_values(
    ascending=False
)
```

SQL:

```sql
ORDER BY ... DESC
```

와 비슷하다.

---

# 116. 프로젝트에서 배운 점 1

문제를 풀기 전에:

```text
무엇을 세야 하는가?
```

를 정확히 확인한다.

---

# 117. 배운 점 2

```text
Row 수
```

와:

```text
제품수 합계
```

는 다르다.

---

# 118. 배운 점 3

범주형 변수끼리 관계를 분석할 때:

```text
Crosstab
+
Chi-square Test
```

를 사용할 수 있다.

---

# 119. 배운 점 4

p-value만 제시하지 않는다.

```text
실제 빈도

비율

그래프
```

도 함께 확인한다.

---

# 120. 배운 점 5

```text
통계적으로 유의하다.
```

와:

```text
원인이다.
```

는 다른 말이다.

---

# 121. 배운 점 6

Groupby 문제는:

```text
Group 기준

Aggregation 기준
```

을 분리해서 생각하면 쉽다.

---

# 122. 예

질문:

```text
성분별 제품수 합계
```

이면:

```text
Group
→ 성분

Aggregation
→ 제품수의 sum
```

이다.

---

# 123. 또 다른 예

질문:

```text
투여경로별 1등급 비율
```

이면:

```text
Group
→ 투여경로

Target Category
→ 1등급

Aggregation
→ 비율
```

이다.

---

# 124. 프로젝트 발표 흐름

```text
1. 문제 정의

2. Dataset 구조

3. Data Cleaning

4. 금기등급 분포

5. 투여경로 분포

6. 투여경로 × 금기등급 Crosstab

7. 비율 비교

8. Chi-square Test

9. 성분별 제품수 집계

10. Top Ingredient

11. Top 병용성분 조합

12. 결과 해석

13. 한계
```

---

# 125. 발표 예시

```text
임부금기 Data에서
금기등급별 분포를 확인한 결과

1등급은 3,132건,
2등급은 15,480건이었다.

투여경로별 1등급 비율을 확인한 결과
내복 약 19.08%,
외용 약 3.15%,
주사 약 5.94%로
분포 차이가 관찰되었다.

카이제곱 독립성 검정을 실시한 결과
χ² ≈ 353.766,
p ≈ 1.52 × 10^-77로

투여경로와 금기등급 사이에
통계적으로 유의한 관련성이 관찰되었다.
```

---

# 126. 좋은 표현

```text
관련성이 관찰되었다.
```

라고 한다.

---

# 127. 피해야 할 표현

```text
내복이 1등급 금기를 발생시킨다.
```

처럼 인과관계를 단정하지 않는다.

---

# 128. 분석 한계

다음과 같은 한계를 고려한다.

```text
Dataset 자체의 수집 방식

중복 Record 가능성

제품수 정의

성분 Pair 방향성

Category 표기 차이

Missing Value

큰 Sample Size에 따른 매우 작은 p-value
```

등이다.

---

# 129. 추가 분석

다음과 같은 분석으로 확장할 수 있다.

```text
성분별 금기등급 분포

투여경로별 제품수

금기등급별 주요 성분

병용금기 Network

제품수 상위 Pair

성분별 연결 정도
```

---

# 130. Network 분석

병용금기 관계를:

```text
Node
→ 성분

Edge
→ 병용금기 관계
```

로 표현할 수도 있다.

---

# 131. 예

```text
Ingredient A
    │
    ├── Ingredient B
    │
    └── Ingredient C
```

처럼 연결 관계를 분석할 수 있다.

---

# 132. Network에서 질문

```text
어떤 성분이 가장 많은 금기 관계를 가지는가?

어떤 Pair의 제품수가 가장 많은가?

특정 성분 주변에는 어떤 성분이 연결되는가?
```

등을 분석할 수 있다.

---

# 133. Machine Learning이 필요한가?

현재 질문에서는 반드시 필요하지 않다.

---

# 134. 이유

우리가 알고 싶은 것은:

```text
Prediction
```

이 아니라:

```text
현재 Data의 분포와 관계
```

이기 때문이다.

---

# 135. 만약 Prediction으로 확장한다면

예를 들어:

```text
의약품 특성
↓
금기등급 Prediction
```

같은 새로운 문제를 정의할 수는 있다.

---

# 136. 하지만

Prediction Target을 만든다고 무조건 유용한 AI 문제가 되는 것은 아니다.

실제 서비스 목적과 Prediction 시점에 사용 가능한 정보를 먼저 정의해야 한다.

---

# 137. 이 프로젝트의 핵심

```text
질문
↓
필요한 Column 선택
↓
Cleaning
↓
Groupby / Crosstab
↓
Statistical Test
↓
Result
↓
Interpretation
```

이다.

---

# 138. 전체 Workflow

```text
Drug Contraindication Data
↓
Shape / Columns / Type 확인
↓
Missing / String Cleaning
↓
금기등급 Count
↓
투여경로 Count
↓
투여경로 × 금기등급 Crosstab
↓
투여경로별 금기등급 비율
↓
Chi-square Test
↓
χ² ≈ 353.766
p ≈ 1.52e-77
↓
통계적 관련성 확인
↓
성분별 제품수 Sum
↓
Top Ingredient
↓
해당 성분 Filtering
↓
병용성분별 제품수 Sum
↓
Top Combination
↓
결과 해석
```

---

# 139. 핵심 결과

```text
임부금기 1등급
3,132건

임부금기 2등급
15,480건
```

투여경로별 1등급 비율:

```text
내복
약 19.08%

외용
약 3.15%

주사
약 5.94%
```

카이제곱 검정:

```text
χ²
약 353.766

p-value
약 1.52 × 10^-77
```

---

# 140. 가장 중요한 해석

```text
투여경로에 따라
금기등급의 분포 차이가 관찰되었으며,

카이제곱 검정에서도
두 변수 사이의 통계적으로 유의한 관련성이
확인되었다.
```

---

# 141. 가장 중요한 코딩 교훈

```text
value_counts()
```

와:

```text
groupby().sum()
```

을 구분한다.

---

# 142. value_counts()

질문:

```text
각 성분이 몇 Row 등장했는가?
```

---

# 143. groupby().sum()

질문:

```text
각 성분의 제품수를 모두 더하면 얼마인가?
```

---

# 144. Crosstab

질문:

```text
두 Category가 어떤 조합으로 등장하는가?
```

---

# 145. Chi-square

질문:

```text
두 Category 사이에
통계적인 관련성이 있는가?
```

---

# 146. 한 문장으로 설명

이 프로젝트는:

```text
의약품 임부금기 및 병용금기 Data를 이용하여
금기등급과 투여경로의 분포를 분석하고,

카이제곱 검정을 통해
범주형 변수 사이의 관련성을 검토하며,

Groupby와 Aggregation을 이용해
제품수 합계가 큰 성분과
주요 병용금기 조합을 탐색한
의약품 Data Analysis 프로젝트
```

이다.

---

# 147. 마지막 교훈

모든 AI/Data 프로젝트에서 중요한 것은:

```text
복잡한 Model을 사용하는 것
```

이 아니다.

먼저:

```text
질문이 무엇인지 정확히 이해하고

그 질문에 필요한 집계가
Count인지 Sum인지 Ratio인지 구분하고

적절한 통계 방법을 사용하여

결과를 과도하게 해석하지 않는 것
```

이 중요하다.