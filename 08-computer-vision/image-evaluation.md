# Image Evaluation

## 1. Image Classification Evaluation이란?

Image Classification Model을 Training한 뒤에는 Model이 새로운 Image를 얼마나 잘 분류하는지 평가해야 한다.

단순히:

```text
Training이 끝났다
```

가 아니라:

```text
어떤 Class를 잘 맞히는가?

어떤 Class를 자주 틀리는가?

False Positive는 얼마나 발생하는가?

False Negative는 얼마나 발생하는가?

Prediction Probability는 신뢰할 만한가?

어떤 Image에서 실패하는가?
```

를 확인해야 한다.

---

# 2. Evaluation 전체 흐름

```text
Validation Images
↓
Model
↓
Logits
↓
Probability
↓
Prediction
↓
True Label과 비교
↓
Metrics
↓
Failure Analysis
```

---

# 3. Validation Mode

Evaluation에서는:

```python
model.eval()
```

을 사용한다.

그리고 Gradient가 필요하지 않으므로:

```python
with torch.no_grad():
```

를 사용한다.

---

# 4. Validation Prediction 수집

Binary Classification을 2개의 Logit으로 출력한다고 하자.

```python
all_labels = []

all_preds = []

all_probs = []


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


        probs = torch.softmax(
            logits,
            dim=1
        )


        preds = logits.argmax(
            dim=1
        )


        all_labels.extend(
            labels.cpu().numpy()
        )


        all_preds.extend(
            preds.cpu().numpy()
        )


        all_probs.extend(
            probs[:, 1].cpu().numpy()
        )
```

---

# 5. 세 가지 결과

위 코드에서는:

```text
all_labels
→ 실제 정답

all_preds
→ Model이 최종 선택한 Class

all_probs
→ Class 1 Probability
```

를 저장한다.

---

# 6. 예

실제:

```text
all_labels

[0, 1, 1, 0]
```

Prediction:

```text
all_preds

[0, 1, 0, 1]
```

Probability:

```text
all_probs

[0.02, 0.95, 0.40, 0.80]
```

처럼 나올 수 있다.

---

# 7. Probability와 Prediction은 다르다

Probability:

```text
Class 1일 가능성에 대한 Model Score
```

Prediction:

```text
최종적으로 선택한 Class
```

이다.

---

# 8. Binary Classification

예:

```text
0 = Normal

1 = Pneumonia
```

이라고 하자.

Positive Class:

```text
1 = Pneumonia
```

으로 정의했다고 가정한다.

---

# 9. Positive / Negative는 좋고 나쁨이 아니다

```text
Positive
→ 관심 Class

Negative
→ 그 반대 Class
```

이다.

문제 정의에 따라 어떤 Class를 Positive로 정할지 결정한다.

---

# 10. Confusion Matrix

Binary Classification에서 가장 중요한 평가 도구 중 하나이다.

```text
                 Prediction

                 0       1

True 0          TN      FP

True 1          FN      TP
```

---

# 11. TP

True Positive:

```text
실제 Positive

Prediction Positive
```

이다.

예:

```text
실제 Pneumonia

Prediction Pneumonia
```

---

# 12. TN

True Negative:

```text
실제 Negative

Prediction Negative
```

이다.

예:

```text
실제 Normal

Prediction Normal
```

---

# 13. FP

False Positive:

```text
실제 Negative

Prediction Positive
```

이다.

예:

```text
실제 Normal

Prediction Pneumonia
```

---

# 14. FN

False Negative:

```text
실제 Positive

Prediction Negative
```

이다.

예:

```text
실제 Pneumonia

Prediction Normal
```

---

# 15. Confusion Matrix 계산

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

# 16. 예

```text
[[245, 23],
 [  3, 773]]
```

이라면:

```text
TN = 245

FP = 23

FN = 3

TP = 773
```

이다.

---

# 17. 코드로 꺼내기

```python
tn, fp, fn, tp = cm.ravel()


print(
    "TN:",
    tn
)


print(
    "FP:",
    fp
)


print(
    "FN:",
    fn
)


print(
    "TP:",
    tp
)
```

---

# 18. Accuracy

전체 Prediction 중 맞춘 비율이다.

공식:

```text
Accuracy

=
(TP + TN)
/
(TP + TN + FP + FN)
```

---

# 19. sklearn

```python
from sklearn.metrics import (
    accuracy_score
)


accuracy = accuracy_score(
    all_labels,
    all_preds
)


print(
    accuracy
)
```

---

# 20. Accuracy 해석

예:

```text
Accuracy = 0.95
```

이면 전체 Sample 중 약 95%를 올바르게 분류했다는 의미이다.

---

# 21. Accuracy의 문제

Class Imbalance가 심하면 Accuracy가 Model을 과대평가할 수 있다.

예:

```text
Normal = 5%

Pneumonia = 95%
```

라고 하자.

---

# 22. 모든 Image를 Pneumonia라고 Prediction

```text
Prediction

전부 1
```

이어도 Accuracy는:

```text
95%
```

가 될 수 있다.

---

# 23. 하지만 좋은 Model인가?

아니다.

Normal Class를 하나도 제대로 분류하지 못한다.

따라서 Accuracy 하나만 보면 안 된다.

---

# 24. Precision

Positive라고 Prediction한 것 중 실제 Positive인 비율이다.

```text
Precision

=
TP
/
(TP + FP)
```

---

# 25. 질문으로 이해

```text
Model이 Positive라고 했을 때
얼마나 맞았는가?
```

이다.

---

# 26. 예

Model이 Pneumonia라고 100명에게 Prediction했다.

그중 실제 Pneumonia가 90명이면:

```text
Precision = 90%
```

이다.

---

# 27. sklearn

```python
from sklearn.metrics import (
    precision_score
)


precision = precision_score(
    all_labels,
    all_preds
)
```

---

# 28. Recall

실제 Positive 중 Model이 Positive로 찾아낸 비율이다.

```text
Recall

=
TP
/
(TP + FN)
```

---

# 29. 질문으로 이해

```text
실제 Positive 환자 중
얼마나 찾아냈는가?
```

이다.

---

# 30. Recall의 다른 이름

상황에 따라:

```text
Sensitivity

민감도
```

라고도 한다.

---

# 31. sklearn

```python
from sklearn.metrics import (
    recall_score
)


recall = recall_score(
    all_labels,
    all_preds
)
```

---

# 32. False Negative와 Recall

FN이 증가하면 Recall은 감소한다.

```text
FN ↑
↓
Recall ↓
```

이다.

---

# 33. 의료영상에서 Recall

질환 환자를 놓치는 것이 큰 문제라면 Recall이 중요할 수 있다.

하지만 항상 Recall만 최대화하면 된다는 뜻은 아니다.

False Positive와 실제 사용 목적도 함께 고려한다.

---

# 34. Specificity

실제 Negative 중 Model이 Negative로 올바르게 분류한 비율이다.

```text
Specificity

=
TN
/
(TN + FP)
```

---

# 35. 질문으로 이해

```text
실제 Negative 중
얼마나 Negative로 잘 구분했는가?
```

이다.

---

# 36. 코드

```python
specificity = (
    tn
    /
    (
        tn
        +
        fp
    )
)


print(
    specificity
)
```

---

# 37. Sensitivity와 Specificity

```text
Sensitivity
→ Positive를 얼마나 잘 찾는가?

Specificity
→ Negative를 얼마나 잘 구분하는가?
```

이다.

---

# 38. F1-score

Precision과 Recall의 조화평균이다.

```text
F1

=
2 ×
Precision × Recall
/
(Precision + Recall)
```

---

# 39. 왜 사용할까?

Precision과 Recall을 하나의 숫자로 함께 고려하고 싶을 때 유용하다.

---

# 40. sklearn

```python
from sklearn.metrics import (
    f1_score
)


f1 = f1_score(
    all_labels,
    all_preds
)
```

---

# 41. Classification Report

여러 Metric을 한 번에 확인할 수 있다.

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

# 42. 예

```text
              precision    recall    f1-score

0                0.95       0.91       0.93

1                0.97       0.99       0.98
```

처럼 볼 수 있다.

---

# 43. Class별 Metric을 본다

전체 Accuracy만 보는 것이 아니라:

```text
Class 0 Precision

Class 0 Recall

Class 0 F1

Class 1 Precision

Class 1 Recall

Class 1 F1
```

