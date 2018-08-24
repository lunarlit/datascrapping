# 모범 답안

```python

from bs4 import BeautifulSoup
from urllib import request
import openpyxl
import datetime

xl = openpyxl.Workbook()
sheet = xl.active
sheet.title = "질문 리스트"

dict = {}

for page in range(10):
    page = page + 1
    raw = request.urlopen('https://hashcode.co.kr/?page=' + str(page))
    html = BeautifulSoup(raw, 'html.parser')

    list = html.select('.question-list-item')

    for question in list:
        tags = question.select('.question-tags > .label-tag')

        for tag in tags:
            t = tag.select_one('a > span').text

            if t in dict:
                dict[t] += 1
            else:
                dict[t] = 1

    print(page, '/ 10')


def sortKey(tagInfo):
    return tagInfo[1]


sortedList = sorted(dict.items(), key=sortKey, reverse=True)

print(sortedList)

for item in sortedList:
    sheet.append([item[0], item[1]])

filename = 'hashcode_' + datetime.datetime.now().strftime("%Y_%m_%d")
xl.save(filename + '.xlsx')
```

