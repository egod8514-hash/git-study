# Data Augmentation

## 1. Data Augmentation이란?

Data Augmentation은 기존 Training Data에 변형을 적용하여 다양한 형태의 Training Sample을 만드는 방법이다.

한국어로는:

```text
데이터 증강
```

이라고 한다.

Image에서는 대표적으로:

```text
Flip

Rotation

Crop

Translation

Brightness

Contrast

Scale
```

등을 사용할 수 있다.

---

# 2. 기본 아이디어

원본 Image가 하나 있다고 하자.

```text
Original Image
```

여기에 여러 변형을 적용한다.

```text
Original
├── 좌우 반전
├── 약간 회전
├── 밝기 변화
├── Crop
└── 위치 변화
```

Model은 매번 조금씩 다른 형태의 Image를 볼 수 있다.

---

# 3. 왜 사용할까?

Training Data가 제한적이면 Model이 Training Image 자체를 외울 수 있다.

즉:

```text
Overfitting
```

이 발생할 수 있다.

Augmentation은 Training Data에 변화를 주어 Model이 특정 Image의 세부적인 모습만 외우는 것을 줄이는 데 도움을 줄 수 있다.

---

# 4. 목표

좋은 Augmentation의 목표는:

```text
Label의 의미는 유지하면서
Input Image에는 적절한 변화를 주는 것
```

이다.

이것이 매우 중요하다.

---

# 5. 예

강아지 Image를 약간 좌우 이동시켜도:

```text
강아지
```

라는 Label은 그대로일 수 있다.

따라서 이런 변형은 Classification Training에 도움이 될 수 있다.

---

# 6. 하지만 모든 변형이 Label을 유지하는 것은 아니다

예를 들어 숫자:

```text
6
```

을 회전했더니:

```text
9
```

처럼 보일 수 있다.

이 경우 Rotation은 Label 의미를 바꿀 수 있다.

---

# 7. 따라서 Augmentation의 핵심 질문

항상:

```text
이 변형을 적용해도
Label이 여전히 맞는가?
```

를 생각한다.

---

# 8. torchvision.transforms

PyTorch에서는:

```python
from torchvision import transforms
```

를 사용할 수 있다.

---

# 9. Horizontal Flip

Image를 좌우로 반전한다.

```python
transforms.RandomHorizontalFlip()
```

---

# 10. 기본 Probability

일반적으로 기본값:

```text
p = 0.5
```

이다.

즉 대략 절반의 확률로 좌우 반전을 적용한다.

---

# 11. 직접 지정

```python
transforms.RandomHorizontalFlip(
    p=0.5
)
```

---

# 12. Random이라는 의미

Training Dataset에서 같은 Image를 다시 불러와도 매번 같은 결과가 나온다는 보장이 없다.

예:

```text
Epoch 1
→ Original

Epoch 2
→ Flip

Epoch 3
→ Original

Epoch 4
→ Flip
```

처럼 달라질 수 있다.

---

# 13. Vertical Flip

상하 반전:

```python
transforms.RandomVerticalFlip(
    p=0.5
)
```

---

# 14. Vertical Flip은 특히 주의

모든 Image 문제에서 자연스러운 변형이 아니다.

예를 들어 의료영상에서 사람의 해부학적 구조를 위아래로 뒤집는 것은 실제 Data Distribution과 크게 다를 수 있다.

따라서 무조건 사용하지 않는다.

---

# 15. Random Rotation

Image를 Random하게 회전한다.

```python
transforms.RandomRotation(
    degrees=10
)
```

---

# 16. 의미

대략:

```text
-10° ~ +10°
```

범위에서 Random Rotation을 적용할 수 있다.

---

# 17. 왜 작은 각도를 사용할까?

실제 촬영 과정에서 발생할 수 있는 작은 자세나 방향 차이를 모방하려는 경우가 있다.

---

# 18. 너무 큰 Rotation

예:

```text
90°

180°
```

처럼 크게 회전하면 실제 Dataset에서 존재하지 않는 Image가 만들어질 수 있다.

---

# 19. Random Crop

