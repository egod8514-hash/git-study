# Image Data Basic

## 1. Image도 결국 Data다

사람이 Image를 보면:

```text
사람

강아지

자동차

폐 X-ray
```

처럼 의미를 이해한다.

하지만 Computer가 Image를 처리할 때는 기본적으로:

```text
숫자의 배열
```

로 처리한다.

즉:

```text
Image
↓
Pixel
↓
Number
↓
Array
↓
Tensor
```

로 연결된다.

---

# 2. Pixel이란?

Pixel:

```text
Picture Element
```

Image를 구성하는 작은 단위이다.

예를 들어:

```text
3 × 3
```

Grayscale Image가 있다고 하자.

```text
0    30    60

90   120   150

180  220   255
```

각 숫자 하나가 Pixel의 밝기를 나타낼 수 있다.

---

# 3. Grayscale Image

Grayscale Image는 색상보다는 밝기 정보를 표현한다.

일반적인 8-bit Image에서는 Pixel 값이:

```text
0 ~ 255
```

범위를 갖는 경우가 많다.

---

# 4. Pixel 값

일반적으로:

```text
0
→ 검정

255
→ 흰색
```

그 사이:

```text
128
→ 중간 정도 밝기
```

처럼 이해할 수 있다.

---

# 5. 8-bit

8-bit로 표현할 수 있는 값의 개수:

```text
2^8
=
256
```

따라서:

```text
0 ~ 255
```

총 256개의 값을 표현할 수 있다.

---

# 6. 모든 Image가 반드시 0~255인가?

아니다.

Image가 어떤 형식으로 저장되어 있고 어떻게 변환되었는지에 따라 값의 범위와 dtype은 달라질 수 있다.

예:

```text
uint8
→ 0 ~ 255

Tensor float
→ 0.0 ~ 1.0

Normalize 후
→ 음수와 양수가 함께 존재 가능
```

따라서 실제 Data의:

```text
dtype

min

max
```

를 확인하는 습관이 중요하다.

---

# 7. Resolution

Image Resolution은 Image의 Width와 Height를 나타낸다.

예:

```text
224 × 224
```

---

# 8. Pixel 개수

Grayscale 224×224 Image:

```text
224 × 224
=
50,176 Pixels
```

이다.

---

# 9. 큰 Image

예:

```text
1024 × 1024
```

이면:

```text
1,048,576 Pixels
```

이다.

Image Size가 커질수록 처리해야 할 Data도 많아진다.

---

# 10. Image Size와 계산량

Image가 커지면 일반적으로:

```text
Memory 사용량 증가

연산량 증가

Training 시간 증가
```

가 발생할 수 있다.

그래서 Model에 넣기 전에 Resize를 사용하는 경우가 많다.

---

# 11. Width와 Height

예:

```text
640 × 480
```

이라고 표현하면 일반적으로:

```text
Width = 640

Height = 480
```

을 의미한다.

하지만 Library마다 Shape을 표시하는 순서가 다를 수 있으므로 주의한다.

---

# 12. PIL Image

Python에서 Image를 다룰 때 자주 사용하는 Library:

```python
from PIL import Image
```

이다.

---

# 13. Image 열기

```python
from PIL import Image


image = Image.open(
    "sample.png"
)
```

---

# 14. PIL Image Size

```python
print(
    image.size
)
```

예:

```text
(640, 480)
```

PIL의 `size`는:

```text
(width, height)
```

순서이다.

---

# 15. Image Mode

```python
print(
    image.mode
)
```

Image가 어떤 형태인지 확인할 수 있다.

---

# 16. 대표적인 PIL Mode

```text
L
→ Grayscale

RGB
→ Red, Green, Blue

RGBA
→ RGB + Alpha
```

등이 있다.

---

# 17. L Mode

```text
L
```

은 Grayscale Image에서 자주 볼 수 있다.

예:

```python
image = image.convert(
    "L"
)
```

---

# 18. RGB Mode

```text
RGB
```

는 3개의 Channel을 사용한다.

