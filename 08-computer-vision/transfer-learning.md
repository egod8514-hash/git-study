# Transfer Learning

## 1. Transfer Learning이란?

Transfer Learning은 이미 다른 Dataset에서 학습된 Model의 지식을 새로운 문제에 활용하는 방법이다.

한국어로:

```text
전이학습
```

이라고 한다.

기본 개념:

```text
대규모 Dataset
↓
Pretrained Model
↓
이미 학습된 Weight
↓
새로운 Dataset에 활용
```

---

# 2. 처음부터 학습하는 방법

CNN을 직접 만들고 Weight를 Random하게 초기화할 수 있다.

```python
model = SimpleCNN()
```

Training 시작 시 Model은 Image Pattern에 대해 학습된 지식이 거의 없다.

```text
Random Weight
↓
Training
↓
Edge 학습
↓
Texture 학습
↓
Shape 학습
↓
Classification Pattern 학습
```

처럼 처음부터 학습한다.

이를:

```text
Training From Scratch
```

라고 표현한다.

---

# 3. Transfer Learning

Transfer Learning에서는 이미 학습된 Model을 가져온다.

```text
Pretrained Model
↓
새 Dataset
↓
추가 Training
```

이다.

예:

```text
ImageNet
↓
ResNet18 Training
↓
Pretrained ResNet18
↓
Chest X-ray Dataset
↓
Normal / Pneumonia Classification
```

---

# 4. Pretrained Model

Pretrained Model은 이미 어떤 Dataset으로 Training이 완료된 Model이다.

예:

```text
ResNet

DenseNet

EfficientNet

VGG
```

등의 Architecture에 학습된 Weight를 사용할 수 있다.

---

# 5. Architecture와 Weight는 다르다

중요하다.

```text
Architecture
→ Model의 구조

Weight
→ Training을 통해 학습된 Parameter
```

이다.

---

# 6. 예

```python
from torchvision.models import (
    resnet18
)


model = resnet18(
    weights=None
)
```

이면 ResNet18 구조는 사용하지만 Pretrained Weight는 사용하지 않는다.

---

# 7. Pretrained Weight 사용

```python
from torchvision.models import (
    resnet18,
    ResNet18_Weights
)


weights = (
    ResNet18_Weights.DEFAULT
)


model = resnet18(
    weights=weights
)
```

이 경우:

```text
ResNet18 Architecture
+
Pretrained Weight
```

를 사용한다.

---

# 8. ImageNet

Computer Vision에서 자주 등장하는 Dataset이다.

ImageNet은 매우 많은 자연 Image와 Class를 포함하는 대규모 Image Dataset이다.

많은 CNN Model이 ImageNet으로 Pretraining된 Weight를 제공한다.

---

# 9. ImageNet Pretraining

개념적으로:

```text
많은 Image
↓
CNN Training
↓
일반적인 Visual Feature 학습
```

이다.

Model은 Training 과정에서 여러 Image Pattern을 학습한다.

---

# 10. 초기 Layer

CNN의 비교적 초기 Layer는:

```text
Edge

Line

Simple Texture

Brightness Change
```

같은 비교적 일반적인 Visual Pattern을 표현할 수 있다.

---

# 11. 깊은 Layer

깊은 Layer로 갈수록 앞에서 추출된 Feature를 조합해 더 복잡한 표현을 만든다.

개념적으로:

```text
Edge
↓
Texture
↓
Shape
↓
복잡한 Pattern
```

이다.

---

# 12. 왜 다른 Dataset에서도 사용할 수 있을까?

새로운 Dataset에서도:

```text
Edge

Texture

Shape

Local Pattern
```

같은 기본적인 Visual Feature가 필요할 수 있기 때문이다.

그래서 기존 Weight를 시작점으로 사용할 수 있다.

---

# 13. Transfer Learning의 장점

대표적으로:

```text
Training 시간 절약

적은 Data에서도 활용 가능

좋은 초기 Weight 사용

성능 개선 가능성

Training 안정성 향상 가능
```

등이 있다.

---

# 14. Dataset이 작을 때

Image Dataset이 작다면 큰 CNN을 처음부터 학습시키기 어려울 수 있다.

```text
Data 부족
+
큰 Model
↓
Overfitting 위험
```

