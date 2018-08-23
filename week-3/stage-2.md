# Stage 2 - HTML 문서에서 데이터를 추출해보자

이번 스테이지에서는 방금 가져온 HTML 문서에서 1주차에 배운 선택자 경로를 활용하여 의미를 갖는 데이터를 추출해 볼 것입니다.

이 과정에서 2주차에 학습했던 파이썬 문법들이 사용됩니다.

2주 동안 잘 따라오셨다면 어렵지 않을 거에요. 그럼 첫번째 데이터 추출을 위해 출발!

## BeautifulSoup 라이브러리를 사용해보자!

```python
import requests
from bs4 import BeautifulSoup
```

최상단의 import requests 아래에 BeautifulSoup을 불러오는 코드를 작성해줍니다.

그런데 모양이 조금 다르죠?

Stage 3에서 설치한 BeautifulSoup4 라이브러리의 변수명은 bs4이고, 이는 requests와 달리 여러 클래스들을 포함하고 있습니다.

우리가 사용할 것은 그 여러 클래스 중 BeautifulSoup 뿐이라 bs4 중 BeautifulSoup 만 불러오겠다는 의미입니다.



## 소스 코드를 살아있는 HTML로 만들기

```python
req = requests.get('https://tv.naver.com/r/')
raw = req.text

html = BeautifulSoup(raw, 'html.parser')
```

다음으로 전체 HTML 문서는 한번 확인했으니 print 하는 코드를 지우고,  
4번째 줄을 추가해줍니다.

BeautifulSoup 이라는 함수에 HTML 문서 텍스트를 담고있는 raw 변수와 또 다른 문자열을 전달했습니다.

![](../.gitbook/assets/image%20%28339%29.png)

requests로 가져온 텍스트는 사실 HTML 문서가 아닙니다.

정확히 말하면 내용은 HTML 문서가 맞지만, String 타입의 일반적인 텍스트에 불과합니다.

따라서 .strip\(\)이나 .replace\(\)와 같은 함수는 적용할 수 있지만, 내용 안의 태그명이나 클래스, ID 등이 의미를 갖지 않는 단순한 텍스트입니다.

BeautifulSoup이라는 라이브러리는 이러한 String 클래스의 값을, 살아있는 HTML 문서로 바꾸어 줍니다. 따라서 위 코드의 결과로 얻어진 html 이라는 변수는 태그 하나, 클래스 하나하나가 실제 웹페이지와 같은 의미를 가지고 있습니다.

대신 html.strip\(\) 같은 텍스트 클래스 함수는 사용하지 못하겠죠?

이렇게 변경된 HTML 클래스 변수의 기능을 사용해보면 의미가 명확해질 것입니다.



## HTML 문서에서 필요한 요소만 선택하기

```python
infos = html.select('div.cds')
```

맨 아래에 위와 같은 코드를 입력해주세요.

'div.cds' 라는 선택자 기억나시나요?

1주차 스터디를 잘 들으신 분은 **"cds 클래스를 가진 div 태그"** 라는 것을 기억하실테고, 기억력이 좋으신 분들은 이 선택자가 네이버TV TOP100의 클립 하나하나의 정보를 담고있다는 사실까지 기억하실 겁니다.



{% page-ref page="../week-1/stage-4-conditions.md" %}

기억이 잘 나지 않으신다면 꼭 1주차의 Stage 4를 복습하고 오세요!



![](../.gitbook/assets/image%20%2894%29.png)

여기있는 클립 하나하나가 div.cds에 담겨있습니다.

우리의 최종 목표는 클립의 제목, 채널명, 조회수, 좋아요 수를 가져오는 것이지만, 응집성을 고려하여 먼저 클립 전체 정보 HTML을 거점으로 하여 수집하기로 했었습니다.

바로 여기서 이 클립 전체 정보를 가져오겠습니다.

html.select\(\) 함수를 사용하면, html 변수에 담긴 HTML 문서 안에서 입력된 선택자를 갖는 요소를 리스트 타입으로 모두 가져옵니다.

```python
infos = html.select('div.cds')
```

![](../.gitbook/assets/image%20%28364%29.png)

{% hint style="info" %}
HTML변수.select\('선택자'\) 의 결과는 여러 요소가 존재할 경우를 상정하여 결과를 항상 리스트로 돌려줍니다!

결과가 하나뿐이어도 크기가 1인 리스트에 담아 돌려주니, 리스트가 아닌 요소를 찾을 때에는 아래에서 소개할 select\_one\(\) 함수를 쓰는 것이 좋습니다.
{% endhint %}

### 

## 선택된 요소 뜯어보기

이 정보들 중 하나를 출력해볼까요?

```python
print(infos[0])
```

리스트에 담긴 첫번째 값을 출력해보겠습니다.

![](../.gitbook/assets/image%20%28279%29.png)

