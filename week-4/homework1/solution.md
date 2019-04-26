# 모범 답안



```python
import requests
from bs4 import BeautifulSoup


raw = requests.get('https://tv.naver.com/r/').text
html = BeautifulSoup(raw, 'html.parser')

chn_infos = {}


top3_infos = html.select('ul.rolling > li')

for info in top3_infos:
    chn = info.select_one('span.ch').text
    hit = int(info.select_one('span.hit').text[4:].replace(',', ''))
    like = int(info.select_one('span.like').text[5:].replace(',', ''))
    score = (like * 350 + hit) / 100

    if chn not in chn_infos.keys():
        chn_infos[chn] = {'hit': hit, 'like': like, 'score': score}
    else:
        chn_infos[chn]['hit'] += hit
        chn_infos[chn]['like'] += like
        chn_infos[chn]['score'] += score

else_infos = html.select('.cds_info')

for info in else_infos:
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

TOP3의 클립 정보, 4위~100위의 클립 정보를 나누어서 처리하되 같은 Dictionary에 데이터가 모이도록 처리해주면 됩니다.