을 확인한다.

---

# 44. Macro Average

각 Class의 Metric을 동일한 중요도로 평균한다.

개념:

```text
Class 0 Metric
+
Class 1 Metric
↓
단순 평균
```

이다.

---

# 45. Weighted Average

각 Class의 Sample 수를 고려하여 평균한다.

따라서 Majority Class의 영향이 더 커질 수 있다.

---

# 46. Imbalanced Dataset

Class Imbalance가 있다면:

```text
Macro F1
```

도 유용하게 확인할 수 있다.

---

# 47. Macro F1

```python
macro_f1 = f1_score(
    all_labels,
    all_preds,
    average="macro"
)
```

---

# 48. Weighted F1

```python
weighted_f1 = f1_score(
    all_labels,
    all_preds,
    average="weighted"
)
```

---

# 49. Probability 기반 평가

지금까지:

```text
Accuracy

Precision

Recall

F1
```

은 최종 Class Prediction을 주로 사용했다.

하지만 Model은 Probability도 출력한다.

---

# 50. 예

두 Model:

```text
Model A
Probability = 0.51
Prediction = 1

Model B
Probability = 0.99
Prediction = 1
```

최종 Class는 같지만 Confidence는 다르다.

---

# 51. ROC Curve

Threshold를 여러 값으로 바꾸면서:

```text
True Positive Rate

False Positive Rate
```

의 관계를 확인한다.

---

# 52. True Positive Rate

TPR은 Recall과 같다.

```text
TPR
=
TP / (TP + FN)
```

---

# 53. False Positive Rate

```text
FPR

=
FP
/
(FP + TN)
```

이다.

---

# 54. ROC-AUC

ROC Curve 아래 면적을:

```text
ROC-AUC
```

라고 한다.

---

# 55. 의미

Model이 Positive와 Negative Sample의 점수를 얼마나 잘 구분해 순위를 매기는지를 보는 지표로 이해할 수 있다.

---

# 56. sklearn

```python
from sklearn.metrics import (
    roc_auc_score
)


roc_auc = roc_auc_score(
    all_labels,
    all_probs
)


print(
    roc_auc
)
```

---

# 57. 중요한 점

ROC-AUC에는 일반적으로:

```text
최종 Class Prediction
```

이 아니라:

```text
Positive Class Score / Probability
```

를 사용한다.

---

# 58. 잘못된 예

```python
roc_auc_score(
    all_labels,
    all_preds
)
```

처럼 0/1 Prediction만 넣으면 Probability가 가진 순위 정보를 잃는다.

---

# 59. 올바른 방향

```python
roc_auc_score(
    all_labels,
    all_probs
)
```

이다.

---

# 60. ROC-AUC 범위

일반적으로:

```text
0 ~ 1
```

범위를 갖는다.

Binary 문제에서 무작위 수준의 Ranking은 보통:

```text
0.5
```

부근이다.

---

# 61. ROC-AUC가 높다고 모든 것이 좋은 것은 아니다

특히 심한 Class Imbalance에서는 ROC-AUC만으로 실제 Positive Prediction 품질을 충분히 이해하기 어려울 수 있다.

---

# 62. Precision-Recall Curve

Threshold 변화에 따른:

```text
Precision

Recall
```

관계를 확인한다.

---

# 63. PR-AUC / Average Precision

Positive Class가 희귀한 문제에서 유용하게 볼 수 있다.

---

# 64. sklearn

```python
from sklearn.metrics import (
    average_precision_score
)


ap = average_precision_score(
    all_labels,
    all_probs
)


print(
    ap
)
```

---

# 65. ROC-AUC와 PR-AUC

둘 중 무조건 하나가 더 좋은 Metric이라고 할 수는 없다.

Dataset과 문제 목적에 따라 선택한다.

---

# 66. Class Imbalance가 심하다면

다음을 함께 확인할 수 있다.

```text
Recall

Precision

F1

PR Curve

Average Precision

Confusion Matrix
```

---

# 67. Threshold

Binary Classification에서 Probability를 Class로 바꾸기 위한 기준값이다.

예:

```text
Probability >= 0.5
→ Class 1

Probability < 0.5
→ Class 0
```

---

# 68. 코드

