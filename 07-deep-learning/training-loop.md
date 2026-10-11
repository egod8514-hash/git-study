# Training Loop

## 1. Training Loop란?

Training Loop는 Neural Network가 Dataset을 반복해서 학습하는 전체 과정을 코드로 구현한 것이다.

기본 흐름:

```text
Epoch 시작
↓
Training Mode
↓
DataLoader에서 Batch 가져오기
↓
Device로 이동
↓
Gradient 초기화
↓
Forward
↓
Loss 계산
↓
Backward
↓
Optimizer Step
↓
다음 Batch
↓
모든 Batch 완료
↓
Epoch 결과 계산
```

이 과정을 여러 Epoch 반복한다.

---

# 2. 가장 기본적인 Training Loop

```python
for epoch in range(epochs):

    model.train()

    for x, y in train_loader:

        x = x.to(device)
        y = y.to(device)

        optimizer.zero_grad()

        outputs = model(x)

        loss = loss_fn(
            outputs,
            y
        )

        loss.backward()

        optimizer.step()
```

이 코드가 PyTorch Training의 가장 중요한 기본 구조이다.

---

# 3. 가장 바깥쪽 반복문

```python
for epoch in range(epochs):
```

는 Epoch를 반복한다.

예:

```python
epochs = 5
```

이면:

```text
Epoch 1
Epoch 2
Epoch 3
Epoch 4
Epoch 5
```

총 5번 Dataset을 학습한다.

---

# 4. Epoch란?

Epoch는:

```text
Training Dataset 전체를
한 번 학습하는 것
```

이다.

예:

```text
Training Samples = 1000
```

이면 1000개의 Training Sample을 모두 한 번 사용했을 때:

```text
1 Epoch
```

이 완료된다.

---

# 5. 하지만 1000개를 한 번에 넣는 것은 아니다

보통 Dataset을 Batch로 나눈다.

예:

```text
Samples = 1000
Batch Size = 100
```

이면 대략:

```text
10 Batch
```

가 만들어진다.

---

# 6. 따라서 1 Epoch

```text
Epoch 시작

Batch 1
→ 학습

Batch 2
→ 학습

Batch 3
→ 학습

...

Batch 10
→ 학습

Epoch 종료
```

이다.

---

# 7. Epoch와 Batch 관계

```text
Dataset
↓
여러 Batch로 분리
↓
모든 Batch를 한 번 사용
↓
1 Epoch
```

이다.

---

# 8. Training Mode

```python
model.train()
```

을 사용한다.

이 명령은 Model을:

```text
Training Mode
```

로 변경한다.

---

# 9. model.train()이 Weight를 학습시키는 명령인가?

아니다.

이 부분을 구분해야 한다.

```python
model.train()
```

자체가 Weight를 Update하는 것은 아니다.

실제 Weight Update:

```python
optimizer.step()
```

이다.

---

# 10. model.train()의 역할

Model 안에 다음과 같은 Layer가 있을 때 Training 방식으로 동작하도록 설정한다.

대표적으로:

```text
Dropout

Batch Normalization
```

이다.

---

# 11. DataLoader 반복

```python
for x, y in train_loader:
```

Training DataLoader에서 Batch를 하나씩 가져온다.

예:

```text
Batch 1
x, y

Batch 2
x, y

Batch 3
x, y
```

이다.

---

# 12. x와 y

일반적으로:

```text
x
→ Input / Feature

y
→ Target / Label
```

이다.

---

# 13. Image Classification 예

예:

```text
x.shape
=
[32, 3, 224, 224]
```

의미:

```text
32
→ Batch Size

3
→ RGB Channel

224 × 224
→ Image Size
```

Target:

```text
y.shape
=
[32]
```

일 수 있다.

---

# 14. Device 이동

```python
x = x.to(device)
y = y.to(device)
```

Model이 GPU에 있다면 Input과 Target도 필요한 경우 같은 Device로 이동해야 한다.

---

# 15. Model도 Device로 이동

Training 전에:

```python
model = model.to(device)
```

를 실행한다.

---

# 16. Device 설정 예

```python
device = torch.device(
    "cuda"
    if torch.cuda.is_available()
    else "cpu"
)
```

---

# 17. 구조

```text
Model
→ CUDA

Input
→ CUDA

Target
→ CUDA
```

처럼 맞춘다.

---

# 18. Device가 다르면?

예:

```text
Model
→ GPU

Input
→ CPU
```

이면 Device mismatch 오류가 발생할 수 있다.

---

# 19. Gradient 초기화

```python
optimizer.zero_grad()
```

이전 Batch의 Gradient를 초기화한다.

PyTorch에서는 Gradient가 기본적으로 누적될 수 있기 때문이다.

---

# 20. Forward

```python
outputs = model(
    x
)
```

Input을 Model에 전달한다.

결과:

```text
Prediction
또는
Logits
```

를 얻는다.

---

# 21. Multi-class 예

Class가 10개라면:

```text
outputs.shape
=
[32, 10]
```

일 수 있다.

---