```text
R
→ Red

G
→ Green

B
→ Blue
```

---

# 19. RGB로 변환

```python
image = image.convert(
    "RGB"
)
```

---

# 20. Grayscale을 RGB로 변환하면?

원래 없던 실제 Color 정보가 생기는 것은 아니다.

Grayscale 정보를 3개의 Channel 형식으로 맞춰 사용할 수 있게 된다.

이 방법은 RGB 입력을 기대하는 Pretrained Model을 사용할 때 볼 수 있다.

---

# 21. Channel이란?

Image를 구성하는 정보 층이라고 생각할 수 있다.

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

# 22. RGB Pixel

RGB Image의 Pixel 하나는 예를 들어:

```text
[255, 0, 0]
```

처럼 3개의 값을 가질 수 있다.

---

# 23. 예

```text
[255, 0, 0]
→ Red

[0, 255, 0]
→ Green

[0, 0, 255]
→ Blue
```

처럼 생각할 수 있다.

---

# 24. 색상은 Channel 조합

예:

```text
[255, 255, 0]
```

처럼 R, G, B 값의 조합으로 여러 색상을 표현할 수 있다.

---

# 25. NumPy로 Image 변환

```python
import numpy as np


arr = np.array(
    image
)
```

---

# 26. Shape 확인

```python
print(
    arr.shape
)
```

---

# 27. Grayscale NumPy Shape

Grayscale Image라면 일반적으로:

```text
[Height, Width]
```

예:

```text
(224, 224)
```

처럼 나타날 수 있다.

---

# 28. RGB NumPy Shape

RGB Image:

```text
[Height, Width, Channel]
```

예:

```text
(224, 224, 3)
```

이다.

---

# 29. HWC

NumPy Image에서 자주 보는 구조:

```text
HWC
```

이다.

각각:

```text
H
→ Height

W
→ Width

C
→ Channel
```

이다.

---

# 30. 예

```text
(224, 224, 3)
```

를 보면:

```text
Height = 224

Width = 224

Channel = 3
```

이라고 읽는다.

---

# 31. NumPy dtype

```python
print(
    arr.dtype
)
```

일반적인 Image에서는:

```text
uint8
```

을 자주 볼 수 있다.

---

# 32. uint8

```text
unsigned 8-bit integer
```

이다.

일반적으로:

```text
0 ~ 255
```

를 표현할 수 있다.

---

# 33. Pixel 최솟값

```python
print(
    arr.min()
)
```

---

# 34. Pixel 최댓값

```python
print(
    arr.max()
)
```

---

# 35. 평균 Pixel 값

```python
print(
    arr.mean()
)
```

Grayscale Image에서는 간단한 평균 밝기 지표로 해석할 수 있다.

---

# 36. 한 번에 확인

```python
print(
    "Shape:",
    arr.shape
)

print(
    "dtype:",
    arr.dtype
)

print(
    "Min:",
    arr.min()
)

print(
    "Max:",
    arr.max()
)

print(
    "Mean:",
    arr.mean()
)
```

---

# 37. Image를 실제로 보기

```python
import matplotlib.pyplot as plt


plt.imshow(
    image
)

plt.axis(
    "off"
)

plt.show()
```

---

# 38. Grayscale

```python
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

# 39. Image AI에서는 직접 보는 것이 중요하다

숫자만 확인하지 않고 Sample Image를 직접 확인한다.

예:

```text
Image가 깨져 있지 않은가?

방향이 이상하지 않은가?

Border가 있는가?

Text가 있는가?

밝기가 이상하지 않은가?

