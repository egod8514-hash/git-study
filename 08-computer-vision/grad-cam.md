# Grad-CAM

## 1. Grad-CAM이란?

Grad-CAM은:

```text
Gradient-weighted
Class Activation Mapping
```

의 약자이다.

CNN 기반 Image Classification Model이 특정 Class를 Prediction할 때 어떤 Image 영역이 Prediction에 상대적으로 영향을 주었는지 시각적으로 살펴보는 방법이다.

결과는 보통:

```text
Original Image
+
Heatmap
```

형태로 표현한다.

---

# 2. 왜 Grad-CAM을 사용할까?

Model의 최종 Prediction만 보면:

```text
Prediction = Pneumonia
Probability = 0.98
```

이라는 결과는 알 수 있다.

하지만:

```text
Image의 어느 부분이
이 Prediction과 관련되었는가?
```

는 알기 어렵다.

Grad-CAM은 이 부분을 분석하는 데 도움을 준다.

---

# 3. 기본 흐름

```text
Image
↓
CNN
↓
Feature Map
↓
Target Class Score
↓
Gradient 계산
↓
Feature Map 중요도 계산
↓
Heatmap 생성
↓
Original Image에 Overlay
```

---

# 4. 중요한 개념

Grad-CAM을 이해하려면 다음 네 가지가 중요하다.

```text
Feature Map

Target Layer

Target Class

Gradient
```

---

# 5. Feature Map

CNN의 Convolution Layer는 Image에서 Feature를 추출한다.

예:

```text
Input Image
[3, 224, 224]

↓

Conv Layer

↓

Feature Maps
[512, 7, 7]
```

처럼 여러 Feature Map이 생성될 수 있다.

---

# 6. Feature Map은 무엇인가?

Feature Map은 CNN이 입력 Image에서 추출한 특징 표현이다.

초기 Layer에서는 비교적 단순한:

```text
Edge

Line

Texture
```

등을 표현할 수 있고,

더 깊은 Layer에서는 앞선 Feature를 조합한 더 복잡한 표현을 학습할 수 있다.

---

# 7. Target Layer

Grad-CAM에서는 어떤 Layer의 Feature Map을 사용할지 선택해야 한다.

이를:

```text
Target Layer
```

라고 한다.

---

# 8. 왜 마지막 Convolution Layer를 많이 사용할까?

마지막 쪽 Convolution Layer는:

```text
공간 정보
+
비교적 높은 수준의 Feature
```

를 함께 가지고 있기 때문이다.

그래서 Grad-CAM에서는 마지막 Convolution Layer를 Target Layer로 많이 선택한다.

---

# 9. 하지만 무조건 마지막 Layer?

아니다.

Model Architecture와 분석 목적에 따라 다른 Layer를 선택할 수도 있다.

---

# 10. ResNet18 예

ResNet18에서는 마지막 Convolution Block 쪽을 Target으로 사용할 수 있다.

예:

```python
target_layer = (
    model.layer4[-1].conv2
)
```

---

# 11. Target Class

Grad-CAM은:

```text
어떤 Class에 대한 설명인가?
```

를 정해야 한다.

예:

```text
Class 0 = Normal

Class 1 = Pneumonia
```

이라면 Class 1에 대한 Grad-CAM을 만들 수 있다.

---

# 12. Prediction Class를 Target으로

Model이 Prediction한 Class를 사용할 수도 있다.

```python
pred_class = logits.argmax(
    dim=1
).item()
```

---

# 13. True Class를 Target으로 볼 수도 있다

예:

```text
True = 0

Pred = 1
```

인 Error Image에서:

```text
Predicted Class 1 Grad-CAM

vs

True Class 0 Grad-CAM
```

을 비교하는 분석도 가능하다.

---

# 14. Gradient

Gradient는 Output Score가 어떤 내부 Feature 변화에 얼마나 민감한지를 계산하는 데 사용된다.

Grad-CAM에서는:

```text
Target Class Score
```

에 대한:

```text
Feature Map의 Gradient
```

를 이용한다.

---

# 15. 직관적으로 이해

어떤 Feature Map이 Target Class Prediction에 중요한 역할을 했다면 해당 Feature Map과 Target Score 사이의 Gradient 정보를 이용해 중요도를 계산할 수 있다.

---

# 16. Grad-CAM 핵심 아이디어

개념적으로:

```text
Feature Map
+
Target Class Gradient
↓
각 Feature Map의 중요도 계산
↓
Weighted Combination
↓
Heatmap
```

