# Validation & Overfitting

## 1. 왜 Validation이 필요한가?

Deep Learning Model의 목적은 Training Data를 외우는 것이 아니다.

목표는:

```text
처음 보는 새로운 Data에서도
잘 Prediction하는 Model
```

을 만드는 것이다.

이를:

```text
Generalization
```

이라고 한다.

---

# 2. Training 성능만 보면 안 되는 이유

예를 들어:

```text
Train Accuracy
99%
```

가 나왔다고 하자.

이것만 보고:

```text
Model 성능 = 99%
```

라고 판단하면 안 된다.

Model이 Training Data를 외웠을 수도 있기 때문이다.

---

# 3. 그래서 데이터를 나눈다

기본적으로:

```text
Train
Validation
Test
```

로 나눌 수 있다.

---

# 4. Train Data

Train Data의 역할:

```text
Model Parameter 학습
```

이다.

즉:

```text
Weight
Bias
```

를 학습하는 데 사용한다.

---

# 5. Validation Data

Validation Data의 역할:

```text
학습 중 Model 성능 확인
```

이다.

예:

```text
Hyperparameter 비교

Best Epoch 선택

Model 구조 비교

Learning Rate 비교

Overfitting 확인

Early Stopping
```

등이다.

---

# 6. Test Data

Test Data는 일반적으로:

```text
최종 Model 평가
```

에 사용한다.

즉:

```text
Train
→ 학습

Validation
→ 선택

Test
→ 최종 평가
```

이다.

---

# 7. 중요한 원칙

Validation과 Test Data로:

```text
loss.backward()

optimizer.step()
```

을 실행하지 않는다.

즉 Model Parameter를 직접 학습시키지 않는다.

---

# 8. Training

```python
model.train()

for x, y in train_loader:

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

# 9. Validation

```python
model.eval()

with torch.no_grad():

    for x, y in val_loader:

        outputs = model(
            x
        )

        loss = loss_fn(
            outputs,
            y
        )
```

Validation에는:

```text
Backward 없음

Optimizer Step 없음
```

이다.

---

# 10. Generalization

Generalization은 Model이 Training에서 보지 않은 Data에서도 잘 동작하는 능력이다.

좋은 Model의 목표:

```text
Train Data에서만 잘함
```

이 아니라:

```text
새로운 Data에서도 잘함
```

이다.

---

# 11. Overfitting

Overfitting:

```text
과적합
```

이다.

Model이 Training Data에 지나치게 맞춰져 새로운 Data에서 성능이 떨어지는 현상이다.

---

# 12. Overfitting 예

```text
Train Accuracy
99%

Validation Accuracy
82%
```

처럼 큰 차이가 나타날 수 있다.

---

# 13. Loss로 보면

예:

```text
Epoch 1

Train Loss = 0.80
Val Loss   = 0.85
```

```text
Epoch 5

Train Loss = 0.30
Val Loss   = 0.40
```

```text
Epoch 10

Train Loss = 0.10
Val Loss   = 0.65
```

이다.

---

# 14. 해석

Training Loss:

```text
0.80
→
0.30
→
0.10
```

계속 좋아지고 있다.

하지만 Validation Loss:

```text
0.85
→
0.40
→
0.65
```

어느 순간 다시 나빠졌다.

이런 패턴은 Overfitting 신호일 수 있다.

---

# 15. Learning Curve

Epoch에 따른:

```text
Train Loss

Validation Loss
```

를 Graph로 그리면 Model의 학습 상태를 확인하기 쉽다.

---

# 16. 기록

```python
train_losses = []
val_losses = []
```

Epoch마다:

```python
train_losses.append(
    train_loss
)

val_losses.append(
    val_loss
)
```

---

# 17. Graph

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

# 18. 정상적으로 학습되는 패턴의 한 예

```text
Train Loss
↓ ↓ ↓

Val Loss
↓ ↓ ↓
```

두 Loss가 함께 감소한다면 좋은 신호일 수 있다.

---

# 19. Overfitting 패턴

```text
Train Loss
↓ ↓ ↓ ↓ ↓

Val Loss
↓ ↓ ↓ ↑ ↑
```

이다.

Validation Loss가 가장 낮았던 지점 이후 Model이 Training Data에 더 강하게 맞춰지고 있을 수 있다.

---

# 20. Accuracy에서도 확인 가능

```text
Train Accuracy
↑ ↑ ↑ ↑

Validation Accuracy
↑ ↑ → ↓
```

같은 패턴이 나타날 수 있다.

---

# 21. Train과 Validation 차이

이를:

```text
Generalization Gap
```

이라고 표현하기도 한다.

예:

```text
Train Acc = 0.99
Val Acc   = 0.85

Gap = 0.14
```

이다.

---

# 22. Gap이 크면 무조건 Overfitting인가?

반드시 그렇다고 단정할 수는 없다.

다음도 확인해야 한다.

```text
Train / Validation 분포 차이

