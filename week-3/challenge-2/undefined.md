# 모범 답안

```python
import requests
from bs4 import BeautifulSoup


req = requests.get('https://tv.naver.com/r/')

raw = req.text

html = BeautifulSoup(raw, 'html.parser')

infos = html.select('.cds_info')

chnInfos = {}

for info in infos:
    chn = info.select_one('dd.chn > a').text
    hit = int(info.select_one('span.hit').text[4:].replace(',', ''))
    like = int(info.select_one('span.like').text[5:].replace(',', ''))
    score = (like * 350 + hit) / 100

    if chn not in chnInfos.keys():
        chnInfos[chn] = {'hit': hit, 'like': like, 'score': score}
    else:
        chnInfos[chn]['hit'] += hit
        chnInfos[chn]['like'] += like
        chnInfos[chn]['score'] += score


def sortKey(item):
    return item[1]['score']


sortedList = sorted(chnInfos.items(), key=sortKey, reverse=True)

for sortedInfo in sortedList:
    print(sortedInfo)
```

비중 상수 C를 350으로 하여 계산하고 단위가 너무 커서 100으로 나누어 주었습니다. 22행과 26행에서 계산한 score를 dictionary에 추가해주고 있고, 30행에서는 정렬 함수의 기준을 score로 바꾸어 주었습니다.

