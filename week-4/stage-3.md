# Stage 3 - 페이지 주소의 비밀을 알아보자

지금까지 한 페이지짜리 네이버 TV TOP100을 이용하여 데이터를 수집하고 원하는대로 가공하여 저장하는 것까지 멋지게 성공했습니다.

이제 페이지를 벗어나 여러 페이지에 걸쳐 데이터를 수집해볼 것입니다. 실제로 데이터 수집을 하다보면 수집할 데이터가 한 페이지에 모여있는 경우는 많지 않습니다.

먼저 페이지의 주소를 파헤쳐 여러 페이지를 쉽게 수집할 힌트를 찾아보겠습니다.

## 네이버시 뉴스구 아시안게임동...

이제 네이버 TV TOP 100 예제는 마무리 짓고 새로운 대상을 수집해 보겠습니다.

네이버 검색, 그 중에서도 뉴스 검색을 수집해 볼텐데요.

![](../.gitbook/assets/image%20%28174%29.png)

어떤 키워드로 뉴스를 검색해서 나오는 기사들을, 언론사별로 정리해보려고 합니다.

그러면 이 키워드에 대해 해당 언론사가 어느 정도 비중을 두고 보도하는지, 논조는 어떤지 알 수 있겠죠?

이때까지 배운 문법과 라이브러리 지식을 활용하면, 첫 페이지의 언론사와 제목을 수집하는 정도는 그리 어렵지 않을 거라고 생각합니다.



이번 스테이지는 그보다 페이지를 이동하는 방법에 초점을 맞출 것인데요.

![](../.gitbook/assets/image%20%28192%29.png)

이것은 네이버 뉴스에 "아시안게임" 이라는 키워드로 검색했을 때의 페이지 주소입니다. 약간 다를 수도 있지만, 다음에 나오는 요소들은 모두 포함하고 있을 것입니다.



![](../.gitbook/assets/image%20%2855%29.png)

주소를 잘 찾아보면 물음표가 딱 하나 있을 것입니다.

이 물음표를 기준으로 앞부분은 네이버 검색 페이지의 주소입니다. 네이버 검색을 사용하면 통합검색이든, 이미지 검색이든, 어떤 키워드를 검색하든 이 주소는 변하지 않습니다.



![](../.gitbook/assets/image%20%28227%29.png)

물음표의 뒷부분은 요청값을 전달하는 부분입니다. 쿼리스트링, 쿼리문자열이라고도 합니다. 이 값을 좀 더 뜯어보겠습니다.



![](../.gitbook/assets/image%20%2889%29.png)

잘 보면 어떤 규칙이 보이실 겁니다. 마치 변수에 값을 넣듯 a=b의 형태로 이루어져있고 각 요소들은 &로 이어져 있습니다.



![](../.gitbook/assets/image%20%28152%29.png)

영단어 뜻으로 추측하거나 검색 조건을 이리저리 바꾸면서 테스트 해봤을 때, 각 값은 다음과 같은 의미를 갖는 것 같습니다.

sm은 뭔지 잘 모르겠습니다. 심지어 없애버려도 결과가 달라지지 않습니다.

where는 검색 탭을 의미합니다. 이미지로 바꾸면 image, 실시간검색은 realtime 등으로 값이 바뀌는 것을 볼 수 있습니다.

query는 검색어입니다. 초록창에 검색어를 입력하지 않고도 이 부분을 바꿔 접속보면 해당 키워드로 검색되는 것을 볼 수 있습니다.

이처럼 웹페이지의 주소는 기존에 여러분이 알던 것 이상으로 많은 정보를 내포하고 있습니다.



## 페이지를 이동해보자

이제 뉴스 검색 결과에서 페이지를 이동하며 주소의 변화를 지켜봅시다.

페이지를 이동하면 주소의 요청값에 엄청나게 많은 값들이 붙는 것을 볼 수 있습니다.

하지만 대부분은 지금 필요한 것이 아니고, 하나만 확인하면 됩니다.



![](../.gitbook/assets/image%20%28130%29.png)

페이지를 옮길 때마다 start의 값이 변합니다. 그리고 일정한 규칙을 찾을 수 있는데 \(페이지 - 1\) \* 10 + 1의 값을 갖습니다.

이는 단어의 의미와 가진 정보로 추측해보건대 현재 페이지가 가지고 있는 기사가 전체 기사의 31번째에서 시작한다는 의미일 것입니다.

{% hint style="info" %}
이번 스테이지 들어 부쩍 문장을 추측형으로 끝내고 있다는 걸 눈치채셨나요?

사실 웹 데이터 수집은 규칙을 추측하는 싸움이라고 말할 수 있습니다. 

실제 개발자가 아닌 이상 이 규칙이 이것을 의미한다고 100% 확신할 수 없고, 대신 최대한 추측하여 수집에 역이용하는 것이죠.
{% endhint %}