```python
transforms.RandomCrop(
    224
)
```

Image에서 Random한 위치를 잘라낸다.

---

# 20. RandomResizedCrop

자주 사용하는 Augmentation:

```python
transforms.RandomResizedCrop(
    224
)
```

이다.

---

# 21. 동작 개념

```text
Original Image
↓
Random 영역 선택
↓
Crop
↓
224 × 224 Resize
```

이다.

---

# 22. 장점

Model이 항상 정확히 같은 위치만 보지 않도록 할 수 있다.

---

# 23. 단점

중요한 Object나 병변이 Crop 밖으로 사라질 수 있다.

특히 작은 병변 Classification에서는 주의한다.

---

# 24. ColorJitter

Image의:

```text
Brightness

Contrast

Saturation

Hue
```

등을 Random하게 변경할 수 있다.

---

# 25. 예

```python
transforms.ColorJitter(
    brightness=0.2,
    contrast=0.2
)
```

---

# 26. Brightness

Image 밝기를 변경한다.

개념적으로:

```text
Dark
←
Original
→
Bright
```

이다.

---

# 27. Contrast

밝은 부분과 어두운 부분의 차이를 변화시킨다.

---

# 28. 일반 Image에서는

촬영 환경이나 조명 변화에 강한 Model을 만드는 데 도움을 줄 수 있다.

---

# 29. 의료 Image에서는 주의

Brightness와 Contrast 자체가 질병 Pattern과 관련될 수 있다.

따라서 지나치게 강한 변화는 실제 의료 정보를 훼손할 수 있다.

---

# 30. Saturation

색상의 강도를 변화시킨다.

RGB Image에서 의미가 있다.

---

# 31. Grayscale Image

Grayscale Image에는 Color 관련 Augmentation이 일반 RGB 사진과 같은 의미를 갖지 않을 수 있다.

---

# 32. RandomAffine

다양한 기하학적 변형을 적용할 수 있다.

```python
transforms.RandomAffine(
    degrees=5,
    translate=(
        0.05,
        0.05
    )
)
```

---

# 33. Translation

Image를:

```text
왼쪽

오른쪽

위

아래
```

로 조금 이동시키는 변형이다.

---

# 34. Scale

Image 크기를 조금 확대하거나 축소할 수 있다.

```python
transforms.RandomAffine(
    degrees=0,
    scale=(
        0.9,
        1.1
    )
)
```

---

# 35. Shear

Image를 기울이는 변형이다.

```python
transforms.RandomAffine(
    degrees=0,
    shear=5
)
```

---

# 36. Gaussian Blur

Image를 약간 흐리게 만들 수 있다.

```python
transforms.GaussianBlur(
    kernel_size=3
)
```

---

# 37. Blur도 주의

미세한 Texture나 작은 병변이 중요한 문제라면 Blur가 필요한 정보를 제거할 수 있다.

---

# 38. Random Perspective

Perspective 변형:

```python
transforms.RandomPerspective(
    distortion_scale=0.2,
    p=0.5
)
```

같은 Transform도 있다.

---

# 39. 모든 Augmentation을 넣는 것이 좋은가?

아니다.

Augmentation은 많을수록 좋은 것이 아니다.

```text
Task

Dataset

Domain

Label
```

을 고려해야 한다.

---

# 40. Augmentation의 가장 중요한 원칙

```text
현실적으로 가능한 변화인가?

Label을 유지하는가?

Prediction에 필요한 Feature를 보존하는가?
```

이다.

---

# 41. Training에만 Random Augmentation 적용

일반적인 구조:

```text
Train
→ Random Augmentation

Validation
→ Random Augmentation 없음

Test
→ Random Augmentation 없음
```

이다.

---

# 42. 왜 Validation에는 적용하지 않을까?

Validation은 Model 성능을 일정한 조건에서 비교하기 위한 Data이다.

Random Augmentation을 적용하면 평가할 때마다 Input이 달라질 수 있다.

---

# 43. 예

Train:

```python
train_transform = transforms.Compose(
    [
        transforms.Resize(
            (
                224,
                224
            )
        ),

        transforms.RandomHorizontalFlip(
            p=0.5
        ),

        transforms.RandomRotation(
            degrees=10
        ),

        transforms.ToTensor()
    ]
)
```

---

# 44. Validation

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

# 45. 차이

```text
Train
Resize
→ Random Flip
→ Random Rotation
→ ToTensor

Validation
Resize
→ ToTensor
```

이다.

---

# 46. Normalize까지 사용한다면

```python
train_transform = transforms.Compose(
    [
        transforms.Resize(
            (
                224,
                224
            )
        ),

        transforms.RandomHorizontalFlip(
            p=0.5
        ),

        transforms.RandomRotation(
            degrees=10
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

# 47. Validation

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

# 48. 기본 Preprocessing은 맞춘다

Train과 Validation에서 Random Augmentation은 다르지만:

```text
Channel

Input Size

Normalize
```

같은 기본 Input 조건은 일관되게 유지한다.

---

# 49. Augmentation은 Dataset에서 적용된다

Custom Dataset:

```python
if self.transform is not None:

    image = self.transform(
        image
    )
```

---

# 50. 같은 Image를 여러 번 확인

Random Transform이 실제로 동작하는지 확인할 수 있다.

```python
image = Image.open(
    "sample.png"
).convert(
    "RGB"
)


for i in range(
    5
):

    x = train_transform(
        image
    )

    print(
        x.shape
    )
```

---

# 51. 하지만 Shape만 보면 부족하다

Random Augmentation은 반드시 실제 Image를 시각화해서 확인하는 것이 좋다.

---

# 52. Tensor 시각화

Normalize 전 Tensor라면:

```python
x = train_transform(
    image
)


display_image = x.permute(
    1,
    2,
    0
)


plt.imshow(
    display_image
)

plt.axis(
    "off"
)

plt.show()
```

---

# 53. 여러 Augmentation 결과 확인

```python
fig, axes = plt.subplots(
    2,
    3,
    figsize=(
        10,
        7
    )
)


for ax in axes.flatten():

    x = train_transform(
        image
    )

    x = x.permute(
        1,
        2,
        0
    )

    ax.imshow(
        x
    )

    ax.axis(
        "off"
    )


plt.show()
```

---

# 54. 반드시 눈으로 확인

질문:

```text
원본 Class 의미가 유지되는가?

Image가 너무 심하게 변하지 않았는가?

중요한 부분이 잘리지 않았는가?

실제 Data에서 가능한 모습인가?
```

를 확인한다.

---

# 55. Normalize가 포함되어 있다면

Normalize된 Tensor를 그대로 표시하면 Image가 이상하게 보일 수 있다.

시각화용으로 Denormalize하거나 Augmentation 확인용 Transform을 따로 만드는 것도 좋다.

---

# 56. Augmentation 확인용 Transform

예:

```python
visual_transform = transforms.Compose(
    [
        transforms.Resize(
            (
                224,
                224
            )
        ),

        transforms.RandomRotation(
            10
        ),

        transforms.ToTensor()
    ]
)
```

처럼 Normalize를 제외하고 변형 자체를 확인할 수 있다.

---

# 57. Augmentation은 실제 File을 늘리는가?

일반적인 torchvision Transform 방식에서는 원본 File을 복사하여 Disk에 새 Image를 저장하는 것이 아니다.

---

# 58. On-the-fly Augmentation

Image가 Dataset에서 불러와질 때 Random Transform을 적용한다.

```text
Disk
Original Image 1장
↓
Dataset 호출
↓
Random Transform
↓
Training Tensor
```

이다.

---

# 59. 다음 Epoch

같은 File:

```text
Original Image
```

을 다시 읽더라도 다른 Random Transform 결과가 나올 수 있다.

---

# 60. 따라서

```text
Image File 수가 실제로 10배 증가
```

하는 것과 정확히 같은 개념은 아니다.

Training 과정에서 다양한 변형을 계속 생성하는 것이다.

---

# 61. DataLoader와 연결

```text
Original Files
↓
Dataset.__getitem__()
↓
Random Transform
↓
Tensor
↓
DataLoader
↓
Batch
↓
Model
```

이다.

---

# 62. Augmentation은 Label을 바꾸지 않는 경우가 많다

```python
return image, label
```

에서:

```text
image
→ 변형

