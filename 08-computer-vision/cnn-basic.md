# CNN Basic

## 1. CNN이란?

CNN은:

```text
Convolutional Neural Network
```

의 약자이다.

한국어로:

```text
합성곱 신경망
```

이라고 한다.

Image를 처리할 때 대표적으로 사용하는 Deep Learning 구조이다.

---

# 2. 왜 Image에서 CNN을 사용할까?

일반 Neural Network에서는 Image를 다음처럼 펼칠 수 있다.

```text
Image

28 × 28
↓
Flatten
↓
784
↓
Linear
```

하지만 이렇게 하면 Image의 공간적인 구조를 직접 활용하기 어렵다.

Image에서는:

```text
어떤 Pixel이 서로 가까운가?

어디에 Edge가 있는가?

어떤 Texture가 있는가?

어떤 Shape이 있는가?
```

같은 공간적 관계가 중요하다.

CNN은 작은 영역을 보면서 이러한 Pattern을 학습한다.

---

# 3. CNN의 핵심

CNN에서는 작은 Filter가 Image 위를 이동하면서 Pattern을 찾는다.

```text
Image
↓
Filter
↓
Convolution
↓
Feature Map
```

이다.

---

# 4. CNN 전체 흐름

대표적인 구조:

```text
Image
↓
Conv2d
↓
ReLU
↓
Pooling
↓
Conv2d
↓
ReLU
↓
Pooling
↓
Flatten
↓
Linear
↓
Class Prediction
```

---

# 5. Convolution

Convolution은 CNN의 핵심 연산이다.

Image 전체를 한 번에 보는 대신 작은 영역을 순서대로 살펴본다.

예를 들어:

```text
3 × 3
```

Filter를 사용할 수 있다.

---

# 6. Kernel / Filter

CNN에서:

```text
Kernel

Filter
```

라는 표현을 자주 볼 수 있다.

입문 단계에서는 Image의 작은 영역에 적용되는 학습 가능한 Weight라고 이해하면 된다.

---

# 7. 예

Image의 일부:

```text
1  2  3

4  5  6

7  8  9
```

Filter:

```text
1   0  -1

1   0  -1

1   0  -1
```

같은 작은 Matrix를 생각할 수 있다.

---

# 8. 실제 CNN의 Filter

실제 CNN에서는 Filter 값을 사람이 직접 정하는 것이 아니라 Training을 통해 Weight를 학습한다.

즉:

```text
처음 Weight
↓
Forward
↓
Loss
↓
Backward
↓
Gradient
↓
Optimizer
↓
Filter Weight Update
```

이다.

---

# 9. Chapter 07과 연결

CNN도 결국 Neural Network이다.

따라서:

```text
loss.backward()

optimizer.step()
```

으로 Parameter를 학습한다.

Conv Filter 역시 Model Parameter이다.

---

# 10. PyTorch Conv2d

PyTorch에서는:

```python
nn.Conv2d(
    in_channels=3,
    out_channels=16,
    kernel_size=3
)
```

처럼 사용한다.

---

# 11. 각각의 의미

```text
in_channels=3
→ Input Channel 3개

out_channels=16
→ 16개의 Output Feature Map

kernel_size=3
→ 3 × 3 Kernel
```

이다.

---

# 12. RGB Input

RGB Image:

```text
[B, 3, H, W]
```

이므로 첫 Conv Layer는:

```python
nn.Conv2d(
    3,
    16,
    3
)
```

처럼 만들 수 있다.

---

# 13. Grayscale Input

Grayscale Image:

```text
[B, 1, H, W]
```

라면:

```python
nn.Conv2d(
    1,
    16,
    3
)
```

처럼 만들 수 있다.

---

# 14. in_channels가 중요한 이유

Input:

```text
[B, 1, 224, 224]
```

인데 Model:

```python
nn.Conv2d(
    in_channels=3,
    ...
)
```

이면 Channel이 맞지 않아 오류가 발생할 수 있다.

---

# 15. out_channels

```python
out_channels=16
```

은 16개의 Feature Map을 만든다는 의미로 이해할 수 있다.

Input:

```text
[B, 3, H, W]
```

↓

Conv:

```text
[B, 16, H', W']
```

가 된다.

---

# 16. Channel 증가

CNN에서는 Layer가 깊어지면서 Channel 수를 증가시키는 구조를 자주 볼 수 있다.

예:

```text
3
↓
16
↓
32
↓
64
↓
128
```

---

# 17. 왜 Channel을 늘릴까?

각 Channel이 서로 다른 Feature 표현을 학습할 수 있기 때문이다.

개념적으로:

```text
Edge 관련 Feature

Texture 관련 Feature

Shape 관련 Feature
```

등 다양한 Pattern을 표현할 수 있다.

단, 특정 Channel 하나가 반드시 사람이 이름 붙일 수 있는 Feature 하나와 정확히 대응한다고 생각하면 안 된다.

---

# 18. Feature Map

Convolution 결과:

```text
Feature Map
```

이라고 부른다.

예:

```text
Input
[32, 3, 224, 224]

↓

Conv2d

↓

Feature Map
[32, 16, 222, 222]
```

처럼 나올 수 있다.

---

# 19. 왜 224가 222가 되었을까?

다음 설정을 생각하자.

```python
nn.Conv2d(
    3,
    16,
    kernel_size=3,
    stride=1,
    padding=0
)
```

Padding 없이 3×3 Kernel을 적용하면 Spatial Size가 줄어든다.

---

# 20. Convolution Output Size 공식

한 Dimension에 대해:

```text
Output
=
floor(
    (Input + 2P - K) / S
)
+ 1
```

이다.

여기서:

```text
P
→ Padding

K
→ Kernel Size

S
→ Stride
```

이다.

---

# 21. 예

Input:

```text
224
```

Kernel:

```text
3
```

Padding:

```text
0
```

Stride:

```text
1
```

이면:

```text
(224 - 3) / 1 + 1
=
222
```

이다.

---

# 22. 따라서

```text
[32, 3, 224, 224]
```

↓

```python
nn.Conv2d(
    3,
    16,
    3
)
```

↓

```text
[32, 16, 222, 222]
```

가 될 수 있다.

---

# 23. Padding

Image 가장자리에 값을 추가하는 개념이다.

예:

```python
nn.Conv2d(
    3,
    16,
    kernel_size=3,
    padding=1
)
```

---

# 24. Padding=1

3×3 Kernel, Stride=1에서 Padding=1을 사용하면 Spatial Size를 유지할 수 있다.

```text
224 × 224
↓
224 × 224
```

---

# 25. 계산

```text
(224 + 2×1 - 3) / 1 + 1
=
224
```

이다.

---

# 26. 그래서 자주 보는 구조

```python
nn.Conv2d(
    3,
    16,
    kernel_size=3,
    padding=1
)
```

이다.

---

# 27. Stride

Filter가 한 번에 몇 Pixel씩 이동할지를 나타낸다.

```text
stride=1
→ 한 칸씩

stride=2
→ 두 칸씩
```

이다.

---

# 28. Stride=2

Spatial Size를 줄이는 데 사용할 수 있다.

예:

```python
nn.Conv2d(
    16,
    32,
    kernel_size=3,
    stride=2,
    padding=1
)
```

---

# 29. 대략적인 변화

```text
224 × 224
↓
112 × 112
```

처럼 Spatial Size가 줄어들 수 있다.

---

# 30. Kernel Size

대표적으로:

```text
3 × 3

5 × 5

7 × 7
```

등을 볼 수 있다.

현대 CNN에서는 3×3 Kernel을 매우 자주 볼 수 있다.

---

# 31. Kernel이 보는 영역

3×3 Kernel은 한 번의 연산에서 주변 3×3 영역을 본다.

하지만 CNN Layer를 여러 개 쌓으면 더 넓은 영역의 정보를 간접적으로 사용할 수 있다.

---

# 32. Receptive Field

한 Neuron이 원본 Image에서 영향을 받을 수 있는 영역을:

```text
Receptive Field
```

라고 한다.

Layer가 깊어지면서 Receptive Field가 커질 수 있다.

---

# 33. CNN 초기 Layer

초기 Layer에서는 상대적으로 Local Pattern을 표현한다.

