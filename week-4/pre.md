# Pre - 실습 코드 데이터

## Stage 1 - txt 파일 다루기

```python
# ---- 파일 쓰기 ----

# f = <           >
#
# f.<           >
# f.<           >


# ---- 파일에 추가 ----

# f = open('test.txt', < >)
#
# f.write("hello world!")
# f.close()


# ---- 파일 읽기 ----

# f = open('test.txt', < >)
# lines = f.<           >
#
# for line in lines:
#     print(line)
#
# f.close()
```



## Stage 1 - 네이버 TV 수집 결과 저장

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

    if chn in chnInfos.keys():
        chnInfos[chn]['hit'] += hit
        chnInfos[chn]['like'] += like
    else:
        chnInfos[chn] = {'hit': hit, 'like': like}
        

def sortKey(item):
    return item[1]['hit']


sortedList = sorted(chnInfos.items(), key=sortKey, reverse=True)

f = <            >

for sortedInfo in sortedList:
    f.<                >

f.<       >
```



## Stage 1 - 시간 정보 포매팅

```python
import datetime

dt = datetime.datetime.now()

# print(dt)

df = dt.strftime(<       >)

# print(df)
```



## Stage 2 - 엑셀 파일 다루기

```python
# ---- 1. 엑셀 파일 만들고 저장하기 ----

# import openpyxl
#
# wb = openpyxl.<       >
# wb.<       >


# ---- 2. 엑셀 시트 & 셀에 접근하고 수정하기 ----

# import openpyxl
#
# wb = openpyxl.Workbook()
# sheet = <       >
#
# sheet<   > = 'hello world'
# sheet.<   > = '3, 3'
# sheet.<      >
#
# wb.save('test2.xlsx')


# ---- 3. 시트 변환 ----

# import openpyxl
#
# wb = openpyxl.Workbook()
# sheet1 = wb<       >
# sheet1.<       > = '수집 데이터'
# sheet1['A1'] = '첫번째 시트'
#
# sheet2 = wb.<       >
# sheet2['A1'] = '두번째 시트'
#
# sheet1['A2'] = '다시 첫번째 시트'
#
# wb.save('test3.xlsx')


# ---- 4. 기존 파일 불러오기 ----

# import openpyxl
#
# wb = openpyxl.<       >
#
# sheet1 = wb.active
#
# sheet1.title = "이름 변경"
# sheet1.append(range(10))
#
# wb.save('test2.xlsx')
```



## Stage 3 - url을 분석하여 데이터 수집하기



```python

from urllib import <    >, <    >
from bs4 import BeautifulSoup

for i in range(3):
    print('-------------- page' + <    > + '출력중 -----------')
    page = <    >
    req = request.Request('https://search.naver.com/search.naver?&where=news&query=' + <    > + '&start=' + <    >,
                          headers={'User-Agent': 'Mozilla/5.0'})
    raw = request.urlopen(req).read()
    html = BeautifulSoup(raw, 'html.parser')
    list = html.select('.type01 dl')

    for article in list:
        journal = article.select_one('span._sp_each_source').text
        title = article.select_one('dt').text

        print(journal, title)
```



## Stage 3  수집한 데이터 가공하기

```python

from urllib import request, parse
from bs4 import BeautifulSoup

journalTitles = <     >

for i in range(3):
    req = request.Request('https://search.naver.com/search.naver?&where=news&query=' + parse.quote('아시안게임') + '&start=' + str(i * 10 + 1),
                          headers={'User-Agent': 'Mozilla/5.0'})
    raw = request.urlopen(req).read()
    html = BeautifulSoup(raw, 'html.parser')

    list = html.select('.type01 dl')

    for article in list:
        journal = article.select_one('span._sp_each_source').text
        title = article.select_one('dt').text

        if <     >:
            <     > = []
            <     >
        else:
            if title not in journalTitles[journal]:
                <     >

for key in journalTitles:
    print(key, journalTitles[key])
```



## Stage 4 - 여러 페이지 수집 실습



```python

from urllib import request, parse
from bs4 import BeautifulSoup

page = 1

<          > :
    raw = request.urlopen('https://search.naver.com/search.naver?&where=news&query=' + parse.quote('아시안게임') + '&start=' + str(page * 10 + 1) + '&pd=12')
    html = BeautifulSoup(raw, 'html.parser')

    list = html.select('.type01 dl')

    for article in list:
        journal = article.select_one('span._sp_each_source').text
        title = article.select_one('dt').text

        print(journal, title)

    total = total + 1
```



