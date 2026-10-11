# Tensor Basic

## 1. Tensor란?

PyTorch에서 데이터를 표현하는 가장 기본적인 자료구조가:

```text
Tensor
```

이다.

쉽게 생각하면:

```text
숫자를 담는 다차원 배열
```

이다.

Python에서는:

```text
List
```

NumPy에서는:

```text
ndarray
```

PyTorch에서는:

```text
Tensor
```

를 사용한다.

---

# 2. Tensor가 중요한 이유

PyTorch에서는 다음 대부분이 Tensor로 처리된다.

```text
Input Data

Image

Label

Model Output

Loss 계산에 사용되는 값

Gradient
```

즉 Deep Learning에서 데이터를 Model에 넣으려면 Tensor를 이해해야 한다.

---

# 3. PyTorch Import

```python
import torch
```

보통 PyTorch는:

```python
torch
```

라는 이름으로 사용한다.

---

# 4. 가장 간단한 Tensor

```python
x = torch.tensor(
    [
        1,
        2,
        3
    ]
)

print(
    x
)
```

결과:

```text
tensor([1, 2, 3])
```

---

# 5. Python List와 비교

Python List:

```python
x = [
    1,
    2,
    3
]
```

PyTorch Tensor:

```python
x = torch.tensor(
    [
        1,
        2,
        3
    ]
)
```

겉보기에는 비슷하지만 Tensor는 수치 계산과 GPU 연산, 자동 미분 등에 사용할 수 있다.

---

# 6. 0차원 Tensor

숫자 하나도 Tensor로 만들 수 있다.

```python
x = torch.tensor(
    5
)

print(
    x
)

print(
    x.shape
)
```

Shape:

```text
torch.Size([])
```

이런 Tensor를 Scalar Tensor라고 생각할 수 있다.

---

# 7. 1차원 Tensor

```python
x = torch.tensor(
    [
        10,
        20,
        30
    ]
)
```

Shape:

```text
[3]
```

이다.

개념적으로:

```text
[10, 20, 30]
```

한 줄의 데이터이다.

---

# 8. 2차원 Tensor

```python
x = torch.tensor(
    [
        [1, 2, 3],
        [4, 5, 6]
    ]
)
```

Shape:

```text
[2, 3]
```

이다.

즉:

```text
2행
3열
```

이다.

---

# 9. 3차원 Tensor

```python
x = torch.tensor(
    [
        [
            [1, 2],
            [3, 4]
        ],

        [
            [5, 6],
            [7, 8]
        ]
    ]
)
```

Shape:

```text
[2, 2, 2]
```

이다.

---

# 10. Tensor 차원

Tensor의 차원 수는:

```python
x.ndim
```

으로 확인할 수 있다.

예:

```python
print(
    x.ndim
)
```

---

# 11. Shape

Deep Learning에서 가장 중요한 것 중 하나가:

```text
Shape
```

이다.

확인:

```python
print(
    x.shape
)
```

---

# 12. size()

다음처럼도 확인할 수 있다.

```python
print(
    x.size()
)
```

`shape`와 `size()`는 Tensor 크기를 확인할 때 자주 사용한다.

---

# 13. numel()

Tensor에 총 몇 개의 값이 있는지 확인:

```python
print(
    x.numel()
)
```

예:

```text
Shape = [2, 3]

값 개수
=
2 × 3
=
6
```

이다.

---

# 14. dtype

Tensor가 어떤 Data Type을 사용하는지 확인:

```python
print(
    x.dtype
)
```

---

# 15. Integer Tensor

```python
x = torch.tensor(
    [
        1,
        2,
        3
    ]
)

print(
    x.dtype
)
```

환경과 생성 방식에 따라 정수 Tensor는 일반적으로 `torch.int64`가 사용되는 경우가 많다.

---

# 16. Float Tensor

```python
x = torch.tensor(
    [
        1.0,
        2.0,
        3.0
    ]
)

print(
    x.dtype
)
```

일반적으로:

```text
torch.float32
```

를 자주 사용한다.

---

# 17. dtype 직접 지정

```python
x = torch.tensor(
    [
        1,
        2,
        3
    ],
    dtype=torch.float32
)
```

---

# 18. 왜 dtype이 중요할까?

Model은 특정 dtype의 Tensor를 기대할 수 있다.

예를 들어 일반적인 Neural Network Weight는:

```text
float32
```

를 사용하는 경우가 많다.

Input이 정수 Tensor이면 오류가 발생할 수 있다.

---

# 19. Float로 변환

```python
x = x.float()
```

또는:

```python
x = x.to(
    torch.float32
)
```

---

# 20. Long으로 변환

```python
x = x.long()
```

이는:

```text
torch.int64
```

로 변환한다.

---

# 21. Classification Label

PyTorch의:

```python
nn.CrossEntropyLoss()
```

를 사용하는 일반적인 Multi-class Classification에서는 Target Label이 보통:

```text
torch.long
```

형식이어야 한다.

예:

```python
y = y.long()
```

---

# 22. Input과 Label dtype

일반적인 Classification 예:

```text
Input X
→ float32

Target y
→ long
```

이라고 기억하면 이해하기 쉽다.

단, Loss Function에 따라 요구되는 dtype과 shape는 달라질 수 있다.

---

# 23. zeros

모든 값이 0인 Tensor:

```python
x = torch.zeros(
    3,
    4
)

print(
    x
)
```

Shape:

```text
[3, 4]
```

---

# 24. ones

모든 값이 1:

```python
x = torch.ones(
    3,
    4
)
```

---

# 25. rand

0과 1 사이의 Random 값:

```python
x = torch.rand(
    3,
    4
)
```

---

# 26. randn

Normal Distribution 기반 Random Tensor:

```python
x = torch.randn(
    3,
    4
)
```

---

# 27. arange

NumPy의 `arange`와 비슷하다.

```python
x = torch.arange(
    0,
    10
)

print(
    x
)
```

결과:

```text
tensor([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
```

---

# 28. linspace

일정한 간격으로 값을 생성한다.

```python
x = torch.linspace(
    0,
    1,
    steps=5
)

print(
    x
)
```

---

# 29. Tensor Indexing

```python
x = torch.tensor(
    [
        10,
        20,
        30,
        40
    ]
)
```

첫 번째 값:

```python
print(
    x[0]
)
```

---

# 30. 음수 Index

```python
print(
    x[-1]
)
```

마지막 값을 가져온다.

Python List와 비슷하다.

---

# 31. 2차원 Indexing

```python
x = torch.tensor(
    [
        [1, 2, 3],
        [4, 5, 6]
    ]
)
```

첫 번째 행:

```python
print(
    x[0]
)
```

---

# 32. 특정 값

```python
print(
    x[0, 1]
)
```

결과:

```text
2
```

이다.

---

# 33. Slicing

```python
print(
    x[
        :,
        0
    ]
)
```

모든 행의 첫 번째 열을 가져온다.

NumPy와 비슷하다.

---

# 34. Boolean Indexing

```python
x = torch.tensor(
    [
        1,
        5,
        10,
        15
    ]
)


print(
    x[
        x > 5
    ]
)
```

결과:

```text
tensor([10, 15])
```

---

# 35. Tensor 연산

```python
a = torch.tensor(
    [
        1.0,
        2.0,
        3.0
    ]
)


b = torch.tensor(
    [
        4.0,
        5.0,
        6.0
    ]
)
```

더하기:

```python
print(
    a + b
)
```

---

# 36. 뺄셈

```python
print(
    a - b
)
```

---

# 37. 곱셈

```python
print(
    a * b
)
```

이것은 Element-wise Multiplication이다.

즉 같은 위치의 값끼리 곱한다.

---

# 38. Matrix Multiplication과 다르다

행렬 곱은:

```python
torch.matmul()
```

또는:

```python
@
```

를 사용할 수 있다.

예:

```python
result = a @ b
```

연산 의미는 Tensor Shape에 따라 달라진다.

---

# 39. Element-wise와 Matrix Multiplication

```text
a * b
→ 같은 위치 값끼리 곱셈

a @ b
→ 행렬 곱 / 벡터 곱 규칙
```

이다.

---

# 40. 평균

```python
x = torch.tensor(
    [
        1.0,
        2.0,
        3.0
    ]
)


print(
    x.mean()
)
```

---

# 41. 합계

```python
print(
    x.sum()
)
```

---

# 42. 최대 / 최소

```python
print(
    x.max()
)


print(
    x.min()
)
```

---

# 43. argmax

가장 큰 값의 위치:

```python
print(
    x.argmax()
)
```

Classification Prediction에서 매우 자주 사용한다.

---

# 44. Classification 예

Model Output:

```python
logits = torch.tensor(
    [
        [0.2, 1.5, 0.3],
        [2.0, 0.4, 0.1]
    ]
)
```

Shape:

```text
[2, 3]
```

의미:

```text
2 Samples
3 Classes
```

이다.

---

# 45. Class Prediction

```python
pred = logits.argmax(
    dim=1
)


print(
    pred
)
```

결과:

```text
tensor([1, 0])
```

이다.

---

# 46. dim이란?

Tensor에서:

```text
어느 차원을 기준으로 계산할 것인가?
```

를 지정한다.

예:

```text
Shape = [2, 3]
```

일 때:

```text
dim=0
→ 행 방향을 따라 모아 계산하여 열별 결과

dim=1
→ 열 방향을 따라 모아 계산하여 행별 결과
```

라고 이해할 수 있다.

---

# 47. Classification에서는 왜 dim=1?

Logits Shape가:

```text
[Batch, Class]
```

라면 각 Sample마다 Class 중 가장 큰 값을 찾아야 한다.

