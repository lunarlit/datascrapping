# 모범 답안



```python
numbers = [13, 26, 145, 796, 2411, 4, 78, 532,
           22, 734, 1633, 29, 8, 17, 792, 15, 2,
           273, 453, 546, 724, 298, 47, 35, 7]

odds = []
evens = []

for n in numbers:
    if n % 2 == 0:
        evens.append(n)
    else:
        odds.append(n)

print(odds)
print(evens)
```

반복문 안에 조건문을 사용하여 배열의 모든 숫자를 반복하며 짝수인지 홀수인지 검사하고 있습니다.

배열의 출력은 그냥 print\( \) 안에 넣으면 됩니다.

