# Loss Function

## 1. Loss Function이란?

Loss Function은 Model의 Prediction과 실제 정답 사이의 차이를 숫자로 계산하는 함수이다.

한국어로는:

```text
손실 함수
```

라고 한다.

기본 구조:

```text
Input
↓
Model
↓
Prediction
↓
정답과 비교
↓
Loss
```

예:

```text
정답 = Pneumonia

Model Prediction = Normal

↓

Loss 발생
```

---

# 2. 왜 Loss가 필요한가?

Neural Network가 학습하려면:

```text
현재 Prediction이
얼마나 잘못되었는가?
```

를 알아야 한다.

그 기준을 Loss Function이 제공한다.

---

# 3. Training 전체 흐름

```text
Input
↓
Model
↓
Prediction
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
Weight Update
```

즉 Loss는:

```text
Prediction
```

과:

```text
Weight Update
```

를 연결하는 중요한 기준이다.

---

# 4. 가장 기본적인 코드

```python
outputs = model(
    x
)

loss = loss_fn(
    outputs,
    y
)
```

여기서:

```text
outputs
→ Model Prediction

y
→ 정답

loss_fn
→ Loss Function

loss
→ 계산된 손실값
```

이다.

---

# 5. Loss가 작으면?

일반적으로:

```text
Prediction이 정답에 더 잘 맞음
```

을 의미한다.

Training의 기본 목적은:

```text
Loss를 줄이는 방향으로
Model Parameter를 학습
```

하는 것이다.

---

# 6. Loss = 0이면?

사용하는 Loss Function에 따라 의미가 조금 다르지만 일반적으로:

```text
Prediction과 Target 사이의
손실이 매우 작거나 없음
```

을 의미한다.

항상 실제 데이터에서 정확히 0이 되어야 좋은 Model이라는 뜻은 아니다.

---

# 7. Loss와 Accuracy는 다르다

이 부분이 매우 중요하다.

```text
Loss
≠
Accuracy
```

이다.

Accuracy는:

```text
몇 개를 맞혔는가?
```

를 본다.

Loss는:

```text
Prediction이 정답과
얼마나 차이가 나는가?
```

를 더 세밀하게 계산한다.

---

# 8. 예

두 Model이 같은 Sample을 모두 맞혔다고 하자.

정답:

```text
Class 1
```

Model A:

```text
Class 1 Probability = 0.51
```

Model B:

```text
Class 1 Probability = 0.99
```

둘 다 Class Prediction은:

```text
Class 1
```

이므로 Accuracy 관점에서는 둘 다 정답이다.

하지만 Loss는 두 Prediction을 다르게 평가할 수 있다.

---

# 9. 즉

```text
Accuracy
→ 최종 Class를 맞혔는가?

Loss
→ Model Output이 Target에 얼마나 잘 맞는가?
```

라고 생각할 수 있다.

---

# 10. Loss는 학습에 사용

일반적으로:

```python
loss.backward()
```

를 실행하여 Loss를 기준으로 Gradient를 계산한다.

Accuracy에는 일반적으로:

```python
accuracy.backward()
```

같은 방식으로 학습하지 않는다.

---

# 11. Loss Function은 문제에 따라 다르다

대표적인 조합:

```text
Multi-class Classification
→ CrossEntropyLoss

Binary Classification
→ BCEWithLogitsLoss

Multi-label Classification
→ BCEWithLogitsLoss

Regression
→ MSELoss / L1Loss
```

이다.

---

# 12. CrossEntropyLoss

Multi-class Classification에서 매우 많이 사용하는 Loss Function이다.

PyTorch:

```python
loss_fn = nn.CrossEntropyLoss()
```

---

# 13. Multi-class란?

여러 Class 중:

```text
하나
```

를 선택하는 문제이다.

예:

```text
Cat
Dog
Horse
```

중 하나를 예측한다.

---

# 14. Model Output

3개 Class라면 Model 마지막 Layer:

```python
nn.Linear(
    64,
    3
)
```

처럼 만들 수 있다.

Output Shape:

```text
[Batch, 3]
```

이다.

---

# 15. Output은 Raw Logits

예:

```text
Sample 1

[
    2.1,
    -0.5,
    1.2
]
```

이 값들은 Probability가 아니라:

```text
Logits
```

이다.

---

# 16. CrossEntropyLoss에는 Logits를 넣는다

```python
logits = model(
    x
)

loss = loss_fn(
    logits,
    y
)
```

이다.

---

# 17. Softmax를 먼저 하지 않는다

일반적인 PyTorch Multi-class Training에서는:

```python
prob = torch.softmax(
    logits,
    dim=1
)

loss = loss_fn(
    prob,
    y
)
```

