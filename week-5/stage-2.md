# Stage 2 - 파이썬으로 엑셀을 다뤄보자

두 번째 스테이지에서는 openpyxl 라이브러리를 사용하여 파이썬 코드만으로 엑셀 파일을 생성하고 수정해봅니다.

간단한 데이터를 다룰 때에는 스테이지 1에서 만들어본 txt나 csv파일로 충분하지만, 복잡하고 방대한 데이터를 저장해두고 사용하기엔 부족한 감이 있습니다.

작성 방식은 약간 더 복잡하지만 익숙해지면 별로 어렵지 않으니 같이 배워보도록 하겠습니다.

## 엑셀 파일을 만드는 이유

csv도 엑셀로 열면 똑같이 보이는데 왜 xlsx로 된 엑셀 파일을 만드려는 걸까요?

파일을 열어 조금 다뤄보면 알 수 있습니다.

![](../.gitbook/assets/image%20%28115%29.png)

csv 파일은 말 그대로 "쉼표로 구분된 값들" 이 저장된 파일입니다.

바꿔 말하면 그 이상은 저장할 수 없다는 뜻이 됩니다.

![](../.gitbook/assets/image%20%28488%29.png)

엑셀에서 사용할 수 있는 수식, 서식, 필터, 차트 등 강력한 스프레드시트의 기능들은 csv로 저장하고나면 사라져 버립니다.

또다른 약점도 있습니다. csv는 무조건 위에서 아래로, 좌에서 우로 값을 채워나가기 때문에 내용을 자유롭게 구성해나가기가 힘듭니다.

파일에서 값을 불러와 사용하고 싶을 때에도 문제가 생깁니다.

한줄 한줄이 "2,3,4" 와 같은 문자열로 인식되기 때문에 원래 값을 되찾으려면 쉼표 기준으로 나눠야 하는 등 예상되는 불편이 많습니다.

## 엑셀 파일 만들어보기

엑셀 파일을 다루기 위해서는 추가 라이브러리가 필요합니다.

![](../.gitbook/assets/image%20%28371%29.png)

혹시 라이브러리 추가 방법은 기억 나시나요? 2주차에서 한 번 해보았는데요.

위와 같이 상단 메뉴에서 File &gt; Settings에 들어간 후 Project Interpreter 메뉴로 들어가 나오는 창에서 우측의 녹색 + 버튼을 누르면 됩니다.

![](../.gitbook/assets/image%20%28331%29.png)

이런 창이 뜨면 openpyxl을 검색하여 아래의 Install Package 버튼을 눌러주세요.

엑셀 파일을 다루는 다양한 파이썬 오픈소스 라이브러리가 있지만 여기서는 openpyxl을 사용해 보도록 하겠습니다.

설치 성공 메시지가 확인되면 창들을 닫아주세요.



### openpyxl 용어 정리

먼저 앞으로 쓰일 용어를 정리해보겠습니다. 엑셀에서 쓰이는 용어와 동일합니다.

![](../.gitbook/assets/image%20%28340%29.png)

Workbook은 엑셀 파일 전체를 말합니다. 여러 개의 워크 시트를 가질 수 있습니다. openpyxl의 Workbook 타입과 연결됩니다.

Worksheet는 엑셀의 시트 하나하나를 말합니다. 여러 개의 셀을 가지고 있습니다. openpyxl의 Worksheet 타입과 연결됩니다.

cell은 엑셀 시트의 칸 하나하나를 말합니다. 값을 입력할 수 있습니다. openpyxl의 cell 타입과 연결됩니다.

## 엑셀 파일 다루기 - 워크북 생성

```python
import openpyxl

wb = openpyxl.Workbook()
wb.save('test.xlsx')
```

이제 새로 파이썬 파일을 하나 만들어 위와 같이 입력 후 실행해 봅니다.

![](../.gitbook/assets/image%20%28132%29.png)

코드가 있는 폴더에 test.xlsx 가 생성된 것을 볼 수 있습니다.

openpyxl.Workbook\(\) 함수는 임시로 엑셀 파일\(워크북 타입 값\)을 하나 만들어 되돌려줍니다.

이를 받은 변수 wb를 이용해 이 엑셀 파일을 다룰 수 있습니다.

하지만 임시 파일이기 떄문에 .save\(\) 함수를 호출하여 저장해주어야만 실제 파일을 만들 수 있습니다!

## 엑셀 파일 다루기 - 시트 조작

