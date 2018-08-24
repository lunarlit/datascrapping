# Stage 1 - Dictionary에 대해 배워보자

첫 번째 스테이지에서는 새로운 데이터 구조 Dictionary에 대해 알아봅니다.

Dictionary는 리스트와 비슷하게 데이터를 한 데 모을 수 있는 데이터 구조이지만, 리스트와 명확히 구분되는 장점을 가지고 있어 데이터 가공에 많이 쓰입니다.

## Dictionary를 소개합니다

![](../.gitbook/assets/image%20%2830%29.png)

리스트는 값을 차례로 저장하고 "인덱스" 라는 숫자 번호를 통해 저장된 값을 불러와 활용할 수 있습니다.

그런데 Dictionary는 리스트와 달리 값을 번호가 아닌 이름표로 구분합니다. 이 이름표를 "키\(Key\)" 라고 부릅니다.

저장되는 데이터가 약간 늘어나긴 하지만, 데이터에 이름을 붙여 저장하고 불러올 수 있다는 것은 확실하게 구별되는 강점입니다.

## Dictionary의 형식

\[정의 그림\]

Dictionary는 크게 중괄호 { } 로 둘러싸는 것으로 정의합니다.

중괄호 안에 들어가는 값 하나하나는 "키\(Key\)"와 "값\(Value\)"의 쌍이 됩니다.

작성시엔 '키 이름': '값'의 형태로 쓰며 여러 값들을 쉼표로 구분합니다.

\[사용 그림\]

저장된 데이터를 사용할 때에는 리스트와 비슷합니다.

Dictionary의 변수명에 \[\]를 붙이는데 그 안에 들어가는 값이 숫자 인덱스가 아니라 키라는 점이 다를 뿐입니다.

## Dictionary의 접근법

### 값 할당

```python
people = {'korean': 380, 'american': 42, 'japanese': 15}

print(people)
print(people['korean'])

people['american'] = 63

print(people['american'])
print(people['japanese'])
```

\[출력 결과\]

리스트에서와 마찬가지로 people\['american'\] 등 단일 접근자 역시 하나의 변수와 같이 다룰 수 있습니다.

people\['american'\]에 63이라는 값을 할당하면 Dictionary의 'american' 키와 연결된 값이 63으로 바뀝니다.

### 새 키-값 추가

```python
people['german'] = 29

print(people)
```

\[출력 결과\]

Dictionary에 새로운 값을 추가하려면 어떻게 해야 할까요?

최초에 정의된 Dictionary에는 'german'이라는 키가 존재하지 않습니다.

그럼에도 불구하고 위와 같이 people\['german'\]에 값 할당을 시도하면, Dictionary에 알아서 새 키가 생기고 값이 할당됩니다.

## 중첩 Dictionary 구조

```python
chn_infos = {'하트시그널2': {'hit': 20000, 'like': 3800},
            '미스터션샤인': {'hit': 18000, 'like': 3500},
            '쇼미더머니7': {'hit': 25000, 'like': 2200}}

print(chn_infos)
print(chn_infos['하트시그널2'])
print(chn_infos['미스터션샤인']['hit'])
```

중첩 리스트와 유사하게, Dictionary의 키와 연결된 값이 또다른 Dictionary인 경우입니다.

접근 역시 중첩 리스트와 유사하게 하면 됩니다.

\[중첩 Dictionary 접근 사진\]

chn\_infos\[첫 번째 키\]\[두 번째 키\] 를 사용하면 가장 안쪽의 값까지 접근할 수 있습니다.

### 중첩 Dictionary 구조의 수정

```python
chn_infos['하트시그널2']['hit'] = chn_infos['하트시그널2']['hit'] + 4900
chn_infos['하트시그널2']['like'] += 280

print(chn_infos['하트시그널2'])
```

이러한 2중 접근자 역시 변수처럼 다룰 수 있습니다.

예시에서는 하트시그널의 조회수에 4900을 더하여 다시 하트시그널의 조회수 값으로 할당하고 있습니다.

그러면 chn\_infos의 '하트시그널2' 키와 연결된 값 Dictionary 안에서, 'hit' 키와 연결된 값이 수정됩니다.

### 이항 연산자

\[삼항 연산자 사진\]

일반적인 삼항 연산자입니다. 연산할 두 개의 값과 결과를 저장할 하나의 변수, 총 세 개의 값 또는 변수가 필요합니다.

\[이항 연산자 사진\]

연산할 두 개의 값 중 하나가 자기 자신이라면 어떨까요? 이미 존재하는 값에 다른 하나의 값을 연산하여 다시 저장하는 경우입니다. 이럴 때 똑같은 변수를 두 번 쓰는 것은 낭비겠죠?

### 중첩 Dictionary 구조의 내부 키-값 추가

chn\_infos에 새로운 채널인 '꽃보다할배'에 대한 정보를 입력하려고 합니다.