처럼 하지 않는다.

`CrossEntropyLoss`에는 raw Logits를 전달한다.

---

# 18. 이유

`CrossEntropyLoss`는 Logits를 입력받아 필요한 계산을 내부적으로 수치적으로 안정된 방식으로 처리한다.

따라서:

```text
Model
↓
Raw Logits
↓
CrossEntropyLoss
```

라고 기억한다.

---

# 19. CrossEntropyLoss Target

예:

```python
y = torch.tensor(
    [
        0,
        2,
        1,
        0
    ]
)
```

Target은 각 Sample의:

```text
Class Index
```

이다.

---

# 20. 예

Class 정의:

```text
0 = Cat
1 = Dog
2 = Horse
```

Target:

```text
[0, 2, 1, 0]
```

의 의미:

```text
Sample 1 → Cat
Sample 2 → Horse
Sample 3 → Dog
Sample 4 → Cat
```

이다.

---

# 21. CrossEntropyLoss Shape

예:

```text
Logits
[32, 10]

Target
[32]
```

이다.

10개 Class Classification의 경우이다.

---

# 22. Target dtype

Class Index Target은 일반적으로:

```python
torch.long
```

이어야 한다.

확인:

```python
print(
    y.dtype
)
```

---

# 23. 변환

필요하다면:

```python
y = y.long()
```

으로 변환할 수 있다.

---

# 24. CrossEntropyLoss 전체 예

```python
import torch
import torch.nn as nn


model = nn.Sequential(
    nn.Linear(
        20,
        64
    ),
    nn.ReLU(),
    nn.Linear(
        64,
        3
    )
)


loss_fn = nn.CrossEntropyLoss()


x = torch.randn(
    32,
    20
)


y = torch.randint(
    0,
    3,
    (
        32,
    )
)


logits = model(
    x
)


loss = loss_fn(
    logits,
    y
)


print(
    loss.item()
)
```

---

# 25. Prediction

Training Loss 계산과 별개로 Prediction Class는:

```python
pred = logits.argmax(
    dim=1
)
```

으로 구할 수 있다.

---

# 26. Probability

Probability가 필요하면:

```python
prob = torch.softmax(
    logits,
    dim=1
)
```

을 사용한다.

---

# 27. 정리

```text
Training
Model
↓
Logits
↓
CrossEntropyLoss
```

Prediction:

```text
Logits
↓
argmax
↓
Class
```

Probability:

```text
Logits
↓
Softmax
↓
Probability
```

---

# 28. BCEWithLogitsLoss

Binary Classification에서 매우 중요한 Loss Function이다.

```python
loss_fn = nn.BCEWithLogitsLoss()
```

---

# 29. Binary Classification

예:

```text
0 = Normal

1 = Pneumonia
```

처럼 두 가지 결과를 예측한다.

---

# 30. 1-Logit 방식

Model 마지막 Layer:

```python
nn.Linear(
    64,
    1
)
```

Output:

```text
1 Logit
```

이다.

---

# 31. Model 예

```python
model = nn.Sequential(
    nn.Linear(
        20,
        64
    ),
    nn.ReLU(),
    nn.Linear(
        64,
        1
    )
)
```

---

# 32. BCEWithLogitsLoss에는 Raw Logit

```python
logits = model(
    x
)


loss = loss_fn(
    logits,
    y
)
```

이다.

---

# 33. Sigmoid를 먼저 하지 않는다

다음처럼 하지 않는다.

```python
prob = torch.sigmoid(
    logits
)

loss = loss_fn(
    prob,
    y
)
```

`BCEWithLogitsLoss`는 raw Logit을 받는다.

---

# 34. BCEWithLogitsLoss 의미

이름을 나눠보면:

```text
BCE
+
With
+
Logits
+
Loss
```

이다.

즉:

```text
Binary Cross Entropy
+
Logit 처리
```

를 함께 수행한다.

---

# 35. Target dtype

BCE 계열에서는 일반적으로 Target을 Float 형태로 준비한다.

예:

```python
y = y.float()
```

---

# 36. Shape도 맞춰야 한다

Model Output:

```text
[32, 1]
```

Target:

```text
[32, 1]
```

처럼 맞출 수 있다.

---

# 37. Target Shape 변경

현재 Target:

```text
[32]
```

이라면:

```python
y = y.float().unsqueeze(
    1
)
```

을 사용하면:

```text
[32, 1]
```

이 된다.

---

# 38. 전체 예

```python
model = nn.Sequential(
    nn.Linear(
        20,
        64
    ),
    nn.ReLU(),
    nn.Linear(
        64,
        1
    )
)


loss_fn = nn.BCEWithLogitsLoss()


x = torch.randn(
    32,
    20
)


y = torch.randint(
    0,
    2,
    (
        32,
        1
    )
).float()


logits = model(
    x
)


loss = loss_fn(
    logits,
    y
)
```

