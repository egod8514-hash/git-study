# OrganAMNIST Abdominal CT Classification

## 1. 프로젝트 개요

이 프로젝트는 복부 CT에서 추출된 장기 이미지를 이용하여 이미지가 어떤 장기에 해당하는지 분류하는 Computer Vision 프로젝트이다.

전체 흐름은 다음과 같다.

```text
Abdominal CT
↓
장기 영역 Image
↓
Image Preprocessing
↓
Dataset
↓
DataLoader
↓
CNN
↓
Training
↓
Validation
↓
장기 Class Prediction
```

---

# 2. 프로젝트 유형

이번 문제는:

```text
Image Classification
```

이다.

Input:

```text
복부 CT 장기 Image
```

Output:

```text
장기 Class
```

이다.

---

# 3. Tabular Project와 차이

이전 프로젝트에서는:

```text
Age

BMI

Blood Pressure

Pain Score
```

같은 Feature를 직접 Model에 넣었다.

이번 프로젝트에서는:

```text
Image Pixel
```

자체가 Input이다.

---

# 4. Machine Learning 방식

일반적인 Tabular Machine Learning은 사람이 Feature를 정의하는 경우가 많다.

```text
Data
↓
Feature Engineering
↓
Model
```

---

# 5. Deep Learning 방식

CNN은 Image에서 필요한 Feature를 학습 과정에서 직접 찾아간다.

```text
Image
↓
CNN
↓
Feature Extraction
↓
Classification
```

---

# 6. Dataset

사용한 Dataset은:

```text
OrganAMNIST
```

계열의 복부 장기 이미지 Dataset이었다.

프로젝트에서는 압축 파일 형태로 Data를 준비했다.

예:

```text
03_OrganAMNIST.zip
```

---

# 7. Data Upload

Google Colab에서는 파일을 먼저 Upload할 수 있다.

```python
from google.colab import files


uploaded = files.upload()
```

---

# 8. Upload 확인

Upload가 완료되면 현재 Folder를 확인한다.

```python
import os


print(
    os.listdir(
        "/content"
    )
)
```

---

# 9. 왜 확인할까?

다음과 같은 Error를 예방하기 위해서이다.

```text
FileNotFoundError
```

---

# 10. 압축 해제

```python
import zipfile


zip_path = (
    "/content/03_OrganAMNIST.zip"
)


extract_path = (
    "/content/organamnist"
)


with zipfile.ZipFile(
    zip_path,
    "r"
) as zip_ref:

    zip_ref.extractall(
        extract_path
    )
```

---

# 11. 압축 해제 확인

```python
print(
    os.listdir(
        extract_path
    )
)
```

프로젝트에서 확인한 주요 Folder는:

```text
train

test
```

였다.

---

# 12. 중요한 교훈

압축을 풀었다고 바로 Dataset Code를 작성하지 않는다.

먼저:

```text
어떤 Folder가 생겼는가?

Image가 어디에 있는가?

Class Folder가 존재하는가?
```

를 확인한다.

---

# 13. Folder Structure

Image Classification Dataset은 다음과 같은 구조를 사용할 수 있다.

```text
train/
├── class_0/
│   ├── image1.png
│   └── image2.png
│
├── class_1/
│   ├── image1.png
│   └── image2.png
│
└── ...

test/
└── ...
```

실제 구조는 압축을 푼 결과를 기준으로 확인한다.

---

# 14. Folder 확인

```python
train_dir = (
    "/content/organamnist/train"
)


print(
    os.listdir(
        train_dir
    )
)
```

---

# 15. Class 확인

Class Folder가 있다면:

```python
classes = sorted(
    os.listdir(
        train_dir
    )
)


print(
    classes
)
```

---

# 16. 왜 sorted()?

Filesystem에서 Folder가 반환되는 순서는 우리가 원하는 Class 순서를 보장하지 않는다.

따라서 Class Mapping을 일정하게 유지하기 위해 정렬해서 사용할 수 있다.

---

# 17. 하지만 주의

Class 번호와 실제 장기 이름의 관계가 공식적으로 정의되어 있다면:

```text
공식 Label Mapping
```

을 우선해야 한다.

단순 Alphabetical Order를 의학적 Label 정의라고 가정하면 안 된다.

---

# 18. Image 개수

