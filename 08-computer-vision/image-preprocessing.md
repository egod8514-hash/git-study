# Image Preprocessing

## 1. Image Preprocessing이란?

Image를 Deep Learning Model에 입력하기 전에 필요한 형태로 변환하는 과정을 말한다.

전체 흐름:

```text
Raw Image
↓
Resize
↓
Channel 처리
↓
Tensor 변환
↓
Normalize
↓
Model Input
```

예:

```text
PNG Image
640 × 480
Grayscale
↓
Resize
224 × 224
↓
RGB 변환
↓
Tensor
[3, 224, 224]
↓
Normalize
↓
CNN
```

---

# 2. 왜 Preprocessing이 필요한가?

원본 Image들은 서로 다를 수 있다.

예:

```text
Image Size가 다름

Grayscale / RGB가 다름

Pixel 값 범위가 다름

File Format이 다름

방향이 다름

Aspect Ratio가 다름
```

Model은 일정한 형식의 Input을 기대하는 경우가 많다.

따라서 Input을 일관된 형태로 준비해야 한다.

---

# 3. 대표적인 Image Preprocessing

주로 다음을 볼 수 있다.

```text
Resize

Crop

Padding

Channel 변환

ToTensor

Normalize
```

---

# 4. torchvision.transforms

PyTorch에서는 Image Transform을 만들 때 `torchvision.transforms`를 자주 사용한다.

```python
from torchvision import transforms
```

---

# 5. Compose

여러 Transform을 순서대로 적용할 때:

```python
transforms.Compose(
    [...]
)
```

를 사용한다.

---

# 6. 기본 Transform

```python
transform = transforms.Compose(
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

# 7. 적용 순서

위 코드는:

```text
Image
↓
Resize
↓
ToTensor
```

순서로 실행된다.

Transform에서는 **순서도 중요하다.**

---

# 8. Resize

Image Size를 변경한다.

```python
transforms.Resize(
    (
        224,
        224
    )
)
```

---

# 9. 결과

원본:

```text
640 × 480
```

↓

```text
224 × 224
```

형태로 변환할 수 있다.

---

# 10. 왜 Resize를 사용할까?

DataLoader가 여러 Image를 하나의 Batch Tensor로 묶으려면 일반적으로 Image Shape을 맞춰야 한다.

예:

```text
Image A
[3, 640, 480]

Image B
[3, 300, 300]
```

를 그대로 하나의 일반적인 Batch Tensor로 묶기 어렵다.

Resize 후:

```text
Image A
[3, 224, 224]

Image B
[3, 224, 224]
```

처럼 맞출 수 있다.

---

# 11. Resize의 문제점

원본 Aspect Ratio가:

```text
640 : 320
=
2 : 1
```

인데 이를:

```text
224 : 224
=
1 : 1
```

로 강제로 바꾸면 Image가 찌그러질 수 있다.

---

# 12. Aspect Ratio

```text
Width : Height
```

의 비율이다.

Image 구조를 보존해야 하는 문제에서는 중요할 수 있다.

---

# 13. Resize가 항상 나쁜 것은 아니다

Task와 Dataset에 따라 단순 Resize도 충분히 사용할 수 있다.

중요한 것은:

```text
변형이 실제 Prediction에 필요한 정보를
심각하게 훼손하는가?
```

를 확인하는 것이다.

---

# 14. Resize 후 직접 확인

Transform을 만든 뒤 Sample Image를 직접 시각화한다.

```python
image = Image.open(
    "sample.png"
)

x = transform(
    image
)

print(
    x.shape
)
```

---

# 15. Resize 한 변만 지정

다음 코드도 가능하다.

```python
transforms.Resize(
    256
)
```

이 방식은 Image의 크기와 비율에 따라 동작이 달라질 수 있으므로 `(224, 224)`와 동일하다고 생각하면 안 된다.

---

# 16. Crop

Image의 일부 영역을 잘라내는 작업이다.

대표적으로:

```text
CenterCrop

RandomCrop