---

# 39. Probability 계산

Inference에서:

```python
prob = torch.sigmoid(
    logits
)
```

이다.

---

# 40. Class Prediction

```python
pred = (
    prob >= 0.5
).long()
```

---

# 41. Binary Classification의 다른 방법

Binary Classification을:

```text
2개의 Logit
```

으로 만들 수도 있다.

Model:

```python
nn.Linear(
    64,
    2
)
```

Loss:

```python
nn.CrossEntropyLoss()
```

이다.

---

# 42. 두 방법 비교

방법 A:

```text
Output = 1

BCEWithLogitsLoss

Sigmoid
```

방법 B:

```text
Output = 2

CrossEntropyLoss

Softmax
```

둘 다 Binary Classification에 사용할 수 있다.

---

# 43. 가장 중요한 것은 일관성

다음이 서로 맞아야 한다.

```text
Model Output

Target Shape

Target dtype

Loss Function
```

---

# 44. Multi-label Classification

Multi-label 문제에서는 여러 Label이 동시에 Positive일 수 있다.

예:

```text
Pneumonia = 1
Effusion = 1
Nodule = 0
```

---

# 45. Output

3개 Label이라면:

```text
[Batch, 3]
```

Logit을 출력할 수 있다.

---

# 46. Loss

```python
loss_fn = nn.BCEWithLogitsLoss()
```

를 사용할 수 있다.

각 Label을 독립적인 Binary 문제처럼 처리한다.

---

# 47. Probability

```python
prob = torch.sigmoid(
    logits
)
```

을 각 Label에 적용한다.

---

# 48. MSELoss

Regression에서 대표적으로 사용하는 Loss이다.

MSE:

```text
Mean Squared Error
```

이다.

---

# 49. 기본 개념

Prediction과 Target의 차이를:

```text
제곱
```

한 뒤 평균을 계산한다.

수식:

```text
MSE
=
평균((Prediction - Target)^2)
```

---

# 50. 예

Target:

```text
10
```

Prediction:

```text
8
```

Error:

```text
8 - 10
=
-2
```

Squared Error:

```text
(-2)^2
=
4
```

이다.

---

# 51. PyTorch

```python
loss_fn = nn.MSELoss()
```

---

# 52. Regression Model

```python
model = nn.Sequential(
    nn.Linear(
        10,
        32
    ),
    nn.ReLU(),
    nn.Linear(
        32,
        1
    )
)
```

---

# 53. Loss 계산

```python
pred = model(
    x
)


loss = loss_fn(
    pred,
    y
)
```

---

# 54. Shape 확인

Regression에서도 Prediction과 Target Shape를 맞추는 것이 중요하다.

예:

```text
Prediction
[32, 1]

Target
[32, 1]
```

이다.

---

# 55. MSE의 특징

큰 Error를 제곱하기 때문에:

```text
큰 오차에 더 큰 벌점
```

을 준다.

따라서 큰 Error에 민감하다.

---

# 56. L1Loss

PyTorch:

```python
loss_fn = nn.L1Loss()
```

이다.

L1 Loss는 Absolute Error를 사용한다.

---

# 57. 개념

```text
|Prediction - Target|
```

의 평균을 계산한다.

Machine Learning에서 배운:

```text
MAE
```

와 연결해서 이해할 수 있다.

---

# 58. 예

Target:

```text
10
```

Prediction:

```text
8
```

Absolute Error:

```text
|8 - 10|
=
2
```

이다.

---

# 59. MSE vs L1

MSE:

```text
오차를 제곱

큰 오차에 더 민감
```

L1:

```text
절댓값 오차

MSE보다 큰 이상치에 덜 민감할 수 있음
```

이다.

---

# 60. Loss와 Metric은 다를 수 있다

Training:

```text
MSELoss
```

를 사용하면서 평가에서는:

```text
MAE
RMSE
R²
```

등을 같이 볼 수 있다.

즉:

```text
Loss Function
≠
반드시 최종 Evaluation Metric
```

이다.

---

# 61. Classification도 마찬가지

Training:

```text
CrossEntropyLoss
```

Evaluation:

```text
Accuracy

Precision

Recall

F1

ROC-AUC
```

등을 사용할 수 있다.

---

# 62. 왜 Loss와 Metric을 구분할까?

Loss는 주로:

```text
Gradient를 계산하여
Model을 학습
```

하기 위한 함수이다.

Metric은:

```text
Model 성능을
사람이 이해하고 평가
```

