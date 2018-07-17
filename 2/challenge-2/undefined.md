# 모범 답안

```python
import requests
from bs4 import BeautifulSoup

req = requests.get('https://tv.naver.com/r/')

raw = req.text

html = BeautifulSoup(raw, 'html.parser')

infos = html.select('div.cds')

print(infos[0].select_one('dt.title tooltip').text, '/', infos[0].select_one('dd.chn > a').text, '/', infos[0].select_one('span.hit').text)
print(infos[1].select_one('dt.title tooltip').text, '/', infos[1].select_one('dd.chn > a').text, '/', infos[1].select_one('span.hit').text)
print(infos[2].select_one('dt.title tooltip').text, '/', infos[2].select_one('dd.chn > a').text, '/', infos[2].select_one('span.hit').text)
print(infos[3].select_one('dt.title tooltip').text, '/', infos[3].select_one('dd.chn > a').text, '/', infos[3].select_one('span.hit').text)
print(infos[4].select_one('dt.title tooltip').text, '/', infos[4].select_one('dd.chn > a').text, '/', infos[4].select_one('span.hit').text)
```

다른 순위의 클립에 접근하기 위해 클립 배열인 infos에 숫자를 바꿔서 사용하고 있습니다.

선택자 경로가 다른 것은 괜찮습니다.  
어떤 선택자 경로가 더 의미를 알기 쉽고 짧은지 비교해보세요.

