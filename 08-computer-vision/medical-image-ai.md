# Medical Image AI

## 1. 의료영상 AI란?

의료영상 AI는 X-ray, CT, MRI 등의 의료영상을 AI Model이 분석하도록 만드는 분야이다.

예:

```text
Chest X-ray
↓
CNN
↓
Normal / Pneumonia Classification
```

또는:

```text
MRI
↓
Deep Learning Model
↓
질환 Grade Classification
```

처럼 사용할 수 있다.

---

# 2. 대표적인 의료영상

대표적으로:

```text
X-ray

CT

MRI

Ultrasound

Pathology Image
```

등이 있다.

각 영상은 생성 방식과 Data 특성이 다르기 때문에 같은 Image라고 해서 완전히 동일하게 처리할 수 있는 것은 아니다.

---

# 3. X-ray

X-ray는 신체를 통과하는 X선을 이용해 만들어지는 영상이다.

예:

```text
Chest X-ray
```

에서는:

```text
폐

심장

뼈

횡격막
```

등의 구조를 볼 수 있다.

---

# 4. X-ray와 AI

예:

```text
Chest X-ray
↓
CNN
↓
Normal / Pneumonia
```

같은 Classification 문제를 만들 수 있다.

---

# 5. X-ray는 2D Image

일반적인 Chest X-ray는 하나의 2D Image로 다룰 수 있다.

예:

```text
[H, W]
```

또는 PyTorch Tensor:

```text
[1, H, W]
```

이다.

---

# 6. Pretrained Model을 사용할 때

ImageNet Pretrained Model은 보통 RGB 3-Channel Input을 기대한다.

따라서 Grayscale X-ray를:

```text
L
↓
RGB
```

로 변환해:

```text
[3, H, W]
```

형태로 사용할 수도 있다.

---

# 7. 주의

Grayscale을 RGB로 바꾼다고 새로운 색상 정보가 생기는 것은 아니다.

같은 Grayscale 정보를 여러 Channel에 맞춰 표현하는 것이다.

---

# 8. CT

CT는 여러 단면 Image를 이용해 인체 내부를 표현한다.

예:

```text
Slice 1
Slice 2
Slice 3
...
Slice N
```

처럼 여러 Slice가 하나의 Volume을 구성할 수 있다.

---

# 9. CT와 2D

CT의 특정 Slice만 사용하면:

```text
2D Classification
```

문제로 만들 수 있다.

예:

```text
Abdominal CT Slice
↓
CNN
↓
Organ Classification
```

---

# 10. CT와 3D

여러 Slice를 함께 사용하면:

```text
3D Volume
```

으로 분석할 수도 있다.

예:

```text
[D, H, W]
```

형태의 Data이다.

---

# 11. 3D CNN

3D Data에서는:

```python
nn.Conv3d
```

등을 사용하는 Model도 생각할 수 있다.

하지만:

```text
GPU Memory

Training 시간

Data 처리

Model 복잡도
```

가 크게 증가할 수 있다.

---

# 12. 2D와 3D는 다른 문제

```text
2D CNN
→ 한 Slice 중심

3D CNN
→ 여러 Slice의 공간적 관계까지 활용 가능
```

이다.

어떤 방법이 더 좋은지는 문제와 Data에 따라 다르다.

---

# 13. MRI

MRI는 자기장과 고주파 신호 등을 이용하여 인체 조직을 영상화한다.

MRI는 여러 Sequence를 가질 수 있다.

예:

```text
T1

T2

FLAIR
```

등이다.

---

# 14. MRI에서 중요한 점

단순히:

```text
MRI Image
```

라고만 생각하지 않고:

```text
어떤 Sequence인가?

어떤 Plane인가?

어떤 Slice인가?
```

를 확인해야 한다.

---

# 15. 의료영상의 Plane

대표적으로:

```text
Axial

Coronal

Sagittal
```

이 있다.

---

# 16. Axial

몸을 위와 아래 방향으로 나누는 단면으로 이해할 수 있다.

---

# 17. Coronal

몸을 앞과 뒤 방향으로 나누는 단면이다.

---

# 18. Sagittal

몸을 왼쪽과 오른쪽 방향으로 나누는 단면이다.

---

# 19. 왜 중요할까?

같은 장기라도 Plane에 따라 보이는 형태가 다르기 때문이다.

---

# 20. PNG/JPEG와 의료영상

일반 Computer Vision Dataset에서는:

```text
PNG

JPEG
```

등을 많이 사용한다.

하지만 실제 의료영상에서는:

```text
DICOM
```

형식이 많이 사용된다.

---

# 21. DICOM

DICOM은:

```text
Digital Imaging and
Communications in Medicine
```

의 약자이다.

의료영상과 관련 Metadata를 함께 다룰 수 있는 표준이다.

---

# 22. DICOM에는 무엇이 있을까?