```python
newchn = '꽃보다할배'
newhit = 19400
newlike = 2760

chn_infos[newchn]['hit'] = newhit
chn_infos[newchn]['like'] = newlike
```

chn\_infos의 채널 키 중 '꽃보다할배' 는 없지만, Dictionary는 없는 키도 알아서 추가해주니 위와 같이 작성하면 될까요?

실제로 실행해보면 오류가 납니다.

존재하는 키던 존재하지 않는 키던, 키를 통한 접근이 성공하는 경우는 접근하려는 Dictionary가 실제로 존재하는 경우입니다.

위의 단순 Dictionary 키-값 추가 예제에서는 'german' 키를 통해 접근하려던 people이라는 Dictionary가 존재하고 있기 때문에 새 키를 생성할 수 있었습니다.

하지만 여기서는 다릅니다. 코드는 오른쪽에서 왼쪽으로 실행되기 때문에, 프로그램은 'hit'이란 키로 접근하기 위해 먼저 chn\_infos\[newchn\] 이라는 Dictionary를 찾습니다. 하지만 chn\_infos는 newchn 키를 가지고 있지 않기 때문에, 해당 Dictionary 자체가 없습니다.

여기서 프로그램은 "Dictionary가 있어야 키를 추가하던 수정하던 할 거 아냐" 라며 오류를 내고 종료합니다.

```python
chn_infos[newchn] = {'hit': newhit, 'like': newlike}
```

그래서 중첩 Dictionary 구조에서는 존재하지 않는 키를 건너뛰어 접근하면 안되고, 먼저 키를 생성하도록 명령해야 합니다.

여기서는 chn\_infos의 newchn 키에 새로운 Dictionary를 만들어 할당해주고 있습니다.

chn\_infos는 멀쩡히 존재하고 있는 Dictionary이기 때문에 오류없이 새 키가 잘 추가됩니다.

### Dictionary의 여러가지 변형

이렇게 멋진 장점을 가지고 있는 Dictionary이지만, 때에 따라 리스트의 사용법이 필요할 때가 있습니다.

축적한 데이터를 for문을 통해 반복 처리하고 싶은 경우나 뒤에 나올 정렬을 수행해야 할 때가 그렇죠.

이를 위해 Dictionary 클래스에는 몇 가지 함수가 준비되어 있습니다.

```python
people = {'korean': 380, 'american': 42, 'japanese': 15, 'german': 26, 'french': 7, 'chinese': 213, 'canadian': 11}

print(people.keys())
print(people.values())
print(people.items())
```

\[출력 결과\]

먼저 .keys\(\) 함수는 이름에서도 알 수 있듯이, Dictionary의 모든 키만을 뽑아 리스트로 만들어 줍니다.

당연히 .values\(\) 함수는 반대로 값들을 리스트로 만들어 주겠죠?

마지막으로 .items\(\) 함수는 한 쌍의 키와 값을 "튜플"에 넣어 리스트로 만들어 줍니다.

튜플이란 \(\) 로 감싸져 있는 데이터 구조입니다. 생성 후 수정이 불가능하다는 점 빼고는 \[\] 로 감싸는 리스트와 거의 같다고 보시면 됩니다.

즉, 리스트 안의 요소 하나하나가 수정불가 리스트\(튜플\) 인 중첩 리스트 구조입니다.

따라서 people.items\(\)\[1\]\[0\] 은 'american' 이 됩니다.

이렇게 Dictionary를 리스트로 변형시키면, 다음과 같이 for문으로 반복 처리할 수가 있습니다.

```python
for p_item in people.items():
    print('There are', p_item[1], p_item[0] + 's')
```

\[출력 결과\]

people.items\(\)의 각 요소는 \('korean', 380\) 과 같은 튜플입니다. 따라서 반복 매개 변수 p\_item의 \[0\] 인덱스에 'korean', \[1\] 인덱스에 380이 들어있게 됩니다.

중첩 Dictionary 구조에도 적용이 가능합니다.

```python
chn_infos = {'하트시그널2': {'hit': 20000, 'like': 3800},
            '미스터션샤인': {'hit': 18000, 'like': 3500},
            '쇼미더머니7': {'hit': 25000, 'like': 2200}}

print(chn_infos.keys())
print(chn_infos.values())
print(chn_infos.items())
```

\[출력 결과\]

여기서 Dictionary의 내용을 반복 처리하려면 다음과 같습니다.

```python
for chn_info in chn_infos.items():
    print(chn_info[0], '의 조회수는', chn_info[1]['hit'], '입니다.')
```

chn\_infos.items\(\)의 각 요소는 \('하트시그널2', {'hit': 20000, 'like': 3800}\) 과 같은 튜플입니다. 따라서 반복 매개 변수 chn\_info의 \[0\] 인덱스에 '하트시그널2', \[1\] 인덱스에 {'hit': 20000, 'like': 3800} 가 들어갑니다. 조회수 숫자까지 도달하기 위해서는 chn\_info\[1\]\['hit'\] 으로 Dictionary 안까지 들어가야겠죠?

