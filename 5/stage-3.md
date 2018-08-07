# Stage 3 - 가상브라우저 고급 활용법

이번 스테이지에서는 가상브라우저의 여러 요소들과 더 복잡하게 상호작용하는 법을 알아보고 여러 페이지의 동적 데이터를 수집해볼 것입니다.

또한 새로운 선택자인 XPath를 사용하여 좀 더 다양한 상황에서 사용할 수 있는 도구를 익히며 페이지 반복을 적절히 종료하는 법에 대해서도 알아볼 것입니다.



## 기존 방식으로 페이지 넘기기

![](../.gitbook/assets/image%20%2822%29.png)

네이버 지도 하단의 페이지 버튼은 이렇게 생겼습니다. 그리고 그 HTML 소스코드는 다음과 같습니다.

```markup
<div class="paginate loaded">
	<span class="pre">이전</span>
	<strong class="first-child">1</strong>
	<a href="#">2</a>
	<a href="#">3</a>
	<a href="#">4</a>
	<a href="#" class="last-child">5</a>
	<a href="#" title="다음" class="next">다음</a>
</div>
```

이전 버튼은 첫 5페이지까지는 누를 수 없습니다. 따라서 a가 아닌 span태그로 되어있습니다.

1 페이지 버튼 역시 현재 페이지이기 때문에 a가 아니라 누를 수 없는 strong이라는 태그로 되어있습니다.

나머지 누를 수 있는 페이지 버튼과 다음 5개의 페이지를 로딩하는 다음 버튼은 a 태그로 되어있습니다.

이런 구조에서 페이지 버튼을 순서대로 누르며 데이터를 수집하고, 5페이지가 끝나면 다음 버튼을 누를 수 있게 하려면 어떻게 설계해야 할까요?

여러가지 방법이 있겠지만 다음과 같은 방법을 택해보겠습니다.

![](../.gitbook/assets/image%20%28201%29.png)

div.paginate 의 자식 태그들에 순서대로 번호를 매겨보면 위와 같습니다. 그러면 5페이지마다 어떤 규칙이 반복되는 것을 알 수 있습니다.

![](../.gitbook/assets/image%20%2887%29.png)

현재 n페이지에 있다면, div.paginate의 자식 태그들 중 n%5+1번째 것을 누르면 됩니다. \(n%5는 n/5의 나머지입니다.\) 단, 5의 배수의 페이지일 때는 n%5+1 = 1이 아니라 6을 눌러야 하기 때문에 예외로 처리해야 합니다. 

```python
page = 0

while True:
    page = page + 1
    html = driver.page_source
    #페이지에서 데이터 수집 생략...
    
    index = page % 5 + 1
    
    if index == 1:
        index = 6
    
    driver.find_element_by_class_name('paginate').find_elements_by_css_selector('*')[index].click()
```

따라서 연속으로 페이지를 넘기는 코드는 위와 같습니다. 현재 페이지를 page 변수에 저장하고 있으며, index라는 변수에 page % 5 + 1을 계산합니다. 

index가 1이 되는 경우는 페이지가 5의 배수가 될 때밖에 없으므로 index를 6으로 만들어 "다음" 버튼을 누를 수 있게 합니다.

index를 구했다면 드라이버 변수에서 .paginate 를 찾은 후, 모든 자식 태그를 선택하는 \* 선택자를 이용해 .paginate의 모든 자식 태그를 배열로 받아옵니다.

그 배열에서 \[index\] 라는 접근자로 index번째의 자식요소를 선택하여 클릭합니다. 복잡하죠?



## XPath를 사용하여 페이지 넘기기

```markup
<div class="paginate loaded">
	<span class="pre">이전</span>
	<strong class="first-child">1</strong>
	<a href="#">2</a>
	<a href="#">3</a>
	<a href="#">4</a>
	<a href="#" class="last-child">5</a>
	<a href="#" title="다음" class="next">다음</a>
</div>
```

위와 같이 복잡한 방식으로 페이지를 넘길 수 밖에 없었던 이유는, 기존의 지식으로는 위 HTML 구조에서 2, 3, 4 등의 버튼을 구별할 수 없기 때문입니다. 태그가 같으며 ID와 클래스는 아예 없으니까요.

다른 것이라고는 내부 텍스트인 2, 3, 4 뿐입니다. 그런데 이걸로는 절대 구별이 안되는 걸까요?



```python
driver.find_element_by_xpath('//a[text()=2]')
```

아닙니다. XPath라는 선택자를 이용하여 위와 같은 명령을 하면 내부 텍스트가 2인 a 태그를 선택할 수 있습니다.

좀 더 복잡하지만 강력한 선택 기준을 제공하는 XPath에 대해 조금만 알아보겠습니다.



### 1. Xpath의 기본 사용법

```text
Xpath = //태그명[@속성='값']
```

기본적으로 Xpath는 위와 같은 모양으로 만들어집니다. 태그명을 기본으로 추가적인 속성을 통해 검색할 수 있으며, 속성에는 a의 href나 img의 src 등 모든 속성이 포함될 뿐만 아니라 ID와 클래스까지 해당됩니다. 예시를 몇 가지 들어보겠습니다.

