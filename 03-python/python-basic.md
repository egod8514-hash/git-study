# Python 기초

## 1. Python이란?

Python은 문법이 비교적 간단하고 다양한 분야에서 사용할 수 있는 프로그래밍 언어이다.

Python은 다음과 같은 분야에서 많이 사용된다.

```text
웹 개발
데이터 분석
자동화
머신러닝
딥러닝
인공지능
컴퓨터 비전
API 개발
```

내가 이후에 학습한 Pandas, Scikit-learn, PyTorch 등도 모두 Python을 기반으로 사용한다.

---

# 2. Python 코드 실행

가장 간단한 Python 코드는 다음과 같다.

```python
print("Hello Python")
```

실행 결과:

```text
Hello Python
```

`print()`는 값을 화면에 출력하는 함수이다.

---

# 3. 주석

코드에 설명을 작성하고 싶을 때 주석을 사용할 수 있다.

Python에서는 `#`을 사용한다.

```python
# 화면에 Hello를 출력한다.
print("Hello")
```

`#` 뒤의 내용은 Python이 실행하지 않는다.

예:

```python
# 환자의 나이
age = 30
```

주석은 코드가 어떤 역할을 하는지 설명할 때 사용할 수 있다.

---

# 4. 변수

변수는 데이터를 저장하기 위한 이름이다.

```python
age = 30
```

여기서:

```text
age → 변수 이름
30  → 저장된 값
=   → 값을 변수에 저장
```

라고 이해할 수 있다.

주의할 점은 프로그래밍에서 `=`은 수학의 "같다"와 조금 다르다는 것이다.

```python
age = 30
```

은:

> 30이라는 값을 age라는 변수에 저장한다.

라는 의미이다.

---

# 5. 변수 사용

변수에 값을 저장하면 나중에 다시 사용할 수 있다.

```python
age = 30

print(age)
```

결과:

```text
30
```

변수의 값을 변경할 수도 있다.

```python
age = 30
age = 31

print(age)
```

결과:

```text
31
```

마지막으로 저장된 값이 사용된다.

---

# 6. 여러 변수

여러 데이터를 각각 변수에 저장할 수 있다.

```python
name = "Kim"
age = 30
height = 175.5

print(name)
print(age)
print(height)
```

예를 들어 의료 데이터에서도 비슷하게 생각할 수 있다.

```python
age = 65
sbp = 130
dbp = 80
heart_rate = 90
```

각 변수에 환자의 정보를 저장한 것이다.

---

# 7. 변수 이름 작성

변수 이름은 데이터의 의미를 알 수 있도록 작성하는 것이 좋다.

좋은 예:

```python
age = 30
height = 175
weight = 70
blood_pressure = 120
```

의미를 이해하기 어려운 예:

```python
a = 30
b = 175
c = 70
d = 120
```

간단한 연습에서는 `a`, `b`를 사용할 수 있지만 실제 프로젝트에서는 의미 있는 이름을 사용하는 것이 코드를 이해하기 쉽다.

---

# 8. 자료형

Python에서 데이터는 종류에 따라 자료형(Data Type)을 가진다.

대표적인 자료형은 다음과 같다.

```text
int     정수
float   실수
str     문자열
bool    참/거짓
```

---

# 9. int

`int`는 정수형이다.

```python
age = 30
count = 100
year = 2026
```

소수점이 없는 숫자이다.

```python
print(type(age))
```

결과:

```text
<class 'int'>
```

---

# 10. float

`float`는 실수형이다.

```python
height = 175.5
weight = 72.3
temperature = 36.5
```

소수점이 있는 숫자를 표현할 수 있다.

```python
print(type(height))
```

결과:

```text
<class 'float'>
```

데이터 분석에서는 매우 자주 볼 수 있는 자료형이다.

---

# 11. str

`str`은 문자열(String)이다.

문자를 저장할 때 사용한다.

```python
name = "Kim"
gender = "Male"
diagnosis = "Pneumonia"
```

문자열은 따옴표를 사용한다.

```python
"Hello"
'Hello'
```

둘 다 문자열이다.

```python
print(type(name))
```

결과:

```text
<class 'str'>
```

---

# 12. bool

`bool`은 참(True) 또는 거짓(False)을 나타낸다.

```python
is_student = True
is_smoker = False
```

값은:

```text
True
False
```

두 가지이다.

조건문에서 자주 사용된다.

---

# 13. type()

변수의 자료형을 확인할 때 `type()`을 사용할 수 있다.

