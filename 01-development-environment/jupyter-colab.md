# Jupyter Notebook과 Google Colab

## 1. Jupyter Notebook이란?

Jupyter Notebook은 코드를 셀(Cell) 단위로 작성하고 실행할 수 있는 개발 환경이다.

일반적인 Python 파일은 `.py` 확장자를 사용하지만 Jupyter Notebook은 다음과 같은 확장자를 사용한다.

```text
.ipynb
```

예:

```text
analysis.ipynb
mission1.ipynb
mission2.ipynb
```

데이터 분석, 머신러닝, 딥러닝 실습에서 많이 사용했다.

---

## 2. Python 파일과 Notebook의 차이

일반적인 Python 파일은 다음과 같이 작성한다.

```text
main.py
```

```python
print("Hello")

a = 10
b = 20

print(a + b)
```

보통 파일 전체 또는 특정 코드를 실행한다.

Jupyter Notebook에서는 코드를 여러 셀로 나눌 수 있다.

```text
Cell 1
↓
데이터 불러오기

Cell 2
↓
데이터 전처리

Cell 3
↓
모델 학습

Cell 4
↓
모델 평가
```

따라서 데이터 분석이나 머신러닝 실험 과정을 단계별로 확인하기 편리하다.

---

## 3. Cell

Jupyter Notebook의 기본 실행 단위를 Cell이라고 한다.

대표적으로 두 종류의 Cell을 사용한다.

### Code Cell

Python 코드를 작성하고 실행한다.

```python
import pandas as pd

df = pd.read_csv("data.csv")
df.head()
```

### Markdown Cell

설명이나 제목 등을 작성한다.

```markdown
# 데이터 분석

## 데이터 확인

이 단계에서는 데이터의 크기와 결측치를 확인한다.
```

따라서 Notebook 안에서 코드와 설명을 함께 정리할 수 있다.

---

## 4. Kernel

Notebook의 Python 코드를 실제로 실행하는 프로그램을 Kernel이라고 한다.

구조를 단순화하면 다음과 같다.

```text
Jupyter Notebook
       ↓
     Kernel
       ↓
     Python
       ↓
설치된 라이브러리
```

예를 들어:

```text
mission3.ipynb
       ↓
ml_env Kernel
       ↓
Python 3.10
```

과 같이 연결될 수 있다.

---

## 5. 셀 실행 순서가 중요한 이유

Jupyter Notebook은 실행한 셀의 결과를 메모리에 유지한다.

예를 들어 먼저:

```python
x = 10
```

을 실행하고 다른 셀에서:

```python
print(x)
```

를 실행하면:

```text
10
```

이 출력된다.

하지만 첫 번째 셀을 실행하지 않은 상태에서 바로:

```python
print(x)
```

를 실행하면 다음과 같은 오류가 발생할 수 있다.

```text
NameError: name 'x' is not defined
```

실제로 데이터 분석을 하면서 다음과 같은 오류를 경험했다.

```text
NameError: name 'train' is not defined
NameError: name 'df_patient' is not defined
```

이는 필요한 변수를 만드는 이전 셀이 실행되지 않았거나 Kernel이 초기화된 경우에도 발생할 수 있다.

---

## 6. Kernel Restart

Kernel을 다시 시작하면 메모리에 저장되어 있던 변수들이 사라진다.

예를 들어 이전에:

```python
train = pd.read_csv("train.csv")
```

를 실행했더라도 Kernel을 Restart하면 `train` 변수는 다시 만들어야 한다.

따라서 Notebook을 작성할 때는 위에서부터 순서대로 실행해도 정상적으로 동작하도록 만드는 것이 중요하다.

---

## 7. Google Colab

Google Colab은 웹 브라우저에서 Jupyter Notebook 형태로 Python 코드를 실행할 수 있는 환경이다.

별도의 Python 개발 환경을 직접 구성하지 않아도 브라우저에서 데이터 분석, 머신러닝, 딥러닝 등을 실행할 수 있다는 장점이 있다.

실습에서 Colab을 많이 사용했다.

---

## 8. Colab의 `/content` 경로

