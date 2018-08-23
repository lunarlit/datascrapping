# 모범 답안

### 1. 카카오 기술 블로그 태그 수집

```python
import requests
from bs4 import BeautifulSoup

# 1페이지 처리
raw = requests.get('http://tech.kakao.com/').text
html = BeautifulSoup(raw, 'html.parser')

tags = html.select('a.tag')

for tag in tags:
    print(tag.text)

# 2~10페이지 처리
for page in range(2, 11):
    raw = requests.get('http://tech.kakao.com/page/' + str(page)).text
    html = BeautifulSoup(raw, 'html.parser')

    tags = html.select('a.tag')

    for tag in tags:
        print(tag.text)
```

페이지에 따라 주소가 변하는데 page/1 로는 접속이 되지 않습니다.

첫 페이지는 예외적으로 [http://tech.kakao.com/](http://tech.kakao.com/'에서) 에서 수집하고 나머지 10페이지까지를 다시 for문으로 처리합니다.





### 2. 네이버 증권 종목명 수집

```python
import requests
from bs4 import BeautifulSoup


raw = requests.get('https://finance.naver.com/sise/lastsearch2.nhn').text
html = BeautifulSoup(raw, 'html.parser')

stocks = html.select('a.tltle')

for stock in stocks:
    print(stock.text)
```

네이버 개발자의 오타인지 모르겠지만 a.title이 아니라 a.tltle 입니다!