### 활성화된 시트 불러오기

```python
import openpyxl
wb = openpyxl.Workbook()
sheet = wb.active
```

워크북 변수에서 .active를 사용하면 활성화 되어있는 시트를 불러올 수 있습니다. openpyxl으로 워크북을 생성하면 시트가 하나뿐이기 때문에 여기서는 첫 번째 시트가 선택됩니다. 이제 이를 할당받은 sheet 변수로 이 워크시트에 값을 추가하는 등 조작할 수 있습니다.

### 새 시트 만들기

```python
sheet2 = wb.create_sheet('두번째 시트')
```

워크북 변수에서 .create\_sheet\('시트명'\) 함수를 사용하면 입력된 시트명으로 새 시트가 생성됩니다. 그리고 결과를 할당받은 변수 sheet2를 사용해서 '두 번째 시트'를 조작할 수 있습니다.

### 시트 불러오기

```python
sheet2 = wb['두 번째 시트']
```

이미 존재하는 시트의 경우 Dictionary에서 키로 값을 불러오듯 워크북에서 시트이름으로 불러올 수 있습니다. 이제 이를 할당받은 변수 sheet2를 사용해서 '두 번째 시트'를 조작할 수 있습니다.

### 시트 이름 바꾸기

```python
sheet2.title = '수집 데이터'
```

워크시트 타입 변수에서는 .title을 이용하여 시트명을 바꿀 수 있습니다.

## 엑셀 파일 다루기 - 셀 조작

워크북과 워크시트를 만들었으니 내용도 수정할 수 있어야겠죠.

방대한 기능을 갖춘 openpyxl은 내용을 작성하는 방법도 엄청나게 많지만, 일단 당장에 쓸만한 세 가지만 알아보도록 하겠습니다.

```python
import openpyxl

wb = openpyxl.Workbook()
sheet = wb.active

sheet['B2'] = 'b2'
sheet.cell(row=3, column=3).value = '3, 3'
sheet.append([1, 2, 3, 4, 5])

wb.save('test2.xlsx')
```

![](../.gitbook/assets/image%20%28476%29.png)

위 코드의 실행 결과입니다.

먼저 4행에서 wb.active 라는 값을 불러 sheet 변수에 집어넣고 있습니다.

이제 sheet 변수는 해당 시트를 컨트롤할 수 있는 변수가 됩니다.

### 1. \[ \] 접근

```python
sheet['A1'] = 'hello world'
```

6행에서는 Dictionary에서 키를 통해 접근하듯 시트 변수 sheet에서 \['A1'\] 이라는 값을 통해 셀에 접근하고 있습니다.

이는 B32, F7, AY3 등 엑셀에서 사용하는 행-열 번호를 그대로 사용하여 셀을 조작하는 방식입니다. 가장 직관적인 방법이죠.

### 2. .cell\(row=, column=\) 접근

```python
sheet.cell(row=3, column=3).value = '3, 3'
```

7행에서는 시트 변수에서 .cell\(\) 이라는 함수를 호출하여 셀에 접근하고 있습니다.

옵션으로 넘겨준 row=3은 엑셀의 3행에, column=3은 엑셀의 C열에 대응되어 C3 셀에 입력한 값이 들어갔네요.

1번의 \[ \] 접근 방식에 비해 코드도 길고 불편할 것 같은데 왜 이 방식을 사용해야 할까요?

우리가 사람의 손이 아닌 코드로 엑셀 파일을 생성할 것이기 때문입니다.

'A1'과 같은 셀 번호는 바꾸기가 어렵지만 파이썬에서 숫자를 변경하며 반복하는 것은 매우 쉽죠.

A1, B1, C1, D1, E1에 hello world! 를 입력한다고 하면

```python
sheet['A1'] = 'hello world!'
sheet['B1'] = 'hello world!'
sheet['C1'] = 'hello world!'
sheet['D1'] = 'hello world!'
sheet['E1'] = 'hello world!'
```

```python
for a in range(5):
    sheet.cell(row=1, column=a+1).value = 'hello world!'
```

위가 \[ \] 를 사용하는 방식이고, 아래가 .cell\( \) 을 사용하는 방식입니다.

이와 같이 \[ \] 는 보기엔 직관적이지만 실제로 사용하기에는 유연성이 떨어져 사실 많이 사용하지 않게 됩니다.

### 3. .append\( \) 함수 사용