```python
for class_name in classes:

    class_dir = os.path.join(
        train_dir,
        class_name
    )

    count = len(
        os.listdir(
            class_dir
        )
    )

    print(
        class_name,
        count
    )
```

---

# 19. 왜 Class별 개수를 볼까?

Class Imbalance가 있는지 확인하기 위해서이다.

예:

```text
Class A
1,000장

Class B
100장
```

이면 Model이 Class A를 더 쉽게 Prediction하는 문제가 발생할 수 있다.

---

# 20. Image 하나 확인

```python
from PIL import Image


image_path = (
    "실제 이미지 경로"
)


image = Image.open(
    image_path
)


print(
    image.size
)


print(
    image.mode
)
```

---

# 21. Image Size

이번 Data는 작은 크기의 장기 Patch Image를 사용하는 구조였다.

예:

```text
64 × 64
```

크기의 Image를 다룰 수 있다.

---

# 22. Grayscale

CT 기반 Image는:

```text
Grayscale
```

형태일 수 있다.

PIL에서는 보통:

```text
L
```

Mode로 나타난다.

---

# 23. 확인

```python
print(
    image.mode
)
```

결과 예:

```text
L
```

---

# 24. Grayscale의 Channel

Grayscale Image는 기본적으로:

```text
1 Channel
```

이다.

---

# 25. RGB와 비교

```text
Grayscale
1 Channel

RGB
3 Channels
```

이다.

---

# 26. NumPy 변환

```python
import numpy as np


image_array = np.array(
    image
)


print(
    image_array.shape
)
```

---

# 27. Grayscale NumPy Shape

예:

```text
(64, 64)
```

이다.

즉:

```text
Height × Width
```

이다.

---

# 28. Visualization

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

# 29. 여러 Image 확인

Training 전에 각 Class의 Image를 직접 보는 것이 중요하다.

확인할 것:

```text
장기 위치

밝기

방향

Crop

Image Quality

Class별 차이

이상 Image
```

등이다.

---

# 30. Image EDA

Computer Vision에서도 EDA가 필요하다.

```text
Image 수

Class Distribution

Image Size

Channel

Pixel Range

Brightness

Duplicate

Corrupted File
```

등을 확인한다.

---

# 31. Pixel Range

```python
print(
    image_array.min()
)


print(
    image_array.max()
)


print(
    image_array.mean()
)
```

---

# 32. Pixel 값

일반적인 8-bit Image라면:

```text
0 ~ 255
```

범위를 가질 수 있다.

하지만 의료영상 원본의 Pixel 의미가 항상 단순 0~255인 것은 아니다.

---

# 33. CT 주의

원본 CT는:

```text
DICOM

Hounsfield Unit

Windowing
```

등의 개념이 중요할 수 있다.

하지만 PNG 등으로 전처리된 Dataset은 원본 CT 정보가 일부 변환된 상태일 수 있다.

---

# 34. 따라서

```text
PNG Pixel
=
원본 CT의 모든 정보
```

라고 생각하면 안 된다.

---

# 35. PyTorch

Deep Learning Model을 만들기 위해 PyTorch를 사용할 수 있다.

```python
import torch
import torch.nn as nn
```

---

# 36. torchvision

```python
from torchvision import (
    transforms
)
```

---

# 37. Transform

Image를 Model이 사용할 Tensor로 변환한다.

```python
transform = transforms.Compose(
    [
        transforms.ToTensor()
    ]
)
```

---

# 38. ToTensor

PIL Image:

```text
[H, W]
```

형태를 PyTorch Tensor:

```text
[C, H, W]
```

형태로 바꾼다.

---

# 39. 예

Grayscale 64×64:

```text
PIL / NumPy
[64, 64]

↓

PyTorch
[1, 64, 64]
```

이다.

---

# 40. Pixel Scaling

일반적인 uint8 Image에 `ToTensor()`를 적용하면 보통:

```text
0 ~ 255
↓
0.0 ~ 1.0
```

범위로 변환된다.

---

# 41. Tensor 확인

```python
tensor_image = transform(
    image
)


print(
    tensor_image.shape
)


print(
    tensor_image.dtype
)


print(
    tensor_image.min()
)


print(
    tensor_image.max()
)
```

---

# 42. Custom Dataset

