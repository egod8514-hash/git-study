# Neural Network

## 1. Neural Network란?

Neural Network는 여러 개의 Layer를 연결하여 입력 데이터를 원하는 출력으로 변환하는 Model이다.

기본 구조:

```text
Input
  ↓
Hidden Layer
  ↓
Hidden Layer
  ↓
Output
```

예를 들어 환자 데이터를 이용해 질병 여부를 예측한다면:

```text
Age
BMI
Blood Pressure
Glucose
        ↓
Neural Network
        ↓
Disease / Normal
```

과 같은 구조를 만들 수 있다.

---

# 2. Neural Network의 핵심

Neural Network에서는 입력값을 여러 Layer에 통과시킨다.

```text
Input
↓
Linear
↓
Activation
↓
Linear
↓
Activation
↓
Linear
↓
Output
```

각 Layer를 통과하면서 데이터가 새로운 표현으로 변환된다.

---

# 3. Layer란?

Layer는 입력 데이터를 받아 계산한 뒤 새로운 값을 출력하는 단계이다.

PyTorch에서는 대표적으로:

```text
nn.Linear
nn.ReLU
nn.Conv2d
nn.Dropout
nn.BatchNorm
```

등을 사용할 수 있다.

---

# 4. 가장 기본적인 Layer

가장 먼저 이해해야 할 것은:

```python
nn.Linear()
```

이다.

Linear Layer는 개념적으로:

```text
y = Wx + b
```

계산을 수행한다.

---

# 5. Weight와 Bias

수식:

```text
y = Wx + b
```

에서:

```text
W
→ Weight

b
→ Bias

x
→ Input

y
→ Output
```

이다.

Weight와 Bias는 Training 과정에서 학습되는 Parameter이다.

---

# 6. Linear Layer 만들기

```python
import torch.nn as nn


layer = nn.Linear(
    4,
    10
)
```

의미:

```text
Input Feature = 4

Output Feature = 10
```

이다.

---

# 7. Shape으로 이해

Input:

```text
[Batch, 4]
```

Linear:

```text
Linear(4, 10)
```

Output:

```text
[Batch, 10]
```

이다.

예:

```text
[32, 4]
↓
Linear(4, 10)
↓
[32, 10]
```

---

# 8. Batch Size는 유지된다

```text
Input
[32, 4]

↓

Linear(4, 10)

↓

Output
[32, 10]
```

여기서:

```text
32
```

는 Batch Size이다.

Linear Layer는 Feature Dimension을:

```text
4 → 10
```

으로 바꾼다.

---

# 9. Weight Shape

```python
layer = nn.Linear(
    4,
    10
)
```

이라면 Weight Shape는:

```text
[10, 4]
```

이다.

PyTorch Linear Weight는:

```text
[Output Features, Input Features]
```

형태이다.

---

# 10. Bias Shape

Bias는 Output Feature마다 하나씩 있으므로:

```text
[10]
```

이다.

---

# 11. 확인

```python
print(
    layer.weight.shape
)


print(
    layer.bias.shape
)
```

결과:

```text
torch.Size([10, 4])

torch.Size([10])
```

이다.

---

# 12. Parameter란?

Model이 Training 과정에서 학습하는 값을:

```text
Parameter
```

라고 한다.

대표적으로:

```text
Weight
Bias
```

가 있다.

---

# 13. Parameter 확인

```python
for name, param in layer.named_parameters():

    print(
        name,
        param.shape
    )
```

결과:

```text
weight torch.Size([10, 4])

bias torch.Size([10])
```

---

# 14. model.parameters()

Optimizer를 만들 때 자주 사용한다.

```python
optimizer = torch.optim.Adam(
    model.parameters(),
    lr=0.001
)
```

여기서:

```python
model.parameters()
```

는 Model이 학습해야 하는 Parameter들을 Optimizer에 전달한다.

---

# 15. Neuron

Output Feature가 10이라면 개념적으로 10개의 Unit이 출력된다고 생각할 수 있다.

```text
4 Input Features
      ↓
Linear Layer
      ↓
10 Output Units
```

각 Output Unit은 Input Feature들을 Weight와 결합해 값을 계산한다.

---

# 16. Hidden Layer

Input과 Output 사이에 있는 Layer를:

```text
Hidden Layer
은닉층
```

이라고 한다.

예:

```text
Input
  ↓
Hidden Layer 1
  ↓
Hidden Layer 2
  ↓
Output
```

---

# 17. 가장 간단한 신경망

```python
model = nn.Sequential(
    nn.Linear(4, 10),
    nn.ReLU(),
    nn.Linear(10, 2)
)
```

---

# 18. 구조 읽기

위 코드는:

