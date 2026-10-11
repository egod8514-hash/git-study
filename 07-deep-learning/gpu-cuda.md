# GPU & CUDA

## 1. 왜 Deep Learning에서 GPU를 사용할까?

Deep Learning에서는 매우 많은 수치 계산이 반복된다.

예:

```text
Image
↓
Convolution
↓
Matrix Calculation
↓
Activation
↓
다음 Layer
↓
...
↓
Loss
↓
Backward
```

이 과정에서 수많은 곱셈과 덧셈이 수행된다.

특히 Image Dataset과 큰 Neural Network에서는 계산량이 매우 많다.

그래서 GPU를 많이 사용한다.

---

# 2. CPU란?

CPU:

```text
Central Processing Unit
```

컴퓨터의 범용적인 계산을 담당한다.

예:

```text
프로그램 실행

운영체제 처리

웹 브라우저

파일 작업

일반 계산
```

등 다양한 작업을 수행한다.

---

# 3. GPU란?

GPU:

```text
Graphics Processing Unit
```

원래 그래픽 연산을 위해 발전했지만 많은 계산을 병렬로 처리하는 데 강점이 있다.

Deep Learning에서는 대규모 Tensor와 Matrix 연산을 많이 수행하기 때문에 GPU를 활용할 수 있다.

---

# 4. CPU vs GPU

단순하게 이해하면:

```text
CPU
→ 다양한 작업을 빠르게 처리하는 범용 Processor

GPU
→ 많은 비슷한 계산을 병렬로 처리하는 데 강점
```

이다.

---

# 5. GPU가 항상 빠른가?

아니다.

작은 계산에서는:

```text
CPU → GPU Data 이동
```

같은 추가 비용 때문에 GPU의 장점이 크지 않을 수 있다.

GPU가 특히 유리한 경우:

```text
큰 Neural Network

많은 Training Data

Image / Video

큰 Batch

대규모 Tensor 연산
```

등이다.

---

# 6. Deep Learning과 Matrix

Neural Network의 Linear Layer도 Matrix 계산을 사용한다.

예:

```text
Y = XWᵀ + b
```

이다.

CNN도 내부적으로 많은 Tensor 연산을 수행한다.

그래서 GPU가 Deep Learning과 잘 맞는다.

---

# 7. NVIDIA GPU

PyTorch에서 GPU 학습을 할 때 자주 접하는 것이:

```text
NVIDIA GPU
```

이다.

그리고 NVIDIA GPU 계산 플랫폼과 관련하여:

```text
CUDA
```

라는 용어를 만나게 된다.

---

# 8. CUDA란?

CUDA:

```text
Compute Unified Device Architecture
```

NVIDIA가 제공하는 GPU 병렬 컴퓨팅 플랫폼 및 프로그래밍 모델이다.

쉽게 생각하면:

```text
PyTorch가 NVIDIA GPU를 이용해
계산할 수 있도록 연결되는 핵심 기술
```

과 관련된다.

---

# 9. 관계

전체적으로:

```text
PyTorch
↓
CUDA 지원
↓
NVIDIA GPU
↓
Deep Learning 계산
```

이라고 이해할 수 있다.

---

# 10. CUDA = GPU인가?

아니다.

```text
GPU
→ 실제 Hardware

CUDA
→ NVIDIA GPU를 계산에 활용하기 위한 Platform / Programming Model
```

이다.

둘은 같은 것이 아니다.

---

# 11. PyTorch가 CUDA를 지원하면

예:

```python
torch.cuda.is_available()
```

를 통해 현재 PyTorch 환경에서 CUDA GPU를 사용할 수 있는지 확인할 수 있다.

---

# 12. 가장 먼저 확인

```python
import torch


print(
    torch.cuda.is_available()
)
```

결과:

```text
True
```

이면 현재 PyTorch에서 CUDA를 사용할 수 있다는 의미이다.

---

# 13. False라면?

```text
False
```

가 나왔다고 반드시 GPU Hardware가 없다는 뜻은 아니다.

가능한 원인:

```text
NVIDIA GPU가 없음

GPU Driver 문제

CUDA를 지원하지 않는 PyTorch 설치

환경 문제

잘못된 Python Environment

GPU가 현재 Runtime에 연결되지 않음
```

등이 있다.

---

# 14. GPU 이름 확인

```python
if torch.cuda.is_available():

    print(
        torch.cuda.get_device_name(
            0
        )
    )
```

예:

```text
NVIDIA GeForce ...
```

처럼 표시될 수 있다.

---

# 15. GPU 개수

