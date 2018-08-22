
```python
import requests
from bs4 import BeautifulSoup


for i in range(3):
    raw = requests.get('https://news.ycombinator.com/news?p=' + str(i + 1)).text
    html = BeautifulSoup(raw, 'html.parser')

    titles = html.select('a.storylink')

    for title in titles:
        print(title.text)
```

More를 눌러 페이지를 이동시켜보면 주소의 쿼리 스트링이 어떻게 바뀌는지 찾아낼 수 있을 것입니다.

range() 함수는 숫자를 0부터 만들어내기 때문에, 1부터 시작하는 페이지 주소를 만드려면 1을 더해주어야 합니다.

str() 함수를 이용해 숫자를 문자열로 바꿔주는 것도 잊지 마세요!