RandomResizedCrop
```

등이 있다.

---

# 17. CenterCrop

Image 중앙 영역을 자른다.

```python
transforms.CenterCrop(
    224
)
```

---

# 18. 예

```text
256 × 256
↓
중앙 영역 선택
↓
224 × 224
```

이다.

---

# 19. CenterCrop 장점

Aspect Ratio를 강제로 찌그러뜨리는 대신 특정 영역을 잘라 일정한 Size로 만들 수 있다.

---

# 20. Crop의 단점

중요한 Object나 병변이 중앙에 없으면 필요한 정보가 잘릴 수 있다.

특히 의료영상에서는 주의해야 한다.

---

# 21. RandomCrop

Training Image에서 Random한 위치를 Crop한다.

```python
transforms.RandomCrop(
    224
)
```

---

# 22. RandomCrop은 Augmentation 역할도 할 수 있다

매번 다른 위치가 선택될 수 있기 때문이다.

따라서:

```text
Preprocessing
+
Data Augmentation
```

의 성격을 동시에 가질 수 있다.

---

# 23. RandomResizedCrop

자주 볼 수 있는 Transform:

```python
transforms.RandomResizedCrop(
    224
)
```

이다.

Image의 일부 영역을 Random하게 선택한 후 지정된 크기로 Resize한다.

---

# 24. 주의

RandomResizedCrop을 너무 강하게 설정하면 중요한 영역이 사라질 수 있다.

특히:

```text
작은 병변

특정 장기

미세한 이상 소견
```

이 중요한 문제에서는 실제 Image를 확인해야 한다.

---

# 25. Padding

Image 주변에 Pixel을 추가하는 방식이다.

개념:

```text
원본 Image

↓

┌─────────────────┐
│     Padding     │
│   ┌─────────┐   │
│   │  Image  │   │
│   └─────────┘   │
│                 │
└─────────────────┘
```

---

# 26. Padding을 사용하는 이유

Aspect Ratio를 유지하면서 특정 Size로 맞추는 데 사용할 수 있다.

예:

```text
640 × 320
↓
Resize
224 × 112
↓
Padding
224 × 224
```

같은 방식이다.

---

# 27. Padding도 정보가 될 수 있다

주의해야 한다.

Class마다 Padding 형태가 다르다면 Model이 Padding을 Shortcut Feature로 사용할 가능성이 있다.

---

# 28. Channel 처리

Image Mode를 명확하게 맞추는 것도 중요하다.

```python
image = Image.open(
    image_path
).convert(
    "RGB"
)
```

---

# 29. 왜 convert("RGB")를 사용할까?

Dataset에:

```text
L

RGB

RGBA
```

Image가 섞여 있으면 Channel 수가 달라질 수 있다.

Model이 RGB Input을 기대한다면 모두 3 Channel로 맞출 수 있다.

---

# 30. Grayscale Model

직접 만든 CNN이:

```python
nn.Conv2d(
    in_channels=1,
    ...
)
```

이라면 Grayscale 1 Channel을 그대로 사용할 수도 있다.

---

# 31. Pretrained Model

ImageNet Pretrained Model은 일반적으로 RGB 3 Channel Input을 기준으로 사용되는 경우가 많다.

따라서 Grayscale Image를:

```python
.convert(
    "RGB"
)
```

로 바꾸어 사용할 수 있다.

---

# 32. 중요한 점

```text
Grayscale → RGB
```

변환은 새로운 Color 정보를 생성하는 것이 아니다.

Model이 기대하는 Channel 구조에 맞추는 것이다.

---

# 33. ToTensor

```python
transforms.ToTensor()
```

는 Image를 PyTorch Tensor로 변환한다.

---

# 34. Shape 변화

일반적인 RGB Image:

```text
PIL / NumPy

[H, W, C]
```

↓

```text
PyTorch Tensor

[C, H, W]
```

이다.

---

# 35. Pixel Scale

일반적인 uint8 PIL Image에 `ToTensor()`를 적용하면:

```text
0 ~ 255
```

↓

```text
0.0 ~ 1.0
```

float Tensor로 변환된다.

---

# 36. 확인

```python
x = transforms.ToTensor()(
    image
)


print(
    x.shape
)

print(
    x.dtype
)

print(
    x.min()
)

