# Optimizer & Backpropagation

## 1. 지금까지의 흐름

지금까지 Neural Network에서 다음 과정을 배웠다.

```text
Input
↓
Model
↓
Prediction
↓
Loss
```

하지만 Loss를 계산하는 것만으로 Model이 학습되는 것은 아니다.

Model의 Weight와 Bias를 실제로 수정해야 한다.

전체 Training 과정은:

```text
Input
↓
Forward
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
Parameter Update
```

이다.

---

# 2. Backpropagation이란?

Backpropagation은 한국어로:

```text
역전파
```

라고 한다.

Loss를 기준으로 Model의 각 Parameter가 Loss에 얼마나 영향을 주었는지 계산하고, Gradient를 뒤쪽 Layer에서 앞쪽 Layer 방향으로 전달하는 과정이다.

---

# 3. Forward와 Backward

Forward:

```text
Input
↓
Layer 1
↓
Layer 2
↓
Output
↓
Loss
```

Backward:

```text
Loss
↑
Layer 2
↑
Layer 1
```

라고 생각할 수 있다.

---

# 4. Forward Pass

예:

```python
outputs = model(
    x
)
```

Model이 Input을 이용해 Output을 계산한다.

이를:

```text
Forward Pass
```

라고 한다.

---

# 5. Loss 계산

```python
loss = loss_fn(
    outputs,
    y
)
```

Prediction과 Target을 비교하여 Loss를 계산한다.

---

# 6. Backward

```python
loss.backward()
```

를 실행한다.

이 명령은 Loss를 기준으로 각 Parameter의 Gradient를 계산한다.

---

# 7. Gradient란?

Gradient는 현재 Parameter를 조금 변화시켰을 때 Loss가 어느 방향으로 얼마나 변하는지를 나타내는 값이라고 생각할 수 있다.

쉽게 말하면:

```text
Weight를 어느 방향으로 수정해야
Loss를 줄일 수 있는가?
```

에 대한 정보이다.

---

# 8. 아주 단순한 예

어떤 Weight가 있다고 하자.

```text
Weight = 5
```

현재 Gradient:

```text
Gradient = +2
```

라면 Loss를 줄이기 위해 일반적인 Gradient Descent에서는 Weight를 반대 방향으로 이동시킨다.

```text
Weight ↓
```

---

# 9. 반대의 경우

```text
Gradient = -2
```

라면 반대 방향으로 움직이므로:

```text
Weight ↑
```

하게 된다.

---

# 10. Gradient Descent

가장 기본적인 Parameter Update 개념:

```text
새 Weight
=
기존 Weight
-
Learning Rate × Gradient
```

이다.

수식:

```text
w_new
=
w_old - lr × gradient
```

---

# 11. Learning Rate

Learning Rate는:

```text
학습률
```

이다.

보통:

```text
lr
```

이라고 작성한다.

예:

```python
lr = 0.001
```

---

# 12. Learning Rate 역할

Gradient가:

```text
어느 방향으로 갈 것인가?
```

를 알려준다면 Learning Rate는:

```text
한 번에 얼마나 이동할 것인가?
```

를 결정한다.

---

# 13. 예

현재 Weight:

```text
5.0
```

Gradient:

```text
2.0
```

Learning Rate:

```text
0.1
```

이라면:

```text
new weight
=
5.0 - 0.1 × 2.0

=
4.8
```

이다.

---

# 14. Learning Rate가 너무 크면

예:

```text
lr = 10
```

처럼 지나치게 크면 최적점을 지나치면서 Training이 불안정해질 수 있다.

```text
Loss 감소
↓
갑자기 증가
↓
진동
↓
발산
```

할 수 있다.

---

# 15. Learning Rate가 너무 작으면

예:

```text
lr = 0.0000001
```

처럼 지나치게 작으면:

```text
Parameter Update가 매우 작음
↓
Training이 매우 느림
```

이 될 수 있다.

---

# 16. 따라서 Learning Rate는 중요하다

Deep Learning에서 Learning Rate는 매우 중요한 Hyperparameter 중 하나이다.

```text
너무 큼
→ 불안정

너무 작음
→ 학습 느림

적절한 값
→ 안정적인 학습 가능
```

---

# 17. Parameter와 Gradient

PyTorch Parameter에는 Gradient가 저장될 수 있다.

예:

```python
for name, param in model.named_parameters():

    print(
        name,
        param.grad
    )
```

---

# 18. backward 전에는?

Training 시작 직후 아직:

```python
loss.backward()
```

를 실행하지 않았다면:

```python
param.grad
```

가 `None`일 수 있다.

---

# 19. backward 후

```python
loss.backward()
```

를 실행하면 계산에 참여한 Parameter의 `.grad`에 Gradient가 저장된다.

---

# 20. 간단한 Autograd 실습

```python
import torch


w = torch.tensor(
    2.0,
    requires_grad=True
)


loss = w ** 2


loss.backward()


print(
    w.grad
)
```

---

# 21. 결과

수식:

```text
loss = w²
```

미분:

```text
d(loss) / dw
=
2w
```

현재:

```text
w = 2
```

이므로:

```text
gradient = 4
```

이다.

---

# 22. requires_grad

