# 모범 답안



```python
import requests
from bs4 import BeautifulSoup
import datetime
import openpyxl

req = requests.get('https://tv.naver.com/r/')
raw = req.text
html = BeautifulSoup(raw, 'html.parser')


infos = html.select('.cds_info')
chnInfos = {}

excel = openpyxl.Workbook()
sheet = excel.active
sheet.title = '조회수별 정렬'


for info in infos:
    chn = info.select('dd.chn > a')[0].text
    hit = int(info.select('span.hit')[0].text[4:].replace(',', ''))
    like = int(info.select('span.like')[0].text[5:].replace(',', ''))

    if chn not in chnInfos.keys():
        chnInfos[chn] = {'hit': hit, 'like': like}
    else:
        chnInfos[chn]['hit'] += hit
        chnInfos[chn]['like'] += like


def sortKey(item):
    return item[1]['hit']


sortedList = sorted(chnInfos.items(), key=sortKey, reverse=True)

for sortedInfo in sortedList:
    sheet.append([sortedInfo[0], sortedInfo[1]['hit'], sortedInfo[1]['like']])

# ---- 새로 추가된 부분 ----

def sortByLike(item):
    return item[1]['like']


sheet2 = excel.create_sheet('좋아요별 정렬')

sortedList = sorted(chnInfos.items(), key=sortByLike, reverse=True)

for sortedInfo in sortedList:
    sheet2.append([sortedInfo[0], sortedInfo[1]['hit'], sortedInfo[1]['like']])

# ----------------------

dt = datetime.datetime.now()
filename = dt.strftime("TOP_100_%Y_%m_%d")

excel.save(filename + '.xlsx')
```

