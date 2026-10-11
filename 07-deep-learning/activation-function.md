# Activation Function

## 1. Activation Function이란?

Activation Function은 Neural Network의 Layer 출력값에 변환을 적용하는 함수이다.

한국어로:

```text
활성화 함수
```

라고 한다.

대표적인 Activation Function:

```text
ReLU
Sigmoid
Tanh
Softmax
```

등이 있다.

---

# 2. 기본적인 Neural Network

예:

```python
model = nn.Sequential(
    nn.Linear(4, 10),
    nn.ReLU(),
    nn.Linear(10, 2)
)
```

구조:

```text
Input
↓
Linear
↓
ReLU
↓
Linear
↓
Output
```

여기서:

```python
nn.ReLU()
```

가 Activation Function이다.

---

# 3. 왜 Activation Function이 필요할까?

이 부분이 가장 중요하다.

Linear Layer는 기본적으로:

```text
y = Wx + b
```

형태의 선형 변환을 수행한다.

그렇다면 Linear Layer를 여러 개 연결하면 더 복잡한 Model이 될 것처럼 보인다.

예:

```text
Input
↓
Linear 1
↓
Linear 2
↓
Linear 3
↓
Output
```

하지만 중간에 비선형 Activation이 없다면 중요한 한계가 있다.

---

# 4. Linear를 여러 개 연결해도 결국 Linear

간단하게 생각해보자.

첫 번째 Layer:

```text
h = W1x + b1
```

두 번째 Layer:

```text
y = W2h + b2
```

첫 번째 식을 두 번째 식에 넣으면:

```text
y = W2(W1x + b1) + b2
```

정리하면:

```text
y = W2W1x + W2b1 + b2
```

결국 다시:

```text
y = Wx + b
```

형태이다.

---

# 5. 즉

```text
Linear
↓
Linear
↓
Linear
```

을 여러 개 연결해도 중간에 비선형 변환이 없다면 전체적으로 하나의 Linear Transformation과 같은 형태로 합쳐질 수 있다.

따라서 Layer를 여러 개 쌓은 의미가 크게 줄어든다.

---

# 6. 그래서 Activation Function을 넣는다

```text
Linear
↓
Activation
↓
Linear
↓
Activation
↓
Linear
```

처럼 비선형 Activation Function을 넣는다.

이를 통해 Neural Network가 더 복잡한 비선형 관계를 표현할 수 있게 된다.

---

# 7. Linear vs Non-linear

Linear:

```text
직선적인 관계
```

Non-linear:

```text
곡선

복잡한 경계

복잡한 Pattern
```

을 표현할 수 있다.

실제 데이터에는 복잡한 관계가 많기 때문에 비선형성이 중요하다.

---

# 8. 대표적인 Activation Function

주로 배우는 Activation Function:

```text
ReLU
Sigmoid
Tanh
Softmax
```

각각 역할이 조금 다르다.

---

# 9. ReLU

가장 대표적인 Activation Function 중 하나이다.

정식 이름:

```text
Rectified Linear Unit
```

수식:

```text
ReLU(x) = max(0, x)
```

---

# 10. ReLU 동작

입력이 음수라면:

```text
0
```

입력이 양수라면:

```text
그 값을 그대로 사용
```

한다.

---

# 11. 예

입력:

```text
[-5, -2, 0, 3, 7]
```

ReLU:

```text
[0, 0, 0, 3, 7]
```

이다.

---

# 12. PyTorch ReLU

```python
import torch.nn as nn


relu = nn.ReLU()
```

---

# 13. Tensor에 적용

```python
import torch


x = torch.tensor(
    [
        -5.0,
        -2.0,
        0.0,
        3.0,
        7.0
    ]
)


relu = nn.ReLU()


y = relu(
    x
)


print(
    y
)
```

결과:

```text
tensor([0., 0., 0., 3., 7.])
```

---

# 14. 함수형 ReLU

다음처럼 사용할 수도 있다.

```python
import torch.nn.functional as F


y = F.relu(
    x
)
```

---

# 15. ReLU를 Hidden Layer에서 많이 사용하는 이유

ReLU는:

```text
계산이 단순함

비선형성을 추가함

깊은 Neural Network에서 널리 사용됨
```

등의 이유로 Hidden Layer에서 자주 사용된다.

---

# 16. ReLU Shape

ReLU는 일반적으로 Tensor의 Shape를 바꾸지 않는다.

```text
Input
[32, 128]

↓

ReLU

↓

Output
[32, 128]
```

