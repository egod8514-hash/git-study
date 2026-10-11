# Chest X-ray Binary Classification Hackathon

## 1. 프로젝트 개요

이 프로젝트는 Chest X-ray Image를 이용하여 두 개의 Class를 구분하는 Binary Image Classification 프로젝트이다.

단순히 CNN Model을 한 번 학습하는 것이 아니라:

```text
EDA
↓
Baseline
↓
문제점 발견
↓
가설 설정
↓
Experiment
↓
Model 비교
↓
Failure Analysis
↓
Grad-CAM
↓
Test Inference
↓
Submission
↓
Leaderboard 결과 분석
```

까지 전체 Machine Learning / Deep Learning 실험 과정을 경험하는 것이 핵심이었다.

---

# 2. 프로젝트 문제

Input:

```text
Chest X-ray Image
```

Output:

```text
0
또는
1
```

인 Binary Classification 문제이다.

프로젝트에서는 다음과 같은 Label 구조를 사용했다.

```text
0 = Normal

1 = Pneumonia
```

---

# 3. Dataset 구조

주요 파일은:

```text
train.csv

test.csv

sample_submission.csv
```

였다.

그리고 실제 Image는 별도의 Train / Test Image Folder에 존재했다.

---

# 4. Train Data

Training Data에는:

```text
Image ID

Label
```

정보가 존재한다.

전체 Training Image는:

```text
5,216장
```

이었다.

---

# 5. Test Data

Test Image는:

```text
624장
```

이었다.

Test Data에는 정답 Label이 제공되지 않는다.

---

# 6. sample_submission.csv

Competition에서 제출해야 하는 결과 형식을 알려주는 File이다.

예:

```text
ID

Label
```

형태이다.

---

# 7. 중요한 구분

```text
test.csv
```

는:

```text
Test Prediction 결과
```

가 아니다.

---

# 8. test.csv란?

Prediction해야 하는 대상 Data이다.

```text
test.csv
↓
Model
↓
Prediction
↓
submission.csv
```

이다.

---

# 9. Dataset 확인

```python
import pandas as pd


train = pd.read_csv(
    "train.csv"
)


test = pd.read_csv(
    "test.csv"
)


submission = pd.read_csv(
    "sample_submission.csv"
)
```

---

# 10. Shape

```python
print(
    "train:",
    train.shape
)


print(
    "test:",
    test.shape
)


print(
    "submission:",
    submission.shape
)
```

---

# 11. Train Shape

프로젝트에서 Train Data는 약:

```text
5,216 × 2
```

구조였다.

---

# 12. Test Shape

Test Data는 약:

```text
624 × 1
```

구조였다.

---

# 13. 왜 Column 수가 다를까?

Train에는:

```text
정답 Label
```

이 있지만 Test에는 없기 때문이다.

---

# 14. Image 확인

프로젝트에서 사용한 Image는:

```text
224 × 224
```

크기로 처리했다.

---

# 15. Image Mode

원본 Image는:

```text
L
```

즉 Grayscale 형태였다.

---

# 16. PIL 확인

```python
from PIL import Image


image = Image.open(
    "image_path"
)


print(
    image.size
)


print(
    image.mode
)
```

예:

```text
(224, 224)

L
```

---

# 17. 왜 RGB로 바꿀까?

Pretrained CNN은 일반적으로:

```text
3 Channel RGB
```

Input을 기대한다.

따라서:

```python
image = image.convert(
    "RGB"
)
```

로 변환할 수 있다.

---

# 18. 주의

Grayscale을 RGB로 바꾼다고 새로운 Color 정보가 생기는 것은 아니다.

같은 Grayscale 정보를 3 Channel 형태로 맞추는 것이다.

---

# 19. Label Distribution

먼저 Label 분포를 확인했다.

```python
train[
    "label"
].value_counts()
```

실제 Column 이름은 Dataset에 맞게 사용한다.

---

# 20. 당시 Label Distribution

```text
Label 0
1,341

Label 1
3,875
```

이었다.

---

# 21. 비율

```text
Label 0
25.71%

Label 1
74.29%
```

정도였다.

---

# 22. 여기서 문제

두 Class의 Sample 수가 상당히 다르다.

즉:

```text
Class Imbalance
```

가 존재한다.

---

# 23. Accuracy 함정

Model이 Label 1을 지나치게 많이 Prediction해도 전체 Accuracy가 높게 나올 가능성이 있다.

따라서:

```text
Accuracy
```

만 보면 안 된다.

---

