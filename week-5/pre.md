# Pre - 실습 코드 데이터

여러분의 소중한 시간을 절약하기 위해 의미없이 타이핑해야하는 데이터들은 미리 준비해두었습니다.

코드 창 우측 상단의 Copy 버튼을 눌러 Pycharm 등의 코드 편집기에 붙여넣으시면 됩니다.

그냥 복사 - 붙여넣기를 할 경우 오류가 생길 확률이 높으니 꼭 Copy 버튼을 사용해 주세요.

## Stage 1 - txt 파일 다루기 <a id="stage-1-txt"></a>

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

## Stage 1 - 네이버 TV 수집 결과 저장 <a id="stage-1-tv"></a>

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

## Stage 1 - 시간 정보 포매팅 <a id="stage-1-datetime"></a>

```python
import datetime

dt = datetime.datetime.now()

# print(dt)

df = dt.strftime(<       >)

# print(df)
```

## Stage 2 - 엑셀 파일 다루기 <a id="stage-2-openpyxl"></a>

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

4주차 스테이지 4에서 만들었던 자신의 네이버 TV TOP 100 수집/가공 코드를 복사하여 사용하세요. 

자신의 코드를 사용할 수 없는 상황이라면 아래 코드를 복사하여 시작하세요.

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



## Stage 4 - 네이버 뉴스 예제 저장 <a id="stage-4-news"></a>

3주차 스테이지 4에서 만들었던 자신의 네이버 뉴스 수집 코드를 복사하여 사용하세요. 

자신의 코드를 사용할 수 없는 상황이라면 아래 코드를 복사하여 시작하세요.

```python
import requests
from bs4 import BeautifulSoup

for i in range(3):
    raw = requests.get('https://search.naver.com/search.naver?&where=news&query=아시안게임&start=' + str(i * 10 + 1), headers={'User-Agent': 'Mozilla/5.0'}).text
    html = BeautifulSoup(raw, 'html.parser')

    list = html.select('.type01 > li')

    for article in list:
        journal = article.select_one('span._sp_each_source').text
        title = article.select_one('a._sp_each_title').text

        print(journal, title)
```