print(
    x.max()
)
```

---

# 37. Normalize

Tensor의 값을 일정한 기준으로 변환한다.

```python
transforms.Normalize(
    mean,
    std
)
```

---

# 38. 공식

Channel별로:

```text
normalized
=
(x - mean) / std
```

를 적용한다.

---

# 39. RGB Normalize

RGB는 Channel이 3개이므로 각각의 mean과 std를 지정할 수 있다.

예:

```python
transforms.Normalize(
    mean=[
        0.5,
        0.5,
        0.5
    ],

    std=[
        0.5,
        0.5,
        0.5
    ]
)
```

---

# 40. 계산 예

Pixel 값이:

```text
x = 0.8
```

mean:

```text
0.5
```

std:

```text
0.5
```

이면:

```text
(0.8 - 0.5) / 0.5
=
0.6
```

이다.

---

# 41. Normalize 후 값

Normalize 후에는:

```text
0 ~ 1
```

범위가 아닐 수 있다.

음수도 정상적으로 나올 수 있다.

---

# 42. ToTensor와 Normalize 순서

일반적으로:

```text
Image
↓
ToTensor
↓
Normalize
```

순서를 사용한다.

Normalize는 Tensor 값을 기준으로 동작하기 때문이다.

---

# 43. 기본 Transform 예

```python
transform = transforms.Compose(
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
                0.5,
                0.5,
                0.5
            ],

            std=[
                0.5,
                0.5,
                0.5
            ]
        )
    ]
)
```

---

# 44. Normalize 값은 아무거나 사용하면 되는가?

아니다.

특히 Pretrained Model을 사용하는 경우 해당 Pretrained Weight에 맞는 Preprocessing을 사용하는 것이 중요하다.

---

# 45. Pretrained Model과 Preprocessing

예:

```text
Pretrained Weight
↓
Training 당시 사용한 Input 규칙
↓
Resize / Crop / Normalize
```

가 연결되어 있다.

---

# 46. torchvision Weight Transform

최근 torchvision Model Weight는 해당 Weight에 맞는 Transform 정보를 제공할 수 있다.

예:

```python
from torchvision.models import (
    ResNet18_Weights
)


weights = (
    ResNet18_Weights.DEFAULT
)


preprocess = (
    weights.transforms()
)
```

---

# 47. 의미

```text
이 Pretrained Weight를 사용할 때
권장되는 Image Preprocessing
```

을 사용할 수 있다.

---

# 48. Model 생성

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

---

# 49. Transform

```python
preprocess = (
    weights.transforms()
)
```

---

# 50. 적용

```python
image = Image.open(
    "sample.png"
).convert(
    "RGB"
)


x = preprocess(
    image
)
```

---

# 51. 확인

```python
print(
    x.shape
)
```

일반적으로 Model이 기대하는 Image Tensor 형태가 만들어진다.

---

# 52. 왜 이 방법이 좋은가?

Model은 Pretraining 때 특정 Input 분포를 기준으로 Weight를 학습했다.

따라서 Pretrained Weight를 사용할 때 그 Weight와 호환되는 Preprocessing을 사용하는 것이 안전하다.

---

# 53. 직접 mean/std를 외울 필요는 없다

가능하면 사용하는 Weight의 공식 Transform 정보를 활용하거나 공식 문서를 확인한다.

---

# 54. Training과 Validation Transform

Training과 Validation은 Transform이 완전히 같지 않을 수 있다.

---

# 55. Training

Training에서는 Random Augmentation을 추가할 수 있다.

예:

```text
Resize

RandomRotation

RandomHorizontalFlip

ToTensor

Normalize
```

---

# 56. Validation

Validation에서는 일반적으로 Random 변형을 제외한다.

```text
Resize

ToTensor

Normalize
```

---

# 57. 왜?

Validation의 목적은 현재 Model을 일정한 조건에서 평가하는 것이다.

평가할 때마다 Image가 Random하게 달라지면 결과 해석이 복잡해질 수 있다.

---

# 58. 예

```python
train_transform = transforms.Compose(
    [
        transforms.Resize(
            (
                224,
                224
            )
        ),

        transforms.RandomHorizontalFlip(),

        transforms.ToTensor(),

        transforms.Normalize(
            mean=[
                0.5,
                0.5,
                0.5
            ],

            std=[
                0.5,
                0.5,
                0.5
            ]
        )
    ]
)
```

---

# 59. Validation Transform

```python
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
                0.5,
                0.5,
                0.5
            ],

            std=[
                0.5,
                0.5,
                0.5
            ]
        )
    ]
)
```

---

# 60. Test Transform

일반적인 Test Inference에서도 Validation과 동일한 결정적 Transform을 사용하는 경우가 많다.

```text
Validation Transform
≈
Test Transform
```

---

# 61. Train과 Validation에서 반드시 맞아야 할 것

Random Augmentation은 달라도 되지만 기본적인 Model Input 규칙은 일관되어야 한다.

예:

```text
Channel