이 있기 때문이다.

이런 경우 Transfer Learning이 좋은 Baseline이 될 수 있다.

---

# 15. 하지만 항상 좋은 것은 아니다

Pretraining Dataset과 새로운 Dataset의 차이가 매우 크면 Transfer Learning의 효과가 제한될 수도 있다.

---

# 16. Domain Shift

Training에 사용된 Domain과 실제 적용 Domain이 다른 현상을:

```text
Domain Shift
```

라고 한다.

---

# 17. 예

ImageNet:

```text
자동차

개

고양이

사람

건물
```

같은 자연 Image가 많다.

새로운 Dataset:

```text
Chest X-ray
```

라면 Domain이 상당히 다르다.

---

# 18. 그런데도 사용할 수 있는 이유

초기 Layer에서 학습한 일부 일반적인 Visual Feature가 의료영상에서도 도움이 될 가능성이 있기 때문이다.

하지만:

```text
ImageNet Pretrained
=
의료영상에 최적화 완료
```

라는 뜻은 아니다.

---

# 19. Backbone

CNN에서 Feature를 추출하는 주요 부분을:

```text
Backbone
```

이라고 부른다.

전체 구조:

```text
Image
↓
Backbone
↓
Feature
↓
Classifier
↓
Prediction
```

이다.

---

# 20. ResNet 예

개념적으로:

```text
ResNet18

Input
↓
Convolution
↓
Residual Blocks
↓
Feature
↓
Fully Connected Layer
↓
1000 Classes
```

이다.

ImageNet용 ResNet18의 마지막 Classifier는 원래 ImageNet Class를 예측하도록 만들어져 있다.

---

# 21. 새로운 문제

우리가:

```text
Normal

Pneumonia
```

2개 Class만 Prediction하고 싶다고 하자.

기존 마지막 Layer는 새로운 문제와 맞지 않는다.

---

# 22. 따라서 Classifier를 변경한다

기존:

```text
Feature
↓
1000 Classes
```

새로운 문제:

```text
Feature
↓
2 Classes
```

로 바꾼다.

---

# 23. PyTorch ResNet18

```python
import torch.nn as nn

from torchvision.models import (
    resnet18,
    ResNet18_Weights
)


weights = (
    ResNet18_Weights.DEFAULT
)


model = resnet18(
    weights=weights
)
```

---

# 24. 기존 Classifier 확인

```python
print(
    model.fc
)
```

예:

```text
Linear(
    in_features=512,
    out_features=1000
)
```

처럼 볼 수 있다.

---

# 25. in_features 저장

```python
num_features = (
    model.fc.in_features
)
```

---

# 26. Classifier 교체

```python
model.fc = nn.Linear(
    num_features,
    2
)
```

---

# 27. 결과

```text
ResNet Backbone
↓
512 Features
↓
새 Linear Layer
↓
2 Logits
```

이다.

---

# 28. num_classes 변수

```python
num_classes = 2


model.fc = nn.Linear(
    model.fc.in_features,
    num_classes
)
```

처럼 작성할 수 있다.

---

# 29. Model 확인

```python
print(
    model.fc
)
```

결과는 개념적으로:

```text
Linear(
    in_features=512,
    out_features=2
)
```

이다.

---

# 30. 마지막 Layer는 새로 학습된다

새롭게 만든:

```python
nn.Linear(
    512,
    2
)
```

는 새로운 Parameter를 가지고 있다.

따라서 새로운 Dataset으로 학습해야 한다.

---

# 31. Transfer Learning의 두 가지 큰 방법

대표적으로:

```text
1. Feature Extraction

2. Fine-tuning
```

으로 생각할 수 있다.

---

# 32. Feature Extraction

Pretrained Backbone을 고정하고 마지막 Classifier만 Training한다.

```text
Pretrained Backbone
→ Freeze

New Classifier
→ Train
```

이다.

---

# 33. Freeze

Parameter를 Training하지 않도록:

```python
param.requires_grad = False
```

로 설정한다.

---

# 34. 모든 Parameter Freeze

```python
for param in model.parameters():

    param.requires_grad = False
```

---

# 35. 중요한 순서

모든 Parameter를 Freeze한 후 새로운 Classifier를 만든다.