하기 위한 지표이다.

---

# 63. Differentiable

Neural Network 학습에서는 Loss로부터 Gradient를 계산해야 한다.

따라서 Loss Function은 Gradient 기반 최적화가 가능하도록 구성된다.

Accuracy 같은 지표는 Class가 맞았는지 여부가 불연속적으로 변하기 때문에 일반적으로 직접 Gradient 학습 목적함수로 사용하지 않는다.

---

# 64. loss.backward()

Loss를 계산한 뒤:

```python
loss.backward()
```

를 실행한다.

이때 PyTorch Autograd가 Model Parameter에 대한 Gradient를 계산한다.

---

# 65. 예

```python
outputs = model(
    x
)


loss = loss_fn(
    outputs,
    y
)


loss.backward()
```

---

# 66. 여기까지는 Weight가 바뀌지 않는다

중요하다.

```python
loss.backward()
```

는:

```text
Gradient 계산
```

을 한다.

Weight를 실제로 업데이트하는 것은:

```python
optimizer.step()
```

이다.

---

# 67. 전체 순서

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

# 68. 순서 해석

```text
zero_grad
→ 이전 Gradient 초기화

Model
→ Prediction

Loss
→ 오차 계산

backward
→ Gradient 계산

step
→ Weight Update
```

이다.

---

# 69. loss.item()

Loss Tensor에서 Python 숫자를 얻고 싶다면:

```python
loss.item()
```

을 사용한다.

---

# 70. 예

```python
print(
    loss.item()
)
```

결과:

```text
0.4382
```

처럼 볼 수 있다.

---

# 71. 왜 item()을 사용할까?

`loss` 자체는 Tensor이다.

```python
print(
    type(
        loss
    )
)
```

는:

```text
torch.Tensor
```

이다.

Logging이나 평균 계산을 위해 Python 숫자가 필요할 때 `item()`을 사용한다.

---

# 72. Epoch Loss

Training 중 Batch마다 Loss가 나온다.

예:

```text
Batch 1
0.8

Batch 2
0.6

Batch 3
0.5
```

이를 모아서 Epoch 전체 Loss를 계산할 수 있다.

---

# 73. 단순 평균

```python
running_loss = 0.0


for x, y in train_loader:

    ...

    running_loss += loss.item()


epoch_loss = (
    running_loss
    /
    len(
        train_loader
    )
)
```

---

# 74. 하지만 마지막 Batch 크기가 다를 수 있다

예:

```text
Batch 1 = 32
Batch 2 = 32
Batch 3 = 4
```

라면 Batch Loss를 단순 평균하는 것과 Sample 기준 평균이 약간 다를 수 있다.

---

# 75. Sample 기준 평균

`reduction="mean"`인 Loss라면 다음처럼 Batch 크기를 반영할 수 있다.

```python
running_loss = 0.0
total_samples = 0


for x, y in train_loader:

    ...

    batch_size = x.size(
        0
    )

    running_loss += (
        loss.item()
        *
        batch_size
    )

    total_samples += batch_size


epoch_loss = (
    running_loss
    /
    total_samples
)
```

---

# 76. reduction

PyTorch Loss Function에서 자주 보는 옵션이다.

예:

```python
nn.CrossEntropyLoss(
    reduction="mean"
)
```

---

# 77. reduction="mean"

각 Sample의 Loss를 평균하여 하나의 값으로 반환한다.

일반적인 기본값이다.

---

# 78. reduction="sum"

```python
nn.CrossEntropyLoss(
    reduction="sum"
)
```

각 Loss를 합산한다.

---

# 79. reduction="none"

```python
nn.CrossEntropyLoss(
    reduction="none"
)
```

각 Sample의 Loss를 그대로 반환한다.

---

# 80. 예

Batch Size:

```text
4
```

이라면:

```text
reduction="none"

↓

[loss1, loss2, loss3, loss4]
```

처럼 얻을 수 있다.

---

# 81. 언제 none이 유용할까?

예:

```text
Sample별 Loss 분석

특정 Sample Weight 적용

Hard Example 분석

Custom Loss 계산
```

등에서 활용할 수 있다.

---

# 82. Class Imbalance

Classification Dataset이:

```text
Class 0 = 1000

Class 1 = 100
```

처럼 불균형할 수 있다.

이 경우 Loss에 Class Weight를 적용하는 방법을 고려할 수 있다.

---

# 83. CrossEntropyLoss Weight

예:

```python
class_weights = torch.tensor(
    [
        1.0,
        5.0
    ],
    device=device
)


loss_fn = nn.CrossEntropyLoss(
    weight=class_weights
)
```

---

# 84. 의미

Class 1의 Error에 더 큰 Weight를 줄 수 있다.

