# Deep Learning Basic

## 1. Deep Learning이란?

Deep Learning은 Machine Learning의 한 분야이다.

전체 관계를 크게 보면:

```text
Artificial Intelligence
        ↓
Machine Learning
        ↓
Deep Learning
```

즉:

```text
AI
└── Machine Learning
    └── Deep Learning
```

이라고 볼 수 있다.

---

# 2. Machine Learning과 Deep Learning

Machine Learning에서는 보통 다음과 같은 모델을 사용했다.

```text
Logistic Regression
Random Forest
XGBoost
LightGBM
CatBoost
```

Deep Learning에서는:

```text
Neural Network
신경망
```

을 이용한다.

---

# 3. 가장 큰 차이

전통적인 Machine Learning에서는 사람이 Feature를 직접 만들거나 선택하는 과정이 매우 중요하다.

예를 들어 환자 데이터가 있다고 하자.

```text
키
몸무게
나이
혈압
혈당
```

여기서 사람이:

```text
BMI = 몸무게 / 키²
```

같은 Feature를 만들 수 있다.

이것이:

```text
Feature Engineering
```

이다.

---

# 4. Deep Learning에서는?

Deep Learning은 여러 Layer를 통해 데이터에서 유용한 표현을 학습한다.

특히 이미지에서는 이 특징이 중요하다.

예:

```text
Chest X-ray Image
        ↓
Deep Learning
        ↓
Image Pattern 학습
        ↓
Normal / Pneumonia
```

사람이 이미지의 모든 Feature를 직접 숫자로 만드는 대신 모델이 학습 과정에서 유용한 표현을 찾는다.

---

# 5. 하지만 Feature Engineering이 필요 없다는 뜻은 아니다

Deep Learning이라고 해서 사람이 아무것도 하지 않아도 되는 것은 아니다.

여전히 중요하다.

```text
Data Quality
Label Quality
Train / Validation Split
Normalization
Augmentation
Model Architecture
Loss Function
Optimizer
Learning Rate
Evaluation Metric
```

즉:

```text
Deep Learning
=
자동으로 모든 문제를 해결
```

은 아니다.

---

# 6. Deep Learning의 핵심

Deep Learning의 핵심은:

```text
Artificial Neural Network
인공 신경망
```

이다.

신경망은 여러 개의 Layer를 연결하여 데이터를 변환한다.

---

# 7. 가장 단순한 신경망

```text
Input
  ↓
Linear Layer
  ↓
Activation
  ↓
Output
```

예를 들어:

```text
환자 정보
  ↓
Neural Network
  ↓
질병 여부
```

와 같은 구조이다.

---

# 8. Layer란?

Layer는 입력 데이터를 다른 형태로 변환하는 계산 단계라고 생각할 수 있다.

예:

```text
Input
↓
Layer 1
↓
Layer 2
↓
Layer 3
↓
Output
```

Layer가 여러 개 쌓여 있기 때문에:

```text
Deep Neural Network
```

라고 부른다.

---

# 9. Input Layer

Input Layer는 모델에 들어가는 Feature이다.

예:

```text
Age
BMI
BloodPressure
Glucose
```

4개의 Feature가 있다면:

```text
Input Feature 수 = 4
```

이다.

---

# 10. Hidden Layer

입력과 출력 사이의 Layer를:

```text
Hidden Layer
은닉층
```

이라고 한다.

예:

```text
Input Layer
     ↓
Hidden Layer 1
     ↓
Hidden Layer 2
     ↓
Output Layer
```

---

# 11. 왜 Hidden이라고 할까?

Input과 Output 사이에서 내부적으로 계산되는 표현이기 때문이다.

예:

```text
Input
[Age, BMI, BP, Glucose]

        ↓

Hidden Representation

        ↓

Output
Disease Probability
```

---

# 12. Output Layer

Output Layer는 최종 Prediction을 만들기 위한 Layer이다.

문제에 따라 출력 구조가 달라진다.

예:

```text
Binary Classification
→ 2 Class

Multi-class Classification
→ 여러 Class

Regression
→ 연속값
```

---

# 13. Neuron

신경망의 기본 계산 단위를 흔히:

```text
Neuron
Node
Unit
```

