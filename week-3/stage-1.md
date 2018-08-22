# Stage 1 - 모든 정보를 빠르게 가져오자

### 외부 라이브러리

이렇게 파이썬 개발자들이 파이썬에 자체적으로 몇 가지 클래스를 내장해 두었지만, 우리가 사용하고 싶은 여러 다양한 기능들이 모두 있을까요?

파이썬 코어 개발자는 그렇게 많지가 않기 때문에 수천만명이 사용하고 싶은 기능을 다 만들어줄 능력도, 생각도 없습니다.

대신 그들은 전 세계의 다른 개발자들에게 책임을 맡깁니다.

![](../.gitbook/assets/image%20%2859%29.png)

전 세계의 개발자들은 별 다른 계약 없이도, 파이썬을 이용해 유용한 기능들을 만들고 공유할 수 있습니다.

서로 대가없이 코드를 공유하며 자신의 작업 속도, 더 나아가 파이썬 생태계를 발전시키는 오픈 소스 문화 덕분이죠.

덕분에 위 그림의 우측에 있는 강력하고 유명한 라이브러리들처럼, 파이썬의 힘은 무한히 커질 수 있습니다.  
\(이는 다른 언어도 마찬가지입니다. 다만 메이저 언어에 속하는 파이썬은 상대적으로 오픈소스에 공헌하는 개발자들이 많겠죠?\)

위에서 클래스에 대해 먼저 소개해드린 이유는 이런 오픈소스 라이브러리들이 **클래스의 집합** 형태로 제공되기 때문입니다.  
따라서 여러분은 클래스와 함수가 제공하는 **"추상성"**에 의해 HTML 문서 요청, HTML 문서 탐색과 같은 복잡한 기능을 text.strip\(\)을 사용하듯 편하게 쓸 수 있습니다.

구체적인 내용은 requests 라이브러리를 사용하며 알아보겠습니다.

### 외부 라이브러리 설치 방법

![](../.gitbook/assets/image%20%2846%29.png)

PyCharm의 File 메뉴를 열어 Settings 를 선택합니다.

Settings 콘솔이 열리면 좌측 메뉴에서 Project: 프로젝트명 &gt; Project Interpreter를 선택합니다.

선택 후 우측의 초록색 + 버튼을 누릅니다.

![](../.gitbook/assets/image%20%28110%29.png)

이와 같은 패키지 설치 콘솔이 나올 것입니다.

검색 창에 requests를 검색하여 아래의 Install Package를 눌러줍니다.

![](../.gitbook/assets/image%20%2816%29.png)

잠시 기다리면 이렇게 녹색 알림으로 설치 완료 메세지가 나옵니다.

![](../.gitbook/assets/image%20%2897%29.png)

마찬가지로 beautifulsoup4 도 설치해 주세요. \(Stage 4에서 사용합니다.\)

### requests 라이브러리의 사용