Split 방법

Label 품질

Dataset 크기

Augmentation

Metric 변동성
```

등이다.

---

# 23. Underfitting

반대 개념:

```text
Underfitting
```

한국어:

```text
과소적합
```

이다.

Model이 Training Data의 Pattern조차 충분히 학습하지 못한 상태이다.

---

# 24. Underfitting 예

```text
Train Accuracy = 60%

Val Accuracy   = 58%
```

둘 다 낮다.

---

# 25. Loss 예

```text
Train Loss
높음

Validation Loss
높음
```

이고 충분한 Training 후에도 잘 개선되지 않을 수 있다.

---

# 26. Underfitting 가능한 원인

```text
Model이 너무 단순함

Training Epoch 부족

Learning Rate 문제

잘못된 Feature / Data

과도한 Regularization

Preprocessing 문제
```

등이 있다.

---

# 27. Overfitting 가능한 원인

```text
Model이 지나치게 복잡함

Training Data 부족

Epoch가 너무 많음

Noise 학습

Augmentation 부족

Regularization 부족

Data Leakage / Split 문제
```

등을 확인할 수 있다.

---

# 28. 세 가지 상태

### Underfitting

```text
Train 성능 낮음
Val 성능 낮음
```

### 적절한 학습

```text
Train 성능 좋음
Val 성능도 좋음
```

### Overfitting

```text
Train 성능 매우 좋음
Val 성능 상대적으로 나쁨
```

---

# 29. Epoch를 많이 돌리면 무조건 좋은가?

아니다.

초반:

```text
Model이 Pattern 학습
```

후반:

```text
Training Data의 세부 Noise까지 학습
```

할 수 있다.

따라서 Epoch가 많다고 무조건 좋은 것은 아니다.

---

# 30. Best Epoch

예:

```text
Epoch 1
Val Acc = 0.82

Epoch 2
Val Acc = 0.87

Epoch 3
Val Acc = 0.91

Epoch 4
Val Acc = 0.90

Epoch 5
Val Acc = 0.88
```

가장 좋은 Validation Accuracy:

```text
Epoch 3
```

이다.

마지막 Epoch 5가 반드시 Best Model은 아니다.

---

# 31. Best Model 저장

```python
best_val_acc = 0.0


if val_acc > best_val_acc:

    best_val_acc = val_acc

    torch.save(
        model.state_dict(),
        "best_model.pt"
    )
```

---

# 32. Validation Loss 기준

```python
best_val_loss = float(
    "inf"
)


if val_loss < best_val_loss:

    best_val_loss = val_loss

    torch.save(
        model.state_dict(),
        "best_model.pt"
    )
```

---

# 33. 어떤 기준을 선택할까?

목표에 따라 달라진다.

예:

```text
Competition Metric
→ Accuracy

불균형 Classification
→ F1 / PR-AUC 등

의료 Screening
→ Recall / Sensitivity 중요 가능

Probability 품질
→ Loss / Calibration 등
```

즉 항상 Validation Loss 하나만 보는 것은 아니다.

---

# 34. Early Stopping

Validation 성능이 일정 기간 개선되지 않으면 Training을 중단하는 방법이다.

```text
Early Stopping
```

이라고 한다.

---

# 35. 왜 사용할까?

Model이 계속 Training Data에만 맞춰지는 것을 줄이고 불필요한 Training 시간을 절약할 수 있다.

---

# 36. 예

```text
Epoch 1
Val Loss = 0.80

Epoch 2
Val Loss = 0.60

Epoch 3
Val Loss = 0.50

Epoch 4
Val Loss = 0.51

Epoch 5
Val Loss = 0.53

Epoch 6
Val Loss = 0.55
```

Best:

```text
Epoch 3
```

이다.

---

# 37. patience

```text
patience
```

는 몇 Epoch 동안 개선이 없어도 기다릴지를 나타낸다.

예:

```python
patience = 3
```

---

# 38. 개념

```text
Best Epoch
↓
1 Epoch 개선 없음
↓
2 Epoch 개선 없음
↓
3 Epoch 개선 없음
↓
Stop
```

이다.

---

# 39. 간단한 Early Stopping 구현

```python
best_val_loss = float(
    "inf"
)

patience = 3
counter = 0


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

    if val_loss < best_val_loss:

        best_val_loss = val_loss

        counter = 0

        torch.save(
            model.state_dict(),
            "best_model.pt"
        )

    else:

        counter += 1

    if counter >= patience:

        print(
            "Early Stopping"
        )

        break