```python
for param in model.parameters():

    param.requires_grad = False


model.fc = nn.Linear(
    model.fc.in_features,
    2
)
```

---

# 36. 왜 이 순서가 편할까?

기존 Parameter:

```text
requires_grad = False
```

가 된다.

그 후 새로 생성한 `model.fc`의 Parameter는 기본적으로:

```text
requires_grad = True
```

이다.

---

# 37. 결과

```text
Backbone
Frozen

Classifier
Trainable
```

이 된다.

---

# 38. 확인

```python
for name, param in model.named_parameters():

    print(
        name,
        param.requires_grad
    )
```

---

# 39. Trainable Parameter만 확인

```python
for name, param in model.named_parameters():

    if param.requires_grad:

        print(
            name
        )
```

---

# 40. 예상

주로:

```text
fc.weight

fc.bias
```

만 출력될 수 있다.

---

# 41. Optimizer

Trainable Parameter만 Optimizer에 넣을 수 있다.

```python
optimizer = torch.optim.Adam(
    filter(
        lambda p: p.requires_grad,
        model.parameters()
    ),
    lr=0.001
)
```

---

# 42. 또는 Classifier만 명시

```python
optimizer = torch.optim.Adam(
    model.fc.parameters(),
    lr=0.001
)
```

처럼 사용할 수도 있다.

---

# 43. Loss

2-Class를 2 Logit으로 출력한다면:

```python
loss_fn = nn.CrossEntropyLoss()
```

를 사용할 수 있다.

---

# 44. Training Loop는 그대로

```python
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

이다.

---

# 45. 무엇이 다른가?

Training Loop 자체보다:

```text
어떤 Parameter가
requires_grad=True인가?
```

가 다르다.

---

# 46. Frozen Parameter

Frozen Backbone은:

```text
Gradient를 이용한 Weight Update 대상에서 제외
```

된다.

---

# 47. Trainable Parameter

Classifier는:

```text
Backward
↓
Gradient
↓
Optimizer
↓
Weight Update
```

가 이루어진다.

---

# 48. Feature Extractor로 사용하는 것

Backbone은 Image를:

```text
Image
↓
Useful Feature
```

로 바꾸는 역할을 한다.

Classifier가 그 Feature를 새로운 Class에 맞게 학습한다.

---

# 49. 예

```text
Chest X-ray
↓
Pretrained ResNet Backbone
↓
512-dimensional Feature
↓
New Classifier
↓
Normal / Pneumonia
```

이다.

---

# 50. Fine-tuning

Fine-tuning은 Pretrained Parameter 일부 또는 전체를 새로운 Dataset에 맞게 추가로 학습하는 방법이다.

---

# 51. 개념

```text
Pretrained Weight
↓
새로운 Dataset
↓
Weight를 조금 더 조정
```

이다.

---

# 52. Feature Extraction과 차이

Feature Extraction:

```text
Backbone
고정

Classifier
학습
```

Fine-tuning:

```text
Backbone 일부 또는 전체
학습

Classifier
학습
```

이다.

---

# 53. 단계적 Training

실제 Project에서는:

```text
Stage 1
Backbone Freeze
Classifier Training

↓

Stage 2
Backbone 일부 Unfreeze
Fine-tuning
```

처럼 할 수 있다.

---

# 54. 왜 먼저 Freeze할까?

새 Classifier는 Random Weight로 시작한다.

처음부터 Backbone 전체를 큰 Learning Rate로 업데이트하면 이미 학습된 좋은 Weight를 크게 변경할 위험이 있다.

---

# 55. Stage 1

```text
Pretrained Backbone
→ Freeze

New Classifier
→ Training
```

한다.

---

# 56. Stage 2

그다음 Backbone 일부를 열어:

```text
Fine-tuning
```

한다.

---

# 57. Unfreeze

```python
for param in model.parameters():

    param.requires_grad = True
```

로 전체를 다시 Trainable하게 만들 수 있다.

---

# 58. 하지만 전체를 바로 열 필요는 없다

특정 Block만 열 수도 있다.

예:

```text
초기 Layer
→ Freeze

후반 Layer
→ Train

Classifier
→ Train
```

이다.

---

# 59. ResNet 일부만 Unfreeze

예:

```python
for param in model.layer4.parameters():

    param.requires_grad = True
