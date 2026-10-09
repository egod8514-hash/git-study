# Python 함수

## 1. 함수란?

함수(Function)는 특정 작업을 수행하는 코드를 하나로 묶어 이름을 붙인 것이다.

예를 들어 같은 계산을 여러 번 해야 한다고 생각해보자.

```python
a = 10
b = 20
result = a + b
print(result)
```

다른 숫자를 계산할 때 또 작성해야 한다.

```python
a = 30
b = 40
result = a + b
print(result)
```

함수를 만들면 반복되는 코드를 하나로 묶을 수 있다.

```python
def add(a, b):
    result = a + b
    return result
```

사용:

```python
print(add(10, 20))
print(add(30, 40))
```

결과:

```text
30
70
```

---

# 2. 함수를 사용하는 이유

함수를 사용하는 대표적인 이유는 다음과 같다.

```text
코드 중복 감소
코드 재사용
기능별 코드 분리
코드 이해가 쉬워짐
수정과 유지보수가 쉬워짐
```

예를 들어 머신러닝 프로젝트에서도:

```text
데이터 전처리
모델 학습
모델 평가
예측
결과 출력
```

을 각각 함수로 분리할 수 있다.

---

# 3. 함수 만들기 - def

Python에서는 `def`를 이용하여 함수를 만든다.

기본 구조:

```python
def 함수이름():
    실행할 코드
```

예:

```python
def hello():
    print("Hello Python")
```

여기까지는 함수를 정의한 것이다.

아직 실행된 것은 아니다.

---

# 4. 함수 호출

만든 함수를 실행하려면 함수 이름 뒤에 `()`를 붙인다.

```python
def hello():
    print("Hello Python")

hello()
```

결과:

```text
Hello Python
```

즉:

```text
def hello()
→ 함수 정의

hello()
→ 함수 호출
```

이다.

---

# 5. 함수는 여러 번 호출할 수 있다

```python
def hello():
    print("Hello")

hello()
hello()
hello()
```

결과:

```text
Hello
Hello
Hello
```

함수를 한 번 만들어두면 필요한 곳에서 여러 번 사용할 수 있다.

---

# 6. 매개변수

함수에 데이터를 전달할 수도 있다.

```python
def hello(name):
    print(f"Hello {name}")
```

여기서:

```text
name
```

은 **매개변수(Parameter)**이다.

함수 내부에서 사용할 값을 전달받는 변수이다.

---

# 7. 인자

함수를 사용할 때 실제로 전달하는 값을 인자(Argument)라고 한다.

```python
hello("Kim")
```

여기서:

```text
"Kim"
```

이 인자이다.

정리하면:

```python
def hello(name):
    print(name)
```

에서:

```text
name
→ 매개변수 Parameter
```

그리고:

```python
hello("Kim")
```

에서:

```text
"Kim"
→ 인자 Argument
```

이다.

---

# 8. 여러 개의 매개변수

함수는 여러 값을 전달받을 수도 있다.

```python
def add(a, b):
    print(a + b)

add(10, 20)
```

결과:

```text
30
```

여기서:

```text
a ← 10
b ← 20
```

이 된다.

---

# 9. return

함수에서 계산한 결과를 함수 밖으로 전달하려면 `return`을 사용한다.

```python
def add(a, b):
    result = a + b
    return result
```

사용:

```python
answer = add(10, 20)

print(answer)
```

결과:

```text
30
```

흐름:

```text
add(10, 20)
     ↓
a = 10
b = 20
     ↓
result = 30
     ↓
return 30
     ↓
answer = 30
```

---

# 10. print와 return의 차이

처음 함수를 배울 때 많이 혼동하는 부분이다.

다음 함수는:

```python
def add(a, b):
    print(a + b)
```

결과를 화면에 보여준다.

반면:

```python
def add(a, b):
    return a + b
```

는 계산 결과를 함수 밖으로 돌려준다.

따라서:

```python
result = add(10, 20)
```

처럼 결과를 다른 변수에 저장할 수 있다.

---

# 11. return 결과 재사용

```python
def add(a, b):
    return a + b

result = add(10, 20)

final_result = result * 2

print(final_result)
```

결과:

```text
60
```

`return`으로 받은 값을 다른 계산에 다시 사용할 수 있다.

이 때문에 실제 프로그램에서는 `return`이 매우 중요하다.

---

# 12. 여러 값을 return

Python에서는 여러 값을 반환할 수도 있다.

```python
def calculate(a, b):

    add_result = a + b
    sub_result = a - b

    return add_result, sub_result
```

