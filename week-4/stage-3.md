

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