Label과 Image가 맞는가?
```

등이다.

---

# 40. PyTorch Tensor

PyTorch Model에서는 Image를 Tensor 형태로 사용한다.

예:

```python
import torch
```

---

# 41. torchvision Transform

Image를 Tensor로 바꿀 때:

```python
from torchvision import transforms
```

를 사용할 수 있다.

---

# 42. ToTensor

```python
transform = transforms.ToTensor()
```

---

# 43. 적용

```python
tensor_image = transform(
    image
)
```

---

# 44. Shape 확인

```python
print(
    tensor_image.shape
)
```

---

# 45. RGB Tensor Shape

PIL/NumPy에서는:

```text
[H, W, C]
```

였지만 PyTorch Image Tensor는 일반적으로:

```text
[C, H, W]
```

이다.

---

# 46. 예

NumPy:

```text
[224, 224, 3]
```

↓

PyTorch:

```text
[3, 224, 224]
```

이다.

---

# 47. CHW

PyTorch Image Tensor에서 자주 보는:

```text
CHW
```

각각:

```text
C
→ Channel

H
→ Height

W
→ Width
```

이다.

---

# 48. NumPy와 PyTorch 차이

```text
NumPy Image
→ HWC

PyTorch Image Tensor
→ CHW
```

를 자주 사용한다.

---

# 49. 매우 중요

```text
HWC
≠
CHW
```

이다.

Shape 숫자가 같아 보여도 순서가 다르면 Model이 기대하는 형태와 맞지 않을 수 있다.

---

# 50. ToTensor와 Pixel Range

일반적인 `uint8` PIL Image에 `ToTensor()`를 사용하면 Pixel 값을 float Tensor로 변환하면서:

```text
0 ~ 255
```

를:

```text
0.0 ~ 1.0
```

범위로 변환한다.

---

# 51. 확인

```python
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

예:

```text
torch.float32

tensor(0.)

tensor(1.)
```

처럼 볼 수 있다.

---

# 52. 왜 255로 나누는가?

예:

```text
255
↓
255 / 255
↓
1.0
```

```text
128
↓
128 / 255
↓
약 0.502
```

처럼 Scale이 변한다.

---

# 53. 단순히 Tensor로 바꾸는 것과 Normalize는 다르다

```text
ToTensor
```

와:

```text
Normalize
```

는 같은 작업이 아니다.

---

# 54. ToTensor

일반적인 uint8 Image의 경우:

```text
Image
↓
Tensor

HWC
↓
CHW

0~255
↓
0~1
```

같은 변화가 일어날 수 있다.

---

# 55. Normalize

그 다음:

```python
transforms.Normalize(
    mean,
    std
)
```

를 사용하면 각 Channel 값을 지정된 평균과 표준편차를 기준으로 변환한다.

---

# 56. Normalize 공식

```text
x_normalized
=
(x - mean) / std
```

이다.

---

# 57. 그래서 Normalize 후에는

Pixel Tensor 값이:

```text
0 ~ 1
```

범위를 벗어날 수 있다.

예:

```text
-1.2

0.3

1.8
```

등이 가능하다.

---

# 58. Normalize된 Tensor를 보고 이상하다고 생각하면 안 된다

Normalize 후:

```python
tensor.min()
```

이 음수라고 해서 Image가 잘못된 것은 아니다.

어떤 Transform을 적용했는지 확인해야 한다.

---

# 59. Batch

Image 하나:

```text
[C, H, W]
```

여러 Image를 DataLoader가 Batch로 묶으면:

```text
[B, C, H, W]
```

가 된다.

---

# 60. BCHW

```text
B
→ Batch

C
→ Channel

H
→ Height

W
→ Width
```

이다.

---

# 61. 예

```text
[32, 3, 224, 224]
```

이면:

```text
32
→ Image 개수

3
→ RGB Channel

224
→ Height

224
→ Width
```

이다.

---

# 62. Grayscale Batch

```text
[32, 1, 224, 224]
```

이다.

---

# 63. RGB Batch

```text
[32, 3, 224, 224]
```

이다.

---

# 64. Model이 기대하는 Channel과 맞아야 한다

예:

```python
nn.Conv2d(
    in_channels=3,
    out_channels=16,
    kernel_size=3
)
```

이면 Model은:

```text
3 Channel
```

Input을 기대한다.

---

# 65. 그런데 Input이 Grayscale이라면?

Input:

```text
[B, 1, H, W]
```

Model:

```text
in_channels=3
```

이면 Channel이 맞지 않아 오류가 발생할 수 있다.

---

# 66. 해결 방법은 문제에 따라 다르다

예:

```text
Model 첫 Layer를 1 Channel로 변경

또는

Grayscale Image를 3 Channel 형태로 변환
```

등을 고려할 수 있다.

---

# 67. Pretrained Model

ImageNet으로 Pretraining된 많은 Model은:

```text
RGB 3 Channel
```

입력을 기준으로 만들어져 있다.

그래서 Grayscale 의료 Image를:

```python
Image.open(
    image_path
).convert(
    "RGB"
)
```

형태로 사용하는 경우도 있다.

---

# 68. Grayscale → RGB

개념적으로:

```text
Gray Channel
↓
R

Gray Channel
↓
G

Gray Channel
↓
B
```

처럼 동일한 밝기 정보를 3 Channel 형태로 표현할 수 있다.

---

# 69. 실제 Color 정보가 추가되는 것은 아니다

중요하다.

```text
1 Channel
→ 3 Channel
```

이 되었다고 실제 Color 정보가 새롭게 생긴 것은 아니다.

Model 입력 형식을 맞춘 것이다.

---

# 70. Alpha Channel

일부 PNG Image는:

```text
RGBA
```

일 수 있다.

Shape:

```text
[H, W, 4]
```

가 될 수 있다.

---

# 71. Alpha란?

투명도 정보를 나타내는 Channel이다.

---

# 72. Model이 RGB를 기대한다면

```python
image = Image.open(
    image_path
).convert(
    "RGB"
)
```

로 명확하게 맞출 수 있다.

---

# 73. Image Format

대표적인 Image File:

```text
PNG

JPG / JPEG

BMP

TIFF
```

등이 있다.

---

# 74. JPG

JPEG는 일반적으로 손실 압축을 사용한다.

File Size를 줄이는 과정에서 원본 정보 일부가 변할 수 있다.

---

# 75. PNG

PNG는 일반적으로 무손실 압축을 사용한다.

또한 Alpha Channel을 포함할 수 있다.

---

# 76. 의료영상에서는 DICOM도 중요

실제 의료기관의 영상에서는:

```text
DICOM
```

형식을 자주 사용한다.

---

# 77. DICOM

DICOM은 단순 Image Pixel뿐 아니라 다양한 의료영상 Metadata를 포함할 수 있다.

예:

```text
촬영 정보

Image 위치

Pixel 관련 정보

Series 정보
```

등이다.

---

# 78. PNG와 DICOM은 같지 않다

의료영상 Dataset에서 DICOM을 PNG로 변환했다면 일부 원본 정보나 Metadata가 제외될 수 있다.

따라서:

```text
PNG Image Classification
```

과:

```text
원본 DICOM 기반 분석
```

은 완전히 같은 문제라고 볼 수 없다.

---

# 79. Image dtype

Image Data를 볼 때 dtype을 확인한다.

예:

```text
uint8

uint16

float32
```

등이다.

---

# 80. 의료 Image에서는 uint16도 볼 수 있다

일부 의료 Image는 일반적인 8-bit Image보다 더 넓은 값 범위를 가질 수 있다.

따라서 무조건:

```text
0~255
```

라고 가정하면 안 된다.

---

# 81. 핵심

Image를 처음 받으면:

```python
print(
    arr.dtype
)

print(
    arr.min()
)

print(
    arr.max()
)
```

를 직접 확인한다.

---

# 82. Image Shape도 직접 확인

```python
print(
    arr.shape
)
```

---

# 83. 모든 Image Size가 같은가?

Dataset에 따라 Image마다 Size가 다를 수 있다.

예:

```text
Image 1
640 × 480

Image 2
1024 × 768

Image 3
512 × 512
```

---

# 84. Batch 문제

서로 다른 Shape의 Image를 그대로 하나의 Tensor Batch로 묶기 어려울 수 있다.

그래서:

```text
Resize

Crop

Padding
```