```python
age = 30
height = 175.5
name = "Kim"
is_student = True

print(type(age))
print(type(height))
print(type(name))
print(type(is_student))
```

결과:

```text
<class 'int'>
<class 'float'>
<class 'str'>
<class 'bool'>
```

데이터를 다룰 때 자료형을 확인하는 것은 매우 중요하다.

---

# 14. 기본 연산

Python으로 숫자 계산을 할 수 있다.

```python
a = 10
b = 3
```

더하기:

```python
print(a + b)
```

결과:

```text
13
```

빼기:

```python
print(a - b)
```

결과:

```text
7
```

곱하기:

```python
print(a * b)
```

결과:

```text
30
```

나누기:

```python
print(a / b)
```

결과:

```text
3.3333333333333335
```

---

# 15. 주요 산술 연산자

```text
+   더하기
-   빼기
*   곱하기
/   나누기
//  몫
%   나머지
**  거듭제곱
```

예:

```python
print(10 // 3)
print(10 % 3)
print(2 ** 3)
```

결과:

```text
3
1
8
```

---

# 16. 문자열 연결

문자열도 `+`를 사용할 수 있다.

```python
first_name = "Python"
last_name = "Study"

result = first_name + " " + last_name

print(result)
```

결과:

```text
Python Study
```

숫자의 `+`와 문자열의 `+`는 동작이 다르다는 것을 알 수 있다.

---

# 17. 형변환

데이터의 자료형을 변경하는 것을 형변환(Type Casting)이라고 한다.

대표적인 함수:

```text
int()
float()
str()
bool()
```

예:

```python
age = "30"

print(type(age))
```

현재는 문자열이다.

```python
age = int(age)

print(type(age))
```

이제 정수형으로 변경된다.

---

# 18. 문자열과 숫자의 차이

다음 두 값은 다르다.

```python
a = 10
b = "10"
```

`a`는 숫자이고 `b`는 문자열이다.

```python
print(type(a))
print(type(b))
```

결과:

```text
<class 'int'>
<class 'str'>
```

따라서 다음처럼 바로 더할 수 없다.

```python
a + b
```

자료형을 맞춰야 한다.

```python
a + int(b)
```

결과:

```text
20
```

---

# 19. input()

사용자에게 값을 입력받을 때 `input()`을 사용할 수 있다.

```python
name = input("이름을 입력하세요: ")

print(name)
```

사용자가:

```text
Kim
```

을 입력하면 `name` 변수에 `"Kim"`이 저장된다.

---

# 20. input()의 주의점

`input()`으로 입력받은 값은 기본적으로 문자열이다.

```python
age = input("나이를 입력하세요: ")

print(type(age))
```

30을 입력해도:

```text
<class 'str'>
```

이 된다.

숫자로 사용하려면:

```python
age = int(input("나이를 입력하세요: "))
```

처럼 형변환할 수 있다.

---

# 21. f-string

변수와 문자열을 함께 출력할 때 f-string을 사용하면 편리하다.

```python
name = "Kim"
age = 30

print(f"이름은 {name}이고 나이는 {age}살입니다.")
```

결과:

```text
이름은 Kim이고 나이는 30살입니다.
```

`{}` 안에 변수를 넣으면 변수의 값이 출력된다.

---

# 22. 비교 연산자

두 값을 비교할 수 있다.

```text
==   같다
!=   같지 않다
>    크다
<    작다
>=   크거나 같다
<=   작거나 같다
```

예:

```python
age = 30

print(age > 20)
print(age == 30)
print(age < 20)
```

결과:

```text
True
True
False
```

---

# 23. = 과 == 의 차이

처음 Python을 배울 때 특히 중요한 부분이다.

### `=`

값을 저장한다.

```python
age = 30
```

### `==`

두 값이 같은지 비교한다.

```python
age == 30
```

결과:

```text
True
```

따라서:

```text
=   대입
==  비교
```

로 구분할 수 있다.

---

# 24. 논리 연산자

여러 조건을 함께 사용할 때 논리 연산자를 사용할 수 있다.

```text
and
or
not
```

예:

```python
age = 30

print(age >= 20 and age <= 40)
```

두 조건이 모두 참이면:

```text
True
```

가 된다.

---

# 25. None

`None`은 값이 없음을 표현할 때 사용한다.

```python
result = None
```

예를 들어 아직 결과가 계산되지 않은 상태를 나타낼 수 있다.

```python
prediction = None
```

이후 계산 결과가 만들어지면:

```python
prediction = 1
```

