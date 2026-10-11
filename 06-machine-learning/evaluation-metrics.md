# Evaluation Metrics 모델 평가 지표

## 1. Evaluation Metric이란?

Machine Learning 모델을 학습한 뒤에는:

```text
이 모델이 얼마나 잘 예측하는가?
```

를 평가해야 한다.

이때 사용하는 기준을:

```text
Evaluation Metric
평가 지표
```

라고 한다.

---

# 2. 왜 평가 지표가 필요한가?

예를 들어 두 모델이 있다고 하자.

```text
Model A
Accuracy = 95%

Model B
Accuracy = 90%
```

Accuracy만 보면:

```text
Model A가 더 좋다.
```

라고 생각하기 쉽다.

하지만 실제로는:

```text
어떤 Class를 맞혔는가?

어떤 Class를 놓쳤는가?

False Positive가 얼마나 있는가?

False Negative가 얼마나 있는가?
```

까지 확인해야 한다.

---

# 3. 문제에 따라 좋은 모델의 기준이 다르다

예:

```text
암 환자 선별
→ 환자를 놓치지 않는 것이 중요할 수 있음

스팸 이메일 분류
→ 정상 이메일을 스팸 처리하지 않는 것이 중요할 수 있음

대회 평가 기준이 Accuracy
→ Accuracy 자체가 직접적인 목표
```

따라서:

```text
무조건 가장 좋은 Metric
```

은 없다.

---

# 4. Classification과 Regression의 Metric

Classification:

```text
Accuracy
Precision
Recall
Specificity
F1
ROC-AUC
PR-AUC
Log Loss
```

Regression:

```text
MAE
MSE
RMSE
R²
```

등을 사용한다.

이번 파일에서는 Classification Metric을 중심으로 보고 마지막에 Regression Metric도 다시 정리한다.

---

# 5. 가장 먼저 알아야 할 Confusion Matrix

Binary Classification 결과를 다음처럼 정리할 수 있다.

```text
                     Prediction

                   0             1

Actual 0          TN            FP

Actual 1          FN            TP
```

이것을:

```text
Confusion Matrix
혼동 행렬
```

이라고 한다.

---

# 6. Positive / Negative

예를 들어:

```text
0 = 정상
1 = 질병
```

이라고 하자.

그러면 일반적으로:

```text
Positive = 질병
Negative = 정상
```

으로 정의할 수 있다.

---

# 7. True / False

```text
True
→ 예측이 맞음

False
→ 예측이 틀림
```

이다.

따라서:

```text
TP
TN
FP
FN
```

을 읽을 수 있다.

---

# 8. TP

```text
True Positive
```

실제 Positive이고 예측도 Positive이다.

예:

```text
실제 질병
→ 질병이라고 예측
```

---

# 9. TN

```text
True Negative
```

실제 Negative이고 예측도 Negative이다.

예:

```text
실제 정상
→ 정상이라고 예측
```

---

# 10. FP

```text
False Positive
```

실제 Negative인데 Positive라고 잘못 예측했다.

예:

```text
실제 정상
→ 질병이라고 예측
```

---

# 11. FN

```text
False Negative
```

실제 Positive인데 Negative라고 잘못 예측했다.

예:

```text
실제 질병
→ 정상이라고 예측
```

---

# 12. 숫자로 직접 계산해보기

100명의 환자가 있다고 하자.

```text
TN = 70
FP = 10
FN = 5
TP = 15
```

전체:

```text
70 + 10 + 5 + 15
=
100
```

이다.

Confusion Matrix:

```text
                     Prediction

                   정상        질병

Actual 정상         70          10

Actual 질병          5          15
```

---

# 13. Accuracy

Accuracy는:

```text
전체 Sample 중
정답을 맞힌 비율
```

이다.

수식:

```text
Accuracy
=
(TP + TN)
/
(TP + TN + FP + FN)
```

---

# 14. 직접 계산

현재:

```text
TP = 15
TN = 70
FP = 10
FN = 5
```

이므로:

```text
Accuracy
=
(15 + 70)
/
100

=
0.85
```

즉:

```text
Accuracy = 85%
```

이다.

---

# 15. Python Accuracy

```python
from sklearn.metrics import (
    accuracy_score
)


accuracy = accuracy_score(
    y_true,
    y_pred
)


print(
    "Accuracy:",
    accuracy
)
```

---

# 16. Accuracy의 장점

직관적이다.

```text
전체 중 몇 %를 맞혔는가?
```

를 바로 알 수 있다.

Class가 비교적 균형적이고 오류 비용도 비슷하다면 유용하다.

---

# 17. Accuracy의 한계

예:

```text
정상 = 990
질병 = 10
```

모델:

```text
1000명 모두 정상이라고 예측
```

하면:

```text
Accuracy = 99%
```

이다.

하지만:

```text
질병 환자 발견 = 0명
```

이다.

따라서 불균형 데이터에서는 Accuracy 하나만 보면 위험하다.

---

# 18. Precision

Precision은:

```text
모델이 Positive라고 예측한 것 중
실제로 Positive인 비율
```

이다.

수식:

```text
Precision
=
TP
/
(TP + FP)
```

---

# 19. 직접 계산

```text
TP = 15
FP = 10
```

이므로:

```text
Precision
=
15 / (15 + 10)

=
15 / 25

=
0.60
```