```python
print(
    torch.cuda.device_count()
)
```

예:

```text
1
```

이면 PyTorch가 사용할 수 있는 CUDA Device를 하나 인식하고 있다는 뜻이다.

---

# 16. 현재 GPU 번호

```python
print(
    torch.cuda.current_device()
)
```

일반적으로 첫 GPU:

```text
0
```

부터 시작한다.

---

# 17. Device란?

PyTorch에서 Tensor와 Model이 어디에서 계산될지를 나타내는 개념이다.

대표적으로:

```text
cpu

cuda
```

가 있다.

---

# 18. CPU Device

```python
device = torch.device(
    "cpu"
)
```

---

# 19. CUDA Device

```python
device = torch.device(
    "cuda"
)
```

---

# 20. 자동 선택

가장 자주 사용하는 형태:

```python
device = torch.device(
    "cuda"
    if torch.cuda.is_available()
    else "cpu"
)
```

---

# 21. 해석

```text
CUDA 사용 가능?

YES
→ cuda

NO
→ cpu
```

이다.

---

# 22. 출력

```python
print(
    device
)
```

결과:

```text
cuda
```

또는:

```text
cpu
```

이다.

---

# 23. 더 간단하게 작성

```python
device = (
    "cuda"
    if torch.cuda.is_available()
    else "cpu"
)
```

처럼 문자열로 사용하는 코드도 볼 수 있다.

---

# 24. GPU 번호 지정

GPU가 여러 개 있다면:

```python
device = torch.device(
    "cuda:0"
)
```

처럼 지정할 수 있다.

---

# 25. 두 번째 GPU

```python
device = torch.device(
    "cuda:1"
)
```

이다.

단 실제로 해당 Device가 존재해야 한다.

---

# 26. Tensor는 처음 어디에 만들어질까?

기본적으로:

```python
x = torch.tensor(
    [
        1,
        2,
        3
    ]
)
```

처럼 만들면 일반적으로 CPU Tensor이다.

---

# 27. 확인

```python
print(
    x.device
)
```

결과:

```text
cpu
```

---

# 28. Tensor를 GPU로 이동

```python
x = x.to(
    device
)
```

---

# 29. 확인

```python
print(
    x.device
)
```

CUDA를 사용한다면:

```text
cuda:0
```

처럼 나타날 수 있다.

---

# 30. `.to(device)`가 중요한 이유

다음 코드:

```python
x.to(
    device
)
```

만 작성하고 결과를 다시 저장하지 않으면 주의해야 한다.

일반적으로:

```python
x = x.to(
    device
)
```

처럼 반환된 Tensor를 사용한다.

---

# 31. Model도 GPU로 이동

```python
model = model.to(
    device
)
```

---

# 32. Model Device 확인

```python
print(
    next(
        model.parameters()
    ).device
)
```

---

# 33. 가장 중요한 규칙

Model과 Input Tensor는 같은 Device에 있어야 한다.

예:

```text
Model
→ cuda

Input
→ cuda
```

이다.

---

# 34. 잘못된 예

```text
Model
→ cuda

Input
→ cpu
```

이면 오류가 발생할 수 있다.

---

# 35. 자주 보는 Device 오류

예:

```text
Expected all tensors to be on the same device
```

비슷한 오류가 나오면 가장 먼저:

```text
Model Device

Input Device

Target Device
```

를 확인한다.

---

# 36. 확인 코드

```python
print(
    "Model:",
    next(
        model.parameters()
    ).device
)

print(
    "Input:",
    x.device
)

print(
    "Target:",
    y.device
)
```

---

# 37. Training Loop에서 Device

```python
for x, y in train_loader:

    x = x.to(
        device
    )

    y = y.to(
        device
    )

    optimizer.zero_grad()

    outputs = model(
        x
    )

    loss = loss_fn(
        outputs,
        y
    )

    loss.backward()

    optimizer.step()
```

---

# 38. 왜 DataLoader에서 바로 GPU Data를 주지 않을까?

일반적인 Dataset/DataLoader는 CPU에서 데이터를 읽고 준비한 뒤 Training Loop에서 필요한 Batch를 GPU로 이동시키는 구조를 많이 사용한다.

```text
Storage
↓
CPU Memory
↓
DataLoader
↓
Batch
↓
GPU Memory
↓
Model
```

이다.

---

# 39. GPU Memory

GPU에는 GPU가 계산에 사용하는 Memory가 있다.

흔히:

```text
VRAM
```

이라고 부른다.

---

# 40. VRAM에 들어가는 것

Training 중 대표적으로:

```text
Model Parameters

Input Batch

Activation

Gradient

Optimizer State
```

등이 GPU Memory를 사용한다.

---

# 41. Training이 Inference보다 Memory를 많이 사용할 수 있는 이유

Training에서는:

```text
Forward
+
Backward
+
Gradient
+
Optimizer State
```

가 필요하다.

Inference에서는 일반적으로 Gradient 계산이 필요 없다.

그래서 Training이 더 많은 Memory를 사용할 수 있다.

---

# 42. CUDA Out Of Memory

자주 보는 오류:

```text
CUDA out of memory
```

이다.

줄여서:

```text
OOM
```

이라고도 한다.

---

# 43. OOM 의미

필요한 GPU Memory가 현재 사용 가능한 GPU Memory보다 많다는 의미이다.

---

# 44. OOM이 발생하면 가장 먼저

```text
Batch Size
```

를 줄여본다.

예:

```python
batch_size = 64
```

에서:

```python
batch_size = 32
```

또는:

```python
batch_size = 16
```

으로 줄인다.

---

# 45. 왜 Batch Size를 줄이면 될까?

한 번에 GPU로 보내는 Sample 수가 감소한다.

예:

```text
64 Images
↓
32 Images
```

따라서 Batch 관련 Memory 사용량이 감소한다.

---

# 46. Image Size도 중요

예:

```text
224 × 224
```

보다:

```text
1024 × 1024
```

Image가 훨씬 많은 Memory와 계산량을 필요로 한다.

---

# 47. Channel도 영향

```text
Grayscale
[B, 1, H, W]
```

와:

```text
RGB
[B, 3, H, W]
```

는 Input 크기가 다르다.

---

# 48. Model Size도 영향

큰 Model일수록:

```text
Parameter

Activation

Gradient
```

등이 더 많은 Memory를 사용할 수 있다.

---

# 49. OOM 대응 방법

대표적으로:

```text
Batch Size 감소

Image Size 감소

더 작은 Model 사용

불필요한 Tensor 저장 제거

Gradient Accumulation

Mixed Precision

Gradient가 필요 없는 곳에서 no_grad 사용
```

등을 고려할 수 있다.

---

# 50. torch.no_grad()

Validation:

```python
with torch.no_grad():

    outputs = model(
        x
    )
```

처럼 사용한다.

Gradient 계산을 하지 않으므로 평가 시 Memory 사용량과 계산 비용을 줄이는 데 도움이 된다.

---

# 51. inference_mode

Inference 전용 코드에서는:

```python
with torch.inference_mode():

    outputs = model(
        x
    )
```

도 사용할 수 있다.

---

# 52. no_grad와 inference_mode

둘 다 Gradient 계산이 필요 없는 평가/Inference에서 볼 수 있다.

`inference_mode()`는 Inference에 더 특화된 방식이다.

처음에는:

```text
Training
→ Gradient 필요

Validation / Inference
→ Gradient 불필요
```

를 확실히 이해한다.

---

# 53. GPU Memory 확인

PyTorch:

```python
print(
    torch.cuda.memory_allocated()
)
```

현재 PyTorch가 할당한 GPU Memory를 Byte 단위로 확인할 수 있다.

---

# 54. MB로 보기

```python
memory_mb = (
    torch.cuda.memory_allocated()
    /
    1024 ** 2
)


print(
    memory_mb
)
```

---

# 55. 예약된 Memory

```python
print(
    torch.cuda.memory_reserved()
)
```

도 확인할 수 있다.

---

# 56. allocated와 reserved

간단하게:

```text
allocated
→ Tensor 등에 실제 사용 중인 Memory

reserved
→ PyTorch Memory Allocator가 확보해 둔 Memory
```

로 이해할 수 있다.

---

# 57. nvidia-smi

NVIDIA GPU 상태를 확인할 때 자주 사용하는 명령:

```powershell
nvidia-smi
```

이다.

Windows PowerShell이나 Terminal에서 실행할 수 있다.

---

# 58. nvidia-smi에서 볼 수 있는 것

환경에 따라:

```text
GPU 이름

Driver Version

Memory Usage

GPU Utilization

실행 중 Process
```

등을 확인할 수 있다.

---

# 59. 예

```text
GPU Memory

3000 MiB / 12000 MiB
```

라면:

```text
약 12GB 중
약 3GB 사용
```

처럼 이해할 수 있다.

---

# 60. GPU Utilization

예:

```text
GPU-Util
95%
```

이면 해당 시점에 GPU 연산이 활발하게 수행되고 있을 가능성이 있다.

