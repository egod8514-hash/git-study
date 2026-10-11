# Computer Vision Basic

## 1. Computer Vision이란?

Computer Vision은 Computer가 Image나 Video를 분석하여 의미 있는 정보를 추출하도록 만드는 분야이다.

영어:

```text
Computer Vision
```

줄여서:

```text
CV
```

라고도 한다.

---

# 2. 사람이 Image를 보는 것과 Computer가 보는 것은 다르다

사람이 사진을 보면:

```text
강아지가 있다.

자동차가 있다.

폐 X-ray에 이상이 있다.
```

처럼 의미를 바로 이해할 수 있다.

하지만 Computer에게 Image는 기본적으로:

```text
숫자의 배열
```

이다.

---

# 3. Image는 숫자다

예를 들어 아주 작은 Grayscale Image가 있다고 하자.

```text
0    20   50

100  150  200

220  240  255
```

각 숫자는 Pixel의 밝기를 나타낼 수 있다.

즉 Computer는 Image를:

```text
Pixel 값들의 배열
```

로 처리한다.

---

# 4. Pixel이란?

Pixel은 Image를 구성하는 가장 작은 단위이다.

```text
Image
↓
수많은 Pixel
```

로 이루어진다.

---

# 5. Image Resolution

예:

```text
224 × 224
```

Image라면:

```text
Height = 224

Width = 224
```

이다.

Pixel 수:

```text
224 × 224
=
50,176
```

이다.

---

# 6. Grayscale Image

흑백 Image는 일반적으로 하나의 Channel로 표현할 수 있다.

예:

```text
0
→ 어두움

255
→ 밝음
```

중간 값은 그 사이의 밝기를 나타낸다.

---

# 7. Grayscale Shape

예:

```text
Height = 224
Width = 224
Channel = 1
```

Deep Learning Tensor에서는 보통:

```text
[1, 224, 224]
```

형태로 볼 수 있다.

---

# 8. RGB Image

Color Image는 대표적으로:

```text
Red

Green

Blue
```

3개의 Channel을 사용한다.

그래서:

```text
RGB
```

라고 한다.

---

# 9. RGB Tensor

PyTorch에서는 일반적으로:

```text
[Channel, Height, Width]
```

순서를 사용한다.

따라서 RGB 224×224 Image:

```text
[3, 224, 224]
```

이다.

---

# 10. Batch까지 포함하면

Batch Size가 32라면:

```text
[32, 3, 224, 224]
```

가 될 수 있다.

각각:

```text
32
→ Batch

3
→ Channel

224
→ Height

224
→ Width
```

이다.

---

# 11. Computer Vision의 대표 Task

Computer Vision에는 여러 문제가 있다.

대표적으로:

```text
Image Classification

Object Detection

Image Segmentation

Image Generation
```

등이 있다.

---

# 12. Image Classification

Image 전체가 어떤 Class인지 Prediction한다.

예:

```text
Image
↓
Model
↓
Cat
```

또는:

```text
Chest X-ray
↓
Model
↓
Normal / Pneumonia
```

이다.

---

# 13. Classification 입력과 출력

입력:

```text
Image
```

출력:

```text
Class
```

이다.

예:

```text
X-ray Image

↓

Normal = 0
Pneumonia = 1
```

---

# 14. Binary Classification

Class가 두 개인 경우:

```text
Class 0

Class 1
```

이다.

예:

```text
Normal

Pneumonia
```

---

# 15. Multi-class Classification

Class가 여러 개인 경우:

```text
Class 0

Class 1

Class 2

...

Class N
```

중 하나를 Prediction한다.

예를 들어 장기 Image를 여러 장기 종류 중 하나로 분류하는 문제가 될 수 있다.

---

# 16. Object Detection

Image 안에서:

```text
무엇이 있는가?

+

어디에 있는가?
```

를 찾는다.

---

# 17. 예

```text
Image
↓
Person
[x1, y1, x2, y2]

Car
[x1, y1, x2, y2]
```

처럼 Object Class와 Bounding Box를 Prediction한다.

---

# 18. Bounding Box

Object의 위치를 사각형으로 표현한다.

```text
┌─────────────┐
│             │
│    Object   │
│             │
└─────────────┘
```

---

# 19. 대표 Object Detection Model

예:

```text
YOLO

Faster R-CNN

SSD
```

등이 있다.

---

# 20. Image Segmentation