즉:

```text
Precision = 60%
```

이다.

---

# 20. Precision 해석

모델이:

```text
질병이라고 25명 예측
```

했는데 실제 질병은:

```text
15명
```

이었다.

따라서:

```text
질병이라고 예측한 사람 중
60%가 실제 질병
```

이라는 의미다.

---

# 21. Python Precision

```python
from sklearn.metrics import (
    precision_score
)


precision = precision_score(
    y_true,
    y_pred
)
```

---

# 22. Precision이 중요한 경우

False Positive를 줄이는 것이 중요한 문제에서 Precision을 중요하게 볼 수 있다.

예:

```text
정상 이메일을 스팸으로 잘못 차단

정상 거래를 사기로 잘못 판단

정상 환자에게 불필요한 추가 검사를 많이 권고
```

등이다.

---

# 23. Recall

Recall은:

```text
실제 Positive 중
모델이 Positive라고 찾아낸 비율
```

이다.

수식:

```text
Recall
=
TP
/
(TP + FN)
```

---

# 24. 직접 계산

```text
TP = 15
FN = 5
```

이므로:

```text
Recall
=
15 / (15 + 5)

=
15 / 20

=
0.75
```

즉:

```text
Recall = 75%
```

이다.

---

# 25. Recall 해석

실제 질병 환자가:

```text
20명
```

있었는데 모델이:

```text
15명
```

을 찾아냈다.

따라서:

```text
실제 질병 환자의 75%를 발견
```

했다는 뜻이다.

---

# 26. Python Recall

```python
from sklearn.metrics import (
    recall_score
)


recall = recall_score(
    y_true,
    y_pred
)
```

---

# 27. Sensitivity

의료 분야에서는 Recall을:

```text
Sensitivity
민감도
```

라고 부르는 경우가 많다.

즉:

```text
Recall
=
Sensitivity
=
TP / (TP + FN)
```

이다.

---

# 28. Recall이 중요한 경우

False Negative를 줄이는 것이 중요한 문제에서 Recall을 중요하게 볼 수 있다.

예:

```text
질병 선별

위험 환자 탐지

사기 거래 탐지

고장 가능성 탐지
```

등이다.

단, 실제 운영에서는 False Positive 비용도 함께 고려해야 한다.

---

# 29. Specificity

Specificity는:

```text
실제 Negative 중
Negative라고 정확히 예측한 비율
```

이다.

수식:

```text
Specificity
=
TN
/
(TN + FP)
```

---

# 30. 직접 계산

```text
TN = 70
FP = 10
```

이므로:

```text
Specificity
=
70 / (70 + 10)

=
70 / 80

=
0.875
```

즉:

```text
Specificity = 87.5%
```

이다.

---

# 31. Specificity 해석

실제 정상:

```text
80명
```

중:

```text
70명
```

을 정상이라고 올바르게 판단했다.

---

# 32. Recall과 Specificity

```text
Recall / Sensitivity
→ Positive를 얼마나 잘 찾는가?

Specificity
→ Negative를 얼마나 잘 찾는가?
```

이다.

---

# 33. Specificity Python 계산

Scikit-learn에 별도의 `specificity_score`를 사용하지 않고 Confusion Matrix로 직접 계산할 수 있다.

```python
from sklearn.metrics import (
    confusion_matrix
)


tn, fp, fn, tp = (
    confusion_matrix(
        y_true,
        y_pred
    ).ravel()
)


specificity = (
    tn
    /
    (tn + fp)
)


print(
    "Specificity:",
    specificity
)
```

---

# 34. F1 Score

F1 Score는:

```text
Precision과 Recall의 조화평균
```

이다.

수식:

```text
F1
=
2 ×
(Precision × Recall)
/
(Precision + Recall)
```

---

# 35. 직접 계산

현재:

```text
Precision = 0.60
Recall = 0.75
```

따라서:

```text
F1
=
2 ×
(0.60 × 0.75)
/
(0.60 + 0.75)

=
0.667 정도
```

이다.

---

# 36. 왜 단순 평균이 아닌가?

단순 평균이라면:

```text
(0.60 + 0.75) / 2
=
0.675
```

이다.

F1은 조화평균을 사용하기 때문에 둘 중 하나가 매우 낮으면 Score도 크게 영향을 받는다.

---

# 37. Python F1

```python
from sklearn.metrics import (
    f1_score
)


f1 = f1_score(
    y_true,
    y_pred
)
```

---

# 38. Precision vs Recall

쉽게 기억하면:

```text
Precision
→ 내가 Positive라고 한 것들이 얼마나 정확한가?

Recall
→ 실제 Positive를 얼마나 많이 찾아냈는가?
```

이다.

---

# 39. Precision과 Recall의 Trade-off

Threshold를 바꾸면 둘 사이에 Trade-off가 생기는 경우가 많다.

예:

```text
Threshold 낮춤

Positive 예측 증가
↓
Recall 증가 가능
↓
FP 증가 가능
↓
Precision 감소 가능
```

---

# 40. 반대

```text
Threshold 높임

Positive 예측 감소
↓
FP 감소 가능
↓
Precision 증가 가능
↓
FN 증가 가능
↓
Recall 감소 가능
```

---

# 41. classification_report

여러 Classification Metric을 한 번에 확인할 수 있다.