Folder 구조와 Label 방식에 따라 직접 Dataset을 만들 수 있다.

```python
from torch.utils.data import (
    Dataset
)
```

---

# 43. Dataset의 역할

```text
Index 하나를 받는다.

↓

Image 하나를 읽는다.

↓

Transform한다.

↓

Image와 Label을 반환한다.
```

이다.

---

# 44. 기본 구조

```python
class OrganDataset(
    Dataset
):

    def __init__(
        self,
        samples,
        transform=None
    ):

        self.samples = samples
        self.transform = transform


    def __len__(
        self
    ):

        return len(
            self.samples
        )


    def __getitem__(
        self,
        idx
    ):

        image_path, label = (
            self.samples[
                idx
            ]
        )

        image = Image.open(
            image_path
        ).convert(
            "L"
        )

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

# 45. `__len__`

Dataset에 몇 개의 Sample이 있는지 반환한다.

---

# 46. `__getitem__`

특정 Index의:

```text
Image

Label
```

을 반환한다.

---

# 47. samples 만들기

예:

```python
samples = []


for label, class_name in enumerate(
    classes
):

    class_dir = os.path.join(
        train_dir,
        class_name
    )

    for filename in os.listdir(
        class_dir
    ):

        image_path = os.path.join(
            class_dir,
            filename
        )

        samples.append(
            (
                image_path,
                label
            )
        )
```

---

# 48. 확인

```python
print(
    len(
        samples
    )
)


print(
    samples[
        :5
    ]
)
```

---

# 49. Label Mapping

```python
class_to_idx = {
    class_name: idx
    for idx, class_name
    in enumerate(
        classes
    )
}


print(
    class_to_idx
)
```

---

# 50. 반드시 저장할 것

최종 Model을 사용할 때도:

```text
0 → 어떤 장기?

1 → 어떤 장기?
```

를 알아야 한다.

따라서 Class Mapping을 기록해야 한다.

---

# 51. Dataset 생성

```python
dataset = OrganDataset(
    samples=samples,
    transform=transform
)
```

---

# 52. 한 Sample 확인

```python
x, y = dataset[
    0
]


print(
    x.shape
)


print(
    y
)
```

---

# 53. 기대 결과

```text
Image
[1, 64, 64]

Label
정수
```

형태이다.

---

# 54. Train / Validation Split

Training Data를 다시 Train과 Validation으로 나눈다.

---

# 55. 왜?

```text
Train
→ Model 학습

Validation
→ 학습하지 않은 Data에서 성능 확인
```

을 하기 위해서이다.

---

# 56. random_split

간단하게는:

```python
from torch.utils.data import (
    random_split
)


train_size = int(
    len(
        dataset
    )
    *
    0.8
)


val_size = (
    len(
        dataset
    )
    -
    train_size
)


train_dataset, val_dataset = (
    random_split(
        dataset,
        [
            train_size,
            val_size
        ]
    )
)
```

---

# 57. 하지만 주의

Class Imbalance가 있다면 단순 Random Split보다 Class 비율을 유지하는 Stratified Split이 더 적절할 수 있다.

---

# 58. 의료영상에서 더 중요한 것

한 Patient에게 여러 Image가 있다면:

```text
Patient-level Split
```

을 해야 한다.

---

# 59. 잘못된 Split

```text
Patient A Image 1
→ Train

Patient A Image 2
→ Validation
```

이면 같은 Patient 정보가 양쪽에 들어간다.

---

# 60. Leakage

이 경우 Model이 새로운 Patient에 Generalization한 것이 아니라 같은 Patient의 비슷한 Image를 이미 봤을 수 있다.

---

# 61. 따라서

Patient ID가 존재한다면:

```text
Patient 기준으로 먼저 Split
```

한다.

---

# 62. DataLoader

```python
from torch.utils.data import (
    DataLoader
)


train_loader = DataLoader(
    train_dataset,
    batch_size=32,
    shuffle=True
)


val_loader = DataLoader(
    val_dataset,
    batch_size=32,
    shuffle=False
)
```

---

# 63. Batch

```text
Image 하나씩
```

Model에 넣는 대신 여러 Image를 묶어 Training한다.

---

# 64. 한 Batch 확인

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

# 65. 예상 Shape

Batch Size가 32라면:

```text
Images
[32, 1, 64, 64]

