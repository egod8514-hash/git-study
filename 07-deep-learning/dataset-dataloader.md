# Dataset & DataLoader

## 1. Dataset과 DataLoader는 왜 필요한가?

Deep Learning에서는 보통 많은 데이터를 반복해서 Model에 넣어 학습한다.

예를 들어 Training Image가:

```text
5,216장
```

있다고 하자.

이 데이터를 하나씩 직접:

```python
image1 = ...
image2 = ...
image3 = ...
```

불러오는 것은 비효율적이다.

PyTorch에서는 이를 체계적으로 처리하기 위해:

```text
Dataset
DataLoader
```

를 사용한다.

---

# 2. 전체 구조

가장 먼저 전체 흐름부터 이해한다.

```text
Raw Data
   ↓
Dataset
   ↓
DataLoader
   ↓
Batch
   ↓
Model
```

조금 더 자세히 보면:

```text
Image Files + Labels
        ↓
      Dataset
        ↓
    DataLoader
        ↓
 [32개 Image]
 [32개 Label]
        ↓
      Model
```

---

# 3. Dataset이란?

Dataset은:

```text
데이터 하나를
어떻게 가져올 것인가?
```

를 정의하는 역할을 한다.

예를 들어:

```text
0번 Sample을 요청
↓
0번 Image 불러오기
↓
0번 Label 가져오기
↓
Image와 Label 반환
```

하는 구조이다.

---

# 4. DataLoader란?

DataLoader는 Dataset에서 데이터를 가져와:

```text
Batch 단위
```

로 Model에 공급하는 역할을 한다.

예:

```text
Dataset
5000 Samples

↓

DataLoader
batch_size=32

↓

Batch 1
32 Samples

Batch 2
32 Samples

Batch 3
32 Samples

...
```

---

# 5. Dataset과 DataLoader 차이

가장 간단하게 기억하면:

```text
Dataset
→ 데이터 하나를 어떻게 가져올지 정의

DataLoader
→ Dataset에서 여러 데이터를 묶어 Batch로 공급
```

이다.

---

# 6. 음식점에 비유

Dataset을:

```text
창고
```

라고 생각해보자.

창고에는:

```text
Sample 0
Sample 1
Sample 2
Sample 3
...
```

가 있다.

DataLoader는:

```text
운반 담당
```

이다.

예:

```text
창고 Dataset
↓
32개 가져오기
↓
Model에 전달
```

한다.

---

# 7. PyTorch Dataset

PyTorch에서는:

```python
from torch.utils.data import Dataset
```

을 사용할 수 있다.

---

# 8. 기본 Dataset 구조

직접 Dataset을 만들 때 일반적으로:

```python
class MyDataset(Dataset):

    def __init__(self):
        pass

    def __len__(self):
        pass

    def __getitem__(self, idx):
        pass
```

형태를 사용한다.

처음 보면 복잡해 보이지만 핵심은 세 부분이다.

```text
__init__

__len__

__getitem__
```

---

# 9. __init__

```python
def __init__(self):
```

Dataset을 처음 만들 때 필요한 정보를 준비한다.

예:

```text
CSV 읽기

Image 경로 저장

Label 저장

Transform 저장
```

등을 할 수 있다.

---

# 10. __len__

```python
def __len__(self):
```

Dataset에 Sample이 총 몇 개인지 알려준다.

예:

```python
def __len__(self):

    return len(
        self.data
    )
```

---

# 11. Dataset에 5,216개가 있다면

```python
len(
    dataset
)
```

결과:

```text
5216
```

이 된다.

---

# 12. __getitem__

가장 중요한 부분이다.

```python
def __getitem__(
    self,
    idx
):
```

특정 Index의 Sample을 어떻게 가져올지 정의한다.

---

# 13. 예

```python
def __getitem__(
    self,
    idx
):

    x = self.x[idx]
    y = self.y[idx]

    return x, y
```

---

# 14. 의미

Dataset에:

```python
dataset[0]
```

을 요청하면 내부적으로:

```python
__getitem__(
    0
)
```

이 호출된다.

---

# 15. 즉

```text
dataset[0]
↓
0번째 Input
+
0번째 Label
↓
return
```

이다.

---

# 16. 간단한 Custom Dataset

```python
import torch

from torch.utils.data import Dataset


class SimpleDataset(Dataset):

    def __init__(
        self,
        x,
        y
    ):

        self.x = x
        self.y = y

    def __len__(
        self
    ):

        return len(
            self.x
        )

    def __getitem__(
        self,
        idx
    ):

        return (
            self.x[idx],
            self.y[idx]
        )
```

---

# 17. 데이터 준비

```python
x = torch.tensor(
    [
        [1.0, 2.0],
        [3.0, 4.0],
        [5.0, 6.0],
        [7.0, 8.0]
    ]
)


y = torch.tensor(
    [
        0,
        1,
        0,
        1
    ]
)
```

