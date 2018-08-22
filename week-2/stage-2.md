# Stage 2 - 함수에 대해 알아보자

이번 스테이지에서는 데이터를 편집, 가공하는 데에 필수적인 연산과 함수에 대해 배워봅니다.

실제 데이터 수집을 할 때에도 굉장히 많이 사용되는 부분이니 잘 따라오며 이해해주세요!

먼저 새로운 스테이지가 되면 학습 내용이 뒤섞이지 않도록 새 파일을 만드시는 것을 권장합니다.

## 기본 연산자

숫자 값 끼리는 일반적인 사칙 연산이 가능합니다.

```python
print(1 + 2)
print(13 * 24)
print(3000 / 60 * 12)

secondsOfDay = 60 * 60 * 24
print(secondsOfDay)
```

숫자 대신 변수를 사용할 수도 있습니다. 변수를 사용할 때에는 언제나 변수명이 아닌 변수에 담긴 값이 대신 사용됩니다.

```python
product_name = '29인치 모니터'
price = 160000
company = '미타전자'
available = True

quantity = 24
discount = 0.1

total_price = price * quantity * (1 - discount)

print(total_price)
```

문자열도 연산이 가능할까요?

```python
name = "코알라"
cls = " 데이터 수집 스터디"

print("안녕" + name)  # 안녕코알라
print(name + cls)    # 코알 데이터 수집 스터디
print(name * 3)      # 코알라코알라코알라
print("ㅎ" * 10)     # ㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎ
```

문자열끼리 더하면 이어붙인 새로운 문자열이 만들어지고

문자열과 숫자를 곱하면 숫자만큼 반복된 새로운 문자열이 만들어집니다.

## 함수란 무엇일까?

> [https://github.com/coalastudy/python-datascrapping-code/blob/master/week2/stage2-0-function.py](https://github.com/coalastudy/python-datascrapping-code/blob/master/week2/stage2-0-function.py)

정해진 일련의 명령을 손쉽게 반복할 수 있도록 저장하는 **매크로**같은 역할입니다.

여러분은 print\(\) 함수를 사용할 때 어떤 생각을 하나요? 입력된 텍스트가 거치는 과정과 사용되는 기술들을 속속들이 알고있으신가요?

아닙니다. 하지만 여러분은 print\(\) 함수의 괄호 안에 변수나 값을 넣으면 콘솔 창에 출력된다는 사실만 알고도 print\(\) 함수를 잘 사용합니다.

이것이 함수의 강력한 장점입니다. 구현된 내용을 몰라도 어떤 값을 입력하면 어떤 결과가 나오는 지만 알면 사용할 수 있습니다.

```python
num = abs(-110)
print(num)

length_of_text = len('welcome to coala study!')
print(length_of_text)

print(max(15, 363, 166, 2, 97, 218))
```

abs, len, max 등은 print와 마찬가지로 python에 기본적으로 내장되어 언제든지 사용 가능한 함수입니다.

함수는 함수명\(입력값1, 입력값2...\) 의 형태로 호출되며, 실행이 종료된 후에는 결과값으로 바뀝니다.

Stage1 에서 다뤘던 문자열 타입을 기억하시나요?

문자열은 데이터 수집을 하며 가장 많이 다루는 데이터 타입입니다.

데이터 수집 뿐만 아니라 모든 프로그램에서 유용하게 쓰이는만큼 문자열을 편집하는 함수들도 잘 준비되어 있습니다.

그런데 이 함수들은 호출 방법이 약간 달라 보입니다.

단독으로 호출하는 것이 아니라, 문자열 변수 또는 값에 .을 찍어 실행합니다.

문자열 편집에 특화된 함수이기 때문에 문자열 변수에서만 호출할 수 있습니다.

