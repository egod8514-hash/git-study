# Model Save & Load

## 1. 왜 Model을 저장할까?

Deep Learning Model을 학습하면 Model 내부의:

```text
Weight
Bias
```

가 학습된다.

예:

```text
처음 Model
↓
Random Weight

Training
↓
Weight Update

Training 완료
↓
학습된 Weight
```

이 학습 결과를 저장하지 않으면 프로그램이 종료된 뒤 다시 사용할 수 없다.

그래서 Model을 File로 저장한다.

---

# 2. Model 저장이 필요한 상황

대표적으로:

```text
학습 완료 후 Model 보관

Best Model 저장

Training 중간 Checkpoint 저장

Training 재개

Test Prediction

실제 서비스 배포

FastAPI에서 Model 사용
```

등이 있다.

---

# 3. PyTorch Model에는 무엇이 있을까?

예:

```python
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

Model에는:

```text
Model Architecture

Weight

Bias
```

등이 존재한다.

---

# 4. Parameter 확인

```python
for name, param in model.named_parameters():

    print(
        name,
        param.shape
    )
```

예:

```text
0.weight
0.bias
2.weight
2.bias
```

---

# 5. state_dict

PyTorch에서 매우 중요한 개념:

```python
model.state_dict()
```

이다.

---

# 6. state_dict란?

Model의 학습 가능한 Parameter와 필요한 상태들을:

```text
Dictionary 형태
```

로 가지고 있는 객체이다.

---

# 7. 확인

```python
print(
    model.state_dict()
)
```

---

# 8. Key 확인

```python
print(
    model.state_dict().keys()
)
```

예:

```text
0.weight
0.bias
2.weight
2.bias
```

등이 나타날 수 있다.

---

# 9. 쉽게 생각하면

```text
Model Architecture
=
Model의 설계도

state_dict
=
학습된 Model의 핵심 상태
```

라고 이해할 수 있다.

---

# 10. 가장 권장되는 기본 저장 방식

PyTorch에서는 일반적으로:

```python
torch.save(
    model.state_dict(),
    "model.pt"
)
```

처럼 `state_dict`를 저장한다.

---

# 11. 확장자

자주 볼 수 있는 확장자:

```text
.pt

.pth
```

이다.

둘 다 PyTorch Model 저장 파일에서 자주 사용된다.

---

# 12. 저장

```python
torch.save(
    model.state_dict(),
    "model.pt"
)
```

현재 작업 Directory에:

```text
model.pt
```

가 생성된다.

---

# 13. 특정 Folder에 저장

```python
torch.save(
    model.state_dict(),
    "./models/model.pt"
)
```

---

# 14. Folder가 없다면?

먼저 Folder를 만들 수 있다.

```python
import os


os.makedirs(
    "models",
    exist_ok=True
)
```

---

# 15. 전체

```python
import os
import torch


os.makedirs(
    "models",
    exist_ok=True
)


torch.save(
    model.state_dict(),
    "models/model.pt"
)
```

---

# 16. 저장된 Model을 불러오기

중요한 점:

`state_dict`만 저장했다면 먼저 같은 Architecture의 Model을 다시 만들어야 한다.

---

# 17. Model 구조 생성

```python
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

# 18. Weight 불러오기

```python
state_dict = torch.load(
    "model.pt"
)
```

---

# 19. Model에 적용

```python
model.load_state_dict(
    state_dict
)
```

---

# 20. 전체 흐름

```text
Model Architecture 생성
↓
저장된 state_dict Load
↓
Model에 state_dict 적용
↓
학습된 Model 복원
```

이다.

---

# 21. 왜 Architecture를 다시 만들어야 할까?

`state_dict` 방식은 기본적으로 Model 구조 자체를 정의하는 Python Code와 Parameter 상태를 분리해서 관리하기 때문이다.

예:

```text
Python Code
→ Model 구조

model.pt
→ 학습된 상태
```

이다.

---

# 22. Model 구조가 다르면?

저장 당시 Model:

```text
10
↓
32
↓
2
```

인데 불러오는 Model이:

```text
10
↓
64
↓
2
```

라면 Parameter Shape가 맞지 않는다.

오류가 발생할 수 있다.

---

# 23. 따라서 저장할 때 기록하면 좋은 것

```text
Model Architecture

Number of Classes

Input Size

Preprocessing

Normalization

Label Mapping
```

등이다.

Model Weight만 가지고 있다고 모든 Prediction 과정이 자동으로 복원되는 것은 아니다.

---

# 24. Device 문제

GPU에서 저장한 Model을 CPU에서 불러오거나 Device를 명확히 지정하고 싶다면:

```python
state_dict = torch.load(
    "model.pt",
    map_location=device
)
```

처럼 사용할 수 있다.

---

# 25. Device 설정

```python
device = torch.device(
    "cuda"
    if torch.cuda.is_available()
    else "cpu"
)
```

---

# 26. Load

```python
state_dict = torch.load(
    "model.pt",
    map_location=device
)
```

---

# 27. 적용

```python
model.load_state_dict(
    state_dict
)
```

---

# 28. Model도 Device 이동

```python
model = model.to(
    device
)
```

---

# 29. Inference 전 eval()

Prediction 전에:

```python
model.eval()
```

을 실행한다.

---

# 30. 왜?

Model에:

```text
Dropout

BatchNorm
```

등이 있다면 Training과 Evaluation 동작이 다르기 때문이다.

---

# 31. Inference에서는 Gradient 필요 없음

```python
with torch.no_grad():

    outputs = model(
        x
    )
```

을 사용할 수 있다.

---

# 32. Inference 전형적인 구조

```python
model.eval()


with torch.no_grad():

    outputs = model(
        x
    )

    pred = outputs.argmax(
        dim=1
    )
```

---

# 33. 전체 Load + Inference

```python
import torch
import torch.nn as nn


device = torch.device(
    "cuda"
    if torch.cuda.is_available()
    else "cpu"
)


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


state_dict = torch.load(
    "model.pt",
    map_location=device
)


model.load_state_dict(
    state_dict
)


model = model.to(
    device
)


model.eval()


x = torch.randn(
    1,
    10
).to(
    device
)


with torch.no_grad():

    outputs = model(
        x
    )

    pred = outputs.argmax(
        dim=1
    )


print(
    pred.item()
)
```

---

# 34. Save와 Load를 한 줄로 이해

저장:

```python
torch.save(
    model.state_dict(),
    "model.pt"
)
```

불러오기:

```python
model.load_state_dict(
    torch.load(
        "model.pt",
        map_location=device
    )
)
```

이다.

---

# 35. Best Model 저장

Training에서는 마지막 Epoch Model보다 Validation 성능이 가장 좋았던 Model을 저장하는 경우가 많다.

---

# 36. 예

```python
best_val_acc = 0.0
```

Epoch마다:

```python
if val_acc > best_val_acc:

    best_val_acc = val_acc

    torch.save(
        model.state_dict(),
        "best_model.pt"
    )
```

---

# 37. 의미

```text
현재 Validation Accuracy
>
기존 Best Accuracy

↓

Best 갱신

↓

Model 저장
```

이다.

---

# 38. Validation Loss 기준

```python
best_val_loss = float(
    "inf"
)
```

그리고:

```python
if val_loss < best_val_loss:

    best_val_loss = val_loss

    torch.save(
        model.state_dict(),
        "best_model.pt"
    )
```

---

# 39. Last Model과 Best Model

둘은 다를 수 있다.

예:

```text
Epoch 1
Val Acc = 0.82

Epoch 2
Val Acc = 0.88

Epoch 3
Val Acc = 0.93
← Best

Epoch 4
Val Acc = 0.91

Epoch 5
Val Acc = 0.89
← Last
```

---

# 40. 마지막 Model

Epoch 5:

```text
Last Model
```

---

# 41. 가장 좋은 Model

Epoch 3:

```text
Best Model
```

이다.

---

# 42. 그래서 Training 종료 후

```python
model.load_state_dict(
    torch.load(
        "best_model.pt",
        map_location=device
    )
)
```

