# Stage 3 - 네이버 TV 예제에 적용해보자

세 번째 스테이지에서는 어느 정도 openpyxl의 다양한 기능들을 알아보았으니 다시 네이버 TV TOP 100 예제로 돌아가 수집하고 가공한 데이터를 저장해보도록 하겠습니다.

실제 데이터를 저장하려할 때 가능한 다양한 변형에 대해 알아보고, 기존 수집 코드에 저장 기능을 추가하려면 어디에 어떻게 작성해야 하는지 알아봅니다.

## 클립 정보 저장

첫 번째 시트에는 TOP 100 클립의 정보를 순서대로 저장하겠습니다.

### 워크북 변수 준비 및 초기화

```python
import datetime
import openpyxl


# ---- 엑셀 파일 초기화 ----

excel = openpyxl.Workbook()
clips_sheet = excel.active
clips_sheet.title = 'TOP 100 클립'

clips_sheet.append(['클립 제목', '채널', '조회수', '좋아요 수'])

# -------------------------
```

기존 코드에 openpyxl을 잊지말고 import 해주세요.

저장하기 위해 새로운 임시 엑셀 파일을 만들어 excel이라는 변수에 넣고 TOP 100 클립 시트를 준비하였습니다.

시트의 변수명을 clips\_sheet로 하여 알기 쉽게 만듭니다. 시트 제목도 바꾸어 주었습니다.

첫 행에는 .append\(\) 함수를 사용해 데이터 범주를 입력해주었습니다.

\[사진\]

### 클립 데이터 작성

```python
for info in infos:
    title = info.select_one('dt.title tooltip').text
    chn = info.select_one('dd.chn > a').text
    hit = int(info.select_one('span.hit').text[4:].replace(',', ''))
    like = int(info.select_one('span.like').text[5:].replace(',', ''))

    #... 생략

    clips_sheet.append([title, chn, hit, like])
```

클립 데이터는 이렇게 클립 정보를 순회하는 동안 .append\(\) 함수를 이용해 한 행씩 추가해주면 됩니다.

### 파일명 생성 및 저장

```python
# ----- 엑셀 파일 저장 -----

dt = datetime.datetime.now()
filename = dt.strftime("TOP_100_%Y_%m_%d") + '.xlsx'

excel.save(filename)
excel.close()

# ------------------------
```

마지막으로 datetime 라이브러리를 이용해 오늘 날짜가 포함된 파일명을 생성하여 저장합니다.

![](../.gitbook/assets/image%20%28114%29.png)

성공적으로 엑셀 파일에 담긴 모습입니다!

여러분도 코드를 실행하여 결과를 확인해보세요!

## 채널별 저장 \(조회수 내림차순 정렬\)

두 번째 시트에는 채널별 데이터를 조회수 내림차순으로 정렬하여 저장합니다.

### 워크북 및 시트 준비

```python
# ---- 엑셀 파일 초기화 ----

excel = openpyxl.Workbook()
clips_sheet = excel.active
clips_sheet.title = 'TOP 100 클립'
by_hits_sheet = excel.create_sheet('조회수별 정렬')

clips_sheet.append(['클립 제목', '채널', '조회수', '좋아요 수'])
by_hits_sheet.append(['채널명', '총 조회수 합', '총 좋아요 수 합'])

# -------------------------
```

초기화 영역에 '조회수별 정렬' 시트를 새로 만들고 by\_hits\_sheet 라는 변수에 할당합니다.

새로 만든 시트의 첫 열에도 데이터 범주를 입력해줍니다.

### 채널 데이터 작성

```python
def sort_by_hit(item):
    return item[1]['hit']


sorted_by_hits = sorted(chn_infos.items(), key=sort_by_hit, reverse=True)

for sortedInfo in sorted_by_hits:
    by_hits_sheet.append([sortedInfo[0], sortedInfo[1]['hit'], sortedInfo[1]['like']])
```

채널별 데이터는 클립 순회가 끝나고 Dictionary가 완성된 후, 정렬 결과가 나왔을 때 저장할 수 있습니다.

정렬 함수의 이름을 sort\_by\_hit으로 더 명확히 바꿔줬습니다.

정렬 결과를 할당받는 리스트도 sorted\_by\_hits라는 이름을 지어주었습니다.

이 리스트를 순회하며 정렬된 결과를 한 행씩 by\_hits\_sheet라는 워크시트 변수에 .append\(\) 함수를 이용하여 추가합니다.

### 엑셀 파일 저장

저장부분은 건드릴 필요가 없습니다.

실행하여 결과를 확인해 보세요!

## 채널별 저장 \(좋아요 수 내림차순 정렬\)

세 번째 시트에는 같은 채널별 축적 데이터를 좋아요 수 기준으로 정렬하여 저장해 보겠습니다.

### 워크북 및 시트 준비

```python
# ---- 엑셀 파일 초기화 ----

excel = openpyxl.Workbook()
clips_sheet = excel.active
clips_sheet.title = 'TOP 100 클립'
by_hits_sheet = excel.create_sheet('조회수별 정렬')
by_likes_sheet = excel.create_sheet('좋아요별 정렬')

clips_sheet.append(['클립 제목', '채널', '조회수', '좋아요 수'])
by_hits_sheet.append(['채널명', '총 조회수 합', '총 좋아요 수 합'])
by_likes_sheet.append(['채널명', '총 좋아요 수 합', '총 조회수 합'])

# -------------------------
```

두 번째 시트와 비슷하게 세 번째 시트를 준비합니다. 여기서는 좋아요가 더 비중있는 데이터이니 순서를 바꿔주는 게 좋겠죠?

### 채널 데이터 작성

```python
def sort_by_hit(item):
    return item[1]['hit']


def sort_by_like(item):
    return item[1]['like']


sorted_by_hits = sorted(chn_infos.items(), key=sort_by_hit, reverse=True)
sorted_by_likes = sorted(chn_infos.items(), key=sort_by_like, reverse=True)

for sortedInfo in sorted_by_hits:
    by_hits_sheet.append([sortedInfo[0], sortedInfo[1]['hit'], sortedInfo[1]['like']])

for sortedInfo in sorted_by_likes:
    by_likes_sheet.append([sortedInfo[0], sortedInfo[1]['like'], sortedInfo[1]['hit']])
```

두 번째 시트의 내용을 작성하는 부분에 비슷하게 작성해줍니다.

두 개의 정렬 함수, 두 개의 정렬 결과를 가지고 각각 순회하며 하나는 by\_hits\_sheet에 작성, 다른 하나는 by\_likes\_sheet에 작성하고 있습니다.

여기서도 .append\(\) 를 실행할 때 hit과 like 의 순서가 바뀌는 것에 주의하세요!

이제 실행하여 최종 결과를 확인해 보세요!