이다.

---

# 17. 수식 개념

Target Class를:

```text
c
```

라고 하자.

Feature Map을:

```text
Aᵏ
```

라고 하면 각 Feature Map의 Weight를 Gradient의 Spatial Average로 계산한다.

개념:

```text
αₖᶜ
=
Average(
∂yᶜ / ∂Aᵏ
)
```

이다.

---

# 18. 의미

```text
Target Class Score가
Feature Map k에 얼마나 민감한가?
```

를 나타내는 Weight로 생각할 수 있다.

---

# 19. Feature Map 결합

각 Feature Map에 Weight를 곱한다.

```text
α₁ × Feature Map 1

+

α₂ × Feature Map 2

+

...

↓

Class Activation Map
```

---

# 20. ReLU

Grad-CAM에서는 일반적으로 마지막에 ReLU를 적용한다.

```text
Grad-CAM
=
ReLU(
Weighted Feature Maps
)
```

Target Class에 양의 방향으로 기여하는 영역을 강조하기 위한 것이다.

---

# 21. Heatmap

결과는 작은 Spatial Map일 수 있다.

예:

```text
7 × 7
```

이다.

---

# 22. 원본 크기로 확대

원본 Image가:

```text
224 × 224
```

라면 Heatmap도:

```text
224 × 224
```

로 Resize해서 Overlay할 수 있다.

---

# 23. 전체 원리

```text
Image
↓
Forward
↓
Feature Map 저장
↓
Target Class Score 선택
↓
Backward
↓
Gradient 저장
↓
Gradient Average
↓
Channel별 Weight
↓
Feature Map Weighted Sum
↓
ReLU
↓
Normalize
↓
Resize
↓
Overlay
```

---

# 24. Hook

PyTorch에서는 중간 Layer의 값이나 Gradient를 가져오기 위해:

```text
Hook
```

을 사용할 수 있다.

---

# 25. Forward Hook

Forward 과정에서 특정 Layer의 Output을 저장할 수 있다.

```python
def forward_hook(
    module,
    input,
    output
):

    activations.append(
        output
    )
```

---

# 26. Activation

여기서 저장되는 Layer Output을:

```text
Activation
```

이라고 부를 수 있다.

Grad-CAM에서는 Target Layer의 Feature Map으로 사용한다.

---

# 27. Gradient 저장

Target Layer Output에 Hook을 걸어 Gradient를 저장할 수도 있다.

---

# 28. 간단한 Grad-CAM Class

```python
import torch
import torch.nn.functional as F


class GradCAM:

    def __init__(
        self,
        model,
        target_layer
    ):

        self.model = model

        self.target_layer = (
            target_layer
        )

        self.activations = None

        self.gradients = None


        self.forward_handle = (
            target_layer.register_forward_hook(
                self._save_activation
            )
        )


        self.backward_handle = (
            target_layer.register_full_backward_hook(
                self._save_gradient
            )
        )


    def _save_activation(
        self,
        module,
        input,
        output
    ):

        self.activations = (
            output.detach()
        )


    def _save_gradient(
        self,
        module,
        grad_input,
        grad_output
    ):

        self.gradients = (
            grad_output[0].detach()
        )


    def remove_hooks(
        self
    ):

        self.forward_handle.remove()

        self.backward_handle.remove()
```

---

# 29. Model 준비

예:

```python
model.eval()
```

Grad-CAM 분석에서는 Model Weight를 학습시키려는 것이 아니다.

---

# 30. 중요한 점

Grad-CAM에서는 Gradient가 필요하다.

따라서 전체 Forward를:

```python
with torch.no_grad():
```

안에 넣으면 안 된다.

---

# 31. 왜?

`torch.no_grad()`는 Gradient 계산을 비활성화하기 때문이다.

Grad-CAM은:

```text
Gradient
```

를 사용해야 한다.

---

# 32. 일반 Inference와 차이

일반 Inference:

```python
model.eval()


with torch.no_grad():

    logits = model(
        image
    )
```

Grad-CAM:

```python
model.eval()


logits = model(
    image
)


score = logits[
    0,
    target_class
]


score.backward()
```

이다.

---

# 33. Grad-CAM 계산 함수

Class에 다음 Method를 추가할 수 있다.