Google Colab에서는 기본 작업 경로로 다음 위치를 많이 사용한다.

```text
/content/
```

예를 들어:

```python
train = pd.read_csv("/content/train.csv")
```

는 Colab의 `/content` 폴더에 있는 `train.csv`를 읽는 코드이다.

---

## 9. FileNotFoundError

Colab에서 많이 경험한 오류 중 하나이다.

예:

```text
FileNotFoundError
```

다음 코드를 실행했는데:

```python
train = pd.read_csv("/content/train.csv")
```

실제 `/content` 폴더에 `train.csv`가 없다면 오류가 발생한다.

따라서 파일을 불러오기 전에 현재 파일을 확인하는 것이 좋다.

```python
import os

print(os.listdir("/content"))
```

또는:

```python
!ls /content
```

을 사용할 수 있다.

---

## 10. Colab에 파일 업로드

로컬 컴퓨터에 있는 파일을 Colab에 업로드하여 사용할 수 있다.

예를 들어 다음과 같은 파일들을 업로드하여 프로젝트를 진행했다.

```text
train.csv
test.csv
sample_submission.csv
.zip 데이터
이미지 데이터
```

업로드한 파일의 실제 이름과 경로를 확인한 후 코드에서 사용해야 한다.

---

## 11. ZIP 파일 압축 해제

이미지 데이터처럼 여러 파일이 들어 있는 데이터는 ZIP 파일 형태로 제공되는 경우가 있었다.

Python에서는 다음과 같이 압축을 해제할 수 있다.

```python
import zipfile

zip_path = "/content/data.zip"
extract_path = "/content/data"

with zipfile.ZipFile(zip_path, "r") as zip_ref:
    zip_ref.extractall(extract_path)
```

압축을 해제한 뒤 실제 폴더 구조를 확인하는 것이 중요하다.

```python
import os

print(os.listdir("/content/data"))
```

---

## 12. 경로 문제

프로젝트를 진행하면서 파일 경로 문제를 여러 번 경험했다.

예:

```text
/content/train.csv

/content/data/train.csv

/content/herniated_data/02_Patient_Data.csv
```

코드가 올바르더라도 실제 파일 위치와 코드에 작성한 경로가 다르면 파일을 찾을 수 없다.

따라서:

```text
현재 작업 위치
+
실제 파일 위치
+
코드에 작성한 경로
```

를 함께 확인해야 한다.

---

## 13. Colab Runtime

Colab에서는 코드가 Runtime에서 실행된다.

Runtime이 종료되거나 초기화되면 메모리에 저장되어 있던 변수와 세션 정보가 사라질 수 있다.

따라서 이전에 실행했던:

```python
train
model
X_train
X_valid
```

등의 변수도 다시 생성해야 할 수 있다.

이 때문에 Notebook의 처음부터 코드를 다시 실행해야 하는 경우가 있다.

---

## 14. CPU와 GPU

머신러닝과 특히 딥러닝에서는 CPU뿐만 아니라 GPU를 이용하여 학습할 수 있다.

PyTorch에서는 다음과 같이 GPU 사용 가능 여부를 확인할 수 있다.

```python
import torch

print(torch.cuda.is_available())
```

GPU를 사용할 수 있다면:

```python
device = torch.device(
    "cuda" if torch.cuda.is_available() else "cpu"
)

print(device)
```

와 같이 사용할 장치를 선택할 수 있다.

---

## 15. 모델을 GPU로 이동

PyTorch 모델을 GPU에서 학습하려면 모델과 데이터를 같은 장치로 이동해야 한다.

예:

```python
model = model.to(device)
```

데이터도:

```python
x = x.to(device)
y = y.to(device)
```

와 같이 이동한다.

모델은 GPU에 있고 데이터는 CPU에 있는 것처럼 서로 다른 장치를 사용하면 오류가 발생할 수 있다.

---

## 16. Colab GPU와 내 컴퓨터 GPU

Colab에서 제공하는 GPU와 내 컴퓨터에 설치된 GPU는 기본적으로 서로 다른 장치이다.

일반적인 Colab:

```text
내 컴퓨터
    ↓
인터넷
    ↓
Google Colab 서버
    ↓
Colab의 GPU
```

즉 Colab에서 GPU Runtime을 선택하면 일반적으로 Google 서버의 GPU를 사용하는 것이다.

반면 로컬 개발 환경에서는:

```text
VS Code / Jupyter
       ↓
내 컴퓨터의 Python
       ↓
CUDA
       ↓
내 컴퓨터의 NVIDIA GPU
```

형태로 사용할 수 있다.

---

## 17. 로컬 GPU

딥러닝 실습에서 로컬 NVIDIA GPU를 이용하여 모델을 학습할 수도 있다.

GPU가 정상적으로 연결되어 있다면 PyTorch에서:

```python
import torch

print(torch.cuda.is_available())

if torch.cuda.is_available():
    print(torch.cuda.get_device_name(0))
```

와 같이 확인할 수 있다.

---

## 18. Colab에서 로컬 환경 사용

Colab 화면을 사용하면서 실제 코드 실행은 내 컴퓨터에서 수행하는 방법도 있다.

개념적으로는:

```text
Colab 화면
     ↓
로컬 Jupyter 서버
     ↓
내 컴퓨터 Python 환경
     ↓
내 컴퓨터 GPU
```

형태이다.

이 경우 Colab의 GPU를 사용하는 것이 아니라 내 컴퓨터의 Python 환경과 GPU를 이용하게 된다.

---

## 19. 머신러닝 프로젝트에서 사용한 흐름

여러 프로젝트에서 Notebook을 다음과 같은 흐름으로 사용했다.

```text
라이브러리 import
        ↓
데이터 불러오기
        ↓
데이터 확인
        ↓
EDA
        ↓
전처리
        ↓
Train / Validation 분리
        ↓
모델 생성
        ↓
모델 학습
        ↓
예측
        ↓
성능 평가
        ↓
결과 해석
```

Notebook을 이용하면 각 과정을 셀 단위로 실행하면서 중간 결과를 확인할 수 있다.

---

## 20. Notebook을 사용할 때 주의할 점

Notebook은 편리하지만 셀을 원하는 순서대로 실행할 수 있기 때문에 실행 순서가 꼬일 수 있다.

예를 들어 화면상으로는:

```text
Cell 1
Cell 2
Cell 3
```

순서여도 실제 실행은:

```text
Cell 3
→ Cell 1
→ Cell 2
```

처럼 했을 수 있다.

이 경우 다른 환경에서 Notebook을 처음부터 실행하면 오류가 발생할 수 있다.

따라서 최종적으로는:

```text
Kernel Restart
      ↓
위에서부터 전체 실행
      ↓
오류 없이 실행되는지 확인
```

하는 습관이 중요하다.

---

## 21. 문제 해결 방법

Jupyter나 Colab에서 문제가 발생했을 때 다음과 같은 순서로 확인할 수 있다.

```text
1. 현재 Kernel 확인
        ↓
2. Python 환경 확인
        ↓
3. 필요한 패키지 확인
        ↓
4. 파일 경로 확인
        ↓
5. 이전 셀 실행 여부 확인
        ↓
6. Runtime / Kernel 상태 확인
        ↓
7. 처음부터 다시 실행
```

---

## 22. 배운 점

Jupyter Notebook은 코드를 셀 단위로 실행할 수 있기 때문에 데이터 분석과 머신러닝 실험에 편리하다는 것을 학습했다.

하지만 셀 실행 순서와 Kernel의 상태에 따라 같은 코드라도 오류가 발생할 수 있다는 점도 알게 되었다.

Google Colab에서는 `/content` 경로와 Runtime의 특성을 이해해야 하며 파일이 실제로 존재하는 위치를 확인하는 것이 중요하다.

또한 딥러닝에서는 CPU와 GPU의 차이를 이해하고 모델과 데이터를 같은 Device에서 실행해야 한다는 것을 학습했다.

개발 환경에서 오류가 발생했을 때는 코드를 바로 수정하기보다 Kernel, Python 환경, 패키지, 파일 경로, 실행 순서를 하나씩 확인하는 것이 중요하다.