따라서:

```python
argmax(
    dim=1
)
```

을 사용한다.

---

# 48. reshape

Tensor Shape를 바꿀 수 있다.

```python
x = torch.arange(
    12
)


print(
    x.shape
)
```

결과:

```text
[12]
```

---

# 49. 3 × 4로 변경

```python
x = x.reshape(
    3,
    4
)


print(
    x
)


print(
    x.shape
)
```

---

# 50. 값 개수는 같아야 한다

12개의 값을:

```text
3 × 4
```

로 바꾸는 것은 가능하다.

하지만:

```text
5 × 4
=
20
```

으로 바꾸려고 하면 값 개수가 맞지 않아 오류가 발생한다.

---

# 51. -1 사용

```python
x = torch.arange(
    12
)


x = x.reshape(
    3,
    -1
)
```

PyTorch가 나머지 Dimension을 자동 계산한다.

결과:

```text
[3, 4]
```

이다.

---

# 52. view

```python
x = x.view(
    3,
    4
)
```

처럼 `view()`도 Shape 변경에 자주 사용된다.

---

# 53. reshape와 view

둘 다 Shape를 변경할 때 사용한다.

하지만 내부 Memory 배치가 연속적인지 여부에 따라 `view()`가 바로 동작하지 않는 경우가 있다.

처음에는:

```text
reshape()
→ 비교적 편하게 Shape 변경

view()
→ PyTorch 코드에서 자주 보이는 Shape 변경
```

정도로 이해해도 된다.

---

# 54. contiguous

Tensor의 Memory 배치가 연속적이지 않은 경우:

```python
x = x.contiguous()
```

를 사용한 뒤 `view()`를 사용하는 코드를 볼 수 있다.

---

# 55. Flatten

여러 Dimension을 펼친다.

```python
x = torch.randn(
    2,
    3,
    4
)


print(
    x.shape
)
```

```python
flat = torch.flatten(
    x
)


print(
    flat.shape
)
```

---

# 56. Batch를 유지하면서 Flatten

Deep Learning에서는 Batch Dimension을 유지하는 것이 중요하다.

예:

```text
[32, 1, 28, 28]
```

을:

```text
[32, 784]
```

로 만들고 싶다.

---

# 57. 방법

```python
x = x.reshape(
    x.size(0),
    -1
)
```

---

# 58. x.size(0)

```python
x.size(
    0
)
```

은 첫 번째 Dimension 크기이다.

일반적인 이미지 Batch에서는:

```text
Batch Size
```

이다.

---

# 59. nn.Flatten

Model 안에서는:

```python
nn.Flatten()
```

을 사용할 수 있다.

예:

```python
model = nn.Sequential(

    nn.Flatten(),

    nn.Linear(
        28 * 28,
        128
    )
)
```

---

# 60. unsqueeze

Dimension 하나를 추가한다.

```python
x = torch.tensor(
    [
        1,
        2,
        3
    ]
)


print(
    x.shape
)
```

결과:

```text
[3]
```

---

# 61. Dimension 추가

```python
x = x.unsqueeze(
    0
)


print(
    x.shape
)
```

결과:

```text
[1, 3]
```

이다.

---

# 62. 왜 unsqueeze를 사용할까?

Model이 Batch Dimension을 기대하는 경우가 있다.

원래:

```text
[Feature]
```

를:

```text
[Batch, Feature]
```

로 만들어야 할 수 있다.

예:

```text
[3]
↓
unsqueeze(0)
↓
[1, 3]
```

이다.

---

# 63. 이미지 한 장

이미지 한 장의 Shape가:

```text
[C, H, W]
```

라고 하자.

Model은 보통:

```text
[B, C, H, W]
```

를 기대한다.

---

# 64. Batch Dimension 추가

```python
image = image.unsqueeze(
    0
)
```

하면:

```text
[C, H, W]
↓
[1, C, H, W]
```

가 된다.

---

# 65. squeeze

크기가 1인 Dimension을 제거한다.

```python
x = torch.randn(
    1,
    3
)


print(
    x.shape
)
```

```python
x = x.squeeze(
    0
)


print(
    x.shape
)
```

결과:

```text
[3]
```

이다.

---

# 66. squeeze 주의

무조건:

```python
x.squeeze()
```

를 사용하면 크기가 1인 모든 Dimension이 제거될 수 있다.

Batch Size가 1인 경우 예상하지 못하게 Batch Dimension까지 사라질 수 있다.

따라서 필요한 Dimension을 명확히 지정하는 것이 안전한 경우가 많다.

---

# 67. transpose

Dimension 위치를 바꿀 수 있다.

```python
x = torch.randn(
    2,
    3
)


y = x.transpose(
    0,
    1
)


print(
    y.shape
)
```

결과:

```text
[3, 2]
```

이다.

---

# 68. permute

여러 Dimension 순서를 한 번에 바꾼다.

