# Stage 4 - 가져온 문서에서 데이터를 추출해보자

아직 데이터 리스트를 맘대로 가지고 놀기에는 부족합니다. 더 강력한 문법들을 배워 3주차의 데이터 수집에 대비하겠습니다.

먼저 리스트 데이터의 모든 요소를 일괄적으로 처리할 수 있는 반복문에 대해 배워봅니다.

수많은 데이터를 다뤄야 하는 데이터 수집 작업에서 특히 중요한 문법입니다.

또한 리스트에 적용할 수 있는 리스트 타입 함수를 몇 가지 배워보겠습니다.

## 반복문 for

스테이지 3에서 다룬 도시 리스트가 있습니다. 관광 데이터와 연동해서 보여주기 위해 도시 뒤에 '명소' 라는 글자를 붙이고 싶습니다.

```python
cities = ['서울', '인천', '수원', '성남', '대전', '원주', '대구', '김해', '군산', '경주', '청주']

print(cities[0] + ' 명소')
print(cities[1] + ' 명소')
print(cities[2] + ' 명소')
print(cities[3] + ' 명소')
print(cities[4] + ' 명소')
print(cities[5] + ' 명소')
print(cities[6] + ' 명소')
print(cities[7] + ' 명소')
print(cities[8] + ' 명소')
print(cities[9] + ' 명소')
print(cities[10] + ' 명소')
```

이게 최선일까요? 도시가 100개로 늘어나면 어떻게 해야 할까요?

여기서 사용해야 할 것이 바로 반복문입니다.

```python
additional = '의 명소를 방문해보세요.'

for city in cities:
    print(city + additional)
    print('-------------------------')
```

\[출력 결과\]

for 문을 작성하고 실행할 기능을 정의해 주기만 하면, cities 리스트 내의 모든 요소에 대해 같은 기능을 반복하여 실행합니다.

for city in cities: 는 영어 문장처럼 해석하면, "cities 안의 city들에 대하여" 라는 뜻입니다. 여기서 cities는 미리 정의해 둔 리스트의 변수명이며, city는 미리 정의한 것이 아니라 for 문 안에서 임시로 사용할 반복 매개 변수입니다.

\[for문 분해 이미지\]

for 문 내부에서는 위와 같은 기능이 실행되고 있다고 볼 수 있습니다. cities의 모든 요소가 한 번씩 city에 할당되어 명령해 둔 작업을 반복 수행합니다.

## 파이썬의 실행 공간

```python
일반 실행 내용

for 매개변수 in 리스트:
    반복 실행할 내용 1
    반복 실행할 내용 2
    ...

# 들여쓰기가 끝나면 반복 종료
일반 실행 내용
```

for문의 기본 문법은 위와 같습니다.

가장 주의할 점은, 들여쓰기 된 부분만 반복 실행할 기능으로 간주한다는 것입니다.

for문이 선언되는 순간 일반 실행 공간과 분리된 공간이 형성됩니다. 분리된 공간은 Tab 또는 띄어쓰기 4개로 구분하며 반복하여 실행할 내용으로 인식합니다. 반복 매개변수는 이 공간 안에서만 사용할 수 있습니다. 리스트의 모든 내용을 반복 실행하면 분리된 공간이 종료되어 다음 줄의 일반 실행 공간으로 넘어갑니다.

\[스코프 사진\]

분리된 공간에서는 바깥 공간의 변수를 사용할 수 있습니다. 바깥 공간에서는 분리된 공간의 변수를 사용하면 안됩니다. \(실행은 되지만 반복되지 않아 의미가 없으며 경고가 발생합니다.\)

## 리스트의 타입 함수

리스트 타입 역시 리스트를 편집할 수 있는 여러 타입 함수를 제공합니다.

\[리스트 타입 함수 표\]

```python
cities = ['서울', '인천', '수원', '성남', '대전', '원주', '대구', '김해', '군산', '경주', '청주']

cities.append('김포')
cities.append('순천')
print(cities)

cities.pop(2)
print(cities)

cities.reverse()
print(cities)

cities.sort()
print(cities)
```

리스트 타입 함수들은 문자열과 달리 원본 리스트를 직접 수정합니다.

더 많은 리스트 타입 함수를 알아보고 싶다면 [https://wikidocs.net/14](https://wikidocs.net/14) 의 내용을 학습하고 연습 문제도 풀어보세요!