하지만 Weight 값은 임의로 크게 넣는 것이 아니라 Training Data 분포와 Validation 결과를 기준으로 결정해야 한다.

---

# 85. Class Weight 자동 계산 예

예를 들어:

```python
from sklearn.utils.class_weight import compute_class_weight
```

를 사용할 수 있다.

---

# 86. 예

```python
import numpy as np

from sklearn.utils.class_weight import compute_class_weight


classes = np.array(
    [
        0,
        1
    ]
)


weights = compute_class_weight(
    class_weight="balanced",
    classes=classes,
    y=train_labels
)


print(
    weights
)
```

---

# 87. Tensor 변환

```python
class_weights = torch.tensor(
    weights,
    dtype=torch.float32,
    device=device
)
```

---

# 88. Loss에 전달

```python
loss_fn = nn.CrossEntropyLoss(
    weight=class_weights
)
```

---

# 89. Training Data로 계산

Class Weight는 Validation/Test 정답 정보를 섞어서 계산하지 않고:

```text
Training Data
```

를 기준으로 계산하는 것이 원칙이다.

---

# 90. BCEWithLogitsLoss의 pos_weight

Binary Classification에서는:

```python
nn.BCEWithLogitsLoss(
    pos_weight=...
)
```

도 사용할 수 있다.

---

# 91. 예

Negative:

```text
900
```

Positive:

```text
100
```

이면 간단한 시작점으로:

```text
900 / 100
=
9
```

같은 Positive Weight를 고려할 수 있다.

---

# 92. 코드

```python
pos_weight = torch.tensor(
    [
        9.0
    ],
    device=device
)


loss_fn = nn.BCEWithLogitsLoss(
    pos_weight=pos_weight
)
```

---

# 93. pos_weight 의미

Positive Sample의 Loss 기여도를 조절한다.

값을 크게 하면 Positive Error를 더 중요하게 다루도록 할 수 있다.

하지만 성능 Trade-off가 있으므로 Validation Metric으로 확인해야 한다.

---

# 94. Weighted Loss가 Accuracy를 무조건 올리나?

아니다.

Weighted Loss의 목적은 단순히:

```text
Accuracy 최대화
```

가 아닐 수 있다.

예를 들어 Minority Class Recall 개선을 위해 사용할 수 있다.

그 과정에서 전체 Accuracy가 조금 감소할 수도 있다.

---

# 95. 그래서 여러 Metric을 본다

불균형 Classification에서는:

```text
Accuracy

Precision

Recall

F1

ROC-AUC

PR-AUC

Confusion Matrix
```

등을 함께 확인한다.

---

# 96. Loss가 감소하면 Accuracy도 반드시 증가할까?

반드시 그렇지는 않다.

Loss는 Probability/Logit의 품질 변화에 민감하고 Accuracy는 최종 Class가 맞았는지를 본다.

따라서:

```text
Loss ↓

Accuracy 동일
```

한 상황도 가능하다.

---

# 97. 반대도 가능하다

특정 시점에서:

```text
Accuracy ↑

Validation Loss ↑
```

같은 현상이 나타날 수도 있다.

특히 Model Confidence와 Overfitting 등을 함께 살펴야 한다.

---

# 98. Train Loss와 Validation Loss

Training 중 둘을 같이 기록한다.

```text
Train Loss

Validation Loss
```

---

# 99. 좋은 학습의 한 예

```text
Epoch 1

Train Loss = 0.8
Val Loss   = 0.9


Epoch 5

Train Loss = 0.4
Val Loss   = 0.5
```

둘 다 감소하고 있다.

---

# 100. Overfitting 신호의 한 예

```text
Train Loss
계속 감소

Validation Loss
어느 순간 증가
```

할 수 있다.

예:

```text
Epoch 1
Train 0.8
Val   0.9

Epoch 5
Train 0.3
Val   0.5

Epoch 10
Train 0.1
Val   0.8
```

---

# 101. 의미

Training Data에는 점점 잘 맞지만:

```text
새로운 Validation Data
```

에는 성능이 악화되고 있을 가능성이 있다.

---

# 102. Best Model 저장

그래서 Validation Loss를 기준으로 Best Model을 저장할 수 있다.

개념:

```python
if val_loss < best_val_loss:

    best_val_loss = val_loss

    torch.save(
        model.state_dict(),
        "best_model.pt"
    )
```

---

# 103. 하지만 Competition Metric이 Accuracy라면?

Validation Loss만 볼 필요는 없다.

예:

```text
대회 Metric = Accuracy
```

라면 Validation Accuracy도 함께 기록해야 한다.

---

# 104. Best 기준도 목적에 따라 결정