```python
import numpy as np


probs = np.array(
    all_probs
)


preds_05 = (
    probs >= 0.5
).astype(
    int
)
```

---

# 69. Threshold 0.5가 항상 정답?

아니다.

0.5는 흔한 기본값일 뿐이다.

문제 목적에 따라 Threshold를 조정할 수 있다.

---

# 70. Threshold를 낮추면

예:

```text
0.5
↓
0.3
```

이면 Class 1이라고 Prediction하기 쉬워진다.

---

# 71. 일반적인 변화

Threshold를 낮추면 대체로:

```text
Positive Prediction ↑

Recall ↑

False Positive ↑

Precision / Specificity는 감소할 수 있음
```

이다.

정확한 변화는 Data에 따라 확인한다.

---

# 72. Threshold를 높이면

예:

```text
0.5
↑
0.8
```

이면 Positive라고 판단하기 더 어려워진다.

---

# 73. 일반적인 변화

```text
Positive Prediction ↓

False Positive ↓

Specificity ↑

Recall ↓
```

가 나타날 수 있다.

---

# 74. Threshold Trade-off

```text
Threshold 낮음
→ 놓치는 Positive 감소 가능
→ FP 증가 가능

Threshold 높음
→ FP 감소 가능
→ Positive를 놓칠 가능성 증가
```

이다.

---

# 75. 의료 AI에서 중요한 이유

질환을 놓치는 비용과 불필요한 추가 검사의 비용이 다를 수 있기 때문이다.

따라서 Threshold는 실제 사용 목적과 연결해 선택해야 한다.

---

# 76. Validation에서 Threshold 선택

Test 결과를 보고 Threshold를 선택하면 Test Data에 맞춘 선택이 된다.

따라서 Threshold는:

```text
Training / Validation 단계
```

에서 결정하고 Test는 최종 평가에 사용하는 것이 원칙이다.

---

# 77. 여러 Threshold 비교

```python
from sklearn.metrics import (
    precision_score,
    recall_score,
    f1_score
)


for threshold in [
    0.2,
    0.3,
    0.4,
    0.5,
    0.6,
    0.7,
    0.8
]:

    preds = (
        probs >= threshold
    ).astype(
        int
    )


    precision = precision_score(
        all_labels,
        preds,
        zero_division=0
    )


    recall = recall_score(
        all_labels,
        preds,
        zero_division=0
    )


    f1 = f1_score(
        all_labels,
        preds,
        zero_division=0
    )


    print(
        threshold,
        precision,
        recall,
        f1
    )
```

---

# 78. Best F1 Threshold

Validation에서 F1이 가장 높은 Threshold를 찾을 수도 있다.

하지만 실제 Project 목적이 Recall 우선이라면 F1 최대가 반드시 최적 Threshold는 아니다.

---

# 79. Threshold 선택 기준

예:

```text
F1 최대

Recall 최소 95% 유지

Specificity 최소 90% 유지

False Negative 최소화
```

등 목적에 따라 달라진다.

---

# 80. Threshold를 바꾸면 ROC-AUC도 바뀌나?

같은 Probability Score에 대해 ROC-AUC 자체는 특정 하나의 Threshold를 고정해서 계산하는 Metric이 아니다.

여러 Threshold에서의 Ranking 성능을 종합해서 본다.

---

# 81. Confidence

Multi-class Model에서:

```python
probs = torch.softmax(
    logits,
    dim=1
)


confidence, preds = probs.max(
    dim=1
)
```

로 가장 높은 Probability를 Confidence처럼 사용할 수 있다.

---

# 82. 예

```text
Probability

Class 0 = 0.02

Class 1 = 0.98
```

이면:

```text
Prediction = 1

Confidence = 0.98
```

이다.

---

# 83. 높은 Confidence

```text
Confidence = 0.999
```

라고 해서 반드시 맞는 Prediction은 아니다.

---

# 84. High-confidence Error

예:

```text
True = 0

Pred = 1

Confidence = 0.999
```

이다.

---

# 85. 왜 중요할까?

Model이 매우 확신하면서 틀린 Case이기 때문이다.

단순한 Boundary 근처 Error와 성격이 다를 수 있다.

---

# 86. Low-confidence Error

예:

```text
True = 0

Pred = 1

Confidence = 0.51
```

이다.

Decision Boundary에 가까운 Case일 수 있다.

---

# 87. Failure Case 분석

결과를 DataFrame으로 만들 수 있다.

```python
import pandas as pd


result_df = pd.DataFrame(
    {
        "true":
            all_labels,

        "pred":
            all_preds,

        "prob_1":
            all_probs
    }
)
```

---

# 88. Error 추출

```python
error_df = result_df[
    result_df[
        "true"
    ]
    !=
    result_df[
        "pred"
    ]
].copy()
```

---

# 89. Confidence 추가

Binary 2-Class 문제에서는:

```python
error_df[
    "confidence"
] = np.maximum(
    error_df[
        "prob_1"
    ],

    1
    -
    error_df[
        "prob_1"
    ]
)
```

처럼 Prediction Confidence를 계산할 수 있다.

---

# 90. 높은 Confidence Error부터 확인

```python
error_df = error_df.sort_values(
    "confidence",
    ascending=False
)
```

---

# 91. 무엇을 확인할까?

Error Image에서:

```text
Image Quality

Brightness

Contrast

Position

Crop

Artifact

Text Marker

Border

병변 크기

Label 오류 가능성

촬영 장비 차이
```

등을 확인한다.

---

# 92. False Positive만 확인

```python
fp_df = result_df[
    (
        result_df[
            "true"
        ]
        == 0
    )
    &
    (
        result_df[
            "pred"
        ]
        == 1
    )
]
```

---

# 93. False Negative만 확인

```python
fn_df = result_df[
    (
        result_df[
            "true"
        ]
        == 1
    )
    &
    (
        result_df[
            "pred"
        ]
        == 0
    )
]
```

---

# 94. 왜 FP/FN을 따로 볼까?

서로 다른 원인으로 발생할 수 있기 때문이다.

예:

```text
FP
→ 특정 정상 구조를 병변으로 착각

FN
→ 작은 병변을 놓침
```

처럼 Pattern이 다를 수 있다.

---

# 95. Error Analysis

좋은 Project에서는:

```text
Accuracy = 95%
```

로 끝내지 않는다.

---

# 96. 더 좋은 질문

```text
나머지 5%는 왜 틀렸는가?
```

를 분석한다.

---

# 97. Failure Analysis → Hypothesis

예:

```text
어두운 Image에서 FN이 많다.
```

---

# 98. Hypothesis

```text
Model이 밝기 변화에 민감한 것 같다.
```

---

# 99. Experiment

```text
적절한 Brightness Augmentation
```

을 실험할 수 있다.

---

# 100. 다시 평가

```text
Baseline

vs

Brightness Augmentation
```

에서 같은 Validation Set의 Metric과 Error Pattern을 비교한다.

---

# 101. Confusion Matrix 시각화

```python
from sklearn.metrics import (
    ConfusionMatrixDisplay
)

import matplotlib.pyplot as plt


ConfusionMatrixDisplay(
    confusion_matrix=cm
).plot()


plt.show()
```

---

# 102. ROC Curve

```python
from sklearn.metrics import (
    roc_curve,
    auc
)


fpr, tpr, thresholds = roc_curve(
    all_labels,
    all_probs
)


roc_auc = auc(
    fpr,
    tpr
)
```

---

# 103. Plot

```python
plt.figure(
    figsize=(
        6,
        5
    )
)


plt.plot(
    fpr,
    tpr,
    label=f"AUC = {roc_auc:.4f}"
)


plt.xlabel(
    "False Positive Rate"
)


plt.ylabel(
    "True Positive Rate"
)


plt.legend()


plt.show()
```

---

# 104. Precision-Recall Curve

```python
from sklearn.metrics import (
    precision_recall_curve
)


precision, recall, thresholds = (
    precision_recall_curve(
        all_labels,
        all_probs
    )
)
```

---

# 105. Plot

```python
plt.figure(
    figsize=(
        6,
        5
    )
)


plt.plot(
    recall,
    precision
)


plt.xlabel(
    "Recall"
)


plt.ylabel(
    "Precision"
)


plt.show()
```

---

# 106. Metric은 목적에 따라 선택한다

예:

```text
Competition 평가 기준
→ Accuracy

Screening
→ Recall 중요 가능

Class Imbalance
→ Macro F1 / PR-AUC 확인

Ranking 성능
→ ROC-AUC
```

등이다.

---

# 107. Competition에서는 평가 산식이 중요

대회가:

```text
Accuracy
```

로 평가한다면 Leaderboard 점수는 Accuracy 최적화와 직접 연결된다.

하지만 Model 분석에서는 다른 Metric도 함께 확인하는 것이 좋다.

---

# 108. Validation Metric과 Leaderboard

Local Validation:

```text
0.99
```

Leaderboard:

```text
0.91
```

처럼 차이가 날 수도 있다.

---

# 109. 왜 차이가 날까?

가능한 원인:

```text
Validation Split이 Test Distribution을 잘 반영하지 못함

Data Leakage

Domain Shift

Overfitting

Threshold 차이

Train/Test Distribution 차이

작은 Validation Sample
```

등이다.

---

# 110. 높은 Validation 성능을 무조건 믿지 않는다

특히 Image에서는:

```text
Duplicate Image

같은 Patient

같은 촬영 Series

Augmented Copy

파일명 정보
```

등이 섞였는지 확인한다.

---

# 111. Patient-level Evaluation

의료영상에서는 Image 단위 성능뿐 아니라 Patient 단위 독립성이 중요할 수 있다.

같은 Patient의 Image가 Train과 Validation에 모두 있으면 성능이 과대평가될 수 있다.

---

# 112. External Validation

가능하다면 다른 기관이나 다른 환경에서 수집된 Data로 평가하는 것은 Domain Generalization을 확인하는 데 중요한 단계가 될 수 있다.

---

# 113. Calibration

Model Probability가 실제 발생 빈도와 얼마나 잘 맞는지를 보는 개념이다.

---

# 114. 예

Model이:

```text
Probability = 0.8
```

이라고 예측한 Sample들을 많이 모았을 때 실제 Positive 비율도 약 80%에 가까운지를 생각할 수 있다.

---

# 115. 높은 ROC-AUC와 Calibration은 다른 문제

Model이 Class 순위를 매우 잘 구분하더라도 Probability 자체가 지나치게 확신되어 있을 수 있다.

---

# 116. 따라서

```text
Discrimination

Calibration
```

은 서로 다른 관점이다.

---

# 117. Confidence Histogram

```python
plt.hist(
    all_probs,
    bins=20
)


plt.xlabel(
    "Probability of Class 1"
)


plt.ylabel(
    "Count"
)


plt.show()
```

---

# 118. 무엇을 볼까?

Prediction Probability가:

```text
0과 1에 몰리는가?

0.5 근처가 많은가?

특정 Class에 치우치는가?
```

를 확인한다.

---

# 119. Prediction Distribution

```python
pd.Series(
    all_preds
).value_counts(
    normalize=True
)
```

---

# 120. True Distribution과 비교

```python
pd.Series(
    all_labels
).value_counts(
    normalize=True
)
```

도 같이 본다.

---

# 121. Prediction Distribution이 다르다고 무조건 오류는 아니다

Model Prediction과 실제 Label Distribution이 반드시 완전히 같아야 하는 것은 아니다.

다만 극단적인 차이는 확인할 가치가 있다.

---

# 122. Test Prediction 분석

Label이 없는 Test Dataset에서는 Accuracy를 계산할 수 없다.

대신:

```text
Prediction Distribution

Probability Distribution

Confidence Distribution

Sample Image Prediction
```

등을 확인할 수 있다.

---

# 123. Test Label이 없는데 Accuracy?

계산할 수 없다.

```text
Prediction
+
True Label
```

이 모두 있어야 Accuracy를 계산할 수 있다.

---

# 124. Competition Test

Competition에서는 실제 Test Label을 주최 측이 가지고 있고 사용자는 Prediction만 제출한다.

---

# 125. Leaderboard Score

제출 Prediction과 숨겨진 Test Label을 주최 측이 비교하여 Score를 계산한다.

---

# 126. Validation과 Test의 차이

```text
Validation
→ Label 있음
→ Model 선택 / 실험 비교

Test
→ 최종 Generalization 평가
```

이다.

---

