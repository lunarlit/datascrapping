# Pre - 실습 코드 데이터

## Stage 1 - txt 파일 다루기 {#5p}

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



## Stage 1 - 네이버 TV 수집 결과 저장 {#10p}

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



## Stage 1 - 시간 정보 포매팅 {#13p}

```python
import datetime

dt = datetime.datetime.now()

# print(dt)

df = dt.strftime(<       >)

# print(df)
```



## Stage 2 - 엑셀 파일 다루기 {#16p}

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



## Stage 3 - url을 분석하여 데이터 수집하기 {#32p}



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



## Stage 3  수집한 데이터 가공하기 {#35p}

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



## Stage 4 - 여러 페이지 수집 실습 {#39p}



```python

from bs4 import BeautifulSoup
from urllib import request, parse
import openpyxl

# ----------------- 준비 -----------------

dict = {}
keyword = '아시안게임'
period = '7'
# pd: 1일 - 4, 1주 - 1, 1개월 - 2, 6개월 - 6, 1년 - 5, 1시간 - 7, 2시간 - 8, 3시간 - 9, 4시간 - 10, 5시간 - 11, 6시간 - 12
req = request.Request('https://search.naver.com/search.naver?&where=news&query=' + parse.quote(keyword) + '&pd=' + period + '&start=1', headers={'User-Agent': 'Mozilla/5.0'})
raw = request.urlopen(req).read()
html = BeautifulSoup(raw, 'html.parser')
total = int(html.select_one('.all_my').text.split('/')[1][:-1].replace(',', '').strip())
page = 1

# ----------------- 수집 -----------------

while <       >:
    req = request.Request(
        'https://search.naver.com/search.naver?&where=news&query=' + parse.quote(keyword) + '&pd=' + period + '&start=' + str((page-1) * 10 + 1),
        headers={'User-Agent': 'Mozilla/5.0'})
    raw = request.urlopen(req).read()
    html = BeautifulSoup(raw, 'html.parser')
    list = html.select('.type01 dl')

    for article in list:
        journal = article.select_one('span._sp_each_source').text
        title = article.select_one('dt').text

        if journal not in dict:
            dict[journal] = [title]
        else:
            if title not in dict[journal]:
                dict[journal].append(title)

    print(page * 10, '/', total)
    page = page + 1

# ----------------- 출력 -----------------

for key in dict.keys():
    print(key, dict[key])

# ----------------- 저장 -----------------

wb = openpyxl.Workbook()
row = 1
ws = wb.active

for journalTitles in dict.items():
    ws.cell(row=row, column=1).value = <     >

    for title in journalTitles[1]:
        ws.cell(row=row, column=2).value = <    >
        <    >

    <    >

wb.save(keyword + '.xlsx')
```

