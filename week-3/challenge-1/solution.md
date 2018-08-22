# 모범 답안

```python
import requests
from bs4 import BeautifulSoup

req = requests.get('http://tv.naver.com/r')
raw = req.text
html = BeautifulSoup(raw, 'html.parser')

infos = html.select('ul.rolling > li')

for info in infos:
    title = info.select_one('strong.tit > span').text
    chn = info.select_one('span.ch').text
    hit = info.select_one('span.hit').text
    like = info.select_one('span.like').text

    print(chn, '/', title, '/', hit, '/', like)
```

span.hit과 span.like는 단일 선택자로는 4~100위 클립의 조회수, 좋아요 수와 겹치지만 지금은 ul.rolling &gt; li 라는 1~3위 클립 안에서만 정확성을 가지면 되기 때문에 그대로 사용해도 무방합니다.