---

# 18. Dataset 만들기

```python
dataset = SimpleDataset(
    x,
    y
)
```

---

# 19. Dataset 크기 확인

```python
print(
    len(
        dataset
    )
)
```

결과:

```text
4
```

---

# 20. 첫 번째 Sample

```python
print(
    dataset[0]
)
```

결과는 개념적으로:

```text
(
    tensor([1., 2.]),
    tensor(0)
)
```

이다.

---

# 21. 두 번째 Sample

```python
print(
    dataset[1]
)
```

개념적으로:

```text
(
    tensor([3., 4.]),
    tensor(1)
)
```

이다.

---

# 22. 핵심

Dataset 자체는 기본적으로:

```text
Sample 하나
```

를 가져오는 방법을 정의한다.

하지만 실제 Training에서는:

```text
32개

64개

128개
```

등을 묶어서 처리하는 경우가 많다.

여기서 DataLoader가 필요하다.

---

# 23. DataLoader Import

```python
from torch.utils.data import DataLoader
```

---

# 24. 가장 기본적인 DataLoader

```python
loader = DataLoader(
    dataset,
    batch_size=2
)
```

---

# 25. 의미

Dataset:

```text
4 Samples
```

Batch Size:

```text
2
```

이면:

```text
Batch 1
→ Sample 0, 1

Batch 2
→ Sample 2, 3
```

처럼 처리할 수 있다.

---

# 26. DataLoader 반복

```python
for batch_x, batch_y in loader:

    print(
        batch_x
    )

    print(
        batch_y
    )
```

---

# 27. DataLoader가 해주는 일

Dataset에서는:

```text
x 한 개
y 한 개
```

가 반환되었다.

DataLoader는 여러 Sample을 모아서:

```text
batch_x
batch_y
```

를 만들어준다.

---

# 28. Shape 확인

```python
for batch_x, batch_y in loader:

    print(
        batch_x.shape
    )

    print(
        batch_y.shape
    )
```

예:

```text
batch_x
[2, 2]

batch_y
[2]
```

이다.

---

# 29. Shape 해석

```text
batch_x = [2, 2]

첫 번째 2
→ Batch Size

두 번째 2
→ Feature 수
```

이다.

---

# 30. Batch Size란?

한 번의 Model 연산에 사용하는 Sample 개수이다.

예:

```python
batch_size = 32
```

이면:

```text
한 번에 32 Samples
```

를 Model에 넣는다.

---

# 31. 왜 전체 데이터를 한 번에 넣지 않을까?

Dataset이 매우 크면:

```text
Memory 부족

GPU Memory 부족

연산 부담
```

이 발생할 수 있다.

그래서 데이터를 작은 Batch로 나눈다.

---

# 32. Mini-batch Training

Deep Learning에서 일반적으로 사용하는 방식이다.

```text
전체 Training Data
↓
작은 Batch들로 나눔
↓
Batch마다 Weight Update
```

한다.

---

# 33. 예

Training Data:

```text
1000 Samples
```

Batch Size:

```text
100
```

이면 대략:

```text
10 Batches
```

이다.

---

# 34. Batch마다 학습

```text
Batch 1
↓
Forward
↓
Loss
↓
Backward
↓
Update

Batch 2
↓
Forward
↓
Loss
↓
Backward
↓
Update

...
```

한다.

---

# 35. 모든 Batch를 처리하면?

Training Dataset 전체를 한 번 처리한 것이다.

이것을:

```text
1 Epoch
```

이라고 한다.

---

# 36. Batch와 Epoch 관계

```text
Dataset
1000 Samples

Batch Size
100

↓

약 10 Batch

↓

10 Batch 처리

↓

1 Epoch
```

이다.

---

# 37. 마지막 Batch

Dataset 크기가 Batch Size로 정확히 나누어지지 않을 수 있다.

예:

```text
Dataset = 100

Batch Size = 32
```

이면:

```text
Batch 1 = 32
Batch 2 = 32
Batch 3 = 32
Batch 4 = 4
```

가 될 수 있다.

---

# 38. 그래서 Batch Size가 항상 같지는 않을 수 있다

특히 마지막 Batch는:

```text
더 작을 수 있다.
```

이 점이 중요하다.

---

# 39. drop_last

마지막 작은 Batch를 버리고 싶다면:

```python
loader = DataLoader(
    dataset,
    batch_size=32,
    drop_last=True
)
```

를 사용할 수 있다.

---

# 40. drop_last=False

기본적으로:

```python
drop_last=False
```

이다.

즉 마지막 작은 Batch도 사용한다.

---

# 41. drop_last=True는 언제?

특정 Model이나 Batch 단위 처리에서 고정된 Batch 크기가 꼭 필요할 때 사용할 수 있다.

하지만 데이터를 버리는 것이므로 무조건 사용하는 것은 아니다.