Input Size

Normalize 기준
```

등이다.

---

# 62. 잘못된 예

Training:

```text
RGB

224 × 224

Normalize A
```

Validation:

```text
Grayscale

512 × 512

Normalize B
```

처럼 Model Input 조건이 의도 없이 달라지면 문제가 생길 수 있다.

---

# 63. Data Augmentation과 Preprocessing 차이

Preprocessing:

```text
Model이 사용할 수 있도록
Input을 준비
```

예:

```text
Resize

ToTensor

Normalize
```

Augmentation:

```text
Training Data에 변형을 주어
다양성을 증가
```

예:

```text
Flip

Rotation

Brightness 변화
```

이다.

---

# 64. 둘이 완전히 분리되는 것은 아니다

예:

```text
RandomResizedCrop
```

은:

```text
Size 맞추기
+
Random 변형
```

두 역할을 동시에 할 수 있다.

---

# 65. 순서도 중요하다

예:

```text
Resize
↓
RandomRotation
↓
ToTensor
↓
Normalize
```

처럼 Transform이 순서대로 실행된다.

---

# 66. Normalize 후 PIL Transform?

Transform 종류에 따라 입력으로 기대하는 Data Type이 다르다.

따라서:

```text
PIL Image 단계에서 수행할 Transform

Tensor 단계에서 수행할 Transform
```

을 구분해야 한다.

---

# 67. Transform Debugging

Transform을 만든 뒤 바로 전체 Training을 시작하지 않는다.

Sample 하나부터 확인한다.

```python
image = Image.open(
    "sample.png"
).convert(
    "RGB"
)


x = train_transform(
    image
)


print(
    x.shape
)

print(
    x.dtype
)

print(
    x.min()
)

print(
    x.max()
)
```

---

# 68. Dataset에서도 확인

```python
image, label = train_dataset[
    0
]


print(
    image.shape
)

print(
    label
)
```

---

# 69. DataLoader에서도 확인

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

# 70. Debugging 흐름

```text
Raw Image
↓
Transform
↓
Dataset Sample
↓
DataLoader Batch
↓
Model Forward
```

순서로 확인한다.

---

# 71. Normalize된 Image 시각화

Normalize된 Tensor를 그대로 `imshow()` 하면 색상이나 밝기가 이상하게 보일 수 있다.

시각화를 위해 Normalize를 되돌리는 과정이 필요할 수 있다.

---

# 72. Denormalize 개념

Normalize:

```text
z
=
(x - mean) / std
```

였다면 원래 Scale로 되돌릴 때:

```text
x
=
z × std + mean
```

을 사용할 수 있다.

---

# 73. 예

```python
mean = torch.tensor(
    [
        0.5,
        0.5,
        0.5
    ]
).view(
    3,
    1,
    1
)


std = torch.tensor(
    [
        0.5,
        0.5,
        0.5
    ]
).view(
    3,
    1,
    1
)


display_image = (
    x * std + mean
)
```

---

# 74. Matplotlib용

```python
display_image = (
    display_image
    .permute(
        1,
        2,
        0
    )
)
```

---

# 75. Pixel 범위 제한

시각화 시 필요하면:

```python
display_image = (
    display_image
    .clamp(
        0,
        1
    )
)
```

처럼 사용할 수 있다.

---

# 76. 왜 원본 Image도 같이 봐야 할까?

Transform 결과만 보면 원래 Data의 문제를 놓칠 수 있다.

따라서:

```text
Raw Image

Transform Image
```

둘 다 확인하는 것이 좋다.

---

# 77. Grayscale 의료 Image

Chest X-ray 같은 Grayscale Image를 다룰 때 선택지가 있다.

```text
1 Channel 그대로 사용

또는