```python
def generate(
    self,
    image,
    target_class=None
):

    self.model.zero_grad()


    logits = self.model(
        image
    )


    if target_class is None:

        target_class = (
            logits.argmax(
                dim=1
            ).item()
        )


    score = logits[
        0,
        target_class
    ]


    score.backward()


    weights = (
        self.gradients.mean(
            dim=(
                2,
                3
            ),
            keepdim=True
        )
    )


    cam = (
        weights
        *
        self.activations
    ).sum(
        dim=1,
        keepdim=True
    )


    cam = F.relu(
        cam
    )


    cam = F.interpolate(
        cam,
        size=image.shape[
            2:
        ],
        mode="bilinear",
        align_corners=False
    )


    cam = cam[
        0,
        0
    ]


    cam = cam - cam.min()


    if cam.max() > 0:

        cam = (
            cam
            /
            cam.max()
        )


    return (
        cam.cpu(),
        target_class,
        logits.detach().cpu()
    )
```

---

# 34. 전체 Class

```python
class GradCAM:

    def __init__(
        self,
        model,
        target_layer
    ):

        self.model = model

        self.target_layer = (
            target_layer
        )

        self.activations = None

        self.gradients = None


        self.forward_handle = (
            target_layer.register_forward_hook(
                self._save_activation
            )
        )


        self.backward_handle = (
            target_layer.register_full_backward_hook(
                self._save_gradient
            )
        )


    def _save_activation(
        self,
        module,
        input,
        output
    ):

        self.activations = (
            output.detach()
        )


    def _save_gradient(
        self,
        module,
        grad_input,
        grad_output
    ):

        self.gradients = (
            grad_output[0].detach()
        )


    def generate(
        self,
        image,
        target_class=None
    ):

        self.model.zero_grad()


        logits = self.model(
            image
        )


        if target_class is None:

            target_class = (
                logits.argmax(
                    dim=1
                ).item()
            )


        score = logits[
            0,
            target_class
        ]


        score.backward()


        weights = (
            self.gradients.mean(
                dim=(
                    2,
                    3
                ),
                keepdim=True
            )
        )


        cam = (
            weights
            *
            self.activations
        ).sum(
            dim=1,
            keepdim=True
        )


        cam = F.relu(
            cam
        )


        cam = F.interpolate(
            cam,
            size=image.shape[
                2:
            ],
            mode="bilinear",
            align_corners=False
        )


        cam = cam[
            0,
            0
        ]


        cam = (
            cam
            -
            cam.min()
        )


        if cam.max() > 0:

            cam = (
                cam
                /
                cam.max()
            )


        return (
            cam.cpu(),
            target_class,
            logits.detach().cpu()
        )


    def remove_hooks(
        self
    ):

        self.forward_handle.remove()

        self.backward_handle.remove()
```

---

# 35. Target Layer 설정

ResNet18 예:

```python
target_layer = (
    model.layer4[-1].conv2
)
```

---

# 36. Grad-CAM 객체

```python
grad_cam = GradCAM(
    model,
    target_layer
)
```

---

# 37. Image Shape

Grad-CAM에 넣는 Image는 Batch Dimension이 필요하다.

```text
[C, H, W]
```

가 아니라:

```text
[1, C, H, W]
```

이다.

---

# 38. Batch Dimension 추가

```python
image = image.unsqueeze(
    0
)
```

---

# 39. GPU 이동

```python
image = image.to(
    device
)
```

---

# 40. Grad-CAM 생성

```python
cam, target_class, logits = (
    grad_cam.generate(
        image
    )
)
```

---

# 41. Prediction Probability

```python
probs = torch.softmax(
    logits,
    dim=1
)


print(
    probs
)
```

---

# 42. Target Class

```python
print(
    target_class
)
```

---

# 43. CAM Shape

```python
print(
    cam.shape
)
```

예:

```text
torch.Size([224, 224])
```

---

# 44. Heatmap 시각화

```python
import matplotlib.pyplot as plt


plt.imshow(
    cam.numpy(),
    cmap="jet"
)


plt.axis(
    "off"
)


plt.show()
```

---

# 45. Heatmap 색상

보통 시각화에서:

```text
따뜻한 색
→ 상대적으로 높은 CAM 값

차가운 색
→ 상대적으로 낮은 CAM 값
```

으로 표현한다.

하지만 색상 자체가 의학적 의미를 가지는 것은 아니다.

---

# 46. Original Image 준비

Normalize된 Tensor를 그대로 보여주면 색이나 밝기가 이상할 수 있다.

시각화용 원본 Image를 별도로 보관하는 것이 좋다.

---

# 47. Grayscale X-ray

예:

```python
plt.imshow(
    original_image,
    cmap="gray"
)
```

---

# 48. Overlay