Image의 Pixel 단위로 Class를 Prediction한다.

즉:

```text
어떤 Pixel이
어떤 영역에 속하는가?
```

를 찾는다.

---

# 21. Segmentation 예

의료 Image에서:

```text
종양 영역

장기 영역

병변 영역
```

을 Pixel 단위로 구분할 수 있다.

---

# 22. Classification과 Segmentation 차이

Classification:

```text
Image 전체
↓
하나의 Class
```

Segmentation:

```text
각 Pixel
↓
Class
```

이다.

---

# 23. Object Detection과 Segmentation 차이

Detection:

```text
Object 위치
→ Box
```

Segmentation:

```text
Object 영역
→ Pixel Mask
```

이다.

---

# 24. Computer Vision + Deep Learning

과거 Computer Vision에서는 사람이 직접 Image Feature를 설계하는 방법도 많이 사용했다.

예:

```text
Edge

Corner

Texture

Shape
```

등이다.

Deep Learning에서는 CNN 등이 Image에서 유용한 Feature를 학습할 수 있다.

---

# 25. Feature란?

Image Classification에서 Feature는 Model이 Classification에 활용하는 Image의 특징이다.

예:

```text
Edge

Texture

Shape

Pattern

밝기 구조
```

등이다.

---

# 26. Neural Network만 사용하면?

일반적인 Fully Connected Neural Network에서도 Image Classification을 할 수 있다.

예:

```text
28 × 28 Image
↓
Flatten
↓
784 Numbers
↓
Linear
↓
Prediction
```

이다.

---

# 27. MNIST 예

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

# 28. 문제점

Image를 단순히 Flatten하면:

```text
Pixel의 공간적인 위치 관계
```

를 충분히 활용하기 어렵다.

Image에서는:

```text
위

아래

왼쪽

오른쪽

주변 Pixel
```

관계가 중요하다.

---

# 29. 그래서 CNN

Image 처리에서 대표적으로 사용하는 Neural Network:

```text
CNN
```

이다.

---

# 30. CNN

CNN:

```text
Convolutional Neural Network
```

한국어:

```text
합성곱 신경망
```

이다.

---

# 31. CNN의 핵심 아이디어

Image 전체를 한 번에 Flatten하기 전에 작은 영역을 보면서 Pattern을 찾는다.

예:

```text
Edge

Line

Corner

Texture
```

등이다.

---

# 32. 초기 Layer

CNN의 초기 Layer에서는 비교적 단순한 Pattern을 학습할 수 있다.

예:

```text
Edge

Line

Brightness 변화
```

---

# 33. 깊은 Layer

더 깊은 Layer에서는 앞에서 추출한 Feature들을 조합하여 더 복잡한 Pattern을 표현할 수 있다.

개념적으로:

```text
Edge
↓
Shape
↓
Part
↓
더 복잡한 Pattern
```

처럼 생각할 수 있다.

---

# 34. CNN 전체 흐름

```text
Image
↓
Convolution
↓
Activation
↓
Feature Map
↓
Pooling
↓
Convolution
↓
...
↓
Classifier
↓
Class Prediction
```

---

# 35. Convolution

CNN에서 핵심 연산이다.

작은 Filter가 Image를 이동하면서 지역적인 Pattern을 계산한다.

---

# 36. Filter

예:

```text
3 × 3
```

크기의 작은 Weight Matrix를 사용할 수 있다.

이 Filter의 Weight 역시 Training을 통해 학습된다.

---

# 37. Feature Map

Filter를 Image에 적용해서 만들어지는 결과를:

```text
Feature Map
```

이라고 한다.

---

# 38. Pooling

Feature Map의 공간 크기를 줄이는 연산을 사용할 수 있다.

대표:

```text
Max Pooling
```

이다.

---

# 39. CNN은 다음 파일에서 자세히

이번 파일에서는:

```text
CNN이 Image Feature를 학습하는 대표적인 Neural Network
```

라는 것까지만 이해한다.

세부적인:

```text
Kernel

Stride

Padding

Feature Map

Pooling
```

은 `cnn-basic.md`에서 자세히 다룬다.

---

# 40. Computer Vision Dataset

Image Classification Dataset은 흔히:

```text
Image

Label
```

로 구성된다.

---

# 41. 예

```text
image_001.png
→ 0

image_002.png
→ 1

image_003.png
→ 0
```

이다.

---

# 42. Folder 구조

Class별 Folder로 구성될 수도 있다.