그래서 이 값을 바꾸면서 접속해보면, 실제로 페이지에 나타나는 기사의 목록이 바뀝니다.

이제 발견한 규칙을 이용해 파이썬 상에서 여러 페이지에 연속적으로 접근해 보겠습니다.



그 전에 한 페이지의 기사 제목을 추출해보겠습니다. 이렇게 해서 잘 수집되는 것을 확인하지 않으면, 페이지를 잘 이동하고 있는 건지 확인할 수단이 없습니다.

새로운 파이썬 파일을 만듭니다.

```python
from urllib import request
from bs4 import BeautifulSoup
```

이번 예제에서는 페이지를 가져올 때 requests 라이브러리 대신 urllib의 request를 사용합니다. 

urllib은 파이썬 기본 라이브러리이므로 따로 설치하지 않으셔도 됩니다.

역할은 거의 같지만 네이버 검색 결과 수집에 requests를 사용했을 때는 불법 수집으로 간주되어 결과가 나오지 않습니다.

{% hint style="info" %}
이 역시 실제 개발자가 아니라면 정확히 원인을 알기는 힘듭니다. 데이터 수집을 하려면 맥가이버칼처럼 여러 도구를 준비해놓고 이것저것 꺼내어 시도해봐야할 때가 많습니다.
{% endhint %}



```python
raw = request.urlopen('https://search.naver.com/search.naver?&where=news&query=아시안게임&start=1')
html = BeautifulSoup(raw, 'html.parser')

list = html.select('.type01 dl')

for article in list:
    journal = article.select_one('span._sp_each_source').text
    title = article.select_one('dt').text

    print(journal, title)
```

이제 네이버 검색에서 탭은 뉴스, 키워드는 아시안게임, 시작 기사는 1로 하는 주소에 접근하여 데이터를 수집할 것입니다.

수집 요청은 기존의 requests.get이 아니라 request.urlopen으로 하고 있습니다. 이 request는 위에서 말한 urllib의 내부 클래스입니다. 

이 때는 requests를 사용했을 때와 달리 결과값의 text를 다시 뽑아내지 않아도 바로 BeautifulSoup을 이용해 html로 변환이 가능합니다.

그런데 이 코드를 실행하면 다음과 같은 에러가 발생합니다.

**UnicodeEncodeError: 'ascii' codec can't encode characters in position 36-40: ordinal not in range\(128\)**

주소의 36-40번째에 있는 문자를 제대로 인식할 수 없다는 뜻입니다. 범인은 바로 '아시안게임' 입니다.



## 문자열 인코딩

주의력이 깊으신 분들은 종종 인터넷 주소에서 %20%41 등 %가 많이 들어가있는 부분을 본 적이 있으실 것입니다.

본적이 없다면 네이버 뉴스에 검색한 주소를 복사해 메모장 등 다른 편집기에 붙여넣어 보세요.

![](../.gitbook/assets/image%20%28131%29.png)

한글 부분이 아래 주소와 같이 자동으로 바뀝니다.

이는 인터넷 주소에 기본적으로 ASCII 문자만 사용할 수 있기 때문입니다. ASCII 문자란 숫자와 영문 대소문자, 기타 자주 사용되는 특수문자 등을 모은 128개의 문자 집합입니다. 

따라서 그 외의 문자는 주소에 사용될 때 16진수로 번호를 매겨 %95, %EC 등으로 대체됩니다.

우리는 이 변환을 직접 해서 코드에 입력할 수 없습니다. 변환 규칙도 모르거니와 안다고 하더라도 너무 비효율적인 작업이죠.



이런 작업은 당연히 라이브러리가 존재할 것입니다.

```python
from urllib import request, parse
from bs4 import BeautifulSoup
```

urllib에서 request 뿐만 아니라 parse라는 클래스도 import합니다.

```python
print(parse.quote('아시안게임'))
```

이 parse의 quote라는 함수를 사용하면 손쉽게 한글 문자열을 ASCII로 인코딩한 결과를 얻을 수 있습니다.

![](../.gitbook/assets/image%20%28100%29.png)



```python

from urllib import request, parse
from bs4 import BeautifulSoup

raw = request.urlopen('https://search.naver.com/search.naver?&where=news&query=' + parse.quote('아시안게임') + '&start=1')
html = BeautifulSoup(raw, 'html.parser')

list = html.select('.type01 dl')

for article in list:
    journal = article.select_one('span._sp_each_source').text
    title = article.select_one('dt').text

    print(journal, title)
```

이제 주소의 '아시안게임' 부분을 직접 입력하지 않고 인코딩한 결과를 사용하도록 바꾸어주면, 정상적으로 데이터가 수집되는 것을 볼 수 있습니다.

![](../.gitbook/assets/image.png)



## 페이지를 이동하며 수집해보자

이제 주소 요청값 중 start를 바꾸어가며 여러 페이지의 기사를 수집해보겠습니다. 가볍게 3페이지까지만 수집합니다.