RGB 3 Channel로 변환
```

이다.

---

# 78. 직접 CNN을 만든다면

예:

```python
nn.Conv2d(
    in_channels=1,
    out_channels=32,
    kernel_size=3
)
```

처럼 1 Channel Model을 만들 수 있다.

---

# 79. Pretrained RGB Model이라면

```python
image = Image.open(
    image_path
).convert(
    "RGB"
)
```

를 사용할 수 있다.

---

# 80. 어느 것이 무조건 더 좋은가?

아니다.

```text
Model

Pretrained Weight

Dataset Size

Task
```

에 따라 달라진다.

실험과 Validation을 통해 판단한다.

---

# 81. Image Orientation

Image 방향도 중요하다.

예:

```text
정방향

90도 회전

좌우 반전

상하 반전
```

---

# 82. 자동 Rotation은 주의

모든 Image 문제에서:

```text
RandomRotation
```

이 의미 있는 것은 아니다.

방향 자체가 중요한 정보일 수 있기 때문이다.

---

# 83. Horizontal Flip도 마찬가지

일반 사진에서는 자연스러울 수 있지만 의료 Image에서는 좌우 정보가 중요할 수 있다.

따라서 Domain Knowledge를 고려해야 한다.

---

# 84. Vertical Flip

의료영상에서 사람을 위아래로 뒤집는 변형은 실제 임상적인 Image 분포와 크게 다를 수 있다.

무조건 적용하지 않는다.

---

# 85. Brightness 변형

촬영 조건 변화에 대한 강건성을 높이는 목적으로 사용할 수 있지만 너무 강하면 실제 병변 Contrast를 훼손할 수 있다.

---

# 86. Contrast 변형

마찬가지로:

```text
병변

조직 경계

밝기 Pattern
```

을 바꿀 수 있으므로 실제 Sample을 확인해야 한다.

---

# 87. Medical Image Preprocessing

의료영상에서는 특히:

```text
이 변형이 의학적 의미를 보존하는가?
```

를 질문해야 한다.

---

# 88. 일반 Image와 의료 Image

일반 Image Classification에서 효과적인 Augmentation이 의료영상에서도 반드시 적절한 것은 아니다.

---

# 89. Train / Validation Split이 먼저

권장 흐름:

```text
Original Data
↓
Train / Validation Split
↓
Train Dataset
→ Train Transform

Validation Dataset
→ Validation Transform
```

이다.

---

# 90. 왜?

Augmentation된 유사 Image가 Train과 Validation에 나뉘어 들어가는 Leakage를 피하기 위해서이다.

---

# 91. Patient-level Split

의료영상에서는 같은 Patient의 Image가 여러 장 존재할 수 있다.

이 경우:

```text
Patient A
→ Train

Patient B
→ Validation
```

처럼 Patient 단위 분리를 고려한다.

---

# 92. 잘못된 구조

```text
Patient A Image 1
→ Train

Patient A Image 2
→ Validation
```

이면 Model이 같은 Patient 특성을 간접적으로 본 상태가 될 수 있다.

---

# 93. Preprocessing Leakage

Tabular Data에서 Scaler를 전체 Data에 `fit`하면 Leakage가 생길 수 있었던 것처럼 Image에서도 전체 Dataset 통계를 이용하는 Preprocessing은 주의해야 한다.

---

# 94. Dataset Mean / Std를 직접 계산한다면

Training Dataset을 기준으로 계산하는 것이 원칙적으로 더 적절하다.

Validation/Test 정보를 Training Preprocessing 결정에 섞지 않는다.

---

# 95. Pretrained Weight의 Mean/Std

Pretrained Weight가 제공하는 기존 Normalize 기준을 사용하는 경우는 해당 Pretraining 규칙을 재사용하는 것이다.

직접 Dataset 전체에서 Mean/Std를 계산하는 것과는 상황이 다르다.

---

# 96. Test Image도 동일한 입력 규칙

Model이 Training에서:

```text
RGB

224 × 224

특정 Normalize
```

를 사용했다면 Test에서도 동일한 기본 Input 규칙을 적용한다.

---

# 97. 실제 서비스에서도 같다

```text
사용자 Image Upload
↓
Training과 호환되는 Preprocessing
↓
Model
↓
Prediction
```

이다.

---

# 98. 따라서 Transform도 Model의 일부처럼 생각할 수 있다

Model Weight만 저장하고:

```text
Resize