```python
requires_grad=True
```

는 해당 Tensor에 대해 Gradient 계산을 추적하도록 설정한다.

Neural Network의 학습 가능한 Parameter는 일반적으로 PyTorch가 자동으로 Gradient 계산 대상으로 관리한다.

---

# 23. Computation Graph

PyTorch는 Forward 과정에서 연산 관계를 추적한다.

예:

```text
x
↓
Linear
↓
ReLU
↓
Linear
↓
Loss
```

이 연산 관계를 이용해 Backward 과정에서 Gradient를 계산한다.

이를 이해할 때:

```text
Computation Graph
```

라는 개념을 사용할 수 있다.

---

# 24. Chain Rule

Backpropagation의 수학적 핵심에는:

```text
Chain Rule
```

즉 연쇄법칙이 있다.

여러 함수가 연결되어 있을 때 미분을 연결해서 계산한다.

---

# 25. 예

```text
x
↓
f
↓
h
↓
g
↓
y
```

라면 각 단계의 미분을 연결하여 앞쪽 변수에 대한 영향을 계산할 수 있다.

처음에는 수식을 모두 외우기보다:

```text
Loss에서 시작해
각 Layer의 Gradient를 뒤에서 앞으로 계산한다.
```

라고 이해하면 된다.

---

# 26. Backpropagation의 목적

Backpropagation 자체가 Weight를 수정하는 것은 아니다.

Backpropagation은:

```text
Gradient 계산
```

을 담당한다.

실제 Weight Update는:

```text
Optimizer
```

가 담당한다.

---

# 27. 매우 중요한 구분

```python
loss.backward()
```

의 역할:

```text
Gradient 계산
```

이다.

```python
optimizer.step()
```

의 역할:

```text
Parameter Update
```

이다.

---

# 28. Optimizer란?

Optimizer는 Gradient를 이용해 Model의 Parameter를 업데이트하는 알고리즘이다.

대표적으로:

```text
SGD

Adam

AdamW
```

등이 있다.

---

# 29. PyTorch Optimizer

```python
import torch.optim as optim
```

또는:

```python
optimizer = torch.optim.Adam(
    model.parameters(),
    lr=0.001
)
```

처럼 사용한다.

---

# 30. model.parameters()

```python
model.parameters()
```

는 Model의 학습 가능한 Parameter들을 Optimizer에 전달한다.

대표적으로:

```text
Weight

Bias
```

이다.

---

# 31. Adam 예

```python
optimizer = torch.optim.Adam(
    model.parameters(),
    lr=0.001
)
```

의미:

```text
Optimizer
→ Adam

학습 대상
→ model.parameters()

Learning Rate
→ 0.001
```

이다.

---

# 32. 기본 Training Step

```python
optimizer.zero_grad()

outputs = model(
    x
)

loss = loss_fn(
    outputs,
    y
)

loss.backward()

optimizer.step()
```

이 다섯 단계는 매우 중요하다.

---

# 33. 1단계 zero_grad

```python
optimizer.zero_grad()
```

이전 Training Step에서 계산된 Gradient를 초기화한다.

---

# 34. 왜 초기화할까?

PyTorch에서는 기본적으로 Gradient가 누적될 수 있다.

따라서 일반적인 Training에서는 Batch마다 이전 Gradient를 지운다.

---

# 35. 예

첫 번째 Backward:

```text
Gradient = 2
```

다음 Backward 전에 초기화하지 않고 또:

```text
Gradient = 3
```

이 계산되면 개념적으로 누적되어:

```text
5
```

가 될 수 있다.

---

# 36. 그래서 일반적인 Training에서는

```python
optimizer.zero_grad()
```

를 사용한다.

---

# 37. Gradient Accumulation

Gradient 누적이 항상 잘못된 것은 아니다.

의도적으로 여러 Batch의 Gradient를 누적한 뒤 한 번 업데이트하는 방법도 있다.

이를:

```text
Gradient Accumulation
```

이라고 한다.

---

# 38. 하지만 기본 학습에서는

처음에는:

```python
optimizer.zero_grad()
```

를 각 Training Step에 사용하는 방식부터 익힌다.

---

# 39. 2단계 Forward

```python
outputs = model(
    x
)
```

Input을 Model에 넣어 Prediction을 계산한다.

---

# 40. 3단계 Loss

```python
loss = loss_fn(
    outputs,
    y
)
```

Prediction과 Target을 비교한다.

---

# 41. 4단계 Backward

```python
loss.backward()
```

Gradient를 계산한다.

---

# 42. 5단계 Step

```python
optimizer.step()
```

계산된 Gradient를 이용해 Parameter를 업데이트한다.

---

# 43. 전체 해석

```text
optimizer.zero_grad()
→ 이전 Gradient 제거

model(x)
→ Prediction

loss_fn(...)
→ 오차 계산

loss.backward()
→ Gradient 계산

optimizer.step()
→ Weight / Bias 수정
```

---

# 44. 순서가 중요하다

기본적인 흐름:

```text
zero_grad
↓
forward
↓
loss
↓
backward
↓
step
```

이다.

---

# 45. optimizer.step()을 빼면?

```python
loss.backward()
```

까지 했더라도:

```python
optimizer.step()
```

을 실행하지 않으면 Parameter가 실제로 업데이트되지 않는다.

즉 Training이 진행되지 않는다.

---

# 46. backward를 빼면?

Gradient가 계산되지 않았으므로 Optimizer가 정상적인 Gradient Update를 수행할 수 없다.

---

# 47. zero_grad를 빼면?

Gradient가 의도치 않게 계속 누적될 수 있다.

따라서 일반적인 Training 결과가 예상과 달라질 수 있다.

---

# 48. SGD

SGD:

```text
Stochastic Gradient Descent
```

이다.

PyTorch:

```python
optimizer = torch.optim.SGD(
    model.parameters(),
    lr=0.01
)
```

---

# 49. 왜 Stochastic인가?

전체 Dataset을 한 번에 사용하는 대신 Mini-batch 등을 이용해 Gradient를 추정하며 Parameter를 업데이트할 수 있다.

Deep Learning에서는 Mini-batch Training이 일반적이다.

---

# 50. 기본 SGD

개념적으로:

```text
Weight
=
Weight
-
Learning Rate × Gradient
```

방식으로 이해할 수 있다.

---

# 51. SGD 장점

```text
구조가 단순함

동작 원리를 이해하기 쉬움

문제에 따라 좋은 일반화 성능
```

등의 장점이 있다.

---

# 52. SGD 단점

문제에 따라:

```text
학습 속도가 느릴 수 있음

Learning Rate 설정에 민감할 수 있음

복잡한 Loss Surface에서 최적화가 어려울 수 있음
```

등이 있다.

---

# 53. Momentum

SGD에 Momentum을 추가할 수 있다.

```python
optimizer = torch.optim.SGD(
    model.parameters(),
    lr=0.01,
    momentum=0.9
)
```

---

# 54. Momentum 개념

현재 Gradient만 보는 것이 아니라 이전 Update 방향의 정보를 함께 이용한다.

개념적으로:

```text
공이 경사면을 내려가면서
관성을 얻는 것
```

처럼 생각할 수 있다.

---

# 55. Momentum의 목적

상황에 따라:

```text
진동 감소

더 빠른 이동

안정적인 최적화
```

에 도움을 줄 수 있다.

---

# 56. Adam

Deep Learning에서 매우 자주 보는 Optimizer이다.

```python
optimizer = torch.optim.Adam(
    model.parameters(),
    lr=0.001
)
```

---

# 57. Adam의 특징

Adam은 Gradient의 과거 정보 등을 이용해 Parameter별 Update 크기를 조절한다.

초보 단계에서는:

```text
SGD보다 자동으로 조절되는 부분이 많고
많은 문제에서 좋은 시작점으로 사용됨
```

정도로 이해하면 된다.

---

# 58. Adam이 항상 최고인가?

아니다.

Dataset, Model, Task에 따라:

```text
SGD

Adam

AdamW

기타 Optimizer
```

의 결과가 다를 수 있다.

---

# 59. Optimizer도 Hyperparameter

어떤 Optimizer를 사용할지는 Model이 자동으로 학습하는 Parameter가 아니다.

사람이 선택한다.

따라서:

```text
Hyperparameter
```

와 관련된다.

---

# 60. AdamW

PyTorch:

```python
optimizer = torch.optim.AdamW(
    model.parameters(),
    lr=0.001,
    weight_decay=0.01
)
```

이다.

---

# 61. Adam과 AdamW

둘은 비슷해 보이지만 Weight Decay를 처리하는 방식에 차이가 있다.

현대 Deep Learning에서는 AdamW도 자주 사용된다.

처음에는:

```text
Adam
→ 자주 사용하는 기본 Optimizer

AdamW
→ Weight Decay를 분리해 적용하는 Adam 계열 Optimizer
```

정도로 구분한다.

---

# 62. Weight Decay

Weight Decay는 Parameter가 지나치게 커지는 것을 억제하는 Regularization 기법과 관련된다.

예:

```python
optimizer = torch.optim.AdamW(
    model.parameters(),
    lr=0.001,
    weight_decay=0.01
)
```

---

# 63. 목적

Model이 Training Data에 지나치게 복잡하게 맞춰지는 것을 줄이는 데 도움이 될 수 있다.

즉:

```text
Overfitting 완화
```

를 위한 방법 중 하나로 활용할 수 있다.

---

# 64. Weight Decay가 크면 무조건 좋은가?

아니다.

너무 강하면 Model 학습 자체를 방해할 수 있다.

따라서:

```text
Validation 성능
```

을 확인하면서 조절한다.

---

# 65. Optimizer 비교

| Optimizer | 특징 |
|---|---|
| SGD | 기본적인 Gradient Descent 계열 |
| SGD + Momentum | 이전 Update 방향 정보 활용 |
| Adam | Parameter별 적응적 Update |
| AdamW | Adam 계열 + 분리된 Weight Decay |

---

# 66. 기본 시작점

처음 Deep Learning Model을 실험한다면 예를 들어:

```python
optimizer = torch.optim.Adam(
    model.parameters(),
    lr=0.001
)
```

을 시작점으로 사용할 수 있다.

하지만 항상 최적이라는 뜻은 아니다.