이라고 부른다.

Neuron에서는 입력값을 받아 계산한다.

---

# 14. 가장 기본적인 계산

Neuron의 핵심 계산은 개념적으로:

```text
z = wx + b
```

이다.

여러 Feature가 있다면:

```text
z =
w1*x1
+
w2*x2
+
w3*x3
+
...
+
b
```

이다.

---

# 15. Weight

```text
w
```

는 Weight, 즉 가중치이다.

각 Feature를 얼마나 반영할지를 나타내는 학습 가능한 값이다.

---

# 16. Bias

```text
b
```

는 Bias, 즉 편향이다.

Linear Model에서 배웠던:

```text
y = wx + b
```

와 연결된다.

---

# 17. Machine Learning과 연결

Linear Regression에서도:

```text
y = wx + b
```

를 사용했다.

Logistic Regression에서도 Feature의 선형 결합을 만들었다.

Neural Network에서는 이런 계산을:

```text
여러 Layer
```

에 걸쳐 반복한다.

---

# 18. Linear Layer

PyTorch에서는 이런 선형 계산을:

```python
nn.Linear()
```

로 만들 수 있다.

예:

```python
import torch.nn as nn


layer = nn.Linear(
    4,
    10
)
```

---

# 19. nn.Linear(4, 10)의 의미

```text
Input Feature = 4

Output Feature = 10
```

이다.

즉:

```text
4개의 숫자
↓
Linear Layer
↓
10개의 숫자
```

로 변환한다.

---

# 20. 예

Input:

```text
Age
BMI
BloodPressure
Glucose
```

총 4개라고 하자.

```python
nn.Linear(
    4,
    10
)
```

을 통과하면 10개의 새로운 값이 만들어진다.

---

# 21. 신경망에 Linear만 여러 개 넣으면 될까?

예:

```python
nn.Linear(
    4,
    10
)

nn.Linear(
    10,
    10
)

nn.Linear(
    10,
    2
)
```

처럼 Linear Layer만 여러 개 연결할 수도 있다.

하지만 중요한 문제가 있다.

---

# 22. Activation Function

신경망에서는 Linear Layer 사이에 보통:

```text
Activation Function
활성화 함수
```

를 사용한다.

대표적인 것이:

```text
ReLU
```

이다.

---

# 23. ReLU

ReLU:

```text
Rectified Linear Unit
```

이다.

수식:

```text
ReLU(x) = max(0, x)
```

즉:

```text
음수 → 0

양수 → 그대로
```

이다.

---

# 24. 예

입력:

```text
[-3, -1, 0, 2, 5]
```

ReLU 적용:

```text
[0, 0, 0, 2, 5]
```

이다.

---

# 25. PyTorch ReLU

```python
nn.ReLU()
```

로 사용할 수 있다.

---

# 26. Hidden Layer 추가

예:

```python
model = nn.Sequential(

    nn.Linear(
        4,
        10
    ),

    nn.ReLU(),

    nn.Linear(
        10,
        2
    )

)
```

---

# 27. nn.Sequential

`nn.Sequential`은 Layer들을 순서대로 연결한다.

```text
Input
↓
Linear(4 → 10)
↓
ReLU
↓
Linear(10 → 2)
↓
Output
```

이다.

---

# 28. 코드 읽는 방법

```python
model = nn.Sequential(
    nn.Linear(4, 10),
    nn.ReLU(),
    nn.Linear(10, 2)
)
```

를 보면:

```text
4개 Feature 입력

↓

10개 값으로 변환

↓

ReLU 적용

↓

2개 값으로 변환
```

이라고 읽으면 된다.

---

# 29. MNIST 예

MNIST Image는:

```text
28 × 28
```

크기의 흑백 이미지이다.

Pixel 수:

```text
28 × 28
=
784
```

이다.

---

# 30. 이미지를 Linear Layer에 넣으려면?

이미지는 원래:

```text
28 × 28
```

이다.

Linear Layer는 일반적으로 마지막 차원을 Feature처럼 사용하므로 기초적인 Fully Connected 모델에서는 이미지를 1차원으로 펼친다.

```text
28 × 28
↓
784
```

---