# 24. 함께 볼 Metric

```text
Accuracy

Precision

Recall

F1

Macro F1

ROC-AUC

Confusion Matrix

Class별 Recall
```

을 확인한다.

---

# 25. Image EDA

Model을 만들기 전에 Image 자체를 확인했다.

확인할 것:

```text
Image Size

Image Mode

Class Distribution

Brightness

Image Quality

Class별 차이
```

이다.

---

# 26. Brightness

Image의 평균 Pixel 값을 이용하여 밝기를 비교할 수 있다.

```python
import numpy as np


brightness = np.array(
    image
).mean()
```

---

# 27. 당시 평균 밝기

대략:

```text
Label 0
100.46

Label 1
83.36
```

으로 차이가 관찰되었다.

---

# 28. 이것이 중요한 이유

Model이 실제 폐렴 Pattern이 아니라:

```text
Image Brightness
```

를 이용해서 Class를 구분할 가능성이 있기 때문이다.

---

# 29. Shortcut Learning

Model이 우리가 원하는 의학적 Pattern이 아니라 더 쉬운 단서를 학습하는 현상을 생각할 수 있다.

예:

```text
Brightness

Image Border

Marker

Crop

Position

Hospital

Scanner
```

등이다.

---

# 30. 밝기 차이 발견의 의미

```text
Label 0과 Label 1의
평균 밝기가 다르다.
```

에서 끝나는 것이 아니다.

질문은:

```text
Model이 밝기를 Shortcut으로
사용하고 있는 것은 아닐까?
```

이다.

---

# 31. Train / Validation Split

Training Data를:

```text
Train

Validation
```

으로 분리했다.

---

# 32. 당시 Split

```text
Train
4,172

Validation
1,044
```

였다.

---

# 33. Class 비율

Train:

```text
Label 0
약 25.72%

Label 1
약 74.28%
```

Validation:

```text
Label 0
약 25.67%

Label 1
약 74.33%
```

수준으로 비슷하게 유지되었다.

---

# 34. Stratified Split

```python
from sklearn.model_selection import (
    train_test_split
)


train_df, val_df = train_test_split(
    train,
    test_size=0.2,
    random_state=42,
    stratify=train[
        "label"
    ]
)
```

---

# 35. stratify

Train과 Validation에서 Label 비율이 비슷하게 유지되도록 도와준다.

---

# 36. 의료영상 주의

같은 Patient의 여러 X-ray가 존재한다면 단순 Image-level Stratified Split보다:

```text
Patient-level Split
```

이 우선되어야 한다.

---

# 37. Baseline

처음에는 복잡한 실험보다 기본 Model을 만든다.

이것이:

```text
Baseline
```

이다.

---

# 38. Baseline의 역할

나중에:

```text
Fine-tuning

Augmentation

Weighted Loss
```

가 실제로 성능을 개선했는지 비교하는 기준이 된다.

---

# 39. Pretrained CNN

Image Classification에서는 Pretrained Model을 활용할 수 있다.

전체적인 구조는:

```text
Pretrained Backbone
↓
Feature Extraction
↓
Classifier
↓
2 Classes
```

이다.

---

# 40. Output

Binary Classification이지만 CrossEntropyLoss를 사용한다면:

```text
2 Logits
```

을 출력할 수 있다.

예:

```text
[Batch, 2]
```

---

# 41. Loss

```python
import torch.nn as nn


loss_fn = nn.CrossEntropyLoss()
```

---

# 42. Optimizer

Baseline에서는 Adam 계열 Optimizer를 사용할 수 있다.

```python
optimizer = torch.optim.Adam(
    model.parameters(),
    lr=learning_rate
)
```

---

# 43. 당시 Model Parameter

전체 Parameter는 약:

```text
11,177,538
```

개였다.

---

# 44. Device

```python
import torch


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

# 45. 프로젝트 GPU

Local 환경에서:

```text
NVIDIA GeForce RTX 5070
```

GPU를 사용해 학습할 수 있었다.

---

# 46. Batch Shape

한 Batch는:

```text
[32, 3, 224, 224]
```

형태였다.

---

# 47. 의미

```text
32
→ Batch Size

3
→ RGB Channel

224
→ Height

224
→ Width
```

이다.

---

# 48. Training

기본 Training Loop는:

```text
model.train()
↓
Batch
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

# 49. Validation

```text
model.eval()
↓
torch.no_grad()
↓
Forward
↓
Prediction
↓
Metric
```

이다.

---

# 50. Baseline 결과