```python
from sklearn.metrics import (
    classification_report
)


print(
    classification_report(
        y_true,
        y_pred,
        zero_division=0
    )
)
```

---

# 42. 예시 출력

```text
              precision    recall  f1-score   support

           0       0.93      0.88      0.90        80
           1       0.60      0.75      0.67        20

    accuracy                           0.85       100
   macro avg       0.77      0.81      0.79       100
weighted avg       0.86      0.85      0.86       100
```

---

# 43. support

`support`는:

```text
실제 해당 Class Sample 수
```

이다.

예:

```text
Class 0 support = 80

Class 1 support = 20
```

이다.

---

# 44. Class별 Precision / Recall

`classification_report`에서는:

```text
Class 0

Class 1
```

각각의 Precision, Recall, F1을 확인할 수 있다.

불균형 데이터에서는 이 부분을 꼭 확인하는 것이 좋다.

---

# 45. Macro Average

```text
macro avg
```

는 각 Class의 Metric을 동일한 중요도로 평균한다.

예:

```text
Class 0 F1 = 0.90

Class 1 F1 = 0.50
```

이면:

```text
Macro F1
=
(0.90 + 0.50) / 2

=
0.70
```

이다.

---

# 46. Macro의 특징

Sample 수가:

```text
Class 0 = 990
Class 1 = 10
```

이어도 두 Class를:

```text
1 : 1
```

처럼 동일한 중요도로 평균한다.

따라서 Class별 성능을 균등하게 중요하게 보고 싶을 때 유용하다.

---

# 47. Weighted Average

```text
weighted avg
```

는 각 Class의 Sample 수를 Weight로 사용하여 평균한다.

즉:

```text
Sample이 많은 Class
→ 평균에 더 큰 영향
```

을 준다.

---

# 48. Macro vs Weighted

```text
Macro
→ 모든 Class를 동일하게 중요하게 봄

Weighted
→ 실제 Class Sample 수를 반영
```

이다.

---

# 49. 불균형 데이터에서는?

예:

```text
Class 0 = 95%
Class 1 = 5%
```

이면 Weighted F1은 Majority Class의 영향을 많이 받을 수 있다.

따라서:

```text
Macro F1

Class 1 Recall

Class 1 Precision
```

등을 함께 확인하는 것이 중요하다.

---

# 50. Micro Average

Micro Average는 모든 Class의 TP, FP, FN 등을 먼저 합친 뒤 Metric을 계산하는 방식이다.

Multi-class 문제에서 자주 등장한다.

Single-label Multi-class Classification에서는 Micro F1이 Accuracy와 같은 값이 되는 경우가 있다.

---

# 51. Binary vs Multi-class

Binary:

```text
0
1
```

Multi-class:

```text
0
1
2
3
...
```

Multi-class에서는 Class별 Metric과:

```text
Macro

Micro

Weighted
```

평균 방식이 중요해진다.

---

# 52. Confusion Matrix Python

```python
from sklearn.metrics import (
    confusion_matrix
)


cm = confusion_matrix(
    y_true,
    y_pred
)


print(
    cm
)
```

---

# 53. 결과 읽기

Binary Classification에서 기본적인 Class 순서가 `[0, 1]`이라면:

```text
[[TN FP]
 [FN TP]]
```

형태이다.

---

# 54. 안전하게 Label 순서 지정

```python
cm = confusion_matrix(
    y_true,
    y_pred,
    labels=[
        0,
        1
    ]
)
```

처럼 명시할 수도 있다.

---

# 55. Confusion Matrix 시각화

```python
from sklearn.metrics import (
    ConfusionMatrixDisplay
)

import matplotlib.pyplot as plt


ConfusionMatrixDisplay.from_predictions(
    y_true,
    y_pred
)

plt.show()
```

---

# 56. Probability가 중요한 이유

Classification 모델은 단순 Class뿐 아니라 Probability 또는 Score를 출력할 수 있다.

```python
prob = model.predict_proba(
    X_test
)[:, 1]
```

예:

```text
0.02
0.13
0.42
0.78
0.96
```

---

# 57. predict와 predict_proba

```python
model.predict(
    X_test
)
```

결과:

```text
0
0
0
1
1
```

반면:

```python
model.predict_proba(
    X_test
)
```

는 Class별 Probability를 반환한다.

---

# 58. Binary predict_proba

예:

```text
[[0.90, 0.10],
 [0.20, 0.80]]
```

각 Row:

```text
[P(Class 0), P(Class 1)]
```

형태이다.

---

# 59. [:, 1]

그래서:

```python
prob = model.predict_proba(
    X_test
)[:, 1]
```

은:

```text
모든 Row의
Class 1 Probability
```

를 가져오는 것이다.

---

# 60. model.classes_ 확인

Class 순서를 확실히 확인하려면:

```python
print(
    model.classes_
)
```

를 확인하는 것이 좋다.

`[:, 1]`이 항상 의미상 원하는 Positive Class라고 무조건 가정해서는 안 된다.

---

# 61. Threshold

Probability를 Class로 바꾸는 기준이다.

예:

```text
Threshold = 0.5
```

이면:

```text
Probability >= 0.5
→ Class 1

Probability < 0.5
→ Class 0
```

으로 만들 수 있다.

---