---

# 61. GPU가 있는데 Utilization이 낮다면?

무조건 문제가 있다는 뜻은 아니다.

가능한 이유:

```text
Data Loading이 느림

CPU Preprocessing이 병목

Batch Size가 매우 작음

Model이 작음

GPU와 CPU 사이 Data 이동 대기

현재 Validation / 대기 상태
```

등이 있다.

---

# 62. GPU가 빠르다고 전체 Training이 무조건 빠른 것은 아니다

전체 Pipeline:

```text
Disk
↓
CPU
↓
Dataset
↓
DataLoader
↓
GPU
↓
Model
```

중 하나가 느리면 GPU가 기다릴 수 있다.

이를:

```text
Bottleneck
```

이라고 생각할 수 있다.

---

# 63. num_workers

DataLoader:

```python
train_loader = DataLoader(
    train_dataset,
    batch_size=32,
    shuffle=True,
    num_workers=4
)
```

에서 `num_workers`는 Data Loading 작업을 병렬화하는 데 관련된다.

---

# 64. Windows에서는 주의

Windows나 Notebook 환경에서는 `num_workers` 설정에 따라 문제가 발생할 수 있다.

Debugging 단계에서는:

```python
num_workers=0
```

으로 시작하는 것이 단순하다.

---

# 65. 먼저 정상 작동 확인

```text
num_workers = 0
↓
Training 정상 확인
↓
필요하면 Worker 수 조정
```

순서로 접근할 수 있다.

---

# 66. pin_memory

CUDA Training에서:

```python
DataLoader(
    ...,
    pin_memory=True
)
```

옵션을 볼 수 있다.

CPU에서 GPU로 Data를 전송하는 과정의 효율과 관련된다.

---

# 67. 예

```python
train_loader = DataLoader(
    train_dataset,
    batch_size=32,
    shuffle=True,
    num_workers=0,
    pin_memory=True
)
```

---

# 68. 반드시 True여야 하나?

아니다.

환경과 Pipeline에 따라 효과가 달라질 수 있다.

처음에는 기본 설정으로 정상 Training을 만드는 것이 우선이다.

---

# 69. non_blocking

다음과 같은 코드도 볼 수 있다.

```python
x = x.to(
    device,
    non_blocking=True
)
```

`pin_memory` 등과 함께 비동기 Data Transfer 최적화에 활용될 수 있다.

초보 단계에서는 필수 사항은 아니다.

---

# 70. 먼저 정확성

Optimization 순서:

```text
1. 코드가 정상 작동

2. Model이 학습됨

3. Validation 정상

4. 그 다음 속도 최적화
```

가 좋다.

---

# 71. CUDA Version 확인

PyTorch에서:

```python
print(
    torch.version.cuda
)
```

를 사용할 수 있다.

---

# 72. PyTorch Version

```python
print(
    torch.__version__
)
```

---

# 73. cuDNN

```python
print(
    torch.backends.cudnn.version()
)
```

처럼 확인할 수 있다.

---

# 74. cuDNN이란?

NVIDIA GPU에서 Deep Neural Network 연산을 가속하기 위한 Library이다.

CNN 연산 등에서 PyTorch가 활용할 수 있다.

---

# 75. CUDA Toolkit과 PyTorch CUDA Runtime

CUDA 관련 Version을 볼 때 혼동하기 쉽다.

예:

```text
nvidia-smi에 표시되는 CUDA Version

nvcc Version

torch.version.cuda
```

가 항상 똑같이 보이지 않을 수 있다.

---

# 76. 왜 다를 수 있을까?

각 값이 나타내는 대상이 완전히 같지 않기 때문이다.

따라서:

```text
숫자가 다르다
=
무조건 설치 오류
```

라고 판단하면 안 된다.

---

# 77. 가장 중요한 확인

PyTorch 학습 목적이라면 우선:

```python
torch.cuda.is_available()
```

이 `True`인지 확인한다.

그리고:

```python
torch.cuda.get_device_name(
    0
)
```

으로 GPU가 인식되는지 본다.

---

# 78. CUDA가 False일 때 확인 순서

```text
1. NVIDIA GPU가 있는가?

2. nvidia-smi가 작동하는가?

3. NVIDIA Driver가 정상인가?

4. 현재 Python Environment가 맞는가?

5. PyTorch 설치가 CUDA 지원 Build인가?

6. Jupyter Kernel이 같은 Environment인가?
```

순서로 확인할 수 있다.

---

# 79. Environment가 중요한 이유

예:

```text
base Environment
→ CPU PyTorch

ml_env
→ CUDA PyTorch
```