Baseline에서도 높은 Validation 성능이 관찰되었다.

예를 들어 초기 Epoch에서:

```text
Accuracy
약 0.98 이상

F1
약 0.98 이상

ROC-AUC
약 0.99 수준
```

의 높은 결과가 나타났다.

---

# 51. 당시 한 Baseline 결과

예:

```text
Validation Loss
0.0399

Accuracy
0.9895

ROC-AUC
0.9988

F1
0.9929
```

수준의 결과가 관찰되었다.

---

# 52. 높은 Validation 성능

처음 보면:

```text
Model이 거의 완벽하다.
```

라고 생각할 수 있다.

하지만 바로 그렇게 결론 내리면 안 된다.

---

# 53. 왜?

최종 Leaderboard에서는:

```text
약 0.915
```

수준의 점수가 나왔다.

---

# 54. Validation과 Test 차이

```text
Validation
약 0.99

Leaderboard
약 0.915
```

처럼 차이가 발생했다.

---

# 55. 이것이 중요한 이유

Local Validation 성능이 높다고 실제 Test에서도 같은 성능이 보장되는 것은 아니다.

---

# 56. 가능한 원인

```text
Validation Overfitting

Data Distribution Difference

Patient Leakage

Duplicate Image

Shortcut Learning

Train / Test Domain Shift

Brightness Difference

Label Noise
```

등을 검토해야 한다.

---

# 57. 실험 전략

Baseline 이후 다음과 같은 가설을 세웠다.

```text
가설 1

단계적 Fine-tuning과
낮은 Learning Rate를 사용하면

Pretrained Feature를 크게 훼손하지 않으면서
일반화 성능을 개선할 수 있을 것이다.
```

---

# 58. 가설 2

```text
밝기와 위치 변화에 대한
Data Augmentation을 적용하면

Model이 특정 Image 조건에 과도하게 의존하는 것을 줄이고
강건성을 높일 수 있을 것이다.
```

---

# 59. 추가 가설

```text
Class Imbalance 때문에
Minority Class 성능이 떨어질 수 있으므로

Weighted Loss를 사용하면
Class 간 균형을 개선할 수 있을 것이다.
```

---

# 60. 실험의 핵심

무작정 여러 기법을 넣는 것이 아니라:

```text
문제 발견
↓
가설
↓
실험
↓
결과
↓
해석
```

순서로 진행한다.

---

# 61. Experiment A - Fine-tuning

Pretrained Model 전체를 처음부터 크게 변경하지 않고 단계적으로 학습한다.

---

# 62. Stage 1

먼저 Backbone을 Freeze하고 Classifier만 학습한다.

```text
Backbone
Frozen

Classifier
Trainable
```

---

# 63. Freeze

```python
for param in model.parameters():

    param.requires_grad = False
```

그 후 Classifier를 새로 설정하면 해당 Layer는 Trainable하게 만들 수 있다.

---

# 64. 당시 Stage 1

Trainable Parameter는 약:

```text
1,026
```

개 수준이었다.

---

# 65. 의미

전체 1천만 개 이상의 Parameter를 모두 학습하는 대신 아주 작은 Classifier 부분만 먼저 학습한 것이다.

---

# 66. Stage 1 결과

예:

```text
Epoch 2

Accuracy
0.9435

F1
0.9631
```

수준이었다.

---

# 67. Baseline보다 낮다고 실패인가?

반드시 그렇지는 않다.

Stage 1은:

```text
최종 Model
```

이라기보다 Fine-tuning을 안정적으로 시작하기 위한 단계일 수 있다.

---

# 68. Stage 2

일부 Backbone Layer를 다시 Unfreeze한다.

```text
Classifier
Trainable

Backbone 일부
Trainable
```

---

# 69. Learning Rate

Fine-tuning에서는 Pretrained Feature를 크게 훼손하지 않도록 낮은 Learning Rate를 사용할 수 있다.

---

# 70. 핵심

```text
Freeze
↓
Classifier 학습
↓
일부 Layer Unfreeze
↓
낮은 LR
↓
Fine-tuning
```

이다.

---

# 71. Experiment B - Augmentation

Training Image에 변형을 적용했다.

목적은:

```text
Image Brightness

위치

작은 변형
```

에 Model이 지나치게 의존하는 것을 줄이는 것이다.

---