를 통해 Best Weight를 다시 불러올 수 있다.

---

# 43. Early Stopping과 연결

```text
Training
↓
Validation
↓
Best Model 저장
↓
성능 개선 없음
↓
Patience 초과
↓
Early Stopping
↓
Best Model Load
```

이다.

---

# 44. Model 전체를 저장하는 방법도 있다

예:

```python
torch.save(
    model,
    "full_model.pt"
)
```

그리고:

```python
model = torch.load(
    "full_model.pt"
)
```

같은 형태의 코드도 볼 수 있다.

---

# 45. 하지만 일반적으로 state_dict 방식이 많이 권장된다

```python
torch.save(
    model.state_dict(),
    "model.pt"
)
```

방식을 우선 익힌다.

---

# 46. 이유

`state_dict` 방식은 Model Architecture를 Code로 명확하게 관리하고 Parameter 상태를 별도로 저장할 수 있다.

전체 객체 저장은 Python Class 정의와 실행 환경 등에 더 의존할 수 있다.

---

# 47. 따라서 기본 습관

```text
Model Code
+
state_dict File
```

을 관리하는 것이다.

---

# 48. Model Weight만 저장하면 Training을 이어갈 수 있을까?

가능은 하지만 정확히 이어서 Training하려면 Model Weight 외에도 필요한 정보가 있다.

예:

```text
Optimizer State

현재 Epoch

Best Metric

Scheduler State
```

등이다.

---

# 49. Checkpoint

이런 Training 상태를 함께 저장한 것을 보통:

```text
Checkpoint
```

라고 한다.

---

# 50. Checkpoint 예

```python
checkpoint = {
    "epoch": epoch,
    "model_state_dict": model.state_dict(),
    "optimizer_state_dict": optimizer.state_dict(),
    "val_loss": val_loss
}
```

---

# 51. 저장

```python
torch.save(
    checkpoint,
    "checkpoint.pt"
)
```

---

# 52. Dictionary를 저장한 것

즉:

```text
checkpoint.pt
```

안에:

```text
Epoch

Model Weight

Optimizer State

Validation Loss
```

등을 함께 저장한다.

---

# 53. Checkpoint 불러오기

```python
checkpoint = torch.load(
    "checkpoint.pt",
    map_location=device
)
```

---

# 54. Model 복원

```python
model.load_state_dict(
    checkpoint[
        "model_state_dict"
    ]
)
```

---

# 55. Optimizer 복원

```python
optimizer.load_state_dict(
    checkpoint[
        "optimizer_state_dict"
    ]
)
```

---

# 56. Epoch 복원

```python
start_epoch = (
    checkpoint[
        "epoch"
    ]
    +
    1
)
```

---

# 57. Validation Loss 복원

```python
best_val_loss = checkpoint[
    "val_loss"
]
```

---

# 58. Training 재개

```python
for epoch in range(
    start_epoch,
    epochs
):

    ...
```

처럼 이어갈 수 있다.

---

# 59. 왜 Optimizer State도 필요할까?

Adam 같은 Optimizer는 단순히 Learning Rate만 가지고 있는 것이 아니다.

Optimization 과정에서 내부 상태를 유지한다.

따라서 이전 Training을 자연스럽게 이어가려면 Optimizer State도 복원하는 것이 중요하다.

---

# 60. Scheduler가 있다면

Scheduler도 저장할 수 있다.

```python
checkpoint = {
    "epoch": epoch,
    "model_state_dict": model.state_dict(),
    "optimizer_state_dict": optimizer.state_dict(),
    "scheduler_state_dict": scheduler.state_dict()
}
```

---

# 61. Load

```python
scheduler.load_state_dict(
    checkpoint[
        "scheduler_state_dict"
    ]
)
```

---

# 62. Best Metric도 저장

```python
checkpoint = {
    "epoch": epoch,
    "model_state_dict": model.state_dict(),
    "optimizer_state_dict": optimizer.state_dict(),
    "best_val_acc": best_val_acc
}
```

---

