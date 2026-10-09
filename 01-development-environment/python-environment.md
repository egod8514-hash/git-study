# Python 개발 환경과 가상환경

## 1. Python 개발 환경이란?

Python으로 개발할 때는 단순히 Python 코드만 작성하는 것이 아니라
Python 버전, 설치된 라이브러리, 가상환경 등을 함께 관리해야 한다.

예를 들어 머신러닝 프로젝트에서는 다음과 같은 라이브러리를 사용할 수 있다.

```text
pandas
numpy
scikit-learn
xgboost
lightgbm
catboost
pytorch
shap
```

이러한 라이브러리들은 Python 환경에 설치되어 있어야 사용할 수 있다.

---

## 2. Python 버전 확인

현재 터미널에서 사용하고 있는 Python 버전은 다음 명령어로 확인할 수 있다.

```powershell
python --version
```

예:

```text
Python 3.10.21
```

Python이 정상적으로 실행되는지도 간단하게 확인할 수 있다.

```powershell
python -c "print('hello')"
```

결과:

```text
hello
```

가 출력되면 Python 자체는 정상적으로 실행되고 있다는 의미이다.

---

## 3. pip란?

`pip`는 Python 라이브러리를 설치하고 관리하는 패키지 관리자이다.

예를 들어 pandas를 설치하려면:

```powershell
pip install pandas
```

scikit-learn을 설치하려면:

```powershell
pip install scikit-learn
```

여러 패키지를 한 번에 설치할 수도 있다.

```powershell
pip install pandas numpy scikit-learn
```

설치된 패키지는 다음 명령어로 확인할 수 있다.

```powershell
pip list
```

---

## 4. 패키지와 import

패키지를 설치했다고 바로 사용하는 것은 아니다.

먼저 설치:

```powershell
pip install pandas
```

그다음 Python 코드에서 불러온다.

```python
import pandas as pd
```

즉,

```text
pip install
    ↓
컴퓨터의 Python 환경에 라이브러리 설치
    ↓
import
    ↓
Python 코드에서 라이브러리 사용
```

의 관계이다.

---

## 5. ModuleNotFoundError

Python을 사용하면서 자주 만났던 오류 중 하나이다.

예:

```text
ModuleNotFoundError: No module named 'catboost'
```

이 오류는 현재 실행 중인 Python 환경에서 해당 라이브러리를 찾을 수 없다는 의미이다.

보통 다음과 같은 경우 발생할 수 있다.

1. 라이브러리를 설치하지 않은 경우
2. 다른 가상환경에 라이브러리를 설치한 경우
3. VS Code가 다른 Python Interpreter를 사용하고 있는 경우
4. Jupyter Notebook이 다른 Kernel을 사용하고 있는 경우

따라서 무조건 다시 설치하기 전에 현재 Python 환경을 확인하는 것이 중요하다.

---

## 6. Conda란?

Conda는 Python 환경과 패키지를 관리할 수 있는 도구이다.

여러 프로젝트에서 서로 다른 Python 버전이나 라이브러리를 사용해야 할 때 가상환경을 만들어 관리할 수 있다.

예를 들어 다음과 같은 환경을 사용할 수 있다.

```text
base
ml_env
```

`base`는 Conda를 설치했을 때 기본적으로 만들어지는 환경이다.

`ml_env`는 머신러닝이나 딥러닝 작업을 위해 별도로 만든 가상환경으로 사용할 수 있다.

---

## 7. 가상환경이 필요한 이유

프로젝트마다 필요한 라이브러리와 버전이 다를 수 있다.

예를 들어:

```text
프로젝트 A
Python 3.10
PyTorch
pandas

프로젝트 B
Python 3.12
FastAPI
SQLAlchemy
```

모든 프로젝트가 하나의 Python 환경을 공유하면 패키지 버전이 서로 충돌할 수 있다.

가상환경을 사용하면:

```text
컴퓨터
│
├── base
│
├── ml_env
│
├── web_env
│
└── project_env
```

처럼 프로젝트별 환경을 분리할 수 있다.

---

## 8. Conda 환경 확인

현재 만들어져 있는 Conda 환경은 다음 명령어로 확인할 수 있다.

```powershell
conda env list
```

또는:

```powershell
conda info --envs
```

현재 활성화된 환경에는 `*` 표시가 나타난다.

예:

```text
base
ml_env     *
```

이 경우 현재 `ml_env`를 사용하고 있다는 의미이다.

---

## 9. Conda 환경 활성화

`ml_env` 환경을 사용하려면:

```powershell
conda activate ml_env
```

활성화되면 터미널 앞부분에 환경 이름이 나타날 수 있다.

```text
(ml_env) C:\Users\user>
```

이 상태에서 실행하는 Python과 pip는 기본적으로 `ml_env` 환경의 것을 사용한다.

---

## 10. Conda 환경 비활성화

현재 가상환경에서 나오려면:

```powershell
conda deactivate
```

를 사용한다.

---

## 11. 새로운 Conda 환경 만들기

