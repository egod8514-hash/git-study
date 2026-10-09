# Python 조건문과 반복문

## 1. 프로그램의 흐름

지금까지 작성한 코드는 기본적으로 위에서 아래로 순서대로 실행된다.

```python
print("1번")
print("2번")
print("3번")
```

결과:

```text
1번
2번
3번
```

하지만 실제 프로그램에서는 항상 모든 코드를 똑같이 실행하는 것은 아니다.

예를 들어:

```text
로그인 성공 → 메인 화면
로그인 실패 → 오류 메시지

성인 → 성인 서비스
미성년자 → 접근 제한

모델 성능 향상 → 모델 저장
모델 성능 하락 → 저장하지 않음
```

처럼 상황에 따라 다른 코드를 실행해야 한다.

이때 사용하는 것이 **조건문**이다.

그리고 같은 작업을 여러 번 반복해야 할 때 사용하는 것이 **반복문**이다.

---

# 2. if 조건문

`if`는 특정 조건이 참일 때 코드를 실행한다.

기본 구조:

```python
if 조건:
    실행할 코드
```

예:

```python
age = 30

if age >= 20:
    print("성인입니다.")
```

결과:

```text
성인입니다.
```

`age >= 20`이 `True`이기 때문에 `print()`가 실행된다.

---

# 3. 조건이 False인 경우

```python
age = 15

if age >= 20:
    print("성인입니다.")
```

이번에는:

```python
age >= 20
```

의 결과가 `False`이다.

따라서 `print()`는 실행되지 않는다.

---

# 4. 들여쓰기

Python에서는 들여쓰기가 매우 중요하다.

```python
if age >= 20:
    print("성인입니다.")
```

`print()` 앞에 들여쓰기가 있다.

이 들여쓰기를 통해 Python은:

```text
이 코드는 if 안에 포함되어 있다.
```

라고 판단한다.

잘못된 예:

```python
if age >= 20:
print("성인입니다.")
```

이렇게 작성하면 오류가 발생한다.

---

# 5. if / else

조건이 참일 때와 거짓일 때 서로 다른 코드를 실행할 수 있다.

```python
age = 18

if age >= 20:
    print("성인입니다.")
else:
    print("미성년자입니다.")
```

결과:

```text
미성년자입니다.
```

흐름:

```text
age >= 20 ?
    │
 ┌──┴──┐
True  False
 │      │
성인   미성년자
```

---

# 6. elif

조건이 여러 개라면 `elif`를 사용할 수 있다.

```python
score = 85

if score >= 90:
    print("A")
elif score >= 80:
    print("B")
elif score >= 70:
    print("C")
else:
    print("D")
```

결과:

```text
B
```

Python은 위에서부터 조건을 확인한다.

```text
score >= 90 → False

score >= 80 → True
                 ↓
               "B"
```

조건이 만족되면 이후 조건은 확인하지 않는다.

---

# 7. 비교 연산자와 조건문

조건문에서는 비교 연산자를 많이 사용한다.

```text
==   같다
!=   다르다
>    크다
<    작다
>=   크거나 같다
<=   작거나 같다
```

예:

```python
temperature = 38.0

if temperature >= 37.5:
    print("체온이 높습니다.")
```

---

# 8. 논리 연산자

여러 조건을 동시에 검사할 수도 있다.

대표적으로:

```text
and
or
not
```

을 사용한다.

---

# 9. and

`and`는 양쪽 조건이 모두 참이어야 한다.

```python
age = 30
temperature = 36.5

if age >= 20 and temperature < 37.5:
    print("조건을 모두 만족합니다.")
```

개념적으로:

```text
조건 A = True
조건 B = True

A and B
   ↓
 True
```

이다.

---

# 10. or

`or`은 둘 중 하나만 참이어도 전체 조건이 참이다.

```python
sbp = 150
dbp = 80

if sbp >= 140 or dbp >= 90:
    print("혈압 확인이 필요합니다.")
```