```

처럼 마지막 Block을 학습하도록 만들 수 있다.

---

# 60. Learning Rate

Fine-tuning에서는 일반적으로 Pretrained Backbone에 작은 Learning Rate를 사용하는 전략을 많이 고려한다.

---

# 61. 이유

이미 학습된 Weight를 너무 크게 변경하지 않기 위해서이다.

---

# 62. 예

Classifier Training:

```text
LR = 1e-3
```

Fine-tuning:

```text
LR = 1e-4
```

처럼 더 작은 값을 실험할 수 있다.

이 값은 예시이며 Dataset과 Model에 따라 조정한다.

---

# 63. Parameter Group

Backbone과 Classifier에 서로 다른 Learning Rate를 줄 수도 있다.

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

# 64. 의미

```text
Backbone
→ 아주 작은 LR

Classifier
→ 조금 더 큰 LR
```

로 학습할 수 있다.

---

# 65. 주의

`requires_grad=False`인 Parameter와 Optimizer Parameter 구성을 항상 확인한다.

---

# 66. Optimizer를 언제 만들었는지도 중요

Model의 Classifier를 교체한 뒤 Optimizer를 만드는 것이 안전하다.

---

# 67. 잘못된 흐름 예

```python
optimizer = torch.optim.Adam(
    model.parameters()
)


model.fc = nn.Linear(
    512,
    2
)
```

---

# 68. 문제

Optimizer를 만든 후 새로운 Layer를 교체하면 새 Classifier Parameter가 기존 Optimizer에 자동으로 들어간다고 가정하면 안 된다.

---

# 69. 좋은 순서

```text
Model 생성
↓
Freeze / Unfreeze 설정
↓
Classifier 교체
↓
Trainable Parameter 확인
↓
Optimizer 생성
```

이다.

---

# 70. 예

```python
weights = (
    ResNet18_Weights.DEFAULT
)


model = resnet18(
    weights=weights
)


for param in model.parameters():

    param.requires_grad = False


model.fc = nn.Linear(
    model.fc.in_features,
    2
)


optimizer = torch.optim.Adam(
    model.fc.parameters(),
    lr=0.001
)
```

---

# 71. Pretrained Model의 Input

Pretrained Model은 Training 당시 사용한 Input Preprocessing과 호환되는 입력을 사용하는 것이 중요하다.

---

# 72. Weight Transform

torchvision에서는 Weight와 연결된 Transform 정보를 사용할 수 있다.

```python
weights = (
    ResNet18_Weights.DEFAULT
)


preprocess = (
    weights.transforms()
)
```

---

# 73. 확인

```python
print(
    preprocess
)
```

---

# 74. 왜 중요할까?

Pretrained Weight는 특정 방식으로 전처리된 Image Distribution을 기반으로 학습되었다.

새로운 Image도 호환되는 방식으로 처리해야 Weight를 제대로 활용하기 쉽다.

---

# 75. ImageNet Normalize

ImageNet 기반 Model에서 자주 보는 Normalize:

```text
mean
0.485
0.456
0.406

std
0.229
0.224
0.225
```

이다.

---

# 76. 직접 Transform

예:

```python
from torchvision import transforms


val_transform = transforms.Compose(
    [
        transforms.Resize(
            (
                224,
                224
            )
        ),

        transforms.ToTensor(),

        transforms.Normalize(
            mean=[
                0.485,
                0.456,
                0.406
            ],

            std=[
                0.229,
                0.224,
                0.225
            ]
        )
    ]
)
```

---

# 77. 하지만 권장 방식

사용 중인 Weight의 공식 Transform 정보를 확인하는 습관이 좋다.

Weight나 Architecture에 따라 입력 조건이 달라질 수 있기 때문이다.

---

# 78. Grayscale Image

의료영상에서는 Grayscale Image를 자주 볼 수 있다.

예:

```text
Chest X-ray
```

---

# 79. Pretrained Model

ImageNet Pretrained Model은 일반적으로:

```text
3 Channel RGB
```

Input을 기대한다.

---

# 80. Grayscale 그대로

Input:

```text
[B, 1, 224, 224]
```

인데 Model 첫 Layer가:

```text
in_channels = 3
```

이면 Channel Error가 발생한다.

---

# 81. 간단한 해결

PIL Image를:

```python
image = Image.open(
    image_path
).convert(
    "RGB"
)
```

로 변환할 수 있다.

---

# 82. Grayscale → RGB

이 경우 원래 Grayscale 정보를 3 Channel 형태로 표현하여 Pretrained Model의 Input 구조와 맞춘다.

---

# 83. Shape

```text
Grayscale
[1, H, W]