Labels
[32]
```

형태이다.

---

# 66. 이것이 중요하다

CNN을 만들기 전에 반드시 Batch Shape를 확인한다.

---

# 67. CNN

이번 문제에서는 Image Classification을 위해 CNN을 사용할 수 있다.

---

# 68. 기본 구조

```text
Image
↓
Conv
↓
ReLU
↓
Pooling
↓
Conv
↓
ReLU
↓
Pooling
↓
Feature
↓
Classifier
↓
Class Logits
```

---

# 69. Simple CNN

```python
class SimpleCNN(
    nn.Module
):

    def __init__(
        self,
        num_classes
    ):

        super().__init__()


        self.features = nn.Sequential(

            nn.Conv2d(
                1,
                16,
                kernel_size=3,
                padding=1
            ),

            nn.ReLU(),

            nn.MaxPool2d(
                2
            ),


            nn.Conv2d(
                16,
                32,
                kernel_size=3,
                padding=1
            ),

            nn.ReLU(),

            nn.MaxPool2d(
                2
            )
        )


        self.pool = (
            nn.AdaptiveAvgPool2d(
                (
                    1,
                    1
                )
            )
        )


        self.classifier = (
            nn.Linear(
                32,
                num_classes
            )
        )


    def forward(
        self,
        x
    ):

        x = self.features(
            x
        )

        x = self.pool(
            x
        )

        x = torch.flatten(
            x,
            1
        )

        x = self.classifier(
            x
        )

        return x
```

---

# 70. 첫 Conv2d

```python
nn.Conv2d(
    1,
    16,
    ...
)
```

의:

```text
1
```

은 Input Channel이다.

---

# 71. 왜 1?

Grayscale Image이기 때문이다.

---

# 72. RGB라면

```python
nn.Conv2d(
    3,
    16,
    ...
)
```

처럼 Input Channel이 3이어야 한다.

---

# 73. Channel Error

Grayscale Tensor:

```text
[B, 1, H, W]
```

를:

```text
in_channels=3
```

Model에 넣으면 Shape Error가 발생할 수 있다.

---

# 74. num_classes

마지막 Layer:

```python
nn.Linear(
    32,
    num_classes
)
```

의 Output 수는 Class 개수와 같아야 한다.

---

# 75. 예

11개의 장기 Class가 있다면:

```text
Output
[B, 11]
```

이 된다.

---

# 76. Model 생성

```python
num_classes = len(
    classes
)


model = SimpleCNN(
    num_classes=num_classes
)
```

---

# 77. Dummy Forward

Training 전에 Forward가 되는지 확인한다.

```python
dummy = torch.randn(
    4,
    1,
    64,
    64
)


output = model(
    dummy
)


print(
    output.shape
)
```

---

# 78. 예상

Class가 11개라면:

```text
[4, 11]
```

이다.

---

# 79. 이 단계가 중요한 이유

Training Loop를 돌린 뒤 Shape Error를 발견하는 것보다 먼저 Model 구조를 검증할 수 있기 때문이다.

---

# 80. Device

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

# 81. GPU

CUDA가 사용 가능하다면 Model과 Tensor를 GPU로 이동한다.

```python
model = model.to(
    device
)
```

---

# 82. Loss

Multiclass Classification에서는:

```python
loss_fn = nn.CrossEntropyLoss()
```

를 사용할 수 있다.

---

# 83. CrossEntropyLoss Input

Model Output:

```text
[B, C]
```

이다.

---

# 84. Target

Label:

```text
[B]
```

형태의 Integer Class Index이다.

---

# 85. Softmax를 먼저 하지 않는다

```python
loss_fn(
    logits,
    labels
)
```

처럼 Raw Logit을 CrossEntropyLoss에 넣는다.

---

# 86. Optimizer

```python
optimizer = torch.optim.Adam(
    model.parameters(),
    lr=0.001
)
```

---

# 87. Training Loop

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

# 88. Training 순서

```text
Batch
↓
Device
↓
zero_grad()
↓
Forward
↓
Loss
↓
Backward
↓
Optimizer Step
```

이다.

---

# 89. Epoch

모든 Training Sample을 한 번 사용하면:

```text
1 Epoch
```

이다.

---

# 90. Validation

```python
model.eval()