여기서는 `sbp >= 140`이 참이므로 조건문이 실행된다.

---

# 11. not

`not`은 참과 거짓을 반대로 만든다.

```python
is_smoker = False

if not is_smoker:
    print("비흡연자입니다.")
```

`is_smoker`가 `False`이므로:

```text
not False
→ True
```

가 된다.

---

# 12. 조건문과 데이터 분석

데이터 분석에서도 조건은 매우 많이 사용된다.

예를 들어 Pandas에서:

```python
cond = df['성별'] == 1
```

이라고 작성하면:

```text
성별 값이 1인가?
```

를 각 행마다 검사한다.

그리고:

```python
df_man = df.loc[cond]
```

처럼 조건을 만족하는 데이터만 선택할 수 있다.

즉 Python에서 배운:

```python
age == 30
```

같은 비교 개념이 Pandas에서도 그대로 사용된다.

---

# 13. 반복문

같은 작업을 여러 번 실행해야 할 때 반복문을 사용한다.

예를 들어:

```python
print("Hello")
print("Hello")
print("Hello")
print("Hello")
print("Hello")
```

라고 작성할 수도 있지만 반복문을 사용하면 더 간단하다.

```python
for i in range(5):
    print("Hello")
```

결과:

```text
Hello
Hello
Hello
Hello
Hello
```

---

# 14. for 반복문

기본 구조:

```python
for 변수 in 반복가능한데이터:
    실행할 코드
```

예:

```python
numbers = [10, 20, 30]

for number in numbers:
    print(number)
```

결과:

```text
10
20
30
```

실행 과정은:

```text
첫 번째 반복
number = 10

두 번째 반복
number = 20

세 번째 반복
number = 30
```

이다.

---

# 15. range()

`range()`는 일정 범위의 숫자를 만들 때 많이 사용한다.

```python
for i in range(5):
    print(i)
```

결과:

```text
0
1
2
3
4
```

중요한 점은:

```python
range(5)
```

가:

```text
0, 1, 2, 3, 4
```

라는 것이다.

5는 포함되지 않는다.

---

# 16. range(start, stop)

시작 값을 지정할 수도 있다.

```python
for i in range(1, 5):
    print(i)
```

결과:

```text
1
2
3
4
```

즉:

```text
start는 포함
stop은 포함하지 않음
```

이다.

---

# 17. range(start, stop, step)

증가하는 간격도 설정할 수 있다.

```python
for i in range(0, 10, 2):
    print(i)
```

결과:

```text
0
2
4
6
8
```

여기서 `2`는 한 번 반복할 때마다 2씩 증가하라는 의미이다.

---

# 18. 리스트와 for

리스트의 데이터를 하나씩 가져올 수 있다.

```python
models = [
    "LogisticRegression",
    "RandomForest",
    "XGBoost"
]

for model in models:
    print(model)
```

결과:

```text
LogisticRegression
RandomForest
XGBoost
```

이 방식은 나중에 여러 머신러닝 모델을 반복해서 학습하거나 비교할 때도 사용할 수 있다.

---

# 19. enumerate()

반복하면서 순서 번호도 같이 필요할 수 있다.

```python
models = [
    "RandomForest",
    "XGBoost",
    "LightGBM"
]

for i, model in enumerate(models):
    print(i, model)
```

결과:

```text
0 RandomForest
1 XGBoost
2 LightGBM
```

1부터 시작하고 싶다면:

```python
for i, model in enumerate(models, 1):
    print(i, model)
```

결과:

```text
1 RandomForest
2 XGBoost
3 LightGBM
```

---

# 20. while 반복문

`while`은 조건이 참인 동안 계속 반복한다.

기본 구조:

```python
while 조건:
    실행할 코드
```

예:

```python
count = 0

while count < 5:
    print(count)
    count += 1
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

# 21. +=

다음 코드는:

```python
count += 1
```

다음과 같은 의미이다.

```python
count = count + 1
```

현재 `count` 값에 1을 더해서 다시 `count`에 저장한다.

비슷하게:

```text
+=
-=
*=
/=
```

등도 사용할 수 있다.

---

# 22. 무한 반복 주의

다음 코드를 보면:

```python
count = 0

while count < 5:
    print(count)
```

`count`의 값이 계속 0이다.

따라서:

```text
0 < 5
```

는 계속 `True`가 된다.

결과적으로 반복문이 끝나지 않는다.

이를 **무한 루프(Infinite Loop)**라고 한다.

따라서 `while`을 사용할 때는 조건이 언젠가는 `False`가 되도록 만들어야 한다.

---

# 23. break

반복문을 강제로 종료할 때 `break`를 사용할 수 있다.

```python
for i in range(10):

    if i == 5:
        break

    print(i)
```

결과:

```text
0
1
2
3
4
```

`i == 5`가 되면 반복문 자체가 종료된다.

---

# 24. continue

`continue`는 현재 반복만 건너뛰고 다음 반복으로 넘어간다.

```python
for i in range(5):

    if i == 2:
        continue

    print(i)
```

결과:

```text
0
1
3
4
```

2일 때만 `print()`를 건너뛴다.

---

# 25. 중첩 반복문

반복문 안에 반복문을 사용할 수도 있다.

```python
for i in range(3):

    for j in range(2):
        print(i, j)
```

결과:

```text
0 0
0 1
1 0
1 1
2 0
2 1
```

바깥 반복이 한 번 실행될 때 안쪽 반복이 모두 실행된다.

---

# 26. 머신러닝과 반복문

머신러닝에서 여러 Fold를 반복할 때도 `for`를 사용한다.

예:

```python
for fold, (train_idx, val_idx) in enumerate(
    skf.split(X, y), 1
):
    print(f"Fold {fold}")
```

처음 보면 복잡해 보이지만 핵심은:

```python
for ...
```

이다.

즉:

```text
Fold 1
Fold 2
Fold 3
Fold 4
Fold 5
```

처럼 교차검증을 반복해서 수행하는 것이다.

---

# 27. KFold와 반복문

5-Fold Cross Validation을 단순화하면 다음과 같이 생각할 수 있다.

```text
for Fold 1:
    모델 학습
    모델 평가

for Fold 2:
    모델 학습
    모델 평가

for Fold 3:
    모델 학습
    모델 평가

for Fold 4:
    모델 학습
    모델 평가

for Fold 5:
    모델 학습
    모델 평가
```

실제로 같은 코드를 다섯 번 작성하지 않고 반복문을 사용한다.

```python
for fold, (train_idx, val_idx) in enumerate(
    skf.split(X, y), 1
):
    # 학습 데이터
    # 검증 데이터
    # 모델 학습
    # 모델 평가
```

---

# 28. 딥러닝과 반복문

PyTorch에서 모델을 여러 Epoch 동안 학습할 때도 반복문을 사용한다.

예:

```python
epochs = 10

for epoch in range(epochs):
    print(epoch)
```

`epochs = 10`이라면:

```text
0
1
2
3
4
5
6
7
8
9
```

총 10번 반복된다.

실제 딥러닝에서는 이 반복 안에서 모델 학습을 수행한다.

```python
for epoch in range(epochs):

    # 모델 학습
    # Loss 계산
    # 역전파
    # 모델 평가
```

---

# 29. Epoch 번호를 1부터 출력하기

실제 학습 결과에서는 보통:

```text
Epoch 1
Epoch 2
Epoch 3
```

처럼 표시하고 싶다.

따라서:

```python
epochs = 3

for epoch in range(epochs):
    print(f"Epoch {epoch + 1}")
```

결과:

```text
Epoch 1
Epoch 2
Epoch 3
```

이 된다.

---

# 30. 우리가 사용했던 딥러닝 코드 이해하기

실제 학습 코드에서 다음과 같은 형태를 볼 수 있다.

```python
for i in range(epochs):

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