처럼 서로 다른 Package가 설치되어 있을 수 있다.

---

# 80. Python 위치 확인

Windows:

```powershell
where python
```

---

# 81. Python Version

```powershell
python --version
```

---

# 82. Notebook 내부

```python
import sys


print(
    sys.executable
)
```

을 사용하면 현재 Notebook이 어떤 Python Interpreter를 사용하고 있는지 확인할 수 있다.

---

# 83. 중요한 이유

Terminal에서 설치한 PyTorch와 Notebook에서 실행하는 Python Environment가 다르면:

```text
Terminal에서는 GPU 가능

Notebook에서는 GPU 불가능
```

같은 상황이 생길 수 있다.

---

# 84. Jupyter Kernel

Jupyter Notebook은 선택된 Kernel의 Python Environment를 사용한다.

따라서:

```text
Conda Environment
```

와:

```text
Jupyter Kernel
```

이 일치하는지 확인하는 것이 중요하다.

---

# 85. Colab GPU

Google Colab에서는 Cloud 환경의 GPU를 사용할 수 있다.

즉:

```text
내 PC GPU
```

를 자동으로 사용하는 것이 아니다.

---

# 86. Colab 기본 구조

```text
내 Browser
↓
Internet
↓
Google Colab Runtime
↓
Cloud CPU / GPU
```

이다.

---

# 87. Colab에서 GPU를 선택하면

Notebook Code가 Google이 제공하는 Runtime의 GPU에서 실행된다.

---

# 88. 로컬 GPU와 Colab GPU 차이

로컬:

```text
내 Computer
↓
내 NVIDIA GPU
```

Colab:

```text
내 Browser
↓
Google Server
↓
Colab GPU
```

이다.

---

# 89. 따라서 Colab에서

```python
torch.cuda.get_device_name(
    0
)
```

을 실행하면 내 PC의 GPU 이름이 아니라 현재 Colab Runtime에 할당된 GPU 이름이 나온다.

---

# 90. Colab에서 로컬 GPU를 자동으로 쓰는 것이 아니다

Browser에서 Colab을 열었다고:

```text
Colab
→ 내 PC GPU
```

가 되는 것은 아니다.

기본적으로 Colab Runtime은 Cloud에서 실행된다.

---

# 91. 로컬 GPU Training

로컬에서 PyTorch를 실행하려면:

```text
Local Python

PyTorch

NVIDIA Driver

CUDA 지원 환경

GPU
```

가 올바르게 연결되어야 한다.

---

# 92. VS Code에서 로컬 GPU

VS Code Terminal이나 Notebook에서 현재 Local Python Environment가 CUDA PyTorch를 사용하고 있다면 Local NVIDIA GPU를 이용할 수 있다.

확인:

```python
import torch


print(
    torch.cuda.is_available()
)


print(
    torch.cuda.get_device_name(
        0
    )
)
```

---

# 93. Local과 Colab의 파일도 다르다

Colab:

```text
/content/...
```

로컬 Windows:

```text
C:\Users\...
```

이다.

---

# 94. 그래서 FileNotFoundError가 날 수 있다

Colab Code:

```python
pd.read_csv(
    "/content/train.csv"
)
```

를 그대로 Windows Local에서 실행하면 해당 경로가 존재하지 않을 수 있다.

---

# 95. Device와 Path는 다른 문제

구분한다.

```text
CUDA Error
→ 계산 Device 문제

FileNotFoundError
→ File Path 문제
```

이다.

GPU를 바꾼다고 File Path 문제가 해결되는 것은 아니다.

---

# 96. Mixed Precision

GPU Training을 최적화할 때:

```text
Mixed Precision
```

을 사용할 수 있다.

---

# 97. 기본 개념

모든 계산을 항상 같은 정밀도로 수행하는 대신 GPU가 효율적으로 처리할 수 있는 낮은 정밀도 연산 등을 적절히 활용한다.

목적:

```text
Training 속도 향상 가능

GPU Memory 절약 가능
```

이다.

---

# 98. PyTorch AMP

AMP:

```text
Automatic Mixed Precision
```

이다.

---

# 99. 예시 개념

최근 PyTorch 코드에서는 환경과 버전에 맞는 AMP API를 사용한다.

개념적인 Training 구조:

```text
Forward
→ Autocast

Loss
→ Scale

Backward

Optimizer Step

Scaler Update
```

이다.

---

# 100. AMP는 먼저 기본 Training 후

처음부터 AMP까지 모두 넣기보다:

```text
기본 FP Training 정상 작동
↓
Validation 정상
↓
그 다음 AMP
```

순서로 적용하면 Debugging이 쉽다.

---

# 101. Gradient Accumulation

GPU Memory가 부족한 경우 여러 작은 Batch의 Gradient를 누적해 큰 Batch와 비슷한 효과를 노릴 수 있다.

예:

```text
Batch Size = 16

4번 누적

↓

Effective Batch Size ≈ 64
```

처럼 생각할 수 있다.

---

# 102. 주의

Gradient Accumulation은 실제로 64개 Image를 한 번에 GPU에 올리는 것은 아니다.

작은 Batch를 순차적으로 처리하면서 Gradient를 누적한다.

---

# 103. GPU 사용 여부가 Model 정확도를 자동으로 높이나?

아니다.

GPU는 주로:

```text
계산 속도
```

와 관련된다.

같은 Algorithm과 조건이라면 GPU를 사용한다고 자동으로 더 정확한 Model이 되는 것은 아니다.

---

# 104. GPU가 하는 일

```text
더 빠르게 계산할 수 있도록 도움
```

을 주는 것이다.

Model 성능은:

```text
Data

Model Architecture

Training Strategy

Loss

Optimizer

Hyperparameter

Validation Strategy
```

등에 영향을 받는다.

---

# 105. GPU가 빠르면 좋은 점

같은 시간에:

```text
더 많은 Experiment

더 많은 Epoch

더 큰 Model

더 큰 Dataset
```

을 시도할 수 있다.

따라서 간접적으로 연구와 실험 효율이 높아질 수 있다.

---

# 106. GPU 사용 확인 전체 코드

```python
import torch


print(
    "PyTorch:",
    torch.__version__
)


print(
    "CUDA Available:",
    torch.cuda.is_available()
)


print(
    "PyTorch CUDA:",
    torch.version.cuda
)


if torch.cuda.is_available():

    print(
        "GPU Count:",
        torch.cuda.device_count()
    )

    print(
        "GPU Name:",
        torch.cuda.get_device_name(
            0
        )
    )
```

---

# 107. Device 설정

```python
device = torch.device(
    "cuda"
    if torch.cuda.is_available()
    else "cpu"
)


print(
    "Using:",
    device
)
```

---

# 108. Model 생성

```python
import torch.nn as nn


model = nn.Sequential(
    nn.Linear(
        10,
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

# 109. Model 이동

```python
model = model.to(
    device
)
```

---

# 110. Dummy Tensor

```python
x = torch.randn(
    32,
    10
)


y = torch.randint(
    0,
    2,
    (
        32,
    )
)
```

---

# 111. Tensor 이동

```python
x = x.to(
    device
)

y = y.to(
    device
)
```

---

# 112. 확인

```python
print(
    "Model:",
    next(
        model.parameters()
    ).device
)


print(
    "X:",
    x.device
)


print(
    "Y:",
    y.device
)
```

---

# 113. Forward

```python
outputs = model(
    x
)
```

---

# 114. Output Device

```python
print(
    outputs.device
)
```

Model과 Input이 GPU에 있다면 Output도 GPU Tensor가 된다.

---

# 115. Loss

```python
loss_fn = nn.CrossEntropyLoss()


loss = loss_fn(
    outputs,
    y
)
```

---

# 116. Optimizer

```python
optimizer = torch.optim.Adam(
    model.parameters(),
    lr=0.001
)
```

---

# 117. Backward

```python
optimizer.zero_grad()

loss.backward()

optimizer.step()
```

이렇게 GPU에서도 Training Step을 수행할 수 있다.

---

# 118. 전체 GPU Training 예

```python
import torch
import torch.nn as nn


device = torch.device(
    "cuda"
    if torch.cuda.is_available()
    else "cpu"
)


model = nn.Sequential(
    nn.Linear(
        10,
        32
    ),

    nn.ReLU(),

    nn.Linear(
        32,
        2
    )
).to(
    device
)


loss_fn = nn.CrossEntropyLoss()


optimizer = torch.optim.Adam(
    model.parameters(),
    lr=0.001
)


x = torch.randn(
    32,
    10
).to(
    device
)


y = torch.randint(
    0,
    2,
    (
        32,
    )
).to(
    device
)


optimizer.zero_grad()


outputs = model(
    x
)


loss = loss_fn(
    outputs,
    y
)


loss.backward()


optimizer.step()


print(
    "Device:",
    device
)