# 72. 예

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

        transforms.RandomRotation(
            5
        ),

        transforms.ColorJitter(
            brightness=0.1,
            contrast=0.1
        ),

        transforms.ToTensor()
    ]
)
```

실제 실험 설정에 맞게 변형 강도는 조절한다.

---

# 73. 의료영상 Augmentation

무조건 강하게 적용하지 않는다.

---

# 74. 질문

```text
이 변환이 실제 X-ray에서 가능한가?

병변의 의미를 훼손하지 않는가?

좌우 방향이 중요한가?

너무 많이 Crop하지 않는가?
```

를 확인한다.

---

# 75. Experiment B 결과

Augmentation 실험에서 한 결과는:

```text
Epoch 1

Accuracy
0.9559

F1
0.9695
```

수준이었다.

---

# 76. 이후

Epoch가 증가하면서:

```text
Validation Loss 상승
```

이 관찰되기도 했다.

---

# 77. 의미

Training을 오래한다고 무조건 Generalization이 좋아지는 것은 아니다.

---

# 78. Best Epoch

Validation Loss 또는 목표 Metric을 기준으로 가장 좋은 Epoch를 저장한다.

---

# 79. Experiment C - Weighted Loss

Class Imbalance를 고려하기 위해 Class Weight를 적용했다.

---

# 80. Training Class Count

당시 Train Split에서:

```text
Class 0
1,073

Class 1
3,099
```

였다.

---

# 81. Class Weight

계산된 Weight 예:

```text
Class 0
1.9441

Class 1
0.6731
```

이었다.

---

# 82. 의미

Sample이 적은 Class 0의 Error에 더 큰 Weight를 주는 방식이다.

---

# 83. Weighted CrossEntropyLoss

```python
import torch


class_weights = torch.tensor(
    [
        1.9441,
        0.6731
    ],
    dtype=torch.float32
).to(
    device
)


loss_fn = nn.CrossEntropyLoss(
    weight=class_weights
)
```

---

# 84. Weighted Loss의 목적

```text
Minority Class를 틀렸을 때
Loss를 더 크게 반영
```

하여 Model이 Minority Class도 중요하게 학습하도록 한다.

---

# 85. Experiment C 결과

Epoch 1:

```text
Accuracy
0.9741

F1
0.9824

Macro F1
0.9667
```

---

# 86. Epoch 2

```text
Accuracy
0.9856

F1
0.9903

Macro F1
0.9813
```

수준의 좋은 결과가 나타났다.

---

# 87. Macro F1

Macro F1은 각 Class의 F1을 동일한 비중으로 평균한다.

따라서 Class Imbalance가 있는 상황에서 전체 Accuracy보다 Class 균형을 보기 좋다.

---

# 88. Weighted Loss의 의미

```text
Accuracy가 높아졌는가?
```

만 보지 않는다.

---

# 89. 함께 확인

```text
Recall 0

Recall 1

Macro F1

Confusion Matrix
```

를 확인한다.

---

# 90. 왜 Recall 0?

Class 0이 Minority이므로 Model이 Class 0을 제대로 찾아내는지 확인해야 한다.

---

# 91. Baseline의 한 비교 결과

예:

```text
Recall 0
0.9851

Recall 1
0.9910
```

수준의 결과도 관찰되었다.

---

# 92. 실험 비교

모든 실험을 같은 표에 정리한다.

```text
Baseline

Fine-tuning

Augmentation

Weighted Loss

Fine-tuning + Augmentation

기타 결합 실험
```

---

# 93. 비교표 예

```text
Experiment       Val Loss   Accuracy   F1      Macro F1

Baseline         0.0399     0.9895     0.9929  ...

Fine-tuning      ...        ...        ...     ...

Augmentation     ...        0.9559     0.9695  ...

Weighted Loss    ...        0.9856     0.9903  0.9813
```

---

# 94. 가장 높은 Accuracy만 선택?

반드시 그렇지는 않다.

---

# 95. 함께 고려

```text
Validation Loss

Accuracy

Macro F1

Class별 Recall

ROC-AUC

Learning Curve

Failure Case

Test 성능
```

을 함께 본다.

---

# 96. 실험 공정성

Model을 비교할 때는 가능한 한:

```text
같은 Train / Validation Split

같은 Metric

같은 Seed

같은 Epoch 기준

같은 Evaluation Code
```

를 사용한다.

---

# 97. 한 번에 여러 조건을 바꾸지 않는다

예:

```text
Model 변경

Augmentation 변경

LR 변경

