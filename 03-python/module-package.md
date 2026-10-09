# Python Module과 Package

## 1. 지금까지 사용했던 import

데이터 분석이나 머신러닝을 하면서 다음과 같은 코드를 많이 사용했다.

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```

또는:

```python
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
```

처음에는 단순히 코드 맨 위에 작성해야 하는 코드처럼 보일 수 있다.

하지만 이 코드는 Python에서 다른 사람이 만들어 놓은 기능을 가져와 사용하기 위한 것이다.

---

# 2. 모든 기능을 직접 만들 필요는 없다

예를 들어 CSV 파일을 읽는 기능을 처음부터 직접 만든다고 생각하면 매우 복잡하다.

하지만 Pandas를 사용하면:

```python
import pandas as pd

df = pd.read_csv("data.csv")
```

만으로 CSV 파일을 읽을 수 있다.

머신러닝 모델도 직접 알고리즘 전체를 구현하지 않고:

```python
from sklearn.ensemble import RandomForestClassifier

model = RandomForestClassifier()
```

처럼 사용할 수 있다.

이것이 Python의 강력한 장점 중 하나이다.

---

# 3. Module이란?

Module은 Python 코드를 담고 있는 파일이라고 이해할 수 있다.

예를 들어:

```text
calculator.py
```

라는 파일이 있고:

```python
def add(a, b):
    return a + b
```

라는 함수가 있다고 하자.

다른 Python 파일에서 이 기능을 가져와 사용할 수 있다.

```python
import calculator
```

그리고:

```python
calculator.add(10, 20)
```

처럼 사용할 수 있다.

즉:

```text
Python 파일
   ↓
Module
   ↓
다른 Python 코드에서 import
```

라고 이해할 수 있다.

---

# 4. Package란?

Package는 여러 Module을 묶어서 관리하는 구조라고 이해할 수 있다.

예를 들어:

```text
my_package/
├── data.py
├── model.py
└── evaluation.py
```

처럼 여러 Python Module을 하나의 기능 단위로 관리할 수 있다.

실제로 우리가 사용하는:

```text
pandas
numpy
sklearn
torch
```

등도 다양한 기능을 포함하고 있다.

---

# 5. Library란?

Library는 특정 목적을 위해 만들어진 여러 기능의 모음이라는 넓은 의미로 많이 사용한다.

예:

```text
NumPy
→ 수치 계산

Pandas
→ 데이터 분석

Matplotlib
→ 시각화

Scikit-learn
→ 머신러닝

PyTorch
→ 딥러닝
```

실제 개발에서는 Module, Package, Library라는 용어를 문맥에 따라 접하게 된다.

처음에는 다음 정도로 이해하면 된다.

```text
Module
→ Python 코드 파일 단위

Package
→ 여러 Module을 묶은 구조

Library
→ 특정 목적을 위한 기능들의 모음
```

---

# 6. import

다른 Module이나 Package의 기능을 현재 코드에서 사용하려면 `import`를 사용한다.

예:

```python
import pandas
```

이제:

```python
pandas.read_csv("data.csv")
```

처럼 Pandas 기능을 사용할 수 있다.

---

# 7. as

긴 이름을 짧은 별명으로 사용할 수 있다.

```python
import pandas as pd
```

이제:

```python
pd.read_csv("data.csv")
```

처럼 사용할 수 있다.

여기서:

```text
pandas
→ 실제 Package 이름

pd
→ 현재 코드에서 사용할 별명
```

이다.

---

# 8. 자주 사용하는 별명

데이터 분석에서는 관례적으로 다음과 같은 별명을 많이 사용한다.

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
```

따라서:

```text
np → numpy
pd → pandas
plt → matplotlib.pyplot
```

라고 읽으면 된다.

`pd`라는 별도의 라이브러리가 존재하는 것이 아니다.

---

# 9. from ... import ...

Package 전체가 아니라 필요한 기능을 직접 가져올 수도 있다.

예:

```python
from sklearn.model_selection import train_test_split
```

구조:

```text
sklearn
   ↓
model_selection
   ↓
train_test_split
```

그리고 바로:

```python
train_test_split(...)
```

을 사용할 수 있다.

---

# 10. import 방식 비교

방법 1:

```python
import pandas as pd

df = pd.read_csv("data.csv")
```

방법 2:

```python
from pandas import read_csv

df = read_csv("data.csv")
```

둘 다 사용할 수 있지만 어떤 방식이 읽기 쉽고 일반적인지는 라이브러리와 프로젝트에 따라 다르다.

Pandas는 보통:

```python
import pandas as pd
```

형태를 많이 사용한다.

---

# 11. 머신러닝 import 읽기

다음 코드를 보자.

```python
from sklearn.ensemble import RandomForestClassifier
```

이를 나누면:

```text
sklearn
→ Scikit-learn

ensemble
→ sklearn 내부의 ensemble 관련 Module

RandomForestClassifier
→ 사용할 머신러닝 클래스
```

이다.

이제:

```python
model = RandomForestClassifier()
```

로 모델을 만들 수 있다.

---

# 12. 여러 기능 import

한 Module에서 여러 기능을 가져올 수도 있다.

```python
from sklearn.metrics import (
    accuracy_score,
    precision_score,
    recall_score,
    f1_score
)
```

이제 각각:

```python
accuracy_score(...)
precision_score(...)
recall_score(...)
f1_score(...)
```

처럼 사용할 수 있다.

---

# 13. 설치와 import는 다르다

Python을 배우면서 매우 중요한 부분이다.

다음 두 작업은 서로 다르다.

```text
설치
import
```

예를 들어:

```bash
pip install pandas
```

는 Pandas를 현재 Python 환경에 설치하는 것이다.

반면:

```python
import pandas as pd
```

는 이미 설치된 Pandas를 현재 Python 코드에서 사용하겠다는 의미이다.

따라서:

```text
pip install
→ 컴퓨터의 Python 환경에 준비

import
→ 현재 Python 코드에서 사용
```

이라고 이해할 수 있다.

---

# 14. pip

`pip`는 Python Package를 설치할 때 많이 사용하는 Package Manager이다.

예:

```bash
pip install pandas
```

```bash
pip install numpy
```

```bash
pip install scikit-learn
```

설치된 Package를 확인하려면:

```bash
pip list
```

를 사용할 수 있다.

---

# 15. Jupyter / Colab에서 pip 사용

Notebook에서는 다음처럼 사용하는 경우가 많다.

```python
!pip install catboost
```

또는:

```python
!pip install optuna shap
```

앞의 `!`는 Notebook에서 Shell 명령어를 실행하기 위해 사용하는 방식이다.

터미널에서는:

```bash
pip install catboost
```

처럼 사용한다.

---

# 16. ModuleNotFoundError

다음과 같은 오류를 자주 경험할 수 있다.

```text
ModuleNotFoundError:
No module named 'skopt'
```

이 오류는 현재 Python 환경에서 해당 Module을 찾을 수 없다는 의미이다.

대표적인 원인은:

```text
1. Package가 설치되지 않음

2. 다른 Python 환경에 설치함

3. Jupyter Kernel과 설치 환경이 다름

4. Package 이름 또는 import 이름을 잘못 입력함
```

등이 있다.

---

# 17. 설치되지 않은 경우

예를 들어:

```python
from skopt import BayesSearchCV
```

를 실행했는데:

```text
ModuleNotFoundError:
No module named 'skopt'
```

가 발생했다고 하자.

필요한 Package를 설치해야 한다.

이 경우 설치 Package 이름은:

```bash
pip install scikit-optimize
```

이다.

설치 이름과 import 이름이 항상 똑같은 것은 아니라는 점도 중요하다.

```text
설치
scikit-optimize

import
skopt
```

처럼 다를 수 있다.

---

# 18. gensim 오류

Word2Vec을 사용하려고:

```python
from gensim.models import Word2Vec
```

를 실행했는데 `gensim`이 설치되지 않았다면:

```text
ModuleNotFoundError
```