# 127. Test를 반복해서 보며 튜닝하면?

Test에 맞춰 Model을 조정하게 되어 Test가 사실상 Validation처럼 사용될 수 있다.

Competition의 Public Leaderboard를 지나치게 반복해서 최적화하는 것도 비슷한 문제가 생길 수 있다.

---

# 128. Multi-class Classification

Class가 3개 이상이면 Confusion Matrix도 커진다.

예:

```text
3 Classes

          Pred
          0  1  2

True 0

True 1

True 2
```

이다.

---

# 129. Multi-class Accuracy

전체 정답 비율을 계산할 수 있다.

---

# 130. Multi-class F1

```python
f1_score(
    all_labels,
    all_preds,
    average="macro"
)
```

등을 사용할 수 있다.

---

# 131. Multi-class ROC-AUC

각 Class Probability가 필요하다.

단순한 Binary `prob[:, 1]` 방식과 다르므로 문제 형태에 맞게 계산해야 한다.

---

# 132. Multi-label Classification

Multi-label은 각 Label이 독립적인 Binary 문제처럼 다뤄질 수 있다.

이 경우:

```text
Sigmoid

BCEWithLogitsLoss

Label별 Threshold
```

등이 사용될 수 있다.

---

# 133. Binary 1-Logit Model

Binary Classification을:

```text
Output
[B, 1]
```

로 만들고:

```python
nn.BCEWithLogitsLoss()
```

를 사용할 수도 있다.

---

# 134. 이 경우 Probability

```python
probs = torch.sigmoid(
    logits
)
```

를 사용한다.

---

# 135. Binary 2-Logit Model

```text
Output
[B, 2]

Loss
CrossEntropyLoss

Probability
Softmax
```

이다.

---

# 136. 둘을 섞지 않는다

잘못된 조합:

```text
2 Logits
+
BCEWithLogitsLoss를 아무 생각 없이 사용
```

처럼 문제 정의와 Output/Loss가 맞지 않는 구성을 피한다.

---

# 137. 평가도 Model 구조와 맞춰야 한다

```text
CE 2-Logit
→ Softmax

BCE 1-Logit
→ Sigmoid
```

이다.

---

# 138. Model Comparison

여러 실험을 비교할 때 표를 만든다.

```text
Model      Acc    Precision Recall F1    ROC-AUC

Baseline   0.95   0.96      0.97   0.96  0.98

Augment    0.96   0.97      0.98   0.97  0.99

FineTune   0.97   0.97      0.99   0.98  0.99
```

---

# 139. 같은 Validation Set

Model Comparison에서는 동일한 Validation Set을 사용하는 것이 중요하다.

---

# 140. Best Model은 Metric에 따라 달라질 수 있다

예:

```text
Model A
Accuracy 최고

Model B
Recall 최고

Model C
F1 최고
```

일 수 있다.

---

# 141. 그래서 Project 목표가 중요

```text
무엇을 가장 중요하게 평가할 것인가?
```

를 먼저 정한다.

---

# 142. Error Count도 기록

```python
num_errors = sum(
    np.array(
        all_labels
    )
    !=
    np.array(
        all_preds
    )
)


print(
    num_errors
)
```

---

# 143. Error Rate

```python
error_rate = (
    num_errors
    /
    len(
        all_labels
    )
)
```

---

# 144. Metric만으로 원인은 알 수 없다

예:

```text
Recall = 0.80
```

은 Positive를 20% 정도 놓쳤다는 것은 알려주지만 왜 놓쳤는지는 알려주지 않는다.

---

# 145. 그래서 Image를 직접 본다

```text
Metric
+
Failure Image
+
Grad-CAM
```

을 함께 분석할 수 있다.

---

# 146. Grad-CAM과 연결

예:

```text
False Positive
↓
Grad-CAM
↓
Model이 Border를 보고 있음
```

같은 Pattern을 발견할 수 있다.

---

# 147. 하지만 주의

Grad-CAM Heatmap만 보고:

```text
이것이 Model의 완전한 판단 이유다.
```

라고 단정하면 안 된다.

---

# 148. 좋은 Evaluation

```text
숫자
+
Class별 성능
+
Error 유형
+
Image 확인
+
Model Confidence
+
설명 가능성 분석
```