Loss 변경
```

을 동시에 하면 무엇 때문에 성능이 변했는지 알기 어렵다.

---

# 98. 좋은 실험

```text
Baseline
↓
한 가지 변경
↓
비교
↓
다음 변경
```

이다.

---

# 99. Failure Analysis

성능 숫자만 보는 것에서 끝나지 않았다.

Model이 틀린 Image를 직접 확인했다.

---

# 100. Error 종류

```text
True 0 → Pred 1

True 1 → Pred 0
```

을 각각 확인한다.

---

# 101. Probability

Prediction뿐 아니라:

```text
P(Class 1)
```

도 확인했다.

---

# 102. High-confidence Error

당시 사례 중:

```text
train_0770

True
0

Pred
1

P(Class 1)
1.0
```

에 가까운 High-confidence Error가 있었다.

---

# 103. 왜 중요할까?

Model이:

```text
잘 모르겠지만 틀린 것
```

이 아니라:

```text
매우 확신하면서 틀린 것
```

이기 때문이다.

---

# 104. 질문

```text
Label 문제인가?

Image가 특이한가?

Model이 Shortcut을 보고 있는가?

실제로 애매한 X-ray인가?
```

를 확인할 수 있다.

---

# 105. Low-confidence Error

다른 사례:

```text
train_1329

True
0

Pred
1

P(Class 1)
약 0.5106
```

였다.

---

# 106. 의미

Decision Boundary 근처에서 Model도 확신하지 못한 상태로 틀린 경우이다.

---

# 107. 반대 Error

예:

```text
train_1380

True
1

Pred
0

P(Class 1)
약 0.3912
```

같은 사례도 확인했다.

---

# 108. Correct Case

정상적으로 맞춘 사례도 비교했다.

예:

```text
train_1018

True
0

P(Class 1)
거의 0
```

이었다.

---

# 109. 왜 Correct Case도 볼까?

Error Image만 보는 것보다:

```text
맞을 때 어디를 보는가?

틀릴 때 어디를 보는가?
```

를 비교할 수 있기 때문이다.

---

# 110. Grad-CAM

Model이 Image의 어느 부분을 중요하게 보고 Prediction했는지 분석하기 위해 Grad-CAM을 사용했다.

---

# 111. Target Layer

당시 Target Layer는 CNN의 후반부 Convolution Layer를 사용했다.

예:

```text
Conv2d(
    512,
    512,
    kernel_size=3,
    stride=1,
    padding=1
)
```

형태의 Layer였다.

---

# 112. 왜 뒤쪽 Conv Layer?

앞쪽 Layer는:

```text
Edge

Texture
```

같은 낮은 수준의 Feature를 주로 학습한다.

뒤쪽 Layer는 상대적으로 더 고수준 Feature를 표현한다.

---

# 113. Grad-CAM 결과에서 볼 것

```text
폐 영역을 보고 있는가?

Image 가장자리를 보고 있는가?

문자 Marker를 보고 있는가?

특정 밝은 부분만 보고 있는가?

병변으로 생각할 수 있는 영역을 보고 있는가?
```

등이다.

---

# 114. 하지만

Grad-CAM은:

```text
의학적 병변 위치의 정답
```

이 아니다.

---

# 115. Grad-CAM의 의미

```text
Model Prediction에
상대적으로 영향을 준 Image 영역
```

을 시각적으로 탐색하는 방법이다.

---

# 116. Grad-CAM ≠ 진단 근거 증명

Heatmap이 폐 영역에 있다고 해서:

```text
Model이 의학적으로 올바르게 판단했다.
```

는 것을 증명하지 않는다.

---

# 117. Failure Analysis + Grad-CAM

두 방법을 같이 사용하면:

```text
어떤 Image에서 틀렸는가?
+
어디를 보고 틀렸는가?
```

를 탐색할 수 있다.

---

# 118. Test Inference

최종 Model을 이용하여 624개의 Test Image를 Prediction했다.

---

# 119. Test Loader

```python
test_loader = DataLoader(
    test_dataset,
    batch_size=32,
    shuffle=False
)
```

---

# 120. 왜 shuffle=False?

Prediction 결과와 Test ID 순서를 유지하기 위해서이다.

---

# 121. Inference

```python
model.eval()


all_probs = []
all_preds = []


with torch.no_grad():

    for images in test_loader:

        images = images.to(
            device
        )


        logits = model(
            images
        )


        probs = torch.softmax(
            logits,
            dim=1
        )[
            :,
            1
        ]


        preds = (
            probs
            >=
            0.5
        ).long()


        all_probs.extend(
            probs.cpu().numpy()
        )


        all_preds.extend(
            preds.cpu().numpy()
        )