가 발생할 수 있다.

설치:

```bash
pip install gensim
```

설치 후:

```python
from gensim.models import Word2Vec
```

를 사용할 수 있다.

---

# 19. medmnist 오류

의료영상 데이터를 사용하면서:

```python
from medmnist import OrganAMNIST
```

를 실행했는데 Package가 없다면:

```text
ModuleNotFoundError:
No module named 'medmnist'
```

가 발생한다.

설치:

```bash
pip install medmnist
```

그 후 다시 import한다.

---

# 20. Package를 설치했는데도 오류가 나는 경우

Package를 설치했는데도:

```text
ModuleNotFoundError
```

가 발생할 수 있다.

이때 중요한 것이 **Python 환경**이다.

예를 들어 컴퓨터에:

```text
base
ml_env
```

두 개의 Conda 환경이 있다고 하자.

`base`에 Package를 설치했는데 Jupyter Notebook은 `ml_env`를 사용한다면 Notebook에서는 해당 Package를 찾지 못할 수 있다.

---

# 21. Python 환경과 Package

개념적으로:

```text
base
├── pandas
├── numpy
└── sklearn


ml_env
├── pandas
├── torch
└── xgboost
```

처럼 환경마다 설치된 Package가 다를 수 있다.

따라서:

```text
설치했는데 왜 import가 안 되지?
```

라는 상황에서는:

> 내가 지금 사용하는 Python과 Package를 설치한 Python이 같은가?

를 확인해야 한다.

---

# 22. 현재 Python 확인

터미널에서:

```bash
python --version
```

으로 Python 버전을 확인할 수 있다.

Windows에서는:

```bash
where python
```

으로 어떤 Python 실행 파일이 사용되고 있는지 확인할 수도 있다.

Python 코드에서는:

```python
import sys

print(sys.executable)
```

을 사용하면 현재 실행 중인 Python의 위치를 확인할 수 있다.

---

# 23. Jupyter Kernel

Jupyter Notebook은 선택된 Kernel의 Python 환경을 사용한다.

예를 들어 VS Code에서:

```text
ml_env (Python 3.10.21)
```

Kernel을 선택했다면 Notebook은 해당 Python 환경에서 실행된다.

따라서 Package도 그 환경에 설치되어 있어야 한다.

---

# 24. 설치 위치 확인

현재 Python 환경에 특정 Package가 설치되어 있는지 확인할 수 있다.

예:

```bash
pip show pandas
```

또는:

```bash
python -m pip show pandas
```

를 사용할 수 있다.

---

# 25. python -m pip

여러 Python 환경이 있는 경우:

```bash
python -m pip install pandas
```

형태를 사용할 수 있다.

현재 `python` 명령어가 가리키는 Python을 기준으로 pip를 실행한다는 의미이다.

예:

```bash
python -m pip install scikit-optimize
```

---

# 26. Conda 환경 확인

Conda를 사용한다면:

```bash
conda env list
```

로 환경을 확인할 수 있다.

예:

```text
base
ml_env
```

환경 활성화:

```bash
conda activate ml_env
```

활성화 후:

```bash
python --version
```

```bash
where python
```

등으로 실제 환경을 확인할 수 있다.

---

# 27. requirements.txt

프로젝트에 필요한 Package를 기록해둘 수도 있다.

예:

```text
pandas
numpy
scikit-learn
matplotlib
torch
```

이 내용을:

```text
requirements.txt
```

파일에 저장할 수 있다.

다른 환경에서는:

```bash
pip install -r requirements.txt
```

로 필요한 Package들을 설치할 수 있다.

---

# 28. 버전을 지정할 수도 있다

특정 버전이 필요한 경우:

```text
pandas==2.3.0
numpy==2.2.0
```

처럼 기록할 수 있다.

Package 버전에 따라 기능이나 사용법이 달라질 수 있기 때문에 프로젝트를 재현해야 하는 경우 버전 관리가 중요해진다.

---

# 29. 직접 Module 만들어보기

예를 들어:

```text
calculator.py
```