```python
plt.imshow(
    original_image,
    cmap="gray"
)


plt.imshow(
    cam.numpy(),
    cmap="jet",
    alpha=0.4
)


plt.axis(
    "off"
)


plt.show()
```

---

# 49. alpha

```python
alpha=0.4
```

는 Heatmap의 투명도를 조절한다.

---

# 50. 왜 Overlay?

Heatmap만 보면 실제 해부학적 위치를 파악하기 어렵다.

원본 Image와 겹치면:

```text
Model Activation이
어느 Image 위치에 나타나는가?
```

를 더 쉽게 볼 수 있다.

---

# 51. Hook 제거

분석이 끝나면:

```python
grad_cam.remove_hooks()
```

를 호출할 수 있다.

---

# 52. 왜 Hook을 제거할까?

같은 Model에 Hook을 계속 추가하면 의도하지 않게 Hook이 중복 등록될 수 있다.

---

# 53. Grad-CAM 분석 대상

Random Image 몇 장만 보는 것보다 의미 있는 Case를 선택한다.

대표적으로:

```text
Correct Positive

Correct Negative

False Positive

False Negative

High-confidence Error

Low-confidence Error
```

를 비교한다.

---

# 54. Correct Positive

예:

```text
True = 1

Pred = 1
```

이다.

---

# 55. Correct Negative

```text
True = 0

Pred = 0
```

이다.

---

# 56. False Positive

```text
True = 0

Pred = 1
```

이다.

---

# 57. False Negative

```text
True = 1

Pred = 0
```

이다.

---

# 58. High-confidence Error

예:

```text
True = 0

Pred = 1

P(Class 1) = 0.999
```

이다.

---

# 59. 왜 중요한가?

Model이 매우 강하게 잘못된 판단을 내린 Case이기 때문이다.

---

# 60. Low-confidence Error

예:

```text
True = 0

Pred = 1

P(Class 1) = 0.51
```

이다.

Decision Boundary 근처에 있는 Sample일 가능성을 생각해볼 수 있다.

---

# 61. Correct와 Error를 비교한다

예:

```text
Correct Positive
→ 폐 영역 중심

False Positive
→ Image Border 중심
```

같은 반복 Pattern이 보일 수 있다.

---

# 62. Shortcut Learning 의심

Model이 실제 병변보다:

```text
Text Marker

Image Border

촬영 장비 흔적

Padding

Position

Brightness
```

등을 반복적으로 사용한다면 Shortcut Learning 가능성을 의심할 수 있다.

---

# 63. Shortcut Learning이란?

Model이 우리가 기대하는 의미 있는 Feature 대신 Dataset에 우연히 존재하는 쉬운 단서를 학습하는 현상이다.

---

# 64. 예

```text
Class 1 Image에만
특정 문자 Marker가 많음
```

이라면 Model이 병변 대신 Marker를 이용할 수 있다.

---

# 65. 높은 Accuracy인데도 문제가 될 수 있다

```text
Validation Accuracy = 99%
```

여도 Model이 실제 병변이 아니라 Dataset Shortcut을 사용했다면 새로운 환경에서 성능이 떨어질 수 있다.

---

# 66. Grad-CAM으로 발견 가능?

도움이 될 수 있다.

하지만 Grad-CAM 하나만으로 Shortcut Learning 여부를 확정할 수는 없다.

---

# 67. 반복 Pattern을 확인한다

한 장이 아니라 여러 Image에서:

```text
같은 위치를 반복해서 보는가?

Border에 집중하는가?

Text Marker에 집중하는가?

Class별로 Pattern이 다른가?
```

를 확인한다.

---

# 68. Grad-CAM 해석 주의

가장 중요한 원칙:

```text
Heatmap이 빨갛다
=
그곳에 질병이 있다
```

가 아니다.

---

# 69. Grad-CAM이 보여주는 것

Grad-CAM은 해당 Target Class Score와 관련된 Model 내부 Activation을 공간적으로 시각화하는 방법이다.

---

# 70. Grad-CAM이 보여주지 않는 것

Grad-CAM만으로:

```text
정확한 병변 위치

의학적 인과관계

진단의 확정 근거

Model의 모든 판단 과정
```

을 보장할 수 없다.

---

# 71. Localization과 Classification

Classification Model은:

```text
Image 전체의 Class
```

를 Prediction하도록 Training된다.

병변 위치를 정확하게 Annotation하도록 Training된 Model이 아니다.

---

# 72. 따라서

Grad-CAM Heatmap은:

```text
Segmentation Mask
```

와 다르다.

---

# 73. Segmentation Mask

Segmentation은 Pixel 단위로:

```text
병변

배경
```

등을 분류하는 별도의 Task이다.

---

# 74. Grad-CAM Resolution

마지막 Feature Map이:

```text
7 × 7
```

이라면 이를:

```text
224 × 224
```

로 확대한다.

따라서 Heatmap은 비교적 거칠 수 있다.

---

# 75. 작은 병변

아주 작은 병변을 정확한 Pixel 수준으로 보여주는 용도로 Grad-CAM을 과신하면 안 된다.

---

# 76. Target Layer에 따라 결과가 달라질 수 있다

초기 Layer:

```text
Spatial Resolution 높음

Feature 수준 낮음
```

후반 Layer:

```text
Spatial Resolution 낮음

Feature 수준 높음
```

인 경향이 있다.

---

# 77. 그래서 Layer 선택도 실험 요소

```text
어떤 Layer를 Target으로 선택했는가?
```

를 기록해야 한다.

---

# 78. Model Architecture마다 다르다

ResNet:

```text
layer4
```

DenseNet:

```text
features
```

EfficientNet:

```text
features
```

등 구조가 다르다.

---

# 79. 먼저 Model 구조 확인

```python
print(
    model
)
```

한다.

---

# 80. 잘못된 Target Layer

Classifier인:

```python
model.fc
```

를 Grad-CAM Target Layer로 선택하는 것은 일반적인 Grad-CAM 목적과 맞지 않는다.

공간 Feature Map을 가지는 Convolution Layer를 선택한다.

---

# 81. Custom CNN

예:

```python
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

    nn.ReLU()
)
```

라면 마지막 Conv Layer를 Target으로 선택할 수 있다.

---

# 82. 예

```python
target_layer = (
    model.features[3]
)
```

단, 실제 Index는 Model 구조를 확인한 후 결정한다.

---

# 83. Grad-CAM 전에 Prediction 확인

먼저:

```text
True Label

Predicted Label

Probability
```

를 확인한다.

---

# 84. 이유

Heatmap만 보면 어떤 Class에 대한 Grad-CAM인지 해석하기 어렵기 때문이다.

---

# 85. 좋은 Figure 제목

예:

```text
True: Normal
Pred: Pneumonia
P(Pneumonia): 0.998
```

처럼 표시한다.

---

# 86. 예

```python
plt.title(
    f"True: {true_label} | "
    f"Pred: {pred_label} | "
    f"P1: {prob_1:.3f}"
)
```

---

# 87. 여러 Case 비교

예:

```text
1. Correct Normal

2. Correct Pneumonia

3. False Positive

4. False Negative
```

를 같은 기준으로 분석한다.

---

# 88. 단순히 예쁜 Heatmap을 만들기 위한 것이 아니다

Grad-CAM의 목적은:

```text
Model Behavior Analysis
```

이다.

---

# 89. Evaluation과 연결

```text
Confusion Matrix
↓
Error Sample 추출
↓
Image 확인
↓
Grad-CAM
↓
반복되는 실패 Pattern 분석
```

이 좋은 흐름이다.

---

# 90. Fine-tuning 전후 비교

Grad-CAM을 이용해:

```text
Stage 1

vs

Stage 2 Fine-tuning
```

의 Model Activation Pattern을 비교할 수도 있다.

---

# 91. 질문

```text
Fine-tuning 후
Model이 더 관련 있어 보이는 영역에
집중하는가?
```

를 탐색할 수 있다.

---

# 92. 하지만 성능도 함께 본다

Grad-CAM이 보기 좋아졌다고 해서 실제 Model 성능이 좋아졌다는 뜻은 아니다.

반드시:

```text
Accuracy

Recall

F1

ROC-AUC

Confusion Matrix
```

등과 함께 확인한다.

---

# 93. Augmentation 전후 비교

```text
Baseline

vs

Augmentation Model
```

의 Grad-CAM Pattern도 비교할 수 있다.

---

# 94. 가설

예:

```text
Baseline Model이
Image Border에 과도하게 집중한다.
```

---

# 95. Experiment

적절한 전처리나 Data 구성 변경을 적용한다.

---

# 96. 다시 분석

```text
Metric 변화

+

Grad-CAM 변화

+

Failure Case 변화
```

를 함께 본다.

---

# 97. Grad-CAM과 Label Error

High-confidence Error를 확인했는데 Heatmap이 의미 있어 보이는 영역에 집중한다면:

```text
Label 자체가 잘못되었을 가능성
```

도 하나의 점검 대상이 될 수 있다.

하지만 Heatmap만으로 Label Error라고 단정하지 않는다.

---

# 98. Data Quality 분석

Error Image에서:

```text
잘못된 Label

손상된 Image

너무 어두운 Image

잘못된 Crop

중복 Image

방향 문제
```

등도 확인한다.

---

# 99. Grad-CAM은 Data Debugging에도 도움

Model Error를 따라가다 보면 Model 문제뿐 아니라 Dataset 문제를 발견할 수도 있다.

---

# 100. Grad-CAM과 Confidence

다음 Case들을 비교하면 유용하다.

```text
High-confidence Correct

Low-confidence Correct

High-confidence Error

Low-confidence Error
```

---

# 101. High-confidence Correct

```text
True = 1

Pred = 1

Confidence = 0.999
```

---

# 102. Low-confidence Correct

```text
True = 1

Pred = 1

Confidence = 0.52
```

---

# 103. High-confidence Error

```text
True = 0

Pred = 1

Confidence = 0.999
```

---

# 104. Low-confidence Error

```text
True = 0

Pred = 1

Confidence = 0.51
```

---

# 105. 이렇게 비교하는 이유

단순한:

```text
맞음 / 틀림
```

뿐 아니라:

```text
Model이 얼마나 확신했는가?
```

까지 함께 볼 수 있기 때문이다.

---

# 106. Binary 2-Logit

```python
probs = torch.softmax(
    logits,
    dim=1
)
```

를 사용한다.

---

# 107. Binary 1-Logit

`BCEWithLogitsLoss` Model이라면:

```python
prob = torch.sigmoid(
    logits
)
```

를 사용한다.

---

# 108. Grad-CAM Target Score도 Model 구조에 맞춰야 한다

2-Logit Model:

```python
score = logits[
    0,
    target_class
]
```

---

# 109. 1-Logit Model

Output 구조가 다르므로 동일한 Indexing 코드를 그대로 사용하면 안 된다.

Model Output Shape를 먼저 확인한다.

---

# 110. Shape 확인

```python
print(
    logits.shape
)
```

---

# 111. Grad-CAM Debugging 1

오류:

```text
Gradient가 None
```

이면:

```text
torch.no_grad()를 사용했는가?

Target Layer Hook이 제대로 등록됐는가?

Target Score에서 backward()를 했는가?
```

를 확인한다.

---

# 112. Debugging 2

```text
Activation이 None
```

이면 Target Layer가 Forward 과정에서 실제로 사용되는 Layer인지 확인한다.

---

# 113. Debugging 3

Heatmap이 전부 0에 가깝다면:

```text
Target Class

Target Layer

Gradient

ReLU 이후 값
```

을 확인한다.

---

# 114. Debugging 4

Shape를 출력한다.

```python
print(
    grad_cam.activations.shape
)


print(
    grad_cam.gradients.shape
)
```

예:

```text
[1, 512, 7, 7]
```

이다.

---

# 115. Debugging 5

Image가 GPU에 있고 Model이 CPU에 있으면 Device Error가 발생한다.

```python
print(
    image.device
)


print(
    next(
        model.parameters()
    ).device
)
```

를 확인한다.

---

# 116. Debugging 6

Batch Size가 여러 개인 경우 단순 구현의:

```python
target_class = logits.argmax(
    dim=1
).item()
```

은 하나의 Sample만 가정한다.

입문 단계에서는 Grad-CAM 분석을:

```text
Batch Size = 1
```

로 하는 것이 이해하기 쉽다.

---

# 117. Debugging 7

Normalize된 Image를 시각화할 때 원본과 다르게 보일 수 있다.

Grad-CAM 입력용 Tensor와 시각화용 Original Image를 구분해서 보관하면 편하다.

---

# 118. Debugging 8

Hook을 반복해서 등록했다면:

```python
remove_hooks()
```

를 호출하고 다시 구성한다.

---

# 119. Library 사용

Grad-CAM을 직접 구현하는 이유는 원리를 이해하기 위해서이다.

실제 Project에서는 검증된 Library를 사용할 수도 있다.

---

# 120. 대표적인 방식

예를 들어 `pytorch-grad-cam` 같은 Library가 사용될 수 있다.

설치 환경과 Version에 따라 API는 달라질 수 있으므로 공식 사용법을 확인한다.

---

# 121. 직접 구현의 장점

