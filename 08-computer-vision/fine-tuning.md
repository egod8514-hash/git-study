# Fine-tuning

## 1. Fine-tuning이란?

Fine-tuning은 이미 학습된 Pretrained Model의 Weight를 새로운 Dataset에 맞게 추가로 학습하는 방법이다.

기본 구조:

```text
Pretrained Model
↓
새로운 Dataset
↓
일부 또는 전체 Weight를 추가 Training
↓
새로운 Task에 적응
```

---

# 2. Transfer Learning과 Fine-tuning

Transfer Learning은 더 큰 개념이다.

```text
Transfer Learning
├── Feature Extraction
│   └── Backbone Freeze
│       Classifier만 Training
│
└── Fine-tuning
    └── Backbone 일부 또는 전체도 Training
```

즉:

```text
Fine-tuning
⊂
Transfer Learning
```

으로 이해할 수 있다.

---

# 3. Feature Extraction

Pretrained Backbone을 그대로 사용한다.

```text
Image
↓
Frozen Backbone
↓
Feature
↓
Trainable Classifier
↓
Prediction
```

Backbone Weight는 업데이트하지 않는다.

---

# 4. Fine-tuning

Fine-tuning에서는 Backbone의 일부 또는 전체를 다시 학습한다.

```text
Image
↓
Trainable Backbone
↓
Feature
↓
Trainable Classifier
↓
Prediction
```

---

# 5. 왜 Fine-tuning을 할까?

Pretrained Model이 학습했던 Dataset과 새로운 Dataset은 다를 수 있다.

예:

```text
Pretraining
ImageNet 자연 Image

↓

새로운 Task
Chest X-ray
```

따라서 기존 Feature를 새로운 Domain에 맞게 조금 조정할 필요가 있을 수 있다.

---

# 6. 처음부터 전체 Fine-tuning?

가능하지만 항상 좋은 시작은 아니다.

새로운 Classifier는 처음에 Random Weight를 가진다.

이 상태에서 Backbone 전체까지 큰 Learning Rate로 학습하면 기존의 유용한 Pretrained Weight가 크게 변할 수 있다.

---

# 7. 단계적 Fine-tuning

그래서 다음 전략을 사용할 수 있다.

```text
Stage 1
Backbone Freeze
↓
Classifier 학습

Stage 2
Backbone 일부 Unfreeze
↓
낮은 LR로 Fine-tuning
```

---

# 8. Stage 1 목적

새롭게 만든 Classifier를 먼저 새로운 Dataset에 적응시킨다.

```text
Pretrained Feature
↓
새 Classifier
↓
새로운 Label 학습
```

---

# 9. Stage 2 목적

Classifier가 어느 정도 학습된 후 Backbone 일부를 열어 새로운 Dataset에 맞는 Feature를 조금 더 학습한다.

---

# 10. ResNet18 준비

```python
import torch
import torch.nn as nn

from torchvision.models import (
    resnet18,
    ResNet18_Weights
)
```

---

# 11. Device

```python
device = torch.device(
    "cuda"
    if torch.cuda.is_available()
    else "cpu"
)


print(
    device
)
```

---

# 12. Pretrained Model

```python
weights = (
    ResNet18_Weights.DEFAULT
)


model = resnet18(
    weights=weights
)
```

---

# 13. 기존 Classifier

```python
print(
    model.fc
)
```

ResNet18은 마지막에:

```text
fc
```

Layer를 사용한다.

---

# 14. Stage 1 - Backbone Freeze

```python
for param in model.parameters():

    param.requires_grad = False
```

---

# 15. Classifier 교체

```python
num_features = (
    model.fc.in_features
)


model.fc = nn.Linear(
    num_features,
    2
)
```

---

# 16. 왜 Freeze 후 Classifier를 바꿀까?

기존 Parameter를 먼저:

```text
requires_grad=False
```

로 만든 뒤 새로운 `fc`를 생성하면 새 `fc`는 기본적으로:

```text
requires_grad=True
```

가 된다.

결과:

```text
Backbone
Frozen

fc
Trainable
```

이다.

---

# 17. Model을 GPU로 이동

```python
model = model.to(
    device
)
```

---

# 18. Trainable Parameter 확인

```python
for name, param in model.named_parameters():

    if param.requires_grad:

        print(
            name
        )
```

예상:

```text
fc.weight
fc.bias
```

---

# 19. Parameter 수 확인

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
    "Total Parameters:",
    total_params
)