```

---

# 40. min_delta

아주 작은 개선까지 모두 개선으로 볼 것인지 정할 수도 있다.

예:

```python
min_delta = 0.001
```

---

# 41. 조건

```python
if val_loss < (
    best_val_loss
    -
    min_delta
):
```

처럼 사용할 수 있다.

---

# 42. 의미

Validation Loss가:

```text
최소 0.001 이상 개선
```

되어야 실제 개선으로 인정하는 방식이다.

---

# 43. Early Stopping의 주의점

Validation 값은 Epoch마다 조금씩 흔들릴 수 있다.

그래서:

```text
한 번 나빠졌다고 즉시 Stop
```

하기보다 `patience`를 사용한다.

---

# 44. Early Stopping은 Test를 보지 않는다

중요하다.

```text
Validation
```

을 기준으로 Early Stopping한다.

```text
Test
```

를 매 Epoch 확인하면서 Best Epoch를 고르면 Test가 사실상 Validation처럼 사용된다.

---

# 45. Overfitting 해결 방법

대표적으로:

```text
Early Stopping

Data Augmentation

Dropout

Weight Decay

Model Complexity 감소

더 많은 Training Data

Transfer Learning

적절한 Validation 전략
```

등이 있다.

---

# 46. Dropout

Dropout은 Training 중 일부 Neuron을 확률적으로 비활성화하는 Regularization 기법이다.

PyTorch:

```python
nn.Dropout(
    p=0.5
)
```

---

# 47. 예

```python
model = nn.Sequential(
    nn.Linear(
        100,
        64
    ),

    nn.ReLU(),

    nn.Dropout(
        p=0.5
    ),

    nn.Linear(
        64,
        2
    )
)
```

---

# 48. p=0.5 의미

Training 중 각 요소를 일정 확률로 Drop한다.

```text
p = 0.5
```

는 Drop probability가 0.5라는 의미이다.

---

# 49. Dropout의 목적

Model이 특정 Neuron 조합에 지나치게 의존하는 것을 줄여 Generalization에 도움을 줄 수 있다.

---

# 50. Training과 Validation에서 Dropout이 다르다

Training:

```python
model.train()
```

Dropout 활성화.

Validation:

```python
model.eval()
```

Dropout 비활성화.

---

# 51. 그래서 model.train() / eval()이 중요

Dropout이 포함된 Model에서:

```python
model.eval()
```

을 빼먹으면 Validation Prediction이 의도한 평가 모드와 달라질 수 있다.

---

# 52. Dropout이 크면 무조건 좋은가?

아니다.

Dropout이 지나치게 강하면 Model이 충분히 학습하지 못할 수 있다.

즉:

```text
Overfitting 완화
```

를 위해 사용하지만 너무 강하면:

```text
Underfitting
```

으로 이어질 수 있다.

---

# 53. Weight Decay

Optimizer에서:

```python
optimizer = torch.optim.AdamW(
    model.parameters(),
    lr=0.001,
    weight_decay=0.01
)
```

처럼 사용할 수 있다.

---

# 54. 목적

Parameter가 지나치게 커지거나 Model이 너무 복잡한 형태로 Training Data에 맞춰지는 것을 억제하는 Regularization과 관련된다.

---

# 55. Weight Decay도 Hyperparameter

예:

```text
0

1e-5

1e-4

1e-3

1e-2
```

등을 실험할 수 있다.

무조건 큰 값이 좋은 것은 아니다.

---

# 56. Dropout vs Weight Decay

Dropout:

```text
Training 중 일부 Activation을 확률적으로 Drop
```

Weight Decay:

```text
Parameter 크기를 규제하는 방향으로 Optimization
```

이다.

둘 다 Overfitting 완화에 활용할 수 있지만 작동 방식은 다르다.

---

# 57. Data Augmentation

Image Deep Learning에서 매우 중요한 방법이다.

Training Image를 조금씩 변형하여 Model이 다양한 입력을 경험하도록 한다.

예:

```text
Flip

Rotation

Crop

Brightness 변화

Contrast 변화
```

등이다.

---

# 58. 목적

원본 Image 하나를 그대로 반복해서 보는 것보다 다양한 형태를 경험하게 하여 Generalization을 높이는 데 도움을 줄 수 있다.

---

# 59. torchvision 예

```python
from torchvision import transforms


train_transform = transforms.Compose(
    [
        transforms.Resize(
            (
                224,
                224
            )
        ),

        transforms.RandomHorizontalFlip(),

        transforms.RandomRotation(
            10
        ),

        transforms.ToTensor()
    ]
)
```

---

# 60. Validation Transform

Validation에는 보통 Random Augmentation을 넣지 않는다.

```python
val_transform = transforms.Compose(
    [
        transforms.Resize(
            (
                224,
                224
            )
        ),

        transforms.ToTensor()
    ]
)
```

---

# 61. 이유

Validation은 같은 기준으로 Model 성능을 안정적으로 평가해야 하기 때문이다.

---

# 62. Train vs Validation Transform

Training:

```text
Resize
Random Augmentation
Tensor
Normalize
```

Validation:

```text
Resize
Tensor
Normalize
```

처럼 구성할 수 있다.

---

# 63. Normalize는 동일하게

Pretrained Model을 사용하는 경우 Training과 Validation에 동일한 Normalization 기준을 적용하는 것이 일반적이다.

---

# 64. 의료 Image에서는 특히 주의

Augmentation은:

```text
많이 변형할수록 좋다
```

가 아니다.

의료 Image의 의미를 바꾸는 변형은 문제가 될 수 있다.

---

# 65. 예

특정 해부학적 방향이나 위치가 의미를 가진다면:

```text
Flip