```text
Activation이 무엇인지

Gradient가 왜 필요한지

Target Layer가 무엇인지

CAM이 어떻게 만들어지는지
```

를 이해할 수 있다.

---

# 122. Library의 장점

```text
구현 편의성

다양한 CAM 방법

Model 지원

Visualization 기능
```

등이 있다.

---

# 123. 다른 CAM 계열 방법

Grad-CAM 외에도:

```text
Grad-CAM++

Score-CAM

LayerCAM
```

등 여러 방법이 있다.

---

# 124. 하지만 먼저 Grad-CAM

입문 단계에서는 Grad-CAM의 원리를 이해하는 것이 좋다.

---

# 125. Grad-CAM과 Explainable AI

Grad-CAM은:

```text
Explainable AI

XAI
```

분석 방법 중 하나로 활용된다.

---

# 126. SHAP과 차이

Tabular Model에서 사용했던 SHAP은 Feature별 기여를 분석하는 데 활용할 수 있다.

Grad-CAM은 CNN Image Model의 공간적 Activation을 시각화하는 데 활용된다.

---

# 127. 개념 비교

```text
SHAP
→ Feature Contribution

Grad-CAM
→ Image Spatial Activation
```

정도로 우선 이해할 수 있다.

---

# 128. 둘 다 주의

둘 모두:

```text
Model Interpretation
```

을 돕지만:

```text
인과관계의 증명
```

은 아니다.

---

# 129. 의료 AI에서 특히 주의

Grad-CAM Heatmap이 병변 위치와 겹쳐 보이더라도 그것만으로:

```text
Model이 의학적으로 올바른 추론을 했다.
```

고 단정할 수 없다.

---

# 130. 전문가 검토

의료영상에서는 가능하다면:

```text
영상의학 전문가

임상의

Domain Expert
```

의 검토와 정량적 평가가 필요할 수 있다.

---

# 131. Ground Truth Localization

병변 위치 Annotation이 있다면 Grad-CAM과 실제 Annotation을 비교하는 추가 분석을 생각할 수 있다.

---

# 132. 하지만 Grad-CAM은 Detection Model이 아니다

Classification Model에서 생성된 Heatmap을 정확한 Bounding Box나 Segmentation Mask처럼 취급하면 안 된다.

---

# 133. 발표에서 좋은 표현

```text
Grad-CAM을 이용하여
Model Prediction과 관련된 주요 활성화 영역을
정성적으로 확인하였다.
```

---

# 134. 조심해야 하는 표현

```text
Grad-CAM으로 Model이
정확히 병변을 보고 있다는 것을 증명했다.
```

처럼 지나치게 강하게 표현하지 않는다.

---

# 135. Failure Case 발표

예:

```text
High-confidence False Positive에서
Grad-CAM이 폐 영역보다 Image 외곽에 집중하는
Pattern이 관찰되었다.
```

라고 표현할 수 있다.

---

# 136. 그다음

```text
외곽 정보가 Prediction에 영향을 주는지
추가 검증이 필요하다.
```

라고 연결할 수 있다.

---

# 137. Grad-CAM → 새로운 가설

```text
Grad-CAM
↓
Border 집중 발견
↓
Shortcut Learning 가설
↓
Data / Crop / Padding 점검
↓
새 Experiment
↓
다시 Evaluation
```

이다.

---

# 138. Grad-CAM을 몇 장만 보면?

대표 Case를 이해하는 데는 도움이 되지만 Dataset 전체 Model Behavior를 대표한다고 단정하기 어렵다.

---

# 139. 여러 Sample 확인

```text
각 Class

Correct

Error

Confidence 수준

촬영 조건
```

별로 여러 Sample을 확인한다.

---

# 140. 자동 저장

Project에서는 Grad-CAM 결과를 파일로 저장할 수 있다.

예:

```text
gradcam/

correct_positive/

correct_negative/

false_positive/

false_negative/
```

처럼 구분할 수 있다.

---

# 141. 파일명에 정보 추가

예:

```text
true0_pred1_prob0.998_image123.png
```

처럼 저장하면 Error Analysis가 편리할 수 있다.

---

# 142. 원본 ID도 보존

```text
Patient ID

Image ID

File Name
```

등 분석에 필요한 식별 정보를 별도 DataFrame에 보존할 수 있다.

단, 실제 의료정보를 다룰 때는 개인정보 및 보안 정책을 준수해야 한다.

---

# 143. Grad-CAM 분석표

예:

```text
Image ID

True Label

Pred Label

Probability

Confidence

Error Type

Grad-CAM Observation
```

을 기록할 수 있다.

---

# 144. Example

```text
image_001

True
0

Pred
1

P1
0.998

Type
False Positive

Observation
Upper border activation
```

처럼 정리할 수 있다.

---

# 145. Model Comparison

Baseline과 Fine-tuned Model을 같은 Image에서 비교할 수 있다.

```text
Same Image
↓
Baseline Grad-CAM
↓
Fine-tuned Grad-CAM
```

---

# 146. 중요한 조건

비교할 때:

```text
같은 Image

같은 Class

같은 Visualization 기준
```

을 사용하면 해석이 더 명확하다.

---

# 147. Grad-CAM과 Model Performance

최종 Model 선택을 Grad-CAM만으로 하지 않는다.

```text
Validation Metric

Generalization

Error Analysis

Grad-CAM

Domain Knowledge
```

를 종합한다.

---

# 148. 좋은 Computer Vision 분석 흐름

```text
Training
↓
Validation Metrics
↓
Confusion Matrix
↓
Failure Case 추출
↓
Original Image 확인
↓
Grad-CAM
↓
Hypothesis
↓
새 Experiment
```

이다.

---

# 149. Grad-CAM 핵심 코드

```python
logits = model(
    image
)


score = logits[
    0,
    target_class
]


score.backward()
```

---

# 150. Gradient Weight

```python
weights = (
    gradients.mean(
        dim=(
            2,
            3
        ),
        keepdim=True
    )
)
```

---

# 151. Weighted Feature Map

```python
cam = (
    weights
    *
    activations
).sum(
    dim=1,
    keepdim=True
)
```

---

# 152. ReLU

```python
cam = F.relu(
    cam
)
```

---

# 153. Resize

```python
cam = F.interpolate(
    cam,
    size=image.shape[
        2:
    ],
    mode="bilinear",
    align_corners=False
)
```

---

# 154. Normalize

```python
cam = (
    cam
    -
    cam.min()
)


cam = (
    cam
    /
    (
        cam.max()
        +
        1e-8
    )
)
```

---

# 155. 핵심 정리

```text
Forward
→ Feature Map 저장

Target Class 선택

Backward
→ Gradient 저장

Gradient Average
→ Feature Map Weight

Weighted Sum

ReLU

Resize

Normalize

Overlay
```

이다.

---

# 156. 꼭 기억할 것

```text
Grad-CAM
≠
병변 위치 정답

Grad-CAM
≠
인과관계 증명

Grad-CAM
≠
Model 전체 사고 과정
```

이다.

---

# 157. Grad-CAM이 유용한 이유

그럼에도 Grad-CAM은:

```text
Model Failure 분석

Shortcut Learning 탐색

Correct/Error 비교

Fine-tuning 전후 비교

Model Behavior 이해
```

에 유용하게 사용할 수 있다.

---

# 158. 실전 Checklist

Grad-CAM을 만들기 전에:

```text
Model은 eval() 상태인가?

Target Layer가 맞는가?

Input Shape은 [1,C,H,W]인가?

Model과 Image Device가 같은가?

torch.no_grad()를 사용하지 않았는가?

Target Class를 확인했는가?
```

---

# 159. 생성 후 Checklist

```text
True Label을 표시했는가?

Pred Label을 표시했는가?

Probability를 표시했는가?

Original Image와 Overlay했는가?

FP/FN을 구분했는가?

High-confidence Error를 확인했는가?

여러 Sample에서 반복 Pattern을 봤는가?
```

---

# 160. 한 줄 정리

Grad-CAM은:

```text
CNN의 Feature Map과
Target Class Gradient를 이용하여
Prediction과 관련된 Image 영역을
Heatmap 형태로 시각화하는 방법
```

이다.

단:

```text
Heatmap을
의학적 정답이나
인과관계로 해석해서는 안 된다.
```

---

# 161. 다음 단계

다음 파일:

```text
medical-image-ai.md
```

에서는 지금까지 공부한 Computer Vision 전체 내용을 의료영상 AI에 연결한다.

다음 내용을 다룬다.

```text
X-ray

CT

MRI

DICOM

2D / 3D Image

Patient-level Split

Data Leakage

Class Imbalance

Domain Shift

Shortcut Learning

Sensitivity / Specificity

Grad-CAM

의료 AI 평가 주의점
```

그리고:

```text
좋은 Validation Accuracy
=
좋은 의료 AI
```

가 왜 아닌지도 정리한다.