등을 사용한다.

---

# 85. Resize

예:

```python
transforms.Resize(
    (
        224,
        224
    )
)
```

---

# 86. 결과

다양한 크기의 Image를:

```text
224 × 224
```

형태로 맞출 수 있다.

---

# 87. 하지만 Resize는 단순하지 않다

원본:

```text
800 × 400
```

을:

```text
224 × 224
```

로 강제로 바꾸면 원래 Aspect Ratio가 변할 수 있다.

---

# 88. Aspect Ratio

Image의:

```text
Width : Height
```

비율이다.

---

# 89. 왜 중요할까?

Aspect Ratio가 크게 바뀌면 Object나 해부학적 구조가 찌그러질 수 있다.

---

# 90. 다른 방법

상황에 따라:

```text
Resize

Center Crop

Random Crop

Padding
```

등을 조합할 수 있다.

---

# 91. 어떤 방식이 무조건 정답은 아니다

Dataset과 Task에 따라 결정한다.

특히 의료 Image에서는 중요한 구조가 잘리지 않는지 확인해야 한다.

---

# 92. Image Tensor 시각화

PyTorch Tensor:

```text
[C, H, W]
```

를 Matplotlib으로 바로 표시하려면 Channel 순서를 바꿔야 할 수 있다.

---

# 93. CHW → HWC

```python
img = tensor_image.permute(
    1,
    2,
    0
)
```

---

# 94. 시각화

```python
plt.imshow(
    img
)

plt.axis(
    "off"
)

plt.show()
```

---

# 95. 왜 permute?

Tensor:

```text
[C, H, W]
```

Matplotlib RGB Image:

```text
[H, W, C]
```

형태를 주로 사용하기 때문이다.

---

# 96. Grayscale Tensor

예:

```text
[1, 224, 224]
```

에서 Channel Dimension을 제거해:

```python
img = tensor_image.squeeze(
    0
)
```

할 수 있다.

---

# 97. 시각화

```python
plt.imshow(
    img,
    cmap="gray"
)
```

---

# 98. squeeze 주의

무조건 `squeeze()`를 사용하면 Size가 1인 모든 Dimension이 제거될 수 있다.

어떤 Dimension을 제거하는지 명확하게:

```python
squeeze(
    0
)
```

처럼 지정하는 것이 안전할 수 있다.

---

# 99. Image 한 장에 Batch Dimension 추가

Model이:

```text
[B, C, H, W]
```

를 기대하는데 Image 한 장이:

```text
[C, H, W]
```

라면:

```python
image_tensor = image_tensor.unsqueeze(
    0
)
```

을 사용할 수 있다.

---

# 100. 변화

```text
[3, 224, 224]
```

↓

```text
[1, 3, 224, 224]
```

이다.

---

# 101. 첫 번째 1

```text
Batch Size = 1
```

을 의미한다.

---

# 102. Dataset에서는 보통 한 Sample

Custom Dataset의:

```python
__getitem__()
```

은 보통 Image 한 장을 반환한다.

Shape:

```text
[C, H, W]
```

이다.

---

# 103. DataLoader

DataLoader가 여러 Sample을 모아서:

```text
[B, C, H, W]
```

를 만든다.

---

# 104. 연결

```text
Dataset[index]
↓
[C, H, W]

DataLoader
↓
[B, C, H, W]
```

이다.

---

# 105. Label

Image Classification Dataset에서는:

```python
return image, label
```

형태를 많이 사용한다.

---

# 106. DataLoader Batch

```python
images, labels = next(
    iter(
        train_loader
    )
)
```

---

# 107. Shape 확인

```python
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

이다.

---

# 108. 반드시 확인

Training을 시작하기 전에 한 Batch를 꺼내서:

```text
Image Shape

Label Shape

dtype

Pixel Range
```

를 확인한다.

---

# 109. 예

```python
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
    "Image dtype:",
    images.dtype
)

print(
    "Image min:",
    images.min()
)

print(
    "Image max:",
    images.max()
)