# 63. Hyperparameter도 저장 가능

예:

```python
checkpoint = {
    "epoch": epoch,
    "model_state_dict": model.state_dict(),
    "optimizer_state_dict": optimizer.state_dict(),

    "learning_rate": 0.001,
    "batch_size": 32,
    "num_classes": 2
}
```

---

# 64. 하지만 모든 것을 Checkpoint에 넣어야 하나?

아니다.

프로젝트 규모에 따라:

```text
Config File

README

Experiment Log

Checkpoint
```

등으로 나누어 관리할 수 있다.

---

# 65. Checkpoint와 Best Model 차이

간단하게:

```text
Best Model
→ 최종 Prediction에 사용할 좋은 Weight

Checkpoint
→ Training을 이어가기 위한 상태
```

라고 구분하면 이해하기 쉽다.

---

# 66. 예

```text
best_model.pt

checkpoint_epoch_05.pt

checkpoint_epoch_10.pt
```

처럼 관리할 수 있다.

---

# 67. 매 Epoch 저장하면?

가능하지만 File이 많아지고 Storage를 많이 사용할 수 있다.

따라서:

```text
Best Model만 저장

일정 Epoch마다 Checkpoint 저장

최근 Checkpoint만 유지
```

같은 전략을 사용할 수 있다.

---

# 68. File 이름에 정보 넣기

예:

```python
file_name = (
    f"model_epoch_{epoch + 1}_"
    f"valacc_{val_acc:.4f}.pt"
)
```

---

# 69. 예

```text
model_epoch_03_valacc_0.9512.pt
```

처럼 저장할 수 있다.

---

# 70. 하지만 이름이 너무 길어지면 관리가 어려워진다

보통:

```text
best_model.pt

last_model.pt

checkpoint.pt
```

처럼 단순하게 두고 Experiment 정보는 별도로 기록하는 방법도 좋다.

---

# 71. state_dict 저장 후 확인

```python
torch.save(
    model.state_dict(),
    "best_model.pt"
)
```

저장 여부:

```python
import os


print(
    os.path.exists(
        "best_model.pt"
    )
)
```

---

# 72. File Size 확인

```python
print(
    os.path.getsize(
        "best_model.pt"
    )
)
```

---

# 73. Path 문제

자주 발생하는 문제:

```text
FileNotFoundError
```

이다.

예:

```python
torch.load(
    "best_model.pt"
)
```

했는데 현재 Working Directory에 파일이 없으면 발생할 수 있다.

---

# 74. 현재 Directory 확인

```python
import os


print(
    os.getcwd()
)
```

---

# 75. File 목록

```python
print(
    os.listdir()
)
```

---

# 76. 특정 Path 확인

```python
model_path = (
    "models/best_model.pt"
)


print(
    os.path.exists(
        model_path
    )
)
```

---

# 77. Colab Path

Colab에서는 예:

```text
/content/best_model.pt
```

같은 경로를 볼 수 있다.

---

# 78. Local Windows Path

예:

```text
C:\Users\user\project\models\best_model.pt
```

같은 형태이다.

---

# 79. Colab Runtime 종료

Colab의 임시 Runtime Storage에 저장한 File은 Runtime이 초기화되면 사라질 수 있다.

중요한 Model은 필요한 경우 영구적인 Storage로 별도 보관해야 한다.

---

# 80. 저장과 Git은 다르다

```python
torch.save(...)
```

는 Model File을 Disk에 저장하는 것이다.

```text
git add
git commit
```

은 Git Version Control이다.

완전히 다른 개념이다.

---

# 81. Model File을 GitHub에 올릴까?

큰 Deep Learning Weight File은 일반 Git Repository에 그대로 넣지 않는 경우가 많다.

이유:

```text
File Size 큼

Repository 용량 증가

Version 관리 비효율
```

등이다.

---

# 82. .gitignore

예:

```gitignore
*.pt
*.pth
checkpoints/
models/
```

처럼 Model Weight를 Git 추적에서 제외할 수 있다.