예:

```text
Edge

Line

밝기 변화
```

등이다.

---

# 34. 깊은 Layer

앞의 Feature를 조합하면서 더 복잡한 표현을 만들 수 있다.

개념적으로:

```text
Edge
↓
Texture / Shape
↓
더 복잡한 Pattern
```

이다.

---

# 35. ReLU

Convolution 뒤에 Activation Function을 적용하는 구조를 자주 사용한다.

```python
nn.ReLU()
```

---

# 36. 왜 ReLU가 필요한가?

Conv2d 역시 Weight를 이용한 Linear 연산이다.

Activation Function을 추가해 Non-linearity를 만든다.

```text
Conv
↓
ReLU
```

이다.

---

# 37. 예

```python
nn.Sequential(
    nn.Conv2d(
        3,
        16,
        kernel_size=3,
        padding=1
    ),

    nn.ReLU()
)
```

---

# 38. Shape

ReLU는 일반적으로 Tensor Shape을 바꾸지 않는다.

```text
Conv
[32, 16, 224, 224]

↓

ReLU

↓

[32, 16, 224, 224]
```

이다.

---

# 39. Pooling

Feature Map의 Spatial Size를 줄이는 방법이다.

대표적으로:

```text
Max Pooling

Average Pooling
```

이 있다.

---

# 40. Max Pooling

일정 영역에서 가장 큰 값을 선택한다.

```python
nn.MaxPool2d(
    kernel_size=2
)
```

---

# 41. 예

```text
1  3

2  4
```

에서 Max:

```text
4
```

를 선택한다.

---

# 42. 2×2 MaxPool

```text
224 × 224
↓
112 × 112
```

처럼 Spatial Size를 절반으로 줄일 수 있다.

---

# 43. PyTorch

```python
nn.MaxPool2d(
    kernel_size=2,
    stride=2
)
```

---

# 44. Pooling 전후

```text
[B, 16, 224, 224]
```

↓

MaxPool:

```text
[B, 16, 112, 112]
```

이다.

Channel은 그대로이고 H, W가 줄어든다.

---

# 45. Pooling의 목적

대표적으로:

```text
Spatial Size 감소

연산량 감소

중요한 Feature 요약
```

등의 역할을 할 수 있다.

---

# 46. 항상 Pooling을 사용해야 하나?

아니다.

현대 CNN에서는 Pooling 대신 Strided Convolution을 사용하는 Architecture도 있다.

CNN마다 구조가 다르다.

---

# 47. CNN Shape 변화 핵심

CNN에서는 흔히:

```text
Channel
↑ 증가

Height / Width
↓ 감소
```

하는 구조를 볼 수 있다.

---

# 48. 예

```text
Input
[32, 3, 224, 224]

↓

Conv
[32, 16, 224, 224]

↓

Pool
[32, 16, 112, 112]

↓

Conv
[32, 32, 112, 112]

↓

Pool
[32, 32, 56, 56]
```

이다.

---

# 49. 왜 Shape 추적이 중요한가?

마지막에 Linear Layer로 연결할 때 정확한 Feature 개수를 알아야 하기 때문이다.

---

# 50. Flatten

CNN Feature Map을 Linear Layer에 넣기 위해 펼칠 수 있다.

```python
nn.Flatten()
```

---

# 51. 예

```text
[32, 32, 56, 56]
```

↓

Flatten:

```text
[32, 32 × 56 × 56]
```

이다.

---

# 52. 계산

```text
32 × 56 × 56
=
100,352
```

따라서:

```text
[32, 100352]
```

가 된다.

---

# 53. Linear

그다음:

```python
nn.Linear(
    100352,
    128
)
```

처럼 연결할 수 있다.

---

# 54. 문제점

Input Image Size나 CNN 구조가 바뀌면:

```text
100352
```

도 달라진다.

직접 계산을 잘못하면 Shape Error가 발생할 수 있다.

---

# 55. 자주 보는 오류

```text
RuntimeError:
mat1 and mat2 shapes cannot be multiplied
```

이다.

---

# 56. 원인

Linear Layer가 기대하는 Feature 수와 실제 Flatten 결과가 다르기 때문이다.