```

실제 Dataset이 `(image, id)` 등을 반환한다면 구조에 맞게 수정한다.

---

# 122. Test Prediction Distribution

당시 624개 Test Image에 대한 Prediction은:

```text
Class 0
184

Class 1
440
```

이었다.

---

# 123. 비율

```text
Class 0
29.49%

Class 1
70.51%
```

정도였다.

---

# 124. Train과 비교

Train:

```text
Class 0
25.71%

Class 1
74.29%
```

Test Prediction:

```text
Class 0
29.49%

Class 1
70.51%
```

였다.

---

# 125. 이것만으로 좋은 Model인가?

아니다.

Distribution이 비슷하다는 것은 Sanity Check일 뿐 정확한 Prediction을 증명하지 않는다.

---

# 126. Probability 분석

Test의 Class 1 Probability도 확인했다.

---

# 127. 당시 평균

```text
Mean
약 0.7091
```

이었다.

---

# 128. Median

```text
약 0.9996
```

수준이었다.

---

# 129. 의미

많은 Sample에서 Model이 매우 극단적인 Probability를 출력했다.

---

# 130. Model Confidence

각 Sample의 Prediction Class Probability를:

```text
Confidence
```

라고 볼 수 있다.

---

# 131. 당시 평균 Confidence

약:

```text
0.9695
```

였다.

---

# 132. Minimum Confidence

약:

```text
0.5191
```

수준이었다.

---

# 133. 이것이 의미하는 것

Model이 Test Data 대부분에 매우 높은 확신을 보였다.

---

# 134. 그런데

Leaderboard 성능은 약:

```text
0.915
```

였다.

---

# 135. 중요한 질문

```text
Model은 매우 자신 있는데
왜 실제 Accuracy는 더 낮은가?
```

이다.

---

# 136. Overconfidence

Model Probability가 실제 정확성보다 지나치게 확신하는 상태를 생각할 수 있다.

---

# 137. Calibration

향후:

```text
Reliability Diagram

Brier Score

Temperature Scaling
```

등으로 Probability Calibration을 평가할 수 있다.

---

# 138. Submission 생성

Prediction 결과를 `sample_submission.csv` 형식에 맞춘다.

```python
submission = pd.read_csv(
    "sample_submission.csv"
)


submission[
    "label"
] = all_preds


submission.to_csv(
    "submission.csv",
    index=False
)
```

실제 Target Column 이름은 Sample Submission을 확인한다.

---

# 139. 제출 전 확인

```python
print(
    submission.shape
)


display(
    submission.head()
)


print(
    submission[
        "label"
    ].value_counts()
)
```

---

# 140. 반드시 확인

```text
Row 수가 Test와 같은가?

ID 순서가 맞는가?

Column 이름이 맞는가?

Label 값이 0/1인가?

Index가 저장되지 않았는가?
```

를 확인한다.

---

# 141. `index=False`

```python
to_csv(
    ...,
    index=False
)
```

를 사용하는 이유는 불필요한 Pandas Index Column이 제출 파일에 들어가는 것을 막기 위해서이다.

---

# 142. Leaderboard

최종 Submission을 제출했을 때:

```text
약 0.91506
```

수준의 Score를 확인했다.

---

# 143. 평가 Metric

대회의 평가 산식은:

```text
Accuracy
```

였다.

---

# 144. 0.91506의 의미

대략적으로 Test 정답과 제출 Prediction이:

```text
91.5%
```

정도 일치했다는 의미로 이해할 수 있다.

정확한 해석은 Competition의 평가 방식과 공개/비공개 Test 구조를 함께 확인한다.

---

# 145. 중요한 경험

Local Validation에서는:

```text
약 98~99%
```

수준이 나왔지만 Leaderboard에서는:

```text
약 91.5%
```

였다.

---

# 146. 이 차이가 더 중요한 공부

단순히:

```text
Validation Accuracy가 높다.
```

에서 끝나는 것이 아니라:

```text
왜 새로운 Data에서 성능이 떨어졌는가?
```

를 고민하게 된다.

---

# 147. Data Leakage 재확인

높은 Validation 성능과 Leaderboard 차이가 크다면 Leakage 가능성도 다시 확인한다.

---

# 148. 확인할 것

```text
같은 Patient가 Train / Validation에 있는가?

Duplicate Image가 있는가?

파일 이름에 Label 정보가 있는가?

전처리 과정에서 Validation 정보가 들어갔는가?

Train과 Validation이 지나치게 비슷한가?