값만 변환한다.

---

# 17. ReLU 예제 Model

```python
model = nn.Sequential(
    nn.Linear(
        12,
        64
    ),

    nn.ReLU(),

    nn.Linear(
        64,
        32
    ),

    nn.ReLU(),

    nn.Linear(
        32,
        2
    )
)
```

---

# 18. 구조

```text
12 Features
↓
Linear
12 → 64
↓
ReLU
↓
Linear
64 → 32
↓
ReLU
↓
Linear
32 → 2
↓
Logits
```

---

# 19. ReLU의 문제점

ReLU가 항상 완벽한 것은 아니다.

음수 영역에서는:

```text
Output = 0
```

이고 Gradient도 0이 된다.

특정 Neuron이 계속 음수 영역에 머물면 학습이 잘 진행되지 않을 수 있다.

이를 흔히:

```text
Dying ReLU
```

라고 부른다.

---

# 20. Leaky ReLU

ReLU의 음수 영역을 완전히 0으로 만들지 않고 작은 기울기를 남기는 방법이다.

```python
nn.LeakyReLU(
    negative_slope=0.01
)
```

---

# 21. 개념

ReLU:

```text
x < 0
→ 0
```

Leaky ReLU:

```text
x < 0
→ 작은 음수 값 유지
```

이다.

---

# 22. 예

입력:

```text
-5
```

ReLU:

```text
0
```

Leaky ReLU에서 `negative_slope=0.01`이라면:

```text
-0.05
```

이다.

---

# 23. ReLU 계열

실제 Neural Network에서는 다음과 같은 함수도 볼 수 있다.

```text
LeakyReLU

PReLU

ELU

GELU
```

등이다.

---

# 24. GELU

최근 여러 Neural Network Architecture에서:

```text
GELU
```

도 자주 사용된다.

PyTorch:

```python
nn.GELU()
```

특히 Transformer 계열 Model에서 자주 볼 수 있다.

처음에는 ReLU를 기준으로 Activation의 역할을 이해하는 것이 중요하다.

---

# 25. Sigmoid

Sigmoid는 입력값을:

```text
0 ~ 1
```

사이의 값으로 변환한다.

수식:

```text
sigmoid(x)
=
1 / (1 + e^(-x))
```

---

# 26. Sigmoid 출력

입력이 매우 작은 음수:

```text
0에 가까움
```

입력:

```text
0
```

이면:

```text
0.5
```

입력이 매우 큰 양수:

```text
1에 가까움
```

이다.

---

# 27. PyTorch Sigmoid

```python
sigmoid = nn.Sigmoid()
```

또는:

```python
prob = torch.sigmoid(
    logits
)
```

을 사용할 수 있다.

---

# 28. 예

```python
x = torch.tensor(
    [
        -2.0,
        0.0,
        2.0
    ]
)


print(
    torch.sigmoid(
        x
    )
)
```

결과는 대략:

```text
[0.119, 0.500, 0.881]
```

이다.

---

# 29. Sigmoid와 Probability

Binary Classification에서 하나의 Logit을 출력하는 Model이라면:

```text
Logit
↓
Sigmoid
↓
0 ~ 1
```

로 변환해 Positive Class Probability처럼 해석할 수 있다.

---

# 30. Binary Classification 예

Model:

```python
model = nn.Sequential(
    nn.Linear(
        20,
        64
    ),

    nn.ReLU(),

    nn.Linear(
        64,
        1
    )
)
```

Output:

```text
1 Logit
```

이다.

---

# 31. Probability 계산

```python
logits = model(
    x
)


prob = torch.sigmoid(
    logits
)
```

---

# 32. Threshold

예:

```python
pred = (
    prob >= 0.5
).long()
```

이면:

```text
Probability >= 0.5
→ Class 1

Probability < 0.5
→ Class 0
```

이다.

---

# 33. 하지만 Training에서는?

`BCEWithLogitsLoss`를 사용한다면 Model 마지막에 Sigmoid를 직접 넣지 않는 것이 일반적이다.

구조:

```text
Model
↓
Raw Logit
↓
BCEWithLogitsLoss
```

이다.

---

# 34. BCEWithLogitsLoss

```python
loss_fn = nn.BCEWithLogitsLoss()
```

이 Loss는 내부적으로 Sigmoid와 Binary Cross Entropy 계산을 안정적으로 결합한다.

따라서 Training 전에 직접:

```python
torch.sigmoid(
    logits
)
```

을 적용한 값을 Loss에 넣지 않는다.