# 22. Loss 계산

```python
loss = loss_fn(
    outputs,
    y
)
```

Model Output과 Target을 비교한다.

---

# 23. 예

```python
loss_fn = nn.CrossEntropyLoss()
```

라면 Multi-class Classification에서:

```text
outputs
→ Raw Logits

y
→ Class Index
```

를 사용한다.

---

# 24. Backward

```python
loss.backward()
```

Loss를 기준으로 Gradient를 계산한다.

---

# 25. Parameter Update

```python
optimizer.step()
```

Gradient를 이용해 Weight와 Bias를 실제로 Update한다.

---

# 26. 한 Batch 학습 완료

따라서:

```python
optimizer.zero_grad()

outputs = model(x)

loss = loss_fn(
    outputs,
    y
)

loss.backward()

optimizer.step()
```

까지 실행하면:

```text
한 Batch에 대한 Training Step
```

이 완료된다.

---

# 27. 다음 Batch

DataLoader가 다음 Batch를 가져온다.

그리고 같은 과정이 반복된다.

```text
Batch
↓
Forward
↓
Loss
↓
Backward
↓
Update
```

---

# 28. Training Loss 기록

학습이 제대로 되고 있는지 확인하려면 Loss를 기록한다.

```python
running_loss = 0.0
```

---

# 29. Batch Loss 추가

```python
running_loss += loss.item()
```

---

# 30. Epoch 평균 Loss

```python
epoch_loss = (
    running_loss
    /
    len(train_loader)
)
```

---

# 31. 전체 예

```python
for epoch in range(epochs):

    model.train()

    running_loss = 0.0

    for x, y in train_loader:

        x = x.to(device)
        y = y.to(device)

        optimizer.zero_grad()

        outputs = model(x)

        loss = loss_fn(
            outputs,
            y
        )

        loss.backward()

        optimizer.step()

        running_loss += loss.item()

    epoch_loss = (
        running_loss
        /
        len(train_loader)
    )

    print(
        epoch,
        epoch_loss
    )
```

---

# 32. Epoch 번호를 1부터 출력

`range()`는 0부터 시작한다.

따라서:

```python
for epoch in range(epochs):
```

에서 첫 번째 `epoch` 값은:

```text
0
```

이다.

사람이 보기 쉽게:

```python
epoch + 1
```

을 사용할 수 있다.

---

# 33. 예

```python
print(
    f"Epoch {epoch + 1}/{epochs}"
)
```

출력:

```text
Epoch 1/5
Epoch 2/5
...
Epoch 5/5
```

---

# 34. Classification Accuracy 계산

Classification에서는 Loss뿐 아니라 Accuracy도 기록할 수 있다.

먼저:

```python
correct = 0
total = 0
```

을 준비한다.

---

# 35. Prediction

Multi-class라면:

```python
pred = outputs.argmax(
    dim=1
)
```

이다.

---

# 36. 맞은 Prediction 수

```python
correct += (
    pred == y
).sum().item()
```

---

# 37. 전체 Sample 수

```python
total += y.size(
    0
)
```

---

# 38. Accuracy

```python
accuracy = (
    correct
    /
    total
)
```

---

# 39. Percentage

```python
accuracy = (
    correct
    /
    total
) * 100
```

으로 표현할 수도 있다.

---

# 40. Training Accuracy 포함

```python
for epoch in range(epochs):

    model.train()

    running_loss = 0.0
    correct = 0
    total = 0

    for x, y in train_loader:

        x = x.to(device)
        y = y.to(device)

        optimizer.zero_grad()

        outputs = model(x)

        loss = loss_fn(
            outputs,
            y
        )

        loss.backward()

        optimizer.step()

        running_loss += loss.item()

        pred = outputs.argmax(
            dim=1
        )

        correct += (
            pred == y
        ).sum().item()

        total += y.size(
            0
        )

    train_loss = (
        running_loss
        /
        len(train_loader)
    )

    train_acc = (
        correct
        /
        total
    )

    print(
        f"Epoch {epoch + 1}/{epochs} "
        f"Loss: {train_loss:.4f} "
        f"Accuracy: {train_acc:.4f}"
    )
```

---

# 41. 출력 예

```text
Epoch 1/5 Loss: 0.6421 Accuracy: 0.7100
Epoch 2/5 Loss: 0.4125 Accuracy: 0.8300
Epoch 3/5 Loss: 0.2914 Accuracy: 0.8900
```

---

# 42. 무엇을 의미할까?

예:

```text
Loss
0.6421
→
0.4125
→
0.2914
```

감소하고 있다.

Accuracy:

```text
0.71
→
0.83
→
0.89
```

증가하고 있다.

Training Data에 대한 학습이 진행되고 있다는 신호일 수 있다.

---

# 43. 하지만 Train 결과만 보면 안 된다

매우 중요하다.

```text
Train Accuracy = 99%
```

라고 해서 새로운 Data에서도 99%라는 뜻은 아니다.

그래서:

```text
Validation
```

이 필요하다.

---

# 44. Training과 Validation