```text
train/
├── normal/
│   ├── 001.png
│   └── 002.png
│
└── pneumonia/
    ├── 003.png
    └── 004.png
```

---

# 43. CSV 방식

CSV에서 Image File과 Label을 연결할 수도 있다.

```text
image_id,label

001.png,0

002.png,1
```

---

# 44. Dataset

PyTorch에서는:

```text
Dataset
```

이 Image를 읽고 Label과 함께 반환한다.

---

# 45. DataLoader

```text
DataLoader
```

가 Dataset에서 여러 Sample을 Batch로 묶어 Model에 공급한다.

---

# 46. 전체 연결

```text
Image File
↓
Dataset
↓
Transform
↓
Tensor
↓
DataLoader
↓
Batch
↓
CNN
↓
Prediction
```

이다.

---

# 47. Transform

Image를 Model이 사용할 수 있도록 변환한다.

대표적으로:

```text
Resize

ToTensor

Normalize
```

등이 있다.

---

# 48. Resize

Image 크기를 맞춘다.

예:

```text
원본
640 × 480

↓

224 × 224
```

---

# 49. 왜 크기를 맞출까?

Batch로 묶고 Model에 입력하려면 일반적으로 Input Shape를 일정하게 맞춰야 하기 때문이다.

---

# 50. ToTensor

Image를 PyTorch Tensor로 변환한다.

예:

```python
transforms.ToTensor()
```

---

# 51. Normalize

Pixel 값을 일정 기준으로 변환한다.

예:

```python
transforms.Normalize(
    mean,
    std
)
```

---

# 52. Preprocessing

이런 과정들을:

```text
Image Preprocessing
```

이라고 한다.

---

# 53. Augmentation

Training Image에 Random 변형을 적용하는 방법이다.

예:

```text
Rotation

Flip

Crop

Brightness
```

등이다.

---

# 54. 목적

Model이 Training Image의 특정 모습만 외우는 것을 줄이고 다양한 Image 변화에 강해지도록 도움을 줄 수 있다.

---

# 55. 하지만 Augmentation은 항상 좋은가?

아니다.

특히 의료 Image에서는 변형이 실제 의미를 바꾸지 않는지 확인해야 한다.

---

# 56. 예

Image를 너무 많이 Crop하면:

```text
병변 영역
```

이 사라질 수 있다.

그러면 Label은 그대로인데 실제 정보가 없어질 수 있다.

---

# 57. Train Transform

Training에서는:

```text
Resize

Random Augmentation

ToTensor

Normalize
```

를 사용할 수 있다.

---

# 58. Validation Transform

Validation에서는 일반적으로:

```text
Resize

ToTensor

Normalize
```

처럼 Random Augmentation을 제외한다.

---

# 59. Image Classification 전체 흐름

```text
Raw Image
↓
Train / Validation Split
↓
Dataset
↓
Transform
↓
DataLoader
↓
CNN
↓
Training
↓
Validation
↓
Best Model
↓
Test Image
↓
Prediction
```

---

# 60. Transfer Learning

Image Deep Learning에서 매우 자주 사용하는 방법이다.

이미 큰 Dataset으로 Training된 Model을 가져와 새로운 문제에 활용한다.

---

# 61. Pretrained Model

대표적으로:

```text
ResNet

DenseNet

EfficientNet

VGG
```

등을 볼 수 있다.

---

# 62. 왜 사용할까?

처음부터 모든 Feature를 학습시키는 대신 이미 학습된 Image Feature를 활용할 수 있다.

특히 Dataset이 크지 않은 경우 유용할 수 있다.

---

# 63. Classifier 변경

Pretrained Model이 원래:

```text
1000 Classes
```

를 분류하도록 만들어졌는데 우리의 문제는:

```text
2 Classes
```

라면 마지막 Classification Layer를 변경한다.

---

# 64. 예

개념적으로:

```text
Pretrained Backbone
↓
새로운 Classifier
↓
2 Classes
```

이다.

---

# 65. Freeze

Pretrained Layer의 Parameter를 학습하지 않도록 할 수 있다.

```python
for param in model.parameters():

    param.requires_grad = False
```

---

# 66. 그 후 Classifier만 학습

```text
Backbone
→ Freeze

Classifier
→ Train
```

이다.

---

# 67. Fine-tuning

그 후 일부 또는 전체 Layer를 다시 학습 가능하게 만들고 작은 Learning Rate로 추가 학습할 수 있다.