---

# 35. 일반적인 Binary Training

```python
logits = model(
    x
)


loss = loss_fn(
    logits,
    y
)
```

이다.

---

# 36. Inference에서는

Probability가 필요하면:

```python
prob = torch.sigmoid(
    logits
)
```

을 적용한다.

---

# 37. 중요한 구분

Training:

```text
Model
↓
Logit
↓
BCEWithLogitsLoss
```

Prediction:

```text
Model
↓
Logit
↓
Sigmoid
↓
Probability
↓
Threshold
↓
Class
```

이다.

---

# 38. Sigmoid를 Hidden Layer에서 사용할 수도 있을까?

가능하다.

하지만 깊은 Neural Network의 Hidden Layer에서는 ReLU 계열을 더 흔히 볼 수 있다.

Sigmoid는 입력의 절댓값이 큰 영역에서 Gradient가 매우 작아질 수 있기 때문이다.

---

# 39. Vanishing Gradient

깊은 Network에서 Gradient가 이전 Layer로 전달되면서 매우 작아지는 현상을:

```text
Vanishing Gradient
```

라고 한다.

Sigmoid와 Tanh 같은 포화 Activation에서 관련 문제가 나타날 수 있다.

---

# 40. 왜 문제가 될까?

Gradient가 매우 작아지면 앞쪽 Layer의 Weight가 거의 업데이트되지 않을 수 있다.

```text
Backward
↓
Gradient
↓
점점 작아짐
↓
앞쪽 Layer 학습 느려짐
```

이다.

---

# 41. Tanh

Tanh는 입력을:

```text
-1 ~ 1
```

범위로 변환한다.

---

# 42. PyTorch Tanh

```python
tanh = nn.Tanh()
```

또는:

```python
y = torch.tanh(
    x
)
```

---

# 43. Sigmoid와 Tanh 비교

Sigmoid:

```text
0 ~ 1
```

Tanh:

```text
-1 ~ 1
```

이다.

---

# 44. Tanh의 특징

Tanh는 0을 중심으로 양수와 음수 값을 출력할 수 있다.

하지만 Tanh도 큰 절댓값 영역에서는 Gradient가 작아지는 문제가 있을 수 있다.

---

# 45. Softmax

Softmax는 Multi-class Classification에서 매우 중요하다.

여러 개의 Logit을:

```text
Probability Distribution
```

형태로 변환한다.

---

# 46. 예

Model Output:

```text
[2.0, 1.0, 0.1]
```

이 값은 아직 Probability가 아니다.

이것을:

```text
Logits
```

라고 한다.

---

# 47. Softmax 적용

```python
prob = torch.softmax(
    logits,
    dim=1
)
```

결과는 개념적으로:

```text
[0.66, 0.24, 0.10]
```

처럼 된다.

---

# 48. Softmax 특징

각 값은:

```text
0 ~ 1
```

사이에 있다.

그리고 Class Probability의 합은:

```text
1
```

이다.

---

# 49. 예

```text
Normal
0.10

Pneumonia
0.90
```

합:

```text
1.00
```

이다.

---

# 50. Multi-class Model

예:

```python
model = nn.Sequential(
    nn.Linear(
        128,
        64
    ),

    nn.ReLU(),

    nn.Linear(
        64,
        10
    )
)
```

마지막 Output:

```text
10 Logits
```

이다.

---

# 51. CrossEntropyLoss

Multi-class Classification에서는:

```python
loss_fn = nn.CrossEntropyLoss()
```

를 많이 사용한다.

---

# 52. 중요한 점

`CrossEntropyLoss`에는 일반적으로 Softmax를 적용하기 전의:

```text
Raw Logits
```

를 넣는다.

즉:

```python
logits = model(
    x
)


loss = loss_fn(
    logits,
    y
)
```

이다.

---

# 53. 잘못 이해하기 쉬운 부분

다음처럼 생각하기 쉽다.

```text
Model
↓
Softmax
↓
CrossEntropyLoss
```

하지만 PyTorch의 `CrossEntropyLoss`는 raw Logits를 받도록 설계되어 있다.

따라서 일반적인 Training에서는 Model 마지막에 Softmax를 직접 넣지 않는다.

---

# 54. Training

```text
Model
↓
Logits
↓
CrossEntropyLoss
```

이다.

---

# 55. Inference

Probability가 필요하면:

```text
Model
↓
Logits
↓
Softmax
↓
Probability
```

이다.

---

# 56. Prediction만 필요하면?

