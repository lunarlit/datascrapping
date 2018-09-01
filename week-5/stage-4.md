# Stage 4 - 네이버 뉴스 예제에 적용해보자

3주차의 Stage 4에서는 네이버 뉴스를 특정 키워드로 검색하여 페이지를 이동하며 언론사명과 기사 제목을 추출하는 수집기를 작성하였습니다.

이 수집기를 더 발전시켜서 수집한 최초 데이터 이상의 새로운 가치를 창출하고, openpyxl을 사용하여 저장해 보겠습니다.

## 데이터 가공

### 가공될 데이터 구조 설계

지금 수집되는 데이터의 형태는 아래와 같습니다.

![](../.gitbook/assets/image%20%28306%29.png)

이를 가공하여 검색 키워드에 대해 어떤 언론사가 어떤 기사를 썼는지 파악할 수 있도록 만들어 보려고 합니다.

![](../.gitbook/assets/image%20%2895%29.png)

가공 결과는 위와 같이 언론사명에 여러 기사 제목이 대응되는 형태가 될 것입니다.

### 데이터 모델 설계

이러한 구조를 코드로 구현하려면 어떻게 해야 할까요?

![](../.gitbook/assets/image%20%28280%29.png)

‘언론사명’ 이라는 중심 키워드에 데이터를 모아야 하기 때문에 전체 구조는 Dictionary를 사용할 것입니다. 키에 대응하는 값은 무엇이 될까요? 네이버 TV 예제처럼 복합적인 구조인가요?

조회수와 좋아요 수를 구분하여 같은 키에 저장해야 했던 네이버 TV 예제와 달리 값의 종류는 기사 제목 하나 뿐입니다. 반면 채널 당 총 조회수와 총 좋아요 수 정보만 유지하면 되었던 네이버 TV 예제와 달리 뉴스 기사는 개수가 가변적입니다.

따라서 수집되는대로 자유롭게 추가할 수 있는 리스트를 값의 구조로 사용하려 합니다.

### 가공 알고리즘

가공 방식과 구조가 완성되었다면 이를 구현할 알고리즘을 짜고 의사 코드로 만들어보는 것이 좋습니다.

![](../.gitbook/assets/image%20%28325%29.png)

페이지마다 여러 개의 언론사명, 기사 제목 쌍이 수집될 것입니다.

이를 축적 Dictionary에 언론사명이 존재하는 경우, 존재하지 않는 경우로 나눕니다.

존재하는 경우라면 Dictionary\[언론사명\] 이 가지고 있는 리스트에 기사 제목을 추가하고 존재하지 않는 경우라면 Dictionary에 언론사명 키를 추가합니다.

이 알고리즘을 토대로 의사 코드를 작성합니다.

```python
journal_articles = {}

for i in range(수집할 페이지):
    raw = requests.get('주소' + 페이지 변수).text
    html = BeautifulSoup(raw, 'html.parser')

    articles = 기사 리스트 수집

    for article in articles:
        journal = 데이터 추출
        title = 데이터 추출

        if 언론사명이 journal_articles.keys()에 들어있다면:
            journal_articles[journal]에 기사 제목 추가
        else:
            journal_articles[journal] = [title]


journal_articles 결과 출력
```

### 알고리즘 구현

위에서 만든 의사 코드를 실제 코드 문법으로 바꾸어 작성합니다.

```python
import requests
from bs4 import BeautifulSoup

journal_articles = {}

for i in range(3):
    raw = requests.get('https://search.naver.com/search.naver?&where=news&query=아시안게임&start=' + str(i * 10 + 1), headers={'User-Agent': 'Mozilla/5.0'}).text
    html = BeautifulSoup(raw, 'html.parser')

    articles = html.select('.type01 > li')

    for article in articles:
        journal = article.select_one('span._sp_each_source').text
        title = article.select_one('a._sp_each_title').text

        if journal in journal_articles.keys():
            journal_articles[journal].append(title)
        else:
            journal_articles[journal] = [title]

for item in journal_articles.items():
    print(item)
```

19행에서는 존재하지 않는 언론사명 키에 대하여 journal\_articles Dictionary에 추가해주면서, \[title\] 이라는 값을 할당하고 있습니다.

이는 방금 수집한 기사 제목 하나가 들어가는 리스트입니다.

17행에서는 존재하는 언론사명 키에 대하여 journal\_aritlces\[journal\] 이라는 접근자로 해당 리스트를 가져와, .append\(\) 함수를 사용하여 리스트에 새 기사 제목을 추가해줍니다.

이제 실행하여 결과를 확인해보세요.

![](../.gitbook/assets/image%20%2838%29.png)

3주차 Stage 4의 출력 결과입니다.

![](../.gitbook/assets/image%20%28368%29.png)

방금 구현한 코드의 출력 결과입니다. 수집된 데이터에서 새로운 가치를 창출했습니다.

## 검색어 변경 기능

