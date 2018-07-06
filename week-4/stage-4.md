# Stage 4 - 여러 페이지에서 데이터를 수집하고 저장해보자

네번째 스테이지에서는 네이버 뉴스 검색 결과를 수집하여 언론사별로 정리하고 엑셀 파일에 저장해 볼 것입니다.

대부분은 이미 배운 내용의 응용일 수 있습니다. 하지만 네이버 TV TOP 100 이후 처음으로 다른 페이지의 정보를 수집해보는 것이기 때문에 헷갈릴 가능성도 많습니다.

데이터 수집에 능숙해지려면 다양한 형태의 많은 웹페이지를 공략해보며 경험을 쌓는 것이 중요합니다.

처음에는 다 다르게 생긴 것 같아 할 때마다 어려움을 겪지만, 결국 패턴이 몇가지로 수렴하기 때문입니다.

## 언제까지 수집해야 할까?

Stage 3에서 기사를 3페이지까지 수집하는 데에 성공했습니다. 

그런데 원하는 기사를 모두 수집하려면 몇 페이지까지 해야할까요?

정답은 **"기사가 더 없을 때까지"** 입니다. 페이지의 수는 그때그때 다르다는 것이죠.



그런데 이전에 조사했던 '아시안게임' 이라는 키워드를 보면 전체 기사가 54만건에 달합니다. 확인해보니 페이지로는 400페이지까지, 4천건에만 접근할 수 있다고 하네요.

4천건도 수집해서 확인하기엔 너무 큰 숫자인 것 같습니다. 네이버 검색의 기간 한정 기능을 이용해 수집 대상을 좀 줄여보겠습니다.

![](../.gitbook/assets/image%20%28123%29.png)

월드컵에서 탈락하고 아시안게임이 다가오니 확실히 기사가 많아지는 것 같습니다. 

편의를 위해 8페이지에서 끝나는 6시간으로 설정해 보겠습니다.



![](../.gitbook/assets/image%20%28100%29.png)

조사 결과 기간을 의미하는 것은 pd라는 것을 알아냈습니다. 그리고 그 값은 다음과 같습니다.

```text
1일 - 4, 1주 - 1, 
1개월 - 2, 6개월 - 6, 1년 - 5, 
1시간 - 7, 2시간 - 8, 3시간 - 9, 4시간 - 10, 5시간 - 11, 6시간 - 12
```

직접 시간을 변경해가며 확인하는 것도 의미있겠죠?

이제 우리가 수집할 주소에는 '&pd=12' 가 포함됩니다.

이제 수집 대상을 8페이지로 줄였으니, 다시 끝까지 수집하는 문제로 돌아가봅시다.



## 또다른 반복문 while에 대해 알아보자

수집 대상이 8페이지라는 것을 알았으니, for문을 8까지 돌리면 될까요? 물론 수집은 잘 될 것입니다.

하지만 기사가 몇 개 더 올라와서 9페이지가 되면 그걸 또 확인해서 9로 바꿔줘야 할 것 같네요. 키워드나 검색 기간을 바꿀 때마다 항상 페이지 수를 확인해 주어야 하구요. 

이는 자동화랑은 조금 거리가 먼 형태인 것 같죠?



for 문은 그 단어의 의미 상 반복해야 할 배열이 있을 때 사용하는 것이 적합합니다.

for content in array: 의 형태로 사용되고, array에 들어있는 content 하나하나에 대하여 반복 처리하겠다는 의미가 들어있으니까요.

우리가 지금 하려는 작업은 약간 다릅니다. 반복시킬 배열이 있는 것이 아니라, 페이지가 더 없을 때까지 증가시키는 것이기 때문이죠.

이럴 때 while을 사용합니다.

![](../.gitbook/assets/image%20%28185%29.png)



while은 쉽게 말하면 **"반복 조건문"** 이라고 생각할 수 있습니다. 조건을 만족하면 한 사이클이 실행되고, 실행된 이후에도 조건을 만족하면 또 한 사이클을 실행하는 식으로 조건이 만족되지 않을 때까지 반복됩니다.

우리의 예제는 "**아직 기사를 다 조사하지 않았다**" 가 만족해야할 조건입니다.

![](../.gitbook/assets/image%20%28173%29.png)

네이버 뉴스 검색에서는 페이지 내에 검색된 기사의 수를 알려주는데요. 이를 이용하면 기사를 다 조사했는지 안했는지 알 수 있겠네요.

한 페이지 당 10개의 기사가 있기 때문에, 기사가 총 439건이라면 44페이지까지만 조사하면 됩니다.

![](../.gitbook/assets/image%20%28133%29.png)

이를 토대로 위와 같은 조건 반복문을 작성할 수 있습니다.  
\( -1 은 꼭 필요합니다. 기사가 총 8건 있을 때 1페이지의 경우를 생각해 계산해보세요.\)