```python
sheet.append([1, 2, 3, 4, 5])
```

코드 8행에서는 .append\( \) 함수를 사용하고 있는데, 어디에 입력한다는 내용없이 입력값으로 배열 하나만 받고 있습니다.

입력 결과는 엑셀 4행에 배열 요소 하나하나가 각 열로 입력된 것을 볼 수 있죠.

.append\( \) 함수는 시트에 데이터가 존재하는 마지막 행 다음에 새 행을 추가해주는 함수입니다.

예제에서는 엑셀 3행에 3,3이 입력된 상태이기 때문에 4행에 데이터가 추가됩니다.

.append\(\) 함수는 입력할 부분을 선택하는 유연성은 다소 떨어지지만, 배열을 통해 여러 열을 한번에 입력할 수 있기 때문에 데이터를 다룰 때 적합합니다.

### 전체적인 사용 예시

```python
import openpyxl

wb = openpyxl.Workbook()
sheet1 = wb['Sheet']
sheet1.title = '수집 데이터'
sheet1['A1'] = '첫번째 시트'

sheet2 = wb.create_sheet('정리 결과')
sheet2.cell(row=1, column=1).value = '두번째 시트'

sheet1.append(['다시', '첫번째 시트'])

wb.save('test3.xlsx')
```

![](../.gitbook/assets/image%20%28210%29.png)

4행에서는 이전과 같이 .active로 시트를 불러오는 것이 아니라, 엑셀 파일 변수에서 \[ '시트 이름' \] 을 사용하여 불러오고 있습니다. \(openpyxl을 통해 기본 생성되는 시트의 이름이 Sheet 입니다.\)

5행에서는 .title 값을 바꾸어 선택한 시트의 이름을 변경하고 있습니다. 결과 사진을 보면 실제로 시트 이름이 변한 것이 보입니다.

8행에서는 워크북 변수의 create\_sheet 함수를 이용해 새 시트를 만들었습니다. 매개 변수로 전달한 문자열이 그대로 시트 이름이 된 것을 볼 수 있습니다.

새로 생긴 시트는 sheet2 변수에 저장되며, 11행에서는 다시 sheet1 변수를 이용해 첫 번째 시트에 값을 넣고 있습니다. 이처럼 한번 지정된 변수를 이용해 언제라도 시트를 오가며 값을 입력할 수 있습니다.

## 기존 엑셀 파일 불러오기

경우에 따라서는 데이터를 조사할 때마다 새 파일을 만드는 것이 아니라, 새 시트를 추가해 입력하거나 기존 데이터에 포함하여 반영해야 할 수도 있습니다.

현재 방식으로는 새 엑셀 파일을 만든 후 기존에 존재하는 파일 이름으로 저장하면 덮어쓰기를 해버립니다.

새 엑셀 파일을 만들지 않고 기존 파일을 수정하는 방법을 알아보겠습니다.

```python
import openpyxl

wb = openpyxl.load_workbook('test2.xlsx')

sheet1 = wb.active

sheet1.title = "이름 변경"
sheet1.append(range(10))

wb.save('test2.xlsx')
```

test2.xlsx은 셀 조작 파트에서 만든 파일입니다. load\_workbook 함수에 이름을 넘겨 이 파일을 불러왔습니다.

활성화된 시트를 불러와 이름을 변경하고 .append\(\)를 이용해 행을 추가하는 코드입니다.

실행 후 test2.xlsx를 열어보면 기존 내용에서 시트 이름이 바뀌고 데이터가 잘 입력된 것을 볼 수 있습니다.

![](../.gitbook/assets/image%20%28264%29.png)

{% hint style="danger" %}
test2.xlsx가 열려있는 상태라면 쓰기 권한이 없어 오류가 생길 것입니다. 같은 파일 이름으로 저장하려면 꼭 열려있는 엑셀 파일을 닫고 실행해주세요.
{% endhint %}

## 기초 기능 정리

### Workbook 타입 관련 기능

![](../.gitbook/assets/image%20%28350%29.png)

### 

### Worksheet 타입 관련 기능

![](../.gitbook/assets/image%20%28209%29.png)

### 

### cell 타입 관련 기능

![](../.gitbook/assets/image%20%28195%29.png)

훨씬 더 많은 기능을 확인하고 싶다면? [https://openpyxl.readthedocs.io/en/stable/index.html](https://openpyxl.readthedocs.io/en/stable/index.html)

를 참고해주세요.

