# Image Classification

## 1. Image Classification이란?

Image Classification은:

```text
Image
↓
Model
↓
Class
```

형태로 Image 전체가 어떤 Class에 속하는지 Prediction하는 문제이다.

예:

```text
Chest X-ray
↓
CNN
↓
Normal / Pneumonia
```

---

# 2. 입력과 출력

입력:

```text
Image
```

출력:

```text
Class
```

예:

```text
Input
[3, 224, 224]

↓

CNN

↓

Output
[2개의 Logit]
```

---

# 3. Binary Classification

Class가 2개인 경우이다.

예:

```text
0 = Normal

1 = Pneumonia
```

---

# 4. Multi-class Classification

Class가 3개 이상이고 그중 하나를 Prediction하는 문제이다.

예:

```text
0 = Liver

1 = Kidney

2 = Spleen

3 = Pancreas
```

등이다.

---

# 5. Multi-label Classification과는 다르다

Multi-class:

```text
여러 Class 중 하나
```

Multi-label:

```text
하나의 Image에 여러 Label이 동시에 존재 가능
```

이다.

예:

```text
Pneumonia = 1

Effusion = 1

Cardiomegaly = 0
```

처럼 여러 Label을 동시에 Prediction할 수 있다.

---

# 6. 이번 파일의 기본 문제

먼저 가장 이해하기 쉬운:

```text
Binary Image Classification
```

을 기준으로 한다.

하지만 Model 출력은:

```text
2 Logits
```

을 사용하고:

```python
nn.CrossEntropyLoss()
```

를 사용하는 구조로 설명한다.

---

# 7. 전체 Pipeline

```text
Image Files
+
Labels
↓
Data Inspection
↓
Train / Validation Split
↓
Transform
↓
Dataset
↓
DataLoader
↓
CNN
↓
Loss
↓
Optimizer
↓
Training
↓
Validation
↓
Best Model
↓
Inference
```

---

# 8. 가장 먼저 Data를 확인한다

Model부터 만들지 않는다.

먼저 확인:

```text
Image 개수

Label 개수

Class 개수

Class Distribution

Image Size

Channel

File Path

중복 여부
```

등이다.

---

# 9. CSV 예

예를 들어:

```text
image,label

image_001.png,0

image_002.png,1

image_003.png,1
```

형태의 CSV가 있다고 하자.

---

# 10. Pandas로 읽기

```python
import pandas as pd


df = pd.read_csv(
    "train.csv"
)


print(
    df.head()
)
```

---

# 11. Shape

```python
print(
    df.shape
)
```

---

# 12. Column

```python
print(
    df.columns
)
```

---

# 13. Label Distribution

```python
print(
    df["label"].value_counts()
)
```

---

# 14. 비율

```python
print(
    df["label"].value_counts(
        normalize=True
    )
)
```

---

# 15. 왜 Class Distribution을 볼까?

예:

```text
Class 0
25%

Class 1
75%
```

처럼 불균형할 수 있기 때문이다.

이 경우 Accuracy만 보면 Model 성능을 잘못 해석할 수 있다.

---

# 16. Image File 확인

```python
import os


image_path = os.path.join(
    "train",
    df.loc[
        0,
        "image"
    ]
)


print(
    image_path
)


print(
    os.path.exists(
        image_path
    )
)
```

---

# 17. Image 열기

```python
from PIL import Image


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

# 18. Sample Image 확인

```python
import matplotlib.pyplot as plt


plt.imshow(
    image,
    cmap="gray"
)

plt.title(
    f"Label: {df.loc[0, 'label']}"
)

plt.axis(
    "off"
)

plt.show()
```

---

# 19. Image와 Label을 같이 확인해야 한다

중요하다.

```text
Image가 실제 Label과 맞는가?
```

를 Sample 여러 장에서 직접 확인한다.

CSV와 Image 연결이 잘못되면 Model을 아무리 잘 만들어도 제대로 학습할 수 없다.

---

# 20. Train / Validation Split

Training Data와 Validation Data를 나눈다.

```python
from sklearn.model_selection import (
    train_test_split
)


train_df, val_df = train_test_split(

    df,

    test_size=0.2,

    random_state=42,

    stratify=df[
        "label"
    ]
)
```

---

# 21. stratify

Classification에서는 Class 비율을 비슷하게 유지하기 위해:

```python
stratify=df["label"]
```

를 사용할 수 있다.

---

# 22. 확인

```python
print(
    train_df[
        "label"
    ].value_counts(
        normalize=True
    )
)