예:

```text
Best Validation Loss

Best Accuracy

Best F1

Best ROC-AUC
```

중 어떤 것을 사용할지는 실제 목표에 따라 결정한다.

---

# 105. Loss 값끼리 무조건 비교하면 안 된다

예:

```text
CrossEntropyLoss = 0.4

MSELoss = 0.2
```

라고 해서 MSE Model이 더 좋은 Model이라는 뜻이 아니다.

Loss 종류와 문제 자체가 다르면 숫자 크기를 직접 비교할 수 없다.

---

# 106. 같은 Loss에서도 조건이 중요

다음이 다르면 Loss 크기를 단순 비교하기 어려울 수 있다.

```text
Dataset

Target Scale

Class Weight

Reduction

Loss Function 설정
```

---

# 107. NaN Loss

Training 중:

```text
loss = nan
```

이 발생할 수 있다.

---

# 108. 가능한 원인

```text
Input에 NaN

Input에 Inf

Learning Rate가 너무 큼

수치적으로 불안정한 연산

Target 오류

잘못된 직접 log 계산

Gradient Explosion
```

등이 있다.

---

# 109. Input 확인

```python
print(
    torch.isnan(
        x
    ).any()
)


print(
    torch.isinf(
        x
    ).any()
)
```

---

# 110. Output 확인

```python
print(
    torch.isnan(
        outputs
    ).any()
)


print(
    torch.isinf(
        outputs
    ).any()
)
```

---

# 111. Loss 확인

```python
if torch.isnan(
    loss
):

    print(
        "NaN Loss 발생"
    )
```

---

# 112. Learning Rate 확인

예:

```python
optimizer = torch.optim.Adam(
    model.parameters(),
    lr=0.001
)
```

Learning Rate가 지나치게 크면 Training이 불안정해질 수 있다.

---

# 113. Loss가 계속 커진다면

다음을 확인한다.

```text
Learning Rate

Input Scale

Target Scale

Loss Function 선택

Output Shape

Target Shape

dtype

NaN / Inf

Model 구조
```

---

# 114. CrossEntropyLoss 오류 체크

확인:

```text
Output Shape
[Batch, Classes]

Target Shape
[Batch]

Target dtype
Long

Target 값
0 ~ Classes-1
```

---

# 115. 예

Classes:

```text
3
```

이라면 Target은:

```text
0
1
2
```

중 하나여야 한다.

---

# 116. 잘못된 Label

Class가 3개인데:

```text
0
1
2
3
```

이 들어있다면 문제가 된다.

---

# 117. Label 확인

```python
print(
    torch.unique(
        y
    )
)
```

으로 확인할 수 있다.

---

# 118. BCEWithLogitsLoss 오류 체크

확인:

```text
Output Shape

Target Shape

Target dtype

Target 값 범위
```

이다.

일반적인 Binary Target은:

```text
0
1
```

이다.

---

# 119. Shape 출력

Training 전에:

```python
print(
    "outputs:",
    outputs.shape
)


print(
    "targets:",
    y.shape
)


print(
    "output dtype:",
    outputs.dtype
)


print(
    "target dtype:",
    y.dtype
)
```

을 확인한다.

---

# 120. Loss Debugging에서 가장 중요

오류가 발생하면 바로 코드를 크게 수정하지 말고:

```text
Prediction Shape

Target Shape

Prediction dtype

Target dtype

Label 값

Loss 종류
```

부터 확인한다.

---

# 121. One-hot Target 주의

CrossEntropyLoss의 일반적인 Class Index 방식에서는 Target을:

```text
[0, 2, 1, 0]
```

처럼 사용한다.

무조건 One-hot Encoding으로 바꿔야 하는 것이 아니다.

---

# 122. 예

Class 2라고 해서 반드시:

```text
[0, 0, 1]
```

로 직접 바꾸는 것이 아니다.

일반적인 Class Index Target:

```text
2
```

를 사용할 수 있다.

---

# 123. Loss Function과 Output Layer 설계

Model을 만들 때 마지막 Layer부터 무작정 정하지 않는다.

먼저:

```text
문제 유형
```

을 정한다.

---

# 124. 순서

```text
문제 유형 결정
↓
Target 형태 확인
↓
Model Output 결정
↓
Loss Function 결정
↓
Prediction 변환 결정
↓
Metric 결정
```

---

# 125. Multi-class

```text
Target
Class Index

↓

Output
n_classes Logits

↓

CrossEntropyLoss

↓

Prediction
argmax

↓

Probability 필요 시
Softmax
```

---

# 126. Binary 1-Logit

```text
Target
0 / 1 Float

↓

Output
1 Logit

↓

BCEWithLogitsLoss

↓

Probability
Sigmoid

↓

Prediction
Threshold
```