print(
    "Trainable Parameters:",
    trainable_params
)
```

---

# 20. Stage 1 Loss

2-Class를 2 Logit으로 Prediction한다면:

```python
loss_fn = nn.CrossEntropyLoss()
```

---

# 21. Stage 1 Optimizer

Classifier만 Training:

```python
optimizer = torch.optim.Adam(
    model.fc.parameters(),
    lr=1e-3
)
```

---

# 22. Stage 1 Learning Rate

예:

```text
1e-3
=
0.001
```

이다.

이 값은 예시이다.

Dataset과 Model에 따라 실험한다.

---

# 23. Stage 1 Training

```python
for epoch in range(
    3
):

    model.train()


    for images, labels in train_loader:

        images = images.to(
            device
        )

        labels = labels.to(
            device
        )


        optimizer.zero_grad()


        logits = model(
            images
        )


        loss = loss_fn(
            logits,
            labels
        )


        loss.backward()


        optimizer.step()
```

---

# 24. 여기서 실제로 Update되는 Parameter

```text
fc.weight

fc.bias
```

이다.

Backbone Parameter는 Frozen 상태이다.

---

# 25. Stage 1 Validation

```python
model.eval()


with torch.no_grad():

    for images, labels in val_loader:

        images = images.to(
            device
        )

        labels = labels.to(
            device
        )


        logits = model(
            images
        )
```

---

# 26. Stage 1 결과 기록

예:

```text
Stage 1

Epoch 1
Val Accuracy = 0.91

Epoch 2
Val Accuracy = 0.94

Epoch 3
Val Accuracy = 0.93
```

---

# 27. Best Epoch

위 결과에서는:

```text
Epoch 2
```

가 가장 좋은 Validation Accuracy를 가질 수 있다.

마지막 Epoch가 항상 Best Model인 것은 아니다.

---

# 28. Best Model 저장

```python
best_val_acc = 0.0


if val_acc > best_val_acc:

    best_val_acc = val_acc


    torch.save(
        model.state_dict(),
        "stage1_best.pt"
    )
```

---

# 29. Stage 2 전에 Best Model 복원

Stage 1 마지막 Epoch가 아니라 Best Weight에서 Fine-tuning을 시작할 수도 있다.

```python
model.load_state_dict(
    torch.load(
        "stage1_best.pt",
        map_location=device
    )
)
```

---

# 30. Stage 2

이제 Backbone 일부를 Unfreeze한다.

ResNet18 예:

```text
conv1
bn1
layer1
layer2
layer3
layer4
fc
```

---

# 31. 전체 구조 확인

```python
print(
    model
)
```

---

# 32. 후반 Layer부터 Fine-tuning

예:

```text
layer1
Frozen

layer2
Frozen

layer3
Frozen

layer4
Trainable

fc
Trainable
```

로 만들 수 있다.

---

# 33. layer4 Unfreeze

```python
for param in model.layer4.parameters():

    param.requires_grad = True
```

---

# 34. 확인

```python
for name, param in model.named_parameters():

    if param.requires_grad:

        print(
            name
        )
```

이제:

```text
layer4....

fc.weight

fc.bias
```

등이 출력된다.

---

# 35. Stage 2 Optimizer를 다시 만든다

중요하다.

```python
optimizer = torch.optim.Adam(
    filter(
        lambda p: p.requires_grad,
        model.parameters()
    ),
    lr=1e-4
)
```

---

# 36. 왜 다시 만들까?

Stage 1과 Stage 2의 Trainable Parameter가 달라졌기 때문이다.

Stage 1:

```text
fc
```

Stage 2:

```text
layer4
+
fc
```

이다.

---

# 37. 기존 Optimizer를 그대로 쓰면?

Stage 1에서 Optimizer를:

```python
optimizer = torch.optim.Adam(
    model.fc.parameters(),
    lr=1e-3
)
```

로 만들었다면 `layer4`를 Unfreeze해도 `layer4` Parameter는 그 Optimizer에 포함되지 않는다.

따라서 Optimizer를 다시 구성한다.

---

# 38. Stage 2 Learning Rate

예:

```text
Stage 1
1e-3

Stage 2
1e-4
```

처럼 Fine-tuning에서 더 작은 Learning Rate를 사용할 수 있다.

---

# 39. 왜 낮은 Learning Rate?

Pretrained Weight를:

```text
크게 바꾸는 것
```

보다:

```text
조금씩 조정
```

하려는 목적이다.

---

# 40. Stage 2 Training

Training Loop 자체는 거의 같다.

```python
for epoch in range(
    5
):

    model.train()


    for images, labels in train_loader:

        images = images.to(
            device
        )

        labels = labels.to(
            device
        )


        optimizer.zero_grad()


        logits = model(
            images
        )


        loss = loss_fn(
            logits,
            labels
        )


        loss.backward()


        optimizer.step()
