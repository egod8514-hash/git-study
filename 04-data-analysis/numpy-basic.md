# NumPy 기초

## 1. NumPy란?

NumPy는 Python에서 수치 계산을 효율적으로 처리하기 위한 Library이다.

설치:

```bash
pip install numpy
```

사용:

```python
import numpy as np
```

보통 NumPy는 다음과 같이 별명을 사용한다.

```text
numpy
→ np
```

NumPy는 데이터 분석, 머신러닝, 딥러닝의 기초가 되는 Library이다.

---

# 2. Python List와 NumPy

Python에서는 여러 데이터를 List로 저장할 수 있다.

```python
numbers = [10, 20, 30, 40]
```

NumPy에서는 배열(Array)을 사용할 수 있다.

```python
import numpy as np

numbers = np.array([10, 20, 30, 40])
```

출력:

```python
print(numbers)
```

결과:

```text
[10 20 30 40]
```

NumPy 배열은 많은 숫자를 빠르게 계산하는 데 적합하다.

---

# 3. ndarray

NumPy의 배열은 `ndarray`라는 자료형을 사용한다.

```python
data = np.array([10, 20, 30])

print(type(data))
```

결과:

```text
<class 'numpy.ndarray'>
```

즉:

```text
Python List
→ list

NumPy Array
→ numpy.ndarray
```

이다.

---

# 4. List와 Array의 계산 차이

Python List:

```python
data = [1, 2, 3]

print(data * 2)
```

결과:

```text
[1, 2, 3, 1, 2, 3]
```

List 자체가 두 번 반복된다.

NumPy:

```python
data = np.array([1, 2, 3])

print(data * 2)
```

결과:

```text
[2 4 6]
```

각 숫자에 2를 곱한다.

이것이 데이터 분석에서 NumPy가 중요한 이유 중 하나이다.

---

# 5. 배열 생성

1차원 배열:

```python
data = np.array([10, 20, 30])
```

2차원 배열:

```python
data = np.array([
    [10, 20, 30],
    [40, 50, 60]
])
```

구조:

```text
10 20 30
40 50 60
```

---

# 6. shape

배열의 형태를 확인할 때 `shape`를 사용한다.

```python
data = np.array([
    [10, 20, 30],
    [40, 50, 60]
])

print(data.shape)
```

결과:

```text
(2, 3)
```

의미:

```text
2행
3열
```

이다.

---

# 7. 머신러닝에서 shape

머신러닝을 하면서 다음과 같은 출력을 많이 확인했다.

```python
print(X.shape)
```

예:

```text
(1267, 24)
```

의미는:

```text
1267개의 데이터
24개의 변수
```

라고 볼 수 있다.

이미지 데이터에서는:

```text
(32, 3, 224, 224)
```

같은 형태도 볼 수 있다.

이는 보통:

```text
32
→ Batch 크기

3
→ Channel

224
→ Height

224
→ Width
```

를 의미한다.

따라서 `shape`를 읽는 능력은 머신러닝과 딥러닝에서도 매우 중요하다.

---

# 8. ndim

배열이 몇 차원인지 확인한다.

```python
data = np.array([
    [10, 20, 30],
    [40, 50, 60]
])

print(data.ndim)
```

결과:

```text
2
```

2차원 배열이라는 뜻이다.

---

# 9. size

전체 데이터 개수를 확인한다.

```python
print(data.size)
```

2행 3열이라면:

```text
6
```

이다.

주의할 점은:

```text
shape
→ 배열 구조

size
→ 전체 원소 개수
```

라는 차이이다.

---

# 10. dtype

배열의 자료형을 확인한다.

```python
data = np.array([10, 20, 30])

print(data.dtype)
```

환경에 따라 다음과 같은 형태가 출력될 수 있다.

```text
int64
```

실수:

```python
data = np.array([10.5, 20.5, 30.5])

print(data.dtype)
```

예:

```text
float64
```

---

# 11. zeros

0으로 채워진 배열을 만들 수 있다.

```python
data = np.zeros(5)

print(data)
```

결과:

```text
[0. 0. 0. 0. 0.]
```

2차원도 가능하다.

```python
data = np.zeros((2, 3))
```

결과:

```text
[[0. 0. 0.]
 [0. 0. 0.]]
```

---