단 프로젝트 목적에 따라 필요한 작은 Weight를 별도로 관리할 수도 있다.

---

# 83. 큰 Model Artifact 관리

규모가 커지면:

```text
Cloud Storage

Model Registry

Artifact Storage

Git LFS
```

등 별도 방법을 사용할 수 있다.

---

# 84. Inference란?

학습된 Model을 사용해 새로운 Input의 Prediction을 만드는 과정이다.

```text
새로운 Data
↓
Preprocessing
↓
Model
↓
Prediction
```

이다.

---

# 85. Inference에서는 Training하지 않는다

따라서 일반적으로:

```text
Backward 없음

Optimizer Step 없음
```

이다.

---

# 86. Inference 기본

```python
model.eval()


with torch.no_grad():

    outputs = model(
        x
    )
```

---

# 87. Multi-class Prediction

```python
pred = outputs.argmax(
    dim=1
)
```

---

# 88. Probability가 필요하다면

```python
prob = torch.softmax(
    outputs,
    dim=1
)
```

---

# 89. Binary 1-logit Model

```python
prob = torch.sigmoid(
    outputs
)
```

---

# 90. Threshold

```python
pred = (
    prob >= 0.5
).long()
```

---

# 91. 저장한 Model만 맞으면 되는가?

아니다.

Inference에서도 Training 때와 동일한 Preprocessing 규칙이 중요하다.

예:

```text
Resize

Channel

Normalization

Feature 순서

Encoding
```

등이다.

---

# 92. Image Model 예

Training:

```text
Image
↓
Resize 224×224
↓
Tensor
↓
Normalize
↓
Model
```

이었다면 Inference에서도 같은 기본 전처리 규칙을 맞춰야 한다.

---

# 93. 단 Random Augmentation은 다르다

Training에서:

```text
RandomRotation

RandomHorizontalFlip
```

등을 사용했다고 Inference에서 매번 Random 변형을 하는 것은 일반적인 기본 방식이 아니다.

Inference에서는 보통 결정적인 평가용 Transform을 사용한다.

---

# 94. Pretrained Model이라면

Training에 사용한:

```text
Image Size

Normalization Mean

Normalization Std
```

등을 기록해 두는 것이 중요하다.

---

# 95. Label Mapping도 중요

예:

```python
class_names = {
    0: "Normal",
    1: "Pneumonia"
}
```

Model Output:

```text
1
```

이 무엇을 의미하는지 알아야 한다.

---

# 96. 저장해야 할 것은 Weight만이 아니다

실제 Model 사용을 위해 필요한 정보:

```text
Model Architecture

Weight

Input Shape

Preprocessing

Class Mapping

Threshold

Library Version
```

등이 있을 수 있다.

---

# 97. Threshold도 저장할 수 있다

예를 들어 Validation에서:

```text
Best Threshold = 0.37
```

을 선택했다면 실제 Inference에서도:

```python
pred = (
    prob >= 0.37
).long()
```

을 사용해야 동일한 Decision Rule을 적용할 수 있다.

---

# 98. Threshold를 Test에서 선택하면 안 되는 이유

Threshold는 Validation Data를 이용해 결정하고 Test는 최종 평가에 사용하는 것이 원칙이다.

---

# 99. Model Version

실제 프로젝트에서는:

```text
model_v1.pt

model_v2.pt

model_v3.pt
```

처럼 Version을 관리할 수 있다.

---

# 100. 하지만 Weight 이름만으로는 부족

다음도 함께 기록하면 좋다.

```text
어떤 Dataset?

어떤 Split?

어떤 Model?

어떤 Epoch?

어떤 Learning Rate?

어떤 Augmentation?

어떤 Metric?
```

---

# 101. Experiment 기록 예

```text
Experiment: C

Model:
ResNet18

Optimizer:
AdamW

Learning Rate:
0.0001

Batch Size:
32

Loss:
Weighted CrossEntropy

Best Epoch:
4

Val Accuracy:
0.9856
```

---

# 102. Model 저장과 재현성

나중에 Model을 다시 사용할 때:

```text
Weight는 있는데
어떻게 학습했는지 모름
```