---

# 57. Debugging

```python
print(
    x.shape
)
```

를 Layer 사이에서 확인한다.

---

# 58. 예

```python
x = self.conv1(
    x
)

print(
    x.shape
)


x = self.pool(
    x
)

print(
    x.shape
)
```

처럼 확인할 수 있다.

---

# 59. Adaptive Pooling

직접 Flatten 크기를 계산하는 부담을 줄이는 데 도움이 되는 방법 중 하나이다.

예:

```python
nn.AdaptiveAvgPool2d(
    (
        1,
        1
    )
)
```

---

# 60. 결과

Input:

```text
[B, C, H, W]
```

↓

Adaptive Average Pool:

```text
[B, C, 1, 1]
```

이다.

---

# 61. Flatten

```text
[B, C, 1, 1]
```

↓

```text
[B, C]
```

가 된다.

---

# 62. 장점

마지막 Spatial Size를 일정하게 만들 수 있어 Classifier 연결이 편해진다.

---

# 63. Global Average Pooling

Feature Map의 각 Channel에 대해 Spatial 평균을 계산하는 형태를 자주 볼 수 있다.

개념:

```text
[B, C, H, W]
↓
[B, C, 1, 1]
↓
[B, C]
```

이다.

---

# 64. 간단한 CNN 만들기

```python
import torch
import torch.nn as nn


model = nn.Sequential(

    nn.Conv2d(
        3,
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
    ),

    nn.Flatten(),

    nn.Linear(
        32 * 56 * 56,
        128
    ),

    nn.ReLU(),

    nn.Linear(
        128,
        2
    )
)
```

---

# 65. Input

위 Model에:

```text
[Batch, 3, 224, 224]
```

가 들어간다고 하자.

---

# 66. 첫 Conv

```text
[32, 3, 224, 224]
```

↓

```text
[32, 16, 224, 224]
```

---

# 67. 첫 Pool

```text
[32, 16, 224, 224]
```

↓

```text
[32, 16, 112, 112]
```

---

# 68. 두 번째 Conv

```text
[32, 16, 112, 112]
```

↓

```text
[32, 32, 112, 112]
```

---

# 69. 두 번째 Pool

```text
[32, 32, 112, 112]
```

↓

```text
[32, 32, 56, 56]
```

---

# 70. Flatten

```text
[32, 32, 56, 56]
```

↓

```text
[32, 100352]
```

---

# 71. Linear

```text
[32, 100352]
```

↓

```text
[32, 128]
```

---

# 72. Output

마지막:

```python
nn.Linear(
    128,
    2
)
```

↓

```text
[32, 2]
```

이다.

---

# 73. 최종 의미

```text
32장의 Image

각 Image에 대해

2개의 Class Logit
```

을 출력한다.

---

# 74. CrossEntropyLoss

2-Class를 두 개의 Logit으로 출력한다면:

```python
loss_fn = nn.CrossEntropyLoss()
```

를 사용할 수 있다.

---

# 75. Target

```text
[32]
```

형태의 Class Index:

```text
0

1
```

을 사용한다.

---

# 76. Softmax를 먼저 하지 않는다

CrossEntropyLoss에 넣을 때:

```python
logits = model(
    images
)


loss = loss_fn(
    logits,
    labels
)
```

처럼 Raw Logits를 사용한다.

---

# 77. Prediction

```python
preds = logits.argmax(
    dim=1
)
```

---

# 78. Probability

Inference에서 필요하면:

```python
probs = torch.softmax(
    logits,
    dim=1
)
```

을 사용할 수 있다.

---

# 79. CNN도 Training Loop는 같다

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

# 80. CNN이라고 Backprop 방식이 다른 것은 아니다

Chapter 07에서 배운:

```text
Forward
↓
Loss
↓
Backward
↓
Gradient
↓
Optimizer Step
```

구조 그대로이다.

---

# 81. CNN Parameter 확인

```python
for name, param in model.named_parameters():

    print(
        name,
        param.shape
    )
```

---

# 82. Conv Weight Shape

예:

```python
nn.Conv2d(
    3,
    16,
    kernel_size=3
)
```

Weight Shape은 개념적으로:

```text
[16, 3, 3, 3]
```

이다.

---

# 83. 의미

```text
16
→ Output Channels

3
→ Input Channels

3 × 3
→ Kernel
```

이다.

---

# 84. Parameter 수

Bias가 있다고 하면:

Conv Weight:

```text
16 × 3 × 3 × 3
=
432
```

Bias:

```text
16
```

총:

```text
448 Parameters
```

이다.

---

# 85. Parameter Sharing

CNN의 중요한 특징이다.

같은 Filter Weight를 Image의 여러 위치에 적용한다.

---

# 86. 왜 중요한가?

Image의 각 위치마다 완전히 다른 Weight를 만드는 것보다 Parameter를 효율적으로 사용할 수 있다.

또한 같은 Pattern을 여러 위치에서 찾을 수 있다.

---

# 87. Local Connectivity

CNN은 한 번에 Image 전체가 아니라 작은 Local Region을 본다.

```text
3 × 3

5 × 5
```

등이다.

---

# 88. CNN 핵심 특징

```text
Local Connectivity

Parameter Sharing

Hierarchical Feature Learning
```

이라고 정리할 수 있다.

---

# 89. Translation 관련 특성

같은 Filter가 여러 위치에 적용되므로 특정 Pattern이 Image의 다른 위치에 나타나도 비슷한 Feature를 검출할 수 있다.

하지만 CNN이 위치 변화에 완전히 영향을 받지 않는다고 단정하면 안 된다.

---

# 90. Pooling과 위치 변화

Pooling 등은 작은 위치 변화에 대한 민감도를 줄이는 데 도움을 줄 수 있다.

---

# 91. 하지만 위치가 중요한 문제도 있다

의료 Image에서는:

```text
병변 위치

좌우

상하

해부학적 위치
```

가 중요할 수 있다.

따라서 무조건 위치 정보를 없애는 것이 좋은 것은 아니다.

---

# 92. CNN Block

실제 Code에서는 반복 구조를 Block으로 묶기도 한다.

예:

```python
self.block1 = nn.Sequential(

    nn.Conv2d(
        3,
        32,
        3,
        padding=1
    ),

    nn.ReLU(),

    nn.MaxPool2d(
        2
    )
)
```

---

# 93. 여러 Block

```text
Block 1
3 → 32

Block 2
32 → 64

Block 3
64 → 128
```

처럼 구성할 수 있다.

---

# 94. Custom CNN

```python
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
                3,
                padding=1
            ),

            nn.ReLU(),

            nn.MaxPool2d(
                2
            ),

            nn.Conv2d(
                32,
                64,
                3,
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

# 95. 이 Model의 장점

Adaptive Pooling을 사용하므로 마지막 Linear Layer를 위해:

```text
64 × H × W
```

를 직접 계산하지 않아도 된다.

---

# 96. Shape 추적

Input:

```text
[B, 3, 224, 224]
```

↓

Block 1:

```text
[B, 32, 112, 112]
```

↓

Block 2:

```text
[B, 64, 56, 56]
```

↓

AdaptiveAvgPool:

```text
[B, 64, 1, 1]
```

↓

Flatten:

```text
[B, 64]
```

↓

Classifier:

```text
[B, 2]
```

이다.

---

# 97. Model Forward Test

Training 전에 Dummy Input으로 확인할 수 있다.

```python
model = SimpleCNN(
    num_classes=2
)


x = torch.randn(
    8,
    3,
    224,
    224
)


y = model(
    x
)


print(
    y.shape
)
```

---

# 98. 예상 결과

```text
torch.Size([8, 2])
```

이다.

---

# 99. 왜 Dummy Forward가 좋은가?

전체 DataLoader와 Training을 돌리기 전에:

```text
Model 구조

Input Channel

Shape

Output Class 수
```

가 맞는지 빠르게 확인할 수 있다.

---

# 100. 실제 Batch Forward

```python
images, labels = next(
    iter(
        train_loader
    )
)


print(
    images.shape
)


logits = model(
    images
)


print(
    logits.shape
)
```

---

# 101. GPU라면

```python
model = model.to(
    device
)


images = images.to(
    device
)