---

# 67. Image Model에서는

Transfer Learning이나 Fine-tuning에서는 더 작은 Learning Rate를 사용하는 경우가 많다.

예:

```python
optimizer = torch.optim.Adam(
    model.parameters(),
    lr=1e-4
)
```

---

# 68. 1e-4 의미

```text
1e-4
=
0.0001
```

이다.

---

# 69. 자주 보는 표현

```text
1e-2 = 0.01

1e-3 = 0.001

1e-4 = 0.0001

1e-5 = 0.00001
```

이다.

---

# 70. Fine-tuning에서 작은 LR을 쓰는 이유

Pretrained Model은 이미 유용한 Weight를 가지고 있다.

너무 큰 Learning Rate로 Update하면 기존에 학습된 유용한 Feature가 크게 변할 수 있다.

그래서 Fine-tuning에서는 작은 Learning Rate를 고려한다.

---

# 71. Freeze된 Parameter

```python
param.requires_grad = False
```

로 Freeze한 Parameter는 Gradient 기반 Update 대상에서 제외된다.

---

# 72. 확인

```python
for name, param in model.named_parameters():

    print(
        name,
        param.requires_grad
    )
```

---

# 73. Trainable Parameter만 Optimizer에 전달

필요하면:

```python
optimizer = torch.optim.Adam(
    filter(
        lambda p: p.requires_grad,
        model.parameters()
    ),
    lr=0.001
)
```

처럼 학습 가능한 Parameter만 전달할 수도 있다.

---

# 74. Transfer Learning 예

```text
Pretrained Backbone
→ Freeze

Classifier
→ Trainable
```

이면 Optimizer는 주로 Classifier Parameter를 업데이트한다.

---

# 75. Parameter Group

서로 다른 Layer에 다른 Learning Rate를 사용할 수도 있다.

예:

```python
optimizer = torch.optim.Adam(
    [
        {
            "params": model.backbone.parameters(),
            "lr": 1e-5
        },
        {
            "params": model.classifier.parameters(),
            "lr": 1e-3
        }
    ]
)
```

---

# 76. 의미

```text
Backbone
→ 작은 Learning Rate

Classifier
→ 더 큰 Learning Rate
```

를 사용할 수 있다.

Fine-tuning에서 활용할 수 있는 방법이다.

---

# 77. Gradient 확인

```python
for name, param in model.named_parameters():

    if param.grad is not None:

        print(
            name,
            param.grad.mean()
        )
```

---

# 78. Gradient가 None이라면

가능한 원인:

```text
아직 backward를 하지 않음

requires_grad=False

Forward 계산에 Parameter가 사용되지 않음

torch.no_grad() 내부에서 계산함
```

등이 있다.

---

# 79. Gradient가 0이라면

항상 오류라는 뜻은 아니다.

Activation이나 현재 Input, Model 상태 등에 따라 Gradient가 0일 수도 있다.

하지만 많은 Layer에서 계속 0에 가까운 Gradient가 발생한다면 학습 문제를 확인할 수 있다.

---

# 80. Vanishing Gradient

앞에서 배운 것처럼 Gradient가 앞쪽 Layer로 갈수록 매우 작아지는 문제를:

```text
Vanishing Gradient
```

라고 한다.

---

# 81. Exploding Gradient

반대로 Gradient가 지나치게 커지는 현상을:

```text
Exploding Gradient
```

라고 한다.

---

# 82. Exploding Gradient 문제

Gradient가 지나치게 커지면:

```text
Training 불안정

Loss 급증

NaN 발생
```

등이 나타날 수 있다.

---

# 83. Gradient Clipping

Gradient가 너무 커지는 것을 제한하는 방법이 있다.

예:

```python
torch.nn.utils.clip_grad_norm_(
    model.parameters(),
    max_norm=1.0
)
```

---

# 84. 위치

일반적으로:

```python
loss.backward()

torch.nn.utils.clip_grad_norm_(
    model.parameters(),
    max_norm=1.0
)

optimizer.step()
```

처럼 Backward 이후, Step 이전에 사용한다.

---

# 85. 항상 Gradient Clipping을 해야 하나?

아니다.

필요한 상황에서 사용하는 기법이다.

무조건 넣는다고 성능이 좋아지는 것은 아니다.

---

# 86. Gradient Accumulation

GPU Memory 때문에 큰 Batch를 한 번에 넣기 어려울 수 있다.

예:

```text
원하는 Batch Size
128

GPU에서 가능한 Batch Size
32
```

라면 여러 Mini-batch의 Gradient를 누적하는 방법을 사용할 수 있다.

---

# 87. 개념

```text
Batch 1
Backward

Batch 2
Backward

Batch 3
Backward

Batch 4
Backward

↓

Optimizer Step
```

처럼 사용할 수 있다.

---

# 88. 기본 예

```python
accumulation_steps = 4


optimizer.zero_grad()


for step, (
    x,
    y
) in enumerate(
    train_loader
):

    outputs = model(
        x
    )

    loss = loss_fn(
        outputs,
        y
    )

    loss = (
        loss
        /
        accumulation_steps
    )

    loss.backward()

    if (
        step + 1
    ) % accumulation_steps == 0:

        optimizer.step()

        optimizer.zero_grad()
```