```text
4 Features
↓
Linear
4 → 10
↓
ReLU
↓
Linear
10 → 2
↓
2 Outputs
```

이다.

---

# 19. Shape 추적

Batch Size가 32라면:

```text
Input
[32, 4]

↓

Linear(4, 10)

↓

[32, 10]

↓

ReLU

↓

[32, 10]

↓

Linear(10, 2)

↓

[32, 2]
```

이다.

---

# 20. ReLU는 Shape를 바꾸지 않는다

```text
[32, 10]

↓

ReLU

↓

[32, 10]
```

Shape는 그대로이고 값만 변환된다.

---

# 21. nn.Sequential

```python
nn.Sequential()
```

은 여러 Layer를 순서대로 실행하는 편리한 방법이다.

예:

```python
model = nn.Sequential(
    nn.Linear(4, 16),
    nn.ReLU(),
    nn.Linear(16, 8),
    nn.ReLU(),
    nn.Linear(8, 2)
)
```

---

# 22. Sequential 흐름

```text
Input 4
↓
Linear
4 → 16
↓
ReLU
↓
Linear
16 → 8
↓
ReLU
↓
Linear
8 → 2
↓
Output
```

이다.

---

# 23. Shape 추적 습관

신경망 코드를 볼 때는 항상:

```text
입력 Shape

↓

각 Layer

↓

출력 Shape
```

를 적어본다.

예:

```text
[32, 4]
↓
[32, 16]
↓
[32, 16]
↓
[32, 8]
↓
[32, 8]
↓
[32, 2]
```

---

# 24. 왜 Activation Function이 필요한가?

Linear Layer만 계속 연결하면 복잡한 비선형 관계를 표현하는 능력이 제한된다.

예:

```text
Linear
↓
Linear
↓
Linear
```

보다:

```text
Linear
↓
ReLU
↓
Linear
↓
ReLU
↓
Linear
```

처럼 비선형 Activation을 사이에 넣는다.

---

# 25. 대표 Activation

```text
ReLU
Sigmoid
Tanh
Softmax
```

등이 있다.

각각의 역할은 다음 파일에서 더 자세히 다룬다.

---

# 26. nn.Module

PyTorch에서 Neural Network를 직접 정의할 때 가장 중요한 Class가:

```python
nn.Module
```

이다.

Custom Model은 보통 `nn.Module`을 상속한다.

---

# 27. 기본 구조

```python
class MyModel(nn.Module):

    def __init__(
        self
    ):

        super().__init__()

    def forward(
        self,
        x
    ):

        return x
```

---

# 28. 처음 보면 어려운 부분

```python
class MyModel(nn.Module):
```

의 의미:

```text
MyModel이라는 새로운 Model Class를 만들고

PyTorch의 nn.Module 기능을 상속받는다.
```

이다.

---

# 29. __init__

```python
def __init__(
    self
):
```

Model을 처음 만들 때 사용할 Layer들을 정의한다.

---

# 30. super().__init__()

```python
super().__init__()
```

은 부모 Class인 `nn.Module`의 초기화 기능을 실행한다.

Custom PyTorch Model에서는 기본적으로 필요하다고 생각하면 된다.

---

# 31. Layer 정의

```python
self.fc1 = nn.Linear(
    4,
    10
)


self.fc2 = nn.Linear(
    10,
    2
)
```

처럼 Model이 사용할 Layer를 저장한다.

---

# 32. forward()

```python
def forward(
    self,
    x
):
```

는 Input이 Model 내부에서 어떤 순서로 계산되는지 정의한다.

---

# 33. Custom Model 예

```python
import torch.nn as nn


class MyModel(nn.Module):

    def __init__(
        self
    ):

        super().__init__()

        self.fc1 = nn.Linear(
            4,
            10
        )

        self.relu = nn.ReLU()

        self.fc2 = nn.Linear(
            10,
            2
        )

    def forward(
        self,
        x
    ):

        x = self.fc1(
            x
        )

        x = self.relu(
            x
        )

        x = self.fc2(
            x
        )

        return x
```

---

# 34. Model 생성

```python
model = MyModel()
```

---

# 35. Model 출력

```python
print(
    model
)
```

결과는 대략:

```text
MyModel(
  (fc1): Linear(in_features=4, out_features=10, bias=True)
  (relu): ReLU()
  (fc2): Linear(in_features=10, out_features=2, bias=True)
)
```

이다.

---

# 36. Model 호출

Input:

```python
import torch


x = torch.randn(
    32,
    4
)
```

Model:

```python
output = model(
    x
)
```

---

# 37. 내부적으로 무슨 일이 일어날까?

```python
model(
    x
)
```

를 호출하면 PyTorch가 내부적으로 `forward()`를 사용해 계산한다.