print(
    "Loss:",
    loss.item()
)
```

---

# 119. DataLoader Training

실제 Training에서는:

```python
for x, y in train_loader:

    x = x.to(
        device
    )

    y = y.to(
        device
    )

    optimizer.zero_grad()

    outputs = model(
        x
    )

    loss = loss_fn(
        outputs,
        y
    )

    loss.backward()

    optimizer.step()
```

이다.

---

# 120. Validation

```python
model.eval()


with torch.no_grad():

    for x, y in val_loader:

        x = x.to(
            device
        )

        y = y.to(
            device
        )

        outputs = model(
            x
        )
```

---

# 121. Prediction을 NumPy로 바꾸기

GPU Tensor:

```python
outputs
```

를 NumPy로 변환하려면 일반적으로 CPU로 이동한다.

```python
outputs_cpu = outputs.detach().cpu()
```

---

# 122. NumPy 변환

```python
outputs_np = (
    outputs
    .detach()
    .cpu()
    .numpy()
)
```

---

# 123. 왜 바로 numpy()를 못 쓰나?

NumPy는 CPU Memory 기반 Array를 사용한다.

CUDA Tensor는 GPU Memory에 있기 때문에 먼저:

```text
GPU
↓
CPU
```

로 이동해야 한다.

---

# 124. Validation에서는

`torch.no_grad()` 내부라면 Gradient Graph가 생성되지 않으므로:

```python
outputs.cpu().numpy()
```

같은 형태를 볼 수도 있다.

그래도:

```text
GPU Tensor는 CPU로 옮겨야 NumPy 변환 가능
```

이라는 원칙을 기억한다.

---

# 125. GPU Debugging 핵심

오류가 발생하면:

```python
print(
    device
)


print(
    next(
        model.parameters()
    ).device
)


print(
    x.device
)


print(
    y.device
)
```

부터 확인한다.

---

# 126. Shape도 같이 확인

GPU 문제처럼 보이지만 실제로 Shape 문제일 수도 있다.

```python
print(
    x.shape
)


print(
    y.shape
)


print(
    outputs.shape
)
```

도 같이 확인한다.

---

# 127. dtype도 확인

```python
print(
    x.dtype
)


print(
    y.dtype
)
```

Loss Function에 따라 Target dtype이 중요하다.

---

# 128. Deep Learning 오류 확인 3종 세트

```text
Shape

dtype

device
```

이다.

많은 PyTorch 오류를 해결할 때 가장 먼저 확인한다.

---

# 129. GPU가 인식되지 않을 때 무작정 CUDA를 재설치하지 않는다

먼저 현재 상태를 확인한다.

```text
nvidia-smi

torch.cuda.is_available()

torch.__version__

torch.version.cuda

sys.executable
```

등이다.

---

# 130. 이유

문제 원인이:

```text
Driver

PyTorch Build

Python Environment

Jupyter Kernel
```

중 무엇인지 먼저 구분해야 하기 때문이다.

---

# 131. nvidia-smi는 되는데 PyTorch CUDA=False

이 경우:

```text
GPU와 Driver는 인식되지만

현재 PyTorch Environment에서
CUDA를 사용하지 못하는 상황
```

일 수 있다.

현재 PyTorch 설치와 Environment를 확인한다.

---

# 132. Notebook만 False

Terminal에서는:

```text
True
```

인데 Notebook에서는:

```text
False
```

라면 Jupyter Kernel과 Terminal Python Environment가 다른지 확인한다.

---

# 133. 확인

Notebook:

```python
import sys


print(
    sys.executable
)
```

Terminal:

```powershell
where python
```

을 비교한다.

---

# 134. Colab 확인

Colab에서도:

```python
import torch


print(
    torch.cuda.is_available()
)


if torch.cuda.is_available():

    print(
        torch.cuda.get_device_name(
            0
        )
    )
```

으로 현재 Runtime GPU를 확인한다.

---

# 135. GPU 사용 중인지 확실하게 확인

Training 중 Terminal에서:

```powershell
nvidia-smi
```

를 실행하여 Python Process가 GPU Memory를 사용하고 있는지 확인할 수 있다.

---

# 136. GPU 사용량 0%가 잠깐 보일 수 있다

`nvidia-smi`는 특정 시점의 상태를 보여준다.

Training이 Batch 준비 중이거나 측정 순간 GPU 계산이 끝났다면 낮게 보일 수 있다.

한 번의 숫자만으로 전체 Training 상태를 단정하지 않는다.

---

# 137. Batch Size를 무조건 크게 하면 좋은가?

아니다.

큰 Batch:

```text
GPU Memory 많이 사용

한 Step에 많은 Sample
```

작은 Batch:

```text
Memory 적게 사용