예:

```python
x = torch.randn(
    3,
    224,
    224
)
```

현재:

```text
[C, H, W]
```

이다.

---

# 69. HWC로 변경

```python
x_hwc = x.permute(
    1,
    2,
    0
)
```

결과:

```text
[224, 224, 3]
```

즉:

```text
[H, W, C]
```

이다.

---

# 70. 왜 필요한가?

PyTorch는 이미지를 보통:

```text
[C, H, W]
```

로 다룬다.

하지만 Matplotlib 등에서는:

```text
[H, W, C]
```

형태를 사용하는 경우가 많다.

그래서 시각화할 때 `permute()`를 자주 볼 수 있다.

---

# 71. NumPy → Tensor

```python
import numpy as np


arr = np.array(
    [
        1,
        2,
        3
    ]
)


x = torch.from_numpy(
    arr
)


print(
    x
)
```

---

# 72. torch.tensor로도 가능

```python
x = torch.tensor(
    arr
)
```

도 가능하다.

---

# 73. from_numpy와 tensor 차이

`torch.from_numpy()`로 만든 Tensor는 CPU에서 NumPy Array와 Memory를 공유하는 경우가 있다.

따라서 한쪽 값을 수정했을 때 다른 쪽에도 영향을 줄 수 있다.

반면 `torch.tensor()`는 일반적으로 새 Tensor로 복사하여 생성한다.

---

# 74. Tensor → NumPy

CPU Tensor라면:

```python
arr = x.numpy()
```

로 변환할 수 있다.

---

# 75. GPU Tensor는 바로 NumPy로 바꿀 수 없다

GPU에 있는 Tensor에:

```python
x.numpy()
```

를 바로 사용하면 문제가 발생한다.

먼저 CPU로 이동해야 한다.

---

# 76. GPU Tensor → NumPy

```python
arr = (
    x
    .detach()
    .cpu()
    .numpy()
)
```

Deep Learning 코드에서 매우 자주 보인다.

---

# 77. 하나씩 해석

```python
x.detach()
```

Autograd 계산 Graph에서 분리한다.

```python
.cpu()
```

CPU로 이동한다.

```python
.numpy()
```

NumPy Array로 변환한다.

---

# 78. item()

값이 하나인 Tensor를 Python 숫자로 바꾸려면:

```python
value = x.item()
```

을 사용할 수 있다.

---

# 79. Loss에서 자주 사용

```python
loss_value = loss.item()
```

처럼 사용한다.

왜냐하면 `loss` 자체는 Tensor이기 때문이다.

---

# 80. 잘못된 item 사용

여러 값을 가진 Tensor에는:

```python
x.item()
```

을 사용할 수 없다.

`item()`은 하나의 값만 가진 Tensor에 사용한다.

---

# 81. Tensor 복사

```python
y = x.clone()
```

을 사용할 수 있다.

---

# 82. detach

```python
y = x.detach()
```

는 Gradient 계산 Graph에서 Tensor를 분리한다.

---

# 83. clone과 detach는 목적이 다르다

```text
clone()
→ Tensor 값을 복사

detach()
→ Autograd Graph에서 분리
```

이다.

필요하면:

```python
y = x.detach().clone()
```

처럼 함께 사용할 수도 있다.

---

# 84. requires_grad

PyTorch는 자동 미분 기능을 제공한다.

Tensor 생성 시:

```python
x = torch.tensor(
    2.0,
    requires_grad=True
)
```

처럼 지정할 수 있다.

---

# 85. 계산

```python
y = x ** 2
```

수식:

```text
y = x²
```

이다.

---

# 86. Backward

```python
y.backward()
```

를 실행하면 Gradient를 계산한다.

---

# 87. Gradient 확인

```python
print(
    x.grad
)
```

`x = 2`라면:

```text
dy/dx = 2x = 4
```

이므로 Gradient가 4가 된다.

---

# 88. 이것이 Backpropagation의 기초

실제 Neural Network에서는:

```text
수많은 Weight Tensor
```

가 있고 PyTorch가 자동으로 Gradient를 계산한다.

이 기능을:

```text
Autograd
```

라고 한다.

---

# 89. Model Parameter

예:

```python
import torch.nn as nn


layer = nn.Linear(
    3,
    2
)
```

이 Layer 내부에는:

```text
Weight Tensor
Bias Tensor
```

가 존재한다.

---

# 90. Parameter 확인

```python
for name, param in layer.named_parameters():

    print(
        name,
        param.shape
    )
```

예:

```text
weight torch.Size([2, 3])
bias torch.Size([2])
```

---

# 91. 왜 Weight Shape가 [2, 3]인가?

```python
nn.Linear(
    3,
    2
)
```

은:

```text
Input Features = 3

Output Features = 2
```

이다.

따라서 Weight Matrix는 PyTorch에서:

```text
[Output Features, Input Features]

[2, 3]
```

