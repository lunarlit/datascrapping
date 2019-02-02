# Stage 1 - 가장 간단하게 저장해보자

첫번째 스테이지에서는 간단한 문법으로 쉽게 데이터를 저장할 수 있는 방법에 대해 알아봅니다.

사용이 간단한만큼 저장된 데이터도 복잡한 형태를 가지기 힘든 txt나 csv파일이 되지만, 닭 잡는데 소 잡는 칼을 쓸 필요가 없듯 간단한 형태로 저장하는 경우도 필요할 때가 있습니다.

또한 파이썬이 외부 파일을 다루는 구조에 대해 이해하기 쉬우니 먼저 이 방법으로 시작해 보겠습니다.

## 파이썬코드로 파일 열기

```python
f = open('test.txt', 'w')
```

시작부터 코드부터 나와 당황하셨겠지만, 위의 한 줄만 실행시켜도 파이썬은 외부 파일을 열게 됩니다.

![](../.gitbook/assets/image%20%28428%29.png)

위 코드를 실행시키면 실행한 폴더에 test.txt가 생성됩니다.

open\( \) 함수에 전달되는 두 매개변수 중 첫번째는 당연히 파일의 이름이고, 두번째는 파일을 다룰 모드입니다.

![](../.gitbook/assets/image%20%28467%29.png)

이 모드의 값에 따라 위와 같이 파일에 대해 실행할 수 있는 기능이 다릅니다.



### 쓰기 모드

먼저 쓰기 모드를 사용해 파일 내용을 작성해보겠습니다.

```python
f = open('test.txt', 'w')

f.write("hello world!")
f.close()
```

open의 결과값을 변수 f에 집어넣고 있습니다.

이제 f는 파일 클래스의 변수가 되어 여러 함수를 통해 test.txt 를 컨트롤할 수 있습니다.

.write\( \) 함수는 매개변수로 전달된 문자열을 파일에 쓰는 역할을 하고,

.close\( \) 함수는 열었던 파일을 닫아 더 이상 컨트롤할 수 없게 만듭니다.

사용이 끝난 파일은 꼭 .close\( \) 를 실행하여 연결을 종료하여야 합니다.

![](../.gitbook/assets/image%20%28372%29.png)

위 코드를 실행하고 코드가 있는 폴더를 열어 생성된 test.txt를 보면, hello world! 가 성공적으로 작성된 것을 볼 수 있습니다.



### 추가 모드

다음으로 위 코드를 바꾸어 파일을 추가 모드 \(append의 'a'\) 로 열어 내용을 작성해 보겠습니다.

```python
f = open('test.txt', 'a')

f.write("hello world!")
f.close()
```

이 코드를 실행하고 test.txt를 열어보면, 기존 내용에 새로 작성한 내용이 더해진 것이 보입니다.

![](../.gitbook/assets/image%20%28303%29.png)

이처럼 추가 모드에서는 기존 내용을 그대로 두고 내용을 추가합니다.



### 읽기 모드

![](../.gitbook/assets/image%20%28217%29.png)

읽기 모드를 알아보기에 앞서, test.txt의 내용에 한 줄을 더 추가해 주세요.

```python
f = open('test.txt', 'r')
lines = f.readlines()

for line in lines:
    print(line)

f.close()
```

읽기 모드를 사용해 파일을 열면, .readlines\( \) 함수를 사용해 내용을 읽어올 수 있습니다.

![](../.gitbook/assets/image%20%28347%29.png)

함수의 결과로 리스트를 돌려주는데, 각 요소는 파일의 한 줄입니다.

따라서 위 코드는 다음 결과를 출력합니다.

![](../.gitbook/assets/image%20%28255%29.png)

![](../.gitbook/assets/image%20%28248%29.png)

## 수집한 데이터 저장해보기

이제 파일에 간단히 저장하는 방법을 알았으니, 네이버 TV TOP 100으로 돌아가 정렬된 결과를 저장해봅시다.

저장된 Dictionary를 정렬하여 sortedList라는 변수에 넣었을 것입니다.

```python
f = open('test.txt', 'w')

for sortedInfo in sortedList:
    f.write(sortedInfo[0] + ',' + str(sortedInfo[1]['hit']) + '\n')

f.close()
```

정렬 후에 마지막 부분에 위 코드를 추가하여 결과를 파일에 씁니다.

\( 채널명, 조회수 \n \) 의 형식으로 하나씩 파일에 쓰게 됩니다.

{% hint style="info" %}
\n 은 줄바꿈 문자입니다.

메모장에 직접 쓸 때는 Enter를 눌러 줄을 바꾸지만, 코드 상에서는 이를 표현할 수 없어 대신 '\n' 을 만나면 줄을 바꾸도록 정해져 있습니다.