상태가 되지 않도록 Experiment를 기록한다.

---

# 103. Model Architecture를 함수로 만들기

예:

```python
def create_model():

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

    return model
```

---

# 104. Training

```python
model = create_model().to(
    device
)
```

---

# 105. Load

```python
model = create_model()


model.load_state_dict(
    torch.load(
        "best_model.pt",
        map_location=device
    )
)


model = model.to(
    device
)
```

이렇게 하면 Model 구조를 재사용하기 쉽다.

---

# 106. Class 형태도 가능

```python
class MyModel(
    nn.Module
):

    def __init__(
        self
    ):

        super().__init__()

        self.fc1 = nn.Linear(
            10,
            32
        )

        self.relu = nn.ReLU()

        self.fc2 = nn.Linear(
            32,
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

# 107. 저장

```python
model = MyModel()


torch.save(
    model.state_dict(),
    "model.pt"
)
```

---

# 108. Load

```python
model = MyModel()


model.load_state_dict(
    torch.load(
        "model.pt",
        map_location=device
    )
)


model = model.to(
    device
)
```

---

# 109. strict

다음과 같은 옵션을 볼 수 있다.

```python
model.load_state_dict(
    state_dict,
    strict=True
)
```

기본적으로 Key가 Model 구조와 맞는지 엄격하게 확인한다.

---

# 110. strict=False

```python
model.load_state_dict(
    state_dict,
    strict=False
)
```

를 사용하면 일부 Key가 맞지 않아도 Load를 진행할 수 있는 경우가 있다.

---

# 111. 하지만 무작정 strict=False를 사용하지 않는다

Missing Key나 Unexpected Key가 왜 발생했는지 먼저 확인하는 것이 중요하다.

Model Architecture가 잘못된 것일 수도 있기 때문이다.

---

# 112. Missing Keys

저장된 Weight에 현재 Model이 필요로 하는 Parameter가 없을 때 발생할 수 있다.

---

# 113. Unexpected Keys

저장 File에는 Parameter가 있는데 현재 Model 구조에는 해당 Parameter가 없을 수 있다.

---

# 114. Transfer Learning에서는 일부만 Load할 수도 있다

예:

```text
Pretrained Backbone Weight
↓
새로운 Classifier
```

처럼 Model 일부 구조가 달라질 수 있다.

이 경우 일부 Parameter만 재사용하는 전략이 가능하다.

---

# 115. Classifier 교체

예:

```text
Pretrained Model

1000 Classes
```

를:

```text
2 Classes
```

문제에 사용한다면 마지막 Layer를 교체한다.

---

# 116. 예

```python
model.fc = nn.Linear(
    model.fc.in_features,
    2
)
```

Model 종류에 따라 마지막 Layer의 이름과 구조는 다르다.

---

# 117. Transfer Learning 저장

Fine-tuning이 끝난 뒤:

```python
torch.save(
    model.state_dict(),
    "best_finetuned_model.pt"
)
```

처럼 저장할 수 있다.

---

# 118. Test Inference

Competition에서는 Best Model을 Load한 뒤 Test DataLoader에 Prediction을 생성한다.

---

# 119. 구조

```text
Best Model Load
↓
model.eval()
↓
Test Loader
↓
Forward
↓
Probability / Prediction
↓
CPU 이동
↓
결과 저장
```

---

# 120. 예

```python
predictions = []


model.eval()


with torch.no_grad():

    for x in test_loader:

        x = x.to(
            device
        )

        outputs = model(
            x
        )

        pred = outputs.argmax(
            dim=1
        )

        predictions.extend(
            pred.cpu().numpy()
        )
```

---

# 121. Test Dataset이 ID도 반환한다면

```python
for x, image_id in test_loader:

    ...