Rotation

Crop
```

이 Label 의미나 임상적 정보를 훼손하지 않는지 확인해야 한다.

---

# 66. Augmentation의 원칙

```text
현실적으로 발생 가능한 변화인가?

Label 의미를 유지하는가?

중요한 병변을 잘라내지 않는가?
```

를 생각한다.

---

# 67. 더 많은 Data

Overfitting을 줄이는 가장 직접적인 방법 중 하나는 다양하고 품질 좋은 Training Data를 더 확보하는 것이다.

하지만 실제 프로젝트에서는 Data 추가가 쉽지 않을 수 있다.

그래서 Augmentation, Transfer Learning, Regularization 등을 함께 사용한다.

---

# 68. Model Complexity 줄이기

Model이 Dataset에 비해 지나치게 크다면 Overfitting 가능성이 높아질 수 있다.

예:

```text
너무 많은 Layer

너무 많은 Hidden Unit

너무 큰 Model
```

등이다.

---

# 69. 간단한 Model로 시작

처음에는:

```text
Baseline Model
```

부터 만든다.

그 후 Validation 결과를 보면서 Model Complexity를 조절한다.

---

# 70. Transfer Learning

Data가 많지 않은 Image Classification에서는 Pretrained Model을 사용하는 방법이 효과적일 수 있다.

예:

```text
ResNet

DenseNet

EfficientNet
```

등이다.

---

# 71. Pretrained Model

이미 큰 Dataset에서 학습된 Feature Extraction 능력을 활용한다.

그 후 자신의 Dataset에 맞게 Classifier를 변경하고 Fine-tuning할 수 있다.

---

# 72. Freeze

초기에는 Backbone을 Freeze하고 마지막 Classifier만 학습할 수 있다.

```text
Pretrained Backbone
→ Freeze

Classifier
→ Train
```

---

# 73. Fine-tuning

그 후 일부 또는 전체 Layer를 Unfreeze하고 작은 Learning Rate로 추가 Training할 수 있다.

```text
Stage 1
Freeze

↓

Stage 2
Unfreeze

↓

Fine-tuning
```

---

# 74. Overfitting 해결은 하나만 있는 것이 아니다

예:

```text
Data 부족
→ Augmentation / Transfer Learning

Model 너무 복잡
→ Model 단순화

Epoch 너무 많음
→ Early Stopping

Regularization 부족
→ Dropout / Weight Decay
```

처럼 원인에 맞는 방법을 선택한다.

---

# 75. Underfitting 해결 방법

반대로 Underfitting이면:

```text
Model Capacity 증가

Epoch 증가

Regularization 감소

Learning Rate 재조정

Feature / Data 개선

Pretrained Model 활용
```

등을 검토할 수 있다.

---

# 76. Dropout을 무조건 추가하면 안 되는 이유

이미 Underfitting인 Model에 강한 Dropout을 추가하면 학습을 더 어렵게 만들 수 있다.

먼저:

```text
현재 문제가
Overfitting인지
Underfitting인지
```

확인한다.

---

# 77. Learning Curve가 중요한 이유

Train/Validation Curve를 보면 문제를 구분하는 데 도움이 된다.

```text
Train도 낮음
Val도 낮음
→ Underfitting 가능성

Train 높음
Val 낮음
→ Overfitting 가능성
```

---

# 78. Data Leakage

Validation 성능이 비정상적으로 높다면 Overfitting만 볼 것이 아니라:

```text
Data Leakage
```

도 확인해야 한다.

---

# 79. 예

같은 Patient의 Image가:

```text
Train

Validation
```

양쪽에 들어간다면 Model이 Patient 특성을 기억해 Validation 성능이 과대평가될 수 있다.

---

# 80. 의료 데이터에서 특히 중요

예:

```text
Patient A
Image 1 → Train

Patient A
Image 2 → Validation
```

이면 완전히 독립적인 Validation이라고 보기 어려울 수 있다.

---

# 81. Patient-level Split

의료 Image에서는 필요에 따라:

```text
Patient 단위
```

로 Train/Validation을 분리해야 한다.

```text
Patient A
→ Train에만

Patient B
→ Validation에만
```

처럼 구성한다.

---

# 82. Group Split

Scikit-learn에서는 상황에 따라:

```text
GroupShuffleSplit

GroupKFold

