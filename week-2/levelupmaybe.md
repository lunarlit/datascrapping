
## 함수란 무엇일까?

> [https://github.com/coalastudy/python-datascrapping-code/blob/master/week2/stage2-0-function.py](https://github.com/coalastudy/python-datascrapping-code/blob/master/week2/stage2-0-function.py)

정해진 일련의 명령을 손쉽게 반복할 수 있도록 저장하는 **매크로**같은 역할입니다.

여러분이 카페의 커피 머신을 작동시키는 프로그램을 만든다고 가정해봅시다.

아메리카노를 만드는 과정은 다음과 같습니다.

1. 에스프레소 샷 추출
2. 얼음 넣기
3. 물 붓기
4. 에스프레소 붓기

![](../.gitbook/assets/image%20%28135%29.png)

이를 표현하기 위해 다음과 같은 4줄의 프로그램이 필요합니다.

```python
print("에스프레소 샷 추출")
print("얼음 넣기")
print("물 붓기")
print("에스프레소 붓기")
```



그런데 아메리카노를 만드는 과정은 언제나 똑같습니다. 주문이 들어올 때마다 이 4줄의 명령을 내려야 할까요? 이를 함수로 만들어봅시다.

```python
def makeAmericano():
    print("에스프레소 샷 추출")
    print("얼음 넣기")
    print("물 붓기")
    print("에스프레소 붓기")
```

함수 정의는 def 로 시작하여 함수 이름을 입력하고 \(\): 까지 쓴 후 아래줄부터 내용을 작성합니다.

makeAmericano 라는 함수가 정의되었습니다.  
이제 5잔의 아메리카노를 만드려면 이를 5번 실행하면 됩니다.

![](../.gitbook/assets/image%20%2826%29.png)

```python
makeAmericano()
makeAmericano()
makeAmericano()
makeAmericano()
makeAmericano()
```

실행은 함수이름에 \(\)를 붙이면 됩니다.

단순 출력을 했을 때는 20줄이 필요했을 코드가 5줄로 줄어들었습니다.  
\(물론 함수 정의를 포함하면 10줄이지만, 의미가 중복되는 코드가 많이 줄어들었죠.\)



또 함수를 만들면 과정을 수정하고 싶을 때도 매우 유리합니다.

마지막에 "스틱으로 저어주기" 과정을 넣고 싶다면 단순 출력으로는 주문이 들어올 때마다

```python
print("스틱으로 저어주기")
```

를 추가해야 하지만, 함수를 사용했을 때는

```python
def makeAmericano():
    print("에스프레소 샷 추출")
    print("얼음 넣기")
    print("물 붓기")
    print("에스프레소 붓기")
    print("스틱으로 저어주기")
```

이렇게 함수 정의에 한 줄을 추가해주면, 그대로 모든 아메리카노 제조 과정에 반영이 되니까요.



## 함수의 입력값

> [https://github.com/coalastudy/python-datascrapping-code/blob/master/week2/stage2-1-input.py](https://github.com/coalastudy/python-datascrapping-code/blob/master/week2/stage2-1-input.py)

다음으로 드립 커피를 만드는 함수를 추가하려고 합니다.  
그런데 드립커피를 만드는 과정은 아메리카노처럼 항상 일정하지 않습니다.  
원두를 선택할 수가 있기 때문입니다.

1. 원두 선택 \( 과테말라 / 콜롬비아 / 칠레 \)
2. 원두 투입
3. 드리핑

![](../.gitbook/assets/image%20%2817%29.png)



그렇다면 원두마다 함수를 따로 만들어야 할까요? 

```python
def makeGuatemalaDripCoffee():
    print("과테말라 원두 투입")
    print("드리핑")
    
def makeColombiaDripCoffee():
    print("콜롬비아 원두 투입")
    print("드리핑")

def makeChileDripCoffee():
    print("칠레 원두 투입")
    print("드리핑")
    
makeGuatemalaDripCoffee()
makeColombiaDripCoffee()
makeChileDripCoffee()
```

이것은 너무 비효율적인 것 같습니다.  
원두 종류 빼고는 모두 중복되는 함수를 세 개나 만들어야 하니까요.



함수의 입력값을 이용하면, 외부에서 실행 조건을 변경시킬 수가 있습니다.  
이는 Stage 1에서 배운 **"변수"**가 있기에 가능합니다.

```python
def makeDripCoffee(bean):
    print(bean, " 원두 투입")
    print("드리핑")
```

{% hint style="info" %}
print\( \) 함수 안에 쉼표로 구분하여 여러 개의 값을 넣으면 차례로 출력됩니다.
{% endhint %}

이제 원두를 선택하고 makeDripCoffee 함수에 넣어주면 알아서 원두를 투입하고 커피를 만듭니다.

```python
makeDripCoffee("과테말라")
makeDripCoffee("콜롬비아")
makeDripCoffee("칠레")
```

![](../.gitbook/assets/image%20%2814%29.png)

이 입력값은 **"매개 변수"**라고 부르며 함수 안에서 변수와 비슷한 역할을 합니다.  
함수 호출 시 bean이라는 변수 자리에 "과테말라"라는 값을 넣으면, 실제 함수 안에서는 bean을 사용할 때마다 사실은 bean 변수 안에 들어있는 "과테말라"라는 값을 사용하는 것이죠.

입력값은 함수 이름 뒤에 오는 \(\) 사이에 집어넣어주면 됩니다. 우리가 여태 사용하던 print\(\) 안에 들어가는 값도 매개변수 였다는 것을 깨달으셨죠?



입력값을 이용하면 코드가 훨씬 유연해집니다.  
원두마다 함수를 따로 만들면 매장에 원두가 추가될 때마다 함수를 하나씩 더 만들어야 하지만, makeDripCoffee 함수를 이용하면 원두만 바꿔서 넣어주면 되거든요.

수정할 때의 유연성도 더 말씀드릴 필요가 없겠죠?



## 함수의 결과값

> [https://github.com/coalastudy/python-datascrapping-code/blob/master/week2/stage2-2-output.py](https://github.com/coalastudy/python-datascrapping-code/blob/master/week2/stage2-2-output.py)

![](../.gitbook/assets/image%20%2823%29.png)

이런 식으로 여러 개의 메뉴를 만들었습니다. 이제 음료가 완성된 것을 손님에게 알리려고 합니다. 알림은 이런 식입니다.

주문하신 "아메리카노" 나왔습니다!

주문하신 "콜롬비아 드립커피" 나왔습니다!

주문하신 "카페라떼" 나왔습니다!

그렇다면 이 출력을 각 함수에 넣어야 할까요?

```python
def makeAmericano():
    print("에스프레소 샷 추출")
    # 중간 생략...
    print("주문하신 아메리카노 나왔습니다!")

def makeCaffeLatte():
    # ...
    print("주문하신 카페라떼 나왔습니다!")
```

이렇게 해도 작동은 하겠지만 별로 좋은 코드는 아닙니다.

먼저 make 함수는 커피를 만드는 함수인데, 출력을 담당하는 것이 부자연스럽습니다.   
이는 코드의 가독성과 유지보수성을 떨어트립니다.   
실제로 규모있는 카페에 가면 카운터 직원과 바리스타가 명확히 구분 되어있죠. 

다음으로는 역시 유연성이 떨어집니다.   
멘트 앞에 "고객님! "을 붙이고 싶어지면, 함수의 개수만큼 수정할 일이 생깁니다.

잘하면 음료 이름을 변수로 사용하는 하나의 명령으로 처리가 가능할 것 같은데요...



```python
def makeAmericano():
    print("에스프레소 샷 추출")
    # ... 생략 ...
    print("에스프레소 붓기")
    return "아메리카노"
```

맨 마지막 줄의 return "아메리카노" 는 함수가 전부 실행된 이후 결과를 돌려주는 역할을 합니다. 결과를 돌려준다 함은, 함수를 호출한 자리에 결과값이 실제로 나타난다는 뜻입니다.

```python
result = makeAmericano()
```

그래서 위 코드를 실행하면 makeAmericano\(\) 함수가 실행된 후 "아메리카노" 라는 값이 됩니다.

```python
result = "아메리카노"
```

그 결과 변수 result에는 "카페라떼"라는 텍스트가 입력됩니다.

```python
print("주문하신", result, "나왔습니다!")
```

결과값은 이렇게 출력할 수 있겠죠?



```python
print("주문하신", makeAmericano(), "나왔습니다!")
```

결과값의 흐름을 잘 이해하면 위와 같이 작성할 수도 있을 것입니다.

