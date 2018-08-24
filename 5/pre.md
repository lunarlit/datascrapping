

# Pre - 실습 코드 데이터

## Stage 1 - txt 파일 다루기 {#stage-1-txt}

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

infos = html.select('div.cds')
chn_infos = {}


for info in infos:
    chn = info.select_one('dd.chn > a').text
    hit = int(info.select_one('span.hit').text[4:].replace(',', ''))
    like = int(info.select_one('span.like').text[5:].replace(',', ''))

    if chn in chn_infos.keys():
        chn_infos[chn]['hit'] += hit
        chn_infos[chn]['like'] += like
    else:
        chn_infos[chn] = {'hit': hit, 'like': like}


def sortKey(item):
    return item[1]['hit']


sortedList = sorted(chn_infos.items(), key=sortKey, reverse=True)

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


## Stage 3 - 네이버 TV 예제 저장

```python
import requests
from bs4 import BeautifulSoup
import datetime
import openpyxl

raw = requests.get('https://tv.naver.com/r/').text
html = BeautifulSoup(raw, 'html.parser')

# ---- 엑셀 파일 초기화 ----

# 이곳에 엑셀 파일과 관련 변수들을 준비하세요.

# -------------------------

infos = html.select('div.cds')
chn_infos = {}

for info in infos:
    title = info.select_one('dt.title tooltip').text
    chn = info.select_one('dd.chn > a').text
    hit = int(info.select_one('span.hit').text[4:].replace(',', ''))
    like = int(info.select_one('span.like').text[5:].replace(',', ''))

    if chn in chn_infos.keys():
        chn_infos[chn]['hit'] += hit
        chn_infos[chn]['like'] += like
    else:
        chn_infos[chn] = {'hit': hit, 'like': like}

    clips_sheet.append([title, chn, hit, like])


def sortKey(item):
    return item[1]['hit']


sortedList = sorted(chn_infos.items(), key=sortKey, reverse=True)


# ----- 엑셀 파일 저장 -----

# 이곳에서 엑셀 파일을 저장하세요.

# ------------------------

```