사용:

```python
x, y = calculate(10, 5)

print(x)
print(y)
```

결과:

```text
15
5
```

---

# 13. 기본값을 가진 매개변수

매개변수에 기본값을 설정할 수도 있다.

```python
def hello(name="Python"):
    print(f"Hello {name}")
```

인자를 전달하지 않으면:

```python
hello()
```

결과:

```text
Hello Python
```

인자를 전달하면:

```python
hello("Kim")
```

결과:

```text
Hello Kim
```

이 된다.

---

# 14. 키워드 인자

매개변수 이름을 직접 지정하여 값을 전달할 수도 있다.

```python
def patient(name, age):
    print(name)
    print(age)

patient(name="Kim", age=30)
```

이 방식을 사용하면 어떤 값이 어떤 매개변수로 전달되는지 이해하기 쉬워진다.

---

# 15. 함수 내부 변수

함수 내부에서 만든 변수는 일반적으로 함수 내부에서 사용된다.

```python
def test():
    result = 100
    print(result)

test()
```

함수 안에서는 `result`를 사용할 수 있다.

이처럼 변수가 사용할 수 있는 범위를 **Scope**라고 한다.

---

# 16. 지역 변수

함수 내부에서 만들어진 변수를 지역 변수(Local Variable)라고 한다.

```python
def calculate():
    result = 100
    return result
```

`result`는 `calculate()` 함수 안에서 만들어진 지역 변수이다.

---

# 17. 전역 변수

함수 밖에서 만든 변수는 전역 변수(Global Variable)라고 한다.

```python
score = 100

def show_score():
    print(score)

show_score()
```

결과:

```text
100
```

함수에서 외부 변수를 사용할 수 있는 경우도 있지만 함수가 너무 많은 전역 변수에 의존하면 코드 관리가 어려워질 수 있다.

필요한 값을 매개변수로 전달하는 방식이 코드를 이해하기 쉬운 경우가 많다.

---

# 18. 함수 이름

함수 이름도 어떤 작업을 하는지 알 수 있도록 만드는 것이 좋다.

예:

```python
calculate_bmi()
train_model()
evaluate_model()
load_data()
preprocess_data()
```

다음처럼 의미를 알기 어려운 이름보다:

```python
a()
test1()
func()
```

역할을 알 수 있는 이름이 좋다.

---

# 19. BMI 계산 함수 예제

Python 기초와 실제 데이터 분석을 연결해보자.

BMI 계산식:

```text
BMI = 체중(kg) / 신장(m)^2
```

함수:

```python
def calculate_bmi(weight, height_cm):

    height_m = height_cm / 100

    bmi = weight / (height_m ** 2)

    return bmi
```

사용:

```python
result = calculate_bmi(70, 175)

print(result)
```

함수 하나를 만들어두면 여러 사람에게 다시 사용할 수 있다.

```python
bmi1 = calculate_bmi(70, 175)
bmi2 = calculate_bmi(80, 180)
bmi3 = calculate_bmi(55, 160)
```

---

# 20. 함수와 조건문

함수 내부에서 조건문도 사용할 수 있다.

```python
def check_age(age):

    if age >= 20:
        return "성인"

    else:
        return "미성년자"
```

사용:

```python
result = check_age(30)

print(result)
```

결과:

```text
성인
```

즉 함수 안에서도 지금까지 배운:

```text
변수
조건문
반복문
```

을 모두 사용할 수 있다.

---

# 21. 함수와 반복문

함수 내부에서 반복문도 사용할 수 있다.

```python
def print_numbers(n):

    for i in range(n):
        print(i)
```

사용:

```python
print_numbers(5)
```

결과:

```text
0
1
2
3
4
```

---

# 22. 라이브러리에서 사용하는 함수

우리가 이미 사용했던 Python 코드에는 많은 함수가 들어 있다.

예:

```python
print()
```

```python
type()
```

```python
len()
```

```python
range()
```

이것들도 함수이다.

Pandas에서는:

```python
pd.read_csv()
```

를 사용했다.

Scikit-learn에서는:

```python
train_test_split()
```

을 사용했다.

즉 우리가 직접 함수를 만들 수도 있고 다른 개발자가 만들어 놓은 함수를 사용할 수도 있다.

---

# 23. 객체의 메서드

다음과 같은 코드도 자주 사용했다.

```python
model.fit(X_train, y_train)
```

```python
model.predict(X_test)
```

여기서:

```text
fit()
predict()
```

은 함수와 비슷하지만 특정 객체에 소속되어 있다.

