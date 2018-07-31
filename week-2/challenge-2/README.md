# Challenge 2

## 도전과제 2

Stage 4에서는 네이버 TOP 100의 4위 이하 클립 정보들을 불러와 보았고, 그 중 첫번째인 4위 클립의 제목을 출력해보았습니다.

다음으로 4 ~ 8 위 클립들의 제목, 채널명, 재생 수를 출력해보세요.

\(div.cds는 4~100위의 클립을 가지고 있으므로 0번째가 4위입니다!\)

![](../../.gitbook/assets/image%20%2882%29.png)

출력 형식은 "클립명 / 채널명 / 재생 수" 입니다.

![2018&#xB144; 6&#xC6D4; 19&#xC77C;&#xC758; &#xACB0;&#xACFC;&#xB294; &#xC774;&#xB807;&#xC2B5;&#xB2C8;&#xB2E4;.](../../.gitbook/assets/image%20%2862%29.png)



아래 코드에서 시작하세요.

```python
import requests
from bs4 import BeautifulSoup

req = requests.get('https://tv.naver.com/r/')

raw = req.text

html = BeautifulSoup(raw, 'html.parser')

infos = html.select('div.cds')
```



### Self Hint

1. 먼저 4위 클립의 정보부터 모두 출력해봅시다. 채널명, 재생 수의 데이터 경로를 찾아보세요.
2. 형식에 맞게 출력하려면 print\(\) 함수를 어떻게 사용해야 할까요?
3. 5 ~ 8위 클립을 출력하려면 어떤 숫자를 어떻게 바꿔야 할까요?