Softmax를 하지 않고:

```python
pred = logits.argmax(
    dim=1
)
```

만 해도 Class Prediction을 얻을 수 있다.

---

# 57. 왜 가능할까?

Softmax는 값의 상대적인 순서를 유지한다.

예:

```text
Logits

[1.0, 5.0, 2.0]
```

가장 큰 값:

```text
5.0
```

Softmax 후에도 해당 Class의 Probability가 가장 크다.

따라서 Class Index만 필요하다면 Logit에 바로 `argmax()`를 사용할 수 있다.

---

# 58. 예

```python
logits = model(
    x
)


pred = logits.argmax(
    dim=1
)
```

---

# 59. Probability가 필요하면

```python
prob = torch.softmax(
    logits,
    dim=1
)
```

을 사용한다.

---

# 60. dim=1은 왜?

Output Shape:

```text
[Batch, Classes]
```

예:

```text
[32, 10]
```

이다.

각 Sample마다 10개 Class를 기준으로 Softmax를 계산해야 한다.

따라서:

```python
dim=1
```

을 사용한다.

---

# 61. 예

```text
[32, 10]

32
→ Samples

10
→ Classes
```

각 행마다 10개의 Class Probability 합이 1이 되도록 계산한다.

---

# 62. Logit이란?

Neural Network의 마지막 Layer에서 나오는:

```text
Activation 적용 전 Raw Score
```

라고 생각하면 된다.

---

# 63. Binary 예

```text
Logit = 2.5
```

Sigmoid:

```text
Probability ≈ 0.924
```

이다.

---

# 64. Multi-class 예

```text
Logits
[1.2, -0.5, 3.1]
```

Softmax:

```text
Probabilities
[0.xx, 0.xx, 0.xx]
```

로 변환된다.

---

# 65. Logit과 Probability는 다르다

Logit:

```text
범위 제한 없음
```

Probability:

```text
0 ~ 1
```

이다.

---

# 66. Activation Function을 어디에 넣을까?

Hidden Layer와 Output Layer를 구분해야 한다.

Hidden Layer에서는:

```text
ReLU

LeakyReLU

GELU
```

등을 사용할 수 있다.

Output Layer는:

```text
문제 유형
+
Loss Function
```

에 따라 결정한다.

---

# 67. Multi-class Classification

일반적인 PyTorch 구성:

```text
Last Linear
↓
Raw Logits
↓
CrossEntropyLoss
```

Model 안에 Softmax:

```text
보통 넣지 않음
```

이다.

---

# 68. Binary Classification - 1 Logit

일반적인 구성:

```text
Last Linear
↓
1 Logit
↓
BCEWithLogitsLoss
```

Model 안에 Sigmoid:

```text
보통 넣지 않음
```

이다.

Inference에서 필요할 때 Sigmoid를 적용한다.

---

# 69. Regression

Regression에서는 마지막 Layer에 Activation을 넣지 않는 경우가 많다.

예:

```python
model = nn.Sequential(
    nn.Linear(
        20,
        64
    ),

    nn.ReLU(),

    nn.Linear(
        64,
        1
    )
)
```

---

# 70. 왜?

Regression Target이:

```text
음수

0

양수
```

등 넓은 범위를 가질 수 있기 때문이다.

마지막에 ReLU나 Sigmoid를 넣으면 Output 범위가 제한될 수 있다.

---

# 71. 하지만 항상 Activation이 없는 것은 아니다

Target의 의미상 Output 범위를 제한해야 하는 문제에서는 적절한 Output Activation을 사용할 수도 있다.

즉 문제 정의에 따라 결정한다.

---

# 72. 문제 유형별 기본 구조

```text
Multi-class Classification

Output:
n_classes Logits

Loss:
CrossEntropyLoss

Training Output Activation:
보통 없음
```

---

# 73. Binary Classification

```text
Output:
1 Logit

Loss:
BCEWithLogitsLoss

Training Output Activation:
보통 없음

Inference:
Sigmoid
```

---

# 74. Regression

```text
Output:
1 또는 여러 Numeric Values

Loss:
MSELoss / L1Loss 등

Output Activation:
문제에 따라 결정
```

---

# 75. 2 Logit Binary Classification

Binary 문제를:

```text
Class 0
Class 1
```

두 개의 Logit으로 만들 수도 있다.

이 경우:

```text
Output = 2

Loss = CrossEntropyLoss
```

를 사용할 수 있다.

---

# 76. 예