# 62. 직접 Threshold 적용

```python
threshold = 0.5


pred = (
    prob >= threshold
).astype(int)
```

---

# 63. Threshold 0.3

```python
threshold = 0.3


pred = (
    prob >= threshold
).astype(int)
```

Threshold가 낮아지면 Positive Prediction이 일반적으로 증가한다.

---

# 64. Threshold는 Metric을 바꾼다

같은 모델이어도:

```text
Threshold = 0.2

Threshold = 0.5

Threshold = 0.8
```

에 따라:

```text
Accuracy
Precision
Recall
F1
Specificity
```

가 달라질 수 있다.

---

# 65. Threshold별 Metric 비교

```python
from sklearn.metrics import (
    accuracy_score,
    precision_score,
    recall_score,
    f1_score
)


thresholds = [
    0.2,
    0.3,
    0.4,
    0.5,
    0.6,
    0.7,
    0.8
]


for threshold in thresholds:

    pred = (
        prob >= threshold
    ).astype(int)

    accuracy = accuracy_score(
        y_valid,
        pred
    )

    precision = precision_score(
        y_valid,
        pred,
        zero_division=0
    )

    recall = recall_score(
        y_valid,
        pred,
        zero_division=0
    )

    f1 = f1_score(
        y_valid,
        pred,
        zero_division=0
    )


    print(
        f"Threshold={threshold:.1f}",
        f"Accuracy={accuracy:.4f}",
        f"Precision={precision:.4f}",
        f"Recall={recall:.4f}",
        f"F1={f1:.4f}"
    )
```

---

# 66. Threshold는 어디서 정할까?

좋은 방법:

```text
Validation Set

또는

Cross Validation OOF Prediction
```

에서 결정한다.

최종 Test 결과를 계속 보면서 Threshold를 바꾸면 Test에 Overfitting될 수 있다.

---

# 67. ROC Curve

ROC:

```text
Receiver Operating Characteristic
```

Curve이다.

Threshold를 여러 값으로 바꾸면서:

```text
True Positive Rate

False Positive Rate
```

의 관계를 본다.

---

# 68. TPR

```text
True Positive Rate
=
Recall
=
Sensitivity
```

이다.

수식:

```text
TPR
=
TP / (TP + FN)
```

---

# 69. FPR

```text
False Positive Rate
```

수식:

```text
FPR
=
FP / (FP + TN)
```

이다.

---

# 70. Specificity와 FPR

```text
Specificity
=
TN / (TN + FP)
```

따라서:

```text
FPR
=
1 - Specificity
```

이다.

---

# 71. ROC Curve Python

```python
from sklearn.metrics import (
    roc_curve
)


fpr, tpr, thresholds = (
    roc_curve(
        y_true,
        prob
    )
)
```

---

# 72. ROC Curve 그리기

```python
import matplotlib.pyplot as plt


plt.plot(
    fpr,
    tpr
)

plt.plot(
    [0, 1],
    [0, 1],
    linestyle="--"
)

plt.xlabel(
    "False Positive Rate"
)

plt.ylabel(
    "True Positive Rate"
)

plt.title(
    "ROC Curve"
)

plt.show()
```

---

# 73. ROC-AUC

AUC:

```text
Area Under the Curve
```

즉 ROC Curve 아래의 면적이다.

```text
ROC-AUC
```

는 모델이 Positive와 Negative를 얼마나 잘 순위 구분하는지를 나타내는 지표로 볼 수 있다.

---

# 74. Python ROC-AUC

```python
from sklearn.metrics import (
    roc_auc_score
)


auc = roc_auc_score(
    y_true,
    prob
)


print(
    "ROC-AUC:",
    auc
)
```

---

# 75. 중요한 점

ROC-AUC에는:

```python
y_pred
```

보다:

```python
prob
```

또는 적절한 continuous decision score를 넣는다.

예:

```python
roc_auc_score(
    y_test,
    prob
)
```

이다.

---

# 76. ROC-AUC 해석

일반적으로:

```text
1.0
→ 완벽한 순위 구분

0.5
→ 무작위 수준의 순위 구분
```

으로 이해할 수 있다.

하지만 특정 숫자를 무조건 좋은/나쁜 모델의 절대 기준으로 사용해서는 안 된다.

---

# 77. AUC 0.9 = Accuracy 90%?

아니다.

```text
ROC-AUC = 0.90
```

은:

```text
Accuracy = 90%
```

라는 뜻이 아니다.

둘은 완전히 다른 Metric이다.

---

# 78. ROC-AUC의 장점

Threshold 하나를 고정하지 않고 여러 Threshold에서 모델의 순위 구분 능력을 평가할 수 있다.

---

# 79. ROC-AUC의 한계

Positive Class가 매우 희귀한 데이터에서는 ROC-AUC가 좋아 보여도 실제 Positive Prediction의 Precision이 낮을 수 있다.

그래서 불균형 문제에서는 Precision-Recall 관점도 함께 본다.

---

# 80. Precision-Recall Curve

Threshold를 바꾸면서:

```text
Precision

Recall
```

의 관계를 보여주는 Curve이다.

---

# 81. Python PR Curve

```python
from sklearn.metrics import (
    precision_recall_curve
)


precision, recall, thresholds = (
    precision_recall_curve(
        y_true,
        prob
    )
)
```