print(
    "Label shape:",
    labels.shape
)

print(
    "Label dtype:",
    labels.dtype
)
```

---

# 110. 이것이 중요한 이유

Full Training을 시작한 뒤 오류를 찾는 것보다:

```text
Dataset Sample 확인
↓
DataLoader Batch 확인
↓
Model Forward 확인
```

순서로 점검하는 것이 훨씬 쉽다.

---

# 111. Computer Vision Debugging 순서

```text
1. File 존재?

2. Image Open 가능?

3. PIL Size / Mode?

4. NumPy Shape / dtype?

5. Transform 결과 Shape?

6. Dataset[index] 정상?

7. DataLoader Batch 정상?

8. Model Forward 정상?
```

이다.

---

# 112. Model Forward 전에 확인

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


print(
    outputs.shape
)
```

---

# 113. Binary 2-Class Classification

예:

```text
Batch = 32

Classes = 2
```

이면 Output:

```text
[32, 2]
```

가 될 수 있다.

---

# 114. Multi-class

예:

```text
Batch = 32

Classes = 11
```

이면:

```text
[32, 11]
```

처럼 나올 수 있다.

---

# 115. Input과 Output을 같이 읽기

예:

```text
Input
[32, 3, 224, 224]

↓

CNN

↓

Output
[32, 2]
```

이다.

---

# 116. 의미

```text
32개의 RGB Image를 입력

각 Image에 대해

2개 Class Logit 출력
```

이다.

---

# 117. Pixel 값과 Model Prediction

Model은 결국:

```text
Pixel 숫자
```

에서 Pattern을 학습한다.

Model이 사람처럼 Image를 보는 것은 아니다.

---

# 118. 따라서 Dataset의 숫자적 차이도 중요

예:

```text
Class 0 Image 평균 밝기 = 100

Class 1 Image 평균 밝기 = 80
```

처럼 Class별 밝기 차이가 크면 Model이 이 차이를 활용할 수도 있다.

---

# 119. 이것이 항상 나쁜가?

반드시 그렇다고 할 수는 없다.

하지만 우리가 의도한 의료적 Pattern이 아니라 Dataset 수집 과정에서 생긴 차이라면:

```text
Shortcut Learning
```

문제가 될 수 있다.

---

# 120. 그래서 Image EDA

Class별로:

```text
Image Size

Brightness

Channel

Pixel Distribution

촬영 Pattern
```

등을 확인할 수 있다.

---

# 121. 평균 밝기 계산

```python
brightness = arr.mean()
```

---

# 122. 여러 Image

개념적으로:

```python
brightness_list = []


for image_path in image_paths:

    image = Image.open(
        image_path
    ).convert(
        "L"
    )

    arr = np.array(
        image
    )

    brightness_list.append(
        arr.mean()
    )
```

---

# 123. Class별 비교

DataFrame에:

```text
Image

Label

Brightness
```

를 저장하고 Group별 평균을 비교할 수도 있다.

---

# 124. Image Histogram

Grayscale Pixel Distribution:

```python
plt.hist(
    arr.flatten(),
    bins=50
)

plt.show()
```

---

# 125. flatten

Image:

```text
[H, W]
```

를:

```text
[H × W]
```

형태로 펼쳐 Histogram을 그릴 수 있다.

---

# 126. Histogram에서 볼 수 있는 것

```text
어두운 Pixel이 많은가?

밝은 Pixel이 많은가?

특정 값에 몰려 있는가?

Contrast가 다른가?
```

등을 볼 수 있다.

---

# 127. RGB Histogram

RGB는 Channel별로 Pixel Distribution을 따로 확인할 수도 있다.

하지만 처음에는 Shape과 실제 Image를 확인하는 것이 우선이다.

---

# 128. Image Standardization

Pretrained Model에서는 특정:

```text
mean

std
```

를 사용하는 경우가 있다.

---

# 129. 중요한 점

Pretrained Weight를 사용한다면 해당 Weight에 권장되는 Preprocessing을 확인하는 것이 좋다.