```python
model = nn.Sequential(
    nn.Linear(
        128,
        2
    )
)


loss_fn = nn.CrossEntropyLoss()
```

---

# 77. 이 경우 Probability

```python
prob = torch.softmax(
    logits,
    dim=1
)
```

으로 계산할 수 있다.

Class 1 Probability:

```python
prob_class1 = prob[
    :,
    1
]
```

이다.

---

# 78. 1 Logit vs 2 Logit

Binary Classification에서는 둘 다 가능하다.

방법 A:

```text
1 Logit
+
BCEWithLogitsLoss
+
Sigmoid
```

방법 B:

```text
2 Logits
+
CrossEntropyLoss
+
Softmax
```

이다.

---

# 79. 중요한 것은 일관성

다음 네 가지가 서로 맞아야 한다.

```text
Model Output Shape

Target Shape

Target dtype

Loss Function
```

---

# 80. 예: CrossEntropyLoss

일반적으로:

```text
Model Output
[32, 2]

Target
[32]

Target dtype
Long
```

이다.

---

# 81. 예: BCEWithLogitsLoss

구성에 따라:

```text
Model Output
[32, 1]

Target
[32, 1]

Target dtype
Float
```

처럼 맞출 수 있다.

---

# 82. 자주 발생하는 실수

```text
Output = [32, 1]

Target = [32]

Loss = BCEWithLogitsLoss
```

처럼 Shape가 서로 다르면 오류나 의도하지 않은 Broadcasting 문제가 발생할 수 있다.

필요하면 Shape를 명확히 맞춘다.

---

# 83. 예

```python
labels = labels.float().unsqueeze(
    1
)
```

하면:

```text
[32]

↓

[32, 1]
```

이 된다.

단, 실제 Model Output Shape에 맞춰야 한다.

---

# 84. 또는 Output을 펼칠 수 있다

```python
logits = model(
    x
).squeeze(
    1
)
```

하면:

```text
[32, 1]

↓

[32]
```

이 된다.

Target도 `[32]`이면 Shape를 맞출 수 있다.

---

# 85. squeeze 주의

Batch Size가 1일 수 있으므로:

```python
squeeze()
```

보다는:

```python
squeeze(
    1
)
```

처럼 제거할 Dimension을 명확히 지정하는 것이 안전한 경우가 많다.

---

# 86. Activation과 Gradient

Activation Function은 Forward에서 값만 바꾸는 것이 아니라 Backward에서 Gradient가 어떻게 전달되는지에도 영향을 준다.

그래서 Activation 선택은 Training에 영향을 준다.

---

# 87. ReLU Gradient

개념적으로:

```text
x > 0
→ Gradient 전달

x < 0
→ Gradient 0
```

이다.

0에서의 미분은 구현상 특정 값으로 처리된다.

---

# 88. Sigmoid Gradient

Sigmoid의 미분은:

```text
sigmoid(x) × (1 - sigmoid(x))
```

형태이다.

출력이 0이나 1에 매우 가까워지면 Gradient가 작아진다.

---

# 89. Tanh Gradient

Tanh도 출력이:

```text
-1

또는

1
```

근처에서 Gradient가 작아질 수 있다.

---

# 90. Saturation

Activation Output이 특정 범위 끝에 가까워져 Gradient가 매우 작아지는 현상을:

```text
Saturation
```

과 연결해서 이해할 수 있다.

---

# 91. Hidden Layer 기본 선택

처음 Model을 만들 때 Tabular나 기본 Fully Connected Network에서는:

```python
nn.ReLU()
```

를 기본적인 시작점으로 사용하기 좋다.

하지만 항상 최고의 Activation이라는 뜻은 아니다.

---

# 92. CNN에서도 ReLU

예:

```text
Conv2d
↓
ReLU
↓
Conv2d
↓
ReLU
```

처럼 CNN에서도 많이 사용된다.

---

# 93. Transformer에서는

다른 Architecture에서는:

```text
GELU
```

같은 Activation을 자주 볼 수 있다.

Architecture에 따라 일반적인 선택이 다를 수 있다.

---

# 94. Softmax를 Hidden Layer에 쓰나?

일반적인 Hidden Layer에서는 Softmax를 주 Activation으로 사용하는 경우가 흔하지 않다.

Softmax는 주로:

```text
여러 Score를 Probability Distribution으로 변환
```

해야 하는 곳에서 사용한다.

---

# 95. Sigmoid는 Multi-label에도 중요하다

Multi-label Classification은 Multi-class와 다르다.

예:

한 Image에:

```text
Pneumonia = Yes

Effusion = Yes

Nodule = No
```

처럼 여러 Label이 동시에 True일 수 있다.

---

# 96. Multi-class

Multi-class:

```text
여러 Class 중 하나 선택
```

예:

```text
Cat
Dog
Horse
```

중 하나이다.

---

# 97. Multi-label

Multi-label:

```text
여러 Label이 동시에 존재 가능
```

이다.

예:

```text
Finding A = 1
Finding B = 0
Finding C = 1
```

---

# 98. Multi-label Output

예:

```text
3 Labels

↓

3 Logits
```

이다.

각 Label을 독립적인 Binary Prediction처럼 처리할 수 있다.

---

# 99. Multi-label에서는

```python
loss_fn = nn.BCEWithLogitsLoss()
```

를 사용할 수 있다.

Inference:

```python
prob = torch.sigmoid(
    logits
)
```

이다.

---

# 100. Softmax를 쓰면 안 되는 이유

Multi-label에서는 여러 Label이 동시에 Positive일 수 있다.

Softmax는 Class들 사이의 Probability 합을 1로 만든다.

따라서 독립적인 여러 Label Probability가 필요한 Multi-label 문제와 목적이 다르다.

---

# 101. Multi-class vs Multi-label

```text
Multi-class

여러 Class 중 하나
→ Softmax 개념
→ CrossEntropyLoss
```

```text
Multi-label

각 Label이 독립적으로 Yes / No
→ Sigmoid 개념
→ BCEWithLogitsLoss
```

이다.

---

# 102. Activation 선택 전에 문제부터 정의

먼저 질문한다.

```text
Regression인가?

Binary Classification인가?

Multi-class Classification인가?

Multi-label Classification인가?
```

그 다음:

```text
Output Layer

Loss Function

Activation
```

을 결정한다.

---

# 103. Activation과 Loss를 따로 외우지 않는다

다음처럼 세트로 이해한다.

```text
문제 유형
↓
Output Shape
↓
Loss Function
↓
필요한 Probability 변환
```

---

# 104. Multi-class 예

```text
문제:
10개 숫자 분류

Output:
[Batch, 10]

Training:
Raw Logits

Loss:
CrossEntropyLoss

Prediction:
argmax

Probability:
Softmax
```

---

# 105. Binary 1-Logit 예

```text
문제:
Normal / Pneumonia

Output:
[Batch, 1]

Training:
Raw Logit

Loss:
BCEWithLogitsLoss

Probability:
Sigmoid

Prediction:
Threshold
```

---

# 106. Regression 예

```text
문제:
가격 예측

Output:
[Batch, 1]

Loss:
MSELoss 등

Output Activation:
문제에 따라 결정
```

---

# 107. Model 예제 1

Multi-class:

```python
model = nn.Sequential(
    nn.Linear(
        100,
        64
    ),

    nn.ReLU(),

    nn.Linear(
        64,
        10
    )
)


loss_fn = nn.CrossEntropyLoss()
```

---

# 108. Training

```python
logits = model(
    x
)


loss = loss_fn(
    logits,
    y
)
```

---

# 109. Prediction

```python
pred = logits.argmax(
    dim=1
)
```

---

# 110. Probability

```python
prob = torch.softmax(
    logits,
    dim=1
)
```

---

# 111. Model 예제 2

Binary:

```python
model = nn.Sequential(
    nn.Linear(
        100,
        64
    ),

    nn.ReLU(),

    nn.Linear(
        64,
        1
    )
)


loss_fn = nn.BCEWithLogitsLoss()
```

---

# 112. Training

```python
logits = model(
    x
)


loss = loss_fn(
    logits,
    y
)
```

Target Shape과 dtype은 Output에 맞게 준비한다.

---

# 113. Binary Probability

```python
prob = torch.sigmoid(
    logits
)
```

---

# 114. Binary Prediction

```python
pred = (
    prob >= 0.5
).long()
```

---

# 115. 0.5는 절대적인 값이 아니다

Binary Classification의 Threshold:

```text
0.5
```

는 기본적인 시작점일 뿐이다.

문제에 따라:

```text
0.3

0.4

0.6
```

등을 Validation Data에서 비교할 수 있다.

---

# 116. Threshold를 낮추면

일반적으로 Positive로 Prediction하기 쉬워진다.

따라서:

```text
Recall 증가 가능

False Positive 증가 가능
```

등의 Trade-off가 생긴다.

---

# 117. Threshold를 높이면

