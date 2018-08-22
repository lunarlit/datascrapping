

# Stage 4 - 정제된 데이터에서 가치를 창출해보자

이번 스테이지에서는 정렬에 대해 중점적으로 알아봅니다.

내림차순 정렬하여 큰 값들을 찾는 것은 수많은 숫자 데이터들 중 비중을 높게 가져가야할 데이터를 찾는 좋은 방법 중 하나입니다.

파이썬의 함수 sorted의 사용법을 배우고, Dictionary를 여러가지 형태로 변형시켜 정렬에 이용해봅니다.

## sorted\(\) 함수를 사용해보자.

sorted\( \) 함수는 파이썬의 내장 함수로써, 배열 등 반복 가능한 타입의 값을 오름차순으로 정렬하여 돌려주는 함수입니다.

```python
numbers = [94, 23, 64, 39, 25, 10, 63, 6, 234, 34, 63, 4, 86, 5, 24, 1, 631, 90]

print(sorted(numbers))
```

![](../.gitbook/assets/image%20%2843%29.png)

위와 같은 숫자 배열 정도는 간단히 정렬하여 출력할 수 있습니다.

주의사항은 함수의 결과값\(return\) 으로 정렬된 배열을 돌려주는 것이지, 기존 배열 자체를 정렬시키는 것은 아니라는 점입니다.

```python
print(numbers)
```

실제로 정렬 함수를 실행한 이후 위와 같이 배열을 출력해보면 값이 그대로임을 확인할 수 있습니다.



그렇다면 숫자가 아닌 문자는 어떨까요?

```python
alphabets = ['r', 'f', 'w', 'b', 'z', 'n', 'm', 'q', 'i', 'y', 'c']

print(sorted(alphabets))

words = ['coffee', 'car', 'carpet', 'candy', 'cure', 'crisis', 'cucumber']

print(sorted(words))
```

![](../.gitbook/assets/image%20%28107%29.png)

위와 같이 문자나 문자열도 사전순\(Lexicographic\) 으로 훌륭하게 정렬하는 모습입니다.



```python
kwords = ['가방', '가면', '가림막', '군인', '누빔', '가판대', '가생이', '경찰', '기업']

print(sorted(kwords))
```

![](../.gitbook/assets/image%20%2831%29.png)

심지어 한글 단어조차 정렬이 되는 모습입니다.



```python
mixed = [3, '호날두', 'Python', 15, '메시', 'Data']

print(sorted(mixed))
```

![](../.gitbook/assets/image%20%2874%29.png)

숫자와 문자열이 섞인 배열으로도 테스트 해보았더니 다음과 같은 오류가 발생했습니다.

str \(String = 문자열\) 과 int \(Integer = 정수\) 타입의 값 사이에서는 &lt; 연산이 지원되지 않는다는 뜻입니다.

숫자와 문자열은 인간의 생각으로도 어떻게 정렬해야 할지 알 수 없으니 에러가 나는 것이 타당할 것입니다.



sorted\( \)는 이렇게 배열에 대해서는 간편하고 정확하게 정렬을 해주는 함수입니다.

그런데 우리는 Stage 3에서 만든 채널별 데이터를 정렬하고 싶습니다.

sorted\( \) 함수를 Dictionary 타입에도 사용할 수 있을까요?

```python
scores = {'h': 16, 'b': 24, 'd': 91, 'c': 138, 'z': 6, 'a': 65}

print(sorted(scores))
```

위와 같이 간단한 키-값 Dictionary를 만들어 정렬해보았습니다. 키는 한 자리 알파벳이고 값은 숫자인데요. 어떤 값을 기준으로 정렬이 될까요? 아니면 에러가 날까요?

![](../.gitbook/assets/image%20%28139%29.png)

결과는 위와 같습니다.

값은 사라지고 키만 정렬된 배열을 돌려주었네요.

값이 없어지지 않았으면 좋겠고, 값을 기준으로도 정렬해보고 싶겠지요?






















## 정렬 기준 바꾸기

하지만 우리는 키가 아니라 값을 기준으로 정렬하고 싶습니다. 

이러한 경우를 위해 sorted\( \) 함수는 정렬 기준을 바꿀 수 있도록 옵션을 제공합니다.

```python
def sortKey(item):
    return item[1]


print(sorted(scores.items(), key=sortKey))
```

먼저 함수를 하나 정의합니다. sorted 함수에 들어온 배열의 요소들에서 서로 비교할 기준을 제공하는 함수입니다.

매개변수로 배열의 요소를 받고, 결과값으로 비교 기준을 돌려줍니다.

scores.items\(\)의 요소들은 \('a', 65\)와 같은 형태이니 65를 반환하도록 item\[1\]을 return 해줍니다.

{% hint style="info" %}
튜플은 수정할 수 없다는 점만 빼면 배열과 같으므로 item\[1\]은 두 번째 값인 65를 가리킵니다.
{% endhint %}

그리고 sorted 함수 안에 key= 라는 옵션을 주어 방금만든 sortKey 함수를 정렬 기준으로 사용하도록 합니다.

![](../.gitbook/assets/image%20%2832%29.png)

결과는 위와 같습니다. 키, 값 튜플이 값을 기준으로 잘 정렬되어 있는 배열이 출력되었습니다.



점수가 높은 것을 먼저 보고 싶다면 정렬 순서를 바꿔야겠죠? 

내림차순 정렬을 하려면 reverse 라는 옵션을 True\(참\)로 주면 됩니다.

```python
print(sorted(scores.items(), key=sortKey, reverse=True))
```

![](../.gitbook/assets/image%20%28215%29.png)
























## 채널 정보 정렬

이제 Dictionary를 원하는 기준으로 정렬하는 방법을 배웠으니, 네이버 TV TOP 100으로 돌아가 만들어낸 데이터를 정렬해 보겠습니다.

Stage 3의 마지막 부분에서 출력해본 chnInfos.items\(\)는 다음과 같았습니다.

![](../.gitbook/assets/image%20%2877%29.png)

Dictionary의 키인 채널명과 'hit'과 'like'를 가지고 있는 내부 정보 Dictionary 값이 튜플을 이루고 있습니다.

이 때 sortKey 함수는 어떻게 작성해야 할까요?

키를 뽑아내야 하는 배열 요소는 \('복면가왕', {'hit': 21929867, 'like': 87771}\) 의 형태이고, 조회수를 기준으로 정렬하기 위해 21929867을 결과값으로 되돌려 주어야 합니다.

```python
def sortKey(item):
    return item[1]['hit']
```

따라서 item\[1\] 을 이용해 튜플의 1번 값인 {'hit': 21929867, 'like': 87771}에 접근하였고, 다시 \['hit'\]을 사용해 Dictionary의 'hit' 키가 가지고 있는 21929867 값을 돌려주고 있습니다.



```python
sortedList = sorted(chnInfos.items(), key=sortKey, reverse=True)

for sortedInfo in sortedList:
    print(sortedInfo)
```

이 정렬 기준 함수를 사용해 정렬한 후, 그 결과 배열을 하나씩 출력하면 원하는 결과를 얻을 수 있습니다.

![](../.gitbook/assets/image%20%28166%29.png)

2018년 6월 28일 19시 22분의 네이버 TV TOP 100 채널 조회수별 정렬 결과입니다.

많은 공을 들여 수집하고, 가공하고, 정렬한 결실을 보는 순간입니다!