```python

from urllib import request, parse
from bs4 import BeautifulSoup

page = 1

while (page-1) * 10 < total :
    raw = request.urlopen('https://search.naver.com/search.naver?&where=news&query=' + parse.quote('아시안게임') + '&start=' + str((page-1) * 10 + 1) + '&pd=12')
    html = BeautifulSoup(raw, 'html.parser')

    list = html.select('.type01 dl')

    for article in list:
        journal = article.select_one('span._sp_each_source').text
        title = article.select_one('dt').text

        print(journal, title)
        
    page = page + 1
```

따라서 이와 같은 코드를 작성하면 전체 기사를 조사할 때까지 while 문이 반복되어 모든 페이지를 수집할 수 있습니다.

for문과 다른 점을 주의하여야 하는데, for문은 배열의 요소들을 자동으로 반복시켜주지만 while문은 그렇지 않습니다. 

따라서 19행에서 page를 직접 증가시켜주며 다음 while 문의 조건 검사를 준비해주는 모습이 보입니다.



## 추가 정보 수집

그런데 위 코드를 실행시키면 오류가 날 것입니다. 전체 기사 수를 의미하는 total이 아직 정의되지 않았기 때문입니다.

페이지를 조회한 후 전체 기사 수를 알려주는 텍스트도 수집해와야겠죠?

![](../.gitbook/assets/image%20%28168%29.png)

데이터 경로를 찾아보면 all\_my 라는 클래스를 가진 div에 해당 정보가 들어있는 것을 알 수 있습니다.



![](../.gitbook/assets/image%20%2897%29.png)

여기서 text를 뽑아오면 위와 같습니다.

그런데 여기서 439라는 숫자를 뽑아내려면 어떻게 해야 할까요? 위치를 통해 슬라이싱하려고 해도 숫자의 단위가 10의 자리, 100의 자리로 변하면 위치가 바뀌어버립니다.



![](../.gitbook/assets/image%20%2870%29.png)

이럴 경우 문자열 클래스의 내장 함수 split\( \)을 사용할 수 있습니다. split\( \) 함수는 매개변수로 전달된 문자나 문자열을 구분자로 사용하여, 호출한 문자열을 쪼개서 배열로 되돌려줍니다.

여기서는 '1-10 / 439건' 이라는 문자열을 '/'를 기준으로 '1-10'과 '439건'으로 쪼개고 이 둘이 포함된 배열을 돌려주고 있습니다.

이 배열에서 1번 자리에 있는 ' 439건'을 선택한 후, \[ : -1\] 이라는 범위로 슬라이싱하여 맨 뒤의 '건' 문자를 지우고 .strip\( \) 함수를 호출하여 앞부분의 공백을 지워야 합니다.

여기서는 간과하기 쉽지만, 1000건이 넘어가면 1,000과 같이 천의 자리 쉼표가 생기기 때문에 .replace\(',', ''\) 도 추가하여 지워주어야 하겠습니다.

마지막으로! 수집한 데이터는 문자열 타입이기 때문에 숫자로 바꿔주어야겠죠?

```python
total = int(html.select_one('.all_my').text.split('/')[1][:-1].replace(',', '').strip())
```

사용하는 함수나 순서는 약간 바뀌어도 결과가 같을 수 있습니다. 각자 나름대로 구성해보세요.



```python
from urllib import request, parse
from bs4 import BeautifulSoup

page = 1

raw = request.urlopen('https://search.naver.com/search.naver?&where=news&query=' + parse.quote('아시안게임') + '&start=1&pd=12')
html = BeautifulSoup(raw, 'html.parser')
total = int(html.select_one('.all_my').text.split('/')[1][:-1].replace(',', '').strip())

print('total = ', total)

while (page-1) * 10 < total:
    print('-------------------- page', page, '--------------------')
    raw = request.urlopen('https://search.naver.com/search.naver?&where=news&query=' + parse.quote('아시안게임') + '&start=' + str((page - 1) * 10 + 1) + '&pd=12')
    html = BeautifulSoup(raw, 'html.parser')

    list = html.select('.type01 dl')

    for article in list:
        journal = article.select_one('span._sp_each_source').text
        title = article.select_one('dt').text

        print(journal, title)

    page = page + 1
```

이제 이 total을 이용하여 종료 조건을 검사할 수 있게 되었습니다. 위 코드를 실행하면 10여 페이지에서 언론사와 기사 제목을 수집하는 것을 확인할 수 있습니다.

10행에서 전체 양을 보여주고 13행에서 현재 처리중인 페이지를 출력해주고 있습니다. 

많은 데이터를 수집하기 시작하면 시간이 꽤 걸리기 때문에 이런 안내 메세지를 사용하여 진행도를 확인할 수 있게 만드는 것이 좋습니다.



## 데이터를 가공하고 저장하기

방식은 이전에 네이버TV TOP100 수집에서 사용한 것과 같지만, 수집 대상에 따라 구조가 달라지는 부분이 있기 때문에 한번 더 다뤄보겠습니다.

![](../.gitbook/assets/image%20%2896%29.png)

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



![](../.gitbook/assets/image%20%2896%29.png)

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