---

# 89. 왜 Loss를 나누는가?

여러 Batch의 Gradient를 누적할 때 Update 크기가 지나치게 커지는 것을 피하고 평균 Gradient와 비슷한 Scale을 유지하기 위해:

```python
loss / accumulation_steps
```

를 사용하는 방식이 흔하다.

---

# 90. 마지막 Batch 처리

Dataset Batch 수가 `accumulation_steps`로 정확히 나누어지지 않으면 마지막에 남은 Gradient도 Step해야 한다.

실제 구현에서는 이 부분도 처리해야 한다.

---

# 91. 초보 단계에서는

Gradient Accumulation은 나중에 GPU Memory 문제가 생겼을 때 다시 자세히 사용하면 된다.

먼저 기본:

```text
한 Batch
↓
zero_grad
↓
forward
↓
loss
↓
backward
↓
step
```

을 확실히 이해한다.

---

# 92. Optimizer State

Adam 같은 Optimizer는 Parameter뿐 아니라 Optimization에 필요한 내부 상태도 가지고 있다.

그래서 Training을 정확히 이어서 재개하려면:

```text
Model State

Optimizer State
```

를 함께 저장할 수 있다.

---

# 93. Optimizer 저장

```python
torch.save(
    optimizer.state_dict(),
    "optimizer.pt"
)
```

---

# 94. Model과 같이 저장

```python
checkpoint = {
    "model_state_dict": model.state_dict(),
    "optimizer_state_dict": optimizer.state_dict(),
    "epoch": epoch
}


torch.save(
    checkpoint,
    "checkpoint.pt"
)
```

---

# 95. 왜 Optimizer State까지 저장할까?

Training을 중간부터 이어갈 때 Adam 등의 내부 상태도 이어서 사용하기 위해서이다.

Inference만 한다면 일반적으로 Model Weight가 핵심이다.

---

# 96. Scheduler란?

Learning Rate를 Training 중 변경하는 기능을:

```text
Learning Rate Scheduler
```

라고 한다.

Optimizer와는 역할이 다르다.

---

# 97. Optimizer vs Scheduler

Optimizer:

```text
Gradient를 이용해
Parameter Update
```

Scheduler:

```text
Training 진행에 따라
Learning Rate 조절
```

이다.

---

# 98. 예

초기:

```text
lr = 0.001
```

Training 후반:

```text
lr = 0.0001
```

처럼 줄일 수 있다.

---

# 99. StepLR

예:

```python
scheduler = torch.optim.lr_scheduler.StepLR(
    optimizer,
    step_size=5,
    gamma=0.1
)
```

---

# 100. 의미

```text
5 Epoch마다

Learning Rate × 0.1
```

이다.

예:

```text
0.001
↓
0.0001
↓
0.00001
```

---

# 101. scheduler.step()

보통 Scheduler 종류에 맞는 위치에서:

```python
scheduler.step()
```

을 호출한다.

Scheduler마다 호출 방식이 다를 수 있으므로 해당 Scheduler의 사용법을 확인해야 한다.

---

# 102. ReduceLROnPlateau

Validation Metric 개선이 멈췄을 때 Learning Rate를 줄이는 Scheduler도 있다.

예:

```python
scheduler = torch.optim.lr_scheduler.ReduceLROnPlateau(
    optimizer,
    mode="min",
    patience=2,
    factor=0.1
)
```

---

# 103. 사용

Validation Loss를 기준으로 한다면:

```python
scheduler.step(
    val_loss
)
```

처럼 사용할 수 있다.

---

# 104. Scheduler가 반드시 필요한가?

아니다.

처음에는 고정 Learning Rate로 Baseline을 만들 수 있다.

그 후 필요하면 Scheduler를 실험한다.

---

# 105. Hyperparameter 관계

Optimizer와 관련해 자주 조절하는 값:

```text
Optimizer 종류

Learning Rate

Momentum

Weight Decay

Scheduler

Gradient Clipping
```

등이 있다.

---

# 106. 한 번에 다 바꾸지 않는다

Experiment를 할 때:

```text
Baseline
↓
Learning Rate 변경
↓
Optimizer 변경
↓
Weight Decay 변경
```

처럼 가능한 한 변경점을 관리하는 것이 좋다.

---

# 107. 왜?

동시에 여러 값을 바꾸면:

```text
무엇 때문에 성능이 변했는가?
```

를 알기 어렵다.

---

# 108. 기본 실습 Model

```python
import torch
import torch.nn as nn


model = nn.Sequential(
    nn.Linear(
        10,
        32
    ),

    nn.ReLU(),

    nn.Linear(
        32,
        2
    )
)
```

---

# 109. Loss

```python
loss_fn = nn.CrossEntropyLoss()
```

---

# 110. Optimizer

```python
optimizer = torch.optim.Adam(
    model.parameters(),
    lr=0.001
)
```

---

# 111. Dummy Data

```python
x = torch.randn(
    32,
    10
)


y = torch.randint(
    0,
    2,
    (
        32,
    )
)
```

---

# 112. Training Step