Positive로 Prediction하기 어려워진다.

따라서 일반적으로:

```text
False Positive 감소 가능

Recall 감소 가능
```

등의 변화가 생길 수 있다.

---

# 118. Threshold는 Test로 선택하지 않는다

Threshold는:

```text
Validation

또는

OOF Prediction
```

등을 이용해 선택한다.

최종 Test를 반복해서 보며 Threshold를 고르면 Test Leakage와 비슷한 평가 왜곡이 생길 수 있다.

---

# 119. Activation Output 확인

간단한 실습:

```python
x = torch.linspace(
    -5,
    5,
    steps=11
)


print(
    x
)


print(
    torch.relu(
        x
    )
)


print(
    torch.sigmoid(
        x
    )
)


print(
    torch.tanh(
        x
    )
)
```

각 Activation이 값을 어떻게 바꾸는지 직접 확인할 수 있다.

---

# 120. Softmax 실습

```python
logits = torch.tensor(
    [
        [1.0, 2.0, 3.0]
    ]
)


prob = torch.softmax(
    logits,
    dim=1
)


print(
    prob
)


print(
    prob.sum(
        dim=1
    )
)
```

Probability 합:

```text
1
```

을 확인할 수 있다.

---

# 121. argmax 실습

```python
pred = logits.argmax(
    dim=1
)


print(
    pred
)
```

가장 큰 Logit의 Index가 Prediction이 된다.

---

# 122. ReLU 앞뒤 Shape 확인

```python
x = torch.randn(
    32,
    64
)


relu = nn.ReLU()


y = relu(
    x
)


print(
    x.shape
)


print(
    y.shape
)
```

둘 다:

```text
[32, 64]
```

이다.

---

# 123. Activation Debugging

Model이 이상할 때 Activation도 확인할 수 있다.

질문:

```text
마지막 Layer에 잘못된 Activation을 넣었는가?

CrossEntropyLoss 전에 Softmax를 넣었는가?

BCEWithLogitsLoss 전에 Sigmoid를 넣었는가?

Regression Output을 ReLU로 제한했는가?

Output Shape와 Loss가 맞는가?
```

---

# 124. CrossEntropyLoss와 Softmax 실수

피해야 할 일반적인 패턴:

```python
model = nn.Sequential(
    ...,
    nn.Linear(
        64,
        10
    ),
    nn.Softmax(
        dim=1
    )
)


loss_fn = nn.CrossEntropyLoss()
```

`CrossEntropyLoss`는 raw Logits를 기대하므로 일반적으로 마지막 Softmax를 제거한다.

---

# 125. BCEWithLogitsLoss와 Sigmoid 실수

피해야 할 일반적인 패턴:

```python
model = nn.Sequential(
    ...,
    nn.Linear(
        64,
        1
    ),
    nn.Sigmoid()
)


loss_fn = nn.BCEWithLogitsLoss()
```

`BCEWithLogitsLoss`는 Logit을 기대하므로 일반적으로 Model 마지막 Sigmoid를 제거한다.

---

# 126. 올바른 Multi-class 예

```python
model = nn.Sequential(
    nn.Linear(
        100,
        64
    ),
    nn.ReLU(),
    nn.Linear(
        64,
        10
    )
)


loss_fn = nn.CrossEntropyLoss()
```

---

# 127. 올바른 1-Logit Binary 예

```python
model = nn.Sequential(
    nn.Linear(
        100,
        64
    ),
    nn.ReLU(),
    nn.Linear(
        64,
        1
    )
)


loss_fn = nn.BCEWithLogitsLoss()
```

---

# 128. 왜 WithLogits라는 이름일까?

```text
BCEWithLogitsLoss
```

의 이름처럼:

```text
Logits
+
Binary Cross Entropy
```

를 함께 처리한다.

그래서 raw Logit을 입력한다.

---

# 129. CrossEntropyLoss도 비슷한 관점

Multi-class Classification에서 `CrossEntropyLoss`는 raw Logits를 입력으로 사용하여 필요한 계산을 내부적으로 안정적으로 처리한다.

따라서 Training Model Output은 raw Logits로 두는 것이 일반적이다.

---

# 130. Numerical Stability

왜 Loss 안에서 처리할까?

Sigmoid나 Softmax를 먼저 계산한 뒤 Log를 취하는 식으로 직접 구현하면 매우 크거나 작은 값에서 수치적으로 불안정할 수 있다.

PyTorch Loss는 이를 더 안정적인 방식으로 계산하도록 구현되어 있다.