```

---

# 41. 달라진 것은?

```text
Stage 1
fc만 Update

Stage 2
layer4 + fc Update
```

이다.

---

# 42. Fine-tuning의 핵심

Training Loop가 특별한 것이 아니라:

```text
어떤 Parameter를 학습할 것인가?

어떤 Learning Rate로 학습할 것인가?
```

가 핵심이다.

---

# 43. 전체 Unfreeze

전체 Model을 Fine-tuning하려면:

```python
for param in model.parameters():

    param.requires_grad = True
```

로 설정할 수 있다.

---

# 44. 전체 Fine-tuning Optimizer

```python
optimizer = torch.optim.Adam(
    model.parameters(),
    lr=1e-5
)
```

처럼 더 작은 Learning Rate를 실험할 수 있다.

---

# 45. 부분 Fine-tuning과 전체 Fine-tuning

부분:

```text
일부 Backbone
+
Classifier
```

전체:

```text
전체 Backbone
+
Classifier
```

이다.

---

# 46. 무엇이 더 좋은가?

정답은 Dataset에 따라 다르다.

확인해야 한다.

```text
Dataset 크기

Pretraining Domain과의 차이

Overfitting 정도

Validation 성능

GPU Memory

Training 시간
```

---

# 47. Dataset이 작은 경우

많은 Parameter를 Unfreeze하면 Overfitting 위험이 커질 수 있다.

따라서:

```text
Classifier Only
↓
후반 Block
↓
필요하면 더 많이 Unfreeze
```

처럼 단계적으로 실험할 수 있다.

---

# 48. Domain 차이가 큰 경우

Pretraining Domain과 새로운 Domain이 많이 다르면 기존 Feature를 더 많이 조정하는 것이 도움이 되는지 실험할 수 있다.

---

# 49. Parameter Group

Backbone과 Classifier에 서로 다른 Learning Rate를 줄 수 있다.

```python
optimizer = torch.optim.Adam(
    [
        {
            "params": model.layer4.parameters(),
            "lr": 1e-5
        },

        {
            "params": model.fc.parameters(),
            "lr": 1e-4
        }
    ]
)
```

---

# 50. 의미

```text
layer4
→ 0.00001

fc
→ 0.0001
```

이다.

---

# 51. 왜 다르게 할까?

Backbone:

```text
이미 학습된 Weight
```

Classifier:

```text
새 Task에 맞게 새로 만든 Layer
```

이기 때문이다.

---

# 52. Classifier는 더 많이 조정

새로운 Classifier는 새로운 Task를 배워야 하므로 상대적으로 더 큰 Learning Rate를 사용할 수 있다.

---

# 53. Backbone은 조금씩 조정

기존 Feature를 보존하면서 새로운 Domain에 맞게 조정하려는 목적이다.

---

# 54. Discriminative Learning Rate

Layer나 Parameter Group마다 다른 Learning Rate를 적용하는 전략을 사용할 수 있다.

---

# 55. 모든 Layer를 같은 LR로 해도 되는가?

가능하다.

하지만 Fine-tuning에서는 Layer 역할과 Pretrained Weight 보존을 고려해 서로 다른 LR를 실험할 수 있다.

---

# 56. Fine-tuning과 BatchNorm

여기서 중요한 주의점이 있다.

```text
requires_grad=False
```

와:

```text
model.eval()
```

은 같은 것이 아니다.

---

# 57. requires_grad=False

Parameter Gradient 계산 및 Weight Update와 관련된다.

---

# 58. model.eval()

BatchNorm과 Dropout 같은 Layer의 동작 Mode를 바꾼다.

---

# 59. model.train()

Training Loop에서:

```python
model.train()
```

을 호출하면 BatchNorm도 Training Mode가 된다.

---

# 60. Frozen Backbone의 BatchNorm

Backbone Weight를 Freeze했다고 해서 BatchNorm의 Running Statistics까지 자동으로 완전히 고정되는 것과 동일하지는 않다.

---

# 61. 왜 알아야 할까?

작은 Dataset이나 작은 Batch Size에서 BatchNorm 통계가 불안정해질 수 있다.

Fine-tuning 전략에 따라 BatchNorm 처리도 고려할 수 있다.

---

# 62. 입문 단계

처음에는:

```text
Freeze / Unfreeze

Learning Rate

Validation 결과
```

를 우선 이해한다.

BatchNorm 세부 제어는 필요할 때 추가로 다룬다.

---

# 63. model.train()을 안 하면?

Training 시 Dropout이나 BatchNorm이 Evaluation 방식으로 동작할 수 있으므로 Training 시작 시:

```python
model.train()
```

을 호출한다.

---

# 64. Validation

```python
model.eval()