# 12. 머신러닝에서 np.zeros()

실제 머신러닝 코드에서 다음과 같은 형태를 사용했다.

```python
oof = {
    m: np.zeros(len(y))
    for m in model_names
}
```

여기서:

```python
np.zeros(len(y))
```

는 정답 데이터 `y`의 개수만큼 0으로 채워진 공간을 먼저 만드는 것이다.

예측값을 나중에 이 공간에 저장할 수 있다.

개념적으로:

```text
처음

[0, 0, 0, 0, 0]

      ↓ 예측값 저장

[0.2, 0.8, 0.7, 0.1, 0.9]
```

와 같은 방식이다.

---

# 13. ones

1로 채워진 배열:

```python
data = np.ones(5)

print(data)
```

결과:

```text
[1. 1. 1. 1. 1.]
```

---

# 14. arange

일정한 범위의 숫자를 만들 수 있다.

```python
data = np.arange(5)

print(data)
```

결과:

```text
[0 1 2 3 4]
```

Python의:

```python
range(5)
```

와 비슷한 개념이다.

---

# 15. reshape

배열의 모양을 변경할 수 있다.

```python
data = np.arange(6)

print(data)
```

결과:

```text
[0 1 2 3 4 5]
```

이를:

```python
data = data.reshape(2, 3)
```

로 변경하면:

```text
[[0 1 2]
 [3 4 5]]
```

가 된다.

즉:

```text
(6,)
      ↓ reshape
(2, 3)
```

이다.

전체 데이터 개수는 그대로 유지되어야 한다.

---

# 16. 인덱싱

NumPy에서도 Index를 사용할 수 있다.

```python
data = np.array([10, 20, 30, 40])

print(data[0])
```

결과:

```text
10
```

마지막:

```python
print(data[-1])
```

결과:

```text
40
```

---

# 17. 2차원 인덱싱

```python
data = np.array([
    [10, 20, 30],
    [40, 50, 60]
])
```

다음과 같이 접근할 수 있다.

```python
data[0, 0]
```

결과:

```text
10
```

```python
data[1, 2]
```

결과:

```text
60
```

구조:

```text
data[행, 열]
```

이다.

---

# 18. 슬라이싱

```python
data = np.array([10, 20, 30, 40, 50])
```

다음과 같이 일부 데이터를 선택할 수 있다.

```python
data[1:4]
```

결과:

```text
[20 30 40]
```

Python List와 마찬가지로:

```text
start 포함
stop 제외
```

이다.

---

# 19. 모든 행 선택

2차원 데이터:

```python
data = np.array([
    [10, 20, 30],
    [40, 50, 60]
])
```

다음 코드:

```python
data[:, 0]
```

의 의미는:

```text
:
→ 모든 행

0
→ 첫 번째 열
```

이다.

결과:

```text
[10 40]
```

---

# 20. 특정 열 선택

```python
data[:, 1]
```

결과:

```text
[20 50]
```

즉 두 번째 열 전체를 선택한다.

머신러닝 데이터를 다룰 때 이런 인덱싱을 자주 볼 수 있다.

---

# 21. 특정 행 선택

```python
data[0, :]
```

결과:

```text
[10 20 30]
```

의미:

```text
0
→ 첫 번째 행

:
→ 모든 열
```

이다.

---

# 22. 조건을 이용한 선택

NumPy 배열에서도 조건을 사용할 수 있다.

```python
data = np.array([10, 20, 30, 40, 50])

condition = data >= 30

print(condition)
```

결과:

```text
[False False True True True]
```

조건을 이용해 데이터를 선택할 수도 있다.

```python
data[data >= 30]
```

결과:

```text
[30 40 50]
```

---

# 23. Pandas와 연결되는 조건 선택

이 개념은 Pandas에서도 비슷하게 사용된다.

NumPy:

```python
data[data >= 30]
```

Pandas:

```python
df[df["Age"] >= 30]
```

즉 Python과 NumPy에서 배운 조건 선택이 Pandas에서도 확장되어 사용된다.

---

# 24. 기본 통계 계산

NumPy는 기본적인 통계 계산도 제공한다.

평균:

```python
data = np.array([10, 20, 30, 40])

print(np.mean(data))
```

결과:

```text
25.0
```

---

# 25. 중앙값

```python
print(np.median(data))
```

