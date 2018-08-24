
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

