# 모범 답안

```python
from bs4 import BeautifulSoup
from urllib import request
import openpyxl
import datetime

xl = openpyxl.Workbook()
sheet = xl.active
sheet.title = "질문 리스트"

for page in range(1, 11):
    print(page, 'page를 수집하는 중입니다...')
    raw = request.urlopen('https://hashcode.co.kr/?page=' + str(page))
    html = BeautifulSoup(raw, 'html.parser')

    questions = html.select('.question-list-item')

    for question in questions:
        title = question.select_one('div.question h4 > a').text
        sheet.append([title])

print('수집 완료!')

filename = 'hashcode_' + datetime.datetime.now().strftime("%Y_%m_%d")
xl.save(filename + '.xlsx')

```