```text
Freeze Training
↓
Unfreeze
↓
Fine-tuning
```

---

# 68. Computer Vision에서 Evaluation

Classification에서는:

```text
Accuracy

Precision

Recall

F1

ROC-AUC

Confusion Matrix
```

등을 사용할 수 있다.

---

# 69. Class Imbalance

예:

```text
Normal = 25%

Pneumonia = 75%
```

처럼 Class 비율이 다를 수 있다.

이 경우 Accuracy만 보는 것은 부족할 수 있다.

---

# 70. Confusion Matrix

Binary Classification:

```text
                 Prediction

                0       1

Actual 0       TN      FP

Actual 1       FN      TP
```

이다.

---

# 71. False Positive

실제:

```text
0
```

인데 Prediction:

```text
1
```

이다.

---

# 72. False Negative

실제:

```text
1
```

인데 Prediction:

```text
0
```

이다.

---

# 73. 의료 AI에서는 Error 종류가 중요

의료 문제에서는:

```text
False Positive

False Negative
```

의 비용이 서로 다를 수 있다.

따라서 Accuracy 하나만으로 Model을 판단하지 않는다.

---

# 74. Failure Case Analysis

Model이 틀린 Image를 직접 확인한다.

예:

```text
High-confidence Error

Low-confidence Error

False Positive

False Negative
```

등이다.

---

# 75. 왜 중요할까?

단순 Metric만으로는:

```text
Model이 왜 틀렸는가?
```

를 알기 어렵다.

Image를 직접 확인하면 새로운 Hypothesis를 만들 수 있다.

---

# 76. 예

Model이 틀리는 Image에서:

```text
밝기가 다름

Position이 다름

Image 품질이 낮음

특정 장비 표시가 있음

병변이 매우 작음
```

같은 Pattern을 발견할 수 있다.

---

# 77. Shortcut Learning

CNN이 우리가 원하는 Feature가 아니라 쉬운 다른 단서를 이용해 Prediction할 수 있다.

예:

```text
밝기

Image Border

Text Marker

촬영 장비 특징

배경
```

등이다.

---

# 78. 예

우리는 Model이:

```text
폐의 병변
```

을 보고 Prediction한다고 생각했는데 실제로는:

```text
Image 밝기
```

를 이용할 수도 있다.

---

# 79. 그래서 Explainability

Model이 어떤 부분을 중요하게 사용했는지 확인하는 방법들이 있다.

Computer Vision에서는:

```text
Grad-CAM
```

을 자주 볼 수 있다.

---

# 80. Grad-CAM

CNN의 특정 Layer Activation과 Gradient를 이용해 Prediction에 영향을 준 Image 영역을 Heatmap 형태로 시각화하는 방법이다.

---

# 81. 결과

원본 Image 위에:

```text
Model이 중요하게 본 영역
```

을 Heatmap으로 표시할 수 있다.

---

# 82. 하지만 주의

Grad-CAM에서 특정 영역이 강조되었다고:

```text
Model이 질병의 원인을 이해했다
```

라고 단정하면 안 된다.

Model 해석을 돕는 보조적인 도구이다.

---

# 83. 의료 Image

대표적으로:

```text
X-ray

CT

MRI

Ultrasound
```

등이 있다.

---

# 84. X-ray

X-ray Image는 Projection Image이다.

Chest X-ray Classification 같은 문제에 Deep Learning을 적용할 수 있다.

---

# 85. CT

CT는 여러 Slice로 구성될 수 있다.

```text
Slice 1

Slice 2

Slice 3

...
```

이다.

---

# 86. MRI

MRI 역시 여러 Sequence와 Slice를 가질 수 있다.

따라서 단순 PNG 하나만 보는 문제와 실제 의료영상 전체를 다루는 문제는 다를 수 있다.

---

# 87. 의료영상 AI에서 중요한 것

단순히 Model Accuracy만 보는 것이 아니다.

다음도 중요하다.

```text
Patient-level Split

Data Leakage

Class Imbalance

Label 품질

촬영 장비 차이

기관 차이

Image Preprocessing

External Validation
```

---

# 88. Patient-level Split

같은 Patient의 여러 Image가 Train과 Validation에 동시에 들어가면 Leakage가 생길 수 있다.

---

# 89. 잘못된 예

```text
Patient A
Image 1 → Train

Patient A
Image 2 → Validation
```

---