즉 개념적으로:

```text
model(x)

↓

forward(x)

↓

fc1

↓

ReLU

↓

fc2

↓

return output
```

이다.

---

# 38. forward를 직접 호출하지 않는 이유

일반적으로:

```python
model(
    x
)
```

형태를 사용한다.

다음처럼 직접:

```python
model.forward(
    x
)
```

를 호출하는 것은 권장되는 일반 사용 방식이 아니다.

`model(x)`를 사용하면 PyTorch Module의 Hook 등 관련 기능이 정상적으로 처리된다.

---

# 39. nn.Sequential vs nn.Module

간단한 순차 구조라면:

```python
nn.Sequential
```

이 편리하다.

복잡한 Model이라면:

```python
nn.Module
```

을 직접 상속해서 `forward()`를 작성하는 방식이 유연하다.

---

# 40. Sequential이 적합한 경우

```text
Input
↓
Layer
↓
Layer
↓
Layer
↓
Output
```

처럼 한 방향으로 단순하게 흐르는 구조이다.

---

# 41. Custom nn.Module이 필요한 경우

예:

```text
여러 Input 사용

중간 결과 재사용

조건문

Residual Connection

여러 Branch

Image + Tabular Data 결합
```

등 복잡한 구조에서는 직접 `forward()`를 작성하는 것이 편리하다.

---

# 42. 예: 여러 Branch

개념적으로:

```text
          ┌→ Branch A ─┐
Input ────┤            ├→ Combine → Output
          └→ Branch B ─┘
```

같은 구조이다.

이런 경우 `nn.Sequential`만으로 표현하기 불편할 수 있다.

---

# 43. Forward Pass

Input이 Model을 통과하여 Output을 만드는 과정을:

```text
Forward Pass
```

라고 한다.

예:

```text
Input
↓
fc1
↓
ReLU
↓
fc2
↓
Output
```

---

# 44. Forward와 Training은 다르다

Forward는:

```text
Prediction 계산
```

이다.

Training은 Forward뿐 아니라:

```text
Forward
↓
Loss
↓
Backward
↓
Optimizer Step
```

까지 포함한다.

---

# 45. Model은 Prediction만 한다

예:

```python
outputs = model(
    x
)
```

여기서는 아직:

```text
Loss 계산 X

Backward X

Weight Update X
```

이다.

단순히 Forward를 수행한 것이다.

---

# 46. Loss 연결

```python
outputs = model(
    x
)


loss = loss_fn(
    outputs,
    y
)
```

---

# 47. Backward 연결

```python
loss.backward()
```

이제 Model Parameter들의 Gradient가 계산된다.

---

# 48. Optimizer 연결

```python
optimizer.step()
```

Gradient를 이용하여 Parameter를 업데이트한다.

---

# 49. 전체 관계

```text
Model
↓
Forward
↓
Output
↓
Loss
↓
Backward
↓
Gradient
↓
Optimizer
↓
Weight Update
```

이다.

---

# 50. Model Parameter 확인

```python
for name, param in model.named_parameters():

    print(
        name,
        param.shape
    )
```

---

# 51. 예

```text
fc1.weight torch.Size([10, 4])

fc1.bias torch.Size([10])

fc2.weight torch.Size([2, 10])

fc2.bias torch.Size([2])
```

---

# 52. Parameter 개수 계산

Parameter Tensor의 값 개수를 합하면 된다.

```python
total_params = sum(
    p.numel()
    for p in model.parameters()
)


print(
    total_params
)
```

---

# 53. Trainable Parameter

모든 Parameter가 반드시 학습되는 것은 아니다.

학습 가능한 Parameter만 계산:

```python
trainable_params = sum(
    p.numel()
    for p in model.parameters()
    if p.requires_grad
)


print(
    trainable_params
)
```

---

# 54. requires_grad

```python
param.requires_grad
```

가:

```text
True
```

이면 Gradient 계산 대상이다.

일반적으로 Optimizer가 해당 Parameter를 가지고 있다면 Training 과정에서 업데이트될 수 있다.

---

# 55. Freeze

Parameter 학습을 막고 싶다면:

```python
param.requires_grad = False
```

로 설정할 수 있다.

---

# 56. 예

```python
for param in model.parameters():

    param.requires_grad = False
```

하면 Model 전체 Parameter를 Freeze한다.

---

# 57. Transfer Learning과 연결

Pretrained Model에서:

```text
Backbone Freeze
↓
Classifier만 Training
```

같은 방식에 사용할 수 있다.

---

# 58. Unfreeze

다시 학습 가능하게:

```python
param.requires_grad = True
```

로 변경할 수 있다.

---

