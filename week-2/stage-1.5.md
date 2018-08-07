# Stage 1.5 - 배열에 대해 알아보자

Stage 4에서 배열이 꼭 필요한데, 구성 상 마땅치 않아 별도로 빼내었습니다.

가볍게 읽어주세요!

## 배열이란 무엇일까?

> [https://github.com/coalastudy/python-datascrapping-code/blob/master/week2/stage1-1-array.py](https://github.com/coalastudy/python-datascrapping-code/blob/master/week2/stage1-1-array.py)

변수를 값을 저장하는 그릇에 비유를 했었습니다.

그런데 같은 종류의 굉장히 많은 값이 있고, 모아서 저장해두고 싶다면 어떻게 해야할까요?

```python
lang1 = "Python"
lang2 = "Javascript"
lang3 = "Java"
lang4 = "C++"
lang5 = "Go"
lang6 = "Kotlin"
#...
```

이렇게 하나의 변수에 값을 하나씩 담으면 될까요?

좀 더 좋은 방법이 있습니다.

```python
langs = ["Python", "Javascript", "Java", "C++", "Go", "Kotlin"]
```

이와 같이 \[ \] 안에 쉼표로 구분된 여러 값을 넣으면, 값들의 리스트가 한번에 langs 변수에 저장됩니다.



```python
print(langs)
```

![](../.gitbook/assets/image%20%2813%29.png)

langs를 print 한 결과입니다. 값들이 잘 들어있네요.

그런데 값을 하나씩 사용하고 싶다면 어떻게 할까요?

```python
print(langs[0])
print(langs[2])
print(langs[5])
```

이와 같이 배열 변수 뒤에 \[ \]를 붙이고, 그 안에 가져올 값의 순서를 넣으면 됩니다.

![](../.gitbook/assets/image%20%287%29.png)

출력 결과는 위와 같습니다.

{% hint style="info" %}
배열의 순서는 0부터 시작합니다!

컴퓨터는 낭비를 싫어해요.
{% endhint %}



```python
lang1 = "Python"
lang2 = "Ruby"
number = 984

langs = [lang1, "Javascript", lang2, "Java", 16, number]
```

값이 들어갈 수 있는 자리라면 언제나 변수도 들어갈 수 있습니다. 값과 변수를 섞어서 넣어도 무방합니다.

서로 종류가 다른 값/변수도 하나의 배열 안에 넣을 수 있습니다.