| **Xpath 예시** | **선택되는 요소** |
| :--- | :--- |
| //input\[@type='password'\] | type이 password인 input 선택 |
| //a\[@href='https://www.naver.com/'\] | href가 특정 주소인 a 선택 |
| //label\[@id='message23'\] | id가 message23인 label 선택 |
| //\*\[@class='article'\] | 클래스가 article인 모든 태그 선택 |

이처럼 태그 꺽쇠 &lt;&gt; 안에 들어가는 모든 속성을 검색 기준으로 활용 가능합니다.



### 2. Xpath의 고급 사용법

ID와 클래스 외의 속성을 검색할 수 있는 것도 큰 장점이지만, Xpath는 그 이상으로 훨씬 다양한 검색 도구들을 제공합니다.

| **도구** | **도구의 기능** | **사용 예시** | **선택되는 요소** |
| :--- | :--- | :--- | :--- |
| contains\(\) | 속성 중 일부가 일치 | a\[contains\(@href, 'naver'\)\] | href 중 naver라는 단어가 있는 a |
| text\( \) | 내부 문자열 검사 | td\[text\(\)='UserID'\] | 내부 문자열이 UserID인 td |

[https://www.guru99.com/xpath-selenium.html](https://www.guru99.com/xpath-selenium.html)   
더 많은 도구를 알아보려면 여기를 참고하세요.



이제 Xpath를 사용하면 복잡한 설계 없이 내부 텍스트를 이용해 페이지를 넘길 수 있습니다.

```python
page = 1

while True:
    page = page + 1

    if page % 5 == 1:
        driver.find_element_by_class_name('next').click()
    else:
        driver.find_element_by_xpath('//a[text()=' + str(page) + ']').click()
```

5, 10, 15 등 5의 배수 페이지에서는 클래스명을 사용해 "다음" 버튼을 누르고 그렇지 않다면 다음 페이지 번호를 내부 텍스트로 갖는 a를 선택하여 클릭하는 코드입니다.



## 오류 처리

이를 실행시키면 페이지를 넘기거나 다음 버튼을 누르는 작업을 잘 수행할 것입니다.

하지만 마지막엔 꼭 다음과 같은 에러를 만나게 됩니다.

![](../.gitbook/assets/image%20%28159%29.png)

NoSuchElementException. "그런 요소가 없는데요?" 라는 오류입니다.

그리고 또 보면 a\[text\(\)=5\] 라는 Xpath를 검색했을 때 생긴 오류임을 알 수 있습니다.

이는 "신촌 스터디룸"을 검색했을 때 나온 메세지입니다.

![](../.gitbook/assets/image%20%28187%29.png)

이 때 페이지는 4까지밖에 없는데, 5페이지를 누르라는 명령을 받아 오류가 난 것입니다.

그러면 프로그램에게 몇 페이지까지 수집하라고 명령해야 할까요? 

뉴스를 수집할 때에는 전체 기사 수를 통해 숫자를 정해주었지만 더 좋은 방법이 있습니다.



바로 "오류가 날 때까지" 수집하는 것입니다.

페이지가 더 없을 때 오류가 나므로, 반대로 생각하면 오류가 났을 때 페이지가 더 없다는 결론이 됩니다.

이제 남은 할 일은 프로그램이 예상치 못한 오류를 만나 경고 메세지를 내보내는 것이 아니라, 예상한 오류로 만들어 정상적으로 종료되도록 하는 것입니다.



### try - except

오류가 없는 완전무결한 프로그램을 만들면 좋겠지만 그것은 불가능합니다. 

프로그램의 기능이 많아지면 통제할 수 없는 외부 요인과 소통하기도 하고, 항상 원하는대로 움직이지만은 않는 일반 사용자의 명령을 받기 때문이죠.

프로그램은 예상치 못한 오류가 생기면 더 치명적인 피해를 방지하기 위해 즉시 종료하고 개발자에게 이를 알립니다.

하지만 오류가 생길 가능성이 있는 지점에 이를 표시하고, 적절한 오류 처리 방법을 마련해준다면 프로그램은 오류가 생겨도 멈추지 않고 작동합니다. 일종의 "비상대책 매뉴얼"인 셈이죠.



```python
try:
    오류가 생길 수 있는 기능 실행
except:
    오류가 생겼을 때 처리 방법 명시
```

try\(오류가 생길 수 있는 코드를 **시도**\)와 except\(오류가 생겼을 때 **예외처리**\)는 각각의 실행 범위를 갖습니다. 

try의 범위가 실행되는 도중 오류가 생기면 즉시 except의 실행 범위로 넘어갑니다.   
오류가 생기지 않는다면 except 범위는 아예 실행되지 않습니다.



```python
page = 1

while True:
    html = driver.page_source
    # 데이터 수집 생략...

    page = page + 1

    try:
        if page % 5 == 1:
            driver.find_element_by_class_name('next').click()
        else:
            driver.find_element_by_xpath('//a[text()=' + str(page) + ']').click()

    except:
        break
        
driver.close()
```

우리의 예제 코드에서는 새 페이지나 다음 버튼을 눌러야 할 때 오류가 생길 가능성이 있으므로 이를 try 범위 안에 넣습니다. 

오류를 처리하는 except 범위 안에는 break를 넣어 while 반복문이 종료되도록 합니다.

이제 실행 시 마지막 페이지까지 탐색 후 오류없이 종료하는 것을 볼 수 있습니다. 마지막에 driver.close\(\)까지 추가해주면 열려있던 브라우저가 자동으로 닫힙니다.