결과:

```text
25.0
```

---

# 26. 표준편차

```python
print(np.std(data))
```

데이터가 평균을 중심으로 얼마나 퍼져 있는지를 나타내는 값이다.

---

# 27. 최솟값과 최댓값

```python
print(np.min(data))
print(np.max(data))
```

결과:

```text
10
40
```

---

# 28. 합계

```python
print(np.sum(data))
```

결과:

```text
100
```

---

# 29. axis

2차원 배열에서는 `axis`라는 개념이 중요하다.

```python
data = np.array([
    [10, 20],
    [30, 40]
])
```

전체 평균:

```python
np.mean(data)
```

결과:

```text
25.0
```

열별 평균:

```python
np.mean(data, axis=0)
```

결과:

```text
[20. 30.]
```

행별 평균:

```python
np.mean(data, axis=1)
```

결과:

```text
[15. 35.]
```

처음에는 다음처럼 기억할 수 있다.

```text
axis=0
→ 행 방향으로 내려가며 계산
→ 열별 결과

axis=1
→ 열 방향으로 옆으로 계산
→ 행별 결과
```

---

# 30. 결측값 NaN

데이터 분석에서는 값이 없는 경우가 있다.

NumPy에서는 다음과 같은 값을 볼 수 있다.

```python
np.nan
```

예:

```python
data = np.array([
    10,
    20,
    np.nan,
    40
])
```

`NaN`은:

```text
Not a Number
```

을 의미한다.

데이터 분석에서는 결측값을 표현할 때 자주 사용된다.

---

# 31. NaN과 평균

다음 데이터를 생각해보자.

```python
data = np.array([
    10,
    20,
    np.nan,
    40
])
```

일반적인 평균:

```python
np.mean(data)
```

에서는 `NaN` 때문에 결과도 `NaN`이 될 수 있다.

결측값을 제외하고 평균을 계산하려면:

```python
np.nanmean(data)
```

를 사용할 수 있다.

---

# 32. 난수

NumPy에서는 Random Number도 만들 수 있다.

```python
np.random.seed(42)

data = np.random.rand(5)

print(data)
```

`seed`를 설정하면 같은 난수 결과를 재현하는 데 도움이 된다.

---

# 33. random_state와 연결

머신러닝에서 다음과 같은 코드를 자주 사용한다.

```python
RANDOM_STATE = 42
```

또는:

```python
np.random.seed(42)
```

그리고:

```python
train_test_split(
    X,
    y,
    random_state=42
)
```

처럼 사용한다.

무작위 과정이 실행될 때 결과를 가능한 한 동일하게 재현하기 위해 Seed나 Random State를 고정하는 것이다.

---

# 34. 배열 연산

NumPy에서는 배열끼리 계산할 수 있다.

```python
a = np.array([1, 2, 3])
b = np.array([10, 20, 30])

print(a + b)
```

결과:

```text
[11 22 33]
```

곱하기:

```python
print(a * b)
```

결과:

```text
[10 40 90]
```

각 위치의 원소끼리 계산된다.

---

# 35. 벡터화 연산

예를 들어 모든 값에 10을 더한다고 하자.

반복문을 사용할 수도 있다.

```python
data = [1, 2, 3]

for value in data:
    print(value + 10)
```

NumPy에서는:

```python
data = np.array([1, 2, 3])

result = data + 10
```

처럼 배열 전체에 연산할 수 있다.

이런 방식은 데이터 분석에서 매우 중요하다.

---

# 36. 머신러닝의 X와 y

머신러닝에서는 일반적으로:

```text
X
→ 모델이 학습할 입력 데이터

y
→ 모델이 맞혀야 하는 정답
```

으로 표현한다.

예:

```python
print(X.shape)
print(y.shape)
```

결과가:

```text
(1000, 20)
(1000,)
```

이라면:

```text
X
→ 1000개의 데이터
→ 20개의 Feature

y
→ 1000개의 정답
```

이라고 이해할 수 있다.

---

# 37. 이미지 데이터와 NumPy

이미지도 결국 숫자의 배열로 표현할 수 있다.

흑백 이미지:

```text
높이 × 너비
```

RGB 이미지:

```text
높이 × 너비 × 채널
```

형태로 생각할 수 있다.

예:

```text
224 × 224
```

