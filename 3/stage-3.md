# Stage 3 - 정보를 효과적으로 가공해보자

세 번째 스테이지에서는 배열과 다른 방식으로 데이터를 모아서 저장하는 Dictionary에 대해 집중적으로 알아볼 것입니다.

많은 데이터를 다뤄야 하는 데이터 수집에서는 적절한 데이터 구조를 선택하는 것이 속도에 많은 영향을 미칩니다.

배열에서 사용하는 번호가 아니라 인터넷 계정의 ID와 비슷한 **"키 값"** 을 이용해 데이터를 저장하는 Dictionary를 사용하여 프로그램을 개선해 보겠습니다.



## 데이터 응집의 필요성

![](../.gitbook/assets/image%20%2868%29.png)

스테이지 2에서 사용했던 3개의 배열 구조입니다.

이를 이용해 만족스러운 결과를 도출하긴 했지만, 사실 우리의 목적에 가장 적합한 구조는 아닙니다.

```python
if chn in channels:
    idx = channels.index(chn)
    hits[idx] = hits[idx] + hit
    likes[idx] = likes[idx] + like
```

세 개의 배열의 같은 번호에 접근하기 위해 위와 같은 코드를 사용했었습니다. 

하지만 이는 각 배열의 길이가 같고 모두 제 위치에 있는 것이 보장될 때만 유효한 코드입니다. 

현재는 단순한 데이터를 수집하고 있기 때문에 크기나 순서에 오류가 생길 일은 없지만, 하나라도 값이 밀린다면 전체 정보가 틀어지게 됩니다.

그리고 지금은 채널마다 두 개의 추가 정보만 가지고 있지만, 유지해야 할 정보가 늘어날수록 이 취약점은 급격히 위험도가 올라가게 됩니다.



또 다른 문제도 있습니다. 채널 정보를 조회수 순으로 정렬할 필요가 생겼다고 해봅시다.

![hits &#xBC30;&#xC5F4;&#xC774; &#xC815;&#xB82C;&#xB41C; &#xC0C1;&#xD0DC;&#xC785;&#xB2C8;&#xB2E4;.](../.gitbook/assets/image%20%2896%29.png)

파이썬에 제공하는 정렬 함수를 이용해 숫자 배열 하나를 정렬하는 것은 매우 쉽습니다.   
\(Stage 4에서 다룹니다.\)

하지만 나머지 두 개의 배열도 따라서 정렬하는 것은 굉장히 어려운 문제로 이어집니다.



한 채널의 정보들을 묶어서 저장할 수 있다면 문제들을 해결할 수 있을텐데요.

다음은 배열 안에 배열도 넣을 수 있다는 점을 이용하여 이중 배열로 구성한 형태입니다.

![&#xC57D;&#xC810;&#xC774; &#xBA85;&#xBC31;&#xD558;&#xAE30; &#xB54C;&#xBB38;&#xC5D0; &#xAD73;&#xC774; &#xAD6C;&#xD604;&#xD574;&#xBCF4;&#xC9C0;&#xB294; &#xC54A;&#xC2B5;&#xB2C8;&#xB2E4;.](../.gitbook/assets/image%20%2897%29.png)

이 경우 한 번의 배열 접근으로 한 채널의 조회수와 좋아요 수를 모두 불러올 수 있습니다.

![](../.gitbook/assets/image.png)

하지만 내부 배열의 0번이 조회수, 1번이 좋아요라는 것을 어딘가에 기록해둬야만 하죠.

코드의 명확성과 가독성을 해치는 일입니다.



## Dictionary를 소개합니다

그런데 다음과 같은 데이터 구조가 있다면 어떨까요?

![](../.gitbook/assets/image%20%2816%29.png)

배열과 달리 값에 번호가 아닌 이름표가 붙어있습니다.

저장되는 데이터가 약간 늘어나긴 하지만, 코드를 쉽게 이해할 수 있게 된다는 것은 비교할 수 없는 장점입니다.

이렇게 { } 로 둘러싸여 있으며, 내부 요소가 '이름' : 값 으로 구성된 집합을 **Dictionary** 라고 합니다.

아래와 같이 사용할 수 있습니다.

```python
people = {'korean': 380, 'american': 42, 'japanese': 15}

print(people)
print(people['korean'])

people['american'] = 63

print(people['american'])
print(people['japanese'])
```

![](../.gitbook/assets/image%20%2899%29.png)

{ } 안에 '이름': 값 의 형태로 요소들을 집어넣어 만들 수 있습니다.

배열에서와 마찬가지로 \[ \] 를 사용하지만 안에 번호 대신 이름을 넣어 값을 찾아오거나 수정할 수 있습니다.



```python
members = {}

members['datascrap'] = 18
members['web'] = 24
members['python'] = 12

print(members)
```

![](../.gitbook/assets/image%20%2832%29.png)

빈 Dictionary는 { } 로 만들 수 있으며 현재 이름이 존재하지 않더라도 \[ \] 를 사용해 쉽게 새 값을 추가할 수 있습니다.



## 데이터 수집에 응용

이제 hits와 likes 배열을 Dictionary를 이용해 meta 라는 하나의 배열로 합칠 수 있습니다.  
\(metadata 는 프로그래밍 세계에서 "정보에 대한 정보" 라는 뜻으로 사용됩니다.\)

![](../.gitbook/assets/image%20%2844%29.png)

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

![](../.gitbook/assets/image%20%2889%29.png)

meta 배열의 각 Dictionary에 값들이 예쁘게 들어있군요.



## 데이터 수집에 응용 2

아직 문제가 모두 해결되지는 않았습니다.

여전히 meta 배열을 정렬해도, channels 배열을 따라서 정렬하기가 힘듭니다.

![](../.gitbook/assets/image%20%2844%29.png)

현재 채널명과 부가 정보를 별개의 배열로 저장하여  "번호" 라는 간접적인 수단으로 연결하고 있습니다. 

이것 역시 Dictionary 구조로 바꿀 수 있지 않을까요?

![&#xC608;&#xC0C1;&#xB418;&#xB294; Dictionary &#xAD6C;&#xC870;](../.gitbook/assets/image%20%2866%29.png)

![](../.gitbook/assets/image%20%2880%29.png)

새로운 클립을 처리할 때, 이름만 가지고도 기존 값에 접근할 수 있게 되는 것이죠.

channels와 meta 배열을 모두 없애고 다음과 같은 전체 채널 정보 Dictionary를 만듭니다.

```python
chnInfos = {}
```



```python
if chn in channels:
```

channels 배열이 없는데 이 부분은 어떻게 처리해야 할까요?

![](../.gitbook/assets/image%20%2866%29.png)

우리가 만드려하는 구조에서는 Dictionary의 '이름' 들이 채널명을 저장하는 channels의 역할을 대신하고 있습니다.



```python
people = {'korean': 380, 'american': 42, 'japanese': 15}

print(people.keys())
```

![](../.gitbook/assets/image%20%2875%29.png)

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

![](../.gitbook/assets/image%20%2845%29.png)

하나의 Dictionary에 각 채널의 정보가 잘 들어있습니다.

이제 예상되는 문제를 해결하였으니 다음 스테이지에서 실제로 정렬해보도록 하겠습니다.

