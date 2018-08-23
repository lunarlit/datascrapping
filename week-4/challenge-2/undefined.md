# 모범 답안

```python
import requests
from bs4 import BeautifulSoup

raw = requests.get('https://tv.naver.com/r/').text
html = BeautifulSoup(raw, 'html.parser')

infos = html.select('div.cds')

chn_infos = {}

for info in infos:
    chn = info.select_one('dd.chn > a').text
    hit = int(info.select_one('span.hit').text[4:].replace(',', ''))
    like = int(info.select_one('span.like').text[5:].replace(',', ''))
    score = (like * 350 + hit) / 100

    if chn not in chn_infos.keys():
        chn_infos[chn] = {'hit': hit, 'like': like, 'score': score}
    else:
        chn_infos[chn]['hit'] += hit
        chn_infos[chn]['like'] += like
        chn_infos[chn]['score'] += score


def sortKey(item):
    return item[1]['score']


sortedList = sorted(chn_infos.items(), key=sortKey, reverse=True)

for sortedInfo in sortedList:
    print(sortedInfo)
```

22행과 26행에서 계산한 score를 dictionary에 추가해주고 있고, 30행에서는 정렬 함수의 기준을 score로 바꾸어 주었습니다.