{% hint style="info" %}
가독성을 위해 불필요한 정보를 많이 생략하였으며, 색을 입혔습니다.  
실제 결과창에는 훨씬 읽기 어려운 단색의 텍스트가 나올 것입니다.  
내용도 TOP 100이 계속 변하기 때문에 다를 것입니다.
{% endhint %}

리스트 infos의 첫 번째 값인 info\[0\]은 위와 같은 정보를 담고 있었습니다. 1주차 Stage 4에서 크롬 개발자 도구를 통해 확인한 실제 클립 정보와 같죠? 그리고 목표하고 있는 클립 제목에 점점 더 가까워지고 있습니다.

다시 **print\(infos\[0\]\) 코드를 삭제**하고 더 깊게 들어가봅시다.

### 

## 최종 목표값 추출하기

```python
clip1 = infos[0]
clip1_title = clip1.select_one('dt.title tooltip')
print(clip1_title)
```

방금 보았던 infos\[0\]의 정보를 clip1 변수에 담았습니다.

이 clip1 역시 HTML 문서이기 때문에 .select\_one\(\) 을 호출할 수 있습니다.

{% hint style="info" %}
select\_one\(\) 함수는 select\(\) 함수와 마찬가지로 HTML 요소 내에서 매개변수로 전달된 선택자를 갖는 요소를 찾아줍니다.

다른 점은 여러 개가 존재할 수 있는 요소도 맨 처음 나오는 하나만 돌려준다는 것입니다!

따라서 클립 정보 리스트와 같이 여러 개를 가져와야 할 경우는 select\(\) 함수, 클립 내의 제목같이 하나만 존재하는 값을 가져올때는 select\_one\(\) 함수를 쓰는 것이 좋습니다.
{% endhint %}

클립 제목에 도달할 수 있는 선택자 경로인 'tooltip' 을 사용하여 제목을 추출합니다.

이 결과값을 clip1\_title이라는 변수에 저장하고 출력해보았습니다.

![](../.gitbook/assets/image%20%28328%29.png)

결과는 위와 같습니다.

클립 제목을 담고 있는 tooltip 태그를 성공적으로 추출하였습니다!

그런데 데이터로서 사용하려면 태그는 필요가 없겠죠? HTML 값에 .text 로 접근하면, 태그를 제외하고 담고 있는 텍스트만 얻을 수 있습니다.

```python
print(clip1_title.text)
```

출력 코드를 위와 같이 바꿔주면

![](../.gitbook/assets/image%20%2857%29.png)

드디어 원하는 데이터만을 추출하는 데에 성공했습니다. 짝짝짝!

```python
infos = html.select('div.cds')
clip1 = infos[0]
clip1_title = clip1.select_one('dt.title tooltip')
print(clip1_title)
```

html 이후의 코드는 위와 같은데, 설명을 위해 불필요한 변수가 많이 생겼습니다.

```python
print(infos[0].select_one('dt.title tooltip').text)
```

이렇게 체이닝을 이용해 한줄로 표현해도 동일한 출력 결과를 얻을 수 있습니다.

이처럼 한번만 사용하고 말 변수는 가독성이 심하게 떨어지지 않는다면 만들지 않는 것이 좋습니다.



이제 기존에 준비했던 선택자 경로들을 이용해서 나머지 정보들도 추출해봅시다.

```python
print(clip1.select_one('tooltip').text)
print(clip1.select_one('dd.chn > a').text)
print(clip1.select_one('span.hit').text)
print(clip1.select_one('span.like').text)
```

![&#xCD9C;&#xB825; &#xACB0;&#xACFC;](../.gitbook/assets/image%20%28171%29.png)

clip1이 div.cds 선택자를 갖는 HTML이기 때문에, 서브 선택자 경로들은 div.cds 의 자손부터 작성하면 됩니다.



## 리스트 모두 추출하기

한 클립의 정보를 모두 추출해 보았으니 마지막으로 for 문을 사용하여 모든 클립의 정보를 추출해 보겠습니다.

```python
for info in infos:
    title = info.select_one('tooltip').text
    chn = info.select_one('dd.chn > a').text
    hit = info.select_one('span.hit').text
    like = info.select_one('span.like').text

    print(chn, '/', title, '/', hit, '/', like)
```

infos = html.select\('div.cds'\) 를 실행하면, 'div.cds' 선택자를 갖는 모든 HTML 요소가 infos에 리스트로 들어오게 됩니다.

즉, for문을 사용해 리스트의 각 클립 HTML에 반복하여 명령을 내릴 수 있게 된다는 뜻입니다.

각 값이 어떤 의미인지는 알아야 나중에 사용이 편하므로 먼저 데이터를 추출해 title, chn, hit, like 라는 변수에 넣어두고, print 함수를 이용해 한번에 출력해보도록 합시다.

![](../.gitbook/assets/image%20%28370%29.png)

멋지군요! 여러분은 이제 막 데이터 수집가가 되셨습니다.

