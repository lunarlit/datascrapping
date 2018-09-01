# Stage 3 - 다른 페이지에 다시 한 번 응용해보자

데이터 수집 클래스에서 가장 중요한 것은 당연히 데이터 수집입니다. 그리고 여러분은 방금 데이터 수집하는 법을 배웠습니다.

데이터 수집을 제대로 익혀두지 않으면 앞으로 어떤 화려한 가공이나 활용 방식을 배워도 쓸모가 없습니다.

그렇기 때문에 다시 한 번 다른 페이지를 통해 데이터 최초 수집의 흐름을 다져보겠습니다.

## 네이버 뉴스 수집 시작!

이번에는 네이버 검색, 그 중에서도 뉴스 검색을 수집해 볼텐데요.

![](../.gitbook/assets/image%20%28180%29.png)

먼저 어떤 키워드로 뉴스를 검색해서 나오는 기사들을 단순히 수집하려고 합니다.

[https://search.naver.com/search.naver?where=news&query=아시안게임](https://search.naver.com/search.naver?where=news&query=아시안게임)

검색창에 '아시안게임'을 검색했을 떄의 뉴스 화면 주소입니다. \(실제 인터넷 창에서 접속한 것과 조금 다를 수도 있습니다.\)

```python
import requests
from bs4 import BeautifulSoup

raw = requests.get('https://search.naver.com/search.naver?&where=news&query=아시안게임').text
html = BeautifulSoup(raw, 'html.parser')
```

HTML 페이지를 요청하고 HTML 코드를 받아와 텍스트 타입에서 HTML 타입으로 바꾸는 작업은 항상 동일합니다.

그런데 가져온 html 데이터를 출력해보면

> 사용 중이신 PC 또는 네트워크에서 네이버의 안정적인 검색 서비스를 방해하는 내용이 감지되었습니다.
>
> 네이버는 안정적인 검색 서비스를 제공하고자, 검색 서비스를 방해하는 비정상적인 움직임이 발견될 시 시스템에 의해 해당 네트워크의 검색을 일시적으로 제한하고 있습니다.
>
> 보안 절차를 통과하면 검색 서비스를 정상적으로 이용하실 수 있습니다.
>
> 보안 절차를 통과하려면 \[제한 해제\] 버튼을 클릭하세요.
>
> ※ "비정상적인 검색"이란? : 프로그램등을 이용, 특정 단어를 반복적으로 대량으로 입력하는 등 네이버 검색 서비스의 안정성을 방해하는 검색 패턴을 의미합니다.

뉴스 기사 데이터는 없고 위와 같은 경고 메시지만 발견할 수 있습니다.

## 안티 크롤링 \(Anti-crawling\)

![](../.gitbook/assets/image%20%28424%29.png)

데이터는 기업의 자산이기 때문에 여러 방법으로 수집을 방해하려 합니다. 이는 데이터수집에 어느정도 익숙해졌을 경우에도 종종 어려움의 원인이 됩니다. 이럴 경우 미리 여러가지 우회 방법을 습득하고 있다가 하나씩 시도해보아야 합니다.

인터넷 공간은 공용 공개가 약속된 곳이기 때문에 데이터 수집 자체가 위법인 것은 아닙니다. 그러나 아래 기사와 같은 부정경쟁 수단으로 사용해서는 안될 것입니다.

[https://www.lawtimes.co.kr/Legal-News/Legal-News-View?serial=98844](https://www.lawtimes.co.kr/Legal-News/Legal-News-View?serial=98844)  
\[판결\] ‘웹사이트 무단 크롤링’ 소송… 잡코리아, 사람인에 승소

## 안티 - 안티 크롤링

![](../.gitbook/assets/image%20%28471%29.png)

우리가 사용하는 인터넷 브라우저들은 웹 페이지 접속 시 브라우저의 정보를 담아 보냅니다.

네이버 뉴스에서 우리의 접속 요청을 프로그램으로 판단한 이유는 이 브라우저 정보가 없었기 때문입니다.

```python
raw = requests.get('https://search.naver.com/search.naver?&where=news&query=아시안게임', headers={'User-Agent': 'Mozilla/5.0'}).text
```

requests.get 함수에 headers={'User-Agent': 'Mozilla/5.0'} 라는 부분을 추가합니다. 파이어폭스 브라우저가 보내는 브라우저 정보를 우리 요청에 똑같이 삽입하는 것입니다.

이제 우리의 요청은 파이어폭스 브라우저 접속 요청과 같습니다.

## 선택자 경로 어게인

지겨우실 수도 있겠지만, 선택자 경로 선정이야말로 최초 데이터 수집에서도 가장 중요한 부분입니다.

코드야 기억이 안나면 베껴 쓰면 그만이지만, 선택자 경로는 웹페이지마다, 수집하려는 데이터마다 다르기 때문입니다.

![](../.gitbook/assets/image%20%28365%29.png)

F12를 눌러 개발자 도구를 열고, 기사 제목에서 검사를 눌러 기사 제목의 HTML 위치를 찾습니다.

![](../.gitbook/assets/image%20%2899%29.png)

부모 HTML 요소로 거슬러 올라가며 기사 전체를 커버하는 HTML 요소를 찾습니다. 추후 이 수집기를 응용하여 언론사, 요약 내용, 연관 뉴스 등을 검색해야 할 수도 있기 때문에, 최소의미단위를 기사 제목이 아니라 기사 전체 정보로 잡아 놓는 것이 좋습니다.

![](../.gitbook/assets/image%20%28423%29.png)

발견한 HTML의 왼쪽 화살표를 접어 자식 요소들을 보이지 않게 합니다. 이 때 비슷한 모양을 가진 형제 HTML들이 보이고, 각각에 마우스를 가져다 대었을 때 다른 기사들이 커버된다면 성공입니다!

이제 발견했으니 선택자 경로를 정해야 합니다. 첫 번째 기사의 선택자는 li\#sp\_nws1 입니다.

하지만 페이지의 기사들을 모두 불러오려면 고유한 값을 갖는 id는 사용할 수 없습니다.

li와 같은 태그명은 너무 흔히 사용하는 것이라 기사 이외의 요소가 선택될 확률이 큽니다.

발견한 요소 자체로 정확성을 확보할 수 없다면 부모 요소로 올라가 살펴봅니다.

기사들을 감싸는 부모 HTML인 ul.type01은 어떨까요?

![](../.gitbook/assets/image%20%28432%29.png)

type01 역시 클래스이므로 검증을 해야합니다.

Ctrl + F를 눌러 검색해봅시다. 여러 번 엔터를 눌러도 기사 리스트를 커버하는 ul.type01만 검색이 됩니다.

이제 .type01 &gt; li 라는 선택자 경로를 사용하면 페이지 내의 기사들을 정확히 불러올 수 있다는 사실이 검증되었습니다.

## 세부 데이터 추출

```python
articles = html.select('.type01 > li')

for article in articles:
    journal = ...기사 정보에서 언론사명 추출
    title = ...기사 정보에서 제목 추출

    print(journal, title)
```

이제 HTML 타입의 .select\( \) 함수를 사용하여 기사 리스트를 articles 변수에 담습니다.

다시 for 문 안에서 각 기사에 대한 세부 정보를 추출해 내야 합니다.

여기서는 언론사명과 기사 제목을 뽑아내 보겠습니다.

![](../.gitbook/assets/image%20%2812%29.png)

다시 기사 HTML 안에서 기사 제목의 위치를 찾습니다. a.\_sp\_each\_title 은 검색 결과 기사 제목에만 사용되는 것을 알 수 있습니다.

```python
import requests
from bs4 import BeautifulSoup

raw = requests.get('https://search.naver.com/search.naver?&where=news&query=아시안게임', headers={'User-Agent': 'Mozilla/5.0'}).text
html = BeautifulSoup(raw, 'html.parser')

articles = html.select('.type01 > li')

for article in articles:
    journal = article.select_one('span._sp_each_source').text
    title = article.select_one('a._sp_each_title').text

    print(journal, title)
```

언론사명도 마찬가지로 선택자를 확정한 후 출력해봅니다. 이제 수집 전 과정이 좀 익숙해 지셨나요?