---

# 131. 따라서

```text
Training
→ Loss가 기대하는 Raw Output 사용

Inference
→ 필요할 때 Probability 변환
```

이라고 기억하면 좋다.

---

# 132. Activation Function 선택 요약

Hidden Layer:

```text
ReLU
LeakyReLU
GELU
등
```

Output:

```text
문제 유형과 Loss에 따라 결정
```

이다.

---

# 133. 가장 중요한 조합 1

Multi-class Classification:

```text
Last Layer
→ Linear(..., n_classes)

Training
→ Raw Logits

Loss
→ CrossEntropyLoss

Prediction
→ argmax

Probability
→ Softmax
```

---

# 134. 가장 중요한 조합 2

Binary Classification:

```text
Last Layer
→ Linear(..., 1)

Training
→ Raw Logit

Loss
→ BCEWithLogitsLoss

Probability
→ Sigmoid

Prediction
→ Threshold
```

---

# 135. 가장 중요한 조합 3

Regression:

```text
Last Layer
→ Linear(..., output_size)

Loss
→ MSELoss / L1Loss 등

Output Activation
→ 문제의 Target 범위에 따라 결정
```

---

# 136. 가장 중요한 조합 4

Multi-label Classification:

```text
Last Layer
→ Linear(..., n_labels)

Training
→ Raw Logits

Loss
→ BCEWithLogitsLoss

Probability
→ Sigmoid

Prediction
→ Label별 Threshold
```

---

# 137. Activation Function 비교

| Activation | 출력 범위 | 대표적 사용 |
|---|---|---|
| ReLU | 0 이상 | Hidden Layer |
| LeakyReLU | 음수 일부 허용 | Hidden Layer |
| Sigmoid | 0 ~ 1 | Binary Probability |
| Tanh | -1 ~ 1 | 특정 Hidden 구조 |
| Softmax | 합이 1인 확률 | Multi-class Probability |
| GELU | 비선형 | Transformer 등 |

---

# 138. Activation을 볼 때 질문

```text
이 Activation은 Hidden Layer용인가?

Output 변환용인가?

어떤 Loss를 사용하는가?

Model이 Logit을 출력해야 하는가?

Probability가 필요한 시점인가?
```

를 확인한다.

---

# 139. 전체 구조

```text
Input
↓
Linear
↓
ReLU
↓
Linear
↓
ReLU
↓
Output Linear
↓
Logits
```

여기까지가 Model일 수 있다.

그 다음 Training:

```text
Logits
↓
Loss
```

Inference:

```text
Logits
↓
Sigmoid / Softmax
↓
Probability
```

이다.

---

# 140. Neural Network와 연결

앞 파일에서:

```text
Linear
→ Shape 변경

Weight / Bias
→ 학습 Parameter
```

를 배웠다.

이번에는:

```text
Activation
→ 비선형성 추가
```

를 배운 것이다.

따라서:

```text
Linear
+
Activation
```

이 Neural Network의 핵심적인 기본 Block이 된다.

---

# 141. 다음 단계

이제 Model은:

```text
Input
↓
Linear
↓
Activation
↓
Output
```

을 만들 수 있다.

하지만 Model이 얼마나 틀렸는지 알아야 한다.

다음에 필요한 것이:

```text
Loss Function
```

이다.

---

# 142. 최종 정리

Activation Function은 Neural Network에 비선형성을 추가한다.

Activation이 없이 Linear Layer만 여러 개 연결하면:

```text
Linear
↓
Linear
↓
Linear
```

이 결국 하나의 Linear Transformation처럼 합쳐질 수 있다.

그래서:

```text
Linear
↓
ReLU
↓
Linear
```

처럼 비선형 Activation을 사용한다.

Hidden Layer에서는:

```text
ReLU
LeakyReLU
GELU
```

등을 볼 수 있다.

Multi-class Classification에서는:

```text
Raw Logits
↓
CrossEntropyLoss
```

를 Training에 사용하고 Probability가 필요할 때:

```text
Softmax
```

를 적용한다.

Binary Classification에서는:

```text
Raw Logit
↓
BCEWithLogitsLoss
```

를 Training에 사용하고 Probability가 필요할 때:

```text
Sigmoid
```

를 적용할 수 있다.

가장 중요한 것은 Activation Function을 혼자 외우는 것이 아니라:

```text
문제 유형
↓
Model Output
↓
Loss Function
↓
Probability 변환
↓
Prediction 방식
```

을 하나의 흐름으로 이해하는 것이다.