영상 Pixel뿐 아니라 다양한 Metadata가 포함될 수 있다.

예:

```text
촬영 정보

장비 정보

Pixel 관련 정보

영상 위치

Slice 정보
```

등이다.

실제 포함 항목은 파일에 따라 다르다.

---

# 23. DICOM을 단순 Image 파일처럼만 보면 안 되는 이유

의료영상에서는 Pixel 값의 의미가 일반 PNG/JPEG와 다를 수 있고 Metadata도 분석에 중요할 수 있기 때문이다.

---

# 24. DICOM Library

Python에서는 대표적으로:

```python
import pydicom
```

을 사용할 수 있다.

---

# 25. 기본 읽기

```python
import pydicom


ds = pydicom.dcmread(
    "sample.dcm"
)


print(
    ds
)
```

---

# 26. Pixel Data

```python
image = ds.pixel_array


print(
    image.shape
)


print(
    image.dtype
)
```

---

# 27. 일반 Image와 차이

일반 Image에서 흔히:

```text
uint8

0 ~ 255
```

를 생각하지만 의료영상은 반드시 이 범위라고 가정하면 안 된다.

---

# 28. 그래서 확인

```python
print(
    image.min()
)


print(
    image.max()
)
```

처럼 실제 Pixel 범위를 확인한다.

---

# 29. CT와 HU

CT에서는 Hounsfield Unit:

```text
HU
```

개념이 중요하다.

조직마다 X-ray 감쇠 특성이 다르기 때문에 CT 값이 조직 특성과 연결된다.

---

# 30. Windowing

CT에서는 관심 조직을 잘 보기 위해 특정 값 범위를 강조하는:

```text
Windowing
```

을 사용할 수 있다.

---

# 31. 왜 Windowing?

하나의 CT에는 매우 넓은 값 범위가 존재할 수 있다.

따라서:

```text
폐

뼈

연부조직
```

등 관심 조직에 따라 적절한 범위를 보는 방식이 달라질 수 있다.

---

# 32. AI에서도 중요

CT를 단순히:

```text
0 ~ 255 Image
```

로 변환하기 전에 어떤 Windowing과 Scaling을 사용할지 고려해야 한다.

---

# 33. 의료영상 AI Task

대표적인 Task는:

```text
Classification

Detection

Segmentation

Regression
```

등이다.

---

# 34. Classification

Image 전체에 Class를 Prediction한다.

예:

```text
Chest X-ray
↓
Normal / Pneumonia
```

---

# 35. Detection

Image에서 Object 또는 병변의 위치를 Bounding Box로 찾는다.

예:

```text
Lesion
↓
Bounding Box
```

---

# 36. Segmentation

Pixel 단위로 영역을 분류한다.

예:

```text
Tumor Region

Lung Region
```

등이다.

---

# 37. Regression

연속적인 값을 Prediction하는 문제도 만들 수 있다.

예:

```text
특정 측정값 Prediction
```

등이다.

---

# 38. Classification과 Localization은 다르다

Classification Model:

```text
이 Image가 어떤 Class인가?
```

를 학습한다.

Detection / Segmentation:

```text
어디에 있는가?
```

까지 학습한다.

---

# 39. Grad-CAM은 Segmentation이 아니다

Classification Model의 Grad-CAM Heatmap을 병변의 정확한 Pixel Mask처럼 사용하면 안 된다.

---

# 40. 의료영상 Dataset 구조

예:

```text
dataset/

├── patient_001/
│   ├── image_001.png
│   ├── image_002.png
│   └── image_003.png
│
├── patient_002/
│   ├── image_001.png
│   └── image_002.png
│
└── patient_003/
```

처럼 한 Patient에게 여러 Image가 존재할 수 있다.

---

# 41. 여기서 중요한 문제

일반적인 Random Image Split을 하면:

```text
patient_001 image_001
→ Train

patient_001 image_002
→ Validation
```

처럼 같은 Patient의 Image가 서로 다른 Split에 들어갈 수 있다.

---

# 42. Patient Leakage

이런 상황에서는 Model이 새로운 Patient에 일반화하는 능력보다 같은 Patient의 특성을 이미 본 효과가 섞일 수 있다.

이를:

```text
Patient-level Leakage
```

관점에서 주의해야 한다.

---

# 43. 좋은 Split

가능하다면:

```text
Train Patients

Validation Patients

Test Patients
```

를 서로 분리한다.

---

# 44. 예

```text
Patient A
Patient B
Patient C

→ Train


Patient D

→ Validation


Patient E

→ Test
```

처럼 구성한다.

---

# 45. Patient-level Split

핵심:

```text
한 Patient는
하나의 Split에만 존재
```

하도록 한다.

---

# 46. GroupShuffleSplit

scikit-learn에서는 Group 기반 Split을 사용할 수 있다.