Channel

Normalize
```

정보를 잃어버리면 나중에 Prediction 결과가 달라질 수 있다.

---

# 99. 기록해야 할 것

```text
Input Size

Image Mode

Channel

Resize 방식

Crop 방식

Normalize Mean

Normalize Std

Augmentation
```

등이다.

---

# 100. Preprocessing과 Model Save

예:

```text
best_model.pt

config.json

README.md
```

등을 이용해 Model과 Preprocessing 설정을 함께 기록할 수 있다.

---

# 101. torchvision의 최신 Weight 방식

다음처럼 명시적인 Weight 객체를 사용하는 코드를 자주 볼 수 있다.

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

---

# 102. 과거 코드

예전 예제에서는:

```python
resnet18(
    pretrained=True
)
```

형태를 볼 수 있다.

최근 torchvision에서는 Weight 객체를 명시하는 방식이 더 일반적이다.

---

# 103. Pretrained Transform 확인

```python
preprocess = (
    weights.transforms()
)


print(
    preprocess
)
```

를 통해 어떤 Transform이 포함되어 있는지 확인할 수 있다.

---

# 104. 직접 Training Augmentation을 추가하려면?

Pretrained Model을 사용한다고 해서 `weights.transforms()`만 그대로 사용해야 하는 것은 아니다.

Training에서는 Model의 기본 입력 조건을 고려하면서 적절한 Augmentation Pipeline을 별도로 설계할 수 있다.

---

# 105. 중요한 원칙

```text
Pretrained Model이 기대하는 기본 입력 분포
```

를 무시하지 않으면서:

```text
Task에 맞는 Training Augmentation
```

을 설계한다.

---

# 106. Baseline

처음에는 단순하게 시작한다.

```python
train_transform = transforms.Compose(
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
                0.5,
                0.5,
                0.5
            ],

            std=[
                0.5,
                0.5,
                0.5
            ]
        )
    ]
)
```

---

# 107. 그 다음 실험

Baseline 이후:

```text
Rotation 추가

Brightness 추가

Crop 변경

Image Size 변경
```

등을 하나씩 비교한다.

---

# 108. 한 번에 전부 바꾸지 않는다

예:

```text
Baseline

Experiment A
→ Rotation만 추가

Experiment B
→ Brightness만 추가

Experiment C
→ Resize 방식 변경
```

처럼 비교하면 무엇이 영향을 주었는지 해석하기 쉽다.

---

# 109. Image Size 실험

예:

```text
224 × 224

320 × 320

384 × 384
```

를 비교할 수 있다.

---

# 110. 큰 Image의 장점

미세한 Pattern이 더 많이 보존될 가능성이 있다.

---

# 111. 큰 Image의 단점

```text
GPU Memory 증가

Training 시간 증가

Batch Size 감소 가능
```

등이다.

---

# 112. 작은 Image의 장점

```text
빠른 Training

적은 Memory
```

이다.

---

# 113. 작은 Image의 단점

작은 병변이나 미세한 Pattern이 손실될 수 있다.

---

# 114. 그래서 Image Size도 Hyperparameter다

무조건:

```text
224 × 224
```

가 정답은 아니다.

---

# 115. Baseline에서는 224가 자주 등장하는 이유

많은 대표적인 Image Classification Model과 Pretrained Pipeline에서 익숙한 크기이고 계산 비용도 비교적 관리하기 쉽기 때문이다.

하지만 사용하는 Weight의 실제 Transform 요구사항을 확인한다.

---

# 116. Custom Dataset 연결

```python
class ImageDataset(
    Dataset
):

    def __init__(
        self,
        df,
        image_dir,
        transform=None
    ):

        self.df = df
        self.image_dir = image_dir
        self.transform = transform


    def __len__(
        self
    ):

        return len(
            self.df
        )


    def __getitem__(
        self,
        idx
    ):

        image_path = ...

        image = Image.open(
            image_path
        ).convert(
            "RGB"
        )

        label = ...


        if self.transform:

            image = self.transform(
                image
            )


        return (
            image,
            label
        )