label
→ 그대로
```

이다.

---

# 63. 그래서 Label-preserving이 중요

변형 후에도 Label이 맞아야 한다.

---

# 64. 의료영상에서 특히 중요한 이유

예를 들어 X-ray에서 병변이 매우 작은데 강한 Crop을 적용해 병변을 제거했다고 하자.

Image에는 병변이 보이지 않는데 Label은:

```text
Pneumonia = 1
```

로 그대로 남을 수 있다.

---

# 65. 이런 경우

Training Data에:

```text
Image 내용
↔
Label
```

불일치가 생길 수 있다.

Model 학습을 오히려 방해할 수 있다.

---

# 66. 의료영상 Augmentation 질문

Augmentation마다 다음을 확인한다.

```text
해부학적으로 가능한가?

촬영 과정에서 가능한 변화인가?

병변을 제거하지 않는가?

좌우 정보가 바뀌어도 되는가?

밝기 변화가 진단 정보를 훼손하지 않는가?
```

---

# 67. Horizontal Flip과 의료영상

일반적인 자연 Image에서는 자주 사용하지만 의료영상에서는 좌우가 임상적으로 중요할 수 있다.

따라서 무조건 적용하지 않는다.

---

# 68. Chest X-ray

Chest X-ray에서도 좌우 해부학적 위치가 존재한다.

따라서 Horizontal Flip을 사용할 때는 Task와 Label의 의미를 확인해야 한다.

---

# 69. Rotation

촬영 자세에서 발생할 수 있는 작은 회전을 모방하는 수준은 고려할 수 있다.

하지만 지나친 Rotation은 비현실적인 Image를 만들 수 있다.

---

# 70. Brightness

촬영 조건 차이에 대한 강건성을 높이는 데 도움이 될 수 있지만 병변의 밝기 Pattern까지 훼손하지 않는지 확인한다.

---

# 71. Contrast

마찬가지로:

```text
조직 경계

병변 Contrast
```

가 중요한 문제에서는 강도를 신중하게 선택한다.

---

# 72. Crop

폐 전체를 봐야 하는 Task인데 폐 일부가 잘리는 Crop을 적용하면 문제가 될 수 있다.

---

# 73. Augmentation은 Domain Knowledge가 필요하다

단순히:

```text
다른 사람이 많이 사용하니까
```

사용하는 것이 아니다.

---

# 74. Baseline 먼저

Augmentation 효과를 알고 싶다면 먼저:

```text
Baseline
```

을 만든다.

예:

```text
Resize
→ ToTensor
→ Normalize
```

---

# 75. Baseline 결과

예:

```text
Val Accuracy = 0.95

Val F1 = 0.96
```

라고 하자.

---

# 76. Experiment A

```text
Baseline
+
RandomRotation
```

만 추가한다.

---

# 77. 결과

예:

```text
Val Accuracy = 0.96

Val F1 = 0.97
```

이면 개선 가능성을 볼 수 있다.

---

# 78. Experiment B

```text
Baseline
+
Brightness
```

만 추가한다.

---

# 79. 이렇게 하는 이유

한 번에 여러 Augmentation을 추가하면:

```text
무엇 때문에 좋아졌는가?

무엇 때문에 나빠졌는가?
```

를 알기 어렵기 때문이다.

---

# 80. 좋은 Experiment 구조

```text
Baseline

A
→ Rotation

B
→ Brightness

C
→ Crop

D
→ A + B
```

처럼 단계적으로 비교할 수 있다.

---

# 81. 같은 Validation Split 사용

실험을 비교할 때:

```text
같은 Train/Validation Split
```

을 사용하는 것이 중요하다.

---

# 82. 같은 조건

가능하면:

```text
Model

Epoch

Optimizer

Learning Rate

Metric