correct = 0
total = 0


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


        pred = torch.argmax(
            logits,
            dim=1
        )


        correct += (
            pred
            ==
            labels
        ).sum().item()


        total += labels.size(
            0
        )
```

---

# 91. Accuracy

```python
accuracy = (
    correct
    /
    total
)


print(
    accuracy
)
```

---

# 92. `model.eval()`

Validation에서:

```text
Dropout

BatchNorm
```

같은 Layer를 Evaluation Mode로 동작시킨다.

---

# 93. `torch.no_grad()`

Gradient 계산을 하지 않아:

```text
Memory 사용 감소

연산량 감소
```

효과를 얻을 수 있다.

---

# 94. Prediction

```python
pred = torch.argmax(
    logits,
    dim=1
)
```

을 이용해 가장 큰 Logit을 가진 Class를 선택한다.

---

# 95. Probability

필요하다면:

```python
prob = torch.softmax(
    logits,
    dim=1
)
```

으로 Class별 Probability 형태로 변환할 수 있다.

---

# 96. Evaluation

Multiclass Classification에서는 Accuracy만 보지 않는다.

```text
Confusion Matrix

Precision

Recall

F1

Macro F1
```

등을 확인할 수 있다.

---

# 97. 왜 Macro F1?

Class마다 Sample 수가 다를 경우 각 Class를 비슷한 비중으로 평가할 수 있기 때문이다.

---

# 98. Confusion Matrix

```python
from sklearn.metrics import (
    confusion_matrix
)


cm = confusion_matrix(
    y_true,
    y_pred
)
```

---

# 99. Confusion Matrix에서 볼 것

```text
어떤 장기를
어떤 장기로 자주 착각하는가?
```

이다.

---

# 100. 예

```text
Class 3
→ Class 4로 자주 Prediction
```

한다면 두 Class의 Image Pattern이 비슷한지 확인할 수 있다.

---

# 101. Failure Analysis

틀린 Image를 직접 본다.

---

# 102. 질문

```text
Image가 너무 어두운가?

Crop이 이상한가?

장기 일부만 보이는가?

두 장기 형태가 비슷한가?

Label이 잘못된 것은 아닌가?
```

를 확인한다.

---

# 103. Confidence

Probability를 이용하여:

```text
High-confidence Error
```

를 찾을 수 있다.

---

# 104. High-confidence Error

Model이 매우 확신했는데 틀린 Image이다.

---

# 105. 왜 중요할까?

단순히 어려운 Image가 아니라 Model이 잘못된 Pattern을 강하게 학습했을 가능성을 찾을 수 있기 때문이다.

---

# 106. Low-confidence Error

Probability가 여러 Class에 비슷하게 분산된 상태에서 틀린 경우이다.

이 경우 Model 자체도 판단하기 어려웠던 Sample일 수 있다.

---

# 107. Overfitting

Training Accuracy는 계속 올라가는데 Validation Accuracy가 떨어지면 Overfitting 가능성이 있다.

---

# 108. 확인

```text
Train Loss

Validation Loss

Train Accuracy

Validation Accuracy
```

를 Epoch별로 기록한다.

---

# 109. Learning Curve

```python
plt.plot(
    train_losses,
    label="Train"
)


plt.plot(
    val_losses,
    label="Validation"
)


plt.legend()


plt.show()
```

---

# 110. Data Augmentation

Overfitting을 줄이기 위해 Training Image에 변형을 줄 수 있다.

예:

```text
Rotation

Crop

Translation
```

등이다.

---

# 111. 하지만 의료영상

아무 Augmentation이나 사용하면 안 된다.

---

# 112. 예

좌우 방향 자체가 의미가 있다면:

```text
Horizontal Flip
```

이 Label 또는 해부학적 의미를 왜곡할 수 있다.

---

# 113. 따라서

Augmentation 전에 질문한다.

```text
이 변환 후에도
같은 장기라고 말할 수 있는가?

실제 CT에서 가능한 변화인가?
```

---

# 114. Transfer Learning

작은 CNN을 직접 학습하는 것 외에 Pretrained Model을 사용할 수도 있다.

예:

```text
ResNet

DenseNet