StratifiedGroupKFold
```

등을 고려할 수 있다.

---

# 83. Augmentation 후 Split하면 안 되는 이유

먼저 Augmentation을 만든 다음 무작위 Split하면:

```text
원본 Image
→ Train

거의 같은 Augmented Image
→ Validation
```

이 될 수 있다.

이는 Leakage를 만들 수 있다.

---

# 84. 올바른 흐름

```text
Original Data
↓
Train / Validation Split
↓
Train에만 Random Augmentation
↓
Validation에는 평가용 Transform
```

이다.

---

# 85. Validation Data를 계속 보고 수정하면?

Validation을 보고:

```text
Model 변경

Hyperparameter 변경

Threshold 변경

Augmentation 변경
```

을 반복한다.

이것 자체가 Validation의 역할이지만 너무 많은 반복을 하면 결국 Validation Set에도 간접적으로 과적합될 수 있다.

---

# 86. Validation Overfitting

이를:

```text
Validation Set에 대한 Overfitting
```

이라고 생각할 수 있다.

---

# 87. 그래서 Test가 필요하다

최종 Model 선택이 끝난 뒤:

```text
한 번도 Model 선택에 사용하지 않은 Test Data
```

에서 최종 평가한다.

---

# 88. Competition에서는

대회 구조:

```text
train.csv
→ Label 있음

test.csv
→ Label 없음

submission
→ Prediction 제출
```

일 수 있다.

이 경우 Local Validation 전략이 매우 중요하다.

---

# 89. Leaderboard도 Validation처럼 과적합 가능

Public Leaderboard 점수를 계속 확인하며 Model을 수정하면 Public Leaderboard에 맞춰진 선택을 하게 될 수 있다.

따라서:

```text
Local Validation
```

을 신뢰할 수 있게 설계하는 것이 중요하다.

---

# 90. Best Validation Accuracy만 보면 될까?

Class Imbalance가 있다면:

```text
Accuracy
```

만으로 부족할 수 있다.

예:

```text
Class 0 = 10%

Class 1 = 90%
```

이다.

---

# 91. 모두 Class 1로 Prediction

Accuracy:

```text
90%
```

가 될 수 있다.

하지만 Class 0은 하나도 찾지 못한다.

---

# 92. 따라서 함께 확인

```text
Confusion Matrix

Precision

Recall

F1

ROC-AUC

PR-AUC

Class별 Recall
```

등을 확인한다.

---

# 93. Best Model 기준도 목적에 맞게

예:

```text
대회 Metric = Accuracy
→ Accuracy 기준

Minority Detection 중요
→ Recall / F1 등

Ranking Probability 중요
→ ROC-AUC / PR-AUC 등
```

이다.

---

# 94. Validation Loss와 Validation Accuracy가 다르게 움직일 수 있다

예:

```text
Epoch 5

Val Acc = 0.90
Val Loss = 0.30
```

```text
Epoch 6

Val Acc = 0.90
Val Loss = 0.40
```

Accuracy는 같지만 Loss는 나빠질 수 있다.

---

# 95. 왜?

Accuracy는:

```text
최종 Class가 맞았는가?
```

만 본다.

Loss는:

```text
얼마나 자신 있게
맞거나 틀렸는가?
```

에도 영향을 받을 수 있다.

---

# 96. High-confidence Wrong Prediction

예:

정답:

```text
Class 0
```

Model:

```text
Class 1 Probability = 0.999
```

이면 매우 자신 있게 틀린 Prediction이다.

이런 Prediction은 Loss에 큰 영향을 줄 수 있다.

---

# 97. Failure Case Analysis

Validation에서 틀린 Sample을 직접 확인하는 것도 중요하다.

예:

```text
High-confidence Error

Low-confidence Error

False Positive

False Negative
```

를 확인한다.

---

# 98. 왜 확인할까?

숫자만 보면:

```text
왜 Model이 틀렸는지
```

알기 어렵다.

Image를 직접 보면:

```text
Label 오류

Image 품질

밝기 차이

Position 차이

특정 Pattern

