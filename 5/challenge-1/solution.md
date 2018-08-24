# 모범 답안

```python

import openpyxl

wb = openpyxl.Workbook()
ws = wb.active
ws.title = 'openpyxl 연습'

ws['A1'] = 'Hello openpyxl!'
days = ['월요일', '화요일', '수요일', '목요일', '금요일', '토요일', '일요일']
ws.append(days)

ws2 = wb.create_sheet('계단식 배치')

for i in range(1, 11):
    ws2.cell(row=i, column=i).value = i

wb.save('openpyxl.xlsx')
```

