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