# 59. Parameter 개수를 왜 확인할까?

Model이 얼마나 큰지 대략 이해할 수 있다.

예:

```text
Model A
100,000 Parameters

Model B
10,000,000 Parameters
```

이면 Model B가 훨씬 많은 학습 Parameter를 가진다.

---

# 60. Parameter가 많으면 무조건 좋은가?

아니다.

Parameter가 많으면:

```text
더 복잡한 Pattern 표현 가능
```

할 수 있지만:

```text
Overfitting

Memory 사용 증가

Training 시간 증가

Inference 비용 증가
```

등의 문제가 생길 수 있다.

---

# 61. Linear Parameter 수 계산

```python
nn.Linear(
    4,
    10
)
```

Weight:

```text
10 × 4
=
40
```

Bias:

```text
10
```

총:

```text
50 Parameters
```

이다.

---

# 62. 공식

`nn.Linear(in_features, out_features)`의 Parameter 수:

```text
in_features × out_features
+
out_features
```

이다.

Bias를 사용하는 기본 설정 기준이다.

---

# 63. 예

```python
nn.Linear(
    128,
    10
)
```

이면:

```text
128 × 10
+
10

=
1290
```

이다.

---

# 64. bias=False

```python
nn.Linear(
    4,
    10,
    bias=False
)
```

처럼 Bias를 사용하지 않을 수도 있다.

이 경우 Parameter 수는:

```text
4 × 10
=
40
```

이다.

---

# 65. Input Feature 수가 중요하다

예:

```python
nn.Linear(
    784,
    128
)
```

은 Input 마지막 Dimension이 784여야 한다.

---

# 66. MNIST

MNIST:

```text
28 × 28
=
784 Pixels
```

이다.

따라서:

```text
[Batch, 1, 28, 28]
```

이미지를 Flatten하면:

```text
[Batch, 784]
```

가 된다.

---

# 67. MNIST Model

```python
model = nn.Sequential(
    nn.Flatten(),
    nn.Linear(
        28 * 28,
        128
    ),
    nn.ReLU(),
    nn.Linear(
        128,
        10
    )
)
```

---

# 68. Shape 흐름

```text
[32, 1, 28, 28]

↓

Flatten

↓

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

# 69. Output 10의 의미

MNIST는:

```text
0 ~ 9
```

총 10개의 Class가 있다.

따라서 마지막 Layer:

```python
nn.Linear(
    128,
    10
)
```

이다.

---

# 70. Output은 Probability가 아니다

마지막 Linear Layer 결과:

```text
[32, 10]
```

은 일반적으로:

```text
Logits
```

이다.

---

# 71. Logits 예

```text
Sample 1

[
  0.2,
  1.1,
 -0.5,
  3.4,
 ...
]
```

각 Class에 대한 raw score이다.

---

# 72. Prediction

```python
pred = outputs.argmax(
    dim=1
)
```

을 이용해 가장 큰 Logit의 Class Index를 선택할 수 있다.

---

# 73. Probability

필요하다면:

```python
prob = torch.softmax(
    outputs,
    dim=1
)
```

으로 Probability 형태로 변환할 수 있다.

---

# 74. CrossEntropyLoss 사용 시 주의

일반적인 Multi-class Classification에서:

```python
nn.CrossEntropyLoss()
```

는 raw Logits를 입력받는다.

따라서 Training Model의 마지막에 Softmax를 직접 넣지 않는 경우가 일반적이다.

---

# 75. 일반적인 구조

```text
Model
↓
Logits
↓
CrossEntropyLoss
```

이다.

Probability가 필요할 때 별도로:

```text
Logits
↓
Softmax
↓
Probability
```

를 계산한다.

---

# 76. Binary Classification

Binary Classification도 여러 방식으로 구성할 수 있다.

방법 1:

```text
Output = 2 Logits
Loss = CrossEntropyLoss
```

예:

```python
nn.Linear(
    128,
    2
)
```

---

# 77. Binary Classification 방법 2

```text
Output = 1 Logit
Loss = BCEWithLogitsLoss
```

예:

```python
nn.Linear(
    128,
    1
)
```

---

# 78. 둘은 Target 형태도 다를 수 있다

`CrossEntropyLoss` 방식에서는 일반적으로:

```text
Output
[Batch, 2]

Target
[Batch]
Long
```

이다.

---

# 79. BCEWithLogitsLoss 방식

일반적으로:

```text
Output
[Batch]
또는
[Batch, 1]

Target
같은 Shape에 맞춘 Float
```

형태를 사용한다.

Loss Function의 요구사항에 맞춰야 한다.

---

# 80. Model Output과 Loss는 세트로 생각

Model 마지막 Layer를 정할 때:

```text
몇 개 Output인가?