# 31. Flatten

PyTorch:

```python
nn.Flatten()
```

을 사용한다.

---

# 32. 예

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

# 33. 구조

```text
28 × 28 Image
      ↓
Flatten
      ↓
784
      ↓
Linear
784 → 128
      ↓
ReLU
      ↓
Linear
128 → 10
      ↓
10 Class Scores
```

---

# 34. 왜 10개 Output인가?

MNIST는 숫자:

```text
0
1
2
3
4
5
6
7
8
9
```

총 10개 Class이기 때문이다.

따라서:

```python
nn.Linear(
    128,
    10
)
```

을 사용한다.

---

# 35. Classification Output

모델의 마지막 출력은 예를 들어:

```text
[
 1.2,
 -0.5,
 0.7,
 3.1,
 ...
]
```

처럼 나올 수 있다.

이 값들을:

```text
Logits
```

라고 부른다.

---

# 36. Logit

Logit은 Softmax를 적용하기 전의 모델 원시 출력값이라고 생각하면 된다.

예:

```text
Class 0 → 1.2
Class 1 → -0.5
Class 2 → 0.7
Class 3 → 3.1
```

가장 큰 값이 Class 3이라면 모델은 Class 3을 가장 강하게 선택하는 방향의 출력을 만든 것이다.

---

# 37. Probability가 필요한 경우

Logit을 Probability로 바꾸기 위해:

```text
Softmax
```

를 사용할 수 있다.

---

# 38. Softmax

Softmax는 여러 Class의 Logit을:

```text
0 ~ 1
```

범위의 값으로 바꾸고 전체 합이:

```text
1
```

이 되도록 만든다.

---

# 39. 예

```text
Logits

[1.2, 0.3, 2.5]
```

Softmax 후 개념적으로:

```text
[0.18, 0.07, 0.75]
```

처럼 될 수 있다.

---

# 40. 가장 높은 Probability

```text
Class 2 = 0.75
```

라면 Class 2를 Prediction할 수 있다.

---

# 41. PyTorch Softmax

```python
import torch


prob = torch.softmax(
    logits,
    dim=1
)
```

처럼 사용할 수 있다.

---

# 42. 하지만 학습할 때 항상 Softmax를 직접 넣는 것은 아니다

PyTorch의:

```python
nn.CrossEntropyLoss()
```

는 일반적인 Multi-class Classification에서 raw Logits를 입력받도록 설계되어 있다.

따라서 모델 마지막에 Softmax를 넣지 않고:

```text
Model
↓
Logits
↓
CrossEntropyLoss
```

로 사용하는 경우가 일반적이다.

---

# 43. 중요한 구조

```text
Training

Model
↓
Logits
↓
CrossEntropyLoss
```

Probability가 필요할 때:

```text
Model
↓
Logits
↓
Softmax
↓
Probability
```

로 생각하면 된다.

---

# 44. Model이 처음부터 잘 예측할까?

아니다.

처음에는 Weight가 학습되지 않은 상태이므로 Prediction이 좋지 않다.

그래서:

```text
Training
학습
```

이 필요하다.

---

# 45. Deep Learning 학습의 핵심

Deep Learning Training은 크게:

```text
1. Forward

2. Loss 계산

3. Backward

4. Weight Update
```

를 반복한다.

---

# 46. Forward

Forward는:

```text
Input을 Model에 넣어
Prediction을 만드는 과정
```

이다.

PyTorch에서는:

```python
output = model(
    x
)
```

이다.

---

# 47. 예

```text
Image
↓
Model
↓
Logits
```

이 과정이 Forward이다.

---

# 48. Prediction과 정답 비교

Model Prediction과 실제 Label을 비교한다.

예:

```text
Prediction
→ Pneumonia

True Label
→ Normal
```

이라면 틀렸다.

이 차이를 숫자로 계산해야 한다.

---

# 49. Loss Function

Prediction이 정답과 얼마나 다른지를 계산하는 함수를:

```text
Loss Function
손실 함수
```

라고 한다.

---

# 50. Classification Loss

Classification에서 대표적으로:

```python
nn.CrossEntropyLoss()
```

를 사용할 수 있다.

---

# 51. Regression Loss