```

---

# 117. Transform은 Dataset 안에서 적용

```python
image = self.transform(
    image
)
```

를 통해 Sample이 불러와질 때 Transform이 적용된다.

---

# 118. Random Transform이라면?

같은 Image라도 Epoch마다 다른 변형이 나올 수 있다.

이것이 Data Augmentation의 핵심 중 하나이다.

---

# 119. Validation Dataset

```python
train_dataset = ImageDataset(
    train_df,
    image_dir,
    transform=train_transform
)


val_dataset = ImageDataset(
    val_df,
    image_dir,
    transform=val_transform
)
```

처럼 서로 다른 Transform을 전달할 수 있다.

---

# 120. Test Dataset

```python
test_dataset = ImageDataset(
    test_df,
    image_dir,
    transform=val_transform
)
```

처럼 평가용 Transform을 재사용할 수 있다.

---

# 121. Transform 적용 전후 Shape

Raw:

```text
640 × 480
L
```

Transform 후:

```text
[3, 224, 224]
float32
```

처럼 바뀔 수 있다.

---

# 122. DataLoader 이후

```text
[3, 224, 224]
```

Image 여러 개가:

```text
[32, 3, 224, 224]
```

Batch가 된다.

---

# 123. Model 입력

```python
outputs = model(
    images
)
```

---

# 124. 전체 Pipeline

```text
Image File
↓
PIL
↓
convert("RGB")
↓
Resize
↓
Augmentation
↓
ToTensor
↓
Normalize
↓
Dataset Sample
[C, H, W]
↓
DataLoader
[B, C, H, W]
↓
CNN
```

---

# 125. Transform 오류가 나면

먼저 확인:

```text
현재 Input이 PIL인가?

NumPy인가?

Tensor인가?

Transform이 무엇을 기대하는가?
```

이다.

---

# 126. Shape 오류

```python
print(
    image.shape
)
```

또는 PIL이면:

```python
print(
    image.size
)

print(
    image.mode
)
```

를 확인한다.

---

# 127. Channel 오류

```text
expected 3 channels
but got 1
```

같은 오류가 발생하면:

```text
Image Mode

Transform 결과 Shape

Model in_channels
```

를 확인한다.

---

# 128. Normalize 오류

RGB Image인데:

```python
mean=[
    0.5
]
```

처럼 Channel 개수와 맞지 않는 Normalize 설정을 사용하면 문제가 생길 수 있다.

---

# 129. RGB

보통:

```text
mean 3개

std 3개
```

이다.

---

# 130. Grayscale

1 Channel Tensor라면:

```python
transforms.Normalize(
    mean=[
        0.5
    ],

    std=[
        0.5
    ]
)
```

처럼 Channel 수에 맞춘다.

---

# 131. Transform 후 Batch 확인

Training 전에 반드시:

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
    images.dtype
)

print(
    images.min()
)

print(
    images.max()
)

print(
    labels.shape
)
```

를 확인한다.

---

# 132. Label도 확인

```python
print(
    labels[:10]
)
```

---

# 133. Class 범위

CrossEntropyLoss라면 일반적으로 Label이:

```text
0 ~ num_classes - 1
```

범위의 정수 Class Index인지 확인한다.

---

# 134. Image와 Label 함께 보기

Dataset 검증에서 매우 중요하다.

```python
image, label = train_dataset[
    0
]


print(
    label
)
```

그리고 Image를 시각화하여 실제 Label과 맞는지 확인한다.

---

# 135. 잘못된 Label 연결

File 이름과 CSV Label 연결이 잘못되어 있으면 아무리 좋은 CNN을 사용해도 제대로 학습하기 어렵다.

---

# 136. Preprocessing보다 먼저 확인할 것

```text
Image와 Label이 올바르게 연결되었는가?
```

이다.

---

# 137. 의료 Image의 특별한 문제

의료영상은 일반 사진보다:

```text
미세한 Contrast

작은 병변

해부학적 방향

촬영 장비

Windowing

Bit Depth
```

등이 중요할 수 있다.

---

# 138. CT Windowing

CT 원본 Data에서는 특정 조직을 잘 보기 위해 값의 범위를 조정하는 Windowing을 사용할 수 있다.

단 PNG로 이미 가공된 CT Dataset이라면 원본 DICOM/CT 값과 동일한 처리를 할 수 있다고 가정하면 안 된다.

---

# 139. MRI

