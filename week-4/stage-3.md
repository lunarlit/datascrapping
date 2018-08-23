# Stage 3 - 조건문 if에 대해 알아보자

지금까지 작성한 코드는 얄짤없이 처음부터 끝까지 모두 실행됩니다.

하지만 경우에 따라 실행되기도 하고, 실행되지 않기도 하는 코드를 만들 수 있다면 좋지 않을까요?

결제를 하려는 데 잔액이 충분하면 결제 창으로 이동하지만 부족하면 에러 메세지를 띄워야 할 수도 있고, 조건에 따라 다른 코드가 실행되어야 하는 경우는 굉장히 많습니다.

조건문 if의 사용법을 습득하고 좀 더 유연한 코드를 만들어 봅시다.

## 참과 거짓

2주차 파이썬 기초학습에서 변수를 설명드릴 때 잠깐 참/거짓 타입에 대해 언급한 적이 있습니다. 여태까지 사용되지 않았지만 참/거짓 타입은 이 조건문에서 큰 힘을 발휘합니다.

참/거짓 타입은 값이 True, False 두 종류밖에 없지만, 참과 거짓을 만들어낼 수 있는 경우는 무척 많습니다.

참/거짓을 만들어내는데 쓰이는 비교 연산자 4종을 소개해 드리겠습니다.

\[비교 연산자 표\]

```python
print(1 == 1)
print(3 > 5)
print('-------')

a = '파이썬'
b = '파이선'
print(a == '파이썬')
print(a != b)
print('-------')

arr = ['파이썬', '자바스크립트', '루비', '고']
print(b in arr)
print(a not in arr)
print('-------')
```

값과 값, 값과 변수, 변수와 변수를 비교하는 것 모두 가능합니다.

## 조건문 if에 대해 배워보자

이제 if문을 배우고 사용해 보겠습니다.

```python
if (참/거짓 값 혹은 비교 연산):
    조건이 참일 경우 실행할 명령
```

if문의 기본적인 형태는 위와 같습니다. if 다음에 참, 거짓을 판별할 조건을 쓰고 :을 찍은 후, 아래 줄부터 조건이 참일 때 실행할 기능을 for문과 같이 Space 4칸, 혹은 Tab 1칸 들여쓰면 됩니다.

```python
a = 1

if a < 2:
    print('a는 2보다 작다.')
```

위 예시에서는 a = 1 이므로 1 &lt; 2로 조건이 참이 됩니다. 따라서 실행할 경우 'a는 2보다 작다.' 라는 메세지가 출력됩니다.

a를 3으로 바꾼 후 다시 실행해보면 메세지가 출력되지 않을 것입니다.

### 조건이 성립하지 않을 경우 실행할 else

위 예제에서 a가 2보다 작지 않으면 사용자는 어떤 메세지도 보지 못한 채 프로그램이 종료됩니다.

조건이 성립하지 않을 때에는 'a는 2보다 작지 않다.' 라는 메세지를 보여줘야 하지 않을까요?

if 문의 조건이 성립하지 않는 경우 실행할 기능을 if와 연결된 else 문에 정의할 수 있습니다.

```python
a = 2

if a < 2:
    print('a는 2보다 작다.')
else:
    print('a는 2보다 작지 않다.')
```

else는 위와 같이 if 조건이 성립할 때 실행할 기능을 모두 작성한 이후, 들여쓰기를 다시 빼내어 else: 라고 작성하고 다시 아래 줄부터 들여쓰기하여 조건이 성립하지 않을 때 실행할 기능을 작성합니다.

종속 연산자 in과 not in도 사용해 볼까요?

```python
subjects = ['파이썬', '자바스크립트', '루비', '고']

if '자바' not in subjects:
    subjects.append('자바')
else:
    print('이미 존재하는 과목입니다.')

print(subjects)
```

arr라는 리스트에 몇 가지 프로그래밍 과목명이 들어있습니다.

그리고 if 문에서는 '자바' 라는 문자열이 subjects에 들어있지 않은지를 물어보고 있습니다. \(not in!\)

그래서 들어있지 않다면 subjects 리스트에 '자바' 를 추가해주고, 들어 있다면 '이미 존재하는 과목입니다.' 라는 메세지를 출력하게 됩니다.

## 네이버 TV 예제 개선하기

현재 저희 코드의 흐름은 아래와 같습니다.

```python
raw = requests.get(~~).text
html = BeautifulSoup(~~)

infos = html.select('div.cds')
chn_infos = {}

for info in infos:
    chn = info에서 추출

    chn_infos[chn] = {'hit': 0, 'like': 0} 을 준비한다

for info in infos:
    chn = info에서 추출
    hit = info에서 추출
    like = info에서 추출

    chn_infos[chn]['hit'] 에 hit을 더한다
    chn_infos[chn]['like'] 에 like를 더한다


print(chn_infos)
```

잘 작동하지만, 불필요한 동작들이 많이 보입니다.

\[사진\]

같은 클립 리스트를 두 번이나 순회해야 하는 것도 맘에 들지 않고, 특히 첫 번째 순회에서는 똑같은 채널명에 {'hit': 0, 'like': 0} 을 여러번 할당하게 됩니다.

이렇게 구현한 이유는 if-else를 배우지 않아서 채널명이 키로 들어있는 경우, 들어있지 않은 경우를 나누어 처리할 수 없었기 때문입니다.

방금 배운 if-else를 사용하여 이 흐름을 깔끔하게 개선해 보겠습니다.

```python
raw = requests.get(~~).text
html = BeautifulSoup(~~)

infos = html.select('div.cds')
chn_infos = {}

for info in infos:
    chn = info에서 추출
    hit = info에서 추출
    like = info에서 추출

    if chn in chn_infos.keys():
        chn_infos[chn]['hit'] 에 hit을 더한다
        chn_infos[chn]['like'] 에 like를 더한다

    else:
        chn_infos[chn] = {'hit': hit, 'like': like}

print(chn_infos)
```

두 번의 순회를 한 번으로 줄였습니다.

if-else를 이용해 키가 Dictionary에 들어있는지 아닌지를 검사하여 들어있다면 키에 접근하여 값을 갱신하고, 들어있지 않다면 키에 Dictionary를 새로 할당하는 구조입니다.

새로 할당할 때는 {'hit': 0, 'like': 0} 이 아니라 현재 순회중인 클립 정보를 즉시 반영하여 {'hit': hit, 'like': like} 로 초기화 해줍니다.

위 의사 코드를 실제로 구현하면 아래와 같습니다.

```python
import requests
from bs4 import BeautifulSoup

raw = requests.get('https://tv.naver.com/r/').text
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


for chn_info in chn_infos.items():
    print(chn_info)
```

이처럼 조건문을 잘 이용하면 불필요한 동작없이 깔끔한 코드 흐름을 구현할 수 있습니다!

