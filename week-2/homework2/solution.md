# 모범 답안



```python

shopping_items = [
    ['맥스틸 TRON G10PRO', 22890, '상품평 2,464', '등록일 2018.08.'],
    ['로지텍G G102', 19820, '상품평 15,337', '등록일 2016.10.'],
    ['몬스타기어 데빌스킬', 35000, '상품평 1,200', '등록일 2018.05.'],
    ['맥스틸 TRON G10', 19800, '상품평 16,788', '등록일 2014.11.'],
    ['로지텍 B100', 10690, '상품평 4,016', '등록일 2017.01.'],
    ['앱코 WM300', 5490, '상품평 826', '등록일 2018.05.'],
    ['앱코 HACKER A660', 29790, '상품평 1,834', '등록일 2017.08.'],
    ['HP M100', 4390, '상품평 3,852', '등록일 2017.09.'],
    ['로지텍 G G402', 35300, '상품평 4,005', '등록일 2014.09.'],
    ['제닉스 M1', 11880, '상품평 7,222', '등록일 2013.05.']]

total = 0

for item in shopping_items:
    total = total + item[1]
    print(item[0], '제품은', item[3][6:8], '년', item[3][-3:-1], '월에 등록되었습니다.')

average = total / len(shopping_items)

print('평균 가격은', average, '원 입니다.')
```

데이터 구조는 중첩 리스트입니다. 상품 하나의 정보가 리스트 형태로 응집되어 있습니다.

먼저 가격을 모두 더할 변수 total을 준비했습니다. 여기에 for 문으로 리스트를 순회하며 가격을 더하도록 만들었습니다.

데이터를 포매팅하여 출력하는 과제는 문자열 슬라이싱을 적극적으로 활용했습니다. 중첩 리스트이기 때문에 대괄호 인덱스를 두 번 사용해야 한다는 점을 주목해주세요.

평균을 구하기 위한 물품의 개수는 내장함수 len을 사용하여 구할 수 있습니다.