---

# 127. Regression

```text
Target
Continuous Number

↓

Output
Continuous Value

↓

MSELoss / L1Loss

↓

MAE / RMSE / R² 등으로 평가
```

---

# 128. Multi-label

```text
Target
여러 0 / 1

↓

Output
Label별 Logit

↓

BCEWithLogitsLoss

↓

Sigmoid

↓

Label별 Threshold
```

---

# 129. PyTorch Loss Function 예

```python
classification_loss = nn.CrossEntropyLoss()

binary_loss = nn.BCEWithLogitsLoss()

regression_mse = nn.MSELoss()

regression_mae = nn.L1Loss()
```

---

# 130. Loss를 변수에 저장하는 이유

```python
loss_fn = nn.CrossEntropyLoss()
```

처럼 저장하면 Training과 Validation에서 같은 설정을 재사용하기 쉽다.

---

# 131. Training

```python
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

# 132. Validation

```python
model.eval()


with torch.no_grad():

    for x, y in valid_loader:

        x = x.to(
            device
        )

        y = y.to(
            device
        )

        outputs = model(
            x
        )

        loss = loss_fn(
            outputs,
            y
        )
```

---

# 133. Validation에서는 backward를 하지 않는다

Validation의 목적은:

```text
Model 평가
```

이다.

따라서 일반적으로:

```text
loss.backward()

optimizer.step()
```

을 실행하지 않는다.

---

# 134. Validation Loss도 중요한 이유

Training Loss만 보면 Model이 Training Data를 외우고 있는지 알기 어렵다.

Validation Loss를 같이 보면:

```text
Generalization
Overfitting
```

을 판단하는 데 도움이 된다.

---

# 135. Training Loss가 낮다고 좋은 Model인가?

반드시 아니다.

예:

```text
Train Loss = 매우 낮음

Val Loss = 높음
```

이면 Overfitting일 수 있다.

---

# 136. Test Loss는 언제?

최종 평가 단계에서 정답이 있는 Test Set이라면 계산할 수 있다.

하지만 Competition의 `test.csv`처럼 Label이 공개되지 않은 Test Data에서는 직접 Test Loss를 계산할 수 없다.

---

# 137. Competition 구조

```text
train.csv
→ Label 있음
→ Training / Validation 가능

test.csv
→ Label 없음
→ Prediction 생성

sample_submission.csv
→ 제출 형식
```

---

# 138. Loss Curve

Epoch마다 Loss를 저장할 수 있다.

```python
train_losses = []

val_losses = []
```

---

# 139. 저장

```python
train_losses.append(
    train_loss
)


val_losses.append(
    val_loss
)
```

---

# 140. 그래프

```python
import matplotlib.pyplot as plt


plt.plot(
    train_losses,
    label="Train Loss"
)


plt.plot(
    val_losses,
    label="Validation Loss"
)


plt.xlabel(
    "Epoch"
)


plt.ylabel(
    "Loss"
)


plt.legend()


plt.show()
```

---

# 141. Loss Curve 해석

패턴 1:

```text
Train Loss ↓
Val Loss ↓
```

학습이 잘 진행되는 신호일 수 있다.

---

# 142. 패턴 2

```text
Train Loss ↓
Val Loss ↑
```

Overfitting 가능성을 확인한다.

---

# 143. 패턴 3

```text
Train Loss 높음
Val Loss 높음
```

Underfitting, 학습 부족, Model 구조, Learning Rate, 데이터 문제 등을 확인할 수 있다.

---

# 144. Early Stopping과 연결

Validation Loss가 일정 기간 개선되지 않으면 Training을 중단하는 방법을:

```text
Early Stopping
```

이라고 한다.

---

# 145. 개념

```text
Epoch 1
Val Loss = 0.8

Epoch 2
Val Loss = 0.6

Epoch 3
Val Loss = 0.5

Epoch 4
Val Loss = 0.51

Epoch 5
Val Loss = 0.55
```

Validation Loss가 더 이상 좋아지지 않는다면 Best Model을 유지하고 Training을 멈추는 전략을 사용할 수 있다.

---

# 146. patience

Early Stopping에서:

```text
patience = 3
```

이라면 보통:

```text
3 Epoch 동안 개선되지 않으면 중단
```

같은 방식으로 사용한다.

구현에 따라 세부 동작은 달라질 수 있다.

---

# 147. Loss가 중요한 이유 다시 보기

Model은 스스로:

```text
내가 틀렸다.
```

를 알 수 없다.

Loss Function이:

```text
얼마나 틀렸는지
```

를 숫자로 제공한다.

---

# 148. Backpropagation은 Loss에서 시작

```text
Loss
↓
Backward
↓
Gradient
↓
각 Layer
↓
Weight / Bias
```

방향으로 Gradient를 계산한다.

---

# 149. 그래서 Loss 선택이 중요

잘못된 Loss를 사용하면 Model이 최적화하는 목표 자체가 문제와 맞지 않을 수 있다.

예:

```text
Multi-class 문제인데
잘못된 Output/Loss 조합 사용
```

하면 정상적인 학습이 어렵다.

---

# 150. Loss를 볼 때 질문

```text
무슨 문제인가?