형태를 가진다.

---

# 92. Linear Input Shape

Input:

```text
[Batch, Input Features]
```

예:

```text
[32, 3]
```

Layer:

```python
nn.Linear(
    3,
    2
)
```

Output:

```text
[32, 2]
```

가 된다.

---

# 93. 핵심 Shape 흐름

```text
Input
[32, 3]

↓

Linear(3, 2)

↓

Output
[32, 2]
```

이다.

Batch Size 32는 그대로 유지되고 Feature Dimension이 3에서 2로 바뀐다.

---

# 94. Shape Error

예:

```text
Input
[32, 3]
```

인데:

```python
nn.Linear(
    12,
    100
)
```

을 사용하면 문제가 생긴다.

---

# 95. 왜?

`nn.Linear(12, 100)`은:

```text
마지막 Dimension = 12
```

를 기대한다.

하지만 실제 Input:

```text
마지막 Dimension = 3
```

이다.

---

# 96. 대표 오류

```text
RuntimeError:
mat1 and mat2 shapes cannot be multiplied
(32x3 and 12x100)
```

같은 오류가 발생할 수 있다.

---

# 97. 이 오류 읽는 방법

```text
32 × 3
```

은 현재 Input Matrix의 Shape와 관련되어 있다.

```text
12 × 100
```

은 Linear 연산에서 사용되는 Weight Matrix 방향과 관련된 정보이다.

핵심은:

```text
3과 12가 맞지 않는다.
```

는 것이다.

---

# 98. 행렬 곱 조건

행렬 곱:

```text
A × B
```

에서:

```text
A의 열 수
=
B의 행 수
```

여야 한다.

예:

```text
[32 × 3]
×
[3 × 100]

→ 가능
```

결과:

```text
[32 × 100]
```

이다.

---

# 99. 하지만

```text
[32 × 3]
×
[12 × 100]
```

은:

```text
3 ≠ 12
```

이므로 불가능하다.

---

# 100. 해결 방법 1

실제 Input Feature가 3개라면:

```python
nn.Linear(
    3,
    100
)
```

으로 수정할 수 있다.

---

# 101. 해결 방법 2

원래 Model이 Feature 12개를 기대하는 것이 맞다면 Input Data를 왜 3개 Feature로 만들었는지 확인해야 한다.

즉 무조건 Model 숫자를 바꾸는 것이 아니라:

```text
Input Shape가 잘못됐는가?

Model 구조가 잘못됐는가?
```

를 먼저 판단한다.

---

# 102. Shape 오류가 나면 가장 먼저 할 것

```python
print(
    x.shape
)


print(
    y.shape
)


print(
    model
)
```

이다.

---

# 103. Layer 중간 Shape 확인

복잡한 Model에서는 중간 Shape를 출력해볼 수 있다.

예:

```python
print(
    "input:",
    x.shape
)


x = layer1(
    x
)


print(
    "layer1:",
    x.shape
)
```

---

# 104. 이미지 Shape

PyTorch 이미지 Batch의 대표 Shape:

```text
[B, C, H, W]
```

이다.

---

# 105. 예

```text
[32, 3, 224, 224]
```

해석:

```text
32
→ Batch

3
→ Channel

224
→ Height

224
→ Width
```

이다.

---

# 106. 이 Shape를 읽는 습관

```python
print(
    images.shape
)
```

결과:

```text
torch.Size([32, 3, 224, 224])
```

이면:

```text
이미지 32장

RGB

224 × 224
```

라고 바로 읽을 수 있어야 한다.

---

# 107. 흑백 이미지

```text
[32, 1, 224, 224]
```

이면:

```text
이미지 32장

Grayscale

224 × 224
```

이다.

---

# 108. Batch 한 개 꺼내보기

```python
images, labels = next(
    iter(
        train_loader
    )
)


print(
    images.shape
)


print(
    labels.shape
)
```

---

# 109. Classification Label Shape

Batch Size가 32라면:

```text
images
[32, 3, 224, 224]

labels
[32]
```

형태가 흔하다.

---

# 110. Model Output Shape

Binary Classification을 2개의 Logit으로 처리한다면:

```text
Input
[32, 3, 224, 224]

↓

Model

↓

Output
[32, 2]
```

가 될 수 있다.

---

# 111. Multi-class

10개 Class:

```text
Output
[32, 10]
```

이다.

---

# 112. CrossEntropyLoss Shape

일반적인 Multi-class Classification에서:

```text
Output
[Batch, Classes]

Target
[Batch]
```

형태를 사용한다.

예:

```text
Output
[32, 10]

Target
[32]
```

이다.

---

# 113. Binary Classification의 다른 방식

Binary Classification에서는 모델 Output을:

```text
[Batch, 1]
```

또는:

```text
[Batch]
```

로 만들고:

```python
nn.BCEWithLogitsLoss()
```

를 사용하는 방식도 있다.