```python
optimizer.zero_grad()


outputs = model(
    x
)


loss = loss_fn(
    outputs,
    y
)


loss.backward()


optimizer.step()
```

---

# 113. Loss 출력

```python
print(
    loss.item()
)
```

---

# 114. 실제 Weight가 바뀌는지 확인

Training Step 전 Weight를 복사한다.

```python
before = model[
    0
].weight.detach().clone()
```

---

# 115. Training Step 실행

```python
optimizer.zero_grad()

outputs = model(
    x
)

loss = loss_fn(
    outputs,
    y
)

loss.backward()

optimizer.step()
```

---

# 116. 이후 Weight

```python
after = model[
    0
].weight.detach().clone()
```

---

# 117. 비교

```python
print(
    torch.equal(
        before,
        after
    )
)
```

일반적으로 Update가 발생했다면:

```text
False
```

가 나온다.

즉 Weight가 실제로 바뀐 것이다.

---

# 118. 변화량 확인

```python
difference = (
    after
    -
    before
)


print(
    difference.abs().mean()
)
```

Parameter가 얼마나 변화했는지 확인할 수 있다.

---

# 119. backward까지만 하고 확인

```python
optimizer.zero_grad()

outputs = model(
    x
)

loss = loss_fn(
    outputs,
    y
)

loss.backward()
```

이 시점에는 Gradient는 계산됐지만:

```text
optimizer.step()
```

을 아직 실행하지 않았다.

---

# 120. 따라서

```text
Backward
→ Gradient 생성

Step
→ Parameter 변경
```

이라는 차이를 직접 실험할 수 있다.

---

# 121. Gradient 확인

```python
print(
    model[
        0
    ].weight.grad
)
```

---

# 122. Step 전 Weight

```python
weight_before_step = model[
    0
].weight.detach().clone()
```

---

# 123. Step

```python
optimizer.step()
```

---

# 124. Step 후 Weight

```python
weight_after_step = model[
    0
].weight.detach().clone()
```

---

# 125. 비교

```python
print(
    torch.equal(
        weight_before_step,
        weight_after_step
    )
)
```

이 실습을 하면:

```text
backward
```

와:

```text
step
```

의 역할 차이를 명확히 확인할 수 있다.

---

# 126. detach()

```python
tensor.detach()
```

는 현재 Tensor를 Autograd Graph에서 분리한 Tensor를 얻을 때 사용한다.

Weight를 단순 비교하거나 NumPy 변환 등을 할 때 자주 볼 수 있다.

---

# 127. clone()

```python
tensor.clone()
```

은 Tensor 값을 복사한다.

따라서:

```python
param.detach().clone()
```

은 특정 시점의 Parameter 값을 별도로 저장해 비교할 때 유용하다.

---

# 128. Optimizer가 제대로 연결되었는지 확인

Model을 만든 뒤 Optimizer를 만든다.

```python
model = MyModel()


optimizer = torch.optim.Adam(
    model.parameters(),
    lr=0.001
)
```

---

# 129. Model Layer를 나중에 교체하면 주의

Optimizer 생성 후 새로운 Layer로 교체하면 새 Parameter가 기존 Optimizer에 자동으로 포함되지 않는 상황이 생길 수 있다.

예:

```python
optimizer = torch.optim.Adam(
    model.parameters(),
    lr=0.001
)


model.fc = nn.Linear(
    128,
    2
)
```

이런 구조 변경 후에는 Optimizer Parameter 구성을 다시 확인하고 필요하면 Optimizer를 다시 생성한다.

---

# 130. Transfer Learning에서 중요

Pretrained Model의 Classifier를 교체한 뒤:

```python
model.fc = nn.Linear(
    in_features,
    2
)
```

그 다음 Optimizer를 만드는 순서가 이해하기 쉽다.

```text
Model 불러오기
↓
Layer 교체
↓
Freeze / Unfreeze 설정
↓
Optimizer 생성
```

---

# 131. 잘못된 순서 예

Optimizer를 먼저 만들고 나중에 Trainable Parameter 구조를 크게 변경하면 의도한 Parameter가 Optimizer에 제대로 연결됐는지 확인이 필요하다.

---

# 132. Training Mode

Training 전에:

```python
model.train()
```

을 사용한다.

Optimizer와 직접 같은 기능은 아니지만 Dropout, BatchNorm 등이 Training Mode로 동작하도록 한다.

---

# 133. Validation에서는

```python
model.eval()
```

그리고:

```python
with torch.no_grad():
```

를 사용한다.

---

# 134. Validation에서 하지 않는 것

일반적인 Validation에서는:

```text
loss.backward()

optimizer.step()
```

을 하지 않는다.

---

# 135. 이유

Validation Data는:

```text
Model 평가
```

를 위한 데이터이기 때문이다.

Validation Data로 Weight를 Update하면 평가 데이터가 Training에 사용되는 문제가 생긴다.

---

# 136. Training vs Validation

Training:

```text
model.train()

Forward

Loss

Backward

Optimizer Step
```

Validation:

```text
model.eval()

no_grad

Forward

Loss / Metric

Parameter Update 없음
```

이다.

---

# 137. 기본 Training Loop 미리 보기