한 Epoch의 일반적인 구조:

```text
Training
↓
Validation
↓
결과 출력
↓
다음 Epoch
```

이다.

---

# 45. Validation Mode

```python
model.eval()
```

을 사용한다.

---

# 46. Gradient 계산 비활성화

```python
with torch.no_grad():
```

를 사용한다.

---

# 47. Validation 기본 구조

```python
model.eval()

with torch.no_grad():

    for x, y in val_loader:

        x = x.to(device)
        y = y.to(device)

        outputs = model(x)

        loss = loss_fn(
            outputs,
            y
        )
```

---

# 48. Validation에서는 하지 않는 것

일반적으로:

```python
optimizer.zero_grad()

loss.backward()

optimizer.step()
```

을 하지 않는다.

---

# 49. 이유

Validation Data는:

```text
Model Parameter 학습
```

이 아니라:

```text
현재 Model 성능 평가
```

에 사용하기 때문이다.

---

# 50. Training vs Validation

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

Loss

Metric

Weight Update 없음
```

이다.

---

# 51. Validation Loss 계산

```python
val_running_loss = 0.0
```

Batch마다:

```python
val_running_loss += loss.item()
```

마지막:

```python
val_loss = (
    val_running_loss
    /
    len(val_loader)
)
```

---

# 52. Validation Accuracy

```python
val_correct = 0
val_total = 0
```

그리고:

```python
pred = outputs.argmax(
    dim=1
)

val_correct += (
    pred == y
).sum().item()

val_total += y.size(
    0
)
```

---

# 53. 최종

```python
val_acc = (
    val_correct
    /
    val_total
)
```

이다.

---

# 54. Training + Validation

```python
for epoch in range(epochs):

    # -----------------
    # Training
    # -----------------

    model.train()

    train_loss_sum = 0.0
    train_correct = 0
    train_total = 0

    for x, y in train_loader:

        x = x.to(device)
        y = y.to(device)

        optimizer.zero_grad()

        outputs = model(x)

        loss = loss_fn(
            outputs,
            y
        )

        loss.backward()

        optimizer.step()

        train_loss_sum += loss.item()

        pred = outputs.argmax(
            dim=1
        )

        train_correct += (
            pred == y
        ).sum().item()

        train_total += y.size(
            0
        )

    train_loss = (
        train_loss_sum
        /
        len(train_loader)
    )

    train_acc = (
        train_correct
        /
        train_total
    )

    # -----------------
    # Validation
    # -----------------

    model.eval()

    val_loss_sum = 0.0
    val_correct = 0
    val_total = 0

    with torch.no_grad():

        for x, y in val_loader:

            x = x.to(device)
            y = y.to(device)

            outputs = model(x)

            loss = loss_fn(
                outputs,
                y
            )

            val_loss_sum += loss.item()

            pred = outputs.argmax(
                dim=1
            )

            val_correct += (
                pred == y
            ).sum().item()

            val_total += y.size(
                0
            )

    val_loss = (
        val_loss_sum
        /
        len(val_loader)
    )

    val_acc = (
        val_correct
        /
        val_total
    )

    print(
        f"Epoch {epoch + 1}/{epochs} | "
        f"Train Loss: {train_loss:.4f} | "
        f"Train Acc: {train_acc:.4f} | "
        f"Val Loss: {val_loss:.4f} | "
        f"Val Acc: {val_acc:.4f}"
    )
```

---

# 55. 이제 Training Loop 전체가 연결되었다

```text
Epoch
│
├─ Training
│  │
│  ├─ model.train()
│  │
│  ├─ Batch
│  │
│  ├─ Forward
│  │
│  ├─ Loss
│  │
│  ├─ Backward
│  │
│  └─ Optimizer Step
│
└─ Validation
   │
   ├─ model.eval()
   │
   ├─ no_grad()
   │
   ├─ Forward
   │
   ├─ Loss
   │
   └─ Metric
```

---

# 56. 함수로 분리하기

Training Loop가 길어지면 함수로 분리할 수 있다.

예:

```python
def train_one_epoch(
    model,
    loader,
    loss_fn,
    optimizer,
    device
):
```

---

# 57. Training 함수

```python
def train_one_epoch(
    model,
    loader,
    loss_fn,
    optimizer,
    device
):

    model.train()

    loss_sum = 0.0
    correct = 0
    total = 0

    for x, y in loader:

        x = x.to(device)
        y = y.to(device)

        optimizer.zero_grad()

        outputs = model(x)

        loss = loss_fn(
            outputs,
            y
        )

        loss.backward()

        optimizer.step()

        loss_sum += loss.item()

        pred = outputs.argmax(
            dim=1
        )

        correct += (
            pred == y
        ).sum().item()

        total += y.size(
            0
        )

    avg_loss = (
        loss_sum
        /
        len(loader)
    )

    accuracy = (
        correct
        /
        total
    )

    return (
        avg_loss,
        accuracy
    )