```python
raw = requests.get('https://search.naver.com/search.naver?&where=news&query=아시안게임&start=' + str(i * 10 + 1), headers={'User-Agent': 'Mozilla/5.0'}).text
```

현재 우리의 검색 키워드는 위 코드에 문자열로 고정되어 있습니다.

다른 뉴스를 검색하고 싶을 때마다 코드를 바꿔야 하는 상태입니다.

이를 아래와 같이 바꾸어 봅시다.

```python
keyword = input('검색어를 입력해주세요 : ')

for i in range(3):
    raw = requests.get('https://search.naver.com/search.naver?&where=news&query=' + keyword + '&start=' + str(i * 10 + 1), headers={'User-Agent': 'Mozilla/5.0'}).text
```

![](../.gitbook/assets/image%20%28165%29.png)

콘솔 창에서 검색어를 입력하고 나서 검색이 실행되는 모습입니다.

파이썬 내장함수 input은 실행 후 사용자에게 값을 입력받아 변수에 할당할 수 있게 합니다. 뭔가 타이핑해야 하는 것은 똑같아서 차이점을 느끼지 못할 수도 있지만 상용 프로그램이라고 생각해보세요. 사용자가 코드를 직접 바꿀 수는 없지만 입력창에 키워드를 넣을 수 있는 것과 같습니다.

검색어는 한번만 입력해야 하기 때문에 input 함수는 for문 바깥에 작성합니다.

## 데이터 저장

### 저장 구조 설계

가공한 데이터를 보기 좋게 저장하는 것도 고민해봐야 할 일입니다.

네이버 TV 수집 예제는 채널명 - 데이터가 1:1 대응이기 때문에 그냥 한 줄로 출력해도 충분했지만 지금의 네이버 뉴스 예제는 다릅니다.

![](../.gitbook/assets/image%20%28155%29.png)

데이터를 위와 같이 언론사명 - 기사 제목이 잘 보이도록 작성하려고 합니다.

이를 현재 우리가 가지고 있는 데이터와 비교하여 어떻게 구현할 지 생각해 봅시다.

### 저장 알고리즘 설계

![](../.gitbook/assets/image%20%28173%29.png)

현재 데이터는 위와 같이 \(언론사명, \[기사 제목 리스트\]\) 로 이루어진 리스트입니다.

![](../.gitbook/assets/image%20%28433%29.png)

먼저 이 리스트를 순회하면 각 언론사의 데이터에 접근할 수 있고, 먼저 언론사명을 A열에 작성합니다.

다음으로 기사 제목 리스트를 순회하며 B열에 행을 증가시키며 작성합니다.

언론사 정보 하나가 종료되면 빈 행을 추가하여 다음 언론사와 구분합니다.

이를 의사 코드로 작성해봅니다.

### 코드 작성

```python
wb = 워크북 생성
ws = 워크시트 준비
row = 1

for item in journal_articles.items():
    ws의 row행 1열 = item[0]

    for title in item[1]:
        ws의 row행 2열 = title
        row += 1  # 다음 기사 제목을 위해 다음 행으로 넘김

    row += 1  # 다음 언론사를 위해 빈 행 추가

wb.save(파일 제목)
```

작성 구조 상 한 행의 형태가 동일해야 유용한 .append\(\)는 사용하기 어려울 것 같습니다.

행을 담당하는 변수 row를 도입하여 언론사명 - 기사 제목의 작성 흐름을 제어하고 있습니다.

셀 작성 방법으로는 행과 열을 코드로 자유롭게 컨트롤할 수 있는 .cell\(\) 함수가 좋을 것 같네요.

위 의사 코드를 포함한 전체 코드는 다음과 같습니다.

```python
import requests
from bs4 import BeautifulSoup
import openpyxl

journal_articles = {}

keyword = input('검색어를 입력해주세요 : ')

for i in range(3):
    raw = requests.get('https://search.naver.com/search.naver?&where=news&query=' + keyword + '&start=' + str(i * 10 + 1), headers={'User-Agent': 'Mozilla/5.0'}).text
    html = BeautifulSoup(raw, 'html.parser')

    articles = html.select('.type01 > li')

    for article in articles:
        journal = article.select_one('span._sp_each_source').text
        title = article.select_one('a._sp_each_title').text

        # ----------- 가공 --------------
        if journal in journal_articles.keys():
            journal_articles[journal].append(title)
        else:
            journal_articles[journal] = [title]
        # ------------------------------

for item in journal_articles.items():
    print(item)

# ----------- 엑셀 파일 저장 --------------

wb = openpyxl.Workbook()
row = 1
ws = wb.active
ws.title = '언론사별 기사 제목'

for journal_titles in journal_articles.items():
    ws.cell(row=row, column=1).value = journal_titles[0]

    for title in journal_titles[1]:
        ws.cell(row=row, column=2).value = title
        row += 1

    row += 1

wb.save(keyword + '.xlsx')

# ---------------------------------------
```