파일을 만든다.

내용:

```python
def add(a, b):
    return a + b


def multiply(a, b):
    return a * b
```

다른 파일에서:

```python
import calculator
```

그리고:

```python
result = calculator.add(10, 20)

print(result)
```

결과:

```text
30
```

이렇게 내가 만든 Python 파일도 Module로 사용할 수 있다.

---

# 30. 필요한 함수만 가져오기

다음처럼 사용할 수도 있다.

```python
from calculator import add
```

그러면:

```python
result = add(10, 20)
```

처럼 바로 사용할 수 있다.

---

# 31. 머신러닝 코드의 import 부분 읽기

다음과 같은 코드를 보자.

```python
import pandas as pd
import numpy as np

from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score
```

이를 하나씩 읽으면:

```text
Pandas
→ 데이터 처리

NumPy
→ 수치 계산

train_test_split
→ 학습/검증 데이터 분리

StandardScaler
→ 데이터 스케일링

RandomForestClassifier
→ 분류 모델

accuracy_score
→ 정확도 평가
```

이다.

즉 import 부분만 봐도 이 코드가 어떤 작업을 할 것인지 어느 정도 예상할 수 있다.

---

# 32. 딥러닝 코드의 import 부분

PyTorch 코드에서는 다음과 같은 형태를 볼 수 있다.

```python
import torch
import torch.nn as nn

from torch.utils.data import DataLoader
```

역할:

```text
torch
→ PyTorch 기본 기능

torch.nn
→ Neural Network 관련 기능

DataLoader
→ 데이터를 Batch 단위로 가져오는 기능
```

이후:

```python
nn.Linear(...)
nn.ReLU()
nn.Sequential(...)
```

같은 기능을 사용할 수 있다.

---

# 33. Computer Vision에서 import

이미지 프로젝트에서는:

```python
from torchvision import transforms
```

같은 코드를 볼 수 있다.

그리고:

```python
transforms.Resize(...)
transforms.ToTensor()
```

등을 이용하여 이미지 전처리를 수행할 수 있다.

즉 프로젝트 분야에 따라 사용하는 Package와 Module이 달라진다.

---

# 34. import 오류를 만났을 때 확인 순서

`ModuleNotFoundError`가 발생하면 바로 코드를 모두 수정하지 말고 다음 순서로 확인한다.

```text
1. 오류 메시지 확인

2. 어떤 Module이 없다고 하는지 확인

3. Package가 설치되어 있는지 확인

4. 현재 Python 환경 확인

5. Jupyter라면 Kernel 확인

6. 필요한 Package 설치

7. Kernel 또는 실행 환경 재시작

8. 다시 import
```

---

# 35. FileNotFoundError와 구분하기

다음 오류:

```text
ModuleNotFoundError
```

는 Python Module이나 Package를 찾지 못한 것이다.

반면:

```text
FileNotFoundError
```

는 데이터 파일 등의 경로를 찾지 못한 것이다.

예:

```python
pd.read_csv("/content/train.csv")
```

에서 파일이 실제로 존재하지 않으면:

```text
FileNotFoundError
```

가 발생할 수 있다.

따라서:

```text
ModuleNotFoundError
→ Package / Module 문제

FileNotFoundError
→ 파일 / 경로 문제
```

로 구분해야 한다.

---

# 36. NameError와도 구분하기

예:

```python
print(train.shape)
```

에서 `train` 변수를 만들지 않았다면:

```text
NameError
```

가 발생한다.

정리:

```text
NameError
→ 변수나 함수 이름을 찾지 못함

ModuleNotFoundError
→ Module을 찾지 못함

FileNotFoundError
→ 파일을 찾지 못함
```

오류 이름만 읽어도 문제의 종류를 어느 정도 판단할 수 있다.

---

# 37. 지금까지 자주 사용한 주요 Library

데이터 분석:

```text
NumPy
Pandas
Matplotlib
```

통계 분석:

```text
SciPy
statsmodels
```

머신러닝:

```text
Scikit-learn
XGBoost
LightGBM
CatBoost
Optuna
SHAP
imbalanced-learn
```