또는:

```text
224 × 224 × 3
```

이다.

각 Pixel에는 숫자가 저장되어 있다.

따라서 Computer Vision에서도 배열 개념이 매우 중요하다.

---

# 38. 딥러닝 Tensor와 NumPy

딥러닝에서는 NumPy Array 대신 Tensor를 많이 사용한다.

NumPy:

```python
import numpy as np

data = np.array([1, 2, 3])
```

PyTorch:

```python
import torch

data = torch.tensor([1, 2, 3])
```

둘 다 여러 숫자를 배열 형태로 관리한다는 점에서 개념적으로 연결된다.

PyTorch Tensor는 추가적으로 GPU 연산과 자동 미분 같은 딥러닝 기능을 지원한다.

---

# 39. shape 오류와 연결

딥러닝을 하면서 다음과 같은 오류를 볼 수 있다.

```text
RuntimeError:
mat1 and mat2 shapes cannot be multiplied
```

이 오류에서 중요한 단어가:

```text
shapes
```

이다.

신경망에 전달한 데이터의 형태와 `Linear` Layer가 기대하는 입력 형태가 맞지 않을 때 발생할 수 있다.

따라서 모델 오류를 해결할 때:

```python
print(x.shape)
```

로 데이터의 형태를 확인하는 습관이 중요하다.

---

# 40. 자주 확인할 NumPy 속성

배열을 받으면 먼저 다음을 확인할 수 있다.

```python
print(data.shape)
print(data.ndim)
print(data.size)
print(data.dtype)
```

각각:

```text
shape
→ 데이터 구조

ndim
→ 차원

size
→ 전체 원소 개수

dtype
→ 데이터 자료형
```

이다.

---

# 41. NumPy에서 자주 사용하는 기능

```text
np.array()
→ 배열 생성

np.zeros()
→ 0 배열

np.ones()
→ 1 배열

np.arange()
→ 연속 숫자 생성

reshape()
→ 배열 모양 변경

np.mean()
→ 평균

np.median()
→ 중앙값

np.std()
→ 표준편차

np.min()
→ 최솟값

np.max()
→ 최댓값

np.sum()
→ 합계

np.nan
→ 결측값

np.random.seed()
→ 난수 Seed 설정
```

---

# 42. NumPy와 이후 학습

NumPy에서 배우는 핵심 개념:

```text
Array
Shape
Dimension
Index
Slicing
Condition
dtype
NaN
통계 계산
```

은 이후:

```text
Pandas
Scikit-learn
PyTorch
Computer Vision
```

에서 계속 등장한다.

---

# 43. 전체 연결

Python에서는:

```python
data = [10, 20, 30]
```

List를 배웠다.

NumPy에서는:

```python
data = np.array([10, 20, 30])
```

Array로 발전한다.

Pandas에서는:

```python
df = pd.DataFrame(...)
```

표 형태의 데이터를 다룬다.

머신러닝에서는:

```python
model.fit(X_train, y_train)
```

NumPy나 Pandas 형태의 데이터를 모델에 전달한다.

딥러닝에서는:

```python
tensor = torch.tensor(...)
```

Tensor 형태로 데이터를 다룬다.

즉 각각 완전히 떨어진 개념이 아니라 서로 연결되어 있다.

---

# 44. 배운 점

NumPy는 Python에서 숫자 데이터를 효율적으로 처리하기 위한 핵심 Library이다.

Python List와 달리 NumPy Array는 배열 전체에 수치 연산을 쉽게 적용할 수 있다.

특히 다음 개념이 중요하다.

```text
shape
ndim
size
dtype
index
slicing
axis
NaN
```

머신러닝에서는 `X.shape`, `y.shape`를 확인하여 데이터 개수와 Feature 개수를 파악하고 딥러닝에서는 입력 Tensor의 Shape를 확인하여 모델 구조와 데이터 구조가 맞는지 확인한다.

따라서:

```python
print(data.shape)
```

는 단순한 출력 코드가 아니라 데이터 분석과 AI 프로젝트에서 문제를 확인하기 위한 매우 중요한 습관이다.

NumPy를 이해하면 이후 Pandas의 DataFrame, Scikit-learn의 학습 데이터, PyTorch의 Tensor와 이미지 배열을 이해하기 쉬워진다.