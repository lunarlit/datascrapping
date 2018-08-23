# Pre - 실습 코드 데이터

여러분의 소중한 시간을 절약하기 위해 의미없이 타이핑해야하는 데이터들은 미리 준비해두었습니다.

코드 창 우측 상단의 Copy 버튼을 눌러 Pycharm 등의 코드 편집기에 붙여넣으시면 됩니다.

그냥 복사 - 붙여넣기를 할 경우 오류가 생길 확률이 높으니 꼭 Copy 버튼을 사용해 주세요.

## Stage 1 - Dictionary에 대해 알아보자 {#stage-1-dictionary}

```python
# ---- dictionary 기본 구성 & 조회 ----

people = {'korean': 380, 'american': 42, 'japanese': 15}




# ---- 중첩 dictionary 구조 ----

chnInfos = {'하트시그널2': {'hit': 20000, 'like': 3800},
            '미스터션샤인': {'hit': 18000, 'like': 3500},
            '쇼미더머니7': {'hit': 25000, 'like': 2200}}
```



## Stage 1 - 이항 연산자 {#stage-1-bin-op}

```python
a = 100
a = a + 50
a = a - 25
a = a / 5
a = a * 10

print(a)

b = 100
b += 50
b -= 25
b /= 5
b *= 10

print(b)
```



## Stage 2 - 네이버 TV 데이터 가공 예제

3주차 스테이지 2에서 만들었던 자신의 네이버 TV TOP 100 수집 코드를 복사하여 사용하세요. 

자신의 코드를 사용할 수 없는 상황이라면 아래 코드를 복사하여 시작하세요.

```python
import requests
from bs4 import BeautifulSoup

req = requests.get('http://tv.naver.com/r')
raw = req.text
html = BeautifulSoup(raw, 'html.parser')

infos = html.select('div.cds')

for info in infos:
    title = info.select_one('tooltip').text
    chn = info.select_one('dd.chn > a').text
    hit = info.select_one('span.hit').text
    like = info.select_one('span.like').text

    print(chn, '/', title, '/', hit, '/', like)
```



## Challenge 1

```python
subjects = ['파이썬', '자바스크립트', '루비', '코틀린', '자바스크립트', '파이썬',
            '파이썬', 'C++', 'iOS', '파이썬', 'Go', '안드로이드', '파이썬', '루비',
            'C++', 'iOS', '안드로이드', '파이썬', '파이썬', '자바스크립트', '루비',
            '안드로이드', '자바', '파이썬', '파이썬', 'C++', 'iOS', '파이썬',
            'Go', '자바', '파이썬', '루비', 'C++', 'iOS', '안드로이드', '파이썬']
```



## Stage 3 - 참/거짓 {#stage-3-true-false}

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



## Stage 3 - if - else

```python
a = 1

if a < 2:
    print('a는 2보다 작다.')

a = 2

if a < 2:
    print('a는 2보다 작다.')
else:
    print('a는 2보다 작지 않다.')


arr = ['파이썬', '자바스크립트', '루비', '고']

if '자바' not in arr:
    arr.append('자바')
else:
    print('이미 존재하는 과목입니다.')

print(arr)
```



## Stage 3 - 네이버 TV 코드 개선 예제

4주차 스테이지 2에서 만들었던 자신의 네이버 TV TOP 100 수집/가공 코드를 복사하여 사용하세요. 

자신의 코드를 사용할 수 없는 상황이라면 아래 코드를 복사하여 시작하세요.

```python
import requests
from bs4 import BeautifulSoup


raw = requests.get('https://tv.naver.com/r/').text
html = BeautifulSoup(raw, 'html.parser')

infos = html.select('div.cds')
chn_infos = {}

for info in infos:
    chn = info.select_one('dd.chn > a').text
    chn_infos[chn] = {'hit': 0, 'like': 0}

for info in infos:
    chn = info.select_one('dd.chn > a').text
    hit = int(info.select_one('span.hit').text[4:].replace(',', ''))
    like = int(info.select_one('span.like').text[5:].replace(',', ''))

    chn_infos[chn]['like'] += like
    chn_infos[chn]['hit'] += hit

print(chn_infos)
```



## Stage 4 - 리스트 정렬 {#stage-4-list-sort}

```python

# ---- 숫자 정렬 해보기 ----

numbers = [94, 23, 64, 39, 25, 10, 63, 6, 234, 34, 63, 4, 86, 5, 24, 1, 631, 90]


# ---- 문자 정렬 해보기 ----

alphabets = ['r', 'f', 'w', 'b', 'z', 'n', 'm', 'q', 'i', 'y', 'c']


words = ['coffee', 'car', 'carpet', 'candy', 'cure', 'crisis', 'cucumber']


kwords = ['가방', '가면', '가림막', '군인', '누빔', '가판대', '가생이', '경찰', '기업']



# ---- 혼합 정렬 해보기 ----

mixed = [3, '호날두', 'Python', 15, '메시', 'Data']


```



## Stage 4 - Dictionary 정렬 {#stage-4-dict-sort}

```python
# ---- dictionary 정렬 해보기 ----
scores = {'h': 16, 'b': 24, 'd': 91, 'c': 138, 'z': 6, 'a': 65}


# ---- 네이버 TV 예제에 적용 준비 ----
chn_infos = {'하트시그널2': {'hit': 20000, 'like': 3800},
            '미스터션샤인': {'hit': 18000, 'like': 2500},
            '쇼미더머니7': {'hit': 25000, 'like': 3200}}
```



## Stage 4 - 튜플 {#stage-4-tuple}

```python
list = [15, 4, 1, 67, 8]
tuple = (15, 4, 1, 67, 8)

list[0] = 1
print(list)

tuple[0] = 1
print(tuple)
```



## Stage 4 - 채널 정보 정렬 {#stage-4-tv}

4주차 스테이지 3에서 만들었던 자신의 네이버 TV TOP 100 수집/가공 코드를 복사하여 사용하세요. 

자신의 코드를 사용할 수 없는 상황이라면 아래 코드를 복사하여 시작하세요.

```python
import requests
from bs4 import BeautifulSoup

req = requests.get('https://tv.naver.com/r/')

raw = req.text

html = BeautifulSoup(raw, 'html.parser')

infos = html.select('.cds_info')

chnInfos = {}

for info in infos:
    chn = info.select_one('dd.chn > a').text
    hit = int(info.select_one('span.hit').text[4:].replace(',', ''))
    like = int(info.select_one('span.like').text[5:].replace(',', ''))

    if chn in chnInfos.keys():
        chnInfos[chn]['hit'] += hit
        chnInfos[chn]['like'] += like
    else:
        chnInfos[chn] = {'hit': hit, 'like': like}


for chnInfo in chnInfos.items():
    print(chnInfo)
```