/ \(슬래시\) 와 \\(역슬래시\) 헷갈리지 않게 주의하세요!
{% endhint %}

위 코드들을 추가하여 실행시키면 다음과 같은 결과 파일을 얻을 수 있습니다.

![](../.gitbook/assets/image%20%28393%29.png)

잘 저장이 되었습니다. 그런데 메모장은 아무래도 열이 맞지 않아 데이터를 확인하기 불편하네요.



### csv 파일로 저장하기

혹시 csv라는 파일 형식을 아시나요?

csv는 Comma Seperated Values의 약자로, 쉼표로 구분된 값들을 의미합니다.

마침 저희가 데이터를 쉼표로 구분하여 저장했는데요.

```python
f = open('test.csv', 'w')
```

파일을 열 때 확장자만 .csv로 바꾸어 다시 실행해 봅시다.

![](../.gitbook/assets/image%20%28491%29.png)

csv 파일은 이처럼 엑셀을 사용해 열 수 있습니다.

쉼표 부분을 분리 기준으로 인식하여 다른 셀에 값을 집어넣어줍니다.

결과적으로 들어있는 값은 똑같지만 보기가 훨씬 편합니다.  
\(엑셀이 설치되어있지 않으신 분들은 엑셀 뷰어로 확인해주세요!\)

## 파일명 자동 변경

네이버 TV TOP100의 데이터를 매일 수집한다고 해봅시다.

open\( \) 함수를 이용해 파일을 쓰기 모드로 열면, 기존에 있던 데이터는 모두 사라지는데요.

그렇다고 추가 모드로 열기엔 날짜별로 데이터가 구분되지 않아 불편할 것 같네요.

![](../.gitbook/assets/image%20%28456%29.png)

대신 수집하는 날짜마다 파일명을 바꾸어 'TOP100\_2018\_06\_28.csv' 와 같이 저장한다면 잘 구분되겠죠?

그런데 실행할 때마다 날짜를 바꿔서 코드에 넣어주어야 한다면 여간 불편한 일이 아닐 것입니다.

완성된 코드를 계속 수정해야 한다는 것은 설계가 잘못되었다는 말이기도 하구요.

다행히 파이썬에서는 날짜를 확인하고 사용할 수 있는 클래스를 제공합니다.

```python
import datetime

dt = datetime.datetime.now()

print(dt)
```

![](../.gitbook/assets/image%20%28469%29.png)

datetime이라는 클래스를 import하여 다음과 같이 현재 시간을 불러올 수 있습니다.

하지만 이를 그대로 파일명에 사용하기엔 부적합합니다.

하루에 한 번 수집할 것이기 때문에 날짜까지만 필요하고, ':' 문자는 파일명에 사용할 수 없기 때문입니다.

```python
df = dt.strftime('%Y_%m_%d')

print(df)
```

![](../.gitbook/assets/image%20%28231%29.png)

datetime 클래스의 .strftime\( \) \(**str**ing **f**ormatted **time** 이라는 뜻이겠네요.\) 함수를 사용하면 날짜 데이터를 원하는 형태로 만들 수 있습니다.

![](../.gitbook/assets/image%20%28274%29.png)

.strftime\( \) 함수에 전달되는 형식 문자열은 위와 같은 내용으로 구성됩니다.

문자열 안의 %Y가 실제 날짜 데이터의 연도 값으로 대체되는 것이죠.

위 표에 나와있지 않은 \_ 같은 문자는 대체하지 않고 그대로 출력됩니다.

그 결과 우리는 파일명에 사용하고자 하는 2018\_06\_28을 얻을 수 있습니다.

```python
dt = datetime.datetime.now()
filename = 'TOP100_' + dt.strftime("%Y_%m_%d")
f = open(filename + '.csv', 'w')
```

이 문자열 앞에 'TOP100\_' 을 붙여 파일명으로 전달하면, 매일 코드를 수정하지 않고도 데이터를 날려먹지 않고 수집할 수 있습니다!

\(그래도 같은 날짜에 두 번 이상 수집하면 이전 데이터가 날아가겠죠?

이럴 경우 시간, 분, 초 단위까지 필요에 따라 활용해서 파일명을 변경해야겠습니다.\)

```python
import requests
from bs4 import BeautifulSoup
import datetime

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

dt = datetime.datetime.now()
filename = 'TOP100_' + dt.strftime("%Y_%m_%d")
f = open(filename + '.csv', 'w')

for sortedInfo in sortedList:
    f.write(sortedInfo[0] + ',' + str(sortedInfo[1]['hit']) + '\n')

f.close()
```

최종 코드입니다. 자신의 코드가 잘 실행되지 않는다면 참고하세요. 그냥 복사 붙여넣기 하는 것보다 자신의 코드와 한줄 한줄 비교하며 어디가 잘못되었는지 파악해보세요.