---

# 82. PR Curve 그리기

```python
import matplotlib.pyplot as plt


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

plt.title(
    "Precision-Recall Curve"
)

plt.show()
```

---

# 83. Average Precision

Scikit-learn에서는:

```python
from sklearn.metrics import (
    average_precision_score
)


ap = average_precision_score(
    y_true,
    prob
)


print(
    "Average Precision:",
    ap
)
```

를 사용할 수 있다.

---

# 84. PR-AUC와 Average Precision

둘은 관련 있지만 계산 방식이 완전히 동일한 용어라고 단순하게 생각하면 안 된다.

Scikit-learn의:

```text
average_precision_score
```

는 Precision-Recall Curve를 요약하는 Average Precision을 계산한다.

---

# 85. 언제 PR 관점이 중요할까?

Positive가 희귀한 문제:

```text
질병 환자

사기 거래

이상 탐지

고장 탐지
```

등에서 Precision과 Recall 관계가 매우 중요할 수 있다.

---

# 86. ROC-AUC와 PR을 같이 보기

예:

```text
ROC-AUC = 0.94

Average Precision = 0.42
```

일 수 있다.

이런 경우:

```text
순위 구분 능력은 좋아 보이지만
희귀 Positive를 실제로 찾아낼 때 Precision 문제가 있을 수 있다.
```

처럼 추가 분석이 필요하다.

---

# 87. Balanced Accuracy

Class Imbalance가 있을 때 사용할 수 있는 또 다른 Metric이다.

Binary Classification에서 개념적으로:

```text
Balanced Accuracy
=
(Sensitivity + Specificity) / 2
```

로 이해할 수 있다.

---

# 88. Python

```python
from sklearn.metrics import (
    balanced_accuracy_score
)


balanced_acc = (
    balanced_accuracy_score(
        y_true,
        y_pred
    )
)
```

---

# 89. 왜 Balanced Accuracy?

일반 Accuracy에서는 Majority Class가 결과를 지배할 수 있다.

Balanced Accuracy는 Class별 Recall을 균형 있게 반영한다.

---

# 90. MCC

MCC:

```text
Matthews Correlation Coefficient
```

도 Binary Classification 평가에 사용할 수 있다.

```python
from sklearn.metrics import (
    matthews_corrcoef
)


mcc = matthews_corrcoef(
    y_true,
    y_pred
)
```

불균형 데이터에서도 유용하게 사용되는 지표 중 하나이다.

---

# 91. Log Loss

Class 자체가 아니라 예측 Probability의 품질을 평가할 수 있는 Metric이다.

```python
from sklearn.metrics import (
    log_loss
)


loss = log_loss(
    y_true,
    prob
)
```

일반적으로:

```text
낮을수록 좋다.
```

---

# 92. Confidence가 틀렸을 때

예:

실제:

```text
Class 0
```

인데 모델이:

```text
P(Class 1) = 0.999
```

처럼 매우 확신하면서 틀리면 Log Loss에서 큰 불이익을 받을 수 있다.

---

# 93. Probability Calibration

모델이:

```text
Probability = 0.8
```

이라고 예측한 Sample들을 모았을 때 실제로 약 80%가 Positive인지 보는 개념이:

```text
Calibration
```

이다.

---

# 94. 높은 AUC = 좋은 Calibration?

아니다.

모델이 순위를 매우 잘 구분해서:

```text
ROC-AUC가 높음
```

이어도 Probability 자체는 Calibration이 나쁠 수 있다.

---

# 95. Metric을 계산할 데이터

다음은 좋지 않다.

```text
Training Data로 학습
↓
같은 Training Data에서 Score
```

이 결과만으로 일반화 성능을 판단하는 것이다.

---

# 96. Validation / Test에서 평가

기본:

```text
Train
→ Model fit

Validation
→ Model 선택

Test
→ 최종 평가
```

이다.

또는:

```text
Train
→ Cross Validation
→ 모델 선택

Test
→ 최종 평가
```

를 사용할 수 있다.

---

# 97. Training Score도 쓸모가 있다

Training Score가 필요 없다는 뜻은 아니다.

예:

```text
Train Accuracy = 0.99

Validation Accuracy = 0.75
```

라면:

```text
Overfitting 가능성
```

을 의심할 수 있다.

---

# 98. 반대로

```text
Train Accuracy = 0.62

Validation Accuracy = 0.60
```

이면 모델이 충분히 학습하지 못한:

```text
Underfitting
```

가능성을 생각할 수 있다.

---

# 99. Metric 비교는 같은 데이터에서

잘못된 비교:

```text
Model A
→ Validation Set A
→ Accuracy 0.90

Model B
→ Validation Set B
→ Accuracy 0.92
```

이렇게 하면 Split 차이까지 섞인다.

가능하면:

```text
같은 Validation

같은 CV Fold

같은 Metric
```

으로 비교한다.

---

# 100. Cross Validation Metric

```python
from sklearn.model_selection import (
    cross_val_score
)


scores = cross_val_score(
    model,
    X,
    y,
    cv=skf,
    scoring="roc_auc"
)


print(
    scores
)

print(
    scores.mean()
)

print(
    scores.std()
)
```

---

# 101. scoring

Classification에서 예:

```text
accuracy

balanced_accuracy

precision

recall

f1

roc_auc
```