을 함께 본다.

---

# 149. Evaluation Checklist

```text
Validation Data가 독립적인가?

Class Distribution은 어떤가?

Confusion Matrix를 봤는가?

Accuracy만 보지 않았는가?

Class별 Precision/Recall/F1을 봤는가?

ROC-AUC는 Probability로 계산했는가?

Class Imbalance가 있다면 PR 성능도 봤는가?

Threshold가 목적에 맞는가?

FP와 FN을 따로 봤는가?

High-confidence Error를 확인했는가?

Failure Image를 직접 봤는가?
```

---

# 150. 의료영상 Evaluation Checklist

```text
Patient-level Split인가?

False Negative가 몇 개인가?

Sensitivity는 충분한가?

Specificity는 어떤가?

Class별 Sample 수는 충분한가?

기관/장비별 성능 차이는 없는가?

Probability가 지나치게 확신되어 있지 않은가?

Failure Case가 특정 촬영 조건에 몰려 있지 않은가?
```

---

# 151. Metric 선택 표

```text
Accuracy
→ 전체적으로 얼마나 맞혔는가?

Precision
→ Positive Prediction이 얼마나 정확한가?

Recall
→ 실제 Positive를 얼마나 찾았는가?

Specificity
→ 실제 Negative를 얼마나 잘 구분했는가?

F1
→ Precision과 Recall의 균형

ROC-AUC
→ Positive/Negative Score Ranking 성능

PR-AUC
→ Positive Class 중심 Precision/Recall 성능
```

---

# 152. 가장 중요한 공식

```text
Accuracy
=
(TP + TN)
/
(TP + TN + FP + FN)
```

```text
Precision
=
TP
/
(TP + FP)
```

```text
Recall
=
TP
/
(TP + FN)
```

```text
Specificity
=
TN
/
(TN + FP)
```

```text
F1
=
2 × Precision × Recall
/
(Precision + Recall)
```

---

# 153. 가장 중요한 관계

```text
FN 증가
→ Recall 감소
```

```text
FP 증가
→ Precision / Specificity에 악영향
```

이다.

---

# 154. Threshold 관계

일반적으로:

```text
Threshold ↓

Positive Prediction ↑

Recall ↑ 가능

FP ↑ 가능
```

반대로:

```text
Threshold ↑

Positive Prediction ↓

FP ↓ 가능

Recall ↓ 가능
```

이다.

---

# 155. 평가의 목적

Evaluation은 단순히:

```text
점수 하나 출력
```

하는 과정이 아니다.

---

# 156. 더 중요한 목적

```text
Model이 무엇을 잘하는가?

무엇을 못하는가?

어떤 Error가 위험한가?

어떤 Data에서 실패하는가?

다음 Experiment는 무엇이어야 하는가?
```

를 알아내는 과정이다.

---

# 157. Evaluation → 다음 Experiment

```text
Evaluation
↓
Failure Analysis
↓
Problem 발견
↓
Hypothesis
↓
새 Experiment
↓
다시 Evaluation
```

이다.

---

# 158. 전체 연결

```text
Train Model
↓
Validation
↓
Probability
↓
Prediction
↓
Confusion Matrix
↓
Accuracy / Precision / Recall / F1
↓
ROC-AUC / PR-AUC
↓
Threshold Analysis
↓
Failure Cases
↓
High-confidence Errors
↓
다음 Experiment
```

---

# 159. 한 줄 정리

Image Classification Evaluation은:

```text
Model의 점수만 확인하는 것이 아니라
어떤 Image를 왜 틀리는지까지 분석하여
Model의 실제 강점과 약점을 파악하는 과정
```

이다.

---

# 160. 다음 단계

다음 파일:

```text
grad-cam.md
```

에서는:

```text
CNN이 Image의 어느 영역을 중요하게 사용했는가?
```

를 시각적으로 확인하는 Grad-CAM을 공부한다.

다음 내용을 연결한다.

```text
Feature Map

Gradient

Target Class

Heatmap

Original Image Overlay

Correct Prediction

False Positive

False Negative

High-confidence Error
```

그리고:

```text
Grad-CAM이 빨간 곳
=
질병이 있는 곳
```

이라고 단정하면 왜 안 되는지도 함께 공부한다.