> [https://github.com/coalastudy/python-datascrapping-code/blob/master/week2/stage3-1-requests.py](https://github.com/coalastudy/python-datascrapping-code/blob/master/week2/stage3-1-requests.py)

```python
import requests
```

먼저 파일의 최상단에 다음과 같이 requests 라이브러리를 사용하겠다는 명령을 작성합니다.

```python
req = requests.get('https://tv.naver.com/r/')
raw = req.text

print(raw)
```

아래에 다음과 같이 코드를 작성해줍니다.

requests 는 방금 들여온 외부 라이브러리이자, **웹페이지에 요청할 수 있는 기능들을 모아놓은 클래스** 입니다.

그 중 웹페이지의 내용을 요청하는 .get\(\) 함수를 사용하고, 매개 변수로 웹페이지의 주소를 입력합니다.

![](../.gitbook/assets/image%20%28121%29.png)

사용한 주소는 네이버TV TOP100 의 주소입니다.

requests.get의 결과로 얻어진 req \(변수명이므로 마음대로 지어도 괜찮습니다.\) 는 **요청의 결과 정보와 결과를 사용하는 여러 함수를 가지고 있는 또다른 클래스**입니다.

여기서 요청 결과를 단순 텍스트로 보여주는 .text 변수를 사용해 변수 raw에 저장했습니다.  
\(가공되지 않는 날 것의 텍스트라는 의미에서 이렇게 지었습니다. 변수나 함수명은 a, b 등 무의미한 단어보다 항상 의미를 알 수 있도록 짓는 것이 좋습니다.\)

이제 print 함수를 사용해 이 raw를 출력해보겠습니다.

![](../.gitbook/assets/image%20%28106%29.png)

1주차에 보던 HTML과 CSS 선택자로 이루어진 파일이 출력되었습니다!

아직 아무 의미도 갖지 못하는 정보이지만, 첫 데이터 수집에 성공한 것을 축하드립니다!

스크롤을 내려보면 분량이 엄청나게 많은 것을 알 수 있습니다.  
실제로 웹서비스를 만들 때 이렇게 많은 코드가 사용되는데요.  
다음 스테이지에서는 이 코드의 모래사장에서 바늘을 찾아보도록 하겠습니다.

## Stage 1 - 모든 정보를 빠르게 가져오자

첫 번째 스테이지에서는 배열 데이터의 요소를 일괄적으로 처리할 수 있는 반복문에 대해 배워봅니다.

수많은 데이터를 다뤄야 하는 데이터 수집 작업에서 특히 중요한 문법입니다.

또한 수집한 데이터를 1차로 가공하는 간단한 방법도 살펴보겠습니다.

### 반복문 for

{% embed data="{\"url\":\"https://github.com/coalastudy/python-datascrapping-code/blob/master/week3/stage1-0-list.py\",\"type\":\"link\",\"title\":\"coalastudy/python-datascrapping-code\",\"description\":\"Contribute to python-datascrapping-code development by creating an account on GitHub.\",\"icon\":{\"type\":\"icon\",\"url\":\"https://github.com/fluidicon.png\",\"aspectRatio\":0},\"thumbnail\":{\"type\":\"thumbnail\",\"url\":\"https://avatars2.githubusercontent.com/u/40313128?s=400&v=4\",\"width\":420,\"height\":420,\"aspectRatio\":1},\"caption\":\"코드를 참고하여 진행해 주세요.\"}" %}

2주차에 배열에 대해 소개해드렸습니다.

그리고 클립 정보를 담은 배열을 수집하여 하나의 요소에 접근해보았는데요.

이제 배열을 더 잘 사용할 수 있는 방법을 배워보겠습니다.

다음과 같이 스터디 주제들의 배열이 있습니다.

```python
subjects = ["파이썬", "페이스북 웹개발", "데이터 수집", "안드로이드", "알고리즘", "Javascript", "iOS"]
```

이는 print 함수를 통해 간단히 출력할 수 있고, \[ \] 안에 번호를 넣어 요소 하나하나도 따로 사용할 수 있습니다.

```python
print(subjects)
print(subjects[0], '/', subjects[1], '/', subjects[3])
```

![](../.gitbook/assets/image%20%2812%29.png)

그런데 이 배열 요소들 뒤에 "스터디" 를 붙여 출력해야 할 일이 생겼습니다. 어떻게 해야 할까요?

```python
print(subjects[0] + " 스터디", subjects[1] + " 스터디", subjects[2] + " 스터디", subjects[3] + " 스터디", 
      subjects[4] + " 스터디", subjects[5] + " 스터디", subjects[6] + " 스터디")
```

현재 알고있는 번호 접근법으로는 이렇게 코드를 작성해야 합니다.

하지만 이전 2주에서도 강조되었듯 코드의 중복은 최대한 피해야 합니다.

"스터디" 라는 글자를 7번 쓰면 오타를 낼 확률이 7배 증가한다는 뜻이고, 과목이 늘어날 때마다 "스터디"라는 글자를 써 주어야 하며, 스터디를 다른 단어로 바꾸려고 하면 최악의 비효율이 생기게 되죠.

여기서 사용해야 할 것이 바로 반복문입니다.

```python
for subject in subjects:
    print(subject, "스터디")
```

![](../.gitbook/assets/image%20%2896%29.png)

for 문을 작성하고 실행할 기능을 정의해 주기만 하면, subjects 배열 내의 모든 요소에 대해 같은 기능을 반복하여 실행합니다.

for subject in subjects: 는 영어 문장처럼 해석하면, "subjects 안의 subject에 대하여" 라는 뜻입니다. 여기서 subjects는 미리 정의해 둔 배열의 변수명이며, subject는 미리 정의한 것이 아니라 for 문 안에서 임시로 사용할 매개 변수입니다.

![](../.gitbook/assets/image%20%28183%29.png)

for 문 내부에서는 위와 같은 기능이 실행되고 있다고 볼 수 있습니다. subjects의 모든 요소가 한 번씩 subject에 할당되어 명령해 둔 작업을 반복 수행합니다.

```python
for 매개변수 in 배열:
    실행할 내용 1
    실행할 내용 2
    ...

실행할 내용 3
```

for문의 기본 문법은 위와 같습니다.

가장 주의할 점은, 들여쓰기 된 부분만 반복 실행할 기능으로 간주한다는 것입니다.

들여쓰기가 끝나면 for문도 끝납니다.  
위 코드를 예로 들면 내용 1과 2는 배열의 모든 요소에 대해 반복 수행되지만 내용 3은 for문의 모든 반복이 끝난 후 한 번 실행되는 일반적인 문장입니다.

### 데이터 수집에 적용

{% embed data="{\"url\":\"https://github.com/coalastudy/python-datascrapping-code/blob/master/week3/stage1-1.py\",\"type\":\"link\",\"title\":\"coalastudy/python-datascrapping-code\",\"description\":\"Contribute to python-datascrapping-code development by creating an account on GitHub.\",\"icon\":{\"type\":\"icon\",\"url\":\"https://github.com/fluidicon.png\",\"aspectRatio\":0},\"thumbnail\":{\"type\":\"thumbnail\",\"url\":\"https://avatars2.githubusercontent.com/u/40313128?s=400&v=4\",\"width\":420,\"height\":420,\"aspectRatio\":1},\"caption\":\"코드를 참고하여 진행해 주세요.\"}" %}

이제 네이버 TV TOP 100 수집 프로그램으로 돌아가 봅시다.

{% page-ref page="../week-2/stage-4.md" %}

infos 라는 배열에 4위부터 100위까지의 클립 정보가 배열로 담겨있는 상황입니다.

이 배열에 for 문을 사용하여, 각 클립 정보 HTML에서 데이터를 추출하고 출력해 봅시다.

```python
for info in infos:
    title = info.select_one('dt.title tooltip').text
    chn = info.select_one('dd.chn > a').text
    hit = info.select_one('span.hit').text
    like = info.select_one('span.like').text

    print(chn, '/', title, '/', hit, '/', like)
```

![](../.gitbook/assets/image%20%28157%29.png)

결과는 위와 같습니다.

97개의 클립 정보가 성공적으로 출력되었습니다!

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