Model Architecture 이름만 보고 임의로 Normalize 값을 정하지 않는다.

---

# 130. Train과 Validation

기본적인 Resize와 Normalize는 일관되게 적용하는 것이 중요하다.

하지만 Random Augmentation은 일반적으로 Training에만 적용한다.

---

# 131. 예

Train:

```text
Resize
↓
Random Augmentation
↓
ToTensor
↓
Normalize
```

Validation:

```text
Resize
↓
ToTensor
↓
Normalize
```

이다.

---

# 132. Test

Test도 일반적으로:

```text
Validation과 동일하거나
동일한 평가용 전처리
```

를 사용한다.

---

# 133. Data Leakage와 Image

Image에서도 Data Leakage가 발생할 수 있다.

예:

```text
같은 Patient의 Image가
Train과 Validation에 동시에 존재
```

---

# 134. Duplicate Image

동일하거나 거의 동일한 Image가:

```text
Train

Validation
```

에 동시에 존재하면 Validation 결과가 지나치게 좋아질 수 있다.

---

# 135. 파일명만 다르다고 다른 Image는 아니다

예:

```text
image_001.png

copy_image_001.png
```

가 실제 Pixel은 동일할 수도 있다.

---

# 136. 의료 Image에서는 Patient ID가 중요할 수 있다

한 Patient가 여러 Image나 Slice를 가지고 있다면 단순 Image 단위 Random Split보다 Patient 단위 Split이 필요할 수 있다.

---

# 137. 2D와 3D Image

지금까지 본:

```text
[H, W]

[C, H, W]
```

는 주로 2D Image를 생각한 것이다.

---

# 138. CT와 MRI

실제 CT/MRI는 여러 Slice로 이루어진 Volume Data로 다룰 수 있다.

개념적으로:

```text
Depth × Height × Width
```

구조를 가질 수 있다.

---

# 139. 2D Slice Classification

CT/MRI의 특정 Slice만 사용하면:

```text
2D Image Classification
```

문제로 만들 수 있다.

---

# 140. 3D Model

여러 Slice를 Volume으로 함께 사용하면:

```text
3D CNN
```

같은 접근도 가능하다.

하지만 계산량과 Data 구조가 더 복잡해진다.

---

# 141. 따라서 문제 정의가 중요

같은 CT라도:

```text
한 Slice 분류

여러 Slice 분류

전체 Volume 분류

병변 Detection

Segmentation
```

은 서로 다른 Task이다.

---

# 142. Image Data를 받았다고 바로 CNN부터 만들지 않는다

먼저:

```text
Image Format

Size

Shape

Channel

dtype

Pixel Range

Label

Class Distribution

Patient 구조
```

를 확인한다.

---

# 143. Image 기본 검사 함수

간단하게 만들 수 있다.

```python
from PIL import Image
import numpy as np


def inspect_image(
    image_path
):

    image = Image.open(
        image_path
    )

    arr = np.array(
        image
    )

    print(
        "PIL size:",
        image.size
    )

    print(
        "PIL mode:",
        image.mode
    )

    print(
        "NumPy shape:",
        arr.shape
    )

    print(
        "dtype:",
        arr.dtype
    )

    print(
        "min:",
        arr.min()
    )

    print(
        "max:",
        arr.max()
    )

    print(
        "mean:",
        arr.mean()
    )
```

---

# 144. 사용

```python
inspect_image(
    "sample.png"
)
```

---

# 145. Tensor 검사 함수

```python
def inspect_tensor(
    x
):

    print(
        "shape:",
        x.shape
    )

    print(
        "dtype:",
        x.dtype
    )

    print(
        "device:",
        x.device
    )

    print(
        "min:",
        x.min().item()
    )

    print(
        "max:",
        x.max().item()
    )
```

---

# 146. Computer Vision에서 중요한 확인 4개

```text
Shape

dtype

Pixel Range

Channel
```

이다.

Deep Learning 전체에서는 여기에:

```text
device
```

도 함께 확인한다.

---

