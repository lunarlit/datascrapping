# Stage 4 - 가져온 문서에서 데이터를 추출해보자

이번 스테이지에서는 1, 2주차에서 배운 모든 지식을 합쳐, 첫 번째 데이터를 추출해봅니다.

2주 동안 잘 따라오셨다면 어렵지 않을 거에요!

## BeautifulSoup 라이브러리를 사용해보자!

```python
import requests
from bs4 import BeautifulSoup
```

최상단의 import requests 아래에 BeautifulSoup을 불러오는 코드를 작성해줍니다.

그런데 모양이 조금 다르죠?

Stage 3에서 설치한 BeautifulSoup4 라이브러리의 변수명은 bs4이고, 이는 requests와 달리 여러 클래스들을 포함하고 있습니다.

우리가 사용할 것은 그 여러 클래스 중 BeautifulSoup 뿐이라 bs4 중 BeautifulSoup 만 불러오겠다는 의미입니다.

```python
req = requests.get('https://tv.naver.com/r/')
raw = req.text

html = BeautifulSoup(raw, 'html.parser')
```

다음으로 전체 HTML 문서는 한번 확인했으니 print 하는 코드를 지우고,  
4번째 줄을 추가해줍니다.

BeautifulSoup 이라는 함수에 HTML 문서 텍스트를 담고있는 raw 변수와 또 다른 문자열을 전달했습니다.

![](../.gitbook/assets/image%20%28128%29.png)

requests로 가져온 텍스트는 사실 HTML 문서가 아닙니다.

정확히 말하면 내용은 HTML 문서가 맞지만, String 클래스에 속하는 일반적인 텍스트에 불과합니다.

따라서 .strip\(\)이나 .replace\(\)와 같은 함수는 적용할 수 있지만, 내용 안의 태그명이나 클래스, ID 등은 아무런 의미가 없는 단순한 텍스트밖에 되지 않습니다.

BeautifulSoup이라는 라이브러리는 이러한 String 클래스의 값을, 살아있는 HTML 문서로 바꾸어 줍니다.   
따라서 위 코드의 결과로 얻어진 html 이라는 변수는 태그 하나, 클래스 하나하나가 실제 웹페이지와 같은 의미를 가지고 있습니다.

대신 html.strip\(\) 은 사용하지 못하겠죠?

이렇게 변경된 HTML 클래스 변수의 기능을 사용해보면 의미가 명확해질 것입니다.

```python
infos = html.select('div.cds')
```

맨 아래에 위와 같은 코드를 입력해주세요.

'div.cds' 라는 것 기억나시나요?

1주차 스터디를 잘 들으신 분은 **"cds 클래스를 가진 div 태그"** 라는 것을 기억하실테고, 기억력이 좋으신 분들은 이 선택자가 네이버TV TOP100의 클립 하나하나의 정보를 담고있다는 사실까지 기억하실 겁니다.

기억이 잘 나지 않으신다면 꼭 1주차의 Stage 4를 복습하고 오세요!

{% page-ref page="../week-1/stage-4.md" %}

![&#xC5EC;&#xAE30;&#xC788;&#xB294; &#xD074;&#xB9BD; &#xD558;&#xB098;&#xD558;&#xB098;&#xAC00; div.cds&#xC5D0; &#xB2F4;&#xACA8;&#xC788;&#xC2B5;&#xB2C8;&#xB2E4;.](../.gitbook/assets/image%20%28119%29.png)

html.select\(\) 함수를 사용하면, html 변수에 담긴 HTML 문서 안에서 매개변수로 전달된 선택자를 갖는 요소만 선택됩니다.

![](../.gitbook/assets/image%20%2815%29.png)

{% hint style="info" %}
someHTML.select\('someSelector'\) 의 결과는 여러 요소가 존재할 경우를 상정하여 항상 배열로 돌려줍니다!
{% endhint %}

이 정보들 중 하나를 출력해볼까요?

```python
print(infos[0])
```

배열에 담긴 첫번째 값을 출력해보겠습니다.

![](../.gitbook/assets/image%20%2835%29.png)

{% hint style="info" %}
가독성을 위해 불필요한 정보를 많이 생략하였으며, 색을 입혔습니다.  
실제 결과창에는 훨씬 읽기 어려운 단색의 텍스트가 나올 것입니다.  
내용도 TOP 100이 계속 변하기 때문에 다를 것입니다.
{% endhint %}

배열 infos의 첫 번째 값인 info\[0\]은 위와 같은 정보를 담고 있었습니다.  
1주차 Stage 4에서 크롬 개발자 도구를 통해 확인한 실제 클립 정보와 같죠?  
그리고 목표하고 있는 클립 제목에 점점 더 가까워지고 있습니다.

다시 **print\(infos\[0\]\) 코드를 삭제**하고 더 깊게 들어가봅시다.

```python
clip1 = infos[0]
clip1_title = clip1.select('dt.title tooltip')[0]
print(clip1_title)
```

방금 보았던 infos\[0\]의 정보를 clip1 변수에 담았습니다.

이 clip1 역시 HTML 문서이기 때문에 .select\(\) 를 호출할 수 있습니다.

클립 제목에 도달할 수 있는 선택자 경로인 'dt.title tooltip' 을 사용하고, 결과가 하나뿐이더라도 항상 배열로 나오기 때문에 \[0\]을 붙여 결과값에 접근해줍니다.

이 결과값을 clip1\_title이라는 변수에 저장하고 출력해보았습니다.

![](../.gitbook/assets/image%20%2845%29.png)

결과는 위와 같습니다.

클립 제목을 담고 있는 tooltip 태그를 성공적으로 추출하였습니다!

그런데 데이터로서 사용하려면 태그는 필요가 없겠죠? HTML 값에 .text 로 접근하면, 태그를 제외하고 담고 있는 텍스트만 얻을 수 있습니다.

```python
print(clip1_title.text)
```

출력 코드를 위와 같이 바꿔주면

![](../.gitbook/assets/image%20%2844%29.png)

드디어 원하는 데이터를 처음으로 추출하는 데에 성공했습니다!

```python
infos = html.select('div.cds')
clip1 = infos[0]
clip1_title = clip1.select('dt.title tooltip')[0]
print(clip1_title)
```

html 이후의 코드는 위와 같은데, 설명을 위해 불필요한 변수가 많이 생겼습니다.

```python
print(infos[0].select('dt.title tooltip')[0].text)
```

이렇게 한줄로 표현해도 동일한 출력 결과를 얻을 수 있습니다.  
중간 변수에 저장하지 않고, 값에서 직접 접근하는 방법입니다.

이렇게 한번만 사용하고 말 변수는 만들지 않는 것도 좋은 방법입니다.