# 90. 더 적절한 예

```text
Patient A
→ Train에만

Patient B
→ Validation에만
```

이다.

---

# 91. Data Leakage

Model이 실제 Prediction 시 사용할 수 없는 정보를 Training 과정에서 사용하게 되는 문제이다.

Computer Vision에서도 매우 중요하다.

---

# 92. Image 중복

같은 Image 또는 거의 동일한 Image가 Train과 Validation에 동시에 존재하면 Validation 성능이 과대평가될 수 있다.

---

# 93. Split이 먼저

일반적인 흐름:

```text
Original Data
↓
Train / Validation Split
↓
각 Dataset 생성
↓
Train Augmentation
```

이다.

---

# 94. Augmentation 후 Split은 주의

먼저 Image를 여러 개 Augmentation한 뒤 Random Split하면:

```text
원본
→ Train

거의 같은 변형 Image
→ Validation
```

이 될 수 있다.

---

# 95. Image Classification Training

전체 구조:

```python
for epoch in range(
    epochs
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

        outputs = model(
            images
        )

        loss = loss_fn(
            outputs,
            labels
        )

        loss.backward()

        optimizer.step()
```

---

# 96. Chapter 07과 같은 구조

Deep Learning Training Loop 자체는 같다.

달라지는 것은 주로:

```text
Input이 Image

Model이 CNN

Image Transform 사용
```

이라는 점이다.

---

# 97. Validation

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

        outputs = model(
            images
        )
```

---

# 98. Best Model 저장

```python
if val_acc > best_val_acc:

    best_val_acc = val_acc

    torch.save(
        model.state_dict(),
        "best_model.pt"
    )
```

---

# 99. Test Inference

```text
Test Image
↓
같은 Preprocessing
↓
Best Model
↓
Prediction
↓
Probability
```

이다.

---

# 100. Computer Vision Project 기본 흐름

```text
1. Image Dataset 확인

2. Label 확인

3. Image Size / Channel 확인

4. Class Distribution 확인

5. Train / Validation Split

6. Transform 작성

7. Dataset 작성

8. DataLoader 작성

9. Model 작성

10. Loss / Optimizer 설정

11. Training

12. Validation

13. Best Model 저장

14. Failure Case 분석

15. Explainability

16. Test Inference
```

---

# 101. Image Dataset을 받으면 먼저 볼 것

```text
Image 개수

Class 개수

Class별 개수

Image Size

Channel

File Format

Label 구조

중복 여부

Patient 정보 여부
```

등이다.

---

# 102. PIL

Python에서 Image를 읽을 때 자주 사용하는 Library:

```python
from PIL import Image
```

---

# 103. Image Open

```python
image = Image.open(
    "image.png"
)
```

---

# 104. 확인

```python
print(
    image.size
)

print(
    image.mode
)
```

---

# 105. PIL size

PIL의:

```python
image.size
```

는:

```text
(width, height)
```

순서이다.

---

# 106. mode

예:

```text
L
→ Grayscale

RGB
→ RGB Color
```

등이다.

---

# 107. 주의

Library에 따라 Image Dimension 순서가 다르게 표현될 수 있다.

예:

```text
PIL
(width, height)

NumPy
(height, width, channel)

PyTorch
(channel, height, width)
```

처럼 볼 수 있다.

---

# 108. Shape를 항상 확인

Computer Vision에서는 Shape 확인이 특히 중요하다.

```python
print(
    image_tensor.shape
)
```

---

# 109. PyTorch Batch

```text
[B, C, H, W]
```

를 기억한다.

---

# 110. 예

```text
[32, 1, 224, 224]
```

이면:

```text
32 Images

1 Channel

224 Height