---

# 42. shuffle

Training Data에서는:

```python
shuffle=True
```

를 자주 사용한다.

예:

```python
train_loader = DataLoader(
    train_dataset,
    batch_size=32,
    shuffle=True
)
```

---

# 43. shuffle=True 의미

Epoch마다 Dataset 순서를 섞어서 Batch를 구성한다.

예:

원래:

```text
0
1
2
3
4
5
```

Shuffle 후:

```text
3
0
5
1
4
2
```

처럼 순서가 바뀔 수 있다.

---

# 44. 왜 Training에서 섞을까?

데이터 순서에 의해 Model이 불필요한 Pattern을 학습하는 것을 줄이고 Training을 안정적으로 만드는 데 도움이 될 수 있다.

---

# 45. Validation에서는?

일반적으로:

```python
valid_loader = DataLoader(
    valid_dataset,
    batch_size=32,
    shuffle=False
)
```

처럼 사용한다.

---

# 46. 왜 Validation은 shuffle=False?

Validation에서는 Weight를 학습하지 않는다.

따라서 반드시 섞을 필요가 없고 원래 순서를 유지하면 Prediction과 원본 Sample을 대응시키기 편하다.

---

# 47. Test도 일반적으로

```python
test_loader = DataLoader(
    test_dataset,
    batch_size=32,
    shuffle=False
)
```

를 많이 사용한다.

---

# 48. 기본적인 Loader 구성

```python
train_loader = DataLoader(
    train_dataset,
    batch_size=32,
    shuffle=True
)


valid_loader = DataLoader(
    valid_dataset,
    batch_size=32,
    shuffle=False
)


test_loader = DataLoader(
    test_dataset,
    batch_size=32,
    shuffle=False
)
```

---

# 49. 한 Batch만 가져오기

전체 반복문을 돌지 않고 첫 Batch만 확인할 수 있다.

```python
x, y = next(
    iter(
        train_loader
    )
)
```

---

# 50. iter()

```python
iter(
    train_loader
)
```

는 DataLoader에서 순서대로 Batch를 꺼낼 수 있는 Iterator를 만든다.

---

# 51. next()

```python
next(...)
```

는 그 Iterator에서 다음 Batch 하나를 가져온다.

---

# 52. 따라서

```python
x, y = next(
    iter(
        train_loader
    )
)
```

는:

```text
train_loader에서
Batch 하나만 가져와서

x
y

에 저장
```

한다.

---

# 53. Shape 확인

```python
print(
    x.shape
)


print(
    y.shape
)
```

이 작업은 Training 시작 전에 매우 중요하다.

---

# 54. 이미지 예

결과:

```text
x
torch.Size([32, 3, 224, 224])

y
torch.Size([32])
```

라면:

```text
32 Images

3 Channels

224 × 224

32 Labels
```

이다.

---

# 55. Training 전 꼭 확인

```python
images, labels = next(
    iter(
        train_loader
    )
)


print(
    "images:",
    images.shape
)


print(
    "labels:",
    labels.shape
)


print(
    "image dtype:",
    images.dtype
)


print(
    "label dtype:",
    labels.dtype
)
```

---

# 56. Label 확인

```python
print(
    labels
)
```

예:

```text
tensor([
    1, 0, 1, 1, 0, ...
])
```

처럼 실제 Label이 정상인지 확인한다.

---

# 57. Unique Label

```python
print(
    torch.unique(
        labels
    )
)
```

Binary Classification이라면 예:

```text
tensor([0, 1])
```

이 나올 수 있다.

---

# 58. DataLoader 길이

```python
print(
    len(
        train_loader
    )
)
```

은 일반적으로:

```text
Batch 개수
```

를 의미한다.

Dataset Sample 수와 혼동하면 안 된다.

---

# 59. 예

Dataset:

```text
1000 Samples
```

Batch Size:

```text
100
```

이면:

```python
len(
    train_dataset
)
```

은:

```text
1000
```

이고:

```python
len(
    train_loader
)
```

는 대략:

```text
10
```

이다.

---

# 60. Dataset과 DataLoader len 차이

```text
len(dataset)
→ Sample 수

len(loader)
→ Batch 수
```

이다.

---

# 61. Image Dataset은 어떻게 만들까?

Image Classification에서는 보통:

```text
Image Path
+
Label
```

을 Dataset이 관리한다.

예:

```text
train_0001.png → 0
train_0002.png → 1
train_0003.png → 1
```

---

# 62. CSV가 있는 경우

예:

```text
id,label

train_0001,0
train_0002,1
train_0003,1
```

Dataset의 `__getitem__()`에서:

```text
CSV에서 Image 이름 확인

↓

Image File 열기

↓

Transform 적용

↓

Label 가져오기

↓

Image Tensor, Label 반환
```

할 수 있다.

---

# 63. Image Custom Dataset 기본 구조