Image Source가 같은가?
```

이다.

---

# 149. Brightness Leakage

앞에서 Class별 Brightness 차이가 있었기 때문에:

```text
Brightness가 Label Proxy 역할을 하는가?
```

도 확인할 필요가 있다.

---

# 150. Domain Shift

Train과 Test가 다른:

```text
Hospital

Scanner

Image Processing

Patient Population

촬영 조건
```

을 가지고 있다면 성능이 떨어질 수 있다.

---

# 151. Generalization

Model의 진짜 목표는:

```text
Training Data를 외우는 것
```

이 아니라:

```text
보지 못한 새로운 Data에서도
잘 Prediction하는 것
```

이다.

---

# 152. Experiment 결과 해석

실험 성능이 Baseline보다 조금 높다고 바로:

```text
개선 성공
```

이라고 판단하지 않는다.

---

# 153. 확인

```text
차이가 충분히 큰가?

Seed를 바꿔도 유지되는가?

다른 Fold에서도 유지되는가?

Leaderboard에서도 개선되는가?
```

를 확인한다.

---

# 154. Mission 구조

프로젝트를 Mission 단위로 정리하면 다음과 같다.

```text
MISSION 0
전체 Workflow 설계

MISSION 1
Baseline

MISSION 2
첫 번째 개선 Experiment

MISSION 3
추가 개선

MISSION 4
Failure Analysis / Grad-CAM

MISSION 5
Test Inference / Submission / Reflection
```

---

# 155. Mission 0

전체 흐름:

```text
EDA
↓
Split
↓
Baseline
↓
문제점
↓
가설
↓
Experiment A
↓
Experiment B
↓
Experiment C
↓
결합 실험
↓
성능 비교
↓
최종 Model
↓
Failure Case
↓
Grad-CAM
↓
Test Inference
```

를 먼저 설계한다.

---

# 156. 왜 Mission 구조가 좋을까?

Model Code만 남기는 것이 아니라:

```text
왜 이 실험을 했는가?
```

를 기록할 수 있기 때문이다.

---

# 157. 프로젝트에서 배운 점 1

Model Training 전에:

```text
Data Shape

Label Distribution

Image Size

Image Mode

Brightness
```

를 확인한다.

---

# 158. 배운 점 2

Class Imbalance가 존재하면:

```text
Accuracy
```

만 보지 않는다.

---

# 159. 배운 점 3

Baseline을 먼저 만든다.

Baseline이 있어야 개선 여부를 판단할 수 있다.

---

# 160. 배운 점 4

실험은:

```text
문제
↓
가설
↓
변경
↓
결과
↓
해석
```

순서로 진행한다.

---

# 161. 배운 점 5

Fine-tuning에서는:

```text
Freeze
↓
Classifier Training
↓
일부 Unfreeze
↓
낮은 Learning Rate
```

전략을 사용할 수 있다.

---

# 162. 배운 점 6

Augmentation은 무조건 성능을 높이지 않는다.

너무 강하면 의료영상의 의미를 훼손할 수 있다.

---

# 163. 배운 점 7

Class Imbalance에서는 Weighted Loss를 사용할 수 있다.

---

# 164. 배운 점 8

Weighted Loss 적용 후에는:

```text
Macro F1

Class별 Recall
```

을 같이 확인한다.

---

# 165. 배운 점 9

Validation 성능이 높다고 Test 성능이 보장되지 않는다.

---

# 166. 배운 점 10

Failure Case를 직접 본다.

---

# 167. 배운 점 11

특히:

```text
High-confidence Error
```

는 중요한 분석 대상이다.

---

# 168. 배운 점 12

Grad-CAM은 Model이 어느 영역을 활용했는지 탐색하는 도구이지 의학적 정답을 증명하는 도구가 아니다.

---

# 169. 배운 점 13

Test Inference에서는:

```text
shuffle=False
```

로 Prediction 순서를 유지한다.

---

# 170. 배운 점 14

Submission은:

```text
Model Prediction
```

을 Competition이 요구하는 파일 형식으로 바꾸는 과정이다.

---

# 171. 배운 점 15

Leaderboard Score가 Local Validation보다 낮다면 단순히 Model을 더 복잡하게 만들기 전에:

```text
Leakage

Domain Shift

Split

Shortcut Learning

Duplicate

