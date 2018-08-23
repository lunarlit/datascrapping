
## 수집한 데이터 저장하기

어느 정도 openpyxl의 다양한 기능들을 알아보았으니 다시 네이버 TV TOP 100 예제로 돌아가 수집하고 가공한 데이터를 저장해보도록 하겠습니다.

```python
import datetime
import openpyxl

excel = openpyxl.Workbook()
sheet = excel.active
sheet.title = '조회수별 정렬'
# ... 생략
```

기존 코드에 openpyxl을 잊지말고 import 해주세요.

저장하기 위해 새로운 임시 엑셀 파일을 만들어 excel이라는 변수에 넣고 시트를 준비하였습니다.

```python
# ... 생략
sortedList = sorted(chnInfos.items(), key=sortKey, reverse=True)

for sortedInfo in sortedList:
    sheet.append([sortedInfo[0], sortedInfo[1]['hit'], sortedInfo[1]['like']])

dt = datetime.datetime.now()
filename = dt.strftime("TOP_100_%Y_%m_%d")

excel.save(filename + '.xlsx')
```

전체 Dictionary를 조회수 내림차순으로 정렬하여 sortedList에 넣은 부분 이후에 추가해줍니다. 

정렬한 리스트에서 정보를 하나씩 꺼내 sheet에 .append\(\) 함수를 이용해 한 행씩 추가해줍니다. 

데이터의 순서는 채널명, 조회수, 좋아요 수로 저장하고 있습니다.

마지막으로 datetime 라이브러리를 이용해 오늘 날짜가 포함된 파일명을 생성하여 저장합니다.

![](../.gitbook/assets/image%20%28114%29.png)

성공적으로 엑셀 파일에 담긴 모습입니다!