```

---

# 58. 함수의 Return

```python
return (
    avg_loss,
    accuracy
)
```

두 값을 반환한다.

호출:

```python
train_loss, train_acc = train_one_epoch(
    model,
    train_loader,
    loss_fn,
    optimizer,
    device
)
```

---

# 59. Validation 함수

```python
def evaluate(
    model,
    loader,
    loss_fn,
    device
):

    model.eval()

    loss_sum = 0.0
    correct = 0
    total = 0

    with torch.no_grad():

        for x, y in loader:

            x = x.to(device)
            y = y.to(device)

            outputs = model(x)

            loss = loss_fn(
                outputs,
                y
            )

            loss_sum += loss.item()

            pred = outputs.argmax(
                dim=1
            )

            correct += (
                pred == y
            ).sum().item()

            total += y.size(
                0
            )

    avg_loss = (
        loss_sum
        /
        len(loader)
    )

    accuracy = (
        correct
        /
        total
    )

    return (
        avg_loss,
        accuracy
    )
```

---

# 60. Main Loop

함수로 만들면 전체 Training Loop가 훨씬 읽기 쉬워진다.

```python
for epoch in range(epochs):

    train_loss, train_acc = train_one_epoch(
        model,
        train_loader,
        loss_fn,
        optimizer,
        device
    )

    val_loss, val_acc = evaluate(
        model,
        val_loader,
        loss_fn,
        device
    )

    print(
        f"Epoch {epoch + 1}/{epochs} | "
        f"Train Loss: {train_loss:.4f} | "
        f"Train Acc: {train_acc:.4f} | "
        f"Val Loss: {val_loss:.4f} | "
        f"Val Acc: {val_acc:.4f}"
    )
```

---

# 61. 함수로 나누는 이유

코드의 역할이 명확해진다.

```text
train_one_epoch()
→ Training 담당

evaluate()
→ Validation 담당

Main Loop
→ Epoch 반복 담당
```

---

# 62. 이전에 봤던 train() 함수의 의미

Deep Learning 코드에서:

```python
tr_loss = train(
    train_loader,
    model,
    loss_fn,
    optimizer,
    device
)
```

같은 코드를 볼 수 있다.

여기서 `train()`은 PyTorch가 반드시 제공하는 특별한 함수가 아니라 작성자가 직접 만든 Training 함수일 수 있다.

---

# 63. model.train()과 train()은 다르다

주의한다.

```python
model.train()
```

은 PyTorch Model의 Training Mode 설정이다.

반면:

```python
train(...)
```

은 사용자가 직접 정의한 Training 함수일 수 있다.

---

# 64. evaluate()도 마찬가지

```python
evaluate(...)
```

도 사용자가 직접 만든 함수일 수 있다.

보통 내부에서:

```text
model.eval()

torch.no_grad()

Forward

Metric 계산
```

등을 수행한다.

---

# 65. 이전 코드 읽기

예:

```python
tr_loss = train(
    train_loader,
    model,
    loss_fn,
    optimizer,
    device
)

val_loss, val_acc = evaluate(
    val_loader,
    model,
    loss_fn,
    device
)
```

이제 다음처럼 읽을 수 있다.

```text
Training Data로
한 Epoch 학습

↓

Validation Data로
현재 Model 평가
```

---

# 66. Training History

Epoch 결과를 저장할 수 있다.

```python
train_losses = []
train_accs = []

val_losses = []
val_accs = []
```

---

# 67. 저장

```python
train_losses.append(
    train_loss
)

train_accs.append(
    train_acc
)

val_losses.append(
    val_loss
)

val_accs.append(
    val_acc
)
```

---

# 68. 왜 저장할까?

나중에:

```text
Training Curve

Validation Curve

Overfitting 확인

Experiment 비교
```

에 사용할 수 있다.

---

# 69. Loss Graph

```python
import matplotlib.pyplot as plt


plt.plot(
    train_losses,
    label="Train"
)

