# Stage 1 - 정적 수집과 동적 수집 비교

첫 번째 스테이지에서는 데이터 수집에서 가장 기본적이면서도 필수적이고 많이 쓰이는 한 페이지 정적 수집에 대해 포인트만 다시 다뤄봅니다.

## 수집 대상 판단

스테이지 1, 2에 걸쳐 네이버 쇼핑에 있는 특정 카테고리의 상품들을 수집해 보겠습니다.

먼저 해당 웹 서비스에 정적 수집 방식을 적용할 수 있는지 판단할 수 있어야 합니다.

![](../.gitbook/assets/image%20%28237%29.png)

```text
https://search.shopping.naver.com/search/category.nhn?pagingIndex=1&pagingSize=40&viewType=list&sort=rel&cat_id=50001203
```

마우스 카테고리의 첫 번째 페이지 주소입니다.

![](../.gitbook/assets/image%20%28158%29.png)

```text
https://search.shopping.naver.com/search/category.nhn?pagingIndex=2&pagingSize=40&viewType=list&sort=rel&cat_id=50001203
```

두 번째 페이지의 주소입니다.

첫 번째 페이지 주소와 비교했을 때 pagingIndex가 1에서 2로 바뀐 것을 알 수 있습니다.

이렇게 하나의 주소에 하나의 결과가 대응될 때 대부분 정적 수집이 가능하다고 판단할 수 있습니다.

반면 페이지 내에서 어떤 행동을 하였을 때 주소가 바뀌지 않고도 다른 데이터를 보여주는 것이 가능하다면 불가능하다고 볼 수 있겠지요?

## requests 라이브러리를 이용한 수집 준비

```python
import requests
from bs4 import BeautifulSoup

req = requests.get('https://search.shopping.naver.com/search/category.nhn?cat_id=50001203&pagingSize=40&pagingIndex=1')
raw = req.text
html = BeautifulSoup(raw, 'html.parser')
```

정적 수집에 항상 사용되는 코드입니다.

requests.get 함수로 원하는 페이지에 요청을 보내 결과를 req 변수에 저장하며, 그 중 HTML코드만 뽑아내 raw 변수에 저장합니다.

이 때 raw는 단순 String 타입의 텍스트이며 HTML로서의 기능을 갖고 있지 않습니다. 따라서 BeautifulSoup 라이브러리를 사용해 HTML 타입 변수로 바꾸어주고 있습니다.

## 리스트 가져오기

![](../.gitbook/assets/image%20%2838%29.png)

원하는 데이터가 있는 페이지에 접근했다면 이제 데이터 선택자 경로를 찾아 리스트를 수집해야 합니다. 개발자 도구를 켜고 해당 부분을 마우스로 우클릭하여 맨 아래의 "검사"를 누릅니다.\(크롬 브라우저 기준\)

![](../.gitbook/assets/image%20%2849%29.png)

![](../.gitbook/assets/image%20%28111%29.png)

div.info로 검색이 되었네요. 그런데 선택 영역을 보면 리스트 전체를 덮고 있지 않습니다. 수집하려는 데이터가 모두 저 영역 안에 있다면 상관이 없지만 영역 오른쪽에 있는 브랜드명도 수집하고 싶기 때문에 좀 더 범위를 넓혀보겠습니다.

![](../.gitbook/assets/image%20%28146%29.png)

![](../.gitbook/assets/image%20%28248%29.png)

li.\_itemSection에 마우스를 대보니 리스트의 모든 영역을 커버하는 것을 알 수 있습니다. 현재 li 태그는 \_model\_list와 \_itemSection 두 개의 클래스를 가지고 있는데요.

둘 중 하나의 클래스, 혹은 좀 더 확실하게 둘 모두 선택자 경로에 넣어도 좋습니다. 클래스를 두 개 가진 요소의 경우 li.\_model\_list.\_itemSection 과 같이 .으로 계속 이어나갑니다.

![](../.gitbook/assets/image%20%2834%29.png)

선택자 경로를 발견했으면 Ctrl+F를 눌러 검색해봅니다. 클래스는 중복하여 존재할 수 있기 때문에 같은 리스트의 요소를 한 번에 선택할 수 있게 해주지만, 리스트 밖의 원하지 않는 요소도 같은 클래스를 가질 수 있기 때문입니다.

검색 결과 li.\_model\_list.\_itemSection 이라는 선택자는 물품 리스트 요소에만 있는 것으로 확인됩니다.

그렇다면 이제 파이썬 코드의 HTML 변수에서 .select 함수를 사용하여 리스트를 수집합니다.

```python
import requests
from bs4 import BeautifulSoup

req = requests.get('https://search.shopping.naver.com/search/category.nhn?cat_id=50001203&pagingSize=40&pagingIndex=1')
raw = req.text
html = BeautifulSoup(raw, 'html.parser')

items = html.select('li._itemSection')
```

## 리스트 요소에서 데이터 추출

전체 페이지 내에서 필요한 물품 리스트만 가져오는 데에 성공했지만, 아직도 필요없는 부분이 훨씬 많습니다. 리스트 요소 내에서 필요한 부분만 추출해야겠지요?

![](../.gitbook/assets/image%20%28215%29.png)

먼저 가장 바깥의 리스트 요소에는 data-expose-rank라는 속성에 랭킹이 들어있습니다. 페이지에 드러나지 않는 비밀정보네요! 텍스트가 아닌 속성은 .attrs\['속성명'\] 으로 수집 가능합니다.

```python
# 생략...
items = html.select('li._itemSection')


for item in items:
    rank = int(item.attrs['data-expose-rank'])
```

![](../.gitbook/assets/image%20%28112%29.png)

![](../.gitbook/assets/image%20%2810%29.png)

페이지에 드러나는 정보는 마찬가지로 우클릭 - 검사를 통해 빠르게 찾아볼 수 있습니다. 제목의 선택자 경로인 a.tit은 정보 영역인 div.info 안에서 처음으로 나오기 때문에, 유일성을 검증해보지 않아도 .select\_one\(\) 함수를 사용해 수집할 수 있습니다.

가격, 가격 갱신일, 별점, 상품평 수, 브랜드명도 마찬가지로 가져오겠습니다.

```python
for item in items:
    rank = int(item.attrs['data-expose-rank'])
    info = item.select_one('div.info')
    name = info.select_one('a.tit').text
    price = info.select_one('span.price span.num')
    reload = price.attrs['data-reload-date']
    price = price.text
    star = info.select_one('span.etc span.star_graph span').attrs['style']
    comments = info.select_one('span.etc em').text
    mall = item.select_one('div.info_mall > p.mall_txt > a.mall_img')
    mall_name = mall.attrs['title']
```

## 오

