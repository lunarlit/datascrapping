

## 데이터를 가공하고 저장하기

방식은 이전에 네이버TV TOP100 수집에서 사용한 것과 같지만, 수집 대상에 따라 구조가 달라지는 부분이 있기 때문에 한번 더 다뤄보겠습니다.

![](../.gitbook/assets/image%20%28116%29.png)

목표로 하는 데이터 구조는 위와 같습니다. 언론사와는 관계 없이 다른 기준으로 정리된 기사들을 수집하여 언론사 이름으로 묶어 보여줄 생각입니다.

언론사명을 키로 하는 Dictionary가 떠올라야겠죠?



```python
dict = {}
keyword = '아시안게임'
period = '7'
# pd: 1일 - 4, 1주 - 1, 1개월 - 2, 6개월 - 6, 1년 - 5, 1시간 - 7, 
#     2시간 - 8, 3시간 - 9, 4시간 - 10, 5시간 - 11, 6시간 - 12
```

수집을 시작하기 전 dict라는 이름의 빈 Dictionary 를 만들어 줍니다.

검색 키워드와 기간은 변수로 만들어두는 것이 좋습니다. 추후 변경시킬 필요가 생기더라도 여러 곳에 사용되는 값을 한 번에 수정할 수 있기 때문입니다.



```python
for article in list:
    journal = article.select_one('span._sp_each_source').text
    title = article.select_one('dt').text

    if journal not in dict:
        dict[journal] = []
        dict[journal].append(title)
    else:
        if title not in dict[journal]:
            dict[journal].append(title)
```

가장 중요한 수집 부분입니다. 언론사명이 dict에 키로 들어있는 경우와 들어있지 않은 경우로 나누어 처리합니다.

5행의 if문에서 들어있지 않은 경우를 처리합니다. Dictionary의 해당 키에 값으로 빈 배열을 주고 배열에 기사 제목을 추가합니다.

8행의 else문에서 언론사명이 이미 Dictionary에 들어있는 경우를 처리합니다. 또다시 if문을 사용하여 제목이 이미 해당 언론사의 기사 제목 배열에 있는지 검사한 후 추가합니다.  
\(네이버의 검색 시스템 상 중복된 기사가 나오는 경우가 있습니다.\)



```python
for key in dict.keys():
    print(key, dict[key])
```

수집이 끝난 후 출력해주는 부분입니다. dict.items\(\)의 배열을 출력하는 것도 좋은 방법입니다.

아래에서 엑셀에 저장할 것이기 때문에 굳이 출력하지 않아도 괜찮습니다.



```python
wb = openpyxl.Workbook()
row = 1
ws = wb.active

for journalTitles in dict.items():
    ws.cell(row=row, column=1).value = journalTitles[0]

    for title in journalTitles[1]:
        ws.cell(row=row, column=2).value = title
        row += 1

    row += 1

wb.save(keyword + '.xlsx')
```

엑셀 파일에 저장하는 코드입니다. 이 부분은 네이버 TV TOP 100의 구조와 많이 다릅니다. 

네이버 TV 예제의 경우 채널명 - 메타 정보가 1:1 관계였지만 뉴스 검색의 경우 언론사명 - 기사 제목이 1:多 관계입니다.

한 줄에 모든 채널 정보를 출력할 수 있는 네이버 TV 데이터와 달리 한 언론사의 제목 정보가 여러 줄을 차지할 수 있는 것이죠.



![](../.gitbook/assets/image%20%28116%29.png)

이를 위해 존재하는 row 변수의 변화에 주목해주세요. 

먼저 언론사명을 작성한 후, 기사 제목 배열을 이용해 다시 for문을 돌립니다. 

이 때 첫 제목은 행 변화 없이 2열에 쓰고 작성 후 row를 1 증가시킵니다. 그럼 다음 제목은 다음 행에 작성되겠죠? 

모든 제목을 다 쓰고나면 다음 언론사로 넘어가기 전 row를 1 증가시킵니다. 이를 통해 언론사 사이에 공백 행이 하나 생기게 됩니다.

제목은 검색 키워드로 지정하고 있습니다. 전에 배운 datetime을 이용해 시간 정보를 입력하는 응용도 가능하겠지요?



```python

from bs4 import BeautifulSoup
from urllib import request, parse
import openpyxl

# ----------------- 준비 ----------------- 

dict = {}
keyword = '아시안게임'
period = '7'
# pd: 1일 - 4, 1주 - 1, 1개월 - 2, 6개월 - 6, 1년 - 5, 1시간 - 7, 2시간 - 8, 3시간 - 9, 4시간 - 10, 5시간 - 11, 6시간 - 12

raw = request.urlopen('https://search.naver.com/search.naver?where=news&query=' + parse.quote(keyword) + '&pd=' + period + '&start=1')
html = BeautifulSoup(raw, 'html.parser')
total = int(html.select_one('.all_my').text.split('/')[1][:-1].replace(',', '').strip())
page = 1

# ----------------- 수집 ----------------- 

while (page - 1) * 10 < total:
    raw = request.urlopen('https://search.naver.com/search.naver?where=news&query=' + parse.quote(keyword) + '&pd=' + period + '&start=' + str((page-1) * 10 + 1))
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

wb = openpyxl.Workbook()
row = 1
ws = wb.active

# ----------------- 저장 ----------------- 

for journalTitles in dict.items():
    ws.cell(row=row, column=1).value = journalTitles[0]

    for title in journalTitles[1]:
        ws.cell(row=row, column=2).value = title
        row += 1

    row += 1

wb.save(keyword + '.xlsx')
```

전체 코드입니다. 자신의 코드와 다른 점이 있다면 비교해보고, 혹시 오류가 생긴다면 참고하세요.

꽤나 긴 코드지만 이제 자연스럽게 읽을 수 있는 능력이 생기셨길 바랍니다^^