Data Leakage 가능성
```

등의 Hypothesis를 만들 수 있다.

---

# 99. Computer Vision에서는 Grad-CAM도 활용 가능

CNN이 Image의 어느 영역을 보고 Prediction했는지 확인하기 위해:

```text
Grad-CAM
```

같은 방법을 사용할 수 있다.

하지만 Grad-CAM은 Model이 실제 임상적 원인을 이해했다는 증거는 아니다.

Model Attention을 해석하는 보조 도구로 사용한다.

---

# 100. Overfitting과 Augmentation 실험

Baseline:

```text
No Augmentation
```

Experiment:

```text
Random Rotation
Brightness
Flip
```

등을 추가한 후 같은 Validation Set에서 비교한다.

---

# 101. 중요한 것

Augmentation을 추가했는데:

```text
Train Accuracy 감소
```

할 수 있다.

이것만 보고 실패라고 판단하면 안 된다.

Training Data가 더 어려워졌기 때문이다.

---

# 102. 예

Baseline:

```text
Train Acc = 99%
Val Acc   = 90%
```

Augmentation:

```text
Train Acc = 95%
Val Acc   = 93%
```

라면 Train Accuracy는 낮아졌지만 Generalization은 좋아진 것이다.

---

# 103. Regularization의 특징

Regularization을 사용하면 Training 성능이 조금 떨어질 수 있다.

하지만 Validation 성능이 좋아질 수 있다.

목적은:

```text
Train Score 최대화
```

가 아니라:

```text
Generalization 개선
```

이다.

---

# 104. Weight Decay 실험 예

Baseline:

```python
optimizer = torch.optim.Adam(
    model.parameters(),
    lr=0.001
)
```

Experiment:

```python
optimizer = torch.optim.AdamW(
    model.parameters(),
    lr=0.001,
    weight_decay=0.01
)
```

Validation 결과를 비교한다.

---

# 105. Dropout 실험 예

Baseline:

```python
nn.Linear(
    128,
    64
)

nn.ReLU()
```

Experiment:

```python
nn.Linear(
    128,
    64
)

nn.ReLU()

nn.Dropout(
    0.3
)
```

이다.

---

# 106. 한 번에 하나씩 비교

좋은 Experiment:

```text
Baseline

↓

Dropout만 추가

↓

Validation 비교
```

이다.

---

# 107. 좋지 않은 비교

```text
Model 변경

Optimizer 변경

Learning Rate 변경

Augmentation 변경

Dropout 추가
```

를 한꺼번에 한 뒤 Baseline과 비교하면 무엇이 효과가 있었는지 알기 어렵다.

---

# 108. Experiment Table

예:

| Experiment | Augmentation | Dropout | Weight Decay | Val Acc |
|---|---|---:|---:|---:|
| Baseline | X | 0 | 0 | 0.90 |
| A | O | 0 | 0 | 0.93 |
| B | X | 0.3 | 0 | 0.92 |
| C | X | 0 | 0.01 | 0.91 |

이런 방식으로 정리하면 비교하기 쉽다.

---

# 109. Validation 결과의 변동성

한 번의 Split에서:

```text
Val Acc = 0.93
```

이 나왔다고 항상 0.93이 보장되는 것은 아니다.

Split이나 Random Seed에 따라 결과가 달라질 수 있다.

---

# 110. Cross Validation

Dataset이 작다면:

```text
K-Fold Cross Validation
```

등을 고려할 수 있다.

하지만 Deep Learning은 Training 비용이 크기 때문에 계산 비용도 함께 고려한다.

---

# 111. Image Data의 Cross Validation

Patient Image처럼 Group 구조가 있다면 일반 KFold보다:

```text
Group 기준 Split
```

이 더 중요할 수 있다.

Validation 방법은 Dataset 구조에 맞춰 선택해야 한다.

---

# 112. Time Data

시간 순서가 있는 Data라면 미래 Data를 이용해 과거를 학습하는 Split이 되지 않도록 주의한다.

즉 무조건 Random Split이 정답은 아니다.

---

# 113. Validation 전략이 Model만큼 중요

좋지 않은 Validation:

```text
Model이 실제보다 좋아 보임
```

좋은 Validation:

```text
실제 새로운 Data 성능을
더 현실적으로 추정
```

할 수 있다.

---

# 114. Checkpoint

Best Model뿐 아니라 Training 상태를 저장할 수도 있다.

```python
checkpoint = {
    "epoch": epoch,
    "model_state_dict": model.state_dict(),
    "optimizer_state_dict": optimizer.state_dict(),
    "best_val_loss": best_val_loss
}


torch.save(
    checkpoint,
    "checkpoint.pt"
)
```

---

# 115. Best Model과 Last Model

구분한다.

```text
Best Model
→ Validation 기준 가장 좋았던 Model

Last Model
→ 마지막 Epoch Model
```

둘은 다를 수 있다.

---

# 116. 예

```text
Epoch 7
Val Acc = 0.95 ← Best

Epoch 15
Val Acc = 0.90 ← Last
```

이면 최종 사용 Model로 Epoch 7 Weight를 선택할 수 있다.

---

# 117. Early Stopping과 Best Model 저장은 같이 사용 가능

```text
Validation 개선
→ Best Model 저장

Validation 개선 없음
→ Counter 증가

Patience 초과
→ Training 종료
```

이다.

---

# 118. 전체 예

```python
best_val_loss = float(
    "inf"
)

patience = 3
counter = 0


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

    print(
        f"Epoch {epoch + 1}/{epochs} | "
        f"Train Loss: {train_loss:.4f} | "
        f"Train Acc: {train_acc:.4f} | "
        f"Val Loss: {val_loss:.4f} | "
        f"Val Acc: {val_acc:.4f}"
    )

    if val_loss < best_val_loss:

        best_val_loss = val_loss

        counter = 0

        torch.save(
            model.state_dict(),
            "best_model.pt"
        )

    else:

        counter += 1

    if counter >= patience:

        print(
            "Early Stopping"
        )

        break