더 많은 Parameter Update Step
```

등의 차이가 있다.

Model 성능과 Optimization에도 영향을 줄 수 있다.

---

# 138. 가장 큰 Batch가 최적은 아니다

Batch Size도:

```text
Hyperparameter
```

이다.

GPU Memory가 허용하는 최대값을 무조건 사용하는 것이 최적이라는 뜻은 아니다.

---

# 139. GPU와 Reproducibility

GPU에서는 일부 연산이 비결정적으로 동작할 수 있다.

같은 Seed를 설정해도 환경과 연산에 따라 결과가 완전히 같지 않을 수 있다.

---

# 140. Seed

```python
import random
import numpy as np
import torch


seed = 42


random.seed(
    seed
)

np.random.seed(
    seed
)

torch.manual_seed(
    seed
)

torch.cuda.manual_seed_all(
    seed
)
```

---

# 141. Seed의 목적

```text
실험 재현성을 높임
```

이다.

하지만 모든 환경에서 완전히 동일한 결과를 보장한다는 뜻은 아니다.

---

# 142. GPU Training 전체 흐름

```text
NVIDIA GPU
↓
CUDA 사용 가능 확인
↓
device 설정
↓
Model → device
↓
DataLoader
↓
Batch → device
↓
Forward
↓
Loss
↓
Backward
↓
Optimizer
↓
GPU에서 반복 계산
```

---

# 143. CPU Fallback

다음 코드의 장점:

```python
device = torch.device(
    "cuda"
    if torch.cuda.is_available()
    else "cpu"
)
```

CUDA가 없으면 자동으로:

```text
CPU
```

를 사용한다.

따라서 같은 Training Code를 여러 환경에서 사용하기 쉽다.

---

# 144. 하지만 CPU에서는 느릴 수 있다

특히:

```text
CNN

큰 Image

많은 Data

큰 Model
```

에서는 GPU보다 Training 시간이 크게 늘어날 수 있다.

---

# 145. Device를 코드 곳곳에 직접 쓰지 않는 이유

다음처럼:

```python
x.cuda()
```

를 직접 사용할 수도 있지만:

```python
x.to(
    device
)
```

를 사용하면 CPU와 GPU 환경을 모두 지원하기 쉽다.

---

# 146. 좋은 구조

```python
device = torch.device(
    "cuda"
    if torch.cuda.is_available()
    else "cpu"
)
```

한 번 정의하고:

```python
model.to(
    device
)

x.to(
    device
)

y.to(
    device
)
```

처럼 사용한다.

---

# 147. 가장 중요한 코드

```python
device = torch.device(
    "cuda"
    if torch.cuda.is_available()
    else "cpu"
)


model = model.to(
    device
)


for x, y in train_loader:

    x = x.to(
        device
    )

    y = y.to(
        device
    )
```

---

# 148. 가장 중요한 Debugging

```python
print(
    torch.cuda.is_available()
)


print(
    device
)


print(
    next(
        model.parameters()
    ).device
)


print(
    x.device
)
```

---

# 149. GPU가 있다고 자동으로 쓰는 것이 아니다

반드시 기억한다.

```text
GPU Hardware 있음

≠

PyTorch가 GPU 사용 중
```

이다.

확인해야 한다.

```text
CUDA Available?

Model Device?

Tensor Device?
```

---

# 150. 최종 정리

CPU:

```text
범용 Processor
```

GPU:

```text
대규모 병렬 계산에 강점
```

CUDA:

```text
NVIDIA GPU를 범용 계산에 활용하기 위한
Platform / Programming Model
```

PyTorch에서 CUDA 확인:

```python
torch.cuda.is_available()
```

Device 설정:

```python
device = torch.device(
    "cuda"
    if torch.cuda.is_available()
    else "cpu"
)
```

Model 이동:

```python
model = model.to(
    device
)
```

Tensor 이동:

```python
x = x.to(
    device
)

y = y.to(
    device
)
```

가장 중요한 규칙:

```text
Model과 Tensor는
같은 Device에 있어야 한다.
```

GPU Memory가 부족하면 먼저:

```text
Batch Size

Image Size

Model Size
```

를 확인한다.

GPU 문제를 Debugging할 때는:

```text
nvidia-smi

torch.cuda.is_available()

torch.cuda.get_device_name(0)

torch.__version__

torch.version.cuda

sys.executable
```

등을 확인한다.

그리고 PyTorch 오류에서는 항상:

```text
Shape

dtype

device
```

세 가지를 먼저 확인하는 습관을 들인다.