딥러닝:

```text
PyTorch
torchvision
```

자연어 처리:

```text
gensim
```

의료영상 실습:

```text
MedMNIST
```

각각 필요한 기능을 직접 처음부터 구현하지 않고 이미 만들어진 기능을 활용하기 위해 사용한다.

---

# 38. Package를 무조건 많이 설치할 필요는 없다

프로젝트마다 필요한 Package가 다르다.

따라서 무조건 모든 Package를 설치하기보다 실제 프로젝트에 필요한 Package를 설치하는 것이 좋다.

예를 들어 간단한 데이터 분석이라면:

```text
pandas
numpy
matplotlib
```

정도만 필요할 수도 있다.

머신러닝을 시작하면:

```text
scikit-learn
xgboost
lightgbm
```

등이 추가될 수 있다.

---

# 39. 전체 구조 이해하기

Python 프로그램을 하나의 구조로 보면:

```text
Python
  │
  ├── 내가 작성한 코드
  │
  └── 외부 Package
          │
          ├── Pandas
          ├── NumPy
          ├── Scikit-learn
          └── PyTorch
```

내 코드에서:

```python
import ...
```

를 사용하여 필요한 기능을 가져온다.

---

# 40. Python 기초 전체 연결

지금까지 배운 Python 기초를 연결하면 다음과 같다.

```text
변수 / 자료형
      ↓
조건문
      ↓
반복문
      ↓
함수
      ↓
자료구조
      ↓
Module / Package
```

그리고 이 개념들을 조합하면:

```python
import pandas as pd

models = [
    "RandomForest",
    "XGBoost"
]

for model in models:

    print(model)
```

같은 프로그램을 작성할 수 있다.

더 발전하면 머신러닝 코드가 된다.

---

# 41. 복잡한 AI 코드도 Python 코드이다

예를 들어:

```python
from sklearn.ensemble import RandomForestClassifier

model = RandomForestClassifier(
    n_estimators=100,
    random_state=42
)

model.fit(X_train, y_train)

pred = model.predict(X_test)
```

구조를 나누면:

```text
from ... import
→ 필요한 기능 가져오기

model =
→ 변수

RandomForestClassifier(...)
→ 객체 생성

model.fit(...)
→ 메서드 호출

X_train, y_train
→ 함수에 전달하는 데이터

pred =
→ 예측 결과 저장
```

이다.

즉 처음에는 복잡한 머신러닝 코드처럼 보이지만 Python 기초 문법을 조합해서 만들어진 코드이다.

---

# 42. 배운 점

Python에서는 모든 기능을 직접 만들지 않고 Module과 Package를 이용하여 이미 만들어진 기능을 가져와 사용할 수 있다.

```python
import pandas as pd
```

에서:

```text
pandas
→ 사용할 Package

pd
→ 별명
```

이다.

또한:

```python
from sklearn.ensemble import RandomForestClassifier
```

처럼 Package 내부에서 필요한 기능만 가져올 수도 있다.

중요한 것은:

```text
설치
≠
import
```

라는 점이다.

```bash
pip install
```

은 Package를 Python 환경에 설치하는 것이고:

```python
import
```

는 설치된 기능을 현재 코드에서 사용하는 것이다.

또한 Package를 설치했는데도 `ModuleNotFoundError`가 발생한다면 현재 Python 환경과 Jupyter Kernel이 같은 환경인지 확인해야 한다.

오류가 발생했을 때:

```text
NameError
ModuleNotFoundError
FileNotFoundError
```

을 모두 같은 오류로 생각하지 않고 오류 이름과 마지막 메시지를 읽어 문제의 종류를 먼저 판단하는 것이 중요하다.

Python의:

```text
변수
조건문
반복문
함수
자료구조
Module
Package
```

를 이해하면 이후 데이터 분석, 머신러닝, 딥러닝 코드가 각각 완전히 새로운 문법이 아니라 Python 기초를 확장해서 사용하는 것이라는 것을 이해할 수 있다.