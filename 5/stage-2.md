# Stage 2 - 가상브라우저를 조종해보자

이번 스테이지에서는 파이썬의 selenium 라이브러리와 크롬 드라이버를 이용하여 브라우저를 컨트롤하는 방법을 알아봅니다. 이를 통해 데이터 수집 뿐만 아니라 로그인, 메일 보내기 등 많은 작업을 자동화할 수 있습니다.



## 설치 및 준비

![](../.gitbook/assets/image%20%2832%29.png)

[http://chromedriver.chromium.org/downloads](http://chromedriver.chromium.org/downloads
) 에 접속하여 크롬드라이버 최신 버전들 다운로드합니다. 크롬드라이버는 인터넷 브라우저 크롬과 같은 기능을 갖는 가상 브라우저입니다.

![](../.gitbook/assets/image%20%2888%29.png)

다운로드한 크롬드라이버를 위와 같이 실행할 파이썬 파일이 있는 폴더에 넣어야 합니다.

다음으로 파이참에서 selenium 라이브러리를 검색하여 설치해주세요.

라이브러리 설치 방법이 기억나지 않으신다면 2주차의 Stage 3을 참고해주세요.

{% page-ref page="../week-2/stage-3-html.md" %}



준비가 끝났으니 시작해볼까요?

```python
from selenium import webdriver

driver = webdriver.Chrome('./chromedriver')
```

selenium 라이브러리에서 webdriver 클래스를 불러와 아까 다운로드 받은 크롬드라이버를 변수로 만들고 있습니다.

![](../.gitbook/assets/image%20%2847%29.png)

코드를 실행시키면 위와 같이 빈 브라우저가 나타나게 됩니다. 앞으로 코드를 통해 이 브라우저를 조종하게 될 것입니다.



## selenium 사용법

![](../.gitbook/assets/image%20%28200%29.png)

브라우저의 거의 모든 것을 실행할 수 있는 방대한 라이브러리이지만, 일단 위에 나온 함수 정도만 알면 현재 필요한만큼은 무리없이 사용할 수 있습니다.

위에서 webdriver.Chrome 함수의 결과를 저장한 driver 변수를 "드라이버 변수"라고 부르겠습니다. 이 드라이버 변수가 웹 브라우저 하나에 대응된다고 생각하면 됩니다.



### 1. get\( \) 함수

```python
driver.get('https://map.naver.com/')
```

이제 위와 같이 드라이버 변수에서 .get\( \) 함수를 호출하면 매개변수로 전달된 주소로 이동합니다.

![](../.gitbook/assets/image%20%28195%29.png)



### 2. find\_element\_by\_~~\( \) 함수

BeautifulSoup의 select\_one\( \) 에 대응하는 함수입니다. 드라이버가 위치하는 웹 페이지 내에서 해당하는 요소를 찾아 되돌려줍니다.

select\_one\( \)함수의 경우 매개변수로 CSS 선택자가 들어갑니다. '.post', 'span.title', 'div\#container &gt; a' 등이 그것입니다. 하지만 find\_element\_by\_~ 함수의 경우 선택의 폭이 훨씬 넓습니다.

| find\_element\_by\_id\( \) | ID를 기준으로 HTML 요소를 찾는다. |
| --- | --- | --- | --- |
| find\_element\_by\_class\_name\( \) | 클래스를 기준으로 HTML 요소를 찾는다. |
| find\_element\_by\_css\_selector\( \) | CSS 선택자를 기준으로 HTML 요소를 찾는다. |
| find\_element\_by\_xpath\( \) | XPath를 기준으로 HTML 요소를 찾는다. |

ID, 클래스 등으로 선택자를 한정할 수 있습니다. 이 때는 .이나 \#이 필요 없습니다.

즉, find\_element\_by\_css\_selector\('\#container'\) 는 find\_element\_by\_id\('container'\) 와 같은 뜻이 됩니다.

그리고 xpath라는 처음보는 단어가 나오는데, Stage 3에서 다룰 예정입니다.



### 3. find\_elements\_by\_~~\( \) 함수

BeautifulSoup의 select\( \) 에 대응하는 함수입니다. 위와 마찬가지로 ID, 클래스, CSS 선택자, XPath 등을 사용할 수 있으며 문서 내에 해당하는 모든 HTML 요소가 배열로 반환됩니다.



### 4. send\_keys\( \) 함수

BeautifulSoup과 달리, selenium은 실제 브라우저와 연결되어있기 때문에 선택된 HTML 요소와 상호작용을 할 수 있습니다. input 등 텍스트를 입력할 수 있는 HTML 요소를 선택하고 이 send\_keys\( \) 함수를 사용하면 매개변수로 전달된 텍스트를 input에 입력합니다.

```python
from selenium import webdriver

driver = webdriver.Chrome('./chromedriver')
driver.get('https://map.naver.com/')

driver.find_element_by_id('search-input').send_keys('신촌 스터디룸')
```

위 코드는 드라이버 변수를 생성하여 https://map.naver.com 으로 이동한 후 find\_element\_by\_id\( \) 함수를 이용해 검색창을 선택하고 '신촌 스터디룸' 이라는 검색어를 입력하는 코드입니다.