Seed
```

등도 동일하게 유지한다.

---

# 83. 바꾸는 것은 Augmentation만

그래야 성능 차이를 Augmentation 변화와 더 직접적으로 연결해서 해석할 수 있다.

---

# 84. Train Accuracy가 떨어질 수도 있다

Augmentation을 사용하면 Training Data가 더 어려워질 수 있다.

예:

```text
Baseline Train Acc = 99%

Augmentation Train Acc = 96%
```

라고 해서 반드시 나쁜 것은 아니다.

---

# 85. Validation이 더 좋아질 수도 있다

예:

```text
Baseline
Train = 99%
Val   = 92%

Augmentation
Train = 96%
Val   = 95%
```

이면 Generalization이 개선되었을 가능성이 있다.

---

# 86. 핵심은 Train Accuracy 최고가 아니다

목표는:

```text
새로운 Data에서도 잘 Prediction
```

하는 것이다.

---

# 87. Augmentation이 너무 강한 경우

```text
Train Accuracy 낮음

Train Loss 높음

Validation도 개선되지 않음
```

이 나타날 수 있다.

---

# 88. Underfitting처럼 보일 수도 있다

Training Data 자체가 지나치게 어려워져 Model이 Pattern을 충분히 학습하지 못할 수 있다.

---

# 89. 따라서 강도를 조절한다

예:

```text
Rotation ±5°
```

부터 시작해서 필요하면:

```text
±10°
```

로 확장하는 식이다.

---

# 90. Probability도 조절

예:

```python
transforms.RandomHorizontalFlip(
    p=0.2
)
```

처럼 모든 Image에 강제로 적용하지 않을 수 있다.

---

# 91. RandomApply

특정 Transform 묶음을 일정 확률로 적용할 수 있다.

```python
transforms.RandomApply(
    [
        transforms.ColorJitter(
            brightness=0.2,
            contrast=0.2
        )
    ],
    p=0.5
)
```

---

# 92. 의미

```text
50% 확률
→ ColorJitter 적용