MRI는 촬영 Sequence에 따라 Image 특성이 달라질 수 있다.

예:

```text
T1

T2

FLAIR
```

등이다.

단순히 모든 MRI를 같은 Image라고 생각하면 안 된다.

---

# 140. X-ray

X-ray에서도:

```text
촬영 방향

장비

Image 후처리

환자 자세
```

등이 Pixel Distribution에 영향을 줄 수 있다.

---

# 141. Shortcut Learning 확인

Preprocessing 전후에:

```text
Border

Text Marker

촬영 표시

밝기

Padding
```

이 Class와 연결되어 있지 않은지 확인한다.

---

# 142. Model이 병변이 아니라 Text를 볼 수도 있다

예를 들어 특정 Class Image에만 특정 Marker가 있다면 CNN이 그 Marker를 학습할 가능성이 있다.

---

# 143. Crop으로 Marker를 제거하면 해결?

가능성이 있지만 무조건 Crop부터 하는 것이 정답은 아니다.

먼저:

```text
왜 Marker가 존재하는가?

Class와 얼마나 연관되어 있는가?

필요한 의료정보도 같이 잘리지 않는가?
```

를 확인한다.

---

# 144. Failure Case와 연결

Model이 틀린 Image를 확인한 뒤:

```text
Image가 너무 어둡다

위치가 다르다

병변이 작다
```

같은 문제를 발견하면 새로운 Preprocessing/Augmentation 실험을 설계할 수 있다.

---

# 145. 좋은 실험 흐름

```text
Baseline
↓
Failure Case 확인
↓
Hypothesis
↓
Preprocessing 변경
↓
다시 Training
↓
같은 Validation 기준으로 비교
```

이다.

---

# 146. Preprocessing은 성능을 올리기 위한 마법이 아니다

목적은:

```text
Model이 의미 있는 정보를
일관된 형태로 학습할 수 있게 준비
```

하는 것이다.

---

# 147. 과도한 Preprocessing

너무 많은 Image 변형이나 강한 전처리를 적용하면 오히려 원본 정보를 잃을 수 있다.

---

# 148. Raw Data를 보존

가능하면 원본 Image를 직접 덮어쓰지 않는다.

```text
Raw Image
→ 보존

Transform
→ Training 시 적용
```

구조가 관리하기 쉽다.

---

# 149. 전체 Debugging Checklist

Training 전에 확인:

```text
Image File이 존재하는가?

PIL로 열리는가?

Mode는 무엇인가?

원본 Size는?

Transform 후 Shape은?

Channel은?

dtype은?

Pixel Range는?

Normalize는 올바른가?

Image와 Label이 맞는가?

Train/Validation Transform이 구분되어 있는가?

Pretrained Weight와 Input 규칙이 맞는가?
```

---

# 150. 핵심 정리

Image Preprocessing:

```text
Raw Image를
Model이 사용할 수 있는
일관된 Input 형태로 준비하는 과정
```

대표 과정:

```text
Resize

Crop

Padding

Channel 처리

ToTensor

Normalize
```

Training에서는 필요에 따라:

```text
Random Augmentation
```

을 추가할 수 있다.

Validation/Test에서는 일반적으로:

```text
결정적인 Evaluation Transform
```

을 사용한다.

Pretrained Model에서는:

```text
Pretrained Weight와 호환되는
Preprocessing
```

이 중요하다.

의료 Image에서는 항상:

```text
이 변환이 실제 의료적 정보를
훼손하지 않는가?
```

를 확인한다.

---

# 151. 가장 중요한 흐름

```text
Raw Image
↓
PIL
↓
Channel 통일
↓
Resize / Crop
↓
Training이면 Augmentation
↓
ToTensor
↓
Normalize
↓
[C, H, W]
↓
DataLoader
↓
[B, C, H, W]
↓
CNN
```

---

# 152. 다음 단계

다음 파일:

```text
cnn-basic.md
```

에서는 지금까지 준비한 Image Tensor가 CNN 내부로 들어간 뒤:

```text
Conv2d

Kernel

Filter

Stride

Padding

Feature Map

ReLU

Pooling

Channel 증가

Spatial Size 감소

Flatten

Classifier
```

를 거쳐 어떻게 Image Feature를 학습하는지 자세히 공부한다.