등을 사용할 수 있다.

---

# 102. 여러 Metric을 동시에 평가

```python
from sklearn.model_selection import (
    cross_validate
)


scoring = {
    "accuracy": "accuracy",
    "precision": "precision",
    "recall": "recall",
    "f1": "f1",
    "auc": "roc_auc"
}


result = cross_validate(
    model,
    X,
    y,
    cv=skf,
    scoring=scoring
)
```

---

# 103. 결과 확인

```python
print(
    result.keys()
)
```

예:

```text
fit_time
score_time
test_accuracy
test_precision
test_recall
test_f1
test_auc
```

---

# 104. 평균 계산

```python
import numpy as np


print(
    "Accuracy:",
    np.mean(
        result[
            "test_accuracy"
        ]
    )
)


print(
    "Recall:",
    np.mean(
        result[
            "test_recall"
        ]
    )
)


print(
    "F1:",
    np.mean(
        result[
            "test_f1"
        ]
    )
)


print(
    "ROC-AUC:",
    np.mean(
        result[
            "test_auc"
        ]
    )
)
```

---

# 105. 어떤 Metric을 선택해야 할까?

먼저 질문한다.

```text
어떤 오류가 더 중요한가?
```

---

# 106. FN이 특히 중요한 경우

False Negative:

```text
실제 Positive인데
Negative라고 예측
```

이다.

FN을 줄이는 것이 중요하면:

```text
Recall / Sensitivity
```

를 중요하게 볼 수 있다.

---

# 107. FP가 특히 중요한 경우

False Positive:

```text
실제 Negative인데
Positive라고 예측
```

이다.

FP를 줄이는 것이 중요하면:

```text
Precision

Specificity
```

등을 함께 볼 수 있다.

어떤 지표가 더 직접적인지는 문제 정의에 따라 다르다.

---

# 108. Precision과 Specificity 차이

헷갈리기 쉽다.

Precision:

```text
Positive라고 예측한 것 중
얼마나 맞았는가?
```

Specificity:

```text
실제 Negative 중
얼마나 Negative로 맞혔는가?
```

이다.

---

# 109. F1을 선택할 수 있는 경우

Precision과 Recall을 둘 다 중요하게 보고 하나의 숫자로 요약하고 싶다면:

```text
F1
```

을 사용할 수 있다.

하지만 F1은 TN을 직접 반영하지 않는다는 점도 기억한다.

---

# 110. Accuracy를 선택할 수 있는 경우

Class가 비교적 균형적이고:

```text
FP와 FN의 비용도 비슷
```

하다면 Accuracy가 직관적인 Metric이 될 수 있다.

또 대회의 공식 평가 Metric이 Accuracy라면 해당 Metric을 직접 최적화하는 것도 중요하다.

---

# 111. ROC-AUC를 선택할 수 있는 경우

특정 Threshold보다:

```text
Positive와 Negative의 전반적인 Ranking 능력
```

을 비교하고 싶을 때 유용하다.

---

# 112. PR Metric을 선택할 수 있는 경우

Positive가 희귀하고:

```text
Positive Prediction의 품질

실제 Positive 발견률
```

이 중요하다면 Precision-Recall 관점이 유용할 수 있다.

---

# 113. Metric 하나만 보는 것의 문제

예:

```text
Accuracy만 높음

Recall 매우 낮음
```

또는:

```text
Recall 매우 높음

Precision 매우 낮음
```

일 수 있다.

그래서 중요한 프로젝트에서는 여러 Metric을 함께 확인한다.

---

# 114. 예시 평가표

| Model | Accuracy | Precision | Recall | F1 | ROC-AUC |
|---|---:|---:|---:|---:|---:|
| Logistic | 0.89 | 0.61 | 0.72 | 0.66 | 0.85 |
| Random Forest | 0.92 | 0.75 | 0.58 | 0.65 | 0.88 |
| XGBoost | 0.91 | 0.69 | 0.74 | 0.71 | 0.90 |

이 경우:

```text
Accuracy만 보면 Random Forest

Recall/F1/AUC까지 보면 XGBoost가 더 적합할 수도 있음
```

이다.

최종 선택은 문제 목적에 따라 결정한다.

---

# 115. Threshold도 같이 기록

예:

| Model | Threshold | Precision | Recall | F1 |
|---|---:|---:|---:|---:|
| XGBoost | 0.3 | 0.55 | 0.88 | 0.68 |
| XGBoost | 0.5 | 0.70 | 0.72 | 0.71 |
| XGBoost | 0.7 | 0.83 | 0.50 | 0.62 |

같은 모델이어도 Threshold에 따라 결과가 달라진다.

---

# 116. Threshold 0.5가 정답?

아니다.

`0.5`는 흔히 사용되는 기본 기준일 뿐 모든 문제의 최적 Threshold라는 뜻은 아니다.

---

# 117. Threshold 선택 예

목표가:

```text
Recall >= 0.90
```

이라고 하자.

그 조건을 만족하는 Threshold 중:

```text
Precision이 가장 높은 Threshold
```

를 선택하는 방식도 가능하다.

즉 Threshold 선택에도 문제의 목적이 필요하다.

---

# 118. 의료 AI에서 중요한 점

의료 AI에서는 단순히:

```text
Accuracy가 가장 높은 모델
```