↓

RGB 형태

[3, H, W]
```

가 된다.

---

# 84. 새로운 색 정보가 생기는 것은 아니다

Grayscale Image를 RGB로 변환한다고 해서 실제 Color 정보가 만들어지는 것은 아니다.

Pretrained Model이 기대하는 Channel 수에 맞추기 위한 방법으로 이해한다.

---

# 85. 첫 Conv Layer를 바꾸는 방법도 있다

ResNet 첫 Layer를 1 Channel용으로 수정하는 방법도 가능하다.

하지만 Pretrained Weight 활용 방식이 달라질 수 있으므로 입문 단계에서는 원리를 이해하고 신중하게 사용한다.

---

# 86. Model별 Classifier 위치

모든 Model이:

```python
model.fc
```

를 사용하는 것은 아니다.

---

# 87. ResNet

대표적으로:

```python
model.fc
```

이다.

---

# 88. DenseNet

대표적으로:

```python
model.classifier
```

를 사용한다.

---

# 89. EfficientNet

torchvision EfficientNet에서는:

```python
model.classifier
```

구조를 확인해야 한다.

---

# 90. VGG

VGG도:

```python
model.classifier
```

를 사용한다.

---

# 91. 따라서 먼저 print(model)

```python
print(
    model
)
```

로 마지막 Classifier 구조를 확인한다.

---

# 92. 무작정 model.fc라고 쓰면 안 된다

Architecture마다 구조가 다르기 때문이다.

---

# 93. ResNet18 예제 전체

```python
import torch
import torch.nn as nn

from torchvision.models import (
    resnet18,
    ResNet18_Weights
)


device = torch.device(
    "cuda"
    if torch.cuda.is_available()
    else "cpu"
)


weights = (
    ResNet18_Weights.DEFAULT
)


model = resnet18(
    weights=weights
)


for param in model.parameters():

    param.requires_grad = False


num_features = (
    model.fc.in_features
)


model.fc = nn.Linear(
    num_features,
    2
)


model = model.to(
    device
)


loss_fn = nn.CrossEntropyLoss()


optimizer = torch.optim.Adam(
    model.fc.parameters(),
    lr=0.001
)
```

---

# 94. Dummy Forward

Training 전에 확인한다.

```python
x = torch.randn(
    8,
    3,
    224,
    224
).to(
    device
)


with torch.no_grad():

    logits = model(
        x
    )


print(
    logits.shape
)
```

---

# 95. 예상

```text
torch.Size([8, 2])
```

이다.

---

# 96. Trainable Parameter 확인

```python
for name, param in model.named_parameters():

    if param.requires_grad:

        print(
            name,
            param.shape
        )
```

---

# 97. Parameter 수 확인

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

# 98. Freeze가 제대로 되었다면

```text
Total Parameters
≫
Trainable Parameters
```

일 수 있다.

Classifier만 학습하기 때문이다.

---

# 99. Training Loop

Transfer Learning이라고 해서 Training Loop가 완전히 달라지는 것은 아니다.

```python
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

# 100. 핵심 차이

```text
Training Loop
→ 거의 동일

Model 초기 Weight
→ Pretrained

Trainable Parameter
→ 선택 가능
```

이다.

---

# 101. Validation도 동일

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

이다.

---

# 102. model.train()과 Freeze는 다른 개념

중요하다.

```python
model.train()
```

은 Module을 Training Mode로 변경한다.

---

# 103. requires_grad

```python
param.requires_grad = False
```

는 Parameter의 Gradient Training 여부와 관련된다.

---

# 104. 따라서

```text
model.train()
≠
모든 Weight가 반드시 Update
```

이다.

Frozen Parameter는 `model.train()` 상태여도 Optimizer에 의해 학습되는 Parameter와 동일하게 취급되는 것이 아니다.