plt.plot(
    val_losses,
    label="Validation"
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

# 70. Accuracy Graph

```python
plt.plot(
    train_accs,
    label="Train"
)

plt.plot(
    val_accs,
    label="Validation"
)

plt.xlabel(
    "Epoch"
)

plt.ylabel(
    "Accuracy"
)

plt.legend()

plt.show()
```

---

# 71. Training Curve를 보는 이유

숫자 하나만 보는 것보다:

```text
Epoch가 진행되면서
성능이 어떻게 변했는가?
```

를 볼 수 있다.

---

# 72. 좋은 패턴의 한 예

```text
Train Loss ↓
Val Loss ↓

Train Accuracy ↑
Val Accuracy ↑
```

이다.

물론 실제 데이터에서는 항상 매끄럽게 움직이지 않는다.

---

# 73. Overfitting 가능성

예:

```text
Train Loss
계속 감소

Val Loss
어느 순간부터 증가
```

이면 Overfitting 가능성을 확인한다.

---

# 74. Train Accuracy만 계속 증가

```text
Train Accuracy = 99%

Val Accuracy = 80%
```

처럼 차이가 커진다면 Generalization 문제를 확인한다.

---

# 75. Best Model

마지막 Epoch의 Model이 항상 최고의 Validation Model은 아니다.

예:

```text
Epoch 1
Val Acc = 0.80

Epoch 2
Val Acc = 0.85

Epoch 3
Val Acc = 0.90 ← Best

Epoch 4
Val Acc = 0.88

Epoch 5
Val Acc = 0.86
```

---

# 76. 따라서 Best Model을 저장

```python
best_val_acc = 0.0
```

---

# 77. 조건

```python
if val_acc > best_val_acc:

    best_val_acc = val_acc

    torch.save(
        model.state_dict(),
        "best_model.pt"
    )
```

---

# 78. 의미

Validation Accuracy가 지금까지 가장 높을 때만:

```text
Model Weight 저장
```

을 한다.

---

# 79. Validation Loss 기준

목적에 따라:

```python
best_val_loss = float(
    "inf"
)
```

로 시작할 수도 있다.

---

# 80. 조건

```python
if val_loss < best_val_loss:

    best_val_loss = val_loss

    torch.save(
        model.state_dict(),
        "best_model.pt"
    )
```

---

# 81. 어떤 기준을 사용할까?

Task와 목표 Metric에 따라 결정한다.

예:

```text
대회 평가 기준 = Accuracy
→ Validation Accuracy 중요

의료 탐지
→ Recall / Sensitivity 중요할 수 있음

확률 예측
→ ROC-AUC, PR-AUC 등도 고려
```

즉 Accuracy만 항상 사용하는 것은 아니다.

---

# 82. Binary Classification에서는 코드가 조금 달라진다

1-Logit Binary Model:

```text
Output
[B, 1]
```

일 수 있다.

---

# 83. Sigmoid

Prediction을 만들 때:

```python
prob = torch.sigmoid(
    outputs
)
```

을 사용한다.

---

# 84. Threshold

```python
pred = (
    prob >= 0.5
).long()
```

이다.

---

# 85. Shape 맞추기

예:

```python
outputs = outputs.squeeze(
    1
)
```

Target이:

```text
[B]
```

라면 Output도 `[B]`로 맞출 수 있다.

---

# 86. Binary Training 예

```python
outputs = model(
    x
).squeeze(
    1
)

loss = loss_fn(
    outputs,
    y.float()
)

prob = torch.sigmoid(
    outputs
)

pred = (
    prob >= 0.5
).long()
```

---

# 87. CrossEntropy와 다르다

Multi-class:

```python
pred = outputs.argmax(
    dim=1
)
```

Binary 1-logit:

```python
prob = torch.sigmoid(
    outputs
)

pred = (
    prob >= 0.5
).long()
```

이다.

---

# 88. 그래서 Training Loop는 Task에 따라 조금 달라진다

공통 부분:

```text
Forward

Loss

Backward

Optimizer
```

은 비슷하다.

하지만:

```text
Prediction 생성

Metric 계산

Output Shape

Target dtype
```

은 문제에 따라 달라질 수 있다.

---

# 89. Regression에서는 Accuracy를 사용하지 않는다

Regression에서는:

```text
Accuracy
```

대신:

```text
MAE

MSE

RMSE

R²
```

등을 사용할 수 있다.

---

# 90. 즉 Training Loop의 뼈대는 같고 Metric이 달라진다

```text
Classification
→ Accuracy / F1 / Recall 등

Regression
→ MAE / RMSE / R² 등
```

---

# 91. Batch Loss 단순 평균 주의

다음:

```python
loss_sum += loss.item()
```

후:

```python
loss_sum / len(loader)
```

는 Batch별 평균 Loss를 다시 평균한 것이다.

마지막 Batch Size가 다르면 Sample 기준 평균과 약간 다를 수 있다.

---

# 92. 더 정확한 Sample 기준 집계

```python
loss_sum += (
    loss.item()
    *
    x.size(0)
)

total += x.size(
    0
)
```

마지막:

```python
avg_loss = (
    loss_sum
    /
    total
)
```

이다.

---

# 93. 개선된 Training 함수

```python
def train_one_epoch(
    model,
    loader,
    loss_fn,
    optimizer,
    device
):

    model.train()

    loss_sum = 0.0
    correct = 0
    total = 0

    for x, y in loader:

        x = x.to(device)
        y = y.to(device)

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

        batch_size = x.size(
            0
        )

        loss_sum += (
            loss.item()
            *
            batch_size
        )

        pred = outputs.argmax(
            dim=1
        )

        correct += (
            pred == y
        ).sum().item()

        total += batch_size

    avg_loss = (
        loss_sum
        /
        total
    )

    accuracy = (
        correct
        /
        total
    )

    return (
        avg_loss,
        accuracy
    )
```

---

# 94. 개선된 Validation 함수

```python
def evaluate(
    model,
    loader,
    loss_fn,
    device
):

    model.eval()

    loss_sum = 0.0
    correct = 0
    total = 0

    with torch.no_grad():

        for x, y in loader:

            x = x.to(device)
            y = y.to(device)

            outputs = model(
                x
            )

            loss = loss_fn(
                outputs,
                y
            )

            batch_size = x.size(
                0
            )

            loss_sum += (
                loss.item()
                *
                batch_size
            )

            pred = outputs.argmax(
                dim=1
            )

            correct += (
                pred == y
            ).sum().item()

            total += batch_size

    avg_loss = (
        loss_sum
        /
        total
    )

    accuracy = (
        correct
        /
        total
    )

    return (
        avg_loss,
        accuracy
    )
```

---

# 95. 완성된 Main Loop

```python
epochs = 10

best_val_acc = 0.0

train_losses = []
train_accs = []

val_losses = []
val_accs = []


for epoch in range(
    epochs
):

    train_loss, train_acc = train_one_epoch(
        model,
        train_loader,
        loss_fn,
        optimizer,
        device
    )

    val_loss, val_acc = evaluate(
        model,
        val_loader,
        loss_fn,
        device
    )

    train_losses.append(
        train_loss
    )

    train_accs.append(
        train_acc
    )

    val_losses.append(
        val_loss
    )

    val_accs.append(
        val_acc
    )

    if val_acc > best_val_acc:

        best_val_acc = val_acc

        torch.save(
            model.state_dict(),
            "best_model.pt"
        )

    print(
        f"Epoch {epoch + 1:02d}/{epochs} | "
        f"Train Loss: {train_loss:.4f} | "
        f"Train Acc: {train_acc:.4f} | "
        f"Val Loss: {val_loss:.4f} | "
        f"Val Acc: {val_acc:.4f}"
    )
```

---

# 96. 출력 예

```text
Epoch 01/10 | Train Loss: 0.5321 | Train Acc: 0.7412 | Val Loss: 0.4211 | Val Acc: 0.8123

Epoch 02/10 | Train Loss: 0.3512 | Train Acc: 0.8510 | Val Loss: 0.3105 | Val Acc: 0.8721

Epoch 03/10 | Train Loss: 0.2501 | Train Acc: 0.9023 | Val Loss: 0.2801 | Val Acc: 0.8912
```

---

# 97. `.4f`

예:

```python
f"{train_loss:.4f}"
```

는 소수점 아래 4자리까지 출력한다.

예:

```text
0.532145

↓

0.5321
```

---

# 98. `:02d`

```python
f"{epoch + 1:02d}"
```

는 정수를 두 자리로 출력한다.

예:

```text
1
→ 01

2
→ 02

10
→ 10
```

이다.

---

# 99. Scheduler가 있다면

Optimizer와 Scheduler를 함께 사용할 수 있다.

예:

```python
scheduler.step()
```

을 Epoch 끝에 실행할 수 있다.

단 Scheduler 종류에 따라 호출 방식이 다르다.

---

# 100. 예

```python
for epoch in range(
    epochs
):

    train_loss, train_acc = train_one_epoch(
        ...
    )

    val_loss, val_acc = evaluate(
        ...
    )

    scheduler.step()
```

---

# 101. ReduceLROnPlateau라면

예:

```python
scheduler.step(
    val_loss
)
```

처럼 Validation Loss를 전달할 수 있다.

Scheduler마다 사용 방법을 확인해야 한다.

---

# 102. 현재 Learning Rate 출력

```python
current_lr = optimizer.param_groups[
    0
][
    "lr"
]
```

---

# 103. 출력

```python
print(
    "Learning Rate:",
    current_lr
)
```

---

# 104. tqdm

Training 진행 상황을 보기 위해 `tqdm`을 사용할 수 있다.

```python
from tqdm.auto import tqdm
```

---

# 105. 예

```python
for x, y in tqdm(
    train_loader
):

    ...
```

Batch 진행률을 볼 수 있다.

---

# 106. tqdm은 학습 자체를 바꾸지 않는다

`tqdm`은:

```text
Progress Bar
```

를 보여주는 도구이다.

Model 성능을 높이는 기능이 아니다.

---

# 107. GPU Training

GPU를 사용할 때도 Training Loop 구조 자체는 거의 같다.

달라지는 핵심:

```python
model.to(device)

x.to(device)

y.to(device)
```

이다.

---

# 108. GPU가 있다고 자동으로 사용되는 것은 아니다

CUDA가 사용 가능하더라도 Model과 Tensor를 GPU Device로 이동해야 한다.

---

# 109. Training 시작 전에 확인

```python
print(
    "Device:",
    device
)

print(
    "Model device:",
    next(
        model.parameters()
    ).device
)
```

---

# 110. 한 Batch 먼저 테스트

전체 Epoch를 돌리기 전에 한 Batch만 테스트하는 것이 좋다.

```python
x, y = next(
    iter(
        train_loader
    )
)
```

---

# 111. Shape 확인

```python
print(
    x.shape
)

print(
    y.shape
)
```

---

# 112. Device 이동

```python
x = x.to(device)
y = y.to(device)
```

---

# 113. Forward

```python
outputs = model(
    x
)
```

---

# 114. Output 확인

```python
print(
    outputs.shape
)
```

---

# 115. Loss 확인

```python
loss = loss_fn(
    outputs,
    y
)

print(
    loss.item()
)
```

---

# 116. Backward 테스트

```python
optimizer.zero_grad()

loss.backward()

optimizer.step()
```

오류가 없다면 기본 연결이 맞는지 확인할 수 있다.

---

# 117. 왜 한 Batch 테스트가 중요한가?

전체 Training을 시작했다가 10분 뒤 오류를 발견하는 것보다:

```text
한 Batch
```

에서 먼저:

```text
Shape

dtype

Device

Loss

Backward
```

를 확인하는 것이 빠르다.

---

# 118. Training Loop 오류가 나면 확인 순서

```text
1. x.shape

2. y.shape

3. x.dtype

4. y.dtype

5. x.device

6. model device

7. outputs.shape

8. Loss Function

9. Label 범위

10. NaN / Inf
```

순서로 확인하면 좋다.

---

# 119. 자주 발생하는 Shape 오류

예:

```text
mat1 and mat2 shapes cannot be multiplied
```

는 Model이 기대하는 Feature 수와 실제 Input Shape가 다를 가능성이 있다.

---

# 120. 해결

```python
print(
    x.shape
)
```

를 먼저 확인한다.

그리고 첫 `Linear` Layer:

```python
nn.Linear(
    in_features,
    ...
)
```

의 `in_features`와 비교한다.

---

# 121. CrossEntropyLoss 오류

확인:

```text
outputs
[B, C]

target
[B]

target dtype
torch.long

target 값
0 ~ C-1
```

이다.

---

# 122. CUDA 오류

예:

```text
Expected all tensors to be on the same device
```

가 나오면:

```text
Model

Input

Target
```

의 Device를 확인한다.

---

# 123. Out of Memory

GPU Memory가 부족하면:

```text
CUDA out of memory
```

오류가 발생할 수 있다.

---

# 124. 우선 확인할 것

```text
Batch Size 줄이기

Image Size 확인

Model 크기 확인

불필요한 Tensor 저장 여부 확인
```

등이다.

---

# 125. Training 중 Prediction을 모두 저장할 필요는 없다

단순 Accuracy만 계산한다면 매 Batch의 전체 Output Tensor를 계속 List에 저장할 필요가 없다.

필요한 통계만 누적하면 Memory를 줄일 수 있다.

---

# 126. 하지만 ROC-AUC 등이 필요하다면

Validation에서 Probability와 Target을 모아야 할 수 있다.

예:

```python
all_probs = []
all_targets = []
```

---

# 127. Batch마다

```python
prob = torch.softmax(
    outputs,
    dim=1
)[
    :,
    1
]

all_probs.extend(
    prob.cpu().numpy()
)

all_targets.extend(
    y.cpu().numpy()
)
```

---

# 128. 왜 cpu()?

GPU Tensor를 NumPy로 직접 변환할 수 없기 때문에 CPU로 이동한다.

Validation에서는 `torch.no_grad()`를 사용하므로 일반적인 평가 수집 과정이 단순해진다.

필요에 따라 `detach()`를 명시할 수도 있다.

---

# 129. ROC-AUC

모든 Validation Probability를 모은 뒤:

```python
from sklearn.metrics import roc_auc_score


auc = roc_auc_score(
    all_targets,
    all_probs
)
```

처럼 계산할 수 있다.

---

# 130. F1

```python
from sklearn.metrics import f1_score


f1 = f1_score(
    all_targets,
    all_preds
)
```

처럼 사용할 수 있다.

---

# 131. Accuracy 하나만 보면 안 되는 경우

Class Imbalance가 심하면:

```text
Accuracy
```

만으로 Model을 평가하기 어렵다.

같이 볼 수 있는 것:

```text
Precision

Recall

F1

ROC-AUC

PR-AUC

Confusion Matrix
```

등이다.

---

# 132. Training Loop와 Evaluation Metric

Training Loop의 핵심은 그대로 유지하면서 필요한 Metric을 추가할 수 있다.

```text
Forward
↓
Loss
↓
Prediction / Probability 저장
↓
Metric 계산
```

---

# 133. Random Seed

실험 재현성을 높이기 위해 Seed를 설정할 수 있다.

```python
import random
import numpy as np
import torch


seed = 42


random.seed(
    seed
)

np.random.seed(
    seed
)

torch.manual_seed(
    seed
)

torch.cuda.manual_seed_all(
    seed
)
```

---

# 134. Seed를 같게 하면 완전히 똑같은 결과인가?

항상 100% 보장되는 것은 아니다.

GPU 연산이나 특정 알고리즘의 비결정적 동작 등으로 차이가 생길 수 있다.

하지만 재현성을 높이는 데 도움이 된다.

---

# 135. Experiment 기록

Training할 때 최소한 다음을 기록하면 좋다.

```text
Model

Optimizer

Learning Rate

Batch Size

Epoch

Loss Function

Augmentation

Train / Validation Split

Random Seed

Best Validation Metric
```

---

# 136. 왜 기록할까?

Experiment A:

```text
Val Acc = 0.95
```

Experiment B:

```text
Val Acc = 0.97
```

만 기록하면:

```text
무엇을 바꿔서 좋아졌는가?
```

를 나중에 알기 어렵다.

---

# 137. Baseline

먼저 단순한 Training Loop로:

```text
Baseline
```

을 만든다.

그 다음 한 가지씩 개선한다.

---

# 138. 예

```text
Baseline

↓

Experiment A
Fine-tuning

↓

Experiment B
Augmentation

↓

Experiment C
Weighted Loss
```

처럼 비교할 수 있다.

---

# 139. 동시에 모두 바꾸지 않는다

한꺼번에:

```text
Optimizer 변경

Learning Rate 변경

Augmentation 변경

Loss 변경

Model 변경
```

하면 무엇 때문에 성능이 변했는지 알기 어렵다.

---

# 140. Training Loop는 실험의 중심

Deep Learning Experiment는 결국 같은 Training Loop를 반복하면서:

```text
Data

Model

Loss

Optimizer

Hyperparameter
```

를 변경하고 Validation 결과를 비교하는 과정이라고 볼 수 있다.

---

# 141. 전체 코드 구조

실제 프로젝트는 보통 다음 흐름을 가진다.

```text
1. Import

2. Seed

3. Device

4. Data Load

5. Dataset

6. DataLoader

7. Model

8. Loss

9. Optimizer

10. Training Function

11. Validation Function

12. Epoch Loop

13. Best Model Save

14. Graph

15. Test Inference
```

---

# 142. 지금까지 배운 파일들이 연결되는 지점

```text
tensor-basic.md
→ Tensor

dataset-dataloader.md
→ Dataset / DataLoader

neural-network.md
→ Model

activation-function.md
→ ReLU 등

loss-function.md
→ Loss

optimizer-backprop.md
→ Backward / Optimizer

training-loop.md
→ 모두 연결
```

---

# 143. Deep Learning Training 핵심 코드

```python
for epoch in range(
    epochs
):

    model.train()

    for x, y in train_loader:

        x = x.to(device)
        y = y.to(device)

        optimizer.zero_grad()

        outputs = model(x)

        loss = loss_fn(
            outputs,
            y
        )

        loss.backward()

        optimizer.step()
```

이 구조는 반드시 익숙해져야 한다.

---

# 144. 코드 읽는 순서

코드를 외우려고 하기보다:

```text
Epoch 반복

↓

Batch 가져오기

↓

GPU로 이동

↓

이전 Gradient 제거

↓

Prediction

↓

Loss 계산

↓

Gradient 계산

↓

Weight Update
```

로 읽는다.

---

# 145. Training과 Validation을 붙이면

```text
Epoch
│
├── Train
│   ├── Forward
│   ├── Loss
│   ├── Backward
│   └── Update
│
└── Validation
    ├── Forward
    ├── Loss
    └── Metric
```

이다.

---

# 146. 가장 중요한 차이

Training:

```python
loss.backward()
optimizer.step()
```

있음.

Validation:

```text
없음
```

이다.

---

# 147. Validation에서 Model이 학습되면 안 된다

Validation은:

```text
현재 Model이
학습에 사용하지 않은 Data에서
얼마나 잘 동작하는가?
```

를 확인하는 과정이다.

따라서 Validation Data로 Parameter를 Update하지 않는다.

---

# 148. Test는 더 마지막

전체 흐름:

```text
Train
→ Parameter 학습

Validation
→ Model / Hyperparameter 선택

Test
→ 최종 평가
```

이다.

Competition에서는 Label 없는 Test Data를 최종 Prediction 생성에 사용한다.

---

# 149. 한 줄 정리

Training Loop란:

```text
Dataset을 Batch 단위로 반복하면서
Forward → Loss → Backward → Update를 수행하고,
Epoch마다 Validation 성능을 확인하는 과정
```

이다.

---

# 150. 최종 정리

PyTorch Training의 핵심 구조:

```python
for epoch in range(epochs):

    model.train()

    for x, y in train_loader:

        x = x.to(device)
        y = y.to(device)

        optimizer.zero_grad()

        outputs = model(x)

        loss = loss_fn(
            outputs,
            y
        )

        loss.backward()

        optimizer.step()
```

각각의 의미:

```text
Epoch
→ Dataset 전체 반복

Batch
→ Dataset의 일부

model.train()
→ Training Mode

zero_grad()
→ 이전 Gradient 초기화

model(x)
→ Forward

loss_fn()
→ Loss 계산

backward()
→ Gradient 계산

optimizer.step()
→ Parameter Update
```

Validation:

```python
model.eval()

with torch.no_grad():

    ...
```

를 사용하며:

```text
Backward 없음

Optimizer Step 없음
```

이다.

전체 구조:

```text
Dataset
↓
DataLoader
↓
Epoch
↓
Batch
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
↓
Validation
↓
Metric
↓
Best Model
```

이 흐름이 PyTorch Deep Learning Training의 핵심이다.