```python
from sklearn.model_selection import (
    GroupShuffleSplit
)


splitter = GroupShuffleSplit(
    n_splits=1,
    test_size=0.2,
    random_state=42
)
```

---

# 47. Group 지정

예:

```python
groups = df[
    "patient_id"
]
```

이다.

---

# 48. Split

```python
train_idx, val_idx = next(
    splitter.split(
        df,
        groups=groups
    )
)
```

---

# 49. 확인

```python
train_patients = set(
    df.iloc[
        train_idx
    ][
        "patient_id"
    ]
)


val_patients = set(
    df.iloc[
        val_idx
    ][
        "patient_id"
    ]
)


print(
    train_patients
    &
    val_patients
)
```

---

# 50. 좋은 결과

```text
set()
```

이면 Patient 중복이 없다는 의미이다.

---

# 51. Class Balance도 중요

Patient-level Split을 했다고 끝나는 것은 아니다.

각 Split의:

```text
Class Distribution
```

도 확인한다.

---

# 52. 예

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

# 53. Group와 Stratification

의료영상에서는:

```text
Class 비율 유지
```

도 중요하지만:

```text
같은 Patient를 분리하지 않는 것
```

이 더 우선적인 제약이 될 수 있다.

---

# 54. StratifiedGroupKFold

상황에 따라:

```python
from sklearn.model_selection import (
    StratifiedGroupKFold
)
```

같은 방법을 사용할 수도 있다.

---

# 55. Data Leakage

의료영상에서 Leakage는 매우 중요한 문제이다.

---

# 56. Leakage 예

```text
같은 Patient가 Train / Validation에 존재

Duplicate Image가 여러 Split에 존재

Augmented Image가 원본과 다른 Split에 존재

Test 정보를 이용해 Preprocessing 결정

Label 정보를 직접 포함한 Metadata 사용
```

등이다.

---

# 57. Augmentation 전에 Split

잘못된 순서:

```text
전체 Dataset
↓
Augmentation
↓
Train / Validation Split
```

---

# 58. 왜 문제?

원본 Image는 Train에 있고 거의 같은 Augmented Image가 Validation에 들어갈 수 있기 때문이다.

---

# 59. 좋은 순서

```text
Original Dataset
↓
Patient-level Split
↓
Train / Validation
↓
Train에만 Random Augmentation
```

이다.

---

# 60. Duplicate Image

의료영상 Dataset에는 동일하거나 거의 동일한 Image가 중복되어 있을 수 있다.

---

# 61. 중복의 문제

중복 Image가 Train과 Validation에 나뉘면 Model이 사실상 이미 본 Image를 다시 평가받을 수 있다.

---

# 62. 따라서

```text
File Name

Patient ID

Study ID

Series ID

Image Hash
```

등을 이용한 중복 점검을 고려할 수 있다.

---

# 63. Metadata Leakage

예를 들어 특정 Hospital, 장비, Marker가 특정 Class와 강하게 연결되어 있다면 Model이 질환 대신 그 정보를 사용할 수 있다.

---

# 64. Shortcut Learning

이를 넓게:

```text
Shortcut Learning
```

문제로 생각할 수 있다.

---

# 65. 예

```text
Pneumonia Image
→ 특정 Hospital에서 많이 촬영

Normal Image
→ 다른 Hospital에서 많이 촬영
```

이라면 Model이 병변 대신 Hospital별 영상 특성을 학습할 수 있다.

---

# 66. 다른 Shortcut 예

```text
Text Marker

Portable X-ray 표시

Image Border

Crop 방식

Padding

Image Resolution

Brightness

Scanner 특성
```

등이다.

---

# 67. 높은 Accuracy인데 위험한 이유

Validation Dataset에도 같은 Shortcut이 존재하면:

```text
Accuracy = 99%
```

가 나올 수도 있다.

하지만 다른 Hospital에서는 성능이 크게 떨어질 수 있다.

---

# 68. Domain Shift

Train과 실제 사용 환경의 Data Distribution이 다른 문제를:

```text
Domain Shift
```

관점에서 볼 수 있다.

---

# 69. 예

```text
Train
Hospital A

Test
Hospital B
```

---

# 70. 차이가 발생할 수 있는 요소

```text
촬영 장비

Protocol

해상도

Patient Population

질환 유병률

Image Processing

Hospital Workflow
```

등이다.

---

# 71. 그래서 External Validation

가능하다면 다른 기관 또는 다른 환경의 Data에서 Model 성능을 평가하는 것이 중요하다.

---

# 72. Internal Validation

같은 Dataset 내부에서 Train / Validation을 나누어 평가하는 방식이다.

---

# 73. External Validation

Training 환경과 다른 독립적인 Dataset에서 평가한다.

---

# 74. 의미

```text
Internal Validation 성능이 높다
```

와:

```text
새로운 환경에서도 잘 작동한다
```

는 같은 말이 아니다.

---

# 75. Class Imbalance

의료 Data에서는 특정 질환 Class가 적을 수 있다.

예:

```text
Normal
900

Disease
100
```

---

# 76. Accuracy 함정

모든 Sample을 Normal이라고 Prediction하면:

```text
Accuracy = 90%
```

이다.

하지만 Disease를 하나도 찾지 못한다.

---

# 77. 그래서 Metric

```text
Precision

Recall

Sensitivity

Specificity

F1

ROC-AUC

PR-AUC

Confusion Matrix
```

등을 함께 본다.

---

# 78. Sensitivity

```text
Sensitivity
=
TP
/
(TP + FN)
```

이다.

Recall과 같은 의미로 사용된다.

---

# 79. 의료적 질문

```text
실제 질환 환자 중
Model이 얼마나 찾아냈는가?
```

를 나타낸다.

---

# 80. Specificity

```text
Specificity
=
TN
/
(TN + FP)
```

이다.

---

# 81. 의료적 질문

```text
실제 정상 중
Model이 얼마나 정상으로 구분했는가?
```

를 나타낸다.

---

# 82. FN

False Negative:

```text
실제 Disease

Prediction Normal
```

이다.

---

# 83. FP

False Positive:

```text
실제 Normal

Prediction Disease
```

이다.

---

# 84. 어떤 것이 더 중요한가?

무조건:

```text
FN이 FP보다 중요하다.
```

라고 일반화하면 안 된다.

실제 의료 Task, Workflow, 후속 검사, 질환 특성 등에 따라 비용이 달라진다.

---

# 85. Screening

질환을 놓치지 않는 것이 특히 중요한 Screening 성격의 문제에서는 높은 Sensitivity가 중요한 목표가 될 수 있다.

하지만 실제 적용에서는 Specificity와 전체 Workflow도 함께 고려한다.

---

# 86. Threshold

Binary Classification에서는:

```text
Probability
↓
Threshold
↓
Prediction
```

이다.

---

# 87. 예

```text
P(Disease) >= 0.5
→ Disease
```

---

# 88. Threshold를 낮추면

일반적으로:

```text
Sensitivity ↑ 가능

FN ↓ 가능

FP ↑ 가능
```

하다.

---

# 89. Threshold를 높이면

일반적으로:

```text
Specificity ↑ 가능

FP ↓ 가능

FN ↑ 가능
```

하다.

---

# 90. Threshold는 Validation에서 선택

Test Set 결과를 보고 Threshold를 계속 바꾸면 Test Set에 맞추게 된다.

따라서 Validation Data에서 목적에 맞게 선택하고 Test는 최종 평가에 사용한다.

---

# 91. 의료영상과 Data Augmentation

의료영상에서도 Augmentation을 사용할 수 있다.

예:

```text
Rotation

Translation

Brightness

Contrast
```

등이다.

---

# 92. 하지만 주의

일반 자연 Image에서 가능한 Transformation이 의료적으로 항상 타당한 것은 아니다.

---

# 93. Horizontal Flip

질환이나 장기 위치에서 좌우 정보가 중요한 문제라면 Horizontal Flip이 Label 의미를 바꾸거나 비현실적인 Sample을 만들 수 있다.

---

# 94. Vertical Flip

사람을 상하로 뒤집는 변환은 많은 의료영상 문제에서 비현실적일 수 있다.

---

# 95. Rotation

작은 Rotation은 촬영 자세 변화를 모방할 수 있지만 지나친 Rotation은 실제 Data Distribution과 달라질 수 있다.

---

# 96. Brightness / Contrast

촬영 조건 변화에 대한 Robustness를 높이는 데 도움을 줄 가능성이 있지만 중요한 병변 정보를 훼손하지 않는 범위인지 확인해야 한다.

---

# 97. 원칙

```text
가능한 Augmentation인가?
```

가 아니라:

```text
의학적으로 타당한 Augmentation인가?
```

를 생각한다.

---

# 98. Augmentation은 Train에만

```text
Train
→ Random Augmentation

Validation
→ Deterministic Preprocessing

Test
→ Deterministic Preprocessing
```

을 기본으로 생각한다.

---

# 99. Transfer Learning

의료영상에서도 ImageNet Pretrained Model을 사용할 수 있다.

예:

```text
ResNet

DenseNet

EfficientNet
```

등이다.

---

# 100. 하지만 Domain 차이

ImageNet:

```text
동물

자동차

사물

자연 Image
```

중심이다.

의료영상:

```text
X-ray

CT

MRI
```

는 상당히 다른 Domain이다.

---

# 101. 그런데 왜 사용할까?

초기 Layer에서 학습한:

```text
Edge

Texture

Shape
```

등의 기본적인 Visual Feature가 새로운 Task에도 도움이 될 가능성이 있기 때문이다.