Regression에서는 예를 들어:

```python
nn.MSELoss()
```

등을 사용할 수 있다.

---

# 52. Loss의 의미

개념적으로:

```text
Loss가 큼
→ Prediction이 좋지 않음

Loss가 작음
→ Prediction이 정답에 가까움
```

이다.

---

# 53. 목표

Training의 목표는:

```text
Loss를 줄이는 방향으로
Weight를 수정하는 것
```

이다.

---

# 54. Weight를 어떻게 수정할까?

여기서:

```text
Gradient
기울기
```

가 등장한다.

---

# 55. Gradient

Gradient는:

```text
Weight를 어느 방향으로 바꾸면
Loss가 증가하거나 감소하는가?
```

에 대한 정보를 제공한다.

---

# 56. Backpropagation

Loss에서부터 역방향으로 Gradient를 계산하는 과정을:

```text
Backpropagation
역전파
```

라고 한다.

PyTorch에서는:

```python
loss.backward()
```

를 사용한다.

---

# 57. Forward와 Backward

```text
Forward

Input
↓
Model
↓
Prediction
↓
Loss


Backward

Loss
↑
Gradient
↑
각 Layer
↑
Weight
```

라고 생각할 수 있다.

---

# 58. Optimizer

Gradient를 계산했다고 Weight가 자동으로 변경되는 것은 아니다.

Weight를 실제로 업데이트하는 역할을:

```text
Optimizer
```

가 담당한다.

---

# 59. 대표적인 Optimizer

```text
SGD

Adam

AdamW
```

등이 있다.

---

# 60. Adam

PyTorch에서는:

```python
optimizer = torch.optim.Adam(
    model.parameters(),
    lr=0.001
)
```

처럼 사용할 수 있다.

---

# 61. model.parameters()

```python
model.parameters()
```

는 모델이 학습해야 하는 Parameter들을 Optimizer에게 전달한다.

주로:

```text
Weight
Bias
```

등이다.

---

# 62. Learning Rate

```python
lr=0.001
```

의 `lr`은:

```text
Learning Rate
학습률
```

이다.

Weight를 얼마나 큰 Step으로 업데이트할지 조절하는 중요한 Hyperparameter이다.

---

# 63. Learning Rate가 너무 크면

```text
Loss Minimum을 지나침

Training 불안정

Loss가 튀거나 발산
```

할 수 있다.

---

# 64. Learning Rate가 너무 작으면

```text
Training이 매우 느림

좋은 지점까지 가는 데 오래 걸림
```

수 있다.

---

# 65. 기본 Training 순서

PyTorch에서는 일반적으로 다음 순서를 반복한다.

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

# 66. 하나씩 해석

```python
optimizer.zero_grad()
```

이전 Step에서 계산된 Gradient를 초기화한다.

---

# 67. Forward

```python
output = model(
    x
)
```

Prediction을 만든다.

---

# 68. Loss

```python
loss = loss_fn(
    output,
    y
)
```

Prediction과 정답의 차이를 계산한다.

---

# 69. Backward

```python
loss.backward()
```

Gradient를 계산한다.

---

# 70. Update

```python
optimizer.step()
```

Gradient를 이용해 Weight를 업데이트한다.

---

# 71. 전체 핵심

```text
Gradient 초기화
↓
Forward
↓
Loss
↓
Backward
↓
Weight Update
```

이다.

---

# 72. 왜 zero_grad가 필요할까?

PyTorch에서는 기본적으로 Gradient가 누적될 수 있다.

따라서 일반적인 Training에서는 매 Step마다:

```python
optimizer.zero_grad()
```

를 호출한다.

---

# 73. Batch

전체 데이터를 한 번에 Model에 넣지 않고 일정한 크기로 나누어 학습하는 경우가 많다.

이 작은 데이터 묶음을:

```text
Batch
```

라고 한다.

---

# 74. 예

Training Data:

```text
10,000개
```

Batch Size:

```text
32
```

라면:

```text
32개씩 Model에 입력
```

하면서 Training한다.

---

# 75. Batch Size

예:

```python
batch_size = 32
```

이다.

---

# 76. DataLoader

PyTorch에서는 Batch 단위로 데이터를 제공하는 데:

```text
DataLoader
```

를 많이 사용한다.

예:

```python
from torch.utils.data import (
    DataLoader
)


train_loader = DataLoader(
    train_dataset,
    batch_size=32,
    shuffle=True
)
```

---

# 77. DataLoader의 역할

```text
Dataset
↓
DataLoader
↓
Batch 1
Batch 2
Batch 3
...
```

형태로 데이터를 제공한다.

---

# 78. Dataset과 DataLoader 차이

```text
Dataset
→ 개별 Sample을 어떻게 가져올지 정의

DataLoader
→ Dataset을 Batch 단위로 공급
```

한다고 생각하면 된다.

---

# 79. Epoch

Training Dataset 전체를 한 번 학습하는 것을:

```text
1 Epoch
```

라고 한다.

---

# 80. 예

```text
Training Data = 1000개

Batch Size = 100
```

이면 대략:

```text
10 Batch
```

를 처리하면 전체 Training Data를 한 번 본 것이다.

이것이:

```text
1 Epoch
```

이다.

---

# 81. 10 Epoch

```text
전체 Training Data를
10번 반복해서 학습
```

한다는 의미이다.

---

# 82. 기본 Epoch Loop

```python
epochs = 10


for epoch in range(
    epochs
):

    print(
        epoch
    )
```

---

# 83. Batch Loop

```python
for x, y in train_loader:

    ...
```

---

# 84. 합치면

```python
for epoch in range(
    epochs
):

    for x, y in train_loader:

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

이것이 Deep Learning Training Loop의 핵심이다.

---

# 85. 전체 Training 구조

```text
Epoch 1
   ↓
Batch 1
   ↓
Forward
Loss
Backward
Update

Batch 2
   ↓
Forward
Loss
Backward
Update

...

Epoch 1 끝

↓

Epoch 2

↓

반복
```

---

# 86. Train과 Validation

Machine Learning에서 배웠던 것처럼 Deep Learning에서도:

```text
Train

Validation

Test
```

를 구분한다.

---

# 87. Train

```text
Weight를 학습
```

하는 데이터이다.

---

# 88. Validation

```text
학습 중 Model 성능 확인

Hyperparameter 선택

Overfitting 확인

Best Model 선택
```

등에 사용한다.

---

# 89. Test

모델 개발이 끝난 뒤:

```text
최종 일반화 성능 평가
```

에 사용한다.

---

# 90. Deep Learning에서도 Test를 계속 보면 안 된다

```text
Epoch마다 Test 확인

Model 바꿀 때마다 Test 확인

Learning Rate 바꿀 때마다 Test 확인
```

하면 Test에 맞춰질 수 있다.

Train / Validation / Test 역할은 Machine Learning과 동일한 원칙을 가진다.

---

# 91. model.train()

PyTorch에서는 Training 전에:

```python
model.train()
```

을 사용한다.

---

# 92. model.eval()

Validation 또는 Inference에서는:

```python
model.eval()
```

을 사용한다.

---

# 93. 왜 필요할까?

일부 Layer는 Training과 Evaluation에서 동작이 달라진다.

대표적으로:

```text
Dropout

Batch Normalization
```

등이 있다.

그래서 Mode를 명확하게 전환한다.

---

# 94. Validation에서는 Gradient가 필요할까?

일반적인 Validation에서는 Weight를 업데이트하지 않는다.

따라서 Gradient 계산도 필요하지 않다.

---

# 95. torch.no_grad()

예:

```python
model.eval()


with torch.no_grad():

    output = model(
        x
    )
```

처럼 사용할 수 있다.

---

# 96. Training vs Validation

Training:

```text
model.train()

Gradient 계산 O

Backward O

Optimizer Step O
```

Validation:

```text
model.eval()

Gradient 계산 X

Backward X

Optimizer Step X
```

이다.

---

# 97. Validation 기본 구조

```python
model.eval()


with torch.no_grad():

    for x, y in valid_loader:

        output = model(
            x
        )

        loss = loss_fn(
            output,
            y
        )
```

---

# 98. Training Loss와 Validation Loss

Training 과정에서:

```text
Train Loss