![](../.gitbook/assets/image%20%2859%29.png)

실행시키면 이와 같은 결과를 볼 수 있습니다.



### 5. click\( \) 함수

말 그대로 HTML 요소를 클릭하는 함수입니다. 검색어를 입력했으니 검색 버튼을 눌러야겠죠?

```python
driver.find_element_by_id('search-input').send_keys('신촌 스터디룸')
driver.find_element_by_css_selector('#header > div.sch > fieldset > button').click()
```

검색창을 선택하여 검색어를 입력하고, find\_element\_by\_css\_selector\( \) 함수를 이용해 검색 버튼을 선택하여 클릭하는 코드입니다.

{% hint style="warning" %}
send\_keys\( \), click\( \) 등 HTML 요소와 상호작용하는 함수는 반드시 **단일 HTML 요소**에 대해 실행되어야 합니다. 즉, find\_element**s**\_by\_~~ 를 사용하여 얻어진 배열에 위 함수를 호출하면 에러가 발생합니다. 단수와 복수가 필요할 때를 각각 잘 구분해주세요!

설계 상 find\_element**s**\_by\_~~ 함수를 사용하는 것이 효율적일 경우, for - in 반복문이나 \[0\] 등 배열 요소 선택을 통해 배열의 단일 요소에 접근한 후 함수를 호출해야 합니다.
{% endhint %}

### 

### 6. page\_source

page\_source는 함수는 아닙니다. 단순히 driver가 위치한 웹페이지의 소스 코드를 얻을 수 있는 기능입니다. 이는 requests 등을 사용한 정적 수집에서 최초로 획득하는 raw html 코드와 같습니다.

```python
html = driver.page_source
soup = BeautifulSoup(html, 'html.parser')
```

따라서 위와 같은 코드를 사용하면 동적 페이지 수집 도중에도 소스 코드를 정적 텍스트로 바꾸어 BeautifulSoup을 사용할 수 있습니다.

find\_element\_by\_~~\( \) 함수가 select\_one \( \) 으로 바뀌는 차이 외에도, 정적 텍스트 분석을 사용하는 BeautifulSoup이 실제 브라우저를 사용하는 selenium보다 훨씬 빠르다는 차이점이 있습니다.

따라서 복잡한 페이지 접근에만 동적 수집을 사용하고, 실제 데이터를 가져오는 부분에서는 정적 수집을 사용하는 것도 좋은 방법입니다.



## 동적으로 데이터 수집하기

이제 브라우저를 조종하여 네이버 지도에 접속하고 "신촌 스터디룸" 이라는 키워드까지 검색해 보았으니, 검색했을 때 나오는 데이터를 수집할 차례입니다.

첫번째 방법은 끝까지 selenium을 사용하여 수집하는 것입니다.

```python
 list = driver.find_elements_by_class_name('lsnx_det')
 
 for data in list:
     print(data.find_element_by_tag_name('a').text)
```

먼저 드라이버 변수에서 find\_element**s**\_by\_class \(**복수형 검색에 유의해 주세요!**\) 을 사용하여 모든 장소 리스트를 배열로 받아옵니다.

다음으로 리스트를 for 문으로 반복하며 그 안에서 스터디룸 이름을 담고 있는 HTML 요소를 선택하여 .text 로 텍스트를 수집, 출력합니다.

수집 과정 자체는 정적 수집과 차이가 없다는 것을 알 수 있습니다.

![](../.gitbook/assets/image%20%2881%29.png)

이 경우 위와 같이 실제 브라우저의 리스트 요소에서 a태그의 텍스트를 선택하여 출력하게 됩니다.



두번째 방법은 원하는 데이터를 가진 페이지가 나오면 정적 수집으로 전환하는 것입니다.

위의 selenium 사용법 중 6번 page\_source에서 현 웹페이지의 전체 소스 코드를 HTML 요소로 만들어 soup이라는 변수에 저장했습니다. 어디서 많이 본 것 같지 않나요?

이 부분은 정적 수집과  완전히 동일합니다.

```python
list = soup.select('.lsnx_det')

for data in list:
     title = data.select_one('a').text
```

BeautifulSoup의 select\(\)와 select\_one\(\)을 사용하여 페이지 소스에서 수집하고 있습니다.

위 두 방법은 장소 리스트를 배열로 수집하여, 요소 하나하나에서 장소명을 추출하는 과정은 완전히 똑같습니다. 

하지만 첫번째 방법은 실제 브라우저의 요소 하나하나에 직접 접근하고 있고, 두번째 방법은 일단 원하는 데이터가 페이지에 나오면 페이지의 전체 소스를 복사하여 BeautifulSoup 데이터로 만든 후 처리합니다.

차이점을 명확히 설명드리기엔 아직 어려운 내용입니다. 단순히 설명하면 파이썬의 외부 프로그램이라고 볼 수 있는 브라우저와 지속적으로 통신하며 연속된 리스트에 접근하는 것보다는, 처음 한번만 데이터를 받아온 후 파이썬의 자체 데이터로 만들어 처리하는 것이 훨씬 빠를 것입니다.

따라서 속도를 생각할 때, 원하는 데이터에 접근이 완료되었다면 정적 수집으로 전환하는 것이 좋습니다.