```python
from torch.utils.data import Dataset

from PIL import Image


class ImageDataset(Dataset):

    def __init__(
        self,
        dataframe,
        image_dir,
        transform=None
    ):

        self.dataframe = dataframe
        self.image_dir = image_dir
        self.transform = transform

    def __len__(
        self
    ):

        return len(
            self.dataframe
        )

    def __getitem__(
        self,
        idx
    ):

        ...
```

---

# 64. __init__에서 저장

```python
self.dataframe = dataframe

self.image_dir = image_dir

self.transform = transform
```

한다.

즉 Dataset 객체가 사용할 정보를 기억해둔다.

---

# 65. Image Path 만들기

예:

```python
import os


image_name = self.dataframe.iloc[
    idx
]["id"]


image_path = os.path.join(
    self.image_dir,
    image_name
)
```

실제 파일 확장자와 CSV 구조에 맞게 작성해야 한다.

---

# 66. Image 열기

```python
image = Image.open(
    image_path
)
```

---

# 67. RGB 변환

일반 RGB Model이라면:

```python
image = image.convert(
    "RGB"
)
```

를 사용할 수 있다.

---

# 68. 흑백 이미지라면

```python
image = image.convert(
    "L"
)
```

을 사용할 수도 있다.

하지만 Pretrained Model이 3 Channel Input을 기대한다면 RGB 형태로 변환하는 전략을 사용할 수도 있다.

모델 구조와 데이터 의미에 맞춰 결정한다.

---

# 69. Label 가져오기

```python
label = self.dataframe.iloc[
    idx
]["label"]
```

---

# 70. Transform 적용

```python
if self.transform:

    image = self.transform(
        image
    )
```

---

# 71. 반환

```python
return (
    image,
    label
)
```

한다.

---

# 72. 전체 예

```python
import os

from PIL import Image

from torch.utils.data import Dataset


class ImageDataset(Dataset):

    def __init__(
        self,
        dataframe,
        image_dir,
        transform=None
    ):

        self.dataframe = dataframe.reset_index(
            drop=True
        )

        self.image_dir = image_dir
        self.transform = transform

    def __len__(
        self
    ):

        return len(
            self.dataframe
        )

    def __getitem__(
        self,
        idx
    ):

        image_name = self.dataframe.iloc[
            idx
        ]["id"]

        label = self.dataframe.iloc[
            idx
        ]["label"]

        image_path = os.path.join(
            self.image_dir,
            image_name
        )

        image = Image.open(
            image_path
        ).convert(
            "RGB"
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

# 73. reset_index(drop=True)는 왜?

Train / Validation Split 이후 DataFrame Index가:

```text
0
3
7
11
...
```

처럼 끊겨 있을 수 있다.

`iloc`는 위치 기반이라 꼭 필요하지 않은 경우도 있지만 Dataset 내부 Index를 깔끔하게 관리하기 위해:

```python
reset_index(
    drop=True
)
```

를 자주 사용한다.

---

# 74. Transform이란?

Image를 Tensor로 변환하거나 크기를 변경하거나 Augmentation을 적용하는 과정이다.

예:

```text
Resize

RandomFlip

Rotation

ToTensor

Normalize
```

등이 있다.

---

# 75. torchvision transforms

예:

```python
from torchvision import transforms


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

# 76. Compose

```python
transforms.Compose()
```

는 여러 Transform을 순서대로 연결한다.

```text
Image
↓
Resize
↓
ToTensor
```

이다.

---

# 77. ToTensor

```python
transforms.ToTensor()
```

는 PIL Image나 NumPy Image를 PyTorch Tensor 형태로 변환하는 데 사용된다.

이미지 배열 순서도 일반적으로:

```text
H × W × C

↓

C × H × W
```

형태로 바뀐다.

---

# 78. Pixel 값

일반적인 8-bit Image에 `ToTensor()`를 적용하면 보통 Pixel 값이:

```text
0 ~ 255

↓

0.0 ~ 1.0
```

범위의 Float Tensor로 변환된다.

데이터 입력 형식에 따라 동작을 확인해야 한다.

---

# 79. Dataset 만들기

```python
train_dataset = ImageDataset(
    train_df,
    train_image_dir,
    transform=transform
)
```

---

# 80. DataLoader 만들기

```python
train_loader = DataLoader(
    train_dataset,
    batch_size=32,
    shuffle=True
)
```

---

# 81. 한 Batch 확인

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

# 82. Model 연결

```python
outputs = model(
    images
)
```

단, Model과 Images가 같은 Device에 있어야 한다.

---

# 83. GPU 사용

```python
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

# 84. Training Loop 연결

```python
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

이제 Dataset과 DataLoader가 실제 Training Loop와 연결된다.

---

# 85. 전체 흐름