Calibration
```

을 다시 확인한다.

---

# 172. 전체 Workflow

```text
Train CSV
5,216
+
Train Images
↓
EDA
↓
Label 0 = 1,341
Label 1 = 3,875
↓
Class Imbalance 확인
↓
Brightness 분석
↓
Train 4,172
Validation 1,044
↓
Baseline
↓
높은 Validation 성능
↓
문제점 분석
↓
Fine-tuning
↓
Augmentation
↓
Weighted Loss
↓
Experiment 비교
↓
Best Model
↓
Failure Analysis
↓
High-confidence Error
↓
Grad-CAM
↓
Test Images
624
↓
Inference
↓
Pred 0 = 184
Pred 1 = 440
↓
Submission
↓
Leaderboard
약 0.91506
↓
Validation ↔ Test Gap 분석
```

---

# 173. 이 프로젝트에서 가장 중요한 비교

```text
Local Validation
≈ 0.98 ~ 0.99

Leaderboard
≈ 0.915
```

이다.

---

# 174. 이것이 알려준 것

```text
높은 Validation Score
=
실전에서도 반드시 높은 Score
```

가 아니다.

---

# 175. 더 좋은 질문

```text
왜 Validation에서는 잘했는데
새로운 Test에서는 떨어졌을까?
```

이다.

---

# 176. 앞으로 개선한다면

```text
Patient-level Split 확인

Duplicate Detection

Stratified Group CV

Brightness / Source Bias 분석

Stronger but medically valid Augmentation

Different Pretrained Backbone

Learning Rate Tuning

Weight Decay

Early Stopping

Weighted Loss 비교

Threshold Validation

Ensemble

Test Time Augmentation

Probability Calibration

External Validation
```

등을 검토할 수 있다.

---

# 177. 하지만 우선순위

무조건:

```text
더 큰 Model
```

부터 사용하는 것은 아니다.

---

# 178. 먼저

```text
Data Leakage가 없는가?

Validation Split이 현실적인가?

Train / Test Distribution이 다른가?

Model이 Shortcut을 배우는가?
```

를 확인한다.

---

# 179. 프로젝트를 한 문장으로 설명

이 프로젝트는:

```text
5,216장의 Chest X-ray Training Image를 이용하여
Normal과 Pneumonia를 분류하는
Binary Image Classification Model을 구축하고,

Baseline 이후
Fine-tuning, Data Augmentation,
Weighted Loss를 비교하면서

Class Imbalance와 Generalization 문제를 분석하고,

Failure Case와 Grad-CAM을 이용해
Model Prediction을 해석한 뒤

624장의 Test Image를 Prediction하여
Competition Submission까지 수행한
End-to-End Computer Vision 프로젝트
```

이다.

---

# 180. 포트폴리오에서 강조할 부분

단순히:

```text
CNN을 만들었습니다.
```

라고 설명하지 않는다.

---

# 181. 더 좋은 설명

```text
Chest X-ray Binary Classification에서
Class Imbalance와 Train/Test Generalization 문제를 분석했습니다.

Baseline을 구축한 후
Fine-tuning, Augmentation, Weighted Loss를
독립적인 실험으로 비교했고,

Accuracy뿐 아니라
F1, Macro F1, ROC-AUC,
Class별 Recall을 함께 평가했습니다.

또한 High-confidence Failure Case와
Grad-CAM을 이용해 Model의 판단 Pattern을 분석하고,

최종적으로 Test Inference와
Competition Submission까지 수행했습니다.
```

라고 설명할 수 있다.

---

# 182. 이 프로젝트의 핵심 교훈

가장 중요한 것은:

```text
Validation Accuracy를
0.99까지 만들었다.
```

가 아니다.

진짜 중요한 것은:

```text
Validation은 매우 높았다.

↓

그런데 Leaderboard는 약 0.915였다.

↓

왜 차이가 생겼을까?

↓

Split을 다시 본다.

↓

Leakage를 의심한다.

↓

Brightness를 본다.

↓

Class Imbalance를 본다.

↓

Failure Case를 본다.

↓

Grad-CAM을 본다.

↓

Generalization 문제를 생각한다.
```

라는 과정이다.

---

# 183. 프로젝트 최종 흐름

```text
Data
↓
EDA
↓
Baseline
↓
Evaluation
↓
Problem Discovery
↓
Hypothesis
↓
Experiment
↓
Comparison
↓
Failure Analysis
↓
Explainability
↓
Test Inference
↓
Submission
↓
Real-world Performance Gap
↓
Reflection
```

이 흐름이 앞으로 다른 AI 프로젝트를 진행할 때도 반복해서 사용할 수 있는 기본 구조이다.