이런 것을 **메서드(Method)**라고 한다.

예:

```text
model.fit()
```

은:

```text
model 객체가 가지고 있는 fit 메서드
```

라고 이해할 수 있다.

---

# 24. 함수와 메서드의 간단한 차이

함수:

```python
print("Hello")
```

```python
len(data)
```

객체의 메서드:

```python
model.fit()
```

```python
model.predict()
```

```python
df.head()
```

형태를 보면:

```text
함수
함수이름()

메서드
객체.메서드()
```

라고 구분해볼 수 있다.

---

# 25. 머신러닝에서 함수가 필요한 이유

머신러닝 프로젝트는 여러 단계로 구성된다.

```text
데이터 로드
    ↓
전처리
    ↓
데이터 분할
    ↓
모델 학습
    ↓
예측
    ↓
평가
```

모든 코드를 한 곳에 작성할 수도 있지만 프로젝트가 커질수록 이해하기 어려워진다.

따라서:

```python
load_data()
preprocess_data()
train_model()
evaluate_model()
```

처럼 역할별로 함수를 만들 수 있다.

---

# 26. 딥러닝의 train 함수

딥러닝을 학습하면서 다음과 비슷한 형태를 사용했다.

```python
def train(
    train_loader,
    model,
    loss_fn,
    optimizer,
    device
):
    # 학습 코드

    return train_loss
```

처음 보면 복잡해 보이지만 구조는 Python 기초에서 배운 것과 같다.

```text
def
→ 함수 생성

train
→ 함수 이름

train_loader
model
loss_fn
optimizer
device
→ 매개변수

return train_loss
→ 학습 결과 반환
```

---

# 27. train 함수 호출

함수를 만들었다면 다음처럼 사용할 수 있다.

```python
tr_loss = train(
    train_loader,
    model,
    loss_fn,
    optimizer,
    device
)
```

흐름:

```text
train_loader ─┐
model        ─┤
loss_fn      ─┤
optimizer    ─┤
device       ─┘
      ↓
   train()
      ↓
모델 학습
      ↓
return train_loss
      ↓
tr_loss
```

이다.

---

# 28. evaluate 함수

검증 작업도 함수로 분리할 수 있다.

```python
def evaluate(
    val_loader,
    model,
    loss_fn,
    device
):
    # 검증 코드

    return val_loss
```

사용:

```python
val_loss = evaluate(
    val_loader,
    model,
    loss_fn,
    device
)
```

따라서:

```text
train()
→ 학습 담당

evaluate()
→ 평가 담당
```

처럼 역할을 나눌 수 있다.

---

# 29. 반복문과 함수의 결합

딥러닝에서는 다음과 같은 구조를 사용할 수 있다.

```python
for epoch in range(epochs):

    tr_loss = train(
        train_loader,
        model,
        loss_fn,
        optimizer,
        device
    )

    val_loss = evaluate(
        val_loader,
        model,
        loss_fn,
        device
    )
```

이 코드를 구조만 보면:

```text
Epoch 반복
    ↓
train() 호출
    ↓
evaluate() 호출
    ↓
다음 Epoch
```

이다.

즉 복잡한 딥러닝 코드도:

```text
for 반복문
+
함수
```

를 조합해서 만든 것이다.

---

# 30. 머신러닝 Cross Validation 함수

머신러닝 프로젝트에서는 다음처럼 여러 작업을 하나의 함수로 묶을 수도 있다.

```python
def run_cv(X, y):

    # Cross Validation 설정

    # 모델 생성

    # Fold 반복

    # 모델 학습

    # 모델 평가

    return scores
```

이렇게 만들어두면:

```python
scores = run_cv(X, y)
```

처럼 사용할 수 있다.

---

# 31. 실제 사용했던 run_final_cv 형태 이해

프로젝트에서 다음처럼 긴 함수도 사용할 수 있다.

```python
def run_final_cv(
    X_raw,
    X_enc,
    X_test_raw,
    X_test_enc,
    y
):
    # 교차검증
    # 모델 학습
    # 예측
    # 결과 저장
```

이것도 특별한 문법이 아니다.

기본 구조는:

```python
def 함수이름(매개변수):
    실행할 코드
```

와 같다.

단지 함수 안에서 수행하는 작업이 많아진 것이다.

---

# 32. 함수가 없으면 어떻게 될까?

예를 들어 5번의 실험을 진행하면서 매번:

```text
전처리 코드
학습 코드
평가 코드
출력 코드
```

를 복사해서 사용하면 코드가 매우 길어질 수 있다.