```text
Image Files
+
CSV Labels
       ↓
ImageDataset
       ↓
DataLoader
       ↓
images, labels
       ↓
GPU
       ↓
Model
       ↓
outputs
       ↓
Loss
       ↓
Backward
       ↓
Optimizer
```

---

# 86. num_workers

DataLoader에서 자주 보는 옵션:

```python
num_workers=0
```

이다.

---

# 87. num_workers 의미

데이터를 준비하는 작업에 사용할 Worker Process 수와 관련된 옵션이다.

값을 늘리면 Data Loading을 병렬화하여 속도가 좋아질 수 있다.

하지만 환경에 따라 문제가 발생할 수도 있다.

---

# 88. 초보 단계에서는

특히 Windows나 Notebook에서 문제가 발생한다면:

```python
num_workers=0
```

부터 시작하는 것이 디버깅하기 편하다.

---

# 89. 예

```python
train_loader = DataLoader(
    train_dataset,
    batch_size=32,
    shuffle=True,
    num_workers=0
)
```

---

# 90. num_workers를 무조건 높이면 안 된다

더 높은 값이 항상 더 빠른 것은 아니다.

영향을 받는 요소:

```text
CPU

Storage

Dataset 처리량

Image Transform

OS

RAM

GPU 속도
```

등이 있다.

실험으로 결정한다.

---

# 91. pin_memory

GPU Training 코드에서:

```python
pin_memory=True
```

를 볼 수 있다.

CPU Memory에서 CUDA GPU로 데이터를 전송할 때 성능에 도움이 될 수 있는 옵션이다.

---

# 92. 예

```python
train_loader = DataLoader(
    train_dataset,
    batch_size=32,
    shuffle=True,
    num_workers=0,
    pin_memory=True
)
```

GPU 사용 환경에서 고려할 수 있다.

항상 성능이 좋아진다고 단정할 수는 없다.

---

# 93. GPU 여부에 따라

```python
pin_memory = (
    device.type == "cuda"
)
```

처럼 설정할 수도 있다.

---

# 94. Batch Size는 클수록 좋은가?

아니다.

Batch Size가 커지면:

```text
GPU Memory 사용 증가
```

한다.

너무 크면:

```text
CUDA out of memory
```

오류가 발생할 수 있다.

---

# 95. 작은 Batch

장점:

```text
GPU Memory 사용 감소
```

하지만 Batch가 너무 작으면 Training 효율이나 학습 특성이 달라질 수 있다.

---

# 96. 적절한 Batch Size

예:

```text
16
32
64
128
```

등을 실험할 수 있다.

하지만 적절한 값은:

```text
Model

Image Size

GPU Memory

Dataset

Training 방식
```

에 따라 다르다.

---

# 97. CUDA Out of Memory

대표적인 오류:

```text
CUDA out of memory
```

가 발생한다면 가장 먼저 시도할 수 있는 방법 중 하나는:

```text
Batch Size 감소
```

이다.

예:

```text
64
↓
32
↓
16
```

---

# 98. shuffle과 Random Seed

재현성을 중요하게 관리한다면 Random Seed 설정도 고려해야 한다.

예:

```python
generator = torch.Generator()

generator.manual_seed(
    42
)
```

그리고:

```python
train_loader = DataLoader(
    train_dataset,
    batch_size=32,
    shuffle=True,
    generator=generator
)
```

처럼 사용할 수 있다.

완전한 재현성은 DataLoader뿐 아니라 여러 설정의 영향을 받을 수 있다.

---

# 99. Validation Dataset에는 Training Augmentation을 그대로 적용할까?

보통은 구분한다.

Training:

```text
Resize

Random Flip

Random Rotation

ToTensor

Normalize
```

Validation:

```text
Resize

ToTensor

Normalize
```

처럼 Random Augmentation은 Training에만 적용하는 경우가 많다.

---

# 100. 왜?

Validation은:

```text
Model의 일반화 성능을
일관된 조건에서 평가
```

하기 위한 데이터이기 때문이다.

---

