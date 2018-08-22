네이버TV 응집 배열로 하기
리스트 슬라이싱, 문자열 슬라이싱
타입 캐스팅
반복문과 조건문 연습 문제
네이버 TV 예제 리스트 응집 -> Dictionary
Dictionary의 변형과 정렬
문자열 인코딩
네이버 TV 스코어 솔루션


만들어진 데이터의 형태는 어떨까요?

우리가 지금 알고있는, 연관된 데이터끼리 모을 수 있는 구조는 **"배열"** 입니다.

배열을 이용해 위 데이터를 표현해보기로 합니다.

![](../.gitbook/assets/image%20%28177%29.png)

3개의 배열 channels, hits, likes를 가지고 있고 각각 채널명, 총 채널 조회수, 총 채널 좋아요 수를 가지고 있습니다.

각 배열의 같은 번호를 조회하면 한 채널의 정보를 모두 얻을 수 있습니다.

channels\[0\], hits\[0\], likes\[0\]을 출력하면 채널명이 '인형의 집' 이라는 것과 '인형의 집' 의 총 조회 수가 25803, 총 좋아요 수가 80이라는 것을 알 수 있겠네요.

그럼 구현을 시작해 봅시다!



## 배열에 새 값 추가하기

먼저 세 배열 channels, hits, likes는 빈 배열에서 시작합니다.

```python
channels = []
hits = []
likes = []
```

빈 배열은 이렇게 \[ \] 안에 아무것도 넣지 않으면 됩니다.



```python
for info in infos:
    chn = info.select_one('dd.chn > a').text
    hit = int(info.select_one('span.hit').text[4:].replace(',', ''))
    like = int(info.select_one('span.like').text[5:].replace(',', ''))

    channels.append(chn)
    hits.append(hit)
    likes.append(like)
```

그리고 infos를 처리하는 과정에서 얻어진 클립 정보를 준비한 세 배열에 각각 밀어넣으면 됩니다.  
\(클립명은 더 이상 필요하지 않게 되었네요. Adios...\)

.append\(\) 는 배열 클래스에 내장된 함수로, 해당 배열에 매개 변수로 전달된 값 하나를 삽입합니다.