을 선택하는 것이 항상 적절하지 않다.

예:

```text
질병을 놓쳤을 때 위험

False Alarm 비용

추가 검사 가능 여부

질병 유병률

실제 사용 환경
```

등을 함께 고려해야 한다.

---

# 119. Screening과 Diagnosis

예를 들어 선별 Screening 시스템에서는:

```text
놓치지 않는 것
```

을 중요하게 보아 Sensitivity를 높게 요구할 수 있다.

하지만 최종 진단 시스템은 다른 요구 조건을 가질 수 있다.

실제 기준은 의료 목적과 임상 검증에 따라 정해야 한다.

---

# 120. Competition Metric

대회에서는 공식 평가 Metric을 먼저 확인해야 한다.

예:

```text
Accuracy

F1

Macro F1

ROC-AUC

Log Loss
```

등이다.

---

# 121. 공식 Metric이 Accuracy라면?

최종 Leaderboard는 Accuracy로 결정될 수 있다.

그렇더라도 개발 과정에서:

```text
Confusion Matrix

Class별 Recall

Probability Distribution
```

등을 확인하면 모델의 문제를 이해하는 데 도움이 된다.

---

# 122. Regression Metric

Regression에서는 연속된 숫자를 예측하므로 Classification Metric을 사용할 수 없다.

예:

```text
실제 집값 = 50000

예측 집값 = 47000
```

처럼 예측 오차의 크기를 평가한다.

---

# 123. MAE

```text
Mean Absolute Error
```

각 예측 오차의 절댓값 평균이다.

수식:

```text
MAE
=
평균(
    |실제값 - 예측값|
)
```

---

# 124. MAE 예

실제:

```text
10
20
30
```

예측:

```text
12
18
35
```

오차 절댓값:

```text
2
2
5
```

MAE:

```text
(2 + 2 + 5) / 3
=
3
```

이다.

---

# 125. Python MAE

```python
from sklearn.metrics import (
    mean_absolute_error
)


mae = mean_absolute_error(
    y_true,
    y_pred
)
```

---

# 126. MSE

```text
Mean Squared Error
```

오차를 제곱한 뒤 평균한다.

```text
MSE
=
평균(
    (실제값 - 예측값)²
)
```

큰 오차에 더 큰 벌점을 준다.

---

# 127. Python MSE

```python
from sklearn.metrics import (
    mean_squared_error
)


mse = mean_squared_error(
    y_true,
    y_pred
)
```

---

# 128. RMSE

```text
Root Mean Squared Error
```

MSE에 Square Root를 적용한다.

```text
RMSE
=
√MSE
```

---

# 129. Python RMSE

버전 호환성을 고려하면:

```python
import numpy as np

from sklearn.metrics import (
    mean_squared_error
)


rmse = np.sqrt(
    mean_squared_error(
        y_true,
        y_pred
    )
)
```

처럼 계산할 수 있다.

---

# 130. MAE vs RMSE

```text
MAE
→ 오차 절댓값 평균
→ 비교적 직관적

RMSE
→ 큰 오차에 더 민감
```

이다.

---

# 131. R²

```text
R-squared
결정계수
```

Regression 모델이 Target의 변동을 얼마나 설명하는지를 나타내는 대표적인 지표이다.

```python
from sklearn.metrics import (
    r2_score
)


r2 = r2_score(
    y_true,
    y_pred
)
```

---

# 132. R² 해석 주의

일반적인 상황에서:

```text
R² = 1
→ 완벽한 예측

R² = 0
→ 평균 예측 수준과 비슷

R² < 0
→ 평균을 예측하는 단순 기준보다 나쁠 수 있음
```

이다.

---

# 133. R² = 0.8은 Accuracy 80%?

아니다.

```text
R² = 0.8
```

은:

```text
Accuracy = 80%
```

라는 뜻이 아니다.

Regression의 R²와 Classification Accuracy는 완전히 다른 지표이다.

---

# 134. Classification Metric 정리

```text
Accuracy
→ 전체 중 맞춘 비율

Precision
→ Positive라고 예측한 것 중 실제 Positive 비율

Recall
→ 실제 Positive 중 찾아낸 비율

Sensitivity
→ Recall과 같은 의미로 사용

Specificity
→ 실제 Negative 중 찾아낸 비율

F1
→ Precision과 Recall의 조화평균

ROC-AUC
→ 여러 Threshold에서 Ranking 성능

PR 관련 Metric
→ Precision과 Recall 관계 중심

Log Loss
→ Probability Prediction 품질
```

---

# 135. Regression Metric 정리

```text
MAE
→ 절대 오차 평균

MSE
→ 제곱 오차 평균

RMSE
→ MSE의 제곱근

R²
→ Target 변동 설명 정도를 나타내는 지표
```

---

# 136. 가장 중요한 질문

모델을 평가하기 전에:

```text
이 문제에서
어떤 실수가 가장 위험한가?
```

를 먼저 생각한다.

그다음 Metric을 선택한다.

---

# 137. 평가 순서

Classification에서 좋은 기본 흐름:

```text
Model Prediction
       ↓
Confusion Matrix
       ↓
Accuracy
       ↓
Precision / Recall
       ↓
F1
       ↓
Class별 Metric
       ↓
ROC-AUC
       ↓
PR Curve / Average Precision
       ↓
Threshold 분석
```