---

# 102. 항상 좋아지는가?

아니다.

반드시:

```text
Baseline

vs

Pretrained

vs

Fine-tuned
```

성능을 실제로 비교한다.

---

# 103. Fine-tuning

의료영상에서는 다음처럼 단계적으로 실험할 수 있다.

```text
Stage 1
Backbone Freeze
Classifier Training

↓

Stage 2
Last Block Unfreeze
Low Learning Rate

↓

Validation 비교
```

---

# 104. Dataset이 작다면

전체 Model을 처음부터 Training하는 것보다 Transfer Learning이 유용한 출발점이 될 수 있다.

하지만 결과는 Dataset에 따라 검증해야 한다.

---

# 105. Weighted Loss

Class Imbalance가 있다면:

```python
loss_fn = nn.CrossEntropyLoss(
    weight=class_weights
)
```

같은 방법을 실험할 수 있다.

---

# 106. Class Weight

Minority Class Error에 더 큰 Weight를 주는 방식이다.

---

# 107. 하지만

Weighted Loss를 적용했다고 무조건 성능이 좋아지는 것은 아니다.

```text
Baseline

vs

Weighted Loss
```

를 같은 Validation 조건에서 비교한다.

---

# 108. Oversampling

Minority Class를 더 자주 Sampling하는 전략도 생각할 수 있다.

하지만 Image Dataset에서는 중복 Sampling으로 인한 Overfitting 가능성도 확인해야 한다.

---

# 109. SMOTE

Tabular Data에서 사용했던 SMOTE를 Raw Image Pixel에 그대로 적용하는 것은 일반적인 첫 선택으로 보기 어렵다.

Image에서는:

```text
Augmentation

Weighted Loss

Sampling 전략
```

등을 먼저 고려할 수 있다.

---

# 110. Model Evaluation

좋은 의료영상 AI Evaluation은:

```text
Accuracy 하나
```

로 끝나지 않는다.

---

# 111. 함께 볼 것

```text
Confusion Matrix

Sensitivity

Specificity

Precision

F1

ROC-AUC

PR-AUC

Probability Distribution

Failure Cases
```

등이다.

---

# 112. Confidence

Model이 높은 Probability를 출력했다고 해서 반드시 맞는 것은 아니다.

---

# 113. 예

```text
True
Normal

Pred
Pneumonia

P(Pneumonia)
0.999
```

이면 High-confidence Error이다.

---

# 114. 이런 Case가 중요한 이유

Model이 단순히 애매해서 틀린 것이 아니라 매우 확신하면서 틀렸기 때문이다.

---

# 115. Error Analysis

다음과 같이 나눈다.

```text
True Positive

True Negative

False Positive

False Negative
```

---

# 116. Confidence까지 추가

```text
High-confidence Correct

Low-confidence Correct

High-confidence Error

Low-confidence Error
```

로 더 나눠볼 수 있다.

---

# 117. Error Image 직접 확인

```text
Image Quality

Position

Brightness

Contrast

Marker

Crop

Artifact

병변 크기

Label
```

등을 확인한다.

---

# 118. Grad-CAM

Error Image에서 Model의 Activation 영역을 탐색할 수 있다.

---

# 119. 좋은 분석 흐름

```text
Metric
↓
Confusion Matrix
↓
Error Sample
↓
Original Image
↓
Grad-CAM
↓
Pattern 발견
↓
Hypothesis
↓
Experiment
```

이다.

---

# 120. Grad-CAM 주의

```text
Heatmap
=
병변 정답 위치
```

가 아니다.

---

# 121. Grad-CAM의 역할

```text
Model Behavior를
정성적으로 탐색하는 도구
```

로 생각한다.

---

# 122. 의료적 판단을 증명하는 도구?

Grad-CAM 하나만으로:

```text
Model이 의학적으로 올바르게 판단했다.
```

고 증명할 수 없다.

---

# 123. Model이 폐를 보고 있다면 충분한가?

아니다.

폐 영역에 Heatmap이 나타났다는 것만으로 Model이 실제 병변 Feature를 사용했다고 확정할 수 없다.

---

# 124. Domain Expert

의료영상 AI에서는 가능하다면 의료 전문가와 함께 결과를 검토하는 것이 중요하다.

---

# 125. Label Quality

AI 성능은 Label 품질의 영향을 크게 받을 수 있다.

---

# 126. Label Error

잘못된 Label이 존재하면 Model Training과 Evaluation 모두 왜곡될 수 있다.

---

# 127. High-confidence Error와 Label 검토

High-confidence Error는 Label을 다시 점검할 후보를 찾는 데 도움을 줄 수 있다.

하지만:

```text
Model이 확신했다
=
Label이 틀렸다
```

는 아니다.

---

# 128. Annotation

의료영상의 Label은:

```text
진단 결과

판독 결과

병변 위치 Annotation

수술 결과

검사 결과
```

등 다양한 Source에서 만들어질 수 있다.

---

# 129. Label 정의가 중요

예:

```text
Pneumonia = 1
```

이라고 하더라도 어떤 기준으로 Pneumonia Label이 결정되었는지 알아야 한다.

---

# 130. Label Ambiguity

의료 Data에서는 전문가 사이에서도 판단이 완전히 일치하지 않을 수 있다.

따라서 Label을 절대적인 완벽한 정답이라고 가정하기 어려운 경우도 있다.

---

# 131. Data Dictionary

의료 Data를 사용할 때는:

```text
Column 의미

단위

Label 정의

Missing 의미

촬영 조건

Patient ID

Study ID
```

등을 먼저 확인한다.

---

# 132. Privacy

의료 Data는 개인정보 및 민감한 정보를 포함할 수 있다.

따라서:

```text
비식별화

접근 권한

저장 위치

공유 범위

보안 정책
```

등을 준수해야 한다.

---

# 133. DICOM Metadata 주의

DICOM Metadata에는 개인을 식별할 수 있는 정보가 포함될 가능성이 있으므로 실제 Data 처리 시 비식별화 여부를 확인해야 한다.

---

# 134. Burned-in Text

Image Pixel 자체에 환자 정보가 문자로 포함된 경우도 있을 수 있다.

Metadata만 제거했다고 항상 비식별화가 완료되는 것은 아니다.

---

# 135. Model의 목적 정의

의료 AI를 만들기 전에:

```text
무엇을 Prediction하는가?

누가 사용하는가?

어떤 상황에서 사용하는가?

Prediction 이후 어떤 행동을 하는가?
```

를 정의해야 한다.

---

# 136. AI가 의사를 대체?

모든 의료영상 AI Project의 목적이 의료진을 대체하는 것은 아니다.

예:

```text
판독 보조

우선순위 분류

Screening 보조

위험 알림

정량화
```

등 다양한 목적이 있을 수 있다.

---

# 137. CDSS와 연결

의료 AI Prediction은 Clinical Decision Support System 같은 형태로 연결될 수도 있다.

예:

```text
Patient Data
↓
AI Model
↓
Risk Score
↓
Alert
↓
의료진 확인
```

이다.

---

# 138. Prediction과 Decision은 다르다

Model이:

```text
Disease Probability = 0.82
```

라고 출력하는 것과 실제 의료적 Decision을 내리는 것은 동일하지 않다.

---

# 139. Probability도 절대적 진실이 아니다

Model Probability는 Model이 학습한 Data와 구조에 기반한 Output이다.

실제 임상 확률과 정확히 일치한다고 자동으로 가정할 수 없다.

---

# 140. Calibration

Prediction Probability와 실제 발생 빈도가 얼마나 잘 맞는지 확인하는 Calibration도 중요할 수 있다.

---

# 141. Generalization

Model이 Training Dataset에서만 잘하는 것이 아니라 새로운 Patient에서도 잘 작동해야 한다.

---

# 142. 좋은 Train Accuracy

```text
Train Accuracy = 99.9%
```

만으로는 충분하지 않다.

---

# 143. Validation

```text
새로운 Data에서 얼마나 잘하는가?
```

를 확인한다.

---

# 144. External Validation

더 나아가:

```text
다른 기관

다른 장비

다른 Patient Population
```

에서도 확인하는 것이 중요할 수 있다.

---

# 145. Overfitting

```text
Train 성능 ↑

Validation 성능 ↓
```

이면 Overfitting을 의심한다.

---

# 146. 의료영상에서 Overfitting 위험

의료 Dataset은 자연 Image Dataset보다 Sample 수가 적은 경우가 많고 한 Patient에서 여러 Image가 생성될 수 있어 독립 Sample 수를 주의해서 봐야 한다.

---

# 147. Image 수와 Patient 수

예:

```text
Images
10,000

Patients
500
```

이라면 10,000개의 완전히 독립적인 Patient Data가 있는 것은 아니다.

---

# 148. 중요한 단위

```text
Image Count
```

뿐 아니라:

```text
Patient Count
```

도 확인한다.

---

# 149. Study / Series

DICOM 환경에서는:

```text
Patient

Study

Series

Instance
```

같은 계층 구조가 존재할 수 있다.

---

# 150. Split 단위

Task에 따라:

```text
Patient-level

Study-level

Series-level
```

중 어떤 단위로 Split해야 Leakage를 막을 수 있는지 확인한다.

---

# 151. Dataset Bias

Dataset 자체가 특정 Population에 편향되어 있다면 Model도 그 Data Distribution에 영향을 받는다.

---

# 152. Subgroup Performance

가능하고 적절한 경우 특정 Subgroup에서 Model 성능 차이가 있는지 분석할 수 있다.