224 Width
```

이다.

---

# 111. RGB

```text
[32, 3, 224, 224]
```

이다.

---

# 112. Model Input Channel

CNN의 첫 Layer:

```python
nn.Conv2d(
    in_channels=3,
    out_channels=16,
    kernel_size=3
)
```

이면 RGB 3 Channel Input을 기대한다.

---

# 113. Grayscale이라면

```python
nn.Conv2d(
    in_channels=1,
    out_channels=16,
    kernel_size=3
)
```

처럼 구성할 수 있다.

---

# 114. Pretrained Model과 Grayscale

ImageNet Pretrained Model은 일반적으로 RGB 3 Channel Input을 기대하는 경우가 많다.

따라서 Grayscale Image를 RGB 형태로 변환하여 사용하는 방법도 있다.

---

# 115. 예

PIL:

```python
image = Image.open(
    image_path
).convert(
    "RGB"
)
```

---

# 116. 이것이 실제 Color 정보를 만드는 것은 아니다

Grayscale Image를 RGB로 변환하면 Channel 형식을 3 Channel로 맞출 수 있지만 원래 존재하지 않던 Color 정보가 새로 생기는 것은 아니다.

---

# 117. Image Size

Pretrained Model에서 흔히:

```text
224 × 224
```

를 볼 수 있지만 모든 Model이 반드시 224×224만 받는 것은 아니다.

Model과 Weight에 맞는 Input preprocessing을 확인한다.

---

# 118. Image Resize의 Trade-off

너무 작게 Resize하면:

```text
작은 병변

미세한 Pattern
```

이 손실될 수 있다.

너무 크게 사용하면:

```text
GPU Memory 증가

Training 시간 증가
```

가 발생한다.

---

# 119. 따라서 Image Size도 Hyperparameter가 될 수 있다

예:

```text
224

320

384

512
```

등을 문제에 맞게 고려할 수 있다.

---

# 120. Baseline부터 시작

처음부터 가장 복잡한 Model을 만들기보다:

```text
단순 Transform

기본 Pretrained Model

기본 Loss

기본 Optimizer
```

로 Baseline을 만든다.

---

# 121. Baseline 이후

문제점을 확인하고:

```text
Augmentation

Weighted Loss

Fine-tuning

Learning Rate

Image Size

Model 변경
```

등을 하나씩 실험한다.

---

# 122. Experiment

좋은 실험:

```text
Baseline
↓
한 가지 변경
↓
Validation 비교
```

이다.

---

# 123. 예

```text
Baseline
→ 기본 Transform

Experiment A
→ Fine-tuning

Experiment B
→ Augmentation

Experiment C
→ Weighted Loss
```

---

# 124. 왜 한 가지씩?

여러 요소를 동시에 바꾸면:

```text
무엇 때문에 성능이 변했는가?
```

를 판단하기 어렵기 때문이다.

---

# 125. Computer Vision은 단순히 CNN 코드만 공부하는 것이 아니다

실제 성능에는:

```text
Dataset

Split

Preprocessing

Augmentation

Model

Loss

Optimizer

Evaluation

Failure Analysis
```

모두 영향을 준다.

---

# 126. 특히 Data가 중요

좋은 Model을 사용해도:

```text
Label 오류

Data Leakage

중복 Image

잘못된 Split

Train/Test 분포 차이
```

가 있으면 실제 성능이 좋지 않을 수 있다.

---

# 127. Computer Vision과 Deep Learning 관계

```text
Computer Vision
→ Image / Video 문제 분야

Deep Learning
→ 문제를 해결하는 방법 중 하나

CNN
→ Image에 많이 사용하는 Deep Learning Model
```

이다.

---

# 128. Computer Vision = CNN은 아니다

Computer Vision은 더 넓은 분야이다.

```text
Computer Vision
├── 전통적인 Image Processing
├── Machine Learning
└── Deep Learning
    ├── CNN
    ├── Vision Transformer
    └── 기타 Model
```

처럼 생각할 수 있다.

---

# 129. Vision Transformer

최근 Computer Vision에서는 CNN뿐 아니라:

```text
Vision Transformer
```

계열 Model도 사용된다.

줄여서:

```text
ViT
```

라고 한다.

---

# 130. 지금은 CNN부터

기본적인 Image 처리와 CNN을 먼저 이해한 후 Transfer Learning과 다른 Architecture를 공부하는 것이 이해하기 쉽다.

---

# 131. 우리가 집중할 흐름

```text
Image
↓
Tensor
↓
CNN
↓
Classification
↓
Transfer Learning
↓
Fine-tuning
↓
Evaluation
↓
Grad-CAM
```

이다.

---

# 132. 의료영상으로 연결

예:

```text
Chest X-ray
↓
Image Tensor
↓
CNN
↓
Normal / Pneumonia
```

---

# 133. CT로 연결

예:

```text
Abdominal CT Patch
↓
CNN
↓
Organ Class
```

---

# 134. MRI로 연결

예:

```text
MRI Image
↓
Preprocessing
↓
CNN
↓
Classification
```

문제로 구성할 수도 있다.

실제 MRI 분석에서는 환자 단위, Slice 구조, Sequence 등 추가적인 요소를 고려해야 한다.

---

# 135. Computer Vision 공부의 핵심 질문

Image Dataset을 받으면 다음 질문을 한다.

```text
무엇을 Prediction하는가?