이 경우 Target dtype과 shape를 Loss 요구사항에 맞춰야 한다.

---

# 114. CrossEntropyLoss와 BCEWithLogitsLoss

대표적으로:

```text
CrossEntropyLoss
→ Multi-class 또는 2-logit Binary Classification

BCEWithLogitsLoss
→ 1-logit Binary Classification 등에 사용
```

할 수 있다.

둘은 Output Shape와 Target 형식이 다르므로 혼동하면 안 된다.

---

# 115. Device 확인

Tensor가 어디에 있는지:

```python
print(
    x.device
)
```

로 확인할 수 있다.

---

# 116. CPU Tensor

예:

```text
cpu
```

---

# 117. GPU Tensor

예:

```text
cuda:0
```

이다.

---

# 118. Device 설정

```python
device = torch.device(
    "cuda"
    if torch.cuda.is_available()
    else "cpu"
)
```

---

# 119. Tensor 이동

```python
x = x.to(
    device
)
```

---

# 120. Model도 이동

```python
model = model.to(
    device
)
```

---

# 121. Model과 Tensor는 같은 Device에

올바른 구조:

```text
Model
→ cuda

Input
→ cuda

Target
→ cuda
```

이다.

---

# 122. 잘못된 구조

```text
Model
→ cuda

Input
→ cpu
```

이면 Device mismatch 오류가 발생할 수 있다.

---

# 123. Training Loop에서

보통:

```python
for x, y in train_loader:

    x = x.to(
        device
    )

    y = y.to(
        device
    )

    output = model(
        x
    )
```

처럼 사용한다.

---

# 124. Tensor 생성 Device 지정

처음부터:

```python
x = torch.tensor(
    [
        1.0,
        2.0
    ],
    device=device
)
```

처럼 만들 수도 있다.

---

# 125. GPU 이름 확인

CUDA가 가능하다면:

```python
print(
    torch.cuda.get_device_name(
        0
    )
)
```

으로 GPU 이름을 확인할 수 있다.

---

# 126. CUDA 사용 가능 여부

```python
print(
    torch.cuda.is_available()
)
```

---

# 127. CUDA가 False라면?

가능한 원인:

```text
NVIDIA GPU가 없음

GPU Driver 문제

CUDA를 지원하지 않는 PyTorch 설치

가상환경이 다름

Jupyter Kernel이 다른 Python 환경 사용
```

등이 있다.

---

# 128. Tensor와 NumPy 차이

공통점:

```text
다차원 배열

Indexing

Slicing

수학 연산

Shape
```

을 지원한다.

Tensor의 중요한 추가 기능:

```text
GPU 연산

Autograd

Deep Learning Model과 직접 연결
```

이다.

---

# 129. Pandas와 Tensor 차이

Pandas DataFrame은:

```text
표 형태 데이터 분석
```

에 매우 편리하다.

Tensor는:

```text
수치 연산
Deep Learning
GPU 계산
```

에 적합하다.

---

# 130. 일반적인 흐름

Tabular Data:

```text
CSV
↓
Pandas DataFrame
↓
Preprocessing
↓
NumPy / Tensor
↓
Deep Learning Model
```

이다.

---

# 131. Image Data

```text
Image File
↓
PIL Image
↓
Transform
↓
Tensor
↓
CNN
```

형태를 많이 사용한다.

---

# 132. Tensor 값 범위도 중요

Image Pixel이:

```text
0 ~ 255
```

일 수도 있고 Transform 후:

```text
0 ~ 1
```

이 될 수도 있다.

Normalization을 적용하면 또 다른 범위를 가질 수 있다.

따라서 Shape뿐 아니라:

```text
dtype

min

max

mean
```

도 확인하면 좋다.

---

# 133. Tensor 검사

```python
print(
    x.shape
)

print(
    x.dtype
)

print(
    x.device
)

print(
    x.min()
)

print(
    x.max()
)
```

---

# 134. Image Batch 검사

```python
images, labels = next(
    iter(
        train_loader
    )
)


print(
    "images shape:",
    images.shape
)

print(
    "labels shape:",
    labels.shape
)

print(
    "dtype:",
    images.dtype
)

print(
    "min:",
    images.min().item()
)

print(
    "max:",
    images.max().item()
)
```

---

# 135. 왜 이것을 먼저 확인할까?

Deep Learning 오류의 많은 부분은:

```text
Shape

dtype

Device

Value Range
```

문제에서 발생하기 때문이다.

---

# 136. NaN 확인

```python
print(
    torch.isnan(
        x
    ).sum()
)
```

Float Tensor에서 NaN이 있는지 확인할 수 있다.

---

# 137. Inf 확인

```python
print(
    torch.isinf(
        x
    ).sum()
)
```

---

# 138. Training이 이상하면

예:

```text
Loss = nan

Accuracy가 이상함

Model이 학습되지 않음
```

이라면 Tensor의:

```text
Shape

dtype

min/max

NaN

Inf
```

를 확인한다.

---

# 139. Tensor Debugging 기본

```python
print(
    "shape:",
    x.shape
)

print(
    "dtype:",
    x.dtype
)

print(
    "device:",
    x.device
)

print(
    "min:",
    x.min().item()
)

print(
    "max:",
    x.max().item()
)

print(
    "nan:",
    torch.isnan(
        x
    ).sum().item()
)
```

---

# 140. Label도 확인

```python
print(
    y.shape
)

print(
    y.dtype
)

print(
    torch.unique(
        y
    )
)
```

Classification에서 실제 Label이:

```text
0, 1
```

인지:

```text
0 ~ 9
```

인지 확인할 수 있다.

---

# 141. Class 수 확인

```python
print(
    torch.unique(
        y,
        return_counts=True
    )
)
```

Class별 Sample 수를 확인할 수도 있다.

---

# 142. Tensor Shape를 읽는 순서

예:

```text
[64, 3, 224, 224]
```

무작정 숫자 4개라고 보지 않는다.

```text
64
→ 몇 개?

3
→ 몇 Channel?

224
→ 높이?

224
→ 너비?
```

라고 의미를 붙인다.

---

# 143. Tabular Tensor

Shape:

```text
[1000, 18]
```

이면 보통:

```text
1000 Samples

18 Features
```

라고 읽을 수 있다.

---

# 144. Sequence Data

예:

```text
[32, 100, 128]
```

은 모델에 따라:

```text
32 Batch

100 Sequence Length

128 Features
```

처럼 해석할 수 있다.

항상 데이터와 Model의 Dimension 정의를 확인해야 한다.

---

# 145. Shape 숫자 자체보다 의미가 중요

같은:

```text
[32, 3, 224, 224]
```

를 보더라도:

```text
32 = Batch

3 = Channel

224 = Height

224 = Width
```

라고 의미를 연결해야 한다.

---

# 146. Shape 추적

Model을 만들 때 다음처럼 Shape 흐름을 적어두면 좋다.

```text
Input
[32, 784]

↓

Linear(784, 128)

↓

[32, 128]

↓

ReLU

↓

[32, 128]

↓

Linear(128, 10)

↓

[32, 10]
```

---

# 147. ReLU는 Shape를 바꾸지 않는다

```text
[32, 128]

↓

ReLU

↓

[32, 128]
```

값은 바뀌지만 Shape는 그대로이다.

---

# 148. Linear는 마지막 Feature Dimension을 바꾼다

```text
[32, 128]

↓

Linear(128, 10)

↓

[32, 10]
```

이다.

---

# 149. Flatten은 Shape를 크게 바꿀 수 있다

```text
[32, 1, 28, 28]

↓

Flatten

↓

[32, 784]
```

이다.

---

# 150. CNN에서는?

CNN에서는:

```text
Channel

Height

Width
```

가 Layer를 지나며 바뀔 수 있다.

이 부분은 Computer Vision Chapter에서 더 자세히 다룬다.

---

# 151. Tensor 관련 자주 보는 오류

```text
mat1 and mat2 shapes cannot be multiplied

Expected all tensors to be on the same device

expected scalar type Long but found Float

expected scalar type Float but found Long

Target size must be the same as input size

IndexError: Target ... is out of bounds
```

등이 있다.

---

# 152. Shape Error

```text
mat1 and mat2 shapes cannot be multiplied
```

→ Linear Layer Input Feature 수 확인.

---

# 153. Device Error

```text
Expected all tensors to be on the same device
```

→ Model / Input / Target Device 확인.

---

# 154. dtype Error

```text
expected scalar type ...
```

→ Input과 Target dtype 확인.

---

# 155. Target Shape Error

```text
Target size must be the same as input size
```

→ 사용 중인 Loss Function이 요구하는 Input/Target Shape 확인.

---

# 156. Target Class Error

```text
IndexError:
Target ... is out of bounds
```

예를 들어 Output Class가 2개인데 Label에:

```text
2
```

가 존재하면 문제가 될 수 있다.

2 Class의 일반적인 Label Index는:

```text
0
1
```

이다.

---

# 157. Debugging 순서

Deep Learning Error가 발생하면:

```text
1. Error Message 읽기

2. Input Shape 출력

3. Target Shape 출력

4. dtype 출력

5. Device 출력

6. Model 구조 출력

7. Output Shape 출력

8. Loss Function 요구사항 확인
```

순서로 보면 좋다.

---

# 158. 한 Batch로 먼저 테스트

전체 Training을 바로 돌리기 전에:

```python
images, labels = next(
    iter(
        train_loader
    )
)
```

한 Batch를 가져온다.

---

# 159. Device 이동

```python
images = images.to(
    device
)


labels = labels.to(
    device
)
```

---

# 160. Forward 테스트