# 101. Train Transform 예

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

        transforms.ToTensor()
    ]
)
```

단, Horizontal Flip이 의미적으로 타당한 데이터인지 확인해야 한다.

---

# 102. Validation Transform

```python
valid_transform = transforms.Compose(
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

# 103. Dataset도 따로 생성

```python
train_dataset = ImageDataset(
    train_df,
    image_dir,
    transform=train_transform
)


valid_dataset = ImageDataset(
    valid_df,
    image_dir,
    transform=valid_transform
)
```

---

# 104. DataLoader도 따로 생성

```python
train_loader = DataLoader(
    train_dataset,
    batch_size=32,
    shuffle=True
)


valid_loader = DataLoader(
    valid_dataset,
    batch_size=32,
    shuffle=False
)
```

---

# 105. Dataset Split과 Dataset은 다른 개념

```text
Train / Validation Split
```

은 어떤 Sample을 어느 집합에 넣을지를 결정한다.

```text
Dataset Class
```

는 그 Sample을 실제로 어떻게 읽어올지 결정한다.

서로 다른 개념이다.

---

# 106. 먼저 Split

예:

```python
from sklearn.model_selection import train_test_split


train_df, valid_df = train_test_split(
    df,
    test_size=0.2,
    random_state=42,
    stratify=df["label"]
)
```

---

# 107. 그 다음 Dataset

```python
train_dataset = ImageDataset(
    train_df,
    image_dir,
    train_transform
)


valid_dataset = ImageDataset(
    valid_df,
    image_dir,
    valid_transform
)
```

---

# 108. 그 다음 DataLoader

```python
train_loader = DataLoader(
    train_dataset,
    batch_size=32,
    shuffle=True
)


valid_loader = DataLoader(
    valid_dataset,
    batch_size=32,
    shuffle=False
)
```

---

# 109. 전체 순서

```text
DataFrame
↓
Train / Validation Split
↓
Dataset 생성
↓
Transform 연결
↓
DataLoader 생성
↓
Training
```

이다.

---

# 110. Stratify

Classification에서 Class 비율을 유지하고 싶다면:

```python
stratify=df["label"]
```

을 사용할 수 있다.

단, 환자 단위 중복이나 시간 순서가 중요한 데이터라면 단순 Stratified Random Split보다 Group/Time 구조를 먼저 고려해야 한다.

---

# 111. Image Leakage 주의

같은 환자의 여러 Image가 있다면:

```text
같은 환자 Image 일부 → Train

같은 환자 Image 일부 → Validation
```

으로 들어가면 Model이 환자 특성을 공유하여 평가가 과도하게 좋아질 수 있다.

---

# 112. 따라서 의료영상에서는

가능하면:

```text
Patient-level Split
```

을 고려한다.

즉 같은 환자의 Image는 한 Split에만 들어가도록 관리한다.

---

# 113. Augmented Image Leakage

원본 Image와 그 Augmentation 결과를 미리 만들어 놓은 경우:

```text
원본 → Train

Augmented Copy → Validation
```

처럼 나뉘면 Leakage가 발생할 수 있다.

---

# 114. 안전한 방식

먼저:

```text
원본 데이터 기준 Split
```

을 하고:

```text
Train Dataset에서만
Random Augmentation
```

을 적용하는 방식이 일반적으로 안전하다.

---

# 115. TensorDataset

이미 Tensor 형태로 X와 y가 준비되어 있다면 간단한 Dataset을 만들 수 있다.

```python
from torch.utils.data import TensorDataset


dataset = TensorDataset(
    x,
    y
)
```

---

# 116. DataLoader 연결

```python
loader = DataLoader(
    dataset,
    batch_size=32,
    shuffle=True
)
```

---

# 117. 언제 편리한가?

Tabular Data처럼 이미:

```text
X Tensor

y Tensor
```

가 메모리에 준비되어 있다면 `TensorDataset`이 간단하다.

---

# 118. Custom Dataset은 언제?

다음과 같은 경우 Custom Dataset이 유용하다.

```text
Image File을 매번 읽어야 함

CSV와 Image를 연결해야 함

복잡한 Preprocessing 필요

여러 Input을 반환해야 함

Metadata와 Image를 함께 사용
```

등이다.

---

# 119. Dataset이 꼭 x, y만 반환해야 하나?

아니다.

필요하다면:

```python
return (
    image,
    label,
    patient_id
)
```

처럼 여러 값을 반환할 수도 있다.

---

# 120. 그러면 DataLoader도

```python
for image, label, patient_id in loader:

    ...
```

처럼 받을 수 있다.

---

# 121. Dictionary 반환도 가능

```python
return {
    "image": image,
    "label": label,
    "id": image_id
}
```

처럼 만들 수도 있다.

---

# 122. DataLoader에서는

```python
for batch in loader:

    images = batch["image"]
    labels = batch["label"]
```

처럼 사용할 수 있다.

---

# 123. 어떤 방식이 좋을까?

단순한 프로젝트:

```text
return image, label
```

이 이해하기 쉽다.

복잡한 프로젝트:

```text
dict
```

구조가 관리하기 편할 수 있다.

---

# 124. Test Dataset

Competition Test Data에는 Label이 없을 수 있다.

그러면 Dataset에서:

```python
return image
```

또는:

```python
return image, image_id
```

를 반환할 수 있다.

---

# 125. Test Dataset 예

```python
def __getitem__(
    self,
    idx
):

    image = ...

    image_id = ...

    return (
        image,
        image_id
    )
```

---

# 126. Test Loader

```python
test_loader = DataLoader(
    test_dataset,
    batch_size=32,
    shuffle=False
)
```

---

# 127. 왜 Test는 shuffle=False가 중요한가?

Competition에서는 Prediction 순서가:

```text
sample_submission
```

순서와 맞아야 할 수 있다.

따라서 원본 순서를 유지하거나 ID를 명시적으로 함께 관리하는 것이 안전하다.

---

# 128. Test Prediction

```python
model.eval()


predictions = []


with torch.no_grad():

    for images in test_loader:

        images = images.to(
            device
        )

        outputs = model(
            images
        )

        ...
```

처럼 사용할 수 있다.

---

# 129. Test에 Label이 없다

Competition의:

```text
test.csv
```

는 정답을 맞히기 위한 데이터가 아니라:

```text
학습된 Model이
Prediction을 만들어야 하는 데이터
```

이다.

실제 정답은 대회 운영 측에서 가지고 있다.

---

# 130. DataLoader Error가 발생하면

가장 먼저 확인한다.

```text
Dataset 자체가 정상인가?

dataset[0]이 되는가?

len(dataset)이 정상인가?

Image Path가 맞는가?

Transform이 정상인가?

Label dtype이 맞는가?
```

---

# 131. 첫 번째 Debugging

DataLoader를 만들기 전에:

```python
print(
    len(
        train_dataset
    )
)


sample = train_dataset[0]


print(
    sample
)
```

을 확인한다.

---

# 132. 이미지라면

전체 Tensor를 출력하기보다:

```python
image, label = train_dataset[0]


print(
    image.shape
)


print(
    label
)
```

처럼 확인한다.

---

# 133. 그 다음 DataLoader

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

을 확인한다.

---

# 134. 즉 Debugging 순서

```text
원본 File 확인

↓

Dataset 1 Sample 확인

↓

DataLoader 1 Batch 확인

↓

Model Forward 확인

↓

Loss 확인

↓

Backward 확인

↓

전체 Training
```

이다.

---

# 135. 이 순서가 중요한 이유

바로 전체 Training을 실행하면 오류가 어디에서 발생했는지 찾기 어렵다.

반면 한 단계씩 확인하면:

```text
File 문제

Dataset 문제

DataLoader 문제

Model 문제

Loss 문제
```

를 구분하기 쉽다.

---

# 136. FileNotFoundError

Custom Dataset에서 자주 발생할 수 있다.

예:

```text
FileNotFoundError
```

이라면:

```python
print(
    image_path
)
```

부터 확인한다.

---

# 137. 실제 파일 존재 확인

```python
print(
    os.path.exists(
        image_path
    )
)
```

결과:

```text
True
```

인지 확인한다.

---

# 138. CSV 이름과 실제 파일명

예:

CSV:

```text
train_0001
```

실제 File:

```text
train_0001.png
```

라면 확장자를 붙여야 한다.

```python
image_name = (
    image_id
    + ".png"
)
```

---

# 139. 절대 경로와 상대 경로

예:

```text
/content/train_images
```

또는:

```text
C:\Users\user\project\train_images
```

처럼 실행 환경에 따라 경로가 다르다.

경로 오류가 나면 추측하지 말고:

```python
import os


print(
    os.getcwd()
)


print(
    os.listdir()
)
```

등으로 실제 위치를 확인한다.

---

# 140. DataLoader에서 오류가 난다고 항상 DataLoader 문제는 아니다

DataLoader는 내부적으로:

```python
dataset[idx]
```

를 호출한다.

따라서 DataLoader 반복 중 오류가 발생해도 실제 원인은:

```text
__getitem__

Image Path

Transform

Label
```

일 수 있다.

---

# 141. 그래서 num_workers=0이 디버깅에 편하다

Worker Process를 여러 개 사용하면 오류 메시지가 복잡해질 수 있다.

처음에는:

```python
num_workers=0
```

으로 문제를 확인한 뒤 필요하면 늘린다.

---

# 142. Training DataLoader 핵심 옵션

```python
train_loader = DataLoader(
    train_dataset,
    batch_size=32,
    shuffle=True,
    num_workers=0
)
```

가 가장 기본적인 형태이다.

---

# 143. Validation DataLoader

```python
valid_loader = DataLoader(
    valid_dataset,
    batch_size=32,
    shuffle=False,
    num_workers=0
)
```

---

# 144. Test DataLoader

```python
test_loader = DataLoader(
    test_dataset,
    batch_size=32,
    shuffle=False,
    num_workers=0
)
```

---

# 145. Training Loop와 최종 연결

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

# 146. 여기서 DataLoader의 역할

이 코드에서:

```python
for images, labels in train_loader:
```

이 한 줄이:

```text
Dataset에서 Sample 가져오기

↓

Batch 만들기

↓

images와 labels로 전달
```

을 담당한다.

---

# 147. DataLoader가 없었다면?

직접:

```text
32개 Sample 선택

Image 읽기

Tensor 변환

하나의 Batch로 합치기

다음 32개 선택
```

등을 계속 구현해야 한다.

DataLoader가 이를 편리하게 처리해준다.

---

# 148. 전체 학습 구조 다시 보기

```text
Raw Data
   ↓
Train / Validation Split
   ↓
Dataset
   ↓
Transform
   ↓
DataLoader
   ↓
Batch
   ↓
Device
   ↓
Model
   ↓
Loss
   ↓
Backward
   ↓
Optimizer
```

---

# 149. Dataset의 핵심 질문

Dataset을 보면:

```text
__init__에서 무엇을 저장하는가?

__len__은 무엇을 반환하는가?

__getitem__은 무엇을 반환하는가?
```

를 확인한다.

---

# 150. DataLoader의 핵심 질문

DataLoader를 보면:

```text
어떤 Dataset인가?

Batch Size는 몇인가?

shuffle은 True인가?

num_workers는 몇인가?

drop_last를 사용하는가?
```

를 확인한다.

---

# 151. Training 시작 전 체크

```python
print(
    "Dataset size:",
    len(
        train_dataset
    )
)


print(
    "Loader batches:",
    len(
        train_loader
    )
)


images, labels = next(
    iter(
        train_loader
    )
)


print(
    "Image shape:",
    images.shape
)


print(
    "Label shape:",
    labels.shape
)


print(
    "Image dtype:",
    images.dtype
)


print(
    "Label dtype:",
    labels.dtype
)
```

---

# 152. 이미지라면 추가 확인

```python
print(
    "Image min:",
    images.min().item()
)


print(
    "Image max:",
    images.max().item()
)
```

---

# 153. Label 분포는 DataFrame에서도 확인

```python
print(
    train_df[
        "label"
    ].value_counts()
)
```

Validation:

```python
print(
    valid_df[
        "label"
    ].value_counts()
)
```

Classification에서는 Split 후 Class 비율도 확인한다.

---

# 154. Dataset과 DataLoader를 이해해야 하는 이유

Deep Learning 코드에서 매우 자주 다음을 보게 된다.

```python
for x, y in train_loader:
```

이 한 줄을 이해하려면:

```text
Dataset

__getitem__

Batch

DataLoader
```

관계를 알아야 한다.

---

# 155. 가장 중요한 관계

```text
Dataset
→ Sample 단위

DataLoader
→ Batch 단위
```

이다.

---

# 156. 가장 중요한 메서드

Dataset:

```text
__init__
→ 초기 설정

__len__
→ Sample 수

__getitem__
→ Sample 하나 반환
```

이다.

---

# 157. 가장 중요한 DataLoader 옵션

```text
batch_size
→ Batch 크기

shuffle
→ 데이터 순서 섞기

num_workers
→ 데이터 로딩 Worker 수

drop_last
→ 마지막 작은 Batch 제거 여부

pin_memory
→ GPU 전송 최적화에 사용할 수 있는 옵션
```

이다.

---

# 158. 가장 중요한 Debugging 순서

```text
dataset[0]
↓
next(iter(loader))
↓
model(batch)
↓
loss
↓
backward
↓
전체 Training
```

이다.

---

# 159. 한 Batch Smoke Test

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


outputs = model(
    images
)


loss = loss_fn(
    outputs,
    labels
)


print(
    "Input:",
    images.shape
)


print(
    "Output:",
    outputs.shape
)


print(
    "Target:",
    labels.shape
)


print(
    "Loss:",
    loss.item()
)
```

여기까지 정상이라면 DataLoader → Model → Loss 연결은 기본적으로 동작하고 있는 것이다.

---

# 160. 최종 정리

PyTorch의 데이터 처리 흐름은:

```text
Raw Data
↓
Dataset
↓
DataLoader
↓
Batch
↓
Model
```

이다.

Dataset은:

```text
데이터 하나를
어떻게 가져올지
```

정의한다.

DataLoader는:

```text
Dataset의 여러 Sample을
Batch로 묶어
Model에 공급
```

한다.

가장 중요한 차이는:

```text
Dataset
→ Sample

DataLoader
→ Batch
```

이다.

Custom Dataset의 핵심은:

```python
__init__()

__len__()

__getitem__()
```

이다.

Training에서는 보통:

```python
train_loader = DataLoader(
    train_dataset,
    batch_size=32,
    shuffle=True
)
```

Validation에서는:

```python
valid_loader = DataLoader(
    valid_dataset,
    batch_size=32,
    shuffle=False
)
```

처럼 사용한다.

그리고 전체 Training을 시작하기 전에 반드시:

```python
images, labels = next(
    iter(
        train_loader
    )
)
```

로 한 Batch를 확인하는 습관을 들이는 것이 좋다.

Deep Learning에서 DataLoader를 이해하면:

```python
for images, labels in train_loader:
```

라는 코드가 단순히 반복문이 아니라:

```text
Dataset에서 데이터를 읽고
↓
Batch를 만들고
↓
Model에 전달하기 위한 데이터를 공급하는 과정
```

이라는 것을 이해할 수 있다.