labels = labels.to(
    device
)
```

로 같은 Device에 둔다.

---

# 102. Conv2d에서 자주 보는 Arguments

```python
nn.Conv2d(
    in_channels,
    out_channels,
    kernel_size,
    stride=1,
    padding=0,
    dilation=1,
    groups=1,
    bias=True
)
```

---

# 103. 처음에는 이것부터

입문 단계에서는:

```text
in_channels

out_channels

kernel_size

stride

padding
```

을 먼저 이해하면 된다.

---

# 104. Dilation

Kernel Element 사이의 간격을 넓혀 Receptive Field를 확장할 수 있는 설정이다.

처음부터 복잡하게 사용할 필요는 없다.

---

# 105. Groups

Channel 연결 방식을 조정한다.

Depthwise Convolution 같은 구조에서 중요하게 사용될 수 있다.

처음에는 기본값:

```text
groups=1
```

을 이해하면 충분하다.

---

# 106. Batch Normalization

CNN에서 자주 볼 수 있다.

```python
nn.BatchNorm2d(
    32
)
```

---

# 107. 대표적인 Block

```text
Conv
↓
BatchNorm
↓
ReLU
```

같은 구조를 볼 수 있다.

---

# 108. BatchNorm 역할

Training 안정화 등에 도움을 줄 수 있다.

다만:

```text
model.train()

model.eval()
```

상태에 따라 동작이 달라지는 Layer라는 점이 중요하다.

---

# 109. 그래서 Validation에서 eval()

```python
model.eval()
```

이 중요하다.

Dropout뿐 아니라 BatchNorm 같은 Layer도 평가 모드의 영향을 받는다.

---

# 110. Dropout

Classifier 부분에서:

```python
nn.Dropout(
    0.5
)
```

같이 사용할 수도 있다.

---

# 111. Dropout

Training 중 일부 Activation을 Random하게 비활성화하는 Regularization 방법이다.

Overfitting을 줄이는 데 도움을 줄 수 있다.

---

# 112. Dropout도 train/eval이 다르다

Training:

```text
Dropout 동작
```

Evaluation:

```text
Dropout 비활성화
```

이다.

그래서 Validation:

```python
model.eval()
```

이 필요하다.

---

# 113. CNN Architecture

대표적인 CNN Architecture:

```text
LeNet

AlexNet

VGG

ResNet

DenseNet

EfficientNet
```

등이 있다.

---

# 114. LeNet

초기의 대표적인 CNN Architecture 중 하나이다.

CNN 기본 개념을 이해하기 좋은 구조이다.

---

# 115. AlexNet

Deep CNN의 가능성을 크게 보여준 대표적인 Architecture 중 하나이다.

---

# 116. VGG

반복적인 3×3 Convolution 구조로 유명하다.

구조가 비교적 이해하기 쉽다.

---

# 117. ResNet

Residual Connection을 사용한다.

대표적으로:

```text
ResNet18

ResNet34

ResNet50
```

등이 있다.

---

# 118. Residual Connection

개념적으로:

```text
x
↓
Layer
↓
F(x)

+

x
```

처럼 입력 정보를 Skip Connection으로 더한다.

---

# 119. 왜 사용할까?

깊은 Network의 Training을 돕는 중요한 아이디어이다.

---

# 120. DenseNet

Layer들이 이전 Feature와 강하게 연결되는 구조를 사용한다.

의료영상 관련 예제에서도 자주 볼 수 있는 Architecture 중 하나이다.

---

# 121. EfficientNet

Model 크기와 성능의 균형을 고려해 설계된 CNN 계열이다.

---

# 122. 직접 CNN vs Pretrained CNN

직접 CNN:

```text
Weight를 처음부터 학습
```

Pretrained CNN:

```text
이미 다른 대규모 Dataset에서 학습된 Weight 사용
```

이다.

---

# 123. 작은 Dataset

Dataset이 작을 때 처음부터 큰 CNN을 학습하면 Overfitting이 발생하기 쉽다.

이런 경우 Transfer Learning이 유용할 수 있다.

---

# 124. Transfer Learning

```text
Pretrained CNN
↓
Classifier 변경
↓
새 Dataset에 Training
```

하는 방식이다.

---

# 125. CNN Backbone

Pretrained Model에서 Image Feature를 추출하는 주요 부분을:

```text
Backbone
```

이라고 부르기도 한다.

---

# 126. Classifier

Backbone이 만든 Feature를 최종 Class로 변환한다.

```text
Image
↓
Backbone
↓
Feature
↓
Classifier
↓
Class
```

이다.

---

# 127. Freeze

Backbone Parameter를:

```python
param.requires_grad = False
```

로 설정하여 학습하지 않게 할 수 있다.

---

# 128. Classifier만 Training

```text
Backbone
→ Freeze