with torch.no_grad():
```

를 사용한다.

---

# 65. Fine-tuning Train 함수

반복되는 코드는 함수로 만들 수 있다.

```python
def train_one_epoch(
    model,
    loader,
    loss_fn,
    optimizer,
    device
):

    model.train()


    total_loss = 0.0

    correct = 0

    count = 0


    for images, labels in loader:

        images = images.to(
            device
        )

        labels = labels.to(
            device
        )


        optimizer.zero_grad()


        logits = model(
            images
        )


        loss = loss_fn(
            logits,
            labels
        )


        loss.backward()


        optimizer.step()


        batch_size = labels.size(
            0
        )


        total_loss += (
            loss.item()
            * batch_size
        )


        preds = logits.argmax(
            dim=1
        )


        correct += (
            preds == labels
        ).sum().item()


        count += batch_size


    return (
        total_loss / count,
        correct / count
    )
```

---

# 66. Validation 함수

```python
def evaluate(
    model,
    loader,
    loss_fn,
    device
):

    model.eval()


    total_loss = 0.0

    correct = 0

    count = 0


    with torch.no_grad():

        for images, labels in loader:

            images = images.to(
                device
            )

            labels = labels.to(
                device
            )


            logits = model(
                images
            )


            loss = loss_fn(
                logits,
                labels
            )


            batch_size = labels.size(
                0
            )


            total_loss += (
                loss.item()
                * batch_size
            )


            preds = logits.argmax(
                dim=1
            )


            correct += (
                preds == labels
            ).sum().item()


            count += batch_size


    return (
        total_loss / count,
        correct / count
    )