```

---

# 119. Training이 끝나면 Best Model Load

```python
model.load_state_dict(
    torch.load(
        "best_model.pt",
        map_location=device
    )
)
```

이후 최종 Validation 확인이나 Test Inference를 수행할 수 있다.

---

# 120. PyTorch 버전에 따른 Load 옵션

PyTorch 버전과 저장 방식에 따라 `torch.load()` 관련 옵션이나 권장 방식이 달라질 수 있다.

중요한 개념은:

```text
저장한 state_dict를 불러와
Model 구조에 적용한다.
```

이다.

---

# 121. Overfitting Debugging 순서

Overfitting처럼 보이면 바로 Dropout부터 넣기보다 다음을 확인한다.

```text
1. Split이 올바른가?

2. Leakage가 없는가?

3. Train / Validation 분포가 지나치게 다른가?

4. Label 오류가 없는가?

5. Learning Curve는 어떤가?

6. Model이 Dataset에 비해 너무 큰가?

7. Augmentation이 적절한가?

8. Regularization이 필요한가?

9. Epoch가 너무 많은가?
```

---

# 122. Underfitting Debugging 순서

```text
1. Data와 Label이 올바른가?

2. Model Output / Loss가 맞는가?

3. Learning Rate가 적절한가?

4. Training Epoch가 충분한가?

5. Model Capacity가 충분한가?

6. Regularization이 너무 강하지 않은가?

7. Input Preprocessing이 적절한가?
```

---

# 123. Validation 성능이 너무 좋다면

예:

```text
Train Acc = 95%

Val Acc = 100%
```

라고 해서 무조건 문제는 아니다.

하지만 다음을 확인할 필요가 있다.

```text
Validation Set이 너무 작은가?

중복 Sample이 있는가?

같은 Patient가 섞였는가?

파일명 / Metadata에 Label 정보가 있는가?

Preprocessing Leakage가 있는가?
```

---

# 124. Data Leakage 예

Model Input에:

```text
정답을 직접 또는 간접적으로 알려주는 정보
```

가 들어가면 Validation 성능이 비정상적으로 높을 수 있다.

---

# 125. Image Dataset의 파일명

예를 들어 파일명이:

```text
normal_001.png

pneumonia_001.png
```

이고 Model Input 자체에는 파일명이 들어가지 않는다면 CNN이 파일명을 보는 것은 아니다.

하지만 별도의 Feature로 파일명 정보를 넣었다면 Leakage가 될 수 있다.

---

# 126. Preprocessing Leakage

전체 Dataset으로 평균과 표준편차 같은 통계를 계산하고 Split 이후 사용하는 경우 문제에 따라 Validation 정보가 Training preprocessing에 들어갈 수 있다.

Tabular ML에서는 특히 주의한다.

---

# 127. Deep Learning Image Normalization

ImageNet Pretrained Model에서 ImageNet의 고정된 mean/std를 사용하는 것은 Training/Validation 전체에서 새로 추정한 통계와는 상황이 다르다.

이미 정의된 Pretrained Model preprocessing 규칙을 사용하는 것이다.

---

# 128. Validation은 학습하지 않지만 선택에는 사용된다

Validation Data는 Weight를 직접 Update하지 않는다.

하지만:

```text
Model 선택

Epoch 선택

Hyperparameter 선택
```

에는 사용된다.

따라서 Validation도 반복적인 의사결정에 영향을 준다.

---

# 129. Test의 역할

최종적으로:

```text
Validation을 이용한 모든 선택이 끝난 뒤
```

Test를 사용해야 더 공정한 최종 평가가 가능하다.

---

# 130. Overfitting을 한 문장으로

```text
Training Data에는 지나치게 잘 맞지만
새로운 Data에서는 성능이 떨어지는 현상
```

이다.

---

# 131. Underfitting을 한 문장으로

```text
Training Data의 Pattern조차
충분히 학습하지 못한 상태
```

이다.

---

# 132. Generalization을 한 문장으로

```text
학습하지 않은 새로운 Data에서도
잘 동작하는 능력
```

이다.

---

# 133. Regularization을 한 문장으로

```text
Model이 Training Data에 지나치게 맞춰지는 것을
줄이기 위한 여러 방법
```

이라고 이해할 수 있다.

---

# 134. 대표 Regularization

```text
Dropout

Weight Decay

Data Augmentation