EfficientNet
```

---

# 115. 하지만 Grayscale

ImageNet Pretrained Model은 일반적으로:

```text
3 Channel RGB
```

Input을 기대한다.

---

# 116. 방법 1

Grayscale을 RGB 형태로 변환한다.

```python
image = Image.open(
    image_path
).convert(
    "RGB"
)
```

---

# 117. 주의

이렇게 한다고 새로운 Color 정보가 생기는 것은 아니다.

같은 Grayscale 정보를 3 Channel 형태로 맞추는 것이다.

---

# 118. 방법 2

Pretrained Model의 첫 Conv Layer를 1 Channel용으로 변경할 수도 있다.

하지만 Pretrained Weight를 어떻게 활용할 것인지 추가적인 처리가 필요하다.

---

# 119. Baseline 우선

처음부터 가장 복잡한 Model을 만들기보다:

```text
Simple CNN
```

을 Baseline으로 만든다.

---

# 120. 그 다음

```text
Experiment A
Data Augmentation

Experiment B
더 깊은 CNN

Experiment C
Transfer Learning

Experiment D
Fine-tuning
```

처럼 비교할 수 있다.

---

# 121. 같은 Validation

실험마다 Validation Data를 계속 바꾸면 Model 개선 여부를 판단하기 어렵다.

---

# 122. 따라서

가능하면:

```text
같은 Split

같은 Metric

같은 Seed
```

에서 비교한다.

---

# 123. Experiment Table

예:

```text
Experiment | Model | Augmentation | Accuracy | Macro F1

Baseline   | CNN   | None         | ...      | ...

Exp A      | CNN   | Rotation     | ...      | ...

Exp B      | ResNet| None         | ...      | ...

Exp C      | ResNet| Fine-tuning  | ...      | ...
```

---

# 124. Best Model

Validation 성능이 가장 적절한 Model을 저장한다.

```python
torch.save(
    model.state_dict(),
    "best_model.pt"
)
```

---

# 125. Last Model과 Best Model

마지막 Epoch의 Model이 항상 가장 좋은 것은 아니다.

---

# 126. 예

```text
Epoch 5
Validation Accuracy = 0.90

Epoch 10
Validation Accuracy = 0.86
```

이면 Epoch 5가 더 좋을 수 있다.

---

# 127. 따라서

Validation Metric을 기준으로 Best Model을 저장한다.

---

# 128. Test Data

최종적으로 Test Image를 Model에 넣어 Class를 Prediction한다.

---

# 129. Test에는 Label이 없을 수 있다

Competition 형태라면:

```text
Train
→ Image + Label

Test
→ Image only
```

일 수 있다.

---

# 130. Test DataLoader

```python
test_loader = DataLoader(
    test_dataset,
    batch_size=32,
    shuffle=False
)
```

---

# 131. 왜 shuffle=False?

Prediction 결과의 순서와 원본 Test Image 순서를 맞추기 위해서이다.

---

# 132. Inference

```python
model.eval()


predictions = []


with torch.no_grad():

    for images in test_loader:

        images = images.to(
            device
        )


        logits = model(
            images
        )


        pred = torch.argmax(
            logits,
            dim=1
        )


        predictions.extend(
            pred.cpu().numpy()
        )
```

실제 Dataset 반환 구조에 따라 코드는 달라질 수 있다.

---

# 133. Class Index 복원

Prediction이:

```text
0

1

2
```

처럼 나오면 Class Mapping을 이용해 실제 장기 이름으로 변환할 수 있다.

---

# 134. idx_to_class

```python
idx_to_class = {
    idx: class_name
    for class_name, idx
    in class_to_idx.items()
}
```

---

# 135. 변환

```python
predicted_classes = [
    idx_to_class[
        pred
    ]
    for pred in predictions
]
```

---

# 136. Model 저장 시 함께 필요한 것

```text
Model Architecture

state_dict

Class Mapping

Image Size

Channel

Normalization

Transform
```

등을 기록해야 한다.

---

# 137. 왜?

Model Weight만 가지고 있어도 Training 때와 다른 Preprocessing을 적용하면 Prediction 결과가 달라질 수 있기 때문이다.

---

# 138. Common Error 1

```text
ModuleNotFoundError:
No module named 'medmnist'
```

이 발생할 수 있다.

---

# 139. 해결

```python
!pip install medmnist
```

---

# 140. 그리고

```python
from medmnist import (
    OrganAMNIST
)
```

처럼 Import할 수 있다.

---

# 141. 중요한 점

Package 설치와 Import는 다른 단계이다.

```text
pip install
→ Package 설치