print(
    val_df[
        "label"
    ].value_counts(
        normalize=True
    )
)
```

---

# 23. 의료 Image에서는 단순 Random Split 주의

한 Patient가 여러 Image를 가지고 있다면:

```text
Patient A Image 1
→ Train

Patient A Image 2
→ Validation
```

이 될 수 있다.

이 경우 Patient 정보가 Train과 Validation 사이에 섞일 수 있다.

---

# 24. Patient-level Split

Patient ID가 있다면 상황에 따라:

```text
Patient 단위
```

로 Split하는 것이 필요하다.

---

# 25. Image Classification에서 Split은 매우 중요하다

좋은 Model보다 먼저:

```text
Validation이 실제 Generalization을
제대로 평가하는가?
```

를 생각한다.

---

# 26. Transform 작성

먼저 Baseline Transform을 만든다.

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

        transforms.ToTensor()
    ]
)


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

# 27. 처음에는 단순하게

처음부터:

```text
Rotation

Flip

Brightness

Contrast

Crop
```

를 모두 넣지 않는다.

먼저 Baseline을 만든다.

---

# 28. 그다음 Experiment

예:

```text
Baseline
→ Resize + ToTensor

Experiment A
→ Fine-tuning

Experiment B
→ Augmentation

Experiment C
→ Weighted Loss
```

처럼 비교할 수 있다.

---

# 29. Normalize

Pretrained Model을 사용한다면 해당 Weight에 맞는 Normalize를 사용하는 것이 중요하다.

직접 CNN을 만드는 예제에서는 간단한 Transform부터 시작할 수 있다.

---

# 30. Custom Dataset

```python
import os

from PIL import Image

from torch.utils.data import Dataset


class ImageDataset(
    Dataset
):

    def __init__(
        self,
        df,
        image_dir,
        transform=None
    ):

        self.df = (
            df.reset_index(
                drop=True
            )
        )

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

        image_name = self.df.loc[
            idx,
            "image"
        ]

        image_path = os.path.join(
            self.image_dir,
            image_name
        )


        image = Image.open(
            image_path
        ).convert(
            "RGB"
        )


        label = int(
            self.df.loc[
                idx,
                "label"
            ]
        )


        if self.transform is not None:

            image = self.transform(
                image
            )


        return (
            image,
            label
        )
```

---

# 31. 왜 reset_index를 사용할까?

`train_test_split()` 이후 DataFrame Index가:

```text
3

18

25

...
```

처럼 원래 Index를 유지할 수 있다.

`__getitem__(idx)`에서는:

```text
0

1

2

...
```

형태로 접근하므로 Index를 다시 정리하면 편리하다.

---

# 32. Dataset 생성

```python
train_dataset = ImageDataset(

    train_df,

    image_dir="train",

    transform=train_transform
)


val_dataset = ImageDataset(

    val_df,

    image_dir="train",

    transform=val_transform
)
```

---

# 33. Dataset Sample 확인

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

예:

```text
torch.Size([3, 224, 224])

0
```

---

# 34. Dataset 길이

```python
print(
    len(
        train_dataset
    )
)


print(
    len(
        val_dataset
    )
)
```

---

# 35. DataLoader

```python
from torch.utils.data import (
    DataLoader
)


train_loader = DataLoader(

    train_dataset,

    batch_size=32,

    shuffle=True,

    num_workers=0
)


val_loader = DataLoader(

    val_dataset,

    batch_size=32,

    shuffle=False,

    num_workers=0
)
```

---

# 36. Training은 shuffle=True

Training에서는 일반적으로 Sample 순서를 섞는다.

```python
shuffle=True
```

---

# 37. Validation은 shuffle=False

Validation에서는:

```python
shuffle=False
```

를 사용하는 경우가 많다.

---

# 38. Windows에서 num_workers

처음 Debugging할 때는:

```python
num_workers=0
```

부터 시작하면 DataLoader 문제를 확인하기 편할 수 있다.

---

# 39. Batch 확인

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

예:

```text
images
[32, 3, 224, 224]

labels
[32]
```

---

# 40. dtype 확인

```python
print(
    images.dtype
)


print(
    labels.dtype
)
```

CrossEntropyLoss에서는 일반적으로:

```text
Image
→ float32

Label
→ int64 / torch.long
```

형태를 사용한다.

---

# 41. Model 만들기

간단한 CNN:

```python
import torch
import torch.nn as nn