```python
for epoch in range(
    epochs
):

    model.train()

    for x, y in train_loader:

        x = x.to(
            device
        )

        y = y.to(
            device
        )

        optimizer.zero_grad()

        outputs = model(
            x
        )

        loss = loss_fn(
            outputs,
            y
        )

        loss.backward()

        optimizer.step()
```

---

# 138. Epoch마다 이 과정이 반복

```text
Epoch 1
→ 모든 Training Batch 학습

Epoch 2
→ 다시 모든 Training Batch 학습

Epoch 3
→ 다시 모든 Training Batch 학습
```

이다.

---

# 139. Batch마다 Weight가 바뀐다

일반적인 Mini-batch Training에서는:

```text
Batch 1
→ Weight Update

Batch 2
→ Weight Update

Batch 3
→ Weight Update
```

처럼 Batch마다 `optimizer.step()`이 실행된다.

---

# 140. Epoch 끝에서만 Weight가 바뀌는 것이 아니다

이 부분을 헷갈리지 않는다.

일반적인 Training Loop에서는:

```text
각 Batch
```

마다 Parameter가 Update된다.

---

# 141. Step의 두 가지 의미 주의

Deep Learning 코드에서 `step`이라는 단어가 여러 의미로 사용될 수 있다.

```python
optimizer.step()
```

은:

```text
Optimizer Parameter Update
```

이다.

반면:

```python
for step, batch in enumerate(
    train_loader
):
```

의 `step`은 단순히 Batch 반복 번호를 의미할 수 있다.

---

# 142. Epoch와 Step

예:

```text
Dataset = 1000 Samples

Batch Size = 100
```

이면 한 Epoch에 대략:

```text
10 Steps
```

가 있다.

---

# 143. 5 Epoch라면

대략:

```text
10 Steps × 5 Epoch
=
50 Parameter Updates
```

가 발생할 수 있다.

마지막 Batch나 `drop_last` 등의 설정에 따라 정확한 횟수는 달라질 수 있다.

---

# 144. Batch Size와 Optimization

Batch Size도 Gradient 특성에 영향을 준다.

작은 Batch:

```text
Gradient 변동이 상대적으로 클 수 있음

Memory 사용 감소
```

큰 Batch:

```text
Gradient가 더 안정적으로 추정될 수 있음

Memory 사용 증가
```

등의 차이가 있다.

---

# 145. Batch Size와 Learning Rate

Batch Size를 크게 변경하면 적절한 Learning Rate도 달라질 수 있다.

따라서 Batch Size와 Learning Rate는 완전히 독립적인 Hyperparameter라고만 생각하지 않는다.

---

# 146. Optimizer Debugging

Training이 안 될 때 확인한다.

```text
Loss가 변하는가?

Learning Rate가 너무 작은가?

Learning Rate가 너무 큰가?

loss.backward()를 했는가?

optimizer.step()을 했는가?

zero_grad() 위치가 맞는가?

Parameter가 requires_grad=True인가?

Optimizer가 올바른 Parameter를 가지고 있는가?

Model이 train() 상태인가?

Input / Target이 정상인가?
```

---

# 147. Weight가 정말 바뀌는지 확인

```python
for name, param in model.named_parameters():

    print(
        name,
        param.requires_grad
    )
```

그리고 Training Step 전후 Parameter를 비교할 수 있다.

---

# 148. Learning Rate 확인

```python
for group in optimizer.param_groups:

    print(
        group["lr"]
    )
```

Scheduler를 사용할 때 현재 Learning Rate를 확인하는 데도 유용하다.

---

# 149. Loss가 전혀 줄지 않는 경우

가능한 원인:

```text
Learning Rate 문제

잘못된 Label

잘못된 Loss

Output / Target Shape 문제

Data Scaling 문제

Gradient가 전달되지 않음

Layer Freeze 문제

Model Capacity 문제

데이터 자체 문제
```

등이 있다.

---

# 150. Loss가 NaN이 되는 경우

확인:

```text
Learning Rate 너무 큼

Input NaN / Inf

Target NaN / Inf

Gradient Explosion

수치적으로 불안정한 계산
```

등이다.

---

# 151. Optimizer만 바꾸면 해결될까?

항상 그렇지 않다.

Training 문제의 원인이:

```text
데이터

Model

Loss

Label

Preprocessing

Learning Rate
```

등 다른 곳에 있을 수 있다.

Optimizer만 계속 바꾸는 것은 좋은 Debugging 방법이 아니다.

---

# 152. 실험 순서 예

Baseline:

```text
Adam
lr = 0.001
```

실험 A:

```text
Adam
lr = 0.0001
```

실험 B:

```text
AdamW
lr = 0.0001
weight_decay = 0.01
```

실험 C:

```text
SGD
lr = 0.01
momentum = 0.9
```

처럼 기록할 수 있다.

---

# 153. 같은 Validation 조건 사용

Optimizer를 비교할 때:

```text
같은 Train / Validation Split

같은 Model

같은 Epoch

가능하면 같은 Seed
```

등을 유지해야 비교가 쉬워진다.

---

# 154. Optimizer와 Overfitting

Optimizer는 Training 과정에 영향을 주지만 Overfitting 문제를 Optimizer 하나만으로 해결하는 것은 아니다.