어떤 Loss를 사용할 것인가?

Target Shape는 무엇인가?

Target dtype은 무엇인가?
```

를 함께 생각해야 한다.

---

# 81. Regression

Regression에서는 마지막 Output을 하나로 만들 수 있다.

```python
nn.Linear(
    64,
    1
)
```

---

# 82. Regression Output

```text
Input
↓
Neural Network
↓
1 Numeric Value
```

예:

```text
예상 입원 기간

예상 가격

예상 혈압
```

등이다.

---

# 83. Regression Loss

예:

```python
loss_fn = nn.MSELoss()
```

를 사용할 수 있다.

---

# 84. Model 구조는 문제에 따라 달라진다

```text
Binary Classification

Multi-class Classification

Regression

Image Classification

Segmentation

Object Detection
```

등에 따라 마지막 Layer와 Loss 구조가 달라진다.

---

# 85. Model을 GPU로 이동

```python
model = model.to(
    device
)
```

---

# 86. Input도 같은 Device

```python
x = x.to(
    device
)
```

---

# 87. Forward

```python
outputs = model(
    x
)
```

Model과 Input이 서로 다른 Device에 있으면 오류가 발생할 수 있다.

---

# 88. Model Device 확인

Parameter를 통해 확인할 수 있다.

```python
print(
    next(
        model.parameters()
    ).device
)
```

---

# 89. model.train()

Training 전에:

```python
model.train()
```

을 사용한다.

---

# 90. model.eval()

Validation 또는 Inference에서는:

```python
model.eval()
```

을 사용한다.

---

# 91. 왜 Mode가 필요한가?

일부 Layer는 Training과 Evaluation에서 동작이 다르다.

대표적으로:

```text
Dropout

BatchNorm
```

이다.

---

# 92. Dropout

Training 중 일부 Unit을 무작위로 비활성화하는 Regularization 방법이다.

예:

```python
nn.Dropout(
    p=0.5
)
```

---

# 93. Training에서는

```python
model.train()
```

상태에서 Dropout이 적용된다.

---

# 94. Evaluation에서는

```python
model.eval()
```

상태에서 Dropout이 Training 때처럼 무작위로 Unit을 제거하지 않는다.

---

# 95. BatchNorm

Batch의 통계 등을 활용해 Activation을 정규화하는 Layer이다.

Training과 Evaluation에서 사용하는 통계 방식이 달라질 수 있기 때문에 `train()` / `eval()` 전환이 중요하다.

---

# 96. model.eval()은 Gradient를 끄는 명령이 아니다

이 부분이 중요하다.

```python
model.eval()
```

은 Model의 Evaluation Mode를 설정한다.

하지만 이것만으로 Gradient 계산 자체가 꺼지는 것은 아니다.

---

# 97. Gradient까지 끄려면

Validation에서:

```python
with torch.no_grad():

    outputs = model(
        x
    )
```

처럼 사용한다.

---

# 98. 즉

```text
model.eval()
→ Layer 동작을 Evaluation Mode로 변경

torch.no_grad()
→ Gradient 계산 비활성화
```

이다.

서로 역할이 다르다.

---

# 99. Training

```python
model.train()


for x, y in train_loader:

    ...
```

---

# 100. Validation

```python
model.eval()


with torch.no_grad():

    for x, y in valid_loader:

        ...
```

---

# 101. Model Summary

Model 구조를 확인하는 가장 기본적인 방법:

```python
print(
    model
)
```

이다.

---

# 102. torchinfo

더 자세한 Model Summary가 필요하면 `torchinfo` 같은 도구를 사용할 수 있다.

설치:

```bash
pip install torchinfo
```

---

# 103. 사용 예

```python
from torchinfo import summary


summary(
    model,
    input_size=(
        32,
        4
    )
)
```

Model 구조에 맞는 Input Shape를 넣어야 한다.

---

# 104. Image Model 예

```python
summary(
    model,
    input_size=(
        32,
        3,
        224,
        224
    )
)
```

---

# 105. Summary에서 볼 수 있는 것

도구와 Model에 따라:

```text
Layer

Input Shape

Output Shape

Parameter 수

Trainable Parameter 수
```

등을 확인할 수 있다.

---

# 106. Parameter 초기화

Model을 생성하면 Weight는 초기값을 가진다.

```python
model = MyModel()
```

Training 전에도 Parameter가 존재한다.

---

# 107. 하지만 아직 학습된 것은 아니다

초기 Weight:

```text
Training으로 얻은 최종 지식 X
```

이다.

Training을 통해:

```text
Loss를 줄이는 방향으로
Weight를 수정
```

한다.

---

# 108. Random Initialization

일반적인 Neural Network Layer는 적절한 초기화 전략으로 Parameter가 초기화된다.

따라서 Model을 새로 만들 때마다 초기 Weight가 달라질 수 있다.

---

# 109. Seed

재현성을 위해:

```python
import torch