```

처럼 사용할 수 있다.

Prediction과 ID 순서를 맞춰야 한다.

---

# 122. Test Loader shuffle

Competition Test Prediction에서는 일반적으로:

```python
shuffle=False
```

를 사용한다.

---

# 123. 이유

Submission의 Sample 순서와 Prediction 순서를 맞추기 쉽게 하기 위해서이다.

---

# 124. Submission

예:

```python
submission[
    "label"
] = predictions
```

---

# 125. CSV 저장

```python
submission.to_csv(
    "submission.csv",
    index=False
)
```

---

# 126. Model 저장과 Competition 흐름

```text
Train
↓
Validation
↓
Best Model 저장
↓
Training 종료
↓
Best Model Load
↓
Test Prediction
↓
submission.csv
```

이다.

---

# 127. 저장하지 않고 바로 Test Prediction도 가능하다

Training 직후 Memory에 있는 Model로 Prediction할 수도 있다.

하지만 Best Epoch가 마지막 Epoch가 아닐 수 있기 때문에 Best Model을 저장하고 다시 Load하는 구조가 안전하고 명확하다.

---

# 128. Ensemble에서는 여러 Model Load

예:

```text
Fold 1 Model

Fold 2 Model

Fold 3 Model

Fold 4 Model

Fold 5 Model
```

을 각각 저장할 수 있다.

---

# 129. Test Prediction 평균

```text
Fold 1 Probability
+
Fold 2 Probability
+
...
+
Fold 5 Probability
↓
Average
```

하는 방식도 가능하다.

---

# 130. Cross Validation Model 저장

예:

```python
torch.save(
    model.state_dict(),
    f"fold_{fold}.pt"
)
```

---

# 131. 나중에 Fold Model Load

```python
for fold in range(
    5
):

    model = create_model()

    model.load_state_dict(
        torch.load(
            f"fold_{fold}.pt",
            map_location=device
        )
    )

    ...
```

---

# 132. 실제 Web Service에서는?

예:

```text
사용자
↓
Image Upload
↓
FastAPI
↓
Preprocessing
↓
PyTorch Model
↓
Prediction
↓
JSON Response
```

처럼 연결할 수 있다.

---

# 133. Server가 시작될 때 Model Load

예:

```python
model = create_model()


model.load_state_dict(
    torch.load(
        "best_model.pt",
        map_location=device
    )
)


model.to(
    device
)


model.eval()
```

---

# 134. 요청마다 Model을 Load하면?

매 요청마다:

```text
Model File Load
```

를 반복하면 비효율적이다.

일반적으로 Application 시작 시 Model을 준비하고 요청마다 Prediction에 재사용하는 구조를 고려한다.

---

# 135. API 요청

```text
POST /predict
```

↓

```text
Image
```

↓

```text
Preprocessing
```

↓

```text
Model Inference
```

↓

```json
{
  "prediction": 1,
  "probability": 0.94
}
```

같은 응답을 만들 수 있다.

---

# 136. 그래서 지금 배우는 저장은 배포와 연결된다

```text
PyTorch Training
↓
best_model.pt
↓
FastAPI
↓
Inference
↓
Web / App
```

이다.

---

# 137. Model 저장은 Training의 끝이면서 Deployment의 시작

학습 관점:

```text
Training
→ Save
```

서비스 관점:

```text
Load
→ Inference
```

이다.

---

# 138. 보안 측면

신뢰할 수 없는 출처에서 받은 Model File이나 임의의 직렬화 파일을 무작정 Load하지 않는다.

Model File도 외부 입력이라는 점을 기억한다.

---

# 139. Model Load 후 확인

```python
model.load_state_dict(
    state_dict
)


model.eval()
```

후 Dummy Input으로 Forward를 확인할 수 있다.

---

# 140. Smoke Test

```python
x = torch.randn(
    1,
    10
).to(
    device
)


with torch.no_grad():

    outputs = model(
        x
    )


print(
    outputs.shape
)
```

---

# 141. 왜 Smoke Test?

File이 Load되었다는 것과 실제 Inference Pipeline이 정상이라는 것은 다를 수 있다.

그래서:

```text
Load

Device

Input Shape

Forward

Output Shape
```

를 확인한다.

---

# 142. Model 저장 후 반드시 기억할 정보

```text
Model Class

Input Shape

Number of Classes

