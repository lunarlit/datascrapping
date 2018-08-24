# Stage 2 - 정보를 의미있게 가공해보자

두 번째 스테이지에서는 준비된 클립별 정보를 취합하여 채널별로 정리합니다.

이는 네이버TV TOP100 페이지에서 제공되지 않는 정보입니다.

데이터 수집 클래스이긴 하지만, 수집한 데이터를 다시 손으로 정리해야 한다면 반쪽짜리 도움밖에 되지 않는 것이기 때문에 이러한 내용도 다루게 되었습니다.

데이터 분석 이론을 다루는 것은 아니지만, 여기에서 다루는 문법들을 손에 익힌다면 다른 이론의 적용은 쉬워질 것입니다.

## 목표 설정 및 설계

![](../.gitbook/assets/image%20%2841%29.png)

현재 데이터는 클립별로 배열에 들어있습니다. 우리는 TV 트렌드 분석 업무를 하고 있는 상황이며, 클립별 데이터를 가공하여 프로그램의 인기도를 알아보려 합니다.

따라서 클립별 데이터를 채널별로 다시 모아야 합니다.

![](../.gitbook/assets/image%20%2898%29.png)

현재 만들어내려는 데이터의 모습입니다.

TOP 100의 클립 데이터 중 같은 채널끼리 모아 조회수, 좋아요 수를 더하는 것입니다.

## 데이터 모델 설계

위와 같이 만들어내려는 목표 데이터의 구조가 나왔다면, 이를 코드로 어떻게 표현할 지 생각해 보아야 합니다.

클립별로 나열된 리스트는 순위대로 수집되기 때문에 그냥 순서대로 있기만 해도 충분했습니다. 하지만 우리가 만드려는 데이터 구조는 클립 정보 중에서도 "채널명"을 기준으로 하여 재구성됩니다.

이렇게 특정한 하나의 데이터 위주로 구조를 만들어야 할 때는 무엇을 사용해야 할까요?

\[구조 청사진\]

답은 Dictionary입니다. 채널명을 키로 하여 총 조회수와 좋아요 수를 값으로 갖는 Dictionary 구조를 만들면 좋을 것 같습니다.

그리하여 97개의 채널 리스트를 순회하며 뽑아낸 정보들을 Dictionary에 채널명을 기준으로 다시 입력해주면 되겠네요!

## 적용 알고리즘 설계

다음으로는 어떤 순서를 거쳐 위에서 설계한 모델을 만들어낼지 의사 코드\(세세한 문법에 신경쓰지 않고 전체 흐름을 보기 위해 간단히 쓴 코드\)를 작성해 봅니다.

```python
raw = requests.get(~~).text
html = BeautifulSoup(~~)

infos = html.select('div.cds')
chn_infos = {}

for info in infos:
    chn = info에서 추출
    hit = info에서 추출
    like = info에서 추출

    chn_infos[chn]['hit'] 에 hit을 더한다
    chn_infos[chn]['like'] 에 like를 더한다


print(chn_infos)
```

위와 같은 구조가 될 것 같습니다. 이를 실행하면 Stage 1에서 실습에 사용했던

```python
chn_infos = {'하트시그널2': {'hit': 20000, 'like': 3800},
            '미스터션샤인': {'hit': 18000, 'like': 3500},
            '쇼미더머니7': {'hit': 25000, 'like': 2200}}
```

와 같은 데이터가 완성될 것입니다.

그런데 예상되는 문제점이 하나 있습니다.

바로 Stage 1에서 보았던 중첩 Dictionary의 키 생성 문제입니다.

for문으로 infos 를 순회하며 추출한 채널명은 chn\_infos Dictionary에 키로 이미 존재할 수도 있고 존재하지 않을 수도 있습니다.

네이버 TV TOP 100의 구조 상 같은 채널의 클립이 여러 개 순위에 올라올 수 있기 때문입니다.

그래서 위 의사 코드와 같이 존재하지 않을 수도 있는 채널명 키에 바로 hit과 like를 더하면 분명 어딘가에서 오류가 날 것이고, 그렇다고 Stage 1의 문제를 해결하듯 chn\_infos\[chn\] 에 바로 {'hit': hit, 'like': like} 를 할당할 수도 없습니다. 이러면 이전에 있던 채널 정보에 조회수와 좋아요 수가 더해지는 것이 아니라, 새로 들어온 값으로 그냥 바뀔 뿐이기 때문입니다.

그래서 아래와 같이 해결책을 고안하였습니다.

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

먼저 모든 클립을 순회하며 Dictionary에 채널명을 키로 준비합니다. 값은 {'hit': 0, 'like': 0} 으로 초기화해둡니다.

두번째로 모든 클립을 다시 순회하며 Dictionary에 채널명 키로 접근하여 조회수와 좋아요 수를 갱신합니다. 첫 번째 순회에서 모든 채널명을 준비해두었기 때문에 키가 존재하지 않는 오류가 나지 않습니다.

## 알고리즘 구현

이제 위에서 설계한 의사 코드대로 실제 코드를 작성합니다.

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

hit과 like를 봐주세요.

최초 데이터가 '재생 수33,650', '좋아요 수1,183' 등으로 수집됩니다.

먼저 텍스트 슬라이싱을 이용해 앞의 한글 부분을 잘라내고, replace 함수로 쉼표를 빈 문자 ''로 만들며, 마지막으로 int\(\) 함수에 넣어 숫자 타입으로 바꿔주어야 합니다.