```python
for page in range(3):
    raw = request.urlopen('https://search.naver.com/search.naver?&where=news&query=' + parse.quote('아시안게임') + '&start=' + str(page * 10 + 1))
    html = BeautifulSoup(raw, 'html.parser')
    # ... 생략
```

방법은 크게 다르지 않습니다. 

한 페이지를 수집하는 코드를 그대로 for문 안에 집어넣고, 반복 변수를 이용해 start만 원하는대로 바꾸어주면 됩니다. 

str\(\) 함수를 사용해 숫자를 문자열로 바꿔주지 않으면 문자열 + 숫자 덧셈 에러가 발생하니 꼭 잊지 마세요!




## 조건문 if에 대해 배워보자

그렇다면 for문에서 새로 처리할 클립이 들어왔을 때 두 가지 경우가 생깁니다.

1. 이미 배열에 해당 채널의 정보가 있는 상황이라면 - 해당 번호에 조회수와 좋아요 수를 더한다.
2. 배열에 해당 채널의 정보가 없다면 - 채널명, 조회수, 좋아요 수를 각 배열에 추가한다.

이렇게 현재 상태에 따라 다른 코드를 실행해야 하는 경우 조건문 if가 사용됩니다.

```python
if 채널이 channels에 존재하면:
    hits의 해당 부분에 hit 더하기
    likes의 해당 부분에 like 더하기
```

for문과 비슷한 구조로 이루어져 있네요. if 다음에 등장하는 조건이 만족될 때에만 들여쓰기한 부분의 코드가 실행됩니다.

사용되는 조건은 **"참"** 혹은 **"거짓"** 둘 중 하나가 되는 문장이어야 합니다.

사용할 수 있는 문장은 다음과 같습니다.

### 같음 비교 문장

어떤 변수나 값이 조건과 일치할 때를 검사합니다.

```python
a = 1
b = 2

if a == 1:
    print('a가 같다') 
    
if b == 1:
    print('b가 같다')
```

### 다름 비교 문장

어떤 변수나 값이 조건과 다를 때를 검사합니다.

```python
a = '코알라'
b = '파이썬'

if a != '파이썬':
    print('a가 다르다')
if b != '파이썬':
    print('b가 다르다')
```

### 종속 비교 문장

어떤 변수나 값이 배열 안에 들어있는지 / 들어있지 않은지 검사합니다.

```python
arr = [1, 3, 5, 7, 9]

if 1 in arr:
    print('1이 있다')

if 2 not in arr:
    print('2가 없다')
```



여기서는 channels 배열에 현재 조사중인 클립의 채널이 들어있는지 검사하여야 하기 때문에 종속 비교 문장을 사용합니다.

```python
for info in infos:
    chn = info.select('dd.chn > a')[0].text
    hit = int(info.select('span.hit')[0].text[4:].replace(',', ''))
    like = int(info.select('span.like')[0].text[5:].replace(',', ''))

    if chn in channels:
        idx = channels.index(chn)
        hits[idx] = hits[idx] + hit
        likes[idx] = likes[idx] + like
```

채널명 chn이 배열 channels에 들어있는 경우를 처리합니다.

.index\( \) 함수는 배열 클래스에 내장된 함수로, 매개변수로 전달된 값의 번호를 돌려줍니다.

이 번호를 이용해 hits 배열과 likes 배열의 해당 부분을 수정할 수 있습니다.

이제 새로 들어온 클립의 채널 정보가 존재하지 않을 때도 처리해 봅시다.





```python
if chn in channels: # 만약 channels 배열에 chn이 들어있으면
    ...

else:               # 들어있지 않으면 
    channels.append(chn)
    hits.append(hit)
    likes.append(like)
```

else는 "만약 그렇지 않다면" 이라는 뜻으로, if문과 짝을 이루어 사용합니다.

if문이 실행되지 않는 조건일 경우, 아래의 else 문이 실행됩니다.  
\(else문이 항상 있어야 하는 것은 아닙니다. else 문이 없으면 아무것도 실행되지 않습니다.\)

채널 정보가 없는 경우 처음에 했던대로 그냥 새로 추가하기만 하면 됩니다.



이제 결과를 출력해봅시다.

.index\( \) 함수를 사용할 수 있으니 다음과 같이 보기 좋게 출력하는 것이 가능합니다.

```python
for channel in channels:
    idx = channels.index(channel)
    print(channel, '/', hits[idx], '/', likes[idx])
```

세 배열을 따로 출력하는 것이 아니라, 같은 번호끼리 묶어 출력하게 됩니다. 서로 길이가 달라 불편했던 기존 출력방식보다 훨씬 보기 좋습니다.

![](../.gitbook/assets/image%20%28164%29.png)

그런데 출력 결과가 이게 뭐죠? 숫자가 이렇게 클 수가 있나요? 온 우주의 생명체가 모두 좋아요를 눌러도 이만큼은 안나올텐데요.