Validation Loss
```

를 함께 확인하는 것이 중요하다.

---

# 99. 정상적인 학습 예

```text
Epoch 1

Train Loss = 0.80
Valid Loss = 0.85


Epoch 2

Train Loss = 0.60
Valid Loss = 0.65


Epoch 3

Train Loss = 0.45
Valid Loss = 0.50
```

둘 다 감소하고 있다.

---

# 100. Overfitting 예

```text
Epoch 1

Train Loss = 0.80
Valid Loss = 0.85


Epoch 5

Train Loss = 0.30
Valid Loss = 0.40


Epoch 10

Train Loss = 0.10
Valid Loss = 0.70
```

Train Loss는 계속 감소하지만 Validation Loss는 다시 증가한다.

---

# 101. 이것이 의미하는 것

```text
Training Data에는 점점 잘 맞음

하지만 새로운 Validation Data에는
오히려 성능이 나빠짐
```

즉:

```text
Overfitting
```

이 발생했을 가능성이 있다.

---

# 102. Best Model 저장

Validation 성능이 가장 좋은 시점의 Model을 저장할 수 있다.

예:

```text
Epoch 1
Val Loss = 0.8

Epoch 2
Val Loss = 0.6

Epoch 3
Val Loss = 0.5 ← Best

Epoch 4
Val Loss = 0.7
```

최종 Epoch 4보다 Epoch 3 Model을 선택할 수 있다.

---

# 103. Early Stopping

Validation 성능이 일정 기간 개선되지 않으면 Training을 중단하는 방법을:

```text
Early Stopping
```

이라고 한다.

---

# 104. GPU

Deep Learning에서는 많은 Matrix 계산을 수행한다.

그래서 GPU를 이용하면 Training을 크게 가속할 수 있는 경우가 많다.

---

# 105. CUDA

NVIDIA GPU에서 PyTorch가 GPU 연산을 사용할 때 흔히:

```text
CUDA
```

를 사용한다.

---

# 106. GPU 확인

```python
import torch


print(
    torch.cuda.is_available()
)
```

결과:

```text
True
```

라면 현재 PyTorch 환경에서 CUDA GPU를 사용할 수 있다는 뜻이다.

---

# 107. Device 설정

```python
device = torch.device(
    "cuda"
    if torch.cuda.is_available()
    else "cpu"
)
```

---

# 108. 출력

```python
print(
    device
)
```

예:

```text
cuda
```

또는:

```text
cpu
```

이다.

---

# 109. Model을 GPU로 이동

```python
model = model.to(
    device
)
```

---

# 110. Data도 GPU로 이동

```python
x = x.to(
    device
)


y = y.to(
    device
)
```

---

# 111. 매우 중요한 원칙

Model과 Tensor가 서로 다른 Device에 있으면 오류가 발생할 수 있다.

잘못된 예:

```text
Model → GPU

Input → CPU
```

---

# 112. 올바른 예

```text
Model → GPU

Input → GPU

Target → GPU
```

같은 Device에 둔다.

---

# 113. Tensor

PyTorch에서 데이터를 다루는 핵심 자료구조가:

```text
Tensor
```

이다.

NumPy의:

```text
ndarray
```

와 비슷한 다차원 배열이라고 생각할 수 있다.

---

# 114. Tensor 예

```python
import torch


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

---

# 115. Tensor Shape

```python
print(
    x.shape
)
```

Shape는 Deep Learning에서 매우 중요하다.

---

# 116. 왜 Shape가 중요할까?

각 Layer는 기대하는 Input Shape가 있다.

예:

```python
nn.Linear(
    12,
    100
)
```

이라면 마지막 Feature Dimension이:

```text
12
```

여야 한다.

---

# 117. Shape Error 예

대표적인 오류:

```text
RuntimeError:
mat1 and mat2 shapes cannot be multiplied
```

이다.

이 오류는 보통:

```text
현재 Tensor의 Feature 수

와

Linear Layer가 기대하는 Feature 수
```

가 맞지 않을 때 발생한다.

---

# 118. 예

```text
Input Shape

(32, 3)
```

그런데 Model:

```python
nn.Linear(
    12,
    100
)
```

이면:

```text
Model은 Feature 12개를 기대

실제 Input은 Feature 3개
```

라서 오류가 발생한다.

---

# 119. 해결의 시작

무조건 먼저:

```python
print(
    x.shape
)
```

를 확인한다.

그리고:

```python
print(
    model
)
```

로 첫 Layer가 기대하는 Feature 수를 확인한다.

---

# 120. 이미지 Tensor Shape

PyTorch Image는 일반적으로:

```text
Batch
Channel
Height
Width
```

순서를 많이 사용한다.

즉:

```text
[B, C, H, W]
```

이다.

---

# 121. 예

```text
[32, 3, 224, 224]
```

의 의미:

```text
32
→ Batch Size

3
→ RGB Channel

224
→ Height

224
→ Width
```

이다.

---

# 122. 흑백 이미지

흑백 Image라면 보통:

```text
[32, 1, 224, 224]
```

처럼 Channel이 1일 수 있다.

---

# 123. RGB 이미지

RGB는:

```text
Red

Green

Blue
```

3개 Channel이므로:

```text
C = 3
```

이다.

---

# 124. CNN

Image Deep Learning에서는 Fully Connected Layer만 사용하는 것보다:

```text
Convolutional Neural Network
CNN
```

을 많이 사용한다.

CNN은 이미지의 공간적 Pattern을 학습하는 데 적합하다.

---

# 125. CNN이 보는 것

초기 Layer에서는 개념적으로:

```text
Edge

Line

Texture
```

같은 단순한 Pattern을 학습할 수 있다.

더 깊은 Layer에서는:

```text
더 복잡한 Shape

Object Part

고수준 Pattern
```

을 학습할 수 있다.

---

# 126. 이미지 분류 구조

```text
Image
↓
CNN
↓
Feature Extraction
↓
Classifier
↓
Class Prediction
```

이다.

---

# 127. Chest X-ray 예

```text
Chest X-ray
      ↓
CNN
      ↓
Image Feature
      ↓
Classifier
      ↓
Normal / Pneumonia
```

와 같은 구조를 만들 수 있다.

---

# 128. Transfer Learning

처음부터 모든 Weight를 학습하지 않고 이미 대규모 데이터에서 학습된 Model을 가져와 사용하는 방법을:

```text
Transfer Learning
전이 학습
```

이라고 한다.

---

# 129. 대표적인 Pretrained Model

예:

```text
ResNet

EfficientNet

DenseNet

VGG
```

등이 있다.

---

# 130. 왜 사용할까?

이미 학습된 Model은 기본적인 이미지 Pattern을 어느 정도 학습하고 있을 수 있다.

그래서 데이터가 제한적인 프로젝트에서 처음부터 학습하는 것보다 유리할 수 있다.

---

# 131. Fine-tuning

Pretrained Model을 가져온 뒤 새로운 데이터에 맞게 Weight를 추가로 학습하는 것을:

```text
Fine-tuning
```

이라고 한다.

---

# 132. Freeze

일부 Layer의 Weight를 업데이트하지 않도록 고정하는 것을:

```text
Freeze
```

라고 한다.

---

# 133. Freeze → Fine-tuning

한 가지 학습 전략:

```text
Stage 1

Backbone Freeze
↓
Classifier 위주 학습


Stage 2

일부 또는 전체 Layer Unfreeze
↓
낮은 Learning Rate
↓
Fine-tuning
```

이다.

항상 이 방식이 최선이라는 뜻은 아니며 실험으로 확인한다.

---

# 134. Data Augmentation

이미지 Training Data를 변형하여 다양한 Training Sample을 만드는 방법이다.

예:

```text
Flip

Rotation

Crop

Brightness Change
```

등이다.

---

# 135. 목적

모델이 특정 Training Image Pattern만 외우는 것을 줄이고 다양한 변화에 더 잘 대응하도록 돕는 것이 목적이다.

---

# 136. Augmentation 주의

의료영상처럼 방향이나 위치 자체가 의미를 가질 수 있는 데이터에서는 모든 Augmentation이 적절한 것은 아니다.

도메인 의미를 훼손하지 않는지 확인해야 한다.

---

# 137. Deep Learning 전체 학습 흐름