```python
outputs = model(
    images
)


print(
    "input:",
    images.shape
)


print(
    "output:",
    outputs.shape
)


print(
    "target:",
    labels.shape
)
```

---

# 161. 왜 이 방법이 좋은가?

몇 시간짜리 Training을 시작하기 전에:

```text
DataLoader가 정상인가?

Shape가 맞는가?

Model Forward가 되는가?

Output Class 수가 맞는가?
```

를 빠르게 확인할 수 있기 때문이다.

---

# 162. Loss까지 테스트

```python
loss = loss_fn(
    outputs,
    labels
)


print(
    loss.item()
)
```

여기까지 정상적으로 실행되면 기본적인 Input → Model → Loss 연결은 된 것이다.

---

# 163. 그다음 Backward

```python
optimizer.zero_grad()

loss.backward()

optimizer.step()
```

까지 한 Batch로 테스트할 수 있다.

---

# 164. Deep Learning 실행 전 Smoke Test

본격적인 Training 전에:

```text
1 Batch 가져오기

↓

Shape 확인

↓

Device 이동

↓

Forward

↓

Output Shape 확인

↓

Loss 계산

↓

Backward

↓

Optimizer Step
```

을 먼저 해보는 습관이 좋다.

---

# 165. 핵심 Tensor 속성

```text
x.shape
→ Shape

x.ndim
→ Dimension 수

x.dtype
→ Data Type

x.device
→ CPU / GPU

x.numel()
→ 전체 값 개수
```

---

# 166. 핵심 Tensor 변환

```text
reshape()
→ Shape 변경

view()
→ Shape 변경

flatten()
→ 펼치기

unsqueeze()
→ Dimension 추가

squeeze()
→ 크기 1 Dimension 제거

transpose()
→ 두 Dimension 교환

permute()
→ Dimension 순서 변경
```

---

# 167. 핵심 dtype 변환

```text
x.float()
→ float32

x.long()
→ int64

x.to(dtype)
→ 원하는 dtype
```

---

# 168. 핵심 Device 변환

```text
x.to(device)
→ 지정 Device로 이동

x.cpu()
→ CPU 이동
```

---

# 169. 핵심 NumPy 변환

```text
NumPy → Tensor

torch.from_numpy(arr)
```

```text
Tensor → NumPy

tensor.detach().cpu().numpy()
```

---

# 170. 가장 중요한 이미지 Shape

```text
[B, C, H, W]
```

이다.

예:

```text
[32, 3, 224, 224]
```

는:

```text
Batch = 32

Channel = 3

Height = 224

Width = 224
```

이다.

---

# 171. 가장 중요한 Linear Shape

```text
Input
[Batch, Input Features]

↓

Linear(Input Features, Output Features)

↓

Output
[Batch, Output Features]
```

이다.

---

# 172. 예

```text
[32, 12]

↓

Linear(12, 100)

↓

[32, 100]
```

은 정상이다.

---

# 173. 반면

```text
[32, 3]

↓

Linear(12, 100)
```

은:

```text
3 ≠ 12
```

이므로 문제가 된다.

---

# 174. Tensor를 볼 때 항상 물어볼 것

```text
이 Tensor는 무엇을 의미하는가?

첫 번째 Dimension은 무엇인가?

마지막 Dimension은 무엇인가?

dtype은 무엇인가?

CPU인가 GPU인가?

Model이 기대하는 Shape와 같은가?
```

이다.

---

# 175. Deep Learning에서 Tensor의 위치

```text
Raw Data
↓
Tensor
↓
DataLoader
↓
Model
↓
Output Tensor
↓
Loss Tensor
↓
Backward
↓
Gradient Tensor
↓
Weight Update
```

Tensor는 전체 Deep Learning 흐름의 중심에 있다.

---

# 176. 최종 정리

Tensor는 PyTorch에서 사용하는 다차원 배열이다.

하지만 단순한 배열을 넘어:

```text
GPU 연산

자동 미분

Neural Network

Loss 계산

Backpropagation
```

과 연결된다.

Tensor를 볼 때 가장 먼저 확인할 것은:

```text
Shape

dtype

Device
```

이다.

특히 Deep Learning Error가 발생하면:

```python
print(
    x.shape
)

print(
    x.dtype
)

print(
    x.device
)
```

부터 확인한다.

이미지에서는:

```text
[B, C, H, W]
```

를 기억한다.

Linear Layer에서는:

```text
[Batch, Input Features]

↓

Linear(Input Features, Output Features)

↓

[Batch, Output Features]
```

를 기억한다.

그리고:

```text
mat1 and mat2 shapes cannot be multiplied
```

오류를 보면 가장 먼저:

```text
실제 Input Feature 수

vs

Linear Layer가 기대하는 Input Feature 수
```

를 비교한다.

Tensor Shape를 읽을 수 있게 되면 PyTorch 오류의 상당 부분을 훨씬 쉽게 이해할 수 있다.