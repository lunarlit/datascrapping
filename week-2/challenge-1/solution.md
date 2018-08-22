# 모범 답안

```python
raw_data = '    코알라 웹개발 스터디에 정상적으로 신청이 완료되었습니다.    '

data = raw_data.strip()
new_data = data.replace('웹개발', '데이터 수집')
data_length = len(new_data)

print(raw_data)
print(data)
print(new_data)
print(data_length)

print(len(raw_data.strip().replace('웹개발', '데이터 수집')))
```

함수 체이닝과 함수 입력값으로 함수를 사용하는 기법을 최대한 활용하여 중간 변수를 모두 없앤 것을 잘 보세요.

간결해서 좋지만 때로 지나치게 활용하면 코드를 읽기가 어려워지기도 합니다. 적절한 중용을 찾으시길 바랍니다.