예를 들어 Python 3.10을 사용하는 새로운 환경을 만들려면:

```powershell
conda create -n my_env python=3.10
```

여기서:

```text
-n my_env
```

는 가상환경의 이름을 `my_env`로 지정한다는 의미이다.

생성 후:

```powershell
conda activate my_env
```

로 사용할 수 있다.

---

## 12. 현재 어떤 Python을 사용하는지 확인

Windows에서는 다음 명령어를 사용할 수 있다.

```powershell
where python
```

가상환경을 활성화한 상태라면 해당 환경의 Python 경로가 나타날 수 있다.

Python 코드에서도 확인할 수 있다.

```python
import sys

print(sys.executable)
```

이 방법은 Jupyter Notebook에서 실제로 어떤 Python이 실행되고 있는지 확인할 때도 유용하다.

---

## 13. VS Code와 가상환경

VS Code에서 `ml_env`를 사용하려면 해당 환경을 Python Interpreter로 선택해야 한다.

구조를 단순화하면:

```text
VS Code
   ↓
Python Interpreter 선택
   ↓
ml_env
   ↓
Python
   ↓
설치된 라이브러리
```

와 같다.

터미널에서 `ml_env`를 활성화했더라도 VS Code가 다른 Interpreter를 선택하고 있다면 서로 다른 환경을 사용할 수 있으므로 주의해야 한다.

---

## 14. Jupyter와 ipykernel

Jupyter Notebook에서는 Python 환경을 Kernel로 등록하여 사용할 수 있다.

`ipykernel`이 설치되어 있는지 확인할 수 있다.

```powershell
python -c "import ipykernel; print(ipykernel.__version__)"
```

가상환경을 Jupyter Kernel로 등록하려면 다음과 같이 사용할 수 있다.

```powershell
python -m ipykernel install --user --name ml_env --display-name "ml_env"
```

등록된 Kernel은 다음 명령어로 확인할 수 있다.

```powershell
jupyter kernelspec list
```

---

## 15. 실제로 겪었던 Kernel 문제

VS Code에서 Jupyter Notebook을 실행할 때 다음과 같이 Kernel 연결이 계속 진행되는 문제가 발생할 수 있었다.

```text
Connecting to kernel...
```

이때 Python 자체가 실행되는지 먼저 확인했다.

```powershell
python --version
python -c "print('hello')"
```

그리고 `ipykernel`도 확인했다.

```powershell
python -c "import ipykernel; print(ipykernel.__version__)"
```

마지막으로 Jupyter에 `ml_env` Kernel이 등록되어 있는지 확인했다.

```powershell
jupyter kernelspec list
```

이를 통해 문제를 해결할 때는 무작정 프로그램을 다시 설치하기보다 문제의 범위를 하나씩 확인하는 것이 중요하다는 것을 알게 되었다.

---

## 16. 라이브러리 설치 오류 경험

머신러닝과 데이터 분석을 진행하면서 여러 라이브러리가 설치되지 않아 오류가 발생한 경험이 있었다.

예:

```text
ModuleNotFoundError: No module named 'skopt'
ModuleNotFoundError: No module named 'statsmodels'
ModuleNotFoundError: No module named 'gensim'
ModuleNotFoundError: No module named 'medmnist'
```

필요한 경우 해당 환경에 패키지를 설치하여 해결할 수 있다.

예:

```powershell
pip install scikit-optimize
pip install statsmodels
pip install gensim
pip install medmnist
```

중요한 것은 패키지를 설치할 때도 **현재 어떤 Python 환경에 설치하고 있는지 확인하는 것**이다.

---

## 17. 개발 환경 문제를 확인하는 순서

Python 코드가 실행되지 않거나 라이브러리를 찾지 못한다면 다음 순서로 확인할 수 있다.

```text
1. 현재 터미널 위치 확인
        ↓
2. Conda 환경 확인
        ↓
3. Python 버전 확인
        ↓
4. Python 실행 확인
        ↓
5. 필요한 패키지 설치 여부 확인
        ↓
6. VS Code Interpreter 확인
        ↓
7. Jupyter Kernel 확인
```

예:

```powershell
pwd
conda env list
python --version
where python
pip list
jupyter kernelspec list
```

---

## 18. 배운 점

Python 개발에서는 코드 자체뿐만 아니라 어떤 Python 환경에서 코드가 실행되고 있는지 이해하는 것이 중요하다는 것을 학습했다.

특히 Conda 가상환경을 사용하면 프로젝트별로 Python과 라이브러리를 분리하여 관리할 수 있다.

또한 `ModuleNotFoundError`가 발생했다고 해서 항상 라이브러리가 설치되지 않은 것은 아니며, 다른 Python 환경이나 Jupyter Kernel을 사용하고 있을 가능성도 확인해야 한다.

개발 환경에서 문제가 발생하면 무작정 다시 설치하기보다 Python → 가상환경 → 패키지 → Interpreter → Kernel 순서로 하나씩 확인하는 것이 중요하다.