단, 실제 의료 연구에서는 적절한 통계적 설계와 개인정보 보호가 필요하다.

---

# 153. Model Architecture보다 Data가 중요할 수 있다

성능을 높이기 위해:

```text
ResNet18
→ ResNet50
→ EfficientNet
```

만 계속 바꾸는 것이 항상 해결책은 아니다.

---

# 154. 먼저 확인

```text
Label이 정확한가?

Split이 올바른가?

Leakage가 없는가?

중복 Image가 없는가?

Class Imbalance는 어떤가?

Preprocessing이 올바른가?

Train/Test Domain이 다른가?
```

를 확인한다.

---

# 155. 좋은 Baseline

복잡한 Model을 시작부터 사용하는 것보다 먼저 재현 가능한 Baseline을 만든다.

예:

```text
Resize

Normalize

Pretrained ResNet18

Classifier 교체

CrossEntropyLoss

Adam

Validation Evaluation
```

이다.

---

# 156. Baseline 이후

```text
Baseline
↓
문제점 분석
↓
가설
↓
Experiment
↓
Evaluation
```

으로 진행한다.

---

# 157. 예

```text
문제
Minority Class Recall 낮음

↓

가설
Class Imbalance 영향

↓

Experiment
Weighted Loss

↓

Evaluation
Recall / F1 / Confusion Matrix 비교
```

---

# 158. 다른 예

```text
문제
Validation Loss 상승

↓

가설
Overfitting

↓

Experiment
Augmentation / Weight Decay / Early Stopping

↓

Evaluation
Learning Curve 비교
```

---

# 159. 또 다른 예

```text
문제
High-confidence Error에서
Border Activation

↓

가설
Shortcut Learning

↓

Experiment
Data / Crop / Padding 점검

↓

Evaluation
Metric + Grad-CAM 비교
```

---

# 160. 실험은 한 번에 하나씩

가능하면:

```text
Baseline

Experiment A
Fine-tuning

Experiment B
Augmentation

Experiment C
Weighted Loss
```

처럼 변화의 원인을 추적할 수 있게 한다.

---

# 161. 결합 실험

각 실험의 효과를 확인한 후:

```text
Fine-tuning
+
Augmentation
+
Weighted Loss
```

처럼 결합할 수 있다.

---

# 162. Experiment Log

예:

```text
Experiment

Model

Pretrained Weight

Image Size

Batch Size

Learning Rate

Loss

Augmentation

Freeze / Unfreeze

Epoch

Best Epoch

Accuracy

Recall

F1

ROC-AUC
```

를 기록한다.

---

# 163. Reproducibility

실험을 다시 실행할 수 있도록:

```text
Random Seed

Library Version

Data Split

Transform

Hyperparameter

Model Weight
```

등을 기록한다.

---

# 164. Model 저장

Best Validation Model을 저장한다.

```python
torch.save(
    model.state_dict(),
    "best_model.pt"
)
```

---

# 165. Inference

새로운 Image:

```text
New Image
↓
Same Preprocessing
↓
Model
↓
Probability
↓
Prediction
```

이다.

---

# 166. Same Preprocessing

Training과 실제 Inference에서:

```text
Image Size

Channel

Normalize
```

등이 일치해야 한다.

---

# 167. Random Augmentation은 Inference에서?

일반적인 기본 Inference에서는 Random Augmentation을 그대로 사용하지 않는다.

Validation/Test와 동일한 결정적 Preprocessing을 사용한다.

---

# 168. Model만 저장하면 충분한가?

실제 Service에서는 Model Weight뿐 아니라:

```text
Class Mapping

Image Preprocessing

Threshold

Model Version

Input Specification
```

도 필요하다.

---

# 169. 예

```text
Model
ResNet18

Input
224 × 224 RGB

Normalize
ImageNet

Class 0
Normal

Class 1
Pneumonia

Threshold
0.40
```

처럼 기록한다.

---

# 170. AI Service

향후:

```text
Image Upload
↓
FastAPI
↓
Preprocessing
↓
AI Model
↓
Prediction
↓
Probability
↓
Web Response
```

구조로 연결할 수 있다.

---

# 171. 의료 AI Web Service

예:

```text
Browser
↓
Chest X-ray Upload
↓
Backend API
↓
Model Inference
↓
Result
↓
Prediction 화면
```

이다.

---

# 172. 하지만 실제 의료서비스

학습용 Prototype과 실제 임상용 의료기기는 요구되는 검증, 규제, 보안, 품질관리 수준이 크게 다르다.

---

# 173. 따라서

학습 Project에서 높은 Accuracy가 나왔다고 바로 실제 환자 진료에 사용할 수 있는 것은 아니다.

---

# 174. Computer Vision 전체 연결