Classifier
→ Train
```

이다.

---

# 129. Fine-tuning

이후 Backbone 일부 또는 전체를 다시 학습 가능하게 만들고 작은 Learning Rate로 추가 Training할 수 있다.

---

# 130. CNN Feature Map과 Grad-CAM

CNN의 마지막 Convolution Layer에서 생성되는 Feature Map과 Gradient를 활용해 Model이 중요하게 본 영역을 시각화하는 방법 중 하나가:

```text
Grad-CAM
```

이다.

---

# 131. 왜 마지막 Conv Layer를 많이 사용할까?

Spatial 정보가 남아 있으면서 비교적 높은 수준의 Feature를 표현하기 때문이다.

---

# 132. Grad-CAM 예

```text
Original X-ray
+
Heatmap
↓
Model이 Prediction에 중요하게 사용한 영역 확인
```

이다.

---

# 133. 하지만 Grad-CAM은 정답지가 아니다

Heatmap이 특정 영역을 강조한다고 해서:

```text
Model이 의학적으로 올바른 이유로 판단했다
```

고 자동으로 결론 내릴 수 없다.

---

# 134. Failure Case와 함께 본다

```text
Correct Prediction

False Positive

False Negative

High-confidence Error
```

등에서 Grad-CAM을 비교할 수 있다.

---

# 135. CNN에서 Overfitting

Train 성능:

```text
99%
```

Validation:

```text
85%
```

처럼 차이가 커질 수 있다.

---

# 136. 해결 방향

예:

```text
Data Augmentation

Regularization

Dropout

Weight Decay

Early Stopping

Transfer Learning

더 많은 Data
```

등을 고려할 수 있다.

---

# 137. 하지만 무조건 하나를 적용하지 않는다

먼저:

```text
Train Loss

Validation Loss

Train Metric

Validation Metric

Failure Cases
```

를 확인한다.

---

# 138. CNN과 Class Imbalance

Image Dataset에서도 Class Imbalance가 있을 수 있다.

예:

```text
Class 0
25%

Class 1
75%
```

---

# 139. 대응

```text
Weighted Loss

Sampling

Threshold 조정

적절한 Evaluation Metric
```

등을 고려할 수 있다.

---

# 140. Accuracy만 보면 안 되는 이유

Majority Class가 많으면 높은 Accuracy가 나와도 Minority Class를 제대로 분류하지 못할 수 있다.

---

# 141. CNN Evaluation

Classification에서는:

```text
Accuracy

Precision

Recall

F1

ROC-AUC

PR-AUC

Confusion Matrix
```

등을 사용할 수 있다.

---

# 142. 의료영상에서는

특히:

```text
False Negative

False Positive
```

의 의미를 확인해야 한다.

---

# 143. CNN Debugging 1

먼저 Input Shape:

```python
print(
    images.shape
)
```

---

# 144. CNN Debugging 2

Model Output:

```python
print(
    logits.shape
)
```

---

# 145. CNN Debugging 3

Label:

```python
print(
    labels.shape
)

print(
    labels.dtype
)

print(
    labels.min()
)

print(
    labels.max()
)
```

---

# 146. CNN Debugging 4

Loss:

```python
loss = loss_fn(
    logits,
    labels
)


print(
    loss.item()
)
```

---

# 147. CNN Debugging 5

Backward:

```python
loss.backward()
```

오류가 없는지 확인한다.

---

# 148. One Batch Smoke Test

Full Training 전에:

```text
1 Batch
↓
Forward
↓
Loss
↓
Backward
↓
Optimizer Step
```

이 되는지 확인한다.

---

# 149. 예

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


model.train()


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


print(
    "loss:",
    loss.item()
)
```