import
→ Python에서 사용
```

이다.

---

# 142. Environment

설치했는데도 Import가 안 되면:

```text
현재 Notebook Kernel

현재 Python Environment
```

가 같은지 확인한다.

---

# 143. Common Error 2

```text
FileNotFoundError
```

---

# 144. 확인

```python
print(
    os.getcwd()
)


print(
    os.listdir(
        "/content"
    )
)
```

---

# 145. Common Error 3

Channel mismatch.

```text
Input
[B, 1, 64, 64]

Model
in_channels=3
```

이면 Error가 발생한다.

---

# 146. Common Error 4

Class 수 mismatch.

```text
실제 Class = 11

Model Output = 10
```

이면 제대로 학습할 수 없다.

---

# 147. Common Error 5

CrossEntropyLoss Target Type.

Label은 일반적으로:

```text
torch.long
```

이어야 한다.

---

# 148. 확인

```python
print(
    labels.dtype
)
```

---

# 149. Common Error 6

Device mismatch.

```text
Model
→ CUDA

Image
→ CPU
```

이면 Error가 발생한다.

---

# 150. 해결

```python
images = images.to(
    device
)


labels = labels.to(
    device
)
```

---

# 151. Debugging 순서

Training이 안 되면 다음 순서로 확인한다.

```text
1. File이 존재하는가?

2. Image가 열리는가?

3. Label이 맞는가?

4. Dataset 한 Sample이 나오는가?

5. DataLoader 한 Batch가 나오는가?

6. Shape이 맞는가?

7. Model Forward가 되는가?

8. Loss가 계산되는가?

9. backward()가 되는가?

10. 1 Epoch가 실행되는가?
```

---

# 152. 한꺼번에 돌리지 않는다

전체 Training부터 실행한 뒤 Error를 찾는 것보다 작은 단계부터 검증하는 것이 빠르다.

---

# 153. One Batch Test

```python
images, labels = next(
    iter(
        train_loader
    )
)


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


loss.backward()


print(
    "Images:",
    images.shape
)


print(
    "Logits:",
    logits.shape
)


print(
    "Loss:",
    loss.item()
)
```

---

# 154. 여기까지 된다면

```text
Dataset

DataLoader

Model

Loss

Backward
```

의 기본 연결은 정상적으로 작동하는 것이다.

---

# 155. 의료영상 해석

Model이 높은 Accuracy를 보인다고 해서:

```text
실제 의료현장에서 사용할 수 있다.
```

고 바로 결론 내리면 안 된다.

---

# 156. 확인해야 할 것

```text
Patient-level Split

External Validation

Image Quality

Acquisition Device

Hospital Difference

Label Quality

Shortcut Learning
```

등이다.

---

# 157. Shortcut Learning

Model이 실제 장기 형태가 아니라:

```text
Image 위치

배경

밝기

Crop 방식

특정 Marker
```

같은 쉬운 단서를 학습할 수 있다.

---

# 158. 예

특정 장기 Class의 Image가 항상 더 밝다면 Model이 장기 모양보다 밝기를 학습할 수 있다.

---

# 159. 따라서

Failure Case와 Image EDA가 중요하다.

---

# 160. Explainability

CNN이 어느 부분을 보고 Prediction했는지 확인하기 위해:

```text
Grad-CAM
```

같은 방법을 사용할 수 있다.

---

# 161. Grad-CAM 질문

```text
Model이 이 장기를 분류할 때
Image의 어느 영역을 중요하게 봤는가?
```

를 탐색한다.

---

# 162. 하지만

Grad-CAM Heatmap이 장기 위치에 있다고 해서 Model의 의학적 판단이 올바르다는 것을 증명하는 것은 아니다.

---

# 163. 프로젝트 발표 흐름

```text
1. 문제 정의

2. OrganAMNIST Dataset

3. Folder Structure

4. Image EDA

5. Image Size / Channel

6. Preprocessing

7. Dataset

8. DataLoader

9. CNN

10. Training

11. Validation

12. Confusion Matrix

13. Failure Analysis

14. Augmentation