처음에는 복잡해 보이지만 구조를 단순화하면:

```python
for i in range(epochs):
    학습
    검증
```

이다.

즉 Python의 기본 `for` 반복문 위에 딥러닝 코드가 들어간 것이다.

---

# 31. 반복문과 조건문 같이 사용하기

실제 프로그램에서는 반복문 안에서 조건문을 함께 사용하는 경우가 많다.

예:

```python
scores = [70, 85, 40, 95]

for score in scores:

    if score >= 80:
        print(score, "합격")
    else:
        print(score, "불합격")
```

결과:

```text
70 불합격
85 합격
40 불합격
95 합격
```

---

# 32. 모델 성능 저장 예시

딥러닝에서는 검증 성능이 좋아졌을 때만 모델을 저장하는 방식도 사용할 수 있다.

개념적인 예:

```python
best_score = 0

for epoch in range(10):

    score = 0.9

    if score > best_score:
        best_score = score
        print("Best model")
```

여기에는:

```text
for → Epoch 반복
if  → 성능 비교
```

가 동시에 사용된다.

---

# 33. 조건문과 반복문을 읽는 방법

복잡한 코드를 볼 때 한 번에 모든 내용을 이해하려고 하지 않고 구조부터 확인한다.

예:

```python
for epoch in range(epochs):

    if val_loss < best_loss:
        best_loss = val_loss
```

먼저:

```text
for
→ 반복하는구나
```

그다음:

```text
if
→ 조건을 확인하는구나
```

마지막으로 변수의 의미를 확인한다.

```text
epoch
→ 학습 회차

val_loss
→ 검증 손실

best_loss
→ 지금까지 가장 좋은 손실
```

이런 식으로 읽으면 머신러닝과 딥러닝 코드도 이해하기 쉬워진다.

---

# 34. 자주 발생하는 실수

## 들여쓰기 오류

잘못된 코드:

```python
if age >= 20:
print("성인")
```

올바른 코드:

```python
if age >= 20:
    print("성인")
```

---

## =와 == 혼동

값 저장:

```python
age = 30
```

비교:

```python
age == 30
```

---

## range 마지막 값

```python
range(5)
```

는:

```text
0 1 2 3 4
```

이다.

5까지가 아니다.

---

## while 무한 반복

```python
while count < 5:
    print(count)
```

처럼 `count`가 변하지 않으면 반복이 끝나지 않을 수 있다.

---

# 35. 전체 흐름 정리

조건문:

```text
조건 확인
   ↓
True / False
   ↓
실행할 코드 결정
```

반복문:

```text
반복할 데이터
   ↓
하나씩 가져오기
   ↓
코드 실행
   ↓
다음 데이터
```

둘을 같이 사용하면:

```text
반복
 ↓
각 데이터 확인
 ↓
조건 판단
 ↓
조건에 맞는 작업 실행
```

이 가능하다.

---

# 36. 배운 점

조건문은 상황에 따라 서로 다른 코드를 실행하기 위해 사용한다.

대표적으로:

```python
if
elif
else
```

를 사용한다.

반복문은 같은 작업을 여러 번 실행하기 위해 사용하며:

```python
for
while
```

를 사용할 수 있다.

처음에는 단순하게:

```python
for i in range(5):
    print(i)
```

와 같이 사용하지만 머신러닝에서는:

```text
Fold 반복
```

딥러닝에서는:

```text
Epoch 반복
Batch 반복
```

과 같이 사용된다.

따라서 머신러닝이나 딥러닝 코드에 등장하는 복잡한 반복문도 결국 Python에서 배운 `for`문의 확장이라는 것을 이해했다.

특히 복잡한 코드를 볼 때:

```text
for → 무엇을 반복하는가?

if → 무엇을 판단하는가?

변수 → 어떤 값을 가지고 있는가?
```

순서로 확인하면 코드의 전체 구조를 이해하기 쉬워진다.