```text
Data
↓
Dataset
↓
DataLoader
↓
Batch
↓
Device 이동
↓
Model
↓
Forward
↓
Loss
↓
Backward
↓
Optimizer Step
↓
반복
↓
Validation
↓
Best Model 저장
↓
Test / Inference
```

---

# 138. PyTorch 전체 흐름

```text
torch
↓
Tensor
↓
Dataset
↓
DataLoader
↓
nn.Module
↓
Loss Function
↓
Optimizer
↓
Training Loop
↓
Validation Loop
↓
Model Save
↓
Inference
```

---

# 139. Machine Learning과 비교

Machine Learning:

```python
model.fit(
    X_train,
    y_train
)


pred = model.predict(
    X_test
)
```

Scikit-learn이 많은 Training 과정을 내부에서 처리한다.

---

# 140. Deep Learning에서는

PyTorch에서는 Training Loop를 직접 작성하는 경우가 많다.

```python
for epoch in range(
    epochs
):

    for x, y in train_loader:

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

그래서 처음에는 코드가 더 복잡하게 느껴질 수 있다.

---

# 141. 하지만 핵심은 반복이다

Deep Learning Training은 결국:

```text
Prediction

↓

정답과 비교

↓

얼마나 틀렸는지 계산

↓

어떻게 Weight를 바꿀지 계산

↓

Weight 수정

↓

다시 Prediction
```

을 반복하는 것이다.

---

# 142. 핵심 용어 정리

```text
Neural Network
→ 여러 Layer로 구성된 학습 Model

Layer
→ Data를 변환하는 계산 단계

Neuron
→ 신경망의 계산 단위

Weight
→ Model이 학습하는 가중치

Bias
→ Model이 학습하는 편향

Activation Function
→ 비선형 변환

ReLU
→ 대표적인 Activation Function

Forward
→ Input으로 Prediction을 만드는 과정

Loss
→ Prediction과 정답의 차이

Gradient
→ Loss 변화 방향에 대한 정보

Backpropagation
→ 역방향으로 Gradient 계산

Optimizer
→ Weight Update 담당

Learning Rate
→ Weight Update Step 크기

Batch
→ 한 번에 처리하는 Sample 묶음

Epoch
→ Training Dataset 전체를 한 번 학습

Tensor
→ PyTorch의 다차원 배열

CUDA
→ NVIDIA GPU 연산 환경

Inference
→ 학습된 Model로 Prediction 수행
```

---

# 143. 가장 중요한 학습 구조

```text
Input
  ↓
Model
  ↓
Prediction
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
  ↓
더 나은 Prediction
```

이 과정을 반복한다.

---

# 144. 코드로 보면

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

이 다섯 줄이 PyTorch Training의 가장 중요한 기본 구조이다.

---

# 145. 앞으로 배울 내용

이 파일에서는 전체 그림을 먼저 봤다.

앞으로 하나씩 자세하게 배운다.

```text
Tensor
↓
Dataset / DataLoader
↓
Neural Network
↓
Activation Function
↓
Loss Function
↓
Optimizer / Backpropagation
↓
Training Loop
↓
Validation / Overfitting
↓
GPU / CUDA
↓
Model Save / Load
```

---

# 146. 마지막 정리

Deep Learning은 Machine Learning의 한 분야이며 Neural Network를 이용해 복잡한 Pattern을 학습한다.

가장 중요한 학습 흐름은:

```text
Data
↓
Model
↓
Forward
↓
Loss
↓
Backward
↓
Optimizer
↓
Weight Update
```

이다.

그리고 이 과정을:

```text
Batch 단위
```

로 반복하고:

```text
전체 Training Dataset을 한 번 처리
=
1 Epoch
```

라고 한다.

Deep Learning을 처음 공부할 때 모든 수식부터 외우기보다 다음 흐름을 먼저 이해하는 것이 중요하다.

```text
Model이 Prediction한다.

↓

정답과 비교해 Loss를 계산한다.

↓

Backward로 Gradient를 계산한다.

↓

Optimizer가 Weight를 수정한다.

↓

다시 Prediction한다.
```

PyTorch Deep Learning의 기본은 결국 이 반복 구조에서 시작한다.