Classification인가 Regression인가?

Binary인가 Multi-class인가?

Multi-label인가?

Model Output Shape는?

Target Shape는?

Target dtype은?

Raw Logit을 넣어야 하는가?

Class Imbalance가 있는가?

최종 Metric은 무엇인가?
```

---

# 151. Loss 선택표

| 문제 | Model Output | Target | Loss |
|---|---|---|---|
| Binary | `[B, 1]` | Float 0/1 | `BCEWithLogitsLoss` |
| Binary 2-logit | `[B, 2]` | Long Class Index | `CrossEntropyLoss` |
| Multi-class | `[B, C]` | Long Class Index | `CrossEntropyLoss` |
| Multi-label | `[B, C]` | Float 0/1 | `BCEWithLogitsLoss` |
| Regression | `[B, 1]` 등 | Float | `MSELoss`, `L1Loss` |

여기서:

```text
B
→ Batch Size

C
→ Class / Label 수
```

이다.

---

# 152. Loss와 Activation 연결표

| 문제 | Training Model Output | Loss | Inference 변환 |
|---|---|---|---|
| Binary 1-logit | Raw Logit | BCEWithLogitsLoss | Sigmoid |
| Multi-class | Raw Logits | CrossEntropyLoss | Softmax |
| Multi-label | Raw Logits | BCEWithLogitsLoss | Sigmoid |
| Regression | Numeric Output | MSE/L1 등 | 문제에 따라 |

---

# 153. 가장 흔한 실수

```text
CrossEntropyLoss 전에 Softmax

BCEWithLogitsLoss 전에 Sigmoid

Target dtype 오류

Output/Target Shape 불일치

Class Index 범위 오류

NaN / Inf 데이터

Validation에서 optimizer.step()

Loss만 보고 Metric을 안 봄
```

---

# 154. Training 전에 한 Batch로 확인

```python
x, y = next(
    iter(
        train_loader
    )
)


x = x.to(
    device
)


y = y.to(
    device
)


outputs = model(
    x
)


print(
    "Output:",
    outputs.shape,
    outputs.dtype
)


print(
    "Target:",
    y.shape,
    y.dtype
)


loss = loss_fn(
    outputs,
    y
)


print(
    "Loss:",
    loss.item()
)
```

---

# 155. 이 테스트가 성공하면

최소한:

```text
DataLoader
↓
Model
↓
Output
↓
Target
↓
Loss
```

연결이 기본적으로 맞는지 확인할 수 있다.

---

# 156. 다음 단계

현재까지:

```text
Dataset
↓
DataLoader
↓
Model
↓
Activation
↓
Prediction
↓
Loss
```

까지 연결했다.

하지만 아직 Model Parameter는 업데이트되지 않았다.

다음에 필요한 것이:

```text
Optimizer
```

이다.

---

# 157. Loss와 Optimizer 연결

```text
Loss
↓
loss.backward()
↓
Gradient
↓
optimizer.step()
↓
Weight Update
```

이다.

---

# 158. 최종 정리

Loss Function은:

```text
Model Prediction과
실제 Target 사이의 차이
```

를 계산한다.

Training의 기본 목표는:

```text
Loss를 줄이는 방향으로
Weight와 Bias를 업데이트
```

하는 것이다.

대표적인 Loss:

```text
Multi-class
→ CrossEntropyLoss

Binary
→ BCEWithLogitsLoss

Multi-label
→ BCEWithLogitsLoss

Regression
→ MSELoss / L1Loss
```

이다.

그리고 반드시 기억한다.

```text
CrossEntropyLoss
→ Raw Logits 입력
→ Training 전에 Softmax 하지 않음
```

```text
BCEWithLogitsLoss
→ Raw Logit 입력
→ Training 전에 Sigmoid 하지 않음
```

Loss를 계산한 다음:

```python
loss.backward()
```

로 Gradient를 계산한다.

하지만 이것만으로 Weight가 바뀌는 것은 아니다.

실제 Weight Update는:

```python
optimizer.step()
```

이 담당한다.

따라서 전체 흐름은:

```text
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
```

이다.