class SimpleCNN(
    nn.Module
):

    def __init__(
        self,
        num_classes=2
    ):

        super().__init__()


        self.features = nn.Sequential(

            nn.Conv2d(
                3,
                32,
                kernel_size=3,
                padding=1
            ),

            nn.ReLU(),

            nn.MaxPool2d(
                2
            ),

            nn.Conv2d(
                32,
                64,
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


        self.classifier = nn.Linear(
            64,
            num_classes
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

# 42. Model 생성

```python
model = SimpleCNN(
    num_classes=2
)
```

---

# 43. Device

```python
device = torch.device(

    "cuda"

    if torch.cuda.is_available()

    else "cpu"
)
```

---

# 44. 확인

```python
print(
    device
)
```

---

# 45. Model 이동

```python
model = model.to(
    device
)
```

---

# 46. Image와 Label도 이동해야 한다

Training Batch마다:

```python
images = images.to(
    device
)


labels = labels.to(
    device
)
```

한다.

---

# 47. 같은 Device

다음이 같은 Device에 있어야 한다.

```text
Model

Images

Labels
```

---

# 48. Loss Function

Multi-class 또는 2-logit Binary Classification:

```python
loss_fn = nn.CrossEntropyLoss()
```

---

# 49. CrossEntropyLoss 입력

Model Output:

```text
[B, Classes]
```

Label:

```text
[B]
```

이다.

---

# 50. 예

```text
logits
[32, 2]

labels
[32]
```

이다.

---

# 51. Label 값

2 Classes:

```text
0

1
```

이다.

---

# 52. Softmax를 먼저 적용하지 않는다

Training:

```python
logits = model(
    images
)


loss = loss_fn(
    logits,
    labels
)
```

이다.

---

# 53. Optimizer

```python
optimizer = torch.optim.Adam(
    model.parameters(),
    lr=0.001
)
```

---

# 54. Training Loop

```python
num_epochs = 5


for epoch in range(
    num_epochs
):

    model.train()


    train_loss_sum = 0.0

    train_correct = 0

    train_count = 0


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


        batch_size = labels.size(
            0
        )


        train_loss_sum += (
            loss.item()
            * batch_size
        )


        preds = logits.argmax(
            dim=1
        )


        train_correct += (
            preds == labels
        ).sum().item()


        train_count += batch_size
```

---

# 55. Training Loss

```python
train_loss = (
    train_loss_sum
    / train_count
)
```

---

# 56. Training Accuracy

```python
train_acc = (
    train_correct
    / train_count
)
```

---

# 57. 왜 Loss에 batch_size를 곱할까?

`CrossEntropyLoss()`의 기본 reduction은 일반적으로 Batch 평균이다.

마지막 Batch가 작을 수 있기 때문에:

```text
Batch Loss 평균들의 단순 평균
```

보다:

```text
Sample 수를 고려한 평균
```

을 계산하는 것이 더 정확하다.

---

# 58. Validation

Training이 끝난 뒤 Validation을 수행한다.

```python
model.eval()
```

---

# 59. no_grad

```python
with torch.no_grad():
```

를 사용한다.

---

# 60. Validation에서는 하지 않는 것

```text
loss.backward()

optimizer.step()
```

을 하지 않는다.

---

# 61. Validation Loop

```python
model.eval()


val_loss_sum = 0.0

val_correct = 0

val_count = 0


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


        loss = loss_fn(
            logits,
            labels
        )


        batch_size = labels.size(
            0
        )


        val_loss_sum += (
            loss.item()
            * batch_size
        )


        preds = logits.argmax(
            dim=1
        )


        val_correct += (
            preds == labels
        ).sum().item()


        val_count += batch_size
```

---

# 62. Validation Metric

```python
val_loss = (
    val_loss_sum
    / val_count
)


val_acc = (
    val_correct
    / val_count
)
```

---

# 63. Epoch 결과 출력

```python
print(
    f"Epoch "
    f"{epoch + 1}/{num_epochs} | "
    f"Train Loss: {train_loss:.4f} | "
    f"Train Acc: {train_acc:.4f} | "
    f"Val Loss: {val_loss:.4f} | "
    f"Val Acc: {val_acc:.4f}"
)
```

---

# 64. Train + Validation 전체

```python
num_epochs = 5


for epoch in range(
    num_epochs
):

    # ====================
    # Train
    # ====================

    model.train()


    train_loss_sum = 0.0

    train_correct = 0

    train_count = 0


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


        batch_size = labels.size(
            0
        )


        train_loss_sum += (
            loss.item()
            * batch_size
        )


        preds = logits.argmax(
            dim=1
        )


        train_correct += (
            preds == labels
        ).sum().item()


        train_count += batch_size


    train_loss = (
        train_loss_sum
        / train_count
    )


    train_acc = (
        train_correct
        / train_count
    )


    # ====================
    # Validation
    # ====================

    model.eval()


    val_loss_sum = 0.0

    val_correct = 0

    val_count = 0


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


            loss = loss_fn(
                logits,
                labels
            )


            batch_size = labels.size(
                0
            )


            val_loss_sum += (
                loss.item()
                * batch_size
            )


            preds = logits.argmax(
                dim=1
            )


            val_correct += (
                preds == labels
            ).sum().item()


            val_count += batch_size


    val_loss = (
        val_loss_sum
        / val_count
    )


    val_acc = (
        val_correct
        / val_count
    )


    print(
        f"Epoch "
        f"{epoch + 1}/{num_epochs} | "
        f"Train Loss: {train_loss:.4f} | "
        f"Train Acc: {train_acc:.4f} | "
        f"Val Loss: {val_loss:.4f} | "
        f"Val Acc: {val_acc:.4f}"
    )
```

---

# 65. Training 결과 예

```text
Epoch 1/5
Train Loss: 0.4200
Train Acc: 0.8100
Val Loss: 0.3500
Val Acc: 0.8500

Epoch 2/5
Train Loss: 0.2800
Train Acc: 0.8900
Val Loss: 0.3000
Val Acc: 0.8800
```

이런 식으로 Epoch별 변화를 확인한다.

---

# 66. Accuracy만 보지 않는다

특히 Class Imbalance가 있다면:

```text
Precision

Recall

F1

ROC-AUC

PR-AUC

Confusion Matrix
```

도 확인한다.

---

# 67. Validation Prediction 저장

Metric을 계산하려면 Validation의 실제 Label과 Prediction을 모을 수 있다.

```python
all_labels = []

all_preds = []

all_probs = []
```

---

# 68. Validation에서 저장

```python
model.eval()


all_labels = []

all_preds = []

all_probs = []


with torch.no_grad():

    for images, labels in val_loader:

        images = images.to(
            device
        )


        logits = model(
            images
        )


        probs = torch.softmax(
            logits,
            dim=1
        )


        preds = logits.argmax(
            dim=1
        )


        all_labels.extend(
            labels.numpy()
        )


        all_preds.extend(
            preds.cpu().numpy()
        )


        all_probs.extend(
            probs[
                :,
                1
            ].cpu().numpy()
        )
```

---

# 69. 왜 cpu()?

CUDA Tensor를 NumPy Array로 바로 변환할 수 없기 때문이다.

```text
GPU Tensor
↓
.cpu()
↓
NumPy
```

이다.

---

# 70. detach는?

`torch.no_grad()` 안에서는 Gradient Graph를 만들지 않지만, 일반적인 상황에서 Tensor를 Model 계산 그래프와 분리할 때:

```python
.detach()
```

를 볼 수 있다.

---

# 71. Accuracy

```python
from sklearn.metrics import (
    accuracy_score
)


acc = accuracy_score(
    all_labels,
    all_preds
)


print(
    acc
)
```

---

# 72. Classification Report

```python
from sklearn.metrics import (
    classification_report
)


print(
    classification_report(
        all_labels,
        all_preds
    )
)
```

---

# 73. Confusion Matrix

```python
from sklearn.metrics import (
    confusion_matrix
)


cm = confusion_matrix(
    all_labels,
    all_preds
)


print(
    cm
)
```

---

# 74. Binary Confusion Matrix

```text
[[TN, FP],
 [FN, TP]]
```

이다.

---

# 75. ROC-AUC

```python
from sklearn.metrics import (
    roc_auc_score
)


auc = roc_auc_score(
    all_labels,
    all_probs
)


print(
    auc
)
```

---

# 76. ROC-AUC에는 Probability 사용

다음처럼 Class Prediction:

```text
0

1
```

만 사용하는 것이 아니라:

```text
Class 1 Probability
```

를 사용한다.

---

# 77. Softmax Probability

```python
probs = torch.softmax(
    logits,
    dim=1
)
```

---

# 78. Class 1 Probability

```python
positive_probs = probs[
    :,
    1
]
```

이다.

---

# 79. Confidence

Prediction Class의 가장 높은 Probability를 Model Confidence처럼 확인할 수 있다.

```python
confidence, preds = probs.max(
    dim=1
)
```

---

# 80. 예

```text
Prediction = 1

Probability Class 1 = 0.99
```

이면 높은 Confidence Prediction이다.

---

# 81. High-confidence Error

Model이:

```text
Prediction = 1
Confidence = 0.99
```

라고 했는데 실제:

```text
Label = 0
```

이면:

```text
High-confidence Error
```

이다.

---

# 82. 왜 중요할까?

Model이 단순히 애매해서 틀린 것이 아니라 매우 확신하면서 잘못 판단한 Case이기 때문이다.

---

# 83. Low-confidence Error

예:

```text
Prediction = 1

Probability = 0.51

True Label = 0
```

이면 Decision Boundary 근처의 Error일 수 있다.

---

# 84. Failure Case 분석

Validation 결과를 DataFrame으로 만들 수 있다.

```python
result_df = pd.DataFrame(
    {
        "true": all_labels,

        "pred": all_preds,

        "prob_1": all_probs
    }
)
```

---

# 85. Error만 선택

```python
error_df = result_df[
    result_df[
        "true"
    ]
    !=
    result_df[
        "pred"
    ]
]
```

---

# 86. Error 개수

```python
print(
    len(
        error_df
    )
)
```

---

# 87. Error를 직접 본다

Image Path도 같이 저장해두면 틀린 Image를 시각화할 수 있다.

이 과정은 Computer Vision에서 매우 중요하다.

---

# 88. Model이 왜 틀렸을까?

예:

```text
Image가 너무 어둡다

병변이 작다

Position이 다르다

Image Quality가 낮다

Border가 다르다

Text Marker가 있다

Label 자체가 의심스럽다
```

등을 확인한다.

---

# 89. Failure Analysis → Hypothesis

예:

```text
어두운 Image에서 Error가 많다
```

↓

Hypothesis:

```text
Brightness 변화에 Model이 약한 것 같다.
```

↓

Experiment:

```text
Brightness Augmentation
```

을 시도할 수 있다.

---

# 90. 실험은 이렇게 연결한다

```text
Failure Case
↓
문제 발견
↓
Hypothesis
↓
Experiment
↓
Validation 비교
```

이다.

---

# 91. Best Model 저장

Validation 성능이 가장 좋은 Model을 저장할 수 있다.

예:

```python
best_val_loss = float(
    "inf"
)
```

---

# 92. Training 중

```python
if val_loss < best_val_loss:

    best_val_loss = val_loss


    torch.save(
        model.state_dict(),
        "best_model.pt"
    )
```

---

# 93. 왜 마지막 Epoch Model만 쓰지 않을까?

예:

```text
Epoch 1
Val Loss = 0.40

Epoch 2
Val Loss = 0.30

Epoch 3
Val Loss = 0.35

Epoch 4
Val Loss = 0.45
```

이면 Epoch 2가 더 좋을 수 있다.

---

# 94. Overfitting

Training 성능은 계속 좋아지지만 Validation 성능이 나빠질 수 있다.

```text
Train Loss ↓

Val Loss ↓
        이후 ↑
```

같은 Pattern이다.

---

# 95. Best Model 기준

문제에 따라:

```text
Validation Loss

Validation Accuracy

Validation F1

Validation ROC-AUC
```

등을 사용할 수 있다.

---

# 96. 기준은 미리 정한다

여러 Metric 중 결과가 가장 좋아 보이는 것을 나중에 골라내는 방식은 Validation Overfitting으로 이어질 수 있다.

주요 Metric을 미리 정하는 것이 좋다.

---

# 97. Model 불러오기

```python
model.load_state_dict(
    torch.load(
        "best_model.pt",
        map_location=device
    )
)
```

---

# 98. Evaluation Mode

```python
model.eval()
```

---

# 99. Test Dataset

Test에는 Label이 없을 수도 있다.

예:

```text
image

test_001.png

test_002.png
```

---

# 100. Test Dataset

Label 없이:

```python
class TestImageDataset(
    Dataset
):

    def __init__(
        self,
        df,
        image_dir,
        transform=None
    ):

        self.df = (
            df.reset_index(
                drop=True
            )
        )

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

        image_name = self.df.loc[
            idx,
            "image"
        ]


        image_path = os.path.join(
            self.image_dir,
            image_name
        )


        image = Image.open(
            image_path
        ).convert(
            "RGB"
        )


        if self.transform is not None:

            image = self.transform(
                image
            )


        return (
            image,
            image_name
        )
```

---

# 101. Test DataLoader

```python
test_loader = DataLoader(

    test_dataset,

    batch_size=32,

    shuffle=False,

    num_workers=0
)
```

---

# 102. 왜 shuffle=False?

Competition이나 File별 Prediction에서는 원래 Test 순서를 유지하는 것이 중요할 수 있다.

---

# 103. Test Inference

```python
model.eval()


test_names = []

test_preds = []

test_probs = []


with torch.no_grad():

    for images, names in test_loader:

        images = images.to(
            device
        )


        logits = model(
            images
        )


        probs = torch.softmax(
            logits,
            dim=1
        )


        preds = logits.argmax(
            dim=1
        )


        test_names.extend(
            names
        )


        test_preds.extend(
            preds.cpu().numpy()
        )


        test_probs.extend(
            probs[
                :,
                1
            ].cpu().numpy()
        )
```

---

# 104. 결과 DataFrame

```python
prediction_df = pd.DataFrame(
    {
        "image": test_names,

        "prediction": test_preds,

        "probability": test_probs
    }
)
```

---

# 105. 확인

```python
print(
    prediction_df.head()
)
```

---

# 106. Test CSV와 Test Prediction은 다르다

중요하다.

```text
test.csv
```

는:

```text
Prediction해야 하는 대상의 정보
```

이다.

---

# 107. Test Prediction

Training된 Model을 Test Image에 적용해서 우리가 새로 만드는 결과이다.

```text
test.csv
↓
Test Dataset
↓
Model
↓
Prediction
```

이다.

---

# 108. Competition

Competition에서는:

```text
sample_submission.csv
```

형식에 맞게 Prediction을 저장할 수 있다.

---

# 109. 예

```python
submission = pd.read_csv(
    "sample_submission.csv"
)


submission[
    "label"
] = test_preds


submission.to_csv(
    "submission.csv",
    index=False
)
```

---

# 110. 제출 전 반드시 확인

```python
print(
    submission.head()
)


print(
    submission.shape
)


print(
    submission[
        "label"
    ].value_counts()
)
```

---

# 111. Row 순서도 중요하다

Prediction 순서와 `sample_submission.csv`의 Row 순서가 맞아야 한다.

---

# 112. Test Dataset에서 shuffle=False가 중요한 이유 중 하나다

```text
Test Row
↔
Prediction
```

연결을 유지하기 쉽다.

---

# 113. Prediction Distribution 확인

```python
prediction_df[
    "prediction"
].value_counts(
    normalize=True
)
```

---

# 114. 왜 볼까?

예:

```text
Class 0 = 0%

Class 1 = 100%
```

처럼 모든 Image를 한 Class로 Prediction했다면 문제를 의심할 수 있다.

---

# 115. Probability Distribution

```python
prediction_df[
    "probability"
].describe()
```

---

# 116. Confidence Distribution

Model이 모든 Image에서:

```text
0.99999
```

처럼 지나치게 높은 Confidence를 보이는지도 확인할 수 있다.

---

# 117. 높은 Confidence = 반드시 좋은 Model?

아니다.

Model은 틀리면서도 높은 Confidence를 가질 수 있다.

---

# 118. Calibration

Prediction Probability가 실제 정답 가능성과 얼마나 잘 맞는지 보는 개념을:

```text
Calibration
```

이라고 한다.

---

# 119. Image Classification에서 중요한 것은 정확도 하나가 아니다

전체적으로:

```text
Metric

Error

Confidence

Data Distribution

Failure Case

Explainability
```

를 함께 본다.

---

# 120. Baseline

첫 Model:

```text
Baseline
```

을 만든다.

목적:

```text
현재 Dataset에서
기본적인 Pipeline이 얼마나 동작하는가?
```

를 확인한다.

---

# 121. Baseline은 너무 복잡할 필요가 없다

```text
단순 Transform

기본 Model

기본 Loss

기본 Optimizer

고정된 Split
```

로 시작한다.

---

# 122. Baseline 이후 실험

예:

```text
Experiment A
→ Fine-tuning

Experiment B
→ Data Augmentation

Experiment C
→ Weighted Loss

Experiment D
→ Image Size 변경

Experiment E
→ 다른 Pretrained Model
```

등이다.

---

# 123. 한 번에 하나씩 변경

Baseline:

```text
Accuracy = 0.90
```

Experiment A:

```text
Fine-tuning만 변경
Accuracy = 0.93
```

라면 변화의 원인을 비교하기 쉽다.

---

# 124. 여러 개를 동시에 바꾸면?

```text
Augmentation

Learning Rate

Model

Loss

Image Size
```

를 한꺼번에 바꾸고 성능이 좋아져도 무엇이 효과가 있었는지 알기 어렵다.

---

# 125. Experiment Table

결과를 표로 정리한다.

```text
Experiment | Model | Augmentation | Loss | LR | Val Acc | F1
```

---

# 126. 재현성

같은 실험을 비교하려면 Random Seed를 고정할 수 있다.

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


if torch.cuda.is_available():

    torch.cuda.manual_seed_all(
        SEED
    )
```

---

# 127. 완전한 재현성이 항상 보장되는 것은 아니다

GPU 연산이나 Library 설정에 따라 일부 비결정성이 존재할 수 있다.

하지만 Seed를 기록하는 것은 실험 관리에 도움이 된다.

---

# 128. History 저장

```python
history = {
    "train_loss": [],
    "val_loss": [],
    "train_acc": [],
    "val_acc": []
}
```

---

# 129. Epoch마다

```python
history[
    "train_loss"
].append(
    train_loss
)


history[
    "val_loss"
].append(
    val_loss
)


history[
    "train_acc"
].append(
    train_acc
)


history[
    "val_acc"
].append(
    val_acc
)
```

---

# 130. Loss Curve

```python
plt.plot(
    history[
        "train_loss"
    ],
    label="Train Loss"
)


plt.plot(
    history[
        "val_loss"
    ],
    label="Val Loss"
)


plt.legend()

plt.show()
```

---

# 131. Accuracy Curve

```python
plt.plot(
    history[
        "train_acc"
    ],
    label="Train Acc"
)


plt.plot(
    history[
        "val_acc"
    ],
    label="Val Acc"
)


plt.legend()

plt.show()
```

---

# 132. 그래프에서 보는 것

```text
Train과 Validation이 같이 개선되는가?

Train만 좋아지는가?

Validation이 어느 Epoch부터 나빠지는가?

Training이 거의 안 되는가?
```

등이다.

---

# 133. Underfitting

Train 성능도 낮고 Validation 성능도 낮다.

가능한 원인:

```text
Model이 너무 단순

Training 부족

Learning Rate 문제

Preprocessing 문제
```

등이다.

---

# 134. Overfitting

Train은 매우 좋지만 Validation은 상대적으로 낮다.

가능한 대응:

```text
Augmentation

Weight Decay

Dropout

Early Stopping

Transfer Learning

Data 추가
```

등이다.

---

# 135. Data Leakage

Train과 Validation 모두 비정상적으로 높은 결과가 나오면 무조건 좋은 Model이라고 생각하지 않는다.

다음도 확인한다.

```text
중복 Image

같은 Patient

파일명 정보

Label이 포함된 Marker

잘못된 Split
```

---

# 136. Shortcut Learning

Model이 실제 병변 대신:

```text
Brightness

Border

Text

촬영 장비

Image Size
```

같은 쉬운 Feature를 사용할 수 있다.

---

# 137. Class별 Brightness

예:

```text
Class 0 평균 밝기 = 100

Class 1 평균 밝기 = 80
```

처럼 큰 차이가 있으면 확인할 가치가 있다.

---

# 138. 그렇다고 밝기 차이를 무조건 제거하면 안 된다

실제 질환과 관련된 Image 변화일 수도 있고 Dataset 수집 Bias일 수도 있다.

Domain Knowledge와 추가 분석이 필요하다.

---

# 139. Image Classification Workflow

```text
1. Data Load

2. Data Inspection

3. Class Distribution

4. Sample Visualization

5. Split

6. Transform

7. Dataset

8. DataLoader

9. One Batch Check

10. Model

11. Dummy Forward

12. Loss

13. Optimizer

14. One Batch Smoke Test

15. Training

16. Validation

17. Metric

18. Best Model

19. Failure Case

20. Test Inference
```

---

# 140. Full Training 전에 3단계 Test

```text
Dataset[0]

↓

next(iter(DataLoader))

↓

model(batch)
```

를 먼저 확인한다.

---

# 141. Dataset Test

```python
image, label = train_dataset[
    0
]


print(
    image.shape,
    label
)
```

---

# 142. DataLoader Test

```python
images, labels = next(
    iter(
        train_loader
    )
)


print(
    images.shape,
    labels.shape
)
```

---

# 143. Model Test

```python
images = images.to(
    device
)


logits = model(
    images
)


print(
    logits.shape
)
```

---

# 144. Loss Test

```python
labels = labels.to(
    device
)


loss = loss_fn(
    logits,
    labels
)


print(
    loss.item()
)
```

---

# 145. Backward Test

```python
optimizer.zero_grad()


loss.backward()


optimizer.step()
```

---

# 146. 여기까지 되면

```text
Image

Dataset

DataLoader

Model

Loss

Backward

Optimizer
```

가 연결된 것이다.

그 후 Full Training으로 넘어간다.

---

# 147. 자주 발생하는 오류 1

```text
FileNotFoundError
```

확인:

```text
Image Path

CSV File Name

Image Directory
```

---

# 148. 오류 2

```text
expected 3 channels
but got 1
```

확인:

```text
Image Mode

convert("RGB")

Model in_channels
```

---

# 149. 오류 3

```text
mat1 and mat2 shapes cannot be multiplied
```

확인:

```text
CNN Output Shape

Flatten Size

Linear in_features
```

---

# 150. 오류 4

```text
Expected all tensors to be on the same device
```

확인:

```text
Model Device

Image Device

Label Device
```

---

# 151. 오류 5

CrossEntropyLoss에서 Target 관련 오류가 발생한다면:

```text
Label dtype

Label Shape

Label Range

Output Class 수
```

를 확인한다.

---

# 152. CrossEntropyLoss 기본 형태

```text
logits
[B, C]

labels
[B]

labels dtype
torch.long

label value
0 ~ C-1
```

이다.

---

# 153. NaN Loss

확인:

```text
Input에 NaN / Inf?

Learning Rate가 너무 큰가?

Normalize가 이상한가?

Loss 설정이 맞는가?
```

등이다.

---

# 154. GPU Out of Memory

```text
CUDA out of memory
```

이면 먼저:

```text
Batch Size 감소
```

를 고려할 수 있다.

---

# 155. 예

```text
Batch 64
↓
32
↓
16
```

으로 줄여본다.

---

# 156. Image Size도 영향

```text
512 × 512
```

는:

```text
224 × 224
```

보다 많은 GPU Memory를 사용할 수 있다.

---

# 157. Model 크기도 영향

큰 Backbone은 더 많은 Memory를 사용할 수 있다.

---

# 158. Image Classification의 핵심

좋은 결과는 Model만으로 결정되지 않는다.

```text
Data

Split

Transform

Model

Loss

Optimizer

Evaluation
```

이 모두 연결되어 있다.

---

# 159. 특히 Validation을 믿을 수 있어야 한다

Validation Split이 잘못되어 있다면:

```text
Val Accuracy = 0.99
```

여도 실제 새로운 Data에서 같은 성능이 나온다는 보장은 없다.

---

# 160. 의료영상에서는 더욱 중요

```text
Patient-level Leakage

기관 차이

장비 차이

Label 품질

중복 Image

촬영 방식
```

등을 고려해야 한다.

---

# 161. Prediction과 Diagnosis는 같은 말이 아니다

Model이 Image Classification 결과를 출력한다고 해서 자동으로 의료적 Diagnosis를 확정하는 것은 아니다.

실제 의료 AI에서는 임상적 검증과 적절한 사용 맥락이 필요하다.

---

# 162. 핵심 코드 흐름

```python
for epoch in range(
    num_epochs
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

# 163. 이것이 Chapter 07과 Chapter 08의 연결이다

Chapter 07:

```text
Training Loop를 학습
```

Chapter 08:

```text
Training Loop의 Input을
Image로 확장
```

한 것이다.

---

# 164. 전체 흐름 다시 보기

```text
CSV
+
Image Files
↓
EDA
↓
Train / Validation Split
↓
Transform
↓
Dataset
↓
DataLoader
↓
[B, C, H, W]
↓
CNN
↓
[B, Classes]
↓
CrossEntropyLoss
↓
Backward
↓
Optimizer
↓
Validation
↓
Metrics
↓
Failure Cases
↓
Best Model
↓
Test Inference
```

---

# 165. 가장 중요한 질문

Image Classification Project를 시작하면 다음을 확인한다.

```text
무엇을 분류하는가?

Class는 몇 개인가?

Label은 정확한가?

Image Shape은?

Channel은?

Class 비율은?

어떻게 Split할 것인가?

Patient Leakage는 없는가?

어떤 Transform을 사용할 것인가?

어떤 Model을 사용할 것인가?

어떤 Metric을 기준으로 할 것인가?

어떤 Error를 중요하게 볼 것인가?
```

---

# 166. 한 줄 정리

Image Classification은 단순히:

```text
CNN을 실행하는 것
```

이 아니라:

```text
Data
→ Split
→ Preprocessing
→ Dataset
→ DataLoader
→ CNN
→ Training
→ Validation
→ Evaluation
→ Error Analysis
→ Inference
```

전체 Pipeline을 설계하고 검증하는 과정이다.

---

# 167. 다음 단계

다음 파일:

```text
data-augmentation.md
```

에서는 Training Image에:

```text
Flip

Rotation

Crop

Brightness

Contrast

Affine Transform
```

등의 변형을 적용하는 이유와 방법을 공부한다.

특히:

```text
왜 Augmentation을 사용하는가?

Train에만 적용하는 이유는?

의료 Image에서 어떤 변형을 조심해야 하는가?

Augmentation이 정말 성능을 개선했는지 어떻게 비교하는가?
```

를 정리한다.