15. Transfer Learning

16. Grad-CAM

17. 한계 및 개선 방향
```

---

# 164. 발표 설명 예

```text
복부 CT에서 추출된
64×64 Grayscale 장기 Image를 이용하여
장기 종류를 분류하는
Multiclass Classification 문제를 구성하였다.

Image를 Tensor 형태로 변환하고
DataLoader를 통해 Batch 단위로 학습했으며,

CNN을 이용해 Image Feature를 추출하고
최종적으로 장기 Class를 Prediction하였다.
```

---

# 165. 이 프로젝트에서 배운 점 1

Computer Vision에서도 Model보다 먼저:

```text
Image Data 구조
```

를 확인한다.

---

# 166. 배운 점 2

PIL:

```text
Width × Height
```

NumPy:

```text
H × W
```

PyTorch:

```text
C × H × W
```

Batch:

```text
B × C × H × W
```

를 구분한다.

---

# 167. 배운 점 3

Grayscale:

```text
1 Channel
```

RGB:

```text
3 Channels
```

이다.

---

# 168. 배운 점 4

Model의:

```text
in_channels
```

와 실제 Image Channel이 일치해야 한다.

---

# 169. 배운 점 5

마지막 Layer의 Output 수는:

```text
Class 개수
```

와 일치해야 한다.

---

# 170. 배운 점 6

CrossEntropyLoss에는 Softmax를 적용하지 않은 Raw Logit을 넣는다.

---

# 171. 배운 점 7

Training 전에:

```text
한 Sample

한 Batch

Dummy Forward

Loss

Backward
```

를 먼저 테스트한다.

---

# 172. 배운 점 8

Accuracy만 보지 않고:

```text
Class별 Recall

Macro F1

Confusion Matrix
```

를 확인한다.

---

# 173. 배운 점 9

틀린 Image를 직접 보는:

```text
Failure Analysis
```

가 중요하다.

---

# 174. 배운 점 10

의료영상에서는 같은 Patient의 Image가 Train과 Validation에 동시에 들어가지 않도록 주의한다.

---

# 175. 전체 Workflow

```text
03_OrganAMNIST.zip
↓
Colab Upload
↓
압축 해제
↓
train / test 확인
↓
Class Folder 확인
↓
Image EDA
↓
64×64 Grayscale 확인
↓
ToTensor
↓
[C, H, W]
↓
Dataset
↓
Train / Validation Split
↓
DataLoader
↓
[B, 1, 64, 64]
↓
CNN
↓
Conv + ReLU + Pool
↓
Classifier
↓
CrossEntropyLoss
↓
Adam
↓
Training
↓
Validation
↓
Confusion Matrix
↓
Failure Analysis
↓
Best Model
↓
Test Inference
```

---

# 176. Tabular와 비교

```text
Tabular ML

CSV
↓
Feature
↓
Preprocessing
↓
Model
↓
Prediction
```

반면:

```text
Computer Vision

Image
↓
Pixel
↓
CNN Feature Extraction
↓
Classifier
↓
Prediction
```

이다.

---

# 177. 프로젝트의 의미

이 프로젝트는 기존의:

```text
Pandas
+
Machine Learning
```

중심 프로젝트에서:

```text
Image
+
PyTorch
+
CNN
```

으로 넘어가는 연결점이다.

---

# 178. 한 문장으로 설명

이 프로젝트는:

```text
복부 CT에서 추출된
64×64 Grayscale 장기 Image를 이용하여

PyTorch Dataset과 DataLoader를 구성하고,
CNN을 학습하여 장기 종류를 분류하면서

Image Shape, Channel, Tensor,
Batch, Convolution, Training Loop,
Validation의 전체 Computer Vision Workflow를
실습한 프로젝트
```

이다.

---

# 179. 가장 중요한 교훈

Computer Vision Model을 만들 때도:

```text
CNN부터 만든다.
```

가 아니다.

먼저:

```text
Image가 실제로 어디 있는가?

몇 장인가?

몇 Class인가?

Image Size는?

Channel은?

Label Mapping은?

한 Sample Shape은?

한 Batch Shape은?
```

를 확인한다.

그 다음:

```text
Dataset
↓
DataLoader
↓
Model
↓
Loss
↓
Backward
↓
Training
```

순서로 연결한다.