Classification인가?

몇 개 Class인가?

Image Shape은?

Channel은?

Label은 어디 있는가?

Class Imbalance가 있는가?

Patient 단위인가?

어떻게 Split해야 하는가?

어떤 Preprocessing이 필요한가?

어떤 Metric을 볼 것인가?
```

---

# 136. Model을 만들기 전에 Data부터

바로 CNN부터 만드는 것보다:

```text
Image 개수

Label

Shape

Channel

Class Distribution

Sample Image
```

를 먼저 확인한다.

---

# 137. EDA와 Computer Vision

Tabular Data에서 EDA를 했던 것처럼 Image에서도 EDA가 필요하다.

예:

```text
Class별 Image 개수

Image Size 분포

Brightness

Channel

Sample Image

이상한 Image

중복 Image
```

등이다.

---

# 138. Image EDA 예

```python
from PIL import Image


image = Image.open(
    "sample.png"
)


print(
    image.size
)


print(
    image.mode
)
```

---

# 139. NumPy로 확인

```python
import numpy as np


arr = np.array(
    image
)


print(
    arr.shape
)


print(
    arr.min()
)


print(
    arr.max()
)


print(
    arr.mean()
)
```

---

# 140. 밝기 평균

Grayscale Image에서는:

```python
arr.mean()
```

을 간단한 평균 밝기 지표로 사용할 수 있다.

Class별 밝기 차이가 크다면 Model이 밝기를 Shortcut으로 활용할 가능성도 생각해볼 수 있다.

---

# 141. 시각화

```python
import matplotlib.pyplot as plt


plt.imshow(
    image,
    cmap="gray"
)

plt.axis(
    "off"
)

plt.show()
```

---

# 142. 직접 보는 것이 중요

Image AI에서는 숫자만 확인하지 말고 실제 Image Sample을 직접 확인한다.

```text
정상적인 Image인가?

회전되어 있지 않은가?

Border가 다른가?

Text가 있는가?

Class마다 촬영 방식이 다른가?
```

등을 볼 수 있다.

---

# 143. 최종적으로 Model보다 먼저 질문

```text
이 Data로 정말 내가 원하는 문제를
학습할 수 있는가?
```

를 생각한다.

---

# 144. Chapter 08 전체 학습 흐름

```text
Computer Vision Basic
↓
Image Data Basic
↓
Image Preprocessing
↓
CNN
↓
Image Classification
↓
Data Augmentation
↓
Transfer Learning
↓
Fine-tuning
↓
Evaluation
↓
Grad-CAM
↓
Medical Image AI
```

---

# 145. Chapter 07과 Chapter 08의 차이

Chapter 07:

```text
Deep Learning 자체를 이해
```

Chapter 08:

```text
Deep Learning을 Image 문제에 적용
```

이다.

---

# 146. 가장 중요한 Shape

PyTorch Image Batch:

```text
[B, C, H, W]
```

이다.

반드시 기억한다.

---

# 147. 가장 중요한 전체 Pipeline

```text
Image File
↓
Dataset
↓
Transform
↓
Tensor
↓
DataLoader
↓
CNN
↓
Loss
↓
Backward
↓
Optimizer
↓
Validation
↓
Best Model
↓
Test Inference
```

---

# 148. Computer Vision에서 가장 중요한 것은 Model 하나가 아니다

다음 전체를 같이 봐야 한다.

```text
Data

Split

Preprocessing

Augmentation

Model

Training

Evaluation

Interpretation
```

---

# 149. 한 줄 정리

Computer Vision은:

```text
Computer가 Image와 Video를 분석하여
의미 있는 정보를 추출하도록 만드는 분야
```

이다.

Deep Learning 기반 Image Classification에서는:

```text
Image
→ Tensor
→ CNN
→ Feature
→ Class Prediction
```

의 흐름을 사용한다.

---

# 150. 다음 단계

다음 파일:

```text
image-data-basic.md
```

에서는 Image 자체를 더 자세히 공부한다.

```text
Pixel

Resolution

Grayscale

RGB

Channel

PIL

NumPy Image

PyTorch Tensor

HWC

CHW

BCHW

Pixel Range

dtype
```

를 연결한다.