Label Mapping

Transform

Normalization

Threshold

PyTorch Version

Best Metric
```

등이다.

---

# 143. 흔한 오류 1

```text
FileNotFoundError
```

확인:

```python
import os


print(
    os.getcwd()
)


print(
    os.path.exists(
        model_path
    )
)
```

---

# 144. 흔한 오류 2

```text
size mismatch
```

가능한 원인:

```text
현재 Model Architecture
≠
저장 당시 Architecture
```

이다.

---

# 145. 흔한 오류 3

```text
Missing key(s)

Unexpected key(s)
```

Model 구조와 state_dict Key가 일치하는지 확인한다.

---

# 146. 흔한 오류 4

Device 관련 오류.

해결:

```python
torch.load(
    model_path,
    map_location=device
)
```

및:

```python
model.to(
    device
)
```

를 확인한다.

---

# 147. 흔한 오류 5

Load는 성공했는데 Prediction이 이상하다.

확인:

```text
model.eval() 했는가?

Training과 같은 Resize인가?

Normalization이 같은가?

RGB / Grayscale이 같은가?

Class Mapping이 같은가?

Threshold가 같은가?

Input Shape가 같은가?
```

---

# 148. 가장 중요한 원칙

Model File만 같다고 Prediction Pipeline 전체가 같은 것은 아니다.

```text
Model
+
Preprocessing
+
Label Mapping
+
Decision Rule
```

이 함께 맞아야 한다.

---

# 149. Chapter 07 전체 연결

```text
deep-learning-basic.md
↓
Deep Learning 전체 개념

tensor-basic.md
↓
Tensor

dataset-dataloader.md
↓
Data 공급

neural-network.md
↓
Model

activation-function.md
↓
Non-linearity

loss-function.md
↓
오차 계산

optimizer-backprop.md
↓
Gradient와 Parameter Update

training-loop.md
↓
반복 학습

validation-overfitting.md
↓
Generalization과 Best Model

gpu-cuda.md
↓
GPU 계산

model-save-load.md
↓
저장 / 복원 / Inference
```

---

# 150. 전체 Deep Learning Workflow

```text
Raw Data
↓
Dataset
↓
DataLoader
↓
Tensor
↓
GPU
↓
Neural Network
↓
Forward
↓
Loss
↓
Backward
↓
Optimizer
↓
Epoch 반복
↓
Validation
↓
Best Model
↓
Save
↓
Load
↓
Test Inference
↓
Deployment
```

---

# 151. 가장 중요한 저장 코드

```python
torch.save(
    model.state_dict(),
    "best_model.pt"
)
```

---

# 152. 가장 중요한 Load 코드

```python
model = create_model()


model.load_state_dict(
    torch.load(
        "best_model.pt",
        map_location=device
    )
)


model = model.to(
    device
)


model.eval()
```

---

# 153. Training을 이어가고 싶다면

단순 Model Weight가 아니라:

```text
Model State

Optimizer State

Epoch

Best Metric

Scheduler State
```

등을 Checkpoint에 저장한다.

---

# 154. 최종 정리

Model 저장:

```python
torch.save(
    model.state_dict(),
    "model.pt"
)
```

Model 불러오기:

```python
model.load_state_dict(
    torch.load(
        "model.pt",
        map_location=device
    )
)
```

Inference 전:

```python
model.eval()
```

그리고:

```python
with torch.no_grad():

    outputs = model(
        x
    )
```

Best Model:

```text
마지막 Epoch가 아니라
Validation 기준 가장 좋았던 Model
```

Checkpoint:

```text
Model Weight뿐 아니라
Training 상태까지 저장
```

이다.

전체 흐름:

```text
Train
↓
Validation
↓
Best Model Save
↓
Program 종료
↓
Model Architecture 생성
↓
state_dict Load
↓
model.eval()
↓
Preprocessing
↓
Inference
↓
Prediction
```

그리고 실제 서비스를 만들 때:

```text
PyTorch Model
↓
best_model.pt
↓
FastAPI
↓
Web / App
```

으로 이어질 수 있다.