Early Stopping
```

등을 볼 수 있다.

엄밀히 말하면 각각 작동 방식은 다르지만 모두 Generalization 개선 전략으로 함께 공부할 수 있다.

---

# 135. 중요한 관점

Deep Learning 목표는:

```text
Train Accuracy를 최대한 100%로 만드는 것
```

이 아니다.

더 중요한 목표:

```text
새로운 Data에서
좋은 성능을 내는 Model
```

이다.

---

# 136. Chest X-ray 같은 문제에서

예를 들어:

```text
Train 성능 매우 높음
Validation 성능도 높음
Leaderboard 성능은 낮음
```

이라면 단순히 Epoch를 늘리기보다 다음을 확인해야 한다.

```text
Validation Split이 실제 Test 분포를 잘 대표하는가?

Leakage가 없는가?

Train / Test 분포 차이가 있는가?

Model이 밝기나 위치 같은 Shortcut을 학습했는가?

Augmentation이 적절한가?
```

---

# 137. Shortcut Learning

Model이 우리가 원하는 병변 특징이 아니라:

```text
밝기

촬영 장비

문자 표시

Image 위치

배경 Pattern
```

같은 쉬운 단서를 이용해 Prediction할 수 있다.

---

# 138. 그래서 Failure Analysis가 중요

```text
왜 맞았는가?

왜 틀렸는가?

어디를 보고 판단했는가?
```

를 확인한다.

Computer Vision에서는 Grad-CAM 등이 보조적으로 사용될 수 있다.

---

# 139. Validation Strategy와 실제 성능

Local Validation이 실제 Test 환경과 비슷할수록:

```text
Validation 성능
↔
실제 성능
```

의 관계가 더 의미 있어질 수 있다.

---

# 140. 최종 전체 흐름

```text
Original Data
↓
올바른 Split
↓
Train Dataset
↓
Train Augmentation
↓
Model Training
↓
Validation
↓
Learning Curve
↓
Overfitting / Underfitting 확인
↓
Regularization / Hyperparameter 실험
↓
Best Model 선택
↓
Test
```

---

# 141. Training Loop와 연결

Training:

```python
model.train()

loss.backward()

optimizer.step()
```

Validation:

```python
model.eval()

with torch.no_grad():
```

이다.

---

# 142. Best Model과 연결

```text
Epoch마다 Validation
↓
Metric 비교
↓
Best 갱신
↓
state_dict 저장
```

이다.

---

# 143. Early Stopping과 연결

```text
Validation 개선
→ 계속 Training

Validation 개선 없음
→ Counter 증가

Patience 도달
→ Stop
```

이다.

---

# 144. Overfitting 해결과 연결

```text
Overfitting 발견
↓
원인 확인
↓
필요한 방법 선택

Augmentation
Dropout
Weight Decay
Early Stopping
Model 단순화
Data 추가
```

이다.

---

# 145. Underfitting 해결과 연결

```text
Underfitting 발견
↓
원인 확인
↓
필요한 방법 선택

Epoch 증가
Model Capacity 증가
Regularization 감소
Learning Rate 조정
Data / Label 점검
```

이다.

---

# 146. 가장 중요한 비교

| 상태 | Train | Validation |
|---|---|---|
| Underfitting | 낮음 | 낮음 |
| 적절한 학습 | 높음 | 높음 |
| Overfitting | 매우 높음 | 상대적으로 낮음 |

단순화한 기준이므로 실제 판단에서는 Loss Curve, Metric, Dataset 특성도 함께 본다.

---

# 147. Best Epoch 선택

```text
Last Epoch
≠
Best Epoch
```

일 수 있다.

항상 마지막 Model을 사용하는 것이 아니라 Validation 기준 Best Model을 저장할 수 있다.

---

# 148. 가장 중요한 원칙

```text
Train
→ 학습

Validation
→ 선택

Test
→ 최종 평가
```

이다.

---

# 149. 다음 단계

지금까지:

```text
Tensor
↓
Dataset / DataLoader
↓
Neural Network
↓
Activation
↓
Loss
↓
Backpropagation
↓
Optimizer
↓
Training Loop
↓
Validation / Overfitting
```

까지 연결했다.

이제 실제 Deep Learning에서 매우 중요한:

```text
GPU
CUDA
Device
```

를 정리한다.

---

# 150. 최종 정리

Validation은:

```text
Model이 새로운 Data에서도
잘 동작하는지 확인
```

하기 위해 사용한다.

Overfitting:

```text
Train 성능은 좋지만
Validation 성능이 상대적으로 나쁨
```

이다.

Underfitting:

```text
Train 성능도 충분히 좋지 않음
```

이다.

Overfitting을 완화하기 위한 대표적인 방법:

```text
Early Stopping

Dropout

Weight Decay

Data Augmentation

Model Complexity 감소

더 많은 Data

Transfer Learning
```

등이 있다.

그리고 반드시 기억한다.

```text
Training에서는
Backward + Optimizer Step

Validation에서는
Backward 없음 + Optimizer Step 없음
```

최종 목표는:

```text
Training Data를 외우는 Model
```

이 아니라:

```text
처음 보는 새로운 Data에서도
잘 동작하는 Model
```

을 만드는 것이다.