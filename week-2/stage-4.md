# Stage 4 - 리스트를 더 깊게 다루어보자

보통 수집한 데이터는 리스트 형태로 들어오기 때문에


## BeautifulSoup 라이브러리를 사용해보자!

# Stage 1 - 모든 정보를 빠르게 가져오자

첫 번째 스테이지에서는 배열 데이터의 요소를 일괄적으로 처리할 수 있는 반복문에 대해 배워봅니다.

수많은 데이터를 다뤄야 하는 데이터 수집 작업에서 특히 중요한 문법입니다.

또한 수집한 데이터를 1차로 가공하는 간단한 방법도 살펴보겠습니다.



## 반복문 for

{% embed data="{\"url\":\"https://github.com/coalastudy/python-datascrapping-code/blob/master/week3/stage1-0-list.py\",\"type\":\"link\",\"title\":\"coalastudy/python-datascrapping-code\",\"description\":\"Contribute to python-datascrapping-code development by creating an account on GitHub.\",\"icon\":{\"type\":\"icon\",\"url\":\"https://github.com/fluidicon.png\",\"aspectRatio\":0},\"thumbnail\":{\"type\":\"thumbnail\",\"url\":\"https://avatars2.githubusercontent.com/u/40313128?s=400&v=4\",\"width\":420,\"height\":420,\"aspectRatio\":1},\"caption\":\"코드를 참고하여 진행해 주세요.\"}" %}



2주차에 배열에 대해 소개해드렸습니다.

그리고 클립 정보를 담은 배열을 수집하여 하나의 요소에 접근해보았는데요.

{% page-ref page="../week-2/stage-1.5.md" %}

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

for subject in subjects:  는 영어 문장처럼 해석하면, "subjects 안의 subject에 대하여" 라는 뜻입니다. 여기서 subjects는 미리 정의해 둔 배열의 변수명이며, subject는 미리 정의한 것이 아니라 for 문 안에서 임시로 사용할 매개 변수입니다.

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
