# Stage 2 - 정보를 의미있게 가공해보자

두 번째 스테이지에서는 준비된 클립별 정보를 취합하여 채널별로 정리합니다.

이는 네이버TV TOP100 페이지에서 제공되지 않는 정보입니다.

데이터 수집 클래스이긴 하지만, 수집한 데이터를 다시 손으로 정리해야 한다면 반쪽짜리 도움밖에 되지 않는 것이기 때문에 이러한 내용도 다루게 되었습니다.

데이터 분석 이론을 다루는 것은 아니지만, 여기에서 다루는 문법들을 손에 익힌다면 다른 이론의 적용은 쉬워질 것입니다.

## 목표 설정 및 설계

![](../.gitbook/assets/image%20%2840%29.png)

현재 데이터는 클립별로 배열에 들어있습니다. 우리는 TV 트렌드 분석 업무를 하고 있는 상황이며, 클립별 데이터를 가공하여 프로그램의 인기도를 알아보려 합니다.

따라서 클립별 데이터를 채널별로 모아야 합니다.

![](../.gitbook/assets/image%20%2893%29.png)

현재 만들어내려는 데이터의 모습입니다.

TOP 100의 클립 데이터 중 같은 채널끼리 모아 조회수, 좋아요 수를 더하는 것입니다.



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



## 조건문 if에 대해 배워보자

그렇다면 for문에서 새로 처리할 클립이 들어왔을 때 두 가지 경우가 생깁니다.

1. 이미 배열에 해당 채널의 정보가 있는 상황이라면 - 해당 번호에 조회수와 좋아요 수를 더한다.
2. 배열에 해당 채널의 정보가 없다면 - 채널명, 조회수, 좋아요 수를 각 배열에 추가한다.

이렇게 현재 상태에 따라 다른 코드를 실행해야 하는 경우 조건문 if가 사용됩니다.

```python
if 채널이 channels에 존재하면:
    hits의 해당 부분에 hit 더하기
    likes의 해당 부분에 like 더하기
```

for문과 비슷한 구조로 이루어져 있네요. if 다음에 등장하는 조건이 만족될 때에만 들여쓰기한 부분의 코드가 실행됩니다.

사용되는 조건은 **"참"** 혹은 **"거짓"** 둘 중 하나가 되는 문장이어야 합니다.

사용할 수 있는 문장은 다음과 같습니다.

### 같음 비교 문장

어떤 변수나 값이 조건과 일치할 때를 검사합니다.

```python
a = 1
b = 2

if a == 1:
    print('a가 같다') 
    
if b == 1:
    print('b가 같다')
```

### 다름 비교 문장

어떤 변수나 값이 조건과 다를 때를 검사합니다.

```python
a = '코알라'
b = '파이썬'

if a != '파이썬':
    print('a가 다르다')
if b != '파이썬':
    print('b가 다르다')
```

### 종속 비교 문장

어떤 변수나 값이 배열 안에 들어있는지 / 들어있지 않은지 검사합니다.

```python
arr = [1, 3, 5, 7, 9]

if 1 in arr:
    print('1이 있다')

if 2 not in arr:
    print('2가 없다')
```



여기서는 channels 배열에 현재 조사중인 클립의 채널이 들어있는지 검사하여야 하기 때문에 종속 비교 문장을 사용합니다.

```python
for info in infos:
    chn = info.select('dd.chn > a')[0].text
    hit = int(info.select('span.hit')[0].text[4:].replace(',', ''))
    like = int(info.select('span.like')[0].text[5:].replace(',', ''))

    if chn in channels:
        idx = channels.index(chn)
        hits[idx] = hits[idx] + hit
        likes[idx] = likes[idx] + like
```

채널명 chn이 배열 channels에 들어있는 경우를 처리합니다.

.index\( \) 함수는 배열 클래스에 내장된 함수로, 매개변수로 전달된 값의 번호를 돌려줍니다.

이 번호를 이용해 hits 배열과 likes 배열의 해당 부분을 수정할 수 있습니다.

이제 새로 들어온 클립의 채널 정보가 존재하지 않을 때도 처리해 봅시다.





```python
if chn in channels: # 만약 channels 배열에 chn이 들어있으면
    ...

else:               # 들어있지 않으면 
    channels.append(chn)
    hits.append(hit)
    likes.append(like)
```

else는 "만약 그렇지 않다면" 이라는 뜻으로, if문과 짝을 이루어 사용합니다.

if문이 실행되지 않는 조건일 경우, 아래의 else 문이 실행됩니다.  
\(else문이 항상 있어야 하는 것은 아닙니다. else 문이 없으면 아무것도 실행되지 않습니다.\)

채널 정보가 없는 경우 처음에 했던대로 그냥 새로 추가하기만 하면 됩니다.



이제 결과를 출력해봅시다.

.index\( \) 함수를 사용할 수 있으니 다음과 같이 보기 좋게 출력하는 것이 가능합니다.

```python
for channel in channels:
    idx = channels.index(channel)
    print(channel, '/', hits[idx], '/', likes[idx])
```

세 배열을 따로 출력하는 것이 아니라, 같은 번호끼리 묶어 출력하게 됩니다. 서로 길이가 달라 불편했던 기존 출력방식보다 훨씬 보기 좋습니다.

![](../.gitbook/assets/image%20%28164%29.png)

그런데 출력 결과가 이게 뭐죠? 숫자가 이렇게 클 수가 있나요? 온 우주의 생명체가 모두 좋아요를 눌러도 이만큼은 안나올텐데요.



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