torch.manual_seed(
    42
)
```

를 사용할 수 있다.

GPU 등까지 포함한 완전한 재현성은 추가 설정의 영향을 받을 수 있다.

---

# 110. Model을 두 번 만들면

```python
model1 = MyModel()

model2 = MyModel()
```

초기 Weight가 서로 다를 수 있다.

따라서 같은 데이터와 Hyperparameter라도 결과가 약간 달라질 수 있다.

---

# 111. Neural Network의 깊이

Hidden Layer가 여러 개 쌓이면 더 깊은 Neural Network가 된다.

예:

```text
Input
↓
Hidden 1
↓
Hidden 2
↓
Hidden 3
↓
Hidden 4
↓
Output
```

---

# 112. 깊으면 무조건 좋은가?

아니다.

깊어지면 더 복잡한 표현을 학습할 수 있지만:

```text
Training 어려움

Overfitting

계산량 증가

Memory 증가
```

등의 문제가 생길 수 있다.

---

# 113. Hidden Unit 수도 Hyperparameter

예:

```python
nn.Linear(
    100,
    64
)
```

에서:

```text
64
```

같은 Hidden Dimension은 사람이 설계하는 Hyperparameter이다.

---

# 114. 예

Model A:

```text
4 → 8 → 2
```

Model B:

```text
4 → 128 → 64 → 2
```

둘 중 어느 것이 더 좋은지는 데이터와 문제에 따라 달라진다.

---

# 115. Parameter vs Hyperparameter

Parameter:

```text
Weight
Bias
```

Training 중 Model이 학습한다.

Hyperparameter:

```text
Hidden Unit 수

Layer 수

Learning Rate

Batch Size

Epoch

Dropout Rate
```

사람이 설정하거나 탐색한다.

---

# 116. Model Architecture

Layer를 어떤 방식으로 구성했는지를:

```text
Model Architecture
```

라고 한다.

예:

```text
Linear
↓
ReLU
↓
Linear
↓
ReLU
↓
Linear
```

도 하나의 Architecture이다.

---

# 117. Image에서는 CNN

이미지에서는:

```python
nn.Conv2d()
```

같은 Convolution Layer를 많이 사용한다.

구조:

```text
Image
↓
Conv
↓
ReLU
↓
Pooling
↓
Conv
↓
ReLU
↓
Classifier
```

이다.

CNN은 Computer Vision Chapter에서 자세히 다룬다.

---

# 118. Pretrained Model

이미 대규모 Dataset으로 학습된 Model을 가져와 사용할 수도 있다.

예:

```text
ResNet

DenseNet

EfficientNet
```

등이다.

---

# 119. 마지막 Classifier 변경

Pretrained Model이 원래 1000개 Class를 출력한다고 하자.

내 문제는:

```text
Normal
Pneumonia
```

2개 Class이다.

그러면 마지막 Layer를 2 Output으로 변경할 수 있다.

---

# 120. 개념

```text
Pretrained Backbone
↓
기존 Classifier 제거/교체
↓
새 Classifier
Output = 2
```

이다.

---

# 121. Feature Extractor와 Classifier

Image Model을 크게:

```text
Feature Extractor
+
Classifier
```

로 생각할 수 있다.

---

# 122. Feature Extractor

Image에서 유용한 Pattern을 추출한다.

```text
Edge

Texture

Shape

Higher-level Feature
```

등을 학습한다.

---

# 123. Classifier

추출된 Feature를 이용해:

```text
어떤 Class인가?
```

를 판단한다.

---

# 124. Freeze와 연결

Transfer Learning에서:

```text
Feature Extractor Freeze
↓
Classifier Training
```

을 먼저 할 수 있다.

그 뒤:

```text
일부 Layer Unfreeze
↓
Fine-tuning
```

할 수도 있다.

---

# 125. Model 구조를 읽는 방법

예:

```python
model = nn.Sequential(
    nn.Flatten(),
    nn.Linear(784, 256),
    nn.ReLU(),
    nn.Dropout(0.3),
    nn.Linear(256, 128),
    nn.ReLU(),
    nn.Linear(128, 10)
)
```

---

# 126. 첫 번째 질문

```text
Input은 무엇인가?
```

MNIST라면:

```text
[Batch, 1, 28, 28]
```

이다.

---

# 127. 두 번째 질문

```text
각 Layer가 Shape를 어떻게 바꾸는가?
```

---

# 128. Shape 추적

```text
[32, 1, 28, 28]