처럼 값을 넣을 수 있다.

실제 머신러닝 코드에서도 `None`을 자주 볼 수 있다.

---

# 26. Python 오류 메시지

Python을 공부하면서 오류 메시지를 읽는 것도 중요하다.

예를 들어:

```python
print(age)
```

를 실행했는데 `age` 변수를 만든 적이 없다면:

```text
NameError
```

가 발생할 수 있다.

즉:

```text
NameError
→ 해당 이름의 변수나 함수 등을 Python이 찾지 못함
```

이라는 의미이다.

---

# 27. 실제로 경험했던 NameError

데이터 분석 과정에서 다음과 같은 코드를 실행했을 때:

```python
print(train.shape)
```

`train` 데이터를 먼저 만들지 않았다면:

```text
NameError: name 'train' is not defined
```

오류가 발생한다.

이 경우 단순히 마지막 코드만 보는 것이 아니라:

```python
train = pd.read_csv(...)
```

처럼 `train`을 만드는 코드가 먼저 실행되었는지 확인해야 한다.

Jupyter Notebook이나 Colab에서는 셀 실행 순서 때문에 이런 문제가 발생할 수 있다.

---

# 28. Python 기초와 데이터 분석의 연결

Python 기초 문법은 이후 데이터 분석 코드에서도 그대로 사용된다.

예를 들어:

```python
RANDOM_STATE = 42
```

도 변수이다.

```python
train.shape
```

에서 `train` 역시 DataFrame을 저장한 변수이다.

```python
train['Age'] > 30
```

에서는 비교 연산자를 사용한다.

```python
print(train.shape)
```

에서는 함수를 사용한다.

즉 복잡해 보이는 데이터 분석 코드도 결국 Python의 기본 문법을 조합해서 작성한 것이다.

---

# 29. Python 기초와 머신러닝의 연결

머신러닝 코드에서도 마찬가지이다.

예:

```python
model = RandomForestClassifier()
```

`model`이라는 변수에 RandomForest 모델 객체를 저장한 것이다.

그리고:

```python
model.fit(X_train, y_train)
```

으로 학습한다.

예측:

```python
pred = model.predict(X_test)
```

여기에서도:

```text
model
X_train
y_train
pred
```

등은 모두 Python 변수이다.

따라서 Python 기초를 이해하면 머신러닝 코드도 훨씬 읽기 쉬워진다.

---

# 30. Python 기초와 딥러닝의 연결

PyTorch에서도 Python 문법을 그대로 사용한다.

예:

```python
epochs = 10
```

학습 횟수를 변수에 저장한다.

```python
device = "cuda"
```

문자열을 변수에 저장한다.

그리고 이후 반복문에서:

```python
for epoch in range(epochs):
    ...
```

처럼 사용한다.

결국 딥러닝 코드 역시 Python의:

```text
변수
자료형
조건문
반복문
함수
클래스
라이브러리
```

를 조합하여 작성한다.

---

# 31. 자주 확인할 것

Python 코드에서 문제가 발생하면 먼저 다음을 확인한다.

```text
1. 변수 이름이 정확한가?
2. 변수를 먼저 만들었는가?
3. 셀을 실행했는가?
4. 자료형이 맞는가?
5. 따옴표와 괄호가 맞는가?
6. 들여쓰기가 맞는가?
7. 필요한 라이브러리를 import 했는가?
```

오류 메시지의 마지막 부분도 중요하다.

예:

```text
NameError
ModuleNotFoundError
FileNotFoundError
TypeError
ValueError
```

오류 종류를 확인하면 문제의 원인을 찾기 쉬워진다.

---

# 32. 배운 점

Python에서는 변수에 데이터를 저장하고 각 데이터는 `int`, `float`, `str`, `bool` 등의 자료형을 가진다.

```python
age = 30
height = 175.5
name = "Kim"
is_student = True
```

변수와 자료형은 Python의 가장 기본적인 개념이지만 이후 데이터 분석, 머신러닝, 딥러닝에서도 계속 사용된다.

특히:

```text
=
```

은 값을 저장하는 대입 연산자이고:

```text
==
```

은 값이 같은지 확인하는 비교 연산자라는 차이를 이해했다.

또한 Python에서 오류가 발생했을 때 무조건 코드를 다시 작성하기보다 오류 종류와 메시지를 확인하는 습관이 중요하다는 것을 학습했다.

Python 기초는 이후 학습할:

```text
조건문
반복문
자료구조
함수
Pandas
머신러닝
PyTorch
```

의 기반이 된다.