50% 확률
→ 적용하지 않음
```

이다.

---

# 93. RandomChoice

여러 Transform 중 하나를 Random하게 선택할 수도 있다.

```python
transforms.RandomChoice(
    [
        transforms.RandomRotation(
            5
        ),

        transforms.RandomHorizontalFlip()
    ]
)
```

---

# 94. 하지만 복잡하게 시작하지 않는다

처음에는 단순한 Augmentation부터 실험한다.

---

# 95. Augmentation Pipeline 예

일반 Image 예:

```python
train_transform = transforms.Compose(
    [
        transforms.Resize(
            (
                256,
                256
            )
        ),

        transforms.RandomResizedCrop(
            224
        ),

        transforms.RandomHorizontalFlip(
            p=0.5
        ),

        transforms.RandomRotation(
            10
        ),

        transforms.ColorJitter(
            brightness=0.1,
            contrast=0.1
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

# 96. 주의

위 Pipeline은 예시일 뿐 모든 Dataset에 그대로 적용하는 정답이 아니다.

특히 의료영상에서는 각 변형의 타당성을 검토한다.

---

# 97. Validation Pipeline

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

# 98. ImageNet Normalize

다음 값:

```text
mean
0.485, 0.456, 0.406

std
0.229, 0.224, 0.225
```

은 ImageNet 기반 Pretrained Model 예제에서 자주 볼 수 있다.

---

# 99. 무조건 이 값을 사용하는 것은 아니다

사용하는 Pretrained Weight의 권장 Transform을 확인하는 것이 더 안전하다.

---

# 100. Pretrained Weight

예:

```python
from torchvision.models import (
    ResNet18_Weights
)


weights = (
    ResNet18_Weights.DEFAULT
)
```

---

# 101. 기본 Evaluation Transform

```python
val_transform = (
    weights.transforms()
)
```

처럼 Weight와 연결된 Transform을 활용할 수 있다.

---

# 102. Training Augmentation

Training에서는 Pretrained Model의 기본 입력 조건을 고려하면서 Random Augmentation을 별도로 구성할 수 있다.

---

# 103. Data Augmentation과 Leakage

Split 전에 Augmentation File을 생성하고 나중에 Random Split하는 것은 주의해야 한다.

---

# 104. 예

원본:

```text
Patient A Image
```

에서:

```text
Original

Rotated

Brightness Changed
```

를 미리 만든 후 Random Split했다고 하자.

---

# 105. 잘못된 결과

```text
Original
→ Train

Rotated Version
→ Validation
```

이 될 수 있다.

---

# 106. 사실상 매우 비슷한 Image

Train과 Validation이 독립적이지 않게 된다.

Validation 성능이 과대평가될 수 있다.

---

# 107. 더 좋은 흐름

```text
Original Dataset
↓
Train / Validation Split
↓
Train에만 Random Augmentation
```

이다.

---

# 108. 의료영상에서는 Patient Split 먼저

```text
Patients
↓
Train Patients / Validation Patients
↓
Train Image에만 Augmentation
```

순서가 중요하다.

---

# 109. Augmentation은 Data Leakage 해결책이 아니다

Split이 잘못되어 있으면 Augmentation을 추가해도 Validation 자체가 신뢰하기 어렵다.

---

# 110. Class Imbalance와 Augmentation

Minority Class에 Augmentation을 더 적용하는 전략을 생각할 수도 있다.

하지만 단순히 같은 Image를 여러 형태로 보여주는 것이 새로운 독립 Patient Data를 확보하는 것과 같지는 않다.

---

# 111. Weighted Loss와는 다르다

Augmentation:

```text
Input Image 다양성 증가
```

Weighted Loss:

```text
Class별 Loss 중요도 조정
```

이다.

---

# 112. Sampling과도 다르다

Oversampling:

```text
특정 Sample을 더 자주 선택
```

Augmentation:

```text
선택된 Image에 변형 적용
```

이다.

---

# 113. 함께 사용할 수도 있다

예:

```text
Weighted Loss
+
Augmentation
```

같은 조합도 가능하다.

하지만 각각의 효과를 먼저 비교하는 것이 해석하기 쉽다.

---

# 114. TTA

Test Time Augmentation:

```text
TTA
```

라는 방법도 있다.

---

# 115. 일반적인 Training Augmentation

```text
Train Image
↓
Random Transform
↓
Training
```

이다.

---

# 116. TTA

Inference할 때 하나의 Test Image에 여러 변형을 적용하고 Prediction을 합치는 방식이다.

```text
Test Image
├── Original → Prediction
├── Flip → Prediction
└── Crop → Prediction
        ↓
     Average
```

같은 구조이다.

---

# 117. TTA는 기본 Test와 다르다

기본 Test:

```text
Image 1개
→ Prediction 1번
```

TTA:

```text
Image 1개
→ 여러 변형
→ 여러 Prediction
→ 결합
```

이다.

---

# 118. 장점

일부 경우 Prediction 안정성이나 성능이 개선될 수 있다.

---

# 119. 단점

```text
Inference 시간 증가

구현 복잡성 증가
```

가 있다.

---

# 120. TTA도 Label 의미를 보존해야 한다

Training Augmentation과 마찬가지이다.

---

# 121. 처음에는 TTA 없이 시작

먼저:

```text
기본 Validation/Test Pipeline
```

을 완성한 뒤 필요할 때 추가한다.

---

# 122. Augmentation과 Reproducibility

Random Augmentation 때문에 같은 Training을 실행해도 일부 결과가 달라질 수 있다.

---

# 123. Seed

```python
import random
import numpy as np
import torch


SEED = 42


random.seed(
    SEED
)

np.random.seed(
    SEED
)

torch.manual_seed(
    SEED
)
```

등을 사용할 수 있다.

---

# 124. 하지만 완전한 동일 결과는 별도 문제

GPU와 DataLoader, Library Algorithm 등에 따라 추가적인 비결정성이 존재할 수 있다.

---

# 125. 실험 기록

Augmentation을 사용했다면 정확한 설정을 기록한다.

잘못된 기록:

```text
Augmentation 사용
```

---

# 126. 좋은 기록

```text
RandomRotation
degrees = 10

RandomHorizontalFlip
p = 0.5

ColorJitter
brightness = 0.1
contrast = 0.1
```

처럼 기록한다.

---

# 127. 왜?

나중에 어떤 실험이 어떤 결과를 만들었는지 비교할 수 있기 때문이다.

---

# 128. Experiment Table

예:

```text
Experiment | Rotation | Flip | Brightness | Val Acc | F1
Baseline   | X        | X    | X          | 0.95    | 0.94
A          | 5°       | X    | X          | 0.96    | 0.95
B          | X        | X    | 0.1        | 0.95    | 0.95
C          | 5°       | X    | 0.1        | 0.97    | 0.96
```

---

# 129. 단일 Run만으로 확정하지 않는다

Deep Learning Training에는 Randomness가 존재한다.

작은 차이라면:

```text
Seed 변화

여러 Run

Cross-validation
```

등을 통해 안정적인 개선인지 추가로 확인할 수 있다.

---

# 130. Augmentation이 좋았다는 의미

단순히:

```text
Train Accuracy가 증가
```

가 아니다.

더 중요한 것은:

```text
Validation Generalization이 개선
```

되는가이다.

---

# 131. Train 성능이 낮아져도 괜찮을 수 있다

Augmentation 때문에 Training Task가 어려워졌기 때문이다.

---

# 132. 예

```text
Baseline
Train Acc = 99%
Val Acc   = 94%

Augmentation
Train Acc = 97%
Val Acc   = 96%
```

이라면 좋은 방향일 수 있다.

---

# 133. 반대 상황

```text
Baseline
Val Acc = 94%

Augmentation
Val Acc = 89%
```

이라면 Augmentation이 너무 강하거나 Task에 맞지 않을 가능성을 확인한다.

---

# 134. Failure Case와 연결

Error Image를 확인했더니:

```text
조금 회전된 Image에서
Error가 많다
```

고 하자.

---

# 135. Hypothesis

```text
Model이 Rotation 변화에 약한 것 같다.
```

---

# 136. Experiment

```text
작은 RandomRotation을
Training에 추가하면 개선될 것이다.
```

---

# 137. 검증

같은 Validation Set에서:

```text
Baseline

vs

Rotation Augmentation
```

을 비교한다.

---

# 138. 이것이 좋은 실험 흐름

```text
관찰
↓
가설
↓
변경
↓
실험
↓
평가
```

이다.

---

# 139. Shortcut Learning과 Augmentation

Model이 특정 Brightness에 지나치게 의존한다면 적절한 Brightness Augmentation으로 의존도를 줄일 가능성을 검토할 수 있다.

---

# 140. 하지만 무조건 해결되는 것은 아니다

Brightness 차이가:

```text
병원 장비

촬영 프로토콜

Class Label
```

과 강하게 연결되어 있다면 Dataset Bias 자체를 분석해야 한다.

---

# 141. Augmentation은 Data Quality 문제를 숨기는 도구가 아니다

예:

```text
잘못된 Label

중복 Image

Patient Leakage

Class별 촬영 장비 차이
```

를 Augmentation만으로 해결할 수 없다.

---

# 142. 먼저 Data를 이해한다

순서:

```text
EDA
↓
Split
↓
Baseline
↓
Failure Analysis
↓
Augmentation Hypothesis
↓
Experiment
```

가 좋다.

---

# 143. 의료 Image 예

Chest X-ray에서:

```text
Image 밝기 변화
```

가 크다고 하자.

바로 Brightness Augmentation을 넣기 전에:

```text
왜 밝기가 다른가?

촬영 장비 차이인가?

질환 자체와 관련 있는가?

전처리 차이인가?
```

를 확인한다.

---

# 144. Image Augmentation과 Model Architecture

Augmentation은 Model Architecture 자체를 바꾸는 것이 아니다.

```text
CNN
→ 그대로

Training Input
→ 변화
```

이다.

---

# 145. Loss도 그대로일 수 있다

Baseline:

```python
loss_fn = nn.CrossEntropyLoss()
```

Augmentation Experiment에서도 동일한 Loss를 유지할 수 있다.

---

# 146. Optimizer도 동일하게

```python
optimizer = torch.optim.Adam(
    model.parameters(),
    lr=0.001
)
```

을 유지하고 Augmentation만 변경하면 비교가 명확하다.

---

# 147. Augmentation 적용 위치

전체 Pipeline:

```text
Image File
↓
PIL
↓
Training Transform
↓
Random Augmentation
↓
ToTensor
↓
Normalize
↓
Dataset
↓
DataLoader
↓
CNN
```

이다.

---

# 148. Validation

```text
Validation Image
↓
Deterministic Transform
↓
ToTensor
↓
Normalize
↓
CNN
```

이다.

---

# 149. Test

```text
Test Image
↓
Validation과 호환되는 Transform
↓
CNN
↓
Prediction
```

이다.

---

# 150. Augmentation Debugging Checklist

Training 전에 확인한다.

```text
Transform 결과가 정상적으로 보이는가?

Label 의미가 유지되는가?

너무 많이 Crop되지 않는가?

너무 많이 회전되지 않는가?

밝기 변화가 과하지 않은가?

Channel은 맞는가?

Output Shape은 동일한가?

Validation에는 Random Transform이 없는가?
```

---

# 151. Experiment Checklist

```text
Baseline 결과가 있는가?

같은 Split을 사용하는가?

같은 Model인가?

같은 Metric인가?

Augmentation만 변경했는가?

설정을 기록했는가?

Failure Case를 다시 확인했는가?
```

---

# 152. 의료영상 Checklist

```text
해부학적으로 가능한 변형인가?

좌우 반전이 허용되는가?

회전 범위가 현실적인가?

병변이 Crop으로 사라지지 않는가?

Brightness/Contrast가 병변을 훼손하지 않는가?

환자 단위 Split을 먼저 했는가?
```

---

# 153. 핵심 정리

Data Augmentation:

```text
Training Image에
Label을 유지하는 적절한 변형을 적용해
Training Data의 다양성을 높이는 방법
```

이다.

대표적인 방법:

```text
Horizontal Flip

Vertical Flip

Rotation

Crop

Translation

Scale

Brightness

Contrast
```

등이 있다.

---

# 154. 가장 중요한 원칙

```text
많이 변형하는 것
≠
좋은 Augmentation
```

이다.

좋은 Augmentation:

```text
현실적인 변화

Label 유지

중요 Feature 보존
```

을 만족해야 한다.

---

# 155. Train과 Validation

```text
Train
→ Random Augmentation 사용 가능

Validation
→ Random Augmentation 일반적으로 제외
```

이다.

---

# 156. Augmentation 적용 순서

```text
Original Dataset
↓
Train / Validation Split
↓
Train에만 Random Augmentation
```

이다.

특히 의료영상에서는:

```text
Patient Split
↓
Train Patient에만 Augmentation
```

을 생각한다.

---

# 157. 성능 평가

Augmentation이 좋은지는 느낌으로 결정하지 않는다.

```text
Baseline
vs
Augmentation Experiment
```

의 Validation 결과로 비교한다.

---

# 158. 전체 연결

```text
EDA
↓
Baseline
↓
Failure Case
↓
Hypothesis
↓
Augmentation
↓
Training
↓
Validation
↓
Metric 비교
↓
새로운 Failure Analysis
```

이다.

---

# 159. 한 줄 정리

Data Augmentation의 목적은:

```text
Training Image를 무작정 많이 변형하는 것이 아니라
실제로 발생할 수 있는 변화를 학습시켜
새로운 Image에 대한 Generalization을 높이는 것
```

이다.

---

# 160. 다음 단계

다음 파일:

```text
transfer-learning.md
```

에서는:

```text
Pretrained Model이 무엇인가?

ImageNet이 무엇인가?

ResNet / DenseNet / EfficientNet은 무엇인가?

Backbone과 Classifier는 무엇인가?

왜 처음부터 CNN을 학습하지 않고
Pretrained Weight를 사용하는가?

마지막 Classifier를 왜 바꾸는가?

Freeze는 무엇인가?
```

를 공부한다.