↓

Flatten

↓

[32, 784]

↓

Linear(784, 256)

↓

[32, 256]

↓

ReLU

↓

[32, 256]

↓

Dropout

↓

[32, 256]

↓

Linear(256, 128)

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

# 129. 세 번째 질문

```text
마지막 Output은 무엇을 의미하는가?
```

여기서는:

```text
10 Class Logits
```

이다.

---

# 130. 네 번째 질문

```text
어떤 Loss와 연결되는가?
```

예:

```python
nn.CrossEntropyLoss()
```

이다.

---

# 131. 다섯 번째 질문

```text
어떤 Parameter가 학습되는가?
```

확인:

```python
for name, param in model.named_parameters():

    print(
        name,
        param.shape,
        param.requires_grad
    )
```

---

# 132. Shape Error

Neural Network에서 자주 보는 오류:

```text
mat1 and mat2 shapes cannot be multiplied
```

이다.

---

# 133. 예

현재 Input:

```text
[32, 3]
```

Model:

```python
nn.Linear(
    12,
    100
)
```

---

# 134. 문제

Model은:

```text
12 Features
```

를 기대한다.

실제 Input은:

```text
3 Features
```

이다.

따라서:

```text
3 != 12
```

라서 계산할 수 없다.

---

# 135. 해결 전에 확인

```python
print(
    x.shape
)


print(
    model
)
```

을 먼저 한다.

---

# 136. 무조건 Linear 숫자를 바꾸지 않는다

먼저 확인한다.

```text
데이터 Shape가 잘못됐는가?

Flatten을 빠뜨렸는가?

Channel 순서가 잘못됐는가?

Model의 in_features가 잘못됐는가?
```

이다.

---

# 137. 중간 Shape Debugging

Custom Model에서는:

```python
def forward(
    self,
    x
):

    print(
        "input:",
        x.shape
    )

    x = self.fc1(
        x
    )

    print(
        "fc1:",
        x.shape
    )

    x = self.relu(
        x
    )

    x = self.fc2(
        x
    )

    print(
        "fc2:",
        x.shape
    )

    return x
```

처럼 임시로 Shape를 출력할 수 있다.

---

# 138. 디버깅 후에는

매 Batch마다 출력하면 너무 많은 로그가 생길 수 있으므로 문제를 해결한 뒤 불필요한 `print()`는 제거한다.

---

# 139. Model Forward Smoke Test

Training 전에:

```python
x, y = next(
    iter(
        train_loader
    )
)


x = x.to(
    device
)


model = model.to(
    device
)


output = model(
    x
)


print(
    "Input:",
    x.shape
)


print(
    "Output:",
    output.shape
)
```

을 먼저 실행한다.

---

# 140. Loss까지 확인

```python
y = y.to(
    device
)


loss = loss_fn(
    output,
    y
)


print(
    "Loss:",
    loss.item()
)
```

---

# 141. Parameter Gradient 확인

Backward 후:

```python
loss.backward()
```

Gradient가 생겼는지 확인할 수 있다.

```python
for name, param in model.named_parameters():

    if param.grad is not None:

        print(
            name,
            param.grad.shape
        )
```

---

# 142. grad가 None이라면?

항상 오류라는 뜻은 아니다.

가능한 원인:

```text
해당 Parameter가 Forward 계산에 사용되지 않음

requires_grad=False

Gradient 계산이 비활성화됨

아직 backward()를 하지 않음
```

등이 있다.

---

# 143. Optimizer가 Parameter를 업데이트

```python
optimizer = torch.optim.Adam(
    model.parameters(),
    lr=0.001
)
```

---

# 144. Training Step

```python
optimizer.zero_grad()

output = model(
    x
)

loss = loss_fn(
    output,
    y
)

loss.backward()

optimizer.step()
```

---

# 145. Neural Network Training 핵심

```text
Parameter 초기값
↓
Forward
↓
Loss
↓
Gradient
↓
Parameter Update
↓
Forward
↓
Loss
↓
Gradient
↓
Parameter Update
↓
반복
```

이다.

---

# 146. Model은 스스로 구조를 바꾸지 않는다

Training 중 Model이 학습하는 것은 주로:

```text
Weight

Bias
```

이다.

다음과 같은 구조 자체는 자동으로 바뀌지 않는다.

```text
Layer 수

Hidden Unit 수

Activation 종류
```

이들은 Model Architecture / Hyperparameter에 해당한다.

---

# 147. Neural Network와 Machine Learning 연결

Logistic Regression에서도:

```text
Feature
↓
Weight
↓
Linear Combination
↓
Prediction
```

을 사용했다.

Neural Network는 이러한 변환을 여러 Layer로 확장한 것으로 생각할 수 있다.