이다.

---

# 138. 불균형 데이터 평가 흐름

```text
Class Distribution 확인
       ↓
Confusion Matrix
       ↓
Minority Class Precision
       ↓
Minority Class Recall
       ↓
F1 / Macro F1
       ↓
ROC-AUC
       ↓
PR 관련 Metric
       ↓
Threshold
```

처럼 접근할 수 있다.

---

# 139. 전체 평가 코드

```python
from sklearn.metrics import (
    accuracy_score,
    precision_score,
    recall_score,
    f1_score,
    confusion_matrix,
    classification_report,
    roc_auc_score,
    average_precision_score
)


pred = model.predict(
    X_test
)


prob = model.predict_proba(
    X_test
)[:, 1]


accuracy = accuracy_score(
    y_test,
    pred
)


precision = precision_score(
    y_test,
    pred,
    zero_division=0
)


recall = recall_score(
    y_test,
    pred,
    zero_division=0
)


f1 = f1_score(
    y_test,
    pred,
    zero_division=0
)


auc = roc_auc_score(
    y_test,
    prob
)


ap = average_precision_score(
    y_test,
    prob
)


print(
    "Accuracy:",
    accuracy
)

print(
    "Precision:",
    precision
)

print(
    "Recall:",
    recall
)

print(
    "F1:",
    f1
)

print(
    "ROC-AUC:",
    auc
)

print(
    "Average Precision:",
    ap
)


print(
    "\nConfusion Matrix"
)

print(
    confusion_matrix(
        y_test,
        pred
    )
)


print(
    "\nClassification Report"
)

print(
    classification_report(
        y_test,
        pred,
        zero_division=0
    )
)
```

---

# 140. 이 코드를 읽는 순서

```text
1. predict()
→ 최종 Class 예측

2. predict_proba()
→ Positive Probability

3. Accuracy
→ 전체 정확도

4. Precision
→ Positive 예측 정확성

5. Recall
→ 실제 Positive 발견률

6. F1
→ Precision / Recall 균형

7. ROC-AUC
→ Ranking 성능

8. Average Precision
→ Precision-Recall 성능 요약

9. Confusion Matrix
→ 어떤 종류의 오류인지 확인

10. Classification Report
→ Class별 결과 확인
```

---

# 141. 자주 하는 실수

```text
Accuracy 하나만 보고 모델 선택

ROC-AUC를 Accuracy처럼 해석

R²를 Accuracy처럼 해석

predict() 결과로만 ROC-AUC 계산

Class 1이 항상 원하는 Positive라고 가정

Threshold를 Test에서 튜닝

불균형 데이터에서 Weighted F1만 보고 Minority 성능을 놓침

Train Score만 보고 모델 성능 판단
```

등을 주의한다.

---

# 142. Metric 선택 표

| 상황 | 우선 확인할 수 있는 Metric |
|---|---|
| 전체 정답 비율 | Accuracy |
| Positive 예측의 정확성 | Precision |
| Positive를 놓치지 않는 능력 | Recall / Sensitivity |
| Negative를 올바르게 제외 | Specificity |
| Precision + Recall 균형 | F1 |
| Class별 균형 | Macro F1 |
| 전반적 Ranking | ROC-AUC |
| 희귀 Positive 중심 | PR Curve / Average Precision |
| Probability 품질 | Log Loss / Calibration |
| Regression 평균 절대 오차 | MAE |
| 큰 Regression 오차를 더 크게 반영 | RMSE |
| Regression 설명 정도 | R² |

---

# 143. 가장 중요한 그림

```text
                    실제 값

              Negative     Positive

예측 Negative     TN           FN

예측 Positive     FP           TP
```

여기서:

```text
Accuracy
→ 전체를 본다.

Precision
→ 예측 Positive를 본다.

Recall
→ 실제 Positive를 본다.

Specificity
→ 실제 Negative를 본다.
```

---

# 144. 한 문장으로 기억하기

```text
Precision
→ "내가 잡았다고 한 것 중 진짜는?"

Recall
→ "진짜 중 내가 얼마나 잡았나?"
```

이렇게 기억하면 두 지표를 구분하기 쉽다.

---

# 145. 배운 점

모델 평가에서 가장 중요한 것은:

```text
Score가 높다
```

가 아니라:

```text
그 Score가 무엇을 의미하는가?
```

를 이해하는 것이다.

예를 들어:

```text
Accuracy = 95%
```

여도 Minority Class를 하나도 찾지 못할 수 있다.

따라서 Classification에서는:

```text
Confusion Matrix

Accuracy

Precision

Recall

Specificity

F1

ROC-AUC

Precision-Recall 관련 Metric
```

을 문제에 맞게 확인한다.

또한 같은 모델이라도:

```text
Threshold
```

에 따라 Precision, Recall, F1 등이 달라질 수 있다.

따라서 최종적으로는:

```text
문제의 목적
↓
중요한 오류 정의
↓
적절한 Metric 선택
↓
Validation / CV 평가
↓
Threshold 결정
↓
최종 Test
```

순서로 접근하는 것이 중요하다.

핵심은:

```text
좋은 모델이란
모든 Metric이 무조건 높은 모델이 아니라

실제 문제에서 중요한 목표를
잘 만족하는 모델이다.
```