{% hint style="info" %}
여러 개를 한번에 삽입하려면 다른 함수를 사용하여야 합니다.  
여기서는 필요하지 않은 기능이지만 관심이 있으시다면  
[https://wikidocs.net/14\#extend](https://wikidocs.net/14#extend) 를 참고해주세요.
{% endhint %}



이렇게 얻은 결과를 확인해봅시다.

```python
print(channels)
print(hits)
print(likes)
```



이러한 구조를 사용하면 infos에 담긴 모든 클립 정보를 처리할 때 다음과 같은 과정을 거칠 것입니다.

![TOP 100&#xC740; &#xD56D;&#xC0C1; &#xBCC0;&#xD558;&#xAE30;&#xC5D0; &#xB370;&#xC774;&#xD130; &#xC790;&#xCCB4;&#xB294; &#xB2E4;&#xB97C; &#xC218; &#xC788;&#xC2B5;&#xB2C8;&#xB2E4;.](../.gitbook/assets/image%20%2880%29.png)

데이터의 형태는 원하는대로 구성된 것 같습니다.

그런데 채널명이 같은 데이터가 여럿 보이네요. 모든 클립 정보를 무작정 추가했기 때문입니다.

![](../.gitbook/assets/image%20%28182%29.png)

이 두 데이터는 배열의 다른 번호에 저장되는 것이 아니라, 한 번호의 값에 합쳐져야 합니다.











### 배열 자르기

잠깐 다시 배열 예제로 돌아와 주세요.

```python
subjects = ["파이썬", "페이스북 웹개발", "데이터 수집", "안드로이드", "알고리즘", "Javascript", "iOS"]
```

우리는 이제 위와 같은 배열에서 모든 요소를 출력하거나 특정 작업에 반복적으로 이용할 수 있습니다.

그런데 이들 중 일부만 사용하고 싶다면요?

예를 들어 인기 순으로 정렬하고 상위 50%와 하위 50%로 나눈 뒤 서로 다른 분석을 하는 경우가 있겠네요.

이럴 때 앞에서부터 하나하나 \[ \] 를 사용하여 가져와야 할까요?

```python
print(subjects[0])
print(subjects[1:3])
```

물론 아닙니다. 배열의 요소 접근자 \[ \] 에는 사실 숫자가 꼭 하나만 들어가는 것은 아니랍니다.  
먼저 위 출력의 결과를 볼까요?

![](../.gitbook/assets/image%20%28180%29.png)

이렇게 \[ \] 안에 1:3 이라는 값을 넣으면, 배열의 1번부터 3번 **이전** 값까지 선택됩니다.  
\(주의!! 3번은 선택되지 않습니다.\)

이처럼 범위 지정에 의해 선택된 값은, 그 부분을 잘라낸 또 다른 배열이 됩니다.

```python
upper = subjects[0:3]

for subject in upper:
    print("[인기]", subject)
```

따라서 위와 같이 잘라낸 후 for문을 사용하거나, 또 다시 잘라내는 것도 물론 가능합니다.

![](../.gitbook/assets/image%20%2838%29.png)

좀 더 발전된 사용방법도 알아볼까요?

```python
print(subjects[:3])   # 처음 ~ 2번
print(subjects[3:])   # 3번 ~ 끝
print(subjects[:])    # 처음 ~ 끝
```

범위 지정자의 숫자들은 이렇게 생략이 가능합니다. 시작 지점을 생략하면 "처음부터"라는 뜻이고, 끝 지점을 생략하면 "끝까지"라는 뜻이 됩니다.

![](../.gitbook/assets/image%20%28204%29.png)

```python
print(subjects[-1])    # 끝에서 첫번째
print(subjects[-3:])   # 끝에서 두개
print(subjects[2: -2]) # 2번째부터 끝에서 2번째까지
```

숫자에 음수를 쓰는 것도 가능합니다. 순서에 음수는 없기 때문에 자동적으로 끝에서부터 ~번째라는 것으로 해석됩니다.

![](../.gitbook/assets/image%20%28207%29.png)

### 데이터 수집에 적용 2

신기하긴한데, 갑자기 왜 다뤘는지 궁금하셨죠?

사실 문자열 값은 배열의 일종입니다. 예를 들면

```python
str = 'string!'

for s in str:
    print(s)
```

이렇게 문자열을 for문에 넣고 출력하여 테스트해볼 수 있습니다.

![](../.gitbook/assets/image%20%2895%29.png)

결과는 위와 같습니다. 즉 문자열은 단일 문자들로 이루어진 배열이라고 볼 수 있겠네요.

이러한 특성에 의해 문자열의 편집에 배열 자르기를 사용할 수가 있습니다.

네이버 TV TOP 100 코드에서

```python
for info in infos:
    ...
    hit = info.select_one('span.hit').text
    like = info.select_one('span.like').text

    print(chn, '/', title, '/', hit, '/', like)
```

이 부분은 아래와 같이 출력됩니다.

![](../.gitbook/assets/image%20%2865%29.png)

여기서 숫자값만 사용하기 위해 문자열을 잘라내 봅시다. \(제목과 채널은 생략하였습니다.\)

```python
for info in infos:
    ...
    hit = info.select_one('span.hit').text[4:]
    like = info.select_one('span.like').text[5:]

    print(chn, '/', title, '/', hit, '/', like)
```

hit은 "재생 수"라는 텍스트가 끝나고 숫자가 나오는 4번부터, likes는 "좋아요 수"가 끝나고 숫자가 나오는 5번부터 잘라서 쓰도록 수정하였습니다.

![](../.gitbook/assets/image%20%2820%29.png)

의도대로 잘 사라졌네요. 불필요하게 중복되는 텍스트를 제거하고, 값을 숫자로 사용할 수 있도록 준비하는 과정입니다.

마지막으로 쉼표를 제거해 보겠습니다.

```python
...
hit = info.select_one('span.hit').text[4:].replace(',', '')
like = info.select_one('span.like').text[5:].replace(',', '')
...
```

![](../.gitbook/assets/image%20%2850%29.png)

2주차에서 다룬 적이 있는 replace\(\) 함수를 이용해, 텍스트에서 ', '가 존재하는 부분을 ''\(빈 문자열\)로 교체하도록 하였습니다.

이렇게 for문을 이용하여 수집한 데이터 덩어리에서 필요한 원료들을 모두 뽑아내고, 텍스트를 편집하여 더 가치있게 사용할 수 있도록 준비하는 작업을 마쳤습니다.






















## 타입 변환

다음 예를 한 번 봅시다.

```python
print('20' + '40')
print(20 + 40)
```

![](../.gitbook/assets/image%20%28222%29.png)

아하! 문자열 타입의 숫자는 더하면 그냥 연결된 결과가 나오는군요.

그렇다면 우리의 저 어마어마한 결과값 역시 이런 오류때문에 나온 거라고 추측해볼 수 있겠습니다. 

실제로 HTML 문서의 모든 값은 문자열\(String\) 입니다. 

앞에 붙은 "조회수" 같은 글자를 떼거나 중간의 쉼표를 제거하는 작업도 해주었지만, 자동으로 숫자로 변하지는 않는 것이죠.

```text
print(int('20') + int('40'))
```

문자열 타입의 숫자는 int\(\) 라는 내장함수를 사용해 간단히 실제 숫자로 바꾸어줄 수 있습니다.   
\(integer\[정수\] 의 약자입니다.\) 

단, 순수한 숫자만을 포함하지 않은 문자열을 매개변수로 쓸 경우 오류가 생깁니다. 쉼표도 안됩니다!



```python
hit = int(info.select('span.hit')[0].text[4:].replace(',', ''))
like = int(info.select('span.like')[0].text[5:].replace(',', ''))
```

이제 hit과 like를 구할 때 숫자로 변환해 준다면 모든 문제가 해결될 것 같습니다.

![](../.gitbook/assets/image%20%28154%29.png)

이제 정상적으로 결과가 나오는 것을 볼 수 있습니다. 

복면가왕이나 뮤직뱅크의 조회수는 오류가 아닌가 싶기도 하지만 실제 값이 맞습니다!









# Challenge 1

## 도전과제 1

첫번째 도전과제에서는 방금 배운 반복문과 조건문을 다시 연습해보겠습니다.

```python
numbers = [13, 26, 145, 796, 2411, 4, 78, 532, 22, 734, 1633, 29,
           8, 17, 792, 15, 2, 273, 453, 546, 724, 298, 47, 35, 7]
```



위와 같은 배열이 있을 때 배열에 담긴 숫자들을 짝수만 담긴 배열과 홀수만 담긴 배열로 나누고 출력해보세요.

Self hint

1. 어떤 수 n이 짝수임을 검사하는 문장은 같음비교를 사용하여 n % 2 == 0 입니다. \(2로 나눈 나머지가 0과 같다.\)
2. 먼저 반복문과 조건문이 어떤 구조를 이뤄야 하는지 생각해보세요.
3. 각 배열에 숫자를 담으려면 어떻게 해야 할까요?












# Stage 3 - 정보를 효과적으로 가공해보자

세 번째 스테이지에서는 배열과 다른 방식으로 데이터를 모아서 저장하는 Dictionary에 대해 집중적으로 알아볼 것입니다.

많은 데이터를 다뤄야 하는 데이터 수집에서는 적절한 데이터 구조를 선택하는 것이 속도에 많은 영향을 미칩니다.

배열에서 사용하는 번호가 아니라 인터넷 계정의 ID와 비슷한 **"키 값"** 을 이용해 데이터를 저장하는 Dictionary를 사용하여 프로그램을 개선해 보겠습니다.






## 데이터 수집에 응용

이제 hits와 likes 배열을 Dictionary를 이용해 meta 라는 하나의 배열로 합칠 수 있습니다.  
\(metadata 는 프로그래밍 세계에서 "정보에 대한 정보" 라는 뜻으로 사용됩니다.\)

![](../.gitbook/assets/image%20%2876%29.png)

```python
meta = []
```

먼저 hits와 likes 배열 정의를 지우고 meta라는 하나의 배열로 바꿉니다.



```python
if chn in channels:
    idx = channels.index(chn)
    meta[idx]['hit'] = meta[idx]['hit'] + hit
    meta[idx]['like'] = meta[idx]['like'] + like
```

이제 hit과 like 배열을 따로 수정하지 않고, meta 배열의 해당 번호로 찾은 Dictionary에서 이름을 통해 값을 수정합니다.



```python
else:
    channels.append(chn)
    meta.append({'hit': hit, 'like': like})
```

채널이 존재하지 않는 경우에도 meta 배열에 새로운 Dictionary를 만들어 추가합니다.



```python
for channel in channels:
    idx = channels.index(channel)
    print(channel, '/', meta[idx])
```

이제 두 개의 배열만 출력해도 모든 채널 정보를 볼 수 있습니다.

![](../.gitbook/assets/image%20%28172%29.png)

meta 배열의 각 Dictionary에 값들이 예쁘게 들어있군요.



## 데이터 수집에 응용 2

아직 문제가 모두 해결되지는 않았습니다.

여전히 meta 배열을 정렬해도, channels 배열을 따라서 정렬하기가 힘듭니다.

![](../.gitbook/assets/image%20%2876%29.png)

현재 채널명과 부가 정보를 별개의 배열로 저장하여  "번호" 라는 간접적인 수단으로 연결하고 있습니다. 

이것 역시 Dictionary 구조로 바꿀 수 있지 않을까요?

![&#xC608;&#xC0C1;&#xB418;&#xB294; Dictionary &#xAD6C;&#xC870;](../.gitbook/assets/image%20%28119%29.png)

![](../.gitbook/assets/image%20%28156%29.png)

새로운 클립을 처리할 때, 이름만 가지고도 기존 값에 접근할 수 있게 되는 것이죠.

channels와 meta 배열을 모두 없애고 다음과 같은 전체 채널 정보 Dictionary를 만듭니다.

```python
chnInfos = {}
```



```python
if chn in channels:
```

channels 배열이 없는데 이 부분은 어떻게 처리해야 할까요?

![](../.gitbook/assets/image%20%28119%29.png)

우리가 만드려하는 구조에서는 Dictionary의 '이름' 들이 채널명을 저장하는 channels의 역할을 대신하고 있습니다.



```python
people = {'korean': 380, 'american': 42, 'japanese': 15}

print(people.keys())
```

![](../.gitbook/assets/image%20%28137%29.png)

Dictionary 클래스에 내장된 .keys\( \) 라는 함수를 이용하면, Dictionary의 key\('이름'\) 값들만 모아 배열로 되돌려 줍니다.

이 배열이 channels와 완전히 같으니 대신 사용할 수 있겠습니다.

```python
for info in infos:
    chn = info.select('dd.chn > a')[0].text
    hit = int(info.select('span.hit')[0].text[4:].replace(',', ''))
    like = int(info.select('span.like')[0].text[5:].replace(',', ''))

    if chn in chnInfos.keys():
        chnInfos[chn]['hit'] += hit
        chnInfos[chn]['like'] += like
    else:
        chnInfos[chn] = {'hit': hit, 'like': like}
```

channels와 meta로 나뉜 구조에서, chnInfos에 모든 것을 같이 저장하는 구조가 되었습니다.

출력하여 확인해봅시다.

```python
for chnInfo in chnInfos.items():
    print(chnInfo)
```

\(.items\( \) 함수는 Stage 4에서 설명합니다.\)

![](../.gitbook/assets/image%20%2877%29.png)

하나의 Dictionary에 각 채널의 정보가 잘 들어있습니다.

이제 예상되는 문제를 해결하였으니 다음 스테이지에서 실제로 정렬해보도록 하겠습니다.






























## Dictionary의 여러가지 변형

Dictionary 타입은 배열과 달리 키 - 값 쌍으로 이루어진 데이터의 모음이고, 배열처럼 순서를 통해 사용하는 것이 아니라 키를 통해 사용하는 것이라고 배웠습니다.

하지만 정렬해보니 키의 배열이 사용됨을 알 수 있었습니다.

다음과 같은 경우도 비슷합니다.

```python
for score in scores:
    print(score)
```

![](../.gitbook/assets/image%20%28115%29.png)

배열 내의 요소를 반복해주는 for문에 Dictionary를 사용하였더니, 키의 배열을 사용하여 반복함을 알 수 있습니다.



```python
for key in scores:
    print(key, scores[key])
```

![](../.gitbook/assets/image%20%2888%29.png)

이를 잘 이해하면 이런 방식으로 이용하는 것도 가능합니다. 반복되는 키를 사용하여 scores에서 키가 가지고 있는 값을 함께 출력해줍니다.

이렇게 Dictionary를 sorted\( \), for문 등 배열이 사용되어야 하는 곳에 사용하면 키의 배열이 대신 사용되는 것을 알 수 있습니다.

그런데 이는 직관적이지 못한 부분입니다. Dictionary 변수명을 넣었는데 갑자기 키 배열이 된다니요? 

문법이 그렇다니 이해는 하겠지만, 코드의 가독성이 떨어지는 것은 인정해야 합니다.

이를 다음과 같이 대체할 수 있습니다.

```python
print(scores.keys())
```

![](../.gitbook/assets/image%20%28129%29.png)

.keys\( \)는 Dictionary 클래스의 내장 함수로, Dictionary에서 키의 배열을 뽑아 돌려줍니다. 

위에서 테스트한 sorted나 for문에 scores 대신 scores.keys\( \)를 사용하면, 결과는 같지만 Dictionary의 키 배열이 사용된다는 사실을 명백히 알 수 있습니다.  
\(배열 앞에 붙은 dict\_keys는 무시하셔도 좋습니다.\)



키 배열을 돌려주는 함수가 있으니 값 배열을 돌려주는 함수도 있을거라고 추측해 볼 수 있겠지요?

```python
print(scores.values())

print(sorted(scores.values()))
```

.values\( \) 함수는 Dictionary에서 값만을 뽑아 배열로 돌려줍니다. 

배열이니 이를 정렬하여 출력할 수도 있습니다.

![](../.gitbook/assets/image%20%28138%29.png)



값도 정렬할 수 있게 되었으니 좋은데, 키와 값이 분리된 상태로 존재하는 것이 맘에 들지 않습니다. 

Dictionary의 키와 값은 떼어놓을 수 없는 관계이기 때문입니다. 

실제로 위에서 점수를 정렬했지만 이 점수가 누구의 것인지는 알 수가 없습니다.

이럴 때는 다음과 같은 함수를 사용할 수 있습니다.

```python
print(scores.items())
```

![](../.gitbook/assets/image%20%2821%29.png)

.items\( \)는 Dictionary 클래스의 내장함수로, Dictionary의 키-값 데이터들을 튜플의 배열로 만들어 돌려줍니다.

{% hint style="info" %}
**튜플\(tuple\)**은 배열과 비슷하나 \[ \]가 아닌 \( \)로 둘러싸여있는 값의 모음입니다.   
배열과의 유일한 차이는 내부 값을 수정할 수 없다는 것입니다.
{% endhint %}

이렇게 돌려받은 결과는 더 이상 Dictionary가 아니기 때문에 키를 사용하여 접근할 수 없지만, 순서를 사용해 \(키, 값\) 튜플에 접근할 수 있고 sorted, for문 등에 사용할 수 있습니다.



```python
print(sorted(scores.items()))
```

![](../.gitbook/assets/image%20%2864%29.png)

scores.items\(\) 를 정렬하니 키를 기준으로 정렬되었습니다. 

\('a', 65\)와 \('b', 24\)를 비교할 기준이 명확하지 않으니, 더 중요한 값인 키를 우선시하는 것입니다.





























## 문자열 인코딩

주의력이 깊으신 분들은 종종 인터넷 주소에서 %20%41 등 %가 많이 들어가있는 부분을 본 적이 있으실 것입니다.

본적이 없다면 네이버 뉴스에 검색한 주소를 복사해 메모장 등 다른 편집기에 붙여넣어 보세요.

![](../.gitbook/assets/image%20%28131%29.png)

한글 부분이 아래 주소와 같이 자동으로 바뀝니다.

이는 인터넷 주소에 기본적으로 ASCII 문자만 사용할 수 있기 때문입니다. ASCII 문자란 숫자와 영문 대소문자, 기타 자주 사용되는 특수문자 등을 모은 128개의 문자 집합입니다. 

따라서 그 외의 문자는 주소에 사용될 때 16진수로 번호를 매겨 %95, %EC 등으로 대체됩니다.

우리는 이 변환을 직접 해서 코드에 입력할 수 없습니다. 변환 규칙도 모르거니와 안다고 하더라도 너무 비효율적인 작업이죠.



이런 작업은 당연히 라이브러리가 존재할 것입니다.

```python
from urllib import request, parse
from bs4 import BeautifulSoup
```

urllib에서 request 뿐만 아니라 parse라는 클래스도 import합니다.

```python
print(parse.quote('아시안게임'))
```

이 parse의 quote라는 함수를 사용하면 손쉽게 한글 문자열을 ASCII로 인코딩한 결과를 얻을 수 있습니다.

![](../.gitbook/assets/image%20%28100%29.png)



```python

from urllib import request, parse
from bs4 import BeautifulSoup

raw = request.urlopen('https://search.naver.com/search.naver?&where=news&query=' + parse.quote('아시안게임') + '&start=1')
html = BeautifulSoup(raw, 'html.parser')

list = html.select('.type01 dl')

for article in list:
    journal = article.select_one('span._sp_each_source').text
    title = article.select_one('dt').text

    print(journal, title)
```

이제 주소의 '아시안게임' 부분을 직접 입력하지 않고 인코딩한 결과를 사용하도록 바꾸어주면, 정상적으로 데이터가 수집되는 것을 볼 수 있습니다.

![](../.gitbook/assets/image.png)





























# 모범 답안

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
    score = (like * 350 + hit) / 100

    if chn not in chnInfos.keys():
        chnInfos[chn] = {'hit': hit, 'like': like, 'score': score}
    else:
        chnInfos[chn]['hit'] += hit
        chnInfos[chn]['like'] += like
        chnInfos[chn]['score'] += score


def sortKey(item):
    return item[1]['score']


sortedList = sorted(chnInfos.items(), key=sortKey, reverse=True)

for sortedInfo in sortedList:
    print(sortedInfo)
```

비중 상수 C를 350으로 하여 계산하고 단위가 너무 커서 100으로 나누어 주었습니다. 22행과 26행에서 계산한 score를 dictionary에 추가해주고 있고, 30행에서는 정렬 함수의 기준을 score로 바꾸어 주었습니다.