---

# 148. Deep의 의미

```text
Deep Learning
```

의 `Deep`은 일반적으로 여러 Layer를 통해 계층적인 표현을 학습한다는 의미와 연결된다.

단순히 Layer를 많이 넣는 것만으로 좋은 Model이 되는 것은 아니다.

---

# 149. Neural Network 전체 구조

```text
Input Tensor
↓
Layer
↓
Activation
↓
Layer
↓
Activation
↓
Output Layer
↓
Logits / Prediction
```

---

# 150. PyTorch 전체 연결

```text
Dataset
↓
DataLoader
↓
Tensor Batch
↓
nn.Module
↓
forward()
↓
Output
↓
Loss
↓
backward()
↓
Optimizer
↓
Parameter Update
```

---

# 151. Model을 보면 확인할 것

```text
1. Input Shape는?

2. 첫 Layer의 in_features는?

3. 각 Layer의 Output Shape는?

4. Activation은 어디에 있는가?

5. 마지막 Output 수는?

6. 문제 유형은 Classification인가 Regression인가?

7. 어떤 Loss와 연결되는가?

8. Parameter 수는?

9. Trainable Parameter는?

10. Model과 Tensor Device는 같은가?
```

---

# 152. 가장 기본적인 Classification Model

```python
import torch
import torch.nn as nn


class Classifier(nn.Module):

    def __init__(
        self,
        n_features,
        n_classes
    ):

        super().__init__()

        self.network = nn.Sequential(
            nn.Linear(
                n_features,
                64
            ),

            nn.ReLU(),

            nn.Linear(
                64,
                32
            ),

            nn.ReLU(),

            nn.Linear(
                32,
                n_classes
            )
        )

    def forward(
        self,
        x
    ):

        return self.network(
            x
        )
```

---

# 153. Model 생성

예:

```python
model = Classifier(
    n_features=12,
    n_classes=2
)
```

---

# 154. Input

```python
x = torch.randn(
    32,
    12
)
```

---

# 155. Forward

```python
output = model(
    x
)


print(
    output.shape
)
```

결과:

```text
torch.Size([32, 2])
```

이다.

---

# 156. Shape 흐름

```text
[32, 12]

↓

Linear(12, 64)

↓

[32, 64]

↓

ReLU

↓

[32, 64]

↓

Linear(64, 32)

↓

[32, 32]

↓

ReLU

↓

[32, 32]

↓

Linear(32, 2)

↓

[32, 2]
```

---

# 157. Model 출력 확인

```python
print(
    model
)
```

Model 구조를 코드 실행 전에 반드시 확인하는 습관을 들인다.

---

# 158. Parameter 수 확인

```python
total_params = sum(
    p.numel()
    for p in model.parameters()
)


trainable_params = sum(
    p.numel()
    for p in model.parameters()
    if p.requires_grad
)


print(
    "Total:",
    total_params
)


print(
    "Trainable:",
    trainable_params
)
```

---

# 159. Model Debugging 기본 세트

```python
print(
    model
)


print(
    "Input:",
    x.shape
)


output = model(
    x
)


print(
    "Output:",
    output.shape
)


print(
    "Parameters:",
    sum(
        p.numel()
        for p in model.parameters()
    )
)
```

---

# 160. 최종 정리

Neural Network는 여러 Layer를 연결하여 Input을 Output으로 변환하는 Model이다.

가장 기본적인 Layer:

```python
nn.Linear(
    input_features,
    output_features
)
```

이다.

Linear Layer는:

```text
y = Wx + b
```

계산을 수행한다.

Model이 학습하는 대표적인 Parameter는:

```text
Weight

Bias
```

이다.

간단한 Model은:

```python
nn.Sequential()
```

로 만들 수 있다.

복잡한 Model은:

```python
class MyModel(nn.Module):
```

형태로 직접 정의할 수 있다.

Custom Model에서는:

```text
__init__
→ Layer 정의

forward
→ Data 흐름 정의
```

라고 이해하면 된다.

Model 코드를 볼 때 가장 중요한 습관은 Shape를 추적하는 것이다.

```text
[Batch, Input Features]

↓

Linear

↓

[Batch, Hidden Features]

↓

Activation

↓

Linear

↓

[Batch, Output Features]
```

그리고 Training은:

```text
Model Parameter
↓
Forward
↓
Loss
↓
Backward
↓
Gradient
↓
Optimizer
↓
Parameter Update
```

의 반복이다.

Neural Network 구조를 이해하면 이후의:

```text
Activation Function

Loss Function

Backpropagation

Optimizer

CNN

Transfer Learning
```

을 훨씬 쉽게 이해할 수 있다.