그리고 한 부분을 수정해야 할 때 여러 곳을 모두 수정해야 할 수도 있다.

함수로 만들면:

```python
train_model()
```

의 내부 코드만 수정하면 이를 사용하는 곳에 동일한 변경사항을 적용할 수 있다.

---

# 33. 함수를 읽는 순서

처음 보는 함수가 길다면 다음 순서로 확인하면 이해하기 쉽다.

예:

```python
def train(
    train_loader,
    model,
    loss_fn,
    optimizer,
    device
):
```

먼저:

```text
1. 함수 이름은 무엇인가?
→ train
```

그다음:

```text
2. 무엇을 전달받는가?
→ train_loader
→ model
→ loss_fn
→ optimizer
→ device
```

그리고:

```text
3. 함수 안에서 무엇을 하는가?
→ 모델 학습
```

마지막:

```text
4. 무엇을 return 하는가?
→ train_loss
```

이 순서로 읽는다.

---

# 34. 함수를 직접 만들 때 생각할 것

함수를 만들기 전에:

```text
이 함수가 무슨 일을 해야 하는가?
```

를 먼저 생각한다.

예:

```text
데이터를 불러오는 함수
→ load_data()

BMI를 계산하는 함수
→ calculate_bmi()

모델을 학습하는 함수
→ train_model()

모델을 평가하는 함수
→ evaluate_model()
```

하나의 함수가 너무 많은 역할을 담당하지 않도록 나누는 것도 중요하다.

---

# 35. 자주 발생하는 오류

## 함수를 만들기 전에 호출

```python
hello()

def hello():
    print("Hello")
```

위에서부터 실행되는 일반적인 상황에서는 `hello`가 아직 정의되지 않았기 때문에 오류가 발생할 수 있다.

먼저 함수를 정의한다.

```python
def hello():
    print("Hello")

hello()
```

---

## return을 저장하지 않음

```python
def add(a, b):
    return a + b

add(10, 20)
```

함수는 실행되지만 결과를 이후에 사용하려면:

```python
result = add(10, 20)
```

처럼 저장할 수 있다.

---

## 매개변수 개수 불일치

```python
def add(a, b):
    return a + b

add(10)
```

`b`에 들어갈 값이 없기 때문에 오류가 발생한다.

올바른 예:

```python
add(10, 20)
```

---

# 36. 전체 구조 정리

함수를 만드는 과정:

```text
반복하거나 분리할 작업 발견
        ↓
def로 함수 정의
        ↓
매개변수 설정
        ↓
함수 내부 코드 작성
        ↓
필요하면 return
        ↓
함수 호출
```

예:

```python
def add(a, b):

    result = a + b

    return result


answer = add(10, 20)
```

구조:

```text
10, 20
   ↓
add()
   ↓
계산
   ↓
return
   ↓
answer
```

---

# 37. Python 기초에서 AI 코드까지

처음에는:

```python
def add(a, b):
    return a + b
```

같은 간단한 함수를 사용한다.

데이터 분석에서는:

```python
def calculate_bmi(weight, height):
    ...
```

머신러닝에서는:

```python
def train_model(X_train, y_train):
    ...
```

딥러닝에서는:

```python
def train(
    train_loader,
    model,
    loss_fn,
    optimizer,
    device
):
    ...
```

처럼 발전한다.

하지만 기본 원리는 모두 같다.

```text
입력
 ↓
함수
 ↓
처리
 ↓
결과
```

이다.

---

# 38. 배운 점

함수는 반복되는 코드를 하나로 묶어 재사용할 수 있도록 만들어주는 Python의 중요한 기능이다.

기본 구조는:

```python
def 함수이름(매개변수):
    실행할 코드
    return 결과
```

이다.

함수를 호출하면서 전달하는 실제 값을 인자라고 하며 함수에서 전달받는 변수를 매개변수라고 한다.

```text
Parameter
→ 함수가 받을 변수

Argument
→ 실제 전달하는 값
```

또한 `print()`와 `return`은 다르다.

```text
print
→ 화면에 출력

return
→ 함수의 결과를 함수 밖으로 전달
```

처음에는 단순한 계산 함수로 시작하지만 데이터 분석, 머신러닝, 딥러닝에서는 반복되는 학습과 평가 코드를 함수로 분리하여 관리한다.

따라서 이전에 사용했던:

```python
train()
evaluate()
run_final_cv()
```

같은 코드도 특별한 AI 문법이 아니라 Python의 기본 함수 개념을 실제 AI 프로젝트에 적용한 것이라는 것을 이해했다.