함께 고려할 수 있는 것:

```text
Data Augmentation

Dropout

Weight Decay

Early Stopping

Model Size

더 많은 Data
```

등이다.

---

# 155. Fine-tuning과 연결

Transfer Learning에서는:

```text
Stage 1
Backbone Freeze
Classifier Training

↓

Stage 2
일부 또는 전체 Unfreeze
작은 Learning Rate로 Fine-tuning
```

같은 전략을 사용할 수 있다.

---

# 156. Stage 1 예

```python
for param in model.features.parameters():

    param.requires_grad = False
```

Classifier만 Trainable하게 한다.

---

# 157. Optimizer

```python
optimizer = torch.optim.Adam(
    filter(
        lambda p: p.requires_grad,
        model.parameters()
    ),
    lr=1e-3
)
```

---

# 158. Stage 2

일부 Parameter를 다시:

```python
param.requires_grad = True
```

로 변경한다.

---

# 159. Fine-tuning Optimizer

예:

```python
optimizer = torch.optim.Adam(
    filter(
        lambda p: p.requires_grad,
        model.parameters()
    ),
    lr=1e-5
)
```

처럼 더 작은 Learning Rate를 사용할 수 있다.

---

# 160. 왜 Optimizer를 다시 만들기도 할까?

Freeze / Unfreeze 상태가 바뀌고 학습할 Parameter 구성이 변경되었기 때문이다.

새로운 Training Stage에서 Optimizer를 다시 구성하면 어떤 Parameter를 학습하는지 명확하게 관리할 수 있다.

---

# 161. Model Training 핵심 5줄

Deep Learning에서 계속 보게 되는 코드:

```python
optimizer.zero_grad()

outputs = model(
    x
)

loss = loss_fn(
    outputs,
    y
)

loss.backward()

optimizer.step()
```

---

# 162. 이 코드를 한국어로 읽기

```text
이전 Gradient를 지운다.

↓

Model이 예측한다.

↓

얼마나 틀렸는지 계산한다.

↓

어느 방향으로 고쳐야 하는지 계산한다.

↓

실제로 Weight를 수정한다.
```

---

# 163. 전체 관계

```text
Model
↓
Forward
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
다음 Batch
```

---

# 164. Parameter와 Hyperparameter 다시 구분

Parameter:

```text
Weight

Bias
```

Model이 Training을 통해 학습한다.

Hyperparameter:

```text
Learning Rate

Optimizer 종류

Weight Decay

Momentum

Batch Size

Epoch
```

사람이 설정하거나 실험한다.

---

# 165. Backpropagation과 Optimizer 차이

Backpropagation:

```text
Gradient를 계산하는 과정
```

Optimizer:

```text
Gradient를 이용해
Parameter를 수정하는 방법
```

이다.

둘은 같은 것이 아니다.

---

# 166. Loss와 Gradient 관계

```text
Loss
↓
Backward
↓
Gradient
```

이다.

Gradient는:

```text
Loss를 줄이기 위해
Parameter를 어느 방향으로 변경할지
```

판단하는 데 사용된다.

---

# 167. Gradient와 Optimizer 관계

```text
Gradient
↓
Optimizer
↓
Parameter Update
```

이다.

---

# 168. Optimizer와 Learning Rate 관계

Optimizer가 Update 방법을 결정하고 Learning Rate가 Update 크기에 큰 영향을 준다.

```text
Optimizer
+
Learning Rate
```

를 함께 생각한다.

---

# 169. 전체 Deep Learning Training 구조

```text
Dataset
↓
DataLoader
↓
Batch
↓
Model
↓
Forward
↓
Logits / Prediction
↓
Loss Function
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
↓
반복
```

---

# 170. 최종 정리

Backpropagation은:

```text
Loss를 기준으로
각 Parameter의 Gradient를 계산하는 과정
```

이다.

PyTorch에서는:

```python
loss.backward()
```

를 사용한다.

하지만 `backward()`만으로 Weight가 바뀌는 것은 아니다.

실제 Parameter Update는:

```python
optimizer.step()
```

이 수행한다.

일반적인 Training Step:

```python
optimizer.zero_grad()

outputs = model(
    x
)

loss = loss_fn(
    outputs,
    y
)

loss.backward()

optimizer.step()
```

이다.

각각의 의미:

```text
zero_grad
→ 이전 Gradient 초기화

Forward
→ Prediction 계산

Loss
→ Prediction Error 계산

Backward
→ Gradient 계산

Step
→ Parameter Update
```

Optimizer의 대표적인 종류:

```text
SGD

SGD + Momentum

Adam

AdamW
```

가 있다.

Learning Rate:

```text
Parameter를 한 번에
얼마나 수정할 것인지
```

에 큰 영향을 주는 중요한 Hyperparameter이다.

그리고 반드시 구분한다.

```text
loss.backward()
→ Gradient 계산
```

```text
optimizer.step()
→ Weight / Bias Update
```

이제 Model이 실제로 어떻게 학습되는지에 필요한 핵심 부품이 모두 연결되었다.

다음 단계에서는 이 부품들을:

```text
Epoch

Batch

Training

Validation
```

과 결합하여 실제 전체 Training Loop를 만든다.