# 147. Library별 Image Shape 정리

```text
PIL
→ size = (W, H)

NumPy Grayscale
→ [H, W]

NumPy RGB
→ [H, W, C]

PyTorch Image
→ [C, H, W]

PyTorch Batch
→ [B, C, H, W]
```

---

# 148. 이 순서는 반드시 익숙해져야 한다

특히:

```text
HWC
↓
CHW
↓
BCHW
```

변화를 이해하면 Computer Vision Code를 읽기가 훨씬 쉬워진다.

---

# 149. 전체 흐름

```text
Image File
↓
PIL Image

(W, H)
↓
Transform
↓
Tensor

[C, H, W]
↓
Dataset
↓
DataLoader
↓
Batch

[B, C, H, W]
↓
CNN
↓
Logits

[B, Classes]
```

---

# 150. 예

```text
PNG File
↓
PIL RGB

(224, 224)
↓
ToTensor
↓
[3, 224, 224]
↓
DataLoader
Batch Size = 32
↓
[32, 3, 224, 224]
↓
CNN
2 Classes
↓
[32, 2]
```

---

# 151. Grayscale 예

```text
X-ray
↓
PIL L
↓
[224, 224]
↓
Tensor
↓
[1, 224, 224]
```

Pretrained RGB Model을 사용한다면 상황에 따라:

```text
L
↓
RGB
↓
[3, 224, 224]
```

형태로 사용할 수 있다.

---

# 152. 가장 자주 생기는 오류

```text
Model expected 3 channels
but input has 1 channel
```

같은 Channel 불일치이다.

---

# 153. 그다음 자주 확인할 문제

```text
Image Size 불일치

HWC / CHW 혼동

dtype 불일치

Normalize 누락

Batch Dimension 누락

Device 불일치
```

등이다.

---

# 154. Image 한 장 Prediction

```text
[C, H, W]
```

인 Tensor에:

```python
x = x.unsqueeze(
    0
)
```

하여:

```text
[1, C, H, W]
```

로 만든 뒤 Model에 입력할 수 있다.

---

# 155. Prediction 후

Multi-class:

```python
with torch.no_grad():

    logits = model(
        x
    )

    pred = logits.argmax(
        dim=1
    )
```

---

# 156. Probability

```python
prob = torch.softmax(
    logits,
    dim=1
)
```

---

# 157. 다시 전체적으로

Computer는 Image를:

```text
사진
```

자체로 이해하는 것이 아니라:

```text
Pixel Number
```

의 Pattern으로 처리한다.

---

# 158. CNN이 학습하는 것

Training을 통해 Filter Weight가 업데이트되면서 Classification에 유용한 Image Pattern을 표현하게 된다.

---

# 159. 따라서 Image Data 이해가 먼저다

CNN을 공부하기 전에:

```text
Pixel

Resolution

Channel

Shape

dtype

Pixel Range
```

를 이해해야 한다.

---

# 160. 핵심 정리

Image:

```text
Pixel들의 집합
```

Grayscale:

```text
1 Channel
```

RGB:

```text
3 Channels
```

PIL:

```text
(W, H)
```

NumPy RGB:

```text
[H, W, C]
```

PyTorch Image:

```text
[C, H, W]
```

PyTorch Batch:

```text
[B, C, H, W]
```

일반적인 uint8:

```text
0 ~ 255
```

일반적인 `ToTensor()` 이후:

```text
0.0 ~ 1.0
```

Normalize 이후:

```text
0~1 범위를 벗어날 수 있음
```

Computer Vision에서 Image를 받으면 먼저:

```text
Shape

dtype

Pixel Range

Channel

Label
```

을 확인한다.

---

# 161. 다음 단계

다음 파일:

```text
image-preprocessing.md
```

에서는 실제 Model에 Image를 넣기 전에 사용하는:

```text
Resize

Crop

ToTensor

Normalize

Transform

Train Transform

Validation Transform

Pretrained Weight Transform

Grayscale → RGB

Aspect Ratio
```

를 자세히 정리한다.