```

---

# 67. Stage Training 함수

```python
def run_training(
    model,
    train_loader,
    val_loader,
    loss_fn,
    optimizer,
    device,
    epochs,
    save_path
):

    best_val_loss = float(
        "inf"
    )


    for epoch in range(
        epochs
    ):

        train_loss, train_acc = (
            train_one_epoch(
                model,
                train_loader,
                loss_fn,
                optimizer,
                device
            )
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


        if val_loss < best_val_loss:

            best_val_loss = val_loss


            torch.save(
                model.state_dict(),
                save_path
            )
```

---

# 68. Stage 1 실행

```python
optimizer_stage1 = torch.optim.Adam(
    model.fc.parameters(),
    lr=1e-3
)


run_training(
    model=model,
    train_loader=train_loader,
    val_loader=val_loader,
    loss_fn=loss_fn,
    optimizer=optimizer_stage1,
    device=device,
    epochs=3,
    save_path="stage1_best.pt"
)
```

---

# 69. Stage 1 Best Weight Load

```python
model.load_state_dict(
    torch.load(
        "stage1_best.pt",
        map_location=device
    )
)
```

---

# 70. Stage 2 Unfreeze

```python
for param in model.layer4.parameters():

    param.requires_grad = True
```

---

# 71. Stage 2 Optimizer

```python
optimizer_stage2 = torch.optim.Adam(
    [
        {
            "params": model.layer4.parameters(),
            "lr": 1e-5
        },

        {
            "params": model.fc.parameters(),
            "lr": 1e-4
        }
    ]
)
```

---

# 72. Stage 2 실행

```python
run_training(
    model=model,
    train_loader=train_loader,
    val_loader=val_loader,
    loss_fn=loss_fn,
    optimizer=optimizer_stage2,
    device=device,
    epochs=5,
    save_path="stage2_best.pt"
)
```

---

# 73. Stage 2 Best Weight

```python
model.load_state_dict(
    torch.load(
        "stage2_best.pt",
        map_location=device
    )
)
```

---

# 74. 최종 Evaluation

```python
val_loss, val_acc = evaluate(
    model,
    val_loader,
    loss_fn,
    device
)


print(
    "Val Loss:",
    val_loss
)


print(
    "Val Accuracy:",
    val_acc
)
```

---

# 75. Stage별 결과 비교

예:

```text
Stage 1
Backbone Freeze
Val Acc = 0.94

Stage 2
layer4 Fine-tuning
Val Acc = 0.97
```

이면 Stage 2가 개선되었을 가능성이 있다.

---

# 76. 하지만 Accuracy만 보면 안 된다

특히 Class Imbalance가 있다면:

```text
Precision

Recall

F1

ROC-AUC

PR-AUC

Confusion Matrix
```

도 비교한다.

---

# 77. Fine-tuning 후 Accuracy만 증가한 경우

예:

```text
Accuracy
0.95 → 0.96

Minority Recall
0.80 → 0.65
```

라면 실제 목적에 따라 개선이라고 보기 어려울 수 있다.

---

# 78. 의료영상

특히:

```text
False Negative

False Positive
```

의 의미가 다를 수 있으므로 Class별 성능을 확인한다.

---

# 79. Learning Curve

Stage별로:

```text
Train Loss

Validation Loss

Train Accuracy

Validation Accuracy
```

를 저장한다.

---

# 80. Fine-tuning 직후 Validation 악화

Stage 1은 좋았는데 Stage 2에서 갑자기 Validation 성능이 떨어진다면 다음을 확인한다.

```text
Learning Rate가 너무 큰가?

너무 많은 Layer를 열었는가?

Dataset이 너무 작은가?

Overfitting이 시작됐는가?
```

---

# 81. Catastrophic Forgetting 개념

Fine-tuning 과정에서 기존에 학습한 유용한 Representation을 지나치게 크게 변경해 성능이 악화될 수 있다.

이를 넓은 의미에서 기존 지식을 잃는 문제와 연결해 생각할 수 있다.

---

# 82. 대응

```text
더 작은 Learning Rate

더 적은 Layer Unfreeze

짧은 Fine-tuning

Best Model 저장
```

등을 고려할 수 있다.

---

# 83. Fine-tuning Epoch

많은 Epoch가 항상 좋은 것은 아니다.

Pretrained Model은 이미 상당한 Feature를 학습하고 있기 때문에 비교적 적은 Epoch로도 빠르게 적응할 수 있다.

---

# 84. Early Stopping

Validation 성능이 더 이상 개선되지 않으면 Training을 중단할 수 있다.

---

# 85. Patience

예:

```text
patience = 3
```

이면 Validation Metric이 3 Epoch 동안 개선되지 않았을 때 Training을 중단하는 방식으로 만들 수 있다.

---

# 86. 기본 변수

```python
best_val_loss = float(
    "inf"
)

patience = 3

patience_count = 0
```

---

# 87. 개선된 경우

```python
if val_loss < best_val_loss:

    best_val_loss = val_loss

    patience_count = 0


    torch.save(
        model.state_dict(),
        "best_model.pt"
    )
```

---

# 88. 개선되지 않은 경우

```python
else:

    patience_count += 1
```

---

# 89. 중단

```python
if patience_count >= patience:

    print(
        "Early Stopping"
    )

    break
```

---

# 90. 왜 필요한가?

Fine-tuning을 계속하면서 Validation 성능이 악화되는 Overfitting을 줄이고 불필요한 Training 시간을 줄이는 데 도움을 줄 수 있다.

---

# 91. Scheduler

Training 중 Learning Rate를 변경하는:

```text
Learning Rate Scheduler
```

도 사용할 수 있다.

---

# 92. ReduceLROnPlateau

Validation 성능 개선이 멈추면 Learning Rate를 낮추는 방식이다.

```python
scheduler = (
    torch.optim.lr_scheduler.ReduceLROnPlateau(
        optimizer,
        mode="min",
        factor=0.1,
        patience=2
    )
)
```

---

# 93. Epoch 후

```python
scheduler.step(
    val_loss
)
```

한다.

---

# 94. Fine-tuning에서 Scheduler

처음부터 필수는 아니다.

먼저:

```text
Freeze

Unfreeze

Learning Rate
```

의 기본 실험을 이해한 뒤 추가한다.

---

# 95. Weight Decay

Optimizer에서 Regularization을 줄 수 있다.

```python
optimizer = torch.optim.AdamW(
    filter(
        lambda p: p.requires_grad,
        model.parameters()
    ),
    lr=1e-4,
    weight_decay=1e-4
)
```

---

# 96. Adam과 AdamW

Fine-tuning에서도:

```text
Adam

AdamW

SGD
```

등 여러 Optimizer를 사용할 수 있다.

어떤 것이 항상 최고라고 정할 수는 없다.

---

# 97. Data Augmentation과 함께 사용

```text
Transfer Learning
+
Fine-tuning
+
Data Augmentation
```

을 함께 사용할 수 있다.

---

# 98. 하지만 실험 순서

가능하면 먼저 각각의 효과를 확인한다.

예:

```text
Baseline

↓

Fine-tuning

↓

Augmentation

↓

Fine-tuning + Augmentation
```

이다.

---

# 99. 한 번에 모두 적용하면?

성능이 좋아져도:

```text
Fine-tuning 때문인가?

Augmentation 때문인가?

Learning Rate 때문인가?
```

를 구분하기 어렵다.

---

# 100. Experiment A

```text
Pretrained Model

Backbone Freeze

Classifier Only
```

---

# 101. Experiment B

```text
Experiment A
+
layer4 Unfreeze
```

---

# 102. Experiment C

```text
Experiment B
+
Augmentation
```

---

# 103. Experiment D

```text
Experiment C
+
Weighted Loss
```

처럼 확장할 수 있다.

---

# 104. Ablation 관점

각 요소를 추가하거나 제거하면서 성능 변화를 확인하는 방식으로 어떤 요소가 실제로 도움이 되었는지 분석할 수 있다.

---

# 105. 동일 Split

실험 간:

```text
Train / Validation Split
```

을 동일하게 유지한다.

---

# 106. 동일 Metric

비교 기준도 동일하게 유지한다.

예:

```text
Validation ROC-AUC
```

를 주요 Metric으로 정했다면 각 실험을 같은 방식으로 비교한다.

---

# 107. Seed 기록

```python
SEED = 42
```

등 실험 조건을 기록한다.

---

# 108. Hyperparameter 기록

최소한:

```text
Model

Pretrained Weight

Image Size

Batch Size

Optimizer

Stage 1 LR

Stage 2 LR

Epoch

Freeze 범위

Augmentation

Loss
```

를 기록한다.

---

# 109. 실험표 예

```text
Exp | Freeze | Unfreeze | Stage1 LR | Stage2 LR | Val Acc | F1
A   | All    | None     | 1e-3      | -         | 0.94    | 0.95
B   | Most   | layer4   | 1e-3      | 1e-4      | 0.96    | 0.97
C   | Most   | layer3+4 | 1e-3      | 1e-5      | 0.95    | 0.96
```

---

# 110. 결과 해석

예:

```text
layer4만 Fine-tuning했을 때 가장 좋았다.
```

라고 할 수 있다.

하지만 한 번의 실험만으로 일반적인 법칙이라고 단정하지 않는다.

---

# 111. Fine-tuning 범위

ResNet18에서 예를 들어:

```text
Option 1
fc only

Option 2
layer4 + fc

Option 3
layer3 + layer4 + fc

Option 4
all layers
```

를 비교할 수 있다.

---

# 112. 점진적 Unfreezing

단계적으로:

```text
fc

↓

layer4 + fc

↓

layer3 + layer4 + fc

↓

전체
```

를 열어가는 방법도 생각할 수 있다.

---

# 113. 매 Stage마다 Validation

Layer를 더 열 때마다 실제 Validation 성능이 좋아지는지 확인한다.

---

# 114. 더 많이 학습한다고 항상 좋아지는 것은 아니다

Trainable Parameter가 많아지면:

```text
적응 능력 증가
```

와 동시에:

```text
Overfitting 위험 증가
```

도 가능하다.

---

# 115. Trainable Parameter 확인 함수

```python
def print_trainable_parameters(
    model
):

    for name, param in model.named_parameters():

        if param.requires_grad:

            print(
                name
            )
```

---

# 116. Parameter 수 함수

```python
def count_parameters(
    model
):

    total = sum(
        p.numel()
        for p in model.parameters()
    )


    trainable = sum(
        p.numel()
        for p in model.parameters()
        if p.requires_grad
    )


    print(
        "Total:",
        total
    )


    print(
        "Trainable:",
        trainable
    )
```

---

# 117. 왜 매 Stage마다 확인할까?

코드에서는 Unfreeze했다고 생각했지만 실제로 다른 Layer를 열었거나 Optimizer에 Parameter가 빠질 수 있기 때문이다.

---

# 118. Optimizer 확인

```python
for i, group in enumerate(
    optimizer.param_groups
):

    print(
        "Group:",
        i,
        "LR:",
        group[
            "lr"
        ]
    )
```

---

# 119. Learning Rate 확인

Scheduler까지 사용하면 현재 LR가 바뀔 수 있으므로 확인하는 습관이 좋다.

---

# 120. Fine-tuning Debugging 1

```text
Stage 2인데 성능이 전혀 변하지 않는다.
```

확인:

```text
정말 Layer가 Unfreeze되었는가?

requires_grad=True인가?

Optimizer에 해당 Parameter가 들어갔는가?
```

---

# 121. Debugging 2

Stage 2에서:

```python
for name, param in model.named_parameters():

    if param.requires_grad:

        print(
            name
        )
```

를 확인한다.

---

# 122. Debugging 3

Optimizer를 Stage 2용으로 다시 생성했는지 확인한다.

---

# 123. Debugging 4

Learning Rate가 너무 작은지 확인한다.

너무 작으면 Weight가 거의 변하지 않아 학습이 매우 느릴 수 있다.

---

# 124. Debugging 5

Learning Rate가 너무 큰지 확인한다.

증상:

```text
Loss 급증

Validation 급락

Training 불안정
```

등이 나타날 수 있다.

---

# 125. Debugging 6

Stage 1 Best Weight에서 시작했는지 확인한다.

실수로 마지막 Weight나 초기 Weight에서 Stage 2를 시작할 수도 있다.

---

# 126. Debugging 7

Classifier Output:

```text
[B, num_classes]
```

인지 확인한다.

---

# 127. Debugging 8

Input Transform이 Pretrained Model과 호환되는지 확인한다.

```text
Image Size

Channel

Normalize
```

등이다.

---

# 128. Debugging 9

Class Mapping을 확인한다.

```text
0 = Normal

1 = Pneumonia
```

등이 Train/Validation/Test에서 동일해야 한다.

---

# 129. Debugging 10

Data Leakage도 확인한다.

Fine-tuning 성능이 비정상적으로 높아도:

```text
Patient Leakage

Duplicate Image

Train/Val 중복
```

가 있다면 신뢰할 수 없다.

---

# 130. Fine-tuning과 GPU

Backbone을 많이 Unfreeze할수록 Gradient를 저장해야 하는 Parameter와 Activation 계산 부담이 증가할 수 있다.

---

# 131. 결과

```text
GPU Memory 사용량 증가

Training 시간 증가
```

가 발생할 수 있다.

---

# 132. OOM

```text
CUDA out of memory
```

가 발생하면:

```text
Batch Size 감소

Image Size 조정

Model 크기 조정

Mixed Precision 고려
```

등을 생각한다.

---

# 133. Classifier Only Training은 비교적 가볍다

Backbone Parameter Update를 하지 않기 때문에 전체 Fine-tuning보다 Training 부담이 줄어들 수 있다.

---

# 134. Fine-tuning과 Inference

Fine-tuning 여부는 Training 방법의 차이다.

Inference에서는 최종적으로 학습된 Model을 사용한다.

```text
Image
↓
Final Model
↓
Prediction
```

이다.

---

# 135. Test에서는 Weight를 Update하지 않는다

```python
model.eval()


with torch.no_grad():

    logits = model(
        images
    )
```

이다.

---

# 136. Fine-tuning 후 Model 저장

```python
torch.save(
    model.state_dict(),
    "fine_tuned_model.pt"
)
```

---

# 137. 더 좋은 방법

Validation Best Model을 저장한다.

```python
if val_loss < best_val_loss:

    torch.save(
        model.state_dict(),
        "best_fine_tuned_model.pt"
    )
```

---

# 138. Checkpoint

Training을 이어서 진행하려면 Model Weight뿐 아니라:

```text
Optimizer

Epoch

Scheduler

Best Metric
```

등도 저장할 수 있다.

---

# 139. 예

```python
torch.save(
    {
        "epoch": epoch,

        "model_state_dict":
            model.state_dict(),

        "optimizer_state_dict":
            optimizer.state_dict(),

        "best_val_loss":
            best_val_loss
    },

    "checkpoint.pt"
)
```

---

# 140. Stage 정보도 기록

예:

```text
stage = 2

unfreeze = layer4

lr_backbone = 1e-5

lr_classifier = 1e-4
```

등을 실험 기록에 남긴다.

---

# 141. Fine-tuning과 Failure Analysis

성능 숫자만 비교하지 않는다.

Stage 1에서 틀렸던 Image를 Stage 2에서는 맞췄는지 볼 수 있다.

---

# 142. 반대도 확인

Stage 1에서 맞췄는데 Stage 2에서 틀린 Image도 있을 수 있다.

---

# 143. 비교 질문

```text
어떤 유형의 Error가 줄었는가?

어떤 유형의 Error가 늘었는가?

Minority Class Recall은 개선됐는가?

Confidence는 어떻게 변했는가?
```

를 확인한다.

---

# 144. Grad-CAM과 연결

Fine-tuning 전후에 Model이 어떤 Image 영역을 보는지도 Grad-CAM으로 비교할 수 있다.

하지만 Grad-CAM만으로 Model이 올바른 의학적 근거를 사용한다고 확정할 수는 없다.

---

# 145. Fine-tuning의 목표

단순히:

```text
Train Accuracy를 100% 만들기
```

가 아니다.

목표:

```text
새로운 Data에서 더 좋은 Generalization
```

이다.

---

# 146. 전체 Stage 흐름

```text
Pretrained ResNet
↓
Classifier 교체
↓
Backbone Freeze
↓
Stage 1 Training
↓
Stage 1 Validation
↓
Best Stage 1 Weight
↓
layer4 Unfreeze
↓
Optimizer 재구성
↓
Learning Rate 감소
↓
Stage 2 Training
↓
Stage 2 Validation
↓
Best Fine-tuned Model
↓
Failure Analysis
↓
Test Inference
```

---

# 147. 핵심 코드 흐름

Stage 1:

```python
for param in model.parameters():

    param.requires_grad = False


model.fc = nn.Linear(
    model.fc.in_features,
    2
)


optimizer = torch.optim.Adam(
    model.fc.parameters(),
    lr=1e-3
)
```

---

# 148. Stage 2

```python
for param in model.layer4.parameters():

    param.requires_grad = True


optimizer = torch.optim.Adam(
    [
        {
            "params":
                model.layer4.parameters(),

            "lr":
                1e-5
        },

        {
            "params":
                model.fc.parameters(),

            "lr":
                1e-4
        }
    ]
)
```

---

# 149. 가장 중요한 차이

```text
Freeze
→ Weight Update 안 함

Unfreeze
→ Weight Update 가능

Fine-tuning
→ Pretrained Weight를
  새 Dataset에 맞게 추가 조정
```

이다.

---

# 150. Fine-tuning을 시작할 때 질문

```text
Pretrained Model인가?

Classifier를 바꿨는가?

Stage 1에서 무엇을 Freeze했는가?

Trainable Parameter를 확인했는가?

Stage 1 Best Model을 저장했는가?

Stage 2에서 무엇을 Unfreeze할 것인가?

Optimizer를 다시 만들었는가?

Learning Rate를 낮췄는가?

Validation Metric은 개선됐는가?

Overfitting은 없는가?
```

---

# 151. 의료영상에서 추가 질문

```text
Patient-level Split인가?

Domain Shift는 어느 정도인가?

ImageNet Pretraining이 실제로 도움이 되는가?

Fine-tuning 후 Minority Class Recall은 어떤가?

False Negative가 증가하지 않았는가?

Model이 해부학적으로 타당한 영역을 보는가?
```

---

# 152. Fine-tuning 실험 해석

좋은 설명:

```text
Pretrained Backbone을 먼저 고정한 상태에서
Classifier를 학습하였다.

이후 ResNet의 마지막 Block인 layer4를
Unfreeze하고 더 작은 Learning Rate를 적용하여
새로운 Dataset에 맞게 Feature를 Fine-tuning하였다.

동일한 Validation Set에서 Stage 1과 Stage 2를 비교하였다.
```

---

# 153. 좋지 않은 설명

```text
Fine-tuning을 했더니 좋아졌다.
```

만으로 끝내면:

```text
무엇을 열었는지

Learning Rate가 무엇인지

어떤 Metric이 좋아졌는지
```

알 수 없다.

---

# 154. 발표에서는

다음처럼 표현할 수 있다.

```text
Stage 1
Feature Extractor 고정

↓

새 Classifier 학습

↓

Stage 2
마지막 Feature Block Unfreeze

↓

낮은 Learning Rate로 Fine-tuning

↓

Validation 성능 비교
```

---

# 155. Fine-tuning은 무조건 성능을 높이는 기술이 아니다

잘못된 설정에서는 오히려 성능이 낮아질 수 있다.

따라서:

```text
가설

실험

Validation

비교
```

가 필요하다.

---

# 156. 가장 중요한 원칙

```text
Pretrained Weight를
무작정 전부 다시 학습시키는 것이 아니라

새로운 Dataset에 필요한 만큼
조심스럽게 조정한다.
```

---

# 157. 한 줄 정리

Fine-tuning은:

```text
Pretrained Model의 일부 또는 전체 Weight를
새로운 Dataset에 맞게 작은 Learning Rate 등으로
추가 학습하여 새로운 Task에 적응시키는 과정
```

이다.

---

# 158. 다음 단계

다음 파일:

```text
image-evaluation.md
```

에서는 Image Classification Model을 학습한 뒤:

```text
Accuracy

Precision

Recall

F1-score

Specificity

ROC-AUC

PR-AUC

Confusion Matrix

Probability

Confidence

Threshold

Failure Case
```

를 어떻게 평가하고 해석하는지 공부한다.

특히:

```text
Accuracy가 높은데 왜 Model이 좋지 않을 수 있는가?

False Positive와 False Negative는 무엇인가?

의료영상에서는 어떤 Metric을 중요하게 봐야 하는가?

High-confidence Error는 무엇인가?

Threshold를 바꾸면 왜 Recall이 변하는가?
```

를 연결해서 정리한다.