---

# 105. BatchNorm 주의

Pretrained CNN에는 BatchNorm Layer가 포함될 수 있다.

`model.train()`에서는 BatchNorm의 동작도 Training Mode가 된다.

따라서:

```text
Parameter Freeze
```

와:

```text
Module Train/Eval Mode
```

는 구분해서 이해해야 한다.

---

# 106. Dropout도 마찬가지

Dropout은:

```text
model.train()

model.eval()
```

에 따라 동작이 달라진다.

---

# 107. Fine-tuning에서 Validation

항상:

```python
model.eval()
```

과:

```python
with torch.no_grad():
```

를 사용한다.

---

# 108. Transfer Learning 실험 구조

Baseline을 다음처럼 구성할 수 있다.

```text
Experiment 0
Simple CNN From Scratch
```

---

# 109. Experiment 1

```text
Pretrained ResNet18

Backbone Freeze

Classifier Only Training
```

---

# 110. Experiment 2

```text
Pretrained ResNet18

Last Block Unfreeze

Small LR
```

---

# 111. Experiment 3

```text
Pretrained ResNet18

More Layers Fine-tuning
```

---

# 112. 비교

```text
Experiment
Model
Freeze
LR
Val Loss
Accuracy
F1
ROC-AUC
```

등을 기록한다.

---

# 113. 한 번에 여러 가지를 바꾸지 않는다

예:

```text
Model 변경

Augmentation 변경

Loss 변경

LR 변경

Image Size 변경
```

을 동시에 하면 어떤 변경이 효과가 있었는지 해석하기 어렵다.

---

# 114. 좋은 비교

```text
Baseline
ResNet18 Freeze

↓

Experiment
같은 조건
+
layer4 Unfreeze
```

처럼 비교한다.

---

# 115. Transfer Learning과 Augmentation

둘은 다른 개념이다.

Transfer Learning:

```text
Model의 시작 Weight를 활용
```

Augmentation:

```text
Training Input을 다양하게 변형
```

이다.

---

# 116. 함께 사용할 수 있다

```text
Pretrained Model
+
Data Augmentation
```

을 함께 사용할 수 있다.

---

# 117. Weighted Loss도 함께 사용할 수 있다

Class Imbalance가 있다면:

```text
Pretrained Model

+

Augmentation

+

Weighted Loss
```

같은 조합도 가능하다.

하지만 각각의 효과를 먼저 확인하는 것이 좋다.

---

# 118. Transfer Learning과 Overfitting

Transfer Learning도 Overfitting이 발생할 수 있다.

특히 Dataset이 매우 작으면:

```text
Train ↑

Validation ↓
```

문제가 나타날 수 있다.

---

# 119. 대응

```text
Freeze 범위 조정

Learning Rate 조정

Data Augmentation

Weight Decay

Dropout

Early Stopping
```

등을 고려한다.

---

# 120. 너무 많이 Freeze한 경우

새로운 Domain에 Model이 충분히 적응하지 못할 수 있다.

즉:

```text
Underfitting
```

가능성도 있다.

---

# 121. 너무 많이 Unfreeze한 경우

작은 Dataset에서 많은 Parameter를 학습하면:

```text
Overfitting
```

위험이 커질 수 있다.

---

# 122. 그래서 실험한다

정답:

```text
무조건 Freeze

또는

무조건 전체 Fine-tuning
```

이 아니다.

Validation 결과를 비교한다.

---

# 123. Dataset 크기

일반적으로 Dataset이 작을수록:

```text
Pretrained Feature 활용
```

의 가치가 커질 수 있다.

하지만 Task와 Domain에 따라 달라진다.

---

# 124. Domain 차이

새로운 Dataset이 Pretraining Dataset과 많이 다르다면 후반 Layer를 Fine-tuning하는 것이 도움이 되는지 실험할 수 있다.

---

# 125. 의료영상

자연 Image Pretraining과 의료 Image는 Domain이 다르다.

따라서:

```text
Pretrained Weight 사용
↓
Validation
↓
Fine-tuning 범위 조정
↓
다시 Validation
```

하는 식으로 검증한다.

---

# 126. Model 선택

대표적으로 비교할 수 있는 Model:

```text
ResNet18

ResNet50

DenseNet121

EfficientNet
```

등이 있다.

---

# 127. 큰 Model이 항상 좋은 것은 아니다

큰 Model은:

```text
Parameter 증가

GPU Memory 증가

Training 시간 증가

Overfitting 가능성 증가
```

등의 비용이 있다.

---

# 128. 작은 Model의 장점

예:

```text
ResNet18
```

은 비교적 가볍기 때문에 Baseline이나 실험용으로 활용하기 좋다.

---

# 129. GPU Memory

Model이 커지면:

```text
CUDA out of memory
```

문제가 발생할 수 있다.

---

# 130. 해결 방향

```text
Batch Size 감소

Image Size 조정

더 작은 Model 사용

Mixed Precision 고려
```

등이 있다.

---

# 131. Transfer Learning과 Model Save

학습 후:

```python
torch.save(
    model.state_dict(),
    "best_resnet18.pt"
)
```

처럼 Weight를 저장할 수 있다.

---

# 132. Load

동일한 Architecture를 다시 만든다.

```python
model = resnet18(
    weights=None
)


model.fc = nn.Linear(
    model.fc.in_features,
    2
)
```

---

# 133. State Dict Load

```python
model.load_state_dict(
    torch.load(
        "best_resnet18.pt",
        map_location=device
    )
)
```

---

# 134. 중요한 점

저장된 `state_dict`를 불러올 때 Model Architecture가 저장 당시와 맞아야 한다.

---

# 135. Inference

```python
model = model.to(
    device
)


model.eval()
```

---

# 136. Probability

```python
with torch.no_grad():

    logits = model(
        images
    )


    probs = torch.softmax(
        logits,
        dim=1
    )
```

---

# 137. Prediction

```python
preds = probs.argmax(
    dim=1
)
```

---

# 138. Preprocessing도 같이 저장해야 한다

Model Weight만 기억하면 부족하다.

다음도 기록한다.

```text
Image Size

Channel

Normalize

Class Mapping

Model Architecture

Number of Classes

Threshold

Training Transform

Validation Transform
```

---

# 139. Class Mapping

예:

```text
0 = Normal

1 = Pneumonia
```

를 기록해야 한다.

나중에 Mapping을 반대로 사용하면 Prediction 해석이 완전히 달라진다.

---

# 140. Transfer Learning Debugging 1

Classifier가 제대로 바뀌었는지 확인한다.

```python
print(
    model.fc
)
```

---

# 141. Debugging 2

Trainable Parameter:

```python
for name, param in model.named_parameters():

    if param.requires_grad:

        print(
            name
        )
```

---

# 142. Debugging 3

Input:

```python
print(
    images.shape
)
```

예:

```text
[B, 3, 224, 224]
```

---

# 143. Debugging 4

Output:

```python
print(
    logits.shape
)
```

예:

```text
[B, 2]
```

---

# 144. Debugging 5

Device:

```python
print(
    next(
        model.parameters()
    ).device
)


print(
    images.device
)
```

---

# 145. Debugging 6

Dummy Forward:

```python
x = torch.randn(
    2,
    3,
    224,
    224
).to(
    device
)


with torch.no_grad():

    y = model(
        x
    )


print(
    y.shape
)
```

---

# 146. Download 문제

처음 Pretrained Weight를 사용하면 torchvision이 Weight File을 다운로드할 수 있다.

인터넷 연결이 없거나 Cache에 Weight가 없으면 Download 관련 오류가 발생할 수 있다.

---

# 147. 이미 Weight가 있다면

환경에 따라 Cache에 저장된 Weight를 재사용할 수 있다.

---

# 148. weights=None

```python
model = resnet18(
    weights=None
)
```

은 Pretrained Weight를 다운로드하지 않는다.

하지만 이 경우 Transfer Learning의 Pretrained Weight를 사용하는 것이 아니다.

---

# 149. DEFAULT

```python
ResNet18_Weights.DEFAULT
```

은 torchvision에서 제공하는 기본 권장 Weight를 선택하는 방식이다.

---

# 150. 예전 코드

과거 torchvision 예제에서:

```python
pretrained=True
```

를 볼 수도 있다.

---

# 151. 현재 스타일

현대 torchvision에서는:

```python
weights=ResNet18_Weights.DEFAULT
```

형태를 사용하는 것이 더 명확하다.

---

# 152. Transfer Learning 전체 흐름

```text
Pretrained Architecture 선택
↓
Pretrained Weight Load
↓
Input Transform 확인
↓
Classifier 구조 확인
↓
새 Classifier로 교체
↓
Freeze 범위 설정
↓
Trainable Parameter 확인
↓
Optimizer 생성
↓
Training
↓
Validation
↓
Best Model 저장
↓
필요하면 Unfreeze
↓
Fine-tuning
↓
다시 Validation
```

---

# 153. From Scratch와 비교

From Scratch:

```text
Random Weight
↓
모든 Feature를 처음부터 학습
```

Transfer Learning:

```text
Pretrained Weight
↓
기존 Feature를 활용
↓
새로운 Task에 적응
```

이다.

---

# 154. Feature Extraction과 Fine-tuning

```text
Feature Extraction

Backbone
Freeze

Classifier
Train
```

그리고:

```text
Fine-tuning

Backbone 일부/전체
Train

Classifier
Train
```

이다.

---

# 155. 핵심 용어

```text
Pretrained Model
→ 이미 학습된 Model

Pretrained Weight
→ 이미 학습된 Parameter

Transfer Learning
→ 기존 학습 지식을 새로운 Task에 활용

Backbone
→ 주요 Feature Extractor

Classifier
→ Feature를 최종 Class로 변환

Freeze
→ Parameter Update를 막음

Unfreeze
→ Parameter를 다시 Trainable하게 만듦

Fine-tuning
→ Pretrained Weight를 새 Dataset에 맞게 추가 학습

Domain Shift
→ Training Domain과 적용 Domain의 차이
```

---

# 156. 가장 중요한 코드

Pretrained Model:

```python
weights = (
    ResNet18_Weights.DEFAULT
)


model = resnet18(
    weights=weights
)
```

---

# 157. Freeze

```python
for param in model.parameters():

    param.requires_grad = False
```

---

# 158. Classifier 교체

```python
model.fc = nn.Linear(
    model.fc.in_features,
    2
)
```

---

# 159. Optimizer

```python
optimizer = torch.optim.Adam(
    model.fc.parameters(),
    lr=0.001
)
```

---

# 160. Training

```text
Image
↓
Pretrained Backbone
↓
Feature
↓
New Classifier
↓
Logits
↓
Loss
↓
Backward
↓
Classifier Weight Update
```

이다.

---

# 161. Transfer Learning에서 가장 중요한 질문

```text
어떤 Pretrained Model을 사용할 것인가?

어떤 Weight를 사용할 것인가?

Input Transform은 맞는가?

Classifier를 제대로 교체했는가?

몇 개 Class를 출력해야 하는가?

어디까지 Freeze할 것인가?

어디부터 Fine-tuning할 것인가?

Learning Rate는 얼마인가?

Validation 성능이 실제로 개선되는가?
```

이다.

---

# 162. 의료영상에서의 질문

추가로:

```text
ImageNet Pretraining이 실제로 도움이 되는가?

Grayscale Image를 어떻게 처리할 것인가?

Patient-level Split이 되어 있는가?

Domain Shift가 큰가?

병원이나 장비 정보에 의존하지 않는가?

Failure Case는 어떤가?
```

를 확인한다.

---

# 163. 한 줄 정리

Transfer Learning은:

```text
이미 학습된 Model의 Weight를 시작점으로 사용하여
새로운 Image Classification 문제를 더 효율적으로 학습하는 방법
```

이다.

---

# 164. 다음 단계

다음 파일:

```text
fine-tuning.md
```

에서는 Transfer Learning에서 한 단계 더 들어가:

```text
Freeze

Unfreeze

Stage 1

Stage 2

Learning Rate

Parameter Group

Optimizer 재생성

Best Epoch

Early Stopping

Fine-tuning 범위
```

를 자세히 공부한다.

특히:

```text
처음에는 Backbone을 Freeze하고

↓

Classifier를 먼저 학습하고

↓

일부 Layer를 Unfreeze하고

↓

낮은 Learning Rate로 다시 학습
```

하는 단계적 Fine-tuning을 코드와 함께 정리한다.