```text
Image
↓
Pixel
↓
Tensor
↓
Preprocessing
↓
Dataset
↓
DataLoader
↓
CNN
↓
Transfer Learning
↓
Fine-tuning
↓
Prediction
↓
Evaluation
↓
Failure Analysis
↓
Grad-CAM
↓
Medical Interpretation
```

---

# 175. Data 단계

```text
Image 수집
↓
Label 확인
↓
Patient ID 확인
↓
Duplicate 확인
↓
Patient-level Split
```

---

# 176. Training 단계

```text
Train Transform
↓
Dataset
↓
DataLoader
↓
Model
↓
Loss
↓
Optimizer
↓
Training
```

---

# 177. Validation 단계

```text
Validation Transform
↓
model.eval()
↓
Prediction
↓
Metrics
↓
Best Model
```

---

# 178. Analysis 단계

```text
Confusion Matrix
↓
FP / FN
↓
Confidence
↓
Failure Image
↓
Grad-CAM
```

---

# 179. Improvement 단계

```text
Problem
↓
Hypothesis
↓
Experiment
↓
Same Validation
↓
Compare
```

---

# 180. Deployment 단계

```text
Best Model
↓
Save Weight
↓
Preprocessing Definition
↓
Class Mapping
↓
Threshold
↓
API
↓
Web Service
```

---

# 181. 가장 위험한 생각

```text
Validation Accuracy가 높다
=
실제 의료 현장에서 좋은 Model이다
```

라고 바로 판단하는 것이다.

---

# 182. 실제로 추가 확인할 것

```text
Leakage

Patient Independence

External Validation

Domain Shift

Class Imbalance

Calibration

Failure Cases

Subgroup Performance

Data Quality

Clinical Workflow
```

등이다.

---

# 183. Accuracy 99%도 질문해야 한다

```text
어떤 Split인가?

같은 Patient가 섞였는가?

어떤 Class가 많은가?

FN은 몇 개인가?

외부 Data에서도 같은가?

Model이 어디를 보고 있는가?
```

를 확인한다.

---

# 184. 좋은 의료 AI 질문

```text
점수가 몇 점인가?
```

에서 끝나는 것이 아니라:

```text
왜 이런 점수가 나왔는가?

어디서 실패하는가?

새로운 Patient에서도 가능한가?

어떤 오류가 중요한가?

Model이 Shortcut을 사용하지 않는가?
```

까지 생각한다.

---

# 185. 의료영상 AI Checklist

Data:

```text
Patient ID 확인

Label 정의 확인

Class Distribution 확인

Duplicate 확인

Metadata 확인

Privacy 확인
```

Split:

```text
Patient-level Split

Train/Val/Test 독립성

Augmentation 전 Split
```

Preprocessing:

```text
Image Size

Channel

Pixel Range

Normalization

CT Windowing / MRI Sequence 등 Domain 특성
```

Training:

```text
Baseline

Transfer Learning

Fine-tuning

Class Imbalance 대응

Best Model 저장
```

Evaluation:

```text
Confusion Matrix

Sensitivity

Specificity

Precision

F1

ROC-AUC / PR-AUC

Threshold

Failure Analysis
```

Interpretation:

```text
Original Image 확인

Grad-CAM

Shortcut Learning 점검

과도한 해석 금지
```

Generalization:

```text
Patient Independence

Domain Shift

External Validation
```

---

# 186. Chapter 08 전체 복습

이번 Chapter에서 배운 흐름:

```text
computer-vision-basic.md
↓
Computer Vision 전체 개념

image-data-basic.md
↓
Pixel / Channel / Shape

image-preprocessing.md
↓
Resize / Tensor / Normalize

cnn-basic.md
↓
Convolution / Feature Map

image-classification.md
↓
Image → Class Prediction

data-augmentation.md
↓
Training Image 다양화

transfer-learning.md
↓
Pretrained Model 활용

fine-tuning.md
↓
Pretrained Weight 추가 학습

image-evaluation.md
↓
Metric / Error Analysis

grad-cam.md
↓
Model Activation 시각화

medical-image-ai.md
↓
의료영상 AI 전체 연결
```

---

# 187. 가장 중요한 연결

```text
좋은 Model
=
높은 Accuracy만 가진 Model
```

이라고 생각하면 안 된다.

더 좋은 관점:

```text
좋은 Data

올바른 Split

Leakage 방지

적절한 Preprocessing

재현 가능한 Training

목적에 맞는 Metric

Failure Analysis

Generalization 검증
```

이 함께 필요하다.

---

# 188. 한 줄 정리

의료영상 AI는:

```text
의료 Image에 Deep Learning을 적용하는 것
```

에서 끝나는 것이 아니라:

```text
Data 구조

Patient-level Split

Leakage

Domain Shift

Class Imbalance

Evaluation

Failure Analysis

Interpretation

Clinical Context
```

까지 함께 고려해야 하는 분야이다.