---

# 150. 이게 되면

최소한:

```text
DataLoader

Input Shape

Model

Loss

Backward

Optimizer
```

가 기본적으로 연결된 것이다.

---

# 151. 그다음 전체 Epoch

```text
One Batch Test
↓
1 Epoch
↓
Validation
↓
여러 Epoch
```

순서로 확장한다.

---

# 152. CNN에서 Shape 읽는 습관

다음처럼 항상 적어보면 좋다.

```text
Input
[B, 3, 224, 224]

Conv
[B, 32, 224, 224]

Pool
[B, 32, 112, 112]

Conv
[B, 64, 112, 112]

Pool
[B, 64, 56, 56]

Adaptive Pool
[B, 64, 1, 1]

Flatten
[B, 64]

Linear
[B, 2]
```

---

# 153. CNN을 이해하는 가장 좋은 방법

코드를 볼 때:

```text
현재 Tensor Shape은 무엇인가?

다음 Layer가 무엇을 바꾸는가?
```

를 계속 추적한다.

---

# 154. Conv2d에서 주로 바뀌는 것

```text
Channel

Height

Width
```

이다.

---

# 155. ReLU

```text
Shape 유지
```

---

# 156. Pooling

주로:

```text
Height / Width 감소
```

---

# 157. Flatten

```text
Feature Map
↓
1차원 Feature Vector
```

---

# 158. Linear

```text
Feature
↓
Class Logit
```

---

# 159. CNN 한 줄 구조

```text
Image
→ Local Feature 추출
→ Feature 조합
→ Feature 요약
→ Classification
```

이다.

---

# 160. Image Classification 전체 연결

```text
Image File
↓
Transform
↓
Tensor
↓
DataLoader
↓
[B, C, H, W]
↓
CNN
↓
Feature Map
↓
Classifier
↓
Logits
↓
Loss
↓
Backward
↓
Optimizer
```

---

# 161. 핵심 용어 정리

```text
CNN
→ Image 처리에 많이 사용하는 Neural Network

Conv2d
→ 2D Convolution Layer

Kernel / Filter
→ Image의 Local Pattern을 계산하는 Weight

Feature Map
→ Convolution 결과

Stride
→ Filter 이동 간격

Padding
→ Image 주변에 값을 추가

Pooling
→ Spatial Size를 줄이는 연산

Receptive Field
→ Neuron이 영향을 받는 Input 영역

Flatten
→ Feature Map을 Vector로 변환

Classifier
→ Feature를 최종 Class Logit으로 변환

Backbone
→ 주요 Feature Extractor 부분
```

---

# 162. 가장 중요한 Shape

```text
CNN Input

[B, C, H, W]
```

이다.

---

# 163. 가장 중요한 Conv 설정

```python
nn.Conv2d(
    in_channels,
    out_channels,
    kernel_size,
    stride,
    padding
)
```

이다.

---

# 164. 가장 중요한 Shape 변화

대표적으로:

```text
[B, 3, 224, 224]

↓

[B, 32, 224, 224]

↓

[B, 32, 112, 112]

↓

[B, 64, 112, 112]

↓

[B, 64, 56, 56]

↓

[B, 64]

↓

[B, 2]
```

처럼 읽을 수 있어야 한다.

---

# 165. CNN 학습 원리

CNN도 결국:

```text
Forward
↓
Loss
↓
Backward
↓
Gradient
↓
Optimizer
↓
Weight Update
```

이다.

CNN에서 특별한 것은:

```text
Image의 공간적 구조를 활용하는
Convolution Layer
```

가 있다는 것이다.

---

# 166. 다음 단계

다음 파일:

```text
image-classification.md
```

에서는 지금까지 배운 내용을 하나로 합친다.

```text
Image Dataset

Label

Train / Validation Split

Transform

Dataset

DataLoader

CNN

Loss

Optimizer

Training

Validation

Prediction

Probability

Confusion Matrix
```

를 연결하여 실제 Image Classification Pipeline을 만든다.